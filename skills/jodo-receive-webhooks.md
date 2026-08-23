---
name: jodo-receive-webhooks
description: Register, verify and safely process Jodo webhook events. Covers subscription management, the three signature/validation mechanisms, the retry and auto-disable policy, and idempotent handling of the 36 documented events.
api: Jodo Webhooks
version: '1.0'
generated: '2026-08-23'
method: generated
source: https://docs.jodo.in/webhooks/handling-webhook-events/
operations:
  - addWebhook
  - listWebhooks
  - getWebhook
  - disableWebhook
---

# Receive Jodo webhooks safely

Jodo delivers 36 documented events across student master data, manual payments, Flex, Pay and Cred.
Every event goes to a URL you register **per event code** — see
`asyncapi/jodo-webhooks-asyncapi.yml` in this repo for the full catalogue.

## Register a subscription

`addWebhook` takes `event_code`, `url` and `failure_notification_email` (required), plus optional
`secret_key`, `header_key`, `header_value`, and `collector_code` if the institute has more than one
branch.

- **This write is idempotent on `event_code`.** One event code maps to exactly one URL; calling
  `addWebhook` again for the same code updates the existing subscription rather than creating a
  duplicate. It is the only documented idempotent write on this API.
- After creation, only `secret_key`, `header_key` and `header_value` can be changed.
- Always set `failure_notification_email`. Failure mails carry the payload, the response code your
  endpoint returned and a failure hint — and the payload is the only published way to recover an
  event body you failed to accept.
- Audit with `listWebhooks` / `getWebhook`; retire with `disableWebhook`.

## Verify every request

Use at least one, ideally more than one:

1. **HMAC signature** — configure `secret_key`, then compute an HMAC SHA-256 hex digest over the
   **raw request body** and compare it to the `X-Jodo-Signature` header. Compare in constant time, and
   read the raw bytes before any JSON parsing or middleware re-serialisation, or the digest will not
   match.
2. **Custom header** — agree a `header_key`/`header_value` pair with Jodo and reject requests that
   lack it.
3. **Source IP allowlist** — production `3.6.234.242`, `3.111.80.40`, `13.232.24.175`,
   `43.204.202.190`; UAT `3.108.86.33`, `13.127.40.177`, `65.0.77.215`. These differ per environment,
   so your allowlist must be environment-aware.

Note that the signature covers the body only. No timestamp is signed and no tolerance window is
documented, so the signature alone does not stop a replay — enforce that with `event_id`
deduplication, below.

## Process idempotently

The envelope is `{ event_id, event, timestamp, version, payload }`.

1. Verify the request came from Jodo.
2. Persist the raw payload keyed on `event_id`.
3. Return **2xx as soon as the event is accepted**, then process asynchronously. Jodo's own guidance:
   "If the business workflow takes time, persist the webhook event first, return a 2xx response, and
   process the event asynchronously."
4. Deduplicate on `event_id`. Delivery is at-least-once — "The same event can be delivered more than
   once because of retries, network timeouts, or delayed responses." If you have already processed an
   event, return 2xx again; do not reprocess.
5. Never create duplicate payments, settlements, subscriptions or loan records during retry handling.

## Understand the failure budget

- Success is any 2xx. A non-2xx, a timeout or an unreachable endpoint is a failed attempt.
- Jodo retries up to **5 times**, spaced across 6-hour and 24-hour intervals, for up to **3 days**.
  Any successful retry stops further attempts.
- **Continuous failure disables the subscription** — after 100 consecutive failures in production, or
  just **10 in UAT**. A development endpoint that goes down overnight will silently unsubscribe
  itself. Monitor the failure emails and re-register with `addWebhook` after an outage.

## Why this matters more on Jodo than on most APIs

Four significant entities — Flex **Subscription**, **Mandate**, Cred **LoanApplication** and
**Disbursement** — exist ONLY in webhook payloads. No REST operation reads any of them. If you drop
those events you cannot re-read the state; the only reconciliation reads available are `getStudent`,
`listStudentPayments`, `listStudentProducts` and `getFlexSchedule`. Treat webhook durability as
part of your financial record-keeping, not as a notification nicety.
