---
title: "Ustawienia logowania zewnętrznego w usłudze Twitter"
author: rick-anderson
description: "W tym samouczku przedstawiono integracji usługi Twitter konta użytkownika uwierzytelniania do istniejącej aplikacji platformy ASP.NET Core."
keywords: Platformy ASP.NET Core, Twitter, logowania, uwierzytelniania
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: E5931607-31C0-4B20-B416-85E3550F5EA8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: 6751b34b42007cffa9ee92ee49170564b8eac997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="configuring-twitter-authentication"></a>Konfigurowanie uwierzytelniania usługi Twitter

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku przedstawiono sposób użytkownicy mogli [Zaloguj się przy użyciu swojego konta w serwisie Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) przy użyciu przykładowego projektu programu ASP.NET 2.0 Core utworzony na [poprzedniej strony](index.md).

## <a name="create-the-app-in-twitter"></a>Tworzenie aplikacji w usłudze Twitter

* Przejdź do [https://apps.twitter.com/](https://apps.twitter.com/) i zaloguj się. Jeśli nie masz jeszcze konta w usłudze Twitter, użyj  **[Zamów teraz](https://twitter.com/signup)**  łącze, aby go utworzyć. Po zalogowaniu, **Zarządzanie aplikacjami** wyświetlana jest strona:

![Zarządzanie aplikacjami Twitter Otwórz w programie Microsoft Edge](index/_static/TwitterAppManage.png)

* Wybierz **Utwórz nową aplikację** i wypełnianie aplikacji **nazwa**, **opis** i publiczne **witryny sieci Web** (może to być tymczasowy aż do identyfikatora URI Zarejestruj nazwę domeny):

![Tworzenie strony aplikacji](index/_static/TwitterCreate.png)

* Wprowadź identyfikator URI programowania z */signin-twitter* dołączany do **prawidłowy OAuth identyfikator URI przekierowania** pola (na przykład: `https://localhost:44320/signin-twitter`). Schemat uwierzytelniania Twitter skonfigurowane w dalszej części tego samouczka automatycznie będzie obsługiwać żądań */signin-twitter* trasy do zaimplementowania przepływu OAuth.

* Wypełnij pozostałej części formularza i wybierz **tworzenie aplikacji Twitter**. Wyświetlane są szczegóły nowej aplikacji:

![Karta Szczegóły na stronie aplikacji](index/_static/TwitterAppDetails.png)

* W przypadku wdrażania lokacji należy ponownie **Zarządzanie aplikacjami** strony i Zarejestruj nowy identyfikator URI publicznego.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Przechowywanie Twitter ConsumerKey i ConsumerSecret

Link ustawień poufnych, takich jak Twitter `Consumer Key` i `Consumer Secret` do swojej aplikacji konfiguracji za pomocą [Manager klucz tajny](../../app-secrets.md). Do celów tego samouczka, nazwa tokeny `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret`.

Tokeny te można znaleźć w **kluczy i tokenów dostępu** kartę po utworzeniu nowej aplikacji Twitter:

![Karta kluczy i tokeny dostępu](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Konfigurowanie uwierzytelniania usługi Twitter

Szablon projektu używany w tym samouczku upewnia się, że [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) pakiet jest już zainstalowany.

* Aby zainstalować ten pakiet przy użyciu programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**.
* Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Dodaj usługę Twitter w `ConfigureServices` metody w *Startup.cs* pliku:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

Dodaj oprogramowaniu pośredniczącym usługi Twitter w `Configure` metody w *Startup.cs* pliku:

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

Zobacz [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelniania usługi Twitter. To może być używane do żądania różne informacje o użytkowniku.

## <a name="sign-in-with-twitter"></a>Zaloguj się przy użyciu usługi Twitter

Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**. Pojawia się opcję, aby zalogować się przy użyciu usługi Twitter:

![Aplikacja sieci Web: użytkownik nie jest uwierzytelniony](index/_static/DoneTwitter.png)

Kliknięcie **Twitter** przekierowuje do usługi Twitter dla uwierzytelniania:

![Strona uwierzytelniania w usłudze Twitter](index/_static/TwitterLogin.png)

Po wprowadzeniu poświadczeń usługi Twitter, nastąpi przekierowanie do witryny sieci web, w którym można ustawić adres e-mail.

Użytkownik jest obecnie zalogowany przy użyciu poświadczeń konta usługi Twitter:

![Aplikacja sieci Web: użytkownik uwierzytelniony](index/_static/Done.png)

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* **Platformy ASP.NET Core tylko 2.x:** Jeśli tożsamość nie jest skonfigurowana przez wywołanie metody `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: należy podać opcję "SignInScheme"*. Szablon projektu używany w tym samouczku zapewnia to zrobić.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, otrzyma *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Wybierz **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować mimo błędu.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać z usługą Twitter. Można wykonać podobne podejścia do uwierzytelniania za pomocą innych dostawców wymienione na [poprzedniej strony](index.md).

* Po opublikowaniu witryny sieci web do aplikacji sieci web platformy Azure, należy zresetować `ConsumerSecret` w portalu dla deweloperów usługi Twitter.

* Ustaw `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret` zgodnie z ustawieniami aplikacji w portalu Azure. System konfiguracji jest skonfigurowany do odczytu klucze zmiennych środowiskowych.
