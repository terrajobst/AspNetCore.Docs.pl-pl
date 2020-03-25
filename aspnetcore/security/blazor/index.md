---
title: ASP.NET Core Blazor uwierzytelniania i autoryzacji
author: guardrex
description: Dowiedz się więcej na temat Blazor scenariuszy uwierzytelniania i autoryzacji.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/21/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: f7ffb4c3d5a05cb916b4f00cdfaf5898634a1a6d
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219028"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="a08e6-103">ASP.NET Core uwierzytelnianie i autoryzacja Blazor</span><span class="sxs-lookup"><span data-stu-id="a08e6-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="a08e6-104">[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="a08e6-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a08e6-105">ASP.NET Core obsługuje konfigurację i zarządzanie zabezpieczeniami w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="a08e6-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="a08e6-106">Scenariusze zabezpieczeń różnią się w zależności od aplikacji Blazor Server i Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="a08e6-106">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="a08e6-107">Ponieważ aplikacje serwera Blazor są uruchamiane na serwerze, sprawdzenia autoryzacji mogą określić:</span><span class="sxs-lookup"><span data-stu-id="a08e6-107">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="a08e6-108">Opcje interfejsu użytkownika wyświetlane użytkownikowi (na przykład, które pozycje menu są dostępne dla użytkownika).</span><span class="sxs-lookup"><span data-stu-id="a08e6-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="a08e6-109">Reguły dostępu do obszarów aplikacji i składników.</span><span class="sxs-lookup"><span data-stu-id="a08e6-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="a08e6-110">Blazor aplikacje webassembly są uruchamiane na kliencie.</span><span class="sxs-lookup"><span data-stu-id="a08e6-110">Blazor WebAssembly apps run on the client.</span></span> <span data-ttu-id="a08e6-111">Autoryzacja jest używana *tylko* do określenia opcji interfejsu użytkownika, które mają być wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="a08e6-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="a08e6-112">Ponieważ sprawdzenia po stronie klienta mogą być modyfikowane lub pomijane przez użytkownika, aplikacja Blazor webassembly nie może wymusić reguł dostępu autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="a08e6-112">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

<span data-ttu-id="a08e6-113">[Razor Pages Konwencji autoryzacji](xref:security/authorization/razor-pages-authorization) nie mają zastosowania do routingu składników Razor.</span><span class="sxs-lookup"><span data-stu-id="a08e6-113">[Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization) don't apply to routable Razor components.</span></span> <span data-ttu-id="a08e6-114">Jeśli składnik Razor nieobsługujący routingu jest [osadzony na stronie](xref:blazor/integrate-components#render-components-from-a-page-or-view), konwencje autoryzacji strony mają pośredni wpływ na składnik Razor wraz z resztą zawartości strony.</span><span class="sxs-lookup"><span data-stu-id="a08e6-114">If a non-routable Razor component is [embedded in a page](xref:blazor/integrate-components#render-components-from-a-page-or-view), the page's authorization conventions indirectly affect the Razor component along with the rest of the page's content.</span></span>

## <a name="authentication"></a><span data-ttu-id="a08e6-115">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="a08e6-115">Authentication</span></span>

<span data-ttu-id="a08e6-116">Blazor używa istniejących mechanizmów uwierzytelniania ASP.NET Core do ustanowienia tożsamości użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a08e6-116">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="a08e6-117">Dokładny mechanizm zależy od tego, w jaki sposób aplikacja Blazor jest hostowana, Blazor Server lub Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="a08e6-117">The exact mechanism depends on how the Blazor app is hosted, Blazor Server or Blazor WebAssembly.</span></span>

### <a name="blazor-server-authentication"></a><span data-ttu-id="a08e6-118">Uwierzytelnianie serwera Blazor</span><span class="sxs-lookup"><span data-stu-id="a08e6-118">Blazor Server authentication</span></span>

<span data-ttu-id="a08e6-119">Aplikacje serwera Blazor działają przez połączenie w czasie rzeczywistym, które zostało utworzone za pomocą usługi Sygnalizującer.</span><span class="sxs-lookup"><span data-stu-id="a08e6-119">Blazor Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="a08e6-120">[Uwierzytelnianie w aplikacjach opartych na sygnalizacji](xref:signalr/authn-and-authz) jest obsługiwane po nawiązaniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="a08e6-120">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="a08e6-121">Uwierzytelnianie może opierać się na pliku cookie lub innym tokenie okaziciela.</span><span class="sxs-lookup"><span data-stu-id="a08e6-121">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="a08e6-122">Szablon projektu serwera Blazor może skonfigurować uwierzytelnianie dla Ciebie podczas tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="a08e6-122">The Blazor Server project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a08e6-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a08e6-123">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a08e6-124">Postępuj zgodnie ze wskazówkami programu Visual Studio w artykule <xref:blazor/get-started>, aby utworzyć nowy projekt serwera Blazor z mechanizmem uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="a08e6-124">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="a08e6-125">Po wybraniu szablonu **aplikacji Blazor Server** w oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** wybierz pozycję **Zmień** w obszarze **uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="a08e6-125">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="a08e6-126">Zostanie otwarte okno dialogowe z zaoferowaniem tego samego zestawu mechanizmów uwierzytelniania dostępnych dla innych projektów ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a08e6-126">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="a08e6-127">**Bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="a08e6-127">**No Authentication**</span></span>
* <span data-ttu-id="a08e6-128">Konta **poszczególnych użytkowników** &ndash; kontach użytkowników mogą być przechowywane:</span><span class="sxs-lookup"><span data-stu-id="a08e6-128">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="a08e6-129">W aplikacji korzystającej z systemu [tożsamości](xref:security/authentication/identity) ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a08e6-129">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="a08e6-130">Z [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="a08e6-130">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="a08e6-131">**Konta służbowe**</span><span class="sxs-lookup"><span data-stu-id="a08e6-131">**Work or School Accounts**</span></span>
* <span data-ttu-id="a08e6-132">**Uwierzytelnianie systemu Windows**</span><span class="sxs-lookup"><span data-stu-id="a08e6-132">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a08e6-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a08e6-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a08e6-134">Postępuj zgodnie ze wskazówkami Visual Studio Code w artykule <xref:blazor/get-started>, aby utworzyć nowy projekt serwera Blazor z mechanizmem uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="a08e6-134">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="a08e6-135">W poniższej tabeli przedstawiono dozwolone wartości uwierzytelniania (`{AUTHENTICATION}`).</span><span class="sxs-lookup"><span data-stu-id="a08e6-135">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="a08e6-136">Mechanizm uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="a08e6-136">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="a08e6-137">wartość `{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="a08e6-137">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="a08e6-138">Bez uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="a08e6-138">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="a08e6-139">Szczegółowe</span><span class="sxs-lookup"><span data-stu-id="a08e6-139">Individual</span></span><br><span data-ttu-id="a08e6-140">Użytkownicy przechowywani w aplikacji z tożsamością ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a08e6-140">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="a08e6-141">Szczegółowe</span><span class="sxs-lookup"><span data-stu-id="a08e6-141">Individual</span></span><br><span data-ttu-id="a08e6-142">Użytkownicy przechowywani w [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="a08e6-142">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="a08e6-143">Konta służbowe</span><span class="sxs-lookup"><span data-stu-id="a08e6-143">Work or School Accounts</span></span><br><span data-ttu-id="a08e6-144">Uwierzytelnianie organizacyjne dla pojedynczej dzierżawy.</span><span class="sxs-lookup"><span data-stu-id="a08e6-144">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="a08e6-145">Konta służbowe</span><span class="sxs-lookup"><span data-stu-id="a08e6-145">Work or School Accounts</span></span><br><span data-ttu-id="a08e6-146">Uwierzytelnianie organizacyjne dla wielu dzierżawców.</span><span class="sxs-lookup"><span data-stu-id="a08e6-146">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="a08e6-147">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="a08e6-147">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="a08e6-148">Polecenie tworzy folder o nazwie przy użyciu wartości podanej dla symbolu zastępczego `{APP NAME}` i używa nazwy folderu jako nazwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a08e6-148">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="a08e6-149">Aby uzyskać więcej informacji, zobacz polecenie [dotnet New](/dotnet/core/tools/dotnet-new) w przewodniku .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a08e6-149">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

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

### <a name="opno-locblazor-webassembly-authentication"></a>Blazor<span data-ttu-id="a08e6-150"> uwierzytelnianie zestawu webassembly</span><span class="sxs-lookup"><span data-stu-id="a08e6-150"> WebAssembly authentication</span></span>

<span data-ttu-id="a08e6-151">W Blazor aplikacjach webassembly sprawdzanie uwierzytelniania można obejść, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a08e6-151">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="a08e6-152">Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="a08e6-152">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="a08e6-153">Dodaj odwołanie do pakietu dla elementu [Microsoft. AspNetCore. Components. Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) do pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a08e6-153">Add a package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>

<span data-ttu-id="a08e6-154">Implementacja niestandardowej usługi `AuthenticationStateProvider` dla aplikacji Blazor webassembly została omówiona w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="a08e6-154">Implementation of a custom `AuthenticationStateProvider` service for Blazor WebAssembly apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="a08e6-155">Usługa AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="a08e6-155">AuthenticationStateProvider service</span></span>

<span data-ttu-id="a08e6-156">aplikacje serwera Blazor zawierają wbudowaną usługę `AuthenticationStateProvider`, która pobiera dane stanu uwierzytelniania z `HttpContext.User`ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a08e6-156">Blazor Server apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="a08e6-157">Jest to sposób integracji stanu uwierzytelniania z istniejącymi mechanizmami uwierzytelniania po stronie serwera ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a08e6-157">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="a08e6-158">`AuthenticationStateProvider` to podstawowa usługa używana przez składnik `AuthorizeView` i składnik `CascadingAuthenticationState` do uzyskania stanu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="a08e6-158">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="a08e6-159">Nie korzystasz zwykle `AuthenticationStateProvider` bezpośrednio z programu.</span><span class="sxs-lookup"><span data-stu-id="a08e6-159">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="a08e6-160">Użyj [AuthorizeView składnika](#authorizeview-component) lub [zadania<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) podejścia opisane w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="a08e6-160">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="a08e6-161">Główną wadą do korzystania z `AuthenticationStateProvider` bezpośrednio jest to, że składnik nie jest automatycznie powiadamiany o zmianach danych stanu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="a08e6-161">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="a08e6-162">Usługa `AuthenticationStateProvider` może udostępniać dane <xref:System.Security.Claims.ClaimsPrincipal> bieżącego użytkownika, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="a08e6-162">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="LogUsername">Write user info to console</button>

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

<span data-ttu-id="a08e6-163">Jeśli `user.Identity.IsAuthenticated` jest `true` i ponieważ użytkownik jest <xref:System.Security.Claims.ClaimsPrincipal>, oświadczenia mogą być wyliczane i członkostwo w rolach oceniane.</span><span class="sxs-lookup"><span data-stu-id="a08e6-163">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="a08e6-164">Aby uzyskać więcej informacji na temat iniekcji zależności (DI) i usług, zobacz <xref:blazor/dependency-injection> i <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="a08e6-164">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="a08e6-165">Implementowanie niestandardowego AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="a08e6-165">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="a08e6-166">Jeśli tworzysz aplikację Blazor webassembly lub jeśli Specyfikacja aplikacji absolutnie wymaga dostawcy niestandardowego, zaimplementuj dostawcę i Przesłoń `GetAuthenticationStateAsync`:</span><span class="sxs-lookup"><span data-stu-id="a08e6-166">If you're building a Blazor WebAssembly app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

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

<span data-ttu-id="a08e6-167">W aplikacji Blazor webassembly usługa `CustomAuthStateProvider` jest zarejestrowana w `Main` *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a08e6-167">In a Blazor WebAssembly app, the `CustomAuthStateProvider` service is registered in `Main` of *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.Extensions.DependencyInjection;
using BlazorSample.Services;

public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddScoped<AuthenticationStateProvider, 
            CustomAuthStateProvider>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

<span data-ttu-id="a08e6-168">Korzystając z `CustomAuthStateProvider`, wszyscy użytkownicy są uwierzytelniani przy użyciu nazwy użytkownika `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="a08e6-168">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="a08e6-169">Uwidacznianie stanu uwierzytelniania jako parametru kaskadowego</span><span class="sxs-lookup"><span data-stu-id="a08e6-169">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="a08e6-170">Jeśli dane stanu uwierzytelniania są wymagane dla logiki proceduralnej, na przykład podczas wykonywania akcji wyzwalanej przez użytkownika, uzyskaj dane stanu uwierzytelniania przez zdefiniowanie parametru kaskadowego typu `Task<AuthenticationState>`:</span><span class="sxs-lookup"><span data-stu-id="a08e6-170">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```razor
@page "/"

<button @onclick="LogUsername">Log username</button>

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
> <span data-ttu-id="a08e6-171">W Blazor składnik aplikacji webassembly Dodaj `Microsoft.AspNetCore.Components.Authorization` przestrzeni nazw (`@using Microsoft.AspNetCore.Components.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="a08e6-171">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Components.Authorization` namespace (`@using Microsoft.AspNetCore.Components.Authorization`).</span></span>

<span data-ttu-id="a08e6-172">Jeśli `user.Identity.IsAuthenticated` jest `true`, oświadczenia można wyliczyć i członkostwo w rolach oceniane.</span><span class="sxs-lookup"><span data-stu-id="a08e6-172">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="a08e6-173">Skonfiguruj `Task<AuthenticationState>` kaskadowy parametr przy użyciu składników `AuthorizeRouteView` i `CascadingAuthenticationState` w pliku *App. Razor* :</span><span class="sxs-lookup"><span data-stu-id="a08e6-173">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components in the *App.razor* file:</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization

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

<span data-ttu-id="a08e6-174">Dodaj usługi dla opcji i autoryzacji do `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="a08e6-174">Add services for options and authorization to `Program.Main`:</span></span>

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

## <a name="authorization"></a><span data-ttu-id="a08e6-175">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="a08e6-175">Authorization</span></span>

<span data-ttu-id="a08e6-176">Po uwierzytelnieniu użytkownika są stosowane reguły *autoryzacji* umożliwiające kontrolę działania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a08e6-176">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="a08e6-177">Dostęp jest zazwyczaj udzielany lub odrzucany w zależności od tego, czy:</span><span class="sxs-lookup"><span data-stu-id="a08e6-177">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="a08e6-178">Użytkownik jest uwierzytelniany (zalogowany).</span><span class="sxs-lookup"><span data-stu-id="a08e6-178">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="a08e6-179">Użytkownik należy do *roli*.</span><span class="sxs-lookup"><span data-stu-id="a08e6-179">A user is in a *role*.</span></span>
* <span data-ttu-id="a08e6-180">Użytkownik ma *wierzytelność*.</span><span class="sxs-lookup"><span data-stu-id="a08e6-180">A user has a *claim*.</span></span>
* <span data-ttu-id="a08e6-181">*Zasady* są spełnione.</span><span class="sxs-lookup"><span data-stu-id="a08e6-181">A *policy* is satisfied.</span></span>

<span data-ttu-id="a08e6-182">Każda z tych koncepcji jest taka sama jak w aplikacji ASP.NET Core MVC lub Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a08e6-182">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="a08e6-183">Aby uzyskać więcej informacji na temat zabezpieczeń ASP.NET Core, zapoznaj się z artykułami w obszarze [ASP.NET Core zabezpieczenia i tożsamość](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="a08e6-183">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="a08e6-184">Składnik AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="a08e6-184">AuthorizeView component</span></span>

<span data-ttu-id="a08e6-185">Składnik `AuthorizeView` selektywnie wyświetla interfejs użytkownika w zależności od tego, czy użytkownik jest uprawniony do jego wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="a08e6-185">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="a08e6-186">Takie podejście jest przydatne, gdy wystarczy *wyświetlić* dane dla użytkownika i nie trzeba używać tożsamości użytkownika w logice proceduralnej.</span><span class="sxs-lookup"><span data-stu-id="a08e6-186">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="a08e6-187">Składnik uwidacznia zmienną `context` typu `AuthenticationState`, za pomocą której można uzyskać dostęp do informacji o zalogowanym użytkowniku:</span><span class="sxs-lookup"><span data-stu-id="a08e6-187">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="a08e6-188">Jeśli użytkownik nie jest uwierzytelniony, można również podać inną zawartość do wyświetlenia:</span><span class="sxs-lookup"><span data-stu-id="a08e6-188">You can also supply different content for display if the user isn't authenticated:</span></span>

```razor
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

<span data-ttu-id="a08e6-189">Zawartość tagów `<Authorized>` i `<NotAuthorized>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="a08e6-189">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="a08e6-190">Warunki autoryzacji, takie jak role lub zasady kontrolujące opcje interfejsu użytkownika lub dostęp, są omówione w sekcji [autoryzacja](#authorization) .</span><span class="sxs-lookup"><span data-stu-id="a08e6-190">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="a08e6-191">Jeśli warunki autoryzacji nie są określone, `AuthorizeView` używa domyślnych zasad i traktuje je:</span><span class="sxs-lookup"><span data-stu-id="a08e6-191">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="a08e6-192">Uwierzytelniony (zalogowany) Użytkownicy jako autoryzowany.</span><span class="sxs-lookup"><span data-stu-id="a08e6-192">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="a08e6-193">Nieuwierzytelnionych (wylogowanych) użytkowników jako nieautoryzowanych.</span><span class="sxs-lookup"><span data-stu-id="a08e6-193">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="a08e6-194">Autoryzacja oparta na rolach i zasadach</span><span class="sxs-lookup"><span data-stu-id="a08e6-194">Role-based and policy-based authorization</span></span>

<span data-ttu-id="a08e6-195">Składnik `AuthorizeView` obsługuje autoryzację opartą *na rolach* lub *zasadach* .</span><span class="sxs-lookup"><span data-stu-id="a08e6-195">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="a08e6-196">W przypadku autoryzacji opartej na rolach Użyj parametru `Roles`:</span><span class="sxs-lookup"><span data-stu-id="a08e6-196">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="a08e6-197">Aby uzyskać więcej informacji, zobacz <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="a08e6-197">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="a08e6-198">W przypadku autoryzacji opartej na zasadach Użyj parametru `Policy`:</span><span class="sxs-lookup"><span data-stu-id="a08e6-198">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="a08e6-199">Autoryzacja oparta na oświadczeniach jest specjalnym przypadkiem autoryzacji opartej na zasadach.</span><span class="sxs-lookup"><span data-stu-id="a08e6-199">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="a08e6-200">Na przykład można zdefiniować zasady, które wymagają, aby użytkownicy mieli pewne wnioski.</span><span class="sxs-lookup"><span data-stu-id="a08e6-200">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="a08e6-201">Aby uzyskać więcej informacji, zobacz <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="a08e6-201">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="a08e6-202">Te interfejsy API mogą być używane na serwerze Blazor lub Blazor aplikacji webassembly.</span><span class="sxs-lookup"><span data-stu-id="a08e6-202">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="a08e6-203">Jeśli nie `Roles` ani `Policy` jest określony, `AuthorizeView` używa domyślnych zasad.</span><span class="sxs-lookup"><span data-stu-id="a08e6-203">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="a08e6-204">Zawartość wyświetlana podczas uwierzytelniania asynchronicznego</span><span class="sxs-lookup"><span data-stu-id="a08e6-204">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="a08e6-205"> umożliwia określenie stanu uwierzytelniania *asynchronicznie*.</span><span class="sxs-lookup"><span data-stu-id="a08e6-205"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="a08e6-206">Głównym scenariuszem tego podejścia jest w Blazor aplikacje webassembly, które składają żądanie do zewnętrznego punktu końcowego w celu uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="a08e6-206">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="a08e6-207">Podczas uwierzytelniania w toku `AuthorizeView` domyślnie nie jest wyświetlana żadna zawartość.</span><span class="sxs-lookup"><span data-stu-id="a08e6-207">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="a08e6-208">Aby wyświetlić zawartość podczas uwierzytelniania, użyj elementu `<Authorizing>`:</span><span class="sxs-lookup"><span data-stu-id="a08e6-208">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

```razor
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

<span data-ttu-id="a08e6-209">Takie podejście nie ma zwykle zastosowania do aplikacji Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="a08e6-209">This approach isn't normally applicable to Blazor Server apps.</span></span> <span data-ttu-id="a08e6-210">aplikacje serwera Blazor znają stan uwierzytelniania zaraz po ustanowieniu stanu.</span><span class="sxs-lookup"><span data-stu-id="a08e6-210">Blazor Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="a08e6-211">zawartość `Authorizing` można podać w składniku `AuthorizeView` aplikacji Blazor Server, ale zawartość nigdy nie jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="a08e6-211">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="a08e6-212">[Autoryzuj] — atrybut</span><span class="sxs-lookup"><span data-stu-id="a08e6-212">[Authorize] attribute</span></span>

<span data-ttu-id="a08e6-213">Atrybut `[Authorize]` może być używany w składnikach Razor:</span><span class="sxs-lookup"><span data-stu-id="a08e6-213">The `[Authorize]` attribute can be used in Razor components:</span></span>

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> <span data-ttu-id="a08e6-214">W Blazor składnik aplikacji webassembly Dodaj `Microsoft.AspNetCore.Authorization` przestrzeni nazw (`@using Microsoft.AspNetCore.Authorization`) do przykładów w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="a08e6-214">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` namespace (`@using Microsoft.AspNetCore.Authorization`) to the examples in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a08e6-215">Za pośrednictwem routera Blazor należy używać tylko `[Authorize]` na składnikach `@page`.</span><span class="sxs-lookup"><span data-stu-id="a08e6-215">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="a08e6-216">Autoryzacja jest wykonywana tylko jako aspekt routingu, a *nie* dla składników podrzędnych renderowanych na stronie.</span><span class="sxs-lookup"><span data-stu-id="a08e6-216">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="a08e6-217">Aby autoryzować wyświetlanie określonych części na stronie, należy zamiast tego użyć `AuthorizeView`.</span><span class="sxs-lookup"><span data-stu-id="a08e6-217">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="a08e6-218">Atrybut `[Authorize]` obsługuje również autoryzację opartą na rolach lub zasadach.</span><span class="sxs-lookup"><span data-stu-id="a08e6-218">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="a08e6-219">W przypadku autoryzacji opartej na rolach Użyj parametru `Roles`:</span><span class="sxs-lookup"><span data-stu-id="a08e6-219">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="a08e6-220">W przypadku autoryzacji opartej na zasadach Użyj parametru `Policy`:</span><span class="sxs-lookup"><span data-stu-id="a08e6-220">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="a08e6-221">Jeśli nie `Roles` ani `Policy` jest określony, `[Authorize]` używa domyślnych zasad, które domyślnie są traktowane:</span><span class="sxs-lookup"><span data-stu-id="a08e6-221">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="a08e6-222">Uwierzytelniony (zalogowany) Użytkownicy jako autoryzowany.</span><span class="sxs-lookup"><span data-stu-id="a08e6-222">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="a08e6-223">Nieuwierzytelnionych (wylogowanych) użytkowników jako nieautoryzowanych.</span><span class="sxs-lookup"><span data-stu-id="a08e6-223">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="a08e6-224">Dostosowywanie nieautoryzowanej zawartości za pomocą składnika routera</span><span class="sxs-lookup"><span data-stu-id="a08e6-224">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="a08e6-225">Składnik `Router`, w połączeniu ze składnikiem `AuthorizeRouteView`, umożliwia aplikacji określenie zawartości niestandardowej, jeśli:</span><span class="sxs-lookup"><span data-stu-id="a08e6-225">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="a08e6-226">Nie znaleziono zawartości.</span><span class="sxs-lookup"><span data-stu-id="a08e6-226">Content isn't found.</span></span>
* <span data-ttu-id="a08e6-227">Do składnika zostanie zastosowany warunek `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="a08e6-227">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="a08e6-228">Atrybut `[Authorize]` jest omówiony w sekcji [`[Authorize]` atrybutu](#authorize-attribute) .</span><span class="sxs-lookup"><span data-stu-id="a08e6-228">The `[Authorize]` attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="a08e6-229">Uwierzytelnianie asynchroniczne jest w toku.</span><span class="sxs-lookup"><span data-stu-id="a08e6-229">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="a08e6-230">W domyślnym szablonie projektu Blazor Server plik *App. Razor* ilustruje sposób ustawiania zawartości niestandardowej:</span><span class="sxs-lookup"><span data-stu-id="a08e6-230">In the default Blazor Server project template, the *App.razor* file demonstrates how to set custom content:</span></span>

```razor
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

<span data-ttu-id="a08e6-231">Zawartość tagów `<NotFound>`, `<NotAuthorized>`i `<Authorizing>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="a08e6-231">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="a08e6-232">Jeśli nie określono elementu `<NotAuthorized>`, `AuthorizeRouteView` używa następującego komunikatu powrotu:</span><span class="sxs-lookup"><span data-stu-id="a08e6-232">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="a08e6-233">Powiadomienie o zmianach stanu uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="a08e6-233">Notification about authentication state changes</span></span>

<span data-ttu-id="a08e6-234">Jeśli aplikacja określi, że dane stanu uwierzytelniania zostały zmienione (na przykład ponieważ użytkownik wylogowany lub inny użytkownik zmienił swoje role), niestandardowa `AuthenticationStateProvider` może opcjonalnie wywołać metodę `NotifyAuthenticationStateChanged` na `AuthenticationStateProvider` klasie bazowej.</span><span class="sxs-lookup"><span data-stu-id="a08e6-234">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="a08e6-235">Spowoduje to powiadomienie klientów o danych stanu uwierzytelniania (na przykład `AuthorizeView`) w celu ponownego renderowania przy użyciu nowych danych.</span><span class="sxs-lookup"><span data-stu-id="a08e6-235">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="a08e6-236">Logika proceduralna</span><span class="sxs-lookup"><span data-stu-id="a08e6-236">Procedural logic</span></span>

<span data-ttu-id="a08e6-237">Jeśli aplikacja jest wymagana do sprawdzenia reguł autoryzacji jako części logiki proceduralnej, należy użyć kaskadowego parametru typu `Task<AuthenticationState>`, aby uzyskać <xref:System.Security.Claims.ClaimsPrincipal>użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a08e6-237">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="a08e6-238">`Task<AuthenticationState>` można łączyć z innymi usługami, takimi jak `IAuthorizationService`, w celu ocenienia zasad.</span><span class="sxs-lookup"><span data-stu-id="a08e6-238">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

```razor
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
> <span data-ttu-id="a08e6-239">W Blazor składnik aplikacji webassembly Dodaj przestrzenie nazw `Microsoft.AspNetCore.Authorization` i `Microsoft.AspNetCore.Components.Authorization`:</span><span class="sxs-lookup"><span data-stu-id="a08e6-239">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a><span data-ttu-id="a08e6-240">Autoryzacja w aplikacjach Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="a08e6-240">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="a08e6-241">W programie Blazor aplikacje webassembly można ominąć sprawdzanie autoryzacji, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a08e6-241">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="a08e6-242">Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="a08e6-242">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="a08e6-243">**Zawsze sprawdzaj autoryzację na serwerze w ramach dowolnych punktów końcowych interfejsu API, do których uzyskuje dostęp aplikacja po stronie klienta.**</span><span class="sxs-lookup"><span data-stu-id="a08e6-243">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

<span data-ttu-id="a08e6-244">Aby uzyskać więcej informacji, zobacz artykuły w obszarze <xref:security/blazor/webassembly/index>.</span><span class="sxs-lookup"><span data-stu-id="a08e6-244">For more information, see the articles under <xref:security/blazor/webassembly/index>.</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="a08e6-245">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="a08e6-245">Troubleshoot errors</span></span>

<span data-ttu-id="a08e6-246">Typowe błędy:</span><span class="sxs-lookup"><span data-stu-id="a08e6-246">Common errors:</span></span>

* <span data-ttu-id="a08e6-247">**Autoryzacja wymaga kaskadowego parametru typu Task\<AuthenticationState >. Rozważ użycie CascadingAuthenticationState, aby to zrobić.**</span><span class="sxs-lookup"><span data-stu-id="a08e6-247">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="a08e6-248">**Odebrano `null` wartość dla `authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="a08e6-248">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="a08e6-249">Prawdopodobnie projekt nie został utworzony przy użyciu szablonu serwera Blazor z włączonym uwierzytelnianiem.</span><span class="sxs-lookup"><span data-stu-id="a08e6-249">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="a08e6-250">Zawiń `<CascadingAuthenticationState>` wokół pewnej części drzewa interfejsu użytkownika, na przykład w *App. Razor* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="a08e6-250">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="a08e6-251">`CascadingAuthenticationState` dostarcza `Task<AuthenticationState>` kaskadowego parametru, który z kolei otrzymuje od podstawowej `AuthenticationStateProvider` DI usługi.</span><span class="sxs-lookup"><span data-stu-id="a08e6-251">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a08e6-252">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a08e6-252">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
* <span data-ttu-id="a08e6-253">[Firma Awesome Blazor:](https://github.com/AdrienTorris/awesome-blazor#authentication) przykładowe linki społeczności uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="a08e6-253">[Awesome Blazor: Authentication](https://github.com/AdrienTorris/awesome-blazor#authentication) community sample links</span></span>
