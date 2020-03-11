---
title: Ustawienia logowania zewnętrznego Google w programie ASP.NET Core
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta Google do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/30/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 83f45143eca1be43410880bfd875a3fce1d2e9c9
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667511"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Ustawienia logowania zewnętrznego Google w programie ASP.NET Core

Autorzy [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku pokazano, jak umożliwić użytkownikom logowanie się za pomocą konta Google przy użyciu projektu ASP.NET Core 3,0 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).

## <a name="create-a-google-api-console-project-and-client-id"></a>Tworzenie projektu konsoli interfejsu API firmy Google i identyfikatora klienta

* Zainstaluj [Microsoft. AspNetCore. Authentication. Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google).
* Przejdź do [strony integracja z logowaniem Google w aplikacji sieci Web](https://developers.google.com/identity/sign-in/web/devconsole-project) i wybierz pozycję **Konfiguruj projekt**.
* W oknie dialogowym **Konfigurowanie klienta uwierzytelniania OAuth** wybierz opcję **serwer sieci Web**.
* W polu tekstowym **autoryzowane adresy URI przekierowania** ustaw identyfikator URI przekierowania. Na przykład: `https://localhost:44312/signin-google`
* Zapisz **Identyfikator klienta** i **klucz tajny klienta**.
* Podczas wdrażania lokacji Zarejestruj nowy publiczny adres URL z poziomu **konsoli Google**.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID i ClientSecret

Przechowuj ustawienia poufne, takie jak Google `Client ID` i `Client Secret`, za pomocą [Menedżera wpisów tajnych](xref:security/app-secrets). Na potrzeby tego samouczka Nazwij `Authentication:Google:ClientId` tokeny i `Authentication:Google:ClientSecret`:

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "<client id>"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Poświadczenia interfejsu API i użycie można zarządzać w [konsoli interfejsu API](https://console.developers.google.com/apis/dashboard).

## <a name="configure-google-authentication"></a>Skonfiguruj uwierzytelnianie Google

Dodaj usługę Google do `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupGoogle3x.cs?highlight=11-19)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Zaloguj się przy użyciu Google

* Uruchom aplikację i kliknij pozycję **Zaloguj**. Zostanie wyświetlona opcja zalogowania się za pomocą usługi Google.
* Kliknij przycisk **Google** , który przekierowuje do usługi Google w celu uwierzytelnienia.
* Po wprowadzeniu poświadczeń Google nastąpi przekierowanie z powrotem do witryny sieci Web.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Więcej informacji o opcjach konfiguracji obsługiwanych przez uwierzytelnianie Google można znaleźć w dokumentacji interfejsu API <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>. Może to służyć do żądania różne informacje o użytkowniku.

## <a name="change-the-default-callback-uri"></a>Zmień domyślny identyfikator URI wywołania zwrotnego

Segment identyfikatora URI `/signin-google` jest ustawiany jako domyślne wywołanie zwrotne dostawcy usługi Google Authentication. Domyślny identyfikator URI wywołania zwrotnego można zmienić podczas konfigurowania oprogramowania pośredniczącego usługi Google Authentication za pośrednictwem dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) .

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* Jeśli logowanie nie działa i nie pojawiają się żadne błędy, przełącz się do trybu deweloperskiego, aby ułatwić debugowanie problemu.
* Jeśli tożsamość nie jest konfigurowana przez wywołanie `services.AddIdentity` w `ConfigureServices`, próba uwierzytelnienia wyników w *argumencieexception: należy podać opcję "SignInScheme"* . Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.
* Jeśli baza danych lokacji nie została utworzona przez zastosowanie początkowej migracji, podczas *przetwarzania błędu żądania nie można wykonać operacji bazy danych* . Wybierz pozycję **Zastosuj migracje** , aby utworzyć bazę danych, a następnie Odśwież stronę, aby kontynuować z powodu błędu.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Google. Podobne podejście można wykonać w celu uwierzytelnienia z innymi dostawcami wymienionymi na [poprzedniej stronie](xref:security/authentication/social/index).
* Po opublikowaniu aplikacji na platformie Azure Zresetuj `ClientSecret` w konsoli interfejsu API firmy Google.
* Ustaw `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret` jako ustawienia aplikacji w Azure Portal. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.
