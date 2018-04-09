---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Tworzenie bezpiecznej aplikacji formularzy sieci Web ASP.NET rejestracja użytkownika w wiadomości e-mail resetowania hasła i potwierdzania (C#) | Dokumentacja firmy Microsoft
author: Erikre
description: W tym samouczku przedstawiono sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą rejestracji użytkownika, potwierdzenie adresu e-mail i hasło resetowania przy użyciu składnika ASP.NET Identity...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 1dc7ace69473b45432fd942b9cf1ba32332cb707
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Tworzenie bezpiecznej aplikacji formularzy sieci Web ASP.NET rejestracja użytkownika w wiadomości e-mail resetowania hasła i potwierdzania (C#)
====================
Przez [Erik Reitan](https://github.com/Erikre)

> Ten samouczek przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą rejestracji użytkownika, potwierdzenie adresu e-mail i hasło resetowania przy użyciu systemu członkostwa ASP.NET Identity. W tym samouczku została oparta na Rick Anderson [samouczek dla platformy MVC](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).


## <a name="introduction"></a>Wprowadzenie

W tym samouczku przedstawiono kroki wymagane do tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Visual Studio i ASP.NET 4.5, aby utworzyć bezpiecznego aplikacji formularzy sieci Web rejestracji użytkownika, poczty e-mail resetowania hasła i potwierdzania.

### <a name="tutorial-tasks-and-information"></a>Samouczek zadań i informacji:

- [Tworzenie sieci Web ASP.NET formularzy aplikacji](#createWebForms)
- [Podłączanie SendGrid](#SG)
- [Wymagaj wiadomości E-mail z potwierdzeniem przed logowania](#require)
- [Hasło odzyskiwania i resetowania](#reset)
- [Ponowne wysyłanie wiadomości E-mail potwierdzenie łącza](#rsend)
- [Rozwiązywanie problemów z aplikacji](#dbg)
- [Dodatkowe zasoby](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Tworzenie aplikacji formularzy sieci Web ASP.NET

Rozpocznij od instalowania i uruchamiania [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub wyższej oraz.

> [!NOTE]
> Ostrzeżenie: Musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej do ukończenia tego samouczka.


1. Utwórz nowy projekt (**pliku**  - &gt; **nowy projekt**) i wybierz **aplikacji sieci Web ASP.NET** szablonu i najnowszych .NET Framework wersja z **nowy projekt** okno dialogowe.
2. Z **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **formularzy sieci Web** szablonu. Pozostaw domyślne uwierzytelnianie jako **indywidualnych kont użytkowników**. Jeśli chcesz udostępniać aplikacji na platformie Azure, pozostaw **Hostuj w chmurze** zaznaczonym polem wyboru.   
 Następnie kliknij przycisk **OK** do utworzenia nowego projektu.  
    ![Okno dialogowe Nowy projekt ASP.NET](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Włącz Secure Sockets Layer (SSL) dla projektu. Wykonaj kroki, które są dostępne w **Włącz SSL dla projektu** sekcji [wprowadzenie do korzystania z samouczka serii formularzy sieci Web](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Uruchom aplikację, kliknij przycisk **zarejestrować** link i zarejestrować nowego użytkownika. W tym momencie tylko sprawdzanie poprawności w wiadomości e-mail jest oparty na [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atrybutu, aby upewnić się, adres e-mail jest poprawnie sformułowany. Należy zmodyfikować kod, aby dodać wiadomości e-mail z potwierdzeniem. Zamknij okno przeglądarki.
5. W **Eksploratora serwera** programu Visual Studio (**widoku**  - &gt; **Eksploratora serwera**), przejdź do **Connections\ danych DefaultConnection\Tables\AspNetUsers**, kliknij prawym przyciskiem myszy i wybierz **Otwórz definicję tabeli**. 

    Poniższy obraz przedstawia `AspNetUsers` schemat tabeli:

    ![Schemat tabeli AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy **AspNetUsers** tabeli i wybierz **Pokaż dane tabeli**.  
  
    ![Dane tabeli AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 W tym momencie poczty e-mail dla zarejestrowanego użytkownika nie został potwierdzony.
7. Kliknij na wiersz i wybierz Usuń, aby usunąć użytkownika. Będzie ponownie dodać ten adres e-mail w następnym kroku i Wyślij komunikat potwierdzenia do adresu e-mail.

## <a name="email-confirmation"></a>Wiadomości e-mail z potwierdzeniem

Jest najlepszym rozwiązaniem w celu potwierdzenia adresu e-mail podczas rejestracji nowego użytkownika, aby sprawdzić ich są nie przeprowadza personifikacji ktoś inny (to znaczy one nie został zarejestrowany przy do kogoś innego adresu e-mail). Załóżmy, że masz forum dyskusyjne, czy chcesz zapobiec `"bob@cpandl.com"` z rejestracją jako `"joe@contoso.com"`. Bez wiadomości e-mail z potwierdzeniem `"joe@contoso.com"` można pobrać niechcianych wiadomości e-mail z aplikacji. Załóżmy, że Bob przypadkowo zarejestrowany jako `"bib@cpandl.com"` i nie zauważyć, ADAM nie będą mogli używać hasła odzyskiwania, ponieważ aplikacja nie ma jego prawidłowy adres e-mail. Wiadomości e-mail z potwierdzeniem chroni tylko ograniczone z robotów i nie zapewnia ochrony z określone nadawcy wiadomości-śmieci.

Ogólnie rzecz biorąc chcesz uniemożliwić przesyłanie danych do witryny sieci Web, zanim zostały potwierdzone pocztą e-mail, wiadomości SMS lub inny mechanizm nowych użytkowników. W poniższych sekcjach możemy włączy wiadomości e-mail z potwierdzeniem i zmodyfikuj kod, aby uniemożliwić użytkownikom nowo zarejestrowanych logowanie do momentu swój adres e-mail został potwierdzony. W tym samouczku użyjesz SendGrid usługa poczty e-mail.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Podłączanie SendGrid

Mimo że w tym samouczku tylko przedstawiono sposób dodawania powiadomienia pocztą e-mail za pomocą [SendGrid](http://sendgrid.com/), możesz wysłać wiadomości e-mail przy użyciu SMTP i innych mechanizmów (zobacz [dodatkowe zasoby](#addRes)).

1. W programie Visual Studio Otwórz **Konsola Menedżera pakietów** (**narzędzia**  - &gt; **Menedżer pakietów NuGet**  - &gt; **Konsola Menedżera pakietów**), a następnie wprowadź następujące polecenie:  
    `Install-Package SendGrid`
2. Przejdź do [stronę tworzenia konta Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) i zarejestrować bezpłatne konto SendGrid. Możesz również Załóż bezpłatne konto SendGrid bezpośrednio na [SendGrid w witrynie](http://www.sendgrid.com).
3. Z **Eksploratora rozwiązań** Otwórz *IdentityConfig.cs* w pliku *aplikacji\_Start* folderu i Dodaj następujący kod wyróżnione kolorem żółtym do `EmailService` klasa do konfigurowania **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Ponadto Dodaj następujące `using` instrukcje na początku *IdentityConfig.cs* pliku: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Aby zachować ten przykład prostego, będą przechowywane wartości konta usługi poczty e-mail w `appSettings` sekcji *web.config* pliku. Dodaj następujący kod XML wyróżnione kolorem żółtym do *web.config* pliku w katalogu głównym projektu:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie magazynu danych poufnych w kodzie źródłowym. W tym przykładzie konta i poświadczenia są przechowywane w **appSetting** sekcji *Web.config* pliku. Na platformie Azure, można bezpiecznie przechowywać te wartości na **[Konfiguruj](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** kartę w portalu Azure. Aby uzyskać odpowiednie informacje zawiera temat Rick Anderson zatytułowany [najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych do platformy ASP.NET i usługi Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Dodaj wartości usługi poczty e-mail aby odzwierciedlał wartości uwierzytelniania SendGrid (nazwa użytkownika i hasło), co pozwala powiodło się wysyłanie wiadomości e-mail z aplikacji. Należy użyć SendGrid nazwa konta, a nie adres e-mail podany SendGrid.

### <a name="enable-email-confirmation"></a>Włącz wiadomości E-mail z potwierdzeniem

 Aby włączyć wiadomości e-mail z potwierdzeniem, będzie zmodyfikować kod rejestracji, wykonując następujące kroki.  
 

1. W *konta* folder, otwórz *Register.aspx.cs* CodeBehind i zaktualizuj `CreateUser_Click` metodę umożliwiającą włączenie wyróżnione następujące zmiany: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Default.aspx* i wybierz **Ustaw jako stronę startową**.
3. Uruchom aplikację, naciskając klawisz **F5.** Po wyświetleniu strony kliknij **zarejestrować** link do wyświetlenia strony rejestrowania.
4. Wprowadź adres e-mail i hasło, a następnie kliknij przycisk **zarejestrować** przycisk, aby wysłać wiadomość e-mail za pośrednictwem SendGrid.  
   Bieżący stan projektu i Kod umożliwi użytkownikowi na logowanie po ukończeniu formularz rejestracji, nawet jeśli jeszcze tego nie potwierdził swojego konta.
5. Sprawdź swoje konto e-mail i kliknij link, aby potwierdzić swój adres e-mail.  
   Po przesłaniu formularza rejestracji będziesz się logować.  
    ![Przykładowe witryny sieci Web — zalogowany](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Wymagaj wiadomości E-mail z potwierdzeniem przed logowania

Mimo że można potwierdzić konto e-mail, w tym momencie nie musisz kliknij link zawarty w wiadomości e-mail weryfikacji, aby być w pełni zalogowany. W poniższej sekcji należy zmodyfikować kod wymagające nowych użytkowników, aby miał potwierdzony adres e-mail, które są rejestrowane (uwierzytelniony).

1. W **Eksploratora rozwiązań** programu Visual Studio, należy zaktualizować `CreateUser_Click` zdarzenia w *Register.aspx.cs* kodem zawarte w *kont* folder o następujące zmiany wyróżnione: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Aktualizacja `LogIn` metody w *Login.aspx.cs* kodem wyróżnione następujące zmiany: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Uruchamianie aplikacji

 Teraz, gdy zostały zaimplementowane kod, aby sprawdzić, czy adres e-mail użytkownika został potwierdzony, można sprawdzić funkcjonalność zarówno **zarejestrować** i **logowania** stron. 

1. Usuwanie wszystkich kont w **AspNetUsers** tabeli, która zawiera alias e-mail chcesz przeprowadzić test.
2. Uruchom aplikację (**F5**) i upewnij się, nie można zarejestrować jako użytkownik, do momentu potwierdzenia adresu e-mail.
3. Przed potwierdzeniem nowego konta za pośrednictwem poczty e-mail, który właśnie został wysłany, próba Zaloguj się przy użyciu nowego konta.  
 Nie można się zalogować i że musisz mieć konto e-mail potwierdzone będą widoczne.
4. Po upewnieniu się, Twój adres e-mail, zaloguj się do aplikacji.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Hasło odzyskiwania i resetowania

1. W programie Visual Studio, usuń znaki komentarza z `Forgot` metody w *Forgot.aspx.cs* kodem zawarte w *konta* folderu, dzięki czemu metoda pojawia się jako następuje: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Otwórz *Login.aspx* strony. Zastąp kod znaczników pod koniec **loginForm** sekcji jak wyróżniono poniżej: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Otwórz *Login.aspx.cs* CodeBehind i Usuń komentarz następujący wiersz kodu wyróżnione kolorem żółtym z `Page_Load` obsługi zdarzeń: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Uruchom aplikację, naciskając klawisz **F5.** Po wyświetleniu strony kliknij **Zaloguj** łącza.
5. Kliknij przycisk **nie pamiętasz hasła?** łącze, aby wyświetlić **nie pamiętasz hasła?** strony.
6. Wprowadź adres e-mail, a następnie kliknij przycisk **przesyłania** przycisk, aby wysłać wiadomość e-mail na adres, co umożliwi zresetowanie hasła.   
   Sprawdź konto poczty e-mail, a następnie kliknij łącze, aby wyświetlić **Resetuj hasło** strony.
7. Na **Resetuj hasło** Wprowadź użytkownika wiadomości e-mail, hasło i potwierdzenie hasła. Naciśnij klawisz **zresetować** przycisku.  
   Po zresetowaniu hasła, **zmiany hasła** zostanie wyświetlona strona. Teraz możesz zalogować się przy użyciu nowego hasła.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Ponowne wysyłanie wiadomości E-mail potwierdzenie łącza

Gdy użytkownik tworzy konto lokalne, są pocztą e-mail łącze potwierdzenia, które są wymagane do używania przed ich zalogowaniem się. Jeśli użytkownik przypadkowo usuwa wiadomości e-mail z potwierdzeniem lub nigdy nie odebraniu wiadomości e-mail, muszą łącze potwierdzenie ponownego wysłania. Poniższy kod zmienia pokazują, jak włączyć tę opcję.

1. W programie Visual Studio Otwórz **Login.aspx.cs** CodeBehind i dodaj następujące programu obsługi zdarzeń po `LogIn` obsługi zdarzeń:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Modyfikowanie `LogIn` obsługi zdarzeń w *Login.aspx.cs* kodem zmieniając kod wyróżnione kolorem żółtym w następujący sposób: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Aktualizacja *Login.aspx* strony przez dodanie kodu wyróżnione kolorem żółtym w następujący sposób: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Usuwanie wszystkich kont w **AspNetUsers** tabeli, która zawiera alias e-mail chcesz przeprowadzić test.
5. Uruchom aplikację (**F5**) i zarejestruj swój adres e-mail.
6. Przed potwierdzeniem nowego konta za pośrednictwem poczty e-mail, który właśnie został wysłany, próba Zaloguj się przy użyciu nowego konta.  
   Nie można się zalogować i że musisz mieć konto e-mail potwierdzone będą widoczne. Ponadto możesz teraz ponownie wysłać komunikat potwierdzenia do Twojego konta e-mail.
7. Wprowadź adres e-mail i hasło, naciśnij klawisz **ponowne wysłanie potwierdzenia** przycisku.
8. Po upewnieniu się, Twój adres e-mail, na podstawie komunikatu nowo wiadomości e-mail, zaloguj się do aplikacji.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Rozwiązywanie problemów z aplikacji

Jeśli nie otrzymasz wiadomość e-mail zawierającą łącze do zweryfikowania poświadczeń:

- Sprawdź folder wiadomości-śmieci lub spamu.
- Zaloguj się na koncie SendGrid i kliknij na [działania pocztą E-mail łączy](https://sendgrid.com/logs/index).
- Można niektórych używana jako nazwa konta użytkownika SendGrid *Web.config* wartość zamiast adres e-mail konta SendGrid.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Łącza do tożsamości ASP.NET zalecane zasobów](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Potwierdzenie konta i hasło odzyskiwania za pomocą tożsamości platformy ASP.NET](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Samouczek serii formularzy sieci Web platformy ASP.NET — Dodaj dostawcę uwierzytelniania OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Wdrażanie aplikacji formularzy bezpiecznej sieci Web platformy ASP.NET z członkostwa, OAuth i bazy danych SQL Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Serii samouczek formularzy sieci Web ASP.NET - Włącz SSL dla projektu](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
