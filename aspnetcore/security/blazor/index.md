---
title: ASP.NET Core Blazor uwierzytelnianie i autoryzacja
author: guardrex
description: Więcej informacji na temat Blazor scenariuszy uwierzytelniania i autoryzacji.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/26/2019
uid: security/blazor/index
ms.openlocfilehash: b3bca26e7088a8353084a065f9b9593c9d8e08e6
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406200"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="fc6e5-103">ASP.NET Core Blazor uwierzytelnianie i autoryzacja</span><span class="sxs-lookup"><span data-stu-id="fc6e5-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="fc6e5-104">Przez [Steve sanderson o](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="fc6e5-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="fc6e5-105">Platforma ASP.NET Core obsługuje konfigurację i zarządzanie zabezpieczeniami w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="fc6e5-106">Scenariusze zabezpieczeń różnią się między aplikacjami Blazor po stronie serwera i klienta.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-106">Security scenarios differ between Blazor server-side and client-side apps.</span></span> <span data-ttu-id="fc6e5-107">Ponieważ Blazor aplikacji po stronie serwera, uruchom na serwerze, są w stanie określić sprawdzeń autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-107">Because Blazor server-side apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="fc6e5-108">Opcji interfejsu użytkownika dostępnych dla użytkownika (na przykład, w której elementy menu są dostępne dla użytkownika).</span><span class="sxs-lookup"><span data-stu-id="fc6e5-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="fc6e5-109">Reguły dostępu dla obszarów aplikacji i składników.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="fc6e5-110">Aplikacje klienta Blazor są uruchamiane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-110">Blazor client-side apps run on the client.</span></span> <span data-ttu-id="fc6e5-111">Autoryzacja jest *tylko* umożliwia określenie opcji interfejsu użytkownika, aby pokazać.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="fc6e5-112">Ponieważ testy po stronie klienta może modyfikować lub pomijany przez użytkownika, aplikacja klienta Blazor nie może wymuszać reguły dostępu do autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-112">Since client-side checks can be modified or bypassed by a user, a Blazor client-side app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="fc6e5-113">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="fc6e5-113">Authentication</span></span>

<span data-ttu-id="fc6e5-114">Blazor używa istniejących mechanizmów uwierzytelniania platformy ASP.NET Core w celu ustalenia tożsamości użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="fc6e5-115">Dokładny mechanizm zależy od tego, jak aplikacja Blazor jest hostowana po stronie serwera lub klienta.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-115">The exact mechanism depends on how the Blazor app is hosted, server-side or client-side.</span></span>

### <a name="blazor-server-side-authentication"></a><span data-ttu-id="fc6e5-116">Blazor po stronie serwera uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="fc6e5-116">Blazor server-side authentication</span></span>

<span data-ttu-id="fc6e5-117">Aplikacje serwerowe Blazor działa za pośrednictwem połączenia w czasie rzeczywistym, który jest tworzony przy użyciu SignalR.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-117">Blazor server-side apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="fc6e5-118">[Uwierzytelnianie w aplikacjach opartych na SignalR](xref:signalr/authn-and-authz) jest obsługiwane, gdy połączenie zostanie nawiązane.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="fc6e5-119">Uwierzytelnianie może bazować na pliku cookie lub innych tokenu elementu nośnego.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="fc6e5-120">Szablon projektu po stronie serwera Blazor uwierzytelnianie można skonfigurować dla Ciebie podczas tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-120">The Blazor server-side project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fc6e5-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fc6e5-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fc6e5-122">Zgodnie z wytycznymi programu Visual Studio w <xref:blazor/get-started> artykuł, aby utworzyć nowy projekt po stronie serwera Blazor przy użyciu mechanizmu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism.</span></span>

<span data-ttu-id="fc6e5-123">Po wybraniu **Blazor (po stronie serwera)** szablonu w **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okno dialogowe, wybierz opcję **zmiany** w obszarze **uwierzytelniania** .</span><span class="sxs-lookup"><span data-stu-id="fc6e5-123">After choosing the **Blazor (server-side)** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="fc6e5-124">Otwiera okno dialogowe do zaoferowania ten sam zestaw mechanizmy uwierzytelniania dostępne dla innych platformy ASP.NET Core projektów:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="fc6e5-125">**Bez uwierzytelniania**</span><span class="sxs-lookup"><span data-stu-id="fc6e5-125">**No Authentication**</span></span>
* <span data-ttu-id="fc6e5-126">**Indywidualne konta użytkowników** &ndash; konta użytkowników mogą być przechowywane:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="fc6e5-127">W aplikacji przy użyciu platformy ASP.NET Core [tożsamości](xref:security/authentication/identity) systemu.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="fc6e5-128">Za pomocą [usługi Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="fc6e5-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="fc6e5-129">**Konta służbowe**</span><span class="sxs-lookup"><span data-stu-id="fc6e5-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="fc6e5-130">**Uwierzytelnianie Windows**</span><span class="sxs-lookup"><span data-stu-id="fc6e5-130">**Windows Authentication**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fc6e5-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fc6e5-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fc6e5-132">Zgodnie z wytycznymi programu Visual Studio Code w <xref:blazor/get-started> artykuł, aby utworzyć nowy projekt po stronie serwera Blazor przy użyciu mechanizmu uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism:</span></span>

```console
dotnet new blazorserverside -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="fc6e5-133">Uwierzytelniania dopuszczalnej wartości (`{AUTHENTICATION}`) są wyświetlane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="fc6e5-134">Mechanizm uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="fc6e5-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="fc6e5-135">`{AUTHENTICATION}` Wartość</span><span class="sxs-lookup"><span data-stu-id="fc6e5-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="fc6e5-136">Bez uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="fc6e5-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="fc6e5-137">Osoby</span><span class="sxs-lookup"><span data-stu-id="fc6e5-137">Individual</span></span><br><span data-ttu-id="fc6e5-138">Użytkownicy przechowywani w aplikacji za pomocą tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="fc6e5-139">Osoby</span><span class="sxs-lookup"><span data-stu-id="fc6e5-139">Individual</span></span><br><span data-ttu-id="fc6e5-140">Użytkownicy są przechowywane w [usługi Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="fc6e5-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="fc6e5-141">Konta służbowe</span><span class="sxs-lookup"><span data-stu-id="fc6e5-141">Work or School Accounts</span></span><br><span data-ttu-id="fc6e5-142">Uwierzytelnianie organizacyjne dla jednej dzierżawy.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="fc6e5-143">Konta służbowe</span><span class="sxs-lookup"><span data-stu-id="fc6e5-143">Work or School Accounts</span></span><br><span data-ttu-id="fc6e5-144">Uwierzytelnianie organizacyjne dla wielu dzierżaw.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="fc6e5-145">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="fc6e5-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="fc6e5-146">Polecenie tworzy folder o nazwie z wartością parametru `{APP NAME}` symboli zastępczych i używa nazwy folderu, jak nazwa aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="fc6e5-147">Aby uzyskać więcej informacji, zobacz [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia w przewodnik platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism:

```console
dotnet new blazorserverside -o {APP NAME} -au {AUTHENTICATION}
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

### <a name="blazor-client-side-authentication"></a><span data-ttu-id="fc6e5-148">Uwierzytelnianie klienta Blazor</span><span class="sxs-lookup"><span data-stu-id="fc6e5-148">Blazor client-side authentication</span></span>

<span data-ttu-id="fc6e5-149">W aplikacjach klienta Blazor kontroli uwierzytelniania można pominąć, ponieważ cały kod po stronie klienta może być modyfikowane przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-149">In Blazor client-side apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="fc6e5-150">Dotyczy to także wszystkie technologie aplikacji po stronie klienta, w tym struktur JavaScript SPA lub natywne aplikacje dla dowolnego systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="fc6e5-151">Implementacji niestandardowego `AuthenticationStateProvider` usługa dla aplikacji po stronie klienta Blazor zostało omówione w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-151">Implementation of a custom `AuthenticationStateProvider` service for Blazor client-side apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="fc6e5-152">Usługa AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="fc6e5-152">AuthenticationStateProvider service</span></span>

<span data-ttu-id="fc6e5-153">Aplikacje serwerowe Blazor obejmują wbudowaną `AuthenticationStateProvider` usługa, która uzyskuje dane o stanie uwierzytelniania z platformy ASP.NET Core `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-153">Blazor server-side apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="fc6e5-154">Jest to, jak stan uwierzytelniania integruje się z istniejących mechanizmów uwierzytelniania po stronie serwera platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-154">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="fc6e5-155">`AuthenticationStateProvider` podstawowy usługa jest używana przez `AuthorizeView` składnika i `CascadingAuthenticationState` składnika można pobrać stanu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-155">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="fc6e5-156">Nie są zwykle używane `AuthenticationStateProvider` bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-156">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="fc6e5-157">Użyj [składnika AuthorizeView](#authorizeview-component) lub [zadań<AuthenticationState> ](#expose-the-authentication-state-as-a-cascading-parameter) podejść opisanych w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-157">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="fc6e5-158">Główną wadą za pomocą `AuthenticationStateProvider` bezpośrednio jest, że składnik nie jest automatycznie powiadomienia, gdy zmianie danych stanu uwierzytelniania podstawowego.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-158">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="fc6e5-159">`AuthenticationStateProvider` Usługi może zapewnić bieżący użytkownik <xref:System.Security.Claims.ClaimsPrincipal> danych, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-159">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

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

<span data-ttu-id="fc6e5-160">Jeśli `user.Identity.IsAuthenticated` jest `true` , a użytkownik jest <xref:System.Security.Claims.ClaimsPrincipal>, mogą być wyliczane oświadczeń i oceny członkostwa w roli.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-160">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="fc6e5-161">Aby uzyskać więcej informacji na temat wstrzykiwanie zależności (DI) i usług, zobacz <xref:blazor/dependency-injection> i <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-161">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="fc6e5-162">Implementowanie niestandardowego AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="fc6e5-162">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="fc6e5-163">Jeśli tworzysz aplikację po stronie klienta Blazor lub jeśli Specyfikacja aplikacji wymaga absolutnie niestandardowego dostawcy implementowanie dostawcy i zastąpić `GetAuthenticationStateAsync`:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-163">If you're building a Blazor client-side app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

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

<span data-ttu-id="fc6e5-164">`CustomAuthStateProvider` Usługa jest zarejestrowana w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-164">The `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
}
```

<span data-ttu-id="fc6e5-165">Za pomocą `CustomAuthStateProvider`, wszyscy użytkownicy są uwierzytelniani za pomocą nazwy użytkownika `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-165">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="fc6e5-166">Udostępnianie stanu uwierzytelniania jako parametr kaskadowe</span><span class="sxs-lookup"><span data-stu-id="fc6e5-166">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="fc6e5-167">Jeśli dane o stanie uwierzytelniania jest wymagany dla logiki proceduralne, takie jak kiedy wykonuje akcję wyzwalany przez użytkownika, należy uzyskać dane stanu uwierzytelniania, definiując kaskadowych parametr typu `Task<AuthenticationState>`:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-167">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

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

<span data-ttu-id="fc6e5-168">Jeśli `user.Identity.IsAuthenticated` jest `true`, mogą być wyliczane oświadczeń i oceny członkostwa w roli.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-168">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="fc6e5-169">Konfigurowanie `Task<AuthenticationState>` cascading przy użyciu parametru `CascadingAuthenticationState` składników:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-169">Set up the `Task<AuthenticationState>` cascading parameter using the `CascadingAuthenticationState` component:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        <NotFoundContent>
            <h1>Sorry</h1>
            <p>Sorry, there's nothing at this address.</p>
        </NotFoundContent>
    </Router>
</CascadingAuthenticationState>
```

## <a name="authorization"></a><span data-ttu-id="fc6e5-170">Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="fc6e5-170">Authorization</span></span>

<span data-ttu-id="fc6e5-171">Po uwierzytelnieniu użytkownika *autoryzacji* reguły są stosowane do kontrolowania czynności użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-171">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="fc6e5-172">Zazwyczaj udzielić lub odmówić dostępu na podstawie:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-172">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="fc6e5-173">Użytkownik jest uwierzytelniany (zalogowany).</span><span class="sxs-lookup"><span data-stu-id="fc6e5-173">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="fc6e5-174">Użytkownik znajduje się w *roli*.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-174">A user is in a *role*.</span></span>
* <span data-ttu-id="fc6e5-175">Użytkownik ma *oświadczenia*.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-175">A user has a *claim*.</span></span>
* <span data-ttu-id="fc6e5-176">A *zasad* jest spełniony.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-176">A *policy* is satisfied.</span></span>

<span data-ttu-id="fc6e5-177">Każdy z tych pojęć jest taki sam jak w aplikacji ASP.NET Core MVC lub stron Razor.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-177">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="fc6e5-178">Aby uzyskać więcej informacji na temat zabezpieczeń platformy ASP.NET Core, zobacz artykuły w obszarze [platformy ASP.NET Core zabezpieczenia i tożsamość](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="fc6e5-178">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="fc6e5-179">Składnik AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="fc6e5-179">AuthorizeView component</span></span>

<span data-ttu-id="fc6e5-180">`AuthorizeView` Składnika selektywnie pojawi się interfejs użytkownika w zależności od tego, czy użytkownik jest uprawniony do wyświetlenia go.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-180">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="fc6e5-181">Takie podejście jest przydatne, gdy trzeba tylko *wyświetlić* danych użytkownika i nie trzeba używać tożsamości użytkownika w procedurach logiki.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-181">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="fc6e5-182">Przedstawia składnik `context` zmiennej typu `AuthenticationState`, który umożliwia dostęp do informacji dotyczących zalogowanego użytkownika:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-182">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="fc6e5-183">Jeśli użytkownik nie jest uwierzytelniony, może też podawać różną zawartość w przypadku wyświetlania:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-183">You can also supply different content for display if the user isn't authenticated:</span></span>

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

<span data-ttu-id="fc6e5-184">Zawartość `<Authorized>` i `<NotAuthorized>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-184">The content of `<Authorized>` and `<NotAuthorized>` can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="fc6e5-185">Warunki autoryzacji, takich jak role lub zasady, które kontrolują dostęp, lub opcji interfejsu użytkownika są objęte [autoryzacji](#authorization) sekcji.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-185">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="fc6e5-186">Jeśli warunki autoryzacji nie są określone, `AuthorizeView` używa domyślnych zasad i traktuje:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-186">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="fc6e5-187">Uwierzytelnieni użytkownicy (zalogowany) jako autoryzowane.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-187">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="fc6e5-188">Brak autoryzacji nieuwierzytelnionych użytkowników (podpisane w poziomie).</span><span class="sxs-lookup"><span data-stu-id="fc6e5-188">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="fc6e5-189">Autoryzacja oparta na rolach i oparte na zasadach</span><span class="sxs-lookup"><span data-stu-id="fc6e5-189">Role-based and policy-based authorization</span></span>

<span data-ttu-id="fc6e5-190">`AuthorizeView` Składnik obsługuje *opartej na rolach* lub *oparte na zasadach* autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-190">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="fc6e5-191">Do autoryzacji opartej na rolach, należy użyć `Roles` parametru:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-191">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="fc6e5-192">Aby uzyskać więcej informacji, zobacz <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-192">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="fc6e5-193">Autoryzacja oparta na zasadach, można użyć `Policy` parametru:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-193">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="fc6e5-194">Autoryzacja oparta na oświadczeniach jest przypadkiem szczególnym autoryzacji opartej na zasadach.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-194">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="fc6e5-195">Na przykład można zdefiniować zasady, które użytkownicy muszą mieć określone oświadczenie.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-195">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="fc6e5-196">Aby uzyskać więcej informacji, zobacz <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-196">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="fc6e5-197">Te interfejsy API może służyć w Blazor po stronie serwera lub aplikacji po stronie klienta Blazor.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-197">These APIs can be used in either Blazor server-side or Blazor client-side apps.</span></span>

<span data-ttu-id="fc6e5-198">Jeśli żadna `Roles` ani `Policy` jest określony, `AuthorizeView` używa domyślnej zasady.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-198">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="fc6e5-199">Zawartość wyświetlana podczas uwierzytelniania asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="fc6e5-199">Content displayed during asynchronous authentication</span></span>

<span data-ttu-id="fc6e5-200">Umożliwia Blazor stan uwierzytelniania określone *asynchronicznie*.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-200">Blazor allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="fc6e5-201">Podstawowy scenariusz, w tym podejściu jest Blazor aplikacji po stronie klienta, które wysłać żądanie do zewnętrznego punktu końcowego na potrzeby uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-201">The primary scenario for this approach is in Blazor client-side apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="fc6e5-202">Gdy uwierzytelnianie jest w toku, `AuthorizeView` domyślnie wyświetla żadnej zawartości.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-202">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="fc6e5-203">Aby wyświetlić zawartość, a uwierzytelnianie odbywa się, należy użyć `<Authorizing>` elementu:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-203">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

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

<span data-ttu-id="fc6e5-204">Ta metoda nie jest zazwyczaj dotyczy Blazor po stronie serwera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-204">This approach isn't normally applicable to Blazor server-side apps.</span></span> <span data-ttu-id="fc6e5-205">Aplikacje serwerowe Blazor znać stan uwierzytelniania zaraz po jego stan zostanie nawiązane.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-205">Blazor server-side apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="fc6e5-206">`Authorizing` zawartość może znajdować się w aplikacji po stronie serwera Blazor `AuthorizeView` składnik, ale zawartość nigdy nie jest wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-206">`Authorizing` content can be provided in a Blazor server-side app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="fc6e5-207">Atrybut [autoryzować]</span><span class="sxs-lookup"><span data-stu-id="fc6e5-207">[Authorize] attribute</span></span>

<span data-ttu-id="fc6e5-208">Tak samo, jak aplikacja może używać `[Authorize]` przy użyciu kontrolera MVC lub strona Razor `[Authorize]` można również ze składnikami Razor:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-208">Just like an app can use `[Authorize]` with an MVC controller or Razor page, `[Authorize]` can also be used with Razor Components:</span></span>

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> <span data-ttu-id="fc6e5-209">Używaj tylko `[Authorize]` na `@page` składniki skontaktować za pośrednictwem routera Blazor.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-209">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="fc6e5-210">Autoryzacja jest realizowane wyłącznie jako aspekt, routingu i *nie* składników podrzędnych renderowane na stronie.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-210">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="fc6e5-211">Aby autoryzować wyświetlanie określonych części strony, należy użyć `AuthorizeView` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-211">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="fc6e5-212">Może być konieczne dodanie `@using Microsoft.AspNetCore.Authorization` do składnika lub do *_Imports.razor* pliku w kolejności, w celu kompilowania składnika.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-212">You may need to add `@using Microsoft.AspNetCore.Authorization` either to the component or to the *_Imports.razor* file in order for the component to compile.</span></span>

<span data-ttu-id="fc6e5-213">`[Authorize]` Atrybutu obsługuje również autoryzacji opartej na rolach lub oparta na zasadach.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-213">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="fc6e5-214">Do autoryzacji opartej na rolach, należy użyć `Roles` parametru:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-214">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="fc6e5-215">Autoryzacja oparta na zasadach, można użyć `Policy` parametru:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-215">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="fc6e5-216">Jeśli żadna `Roles` ani `Policy` jest określony, `[Authorize]` używa domyślnej zasady, która domyślnie jest:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-216">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="fc6e5-217">Uwierzytelnieni użytkownicy (zalogowany) jako autoryzowane.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-217">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="fc6e5-218">Brak autoryzacji nieuwierzytelnionych użytkowników (podpisane w poziomie).</span><span class="sxs-lookup"><span data-stu-id="fc6e5-218">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="fc6e5-219">Dostosowywanie zawartości nieautoryzowanym ze składnikiem routera</span><span class="sxs-lookup"><span data-stu-id="fc6e5-219">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="fc6e5-220">`Router` Składnik umożliwia aplikacji określenie niestandardowej zawartości, jeśli:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-220">The `Router` component allows the app to specify custom content if:</span></span>

* <span data-ttu-id="fc6e5-221">Zawartość nie zostanie znaleziona.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-221">Content isn't found.</span></span>
* <span data-ttu-id="fc6e5-222">Użytkownik nie `[Authorize]` warunek zastosowane do składnika.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-222">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="fc6e5-223">`[Authorize]` Atrybut został omówiony w [atrybutu [Authorize]](#authorize-attribute) sekcji.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-223">The `[Authorize]` attribute is covered in the [[Authorize] attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="fc6e5-224">Asynchroniczne uwierzytelniania jest w toku.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-224">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="fc6e5-225">W domyślnym szablonie projektu po stronie serwera Blazor *App.razor* plik pokazuje, jak ustawić niestandardową zawartość:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-225">In the default Blazor server-side project template, the *App.razor* file demonstrates how to set custom content:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        <NotFoundContent>
            <h1>Sorry</h1>
            <p>Sorry, there's nothing at this address.</p>
        </NotFoundContent>
        <NotAuthorizedContent>
            <h1>Sorry</h1>
            <p>You're not authorized to reach this page.</p>
            <p>You may need to log in as a different user.</p>
        </NotAuthorizedContent>
        <AuthorizingContent>
            <h1>Authentication in progress</h1>
            <p>Only visible while authentication is in progress.</p>
        </AuthorizingContent>
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="fc6e5-226">Zawartość `<NotFoundContent>`, `<NotAuthorizedContent>`, i `<AuthorizingContent>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-226">The content of `<NotFoundContent>`, `<NotAuthorizedContent>`, and `<AuthorizingContent>` can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="fc6e5-227">Jeśli `<NotAuthorizedContent>` nie jest określona, router używa rezerwowej następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-227">If `<NotAuthorizedContent>` isn't specified, the router uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="fc6e5-228">Powiadomienie o zmianach stanu uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="fc6e5-228">Notification about authentication state changes</span></span>

<span data-ttu-id="fc6e5-229">Jeśli aplikacja okaże się, że danych bazowych stanu uwierzytelniania został zmieniony (na przykład, ponieważ zmiany ich ról użytkownika wylogowany lub innego użytkownika), niestandardowe `AuthenticationStateProvider` Opcjonalnie można wywołać metody `NotifyAuthenticationStateChanged` na `AuthenticationStateProvider` podstawowy Klasa.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-229">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="fc6e5-230">To powiadamia klientów uwierzytelniania danych o stanie (na przykład `AuthorizeView`) do rerender przy użyciu nowych danych.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-230">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="fc6e5-231">Proceduralne logiki</span><span class="sxs-lookup"><span data-stu-id="fc6e5-231">Procedural logic</span></span>

<span data-ttu-id="fc6e5-232">Jeśli aplikacja jest wymagany do sprawdzenia reguł autoryzacji jako część logiki proceduralne, użyj parametru kaskadowy typu `Task<AuthenticationState>` uzyskać użytkownika <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-232">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="fc6e5-233">`Task<AuthenticationState>` można łączyć z innymi usługami, takie jak `IAuthorizationService`, aby oceniać zasady.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-233">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

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

## <a name="authorization-in-blazor-client-side-apps"></a><span data-ttu-id="fc6e5-234">Autoryzacja w aplikacji po stronie klienta Blazor</span><span class="sxs-lookup"><span data-stu-id="fc6e5-234">Authorization in Blazor client-side apps</span></span>

<span data-ttu-id="fc6e5-235">Blazor aplikacji po stronie klienta można pominąć sprawdzanie autoryzacji, ponieważ cały kod po stronie klienta może być modyfikowane przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-235">In Blazor client-side apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="fc6e5-236">Dotyczy to także wszystkie technologie aplikacji po stronie klienta, w tym struktur JavaScript SPA lub natywne aplikacje dla dowolnego systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-236">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="fc6e5-237">**Zawsze wykonują sprawdzanie autoryzacji na serwerze w ramach żadnych punktów końcowych interfejsu API dostępne dla aplikacji po stronie klienta.**</span><span class="sxs-lookup"><span data-stu-id="fc6e5-237">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="fc6e5-238">Rozwiązywanie problemów z błędami</span><span class="sxs-lookup"><span data-stu-id="fc6e5-238">Troubleshoot errors</span></span>

<span data-ttu-id="fc6e5-239">Typowe błędy:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-239">Common errors:</span></span>

* <span data-ttu-id="fc6e5-240">**Wymaga kaskadowych parametr typu zadania\<AuthenticationState >. Należy rozważyć użycie CascadingAuthenticationState je podać.**</span><span class="sxs-lookup"><span data-stu-id="fc6e5-240">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="fc6e5-241">**`null` wartość jest odbierane dla `authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="fc6e5-241">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="fc6e5-242">Istnieje prawdopodobieństwo, że projekt nie został utworzony przy użyciu szablonu po stronie serwera Blazor z włączone uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-242">It's likely that the project wasn't created using a Blazor server-side template with authentication enabled.</span></span> <span data-ttu-id="fc6e5-243">OPAKOWYWANIE `<CascadingAuthenticationState>` wokół część drzewo interfejsu użytkownika, na przykład w *App.razor* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fc6e5-243">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="fc6e5-244">`CascadingAuthenticationState` Dostarcza `Task<AuthenticationState>` kaskadowych parametr, który z kolei otrzymuje z bazowego `AuthenticationStateProvider` DI usługi.</span><span class="sxs-lookup"><span data-stu-id="fc6e5-244">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fc6e5-245">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fc6e5-245">Additional resources</span></span>

* <xref:security/index>
* <xref:security/authentication/windowsauth>
