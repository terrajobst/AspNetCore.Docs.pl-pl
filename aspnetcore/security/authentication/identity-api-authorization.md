---
title: Wprowadzenie do uwierzytelniania dla pojedynczej aplikacji strony programu ASP.NET Core
author: javiercn
description: Użyj tożsamości z jednej strony aplikacji hostowaną wewnątrz aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 4afc9ac0a3c54b452c6a1b23e4de31d7e2fc5284
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665340"
---
# <a name="authentication-and-authorization-for-spas"></a>Uwierzytelnianie i autoryzację dla aplikacji jednostronicowych

Platforma ASP.NET Core w wersji 3.0 lub nowszej oferuje uwierzytelnianie w jednej strony aplikacji (aplikacji jednostronicowych) przy użyciu obsługi interfejsu API autoryzacji. Tożsamości platformy ASP.NET Core, uwierzytelniania i przechowywanie użytkowników jest połączony z [IdentityServer](https://identityserver.io/) implementowania Open ID Connect.

Parametr uwierzytelniania został dodany do **Angular** i **React** projektu szablonów, które są podobne do parametru uwierzytelniania w **aplikacji sieci Web (Model-View-Controller)**  (MVC) i **aplikacji sieci Web** szablony projektów (Razor strony). Wartości parametrów dozwolone są **Brak** i **poszczególnych**. **React.js i kontenera Redux** szablonu projektu nie obsługuje parametru uwierzytelniania w tej chwili.

## <a name="create-an-app-with-api-authorization-support"></a>Tworzenie aplikacji dzięki obsłudze autoryzacji interfejsu API

Przy użyciu usług Angular i reagowania aplikacji jednostronicowych można uwierzytelniania i autoryzacji użytkowników. Otwórz powłokę wiersza polecenia, a następnie uruchom następujące polecenie:

**Angular**:

```console
dotnet new angular -o <output_directory_name> -au Individual
```

**React**:

```console
dotnet new react -o <output_directory_name> -au Individual
```

Poprzednie polecenie umożliwia utworzenie aplikacji platformy ASP.NET Core za pomocą *ClientApp* katalog zawierający SPA.

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>Ogólny opis składników platformy ASP.NET Core w aplikacji

W poniższych sekcjach opisano ma zostać dodany do projektu, gdy uwierzytelnianie pomoc techniczna jest dostępna:

### <a name="startup-class"></a>Klasa początkowa

`Startup` Klasa ma następującymi dodatkami:

* Wewnątrz `Startup.ConfigureServices` metody:
  * Tożsamość przy użyciu domyślnego interfejsu użytkownika:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * IdentityServer przy użyciu dodatkowego `AddApiAuthorization` metody pomocnika tej konfiguracji niektóre domyślne konwencje platformy ASP.NET Core na podstawie IdentityServer:

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * Uwierzytelnianie przy użyciu dodatkowego `AddIdentityServerJwt` metody pomocnika, która umożliwia skonfigurowanie aplikacji, aby sprawdzał poprawność tokenów JWT produkowane przez IdentityServer:

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* Wewnątrz `Startup.Configure` metody:
  * Oprogramowanie pośredniczące uwierzytelniania, która jest odpowiedzialna za sprawdzanie poprawności poświadczeń żądania i ustawień użytkownika na kontekst żądania:

    ```csharp
    app.UseAuthentication();
    ```

  * Oprogramowanie pośredniczące IdentityServer, który uwidacznia punkty końcowe Open ID Connect:

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

Ta metoda pomocnika konfiguruje IdentityServer używania naszych obsługiwanej konfiguracji. IdentityServer to wydajna i rozszerzalna struktura do obsługi aplikacji obawy związane z bezpieczeństwem. W tym samym czasie, który udostępnia niepotrzebne złożoność najbardziej typowych scenariuszy. W związku z tym zestaw opcji konfiguracji i konwencje są dostępne, jeśli są one dobry punkt wyjścia. Po zmian potrzeb w zakresie uwierzytelniania, pełnych możliwości IdentityServer jest nadal dostępne dostosować uwierzytelniania do własnych potrzeb.

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt

Ta metoda pomocnika konfiguruje schemat zasad dla aplikacji jako domyślny program obsługi uwierzytelniania. Zasada jest skonfigurowana, aby umożliwić tożsamości obsługuje wszystkie żądania kierowane do dowolnego podrzędną w obszarze adres URL tożsamości "/ tożsamość". `JwtBearerHandler` Obsługuje wszystkie inne żądania. Ponadto ta metoda rejestruje `<<ApplicationName>>API` zasobach interfejsu API za pomocą IdentityServer z domyślny zakres `<<ApplicationName>>API` i konfiguruje oprogramowanie pośredniczące tokenu JWT Bearer do zweryfikowania tokeny wystawione przez IdentityServer dla aplikacji.

### <a name="sampledatacontroller"></a>SampleDataController

W *Controllers\SampleDataController.cs* plików, zwróć uwagę, `[Authorize]` zastosowany do klasy, która wskazuje, że użytkownik musi być autoryzowane na podstawie domyślnego zasad dostępu do zasobu. Domyślne zasady autoryzacji, stanie się być skonfigurowana do używania domyślnego schematu uwierzytelniania, który jest ustawiony przez `AddIdentityServerJwt` na schemat zasad, które wspomniano powyżej, dzięki czemu `JwtBearerHandler` skonfigurowane przez takie metody pomocnika domyślny program obsługi dla żądania do aplikacji.

### <a name="applicationdbcontext"></a>ApplicationDbContext

W *Data\ApplicationDbContext.cs* plików, zwróć uwagę, taka sama `DbContext` jest używany w tożsamości z wyjątkiem, że rozciąga `ApiAuthorizationDbContext` (bardziej pochodne klasy z `IdentityDbContext`) aby objąć IdentityServer schematu.

Aby uzyskać pełną kontrolę nad schemat bazy danych, dziedziczą z jednej z dostępnych tożsamości `DbContext` klasy i skonfigurować kontekstu można dołączyć schematu tożsamości przez wywołanie metody `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` na `OnModelCreating` metody.

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController

W *Controllers\OidcConfigurationController.cs* plików, zwróć uwagę, punkt końcowy, który jest przygotowany do obsługi parametrów OIDC, które klient musi używać.

### <a name="appsettingsjson"></a>appsettings.json

W *appsettings.json* pliku głównego projektu jest nowym `IdentityServer` sekcja, która opisuje listę skonfigurować klientów. W poniższym przykładzie są jednego klienta. Nazwa klienta odpowiada nazwie aplikacji i jest mapowany przez Konwencję w celu uwierzytelniania OAuth `ClientId` parametru. Profil, który wskazuje typ aplikacji jest skonfigurowany. Jest ona używana wewnętrznie w celu konwencje dysku, które upraszczają proces konfiguracji dla serwera. Brak dostępnych kilka profilów, jak wyjaśniono w [profile aplikacji](#application-profiles) sekcji.

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a>appsettings.Development.json

W *appsettings. Development.JSON* pliku katalog główny projektu jest `IdentityServer` sekcji, który opisuje klucz używany do podpisywania tokenów. Podczas wdrażania do środowiska produkcyjnego, klucz musi zostać aprowizowane i wdrażana wraz z aplikacji, jak wyjaśniono w [Wdróż do produkcji](#deploy-to-production) sekcji.

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a>Ogólny opis aplikacji Angular

Uwierzytelnianie i autoryzacja interfejs API obsługuje w Angular szablon znajduje się w jego własnej moduł Angular w *ClientApp\src\api autoryzacji* katalogu. Moduł składa się z następujących elementów:

* składniki 3:
  * *Login.Component.TS*: Obsługuje przepływu logowania aplikacji.
  * *logout.Component.TS*: Obsługuje przepływu wylogowania z aplikacji.
  * *Zaloguj się menu.component.ts*: Widżet, który wyświetli jeden z następujących linków:
    * Zarządzanie profilami użytkowników i łącza, gdy użytkownik jest uwierzytelniany w przypadku wylogowania.
    * Rejestracja i logowanie łącza, gdy użytkownik nie jest uwierzytelniony.
* Strażnik trasy `AuthorizeGuard` , mogą być dodawane do trasy i wymaga od użytkownika uwierzytelniali się przed odwiedzający trasy.
* Interceptor HTTP `AuthorizeInterceptor` , dołącza token dostępu na wychodzące żądania HTTP, przeznaczone dla interfejsu API, gdy użytkownik jest uwierzytelniony.
* Usługa `AuthorizeService` , obsługuje szczegóły niższego poziomu procesu uwierzytelniania i udostępnia informacje o uwierzytelnionym użytkowniku w pozostałej części aplikacji do użycia.
* Moduł Angular, definiujący tras skojarzone z uwierzytelniania części aplikacji. Udostępnia ona składnika menu logowania, interceptor, osłony i usługi do użycia od pozostałej części aplikacji.

## <a name="general-description-of-the-react-app"></a>Ogólny opis aplikacji platformy React

Obsługa uwierzytelniania i autoryzacji interfejsu API w szablonie platformy React znajduje się w *ClientApp\src\components\api autoryzacji* katalogu. Składa się z następujących elementów:

* 4 składniki:
  * *Login.js*: Obsługuje przepływu logowania aplikacji.
  * *Logout.js*: Obsługuje przepływu wylogowania z aplikacji.
  * *LoginMenu.js*: Widżet, który wyświetli jeden z następujących linków:
    * Zarządzanie profilami użytkowników i łącza, gdy użytkownik jest uwierzytelniany w przypadku wylogowania.
    * Rejestracja i logowanie łącza, gdy użytkownik nie jest uwierzytelniony.
  * *AuthorizeRoute.js*: Składnik trasy, który wymaga od użytkownika uwierzytelniali się przed renderowaniem składnika czcionką `Component` parametru.
* Wyeksportowane `authService` wystąpienia klasy `AuthorizeService` , obsługuje szczegóły niższego poziomu procesu uwierzytelniania i udostępnia informacje o uwierzytelnionym użytkowniku w pozostałej części aplikacji do użycia.

Skoro wiesz głównymi składnikami rozwiązania może potrwać lepiej poznać poszczególne scenariusze dla aplikacji.

## <a name="require-authorization-on-a-new-api"></a>Wymaga zezwolenia na nowy interfejs API

Domyślnie system jest skonfigurowany do łatwego wymagają autoryzacji dla nowych interfejsów API. Aby to zrobić, Utwórz nowy kontroler i Dodaj `[Authorize]` atrybutów do klasy kontrolera lub dowolnych akcji w obrębie kontrolera.

## <a name="protect-a-client-side-route-angular"></a>Ochrona trasę po stronie klienta (Angular)

Ochrona trasy klienta odbywa się przez dodanie guard autoryzacji do listy osłony do uruchomienia podczas konfigurowania trasy. Na przykład możesz zobaczyć jak `fetch-data` trasa jest skonfigurowana w module usługi Angular głównej aplikacji:

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

Ważne jest, aby wspomnieć, że ochrona trasy nie chroni istniejący punkt końcowy (które nadal wymaga `[Authorize]` zastosować atrybut), ale czy tylko uniemożliwia użytkownikowi przechodzić do danej trasy po stronie klienta, gdy go nie jest uwierzytelniany na.

## <a name="authenticate-api-requests-angular"></a>Uwierzytelnianie żądań interfejsu API (Angular)

Uwierzytelniania żądań do interfejsów API hostowanych obok aplikacji odbywa się automatycznie przy użyciu interceptor klienta HTTP, które są zdefiniowane przez aplikację.

## <a name="protect-a-client-side-route-react"></a>Ochrona trasę po stronie klienta (React)

Ochrona trasy po stronie klienta za pomocą `AuthorizeRoute` składnika zamiast zwykłego `Route` składnika. Na przykład, zwróć uwagę, jak `fetch-data` trasa jest skonfigurowana w ramach `App` składników:

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

Ochrona trasy:

* Nie chroni istniejący punkt końcowy (które nadal wymaga `[Authorize]` zastosowany do niego).
* Zapobiega tylko użytkownika przejdź do daną trasą po stronie klienta, gdy go nie jest uwierzytelniony.

## <a name="authenticate-api-requests-react"></a>Uwierzytelnianie żądań interfejsu API (React)

Uwierzytelnianie żądań przy użyciu platformy React odbywa się przez importowanie pierwszego `authService` wystąpienia z `AuthorizeService`. Token dostępu jest pobierana z `authService` i jest dołączony do żądania, jak pokazano poniżej. Składniki platformy React, ta praca odbywa się zwykle `componentDidMount` metoda cyklu życia lub w wyniku interakcji z użytkownikiem.

### <a name="import-the-authservice-into-your-component"></a>Importowanie authService do składnika

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>Pobieranie i dołączanie token dostępu do odpowiedzi

```javascript
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-to-production"></a>Wdrażanie w środowisku produkcyjnym

Aby wdrożyć aplikację do środowiska produkcyjnego, należy aprowizować następujące zasoby:

* Bazę danych do przechowywania tożsamości konta użytkowników i przyznaje IdentityServer.
* Certyfikatu produkcyjnego do użycia podczas podpisywania tokenów.
  * Nie istnieją określone wymagania dotyczące tego certyfikatu; może być certyfikat z podpisem własnym lub certyfikatu dostarczanymi za pośrednictwem urzędu certyfikacji.
  * Mogą być generowane przy użyciu standardowych narzędzi, takich jak program PowerShell lub protokołu OpenSSL.
  * Mogą być zainstalowane w magazynie certyfikatów na komputerach docelowych lub wdrożony jako *PFX* pliku silnym hasłem.

### <a name="example-deploy-to-azure-websites"></a>Przykład: Wdrażanie w usłudze Azure Websites

W tej sekcji opisano wdrażanie aplikacji w usłudze Azure websites przy użyciu certyfikatu przechowywanego w magazynie certyfikatów. Aby zmodyfikować aplikację, aby załadować certyfikatu z magazynu certyfikatów, plan usługi App Service musi być włączona co najmniej warstwę standardowa podczas konfigurowania w późniejszym kroku. W aplikacji *appsettings.json* pliku, zmodyfikuj `IdentityServer` sekcji, aby dołączyć szczegóły klucza:

```json
"IdentityServer": {
  "Key": {
    "Type": "Store",
    "StoreName": "My",
    "StoreLocation": "CurrentUser",
    "Name": "CN=MyApplication"
  }
}
```

* Właściwość Nazwa certyfikatu odpowiada wyróżniająca podmiotu certyfikatu.
* Lokalizacja magazynu reprezentuje, gdzie można załadować certyfikatu z (`CurrentUser` lub `LocalMachine`).
* Nazwa magazynu reprezentuje nazwę magazynu certyfikatów, gdy certyfikat jest przechowywany. W tym przypadku punktów w magazynie osobistym użytkownika.

Aby wdrożyć w usłudze Azure Websites, wdrażanie aplikacji, wykonaj czynności w [wdrażanie aplikacji na platformie Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) zasobów platformy Azure niezbędnych do tworzenia i wdrażania aplikacji w środowisku produkcyjnym.

Po wykonaniu poprzednich instrukcji, aplikacja jest wdrażana na platformie Azure, ale nie jest jeszcze funkcjonalności. Certyfikat używany przez aplikację, która jest nadal potrzebuje do skonfigurowania. Znajdź odcisk palca certyfikatu, który ma być używany, a następnie wykonaj kroki opisane w [ładowanie certyfikatów](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).

Podczas tych czynności wspomina o identyfikatorach protokołu SSL znajduje się **certyfikaty prywatne** sekcji w portalu, gdzie możesz przekazać certyfikat aprowizowane za pomocą aplikacji.

Po wykonaniu tego kroku, uruchom ponownie aplikację i powinna być funkcjonalności.

## <a name="other-configuration-options"></a>Inne opcje konfiguracji

Obsługa interfejsu API autoryzacji opartą na IdentityServer z zestawem Konwencji, wartości domyślne i ulepszenia, aby uprościć środowisko dla aplikacji jednostronicowych. Needless, że pełnych możliwości IdentityServer jest dostępna w tle integracji platformy ASP.NET Core nie obejmuje Twojego scenariusza. Obsługa platformy ASP.NET Core koncentruje się na "" aplikacje firmy, gdzie wszystkie aplikacje są tworzone i wdrażane przez naszej organizacji. Jako takie pomoc techniczna nie jest oferowana w przypadku elementów, takich jak zgody lub federacji. W tych scenariuszach Użyj IdentityServer i postępuj zgodnie z ich dokumentacji.

### <a name="application-profiles"></a>Profile aplikacji

Profile aplikacji są wstępnie zdefiniowane konfiguracje dla aplikacji, które uściślić swoich parametrów. W tej chwili obsługiwane są następujące profile:

* `IdentityServerSPA`: Reprezentuje SPA, hostowane obok IdentityServer jako pojedyncza jednostka.
  * `redirect_uri` Wartość domyślna to `/authentication/login-callback`.
  * `post_logout_redirect_uri` Wartość domyślna to `/authentication/logout-callback`.
  * Obejmuje zestaw zakresów `openid`, `profile`i każdy zakres zdefiniowany dla interfejsów API w aplikacji.
  * Zestaw dozwolonych typów odpowiedzi OIDC jest `id_token token` lub każdego z nich osobno (`id_token`, `token`).
  * Tryb odpowiedzi dozwolone jest `fragment`.
* `SPA`: Reprezentuje SPA, która nie jest obsługiwana przy użyciu IdentityServer.
  * Obejmuje zestaw zakresów `openid`, `profile`i każdy zakres zdefiniowany dla interfejsów API w aplikacji.
  * Zestaw dozwolonych typów odpowiedzi OIDC jest `id_token token` lub każdego z nich osobno (`id_token`, `token`).
  * Tryb odpowiedzi dozwolone jest `fragment`.
* `IdentityServerJwt`: Reprezentuje interfejs API, który znajduje się wraz z IdentityServer.
  * Aplikacja jest skonfigurowana z jednego zakresu, które domyślnie jest to nazwa aplikacji.
* `API`: Reprezentuje interfejs API, która nie jest obsługiwana z IdentityServer.
  * Aplikacja jest skonfigurowana z jednego zakresu, które domyślnie jest to nazwa aplikacji.

### <a name="configuration-through-appsettings"></a>Konfigurowanie za pomocą AppSettings

Konfigurowanie aplikacji przy użyciu systemu konfiguracji, dodając je do listy `Clients` lub `Resources`.

Skonfiguruj każdy klient `redirect_uri` i `post_logout_redirect_uri` właściwości, jak pokazano w poniższym przykładzie:

```json
"IdentityServer": {
  "Clients": {
    "MySPA": {
      "Profile": "SPA",
      "RedirectUri": "https://www.example.com/authentication/login-callback",
      "LogoutUri": "https://www.example.com/authentication/logout-callback"
    }
  }
}
```

Podczas konfigurowania zasobów, można skonfigurować zakresy dla zasobu, jak pokazano poniżej:

```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c"
    }
  }
}
```

### <a name="configuration-through-code"></a>Konfigurowanie za pomocą kodu

Można również skonfigurować klientów i zasobów za pomocą kodu za pomocą przeciążenia `AddApiAuthorization` , wykonują akcję, aby skonfigurować opcje.

```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA", spa =>
        spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
           .WithLogoutRedirectUri(
               "http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource =>
        resource.WithScopes("a", "b", "c"));
});
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:spa/angular>
* <xref:spa/react>
