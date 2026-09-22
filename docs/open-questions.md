# Open Questions

These questions are intentionally unresolved. They should be answered through future approved requirements or decisions; agents must not silently choose answers that materially change the product or architecture.

## Product

- What problem does the Marketplace solve, and for whom?
- What category of goods, services, or other supply is in scope?
- What is the initial launch geography, language, currency, and regulatory context?
- Which user roles exist, and what outcomes does each role need?
- What final roles and permissions exist beyond the approved account classifications `admin`, `seller`, and `customer`?
- What is the smallest valuable launch scope, and what is explicitly out of scope?
- What are the success measures and acceptance criteria?

## Business and transaction model

- How are supply, demand, discovery, matching, or purchasing represented?
- Is a transaction, booking, subscription, lead, or another outcome involved?
- Are pricing, fees, taxes, commissions, refunds, or settlement in scope?
- Who handles fulfillment, delivery, disputes, support, and moderation?
- What exact Master Product and Offer fields, price representation, inventory/availability design, SKU requirements, Offer-image policy, Seller-title variation, moderation SLA, status transitions, search ranking, sorting, and archive-card content are required?
- Which commercial CTA types are configured, and which are billable?
- What Articles taxonomy/editorial workflow, comment moderation/review separation, and Ticket statuses/categories/SLA are required?

## Architecture and operations

- Which Laravel version and runtime support policy will be selected?
- Which clients and system boundaries are required?
- Which frontend, backend, storage, hosting, and deployment constraints exist?
- What exact Filament, Livewire, and Tailwind versions and compatibility policy will be selected?
- What Admin, Seller, and Customer feature catalogs, visual design, accessibility, and responsive behavior are required?
- Which route groups, guards, middleware, session behavior, and API consumer boundaries apply to each surface?
- Are search, messaging, files, notifications, analytics, or background jobs required?
- Which external providers or platforms must be integrated?
- What availability, latency, capacity, recovery, and retention targets apply?
- Will Event Sourcing apply globally or only to selected business areas/workflows?
- Which domains will initially use Event Sourcing?
- How will one User's `account_type` be persisted and how are Customer-to-Seller transitions, rejection, and suspension handled?
- What Seller lifecycle status names and transitions apply, and what limited capabilities are available while pending?
- Which Seller verification documents are required, and what validation, private storage, review, retention, deletion, and recovery rules apply?
- What Store lifecycle status names and transitions apply?
- What Store-level role catalog, membership attributes, permissions, and multi-Store limits apply?
- How are Seller history and Customer history retained on the same User identity after account transitions?
- What exact Store Location schema, initial multi-location behavior, map provider, and location CTA experience are required?
- Which Store Location should an Offer use when a physical or Hybrid Store has multiple locations or branches?
- What exact external URL validation rules, Store domain-verification policy, and imported-Offer URL validation apply?
- What Hybrid Offer destination requirements and exact API destination schema apply?
- What Active Store Context selection, persistence, switching, and removed/suspended membership behavior apply?
- What Master Product/Offer identifiers, retention, audit history, and lifecycle propagation rules are required?
- What exact click identity, deduplication window, fraud policy, Seller self-click policy, privacy signals, and redirect boundary apply?
- What Billing Mode persistence, click pricing, subscription plan catalog/quota, Wallet denomination/currency, insufficient-funds behavior, ledger, reconciliation, and recovery policy apply?
- Whether inactive/no-credit Offers remain visible and which Store/Seller/Offer lifecycle changes suppress commercial CTAs?
- What WooCommerce authentication, import-versus-sync, transport, field synchronization, conflict resolution, and manual override policy apply?
- What search technology and indexing strategy will be selected?
- Which workflows require a Transactional Outbox, idempotency keys, locking, or other consistency controls?
- What queue topology, Horizon access, worker model, and failed-job/dead-letter policy will be used?
- What scheduled tasks will exist, and what overlap, timezone, and ownership rules apply?
- Which cache backend and Laravel cache-store configuration will be selected, and will Redis be the default?
- What exact TTLs, jitter ranges, stale windows, lock lifetimes, and bounded lock-wait timeouts apply by domain?
- What is the fallback when a distributed cache lock cannot be acquired or the cache backend is unavailable?
- Which caches refresh synchronously, asynchronously, or through prewarming, and will cache tags be used?
- What cache namespace/version convention, eviction policy, capacity plan, and multi-server topology are required?
- Which metrics and alerts define cache stampede conditions, and which domains must never be cached?

## Security and compliance

- Which roles and permissions will exist?
- Which resources require ownership-based authorization?
- What account, authentication, verification, authorization, and operator-access policy is required?
- Which Admin bootstrap and authorized-Admin account-creation procedure is required?
- Which Fortify authentication workflows will be enabled, and which Sanctum browser/API/token consumers are justified?
- Which registration, email-verification, password-reset, password-confirmation, session-revocation, suspension, token-lifecycle, and operator-access policies apply?
- What token expiration/TTL, rotation, scope, revocation, and consumer-lifecycle rules apply if personal access tokens are enabled?
- Will MFA be optional or mandatory for any user or operation, and what recovery and reauthentication rules apply?
- What SMS OTP provider, expiration, resend interval, attempt limit, fallback, phone-verification, and OTP-bombing protections apply if OTP is enabled?
- What exact account transition rules apply after Seller rejection, suspension, or later reactivation?
- Which data is personal, financial, confidential, or otherwise sensitive?
- What user-generated HTML or rich-text surfaces will exist, if any?
- What upload types and storage providers will be allowed?
- Which external services will publish or consume webhooks?
- Which legal, contractual, regional, accessibility, or industry obligations apply?
- What abuse cases, fraud risks, moderation needs, and incident-response expectations exist?

## Delivery

- What environments and release controls are needed?
- Who owns operations, support, security review, and approval of production changes?
- What testing, accessibility, performance, and observability thresholds are required?
- When should the implementation gate be opened?
