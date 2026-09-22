---
change: 2026-09-22-application-surfaces-identity-store-model
status: completed
---

# Design

## Chosen approach

Add [application-surfaces.md](../../architecture/application-surfaces.md) as the canonical portal document. Update product, architecture, security, authorization, data-model, engineering, roadmap, and handoff documents with links and concise implications rather than duplicating full contracts.

Use one conceptual User identity with an `account_type` classification. Treat Seller verification status, Store status, Store membership, roles, permissions, Policies, Gates, and ownership as separate concerns. Represent multi-Store participation conceptually through Users, Stores, and a membership relationship such as `store_user`; names and persistence details are not final.

## Contracts and state

- Four surfaces: Admin Panel, Seller Panel, Customer Panel, and API in one Laravel application.
- Admin: Filament, Light default, Dark Mode switchable, `/admin/login`, no public registration.
- Seller: Filament with custom theme/pages, `/seller/login`, `/seller/register`, verification before full capability.
- Customer: Blade + Livewire + Tailwind, `/login`, `/register`, feature catalog open.
- API: same application and shared Application / Services and Business Logic layers.
- Identity: one User with conceptual `account_type` values `admin`, `seller`, `customer`.
- Seller: direct registration, document submission, Admin review, and separate seller lifecycle.
- Stores: one User may manage multiple Stores; a Store may have multiple Users with different responsibilities.

## Alternatives and tradeoffs

- **Separate identity tables:** rejected as the default; the approved model uses one unified User identity.
- **Account type as the authorization mechanism:** rejected; account type is classification only.
- **One Store per Seller:** rejected; the model must support multiple Stores.
- **Store owner column only:** rejected as the complete access model; membership is required for multi-user participation.
- **Separate frontend application:** not approved at this stage; server-side/server-driven Laravel surfaces are sufficient for current planning.
- **Full Filament Customer Panel:** rejected; Customer uses Blade + Livewire + Tailwind as the primary direction.

## Migration, rollout, and recovery

Not applicable. No runtime or persisted data changed. Future implementation must separately decide persistence representation, migrations, compatibility, account transition recovery, document upload recovery, and Store membership migration behavior.

## External sources verified

No external source was required. The decisions are based on the explicit user-approved requirements in this documentation round.

## Related ADRs

- [ADR-0014](../../architecture/decisions.md#adr-0014--fortify-and-sanctum-authentication-stack) — existing authentication capabilities.
- [ADR-0015](../../architecture/decisions.md#adr-0015--four-application-surfaces-in-one-laravel-application) — portal and API surfaces.
- [ADR-0016](../../architecture/decisions.md#adr-0016--unified-user-identity-and-account-classification) — unified identity and account type.
- [ADR-0017](../../architecture/decisions.md#adr-0017--seller-verification-and-multi-store-membership-boundary) — seller and Store boundaries.

## Risks and mitigations

- **Risk:** Account type is used as broad authorization. **Mitigation:** current guidance repeats the distinct authorization dimensions and requires Policies/Gates/ownership/membership checks.
- **Risk:** Seller document uploads expose sensitive data. **Mitigation:** classify them as sensitive and defer secure upload/storage details to an explicit design.
- **Risk:** Panel-specific logic diverges. **Mitigation:** require shared Application / Services and Business Logic reuse.
- **Risk:** Pending or suspended Sellers receive full access accidentally. **Mitigation:** keep Seller lifecycle separate and require capability checks for future implementation.
