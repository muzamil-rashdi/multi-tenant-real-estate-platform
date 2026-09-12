# Multi-Tenant Real Estate Platform

## Public architecture case study

This repository documents the architecture and engineering decisions behind a multi-tenant real estate operations platform. The production implementation remains private because it contains client source and operational details. This public version contains no client name, business data, credentials, internal URLs, or proprietary source code.

## What the platform supports

- Property and project operations
- Reservations and customer communications
- Sales workflows and expose generation
- Financing and fee calculations
- Internal operator workflows and a separate customer portal
- Tenant-aware identity, storage, caching, auditability, and service boundaries

## My contribution

I was one of the lead engineers on a team of 5-6. My work covered the core backend, the customer-portal backend, three of the four operator-facing frontends, data-model evolution, identity integration, and cross-service workflows.

## Architecture at a glance

~~~mermaid
flowchart TB
    Admin[Admin service] --> DB[(PostgreSQL - 5 isolated schemas)]
    Hub[Hub service] --> DB
    Sales[Sales service] --> DB
    Finance[Finance service] --> DB
    Portal[Customer portal backend] --> DB
    Operator[Operator frontends] --> Auth[Keycloak - operator realm]
    Customer[Customer portal] --> CustomerAuth[Keycloak - customer realm]
    Auth --> Admin
    Auth --> Hub
    Auth --> Sales
    Auth --> Finance
    CustomerAuth --> Portal
    Admin <--> MQ[RabbitMQ topic exchange with dead-letter queues]
    Hub <--> MQ
    Sales <--> MQ
    Finance <--> MQ
    Services[Service layer] --> Cache[Redis - tenant namespaced keys]
    Services --> Storage[S3-compatible storage - tenant scoped keys]
~~~

## Engineering highlights

- **Modular backend:** Four FastAPI services plus a separate customer-portal backend, backed by a shared PostgreSQL database with five service-owned schemas.
- **Data isolation:** A shared tenant-aware model convention, explicit service-layer filtering, no cross-schema foreign keys, tenant-scoped object storage, and tenant-namespaced cache keys.
- **Identity boundaries:** Keycloak OAuth2/OIDC with separate operator and customer realms so internal staff and external customers do not share an ambient identity boundary.
- **Event-driven workflows:** RabbitMQ topic exchange, dead-letter queues, and HMAC-signed webhooks for cross-service and external workflow updates.
- **Product delivery:** Next.js, React, TypeScript, TanStack Query, and Radix UI across three operator-facing modules.
- **Data-model evolution:** Authored and reviewed Alembic migrations as the platform expanded to 52 models.

## Repository map

- docs/ARCHITECTURE.md - service boundaries, data flow, and deployment shape
- docs/SECURITY-BOUNDARY.md - public-safe explanation of tenant isolation and identity boundaries

## Technology stack

**Python, FastAPI, PostgreSQL, SQLAlchemy, Alembic, Keycloak, RabbitMQ, Redis, S3-compatible storage, Docker Compose, Next.js, React, TypeScript, TanStack Query, Radix UI**

## Why the production code is not included

The public repository demonstrates engineering judgment without publishing client-owned implementation details. The private implementation includes business rules, integrations, internal documentation, and operational configuration that do not belong in a public portfolio.

## Contact

**Muhammad Muzamil** - Full-Stack Developer

- Portfolio: https://muzamil-portfolio-coral.vercel.app/
- LinkedIn: https://www.linkedin.com/in/muhammad-muzamil-82264a288/
- Email: muzamilshah1029@gmail.com
