---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Potwierdzenie konta i odzyskiwanie hasła w produkcie ASP.NET Identity (C#) | Dokumentacja firmy Microsoft
author: HaoK
description: Przed wykonaniem tej instrukcji, które należy wykonać tworzenie bezpiecznej aplikacji sieci web ASP.NET MVC 5 z logowaniem, adres e-mail potwierdzenia i resetowaniem hasła. W tym samouczku...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 84f35cfc0f0e0f1c268e0e9c18fd47aa68deb7d1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577837"
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Potwierdzenie konta i odzyskiwanie hasła w produkcie ASP.NET Identity (C#)
====================
przez [Hao Kung](https://github.com/HaoK), [autorem jest Pranav Rastogi](https://github.com/rustd), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Suhas Joshi](https://github.com/suhasj)

> Przed wykonaniem tego samouczka należy najpierw wykonać [tworzenie bezpiecznej aplikacji sieci web ASP.NET MVC 5 z logowaniem, adres e-mail potwierdzenia i resetowaniem hasła](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). W tym samouczku zawiera więcej szczegółowych informacji i pokażą, jak skonfigurować konta e-mail o potwierdzenie konta lokalnego i użytkownicy mogli zresetować zapomniane hasło w produkcie ASP.NET Identity. W tym artykule został napisany przez Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), autorem jest Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung i Suhas Joshi. Przykład NuGet został napisany głównie przez Hao Kung.


Konto użytkownika lokalnego wymaga od użytkownika utworzyć hasło dla konta, a to hasło jest przechowywane w (bezpieczne) w aplikacji sieci web. ASP.NET Identity obsługuje również kont społecznościowych, które nie wymaga od użytkownika utworzyć hasło aplikacji. [Kont społecznościowych](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) używają innych firm (np. Google, Twitter, Facebook lub Microsoft) do uwierzytelniania użytkowników. W tym temacie omówiono następujące czynności:

- [Tworzenie aplikacji ASP.NET MVC](#createMvc) i zapoznać się z funkcjami tożsamości ASP.NET.
- [Tworzenie przykładowej tożsamości](#build)
- [Konfigurowanie potwierdzenie adresu e-mail](#email)

Nowi użytkownicy rejestrowanie swoich aliasu adresu e-mail, co powoduje utworzenie konta lokalnego.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Klikając przycisk Zarejestruj wysyła wiadomość e-mail z potwierdzeniem zawierające token sprawdzania poprawności adresów e-mail.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

Użytkownik otrzymuje wiadomość e-mail za pomocą tokenu potwierdzenia dla swojego konta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Kliknięcie łącza potwierdzenie konta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Resetowanie/odzyskiwanie hasła

Lokalnych użytkowników, którzy zapomni swoje hasło może mieć token zabezpieczający wysyłane do swojego konta e-mail, umożliwiające ich do zresetowania swojego hasła.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Użytkownik zostanie wkrótce otrzymasz wiadomość e-mail z linkiem, umożliwiając im do zresetowania swojego hasła.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Kliknięcie łącza spowoduje przejście do strony resetowania.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Klikając **resetowania** przycisk wyświetli monit o potwierdzenie zresetowano hasło.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Tworzenie aplikacji sieci Web platformy ASP.NET

Rozpocznij od instalowania i uruchamiania [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej.

> [!NOTE]
> Ostrzeżenie: Należy zainstalować program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) do ukończenia tego samouczka.


1. Utwórz nowy projekt sieci Web platformy ASP.NET, a następnie wybierz szablon MVC. Formularze sieci Web obsługuje również produktu ASP.NET Identity, dzięki czemu można wykonać podobne kroki w aplikacji formularzy sieci web.
2. Pozostaw domyślne uwierzytelnianie jako **indywidualne konta użytkowników**.
3. Uruchom aplikację, kliknij przycisk **zarejestrować** link i zarejestrować użytkownik. W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atrybutu.
4. W Eksploratorze serwera przejdź do **Connections\DefaultConnection\Tables\AspNetUsers danych**, kliknij prawym przyciskiem myszy i wybierz **Otwórz definicję tabeli**.

    Na poniższej ilustracji przedstawiono `AspNetUsers` schematu:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Kliknij prawym przyciskiem myszy **AspNetUsers** tabeli, a następnie wybierz pozycję **Pokaż dane tabeli**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   W tym momencie wiadomości e-mail nie został potwierdzony.

Domyślny magazyn danych dla produktu ASP.NET Identity jest Entity Framework, ale możesz ją skonfigurować tak, aby użyć innych magazynów danych i dodania kolejnych pól. Zobacz [dodatkowe zasoby](#addRes) sekcji na końcu tego samouczka.

[Klasy początkowej OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) jest wywoływana, gdy aplikacja rozpoczyna się i wywołuje `ConfigureAuth` method in Class metoda *aplikacji\_Start\Startup.Auth.cs*, które Konfiguruje potok OWIN i inicjuje tożsamości ASP.NET. Sprawdź `ConfigureAuth` metody. Każdy `CreatePerOwinContext` wywołanie rejestruje wywołanie zwrotne (zapisany w `OwinContext`), będzie lze volat pouze jednou na żądanie, aby utworzyć wystąpienie określonego typu. Możesz ustawić punkt przerwania w Konstruktorze i `Create` metoda każdego typu (`ApplicationDbContext, ApplicationUserManager`) i sprawdź, są one nazywane na każde żądanie. To wystąpienie `ApplicationDbContext` i `ApplicationUserManager` znajduje się w kontekście OWIN, które są dostępne w całej aplikacji. Przechwytuje produktu ASP.NET Identity do potoku OWIN za pośrednictwem oprogramowaniu pośredniczącym pliku cookie. Aby uzyskać więcej informacji, zobacz [na zarządzanie okresem istnienia żądanie dla Menedżera UserManager klasy w produkcie ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Zmiany profilu zabezpieczeń nową sygnaturę bezpieczeństwa jest wygenerowany i przechowywany w `SecurityStamp` pole *AspNetUsers* tabeli. Uwaga: `SecurityStamp` pola różni się od pliku cookie zabezpieczeń. Plik cookie zabezpieczeń nie są przechowywane w `AspNetUsers` tabeli (lub dowolnego miejsca w bazie danych tożsamości). Token pliku cookie zabezpieczeń jest z podpisem własnym za pomocą [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) i jest tworzona przy użyciu `UserId, SecurityStamp` i informacji czasu wygaśnięcia.

Oprogramowaniu pośredniczącym pliku cookie sprawdza, czy plik cookie na każde żądanie. `SecurityStampValidator` Method in Class metoda `Startup` klasy trafienia bazy danych i okresowo sprawdza sygnaturę bezpieczeństwa określone za pomocą `validateInterval`. Dzieje się to tylko co 30 minut (w naszym przykładzie) Jeśli nie zmienisz swój profil zabezpieczeń. W okresie 30 minut została wybrana, aby zminimalizować przesłania do bazy danych. Zobacz Mój [samouczek dotyczący uwierzytelniania dwuskładnikowego](index.md) Aby uzyskać więcej informacji.

Na komentarze w kodzie `UseCookieAuthentication` metoda obsługuje uwierzytelnianie za pomocą plików cookie. `SecurityStamp` Pola i skojarzonego kodu zapewnia dodatkową warstwę zabezpieczeń do aplikacji, po zmianie hasła, będziesz się logować poza przeglądarką podczas logowania. `SecurityStampValidator.OnValidateIdentity` Metody umożliwia aplikacji weryfikacji tokenu zabezpieczeń, gdy użytkownik loguje się, który jest używany podczas zmiany hasła lub logowania zewnętrznego. Jest to niezbędne, aby upewnić się, że wszystkie tokeny (pliki cookie) generowany ze starym hasłem nie są unieważniane. W przykładowym projekcie, jeśli zmienisz hasło użytkownika, następnie nowego tokenu jest generowany dla użytkownika, wszystkie poprzednie tokeny są unieważniane i `SecurityStamp` pola do zaktualizowania.

System tożsamości umożliwiają skonfigurowanie aplikacji tak po zmianie profil zabezpieczeń użytkowników (na przykład, gdy użytkownik zmieni się ich haseł lub zmiany skojarzone logowania (np. od 1.02.2016 Facebook, Google, konta Microsoft, itp.), użytkownik jest zalogowany spośród wszystkich wystąpienia przeglądarki. Na przykład obraz poniżej przedstawia [przykładowy pojedynczy wylogowanie](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) aplikacji, która umożliwia użytkownikowi Wyloguj się ze wszystkich wystąpień przeglądarki (w tym przypadku IE, Firefox i Chrome), klikając jeden z przycisków. Alternatywnie próbki pozwala zalogować się wyłącznie z wystąpienia przeglądarki.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

[Przykładowy pojedynczy wylogowanie](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) aplikacji pokazuje, jak produktu ASP.NET Identity umożliwia ponowne generowanie tokenu zabezpieczającego. Jest to niezbędne, aby upewnić się, że wszystkie tokeny (pliki cookie) generowany ze starym hasłem nie są unieważniane. Funkcja ta zapewnia dodatkową warstwę zabezpieczeń do aplikacji; Po zmianie hasła, użytkownik zostanie wylogowany gdzie użytkownik zalogował się do tej aplikacji.

*Aplikacji\_Start\IdentityConfig.cs* plik zawiera `ApplicationUserManager`, `EmailService` i `SmsService` klasy. `EmailService` i `SmsService` klas każdego implementują `IIdentityMessageService` interfejsu, więc typowe metody każdej klasy, aby skonfigurować wiadomości e-mail i wiadomości SMS. Chociaż ten samouczek przedstawia tylko sposób dodawania powiadomienie e-mail za pośrednictwem [SendGrid](http://sendgrid.com/), możesz wysłać wiadomość e-mail przy użyciu SMTP i innych mechanizmów.

`Startup` Klasa zawiera także kocioł płytkę do dodawania logowań społecznościowych (Facebook, Twitter itp.), zobacz samouczek Moje [MVC 5 aplikacji za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowanie jednokrotne](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Aby uzyskać więcej informacji.

Sprawdź `ApplicationUserManager` klasy, która zawiera informacje o tożsamości użytkowników i konfiguruje następujące funkcje:

- Wymagania dotyczące siły hasła.
- Blokowanie użytkownika (prób i czas).
- Uwierzytelnianie dwuskładnikowe (2FA). W innym samouczku będzie obejmować funkcji 2FA i wiadomości SMS.
- Podłączanie poczty e-mail i usług programu SMS. (I będzie obejmować programu SMS w innym samouczku).

`ApplicationUserManager` Klasa pochodzi od ogólnego `UserManager<ApplicationUser>` klasy. `ApplicationUser` pochodzi od klasy [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` jest pochodną ogólnego `IdentityUser` klasy:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Właściwości przedstawionych powyżej pokrywają się z właściwościami w `AspNetUsers` tabeli powyżej.

Argumenty ogólne na `IUser` umożliwiającą klasy przy użyciu różnych typów dla klucza podstawowego. Zobacz [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) próbki, która pokazuje, jak zmienić klucza podstawowego z ciągu na int lub identyfikator GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) jest zdefiniowany w *Models\IdentityModels.cs* jako:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Generuje wyróżniony kod powyżej [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity i uwierzytelniania plików Cookie OWIN są oparte na oświadczeniach, w związku z tym struktura wymaga aplikację, aby wygenerować `ClaimsIdentity` dla użytkownika. `ClaimsIdentity` zawiera informacje o wszystkie oświadczenia dla użytkownika, takie jak nazwa użytkownika, wiek i role, jakie należy użytkownik. Można również dodać więcej oświadczenia dla użytkownika na tym etapie.

OWIN `AuthenticationManager.SignIn` metoda przekazuje `ClaimsIdentity` i loguje się użytkownik:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[MVC 5 aplikacji za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowanie jednokrotne](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) pokazuje, jak dodać dodatkowe właściwości, aby `ApplicationUser` klasy.

## <a name="email-confirmation"></a>Potwierdzenie adresu e-mail

To dobry pomysł, aby potwierdzić wiadomości e-mail nowy użytkownik rejestruje, aby sprawdzić ich nie podszyć się pod kogoś innego (oznacza to one nie zostały zarejestrowane przy użyciu adresu e-mail osoby). Załóżmy, że masz forum dyskusyjne, czy chcesz uniemożliwić `"bob@example.com"` z rejestracją jako `"joe@contoso.com"`. Bez potwierdzenia e-mail `"joe@contoso.com"` można pobrać niechcianych wiadomości e-mail z aplikacji. Załóżmy, że Bob przypadkowo zarejestrowany jako `"bib@example.com"` i był wystąpieniem, użytkownik nie będzie mogła używać hasła odzyskiwania, ponieważ aplikacja nie ma jego prawidłowy adres e-mail. Potwierdzenie adresu e-mail zawiera tylko ograniczoną ochronę z botami i nie zapewnia ochrony z spamerów określone, ponieważ mają one wiele aliasów e-mail pracy, używanego do rejestrowania. W poniższym przykładzie użytkownik nie będzie mógł zmienić swoje hasło, dopóki ich konto zostało potwierdzone (przez nich klikając link do potwierdzenia odebranych na konto e-mail zarejestrowane w usłudze.) Ten przepływ pracy można stosować do innych scenariuszach, na przykład wysyłanie linku do potwierdzenia i resetowania hasła dla nowych kont utworzonych przez administratora, wysyłanie wiadomości e-mail użytkownika, gdy zostały zmienione swój profil i tak dalej. Zazwyczaj chcesz uniemożliwić nowym użytkownikom publikowanie żadnych danych do witryny sieci web, zanim zostały potwierdzone pocztą e-mail, wiadomość SMS lub innego mechanizmu. <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>Tworzenie bardziej kompletny przykład

W tej sekcji użyjesz NuGet można pobrać pełniejszy przykład, w których firma Microsoft będzie pracować.

1. Utwórz nową ***pusty*** projektu sieci Web ASP.NET.
2. W konsoli Menedżera pakietów wprowadź następujące polecenia: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   W tym samouczku użyjemy [SendGrid](http://sendgrid.com/) do wysyłania wiadomości e-mail. `Identity.Samples` Pakiet instaluje kodu, firma Microsoft będzie pracował z.
3. Ustaw [projektu do używania protokołu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Testowanie tworzenia konta lokalnego, uruchamiając aplikację, klikając **zarejestrować** link i publikowanie formularza rejestracyjnego.
5. Kliknij link wiadomości e-mail pokaz, zasymulowanie potwierdzenie adresu e-mail.
6. Usuń kod potwierdzenia łącze w wiadomości e-mail pokaz z przykładu ( `ViewBag.Link` kod na kontrolerze konta. Zobacz `DisplayEmail` i `ForgotPasswordConfirmation` metody akcji i widokami razor).

> [!NOTE]
> Ostrzeżenie: W przypadku zmiany ustawień zabezpieczeń, w tym przykładzie produkcji aplikacji będzie musiała zostać poddane inspekcji zabezpieczeń, który jawnie wywołuje zmiany wprowadzone.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Sprawdź kod w aplikacji\_Start\IdentityConfig.cs

Przykład pokazuje, jak utworzyć konto i dodaj go do *administratora* roli. Za pomocą adresu e-mail, który będzie używany dla konta administratora, należy zastąpić poczty e-mail w próbce. Najprostszym sposobem teraz, aby utworzyć konto administratora jest Programując `Seed` metody. Mamy nadzieję, że w przyszłości narzędzia, która umożliwi Ci tworzyć i zarządzać nimi, użytkownikami i rolami. Przykładowy kod umożliwiają tworzenie i zarządzanie użytkownikami i rolami, ale musi najpierw mieć konta administratorów w taki sposób, aby uruchomić ról i strony administratora dla użytkownika. W tym przykładzie konto administratora jest tworzone, gdy baza danych jest zasilany.

Zmień hasło i Zmień nazwę konta otrzymywania powiadomień pocztą e-mail.

> [!WARNING]
> Zabezpieczenia — nigdy nie przechowywania danych poufnych w kodzie źródłowym.

Jak wspomniano wcześniej, `app.CreatePerOwinContext` wywołania w klasie uruchamiania dodaje wywołania zwrotne `Create` metoda aplikacji bazy danych zawartości, Menedżera i rolą Menedżera klas użytkowników. OWIN potok wywołuje `Create` metody te klasy dla każdego żądania i przechowuje kontekstu dla każdej klasy. Kontroler kont uwidacznia Menedżera użytkowników z kontekstu HTTP, (który zawiera kontekst OWIN):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Gdy użytkownik rejestruje kontem lokalnym `HTTP Post Register` metoda jest wywoływana:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

Powyższy kod używa modelu danych do utworzenia nowego konta użytkownika, za pomocą poczty e-mail i hasło wprowadzone. Jeśli alias e-mail znajduje się w magazynie danych, tworzenie konta usługi nie powiedzie się i zostanie ponownie wyświetlony formularz. `GenerateEmailConfirmationTokenAsync` Metoda tworzy token potwierdzenia bezpieczny i zapisuje je w magazynie danych tożsamości ASP.NET. [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) metoda tworzy łącze zawierające `UserId` i tokenu potwierdzenia. Ten link jest następnie wysyłany pocztą e-mail do użytkownika, użytkownik może kliknąć łącze w swojej aplikacji poczty e-mail, aby potwierdzić swoje konto.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Konfigurowanie potwierdzenie adresu e-mail

Przejdź do [stronę rejestracji usługi SendGrid platformy Azure](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) i Zarejestruj się bezpłatnie konto. Dodaj kod przypominający poniższy, aby skonfigurować usługi SendGrid:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Klienci poczty e-mail często akceptuje tylko wiadomości tekstowych (bez kodu HTML). Należy podać komunikat w tekst i HTML. W powyższym przykładzie SendGrid odbywa się za pomocą `myMessage.Text` i `myMessage.Html` kodzie pokazanym powyżej.


Poniższy kod przedstawia sposób wysłania wiadomości e-mail przy użyciu [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) klasy gdzie `message.Body` zwraca tylko łącze.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Zabezpieczenia — nigdy nie przechowywania danych poufnych w kodzie źródłowym. Konto i poświadczenia są przechowywane w appSetting. Na platformie Azure, możesz bezpiecznie przechowywać te wartości na **[Konfiguruj](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** kartę w witrynie Azure portal. Zobacz [najlepsze rozwiązania dotyczące wdrażania haseł i innych danych poufnych na platformie ASP.NET i Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


Wprowadź swoje poświadczenia usługi SendGrid, uruchomić aplikację, zarejestruj się za pomocą aliasu adresu e-mail, można kliknąć łącze Potwierdź w wiadomości e-mail. Aby zobaczyć, jak wykonać to za pomocą usługi [Outlook.com](http://outlook.com) konto e-mail, zobacz John Atten [C# konfiguracja SMTP dla hosta SMTP Outlook.Com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) i jego[tożsamości ASP.NET w wersji 2.0: ustawienie Weryfikacja konta i autoryzacji Two-Factor Authentication](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) wpisów.

Gdy użytkownik kliknie **zarejestrować** przycisk zawierające token weryfikacji wiadomość e-mail z potwierdzeniem są wysyłane do swojego adresu e-mail.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

Użytkownik otrzymuje wiadomość e-mail za pomocą tokenu potwierdzenia dla swojego konta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Poszukaj w kodzie

Poniższy kod przedstawia `POST ForgotPassword` metody.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

Metoda kończy się niepowodzeniem dyskretnie Jeśli adres e-mail użytkownika nie został potwierdzony. Jeśli błąd został opublikowany na nieprawidłowy adres e-mail, złośliwi użytkownicy użyć tych informacji można znaleźć prawidłowy identyfikator użytkownika (aliasów e-mail) na ataki.

Poniższy kod przedstawia `ConfirmEmail` metody w kontroler kont, który jest wywoływana, gdy użytkownik kliknie łącze potwierdzenia w wiadomości e-mail wysyłanej do nich:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Po użyciu token zapomniane hasło zostało unieważnione. Następujące zmiany kodu w `Create` — metoda (w *aplikacji\_Start\IdentityConfig.cs* pliku) ustawia tokeny wygaśnie za 3 godziny.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Za pomocą powyższego kodu zapomnianego hasła i tokeny potwierdzenia e-mail wygaśnie 3 godziny. Wartość domyślna `TokenLifespan` to jeden dzień.

Poniższy kod przedstawia metodę potwierdzenie e-mail:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Aby zapewnić bardziej bezpieczne aplikacji, produktu ASP.NET Identity obsługuje uwierzytelnianie dwuskładnikowe (2FA). Zobacz [tożsamości ASP.NET 2.0: Sprawdzanie poprawności konta i autoryzacji Two-Factor Authentication](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) przez Atten Jan. Mimo że można ustawić blokady konta na niepowodzenia próby logowania do hasła, takie podejście sprawia, że Twoje dane logowania podatne na [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) blokady. Zaleca się, że używasz blokady konta tylko za pomocą funkcji 2FA.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Omówienie niestandardowych dostawców magazynu dla produktu ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [MVC 5 aplikacji za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowanie jednokrotne](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) także przedstawiono sposób dodawania informacji o profilu z tabelą użytkowników.
- [ASP.NET MVC i tożsamości w wersji 2.0: zrozumienie podstaw](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) przez Atten Jan.
- [Wprowadzenie do produktu ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Ogłoszenie RTM produktu ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) przez autorem jest Pranav Rastogi.
