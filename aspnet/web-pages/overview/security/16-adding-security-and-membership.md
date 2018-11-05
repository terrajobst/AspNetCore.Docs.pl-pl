---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Dodawanie zabezpieczeń i członkostwa w sieci Web platformy ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym rozdziale pokazano, jak zabezpieczyć witryny sieci Web tak, aby niektóre strony są dostępne tylko dla osób, zaloguj się. (Widoczna będzie również sposób tworzenia stron tha...
ms.author: riande
ms.date: 02/24/2014
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 1c36adf23f3b53e4fbf3dbdce7ca85664b32c975
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021602"
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Dodawanie zabezpieczeń i członkostwa w witrynie ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak zabezpieczyć witrynę sieci Web ASP.NET Web Pages (Razor) tak, aby niektóre strony są dostępne tylko dla osób, zaloguj się. (Również zobaczysz sposób tworzenia stron, które każdy użytkownik może uzyskać dostęp).
> 
> **Zawartość:** 
> 
> - Jak utworzyć witrynę sieci Web, który ma stronę rejestracji i stronę logowania, więc w przypadku niektórych stron można ograniczyć dostęp tylko elementów członkowskich.
> - Jak utworzyć publiczny i tylko do elementu członkowskiego stron.
> - Sposób definiowania ról, które są grupami, które mają uprawnienia zabezpieczeń w witrynie sieci, oraz do przypisywania użytkowników do roli.
> - Jak używać CAPTCHA, aby uniemożliwić automatyczne programom (robotom) tworzenie kont Członkowskich.
>   
> 
> Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzona w artykule:
> 
> - Program WebMatrix **witryny początkowej** szablonu.
> - `WebSecurity` Pomocnika i `Roles` klasy.
> - `ReCaptcha` Pomocnika.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Program WebMatrix 3
> - Bibliotekę pomocników platformy ASP.NET w sieci Web


Można skonfigurować witryny sieci Web, dzięki czemu użytkownicy mogą zalogować się do niej &#8212; oznacza to, aby lokacja obsługuje *członkostwa*. Może to być przydatne w wielu powodów. Na przykład witryna może być stron, które powinny być dostępne tylko do elementów członkowskich. W niektórych przypadkach może wymagać, aby użytkownicy logowali się, aby wyrażać opinie lub Dodaj komentarz.

Nawet wtedy, gdy witryny sieci Web obsługuje członkostwo, użytkownicy nie są zawsze wymagane do logowania, zanim będzie używać niektóre strony w witrynie. Użytkownicy, którzy nie są rejestrowane w są znane jako *użytkowników anonimowych*.

Użytkownik może zarejestrować się w witrynie sieci Web i można następnie zaloguj się do witryny. Witryna sieci Web wymaga nazwy użytkownika (adres e-mail) i hasło, aby upewnić się, że użytkownicy są kogo się podaje. Ten proces logowania i potwierdzenia tożsamości użytkownika jest nazywany *uwierzytelniania*.

Możesz skonfigurować zabezpieczeń i członkostwa na różne sposoby:

- Jeśli używasz programu WebMatrix to prosty sposób jest utworzenie nowej lokacji na podstawie **witryny początkowej** szablonu. Ten szablon został już skonfigurowany dla zabezpieczeń i członkostwa i ma już strony rejestracji, strony logowania i tak dalej.

    Lokacji utworzonej przez szablon ma również opcję, aby zezwolić użytkownikom na zalogowanie się przy użyciu witryny zewnętrznej, takich jak Facebook, Google lub Twitter.
- Jeśli chcesz zwiększyć bezpieczeństwo do istniejącej lokacji lub jeśli nie chcesz używać **witryny początkowej** szablonu, można utworzyć własną stronę rejestracji, strony logowania i tak dalej.

Ten artykuł koncentruje się na pierwszej opcji &mdash; sposób dodawania zabezpieczeń przy użyciu **witryny początkowej** szablonu. Go także pewne podstawowe informacje o tym, jak można implementować własne zabezpieczenia, a następnie udostępnia linki do szczegółowych informacji o tym, jak to zrobić. Brak informacji o sposobie włączania logowań zewnętrznych, który został opisany bardziej szczegółowo w oddzielnym artykule.

## <a name="creating-website-security-using-the-starter-site-template"></a>Tworzenie zabezpieczeń witryny sieci Web za pomocą szablonu witryny Starter

W programie WebMatrix, można użyć **witryny początkowej** szablonu w celu utworzenia witryny sieci Web, która zawiera następujące czynności:

- Bazy danych, który jest używany do przechowywania nazwy użytkownika i hasła na potrzeby członków.
- Strona rejestracji, gdzie można zarejestrować użytkowników anonimowych (nowy).
- Strona logowania i wylogowania.
- Strona odzyskiwania i resetowania hasła.

Poniższa procedura opisuje sposób tworzenia lokacji i skonfigurować go.

1. Uruchom program WebMatrix i **— Szybki Start** wybierz opcję **lokacji za pomocą szablonu**.
2. Wybierz **witryny początkowej** szablonu, a następnie kliknij przycisk **OK**. Program WebMatrix do tworzenia nowej witryny.
3. W okienku po lewej stronie kliknij **pliki** selektor obszaru roboczego.
4. W folderze głównym witryny sieci Web otwórz  *\_AppStart.cshtml* pliku, który to plik specjalny, który zawiera ustawienia globalne. Zawiera on niektóre instrukcje, które są oznaczone jako komentarz przy użyciu `//` znaków:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Te instrukcje konfigurowania `WebMail` pomocnika, która może służyć do wysyłania wiadomości e-mail. System członkostwa można wysyłać potwierdzenia kiedy użytkownik rejestruje, lub jeśli chcą zmienić swoje hasła za pomocą poczty e-mail. (Na przykład po użytkownicy rejestrować, otrzymają one wiadomość e-mail zawierającą łącze, które mogą kliknąć w celu zakończenia procesu rejestracji.)

    Wysyłanie wiadomości e-mail wymaga dostępu do serwera SMTP, zgodnie z opisem w [dodawanie do poczty E-mail witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202899). Ustawienia poczty e-mail będą przechowywane w tym środkowe  *\_AppStart.cshtml* plików, dzięki czemu nie trzeba je wielokrotnie kodu na każdej stronie, którą można wysłać wiadomości e-mail. (Nie trzeba skonfigurować ustawienia protokołu SMTP, aby skonfigurować bazę danych rejestracji; dzięki temu wystarczy ustawień protokołu SMTP Jeśli chcesz zweryfikować użytkowników z ich aliasu adresu e-mail i umożliwić użytkownikom Resetowanie zapomnianego hasła).
5. Usuń znaczniki komentarza oświadczeń, usuwając `//` from in front of każdej z nich.

    Jeśli chcesz skonfigurować potwierdzenie adresu e-mail, możesz pominąć ten krok i następnego kroku. Jeśli nie ustawiono wartości SMTP, nowe konto jest natychmiast dostępna, bez wiadomość e-mail z potwierdzeniem.
6. Zmodyfikuj następujące ustawienia związanych z pocztą e-mail w kodzie:

   - Ustaw `WebMail.SmtpServer` na nazwę serwera SMTP, który ma dostęp do.
   - Pozostaw `WebMail.EnableSsl` równa `true`. To ustawienie zabezpiecza poświadczenia, które są wysyłane do serwera SMTP, szyfrując je.
   - Ustaw `WebMail.UserName` nazwę użytkownika dla konta serwera SMTP.
   - Ustaw `WebMail.Password` hasło dla konta serwera SMTP.
   - Ustaw `WebMail.From` na adres e-mail. To jest adres e-mail, który komunikat jest wysyłany z.

     > [!NOTE] 
     > 
     > **Porada** uzyskać dodatkowe informacje na temat wartości tych właściwości, zobacz [konfigurowania ustawień poczty E-mail](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) w [Dostosowywanie zachowania dla całej witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Zapisz i Zamknij  *\_AppStart.cshtml*.
8. Uruchom *Default.cshtml* strony w przeglądarce.

    ![2-Security członkostwa](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Jeśli zostanie wyświetlony błąd informujący o tym, że właściwość musi być wystąpieniem `ExtendedMembershipProvider`, witryna może nie być skonfigurowany do używania systemu członkostwa ASP.NET Web Pages (SimpleMembership). Czasami może to wystąpić, jeśli serwer dostawcy hostingu skonfigurowano inaczej niż na serwerze lokalnym. Aby rozwiązać ten problem, Dodaj następujący element w witrynie *Web.config* pliku:
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Dodaj ten element jako element podrzędny elementu `<configuration>` elementu i elementem równorzędnym `<system.web>` elementu.
9. W prawym górnym rogu strony kliknij **zarejestrować** łącza. *Register.cshtml* zostanie wyświetlona strona.
10. Wprowadź nazwę użytkownika i hasło, a następnie kliknij przycisk **zarejestrować**.

    ![3-Security członkostwa](16-adding-security-and-membership/_static/image2.png)

    Podczas tworzenia witryny sieci Web z **witryny początkowej** szablonu, bazę danych o nazwie *StarterSite.sdf* został utworzony w tej witrynie *aplikacji\_danych* folderu. Podczas rejestracji informacje o użytkowniku jest dodawany do bazy danych. Jeśli ustawisz wartości SMTP, komunikat jest wysyłane na adres e-mail używany, dzięki czemu możesz ukończyć rejestracji.

    ![security-membership-4](16-adding-security-and-membership/_static/image3.png)
11. Przejdź do programu poczty e-mail i Znajdź komunikat mający Twój kod potwierdzenia i hiperłącza do lokacji.
12. Kliknij hiperlink, aby aktywować konto. Hiperłącze potwierdzenie zostanie otwarta strona potwierdzenia rejestracji.

    ![5-Security członkostwa](16-adding-security-and-membership/_static/image4.png)
13. Kliknij przycisk **logowania** połączyć, a następnie zaloguj się przy użyciu konta, która została zarejestrowana.

      Po zalogowaniu, **logowania** i **zarejestrować** łącza są zastępowane przez **wylogowania** łącza. Twoja nazwa logowania jest wyświetlany jako link. (Ten link pozwala przejść do strony, w którym można zmienić hasła.)

      ![zabezpieczenia członkostwa 6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > Domyślnie strony sieci web ASP.NET wysyłania poświadczeń do serwera w postaci zwykłego tekstu (jako tekst czytelny dla człowieka). Skorzystaj z witryny produkcyjnej bezpiecznego protokołu HTTP (https://, nazywana również *protokołu secure sockets layer* lub SSL) do szyfrowania poufnych informacji, które są wymieniane z serwerem. Możesz wymagany adres e-mail komunikaty wysyłane przy użyciu protokołu SSL, ustawiając `WebMail.EnableSsl=true` co w poprzednim przykładzie. Aby uzyskać więcej informacji na temat protokołu SSL, zobacz [zabezpieczania komunikacji w sieci Web: certyfikaty SSL i https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Funkcje dodatkowe członkostwa w lokacji

Witryna zawiera inne funkcje, która umożliwia zarządzanie kontami użytkowników. Użytkownicy mogą wykonywać następujące czynności:

- Zmienić swoje hasła. Po zalogowaniu po kliknięciu nazwy użytkownika (czyli link). Kliknięcie tej pozycji spowoduje ich strony gdzie można utworzyć nowe hasło (*Account/ChangePassword.cshtml*).
- Odzyskać zapomniane hasło. Na stronie logowania znajduje się link (**czy pamiętasz hasła?**), przejście do strony (*Account/ForgotPassword.cshtml*) mogą wprowadzać adres e-mail. Witryna wysyła do nich wiadomości e-mail z linkiem, który można kliknąć, aby ustawić nowe hasło (*Account/PasswordReset.cshtml*).

Można także pozwolić użytkownikom może również logowanie się za pomocą witryny zewnętrznej, jak opisano w dalszej części.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Tworzenie strony tylko do elementów członkowskich

W chwili obecnej, każda osoba można przeglądać na dowolnej stronie w witrynie sieci Web. Ale możesz chcieć stron, które są dostępne tylko dla osób, którzy logowali się (oznacza to, do elementów członkowskich). ASP.NET umożliwia tworzenie stron, które są dostępne jedynie dla członków zalogowany. Zwykle jeśli użytkownicy anonimowi próbuje uzyskać dostęp do strony tylko do elementów członkowskich, należy przekierowywać je do strony logowania.

W tej procedurze utworzysz folder, który będzie zawierać stron, które są dostępne tylko dla zalogowanych użytkowników.

1. W katalogu głównym witryny należy utworzyć nowy folder. (Na wstążce kliknij strzałkę znajdującą się poniżej **New** , a następnie wybierz **nowy Folder**.)
2. Nadaj nazwę nowego folderu *członków*.
3. Wewnątrz *członków* folderu, Utwórz nową stronę i nazwał ją *MembersInformation.cshtml*.
4. Zastąp istniejącą zawartość następujących kodu i znaczników:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Ten kod sprawdza `IsAuthenticated` właściwość `WebSecurity` obiektu, który zwraca `true` Jeśli użytkownik jest zalogowany. Jeśli użytkownik nie zalogował się w wywołaniach kodu `Response.Redirect` do wysłania użytkownikowi *Login.cshtml* strony w *konta* folderu.

    Adres URL przekierowania zawiera `returnUrl` zapytania wartość ciągu, który używa `Request.Url.LocalPath` można ustawić ścieżki bieżącej strony. Jeśli ustawisz `returnUrl` wartości ciągu zapytania, tak i jeśli zwrotny adres URL jest ścieżką lokalną, strony logowania zostanie zwrócona użytkowników do tej strony, po zalogowaniu.

    Ten kod ustawia również  *\_SiteLayout.cshtml* strony jako stronę jego układu. (Aby uzyskać więcej informacji dotyczących układu stron, zobacz [tworzenie spójnego układu w witrynach stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Uruchamiania witryny. Jeśli nadal zalogowano, kliknij przycisk **wylogowania** znajdujący się u góry strony.
6. W przeglądarce, żądanie strony */składowe/MembersInformation*. Na przykład adres URL może wyglądać następująco:

    `http://localhost:38366/Members/MembersInformation`

    (Numer portu (38366) prawdopodobnie będą inne w przypadku adresu URL).

    Są przekierowywane do *Login.cshtml* strony, ponieważ nie są rejestrowane w.
7. Zaloguj się przy użyciu konta, która została utworzona wcześniej. Możesz teraz przekierowana z powrotem do *MembersInformation* strony. Ponieważ zalogowano, tym razem zobaczysz zawartość strony.

Aby zabezpieczyć dostęp do wielu stron, można to zrobić:

- Dodaj sprawdzanie zabezpieczeń do każdej strony.
- Tworzenie  *\_PageStart.cshtml* strony w folderze, w której Zachowaj chronione stron, a następnie dodaj podczas sprawdzania zabezpieczeń. *\_PageStart.cshtml* strony pełni rolę globalnego strony dla wszystkich stron w folderze. Ta technika jest omówione bardziej szczegółowo w [Dostosowywanie zachowania dla całej witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Tworzenie zabezpieczeń dla grup użytkowników (role)

Jeśli witryna ma wiele elementów członkowskich, nie jest skuteczny w indywidualnie sprawdzenie uprawnień dla poszczególnych użytkowników, przed informacją zostanie wyświetlona strona. Co można zrobić zamiast tego jest utworzenie grupy, lub *role*, że poszczególne składniki należą do. Następnie można sprawdzić uprawnień na podstawie roli. W tej sekcji utworzysz &quot;administratora&quot; roli, a następnie utworzyć stronę, która jest dostępna dla użytkowników, którzy są w (należącymi do) tej roli.

System członkostwa ASP.NET jest skonfigurowany do obsługi ról. Jednak w przeciwieństwie do członkostwa rejestracji i logowania **witryny początkowej** szablon nie zawiera strony, które ułatwiają zarządzanie rolami. (Zarządzanie rolami jest zadaniem administracyjnym, a nie zadania użytkownika). Jednak można dodać grupy bezpośrednio w bazie danych członkostwa w programie WebMatrix.

1. W programie WebMatrix, kliknij przycisk **baz danych** selektor obszaru roboczego.
2. W okienku po lewej stronie, otwórz *StarterSite.sdf* otworzyć węzła **tabel** węzłem, a następnie kliknij dwukrotnie plik *stron sieci Web\_role* tabeli.

    ![7-Security członkostwa](16-adding-security-and-membership/_static/image6.png)
3. Dodaj rolę o nazwie &quot;administratora&quot;. *RoleId* pole jest wypełniane automatycznie. (Jest to klucz podstawowy i została ustawiona na pole identyfikowanie zgodnie z objaśnieniem w [wprowadzenie do pracy z bazą danych w witrynach stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Zwróć uwagę na wartość jest dla *RoleId* pola. (Jeśli jest to pierwsza rola, którą definiujesz, będzie 1.)

    ![8-Security członkostwa](16-adding-security-and-membership/_static/image7.png)
5. Zamknij *stron sieci Web\_role* tabeli.
6. Otwórz *UserProfile* tabeli.
7. Zwróć uwagę na *UserId* wartość jednego lub większej liczby użytkowników w tabeli, a następnie zamknij tabeli.
8. Otwórz *stron sieci Web\_UserInRoles* tabeli, a następnie wprowadź *UserID* i *RoleID* wartości w tabeli. Na przykład, aby umieścić użytkownika od 2 do &quot;administratora&quot; roli, należy wprowadzić następujące wartości:

    ![Security członkostwa w-9](16-adding-security-and-membership/_static/image8.png)
9. Zamknij *stron sieci Web\_UsersInRoles* tabeli.

    Teraz, gdy masz zdefiniowanych ról, można skonfigurować stronę, która jest dostępna dla użytkowników, którzy są w tej roli.
10. W folderze głównym witryny sieci Web, Utwórz nową stronę o nazwie *AdminError.cshtml* i zastąpić istniejącą zawartość następującym kodem. Będzie to strona która użytkownicy są przekierowywani do, jeśli nie mogą uzyskać dostępu do strony.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. W folderze głównym witryny sieci Web, Utwórz nową stronę o nazwie *AdminOnly.cshtml* i Zastąp istniejący kod następującym kodem:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    `Roles.IsUserInRole` Metoda zwraca `true` Jeśli bieżący użytkownik jest członkiem określonej roli (w tym przypadku rolę "Administrator").
12. Uruchom *Default.cshtml* w przeglądarce, ale nie zalogować. (Jeśli już zalogowano, Wyloguj.)
13. W pasku adresu przeglądarki Dodaj *AdminOnly* w adresie URL. (Innymi słowy żądanie *AdminOnly.cshtml* pliku.) Są przekierowywane do *AdminError.cshtml* strony, ponieważ nie są aktualnie zalogowany jako użytkownik w &quot;administratora&quot; roli.
14. Wróć do *Default.cshtml* i zaloguj się jako użytkownik dodany do &quot;administratora&quot; roli.
15. Przejdź do *AdminOnly.cshtml* strony. Tym razem zostanie wyświetlona strona.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Uniemożliwia automatyczne programy dołączenie do swojej witryny sieci Web

Na stronie logowania nie spowoduje zatrzymania automatycznych programów (czasami określane jako *web robotów* lub *botów*) z rejestrowanie w usłudze witryny sieci Web. Ta procedura opisuje sposób włączania test ReCaptcha na stronie rejestracji.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Zarejestruj swoje witryny sieci Web ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Po zakończeniu rejestracji zostanie wyświetlony klucz publiczny i klucz prywatny.
2. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze go.
3. W *konta* folder, otwórz plik o nazwie *Register.cshtml*.
4. W kodzie w górnej części strony, Znajdź następujące wiersze i usuń znaczniki komentarza je, usuwając `//` znaki komentarza:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Zastąp `PRIVATE_KEY` przy użyciu własnego klucza prywatnego ReCaptcha.
6. W znaczniku strony, należy usunąć `@*` i `*@` komentowania znaków z całym następujące wiersze w znaczniku strony:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Zastąp `PUBLIC_KEY` kluczem.
8. Jeśli jeszcze nie usunięto jej już, usunąć `<div>` element, który zawiera tekst, który rozpoczyna się od "Aby włączyć weryfikację CAPTCHA...". (Usuń całą `<div>` elementu i jego zawartość.)

9. Uruchom *Default.cshtml* w przeglądarce. Jeśli logujesz się do witryny, kliknij przycisk **wylogowania** łącza.
10. Kliknij przycisk **zarejestrować** link i przetestować rejestracji za pomocą testu CAPTCHA.

     ![10-Security członkostwa](16-adding-security-and-membership/_static/image9.png)

Aby uzyskać więcej informacji na temat `ReCaptcha` pomocnika, zobacz [przy użyciu CATPCHA uniemożliwić automatyczne programom (robotom) z przy użyciu Twojej witrynie internetowej ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Umożliwienie użytkownikom zalogowanie się przy użyciu witryny zewnętrznej

**Witryny początkowej** szablon zawiera kod i kod znaczników, który umożliwia użytkownikom zalogowanie się przy użyciu usługi Facebook, Windows Live, Twitter, Google lub Yahoo. Ta funkcja nie jest włączona domyślnie. Ogólna procedura używania, dzięki czemu użytkownicy logowanie się za pomocą tych dostawców zewnętrznych to:

- Zdecyduj, które mają być obsługiwane z zewnętrznych witryn.
- Jeśli jest to wymagane, przejdź do tej lokacji i skonfigurować aplikację logowania. (Na przykład, musisz to zrobić w celu umożliwienia logowania do usługi Facebook.)
- W lokacji należy skonfigurować dostawcę. W większości przypadków wystarczy usuń znaczniki komentarza jakiś kod w  *\_AppStart.cshtml* pliku.
- Dodawanie znaczników do strony rejestracji, umożliwiający użytkownikom link do witryny zewnętrznej w celu logowania. Zazwyczaj można skopiować znaczników wymagane i nieco zmienić tekst.

Instrukcje krok po kroku można znaleźć w temacie [Włączanie logowania z zewnętrznych witryn w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969).

Po zalogowaniu się użytkownika z innej lokacji użytkownik powraca do swojej witryny i *kojarzy* tego zaloguj się przy użyciu witryny. W efekcie to tworzy wpis członkostwa w witrynie dla użytkownika logowania zewnętrznego. Pozwala to normalne Australijską członkostwa (na przykład role) za pomocą logowania zewnętrznego.

## <a name="adding-security-to-an-existing-website"></a>Dodawanie zabezpieczeń do istniejącej witryny sieci Web

Procedury w tym artykule opiera się na użyciu **witryny początkowej** szablon jako podstawy dla bezpieczeństwa witryny sieci Web. Jeśli nie jest przydatne rozpocząć od **witryny początkowej** szablonu lub aby skopiować na odpowiednich stronach witryny, na podstawie tego szablonu, można zaimplementować ten sam typ zabezpieczeń do własnej witryny, tworząc je samodzielnie. Utwórz te same typy stron — rejestracji, logowania i tak dalej —, a następnie użyć do konfigurowania członkostwa pomocników i klas.

Podstawowy proces jest opisany wpis w blogu [najbardziej podstawową metodą, aby zaimplementować zabezpieczenia ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). Większość pracy odbywa się przy użyciu następujących metod i właściwości `WebSecurity` pomocy:

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Te metody pozwalają ustalić, czy ktoś jest już zarejestrowane i można je zarejestrować.
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Ta właściwość umożliwia określenie, czy bieżący użytkownik jest zalogowany. Dzięki takiemu grupowaniu można przekierować użytkowników do strony logowania, jeśli ich nie ma już zalogowany.
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Te metody logowania użytkownika wewnątrz lub na zewnątrz.
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Ta właściwość jest przydatny do wyświetlania nazwę bieżącego użytkownika zalogowanego (Jeśli użytkownik jest zalogowany).
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Ta metoda jest przydatna, jeśli skonfigurowano e-mail z potwierdzeniem rejestracji. (Szczegóły zostały opisane w wpis w blogu [przy użyciu funkcji potwierdzenia zabezpieczeń stron ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Aby zarządzać rolami, można użyć [role](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) i [członkostwa](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) klasy, zgodnie z opisem w wpis w blogu.

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Dostosowywanie zachowania dla całej witryny](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Zabezpieczanie komunikacji w sieci Web: Certyfikaty SSL i https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [Najbardziej podstawową metodą, aby zaimplementować zabezpieczenia ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) i [przy użyciu funkcji potwierdzenia zabezpieczeń stron ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Są to wpisy w blogu, które opisują sposób implementowania członkostwa programu ASP.NET bez używania **witryny początkowej** szablonu.
- [Włączanie logowania z zewnętrznych witryn w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969)
- [Dokumentacja interfejsu API klasy WebSecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [Dokumentacja interfejsu API klasy SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [Dokumentacja interfejsu API klasy SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
