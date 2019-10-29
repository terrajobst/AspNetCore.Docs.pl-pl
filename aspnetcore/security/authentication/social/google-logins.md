---
title: Konfiguracja zewnętrznego logowania do usługi Google w ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono integrację uwierzytelniania użytkownika konta Google z istniejącą aplikacją ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/28/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 663029ecab99efd4f63f8deca026957c19c64710
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034314"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Konfiguracja zewnętrznego logowania do usługi Google w ASP.NET Core

Autorzy [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku pokazano, jak umożliwić użytkownikom logowanie się za pomocą konta Google przy użyciu projektu ASP.NET Core 3,0 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).

## <a name="create-a-google-api-console-project-and-client-id"></a>Tworzenie projektu konsoli interfejsu API firmy Google i identyfikatora klienta

* Zainstaluj [Microsoft. AspNetCore. Authentication. Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google).
* Przejdź do [strony integracja z logowaniem Google w aplikacji sieci Web](https://developers.google.com/identity/sign-in/web/devconsole-project) i wybierz pozycję **Konfiguruj projekt**.
* W oknie dialogowym **Konfigurowanie klienta uwierzytelniania OAuth** wybierz opcję **serwer sieci Web**.
* W polu tekstowym **autoryzowane adresy URI przekierowania** ustaw identyfikator URI przekierowania. Na przykład:`https://localhost:44312/signin-google`
* Zapisz **Identyfikator klienta** i **klucz tajny klienta**.
* Podczas wdrażania lokacji Zarejestruj nowy publiczny adres URL z poziomu **konsoli Google**.

## <a name="store-google-clientid-and-clientsecret"></a>Przechowywanie usług Google ClientID i ClientSecret

Przechowuj ustawienia poufne, takie jak Google `Client ID` i `Client Secret`, za pomocą [Menedżera wpisów tajnych](xref:security/app-secrets). Na potrzeby tego samouczka Nazwij `Authentication:Google:ClientId` tokeny i `Authentication:Google:ClientSecret`:

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "<client id>"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Poświadczenia interfejsu API i użycie można zarządzać w [konsoli interfejsu API](https://console.developers.google.com/apis/dashboard).

## <a name="configure-google-authentication"></a>Skonfiguruj uwierzytelnianie Google

Dodaj usługę Google do `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupGoogle3x.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Zaloguj się przy użyciu usługi Google

* Uruchom aplikację i kliknij pozycję **Zaloguj**. Zostanie wyświetlona opcja zalogowania się za pomocą usługi Google.
* Kliknij przycisk **Google** , który przekierowuje do usługi Google w celu uwierzytelnienia.
* Po wprowadzeniu poświadczeń Google nastąpi przekierowanie z powrotem do witryny sieci Web.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Więcej informacji o opcjach konfiguracji obsługiwanych przez uwierzytelnianie Google można znaleźć w dokumentacji interfejsu API <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>. Może to służyć do żądania różnych informacji o użytkowniku.

## <a name="change-the-default-callback-uri"></a>Zmień domyślny identyfikator URI wywołania zwrotnego

Segment identyfikatora URI `/signin-google` jest ustawiany jako domyślne wywołanie zwrotne dostawcy usługi Google Authentication. Domyślny identyfikator URI wywołania zwrotnego można zmienić podczas konfigurowania oprogramowania pośredniczącego usługi Google Authentication za pośrednictwem dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) .

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* Jeśli logowanie nie działa i nie pojawiają się żadne błędy, przełącz się do trybu deweloperskiego, aby ułatwić debugowanie problemu.
* Jeśli tożsamość nie jest konfigurowana przez wywołanie `services.AddIdentity` w `ConfigureServices`, próba uwierzytelnienia wyników w *argumencieexception: należy podać opcję "SignInScheme"* . Szablon projektu używany w tym samouczku zapewnia, że jest to gotowe.
* Jeśli baza danych lokacji nie została utworzona przez zastosowanie początkowej migracji, podczas *przetwarzania błędu żądania nie można wykonać operacji bazy danych* . Wybierz pozycję **Zastosuj migracje** , aby utworzyć bazę danych, a następnie Odśwież stronę, aby kontynuować z powodu błędu.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Google. Podobne podejście można wykonać w celu uwierzytelnienia z innymi dostawcami wymienionymi na [poprzedniej stronie](xref:security/authentication/social/index).
* Po opublikowaniu aplikacji na platformie Azure Zresetuj `ClientSecret` w konsoli interfejsu API firmy Google.
* Ustaw `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret` jako ustawienia aplikacji w Azure Portal. System konfiguracji jest skonfigurowany do odczytywania kluczy ze zmiennych środowiskowych.
