# Observability

## Status

No runtime or hosting environment has been selected. Observability requirements will depend on product criticality, operational ownership, privacy obligations, and availability targets.

## Future observability expectations

- Define the operational signals needed to detect failures and degraded user experience.
- Use structured logs with consistent severity, event names, and correlation context.
- Measure meaningful latency, throughput, error, saturation, and business outcomes once those outcomes are approved.
- Trace important cross-boundary operations when the architecture contains multiple services or providers.
- Redact secrets and minimize personal data in logs, metrics, traces, and support tooling.
- Establish ownership, alert thresholds, escalation paths, retention, and review procedures.
- Ensure health checks do not expose sensitive information or report false readiness.
- Monitor relevant queue health, failed jobs, throughput, retries, worker visibility, and long-running jobs through the approved Laravel Horizon capability.
- Monitor scheduled-task success, failure, overlap prevention, and missed-run behavior when scheduling enters scope.
- Record important audit events separately from ordinary debug logs and exclude secrets and unnecessary personal data.
- Include event, projection, webhook, and idempotency identifiers where they materially improve investigation.
- Cache observability SHOULD include hit, miss, stale-hit, regeneration count/duration, lock acquisition, lock contention, cache error, backend latency, negative-cache, authoritative-fallback, and stampede-indicator metrics.
- Marketplace commerce observability SHOULD distinguish Offer submission/review, import failures, click rate, Raw Click/Billable Click/charged outcomes, duplicate or rejected click classifications, billing failures, entitlement/Wallet consumption, suspicious traffic, redirect failures, broken Seller destination URLs, Store/location CTA activity, and future integration sync failures.

## Signal register

| Signal | Purpose | Owner | Status |
| --- | --- | --- | --- |
| Application errors | Detect unexpected failures. | TBD | Open |
| Request / operation latency | Detect degraded responsiveness. | TBD | Open |
| Dependency health | Detect external-system failures. | TBD | Open |
| Security events | Detect abuse or unauthorized access. | TBD | Open |
| Product outcomes | Validate approved success measures. | TBD | Open |

## Open operational questions

- Who owns production operations and incident response?
- What environments, retention periods, and alerting channels are required?
- Which data must never leave the application boundary?
