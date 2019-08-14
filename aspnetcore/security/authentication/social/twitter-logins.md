---
title: Konfiguracja logowania zewnętrznego usługi Twitter za pomocą ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono integrację uwierzytelniania użytkownika konta usługi Twitter z istniejącą aplikacją ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 6b6fa3e50f602a92fec9112ac3ba43583de33a70
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994283"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Konfiguracja logowania zewnętrznego usługi Twitter za pomocą ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten przykład pokazuje, jak umożliwić użytkownikom zalogowanie się przy użyciu [konta usługi Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) za pomocą przykładowego projektu ASP.NET Core 2,2 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Tworzenie aplikacji w usłudze Twitter

* Przejdź do [ https://apps.twitter.com/ ](https://apps.twitter.com/) i zaloguj się. Jeśli nie masz jeszcze konta usługi Twitter, Użyj linku Utwórz **[teraz](https://twitter.com/signup)** , aby go utworzyć.

* Naciśnij pozycję **Utwórz nową aplikację** i wypełnij pola **Nazwa**aplikacji, **Opis** i publiczny identyfikator URI **witryny sieci Web** (może to być czas tymczasowy do momentu zarejestrowania nazwy domeny):

* Wprowadź identyfikator URI programowania z `/signin-twitter` dołączonym do **prawidłowego pola URI przekierowania OAuth** (na przykład: `https://webapp128.azurewebsites.net/signin-twitter`). Schemat uwierzytelniania usługi Twitter skonfigurowany w dalszej części tego przykładu będzie automatycznie `/signin-twitter` obsługiwał żądania w marszrucie w celu zaimplementowania przepływu OAuth.

  > [!NOTE]
  > Segment `/signin-twitter` identyfikatora URI jest ustawiany jako domyślne wywołanie zwrotne dostawcy uwierzytelniania w usłudze Twitter. Domyślny identyfikator URI wywołania zwrotnego można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania usługi Twitter za pomocą dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .

* Wypełnij resztę formularza i naciśnij pozycję **Utwórz aplikację w usłudze Twitter**. Wyświetlane są nowe szczegóły aplikacji:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Przechowywanie klucza i wpisu tajnego interfejsu API użytkownika usługi Twitter

Uruchom następujące polecenia, aby bezpiecznie przechowywać `ClientId` i `ClientSecret` używać [Menedżera](xref:security/app-secrets)wpisów tajnych:

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

Połącz poufne ustawienia, takie `Consumer Key` jak `Consumer Secret` Twitter i konfiguracja aplikacji, za pomocą [Menedżera](xref:security/app-secrets)wpisów tajnych. Na potrzeby tego przykładu Nazwij tokeny `Authentication:Twitter:ConsumerKey` i. `Authentication:Twitter:ConsumerSecret`

Te tokeny można znaleźć na karcie **klucze i tokeny dostępu** po utworzeniu nowej aplikacji usługi Twitter:

## <a name="configure-twitter-authentication"></a>Konfigurowanie uwierzytelniania w usłudze Twitter

Dodaj usługę `ConfigureServices` Twitter do metody w pliku *Startup.cs* :

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Zobacz Dokumentacja interfejsu API [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) , aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie w usłudze Twitter. Może to służyć do żądania różne informacje o użytkowniku.

## <a name="sign-in-with-twitter"></a>Zaloguj się przy użyciu usługi Twitter

Uruchom aplikację i wybierz pozycję **Zaloguj się**. Zostanie wyświetlona opcja zalogowania się przy użyciu usługi Twitter:

Klikanie przekierowania w usłudze **Twitter** do usługi Twitter w celu uwierzytelnienia:

Po wprowadzeniu poświadczeń usługi Twitter nastąpi przekierowanie do witryny sieci Web, w której można ustawić swój adres e-mail.

Użytkownik jest obecnie zalogowany przy użyciu poświadczeń usługi Twitter:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* **Tylko ASP.NET Core 2. x:** Jeśli tożsamość nie jest skonfigurowana przez `services.AddIdentity` wywołanie `ConfigureServices`w, próba uwierzytelnienia spowoduje powstanie *argumentuexception: Należy podać*opcję "SignInScheme". Szablon projektu używany w tym przykładzie zapewnia, że jest to gotowe.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelnić się w usłudze Twitter. Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).

* Po opublikowaniu witryny sieci Web w usłudze Azure Web App należy zresetować ją `ConsumerSecret` w portalu dla deweloperów w usłudze Twitter.

* Ustaw `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret` jako ustawienia aplikacji w witrynie Azure portal. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.
