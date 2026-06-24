# Architecture Diagram

eShopOnWeb is a multi-project ASP.NET Core reference application implementing a modular, layered architecture with a Razor Pages / MVC web frontend, a Blazor WebAssembly admin panel, and a separate REST API service.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
        BlazorWasm["Blazor WebAssembly (BlazorAdmin)"]
    end
    subgraph Web["Web Application - ASP.NET Core (Web)"]
        RazorPages["Razor Pages + MVC Controllers"]
        BlazorServer["Blazor Server-Side (BlazorAdmin hosted)"]
        Auth["ASP.NET Core Identity + Cookie Auth"]
        HealthCheck["Health Checks"]
    end
    subgraph API["Public API - ASP.NET Core Minimal API (PublicApi)"]
        RestEndpoints["REST Endpoints (Catalog, Auth)"]
        JwtAuth["JWT ******"]
    end
    subgraph AppCore["Application Core (ApplicationCore)"]
        Services["Domain Services (BasketService, OrderService)"]
        Entities["Domain Entities (Basket, Order, CatalogItem)"]
        Specs["Ardalis Specifications"]
        Interfaces["Repository Interfaces"]
    end
    subgraph Infra["Infrastructure (Infrastructure)"]
        EFRepo["EF Core Repository (EfRepository)"]
        CatalogCtx["CatalogContext (SQL Server)"]
        IdentityCtx["AppIdentityDbContext (SQL Server)"]
        IdentitySvc["Identity Token Service"]
    end
    subgraph DataLayer["Data Layer"]
        SqlCatalog[("SQL Server - Catalog DB")]
        SqlIdentity[("SQL Server - Identity DB")]
    end
    subgraph External["External Services"]
        AzureKV["Azure Key Vault"]
        AzureIdentity["Azure Managed Identity"]
    end

    Browser -->|"HTTPS requests"| RazorPages
    BlazorWasm -->|"REST calls"| RestEndpoints
    RazorPages --> Auth
    Auth -->|"authorized"| Services
    BlazorServer -->|"hosted in"| Web
    RazorPages -->|"uses"| Services
    RestEndpoints -->|"uses"| Services
    JwtAuth -->|"validates"| RestEndpoints
    Services -->|"via interfaces"| Interfaces
    Interfaces -->|"implemented by"| EFRepo
    EFRepo -->|"queries"| CatalogCtx
    EFRepo -->|"queries"| IdentityCtx
    CatalogCtx -->|"SQL"| SqlCatalog
    IdentityCtx -->|"SQL"| SqlIdentity
    IdentitySvc -->|"issues JWT"| JwtAuth
    Web -->|"secrets"| AzureKV
    AzureKV -->|"auth"| AzureIdentity
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| Presentation (Web) | ASP.NET Core Razor Pages + MVC | .NET 8 | Server-side web UI for storefront |
| Presentation (Admin) | Blazor WebAssembly | .NET 8 | SPA admin panel hosted in the web app |
| Presentation (Shared) | BlazorShared | .NET 8 | Shared Blazor models and DTOs |
| API | ASP.NET Core Minimal Endpoints | .NET 8 | Public REST API for catalog and auth |
| Application Core | .NET Class Library | .NET 8 | Domain entities, services, interfaces |
| Infrastructure | Entity Framework Core | 8.x | Data access, Identity, repository impl. |
| Database | SQL Server (localdb / Azure SQL) | — | Catalog and Identity data stores |
| Authentication | ASP.NET Core Identity + JWT ****** — | Cookie auth (Web) and JWT auth (API) |
| Dependency Injection | ASP.NET Core built-in DI | — | Service registration and resolution |
| Configuration Secrets | Azure Key Vault | — | Production secret management |
| Health Checks | ASP.NET Core Health Checks | — | API and homepage liveness probes |

### Data Storage & External Services

The application uses two SQL Server databases: `CatalogDb` (catalog items, brands, types, orders, baskets) and an `Identity` database (users, roles) both accessed via Entity Framework Core. In local/development mode these are SQL Server LocalDB instances; in production they are Azure SQL databases with connection strings retrieved from Azure Key Vault. An in-memory EF Core provider is used for integration tests. No cache or message broker is currently used.

### Key Architectural Decisions

- **Clean Architecture with repository pattern**: Application Core defines domain entities and repository interfaces; Infrastructure implements EF Core repositories (`EfRepository`) using Ardalis.Specification for query abstraction.
- **Dual authentication schemes**: The Web project uses cookie-based ASP.NET Core Identity; the PublicApi project uses JWT ****** issued by `IdentityTokenClaimService`.
- **Blazor WebAssembly hosted within MVC**: `BlazorAdmin` is a WASM SPA served as static files from the Web project, communicating with the Public API via HTTP.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        HomeIdx["Index Page"]
        BasketPage["Basket Pages"]
        OrderCtrl["OrderController"]
        ManageCtrl["ManageController"]
        CatalogAPI["CatalogItem Endpoints"]
        AuthAPI["Auth Endpoint"]
        BlazorPages["BlazorAdmin Pages"]
    end
    subgraph Business["Business Logic"]
        BasketSvc["BasketService"]
        OrderSvc["OrderService"]
        TokenSvc["IdentityTokenClaimService"]
        UriComp["UriComposer"]
    end
    subgraph DataAccess["Data Access"]
        EFRepo["EfRepository"]
        CatalogCtx["CatalogContext"]
        IdentityCtx["AppIdentityDbContext"]
        BasketQuery["BasketQueryService"]
    end
    subgraph Infra["Infrastructure / Cross-Cutting"]
        AuthMiddleware["Authentication Middleware"]
        ExceptionMiddleware["Exception Middleware"]
        HealthChecks["HealthChecks"]
        LoggingSvc["AppLogger"]
    end

    HomeIdx -->|"queries"| BasketSvc
    BasketPage -->|"delegates"| BasketSvc
    OrderCtrl -->|"delegates"| OrderSvc
    CatalogAPI -->|"reads"| EFRepo
    AuthAPI -->|"delegates"| TokenSvc
    BlazorPages -->|"REST calls"| CatalogAPI
    BasketSvc -->|"queries"| EFRepo
    BasketSvc -->|"reads"| BasketQuery
    OrderSvc -->|"queries"| EFRepo
    TokenSvc -->|"creates JWT"| AuthAPI
    EFRepo -->|"uses"| CatalogCtx
    EFRepo -->|"uses"| IdentityCtx
    AuthMiddleware -.->|"intercepts"| Presentation
    ExceptionMiddleware -.->|"wraps"| Presentation
    LoggingSvc -.->|"used by"| Business
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| Index Page | Presentation | Razor Page | Catalog browsing and product listing |
| Basket Pages | Presentation | Razor Pages | Shopping cart management (add, update, checkout) |
| OrderController | Presentation | MVC Controller | Order history display for authenticated users |
| ManageController | Presentation | MVC Controller | User account management |
| CatalogItem Endpoints | Presentation | Minimal API Endpoints | CRUD operations for catalog items via REST |
| Auth Endpoint | Presentation | Minimal API Endpoint | JWT token issuance for API clients |
| BlazorAdmin Pages | Presentation | Blazor WASM Pages | Admin SPA for catalog management |
| BasketService | Business Logic | Domain Service | Basket CRUD, item transfer to order |
| OrderService | Business Logic | Domain Service | Order creation from basket |
| IdentityTokenClaimService | Business Logic | Identity Service | JWT token generation for API auth |
| UriComposer | Business Logic | Utility Service | Constructs catalog image URIs |
| EfRepository | Data Access | Generic Repository | EF Core-backed generic repository with Ardalis.Specification |
| CatalogContext | Data Access | DbContext | Catalog, Order, Basket entity persistence |
| AppIdentityDbContext | Data Access | DbContext | User and role identity persistence |
| BasketQueryService | Data Access | Query Service | Dapper-based basket queries |
| Authentication Middleware | Infrastructure | Middleware | Cookie and JWT authentication pipeline |
| Exception Middleware | Infrastructure | Middleware | Global exception handling |
| HealthChecks | Infrastructure | Health Checks | API and home page liveness probes |
| AppLogger | Infrastructure | Logging | Generic typed logger wrapper |
