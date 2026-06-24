# Configuration & Externalized Settings Inventory

eShopOnWeb uses ASP.NET Core's layered `appsettings.json` configuration system across three environments (Development, Docker, Production), with Azure Key Vault as the production secret store and user secrets for local developer overrides.

## Configuration Sources

| Source | Type | Path / Location | Notes |
|---|---|---|---|
| appsettings.json | JSON file | src/Web/appsettings.json, src/PublicApi/appsettings.json | Base defaults for all environments |
| appsettings.Development.json | JSON file | src/Web/appsettings.Development.json, src/PublicApi/appsettings.Development.json | Overrides for Development environment; higher log verbosity |
| appsettings.Docker.json | JSON file | src/Web/appsettings.Docker.json, src/PublicApi/appsettings.Docker.json | Overrides for Docker environment; SQL Server container connection strings |
| User Secrets | .NET User Secrets | ~/.microsoft/usersecrets/{UserSecretsId}/ | Local developer secret overrides; not committed to source control |
| Environment Variables | OS / Container env | ASPNETCORE_ENVIRONMENT, ASPNETCORE_URLS, connection strings | Highest precedence; set via docker-compose.override.yml or deployment |
| Azure Key Vault | Azure secret store | URI from AZURE_KEY_VAULT_ENDPOINT env var | Production only; loaded via `Azure.Extensions.AspNetCore.Configuration.Secrets` using ChainedTokenCredential |
| docker-compose.yml | Docker Compose | docker-compose.yml, docker-compose.override.yml | Defines services, ports, environment variables for local container orchestration |

No Spring Cloud Config, Consul, or AWS AppConfig external config servers are used.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies / Plugins |
|---|---|---|---|
| Debug | Default (manual: `dotnet build`) | Local development build; no optimization; includes debug symbols | All packages; `BuildBundlerMinifier` excluded |
| Release | Manual: `dotnet build -c Release` or `dotnet publish -c Release` | Production build; optimized IL, no debug symbols | Adds `BuildBundlerMinifier` (bundling/minification of CSS/JS) |

No Maven/Gradle build profiles; no conditional compilation symbols defined beyond `Debug`/`Release`.

## Runtime Profiles

| Profile | Activation Method | Config Files Loaded | Key Overrides vs. Base |
|---|---|---|---|
| Development | `ASPNETCORE_ENVIRONMENT=Development` (default in launchSettings.json) | appsettings.json + appsettings.Development.json | Log levels elevated to Debug/Information; LocalDB connection strings; Developer exception page; Migrations endpoint enabled |
| Docker | `ASPNETCORE_ENVIRONMENT=Docker` (set in docker-compose.override.yml) | appsettings.json + appsettings.Docker.json | SQL Server container connection strings (sa credentials); HTTP URLs on port 8080; host.docker.internal base URLs |
| Production (default) | `ASPNETCORE_ENVIRONMENT` not set, or set to `Production` | appsettings.json + Azure Key Vault | Azure SQL connection strings from Key Vault; HSTS enabled; standard exception handler |

## Properties Inventory

### Web (src/Web)

| Property Key | Default Value | Profile Override | Source |
|---|---|---|---|
| baseUrls:apiBase | https://localhost:5099/api/ | Docker: http://localhost:5200/api/ | appsettings.json / appsettings.Docker.json |
| baseUrls:webBase | https://localhost:44315/ | Docker: http://host.docker.internal:5106/ | appsettings.json / appsettings.Docker.json |
| ConnectionStrings:CatalogConnection | LocalDB (eShopOnWeb.CatalogDb) | Docker: SQL Server container (sa) | appsettings.json / appsettings.Docker.json |
| ConnectionStrings:IdentityConnection | LocalDB (eShopOnWeb.Identity) | Docker: SQL Server container (sa) | appsettings.json / appsettings.Docker.json |
| CatalogBaseUrl | (empty string) | — | appsettings.json |
| AZURE_KEY_VAULT_ENDPOINT | — | Production: set via environment variable | Environment variable |
| AZURE_SQL_CATALOG_CONNECTION_STRING_KEY | — | Production: Key Vault secret key name | Environment variable / Key Vault |
| AZURE_SQL_IDENTITY_CONNECTION_STRING_KEY | — | Production: Key Vault secret key name | Environment variable / Key Vault |
| UseOnlyInMemoryDatabase | (not set; defaults to false) | Test: true | Environment variable / appsettings |
| Logging:LogLevel:Default | Warning | Development: Debug; Docker: Information | appsettings.json overridden per environment |
| Logging:LogLevel:Microsoft | Warning | Development: Information; Docker: Warning | appsettings.json overridden per environment |
| AllowedHosts | * | — | appsettings.json |

### PublicApi (src/PublicApi)

| Property Key | Default Value | Profile Override | Source |
|---|---|---|---|
| baseUrls:apiBase | https://localhost:5099/api/ | Docker: http://localhost:5200/api/ | appsettings.json / appsettings.Docker.json |
| baseUrls:webBase | https://localhost:5001/ | Docker: http://host.docker.internal:5106/ | appsettings.json / appsettings.Docker.json |
| ConnectionStrings:CatalogConnection | LocalDB (eShopOnWeb.CatalogDb) | Docker: SQL Server container (sa) | appsettings.json / appsettings.Docker.json |
| ConnectionStrings:IdentityConnection | LocalDB (eShopOnWeb.Identity) | Docker: SQL Server container (sa) | appsettings.json / appsettings.Docker.json |
| Logging:LogLevel:Default | Warning | Development: Information | appsettings.json overridden per environment |
| Logging:LogLevel:Microsoft.Hosting.Lifetime | (not set) | Development: Information | appsettings.Development.json |

## Startup Parameters & Resource Requirements

| Service | Runtime / Startup Options | Ports | Memory / CPU | Instance Count |
|---|---|---|---|---|
| eshopwebmvc (Docker) | ASPNETCORE_ENVIRONMENT=Docker; ASPNETCORE_URLS=http://+:8080 | Host: 5106 → Container: 8080 | Not configured (no Docker mem_limit) | 1 |
| eshoppublicapi (Docker) | ASPNETCORE_ENVIRONMENT=Docker; ASPNETCORE_URLS=http://+:8080 | Host: 5200 → Container: 8080 | Not configured (no Docker mem_limit) | 1 |
| sqlserver (Docker) | SA_PASSWORD=[MASKED]; ACCEPT_EULA=Y | 1433:1433 | Not configured | 1 |
| Web (Development) | Default Kestrel settings; HTTPS on 44315 or 5001 | 44315 (HTTPS), 5106 (HTTP) | Not configured | 1 |
| PublicApi (Development) | Default Kestrel settings; HTTPS on 5099 | 5099 | Not configured | 1 |

No JVM settings (Java project); no explicit .NET thread pool or GC tuning configured.

## Startup Dependency Chain

```
sqlserver (SQL Server container)
    └─► eshopwebmvc  (docker-compose depends_on: sqlserver — simple start order, no health check)
    └─► eshoppublicapi  (docker-compose depends_on: sqlserver — simple start order, no health check)
```

**Startup order:**
1. `sqlserver` — SQL Server Edge container must be accepting connections before Web/API services can complete their EF Core migrations and seed data.
2. `eshopwebmvc` / `eshoppublicapi` — Both start after `sqlserver`. Each runs `CatalogContextSeed.SeedAsync()` and `AppIdentityDbContextSeed.SeedAsync()` synchronously during startup; if SQL Server is not yet ready, startup will fail with a connection error.

**Mechanism:** Docker Compose `depends_on` (no `condition: service_healthy`; no `dockerize` or Kubernetes readiness probes). There is no retry/backoff loop on DB connection at startup. In Kubernetes or production Azure deployments no startup dependency configuration is present in the repository.

**Health check endpoints:**
- `/health` — Web project; responds with JSON status for `api_health_check` and `home_page_health_check`
- `/swagger` — PublicApi; Swagger UI availability (dev mode only)

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage |
|---|---|---|
| ConnectionStrings:CatalogConnection (sa password in Docker) | SQL Server SA password | appsettings.Docker.json (plaintext — development only) |
| ConnectionStrings:IdentityConnection (sa password in Docker) | SQL Server SA password | appsettings.Docker.json (plaintext — development only) |
| SA_PASSWORD (Docker Compose) | SQL Server SA password | docker-compose.yml (plaintext — development only) |
| AZURE_KEY_VAULT_ENDPOINT | Key Vault URI | Environment variable (production) |
| AZURE_SQL_CATALOG_CONNECTION_STRING_KEY | Key Vault secret name for catalog connection string | Environment variable (production) |
| AZURE_SQL_IDENTITY_CONNECTION_STRING_KEY | Key Vault secret name for identity connection string | Environment variable (production) |
| UserSecretsId (Web) | ASP.NET User Secrets | aspnet-Web2-1FA3F72E-E7E3-4360-9E49-1CCCD7FE85F7 — stored in ~/.microsoft/usersecrets/ locally |
| UserSecretsId (PublicApi) | ASP.NET User Secrets | 5b662463-1efd-4bae-bde4-befe0be3e8ff — stored in ~/.microsoft/usersecrets/ locally |
| JWT signing key / token config | JWT secret material | Not visible in config files; likely stored in Key Vault or User Secrets |

### Secrets Provisioning Workflow

**Development:** SQL Server connection strings are stored in plaintext in `appsettings.Docker.json` (Docker) or `appsettings.json` (LocalDB). Developers can override sensitive values via .NET User Secrets, stored outside the repository at `~/.microsoft/usersecrets/{UserSecretsId}/secrets.json`.

**Production:** The application uses `ChainedTokenCredential(AzureDeveloperCliCredential, DefaultAzureCredential)` to authenticate to Azure. It reads the Key Vault URI from the `AZURE_KEY_VAULT_ENDPOINT` environment variable, then loads secrets from Azure Key Vault using `AddAzureKeyVault()`. The actual Azure SQL connection strings are stored as Key Vault secrets, with their names referenced via `AZURE_SQL_CATALOG_CONNECTION_STRING_KEY` and `AZURE_SQL_IDENTITY_CONNECTION_STRING_KEY` environment variables. This requires the deployment identity (managed identity or developer CLI identity) to have Key Vault `get`/`list` secret permissions. No Jasypt encryption, DPAPI, or sealed secrets are used.

## Feature Flags

| Flag Name | Default | Controlled By | Notes |
|---|---|---|---|
| UseOnlyInMemoryDatabase | false (not set) | `configuration["UseOnlyInMemoryDatabase"]` env var | Switches both DbContexts to EF Core InMemory provider; used in test/CI environments |
| CatalogBaseUrl | (empty) | appsettings.json `CatalogBaseUrl` | Sets a path base prefix on all requests if non-empty; used for reverse-proxy deployments |

No LaunchDarkly, Azure App Configuration feature flags, or `Microsoft.FeatureManagement` library is used.

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET SDK | 8.0.x (rollForward: latestFeature) | global.json |
| ASP.NET Core | 8.0.2 | Directory.Packages.props (AspNetVersion) |
| Entity Framework Core | 8.0.2 | Directory.Packages.props (EntityFramworkCoreVersion) |
| Blazor WebAssembly | 8.0.2 | Directory.Packages.props |
| ASP.NET Core Identity | 8.0.2 | Directory.Packages.props |
| System.IdentityModel.Tokens.Jwt | 7.3.1 | Directory.Packages.props |
| Azure.Identity | 1.10.4 | Directory.Packages.props |
| Azure.Extensions.AspNetCore.Configuration.Secrets | 1.3.1 | Directory.Packages.props |
| AutoMapper | 12.0.1 | Directory.Packages.props |
| MediatR | 12.0.1 | Directory.Packages.props |
| FluentValidation | 11.9.0 | Directory.Packages.props |
| Ardalis.Specification | 7.0.0 | Directory.Packages.props |
| Swashbuckle.AspNetCore | 6.5.0 | Directory.Packages.props |
| Docker base image (build) | mcr.microsoft.com/dotnet/sdk:8.0 | src/Web/Dockerfile, src/PublicApi/Dockerfile |
| Docker base image (runtime) | mcr.microsoft.com/dotnet/aspnet:8.0 | src/Web/Dockerfile, src/PublicApi/Dockerfile |
| SQL Server (Docker) | mcr.microsoft.com/azure-sql-edge (latest) | docker-compose.yml |
| Target framework moniker | net8.0 | Directory.Packages.props (TargetFramework) |
