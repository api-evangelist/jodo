---
name: jodo-flex-instalment-plan
description: Configure a Jodo Flex instalment plan for a student and track it through mandate setup, debits, settlements and bounces. Use when an institute's fees should be paid in scheduled instalments under an auto-debit mandate.
api: Jodo ERP Integrations API
version: '1.0'
generated: '2026-08-23'
method: generated
source: https://docs.jodo.in/flex/overview/
operations:
  - getStudent
  - updateStudentFee
  - listFeeComponents
  - manageFlexPlan
  - getFlexSchedule
  - listStudentProducts
  - getAccessToken
---

# Set up and track a Flex instalment plan

Flex lets a parent pay structured fees in instalments under a bank auto-debit mandate
(https://docs.jodo.in/flex/overview/).

## Preconditions

The student must already exist in Jodo with a fee structure and academic-year context — run
`jodo-onboard-student-push` first if not. Confirm with `getStudent` and check
`data.fee_summary.total_fee`.

## Steps

1. **Sync the fee structure.** If it has changed since onboarding, call `updateStudentFee` with the
   full `fee_components[]` set — the PATCH replaces the set rather than merging, and no revert
   operation exists, so capture the prior state from `getStudent` first. Use `listFeeComponents` to
   confirm the valid codes.

2. **Configure the schedule.** `manageFlexPlan` takes `payment_schedule[]`, each entry carrying a
   `due_date` (YYYY-MM-DD) and a `details[]` split of `{component_type, amount}`. Set
   `is_downpayment: true` on components belonging to an upfront downpayment instalment.
   - Make the instalment amounts reconcile to the student's total fee before sending. Nothing in the
     API validates the schedule against the fee structure for you.

3. **Verify.** `getFlexSchedule` reads back the configured schedule. `listStudentProducts` with
   `product_type=flex` shows the enrolment and its status.

4. **Send the payer to Jodo.** Mint a token with `getAccessToken` for the student's user and redirect
   to `https://[base_url]/consumer/login-redirect?access_token=<token>&jodo_student_id=<id>&product_type=flex&callback_url=<yours>`.
   The parent completes setup and authorises the mandate there. Always mint the URL server-side.

5. **Track the lifecycle through webhooks.** There is no REST operation that reads a subscription or
   a mandate — these events are the only view you get:
   - `flex.subscription.setup` — the payer completed setup.
   - `flex.subscription.active` — the bank confirmed debits can proceed.
   - `flex.subscription.updated`, `flex.subscription.cancelled`, `flex.subscription.closed`
   - `flex.mandate.cancelled`, `flex.mandate.expired`
   - `flex.downpayment.debited` → `flex.downpayment.settled`
   - `flex.instalment.debited` → `flex.instalment.settled`
   - `flex.instalment.bounced` — a scheduled debit failed.

   Register each with `addWebhook` — see `jodo-receive-webhooks`.

## Rules

- **Debited is not settled.** `*.debited` means the money left the payer; `*.settled` means it reached
  the institute and carries the `settlement_utr`. Reconcile the institute's books on settlement.
- **`flex.instalment.bounced` carries no bank return reason.** The payload reports the failure but not
  why, so you cannot distinguish insufficient funds from a revoked mandate programmatically. Route
  bounces to a human follow-up queue.
- **No API cancels a Flex plan.** `flex.subscription.cancelled` and `flex.mandate.cancelled` arrive as
  inbound events only — cancellation happens outside this contract. Do not promise a parent an
  API-driven cancellation.
- **The mandate has no identifier in the contract** — no UMRN, no bank, no validity window, no amount
  cap. You cannot reference a specific mandate when talking to Jodo support; use the student and
  subscription IDs instead.
- **No test values are published**, and there is no time-simulation tooling. You cannot deterministically
  produce a bounce, a settlement or a mandate expiry in UAT from the documentation — plan for
  observation in production rather than rehearsal.
