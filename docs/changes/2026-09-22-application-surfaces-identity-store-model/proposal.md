---
change: 2026-09-22-application-surfaces-identity-store-model
status: completed
---

# Proposal

## Problem and intent

The Marketplace documentation previously described authentication capabilities but did not define the approved portal surfaces, unified account classification, seller verification boundary, or multi-Store participation model. Future agents need these boundaries without inferring unapproved product features or implementing them prematurely.

## Scope

### In scope

- Record one Laravel application with Admin Panel, Seller Panel, Customer Panel, and API surfaces.
- Record Filament, custom Seller Filament, and Blade + Livewire + Tailwind directions.
- Record approved conceptual authentication URLs and the prohibition on public Admin registration.
- Record a unified User identity and `account_type` values `admin`, `seller`, and `customer`.
- Keep account type separate from roles, permissions, ownership, membership, Policies, and Gates.
- Record direct Seller registration, verification documents, Admin review, limited pending access, and separate Seller/Store lifecycles.
- Record one User to multiple Stores and multi-user Store membership.
- Define shared application/service and business-logic boundaries for web surfaces and API.

### Out of scope

- Installing or configuring Filament, Livewire, Tailwind, Fortify, or Sanctum.
- Creating panels, routes, controllers, middleware, guards, components, themes, migrations, tables, schemas, or tests.
- Defining customer feature sections, final roles/permissions, exact verification documents, exact status names, or detailed authentication policies.
- Selecting persistence types, indexes, foreign-key behavior, package versions, or API consumers.

## Actors and behavior

- Admin, Seller, and Customer are approved primary account classifications, not complete authorization models.
- A Customer may later transition conceptually to Seller while retaining the same User identity and history.
- A Seller may register directly and enter onboarding, then submit required verification documents for Admin review.
- A Seller pending review has limited onboarding/verification access; exact capabilities remain open.
- A Seller may own/manage multiple Stores and multiple Users may participate in a Store with varying responsibilities.

## Acceptance criteria

- [x] AC-1: Four application surfaces and their approved technology directions are documented.
- [x] AC-2: Approved authentication URL surfaces and no-public-Admin-registration rule are documented.
- [x] AC-3: Single User identity and `account_type` classification are documented without treating account type as authorization.
- [x] AC-4: Account Type, Role, Permission, Ownership, Membership, Policy, and Gate are explicitly distinct.
- [x] AC-5: Seller registration, verification document upload, Admin review, limited pending access, and separate lifecycle concepts are documented.
- [x] AC-6: Multi-Store ownership/management and multi-user Store membership are documented without final schema decisions.
- [x] AC-7: Shared Application / Services and Business Logic reuse across web surfaces and API is documented.
- [x] AC-8: Customer/Seller/Admin authentication details and unapproved feature catalogs remain open.
- [x] AC-9: Documentation validation finds no executable application artifact.

## Invariants

- Documentation-only mode remains active.
- Admin, Seller, and Customer classifications do not bypass backend authorization.
- Seller account approval is separate from Store status.
- Seller membership in one Store does not authorize access to another Store.
- Existing Fortify/Sanctum decisions and the no-DDD architecture decision remain unchanged.

## Edge cases and negative paths

- No public `/admin/register` is allowed by the approved surface model.
- A Seller account type alone cannot authorize Store access.
- A pending, rejected, or suspended Seller cannot be assumed to have full seller capabilities.
- Admin access cannot be implemented as scattered unconditional account-type checks.
- Store membership must support users with different responsibilities and must resist IDOR.

## Permissions and data handling

Seller document uploads are sensitive by default and require future MIME, size, private-storage, access-control, retention, and audit decisions. The exact document types are intentionally unspecified. No user, Store, membership, or document data is created by this change.

## Open decisions

Customer, Seller, and Admin login identifiers and verification; MFA/OTP; session behavior; seller document checklist; status names/transitions; Store roles and permissions; admin hierarchy; and exact persistence representation remain open.
