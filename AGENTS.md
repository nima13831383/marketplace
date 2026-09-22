# Agent Operating Guide

This file is the primary operational guide for coding agents working on the Marketplace project. Read it before changing any file.

## Current operating mode

The repository is in **DOCUMENTATION-ONLY MODE**. Do not implement the application, scaffold Laravel or frontend code, install packages, add runtime dependencies, create migrations, or add executable application logic until the user explicitly authorizes implementation with wording such as **“Start implementation”**.

## Approved technical foundation

- Backend framework: **Laravel**. A specific version is not yet selected.
- Architecture: **Laravel + MVC + Layered Architecture + SOLID**. The current responsibility flow is **Presentation / HTTP -> Application / Services -> Business Logic -> Infrastructure / Data Access**.
- This is a pragmatic layered architecture, not Domain-Driven Design. DDD, Clean Architecture, Hexagonal Architecture, and Onion Architecture are not project architecture requirements. Repositories and interfaces are optional boundary tools, and Eloquent MAY be used where it is the pragmatic choice; agents MUST NOT add pattern ceremony without a real boundary or requirement.
- MVC applies primarily to the Presentation / HTTP boundary; controllers MUST remain thin and MUST NOT contain business logic.
- SOLID, Event-Driven Architecture, and intentional/selective Event Sourcing are required guidance. Event Sourcing is not mandatory for every CRUD concept.
- Authentication capabilities: **Laravel Fortify + Laravel Sanctum**. Fortify owns approved backend authentication workflows; Sanctum supports appropriate authenticated session/API/token mechanisms. Neither replaces backend authorization.
- Application surfaces: one Laravel application with Admin Panel, Seller Panel, Customer Panel, and API. Admin uses Filament; Seller uses Filament with custom theme/pages; Customer uses Blade + Livewire + Tailwind. A separate frontend application is not currently required.
- Identity: use one conceptual User identity with `account_type` values `admin`, `seller`, and `customer`. Account type is classification only and MUST NOT replace roles, permissions, membership, ownership, Policies, Gates, or IDOR protection.
- Seller/Store boundaries: direct Seller registration, administrative verification before full seller capabilities, separate Seller and Store lifecycles, one User to multiple Stores, and multi-user Store membership are approved conceptual requirements. Exact fields, statuses, documents, roles, and permissions remain open.
- Commerce terminology: use **Master Product** for Marketplace-controlled canonical products and **Offer** for Seller/Store-specific commercial entries. Seller-created products are Offers, never Master Products; do not model recursive seller child products.
- Every Offer belongs to a Store. It MAY remain unmatched before review, but a publicly approved Offer MUST have one valid Master Product association. Sellers MUST NOT alter canonical Master Product information except through an approved workflow.
- Store mode (`online`, `physical`, or `hybrid`) determines default Offer destination capabilities. Do not duplicate it as an Offer/product type. Use the conceptual `external_product_url` only for online-capable Offers; physical destinations derive from Store Location.
- Seller-scoped operations MUST use an authorization-aware target Store. Active Store Context is a UX/request-scoping aid, not authorization.
- CTA observation, billable-click classification, and Billing Decision are separate stages. Only configured commercial CTA types are billing candidates; billing MUST be idempotent and concurrency-safe.
- In `subscription_or_wallet` mode, use applicable Subscription Entitlement before Wallet / Prepaid Credit. Cache MUST NOT be authoritative for billing, quota, charging, or Store/Offer authorization.
- Seller imports MUST pass source normalization, Master Product matching, review, and Store-scope boundaries; future WooCommerce support does not bypass these controls.
- Backend authorization is mandatory. Roles, permissions, policies, gates, and resource ownership are distinct concerns; IDOR protection is required.
- OWASP-aligned security, Form Request validation, safe mass assignment, secure uploads/storage, rate limiting, idempotency, transaction safety, and concurrency awareness are mandatory.
- Queue workloads MUST be designed for security and idempotency. Laravel Horizon and Laravel Task Scheduling are approved capabilities, not yet implemented.
- `mews/purifier` is approved only for permitted user-generated HTML sanitization; it does not replace validation, authorization, output escaping, or CSP.
- Caching MUST remain an optimization layer, never the automatic source of truth. Where caching is appropriate, use the approved hybrid strategy: Cache-Aside, event-driven invalidation, stale-while-revalidate, distributed locks, and TTL jitter.
- Cache design MUST prevent stampedes, use bounded distributed coordination across future application instances, keep invalidation out of controllers, and degrade safely when the cache is unavailable.
- Cache MUST NOT be the sole authority for inventory, price correctness, payment, wallet, settlement, authorization, or other transactional/security-sensitive state. Security-sensitive invalidation MUST be prompt.

See the detailed [marketplace core flow](docs/product/marketplace-core-flow.md), [commerce model](docs/architecture/marketplace-commerce-model.md), [architecture](docs/architecture/overview.md), [application surfaces](docs/architecture/application-surfaces.md), [layered architecture](docs/architecture/layered-architecture.md), [authentication](docs/architecture/authentication.md), [caching](docs/architecture/caching-strategy.md), [authorization](docs/architecture/authorization.md), [security](docs/architecture/security.md), [queue and scheduling](docs/engineering/queue-and-scheduling.md), and [testing](docs/engineering/testing-strategy.md) documents instead of duplicating their full rules here.

## Required reading order

Before a documentation change, read:

1. [Documentation map and maintenance process](docs/README.md)
2. [Product requirements](docs/product/requirements.md)
3. [Architecture decisions](docs/architecture/decisions.md)
4. [Authentication](docs/architecture/authentication.md) when the change touches identity or protected requests
5. [Application surfaces](docs/architecture/application-surfaces.md) when the change touches portals, UI technology, routes, or shared web/API boundaries
6. [Marketplace core flow](docs/product/marketplace-core-flow.md) and [commerce model](docs/architecture/marketplace-commerce-model.md) when the change touches catalog, Offers, Store scope, CTAs, billing, or ingestion
7. The relevant product, architecture, engineering, roadmap, and open-question documents

When implementation is eventually authorized, read the complete documentation set that applies to the requested area before editing code.

## Documentation rules

- Treat the newest approved requirements as additions or explicit amendments to earlier requirements.
- Preserve earlier decisions unless a newer requirement replaces them.
- Do not invent major product requirements, roles, workflows, integrations, or technologies.
- Label assumptions, proposals, risks, and unresolved questions clearly.
- Keep detailed decisions in the relevant document and keep this file concise.
- Update cross-references when a change alters scope or terminology.
- Keep examples, schemas, pseudocode, and diagrams non-executable unless implementation has been authorized.
- Use normative language (`MUST`, `MUST NOT`, `SHOULD`, `MAY`) where a constraint is intentional.

## Change workflow

1. Inspect all existing project documentation and the relevant focused architecture/engineering documents.
2. Identify the new approved requirement and its affected documents.
3. Update the smallest coherent set of Markdown files.
4. Check for contradictions, stale links, and accidental implementation artifacts.
5. Report created files, updated files, decisions, assumptions, open questions, conflicts, and implementation status.
6. For high-risk or cross-document changes, preserve the applicable control-plane record under `docs/changes/` and update `docs/handoff/current.md`; these are documentation records, not application scaffolding.

## Implementation gate

The phrase **“Start implementation”**, or an equivalent explicit instruction, is a prerequisite for creating application source code or other executable project artifacts. A request to improve documentation does not open that gate.
