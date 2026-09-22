---
id: 2026-09-22-architecture-authentication-clarification
kind: refactor
level: 3
status: completed
created: 2026-09-22
updated: 2026-09-22
affected_areas:
  - architecture
  - authentication
  - authorization
  - security
  - engineering guidance
  - roadmap
---

# Architecture and authentication clarification

## Summary

Align the current Marketplace documentation with the approved pragmatic Laravel MVC + Layered Architecture + SOLID direction, explicitly exclude DDD as the project architecture, and record Laravel Fortify plus Laravel Sanctum as the approved complementary authentication stack without starting implementation.

## Current state

The documentation update is complete. Current truth, architecture decisions, security guidance, testing expectations, roadmap references, and the agent guide are synchronized. The repository remains documentation-only; no runtime authentication or architecture code exists.

## Package

- Proposal: [proposal.md](proposal.md)
- Design/contracts: [design.md](design.md)
- Tasks: [tasks.md](tasks.md)
- Verification: [verification.md](verification.md)

## Reviews

- Spec compliance: complete through documentation consistency review
- Code quality: not applicable; no production code changed
- Security/performance: complete as a documentation threat-boundary review; runtime verification is not applicable
- Independent review: not applicable to this documentation-only round

## Documentation updated

- Current truth: completed in the linked architecture, product, engineering, roadmap, and security documents.
- History: recorded in [docs/history/CHANGELOG.md](../../history/CHANGELOG.md).
- Handoff: recorded in [docs/handoff/current.md](../../handoff/current.md).

## Remaining follow-ups

- Resolve the product-specific authentication flows and verification policy.
- Resolve mandatory MFA, session/token lifecycle, client boundaries, and exact rate limits.
- Resolve product scope before defining roles, resources, and authorization matrices.
- Keep implementation blocked until explicit user authorization.
