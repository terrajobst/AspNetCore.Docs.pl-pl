---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Tworzenie bezpiecznej aplikacji ASP.NET Web Forms z rejestracją użytkownika wiadomości e-mail z potwierdzeniem i resetowaniem hasła (C#) | Dokumentacja firmy Microsoft
author: Erikre
description: W tym samouczku przedstawiono sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą rejestracji użytkowników, potwierdzenie adresu e-mail i resetowania haseł za pomocą elementu członkowskiego produktu ASP.NET Identity...
ms.author: aspnetcontent
ms.date: 10/02/2014
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 1c230c3f33bd8261312485e9d77f6f88adb49e9e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812772"
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Tworzenie bezpiecznej aplikacji ASP.NET Web Forms z rejestracją użytkownika wiadomości e-mail z potwierdzeniem i resetowaniem hasła (C#)
====================
przez [Erik Reitan](https://github.com/Erikre)

> W tym samouczku przedstawiono sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą rejestracji użytkowników, potwierdzenie adresu e-mail i resetowania haseł za pomocą systemu członkostwa ASP.NET Identity. W tym samouczku został oparty na Rick Anderson [samouczek MVC](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).


## <a name="introduction"></a>Wprowadzenie

Ten samouczek przeprowadzi Cię przez kroki wymagane do tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Visual Studio i platformy ASP.NET 4.5 tworzenie bezpiecznej aplikacji formularzy sieci Web z rejestracją użytkownika, wiadomości e-mail z potwierdzeniem i resetowaniem hasła.

### <a name="tutorial-tasks-and-information"></a>Samouczek zadania i informacji:

- [Tworzenie internetowego interfejsu platformy ASP.NET aplikacja formularzy](#createWebForms)
- [Podpinanie usługi SendGrid](#SG)
- [Wymaga potwierdzenia E-mail przed logowania](#require)
- [Hasło odzyskiwania i Reset](#reset)
- [Wyślij ponownie Link potwierdzenie adresu E-mail](#rsend)
- [Rozwiązywanie problemów z aplikacji](#dbg)
- [Dodatkowe zasoby](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Tworzenie aplikacji formularzy sieci Web platformy ASP.NET

Rozpocznij od instalowania i uruchamiania [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub wyższej oraz.

> [!NOTE]
> Ostrzeżenie: Musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej, aby ukończyć ten samouczek.


1. Utwórz nowy projekt (**pliku**  - &gt; **nowy projekt**) i wybierz **aplikacji sieci Web ASP.NET** szablonu i najnowszej wersji .NET Framework wersja z **nowy projekt** okno dialogowe.
2. Z **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **formularzy sieci Web** szablonu. Pozostaw domyślne uwierzytelnianie jako **indywidualne konta użytkowników**. Jeśli chcesz hostować aplikację na platformie Azure, należy pozostawić **Hostuj w chmurze** pole wyboru zaznaczone.   
 Następnie kliknij przycisk **OK** Aby utworzyć nowy projekt.  
    ![Okno dialogowe Nowy projekt ASP.NET](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Włączanie protokołu Secure Sockets Layer (SSL) dla projektu. Wykonaj kroki, które są dostępne w **Włącz SSL dla projektu** części [wprowadzenie do formularzy sieci Web serii samouczków](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Uruchom aplikację, kliknij przycisk **zarejestrować** link i rejestrowanie nowego użytkownika. W tym momencie tylko sprawdzanie poprawności w wiadomości e-mail opiera się na [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atrybutu, aby upewnić się, adres e-mail jest poprawnie sformułowany. Zmodyfikujesz kod, aby dodać potwierdzenie adresu e-mail. Zamknij okno przeglądarki.
5. W **Eksploratora serwera** programu Visual Studio (**widoku**  - &gt; **Eksploratora serwera**), przejdź do **Connections\ danych DefaultConnection\Tables\AspNetUsers**, kliknij prawym przyciskiem myszy i wybierz **Otwórz definicję tabeli**. 

    Na poniższej ilustracji przedstawiono `AspNetUsers` schematu tabeli:

    ![Schemat tabeli AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy **AspNetUsers** tabeli, a następnie wybierz pozycję **Pokaż dane tabeli**.  
  
    ![Dane tabeli AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 W tym momencie nie został potwierdzony adres e-mail zarejestrowany użytkownik.
7. Kliknij wiersz i wybierz pozycję Usuń — Usuń użytkownika. Dodasz tę wiadomość e-mail ponownie w następnym kroku i Wyślij komunikat potwierdzenia adresu e-mail.

## <a name="email-confirmation"></a>Potwierdzenie adresu e-mail

Jest najlepszym rozwiązaniem potwierdzenia adresu e-mail podczas rejestracji nowego użytkownika, aby sprawdzić ich nie podszyć się pod kogoś innego (oznacza to one nie zostały zarejestrowane przy użyciu adresu e-mail osoby). Załóżmy, że masz forum dyskusyjne, czy chcesz uniemożliwić `"bob@cpandl.com"` z rejestracją jako `"joe@contoso.com"`. Bez potwierdzenia e-mail `"joe@contoso.com"` można pobrać niechcianych wiadomości e-mail z aplikacji. Załóżmy, że Bob przypadkowo zarejestrowany jako `"bib@cpandl.com"` i był wystąpieniem, użytkownik nie będzie mogła używać hasła odzyskiwania, ponieważ aplikacja nie ma jego prawidłowy adres e-mail. Potwierdzenie adresu e-mail zawiera tylko ograniczoną ochronę z botami i nie zapewnia ochrony z spamerów określone.

Zazwyczaj chcesz uniemożliwić nowym użytkownikom publikowanie żadnych danych do swojej witryny sieci Web, zanim zostały potwierdzone pocztą e-mail, wiadomość SMS lub innego mechanizmu. W poniższych sekcjach możemy włączyć potwierdzenie adresu e-mail i zmodyfikować kod, aby uniemożliwić nowym użytkownikom logowanie do momentu swój adres e-mail został potwierdzony. W tym samouczku użyjemy usługi e-mail SendGrid.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Podpinanie usługi SendGrid

Chociaż ten samouczek przedstawia tylko sposób dodawania powiadomienie e-mail za pośrednictwem [SendGrid](http://sendgrid.com/), możesz wysłać wiadomość e-mail przy użyciu SMTP i inne mechanizmy (zobacz [dodatkowe zasoby](#addRes)).

1. W programie Visual Studio, otwórz **Konsola Menedżera pakietów** (**narzędzia**  - &gt; **Menedżera pakietów NuGet**  - &gt; **Konsola Menedżera pakietów**), a następnie wprowadź następujące polecenie:  
    `Install-Package SendGrid`
2. Przejdź do [stronie tworzenia konta usługi SendGrid platformy Azure](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) i Zarejestruj się bezpłatnie konta SendGrid. Możesz również Załóż bezpłatne konto usługi SendGrid bezpośrednio na [witryny SendGrid](http://www.sendgrid.com).
3. Z **Eksploratora rozwiązań** Otwórz *IdentityConfig.cs* w pliku *aplikacji\_Start* folderze i Dodaj następujący kod wyróżniony na żółto do `EmailService` klasa umożliwiająca skonfigurowanie **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Ponadto, Dodaj następujący kod `using` instrukcje do stanu sprzed *IdentityConfig.cs* pliku: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. W celu uproszczenia w tym przykładzie będzie przechowywać wartości konta usługi poczty e-mail w `appSettings` części *web.config* pliku. Dodaj następujący kod XML wyróżniony na żółto do *web.config* pliku w folderze głównym projektu:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie przechowywania danych poufnych w kodzie źródłowym. W tym przykładzie konto i poświadczenia są przechowywane w **appSetting** części *Web.config* pliku. Na platformie Azure, możesz bezpiecznie przechowywać te wartości na **[Konfiguruj](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** kartę w witrynie Azure portal. Aby uzyskać powiązane informacje, zobacz Rick Anderson temacie [najlepsze rozwiązania dotyczące wdrażania haseł i innych danych poufnych na platformie ASP.NET i Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Dodaj wartości usługi poczty e-mail, aby odzwierciedlać wartości uwierzytelniania usługi SendGrid (nazwa użytkownika i hasło) aby można było pomyślnie wysłać wiadomość e-mail z aplikacji. Pamiętaj użyć swojej nazwy konta usługi SendGrid, a nie adres e-mail podany usługi SendGrid.

### <a name="enable-email-confirmation"></a>Włącz potwierdzenie adresu E-mail

 Aby włączyć e-mail z potwierdzeniem, zmodyfikujesz kod rejestracyjny, wykonując następujące kroki.  
 

1. W *konta* folder, otwórz *Register.aspx.cs* związanym z kodem i zaktualizuj `CreateUser_Click` metodę umożliwiającą włączenie wyróżnione następujące zmiany: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Default.aspx* i wybierz **Ustaw jako stronę startową**.
3. Uruchom aplikację, naciskając klawisz **F5.** Po wyświetleniu strony kliknij **zarejestrować** łącze, aby wyświetlić stronę rejestru.
4. Wprowadź swój adres e-mail i hasło, a następnie kliknij przycisk **zarejestrować** przycisk, aby wysłać wiadomość e-mail za pośrednictwem usługi SendGrid.  
   Bieżący stan projektu i kodu pozwoli użytkownikowi na logowanie po ukończeniu formularz rejestracji wskazany nawet, jeśli one nie zostały potwierdzone swojego konta.
5. Sprawdź swoje konto e-mail, a następnie kliknij link, aby potwierdzić swój adres e-mail.  
   Po przesłaniu formularza rejestracji będą rejestrowane w.  
    ![Przykładowej witryny sieci Web — zalogowany](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Wymaga potwierdzenia E-mail przed logowania

Mimo że potwierdzenia konta e-mail, w tym momencie nie należałoby kliknąć link zawarty w wiadomości e-mail weryfikującej, aby w pełni zalogować się. W poniższej sekcji zmodyfikujesz kod, wymagające nowych użytkowników, aby miał potwierdzone pocztą e-mail, które są rejestrowane (uwierzytelnienie).

1. W **Eksploratora rozwiązań** programu Visual Studio, należy zaktualizować `CreateUser_Click` zdarzenia w *Register.aspx.cs* związanym z kodem zawarte w *kont* folder z następujące zmiany wyróżnione: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Aktualizacja `LogIn` method in Class metoda *Login.aspx.cs* związanym z kodem z następującymi zmianami wyróżnione: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Uruchamianie aplikacji

 Teraz, że udało Ci się wdrożyć kod, aby sprawdzić, czy adres e-mail użytkownika został potwierdzony, można sprawdzić funkcjonalność zarówno **zarejestrować** i **logowania** stron. 

1. Usuwanie kont w **AspNetUsers** tabeli, która zawiera alias poczty e-mail, które chcesz przetestować.
2. Uruchom aplikację (**F5**) i sprawdź, nie można zarejestrować jako użytkownik, do momentu potwierdzenia adresu e-mail.
3. Przed potwierdzeniem nowego konta za pośrednictwem poczty e-mail, która właśnie została wysłana, spróbuj zalogować się przy użyciu nowego konta.  
 Nie można się zalogować i posiadanie konta e-mail potwierdzonych będą widoczne.
4. Po upewnieniu się, Twój adres e-mail, zaloguj się do aplikacji.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Hasło odzyskiwania i Reset

1. W programie Visual Studio, należy usunąć znaki komentarza z `Forgot` method in Class metoda *Forgot.aspx.cs* związanym z kodem zawarte w *konta* folderu, tak aby metoda pojawia się jako następuje: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Otwórz *Login.aspx* strony. Zastąp kod znaczników osiągnie koniec cyklu **loginForm** sekcji jak wyróżniono poniżej: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Otwórz *Login.aspx.cs* związanym z kodem i usuń znaczniki komentarza następujący wiersz kodu wyróżniony na żółto z `Page_Load` program obsługi zdarzeń: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Uruchom aplikację, naciskając klawisz **F5.** Po wyświetleniu strony kliknij **Zaloguj** łącza.
5. Kliknij przycisk **nie pamiętasz hasła?** łącze, aby wyświetlić **nie pamiętam hasła** strony.
6. Wprowadź swój adres e-mail, a następnie kliknij przycisk **przesyłania** przycisk, aby wysłać wiadomość e-mail na adres, który umożliwi Ci zresetować hasło.   
   Sprawdź swoje konto poczty e-mail i kliknij łącze, aby wyświetlić **resetowania hasła** strony.
7. Na **resetowania hasła** wpisz swoje wiadomości e-mail, hasło i potwierdzenie hasła. Naciśnij klawisz **resetowania** przycisku.  
   Po zresetowaniu hasła, **zmiany hasła** zostanie wyświetlona strona. Teraz możesz zalogować się przy użyciu nowego hasła.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Wyślij ponownie Link potwierdzenie adresu E-mail

Gdy użytkownik tworzy nowe konto lokalne, są pocztą e-mail link do potwierdzenia, które są wymagane, aby korzystały mogą oni się zalogować. Jeśli użytkownik przypadkowo usuwa wiadomość e-mail z potwierdzeniem lub nigdy nie odebraniu wiadomości e-mail, należy ponownie wysyłane link do potwierdzenia. Następujących zmian w kodzie pokazują, jak włączyć tę opcję.

1. W programie Visual Studio, otwórz **Login.aspx.cs** związanym z kodem i dodaj następującą obsługę zdarzeń po `LogIn` program obsługi zdarzeń:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Modyfikowanie `LogIn` programu obsługi zdarzeń w *Login.aspx.cs* związanym z kodem, zmieniając kod wyróżniony na żółto w następujący sposób: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Aktualizacja *Login.aspx* strony, dodając kod wyróżniony na żółto w następujący sposób: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Usuwanie kont w **AspNetUsers** tabeli, która zawiera alias poczty e-mail, które chcesz przetestować.
5. Uruchom aplikację (**F5**) i zarejestruj swój adres e-mail.
6. Przed potwierdzeniem nowego konta za pośrednictwem poczty e-mail, która właśnie została wysłana, spróbuj zalogować się przy użyciu nowego konta.  
   Nie można się zalogować i posiadanie konta e-mail potwierdzonych będą widoczne. Ponadto możesz teraz ponownie wysłać komunikat z potwierdzeniem z tym kontem e-mail.
7. Wprowadź swój adres e-mail i hasła, naciśnij klawisz **ponowne wysyłanie potwierdzenia** przycisku.
8. Po upewnieniu się, Twój adres e-mail, na podstawie wiadomości e-mail nowo przesłanych, zaloguj się do aplikacji.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Rozwiązywanie problemów z aplikacji

Jeśli nie otrzymasz wiadomość e-mail zawierającą łącze, aby zweryfikować swoje poświadczenia:

- Sprawdź folder wiadomości-śmieci lub spamu.
- Zaloguj się do Twojego konta SendGrid i kliknij pozycję [działania pocztą E-mail link](https://sendgrid.com/logs/index).
- Mieć pewność, używana jest nazwa konta użytkownika usługi SendGrid jako *Web.config* wartości, a nie adres e-mail konta SendGrid.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Łącza do produktu ASP.NET Identity zalecane zasoby](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Potwierdzenie konta i odzyskiwanie hasła w produkcie ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Seria samouczków formularzy sieci Web platformy ASP.NET — Dodaj dostawcę uwierzytelniania OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Wdrażanie aplikacji formularzy bezpiecznej sieci Web platformy ASP.NET z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web Forms serii samouczków - Włącz SSL dla projektu](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
