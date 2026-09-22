# Testing Strategy

## Status

No executable application exists and no test tooling has been selected. This document records the verification approach to refine once requirements and the technology stack are known.

## Testing principles

- Test behavior derived from approved requirements, not implementation details alone.
- Cover happy paths, validation failures, authorization boundaries, state transitions, retries, and recovery behavior.
- Use the lowest-cost test level that gives meaningful confidence, supplemented by higher-level tests for important flows.
- Keep tests deterministic, isolated, and safe to run repeatedly.
- Include security, privacy, accessibility, performance, and compatibility checks where the product context requires them.
- Treat contract changes as compatibility risks and test consumers or integration boundaries accordingly.
- Security regression coverage MUST include policy authorization, gates where applicable, role/permission boundaries, resource ownership, IDOR attack cases, and other unauthorized-access cases.
- Boundary coverage MUST include validation failures, mass assignment, file-upload validation, rate limiting, secure storage access, and authentication-hardening flows where applicable.
- Authentication coverage MUST include successful and failed Fortify workflows, throttling, CSRF/session boundaries, verification and recovery policies when enabled, Sanctum session/API/token boundaries where enabled, token scope/revocation where applicable, MFA behavior where enabled, and authorization after authentication.
- Future surface and authorization coverage MUST verify that Admin cannot publicly register, Customer cannot access Seller Panel without seller capability, Seller cannot access Admin Panel, Seller cannot access another Store without membership/permission, pending or rejected/suspended Sellers lack restricted capabilities, Seller retains approved Customer capabilities, account-type checks do not replace Policies, Store IDOR attempts fail, multiple Store memberships work, authorized Admin access is required for Admin creation, and route surfaces remain isolated.
- Future seller-upload coverage MUST verify authorization, private storage, validation, review access, restricted downloads, redaction, retention/deletion behavior, and failure handling once those policies are approved.
- Marketplace commerce coverage MUST verify Seller inability to view another Store's Offers, Active Store Context scoping, Seller creation of Offers rather than Master Products, blocked direct canonical-Master-Product mutation, Master Product-first archives without duplicate top-level Offers, an Offer belonging to the correct Store, pre-approval unmatched Offer behavior, valid Master Product association before public approval, rejection of recursive Master Product/Offer hierarchy, pending/rejected/suspended Offer visibility, suspended Store effects, online external-product destination behavior, Physical-only Offer absence of required external URL, physical destination derivation through Store Location, Hybrid destination capability, and no conflicting duplicate Offer/product mode.
- Future API coverage MUST verify destination-capability representation aligns with Store/Offer context without treating irrelevant nullable fields as equivalent business data. Future URL validation coverage MUST reject malicious or unsafe external URLs after policy approval.
- Commercial CTA coverage MUST verify Raw Click recording, Billable Click classification, configured CTA eligibility, duplicate/replay/bot-like/self-click handling where detectable, free-mode tracking without charge, each Billing Mode, subscription-first funding, Wallet consumption, idempotent charging, concurrent charge safety, and future insufficient-funds behavior once approved.
- Source coverage MUST verify that manual and future WooCommerce imports remain scoped to the intended Store and pass normalization, Master Product matching, and moderation boundaries.
- Content/support coverage MUST verify Article/Master Product comment target isolation, comment moderation/authorization when defined, Ticket authorization, and Admin Offer-review authorization.
- Asynchronous and consistency coverage MUST include queue idempotency, duplicate delivery, webhook replay, retry behavior, failed jobs, transaction rollback, and race-condition-sensitive workflows.
- Where Event Sourcing is used, test event versioning/evolution, replay, projection rebuilds, projection idempotency, and the absence of unintended external side effects during rebuilds.
- Future cache tests MUST cover hits, misses, stale responses, regeneration, concurrent misses, distributed-lock contention, single-rebuilder behavior, bounded lock timeouts, TTL expiration, direct invalidation, event-driven invalidation, negative caching, backend failure, authoritative fallback, mass expiration, authorization-cache invalidation, inventory correctness, price revalidation, and retry-safe/idempotent rebuilds.

## Planned test layers

| Layer | Purpose | Status |
| --- | --- | --- |
| Documentation validation | Check links, headings, terminology, and required status reporting. | Applicable now |
| Unit tests | Verify isolated domain rules and transformations. | Future / stack TBD |
| Integration tests | Verify persistence and external boundaries. | Future / stack TBD |
| End-to-end tests | Verify approved user flows across the system. | Future / flows TBD |
| Security and abuse tests | Verify trust boundaries and misuse resistance. | Future / threat model TBD |
| Performance tests | Verify approved latency, throughput, or capacity targets. | Future / targets TBD |

## Documentation-only validation

For the current phase, validate that Markdown links resolve, no executable application files were added, and all affected documents agree on status and terminology.
