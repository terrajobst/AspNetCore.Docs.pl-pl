---
title: Wprowadzenie do uwierzytelniania dla aplikacji programu ASP.NET Core
author: ''
description: Tożsamość za pomocą aplikacji jednostronicowej hostowaną wewnątrz aplikacji ASP.NET Core.
ms.author: ''
ms.date: 03/05/2018
uid: security/authentication/identity/spa
ms.openlocfilehash: cf04ec1ff0ae9afea066fd1864ab0a7956ace32c
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346821"
---
# <a name="authentication-and-authorization-for-spas"></a>Uwierzytelnianie i autoryzację dla aplikacji jednostronicowych

W programie ASP.NET 3.0 wprowadzamy obsługę uwierzytelniania w aplikacji jednostronicowej przy użyciu naszej obsługi nowego interfejsu API autoryzacji. Ta funkcja opiera się na kombinacji tożsamości platformy ASP.NET Core uwierzytelniania i przechowywanie użytkowników i tożsamość serwera dotyczące implementowania Open ID Connect.

Dodaliśmy nowy parametr uwierzytelniania do naszej platformy Angular i szablonów platformy React, które są podobne do parametru uwierzytelniania w naszych szablonów stron mvc i razor za pomocą dozwolone wartości "None" i "Indywidualne".

## <a name="create-an-angular-app-with-api-authorization-support"></a>Tworzenie aplikacji Angular dzięki obsłudze autoryzacji interfejsu API

Aby utworzyć nową aplikację platformy Angular dzięki obsłudze uwierzytelniania i autoryzacji użytkowników, Otwórz powłokę wiersza polecenia i uruchom następujące polecenie:

```console
dotnet new angular -o <output_directory_name> -au Individual
```

Poprzednie polecenie umożliwia utworzenie aplikacji platformy ASP.NET Core za pomocą *ClientApp* katalog zawierający aplikacji Angular.

## <a name="create-a-react-app-with-api-authorization-support"></a>Tworzenie aplikacji platformy React z obsługi autoryzacji interfejsu API

Aby utworzyć nową aplikację platformy React z obsługą uwierzytelniania i autoryzacji użytkowników, Otwórz powłokę wiersza polecenia i uruchom następujące polecenie:

```console
dotnet new react -o <output_directory_name> -au Individual
```

Poprzednie polecenie umożliwia utworzenie aplikacji platformy ASP.NET Core za pomocą *ClientApp* katalogu zawierającego aplikację platformy React.

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>Ogólny opis składników platformy ASP.NET Core w aplikacji

Istnieje kilka ma zostać dodany do projektu, gdy firma Microsoft zapewnia obsługę dla uwierzytelniania:

### <a name="startup-class"></a>Klasa początkowa

Jeśli spojrzymy na kod w klasie uruchamiania poniżej Jesteśmy wdzięczni za dołączenia następujące:
* Wewnątrz `public void ConfigureServices(IServiceCollection services)`:
  * Tożsamość przy użyciu domyślnego interfejsu użytkownika.
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * Tożsamość serwera przy użyciu dodatkowej metody pomocnika AddApiAuthorization tej konfiguracji niektóre domyślne konwencje platformy ASP.NET na podstawie tożsamości serwera.
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * Uwierzytelnianie przy użyciu dodatkowej metody pomocnika AddIdentityServerJwt konfiguruje aplikację do weryfikacji tokenów Jwt, generowane przez tożsamość serwera. 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* Wewnątrz `public void Configure(IApplicationBuilder app)`:
  * Oprogramowanie pośredniczące uwierzytelniania, która jest odpowiedzialna za sprawdzanie poprawności poświadczeń w żądaniu przychodzącym i ustawień użytkownika na kontekst żądania.
    ```csharp
    app.UseAuthentication();
    ```
  * Tożsamość serwera oprogramowania pośredniczącego, które uwidacznia punkty końcowe Open ID Connect.
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization 
Ta metoda pomocnika konfiguruje serwer tożsamości do użycia naszych obsługiwanej konfiguracji. Serwer tożsamości jest bardzo wydajna i Rozszerzalna architektura służąca do obsługi aplikacji obawy związane z bezpieczeństwem, ale w tym samym czasie, który udostępnia wiele złożoności, który nie potrzebujemy, aby dowiedzieć się o najbardziej typowych scenariuszy, więc możemy wybrać zestaw konwencje i Opcje konfiguracji, które firma Microsoft uważa, są również dobry punkt wyjścia. Zmian potrzeb w zakresie uwierzytelniania pełnych możliwości serwera tożsamości po nadal dostępne, dzięki czemu możesz dostosować ją do własnych potrzeb.

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt
Ta metoda pomocnika konfiguruje schemat zasad dla aplikacji jako domyślny program obsługi uwierzytelniania. Zasada jest skonfigurowana, aby umożliwić tożsamości obsłużenie wszystkich żądań, które przejść do dowolnego podrzędną w obszarze adres url tożsamości "/ tożsamość" i umożliwić JwtBearerHandler wszystkich innych żądaniach obsługi.
Ta metoda rejestruje Addionally `<<ApplicationName>>API` zasobu interfejsu Api za pomocą tożsamości serwera przy użyciu domyślny zakres `<<ApplicationName>>API` i konfiguruje oprogramowanie pośredniczące tokenu JWT Bearer do weryfikacji tokenów wystawionych przez serwer tożsamości dla aplikacji.

### <a name="sampledatacontroller"></a>SampleDataController
Jeśli spojrzymy na plik Controllers\SampleDataController.cs można zaobserwować `[Authorize]` zastosowany do klasy, która wskazuje, że użytkownik musi być autoryzowane na podstawie domyślnego zasad dostępu do zasobu. Domyślne zasady autoryzacji, stanie się być skonfigurowana do używania domyślny schemat uwierzytelniania, który jest ustawiony przez `AddIdentityServerJwt` na schemat zasad, które wspomniano powyżej, dzięki czemu program obsługi JwtBearer skonfigurowane przez takie metody pomocnika domyślny program obsługi dla żądania do aplikacji.

### <a name="applicationdbcontext"></a>ApplicationDbContext
Jeśli spojrzymy na plik w Data\ApplicationDbContext.cs widać na tym samym typem DbContext, używamy w tożsamości z wyjątkiem tego, że wykracza poza ApiAuthorizationDbContext (bardziej klasy pochodnej od kontekst IdentityDbContext użytkowników) do uwzględnienia schematu dla tożsamości serwera.
Jeśli chcemy pełną kontrolę nad schemat bazy danych możemy po prostu dziedziczą z jednej z dostępnych klas DbContext tożsamości i skonfigurować kontekstu można dołączyć schematu tożsamości przez wywołanie metody `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` na `OnModelCreating` metody.

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController
Jeśli przyjrzymy pliku Controllers\OidcConfigurationController.cs widać punkt końcowy tego możemy hali do obsługi parametrów OIDC, które klient musi używać.

### <a name="appsettingsjson"></a>appsettings.json
Jeśli spojrzymy na plik appsettings.json w folderze głównym projektu, widać nową `IdentityServer` sekcja, która opisuje listę skonfigurowane klientów i widać, że istnieje jednego klienta. Nazwa klienta odpowiada nazwę aplikacji i jest mapowany zgodnie z Konwencją do parametru identyfikatora klienta oAuth. Profil, który wskazuje, jakiego rodzaju aplikacji, które będziemy konfigurować, a firma Microsoft wewnętrznie Użyj dysku konwencjami, które upraszczają proces konfiguracji dla serwera. Istnieje kilka profilów dostępnych wyjaśnione w dalszej części tego artykułu.

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
Jeśli przyjrzymy się appsettings. Development.JSON pliku w katalogu głównym projektu widać nową `IdentityServer` sekcja, która opisuje klucz, firma Microsoft jest używany do podpisywania tokenów. Podczas wdrażania do produkcji klucz musi zostać aprowizowane i wdrażana wraz z aplikacji, co zostało opisane poniżej.

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a>Ogólny opis aplikację platformy Angular
Obsługa uwierzytelniania i autoryzacji interfejsu API w szablonie usługi Angular znajduje się w jego własnej moduł Angular. W obszarze ClientApp\src\api autoryzacji i składa się z następujących elementów:
* Składniki 3:
  * Składnik nazwy logowania: Obsługuje przepływu logowania dla aplikacji.
  * Składnik wylogowania: Obsługuje przepływu wylogowania w aplikacji.
  * Element menu logowania: Element widget, który wyświetla nazwy aktualnie uwierzytelnionego użytkownika wraz z łączami do zarządzania profilu użytkownika i wylogować się lub łączy się zalogować się lub Zarejestruj się, gdy użytkownik nie jest uwierzytelniony.
* Strażnik trasy `AuthorizeGuard` , mogą być dodawane do trasy i wymaga od użytkownika uwierzytelniali się przed odwiedzający trasy.
* Interceptor http `AuthorizeInterceptor` , dołącza token dostępu na wychodzące żądania HTTP, przeznaczone dla interfejsu API, gdy użytkownik jest uwierzytelniony.
* Usługa `AuthorizeService` który obsłuży niższym poziomie procesu uwierzytelniania i udostępnia informacje o uwierzytelnionym użytkowniku w pozostałej części aplikacji do użycia.
* Moduł angular, który definiuje tras skojarzone z części uwierzytelniania aplikacji i udostępnia składnika menu logowania, interceptor, osłony i usługi do użycia z pozostałymi aplikacjami.

## <a name="general-description-of-the-react-application"></a>Ogólny opis aplikacji platformy React
Obsługa uwierzytelniania i autoryzacji interfejsu API w życie szablonu platformy React w obszarze ClientApp\src\components\api authorization\ i składa się z następujących elementów:
* 4 składniki:
  * Składnik nazwy logowania: Obsługuje przepływu logowania dla aplikacji.
  * Składnik wylogowania: Obsługuje przepływu wylogowania w aplikacji.
  * Element menu logowania: Element widget, który wyświetla nazwy aktualnie uwierzytelnionego użytkownika wraz z łączami do zarządzania profilu użytkownika i wylogować się lub łączy się zalogować się lub Zarejestruj się, gdy użytkownik nie jest uwierzytelniony.
  * AuthorizeRoute: Składnik trasy, który wymaga od użytkownika uwierzytelniali się przed renderowaniem składnika wskazanego w parametrze składnika.
  * Wyeksportowane `authService` wystąpienia klasy `AuthorizeService` który obsłuży niższym poziomie procesu uwierzytelniania i udostępnia informacje o uwierzytelnionym użytkowniku w pozostałej części aplikacji do użycia.

Teraz, w związku z czym dostrzegliśmy głównymi składnikami rozwiązania, firma Microsoft może Przyjrzyj się określonych poszczególne scenariusze dla aplikacji:

## <a name="requiring-authorization-on-a-new-api"></a>Wymaganie autoryzacji na nowy interfejs API
System jest skonfigurowany gotowych umożliwiają proste do wymagają autoryzacji dla nowych interfejsów API. Aby to zrobić, po prostu Utwórz nowy kontroler i Dodaj `[Authorize]` atrybutów do klasy kontrolera lub dowolnych akcji w obrębie kontrolera.

## <a name="protecting-a-client-side-route-angular"></a>Ochrona trasę po stronie klienta (Angular)
Ochrona trasy po stronie klienta odbywa się przez dodanie guard autoryzacji do listy osłony do uruchomienia podczas konfigurowania trasy. Na przykład możesz zobaczyć, jak trasa pobierania danych jest skonfigurowana w module usługi angular głównej aplikacji:

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

Ważne jest, aby wspomnieć, że ochrona trasy nie chroni istniejący punkt końcowy (który nadal wymaga `[Authorize]` zastosować atrybut), ale że go tylko uniemożliwia użytkownikowi przejście do trasy po stronie danego klienta, gdy nie jest uwierzytelniony.

## <a name="authenticate-api-requests-angular"></a>Uwierzytelnianie żądań interfejsu API (Angular)

Uwierzytelniania żądań do interfejsów API hostowanych wzdłuż krawędzi, że aplikacja odbywa się automatycznie przy użyciu interceptor klienta HTTP, które są definiowane przez aplikację.

## <a name="protect-a-client-side-route-react"></a>Ochrona trasę po stronie klienta (React)

Ochrona trasy po stronie klienta odbywa się za pomocą składnika AuthorizeRoute zamiast zwykłego składnika trasy. Na przykład możesz zobaczyć, jak trasa pobierania danych jest skonfigurowana w składniku aplikacji:

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

Ważne jest, aby wspomnieć, że ochrona trasy nie chroni istniejący punkt końcowy (który nadal wymaga `[Authorize]` zastosować atrybut), ale że go tylko uniemożliwia użytkownikowi przejście do trasy po stronie danego klienta, gdy nie jest uwierzytelniony.

## <a name="authenticate-api-requests-react"></a>Uwierzytelnianie żądań interfejsu API (React)

Uwierzytelnianie żądań przy użyciu platformy react odbywa się przez importowanie pierwszego `authService` wystąpienia z `AuthorizeService` pobierania tokenu dostępu z authService i dołączania ich do żądania, jak pokazano poniżej. Składniki platformy react zazwyczaj jest to wykonywane w metodzie cyklu życia componentDidMount lub w wyniku z interakcji z użytkownikiem.

### <a name="import-the-authservice-into-your-component"></a>Importowanie authService do składnika

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>Pobieranie i dołączanie token dostępu do odpowiedzi

```js
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-into-production"></a>Wdrażanie w środowisku produkcyjnym

Aby wdrożyć aplikację w środowisku produkcyjnym należy aprowizować kilka zasobów:
* Przyznaje bazę danych do przechowywania tożsamości konta użytkowników i serwera tożsamości.
* Certyfikatu produkcyjnego do użycia podczas podpisywania tokenów.
  * Nie istnieją określone wymagania dotyczące tego certyfikatu; może być certyfikat z podpisem własnym lub certyfikatu dostarczanymi za pośrednictwem urzędu certyfikacji.
  * Mogą być generowane przy użyciu standardowych narzędzi, takich jak program powershell lub protokołu openssl.
  * Może być zainstalowany w magazynie certyfikatów na komputerach docelowych lub wdrożony jako plik pfx z silnym hasłem.

### <a name="example-deploy-into-azure-websites"></a>Przykład: Wdrażanie w usłudze Azure Websites

W tej sekcji użyjemy do wdrożenia aplikacji w usłudze Azure websites przy użyciu certyfikatu przechowywanego w magazynie certyfikatów. Należy zmodyfikować aplikację, aby załadować talonu z magazynu certyfikatów. Aby to zrobić, nasz plan usługi app service musi mieć co najmniej w warstwie standardowa, jeśli skonfigurowaną w późniejszym kroku. W naszej aplikacji po prostu należy zmodyfikować sekcję IdentityServer appsettings.json obejmujący istotne szczegóły:
```json
  "IdentityServer": {
    "Key": {
      "Type": "Store",
      "StoreName": "My",
      "StoreLocation": "CurrentUser",
      "Name": "CN=MyApplication"
    }
  }
}
```
* Właściwość Nazwa certyfikatu odpowiada wyróżniająca podmiotu certyfikatu.
* Lokalizacja magazynu reprezentuje, gdzie można załadować certyfikatu (CurrentUrser lub LocalMachine).
* Nazwa magazynu reprezentuje nazwę magazynu certyfikatów, gdy certyfikat jest przechowywany, w tym przypadku, który wskazuje w magazynie osobistym użytkownika.

Aby wdrożyć w usłudze Azure Websites, wdrażanie aplikacji, wykonaj czynności w [wdrażanie aplikacji na platformie Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) zasobów platformy Azure niezbędnych do tworzenia i wdrażania aplikacji w środowisku produkcyjnym.

Po wykonaniu tego, aplikacja jest wdrażana na platformie Azure, ale nie jest jeszcze całkowicie funkcjonalności nadal trzeba skonfigurować certyfikat używany przez aplikację. Aby to zrobić, należy mieć odcisk palca certyfikatu, będziemy używać, i wykonaj kroki opisane w [ładowanie certyfikatów](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).

Chociaż te kroki wspomina o identyfikatorach protokołu SSL, jest sekcja "Certyfikaty prywatne" w portalu, gdzie można przekazać nasze elastycznie certyfikatu za pomocą naszej aplikacji.

Po wykonaniu tego kroku będziemy mogli ponownie uruchomić naszą aplikację i powinna być w pełni funkcjonalna.

## <a name="other-configuration-options"></a>Inne opcje konfiguracji
Nasze wsparcie dla interfejsu API autoryzacji opartą na tożsamość serwera z zestawem Konwencji, wartości domyślne i ulepszenia, aby uprościć środowisko dla aplikacji. Needless, że pełnych możliwości serwera tożsamości jest dostępna w tle integracji, które firma Microsoft oferuje nie obejmuje Twojego scenariusza. Nasze wsparcie koncentruje się na tak zwany "firmy Microsoft" aplikacji, gdzie wszystkie aplikacje są tworzone i wdrażane przez naszej organizacji. Jako takie firma Microsoft nie oferuje wsparcie dla elementów, takich jak zgody lub federacji. W tych scenariuszach naszych zaleca się używania tożsamości serwera i postępuj zgodnie z ich dokumentacji.

### <a name="application-profiles"></a>Profile aplikacji
Profile aplikacji są wstępnie zdefiniowane konfiguracje dla aplikacji, które uściślić swoich parametrów. Teraz obsługujemy dwa profile:
* IdentityServerSPA: Reprezentuje aplikacji jednostronicowej hostowana razem tożsamości serwera jako pojedynczą jednostką.
  * Wartością domyślną jest redirect_uri `/authentication/login-callback`.
  * Wartością domyślną jest post_logout_redirect_uri `/authentication/logout-callback`.
  * Obejmuje zestaw zakresów `openid`, `profile`i każdy zakres zdefiniowany dla interfejsów API w aplikacji.
  * Zestaw dozwolonych typów odpowiedzi OIDC jest `id_token token` lub każdego z nich osobno (`id_token`, `token`).
  * Tryb odpowiedzi dozwolone jest `fragment`.
* SPA: Reprezentuje aplikacja jednostronicowa, który nie jest obsługiwany za pomocą tożsamości serwera.
  * Obejmuje zestaw zakresów `openid`, `profile`i każdy zakres zdefiniowany dla interfejsów API w aplikacji.
  * Zestaw dozwolonych typów odpowiedzi OIDC jest `id_token token` lub każdego z nich osobno (`id_token`, `token`).
  * Tryb odpowiedzi dozwolone jest `fragment`.
* IdentityServerJwt: Reprezentuje interfejs API, który znajduje się wraz z serwerem tożsamości.
  * Aplikacja jest skonfigurowana do mieć pojedynczy zakres, która domyślnie używa nazwy aplikacji.
* API: Reprezentuje interfejs API, który nie jest obsługiwana za pomocą tożsamości serwera.
  * Aplikacja jest skonfigurowana do mieć pojedynczy zakres, która domyślnie używa nazwy aplikacji.

### <a name="configuration-through-appsettings"></a>Konfigurowanie za pomocą AppSettings
Firma Microsoft można skonfigurować za pomocą nasz system konfiguracji aplikacji, dodając je do listy klientów lub zasoby odpowiednio. 

Podczas konfigurowania klientów można skonfigurować `redirect_uri` i `post_logout_redirect_uri` jak pokazano poniżej:
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

Podczas konfigurowania zasobów możemy skonfigurować zakresy dla zasobu, jak pokazano poniżej:
```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c",
    }
  }
}
```

### <a name="configuration-through-code"></a>Konfigurowanie za pomocą kodu
Firma Microsoft można również skonfigurować klientów i zasobów za pomocą kodu za pomocą przeciążenia AddApiAuthorization, które wykonują akcję, aby skonfigurować opcje.
```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA",
        spa => spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
            .WithLogoutRedirectUri("http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource => resource.WithScopes("a", "b", "c"));
});
```
