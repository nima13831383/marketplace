# Planning Backlog

This backlog contains planning and documentation work only. Product features must be added after the product scope is approved.

| ID | Item | Priority | Status | Output |
| --- | --- | --- | --- | --- |
| PLAN-001 | Provide the product brief and initial problem statement. | High | Open | Updated product overview |
| PLAN-002 | Define target users, roles, permissions, and ownership boundaries. | High | Open | User-role register |
| PLAN-003 | Define the initial user flows and acceptance criteria. | High | Open | User-flow register |
| PLAN-004 | Define the initial scope, non-goals, and success measures. | High | Open | Requirements and overview updates |
| PLAN-005 | Establish domain vocabulary and lifecycle rules. | High | Open | Domain model |
| PLAN-006 | Resolve remaining technology constraints, Laravel version, deployment, storage, and runtime support. | Medium | Open | Accepted ADRs |
| PLAN-007 | Define data ownership, retention, and recovery expectations. | Medium | Open | Data model |
| PLAN-008 | Define API and external integration contracts, if needed. | Medium | Open | API design |
| PLAN-009 | Complete the threat model and security requirements. | High | Open | Security design |
| PLAN-010 | Set quality, testing, observability, and release criteria. | Medium | Open | Engineering and roadmap updates |
| PLAN-011 | Explicitly authorize implementation when planning is sufficiently complete. | High | Blocked | User instruction equivalent to “Start implementation” |
| PLAN-012 | Decide whether Event Sourcing is global or selective and identify approved business areas/workflows. | High | Open | Event Sourcing decision |
| PLAN-013 | Define roles, permissions, Policies/Gates, ownership checks, Store membership, lifecycle restrictions, and protected resources without treating `account_type` as authorization. | High | Open | Authorization strategy |
| PLAN-014 | Define enabled Fortify authentication workflows, Sanctum consumers, verification/recovery/session/token policy, MFA/OTP, upload surfaces, permitted HTML surfaces, and external webhook providers. | High | Open | Authentication, security, and API updates |
| PLAN-015 | Define queue topology, Horizon operations, scheduled-task ownership, and failed-job handling. | Medium | Open | Queue and scheduling strategy |
| PLAN-016 | Define consistency, idempotency, outbox, and concurrency strategies for approved state-changing workflows. | High | Open | Concurrency and data-model updates |
| PLAN-017 | Select cache backend, Laravel store configuration, and multi-server cache topology. | Medium | Open | Caching strategy and ADR updates |
| PLAN-018 | Classify cacheable domains and define freshness, invalidation, lock, fallback, and correctness policies. | High | Open | Caching strategy |
| PLAN-019 | Define cache observability, stampede alerts, and cache failure operational behavior. | Medium | Open | Observability and caching updates |
| PLAN-020 | Resolve portal package versions, surface-specific contracts, theme/accessibility direction, and exact Admin/Seller/Customer feature boundaries. | Medium | Open | Application surfaces and product updates |
| PLAN-021 | Define the unified User persistence representation, `account_type` transition rules, and account lifecycle behavior. | High | Open | Data model and authentication updates |
| PLAN-022 | Define Seller verification document checklist, lifecycle transitions, limited pending capabilities, review permissions, and upload retention/recovery. | High | Open | Seller verification and security updates |
| PLAN-023 | Define Store lifecycle transitions, Store-level role catalog, membership attributes, multi-Store rules, and authorization matrix. | High | Open | Authorization and data-model updates |
| PLAN-024 | Define Master Product and Offer fields, including Seller title variation, price, stock/availability, SKU, image policy, status transitions, moderation SLA, ranking, and archive-card behavior. | High | Open | Marketplace core flow and data model updates |
| PLAN-025 | Define Active Store Context behavior, Store Location schema/multi-branch rollout, Store modes, Offer/location selection, map provider, online/physical/Hybrid destination rules, and location CTA behavior. | High | Open | Commerce model and application-surface updates |
| PLAN-026 | Define configured CTA catalog, click identity/deduplication, fraud/abuse policy, redirect boundary, privacy signals, and analytics behavior. | High | Open | Commerce, security, and observability updates |
| PLAN-027 | Define Billing Mode persistence, pricing, subscription plans/quotas, Wallet denomination, insufficient-funds behavior, financial ledger, reconciliation, and recovery. | High | Open | Commerce, concurrency, and data-model updates |
| PLAN-028 | Define WooCommerce authentication, import versus sync, field synchronization (including external product URL), transport, conflict resolution, manual overrides, URL validation, and Store-scoping behavior. | High | Open | Integration/API/queue updates |
| PLAN-030 | Define API destination representation, external URL validation and Store-domain verification policy, and public/authorized destination-data exposure. | High | Open | API, security, and commerce updates |
| PLAN-029 | Define Article taxonomy/editorial workflow, comment moderation/review separation, Ticket statuses/categories/SLA, and support authorization. | Medium | Open | Product and content/support updates |
