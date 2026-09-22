---
change: 2026-09-22-marketplace-commerce-core
status: verifying
---

# Tasks

| ID | Behavior outcome | Depends on | Status | Acceptance | Verification | Handoff note |
| --- | --- | --- | --- | --- | --- | --- |
| T1 | Canonical product and commerce architecture records define Master Product, Offer, Store, CTA, and billing terminology. | — | done | AC-1, AC-3, AC-4 | V1, V2 | Product and architecture sources are linked from the documentation map. |
| T2 | Store scope, moderation, locations, source normalization, and access boundaries are documented. | T1 | done | AC-2, AC-6 | V1, V2 | Active context remains separate from authorization. |
| T3 | Financial, security, cache, event, concurrency, queue, observability, and testing implications are synchronized. | T1, T2 | done | AC-3, AC-4, AC-5 | V1, V2 | Runtime financial verification remains unimplemented. |
| T4 | Content/support scope, roadmap, open questions, handoff, history, and implementation gate are synchronized. | T1, T2, T3 | done | AC-7, AC-8 | V1, V3 | Independent Level 3 review remains UNVERIFIED. |
| T5 | Offer matching-state, Store-derived destination, API abstraction, security, and test-planning refinement is synchronized. | T1, T2, T3, T4 | done | AC-9 | V4 | Independent Level 3 review remains UNVERIFIED. |

## Decisions during documentation work

- Two canonical documents were created to separate product flow from architecture constraints and prevent duplication.
- The package remains `verifying` until a separate design/security review can assess the financial and authorization model without implementation artifacts.
- The Offer-to-Master relationship was clarified as conditional before approval, not weakened for public Offers; Store mode remains the default source of destination capability.
