---
title: Zabezpieczanie aplikacji hostowanej przez ASP.NET Core Blazor webassembly przy użyciu Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: 0803e436d66ef7df3c68739e674a8652fde11166
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083847"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a>Zabezpieczanie aplikacji hostowanej przez ASP.NET Core Blazor webassembly przy użyciu Azure Active Directory

Autorzy [Javier Calvarro Nelson](https://github.com/javiercn) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



W tym artykule opisano sposób tworzenia [aplikacji hostowanej w programieBlazor webassembly](xref:blazor/hosting-models#blazor-webassembly) , która używa [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) do uwierzytelniania.

## <a name="register-apps-in-aad-b2c-and-create-solution"></a>Rejestrowanie aplikacji w AAD B2C i tworzenie rozwiązania

### <a name="create-a-tenant"></a>Tworzenie dzierżawy

Postępuj zgodnie ze wskazówkami w [przewodniku szybki start: Konfigurowanie dzierżawy](/azure/active-directory/develop/quickstart-create-new-tenant) w celu utworzenia dzierżawy w usłudze AAD.

### <a name="register-a-server-api-app"></a>Rejestrowanie aplikacji interfejsu API serwera

Postępuj zgodnie ze wskazówkami w [przewodniku szybki start: Zarejestruj aplikację przy użyciu platformy tożsamości firmy Microsoft](/azure/active-directory/develop/quickstart-register-app) i kolejnych tematów usługi Azure AAD, aby zarejestrować aplikację w usłudze AAD dla *aplikacji interfejsu API serwera* w **Azure Active Directory** > **rejestracje aplikacji** obszarze Azure Portal:

1. Wybierz pozycję **Nowa rejestracja**.
1. Podaj **nazwę** aplikacji (na przykład **Blazor Server AAD**).
1. Wybierz **obsługiwane typy kont**. W tym środowisku możesz wybrać **tylko konta w tym katalogu organizacji** (pojedynczy dzierżawca).
1. *Aplikacja interfejsu API serwera* nie wymaga **identyfikatora URI przekierowania** w tym scenariuszu, więc pozostaw listę rozwijaną w **sieci Web** i nie wprowadzaj identyfikatora URI przekierowania.
1. Wyłącz **uprawnienia** > **Przyznaj administratorowi OpenID Connect i offline_access** pole wyboru.
1. Wybierz pozycję **Zarejestruj**.

W obszarze **uprawnienia interfejsu API**usuń uprawnienie **Microsoft Graph** > **User. Read** , ponieważ aplikacja nie wymaga dostępu do profilu Logowanie lub UER.

W obszarze **Uwidacznianie interfejsu API**:

1. Wybierz polecenie **Dodaj zakres**.
1. Wybierz przycisk **Zapisz i kontynuuj**.
1. Podaj **nazwę zakresu** (na przykład `API.Access`).
1. Podaj **nazwę wyświetlaną zgody administratora** (na przykład `Access API`).
1. Podaj **Opis zgody administratora** (na przykład `Allows the app to access server app API endpoints.`).
1. Upewnij się, że **stan** jest ustawiony na **włączone**.
1. Wybierz pozycję **Dodaj zakres**.

Zapisz następujące informacje:

* *Aplikacja interfejsu API serwera* Identyfikator aplikacji (identyfikator klienta) (na przykład `11111111-1111-1111-1111-111111111111`)
* Identyfikator katalogu (identyfikator dzierżawy) (na przykład `222222222-2222-2222-2222-222222222222`)
* Domena dzierżawy usługi AAD (na przykład `contoso.onmicrosoft.com`)
* Zakres domyślny (na przykład `API.Access`)

### <a name="register-a-client-app"></a>Rejestrowanie aplikacji klienckiej

Postępuj zgodnie ze wskazówkami w [przewodniku szybki start: Zarejestruj aplikację przy użyciu platformy tożsamości firmy Microsoft](/azure/active-directory/develop/quickstart-register-app) i kolejnych tematów usługi Azure AAD, aby zarejestrować aplikację w usłudze AAD dla *aplikacji klienckiej* w obszarze **Azure Active Directory** > **rejestracje aplikacji** Azure Portal:

1. Wybierz pozycję **Nowa rejestracja**.
1. Podaj **nazwę** aplikacji (na przykład **Blazor klienta AAD**).
1. Wybierz **obsługiwane typy kont**. W tym środowisku możesz wybrać **tylko konta w tym katalogu organizacji** (pojedynczy dzierżawca).
1. Pozostaw pole listy rozwijanej **Identyfikator URI przekierowania** na wartość **Web**i podaj identyfikator URI przekierowania `https://localhost:5001/authentication/login-callback`.
1. Wyłącz **uprawnienia** > **Przyznaj administratorowi OpenID Connect i offline_access** pole wyboru.
1. Wybierz pozycję **Zarejestruj**.

W obszarze **uwierzytelnianie** > **konfiguracjami platformy** > **sieci Web**:

1. Upewnij się, że **Identyfikator URI przekierowania** `https://localhost:5001/authentication/login-callback` jest obecny.
1. W przypadku **niejawnego przydzielenia**zaznacz pola wyboru dla **tokenów dostępu** i **tokenów identyfikatorów**.
1. Pozostałe wartości domyślne dla aplikacji są dopuszczalne dla tego środowiska.
1. Wybierz ikonę **Zapisz**.

W **uprawnienia interfejsu API**:

1. Upewnij się, że aplikacja ma **Microsoft Graph** > uprawnienie **użytkownika. odczyt** .
1. Wybierz pozycję **Dodaj uprawnienia** , a następnie **Moje interfejsy API**.
1. Wybierz *aplikację interfejsu API serwera* z kolumny **Nazwa** (na przykład **Blazor Server AAD**).
1. Otwórz listę **interfejsów API** .
1. Włącz dostęp do interfejsu API (na przykład `API.Access`).
1. Wybierz pozycję **Dodaj uprawnienia**.
1. Wybierz przycisk **Udziel zawartości administratora dla {Nazwa dzierżawy}** . Wybierz pozycję **Tak**, aby potwierdzić.

Zapisz identyfikator aplikacji *aplikacji klienta* (identyfikator klienta) (na przykład `33333333-3333-3333-3333-333333333333`).

### <a name="create-the-app"></a>Tworzymy aplikację.

Zastąp symbole zastępcze w poniższym poleceniu zapisanymi wcześniej informacjami i wykonaj polecenie w powłoce poleceń:

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP CLIENT ID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

Aby określić lokalizację wyjściową, która tworzy folder projektu, jeśli nie istnieje, Uwzględnij opcję Output w poleceniu z ścieżką (na przykład `-o BlazorSample`). Nazwa folderu jest również częścią nazwy projektu.

> [!NOTE]
> Zapoznaj się z sekcją [Obsługa usługi uwierzytelniania](#Authentication service support) , aby uzyskać istotną zmianę konfiguracji w zakresie domyślnego tokenu dostępu. Wartość dostarczona przez szablon Blazor webassembly musi zostać zmieniona ręcznie po utworzeniu *aplikacji klienckiej* na podstawie szablonu.

## <a name="server-app-configuration"></a>Konfiguracja aplikacji serwera

*Ta sekcja dotyczy aplikacji **serwerowej** rozwiązania.*

### <a name="authentication-package"></a>Pakiet uwierzytelniania

Obsługa uwierzytelniania i autoryzowania wywołań ASP.NET Core interfejsów API sieci Web jest zapewniana przez `Microsoft.AspNetCore.Authentication.AzureAD.UI`:

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a>Obsługa usługi uwierzytelniania

Metoda `AddAuthentication` konfiguruje usługi uwierzytelniania w ramach aplikacji i konfiguruje procedurę obsługi okaziciela JWT jako domyślną metodę uwierzytelniania. Metoda `AddAzureADBearer` konfiguruje określone parametry w obsłudze okaziciela JWT wymagane do weryfikacji tokenów emitowanych przez Azure Active Directory:

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

`UseAuthentication` i `UseAuthorization` upewnij się, że:

* Aplikacja próbuje analizować tokeny i sprawdzać ich poprawność w żądaniach przychodzących.
* Wszystkie żądania próbujące uzyskać dostęp do chronionego zasobu bez poprawnego poświadczenia nie powiedzie się.

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a>Ustawienia aplikacji

Plik *appSettings. JSON* zawiera opcje konfigurowania procedury obsługi okaziciela JWT używanej do sprawdzania poprawności tokenów dostępu.

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{API CLIENT ID}",
  }
}
```

### <a name="weatherforecast-controller"></a>Kontroler WeatherForecast

Kontroler WeatherForecast (*controllers/WeatherForecastController. cs*) ujawnia chroniony interfejs API z atrybutem `[Authorize]` zastosowanym do kontrolera. **Ważne** jest, aby zrozumieć, że:

* Atrybut `[Authorize]` w tym kontrolerze interfejsu API jest jedynym warunkiem, że ten interfejs API jest chroniony przed nieautoryzowanym dostępem.
* Atrybut `[Authorize]` używany w aplikacji Blazor webassembly służy tylko jako Wskazówka dla aplikacji, której użytkownik powinien mieć autoryzację, aby aplikacja działała poprawnie.

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a>Konfiguracja aplikacji klienta

*Ta sekcja dotyczy aplikacji **klienckiej** rozwiązania.*

### <a name="authentication-package"></a>Pakiet uwierzytelniania

Gdy aplikacja zostanie utworzona w celu korzystania z kont służbowych (`SingleOrg`), aplikacja automatycznie otrzymuje odwołanie do pakietu dla [biblioteki uwierzytelniania firmy Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`). Pakiet zawiera zestaw elementów podstawowych, które ułatwiają aplikacji uwierzytelnianie użytkowników i uzyskiwanie tokenów do wywoływania chronionych interfejsów API.

W przypadku dodawania uwierzytelniania do aplikacji ręcznie Dodaj pakiet do pliku projektu aplikacji:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

Zastąp `{VERSION}` w powyższym odwołaniu do pakietu w wersji pakietu `Microsoft.AspNetCore.Blazor.Templates` pokazanej w <xref:blazor/get-started> artykule.

Pakiet `Microsoft.Authentication.WebAssembly.Msal` w sposób przechodni dodaje pakiet `Microsoft.AspNetCore.Components.WebAssembly.Authentication` do aplikacji.

### <a name="authentication-service-support"></a>Obsługa usługi uwierzytelniania

Obsługa uwierzytelniania użytkowników jest rejestrowana w kontenerze usługi przy użyciu metody rozszerzenia `AddMsalAuthentication` dostarczonej przez pakiet `Microsoft.Authentication.WebAssembly.Msal`. Ta metoda umożliwia skonfigurowanie wszystkich usług wymaganych przez aplikację do współpracy z dostawcą tożsamości (IP).

*Program.cs*:

Po wygenerowaniu *aplikacji klienckiej* domyślny zakres tokenów dostępu ma format `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`. **Usuń część `api://` wartości zakresu.** Ten problem zostanie rozwiązany w przyszłej wersji zapoznawczej.

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> Domyślny zakres tokenu dostępu musi mieć format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (na przykład `11111111-1111-1111-1111-111111111111/API.Access`). Jeśli schemat lub schemat i Host są dostarczane do ustawienia zakresu (jak pokazano w witrynie Azure Portal), *aplikacja kliencka* zgłasza nieobsłużony wyjątek, gdy odbierze *401 nieautoryzowaną* odpowiedź z *aplikacji interfejsu API serwera*.

Metoda `AddMsalAuthentication` akceptuje wywołanie zwrotne w celu skonfigurowania parametrów wymaganych do uwierzytelnienia aplikacji. Wartości wymagane do skonfigurowania aplikacji można uzyskać z konfiguracji usługi AAD w witrynie Azure Portal podczas rejestrowania aplikacji.

Domyślne zakresy tokenów dostępu reprezentują listę zakresów tokenów dostępu, które są następujące:

* Uwzględnione domyślnie w żądaniu logowania.
* Służy do aprowizacji tokenu dostępu zaraz po uwierzytelnieniu.

Wszystkie zakresy muszą należeć do tej samej aplikacji na Azure Active Directory reguł. Dodatkowe zakresy można dodać do dodatkowych aplikacji interfejsu API w razie potrzeby:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{SCOPE}");
});
```

### <a name="index-page"></a>Strona indeksu

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a>Składnik aplikacji

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>Składnik RedirectToLogin

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>Składnik LoginDisplay

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a>Składnik uwierzytelniania

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>Składnik FetchData

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/authentication/azure-active-directory/index>
