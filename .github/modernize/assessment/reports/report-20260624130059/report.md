# ApplicationCore

## Summary

| Metric | Value |
|--------|-------|
| Total Issues | 20 |
| Mandatory Blockers | 5 |
| Potential Issues | 10 |

## Component Information

| Property | Value |
|----------|-------|
| Language | C# |
| Frameworks | net8.0 |
| Build tools | MSBuild |

## Cloud Readiness Issues

| Issue Name | Criticality | Story Points | Occurrences |
|------------|-------------|--------------|-------------|
| Certificate management dependency detected | Potential | 5 | [20](#Certificate_management_dependency_detected) |
| Access to external resources via HTTP is detected | Potential | 3 | [19](#Access_to_external_resources_via_HTTP_is_detected) |
| Local application configuration detected | Potential | 1 | [16](#Local_application_configuration_detected) |
| Hardcoded URLs detected | Potential | 1 | [14](#Hardcoded_URLs_detected) |
| Connection string is detected | Potential | 3 | [8](#Connection_string_is_detected) |
| Data caching is detected | Potential | 3 | [2](#Data_caching_is_detected) |
| Environment variables dependency detected | Potential | 3 | [1](#Environment_variables_dependency_detected) |
| Hardcoded sensitive data detected | Optional | 3 | [34](#Hardcoded_sensitive_data_detected) |
| Synchronous API usage detected | Optional | 1 | [10](#Synchronous_API_usage_detected) |
| Static content detected | Optional | 3 | [2](#Static_content_detected) |

### Issue Details

<details id="Certificate_management_dependency_detected">
<summary><b>Certificate management dependency detected</b> — affected files</summary>

- `tests\FunctionalTests\PublicApi\ApiTokenHelper.cs (line 45)`
- `tests\FunctionalTests\PublicApi\ApiTokenHelper.cs (line 38)`
- `tests\FunctionalTests\PublicApi\ApiTokenHelper.cs (line 38)`
- `tests\FunctionalTests\PublicApi\ApiTokenHelper.cs (line 42)`
- `tests\FunctionalTests\PublicApi\ApiTokenHelper.cs (line 42)`
- `tests\FunctionalTests\PublicApi\ApiTokenHelper.cs (line 42)`
- `src\Infrastructure\Identity\IdentityTokenClaimService.cs (line 42)`
- `src\Infrastructure\Identity\IdentityTokenClaimService.cs (line 36)`
- `src\Infrastructure\Identity\IdentityTokenClaimService.cs (line 36)`
- `src\Infrastructure\Identity\IdentityTokenClaimService.cs (line 40)`
- `src\Infrastructure\Identity\IdentityTokenClaimService.cs (line 40)`
- `src\Infrastructure\Identity\IdentityTokenClaimService.cs (line 40)`
- `src\PublicApi\Program.cs (line 62)`
- `src\PublicApi\Program.cs (line 65)`
- `tests\PublicApiIntegrationTests\ApiTokenHelper.cs (line 45)`
- `tests\PublicApiIntegrationTests\ApiTokenHelper.cs (line 38)`
- `tests\PublicApiIntegrationTests\ApiTokenHelper.cs (line 38)`
- `tests\PublicApiIntegrationTests\ApiTokenHelper.cs (line 42)`
- `tests\PublicApiIntegrationTests\ApiTokenHelper.cs (line 42)`
- `tests\PublicApiIntegrationTests\ApiTokenHelper.cs (line 42)`

</details>

<details id="Access_to_external_resources_via_HTTP_is_detected">
<summary><b>Access to external resources via HTTP is detected</b> — affected files</summary>

- `src\BlazorAdmin\CustomAuthStateProvider.cs (line 17)`
- `src\BlazorAdmin\CustomAuthStateProvider.cs (line 23)`
- `src\BlazorAdmin\Program.cs (line 22)`
- `src\BlazorAdmin\Services\CatalogLookupDataService.cs (line 21)`
- `src\BlazorAdmin\Services\CatalogLookupDataService.cs (line 25)`
- `src\BlazorAdmin\Services\HttpService.cs (line 12)`
- `src\BlazorAdmin\Services\HttpService.cs (line 17)`
- `tests\FunctionalTests\Web\Controllers\AccountControllerSignIn.cs (line 18)`
- `tests\FunctionalTests\Web\Controllers\CatalogControllerIndex.cs (line 14)`
- `tests\FunctionalTests\Web\Controllers\OrderControllerIndex.cs (line 19)`
- `tests\FunctionalTests\Web\Pages\HomePageOnGet.cs (line 13)`
- `tests\FunctionalTests\Web\Pages\Basket\BasketPageCheckout.cs (line 16)`
- `tests\FunctionalTests\Web\Pages\Basket\CheckoutTest.cs (line 16)`
- `tests\FunctionalTests\Web\Pages\Basket\IndexTest.cs (line 16)`
- `tests\PublicApiIntegrationTests\ProgramTest.cs (line 11)`
- `src\Web\Program.cs (line 101)`
- `src\Web\Program.cs (line 101)`
- `src\Web\HealthChecks\ApiHealthCheck.cs (line 23)`
- `src\Web\HealthChecks\HomePageHealthCheck.cs (line 24)`

</details>

<details id="Local_application_configuration_detected">
<summary><b>Local application configuration detected</b> — affected files</summary>

- `src\BlazorAdmin\wwwroot\appsettings.Development.json`
- `src\BlazorAdmin\wwwroot\appsettings.Docker.json`
- `src\BlazorAdmin\wwwroot\appsettings.json`
- `src\PublicApi\appsettings.Development.json`
- `src\PublicApi\appsettings.Docker.json`
- `src\PublicApi\appsettings.Docker.json`
- `src\PublicApi\appsettings.json`
- `src\PublicApi\appsettings.json`
- `src\PublicApi\appsettings.json`
- `tests\PublicApiIntegrationTests\appsettings.test.json`
- `src\Web\appsettings.Development.json`
- `src\Web\appsettings.Docker.json`
- `src\Web\appsettings.Docker.json`
- `src\Web\appsettings.json`
- `src\Web\appsettings.json`
- `src\Web\appsettings.json`

</details>

<details id="Hardcoded_URLs_detected">
<summary><b>Hardcoded URLs detected</b> — affected files</summary>

- `src\ApplicationCore\Services\UriComposer.cs (line 12)`
- `src\Infrastructure\Data\CatalogContextSeed.cs (line 86)`
- `src\Infrastructure\Data\CatalogContextSeed.cs (line 87)`
- `src\Infrastructure\Data\CatalogContextSeed.cs (line 88)`
- `src\Infrastructure\Data\CatalogContextSeed.cs (line 89)`
- `src\Infrastructure\Data\CatalogContextSeed.cs (line 90)`
- `src\Infrastructure\Data\CatalogContextSeed.cs (line 91)`
- `src\Infrastructure\Data\CatalogContextSeed.cs (line 92)`
- `src\Infrastructure\Data\CatalogContextSeed.cs (line 93)`
- `src\Infrastructure\Data\CatalogContextSeed.cs (line 94)`
- `src\Infrastructure\Data\CatalogContextSeed.cs (line 95)`
- `src\Infrastructure\Data\CatalogContextSeed.cs (line 96)`
- `src\Infrastructure\Data\CatalogContextSeed.cs (line 97)`
- `tests\UnitTests\Builders\OrderBuilder.cs (line 11)`

</details>

<details id="Connection_string_is_detected">
<summary><b>Connection string is detected</b> — affected files</summary>

- `src\PublicApi\appsettings.Docker.json`
- `src\PublicApi\appsettings.Docker.json`
- `src\PublicApi\appsettings.json`
- `src\PublicApi\appsettings.json`
- `src\Web\appsettings.Docker.json`
- `src\Web\appsettings.Docker.json`
- `src\Web\appsettings.json`
- `src\Web\appsettings.json`

</details>

<details id="Data_caching_is_detected">
<summary><b>Data caching is detected</b> — affected files</summary>

- `src\PublicApi\Program.cs (line 51)`
- `src\Web\Program.cs (line 65)`

</details>

<details id="Environment_variables_dependency_detected">
<summary><b>Environment variables dependency detected</b> — affected files</summary>

- `src\PublicApi\Properties\launchSettings.json`

</details>

<details id="Hardcoded_sensitive_data_detected">
<summary><b>Hardcoded sensitive data detected</b> — affected files</summary>

- `src\ApplicationCore\Constants\AuthorizationConstants.cs (line 10)`
- `tests\FunctionalTests\Web\Controllers\AccountControllerSignIn.cs (line 65)`
- `tests\FunctionalTests\Web\Controllers\AccountControllerSignIn.cs (line 85)`
- `tests\FunctionalTests\Web\Pages\Basket\CheckoutTest.cs (line 46)`
- `src\Infrastructure\Identity\Migrations\20201202111612_InitialIdentityModel.Designer.cs (line 187)`
- `src\Infrastructure\Identity\Migrations\AppIdentityDbContextModelSnapshot.cs (line 185)`
- `tests\PublicApiIntegrationTests\AuthEndpoints\AuthenticateEndpointTest.cs (line 16)`
- `tests\PublicApiIntegrationTests\AuthEndpoints\AuthenticateEndpointTest.cs (line 17)`
- `src\Web\Controllers\ManageController.cs (line 178)`
- `src\Web\Controllers\ManageController.cs (line 25)`
- `src\Web\Controllers\ManageController.cs (line 179)`
- `src\Web\Controllers\ManageController.cs (line 227)`
- `src\Web\ViewModels\Account\RegisterViewModel.cs (line 18)`
- `src\Web\ViewModels\Account\RegisterViewModel.cs (line 14)`
- `src\Web\ViewModels\Account\RegisterViewModel.cs (line 19)`
- `src\Web\ViewModels\Account\RegisterViewModel.cs (line 19)`
- `src\Web\ViewModels\Account\ResetPasswordViewModel.cs (line 16)`
- `src\Web\ViewModels\Account\ResetPasswordViewModel.cs (line 17)`
- `src\Web\ViewModels\Account\ResetPasswordViewModel.cs (line 17)`
- `src\Web\ViewModels\Manage\ChangePasswordViewModel.cs (line 18)`
- `src\Web\ViewModels\Manage\ChangePasswordViewModel.cs (line 8)`
- `src\Web\ViewModels\Manage\ChangePasswordViewModel.cs (line 14)`
- `src\Web\ViewModels\Manage\ChangePasswordViewModel.cs (line 19)`
- `src\Web\ViewModels\Manage\ChangePasswordViewModel.cs (line 19)`
- `src\Web\ViewModels\Manage\SetPasswordViewModel.cs (line 13)`
- `src\Web\ViewModels\Manage\SetPasswordViewModel.cs (line 9)`
- `src\Web\ViewModels\Manage\SetPasswordViewModel.cs (line 14)`
- `src\Web\ViewModels\Manage\SetPasswordViewModel.cs (line 14)`
- `src\Web\Views\Manage\ManageNavPages.cs (line 12)`
- `src\Web\Areas\Identity\Pages\Account\Register.cshtml.cs (line 74)`
- `src\Web\Areas\Identity\Pages\Account\Register.cshtml.cs (line 55)`
- `src\Web\Areas\Identity\Pages\Account\Register.cshtml.cs (line 51)`
- `src\Web\Areas\Identity\Pages\Account\Register.cshtml.cs (line 56)`
- `src\Web\Areas\Identity\Pages\Account\Register.cshtml.cs (line 56)`

</details>

<details id="Synchronous_API_usage_detected">
<summary><b>Synchronous API usage detected</b> — affected files</summary>

- `src\BlazorAdmin\Services\CatalogItemService.cs (line 50)`
- `src\BlazorAdmin\Services\CatalogItemService.cs (line 66)`
- `src\BlazorAdmin\Services\CatalogItemService.cs (line 85)`
- `src\BlazorAdmin\Services\CatalogItemService.cs (line 52)`
- `src\BlazorAdmin\Services\CatalogItemService.cs (line 68)`
- `src\BlazorAdmin\Services\CatalogItemService.cs (line 87)`
- `src\BlazorAdmin\Services\CatalogItemService.cs (line 51)`
- `src\BlazorAdmin\Services\CatalogItemService.cs (line 67)`
- `src\BlazorAdmin\Services\CatalogItemService.cs (line 86)`
- `tests\PublicApiIntegrationTests\CatalogItemEndpoints\CatalogItemListPagedEndpoint.cs (line 67)`

</details>

<details id="Static_content_detected">
<summary><b>Static content detected</b> — affected files</summary>

- `src\BlazorAdmin\BlazorAdmin.csproj`
- `src\Web\Web.csproj`

</details>

## DotNET Upgrade Issues [View Details](scenarios/dotnet-version-upgrade/assessment.md)

| Issue Category | Criticality | Story Points | Occurrences |
|----------------|-------------|--------------|-------------|
| Binary incompatible for selected .NET version | Mandatory | 1 | [24](#Binary_incompatible_for_selected_NET_version) |
| Project's target framework(s) needs to be changed | Mandatory | 1 | [10](#Project_s_target_framework_s_needs_to_be_changed) |
| NuGet package functionality is included with framework reference | Mandatory | 1 | [1](#NuGet_package_functionality_is_included_with_framework_reference) |
| NuGet package is incompatible | Mandatory | 1 | [1](#NuGet_package_is_incompatible) |
| IdentityModel & Claims-based Security | Mandatory | 4 | 0 |
| Behavioral change in selected .NET version | Potential | 1 | [51](#Behavioral_change_in_selected_NET_version) |
| NuGet package upgrade is recommended | Potential | 1 | [32](#NuGet_package_upgrade_is_recommended) |
| Source incompatible for selected .NET version | Potential | 1 | [21](#Source_incompatible_for_selected_NET_version) |
| NuGet package is deprecated | Optional | 1 | [10](#NuGet_package_is_deprecated) |
| NuGet package contains security vulnerability | Optional | 1 | [2](#NuGet_package_contains_security_vulnerability) |

### Issue Details

<details id="Binary_incompatible_for_selected_NET_version">
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

<details id="Project_s_target_framework_s_needs_to_be_changed">
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

<details id="NuGet_package_functionality_is_included_with_framework_reference">
<summary><b>NuGet package functionality is included with framework reference</b> — affected files</summary>

- `src\ApplicationCore\ApplicationCore.csproj`

</details>

<details id="NuGet_package_is_incompatible">
<summary><b>NuGet package is incompatible</b> — affected files</summary>

- `src\PublicApi\PublicApi.csproj`

</details>

<details id="Behavioral_change_in_selected_NET_version">
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

<details id="NuGet_package_upgrade_is_recommended">
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

<details id="Source_incompatible_for_selected_NET_version">
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

<details id="NuGet_package_is_deprecated">
<summary><b>NuGet package is deprecated</b> — affected files</summary>

- `tests\FunctionalTests\FunctionalTests.csproj`
- `src\Infrastructure\Infrastructure.csproj`
- `tests\IntegrationTests\IntegrationTests.csproj`
- `src\PublicApi\PublicApi.csproj`
- `tests\UnitTests\UnitTests.csproj`
- `src\Web\Web.csproj`

</details>

<details id="NuGet_package_contains_security_vulnerability">
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
