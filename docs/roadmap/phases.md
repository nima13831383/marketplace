# Delivery Phases

This is a provisional sequence for reducing uncertainty. It is not a promise of features or dates.

## Phase 0 — Documentation baseline

- **Status:** In progress
- Establish the agent guide, documentation map, requirement register, decision log, and open-question register.
- Keep the repository in documentation-only mode.
- Record the approved Laravel and architecture/security foundation without implementing it.

## Phase 1 — Product discovery and scope

- **Status:** Pending
- Approve the product objective, target users, roles, core flows, initial scope, non-goals, and success measures.
- Confirm the detailed portal feature catalogs and visual/product boundaries without inferring unapproved Customer sections.
- Resolve the highest-impact product questions.

## Phase 2 — Business and system design

- **Status:** Pending
- Define domain vocabulary, boundaries, lifecycle rules, data ownership, security model, and external-system assumptions.
- Define unified identity, account classification, Seller verification lifecycle, Store lifecycle, multi-Store participation, and Store membership responsibilities.
- Define Master Product, Offer, Store Location, moderation, availability, source-normalization, and catalog/search boundaries.
- Define CTA classification, Billing Mode, Subscription Entitlement, Wallet / Prepaid Credit, financial audit/reconciliation, click-abuse, and insufficient-funds policies.
- Decide the final role/permission model without using `account_type` as a substitute for authorization.
- Decide whether Event Sourcing is full-system or limited to selected business areas/workflows, and identify any initial candidates.
- Define which Fortify authentication workflows are enabled, how Sanctum is used for approved clients, and which verification, recovery, session, token, and MFA policies apply.
- Define authorization boundaries, ownership rules, consistency boundaries, idempotency needs, and concurrency risks.
- Classify cacheable data by correctness sensitivity and define invalidation, stale-serving, lock, fallback, and load-protection policies.
- Record accepted architecture decisions.

## Phase 3 — Contracts and implementation plan

- **Status:** Pending
- Define API or integration contracts, error behavior, testing approach, observability needs, delivery risks, and a thin vertical-slice plan.
- Define surface-specific contracts for Admin, Seller, Customer, and API adapters while preserving shared Application / Services and Business Logic.
- Define queue, Horizon, scheduling, audit, logging, upload, and secret-management expectations where applicable.
- Define seller verification document-upload contracts, private-storage access, review permissions, retention, and recovery behavior.
- Define WooCommerce import/sync contracts, controlled destination/redirect behavior, map/location behavior, Articles/Comments/Ticketing contracts, and commerce observability.
- Define cache backend constraints, key/version namespaces, observability, and implementation-safe fallback behavior without installing or configuring dependencies.
- Confirm that the implementation gate has been explicitly opened before creating application artifacts.

## Phase 4 — Implementation

- **Status:** Blocked by the implementation gate
- Build only the approved scope in small, verifiable increments.
- Update the documentation when implementation reveals a required clarification or accepted decision.

## Phase 5 — Validation and release readiness

- **Status:** Pending
- Verify approved flows, security expectations, operational readiness, data handling, rollback, and release criteria.

## Sequencing rule

Do not advance a phase merely because its documents exist. Advance when the required decisions and acceptance criteria are approved and the remaining risks are understood.
