---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: "Uwierzytelnianie dwuskładnikowe przy użyciu programu SMS i wiadomości e-mail z tożsamości ASP.NET | Dokumentacja firmy Microsoft"
author: HaoK
description: "W tym samouczku opisano, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) przy użyciu programu SMS i wiadomości e-mail. Ten artykuł dotyczy autorstwa Ricka Andersona ( @RickAndMSFT ), wzór..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0f9ff7cf74048a008b150da1e843ff15333269ab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Uwierzytelnianie dwuskładnikowe przy użyciu programu SMS i wiadomości e-mail z tożsamości platformy ASP.NET
====================
przez [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> W tym samouczku opisano, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) przy użyciu programu SMS i wiadomości e-mail.
> 
> Ten artykuł dotyczy autorstwa Ricka Andersona ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung i Suhas Joshi. Przykładowe NuGet zapisał głównie Hao Kung.


W tym temacie omówiono następujące czynności:

- [Tworzenie przykładowej tożsamości](#build)
- [Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego](#SMS)
- [Włącz uwierzytelnianie dwuskładnikowe](#enable2)
- [Jak zarejestrować dostawcę uwierzytelniania dwuskładnikowego](#reg)
- [Łączenie kont społecznościowych i lokalne logowanie](#combine)
- [Blokada konta przed atakami siłowymi](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Tworzenie przykładowej tożsamości

W tej sekcji użyjesz NuGet można pobrać przykładowy zajmiemy się. Rozpocznij od instalowania i uruchamiania [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej.

> [!NOTE]
> Ostrzeżenie: Należy zainstalować program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) do ukończenia tego samouczka.


1. Utwórz nową ***pusty*** projektu sieci Web ASP.NET.
2. W konsoli Menedżera pakietów, wprowadź następujące polecenia:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
 W tym samouczku użyjemy [SendGrid](http://sendgrid.com/) do wysyłania wiadomości e-mail i [usługi Twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) dla tekstowe programu sms. `Identity.Samples` Pakiet instaluje firma Microsoft będzie działać z kodu.
3. Ustaw [projektu do używania protokołu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Opcjonalne*: postępuj zgodnie z instrukcjami w mojej [samouczek potwierdzenie poczty E-mail](account-confirmation-and-password-recovery-with-aspnet-identity.md) do podpinanie SendGrid, a następnie uruchom aplikację i zarejestrować konto e-mail.
5. * Opcjonalnie: * usunąć kod potwierdzenia pokaz e-mail łączy z próbki ( `ViewBag.Link` kodu w kontrolerze konta. Zobacz `DisplayEmail` i `ForgotPasswordConfirmation` metody akcji i widokami razor).
6. * Opcjonalnie: * Usuń `ViewBag.Status` kodu z kontrolerami zarządzania i konta i *Views\Account\VerifyCode.cshtml* i *Views\Manage\VerifyPhoneNumber.cshtml* widokami razor. Alternatywnie można zachować `ViewBag.Status` wyświetlania, aby sprawdzić, jak ta aplikacja działa lokalnie, bez konieczności Podłączanie i wysyłanie wiadomości e-mail i wiadomości SMS.

> [!NOTE]
> Ostrzeżenie: Jeśli zmienisz jakiekolwiek ustawienia zabezpieczeń w tym przykładzie produkcji aplikacje będą wymagały zastosowane inspekcji zabezpieczeń, która jawnie uwidacznia zmiany wprowadzone do.


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego

Ten samouczek zawiera instrukcje dotyczące korzystania z usługi Twilio lub ASPSMS, ale można użyć dowolnego dostawcy programu SMS.

1. **Tworzenie konta użytkownika z dostawcą programu SMS**  
  
 Utwórz [usługi Twilio](https://www.twilio.com/try-twilio) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) konta.
2. **Instalowanie dodatkowych pakietów lub Dodawanie odwołań do usługi**  
  
 Usługi Twilio:  
 W konsoli Menedżera pakietów wprowadź następujące polecenie:  
    `Install-Package Twilio`  
  
 ASPSMS:  
 Następujące odwołanie do usługi musi zostać dodany:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
 Adres:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 Przestrzeń nazw:  
    `ASPSMSX2`
3. **Ustaleniem, poświadczenia użytkownika dostawcy programu SMS**  
  
 Usługi Twilio:  
 Z **pulpitu nawigacyjnego** kartę konta usługi Twilio kopiowania **identyfikator SID konta** i **token uwierzytelniania**.  
  
 ASPSMS:  
 W ustawieniach konta, przejdź do **Userkey** i skopiować go razem z własnym zdefiniowanych **hasło**.  
  
 Te wartości będą przechowywane później w zmiennych `SMSAccountIdentification` i `SMSAccountPassword` .
4. **Określanie SenderID / inicjator**  
  
 Usługi Twilio:  
 Z **numery** karcie, skopiuj numer telefonu usługi Twilio.  
  
 ASPSMS:  
 W ramach **odblokować nadawcy** Menu, odblokuj co najmniej jednego nadawcy, lub Wybierz zleceniodawcę alfanumeryczne (nieobsługiwane przez wszystkie sieci).  
  
 Ta wartość zostanie później przechowywane w zmiennej `SMSAccountFrom` .
5. **Transferowanie poświadczenia dostawcy programu SMS w aplikacji**  
  
 Udostępnij poświadczenia i numer telefonu nadawcy aplikacji:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie magazynu danych poufnych w kodzie źródłowym. Konta i poświadczenia zostaną dodane do powyżej, aby zachować prosty przykład kodu. Zobacz Jan Atten [platformy ASP.NET MVC: Zachowaj prywatnego poza ustawień kontroli źródła](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementacja transferu danych do dostawcy programu SMS**  
  
 Skonfiguruj `SmsService` klasy w *aplikacji\_Start\IdentityConfig.cs* pliku.  
  
 W zależności od dostawcy programu SMS używanego aktywować **usługi Twilio** lub **ASPSMS** sekcji: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Uruchom aplikację i zaloguj się przy użyciu konta, które wcześniej zarejestrowane.
8. Kliknij nazwę użytkownika, który uaktywnia `Index` metody akcji w `Manage` kontrolera.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Kliknij przycisk Dodaj.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. Za chwilę otrzymasz wiadomość SMS z kodem weryfikacyjnym. Wprowadź go i naciśnij klawisz **przesyłania**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. Zarządzaj ilustracja pokazuje numer telefonu został dodany.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Sprawdź kod

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

`Index` Metody akcji w `Manage` kontroler Ustawia komunikat o stanie oparte na sieci poprzedniej akcji i zawiera łącza, aby zmienić hasło lokalne lub dodać konta lokalnego. `Index` Metoda wyświetla również stan lub 2FA Twojego telefonu numer, logowań zewnętrznych, 2FA włączone i zapamiętać 2FA metoda ta przeglądarka (co omówiono później). Klikając nazwę użytkownika (poczta e-mail) na pasku tytułu nie przeszło wiadomości. Kliknięcie **numer telefonu: Usuń** link przekazuje `Message=RemovePhoneSuccess` jako ciąg zapytania.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber` Metody akcji Wyświetla okno dialogowe, aby wprowadzić numer telefonu, który może odbierać wiadomości SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Kliknięcie **wysłać kod weryfikacyjny** przycisk wpisów numer telefonu HTTP POST `AddPhoneNumber` metody akcji.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

`GenerateChangePhoneNumberTokenAsync` Metoda generuje tokenu zabezpieczającego, które zostaną ustawione w wiadomości SMS. Jeśli usługa programu SMS został skonfigurowany, token jest wysyłany jako ciąg &quot;Twój kod zabezpieczający &lt;tokenu&gt;&quot;. `SmsService.SendAsync` Metoda jest wywoływana asynchronicznie, a następnie aplikacja jest przekierowywany do `VerifyPhoneNumber` metody akcji (która wyświetla to okno dialogowe), gdzie można wprowadzić kod weryfikacyjny.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Po wprowadź kod i kliknij przycisk Prześlij kod jest zamieszczana HTTP POST `VerifyPhoneNumber` metody akcji.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

`ChangePhoneNumberAsync` Metoda sprawdza kod oczekujących na opublikowanie zabezpieczeń. Jeśli kod jest poprawny, numer telefonu jest dodawany do `PhoneNumber` pole `AspNetUsers` tabeli. Jeśli tego wywołania zakończy się pomyślnie, `SignInAsync` metoda jest wywoływana:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

`isPersistent` Parametr określa, czy sesja uwierzytelniania jest trwała dla wielu żądań.

Jeśli zmienisz profil zabezpieczeń nową sygnaturę bezpieczeństwa są generowane i przechowywane w `SecurityStamp` pole *AspNetUsers* tabeli. Uwaga: `SecurityStamp` pola różni się od pliku cookie zabezpieczeń. Plik cookie zabezpieczeń nie są przechowywane w `AspNetUsers` tabeli (lub dowolnego miejsca w bazie danych tożsamości). Token pliku cookie zabezpieczeń jest podpisany przy użyciu [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) i zostanie utworzony z `UserId, SecurityStamp` i informacje dotyczące czasu wygaśnięcia.

Oprogramowanie pośredniczące plików cookie sprawdza, czy plik cookie na każde żądanie. `SecurityStampValidator` Metody w `Startup` klasy trafienia bazę danych i okresowo sprawdza sygnaturę bezpieczeństwa z określonej `validateInterval`. Tylko dzieje co 30 minut (w naszym przykładzie), chyba że zostanie zmienione profil zabezpieczeń. Aby zminimalizować rund do bazy danych wybrano 30 minut.

`SignInAsync` Metoda musi być wywoływany w przypadku wszelkich zmian do profilu zabezpieczeń. Po zmianie profil zabezpieczeń bazy danych jest aktualizacje `SecurityStamp` pól i bez wywoływania elementu `SignInAsync` metody, czy użytkownik pozostanie zalogowany w *tylko* czasu kolejnego potoku OWIN trafienia bazy danych ( `validateInterval`). Można to sprawdzić, zmieniając `SignInAsync` metodę, aby zwrócić natychmiast i ustawienie pliku cookie `validateInterval` właściwości od 30 minut na 5 sekund:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Z powyższych zmiany kodu, można zmienić Twój profil zabezpieczeń (na przykład, zmieniając stan **dwie włączone współczynnik**) i będziesz się logować w ciągu 5 sekund po `SecurityStampValidator.OnValidateIdentity` metoda kończy się niepowodzeniem. Usuń wiersz zwracany w `SignInAsync` metody, ustawić inny profil zabezpieczeń zmiany i nie będziesz się logować. `SignInAsync` Metoda generuje nowy plik cookie zabezpieczeń.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Włącz uwierzytelnianie dwuskładnikowe

W przykładowej aplikacji należy użyć interfejsu użytkownika, aby włączyć uwierzytelnianie dwuskładnikowe (2FA). Aby włączyć 2FA, kliknij nazwę użytkownika (alias e-mail) na pasku nawigacyjnym.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Kliknij pozycję Włącz 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Wyloguj się, a następnie ponowne zalogowanie. Jeśli włączono poczty e-mail (Zobacz Moje [poprzedniego samouczek](account-confirmation-and-password-recovery-with-aspnet-identity.md)), możesz wybrać SMS lub wiadomości e-mail w przypadku 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Zostanie wyświetlona strona Sprawdź kod, gdzie można wprowadzić kod (z programu SMS lub e-mail).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Kliknięcie **Pamiętaj przeglądarka** pola wyboru będzie zwalnia z konieczności korzystania z 2FA logowania się do tego komputera i przeglądarki. Włączanie 2FA i klikając **Pamiętaj przeglądarka** zapewnia 2FA silnej ochrony przed złośliwymi użytkownikami próby dostęp do tego konta, dopóki nie mają dostępu do tego komputera. Można to zrobić na dowolnym komputerze prywatne, regularnie używane. Przez ustawienie **Pamiętaj przeglądarka**, Pobierz zwiększa bezpieczeństwo 2FA z komputerów, które nie są regularnie używane i pobrać wygody na ominięcie za pośrednictwem 2FA własnych komputerów. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Jak zarejestrować dostawcę uwierzytelniania dwuskładnikowego

Podczas tworzenia nowego projektu MVC *IdentityConfig.cs* plik zawiera następujący kod, aby zarejestrować dostawcę uwierzytelniania dwuetapowego:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Dodaj numer telefonu dla 2FA

`AddPhoneNumber` Metody akcji w `Manage` kontrolera generuje token zabezpieczający i wysyła go z numerem telefonu numeru, jaki podano.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Po wysłaniu token, przekierowuje do `VerifyPhoneNumber` metody akcji, w którym można wprowadzić kod, aby zarejestrować SMS 2FA. 2FA programu SMS nie jest używany, dopóki nie upewnisz się numer telefonu.

## <a name="enabling-2fa"></a>Włączanie 2FA

`EnableTFA` 2FA umożliwia metody akcji:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Uwaga `SignInAsync` musi zostać wywołana, ponieważ 2FA Włącz zmianę profil zabezpieczeń. Po włączeniu 2FA użytkownik będzie musiał używać do logowania, 2FA przy użyciu podejścia 2FA zarejestrowanych (SMS i poczty e-mail w próbce).

Można dodać więcej dostawców 2FA, takie jak generatory kodu QR lub napisania własnej (zobacz [za pomocą tokenu Google Authenticator z ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Kody 2FA są generowane przy użyciu [oparte na czasie algorytm hasła jednorazowego](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) i kody są ważne przez sześć minut. Jeśli skorzystasz z więcej niż sześciu minut wprowadź kod, zostanie wyświetlony komunikat o błędzie nieprawidłowy kod.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Łączenie kont społecznościowych i lokalne logowanie

Konta lokalne i społecznościowych można łączyć, klikając link do wiadomości e-mail. W następującej kolejności &quot; RickAndMSFT@gmail.com &quot; najpierw zostanie utworzona jako logowania lokalnego, ale można utworzyć konta społecznościowych logowania w pierwszym, a następnie dodaj lokalny identyfikator logowania.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Polecenie **Zarządzaj** łącza. Uwaga zewnętrznych 0 (społecznościowych logowania) skojarzone z tym kontem.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Kliknij łącze do innego dziennika w usłudze i akceptowania żądań aplikacji. Dwa konta zostały połączone, będzie można logować się na każdym koncie. Możesz użytkownikom dodawanie kont lokalnych, w przypadku ich społecznościowych dziennika w usłudze uwierzytelniania jest wyłączony lub najprawdopodobniej po utracie dostępu do swojego konta społecznościowych.

Na poniższej ilustracji, Tomasz jest społecznościowych logowania (które można wyświetlić z **logowań zewnętrznych: 1** wyświetlany na stronie).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Kliknięcie **Wybierz hasło** umożliwia dodanie w lokalnym dzienniku na skojarzone z kontem.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Blokada konta przed atakami siłowymi

Konta w aplikacji przed atakami słownikowymi chronić przez włączenie blokady użytkownika. Poniższy kod w `ApplicationUserManager Create` metody konfiguruje blokady:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Powyższy kod umożliwia blokady dla tylko uwierzytelniania dwuskładnikowego. Podczas blokady dla nazwy logowania można włączyć, zmieniając `shouldLockout` na wartość true w `Login` metody kontrolera konta, zalecamy nie włączyć blokady dla nazwy logowania ponieważ sprawia, że konto podatne na [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) logowania ataki. W przykładowym kodzie, blokada jest wyłączona dla konta administratora utworzonego w `ApplicationDbInitializer Seed` metody:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Wymaganie użytkownika konta e-mail zweryfikowanych

Poniższy kod wymaga od użytkownika posiadania konta e-mail zweryfikowanych przed można zalogować:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Jak SignInManager wyszukuje 2FA wymaganie

Logowanie lokalne i społecznościowych logowania Sprawdź, czy włączono 2FA. Po włączeniu 2FA `SignInManager` logowania, metoda zwraca `SignInStatus.RequiresVerification`, a użytkownik zostanie przekierowany do `SendCode` metody akcji, w którym będzie musiał wprowadzić kod do wykonania w dzienniku w sekwencji. Jeśli użytkownik ma RememberMe jest ustawiony na użytkowników lokalnych plików cookie, `SignInManager` zwróci `SignInStatus.Success` i nie będzie musiał przejść przez 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Poniższy kod przedstawia `SendCode` metody akcji. A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) jest tworzony za pomocą metod 2FA włączone dla danego użytkownika. [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) jest przekazywana do [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) pomocnika, który umożliwia użytkownikowi wybranie metody 2FA (zazwyczaj poczty e-mail i SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Gdy użytkownik zapisuje podejście 2FA `HTTP POST SendCode` po wywołaniu metody akcji `SignInManager` wysyła kod 2FA, a użytkownik zostanie przekierowany do `VerifyCode` metody akcji, w którym mogą oni wprowadzić kod w celu zakończenia logowania.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2FA blokady

Mimo że można ustawić blokady konta na niepowodzenia próba hasło logowania, takiego podejścia sprawia, że logowanie podatne na [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) blokady. Firma Microsoft zaleca się, że używasz blokady konta tylko z 2FA. Gdy `ApplicationUserManager` jest utworzony, przykładowy kod ustawia blokady 2FA i `MaxFailedAccessAttemptsBeforeLockout` pięć. Po zalogowaniu się użytkownika (za pomocą konta lokalnego lub konta społecznościowych), każdy nieudane próby 2FA są przechowywane i po osiągnięciu maksymalnej liczby prób użytkownik jest zablokowany przez pięć minut (można ustawić blokady czas z `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Dodatkowe zasoby

- [ASP.NET Identity zalecane zasobów](../getting-started/aspnet-identity-recommended-resources.md) pełną listę tożsamości blogi, wideo, samouczki i doskonałym tak łączy.
- [Aplikacji MVC 5 za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) także przedstawiono sposób dodawania informacji o profilu do tabeli użytkowników.
- [ASP.NET MVC i tożsamość 2.0: opis podstawowych funkcji](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) przez Atten Jan.
- [Potwierdzenie konta i hasło odzyskiwania za pomocą tożsamości platformy ASP.NET](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Wprowadzenie do produktu ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Anonsowanie RTM programu ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) przez Pranav Rastogi.
- [Tożsamości ASP.NET 2.0: Sprawdzanie poprawności konta i dwuskładnikowego autoryzacji konfigurowania](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) przez Atten Jan.
