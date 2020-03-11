---
title: Ustawienia zewnętrznej nazwy logowania usługi Facebook w programie ASP.NET Core
author: rick-anderson
description: Samouczek z przykładami kodu demonstrujących integrację uwierzytelniania użytkownika konta w serwisie Facebook w istniejącej aplikacji ASP.NET Core.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 12/02/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: 2e4cc04c6e7ff8e5f5701cc7f9ede73dbc1b4685
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667469"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Ustawienia zewnętrznej nazwy logowania usługi Facebook w programie ASP.NET Core

Autorzy [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten samouczek z przykładami kodu pokazuje, jak umożliwić użytkownikom zalogowanie się za pomocą konta w serwisie Facebook przy użyciu przykładowego projektu ASP.NET Core 3,0 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index). Zacznij od utworzenia identyfikatora aplikacji Facebook, wykonując [czynności urzędowe](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Tworzenie aplikacji w usłudze Facebook

* Dodaj pakiet NuGet [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) do projektu.

* Przejdź do strony [aplikacji deweloperzy serwisu Facebook](https://developers.facebook.com/apps/) i zaloguj się. Jeśli nie masz jeszcze konta w serwisie Facebook, utwórz je za pomocą linku rejestracji w usłudze **Facebook** na stronie logowania.  Gdy masz konto w serwisie Facebook, postępuj zgodnie z instrukcjami, aby zarejestrować się jako deweloper w serwisie Facebook.

* Z menu **Moje aplikacje** wybierz pozycję **Utwórz aplikację** , aby utworzyć nowy identyfikator aplikacji.

   ![Facebook dla portalu deweloperów Otwórz w programie Microsoft Edge](index/_static/FBMyApps.png)

* Wypełnij formularz i naciśnij przycisk **Utwórz identyfikator aplikacji** .

  ![Tworzenie formularza nowego Identyfikatora aplikacji](index/_static/FBNewAppId.png)

* Na nowej karcie aplikacji wybierz pozycję **Dodaj produkt**.  Na karcie **Logowanie w serwisie Facebook** kliknij pozycję **Konfiguruj** . 

  ![Strona Ustawienia produktu](index/_static/FBProductSetup.png)

* Zostanie uruchomiony Kreator **szybkiego startu** z **wybieraniem platformy** jako pierwszej strony. Pomiń kreatora teraz, klikając link **Ustawienia** **logowania w serwisie Facebook** w menu po lewej stronie:

  ![Pomiń Szybki Start](index/_static/FBSkipQuickStart.png)

* Zostanie wyświetlona strona **ustawień uwierzytelniania OAuth klienta** :

  ![Strona Ustawienia uwierzytelniania OAuth klienta](index/_static/FBOAuthSetup.png)

* Wprowadź identyfikator URI programowania z */SignIn-facebooką* dołączoną do **prawidłowego pola URI przekierowania OAuth** (na przykład: `https://localhost:44320/signin-facebook`). Uwierzytelnianie w serwisie Facebook skonfigurowane w dalszej części tego samouczka będzie automatycznie obsługiwać żądania w marszrucie */SignIn-Facebook* w celu zaimplementowania przepływu OAuth.

> [!NOTE]
> Identyfikator URI */SignIn-Facebook* jest ustawiany jako domyślne wywołanie zwrotne dostawcy uwierzytelniania w serwisie Facebook. Domyślny identyfikator URI wywołania zwrotnego można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania w serwisie Facebook za pośrednictwem dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) .

* Kliknij przycisk **Save Changes** (Zapisz zmiany).

* Kliknij pozycję **ustawienia** > łącze **podstawowe** w lewym okienku nawigacji.

  Na tej stronie Zanotuj `App ID` i `App Secret`. Doda zarówno do aplikacji platformy ASP.NET Core w następnej sekcji:

* Podczas wdrażania witryny należy ponownie odwiedzić stronę instalatora logowania do **serwisu Facebook** i zarejestrować nowy publiczny identyfikator URI.

## <a name="store-facebook-app-id-and-app-secret"></a>Identyfikator aplikacji Facebook Store i klucza tajnego aplikacji

Połącz poufne ustawienia, takie jak Facebook `App ID` i `App Secret` z konfiguracją aplikacji za pomocą [Menedżera wpisów tajnych](xref:security/app-secrets). Na potrzeby tego samouczka Nazwij tokeny `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Wykonaj następujące polecenia, aby bezpiecznie przechowywać `App ID` i `App Secret` przy użyciu Menedżera wpisów tajnych:

```dotnetcli
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Konfigurowanie uwierzytelniania serwisu Facebook

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

Zobacz Dokumentacja interfejsu API [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) , aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie w serwisie Facebook. Opcje konfiguracji może służyć do:

* Żądanie różne informacje o użytkowniku.
* Dodaj argumenty ciągu zapytania dostosowywaniu środowiska logowania.

## <a name="sign-in-with-facebook"></a>Zaloguj się za pomocą usługi Facebook

Uruchom aplikację i kliknij pozycję **Zaloguj**. Zostanie wyświetlona opcja logowania się za pomocą usługi Facebook.

![Aplikacja sieci Web: użytkownik nie jest uwierzytelniony](index/_static/DoneFacebook.png)

Gdy klikniesz pozycję **Facebook**, nastąpi przekierowanie do usługi Facebook w celu uwierzytelnienia:

![Strona uwierzytelniania serwisu Facebook](index/_static/FBLogin.png)

Uwierzytelnianie serwisu Facebook żądań publicznego profilu i adres e-mail, domyślnie:

![Ekranie wyrażania zgody strony uwierzytelniania usługi Facebook](index/_static/FBLoginDone.png)

Po wprowadzeniu poświadczeń serwisu Facebook nastąpi przekierowanie do witryny, w którym można ustawić swój adres e-mail.

Obecnie zalogowano Cię przy użyciu poświadczeń serwisu Facebook:

![Aplikacja sieci Web: użytkownik uwierzytelniony](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* **Tylko ASP.NET Core 2. x:** Jeśli tożsamość nie jest konfigurowana przez wywołanie `services.AddIdentity` w `ConfigureServices`, próba uwierzytelnienia spowoduje powstanie *argumentu ArgumentException: należy podać opcję "SignInScheme"* . Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.
* Jeśli baza danych lokacji nie została utworzona przez zastosowanie początkowej migracji, podczas *przetwarzania błędu żądania nie można wykonać operacji bazy danych* . Naciśnij pozycję **Zastosuj migracje** , aby utworzyć bazę danych i odświeżyć, aby kontynuować z powodu błędu.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Facebook. Podobne podejście można wykonać w celu uwierzytelnienia z innymi dostawcami wymienionymi na [poprzedniej stronie](xref:security/authentication/social/index).

* Po opublikowaniu witryny sieci Web w usłudze Azure Web App należy zresetować `AppSecret` w portalu dla deweloperów serwisu Facebook.

* Ustaw `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret` jako ustawienia aplikacji w Azure Portal. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.
