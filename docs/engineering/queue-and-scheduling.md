# Queue and Scheduling Strategy

## Status

Laravel Horizon is **Approved** for relevant queue monitoring, and Laravel Task Scheduling is an **Approved** application capability. No queue, worker, Horizon, or scheduled-task implementation exists.

## Queue security

- Job payloads MUST be treated as trusted internal data only after their creation path is controlled and validated.
- Payloads SHOULD contain identifiers or minimal data rather than unnecessary sensitive records, secrets, tokens, or complete confidential request payloads.
- Delayed work MUST revalidate authorization or relevant business state when the security context or resource state may have changed.
- Jobs MUST define retry limits, backoff, timeout, failure visibility, and duplicate-execution behavior.
- Non-idempotent operations MUST use idempotency keys or another deduplication strategy before retry or replay is allowed.
- External calls MUST handle timeouts, provider failures, response validation, and safe retry behavior.

## Horizon monitoring

Horizon SHOULD be used for relevant visibility into queue health, failed jobs, throughput, retries, worker status, and long-running jobs. Operational ownership, alert thresholds, retention, and access controls remain open.

## Cache refresh workloads

Background cache refresh MAY use queues where it provides a clear benefit, but it is not mandatory for every cache. Refresh jobs MUST be idempotent, retry-bounded, observable, and protected from unbounded duplicate work. Cache regeneration MUST remain a read optimization and MUST NOT create unrelated financial or business side effects. See [caching-strategy.md](../architecture/caching-strategy.md).

## Task scheduling

Every scheduled task MUST document:

- Purpose and owner.
- Frequency and timezone assumptions.
- Failure handling, retry behavior, and alerting.
- Overlap behavior and concurrency safety.
- Idempotency or deduplication requirements.
- Data and authorization revalidation requirements.
- Observability and operational recovery.

Tasks that MUST NOT overlap require appropriate locking or non-overlap strategies. No single locking strategy is selected for every task.

## Pending decisions

- Queue topology, connection, worker, and environment model.
- Horizon access and production monitoring policy.
- Failed-job or dead-letter handling policy.
- Scheduled-task ownership, timezone, and deployment behavior.
- Whether click classification, Billing Decisions, analytics, moderation notifications, or WooCommerce processing are synchronous or queued; future queued work must preserve Store authorization, idempotency, and financial correctness.
