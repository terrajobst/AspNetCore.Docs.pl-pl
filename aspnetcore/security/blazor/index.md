---
title: ASP.NET Core uwierzytelnianie i autoryzacja Blazor
author: guardrex
description: Dowiedz się więcej na temat scenariuszy uwierzytelniania Blazor i autoryzacji.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: security/blazor/index
ms.openlocfilehash: 2ba7b0612c2be50ae0797c50dc3cb0d63c0f0c2d
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800511"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a>ASP.NET Core uwierzytelnianie i autoryzacja Blazor

[Steve Sanderson](https://github.com/SteveSandersonMS)

ASP.NET Core obsługuje konfigurację i zarządzanie zabezpieczeniami w aplikacjach Blazor.

Scenariusze zabezpieczeń różnią się w zależności od Blazor po stronie serwera i aplikacji po stronie klienta. Ponieważ Blazor aplikacje po stronie serwera są uruchamiane na serwerze, sprawdzenia autoryzacji mogą określić:

* Opcje interfejsu użytkownika wyświetlane użytkownikowi (na przykład, które pozycje menu są dostępne dla użytkownika).
* Reguły dostępu do obszarów aplikacji i składników.

Blazor aplikacje po stronie klienta są uruchamiane na kliencie. Autoryzacja jest używana *tylko* do określenia opcji interfejsu użytkownika, które mają być wyświetlane. Ponieważ sprawdzenia po stronie klienta mogą być modyfikowane lub pomijane przez użytkownika, aplikacja po stronie klienta Blazor nie może wymusić reguł dostępu autoryzacji.

## <a name="authentication"></a>Uwierzytelnianie

Blazor używa istniejących mechanizmów uwierzytelniania ASP.NET Core do ustanowienia tożsamości użytkownika. Dokładny mechanizm zależy od tego, jak aplikacja Blazor jest hostowana, po stronie serwera lub po stronie klienta.

### <a name="blazor-server-side-authentication"></a>Blazor uwierzytelnianie po stronie serwera

Blazor aplikacje po stronie serwera działają za pośrednictwem połączenia w czasie rzeczywistym, które zostało utworzone za pomocą usługi Sygnalizującer. [Uwierzytelnianie w aplikacjach opartych na sygnalizacji](xref:signalr/authn-and-authz) jest obsługiwane po nawiązaniu połączenia. Uwierzytelnianie może opierać się na pliku cookie lub innym tokenie okaziciela.

Szablon projektu po stronie serwera Blazor może skonfigurować uwierzytelnianie dla Ciebie podczas tworzenia projektu.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Postępuj zgodnie ze wskazówkami programu <xref:blazor/get-started> Visual Studio w artykule, aby utworzyć nowy projekt po stronie serwera Blazor z mechanizmem uwierzytelniania.

Po wybraniu szablonu **aplikacji Blazor Server** w oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** wybierz pozycję **Zmień** w obszarze **uwierzytelnianie**.

Zostanie otwarte okno dialogowe z zaoferowaniem tego samego zestawu mechanizmów uwierzytelniania dostępnych dla innych projektów ASP.NET Core:

* **Bez uwierzytelniania**
* **Indywidualne konta użytkowników** &ndash; Konta użytkowników mogą być przechowywane:
  * W aplikacji korzystającej z systemu [tożsamości](xref:security/authentication/identity) ASP.NET Core.
  * Z [Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* **Konta służbowe**
* **Uwierzytelnianie systemu Windows**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Postępuj zgodnie ze wskazówkami <xref:blazor/get-started> Visual Studio Code w artykule, aby utworzyć nowy projekt po stronie serwera Blazor z mechanizmem uwierzytelniania:

```console
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

W poniższej tabeli przedstawiono`{AUTHENTICATION}`dozwolone wartości uwierzytelniania ().

| Mechanizm uwierzytelniania                                                                 | `{AUTHENTICATION}`wartościami |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| Bez uwierzytelniania                                                                        | `None`                   |
| Szczegółowe<br>Użytkownicy przechowywani w aplikacji z tożsamością ASP.NET Core.                        | `Individual`             |
| Szczegółowe<br>Użytkownicy przechowywani w [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Konta służbowe<br>Uwierzytelnianie organizacyjne dla pojedynczej dzierżawy.            | `SingleOrg`              |
| Konta służbowe<br>Uwierzytelnianie organizacyjne dla wielu dzierżawców.           | `MultiOrg`               |
| Uwierzytelnianie systemu Windows                                                                   | `Windows`                |

Polecenie tworzy folder o nazwie przy użyciu wartości podanej dla `{APP NAME}` symbolu zastępczego i używa nazwy folderu jako nazwy aplikacji. Aby uzyskać więcej informacji, zobacz polecenie [dotnet New](/dotnet/core/tools/dotnet-new) w przewodniku .NET Core.

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

### <a name="blazor-client-side-authentication"></a>Blazor uwierzytelnianie po stronie klienta

W aplikacjach po stronie klienta Blazor sprawdzanie uwierzytelniania można obejść, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników. Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.

Implementacja niestandardowej `AuthenticationStateProvider` usługi dla aplikacji po stronie klienta Blazor została omówiona w poniższych sekcjach.

## <a name="authenticationstateprovider-service"></a>Usługa AuthenticationStateProvider

Blazor aplikacje po stronie serwera obejmują wbudowaną `AuthenticationStateProvider` usługę, która pobiera dane stanu uwierzytelniania z `HttpContext.User`ASP.NET Core. Jest to sposób integracji stanu uwierzytelniania z istniejącymi mechanizmami uwierzytelniania po stronie serwera ASP.NET Core.

`AuthenticationStateProvider`to podstawowa usługa używana przez `AuthorizeView` składnik i `CascadingAuthenticationState` składnik do uzyskiwania stanu uwierzytelniania.

Zwykle nie są używane `AuthenticationStateProvider` bezpośrednio. Użyj [składnika AuthorizeView](#authorizeview-component) lub podejścia [do<AuthenticationState> zadań](#expose-the-authentication-state-as-a-cascading-parameter) opisanych w dalszej części tego artykułu. Główną wadą zwrotu z używania `AuthenticationStateProvider` bezpośrednio jest to, że składnik nie jest automatycznie powiadamiany o zmianach danych stanu uwierzytelniania.

Usługa może udostępniać <xref:System.Security.Claims.ClaimsPrincipal> dane bieżącego użytkownika, jak pokazano w następującym przykładzie: `AuthenticationStateProvider`

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

Jeśli `user.Identity.IsAuthenticated` jest `true` i<xref:System.Security.Claims.ClaimsPrincipal>ponieważ użytkownik jest, można wyliczyć oświadczenia i członkostwo w rolach.

Aby uzyskać więcej informacji na temat iniekcji zależności (di) i <xref:blazor/dependency-injection> usług <xref:fundamentals/dependency-injection>, zobacz i.

## <a name="implement-a-custom-authenticationstateprovider"></a>Implementowanie niestandardowego AuthenticationStateProvider

Jeśli tworzysz aplikację po stronie klienta Blazor lub jeśli Specyfikacja aplikacji absolutnie wymaga dostawcy niestandardowego, zaimplementuj dostawcę i Przesłoń `GetAuthenticationStateAsync`:

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

Usługa jest zarejestrowana w `Startup.ConfigureServices`: `CustomAuthStateProvider`

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
}
```

Korzystając z `CustomAuthStateProvider`programu, wszyscy użytkownicy są uwierzytelniani przy `mrfibuli`użyciu nazwy użytkownika.

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

Jeśli `user.Identity.IsAuthenticated` jest`true`, oświadczenia mogą być wyliczane i członkostwo w rolach oceniane.

Skonfiguruj parametr `AuthorizeRouteView`kaskadowy przy użyciu składników i `CascadingAuthenticationState`: `Task<AuthenticationState>`

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

`AuthorizeView` Składnik selektywnie wyświetla interfejs użytkownika w zależności od tego, czy użytkownik jest uprawniony do jego wyświetlania. Takie podejście jest przydatne, gdy wystarczy *wyświetlić* dane dla użytkownika i nie trzeba używać tożsamości użytkownika w logice proceduralnej.

Składnik uwidacznia `context` zmienną typu `AuthenticationState`, za pomocą której można uzyskać dostęp do informacji o zalogowanym użytkowniku:

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

Zawartość `<Authorized>` i`<NotAuthorized>` Tagi mogą zawierać dowolne elementy, takie jak inne składniki interaktywne.

Warunki autoryzacji, takie jak role lub zasady kontrolujące opcje interfejsu użytkownika lub dostęp, są omówione w sekcji [autoryzacja](#authorization) .

Jeśli warunki autoryzacji nie są określone `AuthorizeView` , program używa domyślnych zasad i traktuje je:

* Uwierzytelniony (zalogowany) Użytkownicy jako autoryzowany.
* Nieuwierzytelnionych (wylogowanych) użytkowników jako nieautoryzowanych.

### <a name="role-based-and-policy-based-authorization"></a>Autoryzacja oparta na rolach i zasadach

Składnik obsługuje autoryzację opartą *na rolach* lub *zasadach.* `AuthorizeView`

W przypadku autoryzacji opartej na rolach Użyj `Roles` parametru:

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

Aby uzyskać więcej informacji, zobacz <xref:security/authorization/roles>.

W przypadku autoryzacji opartej na zasadach należy `Policy` użyć parametru:

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

Autoryzacja oparta na oświadczeniach jest specjalnym przypadkiem autoryzacji opartej na zasadach. Na przykład można zdefiniować zasady, które wymagają, aby użytkownicy mieli pewne wnioski. Aby uzyskać więcej informacji, zobacz <xref:security/authorization/policies>.

Te interfejsy API mogą być używane w aplikacjach Blazor po stronie serwera lub klienta Blazor.

Jeśli ani nie `AuthorizeView` zostanie określony, program używa domyślnych zasad. `Policy` `Roles`

### <a name="content-displayed-during-asynchronous-authentication"></a>Zawartość wyświetlana podczas uwierzytelniania asynchronicznego

Blazor umożliwia określenie stanu uwierzytelniania w sposób *asynchroniczny*. Głównym scenariuszem tego podejścia jest Blazor aplikacji po stronie klienta, które składają żądanie do zewnętrznego punktu końcowego w celu uwierzytelnienia.

Gdy trwa uwierzytelnianie, `AuthorizeView` domyślnie nie jest wyświetlana żadna zawartość. Aby wyświetlić zawartość podczas uwierzytelniania, należy użyć `<Authorizing>` elementu:

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

Takie podejście nie ma zwykle zastosowania do Blazor aplikacji po stronie serwera. Blazor aplikacje po stronie serwera informują o stanie uwierzytelniania zaraz po ustanowieniu stanu. `Authorizing`zawartość można podać w `AuthorizeView` składniku aplikacji po stronie serwera Blazor, ale zawartość nigdy nie jest wyświetlana.

## <a name="authorize-attribute"></a>[Autoryzuj] — atrybut

Podobnie jak aplikacja może `[Authorize]` być używana z kontrolerem MVC lub stroną Razor, `[Authorize]` może być również używana z składnikami Razor:

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> Używać `[Authorize]` tylko dla `@page` składników uzyskanych za pośrednictwem routera Blazor. Autoryzacja jest wykonywana tylko jako aspekt routingu, a *nie* dla składników podrzędnych renderowanych na stronie. Aby autoryzować wyświetlanie określonych części na stronie, użyj `AuthorizeView` zamiast tego.

Może być konieczne dodanie `@using Microsoft.AspNetCore.Authorization` elementu do składnika lub do pliku *_Imports. Razor* w celu skompilowania składnika.

Ten `[Authorize]` atrybut obsługuje również autoryzację opartą na rolach lub zasadach. W przypadku autoryzacji opartej na rolach Użyj `Roles` parametru:

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

W przypadku autoryzacji opartej na zasadach należy `Policy` użyć parametru:

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

Jeśli ani nie `[Authorize]` zostanie określony, program używa domyślnych zasad, które domyślnie są traktowane: `Policy` `Roles`

* Uwierzytelniony (zalogowany) Użytkownicy jako autoryzowany.
* Nieuwierzytelnionych (wylogowanych) użytkowników jako nieautoryzowanych.

## <a name="customize-unauthorized-content-with-the-router-component"></a>Dostosowywanie nieautoryzowanej zawartości za pomocą składnika routera

Składnik, w połączeniu `AuthorizeRouteView` z składnikiem, umożliwia aplikacji określenie zawartości niestandardowej, jeśli: `Router`

* Nie znaleziono zawartości.
* Użytkownik nie może `[Authorize]` wykonać warunku zastosowanego do składnika. Ten `[Authorize]` atrybut jest pokryty w sekcji [atrybutu [autoryzuje]](#authorize-attribute) .
* Uwierzytelnianie asynchroniczne jest w toku.

W domyślnym szablonie projektu po stronie serwera Blazor plik *App. Razor* ilustruje sposób ustawiania zawartości niestandardowej:

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

Zawartość `<NotFound>`, `<NotAuthorized>`, i`<Authorizing>` Tagi mogą zawierać dowolne elementy, takie jak inne składniki interaktywne.

Jeśli element nie jest określony, zostanie `AuthorizeRouteView` użyty następujący komunikat rezerwowy: `<NotAuthorized>`

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a>Powiadomienie o zmianach stanu uwierzytelniania

Jeśli aplikacja określi, że dane stanu uwierzytelniania zostały zmienione (na przykład ponieważ użytkownik wylogowany lub inny użytkownik zmienił swoje role), niestandardowe `AuthenticationStateProvider` może opcjonalnie wywołać metodę `NotifyAuthenticationStateChanged` na `AuthenticationStateProvider` podstawie określonej. Spowoduje to powiadomienie klientów o danych stanu uwierzytelniania (na przykład `AuthorizeView`) w celu ponownego renderowania przy użyciu nowych danych.

## <a name="procedural-logic"></a>Logika proceduralna

Jeśli aplikacja jest wymagana do sprawdzenia reguł autoryzacji jako części logiki proceduralnej, należy użyć kaskadowego parametru typu `Task<AuthenticationState>` , aby uzyskać <xref:System.Security.Claims.ClaimsPrincipal>użytkownika. `Task<AuthenticationState>`można łączyć z innymi usługami, takimi jak `IAuthorizationService`, do analizowania zasad.

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

## <a name="authorization-in-blazor-client-side-apps"></a>Autoryzacja w aplikacjach po stronie klienta Blazor

W aplikacjach po stronie klienta Blazor sprawdzanie autoryzacji może być pomijane, ponieważ każdy kod po stronie klienta może być modyfikowany przez użytkowników. Jest to samo prawdziwe dla wszystkich technologii aplikacji po stronie klienta, w tym dla struktur SPA skryptów JavaScript lub natywnych aplikacji dla dowolnego systemu operacyjnego.

**Zawsze sprawdzaj autoryzację na serwerze w ramach dowolnych punktów końcowych interfejsu API, do których uzyskuje dostęp aplikacja po stronie klienta.**

## <a name="troubleshoot-errors"></a>Rozwiązywanie problemów z błędami

Typowe błędy:

* **Autoryzacja wymaga parametru kaskadowego typu Task\<AuthenticationState >. Rozważ użycie CascadingAuthenticationState, aby to zrobić.**

* **`null`Odebrano wartość dla`authenticationStateTask`**

Prawdopodobnie projekt nie został utworzony przy użyciu szablonu po stronie serwera Blazor z włączonym uwierzytelnianiem. Zawiń wokół pewnej części drzewa interfejsu użytkownika, na przykład w *App. Razor* w następujący sposób: `<CascadingAuthenticationState>`

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

Dostarcza parametr kaskadowy, który z kolei otrzymuje od podstawowej `AuthenticationStateProvider` usługi di. `Task<AuthenticationState>` `CascadingAuthenticationState`

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/index>
* <xref:security/blazor/server-side>
* <xref:security/authentication/windowsauth>
