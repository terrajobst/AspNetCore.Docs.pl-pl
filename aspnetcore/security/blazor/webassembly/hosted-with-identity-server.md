---
title: Zabezpieczanie ASP.NET Core aplikacji hostowanej Blazor webassembly z serwerem tożsamości
author: guardrex
description: Aby utworzyć nową aplikację hostowaną Blazor z uwierzytelnianiem z poziomu programu Visual Studio, który korzysta z zaplecza usługi [IdentityServer](https://identityserver.io/)
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-identity-server
ms.openlocfilehash: a3993bf635e5a7aae408d72796015f2414e13c14
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434476"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-identity-server"></a>Zabezpieczanie ASP.NET Core aplikacji hostowanej Blazor webassembly z serwerem tożsamości

Autorzy [Javier Calvarro Nelson](https://github.com/javiercn) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Aby utworzyć nową aplikację hostowaną Blazor w programie Visual Studio, która używa [IdentityServer](https://identityserver.io/) do uwierzytelniania użytkowników i wywołań interfejsu API:

1. Użyj programu Visual Studio, aby utworzyć nową aplikację **Blazor webassembly** . Aby uzyskać więcej informacji, zobacz <xref:blazor/get-started>.
1. W oknie dialogowym **Tworzenie nowej aplikacji Blazor** wybierz pozycję **Zmień** w sekcji **uwierzytelnianie** .
1. Wybierz **pojedyncze konta użytkowników** , a następnie **przycisk OK**.
1. Zaznacz pole wyboru **hostowane ASP.NET Core** w sekcji **Zaawansowane** .
1. Wybierz przycisk **Utwórz**.

Aby utworzyć aplikację w powłoce poleceń, wykonaj następujące polecenie:

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

Aby określić lokalizację wyjściową, która tworzy folder projektu, jeśli nie istnieje, Uwzględnij opcję Output w poleceniu z ścieżką (na przykład `-o BlazorSample`). Nazwa folderu jest również częścią nazwy projektu.

## <a name="server-app-configuration"></a>Konfiguracja aplikacji serwera

W poniższych sekcjach opisano Dodatki do projektu w przypadku włączenia obsługi uwierzytelniania.

### <a name="startup-class"></a>Klasa początkowa

Klasa `Startup` ma następujące dodatki:

* W pliku `Startup.ConfigureServices`:

  * Tożsamość z domyślnym interfejsem użytkownika:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * IdentityServer z dodatkową metodą pomocniczą <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>, która konfiguruje niektóre domyślne konwencje ASP.NET Core w oparciu o IdentityServer:

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * Uwierzytelnianie za pomocą dodatkowej metody pomocnika <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>, która konfiguruje aplikację do weryfikowania tokenów JWT utworzonych przez IdentityServer:

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* W pliku `Startup.Configure`:

  * Oprogramowanie pośredniczące uwierzytelniania odpowiedzialne za Weryfikowanie poświadczeń żądania i Ustawianie użytkownika w kontekście żądania:

    ```csharp
    app.UseAuthentication();
    ```

  * Oprogramowanie pośredniczące IdentityServer, które uwidacznia punkty końcowe połączenia Open ID Connect (OIDC):

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

Metoda pomocnika <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> konfiguruje [IdentityServer](https://identityserver.io/) dla scenariuszy ASP.NET Core. IdentityServer to zaawansowane i rozszerzalne środowisko do obsługi zagadnień związanych z zabezpieczeniami aplikacji. IdentityServer ujawnia niepotrzebną złożoność w najbardziej typowych scenariuszach. W związku z tym zestaw Konwencji i opcji konfiguracji jest dostępny, ponieważ rozważamy dobry punkt wyjścia. Gdy uwierzytelnianie będzie wymagało zmiany, pełne możliwości IdentityServer są nadal dostępne, aby dostosować uwierzytelnianie zgodnie z wymaganiami aplikacji.

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt

Metoda pomocnika <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> konfiguruje schemat zasad dla aplikacji jako domyślną procedurę obsługi uwierzytelniania. Zasady są skonfigurowane tak, aby zezwalać na tożsamość do obsługi wszystkich żądań kierowanych do dowolnej ścieżki podrzędnej w obszarze adresu URL tożsamości `/Identity`. <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> obsługuje wszystkie inne żądania. Ponadto ta metoda:

* Rejestruje zasób interfejsu API `{APPLICATION NAME}API` z IdentityServer z domyślnym zakresem `{APPLICATION NAME}API`.
* Konfiguruje oprogramowanie pośredniczące tokenu okaziciela JWT do weryfikowania tokenów wystawionych przez IdentityServer dla aplikacji.

### <a name="weatherforecastcontroller"></a>WeatherForecastController

W `WeatherForecastController` (*controllers/WeatherForecastController. cs*) atrybut [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) jest stosowany do klasy. Ten atrybut wskazuje, że użytkownik musi być autoryzowany na podstawie domyślnych zasad dostępu do zasobu. Domyślne zasady autoryzacji są skonfigurowane tak, aby korzystały z domyślnego schematu uwierzytelniania, który jest konfigurowany przez <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> do schematu zasad, który został wymieniony wcześniej. Metoda pomocnika służy do konfigurowania <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> jako domyślnej procedury obsługi żądań do aplikacji.

### <a name="applicationdbcontext"></a>ApplicationDbContext

W `ApplicationDbContext` (*Data/ApplicationDbContext. cs*) ten sam <xref:Microsoft.EntityFrameworkCore.DbContext> jest używany w tożsamości z wyjątkiem, który rozszerza <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> w celu uwzględnienia schematu IdentityServer. <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> pochodzi od <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.

Aby uzyskać pełną kontrolę nad schematem bazy danych, Dziedzicz z jednej z dostępnych <xref:Microsoft.EntityFrameworkCore.DbContext> klas i skonfiguruj kontekst, aby uwzględnić schemat tożsamości przez wywołanie `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` w metodzie `OnModelCreating`.

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController

W `OidcConfigurationController` (*controllers/OidcConfigurationController. cs*) punkt końcowy klienta jest inicjowany do obsługi parametrów OIDC.

### <a name="app-settings-files"></a>Pliki ustawień aplikacji

W sekcji `IdentityServer` pliku ustawień aplikacji (*appSettings. JSON*) w katalogu głównym projektu opisano listę skonfigurowanych klientów. W poniższym przykładzie istnieje pojedynczy klient. Nazwa klienta odpowiada nazwie aplikacji i jest zamapowana według Konwencji do parametru `ClientId` protokołu OAuth. Profil wskazuje konfigurowany typ aplikacji. Profil jest używany wewnętrznie w celu napędu Konwencji upraszczających proces konfiguracji serwera. <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "BlazorApplicationWithAuthentication.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

W pliku ustawień aplikacji środowiska programistycznego (*appSettings. Development. JSON*) w katalogu głównym projektu sekcja `IdentityServer` opisuje klucz używany do podpisywania tokenów. <!-- When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section. -->

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="client-app-configuration"></a>Konfiguracja aplikacji klienta

### <a name="authentication-package"></a>Pakiet uwierzytelniania

Gdy aplikacja zostanie utworzona w celu używania poszczególnych kont użytkowników (`Individual`), aplikacja automatycznie otrzymuje odwołanie do pakietu dla `Microsoft.AspNetCore.Components.WebAssembly.Authentication` pakietu w pliku projektu aplikacji. Pakiet zawiera zestaw elementów podstawowych, które ułatwiają aplikacji uwierzytelnianie użytkowników i uzyskiwanie tokenów do wywoływania chronionych interfejsów API.

W przypadku dodawania uwierzytelniania do aplikacji ręcznie Dodaj pakiet do pliku projektu aplikacji:

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

Zastąp `{VERSION}` w powyższym odwołaniu do pakietu w wersji pakietu `Microsoft.AspNetCore.Blazor.Templates` pokazanej w <xref:blazor/get-started> artykule.

### <a name="api-authorization-support"></a>Obsługa autoryzacji interfejsu API

Obsługa uwierzytelniania użytkowników jest podłączona do kontenera usługi przez metodę rozszerzenia dostarczoną w pakiecie `Microsoft.AspNetCore.Components.WebAssembly.Authentication`. Ta metoda konfiguruje wszystkie usługi, które są konieczne, aby aplikacja mogła współdziałać z istniejącym systemem autoryzacji.

```csharp
builder.Services.AddApiAuthorization();
```

Domyślnie ładuje on konfigurację aplikacji według Konwencji z `_configuration/{client-id}`. Zgodnie z Konwencją identyfikator klienta jest ustawiany na nazwę zestawu aplikacji. Ten adres URL można zmienić tak, aby wskazywał osobny punkt końcowy przez wywołanie przeciążenia z opcjami.

### <a name="index-page"></a>Strona indeksu

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a>Składnik aplikacji

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>Składnik RedirectToLogin

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>Składnik LoginDisplay

Składnik `LoginDisplay` (*Shared/LoginDisplay. Razor*) jest renderowany w składniku `MainLayout` (*Shared/MainLayout. Razor*) i zarządza następującymi zachowaniami:

* Dla uwierzytelnionych użytkowników:
  * Wyświetla bieżącą nazwę użytkownika.
  * Oferuje link do strony profilu użytkownika w ASP.NET Core tożsamość.
  * Oferuje przycisk umożliwiający wylogowanie się z aplikacji.
* Dla użytkowników anonimowych:
  * Oferuje opcję rejestracji.
  * Oferuje opcję logowania.

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        <a href="authentication/profile">Hello, @context.User.Identity.Name!</a>
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/register">Register</a>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```

### <a name="authentication-component"></a>Składnik uwierzytelniania

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>Składnik FetchData

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a>Uruchamianie aplikacji

Uruchom aplikację z projektu serwera. W przypadku korzystania z programu Visual Studio wybierz projekt serwera w **Eksplorator rozwiązań** a następnie wybierz przycisk **Uruchom** na pasku narzędzi lub Uruchom aplikację z menu **Debuguj** .

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
