---
title: Konfiguracja zewnętrznych logowania za pomocą platformy ASP.NET Core w usłudze Twitter
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta usługi Twitter do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: d816ed27898639b0af6896a51ac035d5526c5d29
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814070"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Konfiguracja zewnętrznych logowania za pomocą platformy ASP.NET Core w usłudze Twitter

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym przykładzie pokazano, jak umożliwić użytkownikom [Zaloguj się przy użyciu konta w serwisie Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) przy użyciu przykładowy projekt programu ASP.NET Core 2.2 tworzone na [poprzedniej strony](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Tworzenie aplikacji w usłudze Twitter

* Przejdź do [ https://apps.twitter.com/ ](https://apps.twitter.com/) i zaloguj się. Jeśli nie masz jeszcze konta w serwisie Twitter, użyj **[Zarejestruj się teraz](https://twitter.com/signup)** łącze, aby go utworzyć.

* Naciśnij pozycję **Utwórz nową aplikację** i wypełnij zgłoszenie **nazwa**, **opis** i publicznych **witryny sieci Web** identyfikatora URI (może to być tymczasowy do momentu Zarejestruj nazwy domeny):

* Wprowadź identyfikator URI programistycznych dzięki `/signin-twitter` dołączany do **prawidłowe OAuth identyfikatory URI przekierowań** pola (na przykład: `https://webapp128.azurewebsites.net/signin-twitter`). Schemat uwierzytelniania usługi Twitter, skonfigurować je później w tym przykładzie będzie automatycznie obsługiwać żądań w `/signin-twitter` trasy do zaimplementowania przepływu uwierzytelniania OAuth.

  > [!NOTE]
  > Segmentem identyfikatora URI `/signin-twitter` jest ustawiony jako domyślny wywołania zwrotnego dostawcy uwierzytelniania usługi Twitter. Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania usługi Twitter za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) Klasa.

* Wypełnij pozostałej części formularza, a następnie naciśnij pozycję **tworzenie aplikacji usługi Twitter**. Wyświetlane są szczegóły nowej aplikacji:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Przechowywanie klucza interfejsu API klienta usługi Twitter i wpisu tajnego

Uruchom następujące polecenia, aby bezpiecznie przechowywać `ClientId` i `ClientSecret` przy użyciu [Menedżera klucz tajny](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerAPISecret <Secret>
```

Link ustawień poufnych, takich jak Twitter `Consumer Key` i `Consumer Secret` z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets). Do celów tego przykładu, nazwa tokeny `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret`.

Tokeny te można znaleźć na **klucze i tokeny dostępu** kartę po utworzeniu nowej aplikacji usługi Twitter:

## <a name="configure-twitter-authentication"></a>Konfigurowanie uwierzytelniania usługi Twitter

Dodaj usługę Twitter w `ConfigureServices` method in Class metoda *Startup.cs* pliku:

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Zobacz [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianiem usługi Twitter. Może to służyć do żądania różne informacje o użytkowniku.

## <a name="sign-in-with-twitter"></a>Zaloguj się przy użyciu usługi Twitter

Uruchom aplikację i wybierz **Zaloguj**. Zostanie wyświetlona opcja Zaloguj się przy użyciu usługi Twitter:

Kliknięcie **Twitter** przekierowuje do usługi Twitter do uwierzytelniania:

Po wprowadzeniu swoje poświadczenia usługi Twitter, nastąpi przekierowanie do witryny sieci web, w którym można ustawić swój adres e-mail.

Obecnie zalogowano Cię przy użyciu poświadczeń usługi Twitter:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* **ASP.NET Core 2.x tylko:** Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: Opcja "SignInScheme" musi być podana*. Szablon projektu, używane w tym przykładzie gwarantuje, że odbywa się.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Twitter. Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).

* Gdy publikujesz witrynę sieci web do aplikacji sieci web platformy Azure, należy zresetować `ConsumerSecret` w portalu dla deweloperów usługi Twitter.

* Ustaw `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret` jako ustawienia aplikacji w witrynie Azure portal. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.