# Jodo

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Jodo is a Bengaluru-based fintech that automates fee collection for Indian educational institutes — schools, colleges, universities, coaching centres and skilling institutes. Founded in 2020 by Atulya Bhat, Raghav Nagarajan and Koustav Dey, the company says it serves 5,000+ institutes across 90+ cities and has processed over INR 30,000 crore in fees for 25 lakh+ students. Jodo ships three products on one platform: Flex (structured fees paid in instalments through an auto-debit mandate), Pay (student-linked collections, hosted checkout orders and shareable payment links) and Cred (education loans originated with NBFC lending partners). Its public ERP Integrations API lets an institute's ERP, finance or student-information system register users and students, sync fee structures and discounts, reconcile manual payments, configure instalment schedules, create checkout orders and payment links, and manage webhook subscriptions. Jodo publishes a substantial developer reference at docs.jodo.in covering 25 REST operations and 36 webhook events, with documented UAT and production environments, Basic Auth, an error code registry, a retry and idempotency policy, HMAC SHA-256 webhook signatures and published source IP ranges — but no machine-readable OpenAPI or AsyncAPI document, and no public pricing.

**Areas:** Company · Payments · Education · Fintech · Fee Collection · Lending · India · Webhook · ERP Integration · Financial Services

## Jodo on APIs.io

Everything profiled for Jodo is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/jodo/](https://apis.io/providers/jodo/)** — or go straight to one of the APIs or properties below.

### APIs (6)

- [Jodo Webhooks](https://apis.io/apis/jodo/jodo-webhooks/) — Event delivery surface for the Jodo platform. Jodo POSTs 36 documented events across student master data, manual payments, Flex…
- [Jodo Configuration API](https://apis.io/apis/jodo/jodo-configuration-api/) — Institute reference data (branches, grades, fee components, discounts) and webhook subscriptions.
- [Jodo Flex API](https://apis.io/apis/jodo/jodo-flex-api/) — Flexible instalment plans backed by an auto-debit mandate.
- [Jodo Pay API](https://apis.io/apis/jodo/jodo-pay-api/) — Checkout orders and shareable hosted payment links.
- [Jodo Students API](https://apis.io/apis/jodo/jodo-students-api/) — Student master data, fee structures and manual payment reconciliation.
- [Jodo Users API](https://apis.io/apis/jodo/jodo-users-api/) — Register ERP/institute users and mint short-lived tokens for Jodo-hosted flows.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Jodo Webhooks](https://apis.io/asyncapis/jodo/jodo-webhooks-asyncapi/)

#### Event channels (36)

Individual events and webhooks the provider emits.

- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-10/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-11/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-12/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-13/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-14/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-15/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-16/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-17/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-18/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-19/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-2/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-20/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-21/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-22/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-23/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-24/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-25/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-26/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-27/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-28/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-29/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-3/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-30/)
- [{subscriberWebhookUrl}](https://apis.io/channels/jodo/jodo-webhooks-subscriberwebhookurl-31/)
- …and 11 more published on APIs.io

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Jodo Integrations Overlay](https://apis.io/overlays/jodo/jodo-integrations-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Jodo Authentication](https://apis.io/security/jodo/jodo-authentication/)
- [Jodo Domain Security](https://apis.io/security/jodo/jodo-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Jodo Plans Pricing](https://apis.io/plans/jodo/jodo-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Jodo Rate Limits](https://apis.io/rate-limits/jodo/jodo-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.jodo.in/)
- [Documentation](https://docs.jodo.in/)
- [Getting started](https://docs.jodo.in/getting-started/introduction/)
- [API reference](https://docs.jodo.in/getting-started/api-structure/)
- [Support](https://www.jodo.in/contact-us/)
- [Blog](https://www.jodo.in/blog/)
- [Trust centre](https://compliance.jodo.in/)
- [Terms of service](https://www.jodo.in/terms-and-conditions/)
- [Privacy policy](https://www.jodo.in/privacy-policy/)
- [GitHub](https://github.com/jodohq)
- [Status](https://status.jodo.in/)

## API Evangelist rating

**Kin Score 38.5** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 24.7 |
| Contract governance | 4.5 |
| Operational transparency | 34.2 |
| Developer ergonomics | 66.1 |
| Access clarity | 35.5 |

**Agent readiness 24.6** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | derived (partial credit) |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 26.8** — Payments

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/jodo/](https://apis.io/providers/jodo/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Jodo: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/jodo/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
