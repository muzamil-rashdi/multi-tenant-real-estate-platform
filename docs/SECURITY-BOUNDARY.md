# Security Boundary

This document is a public-safe summary. It intentionally omits client identifiers, infrastructure addresses, secrets, implementation-specific policy names, and operational runbooks.

## Tenant isolation

- Tenant-aware tables carry an explicit tenant identifier through a shared model convention.
- Service-layer reads and writes require tenant context and apply tenant filtering.
- Each service owns its PostgreSQL schema; cross-schema foreign keys are not used.
- S3-compatible object keys are prefixed with the tenant namespace.
- Redis keys are namespaced per tenant and support version-based invalidation.

## Identity isolation

The operator application and customer portal use separate Keycloak realms. This makes the boundary between internal staff and external customers explicit at the identity layer rather than relying only on application roles.

## Asynchronous workflows

RabbitMQ topic exchanges carry cross-service updates. Dead-letter queues make failed messages observable and recoverable. HMAC-signed webhooks protect callbacks that leave the platform boundary.

## Public-release rule

The production repository remains private. A public portfolio artifact must never include:

- Environment files, tokens, passwords, private keys, or API credentials
- Internal URLs, infrastructure addresses, or deployment configuration
- Client names, customer records, real documents, or database dumps
- Proprietary business rules or internal operational documentation
- Historical commits that contain any of the above
