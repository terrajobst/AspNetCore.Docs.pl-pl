---
title: ASP.NET Core uwierzytelnianie i autoryzacja Blazor
author: guardrex
description: Dowiedz się więcej na temat scenariuszy uwierzytelniania Blazor i autoryzacji.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: security/blazor/index
ms.openlocfilehash: 1fcd54e954d09e66b8bb1c9a51ef56193f3acf93
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007435"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a>ASP.NET Core uwierzytelnianie i autoryzacja Blazor

[Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

ASP.NET Core obsługuje konfigurację i zarządzanie zabezpieczeniami w aplikacjach Blazor.

Scenariusze zabezpieczeń różnią się w zależności od aplikacji Blazor Server i Blazor webassembly. Ponieważ aplikacje serwera Blazor są uruchamiane na serwerze, sprawdzenia autoryzacji mogą określić:

* Opcje interfejsu użytkownika wyświetlane użytkownikowi (na przykład, które pozycje menu są dostępne dla użytkownika).
* Reguły dostępu do obszarów aplikacji i składników.

Blazor aplikacje webassembly są uruchamiane na kliencie. Autoryzacja jest używana *tylko* do określenia opcji interfejsu użytkownika, które mają być wyświetlane. Ponieważ sprawdzenia po stronie klienta mogą być modyfikowane lub pomijane przez użytkownika, aplikacja Blazor webassembly nie może wymusić reguł dostępu autoryzacji.

## <a name="authentication"></a>Authentication

Blazor używa istniejących mechanizmów uwierzytelniania ASP.NET Core do ustanowienia tożsamości użytkownika. Dokładny mechanizm zależy od tego, w jaki sposób aplikacja Blazor jest hostowana, Blazor Server lub Blazor webassembly.

### <a name="blazor-server-authentication"></a>Uwierzytelnianie serwera Blazor

Aplikacje serwera Blazor działają przez połączenie w czasie rzeczywistym, które zostało utworzone za pomocą usługi Sygnalizującer. [Uwierzytelnianie w aplikacjach opartych na sygnalizacji](xref:signalr/authn-and-authz) jest obsługiwane po nawiązaniu połączenia. Uwierzytelnianie może opierać się na pliku cookie lub innym tokenie okaziciela.

Szablon projektu serwera Blazor może skonfigurować uwierzytelnianie dla Ciebie podczas tworzenia projektu.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Postępuj zgodnie ze wskazówkami programu Visual Studio w artykule <xref:blazor/get-started>, aby utworzyć nowy projekt serwera Blazor z mechanizmem uwierzytelniania.

Po wybraniu szablonu **aplikacji Blazor Server** w oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** wybierz pozycję **Zmień** w obszarze **uwierzytelnianie**.

Zostanie otwarte okno dialogowe z zaoferowaniem tego samego zestawu mechanizmów uwierzytelniania dostępnych dla innych projektów ASP.NET Core:

* **Bez uwierzytelniania**
* Konta użytkowników **poszczególnych użytkowników** &ndash; mogą być przechowywane:
  * W aplikacji korzystającej z systemu [tożsamości](xref:security/authentication/identity) ASP.NET Core.
  * Z [Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* **Konta służbowe**
* **Uwierzytelnianie systemu Windows**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Postępuj zgodnie ze wskazówkami Visual Studio Code w artykule <xref:blazor/get-started>, aby utworzyć nowy projekt serwera Blazor z mechanizmem uwierzytelniania:

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

W poniższej tabeli przedstawiono dozwolone wartości uwierzytelniania (`{AUTHENTICATION}`).

| Mechanizm uwierzytelniania                                                                 | wartość `{AUTHENTICATION}` |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| Bez uwierzytelniania                                                                        | `None`                   |
| Szczegółowe<br>Użytkownicy przechowywani w aplikacji z tożsamością ASP.NET Core.                        | `Individual`             |
| Szczegółowe<br>Użytkownicy przechowywani w [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Konta służbowe<br>Uwierzytelnianie organizacyjne dla pojedynczej dzierżawy.            | `SingleOrg`              |
| Konta służbowe<br>Uwierzytelnianie organizacyjne dla wielu dzierżawców.           | `MultiOrg`               |
| Uwierzytelnianie systemu Windows                                                                   | `Windows`                |

Polecenie tworzy folder o nazwie z wartością podaną dla symbolu zastępczego `{APP NAME}` i używa nazwy folderu jako nazwy aplikacji. Aby uzyskać więcej informacji, zobacz polecenie [dotnet New](/dotnet/core/tools/dotnet-new) w przewodniku .NET Core.

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

### <a name="blazor-webassembly-authentication"></a>Blazor uwierzytelniania webassembly

W aplikacjach Blazor webassembly sprawdzanie uwierzytelniania można obejść, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników. Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.

Dodaj odwołanie do pakietu dla elementu [Microsoft. AspNetCore. Components. Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) do pliku projektu aplikacji.

Implementacja niestandardowej usługi `AuthenticationStateProvider` dla aplikacji Blazor webassembly została omówiona w poniższych sekcjach.

## <a name="authenticationstateprovider-service"></a>Usługa AuthenticationStateProvider

Aplikacje serwera Blazor obejmują wbudowaną usługę `AuthenticationStateProvider`, która uzyskuje dane stanu uwierzytelniania ASP.NET Core `HttpContext.User`. Jest to sposób integracji stanu uwierzytelniania z istniejącymi mechanizmami uwierzytelniania po stronie serwera ASP.NET Core.

`AuthenticationStateProvider` to podstawowa usługa używana przez składnik `AuthorizeView` i składnik `CascadingAuthenticationState` do uzyskania stanu uwierzytelniania.

Nie używasz zazwyczaj `AuthenticationStateProvider` bezpośrednio. Użyj podejść [składnika AuthorizeView](#authorizeview-component) lub [zadania @ no__t-2](#expose-the-authentication-state-as-a-cascading-parameter) opisane w dalszej części tego artykułu. Główną wadą do korzystania z `AuthenticationStateProvider` bezpośrednio jest to, że składnik nie jest automatycznie powiadamiany o zmianach danych stanu uwierzytelniania.

Usługa `AuthenticationStateProvider` może udostępniać dane <xref:System.Security.Claims.ClaimsPrincipal> bieżącego użytkownika, jak pokazano w następującym przykładzie:

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

Jeśli `user.Identity.IsAuthenticated` jest `true` i ponieważ użytkownik jest <xref:System.Security.Claims.ClaimsPrincipal>, można wyliczyć oświadczenia i członkostwo w rolach.

Aby uzyskać więcej informacji na temat iniekcji zależności (DI) i usług, zobacz <xref:blazor/dependency-injection> i <xref:fundamentals/dependency-injection>.

## <a name="implement-a-custom-authenticationstateprovider"></a>Implementowanie niestandardowego AuthenticationStateProvider

Jeśli tworzysz aplikację webassembly Blazor lub jeśli Specyfikacja aplikacji absolutnie wymaga dostawcy niestandardowego, zaimplementuj dostawcę i Przesłoń `GetAuthenticationStateAsync`:

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

Usługa `CustomAuthStateProvider` jest zarejestrowana w `Startup.ConfigureServices`:

```csharp
// using Microsoft.AspNetCore.Components.Authorization;
// using BlazorSample.Services;

services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

Przy użyciu `CustomAuthStateProvider` wszyscy użytkownicy są uwierzytelniani przy użyciu nazwy użytkownika `mrfibuli`.

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a>Uwidacznianie stanu uwierzytelniania jako parametru kaskadowego

Jeśli dane stanu uwierzytelniania są wymagane dla logiki proceduralnej, na przykład podczas wykonywania akcji wyzwalanej przez użytkownika, uzyskaj dane stanu uwierzytelniania przez zdefiniowanie parametru kaskadowego typu `Task<AuthenticationState>`:

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
> W składniku aplikacji webassembly Blazor Dodaj przestrzeń nazw `Microsoft.AspNetCore.Components.Authorization` (`@using Microsoft.AspNetCore.Components.Authorization`).

Jeśli `user.Identity.IsAuthenticated` to `true`, można wyliczyć oświadczenia i członkostwo w rolach.

Skonfiguruj parametr kaskadowy `Task<AuthenticationState>` przy użyciu składników `AuthorizeRouteView` i `CascadingAuthenticationState`:

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

## <a name="authorization"></a>Authorization

Po uwierzytelnieniu użytkownika są stosowane reguły *autoryzacji* umożliwiające kontrolę działania użytkownika.

Dostęp jest zazwyczaj udzielany lub odrzucany w zależności od tego, czy:

* Użytkownik jest uwierzytelniany (zalogowany).
* Użytkownik należy do *roli*.
* Użytkownik ma *wierzytelność*.
* *Zasady* są spełnione.

Każda z tych koncepcji jest taka sama jak w aplikacji ASP.NET Core MVC lub Razor Pages. Aby uzyskać więcej informacji na temat zabezpieczeń ASP.NET Core, zapoznaj się z artykułami w obszarze [ASP.NET Core zabezpieczenia i tożsamość](xref:security/index).

## <a name="authorizeview-component"></a>Składnik AuthorizeView

Składnik `AuthorizeView` selektywnie wyświetla interfejs użytkownika w zależności od tego, czy użytkownik jest uprawniony do jego wyświetlania. Takie podejście jest przydatne, gdy wystarczy *wyświetlić* dane dla użytkownika i nie trzeba używać tożsamości użytkownika w logice proceduralnej.

Składnik uwidacznia zmienną `context` typu `AuthenticationState`, za pomocą której można uzyskać dostęp do informacji o zalogowanym użytkowniku:

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

Jeśli użytkownik nie jest uwierzytelniony, można również podać inną zawartość do wyświetlenia:

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

Zawartość tagów `<Authorized>` i `<NotAuthorized>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.

Warunki autoryzacji, takie jak role lub zasady kontrolujące opcje interfejsu użytkownika lub dostęp, są omówione w sekcji [autoryzacja](#authorization) .

Jeśli warunki autoryzacji nie są określone, `AuthorizeView` korzysta z zasad domyślnych i traktuje je:

* Uwierzytelniony (zalogowany) Użytkownicy jako autoryzowany.
* Nieuwierzytelnionych (wylogowanych) użytkowników jako nieautoryzowanych.

### <a name="role-based-and-policy-based-authorization"></a>Autoryzacja oparta na rolach i zasadach

Składnik `AuthorizeView` obsługuje autoryzację opartą *na rolach* lub *zasadach* .

W przypadku autoryzacji opartej na rolach Użyj parametru `Roles`:

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

Aby uzyskać więcej informacji, zobacz <xref:security/authorization/roles>.

W przypadku autoryzacji opartej na zasadach należy użyć parametru `Policy`:

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

Autoryzacja oparta na oświadczeniach jest specjalnym przypadkiem autoryzacji opartej na zasadach. Na przykład można zdefiniować zasady, które wymagają, aby użytkownicy mieli pewne wnioski. Aby uzyskać więcej informacji, zobacz <xref:security/authorization/policies>.

Te interfejsy API mogą być używane w aplikacjach Blazor Server lub Blazor webassembly.

Jeśli nie określono żadnego `Roles` ani `Policy`, `AuthorizeView` używa zasad domyślnych.

### <a name="content-displayed-during-asynchronous-authentication"></a>Zawartość wyświetlana podczas uwierzytelniania asynchronicznego

Blazor umożliwia określenie stanu uwierzytelniania w sposób *asynchroniczny*. Głównym scenariuszem tego podejścia jest Blazor aplikacje webassembly, które składają żądanie do zewnętrznego punktu końcowego w celu uwierzytelnienia.

Gdy trwa uwierzytelnianie, `AuthorizeView` domyślnie nie wyświetla żadnej zawartości. Aby wyświetlić zawartość podczas uwierzytelniania, należy użyć elementu `<Authorizing>`:

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

Takie podejście nie ma zwykle zastosowania do aplikacji serwera Blazor. Aplikacje serwera Blazor wiedzą o stanie uwierzytelniania zaraz po ustanowieniu stanu. zawartość `Authorizing` można podać w składniku `AuthorizeView` aplikacji Blazor Server, ale zawartość nigdy nie jest wyświetlana.

## <a name="authorize-attribute"></a>[Autoryzuj] — atrybut

Atrybut `[Authorize]` może być używany w składnikach Razor:

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> W składniku aplikacji webassembly Blazor Dodaj przestrzeń nazw `Microsoft.AspNetCore.Authorization` (`@using Microsoft.AspNetCore.Authorization`) do przykładów w tej sekcji.

> [!IMPORTANT]
> Za pośrednictwem routera Blazor należy używać tylko `[Authorize]` na składnikach `@page`. Autoryzacja jest wykonywana tylko jako aspekt routingu, a *nie* dla składników podrzędnych renderowanych na stronie. Aby autoryzować wyświetlanie określonych części na stronie, należy zamiast tego użyć `AuthorizeView`.

Atrybut `[Authorize]` obsługuje również autoryzację opartą na rolach lub zasadach. W przypadku autoryzacji opartej na rolach Użyj parametru `Roles`:

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

W przypadku autoryzacji opartej na zasadach należy użyć parametru `Policy`:

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

Jeśli nie określono żadnego `Roles` ani `Policy`, `[Authorize]` używa domyślnych zasad, które domyślnie są traktowane:

* Uwierzytelniony (zalogowany) Użytkownicy jako autoryzowany.
* Nieuwierzytelnionych (wylogowanych) użytkowników jako nieautoryzowanych.

## <a name="customize-unauthorized-content-with-the-router-component"></a>Dostosowywanie nieautoryzowanej zawartości za pomocą składnika routera

Składnik `Router`, w połączeniu ze składnikiem `AuthorizeRouteView`, umożliwia aplikacji określenie zawartości niestandardowej, jeśli:

* Nie znaleziono zawartości.
* Użytkownik nie może wykonać warunku `[Authorize]` zastosowanego do składnika. Atrybut `[Authorize]` znajduje się w sekcji [atrybutu [autoryzuje]](#authorize-attribute) .
* Uwierzytelnianie asynchroniczne jest w toku.

W domyślnym szablonie projektu serwera Blazor plik *App. Razor* ilustruje sposób ustawiania zawartości niestandardowej:

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

Zawartość tagów `<NotFound>`, `<NotAuthorized>` i `<Authorizing>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.

Jeśli nie określono elementu `<NotAuthorized>`, `AuthorizeRouteView` używa następującego komunikatu powrotu:

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a>Powiadomienie o zmianach stanu uwierzytelniania

Jeśli aplikacja określi, że dane stanu uwierzytelniania zostały zmienione (na przykład, ponieważ użytkownik wylogowany lub inny użytkownik zmienił swoje role), niestandardowy `AuthenticationStateProvider` opcjonalnie może wywołać metodę `NotifyAuthenticationStateChanged` w klasie podstawowej `AuthenticationStateProvider`. Spowoduje to powiadomienie klientów o danych stanu uwierzytelniania (na przykład `AuthorizeView`) w celu ponownego renderowania przy użyciu nowych danych.

## <a name="procedural-logic"></a>Logika proceduralna

Jeśli aplikacja jest wymagana do sprawdzenia reguł autoryzacji jako części logiki proceduralnej, należy użyć kaskadowego parametru typu `Task<AuthenticationState>`, aby uzyskać @no__t użytkownika-1. `Task<AuthenticationState>` można łączyć z innymi usługami, takimi jak `IAuthorizationService`, w celu ocenienia zasad.

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
> W składniku aplikacji webassembly Blazor Dodaj przestrzenie nazw `Microsoft.AspNetCore.Authorization` i `Microsoft.AspNetCore.Components.Authorization`:
>
> ```cshtml
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-blazor-webassembly-apps"></a>Autoryzacja w aplikacjach webassembly Blazor

W aplikacjach webassembly Blazor można ominąć sprawdzanie autoryzacji, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników. Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.

**Zawsze sprawdzaj autoryzację na serwerze w ramach dowolnych punktów końcowych interfejsu API, do których uzyskuje dostęp aplikacja po stronie klienta.**

## <a name="troubleshoot-errors"></a>Rozwiązywanie problemów z błędami

Typowe błędy:

* **Authorization wymaga parametru kaskadowego typu Task @ no__t-1AuthenticationState >. Rozważ użycie CascadingAuthenticationState do dostarczenia tego.**

* **Odebrano wartość `null` dla `authenticationStateTask`**

Prawdopodobnie projekt nie został utworzony przy użyciu szablonu serwera Blazor z włączonym uwierzytelnianiem. Zawiń `<CascadingAuthenticationState>` wokół pewnej części drzewa interfejsu użytkownika, na przykład w *App. Razor* w następujący sposób:

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

@No__t-0 dostarcza parametr kaskadowy `Task<AuthenticationState>`, który z kolei otrzymuje od podstawowej `AuthenticationStateProvider` DI usługi.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
