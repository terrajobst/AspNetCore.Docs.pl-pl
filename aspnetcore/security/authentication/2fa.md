---
title: Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) za pomocą aplikacji platformy ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
uid: security/authentication/2fa
ms.openlocfilehash: 19cc4b5326e8359afd47dd75aca3d661c3f92a30
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402123"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Swiss deweloperów](https://github.com/Swiss-Devs)

>[!WARNING]
> Dwa Authentication uwierzytelnianie (2FA) uwierzytelniania aplikacji przy użyciu opartych na czasie jednorazowe hasła algorytm (TOTP), są zalecane podejście do uwierzytelniania 2FA. Funkcji 2FA przy użyciu TOTP jest preferowany dla wiadomości SMS 2FA. Aby uzyskać więcej informacji, zobacz [generowanie kodu QR Włącz dla TOTP aplikacje uwierzytelniania w programie ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) dla platformy ASP.NET Core 2.0 i nowszych.

W tym samouczku pokazano, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) przy użyciu programu SMS. Instrukcje są podane dla [twilio](https://www.twilio.com/) i [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ale można użyć dowolnego dostawcy programu SMS. Firma Microsoft zaleca, po ukończeniu [potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm) przed rozpoczęciem tego samouczka.

Widok [ukończone przykładowe](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Jak pobrać](xref:tutorials/index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Utwórz nowy projekt ASP.NET Core

Utwórz nową aplikację sieci web platformy ASP.NET Core o nazwie `Web2FA` za pomocą indywidualnych kont użytkowników. Postępuj zgodnie z instrukcjami w [Wymuszanie protokołu SSL w aplikacji ASP.NET Core](xref:security/enforcing-ssl) do ustawiania i wymagać protokołu SSL.

### <a name="create-an-sms-account"></a>Tworzenie konta usługi programu SMS

Tworzenie konta usługi programu SMS, na przykład z [twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Zapisz poświadczenia uwierzytelniania (usłudze twilio: accountSid i authToken dla ASPSMS: Userkey i hasło).

#### <a name="figuring-out-sms-provider-credentials"></a>Ustalenie, poświadczenia dostawcy programu SMS

**Twilio:** z karty Pulpit nawigacyjny konta usługi Twilio, skopiuj **identyfikator SID konta** i **tokenu uwierzytelniania**.

**ASPSMS:** w ustawieniach konta, przejdź do **Userkey** i skopiować go razem z Twojej **hasło**.

Później będą przechowywane te wartości przy użyciu narzędzia menedżera klucz tajny w kluczach `SMSAccountIdentification` i `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Określanie SenderID / inicjatora

**Twilio:** na karcie liczby skopiuj usługi Twilio **numer telefonu**.

**ASPSMS:** w Menu nadawcy odblokować odblokować przynajmniej jednego nadawcy, lub wybierz inicjatora alfanumeryczne (nie obsługiwany przez wszystkie sieci).

Później będą przechowywane w tej wartości za pomocą narzędzia menedżera klucz tajny w kluczu `SMSAccountFrom`.


### <a name="provide-credentials-for-the-sms-service"></a>Podaj poświadczenia dla usługi programu SMS

Użyjemy [wzorzec opcje](xref:fundamentals/configuration/options) uzyskać dostępu do ustawień konta i klucza użytkownika.

   * Utwórz klasę, można pobrać bezpieczny klucz wiadomości SMS. W tym przykładzie `SMSoptions` klasa jest tworzona w *Services/SMSoptions.cs* pliku.

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Ustaw `SMSAccountIdentification`, `SMSAccountPassword` i `SMSAccountFrom` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets). Na przykład:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* Dodaj pakiet NuGet dla dostawcy programu SMS. Z pakietu Menedżera konsoli (PMC) uruchom:

**Twilio:**
`Install-Package Twilio`

**ASPSMS:**
`Install-Package ASPSMS`


* Dodaj kod w *Services/MessageServices.cs* plik w celu włączenia programu SMS. Przy użyciu usługi Twilio lub sekcji ASPSMS:


**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Konfigurowanie uruchamiania do użycia `SMSoptions`

Dodaj `SMSoptions` do kontenera usługi w `ConfigureServices` method in Class metoda *Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Włączanie uwierzytelniania dwuskładnikowego

Otwórz *Views/Manage/Index.cshtml* plik widoku Razor i Usuń komentarz znaków (taki sposób, nie ujęty w poziomie).

## <a name="log-in-with-two-factor-authentication"></a>Zaloguj się przy użyciu uwierzytelniania dwuskładnikowego

* Uruchom aplikację i zarejestrować nowego użytkownika

![Widok zarejestrować aplikacji sieci Web otwórz w programie Microsoft Edge](2fa/_static/login2fa1.png)

* Wybierz swoją nazwę użytkownika i aktywuje `Index` metody akcji kontrolera zarządzania. Następnie wybierz numer telefonu **Dodaj** łącza.

![Zarządzanie widoku](2fa/_static/login2fa2.png)

* Dodaj numer telefonu, który uzyskać kod weryfikacyjny i naciśnij pozycję **Wyślij kod weryfikacyjny**.

![Dodaj stronę numer telefonu](2fa/_static/login2fa3.png)

* Otrzymasz wiadomość SMS z kodem weryfikacyjnym. Wprowadź go, a następnie naciśnij przycisk **przesyłania**

![Sprawdź numer telefonu strony](2fa/_static/login2fa4.png)

Jeśli nie otrzymasz wiadomość SMS, zobacz stronę dziennika usługi twilio.

* W widoku Zarządzanie pokazuje numer telefonu został pomyślnie dodany.

![Zarządzanie widoku](2fa/_static/login2fa5.png)

* Naciśnij pozycję **Włącz** do włączenia uwierzytelniania dwuskładnikowego.

![Zarządzanie widoku](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Uwierzytelnianie dwuskładnikowe testu

* Wyloguj się.

* Zaloguj się.

* Konto użytkownika ma włączone uwierzytelnianie dwuskładnikowe, więc należy podać drugi składnik uwierzytelniania. W tym samouczku włączono weryfikację telefoniczną. Wbudowane szablony pozwalają również na konfigurowanie poczty e-mail jako drugiego składnika. Możesz skonfigurować dodatkowych czynników drugi uwierzytelniania, takiego jak kody QR. Naciśnij pozycję **przesłać**.

![Wyślij kod weryfikacyjny widoku](2fa/_static/login2fa7.png)

* Wprowadź kod, który jest pobierany w wiadomości SMS.

* Kliknięcie **pamiętasz tę przeglądarkę** pole wyboru zostaną wykluczone z konieczności korzystania z funkcji 2FA do logowania się w przypadku korzystania z tego samego urządzenia i przeglądarki. Włączanie funkcji 2FA, a następnie klikając **pamiętasz tę przeglądarkę** udostępni silnego uwierzytelniania 2FA, ochronę przed złośliwymi użytkownikami podjęcie próby dostępu do Twojego konta, tak długo, jak nie mają dostępu do Twojego urządzenia. Można to zrobić, na dowolnym urządzeniu prywatnych, regularnie używane. Ustawiając **pamiętasz tę przeglądarkę**, uzyskaj dodatkowe zabezpieczenie w postaci funkcji 2FA z urządzeń, które nie są regularnie używane i Uzyskaj wygody na ominięcie za pomocą funkcji 2FA na własnych urządzeniach.

![Sprawdź widok](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Blokada konta na potrzeby ochrony przed atakami siłowymi

Blokada konta zaleca się za pomocą funkcji 2FA. Po użytkownik loguje się przy użyciu konta lokalnego lub konta w sieci społecznościowej, znajduje się każda nieudanej próby w funkcji 2FA. W przypadku osiągnięcia maksymalnej nieudanych prób dostępu użytkownika jest zablokowane (domyślne: 5 minut blokady po 5 nieudanych prób dostępu). Po pomyślnym uwierzytelnieniu resetuje liczbę prób dostępu nie powiodło się i zresetowanie zegara. Maksymalną liczbę nieudanych prób dostępu i okres blokady, można ustawić za pomocą [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) i [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). Następujące konfiguruje blokady konta po upływie 10 minut po 10 nieudanych prób dostępu:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Upewnij się, że [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) ustawia `lockoutOnFailure` do `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
