---
title: ASP.NET Core uwierzytelnianie i autoryzacja Blazor
author: guardrex
description: Dowiedz się więcej na temat scenariuszy uwierzytelniania Blazor i autoryzacji.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: security/blazor/index
ms.openlocfilehash: 85a6a32ea068e6cd00ebb71bdf7fe0bd06b77618
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391314"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="cd9e6-103">ASP.NET Core uwierzytelnianie i autoryzacja Blazor</span><span class="sxs-lookup"><span data-stu-id="cd9e6-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="cd9e6-104">[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="cd9e6-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="cd9e6-105">ASP.NET Core obsługuje konfigurację i zarządzanie zabezpieczeniami w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="cd9e6-106">Scenariusze zabezpieczeń różnią się w zależności od aplikacji Blazor Server i Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-106">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="cd9e6-107">Ponieważ aplikacje serwera Blazor są uruchamiane na serwerze, sprawdzenia autoryzacji mogą określić:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-107">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="cd9e6-108">Opcje interfejsu użytkownika wyświetlane użytkownikowi (na przykład, które pozycje menu są dostępne dla użytkownika).</span><span class="sxs-lookup"><span data-stu-id="cd9e6-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="cd9e6-109">Reguły dostępu do obszarów aplikacji i składników.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="cd9e6-110">Blazor aplikacje webassembly są uruchamiane na kliencie.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-110">Blazor WebAssembly apps run on the client.</span></span> <span data-ttu-id="cd9e6-111">Autoryzacja jest używana *tylko* do określenia opcji interfejsu użytkownika, które mają być wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="cd9e6-112">Ponieważ sprawdzenia po stronie klienta mogą być modyfikowane lub pomijane przez użytkownika, aplikacja Blazor webassembly nie może wymusić reguł dostępu autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-112">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="cd9e6-113">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="cd9e6-113">Authentication</span></span>

<span data-ttu-id="cd9e6-114">Blazor używa istniejących mechanizmów uwierzytelniania ASP.NET Core do ustanowienia tożsamości użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="cd9e6-115">Dokładny mechanizm zależy od tego, w jaki sposób aplikacja Blazor jest hostowana, Blazor Server lub Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-115">The exact mechanism depends on how the Blazor app is hosted, Blazor Server or Blazor WebAssembly.</span></span>

### <a name="blazor-server-authentication"></a><span data-ttu-id="cd9e6-116">Uwierzytelnianie serwera Blazor</span><span class="sxs-lookup"><span data-stu-id="cd9e6-116">Blazor Server authentication</span></span>

<span data-ttu-id="cd9e6-117">Aplikacje serwera Blazor działają przez połączenie w czasie rzeczywistym, które zostało utworzone za pomocą usługi Sygnalizującer.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-117">Blazor Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="cd9e6-118">[Uwierzytelnianie w aplikacjach opartych na sygnalizacji](xref:signalr/authn-and-authz) jest obsługiwane po nawiązaniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="cd9e6-119">Uwierzytelnianie może opierać się na pliku cookie lub innym tokenie okaziciela.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="cd9e6-120">Szablon projektu serwera Blazor może skonfigurować uwierzytelnianie dla Ciebie podczas tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-120">The Blazor Server project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd9e6-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd9e6-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cd9e6-122">Postępuj zgodnie ze wskazówkami programu Visual Studio w artykule <xref:blazor/get-started>, aby utworzyć nowy projekt serwera Blazor z mechanizmem uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="cd9e6-123">Po wybraniu szablonu **aplikacji Blazor Server** w oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** wybierz pozycję **Zmień** w obszarze **uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="cd9e6-124">Zostanie otwarte okno dialogowe z zaoferowaniem tego samego zestawu mechanizmów uwierzytelniania dostępnych dla innych projektów ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="cd9e6-125">**Bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="cd9e6-125">**No Authentication**</span></span>
* <span data-ttu-id="cd9e6-126">Konta użytkowników **poszczególnych użytkowników** &ndash; mogą być przechowywane:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="cd9e6-127">W aplikacji korzystającej z systemu [tożsamości](xref:security/authentication/identity) ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="cd9e6-128">Z [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="cd9e6-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="cd9e6-129">**Konta służbowe**</span><span class="sxs-lookup"><span data-stu-id="cd9e6-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="cd9e6-130">**Uwierzytelnianie systemu Windows**</span><span class="sxs-lookup"><span data-stu-id="cd9e6-130">**Windows Authentication**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cd9e6-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd9e6-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="cd9e6-132">Postępuj zgodnie ze wskazówkami Visual Studio Code w artykule <xref:blazor/get-started>, aby utworzyć nowy projekt serwera Blazor z mechanizmem uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="cd9e6-133">W poniższej tabeli przedstawiono dozwolone wartości uwierzytelniania (`{AUTHENTICATION}`).</span><span class="sxs-lookup"><span data-stu-id="cd9e6-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="cd9e6-134">Mechanizm uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="cd9e6-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="cd9e6-135">wartość `{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="cd9e6-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="cd9e6-136">Bez uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="cd9e6-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="cd9e6-137">Szczegółowe</span><span class="sxs-lookup"><span data-stu-id="cd9e6-137">Individual</span></span><br><span data-ttu-id="cd9e6-138">Użytkownicy przechowywani w aplikacji z tożsamością ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="cd9e6-139">Szczegółowe</span><span class="sxs-lookup"><span data-stu-id="cd9e6-139">Individual</span></span><br><span data-ttu-id="cd9e6-140">Użytkownicy przechowywani w [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="cd9e6-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="cd9e6-141">Konta służbowe</span><span class="sxs-lookup"><span data-stu-id="cd9e6-141">Work or School Accounts</span></span><br><span data-ttu-id="cd9e6-142">Uwierzytelnianie organizacyjne dla pojedynczej dzierżawy.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="cd9e6-143">Konta służbowe</span><span class="sxs-lookup"><span data-stu-id="cd9e6-143">Work or School Accounts</span></span><br><span data-ttu-id="cd9e6-144">Uwierzytelnianie organizacyjne dla wielu dzierżawców.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="cd9e6-145">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="cd9e6-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="cd9e6-146">Polecenie tworzy folder o nazwie z wartością podaną dla symbolu zastępczego `{APP NAME}` i używa nazwy folderu jako nazwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="cd9e6-147">Aby uzyskać więcej informacji, zobacz polecenie [dotnet New](/dotnet/core/tools/dotnet-new) w przewodniku .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.

| Authentication mechanism                                                                 | `{AUTHENTICATION}` value |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| No Authentication                                                                        | `None`                   |
| Individual<br>Users stored in the app with ASP.NET Core Identity.                        | `Individual`             |
| Individual<br>Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Work or School Accounts<br>Organizational authentication for a single tenant.            | `SingleOrg`              |
| Work or School Accounts<br>Organizational authentication for multiple tenants.           | `MultiOrg`               |
| Windows Authentication                                                                   | `Windows`                |

The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name. For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.

-->

---

### <a name="blazor-webassembly-authentication"></a><span data-ttu-id="cd9e6-148">Blazor uwierzytelniania webassembly</span><span class="sxs-lookup"><span data-stu-id="cd9e6-148">Blazor WebAssembly authentication</span></span>

<span data-ttu-id="cd9e6-149">W aplikacjach Blazor webassembly sprawdzanie uwierzytelniania można obejść, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-149">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="cd9e6-150">Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="cd9e6-151">Dodaj odwołanie do pakietu dla elementu [Microsoft. AspNetCore. Components. Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) do pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-151">Add a package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>

<span data-ttu-id="cd9e6-152">Implementacja niestandardowej usługi `AuthenticationStateProvider` dla aplikacji Blazor webassembly została omówiona w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-152">Implementation of a custom `AuthenticationStateProvider` service for Blazor WebAssembly apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="cd9e6-153">Usługa AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="cd9e6-153">AuthenticationStateProvider service</span></span>

<span data-ttu-id="cd9e6-154">Aplikacje serwera Blazor obejmują wbudowaną usługę `AuthenticationStateProvider`, która uzyskuje dane stanu uwierzytelniania ASP.NET Core `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-154">Blazor Server apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="cd9e6-155">Jest to sposób integracji stanu uwierzytelniania z istniejącymi mechanizmami uwierzytelniania po stronie serwera ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-155">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="cd9e6-156">`AuthenticationStateProvider` to podstawowa usługa używana przez składnik `AuthorizeView` i składnik `CascadingAuthenticationState` do uzyskania stanu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-156">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="cd9e6-157">Nie używasz zazwyczaj `AuthenticationStateProvider` bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-157">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="cd9e6-158">Użyj podejść [składnika AuthorizeView](#authorizeview-component) lub [zadania @ no__t-2](#expose-the-authentication-state-as-a-cascading-parameter) opisane w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-158">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="cd9e6-159">Główną wadą do korzystania z `AuthenticationStateProvider` bezpośrednio jest to, że składnik nie jest automatycznie powiadamiany o zmianach danych stanu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-159">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="cd9e6-160">Usługa `AuthenticationStateProvider` może udostępniać dane <xref:System.Security.Claims.ClaimsPrincipal> bieżącego użytkownika, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-160">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```cshtml
@page "/"
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="@LogUsername">Write user info to console</button>

@code {
    private async Task LogUsername()
    {
        var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

<span data-ttu-id="cd9e6-161">Jeśli `user.Identity.IsAuthenticated` jest `true` i ponieważ użytkownik jest <xref:System.Security.Claims.ClaimsPrincipal>, można wyliczyć oświadczenia i członkostwo w rolach.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-161">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="cd9e6-162">Aby uzyskać więcej informacji na temat iniekcji zależności (DI) i usług, zobacz <xref:blazor/dependency-injection> i <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-162">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="cd9e6-163">Implementowanie niestandardowego AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="cd9e6-163">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="cd9e6-164">Jeśli tworzysz aplikację webassembly Blazor lub jeśli Specyfikacja aplikacji absolutnie wymaga dostawcy niestandardowego, zaimplementuj dostawcę i Przesłoń `GetAuthenticationStateAsync`:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-164">If you're building a Blazor WebAssembly app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Authorization;

namespace BlazorSample.Services
{
    public class CustomAuthStateProvider : AuthenticationStateProvider
    {
        public override Task<AuthenticationState> GetAuthenticationStateAsync()
        {
            var identity = new ClaimsIdentity(new[]
            {
                new Claim(ClaimTypes.Name, "mrfibuli"),
            }, "Fake authentication type");

            var user = new ClaimsPrincipal(identity);

            return Task.FromResult(new AuthenticationState(user));
        }
    }
}
```

<span data-ttu-id="cd9e6-165">Usługa `CustomAuthStateProvider` jest zarejestrowana w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-165">The `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
// using Microsoft.AspNetCore.Components.Authorization;
// using BlazorSample.Services;

services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

<span data-ttu-id="cd9e6-166">Przy użyciu `CustomAuthStateProvider` wszyscy użytkownicy są uwierzytelniani przy użyciu nazwy użytkownika `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-166">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="cd9e6-167">Uwidacznianie stanu uwierzytelniania jako parametru kaskadowego</span><span class="sxs-lookup"><span data-stu-id="cd9e6-167">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="cd9e6-168">Jeśli dane stanu uwierzytelniania są wymagane dla logiki proceduralnej, na przykład podczas wykonywania akcji wyzwalanej przez użytkownika, uzyskaj dane stanu uwierzytelniania przez zdefiniowanie parametru kaskadowego typu `Task<AuthenticationState>`:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-168">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```cshtml
@page "/"

<button @onclick="@LogUsername">Log username</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task LogUsername()
    {
        var authState = await authenticationStateTask;
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="cd9e6-169">W składniku aplikacji webassembly Blazor Dodaj przestrzeń nazw `Microsoft.AspNetCore.Components.Authorization` (`@using Microsoft.AspNetCore.Components.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="cd9e6-169">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Components.Authorization` namespace (`@using Microsoft.AspNetCore.Components.Authorization`).</span></span>

<span data-ttu-id="cd9e6-170">Jeśli `user.Identity.IsAuthenticated` to `true`, można wyliczyć oświadczenia i członkostwo w rolach.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-170">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="cd9e6-171">Skonfiguruj parametr kaskadowy `Task<AuthenticationState>` przy użyciu składników `AuthorizeRouteView` i `CascadingAuthenticationState`:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-171">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components:</span></span>

```cshtml
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

## <a name="authorization"></a><span data-ttu-id="cd9e6-172">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="cd9e6-172">Authorization</span></span>

<span data-ttu-id="cd9e6-173">Po uwierzytelnieniu użytkownika są stosowane reguły *autoryzacji* umożliwiające kontrolę działania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-173">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="cd9e6-174">Dostęp jest zazwyczaj udzielany lub odrzucany w zależności od tego, czy:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-174">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="cd9e6-175">Użytkownik jest uwierzytelniany (zalogowany).</span><span class="sxs-lookup"><span data-stu-id="cd9e6-175">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="cd9e6-176">Użytkownik należy do *roli*.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-176">A user is in a *role*.</span></span>
* <span data-ttu-id="cd9e6-177">Użytkownik ma *wierzytelność*.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-177">A user has a *claim*.</span></span>
* <span data-ttu-id="cd9e6-178">*Zasady* są spełnione.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-178">A *policy* is satisfied.</span></span>

<span data-ttu-id="cd9e6-179">Każda z tych koncepcji jest taka sama jak w aplikacji ASP.NET Core MVC lub Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-179">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="cd9e6-180">Aby uzyskać więcej informacji na temat zabezpieczeń ASP.NET Core, zapoznaj się z artykułami w obszarze [ASP.NET Core zabezpieczenia i tożsamość](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="cd9e6-180">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="cd9e6-181">Składnik AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="cd9e6-181">AuthorizeView component</span></span>

<span data-ttu-id="cd9e6-182">Składnik `AuthorizeView` selektywnie wyświetla interfejs użytkownika w zależności od tego, czy użytkownik jest uprawniony do jego wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-182">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="cd9e6-183">Takie podejście jest przydatne, gdy wystarczy *wyświetlić* dane dla użytkownika i nie trzeba używać tożsamości użytkownika w logice proceduralnej.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-183">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="cd9e6-184">Składnik uwidacznia zmienną `context` typu `AuthenticationState`, za pomocą której można uzyskać dostęp do informacji o zalogowanym użytkowniku:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-184">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="cd9e6-185">Jeśli użytkownik nie jest uwierzytelniony, można również podać inną zawartość do wyświetlenia:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-185">You can also supply different content for display if the user isn't authenticated:</span></span>

```cshtml
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <NotAuthorized>
        <h1>Authentication Failure!</h1>
        <p>You're not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

<span data-ttu-id="cd9e6-186">Zawartość tagów `<Authorized>` i `<NotAuthorized>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-186">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="cd9e6-187">Warunki autoryzacji, takie jak role lub zasady kontrolujące opcje interfejsu użytkownika lub dostęp, są omówione w sekcji [autoryzacja](#authorization) .</span><span class="sxs-lookup"><span data-stu-id="cd9e6-187">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="cd9e6-188">Jeśli warunki autoryzacji nie są określone, `AuthorizeView` korzysta z zasad domyślnych i traktuje je:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-188">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="cd9e6-189">Uwierzytelniony (zalogowany) Użytkownicy jako autoryzowany.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-189">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="cd9e6-190">Nieuwierzytelnionych (wylogowanych) użytkowników jako nieautoryzowanych.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-190">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="cd9e6-191">Autoryzacja oparta na rolach i zasadach</span><span class="sxs-lookup"><span data-stu-id="cd9e6-191">Role-based and policy-based authorization</span></span>

<span data-ttu-id="cd9e6-192">Składnik `AuthorizeView` obsługuje autoryzację opartą *na rolach* lub *zasadach* .</span><span class="sxs-lookup"><span data-stu-id="cd9e6-192">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="cd9e6-193">W przypadku autoryzacji opartej na rolach Użyj parametru `Roles`:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-193">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="cd9e6-194">Aby uzyskać więcej informacji, zobacz <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-194">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="cd9e6-195">W przypadku autoryzacji opartej na zasadach należy użyć parametru `Policy`:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-195">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="cd9e6-196">Autoryzacja oparta na oświadczeniach jest specjalnym przypadkiem autoryzacji opartej na zasadach.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-196">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="cd9e6-197">Na przykład można zdefiniować zasady, które wymagają, aby użytkownicy mieli pewne wnioski.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-197">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="cd9e6-198">Aby uzyskać więcej informacji, zobacz <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-198">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="cd9e6-199">Te interfejsy API mogą być używane w aplikacjach Blazor Server lub Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-199">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="cd9e6-200">Jeśli nie określono żadnego `Roles` ani `Policy`, `AuthorizeView` używa zasad domyślnych.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-200">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="cd9e6-201">Zawartość wyświetlana podczas uwierzytelniania asynchronicznego</span><span class="sxs-lookup"><span data-stu-id="cd9e6-201">Content displayed during asynchronous authentication</span></span>

<span data-ttu-id="cd9e6-202">Blazor umożliwia określenie stanu uwierzytelniania w sposób *asynchroniczny*.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-202">Blazor allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="cd9e6-203">Głównym scenariuszem tego podejścia jest Blazor aplikacje webassembly, które składają żądanie do zewnętrznego punktu końcowego w celu uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-203">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="cd9e6-204">Gdy trwa uwierzytelnianie, `AuthorizeView` domyślnie nie wyświetla żadnej zawartości.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-204">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="cd9e6-205">Aby wyświetlić zawartość podczas uwierzytelniania, należy użyć elementu `<Authorizing>`:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-205">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

```cshtml
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <Authorizing>
        <h1>Authentication in progress</h1>
        <p>You can only see this content while authentication is in progress.</p>
    </Authorizing>
</AuthorizeView>
```

<span data-ttu-id="cd9e6-206">Takie podejście nie ma zwykle zastosowania do aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-206">This approach isn't normally applicable to Blazor Server apps.</span></span> <span data-ttu-id="cd9e6-207">Aplikacje serwera Blazor wiedzą o stanie uwierzytelniania zaraz po ustanowieniu stanu.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-207">Blazor Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="cd9e6-208">zawartość `Authorizing` można podać w składniku `AuthorizeView` aplikacji Blazor Server, ale zawartość nigdy nie jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-208">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="cd9e6-209">[Autoryzuj] — atrybut</span><span class="sxs-lookup"><span data-stu-id="cd9e6-209">[Authorize] attribute</span></span>

<span data-ttu-id="cd9e6-210">Atrybut `[Authorize]` może być używany w składnikach Razor:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-210">The `[Authorize]` attribute can be used in Razor components:</span></span>

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> <span data-ttu-id="cd9e6-211">W składniku aplikacji webassembly Blazor Dodaj przestrzeń nazw `Microsoft.AspNetCore.Authorization` (`@using Microsoft.AspNetCore.Authorization`) do przykładów w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-211">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` namespace (`@using Microsoft.AspNetCore.Authorization`) to the examples in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cd9e6-212">Za pośrednictwem routera Blazor należy używać tylko `[Authorize]` na składnikach `@page`.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-212">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="cd9e6-213">Autoryzacja jest wykonywana tylko jako aspekt routingu, a *nie* dla składników podrzędnych renderowanych na stronie.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-213">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="cd9e6-214">Aby autoryzować wyświetlanie określonych części na stronie, należy zamiast tego użyć `AuthorizeView`.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-214">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="cd9e6-215">Atrybut `[Authorize]` obsługuje również autoryzację opartą na rolach lub zasadach.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-215">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="cd9e6-216">W przypadku autoryzacji opartej na rolach Użyj parametru `Roles`:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-216">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="cd9e6-217">W przypadku autoryzacji opartej na zasadach należy użyć parametru `Policy`:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-217">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="cd9e6-218">Jeśli nie określono żadnego `Roles` ani `Policy`, `[Authorize]` używa domyślnych zasad, które domyślnie są traktowane:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-218">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="cd9e6-219">Uwierzytelniony (zalogowany) Użytkownicy jako autoryzowany.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-219">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="cd9e6-220">Nieuwierzytelnionych (wylogowanych) użytkowników jako nieautoryzowanych.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-220">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="cd9e6-221">Dostosowywanie nieautoryzowanej zawartości za pomocą składnika routera</span><span class="sxs-lookup"><span data-stu-id="cd9e6-221">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="cd9e6-222">Składnik `Router`, w połączeniu ze składnikiem `AuthorizeRouteView`, umożliwia aplikacji określenie zawartości niestandardowej, jeśli:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-222">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="cd9e6-223">Nie znaleziono zawartości.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-223">Content isn't found.</span></span>
* <span data-ttu-id="cd9e6-224">Użytkownik nie może wykonać warunku `[Authorize]` zastosowanego do składnika.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-224">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="cd9e6-225">Atrybut `[Authorize]` znajduje się w sekcji [atrybutu [autoryzuje]](#authorize-attribute) .</span><span class="sxs-lookup"><span data-stu-id="cd9e6-225">The `[Authorize]` attribute is covered in the [[Authorize] attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="cd9e6-226">Uwierzytelnianie asynchroniczne jest w toku.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-226">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="cd9e6-227">W domyślnym szablonie projektu serwera Blazor plik *App. Razor* ilustruje sposób ustawiania zawartości niestandardowej:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-227">In the default Blazor Server project template, the *App.razor* file demonstrates how to set custom content:</span></span>

```cshtml
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)">
            <NotAuthorized>
                <h1>Sorry</h1>
                <p>You're not authorized to reach this page.</p>
                <p>You may need to log in as a different user.</p>
            </NotAuthorized>
            <Authorizing>
                <h1>Authentication in progress</h1>
                <p>Only visible while authentication is in progress.</p>
            </Authorizing>
        </AuthorizeRouteView>
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <h1>Sorry</h1>
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

<span data-ttu-id="cd9e6-228">Zawartość tagów `<NotFound>`, `<NotAuthorized>` i `<Authorizing>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-228">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="cd9e6-229">Jeśli nie określono elementu `<NotAuthorized>`, `AuthorizeRouteView` używa następującego komunikatu powrotu:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-229">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="cd9e6-230">Powiadomienie o zmianach stanu uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="cd9e6-230">Notification about authentication state changes</span></span>

<span data-ttu-id="cd9e6-231">Jeśli aplikacja określi, że dane stanu uwierzytelniania zostały zmienione (na przykład, ponieważ użytkownik wylogowany lub inny użytkownik zmienił swoje role), niestandardowy `AuthenticationStateProvider` opcjonalnie może wywołać metodę `NotifyAuthenticationStateChanged` w klasie podstawowej `AuthenticationStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-231">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="cd9e6-232">Spowoduje to powiadomienie klientów o danych stanu uwierzytelniania (na przykład `AuthorizeView`) w celu ponownego renderowania przy użyciu nowych danych.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-232">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="cd9e6-233">Logika proceduralna</span><span class="sxs-lookup"><span data-stu-id="cd9e6-233">Procedural logic</span></span>

<span data-ttu-id="cd9e6-234">Jeśli aplikacja jest wymagana do sprawdzenia reguł autoryzacji jako części logiki proceduralnej, należy użyć kaskadowego parametru typu `Task<AuthenticationState>`, aby uzyskać @no__t użytkownika-1.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-234">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="cd9e6-235">`Task<AuthenticationState>` można łączyć z innymi usługami, takimi jak `IAuthorizationService`, w celu ocenienia zasad.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-235">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

```cshtml
@inject IAuthorizationService AuthorizationService

<button @onclick="@DoSomething">Do something important</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task DoSomething()
    {
        var user = (await authenticationStateTask).User;

        if (user.Identity.IsAuthenticated)
        {
            // Perform an action only available to authenticated (signed-in) users.
        }

        if (user.IsInRole("admin"))
        {
            // Perform an action only available to users in the 'admin' role.
        }

        if ((await AuthorizationService.AuthorizeAsync(user, "content-editor"))
            .Succeeded)
        {
            // Perform an action only available to users satisfying the 
            // 'content-editor' policy.
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="cd9e6-236">W składniku aplikacji webassembly Blazor Dodaj przestrzenie nazw `Microsoft.AspNetCore.Authorization` i `Microsoft.AspNetCore.Components.Authorization`:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-236">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```cshtml
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-blazor-webassembly-apps"></a><span data-ttu-id="cd9e6-237">Autoryzacja w aplikacjach webassembly Blazor</span><span class="sxs-lookup"><span data-stu-id="cd9e6-237">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="cd9e6-238">W aplikacjach webassembly Blazor można ominąć sprawdzanie autoryzacji, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-238">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="cd9e6-239">Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-239">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="cd9e6-240">**Zawsze sprawdzaj autoryzację na serwerze w ramach dowolnych punktów końcowych interfejsu API, do których uzyskuje dostęp aplikacja po stronie klienta.**</span><span class="sxs-lookup"><span data-stu-id="cd9e6-240">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="cd9e6-241">Rozwiązywanie problemów z błędami</span><span class="sxs-lookup"><span data-stu-id="cd9e6-241">Troubleshoot errors</span></span>

<span data-ttu-id="cd9e6-242">Typowe błędy:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-242">Common errors:</span></span>

* <span data-ttu-id="cd9e6-243">**Autoryzacja wymaga parametru kaskadowego typu Task @ no__t-1AuthenticationState >. Rozważ użycie CascadingAuthenticationState, aby to zrobić.**</span><span class="sxs-lookup"><span data-stu-id="cd9e6-243">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="cd9e6-244">**Odebrano wartość `null` dla `authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="cd9e6-244">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="cd9e6-245">Prawdopodobnie projekt nie został utworzony przy użyciu szablonu serwera Blazor z włączonym uwierzytelnianiem.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-245">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="cd9e6-246">Zawiń `<CascadingAuthenticationState>` wokół pewnej części drzewa interfejsu użytkownika, na przykład w *App. Razor* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cd9e6-246">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="cd9e6-247">@No__t-0 dostarcza parametr kaskadowy `Task<AuthenticationState>`, który z kolei otrzymuje od podstawowej `AuthenticationStateProvider` DI usługi.</span><span class="sxs-lookup"><span data-stu-id="cd9e6-247">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cd9e6-248">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cd9e6-248">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
