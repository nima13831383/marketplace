---
change: 2026-09-22-marketplace-commerce-core
status: verifying
---

# Proposal

## Problem and intent

The Marketplace needs a normalized catalog and Store Offer model that prevents duplicate search results, a Store-safe Seller operating context, and a financially safe commercial CTA model. The prior documentation did not define the relationship between canonical products, Offers, click observation, billing decisions, Subscription Entitlement, and Wallet / Prepaid Credit.

## Scope

### In scope

- Record Master Product as the Marketplace-controlled canonical product and Offer as a Store-specific commercial entity.
- Record one-level Master Product → Offer relationship and Master Product-first archive/search behavior.
- Record Seller Offer submission, Admin moderation, Master Product matching, and lifecycle separation.
- Record that a pre-approval Offer may be unmatched, while a publicly approved Offer requires a valid Master Product association.
- Record Active Store Context, Store modes, Store Location separation, and Store-scoped ingestion/operations.
- Record Store-derived Offer destination capability, conditional `external_product_url`, physical Store Location derivation, and conceptual API destination capability.
- Record explicitly billable CTA candidates, Raw Click/Billable Click/Billing Decision separation, central Billing Mode, and subscription-first priority.
- Record future WooCommerce ingestion through normalization, matching, review, and Store scope.
- Record high-level Articles, Comments, and Ticketing requirements.
- Synchronize authorization, security, cache, event, concurrency, queue, observability, testing, roadmap, handoff, and history guidance.

### Out of scope

- Implementation of any catalog, Offer, moderation, redirect, click tracking, billing, Wallet, entitlement, ledger, integration, CMS, comments, ticketing, queue, event, listener, job, test, or runtime configuration.
- Final fields, schemas, enum names, prices, quotas, plan catalog, fraud rules, SLA, workflow, provider, sync behavior, search ranking, API contract, or UI design.

## Actors and behavior

- Marketplace/Admin controls Master Product matching and Offer review.
- Sellers submit Store-scoped Offers manually and may later use WooCommerce ingestion.
- Visitors discover Master Products, select eligible Offers, and may invoke an explicit commercial CTA.
- The system observes and classifies the interaction before any funding decision.

## Acceptance criteria

- [x] AC-1: Master Product and Offer terminology, one-level relationship, and Master Product-first discovery are documented.
- [x] AC-2: Offer moderation, canonical matching, eligibility, Store modes, locations, and Active Store Context are documented.
- [x] AC-3: CTA observation, Billable Click classification, Billing Decision, and non-billable interaction boundaries are documented.
- [x] AC-4: Billing Mode, Subscription Entitlement/Wallet distinction, and subscription-first precedence are documented.
- [x] AC-5: Financial correctness, anti-abuse, authorization, caching, event, queue, and observability constraints are synchronized.
- [x] AC-6: Manual submission and future WooCommerce ingestion share normalization/matching/review/Store-scope guidance.
- [x] AC-7: Articles, Comments, and Ticketing are documented at high level without invented workflows.
- [x] AC-8: Deferred decisions remain explicit and implementation gate stays closed.
- [x] AC-9: Seller-created Offer semantics, conditional Master Product association, Store-derived destinations, and non-final API destination abstraction are documented.

## Invariants

- An Offer belongs to one Store. It may be unmatched before approval/review, but a publicly approved Offer requires one valid Master Product association; Master Product/Offer recursion is not approved.
- Store mode determines default Offer destination capability; a duplicate Offer/product type is not approved by default.
- `external_product_url` is conditional for online-capable Offers; physical destinations derive from Store Location.
- Store context is not authorization.
- A raw CTA interaction is not automatically billable or charged.
- Only configured commercial CTA types are candidates for billing.
- Subscription Entitlement and Wallet / Prepaid Credit are separate; usable entitlement is preferred in `subscription_or_wallet`.
- A qualified interaction must not consume quota/credit twice.
- Cache is not authoritative for financial, quota, authorization, or click-charging state.

## Edge cases and negative paths

- Duplicate/retried/replayed clicks, crawler/bot activity, Seller self-clicks, suspicious automation, and concurrent processing must not produce blind duplicate charging.
- Pending/rejected/suspended Offers and Store/Seller state may alter Offer visibility; exact policy is open.
- Active Store switching, source ingestion, and Seller operations cannot reveal or mutate another Store's data.
- External destination URLs and location data require security/privacy boundaries before implementation.
- A pending unmatched Offer must not become public or grant Seller authority over canonical Master Product data.

## Permissions and data handling

Offer, Store, moderation, click, financial/quota, integration, location, comment, and ticket data all require future authorization, retention, audit, and privacy decisions. The change records no personal or financial data and creates no persistence.

## Open decisions

All field definitions, statuses, pricing, plans, quota, balance rules, insufficient-funds behavior, ledger shape, fraud policy, sync/redirect boundaries, map provider, integration contract, search/ranking, comment moderation, ticket workflow, and Event Sourcing selection remain open.
