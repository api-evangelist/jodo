---
name: jodo-collect-adhoc-payment
description: Collect a one-off payment through Jodo Pay using a hosted checkout order or a shareable payment link, then reconcile it to completion. Use for event fees, application fees, registration payments and any collection that does not need an existing student record.
api: Jodo ERP Integrations API
version: '1.0'
generated: '2026-08-23'
method: generated
source: https://docs.jodo.in/pay/overview/
operations:
  - createPayOrder
  - getPayOrder
  - createPaymentLink
  - getPaymentLink
  - cancelPaymentLink
---

# Collect an ad-hoc payment with Jodo Pay

Jodo calls this the **order** integration pattern: a collection that does not require a student to
exist first (https://docs.jodo.in/getting-started/integration-patterns/order/).

## Choose the shape

- **Checkout order** (`createPayOrder`) — the payer is redirected to Jodo's hosted checkout right now.
  Pass `callback_url` so they return to you afterwards.
- **Payment link** (`createPaymentLink`) — you get a hosted URL to share over your own channel
  (email, WhatsApp, SMS). Supports `expires_at` and can be cancelled later.

Both require `name`, `phone`, `email` and a `details[]` array of `{component_type, amount}` line
items, and both return `data.order_id` and `data.redirect_url`.

## Steps

1. **Create.** Call `createPayOrder` or `createPaymentLink`. Attach `notes[]` as `{key, value}` pairs
   carrying your ERP reference — this is the documented reconciliation hook, and it round-trips back
   in the webhook payload.

2. **Persist `data.order_id` immediately, before you do anything else.** It is the only handle for
   status checks, cancellation and webhook reconciliation. If you lose it, there is no list-orders
   operation to recover it.

3. **Deliver.** Redirect the payer to `data.redirect_url`, or share it if you created a link. Always
   mint the URL server-side — never from browser or app code.

4. **Reconcile.** Do not trust the callback. Jodo's own guidance: "Treat the callback as a user
   navigation signal, not as final proof of payment." Confirm with `getPayOrder` or `getPaymentLink`,
   and subscribe to the settlement webhooks:
   - Orders: `order.payment.debited` → `order.payment.settled`
   - Links: `payment_link.payment.debited` → `payment_link.payment.settled`, plus
     `payment_link.payment.expired`
   - **Debited is not settled.** Only the `*.settled` event carries `settlement_utr` and `settled_at`
     on each line item. Do not mark the institute's books as reconciled on a debit alone.

5. **Cancel if needed.** `cancelPaymentLink` stops a link accepting payment; status becomes
   `cancelled`. Verify with `getPaymentLink` if your workflow needs confirmation.

## Rules

- **A Pay Order cannot be cancelled.** Only payment links have a documented cancel operation. Do not
  create an order speculatively.
- **No idempotency key.** A retry after a timeout may create a second collectible order or link —
  which means a payer could be charged twice. On any timeout, do not retry blindly; there is no
  list operation to check with, so you must rely on the `order_id` you persisted, and if you never
  received one, escalate to a human rather than re-sending.
- **No refund API exists.** Nothing in the published surface returns funds after a payment settles.
- Order status is `paid` or `unpaid`. Payment link status is `paid`, `unpaid`, `expired` or
  `cancelled`.
- `HUE00000` (400) is a bad request; `HUE00001` (404) means the `order_id` was not found.
