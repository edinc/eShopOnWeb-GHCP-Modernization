# Configuration & Externalized Settings Inventory

The repository uses a layered .NET configuration model built from `appsettings` files, launch profiles, Docker Compose settings, Azure deployment descriptors, and optional Azure Key Vault secrets. Most runtime variation is driven by `ASPNETCORE_ENVIRONMENT`, with additional environment-specific behavior for Docker, test, and production cloud runs.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web appsettings | JSON | `src/Web/appsettings.json` | Base storefront, connection string, base URL, and logging settings |
| Web environment appsettings | JSON | `src/Web/appsettings.Development.json`, `src/Web/appsettings.Docker.json` | Environment-specific URL and connection overrides |
| PublicApi appsettings | JSON | `src/PublicApi/appsettings.json` | Base API connection strings, base URLs, and logging settings |
| PublicApi environment appsettings | JSON | `src/PublicApi/appsettings.Development.json`, `src/PublicApi/appsettings.Docker.json`, `tests/PublicApiIntegrationTests/appsettings.test.json` | Docker and test overrides, including in-memory DB mode |
| BlazorAdmin settings | JSON | `src/BlazorAdmin/wwwroot/appsettings*.json` | Client-side base URLs and logging |
| Launch profiles | JSON | `src/*/Properties/launchSettings.json`, `.vscode/launch.json` | Dev-time URLs and environment variables |
| Docker Compose | YAML | `docker-compose.yml`, `docker-compose.override.yml` | Local multi-container topology, ports, and environment overrides |
| Azure deployment descriptor | YAML | `azure.yaml` | Maps the `Web` project to Azure App Service |
| Azure infrastructure | Bicep/JSON | `infra/main.bicep`, `infra/main.parameters.json` | Cloud resource definitions and secret placeholders |
| Central package/runtime settings | JSON / MSBuild | `global.json`, `Directory.Packages.props` | SDK pinning and central package version management |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Default local build | Developer compilation, test runs, and local debugging | Standard package graph |
| Release | `-c Release` | Publish/optimized builds | Enables `BuildBundlerMinifier` in `Web` |
| Docker | Visual Studio / Compose profile | Containerized local run for `Web` and `PublicApi` | Dockerfiles, Azure container tooling |
| Production publish | `Web - PROD` launch profile or Azure deployment | Run `Web` with production environment settings | Azure Key Vault integration and Azure-hosted SQL connection indirection |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Development | `ASPNETCORE_ENVIRONMENT=Development` | Base `appsettings.json` plus `appsettings.Development.json` and launch settings | Localhost URLs, local DB connection strings, developer exception pages |
| Docker | `ASPNETCORE_ENVIRONMENT=Docker` from Compose | Base `appsettings.json` plus `appsettings.Docker.json` | Container URLs, SQL Edge host names, Compose port bindings |
| Production | `ASPNETCORE_ENVIRONMENT=Production` or Azure host default | Base config plus environment variables and Key Vault values | Azure Key Vault-backed connection resolution, HSTS, exception handler |
| Test | `appsettings.test.json` loaded by `PublicApi` tests | Test settings file | `UseOnlyInMemoryDatabase=true` |

## Properties Inventory

### Web

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `baseUrls.apiBase` | `https://localhost:5099/api/` | Docker overrides host names; Development uses base | `src/Web/appsettings*.json` |
| `baseUrls.webBase` | `https://localhost:44315/` | Docker override points to container port | `src/Web/appsettings*.json` |
| `ConnectionStrings.CatalogConnection` | LocalDB SQL connection | Docker override; Production resolved from Key Vault indirection | `src/Web/appsettings*.json`, environment variables |
| `ConnectionStrings.IdentityConnection` | LocalDB SQL connection | Docker override; Production resolved from Key Vault indirection | `src/Web/appsettings*.json`, environment variables |
| `CatalogBaseUrl` | empty string | Optional per environment | `src/Web/appsettings.json` |
| `Logging.LogLevel.*` | `Warning` defaults | Environment-specific as needed | `src/Web/appsettings.json` |
| `AZURE_KEY_VAULT_ENDPOINT` | none in repo | Production only | Environment variable |
| `AZURE_SQL_CATALOG_CONNECTION_STRING_KEY` | none in repo | Production only | Environment variable |
| `AZURE_SQL_IDENTITY_CONNECTION_STRING_KEY` | none in repo | Production only | Environment variable |

### PublicApi

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `baseUrls.apiBase` | `https://localhost:5099/api/` | Docker override | `src/PublicApi/appsettings*.json` |
| `baseUrls.webBase` | `https://localhost:5001/` | Docker override | `src/PublicApi/appsettings*.json` |
| `ConnectionStrings.CatalogConnection` | LocalDB SQL connection | Docker override; tests bypass via in-memory mode | `src/PublicApi/appsettings*.json` |
| `ConnectionStrings.IdentityConnection` | LocalDB SQL connection | Docker override; tests bypass via in-memory mode | `src/PublicApi/appsettings*.json` |
| `CatalogBaseUrl` | empty string | Optional per environment | `src/PublicApi/appsettings.json` |
| `UseOnlyInMemoryDatabase` | false / unset | Test profile only | `tests/PublicApiIntegrationTests/appsettings.test.json` |

### BlazorAdmin

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `baseUrls.apiBase` | `https://localhost:5099/api/` | Docker/Development variants | `src/BlazorAdmin/wwwroot/appsettings*.json` |
| `baseUrls.webBase` | `https://localhost:44315/` | Docker/Development variants | `src/BlazorAdmin/wwwroot/appsettings*.json` |
| `Logging.LogLevel.Default` | `Information` | Environment-specific variants possible | `src/BlazorAdmin/wwwroot/appsettings.json` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| Web | `ASPNETCORE_ENVIRONMENT`, optional `ASPNETCORE_URLS`, optional Azure Key Vault env vars | Not specified in repo | Not specified |
| PublicApi | `ASPNETCORE_ENVIRONMENT`, optional `ASPNETCORE_URLS` | Not specified in repo | Not specified |
| BlazorAdmin | Browser-hosted WASM runtime, no custom memory flags | Browser-managed | User-driven |
| sqlserver container | Docker env vars for SQL bootstrap | Not specified in Compose | 1 in local Compose |

## Startup Dependency Chain

1. `sqlserver` → awaited implicitly by `Web` and `PublicApi` in Docker via `depends_on`; both applications still rely on runtime EF migration retries during seeding.
2. `Web` / `PublicApi` → each builds its DI container, configures EF Core, then runs database seeding before accepting traffic.
3. `Web` production startup → additionally requires Azure Key Vault access before SQL connection strings can be resolved.
4. Health readiness is exposed mainly from `Web` through `/health`, `home_page_health_check`, and `api_health_check`; Docker Compose does not define health-gated startup conditions.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `ConnectionStrings.CatalogConnection` | SQL connection string | `appsettings*.json` / Key Vault indirection, credentials masked |
| `ConnectionStrings.IdentityConnection` | SQL connection string | `appsettings*.json` / Key Vault indirection, credentials masked |
| `SA_PASSWORD` | SQL admin password | Docker Compose environment variable `[MASKED]` |
| `UserSecretsId` values in `Web` and `PublicApi` | Developer secret store reference | ASP.NET Core user secrets store |
| `AZURE_KEY_VAULT_ENDPOINT` | Key Vault URI | Environment variable |
| `sqlAdminPassword`, `appUserPassword` | Azure deployment secrets | Generated/retrieved through infra parameter helpers |

### Secrets Provisioning Workflow

Local development uses checked-in config files plus ASP.NET Core user secrets references and, in Docker mode, container environment variables. Production configuration shifts secret ownership to Azure Key Vault: the `Web` project authenticates with chained Azure credentials, resolves the Key Vault endpoint from environment variables, then fetches the secret names that hold the effective SQL connection strings. Infrastructure parameters also reference generated or retrieved passwords for Azure resources. `PublicApi` does not contain the same Key Vault wiring in code, so its secure production secret workflow would need to be supplied by deployment-time environment configuration.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| None detected | n/a | No dedicated feature flag framework or conditional feature toggles found |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET SDK | `8.0.x` | `global.json` |
| Target framework | `net8.0` | `Directory.Packages.props` and project files |
| ASP.NET Core packages | `8.0.2` | `Directory.Packages.props` |
| EF Core packages | `8.0.2` | `Directory.Packages.props` |
| Azure Identity | `1.10.4` | `Directory.Packages.props` |
| MediatR | `12.0.1` | `Directory.Packages.props` |
| Swashbuckle | `6.5.0` | `Directory.Packages.props` |
| Docker base image - Web runtime | `mcr.microsoft.com/dotnet/aspnet:8.0` | `src/Web/Dockerfile` |
| Docker base image - Web/PublicApi build | `mcr.microsoft.com/dotnet/sdk:8.0` | `src/Web/Dockerfile`, `src/PublicApi/Dockerfile` |
| Docker database image | `mcr.microsoft.com/azure-sql-edge` | `docker-compose.yml` |
| Build tool | MSBuild / `dotnet` CLI | Solution and project format |
