# Project Documentation

This directory contains the evolving specification and engineering guidance for the Marketplace project.

## Documentation map

### Product

- [Overview](product/overview.md) — product purpose and scope as currently known.
- [Requirements](product/requirements.md) — approved requirements and their status.
- [Marketplace core flow](product/marketplace-core-flow.md) — Master Products, Offers, visitor flow, CTA, billing, sources, and content/support scope.
- [User roles](product/user-roles.md) — approved actors and permissions.
- [User flows](product/user-flows.md) — approved end-to-end journeys.

### Architecture

- [Overview](architecture/overview.md) — system boundaries and architectural shape.
- [Decisions](architecture/decisions.md) — accepted architecture decisions and amendments.
- [Application surfaces](architecture/application-surfaces.md) — Admin, Seller, Customer, and API portal boundaries.
- [Marketplace commerce model](architecture/marketplace-commerce-model.md) — Store scope, catalog relationships, click/billing boundaries, and ingestion model.
- [Authentication](architecture/authentication.md) — Fortify/Sanctum responsibilities and open authentication policies.
- [Caching strategy](architecture/caching-strategy.md) — cache-aside, invalidation, stale reads, locks, TTL jitter, and safety boundaries.
- [Layered architecture](architecture/layered-architecture.md) — Laravel boundary responsibilities and dependency flow.
- [Event-driven and Event Sourcing strategy](architecture/event-driven-and-event-sourcing.md) — event categories, replay, projections, and outbox guidance.
- [Authorization strategy](architecture/authorization.md) — roles, permissions, policies, ownership, and IDOR protection.
- [Concurrency and idempotency](architecture/concurrency-and-idempotency.md) — transactions, duplicate execution, and race-condition guidance.
- [Domain model](architecture/domain-model.md) — domain vocabulary and lifecycle concepts.
- [Data model](architecture/data-model.md) — persistent data design and invariants.
- [API design](architecture/api-design.md) — service and integration contracts.
- [Security](architecture/security.md) — security expectations and threat considerations.

### Engineering

- [Coding guidelines](engineering/coding-guidelines.md) — conventions for future implementation.
- [Testing strategy](engineering/testing-strategy.md) — verification approach.
- [Error handling](engineering/error-handling.md) — error behavior and contract expectations.
- [Observability](engineering/observability.md) — logging, metrics, tracing, and operational signals.
- [Queue and scheduling](engineering/queue-and-scheduling.md) — queue security, Horizon, scheduling, retries, and monitoring.
- [Development workflow](engineering/development-workflow.md) — the documentation and future implementation workflow.

### Roadmap

- [Phases](roadmap/phases.md) — provisional delivery sequence.
- [Backlog](roadmap/backlog.md) — unresolved planning work.
- [Open questions](open-questions.md) — decisions needed before implementation.

### Control-plane records

- [Current handoff](handoff/current.md) — current cross-agent state and the next safe action.
- [Documentation changelog](history/CHANGELOG.md) — chronology of meaningful documentation rounds.
- [Completed architecture/authentication clarification](changes/2026-09-22-architecture-authentication-clarification/README.md) — proposal, design, tasks, and evidence for the current high-risk documentation round.
- [Completed application surfaces/identity/store clarification](changes/2026-09-22-application-surfaces-identity-store-model/README.md) — proposal, design, tasks, and evidence for the portal and identity round.
- [Marketplace commerce core](changes/2026-09-22-marketplace-commerce-core/README.md) — current Level 3 product/domain architecture change and verification state.

## Source-of-truth rules

- User-approved requirements are authoritative for product behavior.
- Accepted decisions in [architecture/decisions.md](architecture/decisions.md) are authoritative for architecture unless superseded by a later approved requirement.
- Focused architecture and engineering documents are canonical for their named topic and should be referenced rather than duplicated.
- Detailed documents explain the implications of requirements and decisions; they must not introduce contradictory behavior.
- `docs/architecture/` uses “domain” as business vocabulary where useful, but the approved project architecture is pragmatic MVC + Layered Architecture; DDD is not an implied or mandatory architecture.
- If two documents conflict, record the conflict, resolve it against the newest approved instruction, and update affected references in the same round.

## Incremental update protocol

For every new requirements round:

1. Read all existing Markdown documentation before editing.
2. Classify each new statement as a requirement, decision, assumption, risk, or open question.
3. Update every affected document, not only the document named by the user.
4. Add or amend an architecture decision when a durable technical choice is made.
5. Keep unsupported details as `TBD`, `Proposed`, or `Open`.
6. Perform a link and consistency review.
7. Return the required [Documentation Report](../AGENTS.md#change-workflow).

For Level 1–3 or otherwise high-risk documentation rounds, the repository uses a single `docs/changes/<change-id>/` package plus `docs/handoff/current.md`. Completed packages remain in place until an archive convention is intentionally adopted.

## Status vocabulary

- **Approved** — explicitly accepted by the user or by a later approved decision.
- **Proposed** — a recommendation awaiting approval.
- **Assumption** — a temporary working interpretation.
- **Open** — requires a product or technical decision.
- **Superseded** — retained for history but no longer current.
