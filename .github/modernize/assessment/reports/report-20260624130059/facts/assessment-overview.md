# Assessment Overview

This document serves as the navigation entry point for all supplementary assessment documents generated for the **eShopOnWeb** application. Each document provides a different dimension of analysis to support modernization planning.

## Supplementary Documents

| Document | Description |
|---|---|
| [Architecture Diagram](./architecture-diagram.md) | Two-layer architecture visualization: application-level architecture with technology stack summary, and component relationship diagram showing how controllers, services, repositories, and infrastructure components interact. |
| [Dependency Map](./dependency-map.md) | Visual map of all external NuGet package dependencies grouped by functional category (Web Frameworks, Database/ORM, Security, Utilities, etc.), including version information, compatibility risks, and test dependencies. |
| [API & Service Communication Contracts](./api-service-contracts.md) | Catalog of all REST API endpoints (PublicApi and Web), DTOs and request/response contracts, communication patterns (sync/async), security posture (JWT and cookie auth), and a sequence diagram showing primary request flows. |
| [Data Architecture & Persistence Layer](./data-architecture.md) | Database configuration per environment, EF Core entity model with ER diagram, repository interfaces and custom query methods, caching strategy, data ownership boundaries, and data classification (PII/PHI/PCI). |
| [Configuration & Externalized Settings Inventory](./configuration-inventory.md) | Comprehensive inventory of all configuration sources (appsettings.json files, environment variables, Azure Key Vault), build and runtime profiles (Development, Docker, Production), properties inventory per service, startup dependency chain, and framework/runtime versions. |
| [Core Business Workflows](./business-workflows.md) | End-to-end documentation of key business processes (catalog browsing, basket management, checkout/order placement, anonymous-to-authenticated basket transfer, admin catalog management), domain entities, service-to-domain mapping, and business rules & decision logic. |
