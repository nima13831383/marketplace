# Product Requirements

## Requirement status

This is the requirements register for the evolving Marketplace specification. Detailed Marketplace behavior remains open, but the backend framework, pragmatic architecture, authentication capabilities, application surfaces, unified identity, catalog/Offer model, commercial CTA/billing boundary, and several security constraints are now approved.

## Approved requirements

| ID | Requirement | Status | Source / note |
| --- | --- | --- | --- |
| DOC-001 | The repository must remain in documentation-only mode until the user explicitly authorizes implementation. | Approved | Initial project instruction |
| DOC-002 | Requirements supplied in later rounds must be treated as additions or explicit amendments to the existing specification. | Approved | Initial project instruction |
| DOC-003 | Documentation must identify assumptions, open questions, risks, decisions, and conflicts rather than silently inventing missing product behavior. | Approved | Initial project instruction |
| DOC-004 | Future coding agents must be able to understand product scope, boundaries, architecture, domain concepts, constraints, security, conventions, testing, phases, and unresolved questions from the documentation. | Approved | Initial project instruction |

## Approved technical and architectural requirements

| ID | Requirement | Status | Canonical document |
| --- | --- | --- | --- |
| TECH-001 | The backend framework MUST be Laravel. A specific Laravel version remains unselected. | Approved | [Architecture decisions](../architecture/decisions.md) |
| ARCH-001 | The application MUST use Laravel + MVC + Layered Architecture + SOLID. The documented responsibility flow is Presentation / HTTP -> Application / Services -> Business Logic -> Infrastructure / Data Access; MVC primarily concerns the Presentation / HTTP boundary. | Approved | [Layered architecture](../architecture/layered-architecture.md) |
| ARCH-002 | SOLID principles MUST guide design without creating unnecessary abstractions. | Approved | [Layered architecture](../architecture/layered-architecture.md) |
| ARCH-003 | Event-Driven Architecture MUST be used where appropriate, with explicit distinctions between framework, application, business/domain, and integration events. The business/domain label is descriptive and does not imply DDD. | Approved | [Event strategy](../architecture/event-driven-and-event-sourcing.md) |
| ARCH-004 | Event Sourcing is an approved capability and MUST be applied intentionally; global versus selective adoption remains open. | Approved | [Event strategy](../architecture/event-driven-and-event-sourcing.md) |
| ARCH-005 | Database writes, event publication, queue work, and external side effects MUST be designed for consistency using transactions, idempotency, and the Transactional Outbox Pattern where necessary. | Approved | [Concurrency and idempotency](../architecture/concurrency-and-idempotency.md) |
| ARCH-006 | The project architecture is pragmatic MVC + Layered Architecture + SOLID, not Domain-Driven Design. DDD, Clean Architecture, Hexagonal Architecture, and Onion Architecture are not required; repositories and interfaces are optional, and Eloquent MAY be used pragmatically. | Approved | [Architecture decisions](../architecture/decisions.md) |
| AUTH-001 | The future backend authentication capabilities MUST use Laravel Fortify for approved authentication workflows and Laravel Sanctum for appropriate authenticated session/API/token mechanisms. They are complementary and are not authorization substitutes. | Approved | [Authentication](../architecture/authentication.md) |
| AUTH-002 | Authentication MUST remain separate from roles, permissions, Policies, Gates, resource ownership, domain authorization, and IDOR protection. | Approved | [Authorization strategy](../architecture/authorization.md) |
| SURFACE-001 | The system MUST provide Admin Panel, Seller Panel, Customer Panel, and API surfaces within the same Laravel application. A separate frontend application is not currently required. | Approved | [Application surfaces](../architecture/application-surfaces.md) |
| UI-001 | The Admin Panel is approved to use Filament with Light / White as the default theme and user-switchable Dark Mode. Exact visual tokens remain open. | Approved | [Application surfaces](../architecture/application-surfaces.md) |
| UI-002 | The Seller Panel is approved to use Filament with custom theme/pages and must remain distinct from the Admin Panel. | Approved | [Application surfaces](../architecture/application-surfaces.md) |
| UI-003 | The Customer Panel is approved to use Laravel Blade + Livewire + Tailwind as its primary direction; it is not a full Filament panel. | Approved | [Application surfaces](../architecture/application-surfaces.md) |
| SURFACE-002 | The conceptual authentication URL surfaces are `/admin/login`, `/seller/login`, `/seller/register`, `/login`, and `/register`. Public Admin registration MUST NOT exist. | Approved | [Application surfaces](../architecture/application-surfaces.md) |
| IDENTITY-001 | The system MUST use one conceptual User identity for Admin, Seller, and Customer classifications; separate Admin, Seller, and Customer identity tables are not the default model. | Approved | [Data model](../architecture/data-model.md) |
| IDENTITY-002 | The User identity MUST include a conceptual `account_type` classification with approved values `admin`, `seller`, and `customer`; the persistence representation remains open. | Approved | [Data model](../architecture/data-model.md) |
| AUTHZ-003 | `account_type` is classification only and MUST remain distinct from roles, permissions, ownership, membership, Policies, and Gates. | Approved | [Authorization strategy](../architecture/authorization.md) |
| SELLER-001 | Sellers MAY register directly through `/seller/register`, enter onboarding, submit required verification documents, and await Admin review. Full seller capabilities require approved verification; pending access is limited to onboarding/verification-related capabilities pending exact policy. | Approved | [Application surfaces](../architecture/application-surfaces.md) |
| SELLER-002 | Seller lifecycle/status MUST remain separate from `account_type`; Store lifecycle/status MUST remain separate from Seller status. | Approved | [Domain model](../architecture/domain-model.md) |
| STORE-001 | The architecture MUST support one User managing multiple Stores and multiple Users participating in a Store through a membership relationship with varying Store-level responsibilities. | Approved | [Data model](../architecture/data-model.md) |
| SURFACE-003 | Web portals and API adapters MUST reuse shared Application / Services and Business Logic layers rather than duplicating business operations per surface. | Approved | [Application surfaces](../architecture/application-surfaces.md) |
| COMMERCE-001 | The Marketplace MUST maintain Marketplace-controlled canonical **Master Products** and Store-specific **Offers**. An Offer belongs conceptually to one Master Product and one Store; recursive product relationships are not approved for this model. | Approved | [Marketplace core flow](marketplace-core-flow.md) |
| COMMERCE-002 | Search/archive/category-style discovery MUST primarily present Master Products, then show eligible Offers on the Master Product detail surface rather than duplicate every Offer as a top-level product result by default. | Approved | [Marketplace core flow](marketplace-core-flow.md) |
| COMMERCE-003 | Seller-submitted Offers MUST enter Marketplace/Admin moderation and Master Product matching. Sellers MUST NOT have unrestricted control over canonical Marketplace taxonomy. | Approved | [Marketplace core flow](marketplace-core-flow.md) |
| COMMERCE-004 | A Seller-created/submitted product MUST be a Store-scoped Offer, not a Master Product. | Approved | [Marketplace core flow](marketplace-core-flow.md) |
| COMMERCE-005 | An Offer MAY be unmatched during draft/review. A publicly approved/published Offer MUST have one valid Master Product association; persistence constraints remain open. | Approved | [Marketplace commerce model](../architecture/marketplace-commerce-model.md) |
| COMMERCE-006 | Master Product data is canonical Marketplace data; Offer data is Seller/Store-specific commercial data. | Approved | [Marketplace core flow](marketplace-core-flow.md) |
| STORE-002 | Seller-scoped operations MUST use an authorization-aware target Store. Active Store Context is approved for Sellers with multiple Stores but MUST NOT replace backend authorization. | Approved | [Marketplace commerce model](../architecture/marketplace-commerce-model.md) |
| STORE-003 | Store mode MUST support conceptual `online`, `physical`, and `hybrid` modes. Store and Store Location MUST remain separate concepts and support future multiple locations/branches. | Approved | [Marketplace core flow](marketplace-core-flow.md) |
| STORE-004 | Store mode determines default Offer destination capabilities. A separate persistent Offer/product type MUST NOT duplicate Store mode unless a future approved Offer-level override requires it. | Approved | [Marketplace commerce model](../architecture/marketplace-commerce-model.md) |
| CTA-002 | `external_product_url` is the preferred conceptual Offer destination value for online-capable Stores and may be absent for Physical-only Offers. Physical destinations derive through Store Location. | Approved | [Marketplace core flow](marketplace-core-flow.md) |
| API-001 | Future API responses SHOULD expose meaningful Store/Offer destination capabilities rather than make consumers infer business meaning from unrelated nullable values. | Approved conceptually | [API design](../architecture/api-design.md) |
| CTA-001 | Only explicitly designated commercial CTA types are candidates for billing. Raw Click tracking, Billable Click classification, and Billing Decision MUST remain separate. | Approved | [Marketplace commerce model](../architecture/marketplace-commerce-model.md) |
| BILLING-001 | One central Billing Mode MUST govern commercial charging with conceptual values `free`, `subscription_only`, `wallet_only`, and `subscription_or_wallet`. | Approved | [Marketplace core flow](marketplace-core-flow.md) |
| BILLING-002 | Subscription Entitlement/quota and Wallet / Prepaid Credit MUST remain distinct. In `subscription_or_wallet`, usable applicable Subscription Entitlement MUST be consumed before Wallet / Prepaid Credit. | Approved | [Marketplace commerce model](../architecture/marketplace-commerce-model.md) |
| BILLING-003 | Future billable CTA consumption MUST be idempotent, concurrency-safe, transactionally consistent where required, auditable, and resistant to duplicate/replay processing. | Approved | [Marketplace commerce model](../architecture/marketplace-commerce-model.md) |
| INGEST-001 | Manual Seller submission and future WooCommerce ingestion MUST use the same conceptual normalization, Master Product matching, review/approval, and Store-scope boundary. | Approved | [Marketplace commerce model](../architecture/marketplace-commerce-model.md) |
| CONTENT-001 | The Marketplace includes high-level Articles, comments on Articles and Master Products, and Ticketing capabilities. Detailed taxonomy, moderation, review/rating, and ticket workflows remain open. | Approved | [Marketplace core flow](marketplace-core-flow.md) |
| SEC-001 | Backend authorization MUST distinguish roles/permissions from resource ownership; Policies are preferred for resource-level checks where appropriate. | Approved | [Authorization strategy](../architecture/authorization.md) |
| SEC-002 | IDOR protection is mandatory. Client-supplied identifiers and route-model binding MUST NOT be treated as authorization. | Approved | [Authorization strategy](../architecture/authorization.md) |
| SEC-003 | OWASP-aligned security practices are the mandatory engineering baseline without implying formal certification. | Approved | [Security expectations](../architecture/security.md) |
| SEC-004 | Laravel Form Requests SHOULD normally handle HTTP input validation, while authorization and domain rules remain separate concerns. | Approved | [Security expectations](../architecture/security.md) |
| SEC-005 | `mews/purifier` MAY sanitize user-generated HTML only where HTML input is explicitly allowed; it MUST NOT replace other XSS controls. | Approved | [Security expectations](../architecture/security.md) |
| SEC-006 | Mass assignment, SQL injection, file uploads, private storage, rate limiting, authentication hardening, audit logging, secure logging, secrets, and secure headers MUST be addressed. | Approved | [Security expectations](../architecture/security.md) |
| OPS-001 | Queue workloads MUST address security, retries, duplicate execution, idempotency, failed jobs, and observability. Laravel Horizon is approved for relevant queue monitoring. | Approved | [Queue and scheduling](../engineering/queue-and-scheduling.md) |
| OPS-002 | Laravel Task Scheduling is an approved capability; every scheduled task MUST define frequency, failure handling, overlap behavior, concurrency safety, and idempotency where required. | Approved | [Queue and scheduling](../engineering/queue-and-scheduling.md) |
| CACHE-001 | Where caching is appropriate, the default strategy MUST combine Cache-Aside, Event-Driven Invalidation, Stale-While-Revalidate, Distributed Cache Locks, and TTL Jitter. Cache MUST NOT be the authoritative source for transactional or security-sensitive state. | Approved | [Caching strategy](../architecture/caching-strategy.md) |

## Marketplace requirements

Detailed Marketplace feature requirements are still open. The approved portal, identity, catalog, Offer, CTA/billing, Seller verification, and Store membership boundaries above are conceptual foundations, not a complete feature catalog. Add each future requirement with a stable ID, a concise statement, acceptance considerations, and its status.

### Requirement template

```text
ID: REQ-###
Statement: The system shall ...
Status: Proposed | Approved | Superseded
Source: Round or decision that introduced it
Acceptance considerations: Observable behavior or constraints
Dependencies: Related requirements or decisions
```

## Requirement change log

| Round | Change | Affected documents |
| --- | --- | --- |
| Initial | Established documentation-only baseline and agent workflow. | All sections of `docs/` |
| Architecture and security round | Approved Laravel, layered/MVC boundaries, event-driven and selective Event Sourcing guidance, authorization, OWASP baseline, queue/scheduling, and concurrency constraints. | Architecture, engineering, roadmap, and open-question documents |
| Cache architecture round | Approved the hybrid cache baseline, safe degradation, domain-specific correctness boundaries, distributed stampede protection, and cache observability expectations. | Architecture, engineering, roadmap, and open-question documents |
| Architecture and authentication clarification round | Approved pragmatic Laravel MVC + Layered Architecture + SOLID terminology, explicitly excluded DDD as the project architecture, and approved complementary Fortify/Sanctum authentication capabilities. | Agent guide, product requirements, architecture, security, engineering, roadmap, and control-plane records |
| Application surfaces and identity/store round | Approved four same-application surfaces, portal technology directions, conceptual auth URL surfaces, unified User identity, account classification, seller verification boundary, separate Seller/Store lifecycles, multi-Store support, and Store membership. | Product, application-surfaces, architecture, authentication, authorization, security, data-model, engineering, roadmap, open questions, handoff, and control-plane records |
| Marketplace commerce core round | Approved Master Product/Offer model, Master Product-first discovery, Store modes/location separation, Active Store Context, Offer moderation/matching, commercial CTA classification, central Billing Mode, subscription-first funding priority, future WooCommerce ingestion boundary, and high-level content/support capabilities. | Product, commerce architecture, data, authorization, security, cache, events, concurrency, queue, observability, engineering, roadmap, handoff, history, and control-plane records |
| Marketplace Offer and destination clarification round | Clarified Seller-created Offer semantics, pre-approval Master Product matching, canonical-versus-commercial information ownership, Store-derived destination capability, conditional `external_product_url`, Store Location derivation, and conceptual API destinations. | Agent guide, product, commerce architecture, data, API, authorization, security, application surfaces, testing, roadmap, open questions, handoff, history, and control-plane records |

## Out of scope until specified

Do not infer features such as listings, search, checkout, payments, messaging, reviews, moderation, logistics, or detailed Customer Panel sections. Portal surfaces, account classifications, seller verification, and Store membership are approved at the documented conceptual level; detailed workflows and capabilities require further requirements.
