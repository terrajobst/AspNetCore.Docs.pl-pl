---
title: Ustawienia logowania zewnętrznego Google w programie ASP.NET Core
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta Google do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: e12d831d2e0a5c9acae5ea41fb4187ad4ca6b0ea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082486"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Ustawienia logowania zewnętrznego Google w programie ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

[Starsze interfejsy API usługi Google + zostały wyłączone z 7 marca 2019](https://developers.google.com/+/api-shutdown). Google + Zaloguj i deweloperzy muszą przejść do nowego systemu Google logowania. Pakiety ASP.NET Core 2,1 i 2,2 dla usługi Google Authentication zostały zaktualizowane w celu uwzględnienia zmian. Aby uzyskać więcej informacji i tymczasowe środki zaradcze dla ASP.NET Core, zobacz [ten problem](https://github.com/aspnet/AspNetCore/issues/6486)w usłudze GitHub. Ten samouczek został zaktualizowany przy użyciu nowego procesu instalacji.

W tym samouczku pokazano, jak umożliwić użytkownikom logowanie się za pomocą konta Google przy użyciu projektu ASP.NET Core 2,2 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).

## <a name="create-a-google-api-console-project-and-client-id"></a>Tworzenie projektu konsoli interfejsu API firmy Google i identyfikatora klienta

* Przejdź do [strony integracja z logowaniem Google w aplikacji sieci Web](https://developers.google.com/identity/sign-in/web/devconsole-project) i wybierz pozycję **Konfiguruj projekt**.
* W oknie dialogowym **Konfigurowanie klienta uwierzytelniania OAuth** wybierz opcję **serwer sieci Web**.
* W polu tekstowym **autoryzowane adresy URI przekierowania** ustaw identyfikator URI przekierowania. Na przykład:`https://localhost:5001/signin-google`
* Zapisz **Identyfikator klienta** i **klucz tajny klienta**.
* Podczas wdrażania lokacji Zarejestruj nowy publiczny adres URL z poziomu **konsoli Google**.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID i ClientSecret

Przechowuj ustawienia poufne, takie jak Google `Client ID` i `Client Secret` with [Secret Manager](xref:security/app-secrets). Na potrzeby tego samouczka Nazwij tokeny `Authentication:Google:ClientId` i: `Authentication:Google:ClientSecret`

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Poświadczenia interfejsu API i użycie można zarządzać w [konsoli interfejsu API](https://console.developers.google.com/apis/dashboard).

## <a name="configure-google-authentication"></a>Skonfiguruj uwierzytelnianie Google

Dodaj usługę Google do `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Zaloguj się przy użyciu Google

* Uruchom aplikację i kliknij pozycję **Zaloguj**. Zostanie wyświetlona opcja zalogowania się za pomocą usługi Google.
* Kliknij przycisk **Google** , który przekierowuje do usługi Google w celu uwierzytelnienia.
* Po wprowadzeniu poświadczeń Google nastąpi przekierowanie z powrotem do witryny sieci Web.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Więcej informacji o opcjach konfiguracji obsługiwanych przez uwierzytelnianie Google można znaleźć w dokumentacji interfejsuAPI.<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> Może to służyć do żądania różne informacje o użytkowniku.

## <a name="change-the-default-callback-uri"></a>Zmień domyślny identyfikator URI wywołania zwrotnego

Segmentem identyfikatora URI `/signin-google` jest ustawiony jako zwrotnym domyślnego dostawcę uwierzytelniania serwisu Google. Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania Google za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) klasy.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* Jeśli logowanie nie działa i nie pojawiają się żadne błędy, przełącz się do trybu deweloperskiego, aby ułatwić debugowanie problemu.
* Jeśli tożsamość nie jest skonfigurowana przez `services.AddIdentity` wywołanie `ConfigureServices`w, próba uwierzytelnienia wyników w *argumencieexception: Należy podać*opcję "SignInScheme". Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, możesz uzyskać *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Wybierz pozycję **Zastosuj migracje** , aby utworzyć bazę danych, a następnie Odśwież stronę, aby kontynuować z powodu błędu.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Google. Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).
* Po opublikowaniu aplikacji na platformie Azure zresetuj ją `ClientSecret` w konsoli interfejsu API firmy Google.
* Ustaw `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret` jako ustawienia aplikacji w witrynie Azure portal. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.
