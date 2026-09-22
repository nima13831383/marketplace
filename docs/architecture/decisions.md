# Architecture Decisions

This document records durable decisions and their rationale. A later approved requirement may supersede a decision; do not silently edit history.

## ADR-0001 — Documentation-first delivery

- **Status:** Accepted
- **Decision:** Begin with a structured Markdown specification and agent operating guide before implementing the application.
- **Rationale:** Requirements are arriving incrementally, and future coding agents need a consistent source of truth.
- **Consequences:** Application source, dependencies, migrations, and executable logic are out of scope until an explicit implementation instruction.

## ADR-0002 — Explicit implementation gate

- **Status:** Accepted
- **Decision:** Implementation starts only after the user explicitly says **“Start implementation”** or gives an equivalent instruction.
- **Rationale:** Planning rounds must not accidentally create production artifacts.
- **Consequences:** Documentation may describe proposed schemas, interfaces, diagrams, and pseudocode, but those artifacts remain non-executable.

## ADR-0003 — No premature technology selection

- **Status:** Superseded in part by ADR-0004
- **Decision:** Do not commit to frontend, database, hosting, deployment, or other unapproved technology choices before product and operational constraints justify them. The backend framework is now separately approved as Laravel.
- **Rationale:** Selecting a stack without requirements would create avoidable rework and would exceed the current scope.
- **Consequences:** Laravel is approved, but its version and the remaining technology choices stay open and must be recorded when approved.

## ADR-0004 — Laravel backend framework

- **Status:** Accepted
- **Decision:** The backend framework MUST be Laravel.
- **Rationale:** This is an explicit project requirement for the future Marketplace backend.
- **Consequences:** Future implementation documentation may use Laravel concepts, but a Laravel version, packages, and executable application files remain unselected or prohibited until separately authorized.

## ADR-0005 — Layered architecture with MVC at the HTTP boundary

- **Status:** Accepted; terminology clarified by ADR-0013
- **Decision (original wording):** Use a layered flow with MVC applying primarily to Presentation / HTTP, not to the entire architecture.
- **Clarification:** The current flow is `Presentation / HTTP -> Application / Services -> Business Logic -> Infrastructure / Data Access`. It describes pragmatic responsibilities and request handling; it is not a DDD dependency rule.
- **Rationale:** This preserves thin transport adapters while keeping orchestration, business rules, and infrastructure concerns separated.
- **Consequences:** Controllers MUST be thin; business logic MUST remain outside controllers. Repositories and interfaces are optional, and Eloquent MAY be used pragmatically. See [layered-architecture.md](layered-architecture.md).

## ADR-0006 — Event-Driven Architecture where appropriate

- **Status:** Accepted
- **Decision:** Use explicit framework, application, domain, and integration event concepts where asynchronous decoupling or business fact publication is appropriate.
- **Rationale:** Side effects and cross-boundary work need clear contracts, retry behavior, idempotency, and observability.
- **Consequences:** Laravel framework events are not automatically domain events. See [event-driven-and-event-sourcing.md](event-driven-and-event-sourcing.md).

## ADR-0007 — Intentional and selective Event Sourcing

- **Status:** Accepted with scope open
- **Decision:** Event Sourcing is an approved capability for domains that justify immutable history, replayability, auditability, or high-value state correctness. It is not mandatory for every CRUD concept.
- **Rationale:** Full adoption has meaningful complexity and cannot be justified before product flows and selected business areas are known.
- **Consequences:** Full-system versus selective adoption for selected business areas/workflows remains an open question. This decision does not adopt DDD or require bounded contexts. See [event-driven-and-event-sourcing.md](event-driven-and-event-sourcing.md).

## ADR-0008 — Transactional Outbox as an available consistency pattern

- **Status:** Accepted as a pattern, not implemented
- **Decision:** Consider the Transactional Outbox Pattern where database writes must remain consistent with event or queue publication.
- **Rationale:** Database transactions alone do not make external publication atomic.
- **Consequences:** Each future workflow must choose an appropriate consistency and retry strategy; no outbox schema is defined yet.

## ADR-0009 — Backend authorization and IDOR protection

- **Status:** Accepted
- **Decision:** Enforce authorization on the backend and keep roles, permissions, Policies, Gates, ownership, and domain authorization distinct. Protect every sensitive operation against IDOR.
- **Rationale:** Role membership, route binding, and client identifiers do not prove access to a particular resource.
- **Consequences:** Authentication and product-specific roles remain open, but authorization checks are mandatory. See [authorization.md](authorization.md).

## ADR-0010 — Laravel queue monitoring and task scheduling capabilities

- **Status:** Accepted capability
- **Decision:** Laravel Horizon is approved for relevant queue monitoring, and Laravel Task Scheduling is approved for scheduled work.
- **Rationale:** Queue and scheduled operations require explicit health, failure, retry, overlap, and observability concerns.
- **Consequences:** Neither Horizon nor scheduled tasks are implemented yet. See [queue-and-scheduling.md](../engineering/queue-and-scheduling.md).

## ADR-0011 — Narrow HTML sanitization boundary

- **Status:** Accepted approach
- **Decision:** `mews/purifier` is the approved approach for sanitizing user-generated HTML only where the product explicitly permits HTML or rich text.
- **Rationale:** HTML sanitization is a focused control and must not be confused with input validation, authorization, output escaping, CSP, or secure rendering.
- **Consequences:** Plain-text fields MUST NOT be converted to arbitrary HTML, and no package is installed during documentation-only mode.

## ADR-0012 — Hybrid caching strategy

- **Status:** Accepted as an architectural baseline; not implemented
- **Decision:** Where caching is appropriate, use `Cache-Aside + Event-Driven Invalidation + Stale-While-Revalidate + Distributed Cache Locks + TTL Jitter` as the default strategy.
- **Rationale:** The strategy supports high-read workloads, database/load protection, cache-stampede prevention, controlled freshness, safe degradation, and future multi-server deployment.
- **Tradeoffs:** It adds invalidation complexity, stale-data windows, lock contention, observability requirements, and coordination between cache and authoritative state.
- **Consequences:** Cache is never automatically the source of truth. Policies MUST vary by domain, cache failures MUST degrade safely, and exact backend, TTL, lock, stale-window, namespace, and topology decisions remain open. See [caching-strategy.md](caching-strategy.md).

## ADR-0013 — Pragmatic Laravel MVC + Layered Architecture; DDD not adopted

- **Status:** Accepted
- **Decision:** The project architecture is Laravel + MVC + Layered Architecture + SOLID. Use the pragmatic responsibility flow `Presentation / HTTP -> Application / Services -> Business Logic -> Infrastructure / Data Access`.
- **Rationale:** The project needs clear boundaries and thin controllers without adopting a heavier architecture or inventing patterns before product requirements justify them.
- **Consequences:** Business/domain vocabulary remains allowed, but DDD, Clean Architecture, Hexagonal Architecture, and Onion Architecture are not project architecture requirements. Repositories and interfaces are optional, and Eloquent is an allowed implementation choice. Event-Driven Architecture and selective Event Sourcing remain independent approved capabilities.

## ADR-0014 — Fortify and Sanctum authentication stack

- **Status:** Accepted capability; policy scope remains open
- **Decision:** Use Laravel Fortify for approved headless backend authentication workflows and Laravel Sanctum for appropriate authenticated stateful browser/API/token mechanisms.
- **Rationale:** The capabilities are complementary: Fortify handles authentication workflows while Sanctum supports the authenticated request mechanism. Separating these responsibilities avoids treating either package as business authorization.
- **Consequences:** Fortify and Sanctum do not define product roles, permissions, Policies, Gates, ownership, or IDOR protection. Registration, verification enforcement, mandatory MFA, session/token lifecycle, client boundaries, and exact rate limits remain open. No packages or runtime configuration are installed in documentation-only mode. See [authentication.md](authentication.md).

## ADR-0015 — Four application surfaces in one Laravel application

- **Status:** Accepted as a planning boundary; implementation not started
- **Decision:** Provide Admin Panel, Seller Panel, Customer Panel, and API surfaces within the same Laravel application. Use Filament for Admin, Filament with custom theme/pages for Seller, Blade + Livewire + Tailwind for Customer, and a Laravel API boundary for API consumers.
- **Rationale:** The project requires distinct administrative, seller-facing, customer-facing, and API experiences while preserving one application and shared business behavior.
- **Consequences:** A separate frontend application is not currently required. Exact package versions, panel configuration, route middleware, API contracts, visual design, customer features, and consumer boundaries remain open. Presentation technologies MUST not become the location for complex business rules. See [application-surfaces.md](application-surfaces.md).

## ADR-0016 — Unified User identity and account classification

- **Status:** Accepted conceptually; persistence representation open
- **Decision:** Use one conceptual User identity for Admin, Seller, and Customer classifications. The conceptual `account_type` values are `admin`, `seller`, and `customer`.
- **Rationale:** A person must be able to retain one identity and history when moving from Customer to Seller, while the application distinguishes primary account classification.
- **Consequences:** Separate `admins`, `sellers`, and `customers` identity tables are not the default model. `account_type` is not a role, permission, ownership, membership, Policy result, or Gate result and MUST NOT be the only authorization mechanism. Exact column type, schema, transitions, and lifecycle persistence remain open. See [data-model.md](data-model.md) and [authorization.md](authorization.md).

## ADR-0017 — Seller verification and multi-Store membership boundary

- **Status:** Accepted conceptual boundary; details open
- **Decision:** Sellers may register directly, submit required verification documents, and remain limited while awaiting Admin review. Seller status is separate from account type. Store status is separate from Seller status. One User may manage multiple Stores, and each Store may have multiple Users through a membership relationship with varying responsibilities.
- **Rationale:** Seller verification and Store participation are distinct lifecycle and authorization concerns; a single owner column or one-Store assumption would not support the approved workflows.
- **Consequences:** Store access MUST evaluate authentication, seller capability/lifecycle, target Store membership, Store role/permission, and Policy/ownership authorization. Exact document types, statuses, transitions, Store role catalog, membership fields, and persistence schema remain open. See [application-surfaces.md](application-surfaces.md), [authorization.md](authorization.md), and [data-model.md](data-model.md).

## ADR-0018 — Master Product and Store Offer model

- **Status:** Accepted conceptual model; implementation not started
- **Decision:** Use Marketplace-controlled Master Products as canonical products and Store-specific Offers as commercial representations. An Offer belongs conceptually to one Master Product and one Store. The relationship is one level and is not a recursive product tree.
- **Rationale:** Master Product-first discovery avoids duplicate top-level results while preserving Store-specific commercial availability and moderation.
- **Consequences:** Search/archive surfaces primarily display Master Products; eligible Offers appear at Master Product detail level. Seller Offers require Marketplace/Admin matching and moderation. Exact fields, statuses, pricing, stock, ranking, and schemas remain open. See [marketplace-core-flow.md](../product/marketplace-core-flow.md).

## ADR-0019 — Active Store Context and source normalization boundary

- **Status:** Accepted conceptual boundary; details open
- **Decision:** Seller-facing Store-scoped operations use an Active Store Context, while all reads and mutations remain authorized against the target Store. Manual submission and future WooCommerce ingestion share normalization, Master Product matching, review/approval, and Store-scope boundaries.
- **Rationale:** Sellers may participate in multiple Stores, and source-specific paths must not bypass authorization, moderation, or canonical catalog controls.
- **Consequences:** Active Store Context is not authorization. Future selection UX, ingestion transport, WooCommerce sync rules, conflict handling, Store membership behavior, and integration configuration remain open. See [marketplace-commerce-model.md](marketplace-commerce-model.md).

## ADR-0020 — Separate CTA observation, classification, and Billing Decision

- **Status:** Accepted architectural boundary; implementation not started
- **Decision:** Treat Raw Click tracking, Billable Click classification, and Billing Decision as separate stages. Only explicitly configured commercial CTA types are billing candidates.
- **Rationale:** Not every HTTP request or UI interaction is commercial, and financial/quota consumption requires validation, duplicate handling, abuse controls, and auditability.
- **Consequences:** Presentation handlers MUST NOT hardcode direct Wallet or entitlement charges. Future redirect/location handling must support controlled observation and validation. Fraud policy, deduplication windows, sync/async processing, and exact CTA catalog remain open. See [marketplace-commerce-model.md](marketplace-commerce-model.md).

## ADR-0021 — Central Billing Mode and subscription-first funding priority

- **Status:** Accepted conceptual policy; financial implementation open
- **Decision:** Use one Billing Mode with conceptual values `free`, `subscription_only`, `wallet_only`, and `subscription_or_wallet`. Subscription Entitlement and Wallet / Prepaid Credit are separate; usable applicable entitlement takes priority in `subscription_or_wallet`.
- **Rationale:** A central mode prevents contradictory charging flags and preserves the semantic difference between included entitlement and prepaid credit.
- **Consequences:** Future consumption is correctness-sensitive and must be idempotent, concurrency-safe, transactionally consistent where required, auditable, and reconciliable. Prices, plans, quotas, Wallet denomination, insufficient-balance behavior, ledger design, and recovery policy remain open. See [marketplace-commerce-model.md](marketplace-commerce-model.md).

## ADR-0022 — Offer matching state and Store-derived destination capability

- **Status:** Accepted conceptual policy; implementation not started
- **Decision:** Seller-created products are Store-scoped Offers, never Master Products. An Offer may be unmatched during draft/review, but a publicly approved/published Offer requires one valid Master Product association. Store mode (`online`, `physical`, `hybrid`) determines default Offer destination capability; do not persist a duplicate Offer/product type by default. `external_product_url` is the preferred conditional online-capable Offer destination concept, while physical destination information derives through Store Location.
- **Rationale:** This preserves Marketplace authority over canonical products, avoids blocking moderation before matching is complete, and prevents inconsistent duplicated commercial-mode/location state.
- **Consequences:** Seller operations require target-Store authorization and do not permit free canonical Master Product mutation. API/presentation boundaries should expose destination capabilities rather than force clients to interpret unrelated nullable fields. Exact fields, nullability, constraints, validation, domain verification, multi-location selection, API shape, and Hybrid UI behavior remain open. See [marketplace-core-flow.md](../product/marketplace-core-flow.md) and [marketplace-commerce-model.md](marketplace-commerce-model.md).

## Decision template

```text
## ADR-#### — Title

- Status: Proposed | Accepted | Superseded
- Context: What problem or constraint requires a decision?
- Decision: What is being chosen?
- Alternatives considered: What credible options were evaluated?
- Rationale: Why was this choice made?
- Consequences: What follows from the decision?
- Related requirements: REQ-###
```
