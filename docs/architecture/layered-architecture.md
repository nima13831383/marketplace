# Layered Architecture

## Status

The pragmatic MVC + Layered Architecture + SOLID direction is **Approved**. The document describes responsibilities for future Laravel implementation; it does not authorize implementation.

## Responsibility and request flow

```text
Presentation / HTTP -> Application / Services -> Business Logic -> Infrastructure / Data Access
```

The flow describes how a request may be handled and where responsibilities usually live. It is not a mandatory DDD dependency graph. The project does not adopt DDD, Clean Architecture, Hexagonal Architecture, or Onion Architecture as its organizing architecture. Repositories and interfaces are optional, and Eloquent MAY be used when direct framework-backed data access is the clearest pragmatic choice.

## Presentation / HTTP layer

This is where MVC primarily applies. Responsibilities include:

- Controllers and HTTP request handling.
- Laravel Form Request validation and authentication context.
- Response transformation and transport-specific concerns.
- Invoking Application-layer use cases.

Controllers MUST remain thin. They MUST NOT contain business rules and MUST NOT become orchestration layers for large workflows. Authorization still MUST be enforced on the backend; UI visibility and route-model binding are not authorization.

## Application / Services layer

Responsibilities include:

- Application use cases and workflow orchestration.
- Coordinating business operations.
- Transaction boundaries where appropriate.
- Invoking data-access or external-service boundaries where appropriate.
- Dispatching application or business events where appropriate.
- Coordinating cache invalidation, queue work, and integration side effects after authoritative state changes.

Application services coordinate workflows but MUST NOT become a dumping ground for rules that belong in reusable Business Logic.

## Business Logic layer

Responsibilities include:

- Business rules and invariants.
- Business state transitions and decisions.
- Focused rules, actions, strategies, policies, or model behavior where useful.
- Business events where a meaningful state change must be published.
- Event-sourced state only where Event Sourcing is intentionally selected.

Business logic SHOULD remain independent of HTTP and external providers wherever practical. It MAY use Laravel or Eloquent when that keeps the design clear and avoids speculative abstraction; a feature-level decision should document material tradeoffs.

## Infrastructure / Data Access layer

Responsibilities include:

- Persistence and database implementations.
- Eloquent models, query/data-access objects, and database transactions where appropriate.
- Caching, queues, and filesystem/storage.
- External APIs, search, mail/SMS providers, and third-party integrations.
- Framework-specific adapters and operational tooling.

Infrastructure code MUST handle unreliable boundaries explicitly. Provider-specific behavior should not spread across business rules without a documented reason.

## SOLID guidance

SOLID is mandatory design guidance:

- **Single Responsibility:** a component SHOULD have one coherent reason to change.
- **Open/Closed:** extend behavior through stable boundaries where practical.
- **Liskov Substitution:** implementations of an abstraction MUST preserve its contract.
- **Interface Segregation:** consumers SHOULD depend on focused interfaces.
- **Dependency Inversion:** policy code SHOULD depend on abstractions rather than concrete infrastructure.

SOLID MUST NOT be used as an excuse for speculative layers or abstractions. Introduce abstractions when they improve a real boundary, testability, replaceability, or business clarity. Do not require a repository, interface, aggregate, value object, or separate domain package for every concept.

## Explicit non-architecture requirements

“Domain” remains useful as a descriptive term for business concepts and business rules. It does not mean the project is adopting Domain-Driven Design. Event-Driven Architecture and selective Event Sourcing are independent capabilities and MUST NOT be used as evidence that DDD patterns are required.
