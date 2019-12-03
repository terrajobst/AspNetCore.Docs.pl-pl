---
title: Konfiguracja logowania zewnętrznego w serwisie Facebook w ASP.NET Core
author: rick-anderson
description: Samouczek z przykładami kodu demonstrujących integrację uwierzytelniania użytkownika konta w serwisie Facebook w istniejącej aplikacji ASP.NET Core.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 12/02/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: 2e4cc04c6e7ff8e5f5701cc7f9ede73dbc1b4685
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717133"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Konfiguracja logowania zewnętrznego w serwisie Facebook w ASP.NET Core

Autorzy [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten samouczek z przykładami kodu pokazuje, jak umożliwić użytkownikom zalogowanie się za pomocą konta w serwisie Facebook przy użyciu przykładowego projektu ASP.NET Core 3,0 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index). Zacznij od utworzenia identyfikatora aplikacji Facebook, wykonując [czynności urzędowe](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Tworzenie aplikacji w usłudze Facebook

* Dodaj pakiet NuGet [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) do projektu.

* Przejdź do strony [aplikacji deweloperzy serwisu Facebook](https://developers.facebook.com/apps/) i zaloguj się. Jeśli nie masz jeszcze konta w serwisie Facebook, utwórz je za pomocą linku rejestracji w usłudze **Facebook** na stronie logowania.  Gdy masz konto w serwisie Facebook, postępuj zgodnie z instrukcjami, aby zarejestrować się jako deweloper w serwisie Facebook.

* Z menu **Moje aplikacje** wybierz pozycję **Utwórz aplikację** , aby utworzyć nowy identyfikator aplikacji.

   ![Portal usługi Facebook dla deweloperów otwarty w przeglądarce Microsoft Edge](index/_static/FBMyApps.png)

* Wypełnij formularz i naciśnij przycisk **Utwórz identyfikator aplikacji** .

  ![Utwórz nowy formularz identyfikatora aplikacji](index/_static/FBNewAppId.png)

* Na nowej karcie aplikacji wybierz pozycję **Dodaj produkt**.  Na karcie **Logowanie w serwisie Facebook** kliknij pozycję **Konfiguruj** . 

  ![Strona konfiguracji produktu](index/_static/FBProductSetup.png)

* Zostanie uruchomiony Kreator **szybkiego startu** z **wybieraniem platformy** jako pierwszej strony. Pomiń kreatora teraz, klikając link **Ustawienia** **logowania w serwisie Facebook** w menu po lewej stronie:

  ![Pomiń Szybki start](index/_static/FBSkipQuickStart.png)

* Zostanie wyświetlona strona **ustawień uwierzytelniania OAuth klienta** :

  ![Strona ustawień uwierzytelniania OAuth klienta](index/_static/FBOAuthSetup.png)

* Wprowadź identyfikator URI programowania z */SignIn-facebooką* dołączoną do **prawidłowego pola URI przekierowania OAuth** (na przykład: `https://localhost:44320/signin-facebook`). Uwierzytelnianie w serwisie Facebook skonfigurowane w dalszej części tego samouczka będzie automatycznie obsługiwać żądania w marszrucie */SignIn-Facebook* w celu zaimplementowania przepływu OAuth.

> [!NOTE]
> Identyfikator URI */SignIn-Facebook* jest ustawiany jako domyślne wywołanie zwrotne dostawcy uwierzytelniania w serwisie Facebook. Domyślny identyfikator URI wywołania zwrotnego można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania w serwisie Facebook za pośrednictwem dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) .

* Kliknij przycisk **Zapisz zmiany**.

* Kliknij pozycję **ustawienia** > łącze **podstawowe** w lewym okienku nawigacji.

  Na tej stronie Zanotuj `App ID` i `App Secret`. Do aplikacji ASP.NET Core należy dodać w następnej sekcji:

* Podczas wdrażania witryny należy ponownie odwiedzić stronę instalatora logowania do **serwisu Facebook** i zarejestrować nowy publiczny identyfikator URI.

## <a name="store-facebook-app-id-and-app-secret"></a>Zapisz identyfikator aplikacji w serwisie Facebook i klucz tajny aplikacji

Połącz poufne ustawienia, takie jak Facebook `App ID` i `App Secret` z konfiguracją aplikacji za pomocą [Menedżera wpisów tajnych](xref:security/app-secrets). Na potrzeby tego samouczka Nazwij tokeny `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Wykonaj następujące polecenia, aby bezpiecznie przechowywać `App ID` i `App Secret` przy użyciu Menedżera wpisów tajnych:

```dotnetcli
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Konfigurowanie uwierzytelniania w usłudze Facebook

Dodaj usługę Facebook w `ConfigureServices` metodzie w pliku *Startup.cs* :

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Zobacz Dokumentacja interfejsu API [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) , aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie w serwisie Facebook. Opcje konfiguracji mogą służyć do:

* Zażądaj innych informacji o użytkowniku.
* Dodaj argumenty ciągu zapytania, aby dostosować środowisko logowania.

## <a name="sign-in-with-facebook"></a>Logowanie za pomocą usługi Facebook

Uruchom aplikację i kliknij pozycję **Zaloguj**. Zostanie wyświetlona opcja zalogowania się za pomocą usługi Facebook.

![Aplikacja sieci Web: użytkownik nie jest uwierzytelniony](index/_static/DoneFacebook.png)

Gdy klikniesz pozycję **Facebook**, nastąpi przekierowanie do usługi Facebook w celu uwierzytelnienia:

![Strona uwierzytelniania w serwisie Facebook](index/_static/FBLogin.png)

Profil publiczny i adres e-mail żądania uwierzytelniania w serwisie Facebook są domyślnie dostępne:

![Ekran wyrażania zgody na stronę uwierzytelniania w serwisie Facebook](index/_static/FBLoginDone.png)

Po wprowadzeniu poświadczeń usługi Facebook przekierowanych z powrotem do witryny, w której możesz ustawić swój adres e-mail.

Zalogowano Cię przy użyciu poświadczeń usługi Facebook:

![Aplikacja sieci Web: użytkownik uwierzytelniony](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* **Tylko ASP.NET Core 2. x:** Jeśli tożsamość nie jest konfigurowana przez wywołanie `services.AddIdentity` w `ConfigureServices`, próba uwierzytelnienia spowoduje powstanie *argumentu ArgumentException: należy podać opcję "SignInScheme"* . Szablon projektu używany w tym samouczku zapewnia, że jest to gotowe.
* Jeśli baza danych lokacji nie została utworzona przez zastosowanie początkowej migracji, podczas *przetwarzania błędu żądania nie można wykonać operacji bazy danych* . Naciśnij pozycję **Zastosuj migracje** , aby utworzyć bazę danych i odświeżyć, aby kontynuować z powodu błędu.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelnić się w usłudze Facebook. Podobne podejście można wykonać w celu uwierzytelnienia z innymi dostawcami wymienionymi na [poprzedniej stronie](xref:security/authentication/social/index).

* Po opublikowaniu witryny sieci Web w usłudze Azure Web App należy zresetować `AppSecret` w portalu dla deweloperów serwisu Facebook.

* Ustaw `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret` jako ustawienia aplikacji w Azure Portal. System konfiguracji jest skonfigurowany do odczytywania kluczy ze zmiennych środowiskowych.
