---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: "Konta potwierdzenie i hasło odzyskiwania za pomocą tożsamości platformy ASP.NET (C#) | Dokumentacja firmy Microsoft"
author: HaoK
description: "Przed wykonaniem tego samouczka, które należy wykonać utworzyć bezpiecznego aplikacji sieci web platformy ASP.NET MVC 5 z dziennika w resetowania hasła i potwierdzania poczty e-mail. W tym samouczku..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 548baaaa06980fb793c079b66b6edc34422eb579
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Potwierdzenie konta i hasło odzyskiwania za pomocą tożsamości platformy ASP.NET (C#)
====================
przez [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Przed wykonaniem tego samouczka należy najpierw wykonać [utworzenia bezpiecznego aplikacji sieci web platformy ASP.NET MVC 5 z dziennika w resetowania hasła i potwierdzania poczty e-mail](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Ten samouczek zawiera więcej szczegółowych informacji i opisano, jak skonfigurować konta e-mail o potwierdzenie konta lokalnego i umożliwiają użytkownikom zresetować zapomniane hasło w produkcie ASP.NET Identity. Ten artykuł dotyczy autorstwa Ricka Andersona ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung i Suhas Joshi. Przykładowe NuGet zapisał głównie Hao Kung.


Konto użytkownika lokalnego wymaga od użytkownika utworzyć hasło dla konta i hasło jest przechowywane (bezpieczny) w aplikacji sieci web. Tożsamość platformy ASP.NET obsługuje również społecznościowych konta, które nie wymagają od użytkownika utworzyć hasło aplikacji. [Kont społecznościowych](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) uwierzytelniania użytkowników za pomocą innych firm (takich jak Google, Twitter, Facebook i firmy Microsoft). W tym temacie omówiono następujące czynności:

- [Tworzenie aplikacji platformy ASP.NET MVC](#createMvc) i Poznaj funkcje tożsamości ASP.NET.
- [Tworzenie przykładowej tożsamości](#build)
- [Konfigurowanie wiadomości e-mail z potwierdzeniem](#email)

Nowi użytkownicy zarejestrować ich alias e-mail, który tworzy konto lokalne.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Kliknięcie przycisku Zarejestruj wysyła wiadomość e-mail z potwierdzeniem zawierające token weryfikacji adresu e-mail.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

Użytkownik jest wysyłana wiadomość e-mail z token potwierdzenia dla swojego konta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Kliknięcie łącza stanowi potwierdzenie konta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Resetowanie odzyskiwania hasła

Lokalnych użytkowników, którzy zapomni swoje hasło może mieć tokenu zabezpieczającego wysyłane do swojego konta poczty e-mail, włączanie ich do zresetowania swojego hasła.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Użytkownik wkrótce otrzyma wiadomość e-mail z łączem, dzięki czemu można zresetować swoje hasło.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Kliknięcie łącza powoduje wyświetlenie strony resetowania.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Kliknięcie przycisku **zresetować** przycisk będzie Potwierdź hasło zostało zresetowane.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Tworzenie aplikacji sieci Web ASP.NET

Rozpocznij od instalowania i uruchamiania [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej.

> [!NOTE]
> Ostrzeżenie: Należy zainstalować program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) do ukończenia tego samouczka.


1. Utwórz nowy projekt sieci Web ASP.NET i wybierz szablon MVC. Formularze sieci Web obsługuje również tożsamości platformy ASP.NET, można wykonać podobne kroki w aplikacji formularzy sieci web.
2. Pozostaw domyślne uwierzytelnianie jako **indywidualnych kont użytkowników**.
3. Uruchom aplikację, kliknij przycisk **zarejestrować** i łącza do zarejestrowania użytkownika. W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atrybutu.
4. W Eksploratorze serwera, przejdź do **Connections\DefaultConnection\Tables\AspNetUsers danych**, kliknij prawym przyciskiem myszy i wybierz **Otwórz definicję tabeli**.

    Poniższy obraz przedstawia `AspNetUsers` schematu:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Kliknij prawym przyciskiem myszy **AspNetUsers** tabeli i wybierz **Pokaż dane tabeli**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
 W tym momencie wiadomości e-mail nie został potwierdzony.

Domyślny magazyn danych dla tożsamości ASP.NET jest Entity Framework, ale można skonfigurować go do używania innych magazynów danych i dodać więcej pól. Zobacz [dodatkowe zasoby](#addRes) sekcji na końcu tego samouczka.

[Klasy początkowej OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) jest wywoływane, gdy rozpoczyna się i wywołuje aplikacji `ConfigureAuth` metody w *aplikacji\_Start\Startup.Auth.cs*, które Konfiguruje potok OWIN i inicjuje tożsamości ASP.NET. Sprawdź `ConfigureAuth` metody. Każdy `CreatePerOwinContext` wywołania rejestruje wywołanie zwrotne (zapisane w `OwinContext`) która będzie wywoływana raz na żądanie utworzenia wystąpienia określonego typu. Można ustawić punktu przerwania w Konstruktorze i `Create` metody każdego typu (`ApplicationDbContext, ApplicationUserManager`) i sprawdź, są one nazywane na każdym żądaniu. Wystąpienie elementu `ApplicationDbContext` i `ApplicationUserManager` jest przechowywana w kontekście OWIN, który jest dostępny w całej aplikacji. Przechwytuje tożsamości ASP.NET do potoku OWIN za pomocą oprogramowania pośredniczącego plików cookie. Aby uzyskać więcej informacji, zobacz [na żądanie Zarządzanie okresem istnienia klasy interfejs UserManager w produkcie ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Jeśli zmienisz profil zabezpieczeń nową sygnaturę bezpieczeństwa są generowane i przechowywane w `SecurityStamp` pole *AspNetUsers* tabeli. Uwaga: `SecurityStamp` pola różni się od pliku cookie zabezpieczeń. Plik cookie zabezpieczeń nie są przechowywane w `AspNetUsers` tabeli (lub dowolnego miejsca w bazie danych tożsamości). Token pliku cookie zabezpieczeń jest podpisany przy użyciu [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) i zostanie utworzony z `UserId, SecurityStamp` i informacje dotyczące czasu wygaśnięcia.

Oprogramowanie pośredniczące plików cookie sprawdza, czy plik cookie na każde żądanie. `SecurityStampValidator` Metody w `Startup` klasy trafienia bazę danych i okresowo sprawdza sygnaturę bezpieczeństwa z określonej `validateInterval`. Tylko dzieje co 30 minut (w naszym przykładzie), chyba że zostanie zmienione profil zabezpieczeń. Aby zminimalizować rund do bazy danych wybrano 30 minut. Zobacz Moje [samouczek uwierzytelniania dwuskładnikowego](index.md) więcej szczegółów.

Na komentarze w kodzie `UseCookieAuthentication` metoda obsługuje uwierzytelniania plików cookie. `SecurityStamp` Pola i skojarzony kod zapewnia dodatkową warstwę zabezpieczeń do aplikacji, gdy zmieniasz hasło, będziesz się logować bez przeglądarki podczas logowania. `SecurityStampValidator.OnValidateIdentity` Metody umożliwia aplikacji weryfikacji tokenu zabezpieczeń podczas logowania użytkownika, używana w przypadku zmiany hasła lub użyj logowania zewnętrznego. Jest to niezbędne do zapewnienia, że wszystkie tokeny (pliki cookie) generowany ze starym hasłem są unieważnione. W przykładowy projekt, w przypadku zmiany hasła użytkowników następnie nowy token jest generowany dla użytkownika, wszystkie poprzednie tokeny są unieważniona i `SecurityStamp` pole jest aktualizowane.

Systemu tożsamości pozwalają skonfigurować aplikację tak po zmianie profil zabezpieczeń użytkowników (na przykład gdy użytkownik zmieni swoje hasło lub zmiany skojarzone logowania (przykład, Facebook, Google, konta Microsoft, itp.), użytkownik jest zalogowany spośród wszystkich wystąpienia przeglądarki. Na przykład obraz poniżej przedstawia [próbki pojedynczego wylogowanie](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) aplikacji, która umożliwia użytkownikowi Wyloguj wszystkie wystąpienia przeglądarki (w tym przypadku IE, Firefox i Chrome), klikając jeden z przycisków. Alternatywnie próbki pozwala tylko wylogować się wystąpienia określonej przeglądarki.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

[Próbki pojedynczego wylogowanie](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) aplikacji pokazuje, jak ASP.NET Identity można ponownie wygenerować tokenu zabezpieczającego. Jest to niezbędne do zapewnienia, że wszystkie tokeny (pliki cookie) generowany ze starym hasłem są unieważnione. Ta funkcja zapewnia dodatkową warstwę zabezpieczeń do tej aplikacji; Jeśli zmienisz hasło, będziesz się logować się, gdy użytkownik zalogował się do tej aplikacji.

*Aplikacji\_Start\IdentityConfig.cs* plik zawiera `ApplicationUserManager`, `EmailService` i `SmsService` klasy. `EmailService` i `SmsService` każdej implementacji klasy `IIdentityMessageService` interfejsu, dlatego należy typowe metody w każdej klasy, aby skonfigurować adres e-mail i SMS. Mimo że w tym samouczku tylko przedstawiono sposób dodawania powiadomienia pocztą e-mail za pomocą [SendGrid](http://sendgrid.com/), możesz wysłać wiadomości e-mail przy użyciu SMTP i innych mechanizmów.

`Startup` Klasa zawiera także kocioł tablicy można dodać logowania społecznościowych (Facebook, Twitter, itp.), zobacz samouczek Mój [aplikacji MVC 5 za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Aby uzyskać więcej informacji.

Sprawdź `ApplicationUserManager` klasy, która zawiera informacje o tożsamości użytkowników i konfiguruje następujące funkcje:

- Wymagania dotyczące siły hasła.
- Blokowanie użytkownika (prób i czas).
- Uwierzytelnianie dwuskładnikowe (2FA). W samouczku innego będzie obejmować 2FA i programu SMS.
- Podłączanie poczty e-mail i usług programu SMS. (I będzie obejmować programu SMS w samouczku innego).

`ApplicationUserManager` Klasa pochodzi od ogólnych `UserManager<ApplicationUser>` klasy. `ApplicationUser`pochodną [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser`pochodzi z ogólnego `IdentityUser` klasy:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Powyżej właściwości pokrywają się z właściwości w `AspNetUsers` tabeli pokazano powyżej.

Argumenty rodzajowe w `IUser` umożliwiającą klasy przy użyciu różnych typów dla klucza podstawowego. Zobacz [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) próbki, która pokazuje, jak zmienić klucz podstawowy z ciągu na int lub identyfikatora GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser`(`public class ApplicationUserManager : UserManager<ApplicationUser>`) jest zdefiniowany w *Models\IdentityModels.cs* jako:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Generuje wyróżniony kod powyżej [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity i uwierzytelniania plików Cookie OWIN są oparte na oświadczeniach, w związku z tym framework wymaga aplikacji do wygenerowania `ClaimsIdentity` dla użytkownika. `ClaimsIdentity`zawiera informacje o wszystkich oświadczenia dla użytkownika, takie jak nazwa użytkownika, wieku i jakie role użytkownika należy do. Na tym etapie można również dodać więcej oświadczenia dla użytkownika.

OWIN `AuthenticationManager.SignIn` metoda przekazuje w `ClaimsIdentity` i loguje się użytkownik:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[Aplikacji MVC 5 za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) przedstawiono sposób dodawania dodatkowych właściwości do `ApplicationUser` klasy.

## <a name="email-confirmation"></a>Wiadomości e-mail z potwierdzeniem

Jest dobrym pomysłem jest potwierdzenia adresu e-mail nowy użytkownik rejestruje, aby sprawdzić ich są nie personifikowanie innego użytkownika (czyli one nie został zarejestrowany przy do kogoś innego adresu e-mail). Załóżmy, że masz forum dyskusyjne, czy chcesz zapobiec `"bob@example.com"` z rejestracją jako `"joe@contoso.com"`. Bez wiadomości e-mail z potwierdzeniem `"joe@contoso.com"` można pobrać niechcianych wiadomości e-mail z aplikacji. Załóżmy, że Bob przypadkowo zarejestrowany jako `"bib@example.com"` i nie zauważyć, ADAM nie będą mogli używać hasła odzyskiwania, ponieważ aplikacja nie ma jego prawidłowy adres e-mail. Wiadomości e-mail z potwierdzeniem chroni tylko ograniczone z robotów i nie zapewnia ochrony z określone nadawcy wiadomości-śmieci, ponieważ mają one wiele aliasów e-mail pracy służące do rejestrowania. W poniższym przykładzie użytkownik będzie mógł zmienić swoje hasło, dopóki ich konto zostało potwierdzone (przez kliknięcie łącza potwierdzenie odebrane w zarejestrowani z konta e-mail.) Ten przepływ pracy można stosować do inne scenariusze, na przykład wysyłanie łącza potwierdzić i zresetować hasło na nowych kont utworzonych przez administratora, wysyłanie wiadomości e-mail użytkownika, gdy swój profil zostały zmienione i tak dalej. Ogólnie rzecz biorąc chcesz uniemożliwić przesyłanie danych do witryny sieci web, zanim zostały potwierdzone za pośrednictwem poczty e-mail, wiadomość SMS lub inny mechanizm nowych użytkowników. <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>Tworzenie bardziej szczegółowy próbki

W tej sekcji użyjesz NuGet można pobrać przykładowy bardziej szczegółowy zajmiemy się.

1. Utwórz nową ***pusty*** projektu sieci Web ASP.NET.
2. W konsoli Menedżera pakietów, wprowadź następujące polecenia: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

 W tym samouczku użyjemy [SendGrid](http://sendgrid.com/) do wysyłania wiadomości e-mail. `Identity.Samples` Pakiet instaluje firma Microsoft będzie działać z kodu.
3. Ustaw [projektu do używania protokołu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Przetestuj tworzenia konta lokalnego, uruchamiając aplikację, klikając **zarejestrować** link i przesyłanie formularza rejestracyjnego.
5. Kliknij łącze pokaz poczty e-mail, która symuluje wiadomości e-mail z potwierdzeniem.
6. Usuń kod potwierdzenia pokaz e-mail łączy z próbki ( `ViewBag.Link` kodu w kontrolerze konta. Zobacz `DisplayEmail` i `ForgotPasswordConfirmation` metody akcji i widokami razor).

> [!NOTE]
> Ostrzeżenie: Jeśli zmienisz jakiekolwiek ustawienia zabezpieczeń w tym przykładzie produkcji aplikacje będą wymagały zastosowane inspekcji zabezpieczeń jawnie wywołującą zmiany wprowadzone do.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Badanie kodu w aplikacji\_Start\IdentityConfig.cs

Przykład pokazuje, jak utworzyć konto i dodać go do *Admin* roli. Wiadomości e-mail w próbce należy zastąpić poczty e-mail, który będzie używany dla konta administratora. Najprostszym sposobem od razu utworzyć konto administratora jest programowo w `Seed` metody. Mamy nadzieję w przyszłości narzędzia, które umożliwiają tworzenie i administrowanie użytkownikami i rolami. Przykładowy kod umożliwiają tworzenie i zarządzanie użytkownikami i rolami, ale najpierw musi mieć konto administratorów do uruchomienia stron administracyjnych na użytkowników i ról. W tym przykładzie konto administratora jest tworzony podczas bazy danych jest obsługiwany.

Zmiany hasła i Zmień nazwę konta, których chcesz otrzymywać powiadomienia e-mail.

> [!WARNING]
> Zabezpieczenia — nigdy nie magazynu danych poufnych w kodzie źródłowym.

Jak wspomniano wcześniej, `app.CreatePerOwinContext` wywołanie w klasie uruchamiania dodaje wywołania zwrotne `Create` metoda aplikacji bazy danych zawartości, Menedżer i roli menedżera klas użytkowników. Wywołania w potoku OWIN `Create` metody dla tych klas dla każdego żądania i przechowuje kontekst dla każdej klasy. Konto kontrolera udostępnia Menedżera użytkowników z kontekstu HTTP (zawierający kontekst OWIN):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Gdy użytkownik rejestruje konta lokalnego, `HTTP Post Register` metoda jest wywoływana:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

Powyższy kod używa danych modelu, aby utworzyć nowe konto użytkownika przy użyciu poczty e-mail i hasło. Jeśli alias e-mail znajduje się w magazynie danych, tworzenie konta usługi nie powiedzie się i ponownie wyświetlić formularza. `GenerateEmailConfirmationTokenAsync` Metoda tworzy token potwierdzenia bezpiecznego i zapisuje je w magazynie danych tożsamości ASP.NET. [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) metoda tworzy łącze zawierające `UserId` i tokenu potwierdzenia. Ten link jest następnie pocztą e-mail do użytkownika, użytkownik może kliknąć łącze w swoich aplikacji poczty e-mail, aby potwierdzić swoje konto.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Konfigurowanie wiadomości e-mail z potwierdzeniem

Przejdź do [stronę Tworzenie konta Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) i zarejestrować bezpłatne konto. Dodaj kod podobne do następujących czynności, aby skonfigurować SendGrid:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Klienci poczty e-mail często akceptuje tylko wiadomości tekstowych (nie HTML). Należy podać komunikat w tekst i HTML. W powyższym przykładzie SendGrid jest to zrobić za pomocą `myMessage.Text` i `myMessage.Html` kodzie pokazanym powyżej.


Poniższy kod przedstawia sposób wysłania wiadomości e-mail przy użyciu [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) klasy where `message.Body` zwraca tylko łącze.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Zabezpieczenia — nigdy nie magazynu danych poufnych w kodzie źródłowym. Konto i poświadczenia są przechowywane w appSetting. Na platformie Azure, można bezpiecznie przechowywać te wartości na  **[Konfiguruj](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  kartę w portalu Azure. Zobacz [najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych do platformy ASP.NET i usługi Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


Wprowadź swoje poświadczenia SendGrid, uruchom aplikację, rejestru z aliasem poczty e-mail można kliknij łącze Potwierdź w wiadomości e-mail. Aby sprawdzić, jak w tym z Twojej [Outlook.com](http://outlook.com) konto e-mail, zobacz Jan Atten [konfiguracji SMTP C# dla hosta SMTP Outlook.Com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) i jego[ASP.NET 2.0 tożsamości: ustawienia weryfikacji konta i dwuskładnikowego autoryzacji](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) ogłoszeń.

Gdy użytkownik kliknie **zarejestrować** przycisk wiadomość e-mail z potwierdzeniem zawierające token weryfikacji są wysyłane na adres e-mail użytkownika.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

Użytkownik jest wysyłana wiadomość e-mail z token potwierdzenia dla swojego konta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Sprawdź kod

Poniższy kod przedstawia `POST ForgotPassword` metody.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

Metoda kończy się niepowodzeniem dyskretnie, jeśli nie został potwierdzony adres e-mail użytkownika. Jeśli błąd został opublikowany dla nieprawidłowego adresu e-mail, złośliwi użytkownicy, można użyć tych informacji można znaleźć prawidłowy identyfikator użytkownika (aliasów e-mail) na ataki.

Poniższy kod przedstawia `ConfirmEmail` metody w kontrolerze konta, które jest wywoływane, gdy użytkownik kliknie łącze potwierdzenia w wiadomości e-mail wysyłane do nich:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Gdy użyto token zapomniane hasło zostało unieważnione. Następujące zmiany kodu `Create` — metoda (w *aplikacji\_Start\IdentityConfig.cs* pliku) ustawia tokeny wygaśnie po upływie 3 godziny.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Z powyższym kodzie zapomniane hasło i tokeny potwierdzenia e-mail wygasa 3 godziny. Wartość domyślna `TokenLifespan` to jeden dzień.

Poniższy kod przedstawia metody potwierdzenia poczty e-mail:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Aby zapewnić bardziej bezpieczne aplikacji, ASP.NET Identity obsługuje uwierzytelnianie dwuskładnikowe (2FA). Zobacz [tożsamości ASP.NET 2.0: Sprawdzanie poprawności konta i dwuskładnikowego autoryzacji](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) przez Atten Jan. Mimo że można ustawić blokady konta na niepowodzenia próba hasło logowania, takiego podejścia sprawia, że logowanie podatne na [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) blokady. Firma Microsoft zaleca się, że używasz blokady konta tylko z 2FA.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Omówienie niestandardowych dostawców magazynu dla produktu ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Aplikacji MVC 5 za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) także przedstawiono sposób dodawania informacji o profilu do tabeli użytkowników.
- [ASP.NET MVC i tożsamość 2.0: opis podstawowych funkcji](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) przez Atten Jan.
- [Wprowadzenie do produktu ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Anonsowanie RTM programu ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) przez Pranav Rastogi.
