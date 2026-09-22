# Marketplace Project

This repository is currently in the planning and documentation phase for a future Marketplace application. The approved backend framework is Laravel; no Laravel version has been selected.

## Current status

**Documentation-only mode is active.** No application source code, runtime dependency, database migration, or executable application logic has been created. Implementation may begin only after an explicit instruction equivalent to **“Start implementation”**.

## Start here

- [Agent operating guide](AGENTS.md)
- [Documentation map and maintenance process](docs/README.md)
- [Product requirements](docs/product/requirements.md)
- [Marketplace core flow](docs/product/marketplace-core-flow.md)
- [Architecture overview](docs/architecture/overview.md)
- [Marketplace commerce model](docs/architecture/marketplace-commerce-model.md)
- [Application surfaces](docs/architecture/application-surfaces.md)
- [Authentication](docs/architecture/authentication.md)
- [Caching strategy](docs/architecture/caching-strategy.md)
- [Layered architecture](docs/architecture/layered-architecture.md)
- [Event-driven and Event Sourcing strategy](docs/architecture/event-driven-and-event-sourcing.md)
- [Authorization strategy](docs/architecture/authorization.md)
- [Security expectations](docs/architecture/security.md)
- [Open questions](docs/open-questions.md)

High-risk documentation rounds may also include a control-plane change record in [docs/changes/](docs/changes/) and a current handoff in [docs/handoff/current.md](docs/handoff/current.md). These files do not authorize implementation.

## How this repository is being developed

Requirements will arrive incrementally. Each approved round must be merged into the existing documentation without silently discarding earlier decisions. Unknown product or technical details must remain visibly marked as pending, assumed, or open.

The documentation is the current source of truth for the future coding workflow. See [AGENTS.md](AGENTS.md) for the concise rules that coding agents must follow once implementation is authorized.
