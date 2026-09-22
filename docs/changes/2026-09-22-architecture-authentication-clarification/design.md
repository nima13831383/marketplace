---
change: 2026-09-22-architecture-authentication-clarification
status: completed
---

# Design

## Chosen approach

Update the existing Markdown source of truth rather than introducing application artifacts. The current architecture language uses four pragmatic responsibility areas: Presentation / HTTP, Application / Services, Business Logic, and Infrastructure / Data Access. The flow describes request handling and ownership of responsibilities; it is not a DDD dependency rule.

Laravel Fortify is documented as the headless authentication workflow capability. Laravel Sanctum is documented as the session/API/token authentication mechanism for appropriate boundaries. Authorization remains a separate backend concern governed by Policies, Gates, roles, permissions, ownership, and IDOR protections.

## Contracts and state

- Current architecture contract: [layered-architecture.md](../../architecture/layered-architecture.md).
- Authentication boundary: [authentication.md](../../architecture/authentication.md).
- Authorization boundary: [authorization.md](../../architecture/authorization.md).
- Security baseline: [security.md](../../architecture/security.md).
- The intended stateful browser sequence is documented as a Mermaid planning diagram only.
- No database, API, route, token, cookie, or package contract is finalized.

## Alternatives and tradeoffs

- **DDD as the organizing architecture:** rejected for this project; business/domain language remains useful, but DDD structures and ceremony are not mandatory.
- **A repository interface for every model:** rejected as a blanket rule; repositories and interfaces remain optional when they improve a real boundary.
- **Fortify or Sanctum alone:** rejected; Fortify covers workflows while Sanctum covers appropriate authenticated request mechanisms.
- **Choosing exact auth policy now:** deferred because product roles, clients, deployment, and regulatory requirements are not defined.

## Migration, rollout, and recovery

Not applicable. No runtime behavior or persisted data changed. When implementation is authorized, the chosen Laravel/package versions, configuration, rollout, rollback, and secret handling must be designed separately.

## External sources verified

No external source was required for this documentation round; the decisions recorded here are based on the explicit user-approved requirements supplied in the task.

## Related ADRs

- [ADR-0005](../../architecture/decisions.md#adr-0005--layered-architecture-with-mvc-at-the-http-boundary) — historical layered/MVC decision, clarified by ADR-0013.
- [ADR-0006](../../architecture/decisions.md#adr-0006--event-driven-architecture-where-appropriate) — Event-Driven Architecture remains approved.
- [ADR-0007](../../architecture/decisions.md#adr-0007--intentional-and-selective-event-sourcing) — selective Event Sourcing remains approved without DDD adoption.
- [ADR-0009](../../architecture/decisions.md#adr-0009--backend-authorization-and-idor-protection) — authentication remains separate from authorization.
- [ADR-0013](../../architecture/decisions.md#adr-0013--pragmatic-laravel-mvc-layered-architecture-ddd-not-adopted).
- [ADR-0014](../../architecture/decisions.md#adr-0014--fortify-and-sanctum-authentication-stack).

## Risks and mitigations

- **Risk:** Future agents infer DDD from terms such as domain events or aggregates. **Mitigation:** every affected document distinguishes descriptive business language from mandatory architecture and makes DDD exclusion explicit.
- **Risk:** Authentication is treated as authorization. **Mitigation:** the canonical authentication document links to the authorization boundary and repeats the required backend checks.
- **Risk:** Documentation is mistaken for installed runtime capability. **Mitigation:** the implementation gate is repeated in the agent guide, auth document, and change record.
