# Architecture

## Context

The platform serves internal real-estate operators and end customers across multiple tenant organizations. The main architectural constraint is that tenant data must remain isolated across relational data, object storage, cache entries, identity, and asynchronous workflows.

## Service boundaries

| Service | Responsibility |
|---|---|
| Admin | Tenant and user administration, permissions, audit-oriented workflows |
| Hub | Reservations, contacts, teams, preferences, and shared operational workflows |
| Sales | Projects, properties, exposes, documents, communications, and sales operations |
| Finance | Fee calculations, financing workflows, and finance-specific documents |
| Customer portal | Customer-facing access to approved property and reservation information |

Each service owns its schema. Cross-service data is composed at the application boundary rather than through cross-schema foreign keys.

## Data flow

1. A user authenticates with the appropriate Keycloak realm.
2. The service validates the token and resolves the tenant context.
3. Tenant-aware queries apply the tenant boundary before loading records.
4. Cross-service updates are published through RabbitMQ or delivered through a signed webhook when an external callback is required.
5. Files and cache entries use tenant-scoped namespaces.

## Why a modular monolith

The platform benefits from clear service ownership and independent API surfaces without paying the operational cost of fully independent databases and deployments for every domain. Schema ownership, explicit boundaries, and asynchronous integration preserve modularity while keeping local development and delivery manageable.

## Frontend delivery

Operator-facing modules use Next.js, React, TypeScript, TanStack Query, and Radix UI. The frontends consume the same identity system as the backend services and keep API state separate from presentation concerns.
