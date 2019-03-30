---
title: Ustawienia logowania zewnętrznego Google w programie ASP.NET Core
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta Google do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 1328bcbce3e6e4786f9d410d1f28f309dc9d2722
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750551"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Ustawienia logowania zewnętrznego Google w programie ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W styczniu 2019 Google zaczęła [Zamknij](https://developers.google.com/+/api-shutdown) Google + Zaloguj się i deweloperów należy przenieść do nowego logowania Google w systemie marca. W lutym w celu uwzględnienia zmiany zostaną zaktualizowane platformy ASP.NET Core 2.1 i 2,2 pakietów dla uwierzytelniania serwisu Google. Aby uzyskać więcej informacji i tymczasowe środki zaradcze dla platformy ASP.NET Core, zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore/issues/6486). W tym samouczku został zaktualizowany przy użyciu nowego procesu instalacji.

W tym samouczku dowiesz się, jak umożliwić użytkownikom logowanie za pomocą swojego konta Google przy użyciu projektu ASP.NET Core 2.2, utworzony na [poprzedniej strony](xref:security/authentication/social/index).

## <a name="create-a-google-api-console-project-and-client-id"></a>Utwórz identyfikator projektu i klienta konsoli interfejsu API Google

* Przejdź do [integracji Google Zaloguj się do aplikacji sieci web](https://developers.google.com/identity/sign-in/web/devconsole-project) i wybierz **SKONFIGURUJ projekt**.
* W **Konfigurowanie klienta OAuth** okno dialogowe, wybierz opcję **serwera sieci Web**.
* W **identyfikatory URI przekierowania autoryzowanych** pole wprowadzania tekstu, Ustaw identyfikator URI przekierowania. Na przykład:`https://localhost:5001/signin-google`
* Zapisz **identyfikator klienta** i **klucz tajny klienta**.
* W przypadku wdrażania w witrynie, należy zarejestrować nowy publiczny adres url z **konsoli Google**.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID i ClientSecret

Store ustawień poufnych, takich jak Google `Client ID` i `Client Secret` z [Menedżera klucz tajny](xref:security/app-secrets). Do celów tego samouczka, nazwa tokeny `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret`:

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Możesz zarządzać poświadczeniami interfejsu API i jego użycia w [Konsola interfejsu API](https://console.developers.google.com/apis/dashboard).

## <a name="configure-google-authentication"></a>Konfigurowanie uwierzytelniania serwisu Google

Dodaj usługę Google, aby `Startup.ConfigureServices`.

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Zaloguj się przy użyciu Google

* Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**. Pojawi się opcja Zaloguj się przy użyciu Google.
* Kliknij przycisk **Google** przycisk, który przekierowuje do firmy Google do uwierzytelniania.
* Po wprowadzeniu poświadczeń Google, nastąpi przekierowanie do witryny sieci web.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Zobacz [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie serwisu Google. Może to służyć do żądania różne informacje o użytkowniku.

## <a name="change-the-default-callback-uri"></a>Zmień domyślny identyfikator URI wywołania zwrotnego

Segmentem identyfikatora URI `/signin-google` jest ustawiony jako zwrotnym domyślnego dostawcę uwierzytelniania serwisu Google. Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania Google za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) klasy.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* Jeśli nie otrzymujesz błędy logowania nie działa, przełącz się do trybu opracowywania, aby ułatwić debugowanie problemu.
* Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`podjęto próbę uwierzytelnienia powoduje *ArgumentException: Opcja "SignInScheme" musi być podana*. Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, możesz uzyskać *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Google. Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).
* Gdy opublikujesz aplikację na platformie Azure, zresetuj `ClientSecret` w konsoli interfejsu API Google.
* Ustaw `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret` jako ustawienia aplikacji w witrynie Azure portal. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.
