# Authorization and IDOR Protection

## Status

Backend authorization and IDOR prevention are **Approved mandatory constraints**. Product roles, permissions, membership, and ownership rules remain partly open. Admin, Seller, and Customer are approved primary account classifications, not a complete authorization model. The authentication capabilities are approved separately as Fortify + Sanctum; their use does not decide authorization policy.

## Separate authorization concerns

```text
Account Type != Role != Permission != Ownership != Membership
```

Policies and Gates remain the backend authorization decision boundary; the dimensions above provide inputs and context but are not interchangeable.

The design MUST keep these concepts distinct:

- **Roles:** broad assigned capabilities or responsibilities.
- **Permissions:** named actions a principal may perform.
- **Account type:** primary classification such as `admin`, `seller`, or `customer`; it is not authorization.
- **Policies:** resource-level authorization rules, preferably used where Laravel resource checks apply.
- **Gates:** targeted authorization checks for abilities that may not map cleanly to a resource policy.
- **Ownership:** whether the current principal owns or is otherwise entitled to a specific resource.
- **Membership:** participation in a scoped resource such as a Store, potentially with Store-level responsibilities.
- **Domain authorization:** business-specific rules that may depend on state, relationships, or workflow.

Having a role such as `seller` MUST NOT authorize access to every resource associated with sellers. Resource ownership and domain-specific authorization MUST still be checked.

The conceptual capability hierarchy is:

```text
Customer -> base customer capabilities
Seller   -> customer capabilities + seller-specific capabilities
Admin    -> administrative capabilities across the system
```

This describes product capability expectations only. It MUST NOT become unsafe implicit authorization inheritance. Actual authorization still evaluates the applicable roles, permissions, ownership/membership, Policies, Gates, and current lifecycle state.

## Account and Seller lifecycle boundaries

- A Customer may later become a Seller through the same User identity; a second User account is not required solely for the transition.
- Seller verification status is separate from `account_type`.
- Pending, rejected, and suspended Seller states MUST be considered when authorizing seller capabilities.
- A Seller account type alone MUST NOT authorize access to a Store.

## Store authorization

Future Store authorization should evaluate a sequence equivalent to:

```text
Authenticated
 -> seller-capable account
 -> member of target Store
 -> required Store role/permission
 -> Policy / ownership authorization
 -> allow or deny
```

A Seller MUST NOT access another Seller's Store merely because the principal has `account_type = seller`. One User may manage multiple Stores, and a Store may have multiple Users with different responsibilities. The exact Store-level role catalog remains open.

## Active Store Context and Offer authorization

Active Store Context MAY help a Seller select the intended Store, but it is not proof of membership or permission. Future Offer, Store Location, source-import, click analytics, Store setting, and Store billing operations MUST authorize the actual target Store before exposing or mutating scoped data.

A Seller MUST NOT view, create, change, submit, import, moderate, or financially affect Offers for a Store the Seller is not authorized to manage. Admin moderation access remains centralized through roles/permissions, Policies/Gates, and resource-specific rules rather than scattered account-type checks.

A Seller-created product enters the Store-scoped Offer workflow. A Seller MUST NOT directly create, replace, or freely mutate canonical Master Product data without an approved, authorized Marketplace workflow. A pending Offer may be unmatched during review; that fact does not authorize the Seller to alter canonical catalog data.

## Admin access boundary

Admin is the highest approved account classification, but Admin access MUST remain centralized through the authorization model. Do not scatter unconditional `if admin then allow everything` logic. Future concepts such as Super Admin, Admin, or Moderator are not an approved final role catalog.

## Required backend behavior

- Every sensitive resource operation MUST be authorized on the backend.
- UI visibility MUST never be treated as authorization.
- Form Request authorization and policies/gates MUST complement input validation; neither replaces the other.
- Authorization MUST occur before sensitive resource access or mutation.
- Route-model binding alone is not an authorization mechanism.
- Application services SHOULD work with authorized resources or trusted domain commands rather than raw client-supplied ownership identifiers.
- Fortify authentication workflows and Sanctum session/API/token mechanisms MUST be followed by the relevant authorization, ownership, and IDOR checks; authentication alone is never sufficient.

## IDOR prevention

The system MUST NOT trust client-submitted `user_id`, `seller_id`, `order_id`, or similar identifiers as proof of ownership. Resource enumeration MUST NOT grant access to unauthorized records. Authorization checks MUST be applied to the actual resource and current security context, including delayed or retried work where the context may have changed.

## Pending decisions

- Which final roles and permissions will exist.
- Which resources require ownership or Store membership checks.
- Which authorization rules belong in Policies, Gates, or Domain services.
- Product authentication flow enablement, session/token lifecycle, verification, MFA, and operator-access policy; see [authentication.md](authentication.md).
- Exact Seller capability restrictions by pending, approved, rejected, or suspended state.
