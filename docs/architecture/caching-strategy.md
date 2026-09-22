# Caching Strategy

## Status

The hybrid caching strategy is **Approved** as the default architectural baseline. This document defines architectural requirements only; no cache backend, Laravel cache store, Redis configuration, lock API, cache service, job, listener, or executable implementation has been created.

## Approved default

```text
Cache-Aside + Event-Driven Invalidation + Stale-While-Revalidate
+ Distributed Cache Locks + TTL Jitter
```

Caching is an optimization layer. It MUST NOT become the authoritative source of truth for transactional, financial, inventory, authorization, or other security-sensitive state.

## 1. Cache-Aside

Cache-Aside is the default read-caching pattern where it provides a clear benefit:

```text
Request -> Cache lookup -> hit: return cached value
Request -> Cache lookup -> miss: load authoritative data -> populate cache -> return
```

Application and domain data remain authoritative outside the cache. Caching MAY reduce database load, repeated expensive computation, safe external-service calls, or read latency, but it MUST NOT be mandated for every query.

## 2. Stampede protection and distributed locks

The architecture MUST protect hot keys from cache stampedes. A coordinated miss SHOULD follow this conceptual flow:

```text
Cache miss -> attempt distributed lock -> one rebuilder -> cache refresh
```

Distributed locks used for cache regeneration MUST:

- Be scoped to the cache resource or key.
- Have a bounded lifetime and never remain indefinitely.
- Define lock-acquisition failure and fallback behavior.
- Protect ownership and release semantics.
- Expose contention and failures to observability.
- Avoid creating an indefinite request queue.

The coordination mechanism MUST work across future load-balanced application instances. Process-local locking alone is insufficient for a shared cache namespace. The final Laravel or Redis implementation API and lock values remain open.

## 3. Stale-While-Revalidate and blocking behavior

Stale-While-Revalidate is approved where slightly stale data is acceptable:

```text
Fresh cache -> return
Stale cache -> return stale value + allow one refresh path
Missing cache -> coordinated rebuild
```

Potential candidates include public product/catalog reads, category pages, public seller profiles, aggregate counters, public metadata, and expensive public read models. These are candidates, not final product commitments.

Where stale data is unacceptable, a domain MAY use blocking mode: coordinate on the rebuild lock, wait only within a bounded timeout, and then retry or fall back according to the domain policy. No request MUST wait indefinitely for cache regeneration. Exact wait and stale-window values remain open.

## 4. Invalidation

Cache invalidation SHOULD be triggered by meaningful application or domain state changes and centralized in dedicated Application/Infrastructure components. Controllers MUST NOT coordinate invalidation across the system manually.

Two complementary invalidation forms are approved:

1. **Direct invalidation** for an obvious, tightly scoped resource key.
2. **Event-driven invalidation** for dependent or broader cache families.

The default invalidation model is:

```text
Direct resource invalidation + event-driven dependent invalidation + TTL fallback
```

TTL MUST NOT be the only invalidation mechanism for important mutable data where explicit invalidation is practical. Cache-affecting invalidation SHOULD occur only after authoritative state commits. Where the Transactional Outbox Pattern is used, cache-affecting events MUST fit that consistency model.

No final event names or implementation classes are approved.

## 5. TTL jitter and cache key versioning

TTL jitter SHOULD be used where synchronized expiration is a risk:

```text
effective TTL = base TTL + bounded random jitter
```

Exact TTL and jitter values MUST be selected per cache category or domain; there is no universal value.

Cache keys SHOULD be predictable, scoped, collision-resistant, versionable, and free of unnecessary sensitive data. User, tenant, resource, and authorization boundaries MUST be represented where required. Versioned or namespaced keys MAY support representation changes and coarse-grained invalidation. Entry invalidation, namespace/version invalidation, and TTL expiration are distinct mechanisms.

Cache tags MAY be considered only if the selected backend and Laravel architecture support them reliably. Cache tags are not a mandatory dependency.

## 6. Domain-specific safety policies

| Data category | Permitted cache role | Correctness rule |
| --- | --- | --- |
| Public product/catalog data | Cache-Aside, stale-while-revalidate, event invalidation, locks, TTL jitter | Candidate only; choose policy per freshness and load needs. |
| Price | Display/read optimization | Revalidate against authoritative data for critical order or checkout decisions. Cached display data MUST NOT determine financial correctness. |
| Inventory / stock | Read optimization | State-changing operations MUST verify authoritative stock and follow concurrency rules. Cache MUST NOT be the sole basis for checkout correctness. |
| Authorization / permissions | Conservative read optimization | Prompt invalidation is required after security changes. Cache MUST NOT replace Policy, Gate, ownership, or backend authorization checks. |
| Wallet / balance / payment / ledger / settlement | Carefully controlled read optimization only | The authoritative transactional system remains the source of truth. |
| Subscription entitlement / click billing / billing decision | Carefully controlled read optimization only | Cache MUST NOT determine eligibility, funding source, quota consumption, Wallet consumption, or duplicate-charge prevention. |
| Master Product archive / eligible Offer list | Cache-Aside and stale-while-revalidate candidate | Candidate only; invalidate after authoritative catalog, Offer, Store, or visibility state commits. |
| Configuration / reference data | Longer-lived cache where appropriate | Combine bounded TTL with explicit invalidation. |

## 7. Penetration, hot keys, and load protection

The architecture MUST consider cache penetration. Negative caching MAY protect the authoritative backend from repeated requests for nonexistent resources, with bounded domain-appropriate TTLs.

The architecture MUST consider hot keys and mass expiration during traffic spikes, cache restarts, deployments, namespace changes, and popular-resource traffic. Possible mitigations include stale serving, coordinated locks, gradual warming, bounded fallback concurrency, local/request-level deduplication, asynchronous refresh, and database/load protection. Sharding and prewarming MUST NOT be mandated for every cache.

## 8. Cache failure and safe degradation

If the cache is unavailable, the application MUST degrade safely. Cache failure MUST NOT normally make authoritative business data unavailable when the source of truth is healthy.

- Cache errors MUST NOT corrupt business state.
- Sensitive writes MUST NOT depend on cache success.
- Read paths SHOULD have controlled authoritative fallback where practical.
- Repeated cache failures MUST be observable.
- Fallback behavior MUST avoid an uncontrolled database stampede.

Cache outage fallback and stampede protection MUST be designed together.

## Marketplace commerce implications

Master Product archives and eligible Offer lists are likely read-cache candidates once product freshness rules are approved. Cache keys and invalidation must respect Master Product, Offer, Store, Store Location, lifecycle, and public-visibility boundaries.

Active Store Context, Offer authorization, Store membership, Raw Click classification, Billing Decision, Subscription Entitlement, and Wallet / Prepaid Credit MUST use authoritative authorization or transactional state. Cache MUST NOT be used as a billing ledger, quota reservation mechanism, charge deduplication mechanism, or source of truth for commercial CTA eligibility.

## 9. Transactions and Event Sourcing

The application MUST NOT update a cache before the related database transaction is known to have succeeded. A safe conceptual sequence is:

```text
Database transaction commits -> publish/handle event -> invalidate or refresh cache
```

For Event-Sourced domains, the event store remains authoritative. Projections and read models MAY be cached, but cache invalidation MUST follow projection/read-model lifecycle. Replay and projection rebuilds MUST NOT trigger uncontrolled external side effects, and a rebuild MAY require cache invalidation or a namespace/version change.

Cache entries MUST NOT be treated as Event Sourcing state.

## 10. Regeneration and background refresh

Cache regeneration MUST be safe under retries and concurrency. It SHOULD be deterministic where practical, have bounded lock duration, define refresh failure fallback, and avoid duplicate financial or business side effects. Regeneration MUST remain a read optimization and MUST NOT mutate unrelated business state.

Background refresh MAY use a queue, scheduled task, event-triggered refresh, or lazy request-triggered refresh where appropriate. It is not mandatory for every cache. Refresh work MUST be idempotent, observable, retry-bounded, and protected from unbounded duplicate work. If queues are used, relevant health signals SHOULD be visible through the approved Horizon capability.

## 11. Cache security

- Sensitive data SHOULD NOT be cached unnecessarily.
- Sensitive cache entries MUST have appropriate isolation.
- User-specific and tenant-specific entries MUST NOT leak across security boundaries.
- Authorization context MUST be included where required.
- Secrets MUST NOT be placed in ordinary application cache.
- Cached HTML MUST still follow sanitization and secure-rendering requirements.
- Cache keys SHOULD NOT expose unnecessary sensitive data.
- Security-sensitive invalidation, including role changes, permission changes, account or seller suspension, access revocation, and policy-relevant changes, MUST receive higher priority than ordinary content freshness.

A stale authorization cache MUST NOT preserve revoked privileges beyond an explicitly accepted security window. That window remains undecided.

## 12. Observability

Caching MUST be observable. Future metrics SHOULD include:

- Cache hit, miss, and stale-hit rates.
- Regeneration count and duration.
- Lock acquisition success, failure, and contention.
- Cache errors and backend latency.
- Negative-cache hit rate where used.
- Authoritative fallback rate.
- Cache stampede indicators.

Exact tooling, alert thresholds, ownership, and retention remain open.

## 13. Consistency classification

Future cacheable data SHOULD be classified before implementation:

- **Strong correctness required:** cache is advisory only; authoritative state is consulted for decisions and writes.
- **Short-lived eventual consistency acceptable:** stale-while-revalidate MAY be appropriate.
- **Longer-lived reference/static data:** longer TTL plus explicit invalidation MAY be appropriate.

This classification prevents one universal cache policy from being applied to every domain.

## Open implementation decisions

- Cache backend and whether Redis will be the default.
- Laravel cache-store configuration and supported runtime topology.
- Exact TTLs, jitter ranges, stale windows, lock lifetimes, and bounded wait timeouts.
- Lock-acquisition failure fallback by domain.
- Synchronous versus asynchronous refresh and prewarming needs.
- Cache tag usage.
- Namespace/version naming conventions and eviction policy.
- Capacity planning, multi-server topology, and whether separate cache clusters are required.
- Metrics and alerts that define cache stampede conditions.
- Negative-caching policies and domains that must never be cached.

Do not invent answers to these questions.
