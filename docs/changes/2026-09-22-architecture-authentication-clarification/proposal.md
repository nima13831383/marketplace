---
change: 2026-09-22-architecture-authentication-clarification
status: completed
---

# Proposal

## Problem and intent

The current documents use a Domain layer label and leave authentication mechanisms open, which can cause future coding agents to infer DDD architecture or choose an inconsistent auth stack. This round must make the approved architecture and authentication boundary explicit while preserving the existing documentation-only gate.

## Scope

### In scope

- Record Laravel + MVC + Layered Architecture + SOLID as the approved architecture.
- Clarify the request/responsibility flow as Presentation / HTTP -> Application / Services -> Business Logic -> Infrastructure / Data Access.
- State that DDD, Clean Architecture, Hexagonal Architecture, and Onion Architecture are not the project architecture requirements.
- Record Laravel Fortify and Laravel Sanctum as complementary approved authentication capabilities.
- Keep authentication distinct from authorization and resource ownership.
- Record the exact open policy for mandatory MFA.
- Synchronize affected product, architecture, security, engineering, roadmap, and agent guidance.

### Out of scope

- Installing packages or selecting package versions.
- Creating Laravel configuration, routes, controllers, migrations, models, policies, or executable logic.
- Choosing product roles, account flows, client domains, CORS/cookie values, token TTLs, or mandatory MFA.
- Defining a concrete API or database schema.

## Actors and behavior

Future authenticated requests may use a Fortify workflow and a Sanctum-supported session or token mechanism where the approved client boundary requires it. The resulting principal must still pass backend authorization, ownership, and IDOR checks. No runtime actor or flow is implemented in this change.

## Acceptance criteria

- [x] AC-1: Current architecture documents use pragmatic MVC + Layered Architecture terminology and do not imply DDD as the project architecture.
- [x] AC-2: Current documents identify repositories/interfaces as optional and Eloquent as an allowed pragmatic implementation choice.
- [x] AC-3: A canonical authentication document explains the complementary Fortify/Sanctum boundary.
- [x] AC-4: Current documents preserve the authentication-versus-authorization boundary and IDOR requirements.
- [x] AC-5: The mandatory MFA status uses the approved open wording.
- [x] AC-6: Documentation validation finds no production source, package installation, migration, or executable application artifact.

## Invariants

- Documentation-only mode remains active.
- Event-Driven Architecture and selective Event Sourcing remain approved capabilities without implying DDD.
- Cache remains a non-authoritative optimization layer.
- Product-specific roles and user flows remain open.

## Edge cases and negative paths

- Future agents must not enable every Fortify capability automatically.
- Sanctum authentication must not be mistaken for business authorization.
- Historical ADR wording must be clearly marked as clarified rather than silently treated as current architecture.
- No token, cookie, CORS, MFA, or verification value may be invented from this change.

## Permissions and data handling

This change defines security boundaries only. It requires that future authentication logs exclude secrets and unnecessary personal data, and that authorization remains enforced on the backend. It does not create accounts, permissions, tokens, or persisted data.

## Open decisions

Product flows, email-verification enforcement, mandatory MFA, account suspension/revocation, token consumers and lifecycle, session/cookie/CORS policy, and exact rate limits remain open.
