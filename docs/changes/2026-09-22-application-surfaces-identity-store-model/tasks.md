---
change: 2026-09-22-application-surfaces-identity-store-model
status: completed
---

# Tasks

| ID | Behavior outcome | Depends on | Status | Acceptance | Verification | Handoff note |
|---|---|---|---|---|---|---|
| T1 | Four application surfaces and shared Laravel boundaries are documented. | — | done | AC-1, AC-2, AC-7 | V1, V2 | See `docs/architecture/application-surfaces.md`. |
| T2 | Unified identity and account-type classification are separated from authorization. | T1 | done | AC-3, AC-4 | V1, V2 | Roles, permissions, ownership, membership, Policies, and Gates remain distinct. |
| T3 | Seller onboarding, verification, lifecycle, Store lifecycle, and membership boundaries are documented. | T2 | done | AC-5, AC-6 | V1, V2 | Exact fields, documents, statuses, and permissions remain open. |
| T4 | Testing, security, roadmap, open questions, handoff, and implementation gate are synchronized. | T1, T2, T3 | done | AC-7, AC-8, AC-9 | V1, V3 | No runtime artifact was created. |

## Decisions during implementation

- This is a new completed change package; the previous authentication clarification package remains historical and unchanged.
- No frontend specialist execution was invoked because the request is documentation-only and explicitly prohibits implementation.
