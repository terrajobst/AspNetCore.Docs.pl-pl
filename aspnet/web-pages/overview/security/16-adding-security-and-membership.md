---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Dodawanie zabezpieczeń i członkostwo w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale pokazuje, jak zabezpieczyć witryny sieci Web, dzięki czemu niektóre strony są dostępne tylko dla osób Zaloguj się. (Widoczne jest również sposób tworzenia stron określona...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/24/2014
ms.topic: article
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 351368a356a71e85d4abfdceac8d4f84e0b217f4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Dodawanie zabezpieczeń i członkostwo w witrynie sieci Web ASP.NET stron (Razor)
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano, jak zabezpieczyć witryny sieci Web platformy ASP.NET Web Pages (Razor) tak, aby niektóre strony są dostępne tylko dla osób, które Zaloguj się. (Również zobaczysz tworzenie stron, które każdy użytkownik może uzyskać dostęp).
> 
> **Zawartość:** 
> 
> - Jak utworzyć witryny sieci Web zawierającej strony rejestracji i strony logowania, dzięki czemu w przypadku niektórych stron można ograniczyć dostęp tylko elementów członkowskich.
> - Jak utworzyć stron publicznego i tylko do elementu członkowskiego.
> - Definiowanie ról, które grupy, które mają uprawnienia zabezpieczeń w swojej witrynie, jak i sposobu przypisywania użytkowników do roli.
> - Jak używać CAPTCHA, aby zapobiec próbom (robotów) tworzenia kont Członkowskich.
>   
> 
> Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzone w artykule:
> 
> - Program WebMatrix **witryny początkowej** szablonu.
> - `WebSecurity` Pomocnika i `Roles` klasy.
> - `ReCaptcha` Pomocnika.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Program WebMatrix 3
> - Biblioteka pomocników sieci Web ASP.NET


Można skonfigurować witryny sieci Web, dzięki czemu użytkownicy mogą logować się do niego &#8212; oznacza to, że lokacja obsługuje *członkostwa*. Może to być przydatne dla wielu powodów. Na przykład witryna może być stron, które powinny być dostępne tylko do elementów członkowskich. W niektórych przypadkach może wymagać użytkowników zalogować się, aby wysłać opinię lub zostaw komentarz.

Nawet w przypadku witryny sieci Web obsługuje członkostwo, użytkownicy nie są wymagane musi się zalogować, aby korzystać z niektórych stron w witrynie. Użytkownicy, którzy nie są rejestrowane w są określane jako *użytkowników anonimowych*.

Użytkownika można zarejestrować w witrynie sieci Web, a następnie zalogować się do witryny. Witryna sieci Web wymaga nazwy użytkownika (adres e-mail) i hasło, aby upewnić się, że użytkownicy są kogo się. Ten proces logowania i potwierdzenia tożsamości użytkownika jest nazywany *uwierzytelniania*.

Można ustawić zabezpieczeń i członkostwo na różne sposoby:

- Jeśli używasz programu WebMatrix, łatwo jest utworzenie jako nowej lokacji na podstawie **witryny początkowej** szablonu. Ten szablon został już skonfigurowany dla zabezpieczeń i członkostwa i ma już strony rejestracji, strony logowania i tak dalej.

    Lokacji utworzonej przez szablon ma również opcję, aby zezwolić użytkownikom na logowanie przy użyciu witryny zewnętrznej, takich jak Facebook, Google, lub w usłudze Twitter.
- Jeśli chcesz dodać do istniejącej lokacji zabezpieczeń lub jeśli nie chcesz używać **witryny początkowej** szablonu, można utworzyć własne strony rejestracji, strony logowania i tak dalej.

Ten artykuł skupia się na pierwszej opcji &mdash; sposób dodawania zabezpieczeń przy użyciu **witryny początkowej** szablonu. Dostępne są także niektóre podstawowe informacje dotyczące sposobu implementacji zabezpieczeń własnych, a następnie zawiera łącza do dodatkowych informacji o tym, jak to zrobić. Brak informacji o sposobie włączania logowań zewnętrznych, który jest opisany bardziej szczegółowo w oddzielnym artykule.

## <a name="creating-website-security-using-the-starter-site-template"></a>Tworzenie przy użyciu szablonu witryny Starter bezpieczeństwa witryny sieci Web

W programie WebMatrix, można użyć **witryny początkowej** szablonu w celu utworzenia witryny sieci Web, który zawiera następujące:

- Baza danych, który jest używany do przechowywania nazwy użytkownika i hasła dla wszystkich członków.
- Strony rejestracji, w którym można zarejestrować użytkowników anonimowych (nowy).
- Strona logowania i wylogowywania.
- Strona odzyskiwania i resetowania hasła.

W poniższej procedurze opisano sposób tworzenia lokacji i skonfigurować go.

1. Uruchom program WebMatrix i **Szybki Start** wybierz pozycję **szablonu z witryny**.
2. Wybierz **witryny początkowej** szablonu, a następnie kliknij przycisk **OK**. Program WebMatrix tworzy nową lokację.
3. W okienku po lewej stronie kliknij **pliki** selektora obszaru roboczego.
4. W folderze głównym witryny sieci Web otwórz  *\_AppStart.cshtml* pliku, który to plik specjalny, który zawiera ustawienia globalne. Zawiera niektóre instrukcji, które są oznaczone jako komentarz przy użyciu `//` znaków:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Te instrukcje konfigurowania `WebMail` pomocnika, która może służyć do wysyłania wiadomości e-mail. System członkostwa może wysyłać potwierdzenie po rejestracji użytkowników, lub gdy trzeba zmienić swoje hasła za pomocą poczty e-mail. (Na przykład zarejestrować użytkowników, one otrzymywać wiadomość e-mail zawierającą łącze, które można kliknąć, aby zakończyć proces rejestracji.)

    Wysyłanie poczty e-mail wymaga dostępu do serwera SMTP, zgodnie z opisem w [dodawanie wiadomości E-mail do witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202899). Ustawienia poczty e-mail będą przechowywane w tym centralnego  *\_AppStart.cshtml* plików, dzięki czemu nie trzeba kodu je wielokrotnie do każdej strony, który może wysyłać wiadomości e-mail. (Nie trzeba skonfigurować ustawienia SMTP do konfigurowania bazy danych rejestracji; wystarczy tylko ustawienia SMTP, jeśli chcesz zweryfikować użytkowników z ich alias e-mail, pozwalając użytkownikom na zresetować zapomniane hasło).
5. Usuń znaczniki komentarza instrukcje przez usunięcie `//` from in front of każdej z nich.

    Jeśli nie chcesz skonfigurować wiadomości e-mail z potwierdzeniem, możesz pominąć ten krok i następnego kroku. Jeśli nie ustawiono wartości SMTP, nowe konto jest natychmiast dostępna bez wiadomość e-mail z potwierdzeniem.
6. Modyfikowanie następujących ustawień związanych z pocztą e-mail w kodzie:

   - Ustaw `WebMail.SmtpServer` na nazwę serwera SMTP, który ma dostęp do.
   - Pozostaw `WebMail.EnableSsl` ustawioną `true`. To ustawienie zabezpiecza poświadczenia, które są wysyłane do serwera SMTP przez ich szyfrowanie.
   - Ustaw `WebMail.UserName` do nazwy użytkownika dla konta serwera SMTP.
   - Ustaw `WebMail.Password` hasło dla konta serwera SMTP.
   - Ustaw `WebMail.From` na adres e-mail. Jest to komunikat jest wysyłany z adres e-mail.

     > [!NOTE] 
     > 
     > **Porada** dodatkowe informacje o wartości tych właściwości, zobacz [Konfigurowanie ustawień poczty E-mail](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) w [Dostosowywanie zachowanie całej witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Zapisz i Zamknij  *\_AppStart.cshtml*.
8. Uruchom *Default.cshtml* strony w przeglądarce.

    ![zabezpieczenia członkostwa 2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Jeśli zostanie wyświetlony błąd informujący o tym, czy właściwość musi być wystąpieniem elementu `ExtendedMembershipProvider`, lokacji może nie być skonfigurowany do korzystania z systemu członkostwa ASP.NET Web Pages (SimpleMembership). Czasami może to występować, jeśli skonfigurowano serwer dostawcy hostingu inaczej niż na serwerze lokalnym. Aby rozwiązać ten problem, Dodaj następujący element w witrynie *Web.config* pliku:
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Dodaj ten element jako element podrzędny `<configuration>` element i jako element równorzędny elementu `<system.web>` elementu.
9. W prawym górnym rogu strony kliknij **zarejestrować** łącza. *Register.cshtml* zostanie wyświetlona strona.
10. Wprowadź nazwę użytkownika i hasło, a następnie kliknij przycisk **zarejestrować**.

    ![zabezpieczenia członkostwa 3](16-adding-security-and-membership/_static/image2.png)

    Podczas tworzenia witryny sieci Web z **witryny początkowej** szablonu, bazy danych o nazwie *StarterSite.sdf* został utworzony w tej witrynie *aplikacji\_danych* folderu. Podczas rejestracji informacje o użytkowniku jest dodane do bazy danych. Po ustawieniu wartości SMTP jest wysyłany komunikat na adres e-mail podany, możesz ukończyć rejestracji.

    ![security-membership-4](16-adding-security-and-membership/_static/image3.png)
11. Przejdź do programu poczty e-mail i Znajdź komunikat, który ma Twój kod potwierdzenia i hiperłącza do lokacji.
12. Kliknij hiperłącze, aby aktywować konto. Hyperlink potwierdzenie otwiera stronę potwierdzenia rejestracji.

    ![security-membership-5](16-adding-security-and-membership/_static/image4.png)
13. Kliknij przycisk **logowania** połączyć, a następnie zaloguj się za pomocą konta, który został zarejestrowany.

      Po zalogowaniu, **logowania** i **zarejestrować** łącza są zastępowane przez **wylogowania** łącza. Nazwy logowania jest wyświetlany jako łącze. (Ten link pozwala przejść do strony, w którym można zmienić swoje hasło.)

      ![security-membership-6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > Domyślnie strony sieci web ASP.NET wysłać poświadczeń do serwera w postaci zwykłego tekstu (jako tekst zrozumiałą dla użytkownika). Witryny produkcyjnym należy używać bezpiecznego protokołu HTTP (https://, nazywany również *protokołu secure sockets layer* lub SSL) do szyfrowania poufnych informacji, które są wymieniane z serwerem. Można wymagane e-mail komunikatów do wysłania przy użyciu protokołu SSL, ustawiając `WebMail.EnableSsl=true` co w poprzednim przykładzie. Aby uzyskać więcej informacji na temat protokołu SSL, zobacz [zabezpieczania komunikacji w sieci Web: certyfikatów SSL i https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Funkcje dodatkowe członkostwa w lokacji

Witryna zawiera inne funkcje, które pozwala użytkownikom na zarządzanie swoje konta. Użytkownicy mogą wykonywać następujące czynności:

- Zmiany haseł. Po zalogowaniu one kliknij nazwę użytkownika (czyli link). Trwa to ich do strony gdzie można utworzyć nowego hasła (*Account/ChangePassword.cshtml*).
- Odzyskać zapomniane hasło. Na stronie logowania jest łączem (**pamiętasz hasła?**) która powoduje przejście do strony (*Account/ForgotPassword.cshtml*) mogą wprowadzać adresu e-mail. Lokacji wysyła je wiadomość e-mail zawierającą łącze, które można kliknąć, aby ustawić nowe hasło (*Account/PasswordReset.cshtml*).

Można także pozwolić użytkownikom może również logowanie się za pomocą witryny zewnętrznej, jak wyjaśniono dalej.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Tworzenie strony tylko do elementów członkowskich

W chwili obecnej, każda osoba, która przejdź do dowolnej strony w witrynie sieci Web. Ale warto mieć stron, które są dostępne tylko dla osób, którzy zalogowali się (to znaczy do elementów członkowskich). ASP.NET umożliwia tworzenie stron, które są dostępne tylko przez członków zalogowany. Zazwyczaj użytkowników anonimowych próby dostępu do strony tylko do elementów członkowskich, należy przekierowanie do strony logowania.

W tej procedurze utworzysz folder, który będzie zawierać stron, które są dostępne tylko do zalogowanych użytkowników.

1. W katalogu głównym witryny Utwórz nowy folder. (Na wstążce kliknij strzałkę poniżej **nowy** , a następnie wybierz **nowy Folder**.)
2. Nazwa nowego folderu *członków*.
3. Wewnątrz *członków* , Utwórz nową stronę i o nazwie go *MembersInformation.cshtml*.
4. Zastąp następujący kod i znaczników istniejącej zawartości:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Ten kod testów `IsAuthenticated` właściwość `WebSecurity` obiektu, który zwraca `true` Jeśli użytkownik jest zalogowany. Jeśli użytkownik nie jest zalogowany, kod wywołuje `Response.Redirect` można wysłać użytkownikowi *Login.cshtml* strony *konta* folderu.

    Zawiera adres URL przekierowania `returnUrl` zapytania wartość ciągu, która używa `Request.Url.LocalPath` można ustawić ścieżkę do bieżącej strony. Jeśli ustawisz `returnUrl` wartości w ciągu zapytania następująco (i jeśli zwrotny adres URL jest ścieżka lokalna), strony logowania zostanie zwrócona użytkowników do tej strony, po zalogowaniu.

    Ustawia również kod  *\_SiteLayout.cshtml* strony jako jego układ strony. (Aby uzyskać więcej informacji o strony układu, zobacz [tworzenie spójnego układu w witrynach stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Uruchamiania witryny. Jeśli nadal zalogowano, kliknij przycisk **wylogowania** u góry strony.
6. W przeglądarce, żądanie strony */członkowie/MembersInformation*. Na przykład adres URL może wyglądać następująco:

    `http://localhost:38366/Members/MembersInformation`

    (Numer portu (38366) prawdopodobnie będą różne w adresie URL).

    Są przekierowywane do *Login.cshtml* strony, ponieważ nie jest zalogowany.
7. Zaloguj się za pomocą utworzonego wcześniej konta. W przypadku przekierowany do *MembersInformation* strony. Ponieważ użytkownik jest zalogowany, teraz możesz zobaczyć strony zawartości.

Aby zabezpieczyć dostęp do wielu stron, można to zrobić:

- Kontrola zabezpieczeń można dodać do każdej strony.
- Utwórz  *\_PageStart.cshtml* strony w folderze, w której Zachowaj chronione stron, a następnie dodaj podczas sprawdzania zabezpieczeń.  *\_PageStart.cshtml* strony pełni rolę rodzaju strony globalne dla wszystkich stron w folderze. Ta technika jest co omówiono bardziej szczegółowo w [Dostosowywanie zachowanie całej witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Tworzenie zabezpieczeń dla grup użytkowników (role)

Jeśli witryna ma wiele elementów członkowskich, nie jest efektywne do sprawdzania uprawnień dla każdego użytkownika indywidualnie, aby umożliwić strona. Co można zrobić, zamiast tego jest tworzenie grup lub *ról*, że poszczególne elementy należą do. Następnie można sprawdzić uprawnień na podstawie roli. W tej sekcji utworzysz &quot;admin&quot; roli, a następnie utworzyć strony, który jest dostępny dla użytkowników, którzy są w (należącymi do) tej roli.

System członkostwa ASP.NET jest skonfigurowany do obsługi ról. Jednak w przeciwieństwie do członkostwa rejestracji i logowania **witryny początkowej** szablon nie zawiera strony, które ułatwiają zarządzanie rolami. (Zarządzanie rolami jest zadań administracyjnych, a nie zadania użytkownika). Można jednak dodać grupy, bezpośrednio w bazie danych członkostwa w programie WebMatrix.

1. W programie WebMatrix, kliknij przycisk **baz danych** selektora obszaru roboczego.
2. W okienku po lewej stronie, otwórz *StarterSite.sdf* otworzyć węzła **tabel** węzeł, a następnie kliknij dwukrotnie plik *stron sieci Web\_ról* tabeli.

    ![security-membership-7](16-adding-security-and-membership/_static/image6.png)
3. Dodaj rolę o nazwie &quot;admin&quot;. *RoleId* pole jest wypełniane automatycznie. (Jest to klucz podstawowy i została ustawiona jako pola określ zgodnie z objaśnieniem w [wprowadzenie do pracy z bazą danych w witrynach stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Zanotuj wartość jest za *RoleId* pola. (Jeśli jest to pierwszy rolę, którą definiujesz, zostanie 1.)

    ![security-membership-8](16-adding-security-and-membership/_static/image7.png)
5. Zamknij *stron sieci Web\_ról* tabeli.
6. Otwórz *UserProfile* tabeli.
7. Zanotuj *UserId* wartość jednego lub więcej użytkowników w tabeli, a następnie zamknij tabelę.
8. Otwórz *stron sieci Web\_UserInRoles* tabeli, a następnie wprowadź *UserID* i *RoleID* wartości do tabeli. Na przykład, aby umieścić użytkownika 2 do &quot;admin&quot; roli, należy wprowadzić następujące wartości:

    ![security-membership-9](16-adding-security-and-membership/_static/image8.png)
9. Zamknij *stron sieci Web\_UsersInRoles* tabeli.

    Teraz, gdy masz zdefiniowanych ról można skonfigurować strony, który jest dostępny dla użytkowników, którzy są w tej roli.
10. W folderze głównym witryny sieci Web, Utwórz nową stronę o nazwie *AdminError.cshtml* i zastąpić istniejącą zawartość następującym kodem. Będzie to strona, do której użytkownicy są przekierowywani do, jeśli nie są one zezwolenie na dostęp do strony.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. W folderze głównym witryny sieci Web, Utwórz nową stronę o nazwie *AdminOnly.cshtml* i Zastąp istniejący kod następującym kodem:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    `Roles.IsUserInRole` Metoda zwraca `true` Jeśli bieżący użytkownik jest członkiem określonej roli (w tym przypadku roli "admin").
12. Uruchom *Default.cshtml* w przeglądarce, ale nie zalogować. (Jeśli użytkownik jest już zalogowany, wyloguj się.)
13. Na pasku adresu przeglądarki, należy dodać *AdminOnly* w adresie URL. (Innymi słowy żądanie *AdminOnly.cshtml* pliku.) Są przekierowywane do *AdminError.cshtml* strony, ponieważ nie jest obecnie zalogowany jako użytkownik w &quot;admin&quot; roli.
14. Wróć do *Default.cshtml* i zaloguj się jako użytkownik dodany do &quot;admin&quot; roli.
15. Przejdź do *AdminOnly.cshtml* strony. Tym razem pojawi się strona.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Uniemożliwia automatyczne programy dołączenie witryny sieci Web

Strona logowania nie spowoduje zatrzymania automatycznych programów (czasami określane jako *sieci web robotów* lub *robotów*) z rejestrowania za pomocą witryny sieci Web. Ta procedura opisuje sposób włączania ReCaptcha testu dla strony rejestracji.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Zarejestruj witryny sieci Web w ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Po zakończeniu rejestracji zostanie wyświetlony klucz publiczny i klucz prywatny.
2. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie jest jeszcze.
3. W *konta* folder, otwórz plik o nazwie *Register.cshtml*.
4. W kodzie w górnej części strony, Znajdź następujące wiersze i Usuń komentarz je przez usunięcie `//` komentarz znaków:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Zastąp `PRIVATE_KEY` z użyciem własnego klucza prywatnego ReCaptcha.
6. W znaczniku strony Usuń `@*` i `*@` komentowania znaków z wokół następujące wiersze w znaczniku strony:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Zastąp `PUBLIC_KEY` kluczem.
8. Jeśli jeszcze tego nie usunięto go już, Usuń `<div>` element zawierający tekst, który rozpoczyna się od "Aby włączyć weryfikację CAPTCHA...". (Usuń całą `<div>` elementu i jego zawartość.)

9. Uruchom *Default.cshtml* w przeglądarce. Jeśli logujesz się do witryny, kliknij przycisk **wylogowania** łącza.
10. Kliknij przycisk **zarejestrować** test rejestracji za pomocą testu CAPTCHA i łącza.

     ![security-membership-10](16-adding-security-and-membership/_static/image9.png)

Aby uzyskać więcej informacji na temat `ReCaptcha` pomocnika, zobacz [za pomocą CATPCHA uniemożliwiają automatyczne programy (robotów) z przy użyciu Your witryna sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Umożliwienie użytkownikom zalogowanie się przy użyciu witryny zewnętrznej

**Witryny początkowej** szablon zawiera kod i kod znaczników, który umożliwia użytkownikom zalogowanie się przy użyciu usługi Facebook, Windows Live, Twitter, Google lub Yahoo. Domyślnie ta funkcja nie jest włączona. Ogólna procedura przy użyciu, dzięki czemu użytkownicy logowanie się za pomocą tych zewnętrznych dostawców jest to:

- Należy określić, które witryny zewnętrzne mają być obsługiwane.
- Jeśli jest to wymagane, przejdź do tej lokacji i skonfigurować aplikację logowania. (Na przykład, należy to zrobić w celu umożliwienia logowania do usługi Facebook.)
- W witrynie należy skonfigurować dostawcę. W większości przypadków należy się Usuń komentarz kodu w  *\_AppStart.cshtml* pliku.
- Dodawanie znaczników do strony rejestracji, informując link do zewnętrznej witryny do logowania. Zazwyczaj można skopiować kod znaczników, należy go i nieznacznie zmieniać tekst.

Instrukcje krok po kroku można znaleźć w temacie [Włączanie logowania z zewnętrznych witryn w witrynie stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251969).

Po zalogowaniu się użytkownika z innej lokacji powrocie do witryny i *kojarzy* tego zaloguj się za pomocą witryny. W efekcie spowoduje to utworzenie wpisu członkostwa w witrynie dla użytkownika logowania zewnętrznego. Umożliwia to normalne urządzeń członkostwa (takich jak role) za pomocą logowania zewnętrznego.

## <a name="adding-security-to-an-existing-website"></a>Dodawanie zabezpieczeń do istniejącej już witryny internetowej

Procedury w tym artykule zależy od tego, za pomocą **witryny początkowej** szablonu jako podstawa dla bezpieczeństwa witryny sieci Web. Jeśli nie jest praktyczne można uruchomić z **witryny początkowej** szablonu lub aby skopiować do poszczególnych stron z lokacji, na podstawie tego szablonu, można wdrożyć ten sam typ zabezpieczeń w witrynie sieci przy kodowania samodzielnie. Utwórz te same rodzaje stron — rejestracji, logowania i tak dalej — a następnie użyć do konfigurowania członkostwa pomocników i klasy.

Podstawowy proces jest opisane w blogu [najbardziej podstawową metodą implementacji zabezpieczeń ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). Większość pracy odbywa się przy użyciu następujących metod i właściwości `WebSecurity` pomocy:

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Te metody pozwalają ustalić, czy ktoś jest już zarejestrowany i można je zarejestrować.
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Ta właściwość umożliwia określenie, czy bieżący użytkownik jest zalogowany. Jest to przydatne przekierować użytkowników do strony logowania, jeśli ich nie ma już zarejestrowane.
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Te metody logowania użytkownika przychodzący lub wychodzący.
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Ta właściwość jest przydatna do wyświetlania nazwy zalogowany bieżący użytkownik (Jeśli użytkownik jest zalogowany).
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Ta metoda jest przydatna, jeśli skonfigurowano wiadomości e-mail z potwierdzeniem rejestracji. (Szczegóły są opisane w blogu [za pomocą funkcji potwierdzenia zabezpieczeń stron ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Aby zarządzać rolami, można użyć [ról](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) i [członkostwa](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) klas, zgodnie z opisem w wpis w blogu.

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Dostosowywanie zachowania dla całej witryny](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Zabezpieczanie komunikacji w sieci Web: Certyfikatów SSL i https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [Najprostszym sposobem implementacji zabezpieczeń ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) i [za pomocą funkcji potwierdzenia zabezpieczeń stron ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Są to wpisy na blogu dotyczące sposobu implementowania funkcji członkostwa ASP.NET bez używania **witryny początkowej** szablonu.
- [Włączanie logowania z zewnętrznych witryn w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969)
- [Dokumentacja interfejsu API klasy WebSecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [Dokumentacja interfejsu API klasy SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [Dokumentacja interfejsu API klasy SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
