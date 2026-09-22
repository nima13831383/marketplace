# Marketplace Commerce Model

## Status

This is the canonical architecture record for the approved Marketplace commerce relationships, Store scope, commercial CTA accounting, billing boundaries, and ingestion path. It is conceptual documentation only; no schema, ledger, redirect, integration, queue, event, cache, or billing implementation exists.

## Conceptual relationship model

```text
User
  -> Store Membership
  -> Store
  -> Store Location(s)

Store
  -> Offer(s)

Offer
  -> one Store
  -> zero or one Master Product while pre-approval/review
  -> one valid Master Product when publicly approved
  -> commercial CTA(s)

Master Product
  -> many Offers

CTA interaction
  -> Raw Click
  -> classification
  -> possible Billing Decision
```

The full product terminology and visitor behavior are owned by [marketplace-core-flow.md](../product/marketplace-core-flow.md). This document owns architecture boundaries and invariants.

## Active Store Context

The Seller Panel requires an Active Store Context for Sellers who participate in multiple Stores. Seller-scoped data such as Offers, analytics, clicks, Store settings, Store integrations, and future billing/credit information must be scoped to the intended Store where appropriate.

The active context is a usability and request-scoping aid. It MUST NOT replace backend authorization. Every Store-scoped read or mutation still requires authorization against the actual target Store, membership, applicable role/permission, ownership, lifecycle, and Policy/Gate decision.

Future Store-switching UX, persistence of selected context, default selection, and handling of removed/suspended membership remain open.

## Store modes and locations

Store mode must support the conceptual values `online`, `physical`, and `hybrid`; the persistence representation is open.

- Store mode determines the default commercial destination capabilities of its Offers. It MUST NOT be duplicated as a separate persistent `offer_type` or `product_type` without a future explicit Offer-level override requirement.
- Online/Hybrid Offers may have product-specific `external_product_url` values; Physical-only Offers may have that value absent.
- Physical/Hybrid Offers may expose location-related CTAs and resolve commercial Store Location information through `Offer -> Store -> Store Location`, without duplicate location values per Offer.
- Store Location is separate from Store and must support forward-compatible multiple branches.

Store commercial location is distinct from private User location. Exact location schema, map provider, navigation behavior, and multi-location rollout are open.

## Master Product and Offer invariants

- A Master Product is the Marketplace-controlled canonical product entity.
- An Offer is Store-specific and belongs to exactly one Store conceptually.
- An Offer MAY have no Master Product association during submission/review. A publicly approved/published Offer MUST belong to one valid Master Product.
- Master Product → Offer is one level; recursive product trees are not approved for this relationship.
- Master Product archives/search display Master Products by default; Offers appear at the relevant Master Product detail level.
- Offer eligibility is independent from Master Product existence. Seller, Store, and Offer state may affect Offer visibility.
- Offer moderation and Master Product matching are Marketplace/Admin-controlled boundaries.
- Master Product data is canonical Marketplace data; Offer data is Seller/Store-specific commercial data. Sellers must not independently mutate canonical Master Product data outside an approved workflow.

## Destination capability and API boundary

The API and other presentation boundaries should expose meaningful destination capabilities rather than require consumers to interpret unrelated nullable fields as equivalent business data. For example, an online capability may be expressed conceptually as `external_product`; a physical capability may be expressed conceptually as `store_location`; and a Hybrid context may expose multiple applicable actions. This is a conceptual destination abstraction only. It does not approve a final JSON shape, field names beyond the preferred `external_product_url` concept, routes, or response contract.

## Source and moderation boundary

Manual Offer submission and future WooCommerce ingestion both enter the same conceptual pipeline:

```text
Source -> Normalize -> Master Product match -> Review/approval -> Publish Offer
```

Imported or submitted Offers MUST be scoped to the intended Store. Source integration must not bypass normalization, moderation, canonical-product matching, Store authorization, or lifecycle checks.

WooCommerce import versus sync, source credentials, transport, synchronization fields, polling/webhooks, conflict handling, and manual override rules remain open.

## CTA, click, and billing boundary

Only configured commercial CTA types are candidates for billing. A CTA click is not automatically a charge.

```text
CTA Click
  -> Raw Click record/observation
  -> validation and classification
  -> Billable Click decision
  -> Billing Decision
  -> no charge | entitlement consumption | Wallet/prepaid consumption
```

The architecture MUST keep observation, classification, and financial consumption separate. Tracking remains available in non-charging mode and must not be implemented as direct mutation of a Wallet or Subscription value in a presentation handler.

For external Offer destinations, the future design must support a controlled Marketplace interaction boundary that can record and validate the CTA before the visitor continues. Redirect routes, middleware, timing, and synchronous/asynchronous behavior are not decided.

Location CTAs follow the same conceptual pipeline when configured as billable.

## Billing modes and source precedence

One coherent Billing Mode governs eligibility and funding priority:

- `free`: track eligible activity without entitlement or Wallet consumption.
- `subscription_only`: consume applicable Subscription Entitlement/quota.
- `wallet_only`: consume Wallet / Prepaid Credit.
- `subscription_or_wallet`: consume applicable Subscription Entitlement first; only then use Wallet / Prepaid Credit.

Subscription Entitlement and Wallet / Prepaid Credit MUST remain separate authoritative concepts. The future design must not charge Wallet credit while usable applicable Subscription Entitlement remains.

Insufficient balance/quota behavior, prices, plans, quotas, currencies, pricing dimensions, refunds, reconciliation, and ledger shape remain open.

Future pricing may vary by approved policy such as CTA type, category, plan, campaign, or Store class. No pricing matrix or variable-pricing calculation is approved.

## Financial correctness invariants

Future billable click consumption is a correctness-sensitive workflow. It MUST be idempotent, concurrency-safe, transactionally consistent where required, and auditable.

The same qualified interaction MUST NOT consume entitlement or credit twice due to retry, redelivery, duplicate request, replay, concurrent processing, or repeated event handling. A visible balance or remaining quota is not sufficient historical evidence; future design must evaluate an auditable billing consumption history or ledger mechanism.

Click classification must consider duplicate activity, retries, crawlers/bots, Seller self-clicks, abuse, suspicious automation, and technical duplicate delivery. No fraud score, deduplication window, or enforcement algorithm is approved.

## Authorization and security boundaries

- Active Store Context is never proof of access.
- Sellers must not view, import, create, change, or bill Offers for Stores they are not authorized to manage.
- Seller-provided destination URLs require future validation of protocols, redirect safety, destination integrity, and appropriate Seller-domain controls.
- Store locations require authorization for editing, coordinate/address validation, and protection against exposing unrelated private location data.
- Click classification and billing decisions require abuse controls, rate limiting, replay protection, idempotency, auditability, and privacy-aware signal handling.
- Offer/Seller/Store suspension and moderation state must be evaluated before public Offer exposure and commercial CTA handling; exact propagation remains open.

## Event, queue, cache, and observability alignment

Potential business facts such as Offer submitted/approved/rejected, Store activated, CTA observed, Billable Click accepted, Subscription Entitlement consumed, and Wallet Credit consumed are conceptual examples only. Event names, contracts, delivery, and whether a particular workflow uses Event Sourcing remain open.

Click/billing workflows must align with the existing Transactional Outbox, idempotency, retry, and authorization-revalidation guidance when asynchronous work is approved. The sync/async boundary is deliberately undecided.

Master Product archives and eligible Offer lists are cache candidates subject to existing cache policy. Cache MUST NOT be authoritative for Wallet/credit, Subscription quota, click charging, Billing Decision, Offer authorization, or Store authorization.

Future observability should distinguish Raw Clicks, Billable Clicks, charged outcomes, rejected/duplicate classifications, billing failures, suspicious traffic, destination/redirect failures, Store/location CTA activity, Offer moderation, and import/sync failures.

## Explicitly deferred

No database types, tables, indexes, keys, foreign-key rules, final status values, API contracts, routes, jobs, events, listeners, queue topology, fraud algorithm, pricing model, ledger design, map provider, redirect behavior, WooCommerce integration contract, search technology, or dashboard design is approved by this document.
