# Development Workflow

## Current workflow: documentation rounds

1. Read all existing project documentation.
2. Read the newest user requirements as additions or explicit amendments.
3. Update the smallest complete set of Markdown documents.
4. Mark decisions, assumptions, risks, and open questions clearly.
5. Check internal consistency and links.
6. Confirm no implementation artifacts were created.
7. Return the required Documentation Report.

## Implementation gate

Until explicit authorization equivalent to **“Start implementation”** is given:

- Only documentation files may be created or changed for this project task.
- Do not install packages or introduce runtime dependencies.
- Do not scaffold frontend or backend applications.
- Do not create migrations, executable logic, or production configuration.
- Do not install Laravel packages, including `mews/purifier` or Horizon, during documentation-only mode.
- Do not implement cache stores, cache services, locks, invalidation listeners, refresh jobs, scheduled commands, or cache tests during documentation-only mode.
- Do not install or configure Fortify or Sanctum, publish authentication routes, create authentication migrations, or implement authentication workflows during documentation-only mode.
- Do not install or configure Filament, Livewire, or Tailwind; create panels, Resources, Pages, Actions, Blade templates, Livewire components, API controllers, themes, Store membership, Seller onboarding, document uploads, roles, permissions, or OTP logic during documentation-only mode.
- Do not create Master Product, Offer, Store Location, click-tracking, Billing Mode, Subscription Entitlement, Wallet, ledger, redirect, WooCommerce integration, Article, Comment, Ticket, job, event, listener, queue, or test implementation during documentation-only mode.

## Approved implementation constraints

When the gate is opened, implementation MUST follow the approved [layered architecture](../architecture/layered-architecture.md), [authentication boundary](../architecture/authentication.md), [event strategy](../architecture/event-driven-and-event-sourcing.md), [authorization rules](../architecture/authorization.md), [security baseline](../architecture/security.md), and [queue/scheduling strategy](queue-and-scheduling.md). Laravel remains the backend framework, but its version and unresolved product decisions must be confirmed first.

## Future implementation workflow

Once authorized, the coding workflow must still begin by reviewing the approved requirements, accepted decisions, security expectations, testing strategy, and relevant roadmap phase. Each implementation change should identify the requirements it satisfies, update documentation when behavior changes, and include proportional verification.

## Review checklist

- Scope matches the approved request.
- No undocumented product behavior was introduced.
- Security and authorization implications were considered.
- Tests or validation are appropriate to the change.
- Documentation and cross-references remain accurate.
- Unresolved decisions were recorded rather than hidden in code.
- Controllers remain thin and domain rules remain outside the Presentation / HTTP layer.
- Authorization, ownership checks, idempotency, transaction boundaries, and concurrency risks were reviewed for state-changing behavior.
- For portal changes, shared Application / Services and Business Logic reuse, surface isolation, account-type boundaries, Store membership, Seller lifecycle, and sensitive-upload handling were reviewed.
- For commerce changes, Master Product/Offer terminology, target-Store authorization, CTA/billing separation, financial idempotency/concurrency, cache non-authority, source normalization, and external URL/location security were reviewed.
