---
title: ASP.NET Core Blazor uwierzytelniania i autoryzacji
author: guardrex
description: Dowiedz się więcej na temat Blazor scenariuszy uwierzytelniania i autoryzacji.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: e9087c246f4805e5931180fa0869fc8a8d23a6c1
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885584"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="2746f-103">ASP.NET Core uwierzytelnianie i autoryzacja Blazor</span><span class="sxs-lookup"><span data-stu-id="2746f-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="2746f-104">[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="2746f-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="2746f-105">ASP.NET Core obsługuje konfigurację i zarządzanie zabezpieczeniami w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="2746f-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="2746f-106">Scenariusze zabezpieczeń różnią się w zależności od aplikacji Blazor Server i Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="2746f-106">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="2746f-107">Ponieważ aplikacje serwera Blazor są uruchamiane na serwerze, sprawdzenia autoryzacji mogą określić:</span><span class="sxs-lookup"><span data-stu-id="2746f-107">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="2746f-108">Opcje interfejsu użytkownika wyświetlane użytkownikowi (na przykład, które pozycje menu są dostępne dla użytkownika).</span><span class="sxs-lookup"><span data-stu-id="2746f-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="2746f-109">Reguły dostępu do obszarów aplikacji i składników.</span><span class="sxs-lookup"><span data-stu-id="2746f-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="2746f-110">Blazor aplikacje webassembly są uruchamiane na kliencie.</span><span class="sxs-lookup"><span data-stu-id="2746f-110">Blazor WebAssembly apps run on the client.</span></span> <span data-ttu-id="2746f-111">Autoryzacja jest używana *tylko* do określenia opcji interfejsu użytkownika, które mają być wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="2746f-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="2746f-112">Ponieważ sprawdzenia po stronie klienta mogą być modyfikowane lub pomijane przez użytkownika, aplikacja Blazor webassembly nie może wymusić reguł dostępu autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="2746f-112">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="2746f-113">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="2746f-113">Authentication</span></span>

<span data-ttu-id="2746f-114">Blazor używa istniejących mechanizmów uwierzytelniania ASP.NET Core do ustanowienia tożsamości użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2746f-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="2746f-115">Dokładny mechanizm zależy od tego, w jaki sposób aplikacja Blazor jest hostowana, Blazor Server lub Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="2746f-115">The exact mechanism depends on how the Blazor app is hosted, Blazor Server or Blazor WebAssembly.</span></span>

### <a name="blazor-server-authentication"></a><span data-ttu-id="2746f-116">Uwierzytelnianie serwera Blazor</span><span class="sxs-lookup"><span data-stu-id="2746f-116">Blazor Server authentication</span></span>

<span data-ttu-id="2746f-117">Aplikacje serwera Blazor działają przez połączenie w czasie rzeczywistym, które zostało utworzone za pomocą usługi Sygnalizującer.</span><span class="sxs-lookup"><span data-stu-id="2746f-117">Blazor Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="2746f-118">[Uwierzytelnianie w aplikacjach opartych na sygnalizacji](xref:signalr/authn-and-authz) jest obsługiwane po nawiązaniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="2746f-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="2746f-119">Uwierzytelnianie może opierać się na pliku cookie lub innym tokenie okaziciela.</span><span class="sxs-lookup"><span data-stu-id="2746f-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="2746f-120">Szablon projektu serwera Blazor może skonfigurować uwierzytelnianie dla Ciebie podczas tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="2746f-120">The Blazor Server project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2746f-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2746f-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2746f-122">Postępuj zgodnie ze wskazówkami programu Visual Studio w artykule <xref:blazor/get-started>, aby utworzyć nowy projekt serwera Blazor z mechanizmem uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="2746f-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="2746f-123">Po wybraniu szablonu **aplikacji Blazor Server** w oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** wybierz pozycję **Zmień** w obszarze **uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="2746f-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="2746f-124">Zostanie otwarte okno dialogowe z zaoferowaniem tego samego zestawu mechanizmów uwierzytelniania dostępnych dla innych projektów ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="2746f-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="2746f-125">**Bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="2746f-125">**No Authentication**</span></span>
* <span data-ttu-id="2746f-126">Konta **poszczególnych użytkowników** &ndash; kontach użytkowników mogą być przechowywane:</span><span class="sxs-lookup"><span data-stu-id="2746f-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="2746f-127">W aplikacji korzystającej z systemu [tożsamości](xref:security/authentication/identity) ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2746f-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="2746f-128">Z [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="2746f-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="2746f-129">**Konta służbowe**</span><span class="sxs-lookup"><span data-stu-id="2746f-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="2746f-130">**Uwierzytelnianie systemu Windows**</span><span class="sxs-lookup"><span data-stu-id="2746f-130">**Windows Authentication**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2746f-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2746f-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2746f-132">Postępuj zgodnie ze wskazówkami Visual Studio Code w artykule <xref:blazor/get-started>, aby utworzyć nowy projekt serwera Blazor z mechanizmem uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="2746f-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="2746f-133">W poniższej tabeli przedstawiono dozwolone wartości uwierzytelniania (`{AUTHENTICATION}`).</span><span class="sxs-lookup"><span data-stu-id="2746f-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="2746f-134">Mechanizm uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="2746f-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="2746f-135">wartość `{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="2746f-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="2746f-136">Bez uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="2746f-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="2746f-137">Szczegółowe</span><span class="sxs-lookup"><span data-stu-id="2746f-137">Individual</span></span><br><span data-ttu-id="2746f-138">Użytkownicy przechowywani w aplikacji z tożsamością ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2746f-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="2746f-139">Szczegółowe</span><span class="sxs-lookup"><span data-stu-id="2746f-139">Individual</span></span><br><span data-ttu-id="2746f-140">Użytkownicy przechowywani w [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="2746f-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="2746f-141">Konta służbowe</span><span class="sxs-lookup"><span data-stu-id="2746f-141">Work or School Accounts</span></span><br><span data-ttu-id="2746f-142">Uwierzytelnianie organizacyjne dla pojedynczej dzierżawy.</span><span class="sxs-lookup"><span data-stu-id="2746f-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="2746f-143">Konta służbowe</span><span class="sxs-lookup"><span data-stu-id="2746f-143">Work or School Accounts</span></span><br><span data-ttu-id="2746f-144">Uwierzytelnianie organizacyjne dla wielu dzierżawców.</span><span class="sxs-lookup"><span data-stu-id="2746f-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="2746f-145">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="2746f-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="2746f-146">Polecenie tworzy folder o nazwie przy użyciu wartości podanej dla symbolu zastępczego `{APP NAME}` i używa nazwy folderu jako nazwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2746f-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="2746f-147">Aby uzyskać więcej informacji, zobacz polecenie [dotnet New](/dotnet/core/tools/dotnet-new) w przewodniku .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2746f-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

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

### <a name="opno-locblazor-webassembly-authentication"></a>Blazor<span data-ttu-id="2746f-148"> uwierzytelnianie zestawu webassembly</span><span class="sxs-lookup"><span data-stu-id="2746f-148"> WebAssembly authentication</span></span>

<span data-ttu-id="2746f-149">W Blazor aplikacjach webassembly sprawdzanie uwierzytelniania można obejść, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2746f-149">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="2746f-150">Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="2746f-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="2746f-151">Dodaj odwołanie do pakietu dla elementu [Microsoft. AspNetCore. Components. Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) do pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2746f-151">Add a package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>

<span data-ttu-id="2746f-152">Implementacja niestandardowej usługi `AuthenticationStateProvider` dla aplikacji Blazor webassembly została omówiona w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="2746f-152">Implementation of a custom `AuthenticationStateProvider` service for Blazor WebAssembly apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="2746f-153">Usługa AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="2746f-153">AuthenticationStateProvider service</span></span>

<span data-ttu-id="2746f-154">aplikacje serwera Blazor zawierają wbudowaną usługę `AuthenticationStateProvider`, która pobiera dane stanu uwierzytelniania z `HttpContext.User`ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2746f-154">Blazor Server apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="2746f-155">Jest to sposób integracji stanu uwierzytelniania z istniejącymi mechanizmami uwierzytelniania po stronie serwera ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2746f-155">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="2746f-156">`AuthenticationStateProvider` to podstawowa usługa używana przez składnik `AuthorizeView` i składnik `CascadingAuthenticationState` do uzyskania stanu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="2746f-156">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="2746f-157">Nie korzystasz zwykle `AuthenticationStateProvider` bezpośrednio z programu.</span><span class="sxs-lookup"><span data-stu-id="2746f-157">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="2746f-158">Użyj [AuthorizeView składnika](#authorizeview-component) lub [zadania<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) podejścia opisane w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="2746f-158">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="2746f-159">Główną wadą do korzystania z `AuthenticationStateProvider` bezpośrednio jest to, że składnik nie jest automatycznie powiadamiany o zmianach danych stanu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="2746f-159">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="2746f-160">Usługa `AuthenticationStateProvider` może udostępniać dane <xref:System.Security.Claims.ClaimsPrincipal> bieżącego użytkownika, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="2746f-160">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```razor
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

<span data-ttu-id="2746f-161">Jeśli `user.Identity.IsAuthenticated` jest `true` i ponieważ użytkownik jest <xref:System.Security.Claims.ClaimsPrincipal>, oświadczenia mogą być wyliczane i członkostwo w rolach oceniane.</span><span class="sxs-lookup"><span data-stu-id="2746f-161">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="2746f-162">Aby uzyskać więcej informacji na temat iniekcji zależności (DI) i usług, zobacz <xref:blazor/dependency-injection> i <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="2746f-162">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="2746f-163">Implementowanie niestandardowego AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="2746f-163">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="2746f-164">Jeśli tworzysz aplikację Blazor webassembly lub jeśli Specyfikacja aplikacji absolutnie wymaga dostawcy niestandardowego, zaimplementuj dostawcę i Przesłoń `GetAuthenticationStateAsync`:</span><span class="sxs-lookup"><span data-stu-id="2746f-164">If you're building a Blazor WebAssembly app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

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

<span data-ttu-id="2746f-165">W aplikacji Blazor webassembly usługa `CustomAuthStateProvider` jest zarejestrowana w `Main` *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2746f-165">In a Blazor WebAssembly app, the `CustomAuthStateProvider` service is registered in `Main` of *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Blazor.Hosting;
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

<span data-ttu-id="2746f-166">Korzystając z `CustomAuthStateProvider`, wszyscy użytkownicy są uwierzytelniani przy użyciu nazwy użytkownika `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="2746f-166">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="2746f-167">Uwidacznianie stanu uwierzytelniania jako parametru kaskadowego</span><span class="sxs-lookup"><span data-stu-id="2746f-167">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="2746f-168">Jeśli dane stanu uwierzytelniania są wymagane dla logiki proceduralnej, na przykład podczas wykonywania akcji wyzwalanej przez użytkownika, uzyskaj dane stanu uwierzytelniania przez zdefiniowanie parametru kaskadowego typu `Task<AuthenticationState>`:</span><span class="sxs-lookup"><span data-stu-id="2746f-168">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```razor
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
> <span data-ttu-id="2746f-169">W Blazor składnik aplikacji webassembly Dodaj `Microsoft.AspNetCore.Components.Authorization` przestrzeni nazw (`@using Microsoft.AspNetCore.Components.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="2746f-169">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Components.Authorization` namespace (`@using Microsoft.AspNetCore.Components.Authorization`).</span></span>

<span data-ttu-id="2746f-170">Jeśli `user.Identity.IsAuthenticated` jest `true`, oświadczenia można wyliczyć i członkostwo w rolach oceniane.</span><span class="sxs-lookup"><span data-stu-id="2746f-170">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="2746f-171">Skonfiguruj `Task<AuthenticationState>` kaskadowy parametr przy użyciu składników `AuthorizeRouteView` i `CascadingAuthenticationState` w pliku *App. Razor* :</span><span class="sxs-lookup"><span data-stu-id="2746f-171">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components in the *App.razor* file:</span></span>

```razor
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

## <a name="authorization"></a><span data-ttu-id="2746f-172">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="2746f-172">Authorization</span></span>

<span data-ttu-id="2746f-173">Po uwierzytelnieniu użytkownika są stosowane reguły *autoryzacji* umożliwiające kontrolę działania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2746f-173">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="2746f-174">Dostęp jest zazwyczaj udzielany lub odrzucany w zależności od tego, czy:</span><span class="sxs-lookup"><span data-stu-id="2746f-174">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="2746f-175">Użytkownik jest uwierzytelniany (zalogowany).</span><span class="sxs-lookup"><span data-stu-id="2746f-175">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="2746f-176">Użytkownik należy do *roli*.</span><span class="sxs-lookup"><span data-stu-id="2746f-176">A user is in a *role*.</span></span>
* <span data-ttu-id="2746f-177">Użytkownik ma *wierzytelność*.</span><span class="sxs-lookup"><span data-stu-id="2746f-177">A user has a *claim*.</span></span>
* <span data-ttu-id="2746f-178">*Zasady* są spełnione.</span><span class="sxs-lookup"><span data-stu-id="2746f-178">A *policy* is satisfied.</span></span>

<span data-ttu-id="2746f-179">Każda z tych koncepcji jest taka sama jak w aplikacji ASP.NET Core MVC lub Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2746f-179">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="2746f-180">Aby uzyskać więcej informacji na temat zabezpieczeń ASP.NET Core, zapoznaj się z artykułami w obszarze [ASP.NET Core zabezpieczenia i tożsamość](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="2746f-180">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="2746f-181">Składnik AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="2746f-181">AuthorizeView component</span></span>

<span data-ttu-id="2746f-182">Składnik `AuthorizeView` selektywnie wyświetla interfejs użytkownika w zależności od tego, czy użytkownik jest uprawniony do jego wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="2746f-182">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="2746f-183">Takie podejście jest przydatne, gdy wystarczy *wyświetlić* dane dla użytkownika i nie trzeba używać tożsamości użytkownika w logice proceduralnej.</span><span class="sxs-lookup"><span data-stu-id="2746f-183">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="2746f-184">Składnik uwidacznia zmienną `context` typu `AuthenticationState`, za pomocą której można uzyskać dostęp do informacji o zalogowanym użytkowniku:</span><span class="sxs-lookup"><span data-stu-id="2746f-184">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="2746f-185">Jeśli użytkownik nie jest uwierzytelniony, można również podać inną zawartość do wyświetlenia:</span><span class="sxs-lookup"><span data-stu-id="2746f-185">You can also supply different content for display if the user isn't authenticated:</span></span>

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

<span data-ttu-id="2746f-186">Zawartość tagów `<Authorized>` i `<NotAuthorized>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="2746f-186">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="2746f-187">Warunki autoryzacji, takie jak role lub zasady kontrolujące opcje interfejsu użytkownika lub dostęp, są omówione w sekcji [autoryzacja](#authorization) .</span><span class="sxs-lookup"><span data-stu-id="2746f-187">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="2746f-188">Jeśli warunki autoryzacji nie są określone, `AuthorizeView` używa domyślnych zasad i traktuje je:</span><span class="sxs-lookup"><span data-stu-id="2746f-188">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="2746f-189">Uwierzytelniony (zalogowany) Użytkownicy jako autoryzowany.</span><span class="sxs-lookup"><span data-stu-id="2746f-189">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="2746f-190">Nieuwierzytelnionych (wylogowanych) użytkowników jako nieautoryzowanych.</span><span class="sxs-lookup"><span data-stu-id="2746f-190">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="2746f-191">Autoryzacja oparta na rolach i zasadach</span><span class="sxs-lookup"><span data-stu-id="2746f-191">Role-based and policy-based authorization</span></span>

<span data-ttu-id="2746f-192">Składnik `AuthorizeView` obsługuje autoryzację opartą *na rolach* lub *zasadach* .</span><span class="sxs-lookup"><span data-stu-id="2746f-192">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="2746f-193">W przypadku autoryzacji opartej na rolach Użyj parametru `Roles`:</span><span class="sxs-lookup"><span data-stu-id="2746f-193">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="2746f-194">Aby uzyskać więcej informacji, zobacz temat <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="2746f-194">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="2746f-195">W przypadku autoryzacji opartej na zasadach Użyj parametru `Policy`:</span><span class="sxs-lookup"><span data-stu-id="2746f-195">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="2746f-196">Autoryzacja oparta na oświadczeniach jest specjalnym przypadkiem autoryzacji opartej na zasadach.</span><span class="sxs-lookup"><span data-stu-id="2746f-196">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="2746f-197">Na przykład można zdefiniować zasady, które wymagają, aby użytkownicy mieli pewne wnioski.</span><span class="sxs-lookup"><span data-stu-id="2746f-197">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="2746f-198">Aby uzyskać więcej informacji, zobacz temat <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="2746f-198">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="2746f-199">Te interfejsy API mogą być używane na serwerze Blazor lub Blazor aplikacji webassembly.</span><span class="sxs-lookup"><span data-stu-id="2746f-199">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="2746f-200">Jeśli nie `Roles` ani `Policy` jest określony, `AuthorizeView` używa domyślnych zasad.</span><span class="sxs-lookup"><span data-stu-id="2746f-200">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="2746f-201">Zawartość wyświetlana podczas uwierzytelniania asynchronicznego</span><span class="sxs-lookup"><span data-stu-id="2746f-201">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="2746f-202"> umożliwia określenie stanu uwierzytelniania *asynchronicznie*.</span><span class="sxs-lookup"><span data-stu-id="2746f-202"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="2746f-203">Głównym scenariuszem tego podejścia jest w Blazor aplikacje webassembly, które składają żądanie do zewnętrznego punktu końcowego w celu uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="2746f-203">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="2746f-204">Podczas uwierzytelniania w toku `AuthorizeView` domyślnie nie jest wyświetlana żadna zawartość.</span><span class="sxs-lookup"><span data-stu-id="2746f-204">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="2746f-205">Aby wyświetlić zawartość podczas uwierzytelniania, użyj elementu `<Authorizing>`:</span><span class="sxs-lookup"><span data-stu-id="2746f-205">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

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

<span data-ttu-id="2746f-206">Takie podejście nie ma zwykle zastosowania do aplikacji Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="2746f-206">This approach isn't normally applicable to Blazor Server apps.</span></span> <span data-ttu-id="2746f-207">aplikacje serwera Blazor znają stan uwierzytelniania zaraz po ustanowieniu stanu.</span><span class="sxs-lookup"><span data-stu-id="2746f-207">Blazor Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="2746f-208">zawartość `Authorizing` można podać w składniku `AuthorizeView` aplikacji Blazor Server, ale zawartość nigdy nie jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="2746f-208">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="2746f-209">[Autoryzuj] — atrybut</span><span class="sxs-lookup"><span data-stu-id="2746f-209">[Authorize] attribute</span></span>

<span data-ttu-id="2746f-210">Atrybut `[Authorize]` może być używany w składnikach Razor:</span><span class="sxs-lookup"><span data-stu-id="2746f-210">The `[Authorize]` attribute can be used in Razor components:</span></span>

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> <span data-ttu-id="2746f-211">W Blazor składnik aplikacji webassembly Dodaj `Microsoft.AspNetCore.Authorization` przestrzeni nazw (`@using Microsoft.AspNetCore.Authorization`) do przykładów w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="2746f-211">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` namespace (`@using Microsoft.AspNetCore.Authorization`) to the examples in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2746f-212">Za pośrednictwem routera Blazor należy używać tylko `[Authorize]` na składnikach `@page`.</span><span class="sxs-lookup"><span data-stu-id="2746f-212">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="2746f-213">Autoryzacja jest wykonywana tylko jako aspekt routingu, a *nie* dla składników podrzędnych renderowanych na stronie.</span><span class="sxs-lookup"><span data-stu-id="2746f-213">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="2746f-214">Aby autoryzować wyświetlanie określonych części na stronie, należy zamiast tego użyć `AuthorizeView`.</span><span class="sxs-lookup"><span data-stu-id="2746f-214">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="2746f-215">Atrybut `[Authorize]` obsługuje również autoryzację opartą na rolach lub zasadach.</span><span class="sxs-lookup"><span data-stu-id="2746f-215">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="2746f-216">W przypadku autoryzacji opartej na rolach Użyj parametru `Roles`:</span><span class="sxs-lookup"><span data-stu-id="2746f-216">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="2746f-217">W przypadku autoryzacji opartej na zasadach Użyj parametru `Policy`:</span><span class="sxs-lookup"><span data-stu-id="2746f-217">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="2746f-218">Jeśli nie `Roles` ani `Policy` jest określony, `[Authorize]` używa domyślnych zasad, które domyślnie są traktowane:</span><span class="sxs-lookup"><span data-stu-id="2746f-218">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="2746f-219">Uwierzytelniony (zalogowany) Użytkownicy jako autoryzowany.</span><span class="sxs-lookup"><span data-stu-id="2746f-219">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="2746f-220">Nieuwierzytelnionych (wylogowanych) użytkowników jako nieautoryzowanych.</span><span class="sxs-lookup"><span data-stu-id="2746f-220">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="2746f-221">Dostosowywanie nieautoryzowanej zawartości za pomocą składnika routera</span><span class="sxs-lookup"><span data-stu-id="2746f-221">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="2746f-222">Składnik `Router`, w połączeniu ze składnikiem `AuthorizeRouteView`, umożliwia aplikacji określenie zawartości niestandardowej, jeśli:</span><span class="sxs-lookup"><span data-stu-id="2746f-222">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="2746f-223">Nie znaleziono zawartości.</span><span class="sxs-lookup"><span data-stu-id="2746f-223">Content isn't found.</span></span>
* <span data-ttu-id="2746f-224">Do składnika zostanie zastosowany warunek `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="2746f-224">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="2746f-225">Atrybut `[Authorize]` jest omówiony w sekcji [`[Authorize]` atrybutu](#authorize-attribute) .</span><span class="sxs-lookup"><span data-stu-id="2746f-225">The `[Authorize]` attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="2746f-226">Uwierzytelnianie asynchroniczne jest w toku.</span><span class="sxs-lookup"><span data-stu-id="2746f-226">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="2746f-227">W domyślnym szablonie projektu Blazor Server plik *App. Razor* ilustruje sposób ustawiania zawartości niestandardowej:</span><span class="sxs-lookup"><span data-stu-id="2746f-227">In the default Blazor Server project template, the *App.razor* file demonstrates how to set custom content:</span></span>

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

<span data-ttu-id="2746f-228">Zawartość tagów `<NotFound>`, `<NotAuthorized>`i `<Authorizing>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="2746f-228">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="2746f-229">Jeśli nie określono elementu `<NotAuthorized>`, `AuthorizeRouteView` używa następującego komunikatu powrotu:</span><span class="sxs-lookup"><span data-stu-id="2746f-229">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="2746f-230">Powiadomienie o zmianach stanu uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="2746f-230">Notification about authentication state changes</span></span>

<span data-ttu-id="2746f-231">Jeśli aplikacja określi, że dane stanu uwierzytelniania zostały zmienione (na przykład ponieważ użytkownik wylogowany lub inny użytkownik zmienił swoje role), niestandardowa `AuthenticationStateProvider` może opcjonalnie wywołać metodę `NotifyAuthenticationStateChanged` na `AuthenticationStateProvider` klasie bazowej.</span><span class="sxs-lookup"><span data-stu-id="2746f-231">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="2746f-232">Spowoduje to powiadomienie klientów o danych stanu uwierzytelniania (na przykład `AuthorizeView`) w celu ponownego renderowania przy użyciu nowych danych.</span><span class="sxs-lookup"><span data-stu-id="2746f-232">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="2746f-233">Logika proceduralna</span><span class="sxs-lookup"><span data-stu-id="2746f-233">Procedural logic</span></span>

<span data-ttu-id="2746f-234">Jeśli aplikacja jest wymagana do sprawdzenia reguł autoryzacji jako części logiki proceduralnej, należy użyć kaskadowego parametru typu `Task<AuthenticationState>`, aby uzyskać <xref:System.Security.Claims.ClaimsPrincipal>użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2746f-234">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="2746f-235">`Task<AuthenticationState>` można łączyć z innymi usługami, takimi jak `IAuthorizationService`, w celu ocenienia zasad.</span><span class="sxs-lookup"><span data-stu-id="2746f-235">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

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
> <span data-ttu-id="2746f-236">W Blazor składnik aplikacji webassembly Dodaj przestrzenie nazw `Microsoft.AspNetCore.Authorization` i `Microsoft.AspNetCore.Components.Authorization`:</span><span class="sxs-lookup"><span data-stu-id="2746f-236">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a><span data-ttu-id="2746f-237">Autoryzacja w aplikacjach Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="2746f-237">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="2746f-238">W programie Blazor aplikacje webassembly można ominąć sprawdzanie autoryzacji, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2746f-238">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="2746f-239">Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="2746f-239">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="2746f-240">**Zawsze sprawdzaj autoryzację na serwerze w ramach dowolnych punktów końcowych interfejsu API, do których uzyskuje dostęp aplikacja po stronie klienta.**</span><span class="sxs-lookup"><span data-stu-id="2746f-240">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="2746f-241">Rozwiązywanie problemów z błędami</span><span class="sxs-lookup"><span data-stu-id="2746f-241">Troubleshoot errors</span></span>

<span data-ttu-id="2746f-242">Typowe błędy:</span><span class="sxs-lookup"><span data-stu-id="2746f-242">Common errors:</span></span>

* <span data-ttu-id="2746f-243">**Autoryzacja wymaga kaskadowego parametru typu Task\<AuthenticationState >. Rozważ użycie CascadingAuthenticationState, aby to zrobić.**</span><span class="sxs-lookup"><span data-stu-id="2746f-243">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="2746f-244">**Odebrano `null` wartość dla `authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="2746f-244">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="2746f-245">Prawdopodobnie projekt nie został utworzony przy użyciu szablonu serwera Blazor z włączonym uwierzytelnianiem.</span><span class="sxs-lookup"><span data-stu-id="2746f-245">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="2746f-246">Zawiń `<CascadingAuthenticationState>` wokół pewnej części drzewa interfejsu użytkownika, na przykład w *App. Razor* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="2746f-246">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="2746f-247">`CascadingAuthenticationState` dostarcza `Task<AuthenticationState>` kaskadowego parametru, który z kolei otrzymuje od podstawowej `AuthenticationStateProvider` DI usługi.</span><span class="sxs-lookup"><span data-stu-id="2746f-247">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2746f-248">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2746f-248">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
