---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Tworzenie bezpiecznej aplikacji sieci web ASP.NET MVC 5 z logowaniem, wiadomości e-mail z potwierdzeniem i resetowaniem hasła (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym samouczku dowiesz się, jak utworzyć aplikację sieci web ASP.NET MVC 5 z potwierdzenie adresu e-mail i resetowania haseł za pomocą systemu członkostwa ASP.NET Identity. Możesz urzędu certyfikacji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 56c1a5c414fdcece8d827d1187144b4948d8eb93
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387046"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Tworzenie bezpiecznej aplikacji sieci web ASP.NET MVC 5 z logowaniem, wiadomości e-mail z potwierdzeniem i resetowaniem hasła (C#)
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> W tym samouczku dowiesz się, jak utworzyć aplikację sieci web ASP.NET MVC 5 z potwierdzenie adresu e-mail i resetowania haseł za pomocą systemu członkostwa ASP.NET Identity. Możesz pobrać gotową aplikację [tutaj](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Pobierany zasób zawiera debugowania wątków, które umożliwiają testowanie potwierdzenie adresu e-mail i SMS bez konfigurowania wiadomości e-mail lub dostawcy programu SMS.
> 
> Ten samouczek został napisany przez [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Tworzenie aplikacji ASP.NET MVC

Rozpocznij od instalowania i uruchamiania [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej.

> [!NOTE]
> Ostrzeżenie: Musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej, aby ukończyć ten samouczek.


1. Utwórz nowy projekt sieci Web platformy ASP.NET, a następnie wybierz szablon MVC. Formularze sieci Web obsługuje również produktu ASP.NET Identity, dzięki czemu można wykonać podobne kroki w aplikacji formularzy sieci web.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Pozostaw domyślne uwierzytelnianie jako **indywidualne konta użytkowników**. Jeśli chcesz hostować aplikację na platformie Azure, pozostaw zaznaczone pole wyboru. W dalszej części tego samouczka wdrożymy na platformie Azure. Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Ustaw [projektu do używania protokołu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Uruchom aplikację, kliknij przycisk **zarejestrować** link i zarejestrować użytkownik. W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atrybutu.
5. W Eksploratorze serwera przejdź do **Connections\DefaultConnection\Tables\AspNetUsers danych**, kliknij prawym przyciskiem myszy i wybierz **Otwórz definicję tabeli**.

    Na poniższej ilustracji przedstawiono `AspNetUsers` schematu:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Kliknij prawym przyciskiem myszy **AspNetUsers** tabeli, a następnie wybierz pozycję **Pokaż dane tabeli**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 W tym momencie wiadomości e-mail nie został potwierdzony.
7. Kliknij wiersz, a następnie wybierz opcję Usuń. Dodasz tę wiadomość e-mail ponownie w następnym kroku i wysłać wiadomość e-mail z potwierdzeniem.

## <a name="email-confirmation"></a>Potwierdzenie adresu e-mail

Jest najlepszym rozwiązaniem potwierdzenia adresu e-mail nowych rejestracji, aby sprawdzić ich nie podszyć się pod kogoś innego (oznacza to one nie zostały zarejestrowane przy użyciu adresu e-mail osoby). Załóżmy, że masz forum dyskusyjne, czy chcesz uniemożliwić `"bob@example.com"` z rejestracją jako `"joe@contoso.com"`. Bez potwierdzenia e-mail `"joe@contoso.com"` można pobrać niechcianych wiadomości e-mail z aplikacji. Załóżmy, że Bob przypadkowo zarejestrowany jako `"bib@example.com"` i był wystąpieniem, użytkownik nie będzie mogła używać hasła odzyskiwania, ponieważ aplikacja nie ma jego prawidłowy adres e-mail. Potwierdzenie adresu e-mail zawiera tylko ograniczoną ochronę z botami i nie zapewnia ochrony z spamerów określone, ponieważ mają one wiele aliasów e-mail pracy, używanego do rejestrowania.

Zazwyczaj chcesz uniemożliwić nowym użytkownikom publikowanie żadnych danych do witryny sieci web, zanim zostały potwierdzone pocztą e-mail, wiadomość SMS lub innego mechanizmu. <a id="build"></a>W poniższych sekcjach możemy włączyć potwierdzenie adresu e-mail i zmodyfikować kod, aby uniemożliwić nowym użytkownikom logowanie do momentu swój adres e-mail został potwierdzony.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Podpinanie usługi SendGrid

Chociaż ten samouczek przedstawia tylko sposób dodawania powiadomienie e-mail za pośrednictwem [SendGrid](http://sendgrid.com/), możesz wysłać wiadomość e-mail przy użyciu SMTP i inne mechanizmy (zobacz [dodatkowe zasoby](#addRes)).

1. W konsoli Menedżera pakietów wpisz następujące polecenie: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Przejdź do [stronę rejestracji usługi SendGrid platformy Azure](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) i zarejestruj o utworzenie bezpłatnego konta SendGrid. Konfigurowanie usługi SendGrid, dodając kod podobny do następującego *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Należy dodać obejmuje następujące czynności:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

W celu uproszczenia w tym przykładzie będziemy przechowywać ustawienia aplikacji w *web.config* pliku:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Zabezpieczenia — nigdy nie przechowywania danych poufnych w kodzie źródłowym. Konto i poświadczenia są przechowywane w appSetting. Na platformie Azure, możesz bezpiecznie przechowywać te wartości na **[Konfiguruj](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** kartę w witrynie Azure portal. Zobacz [najlepsze rozwiązania dotyczące wdrażania haseł i innych danych poufnych na platformie ASP.NET i Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Włącz potwierdzenie adresu e-mail w kontrolerze konta

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Sprawdź *Views\Account\ConfirmEmail.cshtml* plik ma składni razor poprawne. (@ Znaków w pierwszym wierszu może brakować. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Uruchom aplikację, a następnie kliknij link Zarejestruj. Po przesłaniu formularza rejestracji użytkownik jest zalogowany.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Sprawdź swoje konto e-mail, a następnie kliknij link, aby potwierdzić swój adres e-mail.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Wymaga potwierdzenia e-mail przed logowania

Obecnie w przypadku, gdy użytkownik wykona formularz rejestracji, są one rejestrowane w. Zazwyczaj chcesz potwierdzić swój adres e-mail przed ich zalogowaniem się. W poniższej sekcji zmodyfikujemy kod, aby wymagać nowych użytkowników, aby miał potwierdzone pocztą e-mail, które są rejestrowane (uwierzytelnienie). Aktualizacja `HttpPost Register` metody z następującymi zmianami wyróżnione:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Zakomentowując `SignInAsync` metody, użytkownik zostanie nie zalogowany przy rejestracji. `TempData["ViewBagLink"] = callbackUrl;` Wiersza mogą być używane do [debugować aplikację](#dbg) i testować rejestracji bez wysyłania wiadomości e-mail. `ViewBag.Message` Służy do wyświetlania instrukcje Potwierdź. [Pobierz przykładowe](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) zawiera kod, aby przetestować e-mail z potwierdzeniem bez konfiguracji poczty e-mail i może również służyć do debugowania aplikacji.

Utwórz `Views\Shared\Info.cshtml` pliku i Dodaj następujący kod razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Dodaj [atrybut Autoryzuj](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) do `Contact` metody akcji kontrolera głównego. Możesz kliknąć **skontaktuj się z pomocą** łącze, aby sprawdzić, użytkowników anonimowych, nie mają dostępu i uwierzytelnieni użytkownicy mają dostęp.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Należy również zaktualizować `HttpPost Login` metody akcji:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Aktualizacja *Views\Shared\Error.cshtml* widok, aby wyświetlić komunikat o błędzie:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Usuwanie kont w **AspNetUsers** tabeli, która zawiera alias poczty e-mail, które chcesz przetestować za pomocą. Uruchom aplikację i upewnij się, że nie można zalogować się do czasu potwierdzenia adresu e-mail. Po upewnieniu się, Twój adres e-mail, kliknij przycisk **skontaktuj się z pomocą** łącza.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Resetowanie/odzyskiwanie hasła

Usuń znaki komentarza z `HttpPost ForgotPassword` metody akcji w kontrolerze konta:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Usuń znaki komentarza z `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) w *Views\Account\Login.cshtml* plik widoku razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Strony logowania będzie zawierać teraz link resetowania hasła.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Wyślij ponownie link potwierdzenie adresu e-mail

Gdy użytkownik tworzy nowe konto lokalne, są pocztą e-mail link do potwierdzenia, które są wymagane, aby korzystały mogą oni się zalogować. Jeśli użytkownik przypadkowo usuwa wiadomość e-mail z potwierdzeniem lub nigdy nie odebraniu wiadomości e-mail, należy ponownie wysyłane link do potwierdzenia. Następujących zmian w kodzie pokazują, jak włączyć tę opcję.

Dodaj następującą metodę pomocnika do dołu *Controllers\AccountController.cs* pliku:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Zaktualizuj metodę rejestru, użyj nowego pomocnika:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Zaktualizuj metodę logowania ponownie hasło, jeśli konto użytkownika nie zostało potwierdzone:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Łączenie kont społecznościowych i lokalne logowanie

Konta lokalne i społecznościowych można łączyć, klikając link wiadomości e-mail. W następującej kolejności **RickAndMSFT@gmail.com** najpierw jest tworzony podczas logowania lokalnego, ale możesz utworzyć konto jako dziennik społecznościowych najpierw, a następnie dodaj lokalny identyfikator logowania.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Kliknij pozycję **Zarządzaj** łącza. Uwaga **logowania zewnętrzne: 0** skojarzony z tym kontem.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Kliknij link do innego dziennika w usłudze i akceptowania żądań aplikacji. Te dwa konta zostały połączone, będzie można zalogować się przy użyciu dowolnego konta. Możesz zechcieć użytkownikom dodawanie kont lokalnych, w przypadku, gdy ich społecznościowych logowania usługi uwierzytelniania nie działa lub większe prawdopodobieństwo po utracie dostępu do kont społecznościowych.

Na poniższej ilustracji, Tom jest społecznościowych logowania (który można wyświetlić z **logowania zewnętrzne: 1** wyświetlany na stronie).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Kliknięcie **Wybierz hasło** pozwala na dodawanie w lokalnym dzienniku na skojarzone z tego samego konta.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Potwierdzenie adresu e-mail, które bardziej szczegółowo

Moje samouczek [potwierdzenie konta i odzyskiwanie hasła w produkcie ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) przechodzi w tym temacie z bardziej szczegółowymi informacjami.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debugowanie aplikacji

Jeśli nie otrzymasz wiadomość e-mail zawierającą łącze:

- Sprawdź folder wiadomości-śmieci lub spamu.
- Zaloguj się do Twojego konta SendGrid i kliknij pozycję [działania pocztą E-mail link](https://sendgrid.com/logs/index).

Aby przetestować link weryfikacyjny bez konta e-mail, Pobierz [ukończone przykładowe](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Na stronie pojawi się link do potwierdzenia i kodów potwierdzenia.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Łącza do produktu ASP.NET Identity zalecane zasoby](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Potwierdzenie konta i odzyskiwanie hasła w produkcie ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) zawiera bardziej szczegółowe potwierdzenie hasła odzyskiwania i konta.
- [MVC 5 aplikacji za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowanie jednokrotne](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) w tym samouczku dowiesz się, jak napisać aplikację ASP.NET MVC 5 z autoryzacją usługi Facebook i Google OAuth 2. Pokazano również, jak dodać dodatkowe dane do bazy danych tożsamości.
- [Wdrażanie bezpiecznej aplikacji ASP.NET MVC z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL na platformie Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). W tym samouczku dodaje wdrażania platformy Azure, jak zabezpieczyć swoją aplikację przy użyciu ról, jak dodać użytkowników i ról i funkcji zabezpieczeń za pomocą członkostwo interfejsu API.
- [Tworzenie aplikacji Google dla protokołu OAuth 2 i łączenie aplikacji do projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Tworzenie aplikacji w usłudze Facebook i łączenie aplikacji do projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Konfigurowanie protokołu SSL w projekcie](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
