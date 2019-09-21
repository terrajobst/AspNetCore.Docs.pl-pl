---
title: ASP.NET Core uwierzytelnianie i autoryzacja Blazor
author: guardrex
description: Dowiedz się więcej na temat scenariuszy uwierzytelniania Blazor i autoryzacji.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: security/blazor/index
ms.openlocfilehash: 51fb1f9984878fceee0b207d02a02622c3ba191d
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168346"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="164c6-103">ASP.NET Core uwierzytelnianie i autoryzacja Blazor</span><span class="sxs-lookup"><span data-stu-id="164c6-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="164c6-104">[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="164c6-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="164c6-105">ASP.NET Core obsługuje konfigurację i zarządzanie zabezpieczeniami w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="164c6-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="164c6-106">Scenariusze zabezpieczeń różnią się w zależności od aplikacji Blazor Server i Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="164c6-106">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="164c6-107">Ponieważ aplikacje serwera Blazor są uruchamiane na serwerze, sprawdzenia autoryzacji mogą określić:</span><span class="sxs-lookup"><span data-stu-id="164c6-107">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="164c6-108">Opcje interfejsu użytkownika wyświetlane użytkownikowi (na przykład, które pozycje menu są dostępne dla użytkownika).</span><span class="sxs-lookup"><span data-stu-id="164c6-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="164c6-109">Reguły dostępu do obszarów aplikacji i składników.</span><span class="sxs-lookup"><span data-stu-id="164c6-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="164c6-110">Blazor aplikacje webassembly są uruchamiane na kliencie.</span><span class="sxs-lookup"><span data-stu-id="164c6-110">Blazor WebAssembly apps run on the client.</span></span> <span data-ttu-id="164c6-111">Autoryzacja jest używana *tylko* do określenia opcji interfejsu użytkownika, które mają być wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="164c6-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="164c6-112">Ponieważ sprawdzenia po stronie klienta mogą być modyfikowane lub pomijane przez użytkownika, aplikacja Blazor webassembly nie może wymusić reguł dostępu autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="164c6-112">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="164c6-113">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="164c6-113">Authentication</span></span>

<span data-ttu-id="164c6-114">Blazor używa istniejących mechanizmów uwierzytelniania ASP.NET Core do ustanowienia tożsamości użytkownika.</span><span class="sxs-lookup"><span data-stu-id="164c6-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="164c6-115">Dokładny mechanizm zależy od tego, w jaki sposób aplikacja Blazor jest hostowana, Blazor Server lub Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="164c6-115">The exact mechanism depends on how the Blazor app is hosted, Blazor Server or Blazor WebAssembly.</span></span>

### <a name="blazor-server-authentication"></a><span data-ttu-id="164c6-116">Uwierzytelnianie serwera Blazor</span><span class="sxs-lookup"><span data-stu-id="164c6-116">Blazor Server authentication</span></span>

<span data-ttu-id="164c6-117">Aplikacje serwera Blazor działają przez połączenie w czasie rzeczywistym, które zostało utworzone za pomocą usługi Sygnalizującer.</span><span class="sxs-lookup"><span data-stu-id="164c6-117">Blazor Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="164c6-118">[Uwierzytelnianie w aplikacjach opartych na sygnalizacji](xref:signalr/authn-and-authz) jest obsługiwane po nawiązaniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="164c6-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="164c6-119">Uwierzytelnianie może opierać się na pliku cookie lub innym tokenie okaziciela.</span><span class="sxs-lookup"><span data-stu-id="164c6-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="164c6-120">Szablon projektu serwera Blazor może skonfigurować uwierzytelnianie dla Ciebie podczas tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="164c6-120">The Blazor Server project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="164c6-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="164c6-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="164c6-122">Postępuj zgodnie ze wskazówkami programu <xref:blazor/get-started> Visual Studio w artykule, aby utworzyć nowy projekt serwera Blazor z mechanizmem uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="164c6-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="164c6-123">Po wybraniu szablonu **aplikacji Blazor Server** w oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** wybierz pozycję **Zmień** w obszarze **uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="164c6-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="164c6-124">Zostanie otwarte okno dialogowe z zaoferowaniem tego samego zestawu mechanizmów uwierzytelniania dostępnych dla innych projektów ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="164c6-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="164c6-125">**Bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="164c6-125">**No Authentication**</span></span>
* <span data-ttu-id="164c6-126">**Indywidualne konta użytkowników** &ndash; Konta użytkowników mogą być przechowywane:</span><span class="sxs-lookup"><span data-stu-id="164c6-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="164c6-127">W aplikacji korzystającej z systemu [tożsamości](xref:security/authentication/identity) ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="164c6-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="164c6-128">Z [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="164c6-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="164c6-129">**Konta służbowe**</span><span class="sxs-lookup"><span data-stu-id="164c6-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="164c6-130">**Uwierzytelnianie systemu Windows**</span><span class="sxs-lookup"><span data-stu-id="164c6-130">**Windows Authentication**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="164c6-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="164c6-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="164c6-132">Postępuj zgodnie ze wskazówkami <xref:blazor/get-started> Visual Studio Code w artykule, aby utworzyć nowy projekt serwera Blazor z mechanizmem uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="164c6-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="164c6-133">W poniższej tabeli przedstawiono`{AUTHENTICATION}`dozwolone wartości uwierzytelniania ().</span><span class="sxs-lookup"><span data-stu-id="164c6-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="164c6-134">Mechanizm uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="164c6-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="164c6-135">`{AUTHENTICATION}`wartościami</span><span class="sxs-lookup"><span data-stu-id="164c6-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="164c6-136">Bez uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="164c6-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="164c6-137">Szczegółowe</span><span class="sxs-lookup"><span data-stu-id="164c6-137">Individual</span></span><br><span data-ttu-id="164c6-138">Użytkownicy przechowywani w aplikacji z tożsamością ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="164c6-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="164c6-139">Szczegółowe</span><span class="sxs-lookup"><span data-stu-id="164c6-139">Individual</span></span><br><span data-ttu-id="164c6-140">Użytkownicy przechowywani w [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="164c6-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="164c6-141">Konta służbowe</span><span class="sxs-lookup"><span data-stu-id="164c6-141">Work or School Accounts</span></span><br><span data-ttu-id="164c6-142">Uwierzytelnianie organizacyjne dla pojedynczej dzierżawy.</span><span class="sxs-lookup"><span data-stu-id="164c6-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="164c6-143">Konta służbowe</span><span class="sxs-lookup"><span data-stu-id="164c6-143">Work or School Accounts</span></span><br><span data-ttu-id="164c6-144">Uwierzytelnianie organizacyjne dla wielu dzierżawców.</span><span class="sxs-lookup"><span data-stu-id="164c6-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="164c6-145">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="164c6-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="164c6-146">Polecenie tworzy folder o nazwie przy użyciu wartości podanej dla `{APP NAME}` symbolu zastępczego i używa nazwy folderu jako nazwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="164c6-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="164c6-147">Aby uzyskać więcej informacji, zobacz polecenie [dotnet New](/dotnet/core/tools/dotnet-new) w przewodniku .NET Core.</span><span class="sxs-lookup"><span data-stu-id="164c6-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

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

### <a name="blazor-webassembly-authentication"></a><span data-ttu-id="164c6-148">Blazor uwierzytelniania webassembly</span><span class="sxs-lookup"><span data-stu-id="164c6-148">Blazor WebAssembly authentication</span></span>

<span data-ttu-id="164c6-149">W aplikacjach Blazor webassembly sprawdzanie uwierzytelniania można obejść, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="164c6-149">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="164c6-150">Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="164c6-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="164c6-151">Implementacja niestandardowej `AuthenticationStateProvider` usługi dla aplikacji webassembly Blazor została omówiona w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="164c6-151">Implementation of a custom `AuthenticationStateProvider` service for Blazor WebAssembly apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="164c6-152">Usługa AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="164c6-152">AuthenticationStateProvider service</span></span>

<span data-ttu-id="164c6-153">Aplikacje serwera Blazor obejmują wbudowaną `AuthenticationStateProvider` usługę, która pobiera dane stanu uwierzytelniania z `HttpContext.User`ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="164c6-153">Blazor Server apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="164c6-154">Jest to sposób integracji stanu uwierzytelniania z istniejącymi mechanizmami uwierzytelniania po stronie serwera ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="164c6-154">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="164c6-155">`AuthenticationStateProvider`to podstawowa usługa używana przez `AuthorizeView` składnik i `CascadingAuthenticationState` składnik do uzyskiwania stanu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="164c6-155">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="164c6-156">Zwykle nie są używane `AuthenticationStateProvider` bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="164c6-156">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="164c6-157">Użyj [składnika AuthorizeView](#authorizeview-component) lub podejścia [do<AuthenticationState> zadań](#expose-the-authentication-state-as-a-cascading-parameter) opisanych w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="164c6-157">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="164c6-158">Główną wadą zwrotu z używania `AuthenticationStateProvider` bezpośrednio jest to, że składnik nie jest automatycznie powiadamiany o zmianach danych stanu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="164c6-158">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="164c6-159">Usługa może udostępniać <xref:System.Security.Claims.ClaimsPrincipal> dane bieżącego użytkownika, jak pokazano w następującym przykładzie: `AuthenticationStateProvider`</span><span class="sxs-lookup"><span data-stu-id="164c6-159">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```cshtml
@page "/"
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

<span data-ttu-id="164c6-160">Jeśli `user.Identity.IsAuthenticated` jest `true` i<xref:System.Security.Claims.ClaimsPrincipal>ponieważ użytkownik jest, można wyliczyć oświadczenia i członkostwo w rolach.</span><span class="sxs-lookup"><span data-stu-id="164c6-160">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="164c6-161">Aby uzyskać więcej informacji na temat iniekcji zależności (di) i <xref:blazor/dependency-injection> usług <xref:fundamentals/dependency-injection>, zobacz i.</span><span class="sxs-lookup"><span data-stu-id="164c6-161">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="164c6-162">Implementowanie niestandardowego AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="164c6-162">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="164c6-163">Jeśli tworzysz aplikację webassembly Blazor lub jeśli Specyfikacja aplikacji absolutnie wymaga dostawcy niestandardowego, zaimplementuj dostawcę i Przesłoń `GetAuthenticationStateAsync`:</span><span class="sxs-lookup"><span data-stu-id="164c6-163">If you're building a Blazor WebAssembly app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

```csharp
class CustomAuthStateProvider : AuthenticationStateProvider
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
```

<span data-ttu-id="164c6-164">Usługa jest zarejestrowana w `Startup.ConfigureServices`: `CustomAuthStateProvider`</span><span class="sxs-lookup"><span data-stu-id="164c6-164">The `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
}
```

<span data-ttu-id="164c6-165">Korzystając z `CustomAuthStateProvider`programu, wszyscy użytkownicy są uwierzytelniani przy `mrfibuli`użyciu nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="164c6-165">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="164c6-166">Uwidacznianie stanu uwierzytelniania jako parametru kaskadowego</span><span class="sxs-lookup"><span data-stu-id="164c6-166">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="164c6-167">Jeśli dane stanu uwierzytelniania są wymagane dla logiki proceduralnej, na przykład podczas wykonywania akcji wyzwalanej przez użytkownika, uzyskaj dane stanu uwierzytelniania przez zdefiniowanie parametru kaskadowego typu `Task<AuthenticationState>`:</span><span class="sxs-lookup"><span data-stu-id="164c6-167">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

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

<span data-ttu-id="164c6-168">Jeśli `user.Identity.IsAuthenticated` jest`true`, oświadczenia mogą być wyliczane i członkostwo w rolach oceniane.</span><span class="sxs-lookup"><span data-stu-id="164c6-168">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="164c6-169">Skonfiguruj parametr `AuthorizeRouteView`kaskadowy przy użyciu składników i `CascadingAuthenticationState`: `Task<AuthenticationState>`</span><span class="sxs-lookup"><span data-stu-id="164c6-169">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components:</span></span>

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

## <a name="authorization"></a><span data-ttu-id="164c6-170">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="164c6-170">Authorization</span></span>

<span data-ttu-id="164c6-171">Po uwierzytelnieniu użytkownika są stosowane reguły *autoryzacji* umożliwiające kontrolę działania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="164c6-171">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="164c6-172">Dostęp jest zazwyczaj udzielany lub odrzucany w zależności od tego, czy:</span><span class="sxs-lookup"><span data-stu-id="164c6-172">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="164c6-173">Użytkownik jest uwierzytelniany (zalogowany).</span><span class="sxs-lookup"><span data-stu-id="164c6-173">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="164c6-174">Użytkownik należy do *roli*.</span><span class="sxs-lookup"><span data-stu-id="164c6-174">A user is in a *role*.</span></span>
* <span data-ttu-id="164c6-175">Użytkownik ma *wierzytelność*.</span><span class="sxs-lookup"><span data-stu-id="164c6-175">A user has a *claim*.</span></span>
* <span data-ttu-id="164c6-176">*Zasady* są spełnione.</span><span class="sxs-lookup"><span data-stu-id="164c6-176">A *policy* is satisfied.</span></span>

<span data-ttu-id="164c6-177">Każda z tych koncepcji jest taka sama jak w aplikacji ASP.NET Core MVC lub Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="164c6-177">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="164c6-178">Aby uzyskać więcej informacji na temat zabezpieczeń ASP.NET Core, zapoznaj się z artykułami w obszarze [ASP.NET Core zabezpieczenia i tożsamość](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="164c6-178">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="164c6-179">Składnik AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="164c6-179">AuthorizeView component</span></span>

<span data-ttu-id="164c6-180">`AuthorizeView` Składnik selektywnie wyświetla interfejs użytkownika w zależności od tego, czy użytkownik jest uprawniony do jego wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="164c6-180">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="164c6-181">Takie podejście jest przydatne, gdy wystarczy *wyświetlić* dane dla użytkownika i nie trzeba używać tożsamości użytkownika w logice proceduralnej.</span><span class="sxs-lookup"><span data-stu-id="164c6-181">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="164c6-182">Składnik uwidacznia `context` zmienną typu `AuthenticationState`, za pomocą której można uzyskać dostęp do informacji o zalogowanym użytkowniku:</span><span class="sxs-lookup"><span data-stu-id="164c6-182">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="164c6-183">Jeśli użytkownik nie jest uwierzytelniony, można również podać inną zawartość do wyświetlenia:</span><span class="sxs-lookup"><span data-stu-id="164c6-183">You can also supply different content for display if the user isn't authenticated:</span></span>

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

<span data-ttu-id="164c6-184">Zawartość `<Authorized>` i`<NotAuthorized>` Tagi mogą zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="164c6-184">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="164c6-185">Warunki autoryzacji, takie jak role lub zasady kontrolujące opcje interfejsu użytkownika lub dostęp, są omówione w sekcji [autoryzacja](#authorization) .</span><span class="sxs-lookup"><span data-stu-id="164c6-185">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="164c6-186">Jeśli warunki autoryzacji nie są określone `AuthorizeView` , program używa domyślnych zasad i traktuje je:</span><span class="sxs-lookup"><span data-stu-id="164c6-186">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="164c6-187">Uwierzytelniony (zalogowany) Użytkownicy jako autoryzowany.</span><span class="sxs-lookup"><span data-stu-id="164c6-187">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="164c6-188">Nieuwierzytelnionych (wylogowanych) użytkowników jako nieautoryzowanych.</span><span class="sxs-lookup"><span data-stu-id="164c6-188">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="164c6-189">Autoryzacja oparta na rolach i zasadach</span><span class="sxs-lookup"><span data-stu-id="164c6-189">Role-based and policy-based authorization</span></span>

<span data-ttu-id="164c6-190">Składnik obsługuje autoryzację opartą *na rolach* lub *zasadach.* `AuthorizeView`</span><span class="sxs-lookup"><span data-stu-id="164c6-190">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="164c6-191">W przypadku autoryzacji opartej na rolach Użyj `Roles` parametru:</span><span class="sxs-lookup"><span data-stu-id="164c6-191">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="164c6-192">Aby uzyskać więcej informacji, zobacz <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="164c6-192">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="164c6-193">W przypadku autoryzacji opartej na zasadach należy `Policy` użyć parametru:</span><span class="sxs-lookup"><span data-stu-id="164c6-193">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="164c6-194">Autoryzacja oparta na oświadczeniach jest specjalnym przypadkiem autoryzacji opartej na zasadach.</span><span class="sxs-lookup"><span data-stu-id="164c6-194">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="164c6-195">Na przykład można zdefiniować zasady, które wymagają, aby użytkownicy mieli pewne wnioski.</span><span class="sxs-lookup"><span data-stu-id="164c6-195">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="164c6-196">Aby uzyskać więcej informacji, zobacz <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="164c6-196">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="164c6-197">Te interfejsy API mogą być używane w aplikacjach Blazor Server lub Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="164c6-197">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="164c6-198">Jeśli ani nie `AuthorizeView` zostanie określony, program używa domyślnych zasad. `Policy` `Roles`</span><span class="sxs-lookup"><span data-stu-id="164c6-198">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="164c6-199">Zawartość wyświetlana podczas uwierzytelniania asynchronicznego</span><span class="sxs-lookup"><span data-stu-id="164c6-199">Content displayed during asynchronous authentication</span></span>

<span data-ttu-id="164c6-200">Blazor umożliwia określenie stanu uwierzytelniania w sposób *asynchroniczny*.</span><span class="sxs-lookup"><span data-stu-id="164c6-200">Blazor allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="164c6-201">Głównym scenariuszem tego podejścia jest Blazor aplikacje webassembly, które składają żądanie do zewnętrznego punktu końcowego w celu uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="164c6-201">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="164c6-202">Gdy trwa uwierzytelnianie, `AuthorizeView` domyślnie nie jest wyświetlana żadna zawartość.</span><span class="sxs-lookup"><span data-stu-id="164c6-202">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="164c6-203">Aby wyświetlić zawartość podczas uwierzytelniania, należy użyć `<Authorizing>` elementu:</span><span class="sxs-lookup"><span data-stu-id="164c6-203">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

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

<span data-ttu-id="164c6-204">Takie podejście nie ma zwykle zastosowania do aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="164c6-204">This approach isn't normally applicable to Blazor Server apps.</span></span> <span data-ttu-id="164c6-205">Aplikacje serwera Blazor wiedzą o stanie uwierzytelniania zaraz po ustanowieniu stanu.</span><span class="sxs-lookup"><span data-stu-id="164c6-205">Blazor Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="164c6-206">`Authorizing`zawartość można podać w `AuthorizeView` składniku aplikacji serwera Blazor, ale zawartość nigdy nie jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="164c6-206">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="164c6-207">[Autoryzuj] — atrybut</span><span class="sxs-lookup"><span data-stu-id="164c6-207">[Authorize] attribute</span></span>

<span data-ttu-id="164c6-208">Podobnie jak aplikacja może `[Authorize]` być używana z kontrolerem MVC lub stroną Razor, `[Authorize]` może być również używana z składnikami Razor:</span><span class="sxs-lookup"><span data-stu-id="164c6-208">Just like an app can use `[Authorize]` with an MVC controller or Razor page, `[Authorize]` can also be used with Razor Components:</span></span>

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> <span data-ttu-id="164c6-209">Używać `[Authorize]` tylko dla `@page` składników uzyskanych za pośrednictwem routera Blazor.</span><span class="sxs-lookup"><span data-stu-id="164c6-209">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="164c6-210">Autoryzacja jest wykonywana tylko jako aspekt routingu, a *nie* dla składników podrzędnych renderowanych na stronie.</span><span class="sxs-lookup"><span data-stu-id="164c6-210">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="164c6-211">Aby autoryzować wyświetlanie określonych części na stronie, użyj `AuthorizeView` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="164c6-211">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="164c6-212">Może być konieczne dodanie `@using Microsoft.AspNetCore.Authorization` elementu do składnika lub do pliku *_Imports. Razor* w celu skompilowania składnika.</span><span class="sxs-lookup"><span data-stu-id="164c6-212">You may need to add `@using Microsoft.AspNetCore.Authorization` either to the component or to the *_Imports.razor* file in order for the component to compile.</span></span>

<span data-ttu-id="164c6-213">Ten `[Authorize]` atrybut obsługuje również autoryzację opartą na rolach lub zasadach.</span><span class="sxs-lookup"><span data-stu-id="164c6-213">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="164c6-214">W przypadku autoryzacji opartej na rolach Użyj `Roles` parametru:</span><span class="sxs-lookup"><span data-stu-id="164c6-214">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="164c6-215">W przypadku autoryzacji opartej na zasadach należy `Policy` użyć parametru:</span><span class="sxs-lookup"><span data-stu-id="164c6-215">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="164c6-216">Jeśli ani nie `[Authorize]` zostanie określony, program używa domyślnych zasad, które domyślnie są traktowane: `Policy` `Roles`</span><span class="sxs-lookup"><span data-stu-id="164c6-216">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="164c6-217">Uwierzytelniony (zalogowany) Użytkownicy jako autoryzowany.</span><span class="sxs-lookup"><span data-stu-id="164c6-217">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="164c6-218">Nieuwierzytelnionych (wylogowanych) użytkowników jako nieautoryzowanych.</span><span class="sxs-lookup"><span data-stu-id="164c6-218">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="164c6-219">Dostosowywanie nieautoryzowanej zawartości za pomocą składnika routera</span><span class="sxs-lookup"><span data-stu-id="164c6-219">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="164c6-220">Składnik, w połączeniu `AuthorizeRouteView` z składnikiem, umożliwia aplikacji określenie zawartości niestandardowej, jeśli: `Router`</span><span class="sxs-lookup"><span data-stu-id="164c6-220">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="164c6-221">Nie znaleziono zawartości.</span><span class="sxs-lookup"><span data-stu-id="164c6-221">Content isn't found.</span></span>
* <span data-ttu-id="164c6-222">Użytkownik nie może `[Authorize]` wykonać warunku zastosowanego do składnika.</span><span class="sxs-lookup"><span data-stu-id="164c6-222">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="164c6-223">Ten `[Authorize]` atrybut jest pokryty w sekcji [atrybutu [autoryzuje]](#authorize-attribute) .</span><span class="sxs-lookup"><span data-stu-id="164c6-223">The `[Authorize]` attribute is covered in the [[Authorize] attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="164c6-224">Uwierzytelnianie asynchroniczne jest w toku.</span><span class="sxs-lookup"><span data-stu-id="164c6-224">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="164c6-225">W domyślnym szablonie projektu serwera Blazor plik *App. Razor* ilustruje sposób ustawiania zawartości niestandardowej:</span><span class="sxs-lookup"><span data-stu-id="164c6-225">In the default Blazor Server project template, the *App.razor* file demonstrates how to set custom content:</span></span>

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

<span data-ttu-id="164c6-226">Zawartość `<NotFound>`, `<NotAuthorized>`, i`<Authorizing>` Tagi mogą zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="164c6-226">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="164c6-227">Jeśli element nie jest określony, zostanie `AuthorizeRouteView` użyty następujący komunikat rezerwowy: `<NotAuthorized>`</span><span class="sxs-lookup"><span data-stu-id="164c6-227">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="164c6-228">Powiadomienie o zmianach stanu uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="164c6-228">Notification about authentication state changes</span></span>

<span data-ttu-id="164c6-229">Jeśli aplikacja określi, że dane stanu uwierzytelniania zostały zmienione (na przykład ponieważ użytkownik wylogowany lub inny użytkownik zmienił swoje role), niestandardowe `AuthenticationStateProvider` może opcjonalnie wywołać metodę `NotifyAuthenticationStateChanged` na `AuthenticationStateProvider` podstawie określonej.</span><span class="sxs-lookup"><span data-stu-id="164c6-229">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="164c6-230">Spowoduje to powiadomienie klientów o danych stanu uwierzytelniania (na przykład `AuthorizeView`) w celu ponownego renderowania przy użyciu nowych danych.</span><span class="sxs-lookup"><span data-stu-id="164c6-230">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="164c6-231">Logika proceduralna</span><span class="sxs-lookup"><span data-stu-id="164c6-231">Procedural logic</span></span>

<span data-ttu-id="164c6-232">Jeśli aplikacja jest wymagana do sprawdzenia reguł autoryzacji jako części logiki proceduralnej, należy użyć kaskadowego parametru typu `Task<AuthenticationState>` , aby uzyskać <xref:System.Security.Claims.ClaimsPrincipal>użytkownika.</span><span class="sxs-lookup"><span data-stu-id="164c6-232">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="164c6-233">`Task<AuthenticationState>`można łączyć z innymi usługami, takimi jak `IAuthorizationService`, do analizowania zasad.</span><span class="sxs-lookup"><span data-stu-id="164c6-233">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

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

## <a name="authorization-in-blazor-webassembly-apps"></a><span data-ttu-id="164c6-234">Autoryzacja w aplikacjach webassembly Blazor</span><span class="sxs-lookup"><span data-stu-id="164c6-234">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="164c6-235">W aplikacjach webassembly Blazor można ominąć sprawdzanie autoryzacji, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="164c6-235">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="164c6-236">Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="164c6-236">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="164c6-237">**Zawsze sprawdzaj autoryzację na serwerze w ramach dowolnych punktów końcowych interfejsu API, do których uzyskuje dostęp aplikacja po stronie klienta.**</span><span class="sxs-lookup"><span data-stu-id="164c6-237">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="164c6-238">Rozwiązywanie problemów z błędami</span><span class="sxs-lookup"><span data-stu-id="164c6-238">Troubleshoot errors</span></span>

<span data-ttu-id="164c6-239">Typowe błędy:</span><span class="sxs-lookup"><span data-stu-id="164c6-239">Common errors:</span></span>

* <span data-ttu-id="164c6-240">**Autoryzacja wymaga parametru kaskadowego typu Task\<AuthenticationState >. Rozważ użycie CascadingAuthenticationState, aby to zrobić.**</span><span class="sxs-lookup"><span data-stu-id="164c6-240">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="164c6-241">**`null`Odebrano wartość dla`authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="164c6-241">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="164c6-242">Prawdopodobnie projekt nie został utworzony przy użyciu szablonu serwera Blazor z włączonym uwierzytelnianiem.</span><span class="sxs-lookup"><span data-stu-id="164c6-242">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="164c6-243">Zawiń wokół pewnej części drzewa interfejsu użytkownika, na przykład w *App. Razor* w następujący sposób: `<CascadingAuthenticationState>`</span><span class="sxs-lookup"><span data-stu-id="164c6-243">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="164c6-244">Dostarcza parametr kaskadowy, który z kolei otrzymuje od podstawowej `AuthenticationStateProvider` usługi di. `Task<AuthenticationState>` `CascadingAuthenticationState`</span><span class="sxs-lookup"><span data-stu-id="164c6-244">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="164c6-245">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="164c6-245">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
