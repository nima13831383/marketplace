# Marketplace Core Flow

## Status

This document is the canonical product record for the approved Marketplace catalog, Store Offer, CTA, and high-level commercial flow. It records conceptual behavior only; no catalog, billing, moderation, CMS, ticketing, or integration implementation exists.

## Canonical terminology

| Term | Meaning | Status |
| --- | --- | --- |
| Master Product | Marketplace-controlled canonical/root representation of a product. | Approved conceptually |
| Offer | A Store/Seller-specific commercial product entry. It requires a Master Product association before public approval, but may be unmatched during submission/review. Also called a Listing only where needed for reader context. | Approved conceptually |
| Store | Seller-managed commercial presence; a User may participate in multiple Stores. | Approved conceptually |
| Store Location | Commercial physical location or branch information associated with a Store. | Approved conceptually; multi-location behavior open |
| CTA | Explicit commercial action presented for an Offer or Store. | Approved conceptually |
| Raw Click | Observed CTA interaction before billing classification. | Approved conceptually |
| Billable Click | Validated CTA interaction eligible for billing under the active Billing Mode. | Approved conceptually |
| Billing Decision | Decision whether and how a Billable Click consumes entitlement, prepaid credit, or no charge. | Approved conceptually |
| Subscription Entitlement | Subscription quota/capability for eligible billable interactions; distinct from Wallet balance. | Approved conceptually |
| Wallet / Prepaid Credit | Monetary or credit balance usable when the active Billing Mode permits it; distinct from Subscription Entitlement. | Approved conceptually |
| Billing Mode | One central conceptual commercial mode: `free`, `subscription_only`, `wallet_only`, or `subscription_or_wallet`. | Approved conceptually |

Do not use recursive “parent product / child product” terminology for this model. An Offer is not a child product and a Master Product cannot become an Offer under another Master Product.

## Core catalog model

```text
Master Product
  -> many Store Offers

Offer
  -> exactly one Store
  -> zero or one Master Product before approval/review completes
  -> exactly one valid Master Product when publicly approved
```

The Marketplace maintains the canonical Master Product catalog under Marketplace/Admin control. A Master Product is the normalized common representation of a product, such as `iPhone 16 128GB`. Stores commercialize that product through their own Offers.

The Master Product → Offer relationship is one level only. Recursive product trees are not part of this approved relationship.

## Product-information ownership

The Master Product is the Marketplace-controlled canonical representation. It may conceptually carry canonical name, slug, brand, model, category, normalized attributes/specifications, Marketplace-controlled content/images, and canonical SEO/content information. Exact fields remain open.

The Offer is the Seller/Store-specific commercial entry. It may conceptually carry Store association, Master Product association, Seller-facing/display title where needed, price, availability/stock state, Seller SKU where needed, Seller-provided images where allowed, an external product destination, moderation/publication state, and submission/review metadata. These examples are a boundary, not an approved final schema or mandatory-field list.

Canonical product identity MUST NOT be independently duplicated by every Store. Seller-specific price, availability, Store association, and external destination belong to the Offer side.

## Discovery and visitor flow

Marketplace archive, search, and category-style results should primarily display Master Products rather than every Offer as a duplicate top-level result.

```text
Search / Archive
  -> Master Product cards
  -> Master Product detail
  -> eligible Store Offers
  -> relevant CTA
```

The Master Product detail page displays Master Product information and relevant eligible Offers. A visitor selects an Offer and may then use one or more relevant CTAs. Exact page layout, card contents, ranking, sorting, and visual design are open.

## Store modes and locations

Stores support these conceptual modes:

- `online`
- `physical`
- `hybrid`

Store mode determines the default commercial destination capabilities of its Offers. Do not introduce a separate persistent `offer_type` or `product_type` merely to duplicate Store mode; `Offer -> Store -> Store mode` is the approved conceptual source of that capability.

- An Online Store may provide a Store website and an Offer-specific external product destination URL.
- A Physical Store may provide commercial address and location information, including latitude and longitude.
- A Hybrid Store may expose both online and location-related CTAs where appropriate.

Store and Store Location are separate concepts. The architecture must remain able to support one Store with multiple Locations or branches, even if an initial product workflow uses only one location. Map and navigation providers remain open.

### Offer destinations

For an online-capable Offer, `external_product_url` is the preferred conceptual name for the Store-specific URL to the corresponding Seller website product page. It is conditional rather than universally required: it may be expected for an Online Offer when such a page exists, may be absent for a Physical-only Offer, and has final Hybrid requirements still open. One nullable/absent conceptual Offer value is approved; do not introduce a separate `online_offer_details` concept merely to avoid that condition.

Physical Offers do not duplicate commercial address, latitude, or longitude on every Offer. Their visitor-facing physical destination derives through `Offer -> Store -> Store Location`. A Hybrid Store Offer may potentially expose both external-product and Store-location destination actions; exact UI rules remain open.

## Offer submission and moderation

Sellers may manually submit Offers from the Seller Panel. A Seller-created product is an Offer, never a Master Product. Submission does not automatically make an Offer public.

The approved high-level moderation flow is:

```text
Seller
  -> Active Store
  -> create Offer
  -> pending review
  -> Master Product matching
  -> approval or rejection
```

Offer lifecycle concepts include draft, pending review, approved, rejected, and suspended. Exact status names, transitions, moderation SLA, fields, and public-visibility rules remain open.

An Offer MAY be unmatched while it is in draft or review. A publicly approved/published Offer MUST be associated with one valid Master Product. Sellers do not have unrestricted control over canonical Marketplace taxonomy or Master Product data. Marketplace/Admin control determines Master Product matching or creation.

Master Product existence is independent from Offer availability. Only eligible Offers should appear on a Master Product page; Seller or Store lifecycle may affect Offer visibility. The full propagation policy remains open.

## Commercial CTA and click flow

Only explicitly designated commercial CTA types are candidates for billing. Approved examples are:

- `Go to Seller Product Page`
- `Show Store Location`

Ordinary navigation, tabs, filters, pagination, article interactions, and similar UI activity are not automatically billable.

```text
Billable candidate CTA
  -> Raw Click tracking
  -> click validation/classification
  -> Billing Decision
  -> no charge | Subscription Entitlement | Wallet / Prepaid Credit
```

Raw Click, Billable Click, and charged outcome are distinct concepts. Click tracking continues to support analytics even when Billing Mode is `free`; tracking does not itself hardcode a charge. Detailed security, financial, concurrency, and redirect boundaries are canonical in [marketplace-commerce-model.md](../architecture/marketplace-commerce-model.md).

## Billing mode

The Marketplace uses one central conceptual Billing Mode rather than scattered independent flags:

| Mode | Approved semantics |
| --- | --- |
| `free` | Eligible CTA interactions may be tracked but consume neither Subscription Entitlement nor Wallet / Prepaid Credit. |
| `subscription_only` | Eligible charged clicks use applicable Subscription Entitlement/quota. |
| `wallet_only` | Eligible charged clicks use Wallet / Prepaid Credit. |
| `subscription_or_wallet` | Use applicable Subscription Entitlement first; use Wallet / Prepaid Credit only when entitlement is unavailable or exhausted. |

Subscription Entitlement and Wallet / Prepaid Credit must remain separate concepts. Pricing, plan catalog, quotas, renewal duration, currency, insufficient-balance behavior, and pricing variation remain open.

Future pricing must remain extensible enough to vary by approved policy such as CTA type, category, plan, campaign, or Store class. No variable-pricing model or rule is approved by this documentation round.

## Offer sources and future WooCommerce ingestion

Manual Seller submission is approved. Future WordPress/WooCommerce ingestion is also approved as a product direction, but its implementation details are deferred.

All sources must pass the same conceptual Marketplace boundary:

```text
Source
  -> import or submission
  -> normalize
  -> match to Master Product
  -> review / approval
  -> publish Offer
```

Future source types such as API or CSV are possibilities only, not approved product features. WooCommerce one-time import versus continuous synchronization, authentication, frequency, webhooks, polling, field synchronization, conflict resolution, and manual overrides remain open. Source activity must remain scoped to the intended Store.

## Analytics and impressions

The commerce model should support future Seller and Admin analytics without deciding dashboard design. Candidate Seller analytics include impressions, Raw Clicks, Billable Clicks, charged outcomes, top Master Products, top Offers, remaining Subscription Entitlement, and Wallet / Prepaid Credit. Candidate Admin analytics include aggregate traffic, billable activity, anomalies, reconciliation, and suspicious click patterns.

Impressions MAY be measured separately from clicks. An impression is not automatically billable.

## Billing configuration

Future authorized Admin system settings should be able to manage the active Billing Mode through one coherent configuration boundary. Exact settings UI, permissions, change history, rollout, and operational behavior remain open.

## Content and support features

- **Articles:** the Marketplace includes an Articles/Content section. Article taxonomy and editorial workflow are open.
- **Comments:** comments are included for Articles and Master Products. Product comments, Store/Offer reviews, and Seller ratings are separate concepts unless later explicitly unified.
- **Ticketing:** the Marketplace includes a Ticketing system capable of more than a single-message contact form. Ticket, replies, status, priority, assignment, Customer/Seller context, attachments, and audit trail are conceptual possibilities; exact workflow is open.

Future comments require moderation, reporting, abuse/spam controls, authorization, and auditability. Future ticketing requires permissions, lifecycle, and attachment decisions.

## Explicitly deferred

This document does not define Master Product/Offer fields, price representation, stock/availability model, SKU requirements, Seller title variation, Offer-image policy, archive cards, ranking, maps, redirect implementation, fraud algorithm, billing ledger schema, plan catalog, Wallet denomination, WooCommerce field mapping, exact external-URL/domain-validation policy, Hybrid destination requirements, multi-location Offer selection, API destination schema, article taxonomy, review/rating model, ticket SLA, or implementation details.
