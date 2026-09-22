# API Design

## Status

No concrete API surface or integration contract has been approved. Laravel is the approved backend framework, and Fortify/Sanctum are approved authentication capabilities, but this document must not be interpreted as an OpenAPI specification or executable interface.

The API is one of four application surfaces in the same Laravel application. It MUST share Application / Services and Business Logic with the Admin, Seller, and Customer web surfaces; this does not approve concrete endpoints or consumers.

Future APIs or controlled web boundaries may expose Store-scoped Offer submission/import, moderation, Master Product discovery, commercial CTA tracking, and analytics. Any such surface MUST preserve Master Product/Offer normalization, target-Store authorization, idempotency, billing separation, and secure external-destination handling. Concrete routes, payloads, authentication consumers, and error contracts remain open.

## Destination capability abstraction

Future API responses SHOULD represent an Offer's meaningful commercial destination capabilities rather than force clients to infer behavior from unrelated nullable values. The conceptual `destination` representation may convey:

- **Online:** `type = external_product` with the applicable Offer `external_product_url`.
- **Physical:** `type = store_location` with applicable Store Location/address information.
- **Hybrid:** multiple applicable destination actions/capabilities.

Capability derives from the Offer's parent Store mode. An Offer/product mode must not be introduced as a duplicate source of truth by default. This does not finalize response JSON, nesting, naming, cardinality, location payload, authorization semantics, or public API availability.

## API design goals

Once product flows and system boundaries are known, API contracts should make the following explicit:

- Resource or operation purpose and ownership.
- Authentication and authorization requirements.
- Authorized-resource access rather than trusting raw client ownership identifiers.
- Input validation and normalization.
- Successful response shape and pagination semantics where relevant.
- Error codes, safe messages, and retryability.
- Idempotency and concurrency behavior for state-changing operations.
- Versioning and backwards-compatibility expectations.
- Rate limits, audit events, and sensitive-data handling.
- Idempotency keys or deduplication for operations where duplicate delivery could mutate state incorrectly.

## Contract template

```text
### Operation: TBD

- Consumer:
- Purpose:
- Authentication:
- Authorization:
- Input:
- Output:
- Errors:
- Idempotency / retry behavior:
- Side effects:
- Related requirements:
```

## Open interface decisions

- API style and transport.
- Public versus internal interfaces.
- Versioning policy.
- Authentication and session strategy.
- Which Sanctum API/token consumers are justified, if any, and what scopes, revocation, and lifecycle they require.
- Webhook or event contracts, if required.
- Documentation and contract-validation tooling.
- Which external services publish or consume webhooks and how replay is prevented.
