---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplikacja ASP.NET MVC 5 z wiadomości SMS i wiadomości e-mail uwierzytelniania dwuskładnikowego | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym samouczku dowiesz się, jak utworzyć aplikację internetową platformy ASP.NET MVC 5 przy użyciu uwierzytelniania dwuskładnikowego. Należy wykonać tworzenie bezpiecznej platformy ASP.NET MVC 5 aplikacji internetowej za pomocą...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: f31015f5ebd660d0f95a2e978b305c1d272293d5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752493"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Aplikacja ASP.NET MVC 5 z wiadomości SMS i wiadomości e-mail uwierzytelniania dwuskładnikowego
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> W tym samouczku dowiesz się, jak utworzyć aplikację internetową platformy ASP.NET MVC 5 przy użyciu uwierzytelniania dwuskładnikowego. Należy wykonać [tworzenie bezpiecznej aplikacji sieci web ASP.NET MVC 5 z logowaniem, adres e-mail potwierdzenia i resetowaniem hasła](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) przed kontynuowaniem. Możesz pobrać gotową aplikację [tutaj](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Pobierany zasób zawiera debugowania wątków, które umożliwiają testowanie potwierdzenie adresu e-mail i SMS bez konfigurowania wiadomości e-mail lub dostawcy programu SMS.
> 
> Ten samouczek został napisany przez [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Tworzenie aplikacji ASP.NET MVC](#createMvc)
- [Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego](#SMS)
- [Włączanie uwierzytelniania dwuskładnikowego](#enable2)
- [Dodatkowe zasoby](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Tworzenie aplikacji ASP.NET MVC

Rozpocznij od instalowania i uruchamiania [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej.

> [!NOTE]
> Ostrzeżenie: Należy wykonać [tworzenie bezpiecznej aplikacji sieci web ASP.NET MVC 5 z logowaniem, adres e-mail potwierdzenia i resetowaniem hasła](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) przed kontynuowaniem. Należy zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej, aby ukończyć ten samouczek.


1. Utwórz nowy projekt sieci Web platformy ASP.NET, a następnie wybierz szablon MVC. Formularze sieci Web obsługuje również produktu ASP.NET Identity, dzięki czemu można wykonać podobne kroki w aplikacji formularzy sieci web.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Pozostaw domyślne uwierzytelnianie jako **indywidualne konta użytkowników**. Jeśli chcesz hostować aplikację na platformie Azure, pozostaw zaznaczone pole wyboru. W dalszej części tego samouczka wdrożymy na platformie Azure. Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Ustaw [projektu do używania protokołu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Adres:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Przestrzeń nazw:  
    `ASPSMSX2`
3. **Ustalenie, poświadczenia użytkownika dostawcy programu SMS**  
  
   Twilio:  
   Z **pulpit nawigacyjny** karty konta usługi Twilio, kopiowania **identyfikator SID konta** i **tokenu uwierzytelniania**.  
  
   ASPSMS:  
   W ustawieniach konta, przejdź do **Userkey** i skopiować go wraz z własnym zdefiniowanych **hasło**.  
  
   Te wartości będą przechowywane w dalszej części *web.config* pliku w kluczach `"SMSAccountIdentification"` i `"SMSAccountPassword"` .
4. **Określanie SenderID / inicjatora**  
  
   Twilio:  
   Z **numery** kartę, skopiuj numer telefonu usługi Twilio.  
  
   ASPSMS:  
   W ramach **odblokować nadawcy** Menu odblokować przynajmniej jednego nadawcy, lub wybierz inicjatora alfanumeryczne (nie obsługiwany przez wszystkie sieci).  
  
   Później będą przechowywane w tej wartości w *web.config* pliku w kluczu `"SMSAccountFrom"` .
5. **Transferowanie poświadczenia dostawcy programu SMS w aplikacji**  
  
   Udostępnij poświadczenia i numer telefonu nadawcy aplikacji. Aby zachować ich prostotę będą przechowywane te wartości w *web.config* pliku. Gdy firma Microsoft wdrażanie na platformie Azure, firma Microsoft może przechowywać bezpiecznie w wartości **ustawienia aplikacji** Karta Konfigurowanie sekcji w witrynie sieci web. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie przechowywania danych poufnych w kodzie źródłowym. Konto i poświadczenia są dodawane do kodu powyżej, aby uprościć przykład. Zobacz [najlepsze rozwiązania dotyczące wdrażania haseł i innych danych poufnych na platformie ASP.NET i Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementacja transfer danych do dostawcy programu SMS**  
  
   Konfigurowanie `SmsService` klasy w *aplikacji\_Start\IdentityConfig.cs* pliku.  
  
   W zależności od dostawcy programu SMS używanej aktywować **Twilio** lub **ASPSMS** sekcji: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Aktualizacja *Views\Manage\Index.cshtml* widoku Razor: (Uwaga: nie po prostu usuń komentarze w kodzie istniejącej, użyj poniższego kodu.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Sprawdź `EnableTwoFactorAuthentication` i `DisableTwoFactorAuthentication` metod akcji w `ManageController` mają[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atrybutu:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Uruchom aplikację i zaloguj się przy użyciu konta które wcześniej zarejestrowane.
10. Kliknij swój identyfikator użytkownika, które uaktywnia się `Index` metody akcji w `Manage` kontrolera.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Kliknij przycisk Dodaj.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. `AddPhoneNumber` Metody akcji, wyświetlane jest okno dialogowe, aby wprowadzić numer telefonu, który może odbierać wiadomości SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. W ciągu kilku sekund otrzymasz wiadomość SMS z kodem weryfikacyjnym. Wprowadź go, a następnie naciśnij klawisz **przesyłania**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. W widoku Zarządzanie pokazuje, że numer telefonu został dodany.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Włączanie uwierzytelniania dwuskładnikowego

W aplikacji wygenerowanego szablonu należy włączyć uwierzytelnianie dwuskładnikowe (2FA) za pomocą interfejsu użytkownika. Aby włączyć funkcję 2FA, kliknij swoją nazwę użytkownika (alias adresu e-mail) na pasku nawigacyjnym.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Kliknij pozycję Włącz 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Wylogowania, następnie zalogowania. Po włączeniu poczty e-mail (zobacz mój [poprzedniego samouczka](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), możesz wybrać wiadomości SMS lub wiadomości e-mail dla funkcji 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Zostanie wyświetlona strona Sprawdź kodu, gdzie można wprowadzić kod (z wiadomości SMS lub wiadomości e-mail).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Kliknięcie **pamiętasz tę przeglądarkę** pole wyboru zostaną wykluczone z konieczności korzystania z funkcji 2FA, zaloguj się w przypadku korzystania z przeglądarki i urządzenia gdy zaznaczono pole. Tak długo, jak złośliwi użytkownicy nie mogą uzyskać dostęp do urządzenia, włączanie funkcji 2FA i klikając **pamiętasz tę przeglądarkę** zapewnia wygodny dostęp hasło jeden krok, przy jednoczesnym zachowaniu silnego uwierzytelniania 2FA ochrony dostępu do całej z innych zaufanych urządzeń. Można to zrobić, na dowolnym urządzeniu prywatnych, regularnie używane.

Ten samouczek zawiera szybkie wprowadzenie do włączania funkcji 2FA w nowej aplikacji platformy ASP.NET MVC. Moje samouczek [uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail w produkcie ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) określa szczegółowo kod związany z przykładu.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail w produkcie ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) przechodzi do szczegółowych informacji dotyczących uwierzytelniania dwuskładnikowego
- [Łącza do produktu ASP.NET Identity zalecane zasoby](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Potwierdzenie konta i odzyskiwanie hasła w produkcie ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) zawiera bardziej szczegółowe potwierdzenie hasła odzyskiwania i konta.
- [MVC 5 aplikacji za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowanie jednokrotne](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) w tym samouczku dowiesz się, jak napisać aplikację ASP.NET MVC 5 z autoryzacją usługi Facebook i Google OAuth 2. Pokazano również, jak dodać dodatkowe dane do bazy danych tożsamości.
- [Wdrażanie bezpiecznej aplikacji ASP.NET MVC z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL na sieci Web platformy Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). W tym samouczku dodaje wdrażania platformy Azure, jak zabezpieczyć swoją aplikację przy użyciu ról, jak dodać użytkowników i ról i funkcji zabezpieczeń za pomocą członkostwo interfejsu API.
- [Tworzenie aplikacji Google dla protokołu OAuth 2 i łączenie aplikacji do projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Tworzenie aplikacji w usłudze Facebook i łączenie aplikacji do projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Konfigurowanie protokołu SSL w projekcie](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
