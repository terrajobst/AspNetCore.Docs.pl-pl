---
title: Konfigurowanie logowania zewnętrznego Account firmy Microsoft za pomocą programu ASP.NET Core
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta Microsoft do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 4909a0084994654777ad7a6ebda866ac727f0528
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735755"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Konfigurowanie logowania zewnętrznego Account firmy Microsoft za pomocą programu ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku dowiesz się, jak umożliwić użytkownikom logowanie się przy użyciu swojego konta Microsoft, przy użyciu przykładowy projekt programu ASP.NET Core 2.0, utworzony na [poprzedniej strony](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Tworzenie aplikacji w portalu dla deweloperów firmy Microsoft

* Przejdź do [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) i tworzyć ani logować się do konta Microsoft:

![Zaloguj się w oknie dialogowym](index/_static/MicrosoftDevLogin.png)

Jeśli nie masz jeszcze konta Microsoft, naciśnij przycisk  **[Zrób to!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** Po zarejestrowaniu się nastąpi przekierowanie do **aplikacje** strony:

![Otwórz w programie Microsoft Edge portalu dla deweloperów firmy Microsoft](index/_static/MicrosoftDev.png)

* Naciśnij pozycję **Dodaj aplikację** w prawym górnym rogu, a następnie wprowadź swoje **Nazwa aplikacji** i **E-mail kontaktowy**:

![Okno dialogowe nowej rejestracji aplikacji](index/_static/MicrosoftDevAppCreate.png)

* Do celów tego samouczka, należy wyczyścić **instrukcje konfiguracji** pole wyboru.

* Naciśnij pozycję **Utwórz** w dalszym ciągu **rejestracji** strony. Podaj **nazwa** i zanotuj wartość ustawienia **identyfikator aplikacji**, którego używasz jako `ClientId` później w samouczku:

![Strona rejestracji](index/_static/MicrosoftDevAppReg.png)

* Naciśnij pozycję **Dodaj platformy** w **platform** i wybierz pozycję **Web** platformy:

![Dodaj okno dialogowe platformy](index/_static/MicrosoftDevAppPlatform.png)

* W nowym **Web** platformy sekcji, wprowadź adres URL programowania za pomocą `/signin-microsoft` dołączany do **adresy URL przekierowania** pola (na przykład: `https://localhost:44320/signin-microsoft`). Schemat uwierzytelniania firmy Microsoft, które są skonfigurowane w dalszej części tego samouczka będzie automatycznie obsługiwać żądań w `/signin-microsoft` trasy do zaimplementowania przepływu uwierzytelniania OAuth:

![Sekcja platformy sieci Web](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> Segmentem identyfikatora URI `/signin-microsoft` jest ustawiony jako domyślny wywołania zwrotnego dostawcy uwierzytelniania firmy Microsoft. Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania firmy Microsoft za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) klasy.

* Naciśnij pozycję **Dodawanie adresu URL** aby upewnić się, adres URL został dodany.

* Wypełnij wszystkie ustawienia aplikacji, jeśli to konieczne, a następnie naciśnij przycisk **Zapisz** w dolnej części strony Aby zapisać zmiany w konfiguracji aplikacji.

* W przypadku wdrażania w witrynie będzie konieczne ponownie przeanalizowanie **rejestracji** strony, a następnie ustaw nowy publiczny adres URL.

## <a name="store-microsoft-application-id-and-password"></a>Store aplikacji firmy Microsoft, identyfikator i hasło

* Uwaga `Application Id` wyświetlany na **rejestracji** strony.

* Naciśnij pozycję **wygenerować nowe hasło** w **wpisów tajnych aplikacji** sekcji. Spowoduje to wyświetlenie okno, w którym kopiujesz hasło aplikacji:

![Nowe hasło wygenerowane okno dialogowe](index/_static/MicrosoftDevPassword.png)

Link ustawienia poufne, takie jak Microsoft `Application ID` i `Password` z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets). Do celów tego samouczka, nazwa tokeny `Authentication:Microsoft:ApplicationId` i `Authentication:Microsoft:Password`.

## <a name="configure-microsoft-account-authentication"></a>Skonfiguruj uwierzytelnianie za pomocą konta Microsoft

Szablon projektu, w tym samouczku używane zapewnia, że [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) pakiet jest już zainstalowany.

* Aby zainstalować ten pakiet za pomocą programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.
* Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

Dodaj usługę Microsoft Account w `ConfigureServices` method in Class metoda *Startup.cs* pliku:

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Dodaj oprogramowanie pośredniczące Account Microsoft w `Configure` method in Class metoda *Startup.cs* pliku:

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

Mimo że z terminologią używaną w portalu dla deweloperów firmy Microsoft, nazwy te tokeny `ApplicationId` i `Password`, są one widoczne jako `ClientId` i `ClientSecret` konfiguracji interfejsu API.

Zobacz [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie Account firmy Microsoft. Może to służyć do żądania różne informacje o użytkowniku.

## <a name="sign-in-with-microsoft-account"></a>Zaloguj się przy użyciu konta Microsoft

Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**. Zostanie wyświetlona opcja Zaloguj się przy użyciu konta Microsoft:

![Aplikacja sieci Web, dziennik na stronie: Użytkownik nie jest uwierzytelniony](index/_static/DoneMicrosoft.png)

Po kliknięciu firmy Microsoft są przekierowywane do firmy Microsoft do uwierzytelniania. Po zarejestrowaniu się przy użyciu Account firmy Microsoft (Jeśli nie zostało to zrobione) użytkownik jest monitowany, aby umożliwić aplikacji dostęp do informacji:

![Okno dialogowe uwierzytelniania firmy Microsoft](index/_static/MicrosoftLogin.png)

Naciśnij pozycję **tak** i nastąpi przekierowanie do witryny sieci web, w którym można ustawić swój adres e-mail.

Obecnie zalogowano Cię przy użyciu poświadczeń konta Microsoft:

![Aplikacja sieci Web: Użytkownik uwierzytelniony](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* Jeśli dostawca Account Microsoft przekieruje Cię do strony logowania w błąd, weź pod uwagę błąd tytuł i opis parametrów ciągu zapytania bezpośrednio po `#` (hasztag) w identyfikatorze Uri.

  Mimo, że komunikat o błędzie wydaje się, że wskazywać na problem z uwierzytelniania firmy Microsoft, Najczęstszą przyczyną jest niezgodne żadnego z identyfikatora Uri aplikacji **identyfikatory URI przekierowań** określony dla **Web** platformy .
* **ASP.NET Core 2.x tylko:** Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: Opcja "SignInScheme" musi być podana*. Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać z firmą Microsoft. Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).

* Gdy publikujesz witrynę sieci web do aplikacji sieci web platformy Azure, należy utworzyć nową `Password` w portalu dla deweloperów firmy Microsoft.

* Ustaw `Authentication:Microsoft:ApplicationId` i `Authentication:Microsoft:Password` jako ustawienia aplikacji w witrynie Azure portal. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.
