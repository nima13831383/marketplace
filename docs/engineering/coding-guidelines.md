# Coding Guidelines

## Status

Laravel is the approved backend framework, but no Laravel version or complete repository layout has been selected. These guidelines establish maintainability expectations without authorizing implementation.

## General principles

- Keep modules focused on one responsibility and make dependencies explicit.
- Prefer clear names based on the approved domain vocabulary.
- Keep business rules separate from transport, persistence, presentation, and provider-specific concerns.
- Make invalid states difficult to create and validate inputs at boundaries.
- Avoid hidden side effects and global mutable state.
- Keep public contracts stable and document compatibility implications before changing them.
- Favor small, reviewable changes with tests proportional to risk.
- Do not copy an assumption from a framework into the product model without a documented reason.
- Keep the approved pragmatic responsibility flow: Presentation / HTTP -> Application / Services -> Business Logic -> Infrastructure / Data Access. This is not a DDD dependency rule.
- Repositories and interfaces are optional. Eloquent MAY be used directly where it is the clearest implementation; do not introduce a repository, aggregate, value object, or separate domain package solely to satisfy a pattern.
- Keep MVC primarily at the Presentation / HTTP boundary; controllers MUST be thin and MUST NOT contain business logic.
- Use Form Requests for HTTP validation where appropriate, while keeping authorization and domain rules separate.
- Map validated input intentionally; never blindly mass-assign request payloads or trust client ownership identifiers.
- Design event listeners, jobs, scheduled tasks, and integrations for retries, duplicate execution, idempotency, and observability.
- Apply OWASP-oriented secure defaults, private storage for non-public files, risk-specific rate limits, and secret redaction.
- Treat cache access and invalidation as explicit infrastructure/application concerns; cache invalidation MUST NOT be scattered through controllers.
- Use the approved hybrid cache strategy only where a domain policy justifies caching, and never treat cached values as transactional truth.
- Use [authentication.md](../architecture/authentication.md) for the Fortify/Sanctum boundary. Authentication MUST NOT be used as a substitute for authorization or ownership checks.
- Treat Filament Resources/Pages/Actions, Blade views, Livewire components, and API controllers as presentation/application adapters. Reuse shared Application / Services and Business Logic across surfaces; do not duplicate a business operation per portal.
- Treat `account_type` as classification only. Store authorization must include membership and applicable Store-level responsibility/permission checks.
- Use the canonical terms Master Product and Offer. Do not model Store Offers as arbitrary recursive child products.
- Treat Active Store Context as a presentation/request-scoping aid only; every Store-scoped operation still requires target-Store authorization.
- Keep CTA observation, Billable Click classification, and Billing Decision separate. Only configured commercial CTA types may enter the billable path.
- Treat Subscription Entitlement and Wallet / Prepaid Credit as separate concepts. In `subscription_or_wallet`, use applicable entitlement before Wallet credit.
- Keep financial/quota operations authoritative, idempotent, concurrency-safe, and auditable. Do not use cache or presentation-layer state as a charge/entitlement source of truth.
- Route manual and future integration-sourced Offers through normalization, Master Product matching, review/approval, and Store scope; do not create source-specific bypasses.

## Documentation conventions

- Use Markdown headings in a logical hierarchy.
- Link to related documents with repository-relative links.
- Mark content as `Approved`, `Proposed`, `Assumption`, `Open`, or `Superseded` when its status matters.
- Use tables for stable registers and templates for repeatable decisions.
- Keep pseudocode and examples clearly non-production until implementation is authorized.

## Pending stack-specific rules

After the Laravel version and supporting stack are selected, add rules for formatting, linting, type checking, dependency management, naming, module boundaries, configuration, and secrets handling.
