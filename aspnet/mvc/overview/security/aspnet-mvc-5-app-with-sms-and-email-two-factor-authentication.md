---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplikacja ASP.NET MVC 5 z programu SMS i adres e-mail uwierzytelniania dwuskładnikowego | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten samouczek przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET MVC 5 z uwierzytelniania dwuskładnikowego. Należy wykonać aplikację sieci web tworzenie bezpiecznej platformy ASP.NET MVC 5 z...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 5e1c54b3901f2c8c85134445c1fa91ee9f2e0d59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873615"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Aplikacja ASP.NET MVC 5 z programu SMS i adres e-mail uwierzytelniania dwuskładnikowego
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET MVC 5 z uwierzytelniania dwuskładnikowego. Należy wykonać [utworzenia bezpiecznego aplikacji sieci web platformy ASP.NET MVC 5 z dziennika w resetowania hasła i potwierdzania poczty e-mail](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) przed kontynuowaniem. Możesz pobrać ukończona aplikacja [tutaj](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Pobieranie zawiera debugowania wątków, które umożliwiają testowanie potwierdzenie adresu e-mail i SMS bez konfiguracji poczty e-mail lub dostawcy programu SMS.
> 
> W tym samouczku, została napisana przy [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Tworzenie aplikacji platformy ASP.NET MVC](#createMvc)
- [Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego](#SMS)
- [Włącz uwierzytelnianie dwuskładnikowe](#enable2)
- [Dodatkowe zasoby](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Tworzenie aplikacji platformy ASP.NET MVC

Rozpocznij od instalowania i uruchamiania [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej.

> [!NOTE]
> Ostrzeżenie: Należy wykonać [utworzenia bezpiecznego aplikacji sieci web platformy ASP.NET MVC 5 z dziennika w resetowania hasła i potwierdzania poczty e-mail](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) przed kontynuowaniem. Musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej do ukończenia tego samouczka.


1. Utwórz nowy projekt sieci Web ASP.NET i wybierz szablon MVC. Formularze sieci Web obsługuje również tożsamości platformy ASP.NET, można wykonać podobne kroki w aplikacji formularzy sieci web.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Pozostaw domyślne uwierzytelnianie jako **indywidualnych kont użytkowników**. Jeśli chcesz udostępniać aplikacji na platformie Azure, pozostaw zaznaczone pole wyboru. Później w samouczku będziemy zostanie wdrożona na platformie Azure. Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Ustaw [projektu do używania protokołu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Adres:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Przestrzeń nazw:  
    `ASPSMSX2`
3. **Ustaleniem, poświadczenia użytkownika dostawcy programu SMS**  
  
   Usługi Twilio:  
   Z **pulpitu nawigacyjnego** kartę konta usługi Twilio kopiowania **identyfikator SID konta** i **token uwierzytelniania**.  
  
   ASPSMS:  
   W ustawieniach konta, przejdź do **Userkey** i skopiować go razem z własnym zdefiniowanych **hasło**.  
  
   Przechowujemy później tych wartości w *web.config* pliku w kluczach `"SMSAccountIdentification"` i `"SMSAccountPassword"` .
4. **Określanie SenderID / inicjator**  
  
   Usługi Twilio:  
   Z **numery** karcie, skopiuj numer telefonu usługi Twilio.  
  
   ASPSMS:  
   W ramach **odblokować nadawcy** Menu, odblokuj co najmniej jednego nadawcy, lub Wybierz zleceniodawcę alfanumeryczne (nieobsługiwane przez wszystkie sieci).  
  
   Przechowujemy później tę wartość w *web.config* pliku w kluczu `"SMSAccountFrom"` .
5. **Transferowanie poświadczenia dostawcy programu SMS w aplikacji**  
  
   Udostępnij poświadczenia i numer telefonu nadawcy aplikacji. Aby zapewnić proste przechowujemy będzie tych wartości w *web.config* pliku. Gdy firma Microsoft wdrażanie na platformie Azure, firma Microsoft może przechowywać bezpiecznie w wartości **ustawień aplikacji** Karta Konfigurowanie sekcji w witrynie sieci web. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie magazynu danych poufnych w kodzie źródłowym. Konta i poświadczenia zostaną dodane do powyżej, aby zachować prosty przykład kodu. Zobacz [najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych do platformy ASP.NET i usługi Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementacja transferu danych do dostawcy programu SMS**  
  
   Skonfiguruj `SmsService` klasy w *aplikacji\_Start\IdentityConfig.cs* pliku.  
  
   W zależności od dostawcy programu SMS używanego aktywować **usługi Twilio** lub **ASPSMS** sekcji: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Aktualizacja *Views\Manage\Index.cshtml* widoku Razor: (Uwaga: nie po prostu usuń komentarze w kodzie istniejących, użyj poniższy kod.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Sprawdź `EnableTwoFactorAuthentication` i `DisableTwoFactorAuthentication` metod akcji w `ManageController` ma[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atrybutu:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Uruchom aplikację i zaloguj się przy użyciu konta, które wcześniej zarejestrowane.
10. Kliknij nazwę użytkownika, który uaktywnia `Index` metody akcji w `Manage` kontrolera.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Kliknij przycisk Dodaj.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. `AddPhoneNumber` Metody akcji Wyświetla okno dialogowe, aby wprowadzić numer telefonu, który może odbierać wiadomości SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. Za chwilę otrzymasz wiadomość SMS z kodem weryfikacyjnym. Wprowadź go i naciśnij klawisz **przesyłania**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. Zarządzaj ilustracja pokazuje numer telefonu został dodany.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Włącz uwierzytelnianie dwuskładnikowe

W aplikacji szablonu wygenerowanego należy użyć interfejsu użytkownika, aby włączyć uwierzytelnianie dwuskładnikowe (2FA). Aby włączyć 2FA, kliknij nazwę użytkownika (alias e-mail) na pasku nawigacyjnym.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Kliknij pozycję Włącz 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Wyloguj, następnie zalogować ponownie w. Jeśli włączono poczty e-mail (Zobacz Moje [poprzedniego samouczek](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), możesz wybrać SMS lub wiadomości e-mail w przypadku 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Zostanie wyświetlona strona Sprawdź kod, gdzie można wprowadzić kod (z programu SMS lub e-mail).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Kliknięcie **Pamiętaj przeglądarka** pola wyboru będzie zwalnia z konieczności służy do logowania podczas korzystania z przeglądarki i urządzenia, gdy zaznaczono pole 2FA. Tak długo, jak złośliwych użytkowników nie może uzyskać dostępu do urządzenia, włączanie 2FA i klikając **Pamiętaj przeglądarka** zapewnia najwygodniejszy dostęp hasła jeden krok, przy jednoczesnym zachowaniu 2FA silnej ochrony dostępu do całej z urządzeń z systemem innym niż zaufane. Można to zrobić na dowolnym urządzeniu prywatne, regularnie używane.

W tym samouczku przedstawiono krótkie wprowadzenie do włączania 2FA w nowej aplikacji ASP.NET MVC. Moje samouczek [uwierzytelniania dwuskładnikowego przy użyciu programu SMS i wiadomości e-mail z tożsamości ASP.NET](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) przejdzie do szczegółów w kodzie próbki.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Uwierzytelnianie dwuskładnikowe przy użyciu programu SMS i wiadomości e-mail z tożsamości ASP.NET](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) przechodzi do szczegółów dotyczących uwierzytelniania dwuskładnikowego
- [Łącza do tożsamości ASP.NET zalecane zasobów](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Konta potwierdzenie i hasło odzyskiwania za pomocą ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) przechodzi w bardziej szczegółowo na potwierdzenie hasła odzyskiwania i konta.
- [Aplikacji MVC 5 za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) w tym samouczku przedstawiono sposób pisania aplikacji platformy ASP.NET MVC 5 z usługi Facebook i Google OAuth 2 autoryzacji. Ponadto jak dodać dodatkowe dane do bazy danych tożsamości.
- [Wdrażanie aplikacji bezpiecznego platformy ASP.NET MVC z członkostwa, OAuth i bazy danych SQL Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). W tym samouczku dodaje wdrożenia usługi Azure zabezpieczania aplikacji za pomocą ról, jak używać interfejs API członkostwa można dodać użytkowników i role oraz dodatkowe funkcje zabezpieczeń.
- [Tworzenie aplikacji Google OAuth 2 i łączenie aplikacji z projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Tworzenie aplikacji w serwisie Facebook i łączenie aplikacji z projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Konfigurowanie protokołu SSL w projekcie](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
