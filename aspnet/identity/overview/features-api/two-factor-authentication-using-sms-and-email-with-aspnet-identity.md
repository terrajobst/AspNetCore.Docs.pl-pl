---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail w produkcie ASP.NET Identity | Dokumentacja firmy Microsoft
author: HaoK
description: Ten samouczek przedstawia sposób konfigurowania uwierzytelniania dwuskładnikowego (2FA) za pomocą wiadomości SMS i wiadomości e-mail. W tym artykule został napisany przez Rick Anderson ( @RickAndMSFT ), wzór...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0519ae69b3d2ef129d206a936b199f781fef5bf6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399682"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail w produkcie ASP.NET Identity
====================
przez [Hao Kung](https://github.com/HaoK), [autorem jest Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Ten samouczek przedstawia sposób konfigurowania uwierzytelniania dwuskładnikowego (2FA) za pomocą wiadomości SMS i wiadomości e-mail.
> 
> W tym artykule został napisany przez Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), autorem jest Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung i Suhas Joshi. Przykład NuGet został napisany głównie przez Hao Kung.


W tym temacie omówiono następujące czynności:

- [Tworzenie przykładowej tożsamości](#build)
- [Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego](#SMS)
- [Włączanie uwierzytelniania dwuskładnikowego](#enable2)
- [Jak zarejestrować dostawcę uwierzytelniania dwuskładnikowego](#reg)
- [Łączenie kont społecznościowych i lokalne logowanie](#combine)
- [Blokada konta przed atakami siłowymi](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Tworzenie przykładowej tożsamości

W tej sekcji użyjesz NuGet do pobrania próbki, firma Microsoft będzie działać z. Rozpocznij od instalowania i uruchamiania [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej.

> [!NOTE]
> Ostrzeżenie: Należy zainstalować program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) do ukończenia tego samouczka.


1. Utwórz nową ***pusty*** projektu sieci Web ASP.NET.
2. W konsoli Menedżera pakietów wprowadź następujące polecenia:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   W tym samouczku użyjemy [SendGrid](http://sendgrid.com/) do wysyłania wiadomości e-mail i [Twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) dla tekstowe sms. `Identity.Samples` Pakiet instaluje kodu, firma Microsoft będzie pracował z.
3. Ustaw [projektu do używania protokołu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Opcjonalnie*: postępuj zgodnie z instrukcjami w mojej [samouczek potwierdzenie E-mail](account-confirmation-and-password-recovery-with-aspnet-identity.md) do podpinanie SendGrid, a następnie uruchom aplikację i zarejestrować konto e-mail.
5. * Opcjonalne: * usunąć kod potwierdzenia łącze w wiadomości e-mail pokaz z próbki ( `ViewBag.Link` kod na kontrolerze konta. Zobacz `DisplayEmail` i `ForgotPasswordConfirmation` metody akcji i widokami razor).
6. <em>Opcjonalnie: * usunięcie `ViewBag.Status` kodu z kontrolerami zarządzania oraz konta i *Views\Account\VerifyCode.cshtml</em> i <em>Views\Manage\VerifyPhoneNumber.cshtml</em> widokami razor. Alternatywnie możesz zachować `ViewBag.Status` wyświetlany obraz do potrzeb testowania, w jaki sposób ta aplikacja działa lokalnie, bez konieczności Podłączanie i wysłać wiadomość e-mail i wiadomości SMS.

> [!NOTE]
> Ostrzeżenie: W przypadku zmiany ustawień zabezpieczeń, w tym przykładzie produkcji aplikacji będzie musiała zostać poddane inspekcji zabezpieczeń, który jawnie wywołuje się zmian.


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego

Ten samouczek zawiera instrukcje dotyczące korzystania z usługi Twilio lub ASPSMS, ale można użyć dowolnego dostawcy programu SMS.

1. **Tworzenie konta użytkownika z dostawcą programu SMS**  
  
   Tworzenie [Twilio](https://www.twilio.com/try-twilio) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) konta.
2. **Instalowanie dodatkowych pakietów lub dodawania odwołania do usług**  
  
   Twilio:  
   W konsoli Menedżera pakietów wpisz następujące polecenie:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   Należy dodać następujące odwołanie do usługi:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Adres:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Przestrzeń nazw:  
    `ASPSMSX2`
3. **Ustalenie, poświadczenia użytkownika dostawcy programu SMS**  
  
   Twilio:  
   Z **pulpit nawigacyjny** karty konta usługi Twilio, kopiowania **identyfikator SID konta** i **tokenu uwierzytelniania**.  
  
   ASPSMS:  
   W ustawieniach konta, przejdź do **Userkey** i skopiować go wraz z własnym zdefiniowanych **hasło**.  
  
   Te wartości będą później przechowywane w zmiennych `SMSAccountIdentification` i `SMSAccountPassword` .
4. **Określanie SenderID / inicjatora**  
  
   Twilio:  
   Z **numery** kartę, skopiuj numer telefonu usługi Twilio.  
  
   ASPSMS:  
   W ramach **odblokować nadawcy** Menu odblokować przynajmniej jednego nadawcy, lub wybierz inicjatora alfanumeryczne (nie obsługiwany przez wszystkie sieci).  
  
   Ta wartość później będą przechowywane w zmiennej `SMSAccountFrom` .
5. **Transferowanie poświadczenia dostawcy programu SMS w aplikacji**  
  
   Udostępnij poświadczenia i numer telefonu nadawcy aplikacji:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie przechowywania danych poufnych w kodzie źródłowym. Konto i poświadczenia są dodawane do kodu powyżej, aby uprościć przykład. Zobacz Jon Atten [platformy ASP.NET MVC: Zachowaj prywatne poza ustawienia kontroli źródła](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementacja transfer danych do dostawcy programu SMS**  
  
   Konfigurowanie `SmsService` klasy w *aplikacji\_Start\IdentityConfig.cs* pliku.  
  
   W zależności od dostawcy programu SMS używanej aktywować **Twilio** lub **ASPSMS** sekcji: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Uruchom aplikację i zaloguj się przy użyciu konta które wcześniej zarejestrowane.
8. Kliknij swój identyfikator użytkownika, które uaktywnia się `Index` metody akcji w `Manage` kontrolera.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Kliknij przycisk Dodaj.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. W ciągu kilku sekund otrzymasz wiadomość SMS z kodem weryfikacyjnym. Wprowadź go, a następnie naciśnij klawisz **przesyłania**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. W widoku Zarządzanie pokazuje, że numer telefonu został dodany.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Poszukaj w kodzie

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

`Index` Metody akcji w `Manage` kontrolera ustawia komunikat o stanie, oparte na poprzedniej akcji i zawiera łącza, aby zmienić hasło lokalne lub dodać konto lokalne. `Index` Metoda również Wyświetla stan lub usługi uwierzytelniania 2FA telefonów, numer, logowań zewnętrznych, funkcji 2FA, włączona i pamiętać metody uwierzytelniania 2FA, w tej przeglądarce (opisana w dalszej części). Klikając nazwę użytkownika (adres e-mail) na pasku tytułu nie przeszło wiadomość. Kliknięcie **numer telefonu: Usuń** link przebiegów `Message=RemovePhoneSuccess` jako ciąg zapytania.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber` Metody akcji, wyświetlane jest okno dialogowe, aby wprowadzić numer telefonu, który może odbierać wiadomości SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Kliknięcie **Wyślij kod weryfikacyjny** przycisk wpisów numer telefonu HTTP POST `AddPhoneNumber` metody akcji.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

`GenerateChangePhoneNumberTokenAsync` Metoda generuje token zabezpieczający, które zostaną ustawione w wiadomości SMS. Jeśli usługa programu SMS został skonfigurowany, token jest wysyłany jako parametry &quot;Twój kod zabezpieczający: &lt;tokenu&gt;&quot;. `SmsService.SendAsync` Metoda jest wywoływana asynchronicznie, a następnie aplikacja jest przekierowywane do `VerifyPhoneNumber` metody akcji, (która wyświetla następujące okno dialogowe), gdzie można wprowadzić kod weryfikacyjny.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Po wprowadzeniu kodu i kliknij przycisk Prześlij, aby HTTP POST opublikowaniu kodu `VerifyPhoneNumber` metody akcji.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

`ChangePhoneNumberAsync` Metoda sprawdza kod zabezpieczeń opublikowane. Jeśli kod jest poprawny, numer telefonu jest dodawany do `PhoneNumber` pola `AspNetUsers` tabeli. Jeśli to wywołanie zakończy się pomyślnie, `SignInAsync` metoda jest wywoływana:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

`isPersistent` Parametr określa, czy sesja uwierzytelniania jest trwała dla wielu żądań.

Zmiany profilu zabezpieczeń nową sygnaturę bezpieczeństwa jest wygenerowany i przechowywany w `SecurityStamp` pole *AspNetUsers* tabeli. Uwaga: `SecurityStamp` pola różni się od pliku cookie zabezpieczeń. Plik cookie zabezpieczeń nie są przechowywane w `AspNetUsers` tabeli (lub dowolnego miejsca w bazie danych tożsamości). Token pliku cookie zabezpieczeń jest z podpisem własnym za pomocą [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) i jest tworzona przy użyciu `UserId, SecurityStamp` i informacji czasu wygaśnięcia.

Oprogramowaniu pośredniczącym pliku cookie sprawdza, czy plik cookie na każde żądanie. `SecurityStampValidator` Method in Class metoda `Startup` klasy trafienia bazy danych i okresowo sprawdza sygnaturę bezpieczeństwa określone za pomocą `validateInterval`. Dzieje się to tylko co 30 minut (w naszym przykładzie) Jeśli nie zmienisz swój profil zabezpieczeń. W okresie 30 minut została wybrana, aby zminimalizować przesłania do bazy danych.

`SignInAsync` Metody musi być wywoływany w przypadku wszelkich zmian do profilu zabezpieczeń. Po zmianie profil zabezpieczeń bazy danych jest aktualizacje `SecurityStamp` pola i bez wywoływania `SignInAsync` metody, czy użytkownik pozostanie zalogowany w *tylko* dopiero od następnej potoku OWIN uderza w bazie danych ( `validateInterval`). Można to sprawdzić, zmieniając `SignInAsync` metody do zwrócenia natychmiast, a ustawienie pliku cookie `validateInterval` właściwości od 30 minut na 5 sekund:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Za pomocą powyższych zmian w kodzie, można zmienić swój profil zabezpieczeń (na przykład przez zmianę stanu **dwie włączone współczynnik**) i użytkownik zostanie wylogowany w ciągu 5 sekund po `SecurityStampValidator.OnValidateIdentity` metoda kończy się niepowodzeniem. Usuń wiersz zwrotu w `SignInAsync` metody, ustawić inny profil zabezpieczeń, zmiany i nie będziesz się logować. `SignInAsync` Metoda generuje nowy plik cookie zabezpieczeń.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Włączanie uwierzytelniania dwuskładnikowego

W przykładowej aplikacji należy włączyć uwierzytelnianie dwuskładnikowe (2FA) za pomocą interfejsu użytkownika. Aby włączyć funkcję 2FA, kliknij swoją nazwę użytkownika (alias adresu e-mail) na pasku nawigacyjnym.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Kliknij pozycję Włącz 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Wyloguj się, a następnie zaloguj się ponownie. Po włączeniu poczty e-mail (zobacz mój [poprzedniego samouczka](account-confirmation-and-password-recovery-with-aspnet-identity.md)), możesz wybrać wiadomości SMS lub wiadomości e-mail dla funkcji 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Zostanie wyświetlona strona Sprawdź kodu, gdzie można wprowadzić kod (z wiadomości SMS lub wiadomości e-mail).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Kliknięcie **pamiętasz tę przeglądarkę** pole wyboru zostaną wykluczone z konieczności korzystania z funkcji 2FA, zaloguj się przy użyciu tego komputera i przeglądarki. Włączanie funkcji 2FA, a następnie klikając **pamiętasz tę przeglądarkę** udostępni silnego uwierzytelniania 2FA, ochronę przed złośliwymi użytkownikami podjęcie próby dostępu do Twojego konta, tak długo, jak nie mają dostępu do komputera. Można to zrobić na dowolnej maszynie prywatnych, regularnie używane. Ustawiając **pamiętasz tę przeglądarkę**, uzyskaj dodatkowe zabezpieczenie w postaci funkcji 2FA z komputerów, które nie są regularnie używane i Uzyskaj wygody na ominięcie za pomocą funkcji 2FA na własnych komputerów. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Jak zarejestrować dostawcę uwierzytelniania dwuskładnikowego

Podczas tworzenia nowego projektu MVC *IdentityConfig.cs* plik zawiera następujący kod, aby zarejestrować dostawcę uwierzytelniania dwuetapowego:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Dodaj numer telefonu dla funkcji 2FA

`AddPhoneNumber` Metody akcji w `Manage` kontrolera generuje token zabezpieczający, a następnie wysyła je z numerem telefonu numeru, jaki podano.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Po wysłaniu token, zostanie przekierowany do `VerifyPhoneNumber` metody akcji, w którym można wprowadzić kod, aby zarejestrować programu SMS dla uwierzytelniania 2FA. 2FA programu SMS nie jest używana do momentu zweryfikowania numeru telefonu.

## <a name="enabling-2fa"></a>Włączanie funkcji 2FA

`EnableTFA` Metody akcji umożliwia funkcji 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Uwaga `SignInAsync` musi zostać wywołana, ponieważ Włączanie funkcji 2FA według regionów powoduje zmianę do profilu zabezpieczeń. Po włączeniu funkcji 2FA, użytkownik musi zalogować się, za pomocą funkcji 2FA przy użyciu metod uwierzytelniania 2FA, zarejestrowanych (wiadomości SMS i wiadomości e-mail w przykładzie).

Możesz dodać więcej dostawców uwierzytelniania 2FA, takie jak generatory kodu QR lub napisać jesteś właścicielem (zobacz [za pomocą tokenu Google Authenticator w produkcie ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Kody uwierzytelniania 2FA są generowane przy użyciu [oparte na czasie algorytm hasła jednorazowego](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) i kody są ważne przez 6 minut. Jeśli nie podejmiesz więcej niż sześciu minut w celu wprowadzenia kodu, otrzymasz komunikat o błędzie nieprawidłowego kodu.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Łączenie kont społecznościowych i lokalne logowanie

Konta lokalne i społecznościowych można łączyć, klikając link wiadomości e-mail. W następującej kolejności &quot; RickAndMSFT@gmail.com &quot; najpierw jest tworzony podczas logowania lokalnego, ale możesz utworzyć konto jako dziennik społecznościowych najpierw, a następnie dodaj lokalny identyfikator logowania.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Kliknij pozycję **Zarządzaj** łącza. Należy pamiętać, zewnętrzne 0 (logowania społecznościowego) skojarzony z tym kontem.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Kliknij link do innego dziennika w usłudze i akceptowania żądań aplikacji. Te dwa konta zostały połączone, będzie można zalogować się przy użyciu dowolnego konta. Możesz zechcieć użytkownikom dodawanie kont lokalnych, w przypadku, gdy ich społecznościowych logowania usługi uwierzytelniania nie działa lub większe prawdopodobieństwo po utracie dostępu do kont społecznościowych.

Na poniższej ilustracji, Tom jest społecznościowych logowania (który można wyświetlić z **logowania zewnętrzne: 1** wyświetlany na stronie).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Kliknięcie **Wybierz hasło** pozwala na dodawanie w lokalnym dzienniku na skojarzone z tego samego konta.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Blokada konta przed atakami siłowymi

Konta w aplikacji na ataki słownikowe można chronić przez włączenie blokady użytkownika. Poniższy kod w `ApplicationUserManager Create` metoda konfiguruje blokady:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Powyższy kod umożliwia blokady dla tylko z uwierzytelniania dwuskładnikowego. Chociaż można włączyć blokady dla nazw logowania, zmieniając `shouldLockout` na wartość true w `Login` metody kontroler kont, firma Microsoft zaleca, możesz włączyć nie blokady dla nazw logowania, ponieważ to sprawia, że konto podatne na [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) logowania ataki. W przykładowym kodzie blokada jest wyłączona, konto administratora jest utworzony w `ApplicationDbInitializer Seed` metody:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Wymaganie użytkownika konta e-mail zweryfikowane

Poniższy kod wymaga od użytkownika posiadania konta zatwierdzonych wiadomości e-mail przed ich może zalogować się:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Jak SignInManager sprawdza, czy wymaganie uwierzytelniania 2FA

Lokalne logowanie i społecznościowych logowania Sprawdź, czy włączono uwierzytelniania 2FA. Po włączeniu funkcji 2FA `SignInManager` logowania, metoda zwraca `SignInStatus.RequiresVerification`, a użytkownik zostanie przekierowany do `SendCode` metody akcji, w którym konieczne będzie wprowadzenie kodu do wykonania w dzienniku w sekwencji. Jeśli użytkownik ma RememberMe jest ustawiona na użytkowników lokalnych plików cookie, `SignInManager` zwróci `SignInStatus.Success` a nie zostaną wprowadzone za pośrednictwem funkcji 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Poniższy kod przedstawia `SendCode` metody akcji. A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) są tworzone za pomocą wszystkich metod uwierzytelniania 2FA, włączone dla danego użytkownika. [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) jest przekazywany do [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) pomocnika, która umożliwia użytkownikowi wybranie metody uwierzytelniania 2FA (zwykle adres e-mail i wiadomości SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Po użytkownik wysyła podejście funkcji 2FA, `HTTP POST SendCode` metody akcji jest wywoływana, `SignInManager` wysyła kodu funkcji 2FA, a użytkownik jest przekierowywany do `VerifyCode` metody akcji, w którym można wprowadzić kod, aby zakończyć logowanie.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>Funkcji 2FA blokady

Mimo że można ustawić blokady konta na niepowodzenia próby logowania do hasła, takie podejście sprawia, że Twoje dane logowania podatne na [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) blokady. Zaleca się, że używasz blokady konta tylko za pomocą funkcji 2FA. Gdy `ApplicationUserManager` jest tworzony, przykładowy kod ustawia blokady funkcji 2FA i `MaxFailedAccessAttemptsBeforeLockout` do pięciu. Po zalogowaniu się użytkownika (za pomocą konta lokalnego lub konta społecznościowego), każdy nieudanej próby w funkcji 2FA są przechowywane i w przypadku osiągnięcia maksymalnej liczby prób użytkownik jest zablokowany przez pięć minut (można ustawić blokady razem z `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Dodatkowe zasoby

- [ASP.NET Identity — zalecane zasoby](../getting-started/aspnet-identity-recommended-resources.md) pełną listę tożsamości blogi, pliki wideo, samouczki i great WIĘC łączy.
- [MVC 5 aplikacji za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowanie jednokrotne](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) także przedstawiono sposób dodawania informacji o profilu z tabelą użytkowników.
- [ASP.NET MVC i tożsamości w wersji 2.0: zrozumienie podstaw](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) przez Atten Jan.
- [Potwierdzenie konta i odzyskiwanie hasła w produkcie ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Wprowadzenie do produktu ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Ogłoszenie RTM produktu ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) przez autorem jest Pranav Rastogi.
- [Tożsamości ASP.NET 2.0: Sprawdzanie poprawności konta i autoryzacji Two-Factor Authentication konfigurowania](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) przez Atten Jan.
