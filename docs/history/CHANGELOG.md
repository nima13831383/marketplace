# Documentation Changelog

## 2026-09-22 — Architecture and authentication clarification

- Clarified the approved architecture as Laravel + MVC + Layered Architecture + SOLID.
- Explicitly documented that DDD, Clean, Hexagonal, and Onion Architecture are not project architecture requirements.
- Added the Fortify/Sanctum authentication boundary and kept authentication separate from authorization.
- Preserved Event-Driven Architecture and selective Event Sourcing without treating them as DDD adoption.
- Recorded the documentation-only change package and verification evidence in [2026-09-22-architecture-authentication-clarification](../changes/2026-09-22-architecture-authentication-clarification/README.md).

No application implementation, package installation, migration, or executable logic was added.

## 2026-09-22 — Application surfaces, identity, seller onboarding, and Stores

- Documented Admin, Seller, Customer, and API surfaces in one Laravel application.
- Approved Filament for Admin, custom Filament for Seller, and Blade + Livewire + Tailwind for Customer.
- Documented conceptual authentication URL surfaces and disabled public Admin registration.
- Added unified User identity, `account_type` classification, Seller verification, separate Seller/Store lifecycles, multi-Store support, and multi-user Store membership.
- Preserved the distinction between account type and authorization, and required shared Application / Services and Business Logic across surfaces.
- Recorded evidence in [2026-09-22-application-surfaces-identity-store-model](../changes/2026-09-22-application-surfaces-identity-store-model/README.md).

No application implementation, package installation, migration, panel, component, route, upload flow, or executable logic was added.

## 2026-09-22 — Marketplace commerce core

- Added canonical Master Product/Offer, Store scope, commercial CTA, Billing Mode, subscription-first entitlement, and future ingestion documentation.
- Documented Master Product-first discovery, Store Location separation, Offer moderation/matching, Active Store Context, and high-level Articles/Comments/Ticketing scope.
- Preserved financial correctness, authorization, cache non-authority, event, concurrency, queue, security, and observability boundaries.
- Recorded the Level 3 verification package in [2026-09-22-marketplace-commerce-core](../changes/2026-09-22-marketplace-commerce-core/README.md).

No application implementation, package installation, migration, catalog, billing, integration, queue, event, test, or executable logic was added.

## 2026-09-22 — Marketplace commerce Offer and destination clarification

- Clarified that Seller-created products are Store-scoped Offers, not Master Products, and that pending/review Offers may be unmatched while public approval requires a valid Master Product association.
- Recorded Master Product as canonical Marketplace data and Offer as Seller/Store-specific commercial data.
- Preserved Store modes `online`, `physical`, and `hybrid`; documented Store-derived destination capability without a duplicate Offer/product type.
- Added conditional `external_product_url`, Store Location-derived physical destinations, Hybrid capability, and a non-final API destination abstraction.
- Added related authorization, URL security, testing, roadmap, and open-question guidance in [2026-09-22-marketplace-commerce-core](../changes/2026-09-22-marketplace-commerce-core/README.md).

No application implementation, package installation, migration, model, API resource, controller, validation class, test, or executable logic was added.
