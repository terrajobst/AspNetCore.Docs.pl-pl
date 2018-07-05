---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Tworzenie internetowego interfejsu platformy ASP.NET aplikacja formularzy przy użyciu uwierzytelniania dwuskładnikowego wiadomości SMS (C#) | Dokumentacja firmy Microsoft
author: Erikre
description: W tym samouczku przedstawiono sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą uwierzytelniania dwuskładnikowego. W tym samouczku został zaprojektowany jako uzupełnienie samouczek pod tytułem Cr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1231ed0cb238ad35c12405dd9a264ccfd7332960
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401295"
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Tworzenie internetowego interfejsu platformy ASP.NET aplikacja formularzy przy użyciu uwierzytelniania dwuskładnikowego wiadomości SMS (C#)
====================
przez [Erik Reitan](https://github.com/Erikre)

[Pobieranie aplikacji formularzy sieci Web ASP.NET za pomocą poczty E-mail i SMS procedury uwierzytelniania dwuskładnikowego](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> W tym samouczku przedstawiono sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą uwierzytelniania dwuskładnikowego. W tym samouczku został zaprojektowany jako uzupełnienie samouczek pod tytułem [tworzenie bezpiecznej aplikacji ASP.NET Web Forms z rejestracją użytkownika, wiadomości e-mail z potwierdzeniem i resetowaniem hasła](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Ponadto, w tym samouczku został oparty na Rick Anderson [samouczek MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Wprowadzenie

Ten samouczek przeprowadzi Cię przez kroki wymagane do tworzenia aplikacji formularzy sieci Web ASP.NET, która obsługuje uwierzytelnianie dwuskładnikowe przy użyciu programu Visual Studio. Uwierzytelnianie dwuskładnikowe jest krokiem uwierzytelniania dodatkowego użytkownika. Ten dodatkowy krok generuje unikatowy osobistego numeru identyfikacyjnego (PIN), podczas logowania. Numer PIN często są wysyłane do użytkownika jako wiadomości e-mail lub wiadomości SMS. Podczas logowania użytkownika aplikacji następnie przechodzi do numeru PIN jako środek dodatkowe uwierzytelnienie.

### <a name="tutorial-tasks-and-information"></a>Samouczek zadania i informacji:

- [Tworzenie aplikacji formularzy sieci Web platformy ASP.NET](#createWebForms)
- [Konfigurowanie wiadomości SMS i uwierzytelniania dwuskładnikowego](#SMS)
- [Włączanie uwierzytelniania dwuskładnikowego dla zarejestrowanego użytkownika](#use2FA)
- [Dodatkowe zasoby](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Tworzenie aplikacji formularzy sieci Web platformy ASP.NET

Rozpocznij od instalowania i uruchamiania [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub wyższej oraz. Ponadto musisz utworzyć [Twilio](https://www.twilio.com/try-twilio) konta, co zostało opisane poniżej.

> [!NOTE]
> Ważne: Musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej, aby ukończyć ten samouczek.


1. Utwórz nowy projekt (**pliku**  - &gt; **nowy projekt**) i wybierz **aplikacji sieci Web ASP.NET** szablonu wraz z .NET Framework w wersji 4.5.2 z **nowy projekt** okno dialogowe.
2. Z **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **formularzy sieci Web** szablonu. Pozostaw domyślne uwierzytelnianie jako **indywidualne konta użytkowników**. Następnie kliknij przycisk **OK** Aby utworzyć nowy projekt.  
    ![Okno dialogowe Nowy projekt ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Włączanie protokołu Secure Sockets Layer (SSL) dla projektu. Wykonaj kroki, które są dostępne w **Włącz SSL dla projektu** części [wprowadzenie do formularzy sieci Web serii samouczków](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. W programie Visual Studio, otwórz **Konsola Menedżera pakietów** (**narzędzia**  - &gt; **Menedżera pakietów NuGet**  - &gt; **Konsola Menedżera pakietów**), a następnie wprowadź następujące polecenie:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Konfigurowanie wiadomości SMS i uwierzytelniania dwuskładnikowego

W tym samouczku korzysta z usługi Twilio, ale można użyć dowolnego dostawcy programu SMS.

1. Tworzenie [Twilio](https://www.twilio.com/try-twilio) konta.
2. Z **pulpit nawigacyjny** karty konta usługi Twilio, kopiowania **identyfikator SID konta** i **tokenu uwierzytelniania.** Dodasz je do swojej aplikacji później.
3. Z **numery** kartę, skopiuj usługi Twilio **numer telefonu** także.
4. Wprowadź Twilio **identyfikator SID konta**, **tokenu uwierzytelniania** i **numer telefonu** dostępne dla aplikacji. Aby zachować ich prostotę będą przechowywane te wartości w *web.config* pliku. Podczas wdrażania na platformie Azure można przechowywać wartości, które są bezpieczne w **appSettings** Karta Konfigurowanie sekcji w witrynie sieci web. Ponadto dodając numer telefonu, używać tylko cyfry.   
   Zwróć uwagę, można również dodać poświadczenia usługi SendGrid. Usługa SendGrid jest Usługa powiadomień poczty e-mail. Aby uzyskać szczegółowe informacje o sposobie włączania usługi SendGrid, w sekcji "Punktu zaczepienia się SendGrid" samouczek pod tytułem [Utwórz aplikację bezpiecznego formularzy sieci Web ASP.NET z rejestracją użytkownika, wiadomości e-mail z potwierdzeniem i resetowaniem hasła.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie przechowywania danych poufnych w kodzie źródłowym. W tym przykładzie konto i poświadczenia są przechowywane w **appSettings** części *Web.config* pliku. Na platformie Azure, możesz bezpiecznie przechowywać te wartości na **[Konfiguruj](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** kartę w witrynie Azure portal. Aby uzyskać powiązane informacje, zobacz temat Rick Anderson pod tytułem [najlepsze rozwiązania dotyczące wdrażania haseł i innych danych poufnych na platformie ASP.NET i Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Konfigurowanie `SmsService` klasy w *aplikacji\_Start\IdentityConfig.cs* wyróżniane na żółty zmian w plikach, wprowadzając następujące czynności: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Dodaj następujący kod `using` instrukcje do stanu sprzed *IdentityConfig.cs* pliku: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Aktualizacja *Account/Manage.aspx* pliku przez usunięcie wierszy wyróżniony na żółto:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. W `Page_Load` program obsługi *Manage.aspx.cs* związanym z kodem, usuń znaczniki komentarza, następuje po wierszu kodu wyróżniony na żółto, tak aby pojawił się jako: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. W klasie codebehind z *konta*/*TwoFactorAuthenticationSignIn.aspx.cs*, zaktualizuj `Page_Load` program obsługi, dodając następujący kod wyróżniony na żółto: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Podejmując powyżej zmiana w kodzie, DropDownList "Providers", zawierający opcje uwierzytelniania nie zostaną zresetowane do pierwszej wartości. Umożliwi to użytkownikowi pomyślnie wybranie wszystkich opcji używany podczas uwierzytelniania i nie tylko w pierwszym.
10. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Default.aspx* i wybierz **Ustaw jako stronę startową**.
11. Testując aplikację, najpierw utwórz aplikację (**Ctrl**+**Shift**+**B**), a następnie uruchom aplikację (**F5**) i Wybierz opcję **zarejestrować** do tworzenia nowego konta użytkownika, lub wybierz **Zaloguj** Jeśli konto użytkownika zostało już zarejestrowane.
12. Po zalogowaniu mają (jako użytkownika) kliknij nazwę użytkownika (adres e-mail) na pasku nawigacyjnym, aby wyświetlić **Zarządzaj kontem** strony (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Kliknij przycisk **Dodaj** obok **numer telefonu** na **Zarządzaj kontem** strony.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Dodaj numer telefonu, na którym (jako użytkownika) chcesz otrzymywać wiadomości e-mail (wiadomości tekstowe), a następnie kliknij przycisk **przesyłania** przycisku.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    W tym momencie aplikacja będzie używać poświadczeń z *Web.config* do kontaktowania się z usługi Twilio. Wiadomości SMS (SMS) będą wysyłane z numerem telefonu skojarzony z kontem użytkownika. Aby sprawdzić, czy wysłano komunikat usługi Twilio, wyświetlając pulpit nawigacyjny Twilio.
15. W ciągu kilku sekund numer telefonu skojarzony z kontem użytkownika otrzyma wiadomość SMS z kodem weryfikacyjnym. Wprowadź kod weryfikacyjny i naciśnij klawisz **przesyłania**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Włączanie uwierzytelniania dwuskładnikowego dla zarejestrowanego użytkownika

W tym momencie włączono uwierzytelniania dwuskładnikowego dla aplikacji. Dla użytkownika do korzystania z uwierzytelniania dwuskładnikowego one po prostu zmienić ich ustawienia za pomocą interfejsu użytkownika. 

1. Jako użytkownik aplikacji można włączyć uwierzytelnianie dwuskładnikowe dla określonego konta, klikając identyfikator użytkownika (alias adresu e-mail) w pasku nawigacyjnym, aby wyświetlić **Zarządzaj kontem** strony. Następnie kliknij **Włącz** link, aby włączyć uwierzytelnianie dwuskładnikowe dla konta.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Wyloguj się, a następnie zaloguj się ponownie. Po włączeniu poczty e-mail, można wybrać wiadomości SMS lub wiadomości e-mail dla uwierzytelniania dwuskładnikowego. Jeśli nie zostały włączone poczty e-mail, zapoznaj się z samouczkiem pod tytułem [tworzenie aplikacji formularzy sieci Web platformy ASP.NET zabezpieczyć za pomocą rejestracji użytkowników, potwierdzenie adresu E-mail i resetowania hasła](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Zostanie wyświetlona strona uwierzytelnianie dwuskładnikowe, gdzie można wprowadzić kod (z wiadomości SMS lub wiadomości e-mail).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Kliknięcie **pamiętasz tę przeglądarkę** pole wyboru zostaną wykluczone z konieczności korzystania z uwierzytelniania dwuskładnikowego do logowania się w przypadku korzystania z przeglądarki i urządzenia gdy zaznaczono pole. Tak długo, jak złośliwi użytkownicy nie mogą uzyskać dostęp do urządzenia, włączanie uwierzytelniania dwuskładnikowego i klikając **pamiętasz tę przeglądarkę** zapewnia wygodny dostęp hasło jeden krok, przy jednoczesnym zachowaniu strong Ochrona uwierzytelniania dwuskładnikowego wszelki dostęp z innych zaufanych urządzeń. Można to zrobić, na dowolnym urządzeniu prywatnych, regularnie używane.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail w produkcie ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Łącza do produktu ASP.NET Identity zalecane zasoby](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Wdrażanie aplikacji formularzy bezpiecznej sieci Web platformy ASP.NET z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL w witrynie sieci Web platformy Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Seria samouczków formularzy sieci Web platformy ASP.NET — Dodaj dostawcę uwierzytelniania OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.NET Web Forms serii samouczków - Włącz SSL dla projektu](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Potwierdzenie konta i odzyskiwanie hasła w produkcie ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Tworzenie aplikacji w usłudze Facebook i łączenie aplikacji do projektu](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
