---
change: 2026-09-22-marketplace-commerce-core
status: verifying
---

# Design

## Chosen approach

Use [marketplace-core-flow.md](../../product/marketplace-core-flow.md) as the product authority for terminology and user-facing conceptual flow. Use [marketplace-commerce-model.md](../../architecture/marketplace-commerce-model.md) as the architecture authority for relationships, scope, click/billing, financial correctness, integration, and cross-cutting constraints. Other documents link to these authorities and describe only their local implications.

## Contracts and state

- Catalog: Master Product → many Offers. An Offer always maps to one Store; it may be unmatched during draft/review and must map to one valid Master Product before public approval.
- Discovery: search/archive → Master Product → eligible Offer → explicit CTA.
- Store: Active Store Context scopes Seller-facing data but authorization remains mandatory.
- Destination: Store mode controls default Offer capability; online-capable Offers may conceptually use `external_product_url`, physical destination derives through Store Location, and Hybrid may expose both. API consumers receive conceptual destination capabilities, not a final JSON schema.
- Click: CTA observation → classification → Billing Decision.
- Funding: central Billing Mode selects no charge, Subscription Entitlement, Wallet / Prepaid Credit, or subscription-first fallback.
- Source: manual or future WooCommerce → normalize → Master Product match → review → publish.
- Content/support: Articles, comments on Articles/Master Products, and Ticketing are high-level approved capabilities.

## Alternatives and tradeoffs

- **Recursive product hierarchy:** rejected; Offer describes the Store commercial relationship more accurately.
- **Offer-first search/archive:** rejected as the default because it duplicates canonical Marketplace products.
- **Direct charge in CTA handler:** rejected; tracking, validation, and billing are separate stages.
- **Merged Wallet and Subscription balance:** rejected; entitlement and prepaid credit have different semantics.
- **One Store per Seller or context-only security:** rejected; multi-Store membership and backend authorization remain mandatory.
- **WooCommerce bypass path:** rejected; all sources require normalization, matching, moderation, and Store scope.
- **Duplicate Offer/product mode or per-Offer physical location:** rejected by default; Store mode and Store Location remain the source concepts unless a future requirement justifies an override.

## Migration, rollout, and recovery

Not applicable. No data or runtime behavior changed. Future implementation must separately design migrations, reconciliation, financial correction/recovery, redirect fallback, integration recovery, Store scope migration, and cache invalidation.

## External sources verified

No external source was required. All decisions were explicitly supplied by the user for this documentation round.

## Related ADRs

- [ADR-0018](../../architecture/decisions.md#adr-0018--master-product-and-store-offer-model)
- [ADR-0019](../../architecture/decisions.md#adr-0019--active-store-context-and-source-normalization-boundary)
- [ADR-0020](../../architecture/decisions.md#adr-0020--separate-cta-observation-classification-and-billing-decision)
- [ADR-0021](../../architecture/decisions.md#adr-0021--central-billing-mode-and-subscription-first-funding-priority)
- [ADR-0022](../../architecture/decisions.md#adr-0022--offer-matching-state-and-store-derived-destination-capability)

## Risks and mitigations

- **Financial duplication:** require future idempotency, concurrency control, authoritative state, auditability, and reconciliation design.
- **Store cross-access:** preserve target-Store authorization independently of active context.
- **Fraud/abuse:** defer algorithm but require validation, rate limiting, duplicate detection, replay resistance, and auditability.
- **External URL abuse:** require future protocol, redirect, integrity, and Seller-domain controls.
- **Premature schema/UI commitment:** keep all fields, types, routes, providers, panels, and presentation details open.
- **Destination inconsistency or unsafe URL:** derive capability from Store mode, retain Store Location separation, and defer final URL/domain-validation policy to an approved security design.
