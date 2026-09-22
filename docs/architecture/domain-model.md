# Domain Model

## Status

The Marketplace business model is partially defined. Approved conceptual vocabulary includes Users, Stores, Master Products, Offers, commercial CTAs, and billing concepts; detailed entities, fields, roles, statuses, and transitions must not be inferred beyond the approved documentation. “Domain” in this document means business vocabulary and rules; it does not imply Domain-Driven Design.

The future business model SHOULD remain independent from HTTP and external-provider details wherever practical, while allowing pragmatic Laravel/Eloquent usage when a separate abstraction would add no value.

## Domain vocabulary

| Term | Definition | Status | Related requirements |
| --- | --- | --- | --- |
| Marketplace | Project label only; the actual business meaning is pending. | Open | — |
| TBD | To be added from the approved product brief. | Open | — |

## Approved conceptual concepts

| Concept | Current meaning | Status |
| --- | --- | --- |
| User identity | One unified identity shared by Admin, Seller, and Customer classifications. | Approved conceptually |
| Account type | Primary classification with conceptual values `admin`, `seller`, and `customer`; not authorization. | Approved conceptually |
| Seller verification status | Seller lifecycle separate from account type, including pending/review and restricted states conceptually. | Approved conceptually; exact states open |
| Store | A Seller-managed Marketplace Store; one User may manage multiple Stores. | Approved conceptually; product details open |
| Store membership | Multi-user participation in a Store with potentially different responsibilities. | Approved conceptually; role catalog open |
| Master Product | Marketplace-controlled canonical/root product representation. | Approved conceptually |
| Offer | Store/Seller-specific commercial product entry. Always Store-scoped; Master Product association is required before public approval but may be absent during review. | Approved conceptually |
| Store Location | Commercial physical location/branch information distinct from Store. | Approved conceptually; details open |
| CTA | Explicit commercial action related to an Offer or Store. | Approved conceptually |
| Billing Mode | Central charging mode determining entitlement/Wallet behavior. | Approved conceptually |
| Subscription Entitlement | Applicable subscription quota distinct from Wallet / Prepaid Credit. | Approved conceptually |
| Wallet / Prepaid Credit | Prepaid funding concept distinct from Subscription Entitlement. | Approved conceptually |

## Business modeling rules for future rounds

- Define business terms before choosing technical names.
- Identify ownership, lifecycle, invariants, and permitted transitions for each durable concept.
- Distinguish user roles from domain entities and from technical principals.
- Record money, time, location, identity, and status semantics explicitly when they become relevant.
- Keep Seller status separate from Store status and both separate from account classification.
- Preserve Customer-to-Seller transition history on the same User identity conceptually.
- Keep Master Product and Offer distinct: an Offer is not a recursive child product.
- Treat Master Product information as canonical Marketplace data and Offer information as Seller/Store-specific commercial data.
- Derive default Offer destination capabilities from Store mode; do not create a duplicate Offer/product type without an approved override requirement.
- Keep Offer, Seller, Store, and Master Product lifecycle/visibility decisions distinct.
- Keep Raw Click observation, Billable Click classification, and Billing Decision separate.
- Avoid adding an entity solely because a framework or database pattern makes it convenient.
- Link each material domain rule to one or more approved requirements.
- Place workflow orchestration in the Application / Services layer and business rules/state transitions in the Business Logic layer.
- Treat business events as business facts, not as an automatic synonym for every Laravel event.
- Aggregates, value objects, repositories, bounded contexts, and other DDD patterns are optional tools, not mandatory modeling artifacts.
- Consider Event Sourcing only for intentionally selected business areas/workflows; do not event-source every CRUD concept by default.

## Explicit architecture boundary

This file is a vocabulary and business-rule planning aid, not a DDD specification. The project architecture remains pragmatic Laravel MVC + Layered Architecture + SOLID; see [layered-architecture.md](layered-architecture.md).

## Concept template

```text
### Concept: Name

- Purpose:
- Owner / authority:
- Lifecycle states:
- Invariants:
- Relationships:
- Sensitive data:
- Events or side effects:
- Related requirements:
```
