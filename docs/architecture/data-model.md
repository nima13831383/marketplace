# Data Model

## Status

No production database schema or migration has been approved. This document records approved conceptual relationships and planning constraints, not executable persistence design.

No authentication schema, Fortify/Sanctum configuration schema, or migration is approved by this documentation round. Any future authentication persistence must be derived from the selected Laravel/package versions and approved account lifecycle rather than invented now.

## Approved conceptual identity and Store model

The conceptual model must support:

```text
users
  -> one unified identity for Admin, Seller, and Customer
  -> account_type: admin | seller | customer (conceptual classification)

stores
  -> Seller-managed Marketplace Stores

store membership
  -> multiple Users participating in a Store
  -> varying Store-level responsibilities per User
```

A conceptual membership relationship such as `store_user` is approved as a capability, not as a final table or schema requirement. One User MUST be able to own/manage multiple Stores, and a Store MUST be able to have multiple participating Users.

Seller verification status and Store status MUST be modeled as separate concepts from `account_type` and from each other. The model should support Seller lifecycle concepts such as pending onboarding, pending verification, approved, rejected, and suspended, plus Store lifecycle concepts such as draft, pending review, active, rejected, and suspended. These labels are conceptual; exact names and transitions remain open.

## Approved conceptual commerce model

The conceptual data model must support the following relationships without finalizing storage design:

```text
Master Product -> many Offers
Offer -> one Store
Offer -> zero or one Master Product before approval/review
Publicly approved Offer -> one valid Master Product
Store -> zero or more Store Locations
Store -> multiple Offers
CTA interaction -> Raw Click -> possible Billable Click -> possible Billing Decision
```

Master Product, Offer, Store, Store Location, Raw Click, Billable Click, Billing Decision, Subscription Entitlement, Wallet / Prepaid Credit, and Billing Mode are distinct concepts. Future persistence must not merge Subscription Entitlement and Wallet / Prepaid Credit into one ambiguous balance or use cached values as authoritative financial/quota state.

An Offer must always be Store-scoped. It MAY remain unmatched to a Master Product during draft/review; public approval/publication requires a valid Master Product association. This is a business/data invariant only: database nullability, foreign-key constraints, and migration syntax remain undecided.

Master Product information is canonical Marketplace information; Offer information is Seller/Store-specific commercial information. Future Offer data may include an optional `external_product_url` concept for online-capable Stores, while physical destination/location information derives from Store Location. Future persistence MUST NOT duplicate Store mode as a separate Offer/product type or duplicate Store address/coordinates on each physical Offer without a newly approved reason. Do not introduce a separate online-Offer details representation solely to avoid the conditional URL value.

Offer moderation, Offer availability, Seller/Store lifecycle, source/import identity, destination URL, and Store scope require explicit ownership, authorization, retention, and audit design. A future billing history or ledger mechanism must be evaluated; a visible balance/remaining quota alone is not sufficient historical evidence.

## Schema decisions intentionally deferred

Do not finalize the `account_type` storage representation, column types, indexes, foreign keys, membership attributes, role assignment representation, status enum values, or document persistence from this documentation round. No migrations or database schemas may be created in documentation-only mode.

## Current constraints

- Do not create database migrations during documentation-only mode.
- Do not commit to a storage engine or ORM without an accepted architecture decision.
- Do not store sensitive data merely because a future feature may need it.
- Every persistent field should have an owner, purpose, lifecycle, and retention expectation before implementation.
- Sensitive ownership, role, permission, payment, moderation, and financial fields MUST NOT become client-controlled merely because they exist in a future model.
- Database interactions MUST use Laravel ORM/query bindings or other parameterized mechanisms; raw SQL requires careful review against injection.
- If Event Sourcing is selected for a domain, the event store and rebuildable projections/read models MUST be designed as distinct concepts.
- Audit logs MUST be designed separately from ordinary debug logs and MUST exclude secrets and unnecessary sensitive payloads.
- Cache data MUST remain an optimization and MUST NOT become authoritative transactional or security-sensitive state.
- Cache invalidation or refresh tied to authoritative state changes SHOULD occur only after the transaction commits.
- If a cache is used for projections or read models, the event store or authoritative source remains the source of truth.

## Data design checklist

Future data design must address:

- Identity, ownership, and authorization boundaries.
- Required versus optional data and validation rules.
- State transitions and concurrency expectations.
- Uniqueness, referential integrity, and deletion behavior.
- Auditability and history requirements.
- Sensitive data classification, encryption, retention, and redaction.
- Indexing, query patterns, growth expectations, and backup/recovery needs.
- Migration and rollback strategy once implementation is authorized.
- Transactional consistency between writes, events, queues, and external side effects.
- Idempotency, uniqueness, locking, and concurrency constraints for state-changing operations.
- Cache consistency classification, invalidation ownership, fallback behavior, and cache-key scope where caching is approved.

## Pending schema register

| Concern | Decision | Status |
| --- | --- | --- |
| Storage technology | TBD | Open |
| Unified User identity and `account_type` representation | Concept approved; persistence form TBD | Open |
| Store and membership representation | Multi-Store and multi-user membership capability approved; schema TBD | Open |
| Master Product and Offer representation | One-level canonical catalog/Store Offer capability approved; schema TBD | Open |
| Offer-to-Master association | May be absent before review/approval; required for public approval; nullability/constraint design TBD | Open |
| Offer destination capability | Derived from Store mode; optional `external_product_url` concept and Store Location derivation approved; schema/API shape TBD | Open |
| Store Location representation | Separate Store Location and future multiple branches approved; schema TBD | Open |
| Click, billing, entitlement, and Wallet history | Conceptual distinction approved; authoritative records/ledger design TBD | Open |
| Persistent domain concepts | TBD | Open |
| Data retention | TBD | Open |
| Backup and recovery targets | TBD | Open |
| Migration policy | TBD | Open |
