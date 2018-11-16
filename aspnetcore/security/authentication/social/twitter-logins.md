---
title: Konfiguracja logowania zewnętrznego za pomocą platformy ASP.NET Core w usłudze Twitter
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta usługi Twitter do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 43a5ea59d8853d297ae2c1ec3f4b1c0c14ec80c3
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708429"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>Konfiguracja logowania zewnętrznego za pomocą platformy ASP.NET Core w usłudze Twitter

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku pokazano, jak umożliwić użytkownikom [Zaloguj się przy użyciu konta w serwisie Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) przy użyciu przykładowy projekt programu ASP.NET Core 2.0 tworzone na [poprzedniej strony](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Tworzenie aplikacji w usłudze Twitter

* Przejdź do [ https://apps.twitter.com/ ](https://apps.twitter.com/) i zaloguj się. Jeśli nie masz jeszcze konta w serwisie Twitter, użyj **[Zarejestruj się teraz](https://twitter.com/signup)** łącze, aby go utworzyć. Po zalogowaniu **Zarządzanie aplikacjami** wyświetlana strona:

  ![Zarządzanie aplikacjami usługi Twitter, Otwórz w programie Microsoft Edge](index/_static/TwitterAppManage.png)

* Naciśnij pozycję **Utwórz nową aplikację** i wypełnij zgłoszenie **nazwa**, **opis** i publicznych **witryny sieci Web** identyfikatora URI (może to być tymczasowy do momentu Zarejestruj nazwy domeny):

  ![Tworzenie strony aplikacji](index/_static/TwitterCreate.png)

* Wprowadź identyfikator URI programistycznych dzięki `/signin-twitter` dołączany do **prawidłowe OAuth identyfikatory URI przekierowań** pola (na przykład: `https://localhost:44320/signin-twitter`). Schemat uwierzytelniania usługi Twitter, które są skonfigurowane w dalszej części tego samouczka będzie automatycznie obsługiwać żądań w `/signin-twitter` trasy do zaimplementowania przepływu uwierzytelniania OAuth.

  > [!NOTE]
  > Segmentem identyfikatora URI `/signin-twitter` jest ustawiony jako domyślny wywołania zwrotnego dostawcy uwierzytelniania usługi Twitter. Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania usługi Twitter za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) Klasa.

* Wypełnij pozostałej części formularza, a następnie naciśnij pozycję **tworzenie aplikacji usługi Twitter**. Wyświetlane są szczegóły nowej aplikacji:

  ![Karta szczegółów na stronie aplikacji](index/_static/TwitterAppDetails.png)

* W przypadku wdrażania w witrynie będzie konieczne ponownie przeanalizowanie **Zarządzanie aplikacjami** strony, a następnie zarejestrować nowy identyfikator URI publicznego.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Przechowywanie ConsumerKey usługi Twitter i ConsumerSecret

Link ustawień poufnych, takich jak Twitter `Consumer Key` i `Consumer Secret` z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets). Do celów tego samouczka, nazwa tokeny `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret`.

Tokeny te można znaleźć na **klucze i tokeny dostępu** kartę po utworzeniu nowej aplikacji usługi Twitter:

![Karta klucze i tokeny dostępu](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Konfigurowanie uwierzytelniania usługi Twitter

Szablon projektu, w tym samouczku używane zapewnia, że [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) pakiet jest już zainstalowany.

* Aby zainstalować ten pakiet za pomocą programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.
* Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

Dodaj usługę Twitter w `ConfigureServices` method in Class metoda *Startup.cs* pliku:

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

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Dodaj oprogramowaniu pośredniczącym usługi Twitter w `Configure` method in Class metoda *Startup.cs* pliku:

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

Zobacz [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianiem usługi Twitter. Może to służyć do żądania różne informacje o użytkowniku.

## <a name="sign-in-with-twitter"></a>Zaloguj się przy użyciu usługi Twitter

Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**. Zostanie wyświetlona opcja Zaloguj się przy użyciu usługi Twitter:

![Aplikacja sieci Web: użytkownik nie jest uwierzytelniony](index/_static/DoneTwitter.png)

Kliknięcie **Twitter** przekierowuje do usługi Twitter do uwierzytelniania:

![Strona uwierzytelniania w usłudze Twitter](index/_static/TwitterLogin.png)

Po wprowadzeniu swoje poświadczenia usługi Twitter, nastąpi przekierowanie do witryny sieci web, w którym można ustawić swój adres e-mail.

Obecnie zalogowano Cię przy użyciu poświadczeń usługi Twitter:

![Aplikacja sieci Web: użytkownik uwierzytelniony](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* **Platforma ASP.NET Core 2.x tylko:** Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: należy podać opcję "SignInScheme"*. Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Twitter. Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).

* Gdy publikujesz witrynę sieci web do aplikacji sieci web platformy Azure, należy zresetować `ConsumerSecret` w portalu dla deweloperów usługi Twitter.

* Ustaw `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret` jako ustawienia aplikacji w witrynie Azure portal. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.
