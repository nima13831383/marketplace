# Error Handling

## Status

No runtime error contract has been approved. The rules below are design expectations to apply after the system boundaries and API style are known.

## Expected principles

- Distinguish validation, authentication, authorization, missing-resource, conflict, dependency, and unexpected failures.
- Return stable machine-readable categories where a client needs to react.
- Give users safe, actionable messages without leaking secrets, stack traces, or internal topology.
- Preserve diagnostic context in protected logs and correlate related requests or jobs when applicable.
- Make retryability explicit; do not retry non-idempotent operations blindly.
- Keep partial failure and compensation behavior visible for multi-step operations.
- Fail closed at security boundaries and fail predictably at external-service boundaries.
- Authorize access before reading or mutating sensitive resources; do not turn a missing authorization result into a generic resource lookup.
- Queue, event, webhook, and scheduled-task failures MUST make retryability, duplicate execution, and failed-job handling explicit.
- Event replay and projection rebuild errors MUST be observable and MUST NOT blindly repeat external side effects.

## Pending decisions

- Error envelope and transport mapping.
- Public versus internal error detail.
- Correlation and request identifiers.
- Retry, timeout, circuit-breaker, and dead-letter behavior.
- User notification and support escalation behavior.
