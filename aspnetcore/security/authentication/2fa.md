---
title: Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) za pomocą aplikacji platformy ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 424d21e446de02b39daa7685205a00931361b326
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661456"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS w programie ASP.NET Core

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [szwajcarski-deweloperzy](https://github.com/Swiss-Devs)

>[!WARNING]
> Dwa Authentication uwierzytelnianie (2FA) uwierzytelniania aplikacji przy użyciu opartych na czasie jednorazowe hasła algorytm (TOTP), są zalecane podejście do uwierzytelniania 2FA. Funkcji 2FA przy użyciu TOTP jest preferowany dla wiadomości SMS 2FA. Aby uzyskać więcej informacji, zobacz [Włączanie generowania kodu QR dla aplikacji TOTP Authenticator w ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) dla ASP.NET Core 2,0 i nowszych.

W tym samouczku pokazano, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) przy użyciu programu SMS. Instrukcje są podane dla [Twilio](https://www.twilio.com/) i [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ale można użyć dowolnego innego dostawcy programu SMS. Przed rozpoczęciem tego samouczka zalecamy ukończenie [potwierdzeń konta i odzyskiwania hasła](xref:security/authentication/accconfirm) .

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Jak pobrać](xref:index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Utwórz nowy projekt ASP.NET Core

Utwórz nową ASP.NET Core aplikację sieci Web o nazwie `Web2FA` przy użyciu poszczególnych kont użytkowników. Postępuj zgodnie z instrukcjami w <xref:security/enforcing-ssl>, aby skonfigurować i wymagać protokołu HTTPS.

### <a name="create-an-sms-account"></a>Tworzenie konta usługi programu SMS

Utwórz konto programu SMS, na przykład z [Twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Zapisz poświadczenia uwierzytelniania (usłudze twilio: accountSid i authToken dla ASPSMS: Userkey i hasło).

#### <a name="figuring-out-sms-provider-credentials"></a>Ustalenie, poświadczenia dostawcy programu SMS

**Twilio**

Na karcie Pulpit nawigacyjny konta usługi Twilio Skopiuj **Identyfikator SID konta** i **token uwierzytelniania**.

**ASPSMS:**

W ustawieniach konta przejdź do **userKey** i skopiuj go wraz z **hasłem**.

Te wartości zostaną później zapisane za pomocą narzędzia Menedżer kluczy tajnych w obszarze klucze `SMSAccountIdentification` i `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Określanie SenderID / inicjatora

**Twilio:** Na karcie liczby Skopiuj **numer telefonu**Twilio.

**ASPSMS:** W menu odblokowane odblokowywanie Odblokuj co najmniej jeden inicjator lub wybierz inicjator alfanumeryczny (nieobsługiwany przez wszystkie sieci).

Ta wartość zostanie później przechowana przy użyciu narzędzia do zarządzania kluczami tajnymi w ramach klucza `SMSAccountFrom`.

### <a name="provide-credentials-for-the-sms-service"></a>Podaj poświadczenia dla usługi programu SMS

Użyjemy [wzorca opcji](xref:fundamentals/configuration/options) , aby uzyskać dostęp do ustawień konta użytkownika i klucza.

* Utwórz klasę, można pobrać bezpieczny klucz wiadomości SMS. W tym przykładzie Klasa `SMSoptions` jest tworzona w pliku *Services/SMSoptions. cs* .

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Ustaw `SMSAccountIdentification``SMSAccountPassword` i `SMSAccountFrom` za pomocą narzędzia do [zarządzania kluczami tajnymi](xref:security/app-secrets). Na przykład:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* Dodaj pakiet NuGet dla dostawcy programu SMS. Z pakietu Menedżera konsoli (PMC) uruchom:

**Twilio**

`Install-Package Twilio`

**ASPSMS:**

`Install-Package ASPSMS`

* Aby włączyć program SMS, Dodaj kod w pliku *Services/MessageServices. cs* . Przy użyciu usługi Twilio lub sekcji ASPSMS:

**Twilio**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Skonfiguruj uruchamianie, aby używać `SMSoptions`

Dodaj `SMSoptions` do kontenera usługi w metodzie `ConfigureServices` w *Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Włączanie uwierzytelniania dwuskładnikowego

Otwórz plik widoku Razor */Zarządzanie/index. cshtml* , a następnie usuń znaki komentarza (w związku z czym żaden znacznik nie jest oznaczony jako komentarz).

## <a name="log-in-with-two-factor-authentication"></a>Zaloguj się przy użyciu uwierzytelniania dwuskładnikowego

* Uruchom aplikację i zarejestrować nowego użytkownika

![Widok zarejestrować aplikacji sieci Web otwórz w programie Microsoft Edge](2fa/_static/login2fa1.png)

* Wybierz swoją nazwę użytkownika, która aktywuje metodę `Index` akcji w obszarze Zarządzanie kontrolerem. Następnie naciśnij link numer telefonu **Dodaj** .

![Zarządzanie widok — nacisnąć link "Dodaj"](2fa/_static/login2fa2.png)

* Dodaj numer telefonu, który będzie otrzymywał kod weryfikacyjny, a następnie naciśnij pozycję **Wyślij kod weryfikacyjny**.

![Dodaj stronę numer telefonu](2fa/_static/login2fa3.png)

* Otrzymasz wiadomość SMS z kodem weryfikacyjnym. Wprowadź go i naciśnij pozycję **Prześlij**

![Sprawdź numer telefonu strony](2fa/_static/login2fa4.png)

Jeśli nie otrzymasz wiadomość SMS, zobacz stronę dziennika usługi twilio.

* W widoku Zarządzanie pokazuje numer telefonu został pomyślnie dodany.

![Zarządzanie widok — pomyślnie dodano numer telefonu](2fa/_static/login2fa5.png)

* Naciśnij pozycję **Włącz** , aby włączyć uwierzytelnianie dwuskładnikowe.

![Zarządzanie widok — Włączanie uwierzytelniania dwuskładnikowego](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Uwierzytelnianie dwuskładnikowe testu

* Wyloguj się.

* Zaloguj się.

* Konto użytkownika ma włączone uwierzytelnianie dwuskładnikowe, więc należy podać drugi składnik uwierzytelniania. W tym samouczku włączono weryfikację telefoniczną. Wbudowane szablony pozwalają również na konfigurowanie poczty e-mail jako drugiego składnika. Możesz skonfigurować dodatkowych czynników drugi uwierzytelniania, takiego jak kody QR. Naciśnij pozycję **Prześlij**.

![Wyślij kod weryfikacyjny widoku](2fa/_static/login2fa7.png)

* Wprowadź kod, który jest pobierany w wiadomości SMS.

* Kliknięcie pola wyboru **Zapamiętaj tę przeglądarkę** spowoduje zwolnienie z używania funkcji 2FA do logowania się w przypadku korzystania z tego samego urządzenia i przeglądarki. Włączenie funkcji 2FA i kliknięcie opcji **Zapamiętaj, że ta przeglądarka** zapewni mocną ochronę funkcji 2FA przed złośliwymi użytkownikami próbującymi uzyskać dostęp do Twojego konta, o ile nie mają dostępu do urządzenia. Można to zrobić, na dowolnym urządzeniu prywatnych, regularnie używane. Dzięki ustawieniu **Zapamiętaj tę przeglądarkę**możesz uzyskać dodatkowe zabezpieczenia funkcji 2FA z urządzeń, które nie są regularnie używane, i uzyskać wygodę, aby nie mieć możliwości przechodzenia przez funkcji 2FA na własne urządzenia.

![Sprawdź widok](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Blokada konta na potrzeby ochrony przed atakami siłowymi

Blokada konta zaleca się za pomocą funkcji 2FA. Po użytkownik loguje się przy użyciu konta lokalnego lub konta w sieci społecznościowej, znajduje się każda nieudanej próby w funkcji 2FA. W przypadku osiągnięcia maksymalnej nieudanych prób dostępu użytkownika jest zablokowane (domyślne: 5 minut blokady po 5 nieudanych prób dostępu). Po pomyślnym uwierzytelnieniu resetuje liczbę prób dostępu nie powiodło się i zresetowanie zegara. Maksymalna liczba nieudanych prób dostępu i czas blokady można ustawić za pomocą [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) i [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). Następujące konfiguruje blokady konta po upływie 10 minut po 10 nieudanych prób dostępu:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Potwierdź, że [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) ustawia `lockoutOnFailure` na `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
