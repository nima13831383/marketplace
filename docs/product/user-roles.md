# User Roles

## Status

No final marketplace role or permission catalog has been approved. The primary account classifications `admin`, `seller`, and `customer` are approved, but they are not roles and must not be used as the complete authorization model. The authentication capabilities are approved separately; they do not define product roles or permissions.

## Role register

| Role ID | Role name | Purpose | Permissions | Status |
| --- | --- | --- | --- | --- |
| — | TBD | Awaiting the product brief. | TBD | Open |

## Approved account classifications

| Classification | Meaning | Not implied |
| --- | --- | --- |
| `customer` | Primary Customer account classification. | No automatic access to every customer resource or feature. |
| `seller` | Primary Seller account classification. | No automatic access to every Store or full seller capability before verification. |
| `admin` | Primary administrative account classification. | No scattered unconditional allow-all authorization. |

These classifications are represented conceptually through the unified User identity and `account_type`. They remain distinct from roles, permissions, Store membership, ownership, Policies, and Gates.

## Store-level responsibility concept

Store-level responsibilities may eventually include concepts such as Owner, Manager, Product Manager, Order Manager, or Support. These are examples only; the final Store role catalog and permission model remain open. A Store membership may assign different responsibilities to different Users.

## Marketplace/Admin responsibilities

Marketplace/Admin responsibilities include conceptual control over Seller/Store review, Seller verification, Master Products, submitted Offers, Offer-to-Master matching, future billing configuration, Articles, Comments, and Tickets. This is not a final Admin permission catalog; every capability remains subject to centralized authorization.

## Role definition template

For each approved role, document:

- Who the role represents and what outcome it needs.
- What resources it may view, create, change, publish, cancel, or delete.
- Which actions require authentication, verification, approval, or another role.
- What happens when the role is suspended, removed, or changes status.
- Which personal or sensitive data the role may access.

## Open authorization concerns

- Product account lifecycle and enabled authentication workflows are unknown. The approved future capabilities are documented in [authentication](../architecture/authentication.md).
- The final role/permission catalog, Seller verification policy, Store membership permissions, and ownership rules are unknown. The approved separation is documented in [authorization strategy](../architecture/authorization.md).
- Administrative, operational, support, and moderation access are unknown.
- Tenant, organization, or regional boundaries are unknown.

Regardless of the eventual role list, backend authorization MUST distinguish roles and permissions from resource ownership. A role such as `seller` MUST NOT authorize access to every seller resource. Policies, Gates, ownership checks, and domain-specific authorization remain separate concerns; see [authorization strategy](../architecture/authorization.md).
