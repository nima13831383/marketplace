# Event-Driven Architecture and Event Sourcing

## Status

Event-Driven Architecture is **Approved** where appropriate. Event Sourcing is an approved capability, but its scope is intentionally unresolved. This document is planning guidance only.

## Event categories

- **Framework events:** Laravel or framework lifecycle notifications. They are not automatically domain events.
- **Application events:** Notifications that describe an application workflow or use-case outcome.
- **Domain events:** Business facts emitted by domain behavior after a meaningful state transition.
- **Integration events:** Events shaped for another application module, service, provider, or external consumer.

Each category SHOULD have an explicit owner, contract, delivery expectation, and failure policy. Side effects SHOULD be decoupled from core business decisions where appropriate.

Cache invalidation SHOULD be treated as a state-change side effect coordinated from meaningful application or domain events after authoritative state commits. Cache invalidation MUST NOT be scattered through controllers, and no final cache event names are approved.

## Event-driven expectations

Listeners and queued consumers that can be retried MUST be idempotent or use an explicit deduplication strategy. Important asynchronous workflows MUST document:

- Retry and backoff behavior.
- Failure handling and failed-job or dead-letter treatment where relevant.
- Idempotency and duplicate-delivery behavior.
- Observability, correlation, and alerting.
- Authorization or security-context revalidation when work is delayed.

## Event Sourcing scope

Event Sourcing SHOULD be applied intentionally to domains where historical state transitions, auditability, replayability, or high-value transactional/state correctness justify the complexity. It MUST NOT be assumed for every CRUD entity.

Potential candidates include orders, payments, wallets, settlements, disputes, or other high-value transactional domains. These are candidates only and are not approved product requirements.

The following decision remains open:

- Will Event Sourcing apply to the full system, or only to selected business areas/workflows?

“Domain event” is descriptive business language here and does not imply DDD. Bounded contexts, aggregate roots, and other DDD structures are not required by this project architecture.

## Event Sourcing principles

Where Event Sourcing is selected:

- Business events MUST be immutable after persistence.
- Event schemas MUST support explicit versioning and evolution.
- Projections/read models SHOULD remain conceptually separate from the event store.
- Projections SHOULD be rebuildable from event history where applicable.
- Event handlers MUST be replay-safe where necessary.
- Idempotency MUST be considered during replay and asynchronous processing.
- Projection rebuilds MUST NOT blindly re-execute external side effects.
- Operational and audit expectations for event history MUST be defined before implementation.

## Transactional publication

The design MUST prevent inconsistent outcomes between database writes, domain/application events, queue publication, and external side effects. Database transactions, idempotency, and the Transactional Outbox Pattern are approved patterns to consider where necessary. No outbox implementation exists yet.

For cache-affecting events, the same consistency model applies: authoritative state commits before invalidation or refresh is published/handled where appropriate.

## Marketplace commerce alignment

Potential conceptual business facts include Seller approved, Store activated, Offer submitted, Offer approved, Offer rejected, CTA observed, Billable Click accepted, Subscription Entitlement consumed, and Wallet / Prepaid Credit consumed. These are examples only; no event class names, payloads, contracts, ordering guarantees, or listener behavior are approved.

Event-Driven Architecture may support moderation, cache invalidation, analytics, reconciliation, or future asynchronous commerce work. Event Sourcing is not automatically required for every click, Offer, Store, or CRUD operation. Click/billing workflows should be evaluated later for auditability and correctness needs before any Event Sourcing choice is made.
