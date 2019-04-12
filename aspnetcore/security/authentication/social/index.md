---
title: Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono sposób tworzenia platformy ASP.NET Core 2.x aplikacji przy użyciu protokołu OAuth 2.0 przy użyciu dostawcy uwierzytelniania zewnętrznego.
ms.author: riande
ms.custom: mvc
ms.date: 4/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: 61482481358256dc9ddd1a0a894541040a8a452f
ms.sourcegitcommit: 9b7fcb4ce00a3a32e153a080ebfaae4ef417aafa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/12/2019
ms.locfileid: "59516329"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku pokazano, jak utworzyć aplikację platformy ASP.NET Core 2.2, która umożliwia użytkownikom zalogowanie się przy użyciu protokołu OAuth 2.0 przy użyciu poświadczeń z dostawcy uwierzytelniania zewnętrznego.

[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), i [Microsoft](xref:security/authentication/microsoft-logins) dostawców znajdują się w poniższych sekcjach. Innych dostawców są dostępne w innych pakietach przykład [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) i [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Ikony mediów społecznościowych usługi Facebook, Twitter, Google, plus i Windows](index/_static/social.png)

Umożliwienie użytkownikom logowania się za pomocą istniejących poświadczeń:
* Jest przydatna dla użytkowników.
* Przenosi wiele złożoności zarządzania procesu logowania na innej. 

Aby przykładów jak społecznościowych nazw logowania, może zapewnić konwersje typów ruchu i klienta, zobacz przypadków przez [Facebook](https://www.facebook.com/unsupportedbrowser) i [Twitter](https://dev.twitter.com/resources/case-studies).

## <a name="create-a-new-aspnet-core-project"></a>Utwórz nowy projekt platformy ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.
* Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.
* Wybierz **platformy ASP.NET Core 2.2** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.
* Wybierz **Zmień uwierzytelnianie** i uwierzytelniania zestawu **indywidualne konta użytkowników**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).

* Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.

* Uruchom następujące polecenia:

  ```console
  dotnet new webapp -o WebApp1
  code -r WebApp1
  ```

  * `dotnet new` Polecenie tworzy nowy projekt strony Razor w *WebApp1* folderu.
  * `code` Polecenia otwiera *WebApp1* folderu w nowym wystąpieniu programu Visual Studio Code.

  Zostanie wyświetlone okno dialogowe z **"WebApp1" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**

* Wybierz **tak**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

W terminalu uruchom następujące polecenie:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o WebApp1
```

Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do utworzenia projektu stron Razor.

## <a name="open-the-project"></a>Otwórz projekt

Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *WebApp1.csproj* pliku.

<!-- End of VS tabs -->

---

## <a name="apply-migrations"></a>Zastosuj migracji

* Uruchom aplikację i wybierz **zarejestrować** łącza.
* Wprowadź adres e-mail i hasło dla nowego konta, a następnie wybierz **zarejestrować**.
* Postępuj zgodnie z instrukcjami, aby zastosować migracji.

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

* Wybierz **Hello &lt;aliasu adresu e-mail&gt;**  link w prawym górnym rogu, aby przejść do **Zarządzaj** widoku.

![Widok zarządzania aplikacji sieci Web](index/_static/pass1a.png)

* Wybierz pozycję **Utwórz**

![Ustawianie strony hasła](index/_static/pass2a.png)

* Ustawione prawidłowe hasło i umożliwia to logowanie za pomocą poczty e-mail.

## <a name="next-steps"></a>Następne kroki

* W tym artykule wprowadzono uwierzytelniania zewnętrznego i opisano wymagania wstępne dotyczące dodawania logowań zewnętrznych do aplikacji platformy ASP.NET Core.

* Odwołanie do strony właściwe dla dostawcy Konfigurowanie logowania dla dostawcy, wymagane przez aplikację.

* Można utrwalić dodatkowe dane dotyczące użytkownika i tokenami dostępu i odświeżania. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/social/additional-claims>.
