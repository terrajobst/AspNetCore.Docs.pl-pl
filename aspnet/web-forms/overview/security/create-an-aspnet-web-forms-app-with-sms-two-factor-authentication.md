---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Tworzenie sieci Web ASP.NET formularzy aplikacji za pomocą uwierzytelniania dwuskładnikowego programu SMS (C#) | Dokumentacja firmy Microsoft
author: Erikre
description: Ten samouczek przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą uwierzytelniania dwuskładnikowego. W tym samouczku zaprojektowano tak, aby mogła uzupełniać Samouczek zatytułowany Cr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 6c040fd3e0592b8cfd230dcd85ed3293f0a22ba7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Tworzenie sieci Web ASP.NET formularzy aplikacji za pomocą uwierzytelniania dwuskładnikowego programu SMS (C#)
====================
Przez [Erik Reitan](https://github.com/Erikre)

[Pobieranie aplikacji formularzy sieci Web ASP.NET za pomocą poczty E-mail i SMS uwierzytelniania dwuskładnikowego](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Ten samouczek przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą uwierzytelniania dwuskładnikowego. W tym samouczku zaprojektowano tak, aby mogła uzupełniać Samouczek zatytułowany [utworzyć bezpiecznego aplikacji składnika ASP.NET Web Forms z rejestracja użytkownika, poczty e-mail resetowania hasła i potwierdzania](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Ponadto, w tym samouczku została oparta na Rick Anderson [samouczek dla platformy MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Wprowadzenie

W tym samouczku przedstawiono kroki wymagane do tworzenia aplikacji formularzy sieci Web ASP.NET, która obsługuje uwierzytelnianie dwuskładnikowe przy użyciu programu Visual Studio. Uwierzytelnianie dwuskładnikowe jest to krok uwierzytelniania użytkownika dodatkowe. Ten krok dodatkowe generuje unikatowy osobistego numeru identyfikacyjnego (PIN) podczas logowania. Numer PIN jest zwykle wysyłane do użytkownika jako adres e-mail lub wiadomości SMS. Podczas podpisywania w użytkownika aplikacji następnie przechodzi do numeru PIN jako środek dodatkowe uwierzytelnienie.

### <a name="tutorial-tasks-and-information"></a>Samouczek zadań i informacji:

- [Tworzenie aplikacji formularzy sieci Web ASP.NET](#createWebForms)
- [Instalator programu SMS i uwierzytelniania dwuskładnikowego](#SMS)
- [Włącz uwierzytelnianie dwuskładnikowe dla zarejestrowanego użytkownika](#use2FA)
- [Dodatkowe zasoby](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Tworzenie aplikacji formularzy sieci Web ASP.NET

Rozpocznij od instalowania i uruchamiania [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub wyższej oraz. Ponadto należy utworzyć [usługi Twilio](https://www.twilio.com/try-twilio) konta, co zostało opisane poniżej.

> [!NOTE]
> Ważne: Należy zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej do ukończenia tego samouczka.


1. Utwórz nowy projekt (**pliku**  - &gt; **nowy projekt**) i wybierz **aplikacji sieci Web ASP.NET** szablonu wraz z programu .NET Framework w wersji 4.5.2 z **nowy projekt** okno dialogowe.
2. Z **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **formularzy sieci Web** szablonu. Pozostaw domyślne uwierzytelnianie jako **indywidualnych kont użytkowników**. Następnie kliknij przycisk **OK** do utworzenia nowego projektu.  
    ![Okno dialogowe Nowy projekt ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Włącz Secure Sockets Layer (SSL) dla projektu. Wykonaj kroki, które są dostępne w **Włącz SSL dla projektu** sekcji [wprowadzenie do korzystania z samouczka serii formularzy sieci Web](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. W programie Visual Studio Otwórz **Konsola Menedżera pakietów** (**narzędzia**  - &gt; **Menedżer pakietów NuGet**  - &gt; **Konsola Menedżera pakietów**), a następnie wprowadź następujące polecenie:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Instalator programu SMS i uwierzytelniania dwuskładnikowego

W tym samouczku korzysta z usługi Twilio, ale można korzystać z każdego dostawcy programu SMS.

1. Utwórz [usługi Twilio](https://www.twilio.com/try-twilio) konta.
2. Z **pulpitu nawigacyjnego** kartę konta usługi Twilio kopiowania **identyfikator SID konta** i **Token uwierzytelniania.** Możesz spowoduje dodanie ich do aplikacji później.
3. Z **numery** karcie, skopiować z usługi Twilio **numer telefonu** również.
4. Wprowadź usługi Twilio **identyfikator SID konta**, **Token uwierzytelniania** i **numer telefonu** dostępne dla aplikacji. Aby zachować prostych czynności będą przechowywane te wartości w *web.config* pliku. Podczas wdrażania na platformie Azure można przechowywać bezpiecznie w wartości **appSettings** Karta Konfigurowanie sekcji w witrynie sieci web. Ponadto podczas dodawania numer telefonu, używać tylko cyfry.   
   Należy zauważyć, że można również dodać poświadczeń SendGrid. SendGrid jest usługą powiadomień e-mail. Aby uzyskać szczegółowe informacje o sposobie włączania SendGrid, zobacz sekcję "Haku zapasowej SendGrid" samouczka zatytułowany [utworzona aplikacja Secure formularzy sieci Web ASP.NET za pomocą rejestracja użytkownika, e-mail resetowania hasła i potwierdzania.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie magazynu danych poufnych w kodzie źródłowym. W tym przykładzie konta i poświadczenia są przechowywane w **appSettings** sekcji *Web.config* pliku. Na platformie Azure, można bezpiecznie przechowywać te wartości na **[Konfiguruj](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** kartę w portalu Azure. Powiązane informacje, zobacz temat Rick Anderson zatytułowany [najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych do platformy ASP.NET i usługi Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Skonfiguruj `SmsService` klasy w *aplikacji\_Start\IdentityConfig.cs* żółty wyróżniane na zmiany pliku, wykonując następujące czynności: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Dodaj następujące `using` instrukcje na początku *IdentityConfig.cs* pliku: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Aktualizacja *Account/Manage.aspx* pliku przez usunięcie wierszy wyróżnione kolorem żółtym:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. W `Page_Load` obsługi *Manage.aspx.cs* związane z kodem, usuń znaczniki komentarza zgodny wiersz kodu wyróżnione kolorem żółtym, że jest wyświetlany jako: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. W plik codebehind z *konta*/*TwoFactorAuthenticationSignIn.aspx.cs*, zaktualizuj `Page_Load` obsługi przez dodanie poniższego kodu wyróżnione kolorem żółtym: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Wykonując powyższe kodu, zmień DropDownList "Providers" zawierający opcje uwierzytelniania nie zostaną zresetowane do pierwszej wartości. Dzięki temu użytkownikowi na wybranie pomyślnie wszystkich opcji podczas uwierzytelniania, nie tylko w pierwszym.
10. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Default.aspx* i wybierz **Ustaw jako stronę startową**.
11. Podczas testów aplikacji, należy najpierw utworzyć aplikację (**Ctrl**+**Shift**+**B**), a następnie uruchom aplikację (**F5**) i Wybierz opcję **zarejestrować** Aby utworzyć nowe konto użytkownika lub wybierz **Zaloguj** Jeśli konto użytkownika zostało już zarejestrowane.
12. Gdy użytkownik (przy użyciu konta użytkownika) ma zalogowany, kliknij przycisk identyfikatora użytkownika (adres e-mail) na pasku nawigacyjnym, aby wyświetlić **Zarządzaj kontem** strony (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Kliknij przycisk **Dodaj** obok **numer telefonu** na **Zarządzaj kontem** strony.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Numer telefonu, gdzie (przy użyciu konta użytkownika) chcesz otrzymywać wiadomości e-mail (wiadomości tekstowe), a następnie kliknij przycisk Dodaj **przesyłania** przycisku.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    W tym momencie aplikacja będzie używać poświadczeń z *Web.config* do kontaktowania się z usługi Twilio. Wiadomość SMS (wiadomości tekstowej) będą wysyłane z numerem telefonu skojarzony z kontem użytkownika. Aby sprawdzić, czy wysłano komunikat usługi Twilio, wyświetlając pulpit nawigacyjny usługi Twilio.
15. W ciągu kilku sekund telefonu skojarzony z kontem użytkownika otrzyma wiadomość tekstową zawierającą kod weryfikacyjny. Wprowadź kod weryfikacyjny i naciśnij klawisz **przesyłania**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Włącz uwierzytelnianie dwuskładnikowe dla zarejestrowanego użytkownika

W tym momencie włączono uwierzytelniania dwuskładnikowego dla aplikacji. Aby użytkownik mógł korzystać z uwierzytelniania dwuskładnikowego po prostu mogą zmienić ich ustawień za pomocą interfejsu użytkownika. 

1. Jako użytkownik aplikacji można włączyć uwierzytelniania dwuskładnikowego dla określonego konta, klikając nazwę użytkownika (alias e-mail) na pasku nawigacyjnym, aby wyświetlić **Zarządzaj kontem** strony. Następnie kliknij **włączyć** łącze do włączenia uwierzytelniania dwuskładnikowego dla konta.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Wyloguj się, a następnie ponowne zalogowanie. Jeśli włączono poczty e-mail, można wybrać SMS lub wiadomości e-mail w przypadku uwierzytelniania dwuskładnikowego. Jeśli nie włączono poczty e-mail, zobacz Samouczek zatytułowany [tworzenie aplikacji formularzy sieci Web ASP.NET Secure rejestracja użytkownika wiadomości E-mail z potwierdzeniem i resetowania hasła](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Zostanie wyświetlona strona uwierzytelnianie dwuskładnikowe, gdzie można wprowadzić kod (z programu SMS lub e-mail).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Kliknięcie **Pamiętaj przeglądarka** pola wyboru będzie zwalnia z konieczności korzystania z uwierzytelniania dwuskładnikowego do logowania podczas korzystania z przeglądarki i urządzenia, gdy zaznaczono pole. Tak długo, jak złośliwych użytkowników nie może uzyskać dostępu do urządzenia, włączania uwierzytelniania dwuskładnikowego i klikając **Pamiętaj przeglądarka** zapewnia najwygodniejszy dostęp hasła jeden krok, przy jednoczesnym zachowaniu strong Ochrona uwierzytelniania dwuskładnikowego dla każdego dostępu z urządzeń z systemem innym niż zaufane. Można to zrobić na dowolnym urządzeniu prywatne, regularnie używane.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail w produkcie ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Łącza do tożsamości ASP.NET zalecane zasobów](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Wdrażanie aplikacji formularzy bezpiecznej sieci Web platformy ASP.NET z członkostwa, OAuth i bazy danych SQL do witryny sieci Web platformy Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Samouczek serii formularzy sieci Web platformy ASP.NET — Dodaj dostawcę uwierzytelniania OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Serii samouczek formularzy sieci Web ASP.NET - Włącz SSL dla projektu](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Potwierdzenie konta i hasło odzyskiwania za pomocą tożsamości platformy ASP.NET](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Tworzenie aplikacji w serwisie Facebook i łączenie aplikacji z projektu](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
