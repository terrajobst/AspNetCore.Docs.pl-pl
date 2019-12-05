---
title: Konfiguracja logowania zewnętrznego konta Microsoft z ASP.NET Core
author: rick-anderson
description: Ten przykład pokazuje integrację konto Microsoft uwierzytelniania użytkowników w istniejącej aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/4/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/microsoft-logins
ms.openlocfilehash: ddaae1a25a1dcf167ffae0f24b480e2cde6aca5b
ms.sourcegitcommit: f4cd3828e26e6d549ba8d0c36a17be35ad9e5a51
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74825465"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Konfiguracja logowania zewnętrznego konta Microsoft z ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten przykład pokazuje, jak umożliwić użytkownikom logowanie się za pomocą konto Microsoft przy użyciu projektu ASP.NET Core 3,0 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Tworzenie aplikacji w portalu dla deweloperów firmy Microsoft

* Dodaj pakiet NuGet [Microsoft. AspNetCore. Authentication. MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) do projektu.
* Przejdź do strony [Azure Portal-rejestracje aplikacji](https://go.microsoft.com/fwlink/?linkid=2083908) i Utwórz lub Zaloguj się do konto Microsoft:

Jeśli nie masz konto Microsoft, wybierz pozycję **Utwórz**. Po zalogowaniu nastąpi przekierowanie do strony **rejestracje aplikacji** :

* Wybierz **nową rejestrację**
* Wprowadź **nazwę**.
* Wybierz opcję dla **obsługiwanych typów kont**.  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* W obszarze **Identyfikator URI przekierowania**wprowadź adres URL Twojego projektu, `/signin-microsoft` dołączyć. Na przykład `https://localhost:5001/signin-microsoft`. Schemat uwierzytelniania firmy Microsoft skonfigurowany w dalszej części tego przykładu będzie automatycznie obsługiwał żądania w ramach trasy `/signin-microsoft` w celu zaimplementowania przepływu OAuth.
* Wybierz pozycję **zarejestruj**

### <a name="create-client-secret"></a>Utwórz klucz tajny klienta

* W lewym okienku wybierz pozycję **certyfikaty & wpisy tajne**.
* W obszarze wpisy **tajne klienta**wybierz pozycję **nowy klucz tajny klienta** .

  * Dodaj opis wpisu tajnego klienta.
  * Wybierz przycisk **Add** (Dodaj).

* W obszarze wpisy **tajne klienta**skopiuj wartość klucza tajnego klienta.

> [!NOTE]
> Segment identyfikatora URI `/signin-microsoft` jest ustawiany jako domyślne wywołanie zwrotne dostawcy uwierzytelniania firmy Microsoft. Można zmienić domyślny identyfikator URI wywołania zwrotnego podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania firmy Microsoft za pośrednictwem dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) .

## <a name="store-the-microsoft-client-id-and-client-secret"></a>Zapisz identyfikator klienta firmy Microsoft i klucz tajny klienta

Uruchom następujące polecenia, aby bezpiecznie przechowywać `ClientId` i `ClientSecret` przy użyciu [Menedżera wpisów tajnych](xref:security/app-secrets):

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

Połącz poufne ustawienia, takie jak Microsoft `ClientId` i `ClientSecret` z konfiguracją aplikacji za pomocą [Menedżera wpisów tajnych](xref:security/app-secrets). Na potrzeby tego przykładu Nazwij tokeny `Authentication:Microsoft:ClientId` i `Authentication:Microsoft:ClientSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Konfigurowanie uwierzytelniania konta Microsoft

Dodaj usługę konta Microsoft do `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupMS3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie konta Microsoft, zobacz Dokumentacja interfejsu API [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) . Może to służyć do żądania różne informacje o użytkowniku.

## <a name="sign-in-with-microsoft-account"></a>Konto Zaloguj się przy użyciu konta Microsoft

Uruchom aplikację i kliknij pozycję **Zaloguj**. Zostanie wyświetlona opcja zalogowania się do firmy Microsoft. Po kliknięciu firmy Microsoft nastąpi przekierowanie do firmy Microsoft w celu uwierzytelnienia. Po zalogowaniu się przy użyciu konta Microsoft zostanie wyświetlony monit o zezwolenie aplikacji na dostęp do informacji:

Naciśnij pozycję **tak** . nastąpi przekierowanie z powrotem do witryny sieci Web, w której można ustawić swój adres e-mail.

Jesteś teraz zalogowany przy użyciu poświadczeń firmy Microsoft:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* Jeśli dostawca kont Microsoft przekieruje Cię do strony błędu logowania, należy zwrócić uwagę na tytuły i opis błędu parametrów zapytania bezpośrednio po `#` (hasztagów) w identyfikatorze URI.

  Mimo że zostanie wyświetlony komunikat o błędzie z informacją o problemie z uwierzytelnianiem firmy Microsoft, najbardziej typową przyczyną jest to, że identyfikator URI aplikacji nie pasuje do żadnego z **identyfikatorów URI przekierowania** określonych dla danej platformy **sieci Web** .
* Jeśli tożsamość nie jest konfigurowana przez wywołanie `services.AddIdentity` w `ConfigureServices`, próba uwierzytelnienia spowoduje powstanie *argumentu ArgumentException: należy podać opcję "SignInScheme"* . Szablon projektu używany w tym przykładzie zapewnia, że jest to gotowe.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelnić się w firmie Microsoft. Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).

* Po opublikowaniu witryny sieci Web w usłudze Azure Web App Utwórz nowe wpisy tajne klienta w portalu dla deweloperów firmy Microsoft.

* Ustaw `Authentication:Microsoft:ClientId` i `Authentication:Microsoft:ClientSecret` jako ustawienia aplikacji w witrynie Azure portal. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.
