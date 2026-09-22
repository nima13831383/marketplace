---
change: 2026-09-22-architecture-authentication-clarification
status: completed
---

# Tasks

| ID | Behavior outcome | Depends on | Status | Acceptance | Verification | Handoff note |
|---|---|---|---|---|---|---|
| T1 | Existing architecture terminology identifies pragmatic MVC + Layered Architecture responsibilities. | — | done | AC-1, AC-2 | V1, V2 | Current docs use Application / Services, Business Logic, and Infrastructure / Data Access. |
| T2 | Future agents can distinguish Fortify workflows from Sanctum request authentication. | T1 | done | AC-3, AC-4, AC-5 | V1, V2 | See `docs/architecture/authentication.md`. |
| T3 | Product, security, engineering, roadmap, and agent guidance point to the current decisions. | T1, T2 | done | AC-1, AC-4 | V1, V2 | Product-specific policies remain open. |
| T4 | Documentation-only constraints and no-runtime status remain verifiable. | T1, T2, T3 | done | AC-6 | V3 | No production implementation started. |

## Decisions during implementation

- This documentation change is retained in place as a completed package because the repository had no pre-existing archive convention.
- A mutable current handoff and a chronological changelog were added to satisfy the control-plane evidence requirements for this Level 3 documentation change.
