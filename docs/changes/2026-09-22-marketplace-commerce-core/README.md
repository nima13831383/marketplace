---
id: 2026-09-22-marketplace-commerce-core
kind: feature
level: 3
status: verifying
created: 2026-09-22
updated: 2026-09-22
affected_areas:
  - marketplace catalog
  - offers and moderation
  - store scope and locations
  - click tracking and billing
  - subscription entitlement and wallet credit
  - WooCommerce ingestion direction
  - content and ticketing
  - authorization, security, cache, events, concurrency, queue, observability
---

# Marketplace commerce core

## Summary

Document the Master Product → Offer commerce model, including pre-approval matching, Store-derived destination capability, Store-scoped operations, commercial CTA accounting, central Billing Mode, subscription-first funding priority, future WooCommerce ingestion boundary, and high-level Articles, Comments, and Ticketing requirements without creating implementation artifacts.

## Current state

Current-truth documentation and architecture decisions are synchronized. This Level 3 documentation package is in `verifying` status because runtime behavior is intentionally absent and independent review is unavailable in the current task environment. No production source, schema, integration, billing logic, queue, event, job, test, or configuration exists.

## Package

- Proposal: [proposal.md](proposal.md)
- Design/contracts: [design.md](design.md)
- Tasks: [tasks.md](tasks.md)
- Verification: [verification.md](verification.md)

## Reviews

- Spec compliance: complete through documentation review
- Code quality: not applicable; no production code changed
- Security/performance: documentation boundary review complete; runtime behavior unverified
- Independent review: **UNVERIFIED**; no separate reviewer is available in this documentation-only task

## Documentation updated

- Current truth: product, commerce architecture, data, API, authorization, security, cache, event, concurrency, queue, observability, engineering, roadmap, and agent guidance.
- History: [docs/history/CHANGELOG.md](../../history/CHANGELOG.md).
- Handoff: [docs/handoff/current.md](../../handoff/current.md).

## Remaining follow-ups

- Obtain an independent design/security review before treating the financial architecture as implementation-ready.
- Resolve commercial fields, destination/API, pricing, fraud, ledger, moderation, integration, search, and content/support policy questions.
- Keep implementation blocked until explicit user authorization.
