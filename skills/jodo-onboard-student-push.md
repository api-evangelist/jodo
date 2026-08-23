---
name: jodo-onboard-student-push
description: Onboard a student into Jodo from an ERP using the push integration pattern — register the user, create the student with their fee structure, and verify the result. Use when the ERP is the source of truth and should proactively create student and fee context in Jodo.
api: Jodo ERP Integrations API
version: '1.0'
generated: '2026-08-23'
method: generated
source: https://docs.jodo.in/getting-started/integration-patterns/push/
operations:
  - listBranches
  - listGrades
  - listFeeComponents
  - listDiscounts
  - registerUser
  - registerStudent
  - getStudent
---

# Onboard a student into Jodo (push pattern)

Use this when the institute's ERP owns student and fee data and should push it into Jodo. Jodo calls
this the **push** integration pattern (https://docs.jodo.in/getting-started/integration-patterns/push/).

## Before you start

- You need the Basic Auth API key and secret Jodo issued for **this environment**. Production is
  `https://ext.jodo.in`; UAT is `https://ext.devtest1.jodopay.com`. Credentials carry no visible
  environment marker, so confirm which pair you hold before sending anything.
- Never call Jodo from browser or mobile code. Docs: "Do not expose API keys or secrets in browsers,
  mobile apps, or client-side code."

## Steps

1. **Resolve the institute's codes first.** `grade`, `component_type` and `discount_type` are codes
   agreed bilaterally between Jodo and the ERP — never hard-code them.
   - `listBranches` → collector codes. If the institute has more than one branch, you must pass
     `collector_code` on the remaining configuration reads and on webhook registration.
   - `listGrades` → the grade codes valid in `student.grade`.
   - `listFeeComponents` → the codes valid in `fee_components[].component_type`.
   - `listDiscounts` → the codes valid in `discounts[].discount_type`.

2. **Register the user.** `registerUser` with `name`, `phone` (10 digits) and `email`, all required.
   Store the returned `data.registration_id` — it is the path key for the next step and for minting
   hosted-flow tokens later.

3. **Register the student.** `registerStudent` against that `registration_id`. The `student` object
   requires `fullname`, `identifier`, `grade`, `new_admission`, `academic_year_start`,
   `academic_year_end`, `date_of_birth` (YYYY-MM-DD), `primary_contact_name`,
   `primary_contact_number` and `primary_contact_email`.
   - Put the ERP's own enrolment number in `identifier`. It is echoed in every webhook payload and is
     how you reconcile events back to your system.
   - Attach `fee_components[]` (each with `component_type` and `fee_amount`, plus optional
     `discounts[]`) to establish the fee structure in the same call.
   - Attach `payments[]` for fees already collected before onboarding, so Jodo's view matches the
     ERP's. Each needs `amount`, `paid_at` (ISO 8601), `mode`, `transaction_id` and a
     `fee_components[]` allocation.

4. **Verify.** Call `getStudent` and check `data.fee_summary.total_fee` and
   `data.fee_summary.total_discount` against what the ERP expects before moving on. This read is your
   only confirmation — `registerStudent` returns a bare status envelope.

## Rules

- **No idempotency key exists.** If `registerStudent` times out, do NOT blind-retry: you have no
  documented guarantee against a duplicate. There is also no delete-student operation, so a duplicate
  is not something you can clean up through the API.
- **`updateStudentFee` replaces the fee component set.** Read `getStudent` first if you need the prior
  state — no fee history or revert operation is published.
- **No pagination is documented** on any of the configuration list operations. Treat the returned set
  as complete only because the institute's configuration is small; do not assume this for payments.
- Errors return an HTTP status with a machine-readable code. `MAE00001` (404) means the student was
  not found; `MAE00000` (400) is a bad request on the manual-payment surface. Handle unknown codes
  defensively — Jodo publishes no complete error registry (see `errors/jodo-error-codes.yml`).
