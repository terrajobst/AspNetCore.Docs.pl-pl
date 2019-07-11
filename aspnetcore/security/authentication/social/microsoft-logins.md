---
title: Konfigurowanie logowania zewnętrznego Account firmy Microsoft za pomocą programu ASP.NET Core
author: rick-anderson
description: Niniejszy przykład pokazuje, integracja uwierzytelniania użytkownika konta Microsoft do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 2c690e5bd8465806d42091616917cfdd747ef8f0
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815573"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Konfigurowanie logowania zewnętrznego Account firmy Microsoft za pomocą programu ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym przykładzie pokazano, jak umożliwić użytkownikom logowanie za pomocą swojego konta Microsoft, za pomocą projektu programu ASP.NET Core 2.2 utworzone na [poprzedniej strony](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Tworzenie aplikacji w portalu dla deweloperów firmy Microsoft

* Przejdź do [witryna Azure portal — rejestracje aplikacji](https://go.microsoft.com/fwlink/?linkid=2083908) strony i utworzyć lub zaloguj się do konta Microsoft:

Jeśli nie masz konta Microsoft, wybierz opcję **utworzyć**. Po zarejestrowaniu się nastąpi przekierowanie do **rejestracje aplikacji** strony:

* Wybierz **nowej rejestracji**
* Wprowadź **nazwa**.
* Wybierz opcję **obsługiwane typy kont**.  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* W obszarze **identyfikator URI przekierowania**, wprowadź adres URL programowania za pomocą `/signin-microsoft` dołączane. Na przykład `https://localhost:44389/signin-microsoft`. Schemat uwierzytelniania firmy Microsoft, skonfigurować je później w tym przykładzie będzie automatycznie obsługiwać żądań w `/signin-microsoft` trasy do zaimplementowania przepływu uwierzytelniania OAuth.
* Wybierz **zarejestrować**

### <a name="create-client-secret"></a>Utwórz klucz tajny klienta

* W okienku po lewej stronie wybierz **certyfikaty i klucze tajne**.
* W obszarze **wpisów tajnych klienta**, wybierz opcję **nowy wpis tajny klienta**

  * Dodaj opis klucza tajnego klienta.
  * Wybierz **Dodaj** przycisku.

* W obszarze **wpisów tajnych klienta**, skopiuj wartość klucza tajnego klienta.

> [!NOTE]
> Segmentem identyfikatora URI `/signin-microsoft` jest ustawiony jako domyślny wywołania zwrotnego dostawcy uwierzytelniania firmy Microsoft. Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania firmy Microsoft za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) klasy.

## <a name="store-the-microsoft-client-id-and-client-secret"></a>Store Microsoft klienta identyfikator i klucz tajny klienta

Uruchom następujące polecenia, aby bezpiecznie przechowywać `ClientId` i `ClientSecret` przy użyciu [Menedżera klucz tajny](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

Link ustawienia poufne, takie jak Microsoft `ClientId` i `ClientSecret` z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets). Do celów tego przykładu, nazwa tokeny `Authentication:Microsoft:ClientId` i `Authentication:Microsoft:ClientSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Skonfiguruj uwierzytelnianie za pomocą konta Microsoft

Dodaj usługę Account Microsoft do `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Zobacz [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie Account firmy Microsoft. Może to służyć do żądania różne informacje o użytkowniku.

## <a name="sign-in-with-microsoft-account"></a>Zaloguj się przy użyciu konta Microsoft

Uruchom i kliknij przycisk **Zaloguj**. Pojawi się opcja Zaloguj się przy użyciu konta Microsoft. Po kliknięciu firmy Microsoft są przekierowywane do firmy Microsoft do uwierzytelniania. Po zarejestrowaniu się przy użyciu Account firmy Microsoft (Jeśli nie zostało to zrobione) użytkownik jest monitowany, aby umożliwić aplikacji dostęp do informacji:

Naciśnij pozycję **tak** i nastąpi przekierowanie do witryny sieci web, w którym można ustawić swój adres e-mail.

Obecnie zalogowano Cię przy użyciu poświadczeń konta Microsoft:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* Jeśli dostawca Account Microsoft przekieruje Cię do strony logowania w błąd, weź pod uwagę błąd tytuł i opis parametrów ciągu zapytania bezpośrednio po `#` (hasztag) w identyfikatorze Uri.

  Mimo, że komunikat o błędzie wydaje się, że wskazywać na problem z uwierzytelniania firmy Microsoft, Najczęstszą przyczyną jest niezgodne żadnego z identyfikatora Uri aplikacji **identyfikatory URI przekierowań** określony dla **Web** platformy .
* Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: Opcja "SignInScheme" musi być podana*. Szablon projektu, używane w tym przykładzie gwarantuje, że odbywa się.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać z firmą Microsoft. Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).

* Po możesz opublikować witrynę sieci web do aplikacji sieci web platformy Azure, Utwórz nowy klient wpisów tajnych w portalu dla deweloperów firmy Microsoft.

* Ustaw `Authentication:Microsoft:ClientId` i `Authentication:Microsoft:ClientSecret` jako ustawienia aplikacji w witrynie Azure portal. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.