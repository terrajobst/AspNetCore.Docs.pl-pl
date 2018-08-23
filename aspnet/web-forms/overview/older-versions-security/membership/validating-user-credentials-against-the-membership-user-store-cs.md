---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: Sprawdzanie poprawności poświadczeń użytkownika względem Store użytkownika członkostwa (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku zostanie omówiony sposób sprawdzania poprawności poświadczeń użytkownika względem magazynu użytkowników członkostwa przy użyciu zarówno programowy sposób, jak i kontrolka Login...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: ac04ee84ad30015ec3dea272c2b143bb2dae25e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757175"
---
<a name="validating-user-credentials-against-the-membership-user-store-c"></a>Sprawdzanie poprawności poświadczeń użytkownika względem Store użytkownika członkostwa (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> W tym samouczku zostanie omówiony sposób sprawdzania poprawności poświadczeń użytkownika względem magazynu użytkowników członkostwa przy użyciu zarówno programowy sposób, jak i kontrolka Login. Również przyjrzymy się jak dostosować wygląd i zachowanie kontrolki logowania.


## <a name="introduction"></a>Wprowadzenie

W <a id="Tutorial05"> </a> [poprzedni Samouczek](creating-user-accounts-cs.md) zobaczyliśmy, jak utworzyć nowe konto użytkownika w ramach członkostwa. Najpierw przyjrzeliśmy się programowe tworzenie kont użytkowników za pośrednictwem `Membership` klasy `CreateUser` metody, a następnie za pomocą kontrolki CreateUserWizard sieci Web. Na stronie logowania weryfikuje obecnie podane poświadczenia z ustaloną listą par nazwa użytkownika i hasło. Musimy zaktualizować logiki na stronie logowania, tak aby sprawdza poprawność poświadczeń względem magazynu użytkowników struktury członkostwa.

Znacznie takich jak tworzenie kont użytkowników, poświadczenia mogą być sprawdzone programowo, albo w sposób deklaratywny. Interfejs API członkostwa obejmuje metodę programowe sprawdzanie poprawności poświadczeń użytkownika względem magazynu użytkowników. I ASP.NET, który jest dostarczany za pomocą kontrolki logowania sieci Web, która renderuje interfejsu użytkownika za pomocą pól tekstowych dla nazwy użytkownika i hasła oraz przycisk, aby zalogować się.

W tym samouczku zostanie omówiony sposób sprawdzania poprawności poświadczeń użytkownika względem magazynu użytkowników członkostwa przy użyciu zarówno programowy sposób, jak i kontrolka Login. Również przyjrzymy się jak dostosować wygląd i zachowanie kontrolki logowania. Zaczynajmy!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Krok 1: Sprawdzanie poprawności poświadczeń względem Store użytkownika członkostwa

Dla witryn sieci web, które korzystają z uwierzytelniania formularzy użytkownik loguje się do witryny sieci Web, odwiedzając stronę logowania i wprowadzając swoje poświadczenia. Te poświadczenia są porównywane względem magazynu użytkowników. Jeśli są one prawidłowe, użytkownik otrzymuje biletu uwierzytelniania formularzy, który jest token zabezpieczający, który wskazuje tożsamości i autentyczności obiekt odwiedzający.

Aby sprawdzić użytkownika względem framework członkostwa, należy użyć `Membership` klasy [ `ValidateUser` metoda](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx). `ValidateUser` Metoda przyjmuje dwa parametry wejściowe — *`username`* i *`password`* — i zwraca wartość logiczną wskazującą, czy poświadczenia są prawidłowe. Za pomocą `CreateUser` metoda zbadaliśmy w poprzednim samouczku `ValidateUser` metoda deleguje rzeczywista weryfikacja do skonfigurowanego dostawcy członkostwa.

`SqlMembershipProvider` Sprawdza poprawność podanych poświadczeń, uzyskując hasło użytkownika określony za pomocą `aspnet_Membership_GetPasswordWithFormat` procedury składowanej. Pamiętamy `SqlMembershipProvider` przechowuje hasła użytkowników przy użyciu jednej z trzech formatów: wyczyść, szyfrowane, lub mieszany. `aspnet_Membership_GetPasswordWithFormat` Procedura składowana ma zwracać hasła w formacie raw. Dla zaszyfrowanych lub skrótu hasła `SqlMembershipProvider` przekształca *`password`* wartości przekazanej do `ValidateUser` metody na jego równoważnik szyfrowane lub mieszany stanu i porównuje ją z zwrócona z Baza danych. Jeśli hasło przechowywane w bazie danych jest zgodny sformatowane hasła podanego przez użytkownika, poświadczenia są prawidłowe.

Zaktualizujmy naszą stronę logowania (~ /`Login.aspx`) tak, aby sprawdza poprawność podanych poświadczeń względem magazynu użytkowników struktury członkostwa. Utworzyliśmy tę stronę logowania w <a id="Tutorial02"> </a> [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-cs.md) samouczek, Tworzenie interfejsu z dwóch pól tekstowych dla nazwy użytkownika i hasła, Zapamiętaj mnie pole wyboru i przycisku logowania (patrz rysunek 1). Kod sprawdza poprawność wprowadzone poświadczenia z ustaloną listą par nazwa użytkownika i hasło (Scott/hasła, Jisun/hasło i hasła/Sam). W <a id="Tutorial03"> </a> [ *Konfiguracja uwierzytelniania formularzy i Tematy zaawansowane* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) samouczek Zaktualizowaliśmy kodu strony logowania do przechowywania dodatkowych informacji w formularzach bilet uwierzytelnienia `UserData` właściwości.


[![Interfejs strony logowania zawiera dwa pola tekstowe, elementu CheckBoxList i przycisku](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**Rysunek 1**: strony logowania interfejsu zawiera dwa pola tekstowe, elementu CheckBoxList i przycisku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))


Interfejs użytkownika na stronie logowania może pozostać bez zmian, ale musimy Zastąp przycisk logowania `Click` programu obsługi zdarzeń z kodem, który sprawdza poprawność użytkownika względem magazynu użytkowników struktury członkostwa. Aktualizacja programu obsługi zdarzeń, aby jego kod wygląda następująco:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

Ten kod jest niezwykle proste. Rozpoczniemy pracę przez wywołanie metody `Membership.ValidateUser` jest metoda w podanej nazwy użytkownika i hasła. Jeśli ta metoda zwraca wartość true, a następnie użytkownik jest zalogowany do witryny za pomocą `FormsAuthentication` metody RedirectFromLoginPage klasy. (Tak jak Omówiliśmy to w <a id="Tutorial2"> </a> [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-cs.md) samouczku `FormsAuthentication.RedirectFromLoginPage` tworzy bilet uwierzytelniania formularzy i następnie przekierowuje użytkownika do odpowiedniej strony.) Jeśli poświadczenia są nieprawidłowe, jednak `InvalidCredentialsMessage` etykieta jest wyświetlana, informujący użytkownika, że jego nazwa użytkownika lub hasło jest niepoprawne.

To wszystko jest do niego!

Aby sprawdzić, czy strony logowania działa zgodnie z oczekiwaniami, spróbuj zalogować się za pomocą jednego konta użytkownika utworzonego w poprzednim samouczku. Jeśli konto nie zostało jeszcze utworzone, przejdź dalej i utworzyć ją z `~/Membership/CreatingUserAccounts.aspx` strony.

> [!NOTE]
> Gdy użytkownik wprowadzi swoje poświadczenia i przesyła formularz strony logowania, poświadczenia, w tym jej hasła są przesyłane przez Internet do serwera sieci web w *zwykły tekst*. Oznacza to, że wszelkie haker wykrywanie ruchu sieciowego można zobaczyć, nazwę użytkownika i hasło. Aby temu zapobiec, jest niezbędne do szyfrowania ruchu sieciowego przy użyciu [gniazda warstwy SSL (Secure)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Pozwoli to zagwarantować, są zaszyfrowane poświadczenia (a także kod znaczników HTML całą stronę) od momentu jego opuszczają przeglądarki otrzymanie przez serwer sieci web.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Sposób obsługi nieudanych prób logowania w ramach członkostwa

Gdy użytkownik osiągnie strony logowania i przesyła swoich poświadczeń, przeglądarki zgłasza żądanie HTTP do strony logowania. Jeśli poświadczenia są prawidłowe, odpowiedź HTTP zawiera bilet uwierzytelniania w pliku cookie. W związku z tym haker podjęto próbę przerwania do witryny usługi można utworzyć program, który wyczerpująco wysyła żądania HTTP do strony logowania przy użyciu prawidłowej nazwy użytkownika i przewidywanie hasło. Jeśli odgadnięcia hasła jest prawidłowa, strony logowania zwróci pliku cookie biletu uwierzytelniania, w tym momencie program wie, że ma on stumbled po parę prawidłowe nazwy użytkownika i hasła. Poprzez atak siłowy takiego programu może być możliwość natknąć się od hasła użytkownika, zwłaszcza, jeśli hasło jest słabym.

Aby uniknąć takich ataków siłowych, w ramach członkostwa do zablokowania użytkownika, jeśli istnieje wiele prób zalogowania się niepowodzeniem w określonym czasie. Dokładne parametry są konfigurowane za pomocą następujących ustawień konfiguracji dwóch dostawcy członkostwa:

- `maxInvalidPasswordAttempts` -Określa, ile nieprawidłowe hasło prób są dozwolone dla użytkownika w przedziale czasu, zanim konto jest zablokowane. Wartość domyślna to 5.
- `passwordAttemptWindow` -Określa okres czasu, w ciągu kilku minut, podczas których określoną liczbę nieudanych prób logowania spowoduje, że konto zostało zablokowane. Wartość domyślna to 10.

Jeśli użytkownik zostały zablokowane, ona nie mogą się zalogować, dopóki administrator odblokowuje konto. Po użytkownik zostaje zablokowane, `ValidateUser` zostanie metoda *zawsze* zwracają `false`nawet wtedy, gdy podano prawidłowe poświadczenia. Podczas tego zachowania zmniejsza prawdopodobieństwo, że haker spowoduje awarię do witryny za pomocą metod atak siłowy, jego znajdą blokowaniem prawidłowym użytkownikiem, który po prostu zapomniał swojego hasła lub przypadkowo ma włączony klawisz Caps Lock lub wystąpił zły dzień Pisownia.

Niestety nie ma wbudowanego narzędzia odblokowywania konta użytkownika. Aby odblokować konto, możesz zmodyfikować bazy danych bezpośrednio — zmienić `IsLockedOut` pole `aspnet_Membership` tabeli na odpowiednim kontem użytkownika — lub utworzyć opartego na sieci web interfejs, który wyświetla blokady konta przy użyciu opcji, aby je odblokować. Będziemy sprawdzać Tworzenie interfejsów administracyjnych do wykonywania typowych zadań związanych z kontem i roli użytkownika w przyszłości zapoznać się z samouczkiem.

> [!NOTE]
> Jednym z minusów `ValidateUser` metodą jest, że gdy podane poświadczenia są nieprawidłowe, nie zapewnia żadnych wyjaśnienie, dlaczego. Poświadczenia mogą być nieprawidłowe, ponieważ nie istnieje żadne pasującą parę nazwy użytkownika i hasła w magazynie użytkownika lub ponieważ użytkownik nie ma jeszcze zatwierdzone lub użytkownik został zablokowany. W kroku 4 zobaczymy sposób wyświetlania bardziej szczegółowy komunikat dla użytkownika, gdy ich próba logowania nie powiedzie się.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Krok 2: Zbieranie poświadczeń za pomocą kontrolki sieci Web logowania

[Formantu sieci Web logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) powoduje wyświetlenie domyślnego interfejsu użytkownika jest bardzo podobne do przedstawionego utworzyliśmy w <a id="SKM5"> </a> [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-cs.md) samouczka. Za pomocą kontrolki logowania zapisuje NAS Praca z konieczności tworzenia interfejsu do zbierania poświadczeń s osoby odwiedzającej. Ponadto kontrolki logowania automatycznie zaloguje użytkowników (przy założeniu, że przesłane poświadczenia są poprawne), a tym samym zapisywanie nam od konieczności pisania kodu.

Zaktualizujmy `Login.aspx`, zastępując utworzonych ręcznie interfejsu i kodu za pomocą kontrolki logowania. Start, usuwając istniejący kod znaczników i kodu w `Login.aspx`. Możesz usunąć ją od razu wykupić lub po prostu komentarz dotyczący działanie. Aby przekształcić w komentarz oznaczeniu deklaracyjnym, należy ująć ją za pomocą `<%--` i `--%>` ograniczników. Ograniczniki te można wprowadzić ręcznie lub, jak pokazano na rysunku 2, możesz wybrać tekst, który ma komentarz, a następnie kliknij przycisk Komentarz zaznaczonych wierszach ikonę na pasku narzędzi. Podobnie umożliwia komentarz ikonę wybranych wierszy w komentarz zaznaczony kod w klasie CodeBehind.


[![Komentarz istniejące deklaratywne znaczników i kodu źródłowego w Login.aspx](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**Rysunek 2**: komentarz się istniejących deklaratywne znaczników i kodu źródłowego w `Login.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))


> [!NOTE]
> Komentarz ikonę wybranych wierszy nie jest dostępna podczas wyświetlania oznaczeniu deklaracyjnym w programie Visual Studio 2005. Jeśli nie używasz programu Visual Studio 2008 należy ręcznie dodać `<%--` i `--%>` ograniczników.


Następnie przeciągnij formant logowania z przybornika do strony i ustaw jego `ID` właściwość `myLogin`. W tym momencie ekran powinien wyglądać podobnie do rysunek 3. Należy pamiętać, że kontrolka Login domyślny interfejs zawiera kontrolki TextBox dla nazwy użytkownika i hasło, Pamiętaj mnie przy następnym pole wyboru i przycisk w dzienniku. Dostępne są także `RequiredFieldValidator` kontrolki dla dwóch pól tekstowych.


[![Dodaj kontrolkę logowanie do strony](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**Rysunek 3**: Dodawanie formantu logowanie do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))


I wszystko jest gotowe! Po kliknięciu przycisk Zaloguj kontrolki logowania nastąpi odświeżenie strony i kontrolki logowania będzie wywoływać `Membership.ValidateUser` metody, przekazując wprowadzić nazwę użytkownika i hasło. Jeśli poświadczenia są nieprawidłowe, kontrolka Login wyświetla komunikat informujący, np. Jeśli jednak poświadczenia są prawidłowe, kontrolka Login tworzy bilet uwierzytelniania formularzy i przekierowuje użytkownika do odpowiedniej strony.

Kontrolka Login używa czterech czynników do określenia odpowiedniej strony do przekierowania użytkownika po pomyślnym zalogowaniu:

- Czy kontrolki logowania się na stronie logowania, zgodnie z definicją `loginUrl` ustawieniu konfiguracji uwierzytelniania formularzy; wartość domyślna tego ustawienia to `Login.aspx`
- Obecność `ReturnUrl` parametr querystring
- Wartość kontrolki logowania [ `DestinationUrl` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- `defaultUrl` Wartość określona w formularzach Ustawienia konfiguracji uwierzytelniania; wartość domyślna tego ustawienia to `Default.aspx`

Rysunek 4 przedstawia, jak kontrolka Login korzysta z tych czterech parametrów na decyzję odpowiedniej strony.


[![Dodaj kontrolkę logowanie do strony](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**Rysunek 4**: Dodawanie formantu logowanie do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))


Poświęć chwilę, w celu przetestowania kontrolki logowania, odwiedzając witrynę za pośrednictwem przeglądarki i logując się jako istniejącego użytkownika w ramach członkostwa.

Kontrolka Login renderowanych interfejs jest wysoce konfigurowalne. Istnieje kilka właściwości, które mają wpływ na jego wygląd; Co więcej kontrolki logowania, można przekonwertować na szablon precyzyjną kontrolę nad tym układ elementów interfejsu użytkownika. W pozostałej części tego kroku zbadamy, jak dostosować wygląd i układ.

### <a name="customizing-the-login-controls-appearance"></a>Dostosowywanie wyglądu kontrolki logowania

Kontrolka Login domyślne ustawienia właściwości renderowania interfejsu użytkownika z tytułem (Zaloguj), kontrolki pola tekstowego i etykiety dla nazwy użytkownika i hasła danych wejściowych, Pamiętaj mnie czasu obok pola wyboru, a przycisk Zaloguj. Wystąpienia te elementy są wszystkie konfigurowane za pomocą kontrolki logowania wiele właściwości. Ponadto można dodać dodatkowe elementy interfejsu użytkownika — np. łącze do strony, aby utworzyć nowe konto użytkownika — przez ustawienie właściwości lub dwie.

Poświęć kilka chwil gussy wygląd naszego kontrolka Login. Ponieważ `Login.aspx` strona ma już tekstu w górnej części strony, która jest wyświetlany komunikat logowania, tytuł kontrolka Login jest zbędny. W związku z tym, wyczyść [ `TitleText` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) wartość, aby można było usunąć tytuł kontrolka Login.

Nazwa użytkownika: i hasło: etykiety na lewo od dwóch kontrolek TextBox można dostosować za pomocą [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) i [ `PasswordLabelText` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx), odpowiednio. Zmienimy nazwę użytkownika: etykiety, które można odczytać nazwy użytkownika:. Style etykiety i pole tekstowe są konfigurowane za pomocą [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) i [ `TextBoxStyle` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx), odpowiednio.

Zapamiętaj mnie właściwości Text następnego wyboru czasu można ustawić za pomocą kontrolki logowania [ `RememberMeText property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx), i domyślną zaewidencjonowane, stan jest konfigurowane za pomocą [ `RememberMeSet property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (który Wartość domyślna to False). Przejdź dalej i ustawić `RememberMeSet` właściwości na wartość True, dzięki czemu Pamiętaj mnie czasu obok pola wyboru będzie sprawdzany przez domyślny.

Kontrolka Login oferuje dwie właściwości dostosowania układu jej kontrolek interfejsu użytkownika. [ `TextLayout property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) Wskazuje, czy nazwa użytkownika: i hasło: etykiety pojawiają się po lewej stronie, ich odpowiednich pól tekstowych (ustawienie domyślne) lub nad nimi. [ `Orientation property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) Wskazuje, czy dane wejściowe użytkownika i hasło są umieszczone w pionie (jeden nad drugim) lub w poziomie. Zamierzam pozostawić te dwie właściwości ustawione na ich wartości domyślne, ale zachęcam Cię do wypróbowania ustawienie tych dwóch właściwości do wartości innych niż domyślne, aby zobaczyć efekt wynikowy.

> [!NOTE]
> W następnej sekcji konfigurowania układu kontrolki logowania przyjrzymy się przy użyciu szablonów definiują dokładny układ elementy interfejsu użytkownika formantu układu.


Podsumowanie ustawień właściwości kontrolki logowania, ustawiając [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) i [ `CreateUserUrl` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) do nie zarejestrowano jeszcze? Tworzenie konta usługi! i `~/Membership/CreatingUserAccounts.aspx`, odpowiednio. Spowoduje to dodanie hiperłącze interfejsu kontrolka Login wskazuje stronę utworzonego w <a id="SKM6"> </a> [poprzedni Samouczek](creating-user-accounts-cs.md). Kontrolka Login [ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) i [ `HelpPageUrl` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) i [ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) i [ `PasswordRecoveryUrl` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) działają w taki sam sposób renderowania łączy do strony pomocy i strona odzyskiwania hasła.

Po wprowadzeniu tych zmian właściwości oznaczeniu deklaracyjnym i wyglądu kontrolki logowania powinien wyglądać podobnie do przedstawionego na rysunku 5.


[![Wartości właściwości kontrolek logowania dyktowanie jego wygląd](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**Rysunek 5**: właściwości kontrolek logowania wartości dyktowanie jego wygląd ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Konfigurowanie układu kontrolki logowania

Kontrolka sieci Web logowania domyślnym interfejsem użytkownika decydujących o interfejsu w kodzie HTML `<table>`. Ale co zrobić, jeśli potrzebujemy bardziej precyzyjną kontrolę nad wyniku renderowania? Może być chcemy zastąpić `<table>` z serią `<div>` tagów. Lub co zrobić, jeśli nasza aplikacja wymaga dodatkowych poświadczeń do uwierzytelniania? Wiele witryn sieci Web finansowych, na przykład wymagać od użytkowników Podaj nie tylko nazwę użytkownika i hasło, ale również osobisty numer identyfikacyjny (PIN) lub inne informacje identyfikacyjne. Niezależnie od przyczyny mogą być, jest możliwe do przekonwertowania kontrolka Login szablon, w którym firma Microsoft wyraźnie zdefiniować oznaczeniu deklaracyjnym interfejsu.

Musimy wykonać dwie czynności, aby można było zaktualizować kontrolki logowania do zbierania dodatkowych poświadczeń:

1. Zaktualizuj interfejs kontrolki logowania do uwzględnienia w kontrolkach internetowych do zbierania dodatkowych poświadczeń.
2. Tak, aby użytkownik jest uwierzytelniany tylko, jeśli nazwy użytkownika i hasła jest prawidłowa, a ich dodatkowe poświadczenia są prawidłowe, zbyt, musi zostać zastąpiona logiki uwierzytelniania wewnętrznego kontrolka Login.

Aby wykonać pierwsze zadanie, musimy przekonwertować kontrolki logowania na szablon i Dodaj wymagane kontrolki sieci Web. Jak w przypadku drugie zadanie logiki uwierzytelniania kontrolki logowania może zostać zastąpiona przez tworzenie obsługi zdarzeń dla formantu [ `Authenticate` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx).

Zaktualizujmy kontrolki logowania, który monituje użytkowników o ich nazwy użytkownika, hasło i adres e-mail i uwierzytelnia użytkownika tylko wtedy, jeśli podany adres e-mail jest zgodny adres e-mail użytkownika w pliku. Najpierw musimy przekonwertować interfejsu kontroli Zaloguj się do szablonu. Kontrolka Login tagu inteligentnego Wybierz Konwertuj do opcji szablonu.


[![Konwertuj kontrolkę Zaloguj się do szablonu](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**Rysunek 6**: Konwertuj kontrolkę Zaloguj się do szablonu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))


> [!NOTE]
> Aby cofnąć kontrolka Login wstępnie template wersji, kliknij link resetowania z tagu kontrolki.


Konwertowanie szablonu kontrolki logowania dodaje `LayoutTemplate` do formantu w oznaczeniu deklaracyjnym za pomocą elementów kodu HTML i formantów sieci Web, definiowanie interfejsu użytkownika. Jak pokazano na rysunku 7, konwertowania formant z szablonem usuwa wiele właściwości w oknie właściwości, takie jak `TitleText`, `CreateUserUrl`, i tak dalej, ponieważ wartości tych właściwości są ignorowane, gdy przy użyciu szablonu.


[![Mniej właściwości są dostępne podczas logowania kontrolny została przeniesiona do szablonu](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**Rysunek 7**: mniej właściwości są dostępne podczas logowania kontrolny jest konwertowana na szablon ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))


Kod znaczników HTML w `LayoutTemplate` może być modyfikowany. Podobnie możesz dodać żadnych nowych formantów sieci Web do szablonu. Jednak ważne jest, kontrolki sieci Web core tej kontrolki logowania pozostają w szablonie i zachować przypisanych do nich `ID` wartości. W szczególności nie zostaje usunięte ani zmienić nazwy `UserName` lub `Password` pola tekstowe, `RememberMe` zaznacz pole wyboru `LoginButton` przycisku `FailureText` etykietę, lub `RequiredFieldValidator` kontrolki.

Aby zebrać adres e-mail osoby odwiedzającej, należy dodać pole tekstowe do szablonu. Dodaj następujące oznaczeniu deklaracyjnym między wierszem tabeli (`<tr>`) zawierający `Password` pola tekstowego i wiersza tabeli, który przechowuje Pamiętaj mnie obok czasu wyboru:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

Po dodaniu `Email` pola tekstowego, odwiedź stronę za pośrednictwem przeglądarki. Jak pokazano na rysunku 8, interfejs użytkownika kontrolki logowania zawiera teraz trzecie pole tekstowe.


[![Kontrolka Login teraz zawiera pole tekstowe dla adresu E-mail użytkownika](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**Rysunek 8**: kontrolka Login teraz zawiera pole tekstowe dla adresu E-mail użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))


Na tym etapie kontrolka Login jest nadal przy użyciu `Membership.ValidateUser` metodę, aby sprawdzić poprawność podanych poświadczeń. Odpowiednio wartość zawierana `Email` pole tekstowe nie ma żadnego wpływu na tego, czy użytkownik może zalogować się. W kroku 3 przedstawiony zostanie sposób zastąpienia kontrolka Login logiki uwierzytelniania, dzięki czemu poświadczenia tylko są uznawane za ważne, jeśli nazwa użytkownika i hasło są prawidłowe, a podany adres e-mail jest zgodny za pomocą adresu e-mail w pliku.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Krok 3: Zmodyfikowanie logiki uwierzytelniania kontrolki logowania

Po użytkownik dostarcza jej poświadczenia i kliknięcia, które ensues przycisk Zaloguj odświeżenie strony i logowania sterowania przechodzi od jego przepływu pracy uwierzytelniania. Przepływ pracy jest uruchamiany, tworząc [ `LoggingIn` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Wszystkie procedury obsługi zdarzeń skojarzonych z tym zdarzeniem może spowodować anulowanie dziennika operacji, ustawiając `e.Cancel` właściwość `true`.

Jeśli nie zostanie anulowana dziennika operacji, przepływ pracy w miarę, tworząc [ `Authenticate` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Jeśli program obsługi zdarzeń dla `Authenticate` zdarzeń, a następnie ją jest odpowiedzialny za sprawdzenie, czy podane poświadczenia są prawidłowe. Jeśli zostanie określona żadna procedura obsługi zdarzeń, korzysta z kontrolki logowania `Membership.ValidateUser` metody do sprawdzania poprawności poświadczeń.

Jeśli podane poświadczenia są prawidłowe, a następnie zostanie utworzony bilet uwierzytelniania formularzy, [ `LoggedIn` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) jest wywoływane, a użytkownik jest przekierowywany do odpowiedniej strony. Jeśli jednak poświadczenia zostaną uznane za nieprawidłowe, a następnie [ `LoginError` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) jest wywoływane i zostanie wyświetlony komunikat informujący użytkownika, że ich poświadczenia były nieprawidłowe. Domyślnie w przypadku niepowodzenia logowania kontroli po prostu ustawia jego `FailureText` etykiety formantu właściwość Text na komunikat o błędzie (próba logowania zakończyła się niepowodzeniem. Spróbuj ponownie później). Jednak jeśli kontrolka Login [ `FailureAction` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) ustawiono `RedirectToLoginPage`, następnie problemów kontrolki logowania `Response.Redirect` do strony logowania, dodając parametr querystring `loginfailure=1` (co powoduje, że nazwa logowania formant, aby wyświetlić komunikat o błędzie).

Rysunek 9 oferuje jest blokowy przepływu pracy uwierzytelniania.


[![Przepływ pracy uwierzytelniania kontrolki logowania](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**Rysunek 9**: przepływ pracy uwierzytelniania kontrolka Login ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))


> [!NOTE]
> Jeśli zastanawiasz zastosowania `FailureAction`firmy `RedirectToLogin` Strona opcji, należy rozważyć następujący scenariusz. Obecnie nasz `Site.master` strony wzorcowej ma obecnie tekst Witaj, stranger wyświetlana w kolumnie po lewej stronie, gdy odwiedzanych przez użytkownika anonimowego, ale Wyobraź sobie, że Chcieliśmy, aby zastąpić ten tekst za pomocą kontrolki logowania. Pozwoliłoby to użytkownik anonimowy zalogować się na dowolnej stronie w witrynie, nie wymagając od nich bezpośrednio odwiedź stronę logowania. Jednak jeśli użytkownik nie może zalogować się za pomocą kontrolki logowania renderowany przez stronę wzorcową, sensowne przekierowywania ich do strony logowania (`Login.aspx`), ponieważ tej strony, prawdopodobnie zawiera dodatkowe informacje, łącza i innych pomoc — np. linki w celu utworzenia nowe konto lub pobrać utraty hasła —, które nie zostały dodane do strony wzorcowej.


### <a name="creating-theauthenticateevent-handler"></a>Tworzenie`Authenticate`program obsługi zdarzeń

Aby podłączyć naszych logiki uwierzytelniania niestandardowego, należy utworzyć program obsługi zdarzeń dla formantu logowania `Authenticate` zdarzeń. Tworzenie zdarzenia obsługi dla `Authenticate` zdarzeń wygeneruje następującą definicję obsługi zdarzenia:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

Jak widać, `Authenticate` programu obsługi zdarzeń jest przekazywany obiekt typu [ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) jako drugi parametr wejściowy. `AuthenticateEventArgs` Klasa zawiera właściwość typu Boolean o nazwie `Authenticated` używany do określenia, czy podane poświadczenia są prawidłowe. Nasze zadania, a następnie jest napisać tutaj kod, który określa, czy podane poświadczenia są prawidłowe i ustawienie `e.Authenticate` właściwość odpowiednio.

### <a name="determining-and-validating-the-supplied-credentials"></a>Określanie i sprawdzanie poprawności podanych poświadczeń

Korzystanie z kontrolki logowania [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) i [ `Password` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) można określić nazwę użytkownika i hasło poświadczeń wprowadzonych przez użytkownika. Aby określić wartości wprowadzone do wszelkich dodatkowych formantów sieci Web (takie jak `Email` TextBox dodane w poprzednim kroku), użyj *`LoginControlID`* `.FindControl`("*`controlID`*") można uzyskać programowy odwołanie do formantu sieci Web w szablonie którego `ID` właściwości jest równa *`controlID`*. Na przykład, aby pobrać odwołanie do `Email` pola tekstowego, użyj następującego kodu:

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

Aby zweryfikować poświadczenia użytkownika należy wykonać dwie czynności:

1. Upewnij się, że podana nazwa użytkownika i hasło są prawidłowe
2. Upewnij się, czy wprowadzony adres e-mail jest zgodna z adresem e-mail plik dla użytkowników próbujących się zalogować

Aby osiągnąć Sprawdź najpierw, możemy po prostu użyć `Membership.ValidateUser` metody, takie jak widzieliśmy w kroku 1. Dla drugiego wyboru należy ustalić adres e-mail użytkownika, aby firma Microsoft może porównać na adres e-mail, wprowadzone w formancie TextBox. Aby uzyskać informacje na temat określonego użytkownika, należy użyć `Membership` klasy [ `GetUser` metoda](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx).

`GetUser` Metoda ma kilka przeciążeń. Jeśli nie określono przekazywanie w żadnych parametrów, zwraca informacje o aktualnie zalogowanego użytkownika. Aby uzyskać informacje na temat określonego użytkownika, wywołaj `GetUser` przekazując swoją nazwę użytkownika. W obu przypadkach `GetUser` zwraca [ `MembershipUser` obiektu](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), który ma właściwości, takie jak `UserName`, `Email`, `IsApproved`, `IsOnline`i tak dalej.

Poniższy kod implementuje tych dwóch kontroli. Jeśli oba zakończy się pomyślnie, następnie `e.Authenticate` ustawiono `true`, w przeciwnym razie jest ona przypisana `false`.

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

Przy użyciu tego kodu w miejscu spróbuj zalogować się jako prawidłowego użytkownika, wprowadzając prawidłową nazwę użytkownika, hasło i adres e-mail. Spróbuj ponownie, ale tym razem użyj celowo nieprawidłowy adres e-mail (zobacz rysunek 10). Na koniec Wypróbuj raz trzeci przy użyciu nazwy użytkownika nie istnieje. W pierwszym przypadku należy pomyślnie zalogowanie się do witryny, ale w dwóch ostatnich przypadkach powinien zostać wyświetlony komunikat nieprawidłowe poświadczenia kontrolka Login.


[![Tito nie można zalogować się w przypadku określenia nieprawidłowy adres E-mail](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**Na rysunku nr 10**: Tito nie dziennika w przypadku dostarczenie niepoprawny adres E-mail ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))


> [!NOTE]
> Zgodnie z opisem w sekcji jak członkostwa Framework obsługuje nieprawidłowych prób logowania w kroku 1, gdy `Membership.ValidateUser` metoda jest nazywana i przekazywana nieprawidłowe poświadczenia, jego śledzi Nieprawidłowa próba logowania i blokuje się użytkownika, jeśli określony przekraczają Próg nieudanych prób podania w określonym przedziale czasu. Od naszych wywołania logiki uwierzytelniania niestandardowego `ValidateUser` metody niepoprawnego hasła dla prawidłowej nazwy użytkownika powoduje zwiększenie licznika próba nieprawidłowego logowania, ale ten licznik nie jest zwiększany w przypadku, gdy nazwa użytkownika i hasło są prawidłowe, ale adres e-mail jest nieprawidłowy. To, to zachowanie jest odpowiedni, ponieważ jest mało prawdopodobne, że haker będzie znać nazwę użytkownika i hasło, ale ma używanie atak siłowy techniki, aby określić adres e-mail użytkownika.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Krok 4: Poprawa kontrolka Login nieprawidłowymi poświadczeniami komunikatu

Gdy użytkownik próbuje się zalogować przy użyciu nieprawidłowych poświadczeń, kontrolka Login pojawi się komunikat wyjaśniający, że próba logowania nie powiodła się. W szczególności kontrolka wyświetla komunikat określony przez jego [ `FailureText` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), który ma wartość domyślną próba logowania zakończyła się niepowodzeniem. Spróbuj ponownie później.

Odwołaj, istnieje wiele powodów dlaczego poświadczeń użytkownika może być nieprawidłowy:

- Nazwa użytkownika nie istnieje
- Nazwa użytkownika istnieje, ale hasło jest nieprawidłowe
- Nazwa użytkownika i hasło są prawidłowe, ale użytkownik nie jest jeszcze zatwierdzone.
- Nazwa użytkownika i hasło są prawidłowe, ale użytkownik jest zablokowany limit (najprawdopodobniej ponieważ przekroczyły liczbę nieudanych prób logowania w określonym przedziale czasu)

I może być też inne przyczyny przy użyciu logiki uwierzytelniania niestandardowego. Na przykład z kodem napisaliśmy w kroku 3, nazwa użytkownika i hasło może być prawidłowy, ale adres e-mail jest niepoprawny.

Niezależnie od tego, dlaczego poświadczenia są nieprawidłowe kontrolka Login wyświetla ten sam komunikat o błędzie. Ten brak opinii może być mylące dla użytkownika, którego konto nie ma jeszcze zatwierdzone lub który zostało zablokowane. Usługa trochę pracy, firma Microsoft udostępnia kontrolki logowania, wyświetli się komunikat bardziej odpowiednie.

Zawsze, gdy użytkownik próbuje zalogować się przy użyciu nieprawidłowych poświadczeń zgłasza kontrolka Login jego `LoginError` zdarzeń. Przejdź dalej i utworzyć program obsługi zdarzeń dla tego zdarzenia i Dodaj następujący kod:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

Powyższy kod, który rozpoczyna się przez ustawienie kontrolki logowania `FailureText` właściwości na wartość domyślną (próba logowania zakończyła się niepowodzeniem. Spróbuj ponownie później). Następnie sprawdza, jeśli nazwę, którą podano mapuje do istniejącego konta użytkownika. Jeśli tak, consults wynikowy `MembershipUser` obiektu `IsLockedOut` i `IsApproved` właściwości w celu ustalenia, czy konto zostało zablokowane lub nie została jeszcze zatwierdzona. W obu przypadkach `FailureText` właściwość zostanie zaktualizowany w celu odpowiadająca wartość.

Aby przetestować ten kod, celowo próbować zalogować się jako istniejącego użytkownika, ale użyć niepoprawnego hasła. Wykonaj tym pięć razy pod rząd w przedziale czasu wynoszącym 10 minut, a konto zostanie zablokowane. Jak przedstawia rysunek 11, logowanie kolejnych prób będzie zawsze się nie powieść (nawet w przypadku prawidłowego hasła), ale będą teraz wyświetlane bardziej opisowe Twoje konto zostało zablokowane z powodu zbyt wielu nieudanych prób logowania. Skontaktuj się z administratorem i poproś wiadomości odblokować konto.


[![Tito wykonana zbyt wiele nieudanych prób logowania, a zostało zablokowane](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**Rysunek 11**: Tito wykonywane zbyt wiele nieprawidłowych prób logowania ma został zablokowany do edycji i ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))


## <a name="summary"></a>Podsumowanie

Wcześniejsze tego samouczka naszą stronę logowania sprawdzić poprawności podanych poświadczeń z ustaloną listą par nazwa użytkownika i hasło. W tym samouczku Zaktualizowaliśmy stronę, aby zweryfikować poświadczeń względem framework członkostwa. W kroku 1 przyjrzeliśmy się przy użyciu `Membership.ValidateUser` metoda programowo. W kroku 2 zastąpiliśmy naszego interfejsu ręcznie utworzone przez użytkownika i kodu za pomocą kontrolki logowania.

Kontrolka Login powoduje wyświetlenie interfejsu użytkownika standardowego logowania i automatycznie przeprowadza walidację poświadczeń użytkownika względem framework członkostwa. Ponadto w przypadku prawidłowe poświadczenia, kontrolka Login loguje użytkownika za pomocą uwierzytelniania formularzy. Krótko mówiąc w pełni funkcjonalne logowania użytkownika jest dostępna po prostu przeciągając kontrolki logowania do strony, nie dodatkowych oznaczeniu deklaracyjnym lub kod. Co więcej, kontrolka Login jest dużym stopniu dostosowywane, pozwalając na poprawnie stopnia kontroli nad zarówno renderowanych interfejsu i uwierzytelniania logikę użytkownika.

W tym momencie odwiedzających naszą witrynę internetową można utworzyć nowego konta użytkownika i dziennika do witryny, ale musimy jeszcze Przyjrzyj się ograniczenie dostępu do stron, na podstawie uwierzytelnionego użytkownika. Obecnie każdy użytkownik, uwierzytelnieni lub anonimowi, można wyświetlić dowolną stronę w naszej witrynie. Wraz z kontroli dostępu do stron naszej witrynie na podstawie użytkownika przez użytkownika, firma Microsoft może być niektóre strony, w których funkcje zależą od użytkownika. Następnego samouczka pokazano, jak ograniczyć dostęp i funkcje na stronie, na podstawie zalogowanego użytkownika.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Wyświetlanie niestandardowych komunikatów zablokowane i nie zatwierdzonych użytkowników](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Badanie programu ASP.NET 2.0 na członkostwo, role i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Porady: Tworzenie strony logowania programu ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Dokumentacja techniczna kontrolki logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Korzystanie z kontrolek logowania](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Pracował nad Bento Scottem, autor wiele książek ASP/ASP.NET i założyciel 4GuysFromRolla.com, przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena  *[Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott można z Tobą skontaktować w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Teresa Murphy i Michael Olivero. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Poprzednie](creating-user-accounts-cs.md)
> [dalej](user-based-authorization-cs.md)
