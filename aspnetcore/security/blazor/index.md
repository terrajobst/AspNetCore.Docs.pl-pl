---
title: ASP.NET Core Blazor uwierzytelnianie i autoryzacja
author: guardrex
description: Więcej informacji na temat Blazor scenariuszy uwierzytelniania i autoryzacji.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/26/2019
uid: security/blazor/index
ms.openlocfilehash: 097a747f68729109922af5c68dfd918024ee6146
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67893614"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a>ASP.NET Core Blazor uwierzytelnianie i autoryzacja

Przez [Steve sanderson o](https://github.com/SteveSandersonMS)

Platforma ASP.NET Core obsługuje konfigurację i zarządzanie zabezpieczeniami w aplikacjach Blazor.

Scenariusze zabezpieczeń różnią się między aplikacjami Blazor po stronie serwera i klienta. Ponieważ Blazor aplikacji po stronie serwera, uruchom na serwerze, są w stanie określić sprawdzeń autoryzacji:

* Opcji interfejsu użytkownika dostępnych dla użytkownika (na przykład, w której elementy menu są dostępne dla użytkownika).
* Reguły dostępu dla obszarów aplikacji i składników.

Aplikacje klienta Blazor są uruchamiane na komputerze klienckim. Autoryzacja jest *tylko* umożliwia określenie opcji interfejsu użytkownika, aby pokazać. Ponieważ testy po stronie klienta może modyfikować lub pomijany przez użytkownika, aplikacja klienta Blazor nie może wymuszać reguły dostępu do autoryzacji.

## <a name="authentication"></a>Uwierzytelnianie

Blazor używa istniejących mechanizmów uwierzytelniania platformy ASP.NET Core w celu ustalenia tożsamości użytkownika. Dokładny mechanizm zależy od tego, jak aplikacja Blazor jest hostowana po stronie serwera lub klienta.

### <a name="blazor-server-side-authentication"></a>Blazor po stronie serwera uwierzytelniania

Aplikacje serwerowe Blazor działa za pośrednictwem połączenia w czasie rzeczywistym, który jest tworzony przy użyciu SignalR. [Uwierzytelnianie w aplikacjach opartych na SignalR](xref:signalr/authn-and-authz) jest obsługiwane, gdy połączenie zostanie nawiązane. Uwierzytelnianie może bazować na pliku cookie lub innych tokenu elementu nośnego.

Szablon projektu po stronie serwera Blazor uwierzytelnianie można skonfigurować dla Ciebie podczas tworzenia projektu.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Zgodnie z wytycznymi programu Visual Studio w <xref:blazor/get-started> artykuł, aby utworzyć nowy projekt po stronie serwera Blazor przy użyciu mechanizmu uwierzytelniania.

Po wybraniu **aplikacja serwera Blazor** szablonu w **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okno dialogowe, wybierz opcję **zmiany** w obszarze **uwierzytelniania** .

Otwiera okno dialogowe do zaoferowania ten sam zestaw mechanizmy uwierzytelniania dostępne dla innych platformy ASP.NET Core projektów:

* **Bez uwierzytelniania**
* **Indywidualne konta użytkowników** &ndash; konta użytkowników mogą być przechowywane:
  * W aplikacji przy użyciu platformy ASP.NET Core [tożsamości](xref:security/authentication/identity) systemu.
  * Za pomocą [usługi Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* **Konta służbowe**
* **Uwierzytelnianie Windows**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Zgodnie z wytycznymi programu Visual Studio Code w <xref:blazor/get-started> artykuł, aby utworzyć nowy projekt po stronie serwera Blazor przy użyciu mechanizmu uwierzytelniania:

```console
dotnet new blazorserverside -o {APP NAME} -au {AUTHENTICATION}
```

Uwierzytelniania dopuszczalnej wartości (`{AUTHENTICATION}`) są wyświetlane w poniższej tabeli.

| Mechanizm uwierzytelniania                                                                 | `{AUTHENTICATION}` Wartość |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| Bez uwierzytelniania                                                                        | `None`                   |
| Osoby<br>Użytkownicy przechowywani w aplikacji za pomocą tożsamości platformy ASP.NET Core.                        | `Individual`             |
| Osoby<br>Użytkownicy są przechowywane w [usługi Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Konta służbowe<br>Uwierzytelnianie organizacyjne dla jednej dzierżawy.            | `SingleOrg`              |
| Konta służbowe<br>Uwierzytelnianie organizacyjne dla wielu dzierżaw.           | `MultiOrg`               |
| Uwierzytelnianie systemu Windows                                                                   | `Windows`                |

Polecenie tworzy folder o nazwie z wartością parametru `{APP NAME}` symboli zastępczych i używa nazwy folderu, jak nazwa aplikacji. Aby uzyskać więcej informacji, zobacz [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia w przewodnik platformy .NET Core.

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

### <a name="blazor-client-side-authentication"></a>Uwierzytelnianie klienta Blazor

W aplikacjach klienta Blazor kontroli uwierzytelniania można pominąć, ponieważ cały kod po stronie klienta może być modyfikowane przez użytkowników. Dotyczy to także wszystkie technologie aplikacji po stronie klienta, w tym struktur JavaScript SPA lub natywne aplikacje dla dowolnego systemu operacyjnego.

Implementacji niestandardowego `AuthenticationStateProvider` usługa dla aplikacji po stronie klienta Blazor zostało omówione w poniższych sekcjach.

## <a name="authenticationstateprovider-service"></a>Usługa AuthenticationStateProvider

Aplikacje serwerowe Blazor obejmują wbudowaną `AuthenticationStateProvider` usługa, która uzyskuje dane o stanie uwierzytelniania z platformy ASP.NET Core `HttpContext.User`. Jest to, jak stan uwierzytelniania integruje się z istniejących mechanizmów uwierzytelniania po stronie serwera platformy ASP.NET Core.

`AuthenticationStateProvider` podstawowy usługa jest używana przez `AuthorizeView` składnika i `CascadingAuthenticationState` składnika można pobrać stanu uwierzytelniania.

Nie są zwykle używane `AuthenticationStateProvider` bezpośrednio. Użyj [składnika AuthorizeView](#authorizeview-component) lub [zadań<AuthenticationState> ](#expose-the-authentication-state-as-a-cascading-parameter) podejść opisanych w dalszej części tego artykułu. Główną wadą za pomocą `AuthenticationStateProvider` bezpośrednio jest, że składnik nie jest automatycznie powiadomienia, gdy zmianie danych stanu uwierzytelniania podstawowego.

`AuthenticationStateProvider` Usługi może zapewnić bieżący użytkownik <xref:System.Security.Claims.ClaimsPrincipal> danych, jak pokazano w poniższym przykładzie:

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

Jeśli `user.Identity.IsAuthenticated` jest `true` , a użytkownik jest <xref:System.Security.Claims.ClaimsPrincipal>, mogą być wyliczane oświadczeń i oceny członkostwa w roli.

Aby uzyskać więcej informacji na temat wstrzykiwanie zależności (DI) i usług, zobacz <xref:blazor/dependency-injection> i <xref:fundamentals/dependency-injection>.

## <a name="implement-a-custom-authenticationstateprovider"></a>Implementowanie niestandardowego AuthenticationStateProvider

Jeśli tworzysz aplikację po stronie klienta Blazor lub jeśli Specyfikacja aplikacji wymaga absolutnie niestandardowego dostawcy implementowanie dostawcy i zastąpić `GetAuthenticationStateAsync`:

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

`CustomAuthStateProvider` Usługa jest zarejestrowana w `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
}
```

Za pomocą `CustomAuthStateProvider`, wszyscy użytkownicy są uwierzytelniani za pomocą nazwy użytkownika `mrfibuli`.

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a>Udostępnianie stanu uwierzytelniania jako parametr kaskadowe

Jeśli dane o stanie uwierzytelniania jest wymagany dla logiki proceduralne, takie jak kiedy wykonuje akcję wyzwalany przez użytkownika, należy uzyskać dane stanu uwierzytelniania, definiując kaskadowych parametr typu `Task<AuthenticationState>`:

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

Jeśli `user.Identity.IsAuthenticated` jest `true`, mogą być wyliczane oświadczeń i oceny członkostwa w roli.

Konfigurowanie `Task<AuthenticationState>` cascading przy użyciu parametru `CascadingAuthenticationState` składników:

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

## <a name="authorization"></a>Autoryzacja

Po uwierzytelnieniu użytkownika *autoryzacji* reguły są stosowane do kontrolowania czynności użytkownika.

Zazwyczaj udzielić lub odmówić dostępu na podstawie:

* Użytkownik jest uwierzytelniany (zalogowany).
* Użytkownik znajduje się w *roli*.
* Użytkownik ma *oświadczenia*.
* A *zasad* jest spełniony.

Każdy z tych pojęć jest taki sam jak w aplikacji ASP.NET Core MVC lub stron Razor. Aby uzyskać więcej informacji na temat zabezpieczeń platformy ASP.NET Core, zobacz artykuły w obszarze [platformy ASP.NET Core zabezpieczenia i tożsamość](xref:security/index).

## <a name="authorizeview-component"></a>Składnik AuthorizeView

`AuthorizeView` Składnika selektywnie pojawi się interfejs użytkownika w zależności od tego, czy użytkownik jest uprawniony do wyświetlenia go. Takie podejście jest przydatne, gdy trzeba tylko *wyświetlić* danych użytkownika i nie trzeba używać tożsamości użytkownika w procedurach logiki.

Przedstawia składnik `context` zmiennej typu `AuthenticationState`, który umożliwia dostęp do informacji dotyczących zalogowanego użytkownika:

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

Jeśli użytkownik nie jest uwierzytelniony, może też podawać różną zawartość w przypadku wyświetlania:

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

Zawartość `<Authorized>` i `<NotAuthorized>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.

Warunki autoryzacji, takich jak role lub zasady, które kontrolują dostęp, lub opcji interfejsu użytkownika są objęte [autoryzacji](#authorization) sekcji.

Jeśli warunki autoryzacji nie są określone, `AuthorizeView` używa domyślnych zasad i traktuje:

* Uwierzytelnieni użytkownicy (zalogowany) jako autoryzowane.
* Brak autoryzacji nieuwierzytelnionych użytkowników (podpisane w poziomie).

### <a name="role-based-and-policy-based-authorization"></a>Autoryzacja oparta na rolach i oparte na zasadach

`AuthorizeView` Składnik obsługuje *opartej na rolach* lub *oparte na zasadach* autoryzacji.

Do autoryzacji opartej na rolach, należy użyć `Roles` parametru:

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

Aby uzyskać więcej informacji, zobacz <xref:security/authorization/roles>.

Autoryzacja oparta na zasadach, można użyć `Policy` parametru:

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

Autoryzacja oparta na oświadczeniach jest przypadkiem szczególnym autoryzacji opartej na zasadach. Na przykład można zdefiniować zasady, które użytkownicy muszą mieć określone oświadczenie. Aby uzyskać więcej informacji, zobacz <xref:security/authorization/policies>.

Te interfejsy API może służyć w Blazor po stronie serwera lub aplikacji po stronie klienta Blazor.

Jeśli żadna `Roles` ani `Policy` jest określony, `AuthorizeView` używa domyślnej zasady.

### <a name="content-displayed-during-asynchronous-authentication"></a>Zawartość wyświetlana podczas uwierzytelniania asynchroniczne

Umożliwia Blazor stan uwierzytelniania określone *asynchronicznie*. Podstawowy scenariusz, w tym podejściu jest Blazor aplikacji po stronie klienta, które wysłać żądanie do zewnętrznego punktu końcowego na potrzeby uwierzytelniania.

Gdy uwierzytelnianie jest w toku, `AuthorizeView` domyślnie wyświetla żadnej zawartości. Aby wyświetlić zawartość, a uwierzytelnianie odbywa się, należy użyć `<Authorizing>` elementu:

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

Ta metoda nie jest zazwyczaj dotyczy Blazor po stronie serwera aplikacji. Aplikacje serwerowe Blazor znać stan uwierzytelniania zaraz po jego stan zostanie nawiązane. `Authorizing` zawartość może znajdować się w aplikacji po stronie serwera Blazor `AuthorizeView` składnik, ale zawartość nigdy nie jest wyświetlane.

## <a name="authorize-attribute"></a>Atrybut [autoryzować]

Tak samo, jak aplikacja może używać `[Authorize]` przy użyciu kontrolera MVC lub strona Razor `[Authorize]` można również ze składnikami Razor:

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> Używaj tylko `[Authorize]` na `@page` składniki skontaktować za pośrednictwem routera Blazor. Autoryzacja jest realizowane wyłącznie jako aspekt, routingu i *nie* składników podrzędnych renderowane na stronie. Aby autoryzować wyświetlanie określonych części strony, należy użyć `AuthorizeView` zamiast tego.

Może być konieczne dodanie `@using Microsoft.AspNetCore.Authorization` do składnika lub do *_Imports.razor* pliku w kolejności, w celu kompilowania składnika.

`[Authorize]` Atrybutu obsługuje również autoryzacji opartej na rolach lub oparta na zasadach. Do autoryzacji opartej na rolach, należy użyć `Roles` parametru:

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

Autoryzacja oparta na zasadach, można użyć `Policy` parametru:

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

Jeśli żadna `Roles` ani `Policy` jest określony, `[Authorize]` używa domyślnej zasady, która domyślnie jest:

* Uwierzytelnieni użytkownicy (zalogowany) jako autoryzowane.
* Brak autoryzacji nieuwierzytelnionych użytkowników (podpisane w poziomie).

## <a name="customize-unauthorized-content-with-the-router-component"></a>Dostosowywanie zawartości nieautoryzowanym ze składnikiem routera

`Router` Składnik umożliwia aplikacji określenie niestandardowej zawartości, jeśli:

* Zawartość nie zostanie znaleziona.
* Użytkownik nie `[Authorize]` warunek zastosowane do składnika. `[Authorize]` Atrybut został omówiony w [atrybutu [Authorize]](#authorize-attribute) sekcji.
* Asynchroniczne uwierzytelniania jest w toku.

W domyślnym szablonie projektu po stronie serwera Blazor *App.razor* plik pokazuje, jak ustawić niestandardową zawartość:

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

Zawartość `<NotFoundContent>`, `<NotAuthorizedContent>`, i `<AuthorizingContent>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.

Jeśli `<NotAuthorizedContent>` nie jest określona, router używa rezerwowej następujący komunikat:

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a>Powiadomienie o zmianach stanu uwierzytelniania

Jeśli aplikacja okaże się, że danych bazowych stanu uwierzytelniania został zmieniony (na przykład, ponieważ zmiany ich ról użytkownika wylogowany lub innego użytkownika), niestandardowe `AuthenticationStateProvider` Opcjonalnie można wywołać metody `NotifyAuthenticationStateChanged` na `AuthenticationStateProvider` podstawowy Klasa. To powiadamia klientów uwierzytelniania danych o stanie (na przykład `AuthorizeView`) do rerender przy użyciu nowych danych.

## <a name="procedural-logic"></a>Proceduralne logiki

Jeśli aplikacja jest wymagany do sprawdzenia reguł autoryzacji jako część logiki proceduralne, użyj parametru kaskadowy typu `Task<AuthenticationState>` uzyskać użytkownika <xref:System.Security.Claims.ClaimsPrincipal>. `Task<AuthenticationState>` można łączyć z innymi usługami, takie jak `IAuthorizationService`, aby oceniać zasady.

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

## <a name="authorization-in-blazor-client-side-apps"></a>Autoryzacja w aplikacji po stronie klienta Blazor

Blazor aplikacji po stronie klienta można pominąć sprawdzanie autoryzacji, ponieważ cały kod po stronie klienta może być modyfikowane przez użytkowników. Dotyczy to także wszystkie technologie aplikacji po stronie klienta, w tym struktur JavaScript SPA lub natywne aplikacje dla dowolnego systemu operacyjnego.

**Zawsze wykonują sprawdzanie autoryzacji na serwerze w ramach żadnych punktów końcowych interfejsu API dostępne dla aplikacji po stronie klienta.**

## <a name="troubleshoot-errors"></a>Rozwiązywanie problemów z błędami

Typowe błędy:

* **Wymaga kaskadowych parametr typu zadania\<AuthenticationState >. Należy rozważyć użycie CascadingAuthenticationState je podać.**

* **`null` wartość jest odbierane dla `authenticationStateTask`**

Istnieje prawdopodobieństwo, że projekt nie został utworzony przy użyciu szablonu po stronie serwera Blazor z włączone uwierzytelnianie. OPAKOWYWANIE `<CascadingAuthenticationState>` wokół część drzewo interfejsu użytkownika, na przykład w *App.razor* w następujący sposób:

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

`CascadingAuthenticationState` Dostarcza `Task<AuthenticationState>` kaskadowych parametr, który z kolei otrzymuje z bazowego `AuthenticationStateProvider` DI usługi.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/index>
* <xref:security/authentication/windowsauth>
