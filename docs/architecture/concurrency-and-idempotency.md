# Concurrency, Transactions, and Idempotency

## Status

Transaction safety, idempotency, and race-condition awareness are **Approved mandatory concerns**. Concrete schemas and locking strategies remain domain-specific and open.

## Consistency boundaries

The design MUST consider consistency between:

- Database writes.
- Domain and application event publication.
- Queue publication and worker execution.
- External network calls and provider callbacks.

Atomic business operations SHOULD use database transactions where appropriate. Transactions MUST NOT be treated as a substitute for distributed consistency, and external network calls SHOULD NOT be placed casually inside long-running database transactions.

The Transactional Outbox Pattern is an approved pattern to use where necessary; it is not implemented or mandated for every operation.

Cache regeneration is also concurrency-sensitive. Distributed cache locks MUST be scoped to the cache resource, bounded in lifetime, observable, and safe across future application instances. Lock contention or acquisition failure MUST have a bounded fallback; no request may wait indefinitely for a rebuild. See [caching-strategy.md](caching-strategy.md).

## Idempotency

Operations where duplicate execution could create incorrect state MUST be idempotent or use an explicit deduplication strategy. This is especially important for financial workflows, payments, order transitions, inventory mutation, webhooks, queue jobs, retries, and external integrations.

Future designs MAY use idempotency keys, unique constraints, persisted operation records, event identifiers, or other mechanisms appropriate to the domain. Final schemas MUST NOT be invented before the domain is approved.

## Concurrency and race conditions

Future designs MUST explicitly consider races in inventory, order creation, payment processing, wallet balances, settlement, discount redemption, stock reservation, and webhook handling where those concepts enter scope.

Future commercial CTA billing must also consider Raw Click duplication, retries, queue redelivery, event duplication, concurrent Billing Decisions, Subscription Entitlement consumption, and Wallet / Prepaid Credit consumption. The same qualified interaction MUST NOT consume entitlement or credit twice.

Potential strategies include database constraints, unique indexes, transactions, pessimistic or optimistic locking, atomic database operations, and idempotency. No single strategy is selected for every domain.

The exact click identity, deduplication window, ledger design, reservation behavior, reconciliation, insufficient-funds behavior, and sync/async processing boundary remain open. Cache locks do not replace authoritative transaction or concurrency controls for financial/quota state.

Cache stampede protection, synchronized expiration, hot-key handling, and mass-expiration load protection MUST be considered alongside database concurrency rather than treated as unrelated performance work.

## Required verification

Concurrency-sensitive behavior MUST have tests for duplicate delivery, competing updates, rollback, retry, and recovery behavior as applicable. See [testing-strategy.md](../engineering/testing-strategy.md).
