---
title: ASP.NET Core Blazor uwierzytelniania i autoryzacji
author: guardrex
description: Dowiedz się więcej na temat Blazor scenariuszy uwierzytelniania i autoryzacji.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: 693ac1a5b5bcaf8a9bbf0ff9ab63fb41764e3888
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880452"
---
# <a name="aspnet-core-opno-locblazor-authentication-and-authorization"></a>ASP.NET Core Blazor uwierzytelniania i autoryzacji

[Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

ASP.NET Core obsługuje Konfigurowanie zabezpieczeń w aplikacjach Blazor i zarządzanie nimi.

Scenariusze zabezpieczeń różnią się między Blazor Server i Blazor aplikacjami webassembly. Ponieważ aplikacje serwera Blazor są uruchamiane na serwerze, sprawdzenia autoryzacji mogą określić:

* Opcje interfejsu użytkownika wyświetlane użytkownikowi (na przykład, które pozycje menu są dostępne dla użytkownika).
* Reguły dostępu do obszarów aplikacji i składników.

Blazor aplikacje webassembly działają na kliencie. Autoryzacja jest używana *tylko* do określenia opcji interfejsu użytkownika, które mają być wyświetlane. Ponieważ sprawdzenia po stronie klienta mogą być modyfikowane lub pomijane przez użytkownika, aplikacja Blazor webassembly nie może wymusić reguł dostępu autoryzacji.

## <a name="authentication"></a>Uwierzytelnianie

Blazor używa istniejących mechanizmów uwierzytelniania ASP.NET Core do ustanowienia tożsamości użytkownika. Dokładny mechanizm zależy od tego, w jaki sposób aplikacja Blazor jest hostowana, Blazor Server lub Blazor webassembly.

### <a name="opno-locblazor-server-authentication"></a>Uwierzytelnianie serwera Blazor

aplikacje serwera Blazor działają za pośrednictwem połączenia w czasie rzeczywistym, które zostało utworzone przy użyciu SignalR. Podczas ustanawiania połączenia obsługiwane jest [uwierzytelnianie w aplikacjach opartych na SignalR](xref:signalr/authn-and-authz) . Uwierzytelnianie może opierać się na pliku cookie lub innym tokenie okaziciela.

Szablon projektu serwera Blazor może skonfigurować uwierzytelnianie dla Ciebie podczas tworzenia projektu.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Postępuj zgodnie ze wskazówkami programu Visual Studio w artykule <xref:blazor/get-started>, aby utworzyć nowy projekt Blazor Server z mechanizmem uwierzytelniania.

Po wybraniu szablonu **aplikacjiBlazor Server** w oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** wybierz pozycję **Zmień** w obszarze **uwierzytelnianie**.

Zostanie otwarte okno dialogowe z zaoferowaniem tego samego zestawu mechanizmów uwierzytelniania dostępnych dla innych projektów ASP.NET Core:

* **Bez uwierzytelniania**
* Konta **poszczególnych użytkowników** &ndash; kontach użytkowników mogą być przechowywane:
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
| Pojedyncze<br>Użytkownicy przechowywani w aplikacji z tożsamością ASP.NET Core.                        | `Individual`             |
| Pojedyncze<br>Użytkownicy przechowywani w [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Konta służbowe<br>Uwierzytelnianie organizacyjne dla pojedynczej dzierżawy.            | `SingleOrg`              |
| Konta służbowe<br>Uwierzytelnianie organizacyjne dla wielu dzierżawców.           | `MultiOrg`               |
| Uwierzytelnianie systemu Windows                                                                   | `Windows`                |

Polecenie tworzy folder o nazwie przy użyciu wartości podanej dla symbolu zastępczego `{APP NAME}` i używa nazwy folderu jako nazwy aplikacji. Aby uzyskać więcej informacji, zobacz polecenie [dotnet New](/dotnet/core/tools/dotnet-new) w przewodniku .NET Core.

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

### <a name="opno-locblazor-webassembly-authentication"></a>Blazor uwierzytelnianie zestawu webassembly

W Blazor aplikacjach webassembly sprawdzanie uwierzytelniania można obejść, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników. Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.

Dodaj odwołanie do pakietu dla elementu [Microsoft. AspNetCore. Components. Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) do pliku projektu aplikacji.

Implementacja niestandardowej usługi `AuthenticationStateProvider` dla aplikacji Blazor webassembly została omówiona w poniższych sekcjach.

## <a name="authenticationstateprovider-service"></a>Usługa AuthenticationStateProvider

aplikacje serwera Blazor zawierają wbudowaną usługę `AuthenticationStateProvider`, która pobiera dane stanu uwierzytelniania z `HttpContext.User`ASP.NET Core. Jest to sposób integracji stanu uwierzytelniania z istniejącymi mechanizmami uwierzytelniania po stronie serwera ASP.NET Core.

`AuthenticationStateProvider` to podstawowa usługa używana przez składnik `AuthorizeView` i składnik `CascadingAuthenticationState` do uzyskania stanu uwierzytelniania.

Nie korzystasz zwykle `AuthenticationStateProvider` bezpośrednio z programu. Użyj [AuthorizeView składnika](#authorizeview-component) lub [zadania<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) podejścia opisane w dalszej części tego artykułu. Główną wadą do korzystania z `AuthenticationStateProvider` bezpośrednio jest to, że składnik nie jest automatycznie powiadamiany o zmianach danych stanu uwierzytelniania.

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

Jeśli `user.Identity.IsAuthenticated` jest `true` i ponieważ użytkownik jest <xref:System.Security.Claims.ClaimsPrincipal>, oświadczenia mogą być wyliczane i członkostwo w rolach oceniane.

Aby uzyskać więcej informacji na temat iniekcji zależności (DI) i usług, zobacz <xref:blazor/dependency-injection> i <xref:fundamentals/dependency-injection>.

## <a name="implement-a-custom-authenticationstateprovider"></a>Implementowanie niestandardowego AuthenticationStateProvider

Jeśli tworzysz aplikację Blazor webassembly lub jeśli Specyfikacja aplikacji absolutnie wymaga dostawcy niestandardowego, zaimplementuj dostawcę i Przesłoń `GetAuthenticationStateAsync`:

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

Korzystając z `CustomAuthStateProvider`, wszyscy użytkownicy są uwierzytelniani przy użyciu nazwy użytkownika `mrfibuli`.

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
> W Blazor składnik aplikacji webassembly Dodaj `Microsoft.AspNetCore.Components.Authorization` przestrzeni nazw (`@using Microsoft.AspNetCore.Components.Authorization`).

Jeśli `user.Identity.IsAuthenticated` jest `true`, oświadczenia można wyliczyć i członkostwo w rolach oceniane.

Skonfiguruj `Task<AuthenticationState>` kaskadowy parametr przy użyciu składników `AuthorizeRouteView` i `CascadingAuthenticationState`:

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

## <a name="authorization"></a>Autoryzacja

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

Jeśli warunki autoryzacji nie są określone, `AuthorizeView` używa domyślnych zasad i traktuje je:

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

Aby uzyskać więcej informacji, zobacz temat <xref:security/authorization/roles>.

W przypadku autoryzacji opartej na zasadach Użyj parametru `Policy`:

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

Autoryzacja oparta na oświadczeniach jest specjalnym przypadkiem autoryzacji opartej na zasadach. Na przykład można zdefiniować zasady, które wymagają, aby użytkownicy mieli pewne wnioski. Aby uzyskać więcej informacji, zobacz temat <xref:security/authorization/policies>.

Te interfejsy API mogą być używane na serwerze Blazor lub Blazor aplikacji webassembly.

Jeśli nie `Roles` ani `Policy` jest określony, `AuthorizeView` używa domyślnych zasad.

### <a name="content-displayed-during-asynchronous-authentication"></a>Zawartość wyświetlana podczas uwierzytelniania asynchronicznego

Blazor umożliwia określenie stanu uwierzytelniania *asynchronicznie*. Głównym scenariuszem tego podejścia jest w Blazor aplikacje webassembly, które składają żądanie do zewnętrznego punktu końcowego w celu uwierzytelnienia.

Podczas uwierzytelniania w toku `AuthorizeView` domyślnie nie jest wyświetlana żadna zawartość. Aby wyświetlić zawartość podczas uwierzytelniania, użyj elementu `<Authorizing>`:

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

Takie podejście nie ma zwykle zastosowania do aplikacji Blazor Server. aplikacje serwera Blazor znają stan uwierzytelniania zaraz po ustanowieniu stanu. zawartość `Authorizing` można podać w składniku `AuthorizeView` aplikacji Blazor Server, ale zawartość nigdy nie jest wyświetlana.

## <a name="authorize-attribute"></a>[Autoryzuj] — atrybut

Atrybut `[Authorize]` może być używany w składnikach Razor:

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> W Blazor składnik aplikacji webassembly Dodaj `Microsoft.AspNetCore.Authorization` przestrzeni nazw (`@using Microsoft.AspNetCore.Authorization`) do przykładów w tej sekcji.

> [!IMPORTANT]
> Za pośrednictwem routera Blazor należy używać tylko `[Authorize]` na składnikach `@page`. Autoryzacja jest wykonywana tylko jako aspekt routingu, a *nie* dla składników podrzędnych renderowanych na stronie. Aby autoryzować wyświetlanie określonych części na stronie, należy zamiast tego użyć `AuthorizeView`.

Atrybut `[Authorize]` obsługuje również autoryzację opartą na rolach lub zasadach. W przypadku autoryzacji opartej na rolach Użyj parametru `Roles`:

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

W przypadku autoryzacji opartej na zasadach Użyj parametru `Policy`:

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

Jeśli nie `Roles` ani `Policy` jest określony, `[Authorize]` używa domyślnych zasad, które domyślnie są traktowane:

* Uwierzytelniony (zalogowany) Użytkownicy jako autoryzowany.
* Nieuwierzytelnionych (wylogowanych) użytkowników jako nieautoryzowanych.

## <a name="customize-unauthorized-content-with-the-router-component"></a>Dostosowywanie nieautoryzowanej zawartości za pomocą składnika routera

Składnik `Router`, w połączeniu ze składnikiem `AuthorizeRouteView`, umożliwia aplikacji określenie zawartości niestandardowej, jeśli:

* Nie znaleziono zawartości.
* Do składnika zostanie zastosowany warunek `[Authorize]`. Atrybut `[Authorize]` jest omówiony w sekcji [`[Authorize]` atrybutu](#authorize-attribute) .
* Uwierzytelnianie asynchroniczne jest w toku.

W domyślnym szablonie projektu Blazor Server plik *App. Razor* ilustruje sposób ustawiania zawartości niestandardowej:

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

Zawartość tagów `<NotFound>`, `<NotAuthorized>`i `<Authorizing>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.

Jeśli nie określono elementu `<NotAuthorized>`, `AuthorizeRouteView` używa następującego komunikatu powrotu:

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a>Powiadomienie o zmianach stanu uwierzytelniania

Jeśli aplikacja określi, że dane stanu uwierzytelniania zostały zmienione (na przykład ponieważ użytkownik wylogowany lub inny użytkownik zmienił swoje role), niestandardowa `AuthenticationStateProvider` może opcjonalnie wywołać metodę `NotifyAuthenticationStateChanged` na `AuthenticationStateProvider` klasie bazowej. Spowoduje to powiadomienie klientów o danych stanu uwierzytelniania (na przykład `AuthorizeView`) w celu ponownego renderowania przy użyciu nowych danych.

## <a name="procedural-logic"></a>Logika proceduralna

Jeśli aplikacja jest wymagana do sprawdzenia reguł autoryzacji jako części logiki proceduralnej, należy użyć kaskadowego parametru typu `Task<AuthenticationState>`, aby uzyskać <xref:System.Security.Claims.ClaimsPrincipal>użytkownika. `Task<AuthenticationState>` można łączyć z innymi usługami, takimi jak `IAuthorizationService`, w celu ocenienia zasad.

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
> W Blazor składnik aplikacji webassembly Dodaj przestrzenie nazw `Microsoft.AspNetCore.Authorization` i `Microsoft.AspNetCore.Components.Authorization`:
>
> ```cshtml
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a>Autoryzacja w aplikacjach Blazor webassembly

W programie Blazor aplikacje webassembly można ominąć sprawdzanie autoryzacji, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników. Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.

**Zawsze sprawdzaj autoryzację na serwerze w ramach dowolnych punktów końcowych interfejsu API, do których uzyskuje dostęp aplikacja po stronie klienta.**

## <a name="troubleshoot-errors"></a>Rozwiązywanie problemów

Typowe błędy:

* **Autoryzacja wymaga kaskadowego parametru typu Task\<AuthenticationState >. Rozważ użycie CascadingAuthenticationState, aby to zrobić.**

* **Odebrano `null` wartość dla `authenticationStateTask`**

Prawdopodobnie projekt nie został utworzony przy użyciu szablonu serwera Blazor z włączonym uwierzytelnianiem. Zawiń `<CascadingAuthenticationState>` wokół pewnej części drzewa interfejsu użytkownika, na przykład w *App. Razor* w następujący sposób:

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

`CascadingAuthenticationState` dostarcza `Task<AuthenticationState>` kaskadowego parametru, który z kolei otrzymuje od podstawowej `AuthenticationStateProvider` DI usługi.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
