---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: "Tworzenie bezpiecznej aplikacji sieci web platformy ASP.NET MVC 5 z dziennikiem, poczty e-mail resetowania hasła i potwierdzania (C#) | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: "Ten samouczek przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET MVC 5 z wiadomości e-mail z potwierdzeniem i resetowania przy użyciu systemu członkostwa ASP.NET Identity hasła. Możesz urzędu certyfikacji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: e3d8ad6e00b7fcb95f1c9bbe556021269c1a0624
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Tworzenie bezpiecznej aplikacji sieci web platformy ASP.NET MVC 5 z dziennikiem, poczty e-mail resetowania hasła i potwierdzania (C#)
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET MVC 5 z wiadomości e-mail z potwierdzeniem i resetowania przy użyciu systemu członkostwa ASP.NET Identity hasła. Możesz pobrać ukończona aplikacja [tutaj](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Pobieranie zawiera debugowania wątków, które umożliwiają testowanie potwierdzenie adresu e-mail i SMS bez konfiguracji poczty e-mail lub dostawcy programu SMS.
> 
> W tym samouczku, została napisana przy [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Tworzenie aplikacji platformy ASP.NET MVC

Rozpocznij od instalowania i uruchamiania [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej.

> [!NOTE]
> Ostrzeżenie: Musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej do ukończenia tego samouczka.


1. Utwórz nowy projekt sieci Web ASP.NET i wybierz szablon MVC. Formularze sieci Web obsługuje również tożsamości platformy ASP.NET, można wykonać podobne kroki w aplikacji formularzy sieci web.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Pozostaw domyślne uwierzytelnianie jako **indywidualnych kont użytkowników**. Jeśli chcesz udostępniać aplikacji na platformie Azure, pozostaw zaznaczone pole wyboru. Później w samouczku będziemy zostanie wdrożona na platformie Azure. Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).
3. Ustaw [projektu do używania protokołu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Uruchom aplikację, kliknij przycisk **zarejestrować** i łącza do zarejestrowania użytkownika. W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atrybutu.
5. W Eksploratorze serwera, przejdź do **Connections\DefaultConnection\Tables\AspNetUsers danych**, kliknij prawym przyciskiem myszy i wybierz **Otwórz definicję tabeli**.

    Poniższy obraz przedstawia `AspNetUsers` schematu:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Kliknij prawym przyciskiem myszy **AspNetUsers** tabeli i wybierz **Pokaż dane tabeli**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 W tym momencie wiadomości e-mail nie został potwierdzony.
7. Kliknij wiersz i wybierz polecenie Usuń. Będzie ponownie dodać ten adres e-mail w następnym kroku, a następnie wyślij wiadomość e-mail z potwierdzeniem.

## <a name="email-confirmation"></a>Wiadomości e-mail z potwierdzeniem

Jest najlepszym rozwiązaniem, aby potwierdzić nowej rejestracji użytkownika, aby sprawdzić ich są nie przeprowadza personifikacji ktoś inny adres e-mail (to znaczy one nie został zarejestrowany przy do kogoś innego adresu e-mail). Załóżmy, że masz forum dyskusyjne, czy chcesz zapobiec `"bob@example.com"` z rejestracją jako `"joe@contoso.com"`. Bez wiadomości e-mail z potwierdzeniem `"joe@contoso.com"` można pobrać niechcianych wiadomości e-mail z aplikacji. Załóżmy, że Bob przypadkowo zarejestrowany jako `"bib@example.com"` i nie zauważyć, ADAM nie będą mogli używać hasła odzyskiwania, ponieważ aplikacja nie ma jego prawidłowy adres e-mail. Wiadomości e-mail z potwierdzeniem chroni tylko ograniczone z robotów i nie zapewnia ochrony z określone nadawcy wiadomości-śmieci, ponieważ mają one wiele aliasów e-mail pracy służące do rejestrowania.

Ogólnie rzecz biorąc chcesz uniemożliwić przesyłanie danych do witryny sieci web, zanim zostały potwierdzone za pośrednictwem poczty e-mail, wiadomość SMS lub inny mechanizm nowych użytkowników. <a id="build"></a>W poniższych sekcjach możemy włączy wiadomości e-mail z potwierdzeniem i zmodyfikuj kod, aby uniemożliwić użytkownikom nowo zarejestrowanych logowanie do momentu swój adres e-mail został potwierdzony.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Podłączanie SendGrid

Mimo że w tym samouczku tylko przedstawiono sposób dodawania powiadomienia pocztą e-mail za pomocą [SendGrid](http://sendgrid.com/), możesz wysłać wiadomości e-mail przy użyciu SMTP i innych mechanizmów (zobacz [dodatkowe zasoby](#addRes)).

1. W konsoli Menedżera pakietów, wprowadź następujące polecenie: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Przejdź do [stronę Tworzenie konta Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) i zarejestrować bezpłatne konto SendGrid. Dodaj kod podobne do następujących czynności, aby skonfigurować SendGrid:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Musisz dodać zawiera następujące czynności:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Aby zachować ten przykład prostego, możemy przechowywania ustawień aplikacji w *web.config* pliku:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Zabezpieczenia — nigdy nie magazynu danych poufnych w kodzie źródłowym. Konto i poświadczenia są przechowywane w appSetting. Na platformie Azure, można bezpiecznie przechowywać te wartości na  **[Konfiguruj](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  kartę w portalu Azure. Zobacz [najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych do platformy ASP.NET i usługi Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Włącz wiadomości e-mail z potwierdzeniem w kontrolerze konta

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Sprawdź *Views\Account\ConfirmEmail.cshtml* plik ma składni razor poprawne. (@-Znak w pierwszym wierszu może brakować. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Uruchom aplikację i kliknąć łącze rejestru. Po przesłaniu formularza rejestracji użytkownik jest zalogowany.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Sprawdź swoje konto e-mail i kliknij link, aby potwierdzić swój adres e-mail.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Wymagaj wiadomości e-mail z potwierdzeniem przed logowania

Obecnie, gdy użytkownik kończy formularz rejestracji, są rejestrowane. Zazwyczaj mają Potwierdź swój adres e-mail przed ich zalogowaniem się. W poniższej sekcji firma Microsoft będzie zmodyfikuj kod do wymagają nowych użytkowników, aby miał potwierdzony adres e-mail, które są rejestrowane (uwierzytelniony). Aktualizacja `HttpPost Register` metody z następującymi zmianami wyróżnione:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Przez komentowania limit `SignInAsync` metody, użytkownik nie będzie można zalogował się przy rejestracji. `TempData["ViewBagLink"] = callbackUrl;` Wiersza mogą być używane do [debugowania aplikacji](#dbg) i testowanie rejestracji bez wysyłania wiadomości e-mail. `ViewBag.Message`Służy do wyświetlania instrukcje Potwierdź. [Pobieranie próbki](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) zawiera kod, aby przetestować wiadomości e-mail z potwierdzeniem bez konfiguracji poczty e-mail i może również służyć do debugowania aplikacji.

Utwórz `Views\Shared\Info.cshtml` i Dodaj następujący kod razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Dodaj [atrybutu autoryzacji](https://msdn.microsoft.com/en-us/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) do `Contact` metody akcji kontrolera głównej. Możesz użyć kliknij na **skontaktuj się z** łącze, aby zweryfikować użytkownicy anonimowi nie mają dostępu oraz uwierzytelnieni użytkownicy mają dostęp.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Należy również zaktualizować `HttpPost Login` metody akcji:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Aktualizacja *Views\Shared\Error.cshtml* widok, aby wyświetlić komunikat o błędzie:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Usuwanie wszystkich kont w **AspNetUsers** tabeli, która zawiera alias e-mail chcesz przeprowadzić test z. Uruchom aplikację i sprawdzić, czy nie można zalogować się do momentu potwierdzenia adresu e-mail. Po upewnieniu się, Twój adres e-mail, kliknij przycisk **skontaktuj się z** łącza.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Resetowanie odzyskiwania hasła

Usuń znaki komentarza z `HttpPost ForgotPassword` metody akcji w kontrolerze konta:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Usuń znaki komentarza z `ForgotPassword` [ActionLink](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) w *Views\Account\Login.cshtml* pliku widoku razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Strony logowania będzie zawierać teraz link do resetowania hasła.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Ponowne wysyłanie wiadomości e-mail potwierdzenie łącza

Gdy użytkownik tworzy konto lokalne, są pocztą e-mail łącze potwierdzenia, które są wymagane do używania przed ich zalogowaniem się. Jeśli użytkownik przypadkowo usuwa wiadomości e-mail z potwierdzeniem lub nigdy nie odebraniu wiadomości e-mail, muszą łącze potwierdzenie ponownego wysłania. Poniższy kod zmienia pokazują, jak włączyć tę opcję.

Dodaj następującą metodę pomocnika do dołu *Controllers\AccountController.cs* pliku:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Zaktualizuj metodę rejestru nowy Pomocnik:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Update — metoda logowania, aby ponownie wysłać hasło podczas Jeśli konto użytkownika nie został potwierdzony:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Łączenie kont społecznościowych i lokalne logowanie

Konta lokalne i społecznościowych można łączyć, klikając link do wiadomości e-mail. W następującej kolejności  **RickAndMSFT@gmail.com**  najpierw zostanie utworzona jako logowania lokalnego, ale można utworzyć konta społecznościowych logowania w pierwszym, a następnie dodaj lokalny identyfikator logowania.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Polecenie **Zarządzaj** łącza. Uwaga zewnętrznych 0 (społecznościowych logowania) skojarzone z tym kontem.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Kliknij łącze do innego dziennika w usłudze i akceptowania żądań aplikacji. Dwa konta zostały połączone, będzie można logować się na każdym koncie. Możesz użytkownikom dodawanie kont lokalnych, w przypadku ich społecznościowych dziennika w usłudze uwierzytelniania jest wyłączony lub najprawdopodobniej po utracie dostępu do swojego konta społecznościowych.

Na poniższej ilustracji, Tomasz jest społecznościowych logowania (które można wyświetlić z **logowań zewnętrznych: 1** wyświetlany na stronie).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Kliknięcie **Wybierz hasło** umożliwia dodanie w lokalnym dzienniku na skojarzone z kontem.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Wiadomości e-mail z potwierdzeniem bardziej szczegółowo

Moje samouczek [potwierdzania konta i hasło odzyskiwania za pomocą ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) przechodzi w tym temacie bardziej szczegółowo.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debugowanie aplikacji

Jeśli nie otrzymasz wiadomość e-mail zawierającą łącze:

- Sprawdź folder wiadomości-śmieci lub spamu.
- Zaloguj się na koncie SendGrid i kliknij na [działania pocztą E-mail łączy](https://sendgrid.com/logs/index).

Aby przetestować łącze weryfikacji bez poczty e-mail, Pobierz [ukończone próbki](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Na stronie pojawi się potwierdzenie link i kody potwierdzenia.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Łącza do tożsamości ASP.NET zalecane zasobów](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Konta potwierdzenie i hasło odzyskiwania za pomocą ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) przechodzi w bardziej szczegółowo na potwierdzenie hasła odzyskiwania i konta.
- [Aplikacji MVC 5 za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) w tym samouczku przedstawiono sposób pisania aplikacji platformy ASP.NET MVC 5 z usługi Facebook i Google OAuth 2 autoryzacji. Ponadto jak dodać dodatkowe dane do bazy danych tożsamości.
- [Wdrażanie aplikacji bezpiecznego platformy ASP.NET MVC z członkostwa, OAuth i bazy danych SQL Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). W tym samouczku dodaje wdrożenia usługi Azure zabezpieczania aplikacji za pomocą ról, jak używać interfejs API członkostwa można dodać użytkowników i role oraz dodatkowe funkcje zabezpieczeń.
- [Tworzenie aplikacji Google OAuth 2 i łączenie aplikacji z projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Tworzenie aplikacji w serwisie Facebook i łączenie aplikacji z projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Konfigurowanie protokołu SSL w projekcie](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
