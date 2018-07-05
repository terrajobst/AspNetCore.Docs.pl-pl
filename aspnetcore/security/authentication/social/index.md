---
title: Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono sposób tworzenia platformy ASP.NET Core 2.x aplikacji przy użyciu protokołu OAuth 2.0 przy użyciu dostawcy uwierzytelniania zewnętrznego.
ms.author: riande
ms.date: 11/01/2016
uid: security/authentication/social/index
ms.openlocfilehash: b3fbd98215537fad7b283d1bf96ebd259e0b980a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366278"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku przedstawiono sposób tworzenia platformy ASP.NET Core 2.x aplikacji, która umożliwia użytkownikom zalogowanie się przy użyciu protokołu OAuth 2.0 przy użyciu poświadczeń z dostawcy uwierzytelniania zewnętrznego.

[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), i [Microsoft](xref:security/authentication/microsoft-logins) dostawców znajdują się w poniższych sekcjach. Innych dostawców są dostępne w innych pakietach przykład [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) i [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Ikony mediów społecznościowych usługi Facebook, Twitter, Google, plus i Windows](index/_static/social.png)

Umożliwienie użytkownikom logowania się za pomocą istniejących poświadczeń jest wygodne w przypadku użytkowników i przenosi wiele złożoności zarządzania procesu logowania na innej. Aby przykładów jak społecznościowych nazw logowania, może zapewnić konwersje typów ruchu i klienta, zobacz przypadków przez [Facebook](https://www.facebook.com/unsupportedbrowser) i [Twitter](https://dev.twitter.com/resources/case-studies).

Uwaga: Pakiety przedstawionych w tym miejscu abstrakcji dużym stopniem złożoności przepływu uwierzytelniania OAuth, ale zrozumienie szczegóły mogą okazać się niezbędne podczas rozwiązywania problemów. Wiele zasobów są dostępne; na przykład zobacz [wprowadzenie do protokołu OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) lub [opis protokołu OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/). Niektóre problemy można rozwiązać, analizując [kod źródłowy platformy ASP.NET Core pakiety dostawcy](https://github.com/aspnet/Security/tree/dev/src).

## <a name="create-a-new-aspnet-core-project"></a>Utwórz nowy projekt platformy ASP.NET Core

* W programie Visual Studio 2017, Utwórz nowy projekt z poziomu strony startowej lub za pośrednictwem **Plik > Nowy > Projekt**.

* Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu dostępnego w **Visual C# > .NET Core** kategorii:

![Okno dialogowe nowego projektu](index/_static/new-project.png)

* Naciśnij pozycję **aplikacji sieci Web** i sprawdź **uwierzytelniania** ustawiono **indywidualne konta użytkowników**:

![Okno dialogowe Nowy aplikacji sieci Web](index/_static/select-project.png)

Uwaga: Ten samouczek dotyczy wersji platformy ASP.NET Core 2.0 SDK, które można wybrać w górnej części kreatora.

## <a name="apply-migrations"></a>Zastosuj migracji

* Uruchom aplikację i wybierz **Zaloguj** łącza.
* Wybierz **Zarejestruj się jako nowy użytkownik** łącza.
* Wprowadź adres e-mail i hasło dla nowego konta, a następnie wybierz **zarejestrować**.
* Postępuj zgodnie z instrukcjami, aby zastosować migracji.

## <a name="require-ssl"></a>Wymagaj protokołu SSL

OAuth 2.0 wymaga użycia protokołu SSL do uwierzytelniania za pośrednictwem protokołu HTTPS.

Uwaga: Projekty utworzone za pomocą **aplikacji sieci Web** lub **interfejsu API sieci Web** szablony projektów dla platformy ASP.NET Core 2.x są konfigurowane automatycznie w celu włączenia protokołu SSL, a następnie uruchom za pomocą adresu URL https, jeśli **indywidualnych Konta użytkowników** została zaznaczona opcja **okno dialogowe Zmień uwierzytelnianie** w Kreatorze projektu, jak pokazano powyżej.

* Wymagaj protokołu SSL w lokacji, wykonując kroki opisane w [Wymuszanie protokołu SSL w aplikacji ASP.NET Core](xref:security/enforcing-ssl) tematu.

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>Użyj SecretManager do przechowywania tokenów przypisany przez dostawców logowania

Przypisz dostawców społecznościowych logowania **identyfikator aplikacji** i **klucz tajny aplikacji** tokenów podczas procesu rejestracji. Dokładne nazwy tokenu zależą od dostawcy. Tokeny te reprezentują poświadczenia aplikacja korzysta z dostępu do swojego interfejsu API. Tokeny stanowią "wpisy tajne", które mogą być połączone z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets#secret-manager). Menedżer klucz tajny jest alternatywą bardziej bezpieczne przechowywanie tokenów w pliku konfiguracji, takich jak *appsettings.json*.

> [!IMPORTANT]
> Menedżer klucz tajny jest tylko do celów programowania. Możesz przechowywać i chronić Azure testowych i produkcyjnych wpisów tajnych za pomocą [dostawcę konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration).

Postępuj zgodnie z instrukcjami w [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania w programie ASP.NET Core](xref:security/app-secrets) tematu do przechowywania tokenów przypisany przez każdego dostawcy logowania poniżej.

## <a name="setup-login-providers-required-by-your-application"></a>Konfigurowanie dostawców logowania, wymagane przez aplikację

Użyj poniższych tematów, aby skonfigurować aplikację do używania odpowiednich dostawców:

* [Facebook](xref:security/authentication/facebook-logins) instrukcje
* [W usłudze Twitter](xref:security/authentication/twitter-logins) instrukcje
* [Google](xref:security/authentication/google-logins) instrukcje
* [Microsoft](xref:security/authentication/microsoft-logins) instrukcje
* [Inni dostawcy](xref:security/authentication/otherlogins) instrukcje

[!INCLUDE[](~/includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a>Opcjonalnie Ustaw hasło

Po zarejestrowaniu się przy użyciu dostawcy logowania zewnętrznego, nie ma hasła zarejestrowane z aplikacją. Pozwala to uniknąć z tworzenia i zapamiętywanie hasła dla tej witryny, ale zapewnia także możesz zależne od dostawcy logowania zewnętrznego. Jeśli dostawcy logowania zewnętrznego jest niedostępny, nie można zalogować się do witryny sieci web.

Aby utworzyć hasło i zalogować się przy użyciu ustawioną podczas procesu logowania z zewnętrznych dostawców poczty e-mail:

* Naciśnij pozycję **Hello &lt;aliasu adresu e-mail&gt;**  link w prawym górnym rogu, aby przejść do **Zarządzaj** widoku.

![Widok zarządzania aplikacji sieci Web](index/_static/pass1a.png)

* Naciśnij pozycję **tworzenie**

![Ustawianie strony hasła](index/_static/pass2a.png)

* Ustawione prawidłowe hasło i umożliwia to logowanie za pomocą poczty e-mail.

## <a name="next-steps"></a>Następne kroki

* W tym artykule wprowadzono uwierzytelniania zewnętrznego i opisano wymagania wstępne dotyczące dodawania logowań zewnętrznych do aplikacji platformy ASP.NET Core.

* Odwołanie do strony właściwe dla dostawcy Konfigurowanie logowania dla dostawcy, wymagane przez aplikację.
