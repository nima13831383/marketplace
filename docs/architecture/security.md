# Security Expectations

## Status

The detailed threat model and compliance scope are open. The following OWASP-aligned baseline is **Approved and mandatory** for future implementation, while concrete mechanisms must still be refined against the product and operating context. This baseline does not claim formal certification.

## Baseline expectations

- Enforce authentication and authorization at every protected boundary.
- Apply least privilege to users, services, operators, and data access.
- Validate and normalize untrusted input at system boundaries.
- Prevent injection, cross-site scripting, cross-site request forgery, insecure direct object access, and unsafe deserialization as applicable to the chosen stack.
- Store secrets outside source control and avoid exposing them in logs, errors, or documentation.
- Protect sensitive data in transit and at rest where required by its classification.
- Use safe defaults for sessions, cookies, tokens, uploads, and administrative actions.
- Record security-relevant events without logging unnecessary personal or secret data.
- Define rate limiting, abuse prevention, and account recovery behavior before exposing public operations. The approved future authentication capability split is documented in [authentication.md](authentication.md).
- Plan dependency, patching, backup, incident response, and vulnerability-reporting processes.

## Application-surface security boundaries

- Admin, Seller, Customer, and API surfaces share one Laravel application but MUST retain explicit presentation and authorization boundaries.
- Public Admin registration MUST NOT exist. Admin creation is limited to documented bootstrap or authorized Admin workflows once implemented.
- `account_type` MUST NOT be treated as complete authorization. Store access requires authentication, Seller capability/lifecycle checks, Store membership, required responsibilities/permissions, and Policy/ownership authorization.
- Pending, rejected, and suspended Seller states MUST be considered when deciding access to Seller operations.
- Seller verification documents are sensitive uploads by default. Future implementation MUST use private storage and authorized access, with explicit MIME/type, size, malware/security, retention, audit, and deletion rules.

## Laravel boundary expectations

### Validation and authorization

- Laravel Form Requests SHOULD be the normal mechanism for HTTP input validation where appropriate.
- Validation MUST cover format, type, bounds, allowed values, file constraints, and business preconditions where appropriate.
- Validation MUST NOT replace authorization, and authorization MUST NOT replace validation.
- Policies SHOULD be preferred for resource-level authorization where appropriate; Gates, roles, permissions, ownership, and domain checks remain distinct. See [authorization.md](authorization.md).

### XSS and HTML sanitization

- `mews/purifier` MAY be used specifically for permitted user-generated HTML or rich text.
- It MUST NOT be treated as a general replacement for validation, authorization, output escaping, Content Security Policy, secure rendering, or other XSS protections.
- Plain-text inputs SHOULD remain plain text and MUST NOT be converted into arbitrary HTML unnecessarily.

### SQL injection and mass assignment

- Database interactions SHOULD use Laravel ORM/query bindings or other parameterized mechanisms.
- Raw SQL MUST be reviewed carefully and MUST NOT concatenate untrusted input.
- Request payloads MUST NOT be blindly mass-assigned.
- Assignable attributes MUST be intentional, and validated input SHOULD be mapped to application/domain commands where appropriate.
- Ownership identifiers, roles, permissions, payment state, moderation state, financial values, and privileged flags MUST NOT be client-controlled merely because a model contains them.

### File uploads and secure storage

Every upload flow MUST define and enforce allowed MIME types, extension rules where appropriate, size limits, safe filename handling, storage location, access strategy, authorization, and malware/security considerations where necessary. Original filenames and client-provided MIME types MUST NOT be trusted blindly. Non-public files MUST use private storage and authorized download, signed URL, or temporary URL mechanisms where appropriate.

Seller verification documents MUST remain restricted to the Seller's authorized onboarding context and authorized Admin review context. Document types and retention periods are intentionally open; do not infer identity, tax, banking, or address documents.

### Commercial URLs, locations, clicks, and billing

Seller-provided Offer destination URLs apply only to online-capable Store/Offer contexts and require future validation of allowed protocols, malformed input, redirect safety, destination integrity, and Seller-controlled domain checks where appropriate. Future implementation MUST avoid malicious redirect and open-redirect behavior. Requiring an Offer URL to belong to a verified Store domain is a possible future policy, not an approved rule.

Store commercial locations require coordinate/address validation, Store-scoped edit authorization, and separation from private User location data. Physical Offer destinations must derive from authorized Store Location data rather than copied per-Offer address/coordinate values without an approved reason. Map provider and navigation behavior remain open.

Click classification and Billing Decisions are security- and financial-sensitive. Future design MUST consider rate limits, duplicate/replay detection, bot/crawler filtering, Seller self-clicks, suspicious automation, abuse signals, idempotency, auditability, and privacy-aware handling of session/user/IP/device-like signals where lawful and appropriate. A click MUST NOT blindly charge on every HTTP request.

Subscription Entitlement, Wallet / Prepaid Credit, and billing history require authoritative transaction safety, concurrency controls, secure logging, and reconciliation design. Cached values MUST NOT determine financial correctness.

### Rate limiting and authentication hardening

Rate limits MUST be risk-specific rather than one global limit. Relevant areas include login, password reset, registration, verification, APIs, search abuse, sensitive actions, high-cost endpoints, messaging, uploads, and repeated transactional operations.

Authentication design SHOULD address throttling, secure sessions, password security, verification and reset flows, session invalidation, reauthentication for highly sensitive operations where appropriate, account abuse/lockout considerations, and MFA policy. Fortify and Sanctum are approved capabilities; final authentication product policies remain open. See [authentication.md](authentication.md).

### Audit and secure logging

Audit logging is mandatory for important administrative, security-sensitive, and transactional actions. Audit records SHOULD support actor, action, resource, timestamp, result, and relevant context without storing secrets or unnecessary sensitive payloads. Audit logs MUST be treated differently from ordinary debug logs.

Logs MUST NOT expose passwords, API secrets, access or refresh tokens, private keys, card data, sensitive authentication data, unnecessary personal information, or complete confidential request payloads. Sensitive fields MUST be redacted or excluded.

### Secrets and secure headers

Secrets MUST NOT be committed to source control. Future environments SHOULD use appropriate secret stores, rotation, least privilege, and environment separation. Secure headers SHOULD be considered, including Content Security Policy, X-Content-Type-Options, frame protections, Referrer Policy, HSTS in appropriate HTTPS production contexts, and secure cookie settings. A final CSP MUST NOT be hardcoded before frontend and resource needs are known.

Queue-specific security requirements are defined in [queue-and-scheduling.md](../engineering/queue-and-scheduling.md).

Cache-specific security requirements are defined in [caching-strategy.md](caching-strategy.md). In particular, user- or tenant-specific cache entries MUST remain isolated, secrets MUST NOT be cached in ordinary application cache, authorization caches MUST be invalidated promptly after security changes, and cache MUST NOT replace backend authorization.

## Security design register

| Area | Current status | Required follow-up |
| --- | --- | --- |
| Threat model | Open | Identify assets, actors, trust boundaries, and abuse cases. |
| Data classification | Open | Classify personal, financial, operational, and public data if present. |
| Authentication | Capability approved; policy open | Define enabled Fortify workflows, Sanctum client/token use, session and recovery policy, verification, MFA, and lifecycle behavior. |
| Authorization | Core boundary approved; product matrix open | Define final roles, permissions, Store membership, ownership, lifecycle restrictions, and operator access rules. |
| Seller verification uploads | Boundary approved; details open | Define document checklist, validation, private storage, review access, retention, deletion, audit, and provider/security behavior. |
| Commercial URLs and redirects | Boundary approved; details open | Define protocol/domain validation, redirect protection, destination integrity, and failure behavior. |
| Click abuse and billing | Boundary approved; details open | Define privacy-aware fraud controls, deduplication, replay protection, auditability, financial reconciliation, and retention. |
| Compliance | Open | Determine applicable legal, contractual, and regional obligations. |
| Incident response | Open | Define detection, escalation, containment, and recovery expectations. |

## Security documentation rule

Security-sensitive behavior must be linked to an approved requirement or decision. If the correct behavior is unknown, record it as an open question instead of guessing.
