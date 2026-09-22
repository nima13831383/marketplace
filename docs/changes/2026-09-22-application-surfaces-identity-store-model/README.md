---
id: 2026-09-22-application-surfaces-identity-store-model
kind: feature
level: 3
status: completed
created: 2026-09-22
updated: 2026-09-22
affected_areas:
  - application surfaces
  - authentication
  - authorization
  - identity
  - seller onboarding
  - stores and memberships
  - data model
  - security
---

# Application surfaces, identity, seller onboarding, and stores

## Summary

Record the approved Admin, Seller, Customer, and API surfaces; unify identity through one User classification; clarify account type versus authorization; document seller verification and lifecycle boundaries; and establish multi-Store, multi-user Store membership as a conceptual capability without creating implementation artifacts.

## Current state

The documentation update is complete. The repository remains documentation-only. No Filament, Livewire, Tailwind, route, panel, migration, upload flow, role system, or application logic exists.

## Package

- Proposal: [proposal.md](proposal.md)
- Design/contracts: [design.md](design.md)
- Tasks: [tasks.md](tasks.md)
- Verification: [verification.md](verification.md)

## Reviews

- Spec compliance: complete through current-truth and terminology review
- Code quality: not applicable; no production code changed
- Security/performance: complete as a documentation boundary review; runtime evidence is not applicable
- Independent review: not applicable to this documentation-only round

## Documentation updated

- Current truth: application-surfaces, product, architecture, authentication, authorization, security, engineering, roadmap, and open-question documents.
- History: [docs/history/CHANGELOG.md](../../history/CHANGELOG.md).
- Handoff: [docs/handoff/current.md](../../handoff/current.md).

## Remaining follow-ups

- Define product feature catalogs and exact permissions.
- Define authentication details, verification rules, MFA/OTP policy, and route middleware/configuration.
- Define seller verification document checklist and workflow decisions.
- Define Store lifecycle transitions, Store-level roles, membership permissions, and data-design details.
- Keep implementation blocked until explicit user authorization.
