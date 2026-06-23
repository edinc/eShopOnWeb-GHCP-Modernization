# Modernization Plan: eShopOnWeb Cloud Modernization

**Project**: eShopOnWeb

---

## Technical Framework

- **Language**: C# (.NET 8 → .NET 10)
- **Framework**: ASP.NET Core 8.0, Blazor WebAssembly 8.0
- **Build Tool**: MSBuild / dotnet CLI
- **Database**: SQL Server via EF Core 8.0 (Azure SQL in production)
- **Key Dependencies**: ASP.NET Core Identity 8.0.2, EF Core 8.0.2, MediatR 12.0.1, Ardalis.Specification 7.0.0, Azure.Identity 1.10.4, System.IdentityModel.Tokens.Jwt 7.3.1

---

## Overview

This migration modernizes the eShopOnWeb multi-project e-commerce application from .NET 8 to .NET 10 and remediates cloud readiness issues identified in the assessment. The application currently targets `net8.0` across 10 projects (6 production, 4 test), contains hardcoded sensitive data (JWT secrets, auth keys, default passwords) in source code, a hardcoded URL template, and has known security vulnerabilities in several NuGet dependencies. The new architecture will:

- Upgrade all 10 projects to .NET 10 (`net10.0`) for continued LTS support, resolving mandatory binary/source incompatibilities and updating 24 NuGet packages
- Replace hardcoded JWT/auth secrets and default passwords in source code with Azure Key Vault-backed secrets using Managed Identity authentication
- Externalize the hardcoded URL template in `UriComposer.cs` to Azure App Configuration for environment-specific configuration management
- Remediate known CVE vulnerabilities in dependencies (Azure.Identity, System.Text.Json) and remove/replace deprecated packages

The migration follows a phased approach: first upgrading the runtime and resolving framework incompatibilities, then addressing code-level security issues, and finally performing a comprehensive CVE scan and package remediation pass.

---

## Migration Impact Summary

| Application | Original Service                   | New Azure Service        | Authentication   | Comments                                                                 |
|-------------|------------------------------------|--------------------------|------------------|--------------------------------------------------------------------------|
| eShopOnWeb  | .NET 8 (net8.0)                    | .NET 10 (net10.0)        | N/A              | All 10 projects upgraded; 24 NuGet packages updated; API incompatibilities fixed |
| eShopOnWeb  | Hardcoded secrets in source code   | Azure Key Vault          | Managed Identity | JWT_SECRET_KEY, AUTH_KEY, DEFAULT_PASSWORD externalized from AuthorizationConstants.cs |
| eShopOnWeb  | Hardcoded URL in UriComposer.cs    | Azure App Configuration  | Managed Identity | CatalogBaseUrl template URL externalized to App Configuration            |
| eShopOnWeb  | Vulnerable NuGet dependencies      | Updated/patched packages | N/A              | Azure.Identity ≥1.21.0, System.Text.Json patched, deprecated packages replaced |

---

## Tasks

### Task 1 — Upgrade .NET to Latest LTS (net10.0)

Upgrade all eShopOnWeb projects from .NET 8 to .NET 10, updating target framework monikers, NuGet package versions, and fixing binary/source incompatibilities introduced by the new runtime. This addresses all mandatory blockers identified in the dotnet-version-upgrade assessment.

**Addresses issues from assessment**: Project's target framework(s) needs to be changed, Binary incompatible for selected .NET version, NuGet package is incompatible, NuGet package functionality is included with framework reference, IdentityModel & Claims-based Security, NuGet package upgrade is recommended, Source incompatible for selected .NET version, Behavioral change in selected .NET version, NuGet package is deprecated.

---

### Task 2 — Migrate Hardcoded Secrets to Azure Key Vault

Remove hardcoded sensitive constants (`JWT_SECRET_KEY`, `AUTH_KEY`, `DEFAULT_PASSWORD`) from `src/ApplicationCore/Constants/AuthorizationConstants.cs` and retrieve them from Azure Key Vault using Managed Identity authentication.

**Addresses issues from assessment**: Hardcoded sensitive data detected.

---

### Task 3 — Externalize Hardcoded URL to Azure App Configuration

Externalize the hardcoded URL template string (`http://catalogbaseurltobereplaced`) in `src/ApplicationCore/Services/UriComposer.cs` to Azure App Configuration, eliminating the hard-coded placeholder from source code and enabling per-environment URL management.

**Addresses issues from assessment**: Hardcoded URLs detected.

---

### Task 4 — Security / CVE Remediation

Scan all project dependencies for known CVEs and remediate identified vulnerabilities. Upgrade `Azure.Identity` from 1.10.4 to ≥1.21.0, update `System.Text.Json` to a patched version, and address any additional findings from the CVE scan. Verify the project builds and all tests pass after remediation.

**Addresses issues from assessment**: NuGet package contains security vulnerability (Azure.Identity 1.10.4, System.Text.Json 8.0.3), NuGet package is deprecated (AutoMapper.Extensions.Microsoft.DependencyInjection, System.IdentityModel.Tokens.Jwt, xunit packages).

---

## Open Questions & Questionnaire

- [x] Q: Should the plan include environment/infrastructure provisioning? → A: No — use infrastructure already defined in the repository (infra/main.bicep and azure.yaml are present)
- [x] Q: Should the plan include integration testing? → A: No — skip integration testing (not explicitly requested)
- [x] Q: Should the plan include a security scan and CVE remediation task? → A: Yes — always included by default
- [x] Q: Which Azure deployment target should the plan use? → A: No deployment — migration only (not explicitly requested in assessment scope)
