# User Flows

## Status

No detailed Marketplace feature flows have been approved. A small set of high-level portal, authentication-surface, Seller verification, and identity-transition flows is approved below; detailed behavior must still be derived from explicit requirements rather than generic Marketplace patterns.

Fortify and Sanctum define approved future authentication capabilities, not complete product flows. The portal URL surfaces and high-level Seller verification flow below are approved, while detailed registration, verification, recovery, MFA, and protected Marketplace journeys remain open.

## Flow register

| Flow ID | Name | Primary actor | Outcome | Status |
| --- | --- | --- | --- | --- |
| — | TBD | TBD | Awaiting product requirements. | Open |

## Approved high-level flows

| Flow ID | Name | Primary actor | Outcome | Status |
| --- | --- | --- | --- | --- |
| FLOW-ADMIN-LOGIN | Admin login | Admin | Authenticate through `/admin/login` and enter the Admin Panel if authorized. | Approved conceptually |
| FLOW-SELLER-REGISTER | Direct Seller registration | Seller applicant | Create one User with Seller classification and enter Seller onboarding through `/seller/register`. | Approved conceptually |
| FLOW-SELLER-VERIFY | Seller verification | Seller, Admin | Submit required verification documents, await Admin review, and reach approved or restricted lifecycle state. | Approved high-level flow |
| FLOW-CUSTOMER-AUTH | Customer authentication surfaces | Customer | Use `/login` and `/register`; exact identifiers and verification remain open. | Approved surfaces; details open |
| FLOW-CUSTOMER-TO-SELLER | Customer to Seller classification transition | Customer | Transition the same User identity conceptually from `customer` to `seller` without creating a second identity. | Approved conceptually; workflow open |
| FLOW-OFFER-SUBMIT | Seller Offer submission | Seller | Select an Active Store, create a Store-scoped Offer (not a Master Product), submit it for review, and await Marketplace/Admin Master Product matching and approval/rejection. An Offer may remain unmatched before approval. | Approved high-level flow |
| FLOW-MASTER-DISCOVERY | Master Product discovery | Visitor | Browse/search Master Products, open a Master Product, and select an eligible Store Offer. | Approved high-level flow |
| FLOW-COMMERCIAL-CTA | Commercial CTA interaction | Visitor | Invoke a configured Offer/Store CTA, record/classify the interaction, and apply the active Billing Mode if eligible. | Approved high-level flow; implementation open |

## Flow documentation template

Each approved flow should record:

1. Entry point and preconditions.
2. Primary actor and any participating roles or external systems.
3. Main success path.
4. Alternate paths and recoverable failures.
5. Authorization and data-handling considerations.
6. Completion state and observable outcome.
7. Acceptance criteria and related requirement IDs.

## Flow diagram policy

Mermaid diagrams may be used for approved flows. Diagrams in this file must remain explanatory documentation and must not be treated as executable workflow definitions.
