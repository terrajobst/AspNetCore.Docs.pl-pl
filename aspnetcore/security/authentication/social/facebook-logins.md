---
title: Ustawienia zewnętrznej nazwy logowania usługi Facebook w programie ASP.NET Core
author: rick-anderson
description: Samouczek z przykładami kodu, demonstrując integracja uwierzytelniania użytkownika konta serwisu Facebook do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/04/2019
uid: security/authentication/facebook-logins
ms.openlocfilehash: 84b891f6cee71a26726d49a9d42ae45007d2b429
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705500"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Ustawienia zewnętrznej nazwy logowania usługi Facebook w programie ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten samouczek z przykładami kodu pokazuje, jak umożliwić użytkownikom logowanie się przy użyciu swojego konta w serwisie Facebook przy użyciu przykładowy projekt programu ASP.NET Core 2.0, utworzony na [poprzedniej strony](xref:security/authentication/social/index). Zaczniemy od utworzenia identyfikator aplikacji Facebook, postępując zgodnie z [oficjalnych kroków](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Tworzenie aplikacji w usłudze Facebook

* Przejdź do [aplikacji usługi Facebook deweloperów](https://developers.facebook.com/apps/) strony, a następnie zaloguj się. Jeśli nie masz jeszcze konta w serwisie Facebook, użyj **zarejestrować się za pomocą konta Facebook** łącze na stronie logowania, aby go utworzyć.

* Naciśnij pozycję **Dodaj nową aplikację** przycisk w prawym górnym rogu, aby utworzyć nowy identyfikator aplikacji.

   ![Facebook dla portalu deweloperów Otwórz w programie Microsoft Edge](index/_static/FBMyApps.png)

* Wypełnij formularz, a następnie naciśnij przycisk **utworzyć identyfikator aplikacji** przycisku.

  ![Tworzenie formularza nowego Identyfikatora aplikacji](index/_static/FBNewAppId.png)

* Na **wybierz produkt** kliknij **Set Up** na **logowania do usługi Facebook** karty.

  ![Strona Ustawienia produktu](index/_static/FBProductSetup.png)

* **Szybki Start** z zostanie otwarty Kreator **wybierz platformę** jako pierwszej strony. Pomiń Kreatora teraz, klikając **ustawienia** łącza w menu po lewej stronie:

  ![Pomiń Szybki Start](index/_static/FBSkipQuickStart.png)

* Zostanie wyświetlona **ustawienia uwierzytelniania OAuth klienta** strony:

  ![Strona Ustawienia uwierzytelniania OAuth klienta](index/_static/FBOAuthSetup.png)

* Wprowadź identyfikator URI programistycznych dzięki */signin-facebook* dołączany do **prawidłowe OAuth identyfikatory URI przekierowań** pola (na przykład: `https://localhost:44320/signin-facebook`). Uwierzytelnianie serwisu Facebook skonfigurowane w dalszej części tego samouczka będzie automatycznie obsługiwać żądań w */signin-facebook* trasy do zaimplementowania przepływu uwierzytelniania OAuth.

> [!NOTE]
> Identyfikator URI */signin-facebook* jest ustawiony jako domyślny wywołania zwrotnego dostawcy uwierzytelniania serwisu Facebook. Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowanie pośredniczące uwierzytelniania serwisu Facebook za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) Klasa.

* Kliknij przycisk **Zapisz zmiany**.

* Kliknij przycisk **ustawienia** > **podstawowe** link w obszarze nawigacji po lewej stronie.

  Na tej stronie, zwróć uwagę na Twoje `App ID` i `App Secret`. Doda zarówno do aplikacji platformy ASP.NET Core w następnej sekcji:

* W przypadku wdrażania w witrynie konieczne ponownie przeanalizowanie **logowania do usługi Facebook** strona instalacji i zarejestrować nowy identyfikator URI publicznego.

## <a name="store-facebook-app-id-and-app-secret"></a>Identyfikator aplikacji Facebook Store i klucza tajnego aplikacji

Link ustawień poufnych, takich jak Facebook `App ID` i `App Secret` z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets). Do celów tego samouczka, nazwa tokeny `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Wykonaj następujące polecenia, aby bezpiecznie przechowywać `App ID` i `App Secret` za pomocą Menedżera klucz tajny:

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Konfigurowanie uwierzytelniania serwisu Facebook

::: moniker range=">= aspnetcore-2.0"

Dodawanie usługi Facebook w `ConfigureServices` method in Class metoda *Startup.cs* pliku:

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Zainstaluj [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pakietu.

* Aby zainstalować ten pakiet za pomocą programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.
* Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

Dodaj oprogramowaniu pośredniczącym usługi Facebook w `Configure` method in Class metoda *Startup.cs* pliku:

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

Zobacz [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie serwisu Facebook. Opcje konfiguracji może służyć do:

* Żądanie różne informacje o użytkowniku.
* Dodaj argumenty ciągu zapytania dostosowywaniu środowiska logowania.

## <a name="sign-in-with-facebook"></a>Zaloguj się za pomocą usługi Facebook

Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**. Zostanie wyświetlona opcja logowania się za pomocą usługi Facebook.

![Aplikacja sieci Web: Użytkownik nie jest uwierzytelniony](index/_static/DoneFacebook.png)

Po kliknięciu **Facebook**, nastąpi przekierowanie do usługi Facebook do uwierzytelniania:

![Strona uwierzytelniania serwisu Facebook](index/_static/FBLogin.png)

Uwierzytelnianie serwisu Facebook żądań publicznego profilu i adres e-mail, domyślnie:

![Ekranie wyrażania zgody strony uwierzytelniania usługi Facebook](index/_static/FBLoginDone.png)

Po wprowadzeniu poświadczeń serwisu Facebook nastąpi przekierowanie do witryny, w którym można ustawić swój adres e-mail.

Obecnie zalogowano Cię przy użyciu poświadczeń serwisu Facebook:

![Aplikacja sieci Web: Użytkownik uwierzytelniony](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* **ASP.NET Core 2.x tylko:** Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: Opcja "SignInScheme" musi być podana*. Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, możesz uzyskać *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.

## <a name="next-steps"></a>Następne kroki

* Dodaj [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pakiet NuGet do projektu w przypadku zaawansowanych scenariuszy uwierzytelniania serwisu Facebook. Ten pakiet nie jest wymagane do integracji funkcji zewnętrznej nazwy logowania usługi Facebook z aplikacją. 

* W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Facebook. Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).

* Gdy publikujesz witrynę sieci web do aplikacji sieci web platformy Azure, należy zresetować `AppSecret` w portalu dla deweloperów usługi Facebook.

* Ustaw `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret` jako ustawienia aplikacji w witrynie Azure portal. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.
