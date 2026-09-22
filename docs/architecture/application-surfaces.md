# Application Surfaces and Portals

## Status

The Marketplace will be one Laravel application with four approved application surfaces:

1. Admin Panel.
2. Seller Panel.
3. Customer Panel.
4. API.

The web portals are server-rendered or server-driven surfaces within the same Laravel application. A separate frontend application is not a current requirement. No panel, route, component, package, theme, or runtime configuration has been implemented.

## Surface register

| Surface | Approved technology direction | Approved authentication surface | Current scope |
| --- | --- | --- | --- |
| Admin Panel | Filament | `/admin/login`; no public admin registration | Administrative surface; final admin role hierarchy and feature catalog are open. |
| Seller Panel | Filament with custom theme and custom pages | `/seller/login`, `/seller/register` | Seller onboarding, verification, and future seller operations; exact capabilities are open. |
| Customer Panel | Laravel Blade + Livewire + Tailwind | `/login`, `/register` | Customer-facing purpose-built UX; internal feature list is open. |
| API | Laravel API boundary in the same application | Sanctum where an API consumer is approved | Shared application/service and business logic; exact consumers and contracts are open. |

## Admin Panel

- Filament is the approved administrative panel technology.
- The default theme is Light / White.
- Dark Mode is available and user-switchable.
- There is no public `/admin/register` surface.
- Admin accounts may be created manually for initial bootstrap or by an authorized Admin from inside the Admin Panel.
- The exact bootstrap procedure, admin role hierarchy, permission catalog, branding, typography, colors, and component styling remain open.

Admin authorization MUST remain centralized through the approved authorization model. Future implementation MUST NOT scatter unconditional `admin` checks throughout the application.

Future Admin responsibilities include management/review of Sellers, Stores, Seller verification, Master Products, submitted Offers, Offer-to-Master matching, billing configuration, Articles, Comments, and Tickets. Exact screens and permissions remain open.

## Seller Panel

- Filament is approved with a custom theme and custom pages.
- The Seller Panel MUST be treated as a distinct seller-facing surface, not as a copy of the Admin Panel.
- Custom pages, widgets, layouts, navigation, UX, and standard Filament Resources MAY be used pragmatically.
- Seller registration is direct at `/seller/register`; it does not require first creating a Customer account.
- Seller verification requires document upload and administrative review before full seller capabilities become active.
- A pending Seller may access onboarding and verification-related functionality, but exact allowed and blocked capabilities remain open.
- Seller operations that are Store-scoped, including future Offers, analytics, Store settings, integrations, and billing/credit views, require an explicit Active Store Context and target-Store authorization.
- Seller product creation/submission creates a Store-scoped Offer, never a canonical Master Product. Sellers may manage Seller/Store-specific Offer information but cannot freely mutate canonical Master Product data; Marketplace/Admin matching and moderation remain required.
- Seller-facing destination capability derives from the active Offer's parent Store mode. Online-capable Offers may conceptually use `external_product_url`; physical destinations resolve through Store Location; exact UX remains open.
- The Seller-facing UI should make the currently active Store clear enough to reduce accidental cross-Store operations. Exact switching UX remains open.
- Exact Seller Panel visual design and the final seller feature catalog remain open.

## Customer Panel

- The Customer Panel is not based on a full Filament panel.
- The approved direction is Laravel Blade + Livewire + Tailwind.
- The Customer Panel should provide a purpose-built customer-facing experience.
- Filament components MAY be reused selectively later, but Filament is not the primary Customer Panel framework.
- No feature list is implied. Wallets, favorites, orders, reviews, addresses, notifications, and similar sections remain unapproved unless separately specified.

## API

The API belongs to the same Laravel application and MUST reuse shared Application / Services and Business Logic layers with the web portals. Presentation adapters may differ, but business operations MUST NOT be independently reimplemented for Admin, Seller, Customer, and API surfaces.

The API path structure, consumers, versioning, contracts, error envelope, and token use remain open. Sanctum use is governed by [authentication.md](authentication.md), and authorization remains governed by [authorization.md](authorization.md).

## Shared presentation boundary

Filament Resources, Filament Pages, Filament Actions, Blade views, Livewire components, and API controllers are presentation or application adapters. They MUST remain thin and MUST NOT become the location for complex business rules.

The shared conceptual flow is:

```text
Admin / Seller / Customer UI / API
    -> Presentation adapter
    -> Application / Services
    -> Business Logic
    -> Infrastructure / Data Access
```

The same operation exposed through multiple surfaces should coordinate with shared application/service logic. Authentication identifies the principal; Policies, Gates, roles, permissions, membership, ownership, and business authorization decide access.

## Route and configuration boundary

The following URL surfaces are approved conceptually:

- `/admin/login`
- `/seller/login`
- `/seller/register`
- `/login`
- `/register`

Exact controllers, middleware, route groups, guards, session configuration, validation rules, login identifiers, verification requirements, and rate limits remain open. These documented paths do not authorize route creation during documentation-only mode.

## Theme decisions

- Admin: Light / White by default, Dark Mode available and switchable.
- Seller: custom Filament theme expected; exact design open.
- Customer: custom customer-facing design using the approved Blade/Livewire/Tailwind direction; exact design open.

Branding, color tokens, typography, responsive behavior, accessibility targets, and component styling remain open.

## Implementation gate

This document records approved surfaces and technology direction only. It does not authorize installing Filament, Livewire, or Tailwind; creating panels, routes, Resources, Pages, Blade templates, Livewire components, API controllers, themes, or runtime configuration.
