---
title: Konfiguracja logowania zewnętrznego usługi Twitter za pomocą ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono integrację uwierzytelniania użytkownika konta usługi Twitter z istniejącą aplikacją ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: 5d0695160d90d0c5d31b8e35bc6c4cc984829333
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944216"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Konfiguracja logowania zewnętrznego usługi Twitter za pomocą ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten przykład pokazuje, jak umożliwić użytkownikom [zalogowanie](https://dev.twitter.com/web/sign-in/desktop-browser) się przy użyciu konta usługi Twitter za pomocą przykładowego projektu ASP.NET Core 3,0 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Tworzenie aplikacji w usłudze Twitter

* Dodaj pakiet NuGet [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) do projektu.

* Przejdź do [ https://apps.twitter.com/ ](https://apps.twitter.com/) i zaloguj się. Jeśli nie masz jeszcze konta usługi Twitter, Użyj linku Utwórz **[teraz](https://twitter.com/signup)** , aby go utworzyć.

* Wybierz pozycję **Utwórz aplikację**. Wypełnij pola **Nazwa aplikacji**, **Opis aplikacji** i publiczny identyfikator URI **witryny sieci Web** (może to być czas tymczasowy do momentu zarejestrowania nazwy domeny):

* Wprowadź identyfikator URI programowania z `/signin-twitter` dołączony do pola **adresy URL wywołania zwrotnego** (na przykład: `https://webapp128.azurewebsites.net/signin-twitter`). Schemat uwierzytelniania usługi Twitter skonfigurowany w dalszej części tego przykładu będzie automatycznie obsługiwał żądania na trasie `/signin-twitter` w celu zaimplementowania przepływu OAuth.

  > [!NOTE]
  > Segment identyfikatora URI `/signin-twitter` jest ustawiany jako domyślne wywołanie zwrotne dostawcy uwierzytelniania w usłudze Twitter. Domyślny identyfikator URI wywołania zwrotnego można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania usługi Twitter za pomocą dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .

* Wypełnij resztę formularza i wybierz pozycję **Utwórz**. Wyświetlane są nowe szczegóły aplikacji:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Przechowywanie klucza i wpisu tajnego interfejsu API użytkownika usługi Twitter

Uruchom następujące polecenia, aby bezpiecznie przechowywać `ClientId` i `ClientSecret` przy użyciu [Menedżera wpisów tajnych](xref:security/app-secrets):

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

Połącz poufne ustawienia, takie jak Twitter `Consumer Key` i `Consumer Secret` z konfiguracją aplikacji za pomocą [Menedżera wpisów tajnych](xref:security/app-secrets). Na potrzeby tego przykładu Nazwij tokeny `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret`.

Te tokeny można znaleźć na karcie **klucze i tokeny dostępu** po utworzeniu nowej aplikacji usługi Twitter:

## <a name="configure-twitter-authentication"></a>Konfigurowanie uwierzytelniania w usłudze Twitter

Dodaj usługę Twitter do metody `ConfigureServices` w pliku *Startup.cs* :

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-14)]

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

* **Platforma ASP.NET Core 2.x tylko:** Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: należy podać opcję "SignInScheme"* . Szablon projektu używany w tym przykładzie zapewnia, że jest to gotowe.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelnić się w usłudze Twitter. Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).

* Po opublikowaniu witryny sieci Web w usłudze Azure Web App należy zresetować `ConsumerSecret` w portalu dla deweloperów w usłudze Twitter.

* Ustaw `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret` jako ustawienia aplikacji w witrynie Azure portal. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.
