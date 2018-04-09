---
title: Konfigurowanie logowania zewnętrznego Account firmy Microsoft z platformy ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono integrację uwierzytelnianie użytkownika konta Microsoft do istniejącej aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/microsoft-logins
ms.openlocfilehash: aabbbe66aee8c8b93140bcc4181b432017cec1d7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Konfigurowanie logowania zewnętrznego Account firmy Microsoft z platformy ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku przedstawiono sposób umożliwić użytkownikom logowanie za pomocą swojego konta Microsoft, za pomocą przykładowy projekt platformy ASP.NET Core 2.0, utworzony na [poprzedniej strony](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Utwórz aplikację w portalu dla deweloperów firmy Microsoft

* Przejdź do [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) i utworzyć lub zaloguj się do konta Microsoft:

![Zaloguj się w oknie dialogowym](index/_static/MicrosoftDevLogin.png)

Jeśli nie masz już konto Microsoft, naciśnij przycisk  **[utwórz je!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** Po zalogowaniu użytkownik zostanie przekierowany do **aplikacje** strony:

![Otwórz w programie Microsoft Edge Microsoft Developer Portal](index/_static/MicrosoftDev.png)

* Wybierz **Dodaj aplikację** w prawym górnym rogu, a następnie wprowadź Twojej **Nazwa aplikacji** i **E-mail kontaktu**:

![Okno dialogowe nowego Rejestracja aplikacji](index/_static/MicrosoftDevAppCreate.png)

* Do celów tego samouczka, wyczyść **instrukcje konfiguracji** pole wyboru.

* Wybierz **Utwórz** w dalszym ciągu **rejestracji** strony. Podaj **nazwa** i zanotuj wartość **identyfikator aplikacji**, który jest używany jako `ClientId` dalszej części samouczka:

![Strony rejestracji](index/_static/MicrosoftDevAppReg.png)

* Wybierz **dodać platformy** w **platformy** a następnie wybierz opcję **Web** platformy:

![Platforma okno dialogowe Dodawanie](index/_static/MicrosoftDevAppPlatform.png)

* W nowym **Web** platformy wprowadź adres URL programowanie z */signin-microsoft* dołączany do **adresów URL przekierowań** pola (na przykład: `https://localhost:44320/signin-microsoft`). Schemat uwierzytelniania firmy Microsoft, skonfigurowane w dalszej części tego samouczka automatycznie będzie obsługiwać żądań */signin-microsoft* trasy do zaimplementowania przepływu OAuth:

![Sekcja platformy sieci Web](index/_static/MicrosoftRedirectUri.png)

* Wybierz **Dodaj adres URL** zapewnienie adres URL został dodany.

* Wypełnij wszystkie ustawienia aplikacji, w razie potrzeby, a następnie naciśnij pozycję **zapisać** w dolnej części strony, aby zapisać zmiany w konfiguracji aplikacji.

* W przypadku wdrażania lokacji należy ponownie **rejestracji** i ustaw nowy publiczny adres URL strony.

## <a name="store-microsoft-application-id-and-password"></a>Przechowywanie identyfikator aplikacji firmy Microsoft i hasła

* Uwaga `Application Id` wyświetlany na **rejestracji** strony.

* Wybierz **wygenerować nowe hasło** w **klucze tajne aplikacji** sekcji. Spowoduje to wyświetlenie pola, w którym można kopiować hasło aplikacji:

![Nowe hasło wygenerowane okno dialogowe](index/_static/MicrosoftDevPassword.png)

Link ustawień poufnych, takich jak Microsoft `Application ID` i `Password` do swojej aplikacji konfiguracji za pomocą [Manager klucz tajny](xref:security/app-secrets). Do celów tego samouczka, nazwa tokeny `Authentication:Microsoft:ApplicationId` i `Authentication:Microsoft:Password`.

## <a name="configure-microsoft-account-authentication"></a>Konfigurowanie uwierzytelniania konta Microsoft

Szablon projektu używany w tym samouczku upewnia się, że [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) pakiet jest już zainstalowany.

* Aby zainstalować ten pakiet przy użyciu programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**.
* Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
Dodaj usługę Microsoft Account w `ConfigureServices` metody w *Startup.cs* pliku:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Dodaj oprogramowanie pośredniczące Account Microsoft w `Configure` metody w *Startup.cs* pliku:

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

* * *
Chociaż te tokeny nazwy z terminologią używaną w portalu usługi Microsoft Developer `ApplicationId` i `Password`, są one widoczne jako `ClientId` i `ClientSecret` konfiguracji interfejsu API.

Zobacz [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie Account firmy Microsoft. To może być używane do żądania różne informacje o użytkowniku.

## <a name="sign-in-with-microsoft-account"></a>Zaloguj się przy użyciu konta Microsoft

Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**. Pojawia się opcję, aby zalogować się przy użyciu firmy Microsoft:

![Aplikacja dziennika na stronie sieci Web: użytkownik nie jest uwierzytelniony](index/_static/DoneMicrosoft.png)

Po kliknięciu firmy Microsoft są przekierowywane do firmy Microsoft do uwierzytelniania. Po zarejestrowaniu się za pomocą Account firmy Microsoft (Jeśli nie jest już zalogowany) pojawi się monit, aby umożliwić aplikacji dostęp do informacji:

![Dialog uwierzytelniania firmy Microsoft](index/_static/MicrosoftLogin.png)

Wybierz **tak** i nastąpi przekierowanie do witryny sieci web, w którym można ustawić adres e-mail.

Użytkownik jest obecnie zalogowany przy użyciu poświadczeń konta Microsoft:

![Aplikacja sieci Web: użytkownik uwierzytelniony](index/_static/Done.png)

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* Jeśli dostawca Account Microsoft przekierowuje do logowania strony błędu, weź pod uwagę błąd tytuł i opis parametrów ciągu zapytania bezpośrednio po `#` (hasztagiem) w identyfikatorze Uri.

  Mimo że komunikat o błędzie jest prawdopodobnie wskazywać na problem z uwierzytelniania firmy Microsoft, Najczęstszą przyczyną jest aplikacja nie zgodne z żadnym z identyfikatora Uri **identyfikator URI przekierowania** określona dla **Web** platformy .
* **Platformy ASP.NET Core tylko 2.x:** Jeśli tożsamości nie jest skonfigurowana pod wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: należy podać opcję "SignInScheme"*. Szablon projektu używany w tym samouczku zapewnia to zrobić.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, otrzyma *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Wybierz **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować mimo błędu.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać z firmą Microsoft. Można wykonać podobne podejścia do uwierzytelniania za pomocą innych dostawców wymienione na [poprzedniej strony](xref:security/authentication/social/index).

* Po opublikowaniu witryny sieci web do aplikacji sieci web platformy Azure, należy utworzyć nowy `Password` w portalu dla deweloperów firmy Microsoft.

* Ustaw `Authentication:Microsoft:ApplicationId` i `Authentication:Microsoft:Password` zgodnie z ustawieniami aplikacji w portalu Azure. System konfiguracji jest skonfigurowany do odczytu klucze zmiennych środowiskowych.
