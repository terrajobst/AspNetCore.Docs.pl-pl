---
title: Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono sposób tworzenia platformy ASP.NET Core 2.x aplikacji przy użyciu protokołu OAuth 2.0 przy użyciu dostawcy uwierzytelniania zewnętrznego.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/index
ms.openlocfilehash: 47ac1f966ff727957e6ed700c3c68efa16b1b38b
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/24/2018
ms.locfileid: "53735729"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku przedstawiono sposób tworzenia platformy ASP.NET Core 2.x aplikacji, która umożliwia użytkownikom zalogowanie się przy użyciu protokołu OAuth 2.0 przy użyciu poświadczeń z dostawcy uwierzytelniania zewnętrznego.

[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), i [Microsoft](xref:security/authentication/microsoft-logins) dostawców znajdują się w poniższych sekcjach. Innych dostawców są dostępne w innych pakietach przykład [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) i [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Ikony mediów społecznościowych usługi Facebook, Twitter, Google, plus i Windows](index/_static/social.png)

Umożliwienie użytkownikom logowania się za pomocą istniejących poświadczeń jest wygodne w przypadku użytkowników i przenosi wiele złożoności zarządzania procesu logowania na innej. Aby przykładów jak społecznościowych nazw logowania, może zapewnić konwersje typów ruchu i klienta, zobacz przypadków przez [Facebook](https://www.facebook.com/unsupportedbrowser) i [Twitter](https://dev.twitter.com/resources/case-studies).

## <a name="create-a-new-aspnet-core-project"></a>Utwórz nowy projekt platformy ASP.NET Core

* W programie Visual Studio 2017, Utwórz nowy projekt z poziomu strony startowej lub za pośrednictwem **pliku** > **New** > **projektu**.

* Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu dostępnego w **Visual C#**   >  **platformy .NET Core** kategorii:

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

Projekty utworzone za pomocą **aplikacji sieci Web** lub **interfejsu API sieci Web** projektu szablonów za pomocą platformy ASP.NET Core 2.1 lub nowszej są automatycznie konfigurowane do włączenia protokołu SSL. Uruchomieniu aplikacji przy użyciu domyślnego bezpiecznego punktu końcowego, jeśli **indywidualne konta użytkowników** jest zaznaczona opcja **okno dialogowe Zmień uwierzytelnianie** Kreatora projektu.

Aby uzyskać więcej informacji, zobacz <xref:security/enforcing-ssl>.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

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

[!INCLUDE[](includes/chain-auth-providers.md)]

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

* Można utrwalić dodatkowe dane dotyczące użytkownika i tokenami dostępu i odświeżania. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/social/additional-claims>.
