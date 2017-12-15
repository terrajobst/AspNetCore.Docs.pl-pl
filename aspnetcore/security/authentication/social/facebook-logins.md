---
title: "Ustawienia logowania zewnętrznego Facebook w ASP.NET Core"
author: rick-anderson
description: "W tym samouczku przedstawiono integrację uwierzytelniania użytkownika serwisu Facebook konta do istniejącej aplikacji platformy ASP.NET Core."
keywords: Platformy ASP.NET Core, Facebook, logowania, uwierzytelniania
ms.author: riande
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.assetid: 8c65179b-688c-4af1-8f5e-1862920cda95
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/facebook-logins
ms.openlocfilehash: 058670b4f699288e1acbe76bae08dcebf69346b8
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="configuring-facebook-authentication"></a>Konfigurowanie uwierzytelniania usługi Facebook

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku przedstawiono sposób umożliwić użytkownikom logowanie za pomocą swojego konta usługi Facebook za pomocą przykładowy projekt platformy ASP.NET Core 2.0, utworzony na [poprzedniej strony](index.md). Firma Microsoft Rozpocznij od utworzenia identyfikator aplikacji usługi Facebook, wykonując [oficjalnego kroki](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Tworzenie aplikacji w usłudze Facebook

*  Przejdź do [aplikacji usługi Facebook deweloperzy](https://developers.facebook.com/apps/) strony i zaloguj się. Jeśli nie masz jeszcze konta w usłudze Facebook, użyj **Załóż Facebook** łącze na stronie logowania, aby go utworzyć.

* Wybierz **Dodaj nową aplikację** przycisk w prawym górnym rogu, aby utworzyć nowy identyfikator aplikacji.

   ![Otwórz Facebook dla portalu deweloperów w programie Microsoft Edge](index/_static/FBMyApps.png)

* Wypełnij formularz, a następnie naciśnij pozycję **utworzyć identyfikator aplikacji** przycisku.

   ![Tworzenie formularza nowy identyfikator aplikacji](index/_static/FBNewAppId.png)

* Na **wybierz produkt** kliknij przycisk **Set Up** na **logowania serwisu Facebook** karty.

   ![Strona instalacji produktu](index/_static/FBProductSetup.png)
  
* **Szybkiego startu** z zostanie otwarty Kreator **wybierz platformę** jako pierwszej strony. Pomiń Kreatora teraz klikając **ustawienia** łącza w menu po lewej stronie:

   ![Pomiń Szybki Start](index/_static/FBSkipQuickStart.png)

* Dostępne są **ustawień klienta OAuth** strony:

![Strona Ustawienia OAuth klienta](index/_static/FBOAuthSetup.png)

* Wprowadź identyfikator URI programowania z */signin-facebook* dołączany do **prawidłowy OAuth identyfikator URI przekierowania** pola (na przykład: `https://localhost:44320/signin-facebook`). Uwierzytelniania serwisu Facebook skonfigurowane w dalszej części tego samouczka automatycznie będzie obsługiwać żądań */signin-facebook* trasy do zaimplementowania przepływu OAuth.

* Kliknij przycisk **zapisać zmiany**.

* Kliknij przycisk **pulpitu nawigacyjnego** łącza na lewym pasku nawigacyjnym. 

    Na tej stronie, zanotuj Twojej `App ID` i `App Secret`. Doda zarówno do aplikacji platformy ASP.NET Core w następnej sekcji:

   ![Pulpit nawigacyjny dewelopera usługi Facebook](index/_static/FBDashboard.png)

* W przypadku wdrażania lokacji należy ponownie **logowania serwisu Facebook** strona instalacji i Zarejestruj nowy identyfikator URI publicznego.

## <a name="store-facebook-app-id-and-app-secret"></a>Identyfikator aplikacji Facebook magazynu i klucz tajny aplikacji

Link ustawień poufnych, takich jak Facebook `App ID` i `App Secret` do swojej aplikacji konfiguracji za pomocą [Manager klucz tajny](xref:security/app-secrets). Do celów tego samouczka, nazwa tokeny `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret`.

Wykonaj następujące polecenia, aby bezpiecznie przechowywać `App ID` i `App Secret` za pomocą Menedżera klucz tajny:

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Konfigurowanie uwierzytelniania serwisu Facebook

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Dodaj usługę Facebook w `ConfigureServices` metody w *Startup.cs* pliku:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

Zainstaluj [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pakietu.

* Aby zainstalować ten pakiet przy użyciu programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**.
* Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

Dodaj oprogramowaniu pośredniczącym usługi Facebook w `Configure` metody w *Startup.cs* pliku:

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

Zobacz [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelniania serwisu Facebook. Opcje konfiguracji może służyć do:

* Żądanie różne informacje o użytkowniku.
* Dodaj argumenty ciągu zapytania dostosowywaniu środowiska logowania.

## <a name="sign-in-with-facebook"></a>Zaloguj się za pomocą usługi Facebook

Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**. Widzisz opcję, aby zalogować się za pomocą usługi Facebook.

![Aplikacja sieci Web: użytkownik nie jest uwierzytelniony](index/_static/DoneFacebook.png)

Po kliknięciu **Facebook**, nastąpi przekierowanie do usługi Facebook dla uwierzytelniania:

![Strona uwierzytelniania serwisu Facebook](index/_static/FBLogin.png)

Uwierzytelniania serwisu Facebook żądań publicznego profilu i adres e-mail domyślnie:

![Strona uwierzytelniania serwisu Facebook](index/_static/FBLoginDone.png)

Po wprowadzeniu poświadczeń usługi Facebook nastąpi przekierowanie do witryny, w którym można ustawić adres e-mail.

Teraz jest zalogowany przy użyciu poświadczeń usługi Facebook:

![Aplikacja sieci Web: użytkownik uwierzytelniony](index/_static/Done.png)

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* **Platformy ASP.NET Core tylko 2.x:** Jeśli tożsamość nie jest skonfigurowana przez wywołanie metody `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: należy podać opcję "SignInScheme"*. Szablon projektu używany w tym samouczku zapewnia to zrobić.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, możesz uzyskać *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Wybierz **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować mimo błędu.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać z serwisem Facebook. Można wykonać podobne podejścia do uwierzytelniania za pomocą innych dostawców wymienione na [poprzedniej strony](index.md).

* Po opublikowaniu witryny sieci web do aplikacji sieci web platformy Azure, należy zresetować `AppSecret` w portalu dla deweloperów usługi Facebook.

* Ustaw `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret` zgodnie z ustawieniami aplikacji w portalu Azure. System konfiguracji jest skonfigurowany do odczytu klucze zmiennych środowiskowych.
