# ApplicationCore

## Summary

| Metric | Value |
|--------|-------|
| Total Issues | 2 |
| Mandatory Blockers | 0 |
| Potential Issues | 1 |

## Component Information

| Property | Value |
|----------|-------|
| Language | C# |
| Frameworks | net8.0 |
| Build tools | MSBuild |

## Cloud Readiness Issues

| Issue Name | Criticality | Story Points | Occurrences |
|------------|-------------|--------------|-------------|
| Hardcoded URLs detected | Potential | 1 | [1](#Hardcoded_URLs_detected) |
| Hardcoded sensitive data detected | Optional | 3 | [1](#Hardcoded_sensitive_data_detected) |

### Issue Details

<details id="Hardcoded_URLs_detected">
<summary><b>Hardcoded URLs detected</b> — affected files</summary>

- `src\ApplicationCore\Services\UriComposer.cs (line 12, col 35)`

</details>

<details id="Hardcoded_sensitive_data_detected">
<summary><b>Hardcoded sensitive data detected</b> — affected files</summary>

- `src\ApplicationCore\Constants\AuthorizationConstants.cs (line 10, col 41)`

</details>

## DotNET Upgrade Issues [View Details](scenarios/dotnet-version-upgrade/assessment.md)

| Issue Category | Criticality | Story Points |
|----------------|-------------|--------------|
| Binary incompatible for selected .NET version | Mandatory | 1 |
| Project's target framework(s) needs to be changed | Mandatory | 1 |
| NuGet package functionality is included with framework reference | Mandatory | 1 |
| NuGet package is incompatible | Mandatory | 1 |
| IdentityModel & Claims-based Security | Mandatory | 4 |
| Behavioral change in selected .NET version | Potential | 1 |
| NuGet package upgrade is recommended | Potential | 1 |
| Source incompatible for selected .NET version | Potential | 1 |
| NuGet package is deprecated | Optional | 1 |
| NuGet package contains security vulnerability | Optional | 1 |

### Issue Details

<details>
<summary><b>Binary incompatible for selected .NET version</b> — affected files</summary>

- `src\BlazorAdmin\Program.cs (line 20, col 0)`
- `tests\FunctionalTests\PublicApi\ApiTokenHelper.cs (line 46, col 8)`
- `tests\FunctionalTests\PublicApi\ApiTokenHelper.cs (line 45, col 8)`
- `tests\FunctionalTests\PublicApi\ApiTokenHelper.cs (line 44, col 8)`
- `src\Infrastructure\Identity\IdentityTokenClaimService.cs (line 43, col 8)`
- `src\Infrastructure\Identity\IdentityTokenClaimService.cs (line 42, col 8)`
- `src\Infrastructure\Identity\IdentityTokenClaimService.cs (line 24, col 8)`
- `src\PublicApi\Program.cs (line 84, col 0)`
- `src\PublicApi\Program.cs (line 49, col 0)`
- `src\PublicApi\Program.cs (line 48, col 0)`
- `src\PublicApi\Program.cs (line 42, col 0)`
- `src\PublicApi\Program.cs (line 41, col 0)`
- `tests\PublicApiIntegrationTests\ApiTokenHelper.cs (line 46, col 12)`
- `tests\PublicApiIntegrationTests\ApiTokenHelper.cs (line 45, col 12)`
- `tests\PublicApiIntegrationTests\ApiTokenHelper.cs (line 44, col 12)`
- `src\Web\Configuration\ConfigureWebServices.cs (line 15, col 8)`
- `src\Web\Configuration\ConfigureWebServices.cs (line 10, col 8)`
- `src\Web\Configuration\ConfigureCoreServices.cs (line 21, col 8)`
- `src\Web\Program.cs (line 140, col 0)`
- `src\Web\Program.cs (line 98, col 0)`
- `src\Web\Program.cs (line 97, col 0)`

</details>

<details>
<summary><b>Project's target framework(s) needs to be changed</b> — affected files</summary>

- `src\ApplicationCore\ApplicationCore.csproj`
- `src\BlazorAdmin\BlazorAdmin.csproj`
- `src\BlazorShared\BlazorShared.csproj`
- `tests\FunctionalTests\FunctionalTests.csproj`
- `src\Infrastructure\Infrastructure.csproj`
- `tests\IntegrationTests\IntegrationTests.csproj`
- `src\PublicApi\PublicApi.csproj`
- `tests\PublicApiIntegrationTests\PublicApiIntegrationTests.csproj`
- `tests\UnitTests\UnitTests.csproj`
- `src\Web\Web.csproj`

</details>

<details>
<summary><b>NuGet package functionality is included with framework reference</b> — affected files</summary>

- `src\ApplicationCore\ApplicationCore.csproj`

</details>

<details>
<summary><b>NuGet package is incompatible</b> — affected files</summary>

- `src\PublicApi\PublicApi.csproj`

</details>

<details>
<summary><b>Behavioral change in selected .NET version</b> — affected files</summary>

- `src\BlazorAdmin\Services\HttpService.cs (line 90, col 8)`
- `src\BlazorAdmin\Services\HttpService.cs (line 56, col 12)`
- `src\BlazorAdmin\Program.cs (line 22, col 0)`
- `tests\FunctionalTests\Web\Pages\HomePageOnGet.cs (line 21, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\IndexTest.cs (line 94, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\IndexTest.cs (line 92, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\IndexTest.cs (line 79, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\IndexTest.cs (line 65, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\IndexTest.cs (line 54, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\IndexTest.cs (line 52, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\IndexTest.cs (line 39, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\IndexTest.cs (line 25, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\CheckoutTest.cs (line 64, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\CheckoutTest.cs (line 62, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\CheckoutTest.cs (line 51, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\CheckoutTest.cs (line 43, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\CheckoutTest.cs (line 38, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\CheckoutTest.cs (line 25, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\BasketPageCheckout.cs (line 47, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\BasketPageCheckout.cs (line 40, col 8)`
- `tests\FunctionalTests\Web\Pages\Basket\BasketPageCheckout.cs (line 25, col 8)`
- `tests\FunctionalTests\Web\Controllers\OrderControllerIndex.cs (line 25, col 8)`
- `tests\FunctionalTests\Web\Controllers\CatalogControllerIndex.cs (line 22, col 8)`
- `tests\FunctionalTests\Web\Controllers\AccountControllerSignIn.cs (line 108, col 8)`
- `tests\FunctionalTests\Web\Controllers\AccountControllerSignIn.cs (line 94, col 8)`
- `tests\FunctionalTests\Web\Controllers\AccountControllerSignIn.cs (line 81, col 8)`
- `tests\FunctionalTests\Web\Controllers\AccountControllerSignIn.cs (line 72, col 8)`
- `tests\FunctionalTests\Web\Controllers\AccountControllerSignIn.cs (line 60, col 8)`
- `tests\FunctionalTests\Web\Controllers\AccountControllerSignIn.cs (line 49, col 8)`
- `tests\FunctionalTests\Web\Controllers\AccountControllerSignIn.cs (line 25, col 8)`
- `src\PublicApi\Program.cs (line 31, col 0)`
- `tests\PublicApiIntegrationTests\CatalogItemEndpoints\DeleteCatalogItemEndpointTest.cs (line 20, col 8)`
- `tests\PublicApiIntegrationTests\CatalogItemEndpoints\CatalogItemListPagedEndpoint.cs (line 43, col 8)`
- `tests\PublicApiIntegrationTests\CatalogItemEndpoints\CatalogItemListPagedEndpoint.cs (line 37, col 8)`
- `tests\PublicApiIntegrationTests\CatalogItemEndpoints\CatalogItemListPagedEndpoint.cs (line 21, col 8)`
- `tests\PublicApiIntegrationTests\CatalogItemEndpoints\CatalogItemGetByIdEndpointTest.cs (line 16, col 8)`
- `tests\PublicApiIntegrationTests\CatalogItemEndpoints\CreateCatalogItemEndpointTest.cs (line 43, col 8)`
- `tests\PublicApiIntegrationTests\AuthEndpoints\AuthenticateEndpointTest.cs (line 28, col 8)`
- `src\Web\HealthChecks\HomePageHealthCheck.cs (line 26, col 8)`
- `src\Web\HealthChecks\ApiHealthCheck.cs (line 25, col 8)`
- `src\Web\Program.cs (line 179, col 4)`
- `src\Web\Program.cs (line 101, col 0)`
- `src\Web\Program.cs (line 31, col 4)`
- `src\Web\Program.cs (line 22, col 0)`

</details>

<details>
<summary><b>NuGet package upgrade is recommended</b> — affected files</summary>

- `src\ApplicationCore\ApplicationCore.csproj`
- `src\BlazorAdmin\BlazorAdmin.csproj`
- `tests\FunctionalTests\FunctionalTests.csproj`
- `src\Infrastructure\Infrastructure.csproj`
- `tests\IntegrationTests\IntegrationTests.csproj`
- `src\PublicApi\PublicApi.csproj`
- `tests\PublicApiIntegrationTests\PublicApiIntegrationTests.csproj`
- `src\Web\Web.csproj`

</details>

<details>
<summary><b>Source incompatible for selected .NET version</b> — affected files</summary>

- `src\ApplicationCore\Exceptions\EmptyBasketOnCheckoutException.cs (line 11, col 153)`
- `src\PublicApi\Program.cs (line 62, col 4)`
- `src\PublicApi\Program.cs (line 61, col 4)`
- `src\PublicApi\Program.cs (line 60, col 4)`
- `src\PublicApi\Program.cs (line 56, col 4)`
- `src\PublicApi\Program.cs (line 54, col 0)`
- `src\PublicApi\Program.cs (line 35, col 0)`
- `src\Web\Configuration\ConfigureCookieSettings.cs (line 25, col 12)`
- `src\Web\Program.cs (line 173, col 4)`
- `src\Web\Program.cs (line 113, col 0)`
- `src\Web\Program.cs (line 54, col 0)`
- `src\Web\Program.cs (line 31, col 4)`

</details>

<details>
<summary><b>NuGet package is deprecated</b> — affected files</summary>

- `tests\FunctionalTests\FunctionalTests.csproj`
- `src\Infrastructure\Infrastructure.csproj`
- `tests\IntegrationTests\IntegrationTests.csproj`
- `src\PublicApi\PublicApi.csproj`
- `tests\UnitTests\UnitTests.csproj`
- `src\Web\Web.csproj`

</details>

<details>
<summary><b>NuGet package contains security vulnerability</b> — affected files</summary>

- `src\ApplicationCore\ApplicationCore.csproj`
- `src\Web\Web.csproj`

</details>

---

## Codebase Insights

> **Note:** These documents are generated by AI and may contain inaccuracies or incomplete information. Please review carefully.

1. **[Architecture Diagram](facts/architecture-diagram.md)** — Understand the big picture: system layers and component relationships
2. **[Dependency Map](facts/dependency-map.md)** — Know what the project depends on and where the risks are
3. **[API & Service Contracts](facts/api-service-contracts.md)** — See how services communicate and what contracts they expose
4. **[Data Architecture](facts/data-architecture.md)** — Explore data models, storage, and data flow patterns
5. **[Configuration Inventory](facts/configuration-inventory.md)** — Review how the application is configured across environments
6. **[Business Workflows](facts/business-workflows.md)** — Trace end-to-end business processes and domain logic

[Share feedback](https://aka.ms/ghcp-appmod/feedback)
