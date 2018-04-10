---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: Sprawdzanie poprawności poświadczeń użytkownika z członkostwa w magazynie użytkownika (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku omówione sprawdzania poprawności poświadczeń użytkownika przed magazynie użytkownika członkostwa przy użyciu zarówno programowy sposób, jak i kontroli logowania...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: 484a0f16265ee2d887ee08f6ae7ada47047f1f04
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="validating-user-credentials-against-the-membership-user-store-c"></a>Sprawdzanie poprawności poświadczeń użytkownika z członkostwa w magazynie użytkownika (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> W tym samouczku omówione sprawdzania poprawności poświadczeń użytkownika przed magazynie użytkownika członkostwa przy użyciu zarówno programowy sposób, jak i kontroli logowania. Ponadto przedstawiono, jak dostosować wygląd i zachowanie kontroli logowania.


## <a name="introduction"></a>Wprowadzenie

W <a id="Tutorial05"> </a> [poprzedniego samouczek](creating-user-accounts-cs.md) Analizujemy sposób tworzenia nowego konta użytkownika w ramach członkostwa. Analizujemy najpierw programowe tworzenie kont użytkowników za pomocą `Membership` klasy `CreateUser` metody, a następnie w formancie CreateUserWizard sieci Web. Strona logowania weryfikuje obecnie dostarczonych poświadczeń z wpisaną na stałe listą par nazwa użytkownika i hasło. Należy zaktualizować logiki strony logowania, dzięki czemu jest sprawdzane poświadczenia magazynu użytkowników w ramach członkostwa.

Wiele takich jak tworzenie kont użytkowników można można sprawdzić poprawności poświadczeń programowo i deklaratywnie. Interfejs API członkostwa zawiera metodę programowane sprawdzanie poprawności poświadczeń użytkownika przed magazynie użytkownika. I ASP.NET jest dostarczany za pomocą formantu logowania w sieci Web, która renderuje interfejs użytkownika z pól tekstowych dla nazwy użytkownika i hasła oraz przycisk, aby zalogować się.

W tym samouczku omówione sprawdzania poprawności poświadczeń użytkownika przed magazynie użytkownika członkostwa przy użyciu zarówno programowy sposób, jak i kontroli logowania. Ponadto przedstawiono, jak dostosować wygląd i zachowanie kontroli logowania. Dzieła!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Krok 1: Sprawdzanie poprawności poświadczeń z magazynu użytkowników członkostwa

Dla witryn sieci web, które korzystają z uwierzytelniania formularzy, użytkownik loguje się do witryny sieci Web odwiedzając stronę logowania i wprowadzić swoje poświadczenia. Te poświadczenia są porównywane ze sklepem użytkownika. Jeśli są one prawidłowe, użytkownik otrzymuje biletu uwierzytelniania formularzy, który jest token zabezpieczający, który wskazuje tożsamości i autentyczności dla obiekt odwiedzający.

Aby zweryfikować użytkownika względem framework członkostwa, użyj `Membership` klasy [ `ValidateUser` metody](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx). `ValidateUser` Metoda przyjmuje dwa parametry wejściowe - *`username`* i *`password`* - i zwraca wartość logiczną wskazującą, czy poświadczenia są prawidłowe. Jak `CreateUser` możemy się zbadana poprzedniej samouczka — metoda `ValidateUser` metody deleguje rzeczywista weryfikacja do skonfigurowanego dostawcy członkostwa.

`SqlMembershipProvider` Weryfikuje podanych poświadczeń, uzyskując określonego użytkownika hasła za pośrednictwem `aspnet_Membership_GetPasswordWithFormat` procedury składowanej. Odwołania, który `SqlMembershipProvider` przechowuje hasła użytkowników przy użyciu jednej z trzech formatów: wyczyść, szyfrowane lub mieszany. `aspnet_Membership_GetPasswordWithFormat` Procedury składowanej zwraca hasło w jego format raw. Dla zaszyfrowanych lub skrótu hasła `SqlMembershipProvider` przekształca *`password`* wartość przekazany `ValidateUser` metody do jego odpowiednik szyfrowane lub mieszany stanu i porównuje ją z zwrócił co Baza danych. Jeśli hasło przechowywane w bazie danych odpowiada sformatowany hasła podanego przez użytkownika, poświadczenia są prawidłowe.

Ta funkcja pozwala zaktualizować naszą stronę logowania (~ /`Login.aspx`), aby sprawdza poprawność dostarczonych poświadczeń z magazynu użytkowników framework członkostwa. Utworzyliśmy tę stronę logowania w <a id="Tutorial02"> </a> [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-cs.md) samouczek, Tworzenie interfejsu z dwóch pól tekstowych dla nazwy użytkownika i hasła, Zapamiętaj mnie, pole wyboru, a przycisk logowania (zobacz rysunek 1). Kod weryfikuje wprowadzone poświadczenia z wpisaną na stałe listą par nazwa użytkownika i hasło (Scott/hasło, Jisun/hasło i Sam i hasła). W <a id="Tutorial03"> </a> [ *konfiguracji uwierzytelniania formularzy i Tematy zaawansowane* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) samouczek Zaktualizowaliśmy stronę logowania kod do przechowywania dodatkowych informacji w formularzach bilet uwierzytelnienia `UserData` właściwości.


[![Interfejs strony logowania zawiera dwa pola tekstowe, elementu CheckBoxList i przycisk](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**Rysunek 1**: strony logowania interfejsu zawiera dwa pola tekstowe, elementu CheckBoxList i przycisk ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))


Interfejs użytkownika strony logowania może pozostać bez zmian, ale należy zastąpić przycisku Zaloguj `Click` obsługi zdarzenia z kodu, która weryfikuje użytkownika przed magazynu użytkowników framework członkostwa. Aktualizacja programu obsługi zdarzeń, aby jego kod wygląda następująco:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

Ten kod jest niezwykle proste. Rozpoczniemy przez wywołanie metody `Membership.ValidateUser` metody, przekazując podanej nazwie użytkownika i hasło. Jeśli ta metoda zwraca wartość true, a następnie użytkownik jest zalogowany do witryny za pomocą `FormsAuthentication` metody RedirectFromLoginPage klasy. (Jak wspomniano w <a id="Tutorial2"> </a> [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-cs.md) samouczka `FormsAuthentication.RedirectFromLoginPage` tworzy biletu uwierzytelniania formularzy, a następnie przekierowuje użytkownika do odpowiedniej strony.) Jeśli jednak poświadczenia są nieprawidłowe, `InvalidCredentialsMessage` jest wyświetlana etykieta, którego użytkownik dowie się ich nazwa użytkownika lub hasło jest niepoprawne.

To wszystko jest do niego!

Aby sprawdzić, czy strony logowania działa zgodnie z oczekiwaniami, spróbuj zalogować się za pomocą jednego konta użytkownika utworzonego w poprzednim samouczka. Lub, jeśli jeszcze nie utworzono konta, przejdź dalej i utwórz je z `~/Membership/CreatingUserAccounts.aspx` strony.

> [!NOTE]
> Gdy użytkownik wprowadza swoje poświadczenia i przesłaniu formularza strony logowania, poświadczenia, w tym swojego hasła są przekazywane za pośrednictwem Internetu do serwera sieci web w *zwykły tekst*. Oznacza to, że wszystkie haker wykrywanie ruchu sieciowego można wyświetlić nazwy użytkownika i hasła. Aby tego uniknąć, jest niezbędne do szyfrowania ruchu sieciowego za pomocą [gniazda warstwy SSL (Secure)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Daje to pewność, są szyfrowane poświadczenia (a także kod znaczników HTML strony Cała) od momentu jego opuszczają przeglądarki, dopóki nie zostaną odebrane przez serwer sieci web.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Jak Framework członkostwa obsługuje nieudanych prób logowania

Jeśli użytkownik osiągnie strony logowania i prześle poświadczeń, przeglądarce zgłasza żądanie HTTP do strony logowania. Jeśli poświadczenia są prawidłowe, odpowiedź HTTP zawiera biletu uwierzytelniania w pliku cookie. W związku z tym haker próby przerwanie i przejście do witryny można utworzyć program, który wyczerpujący wysyła żądania HTTP do strony logowania z prawidłową nazwę użytkownika i wynik w hasło. Jeśli wynik hasła jest prawidłowa, strony logowania zwróci pliku cookie biletu uwierzytelniania, w którym program wie, że ma ona stumbled na parę prawidłowe nazwy użytkownika i hasła. Poprzez atak siłowy takiego programu można natknąć się od hasła użytkownika, zwłaszcza, jeśli hasło jest słaba.

Aby zapobiec takich ataków siłowych, członkostwo w ramach blokuje użytkownika, jeśli istnieje wiele prób zalogowania się niepowodzeniem w danym okresie czasu. Można skonfigurować za pomocą następujących ustawień konfiguracji dwa dostawcy członkostwa są dokładne parametry:

- `maxInvalidPasswordAttempts` — Określa, ile nieprawidłowe hasło prób są dozwolone dla użytkownika w określonym przedziale czasu przed zablokowaniem konta. Wartość domyślna to 5.
- `passwordAttemptWindow` -Wskazuje czas w minutach, w których określoną liczbę nieudanych prób logowania spowoduje, że konto zostało zablokowane. Wartość domyślna to 10.

Jeśli zostało zablokowane przez użytkownika, użytkownik nie może zalogować się aż administrator odblokowuje konto. Gdy użytkownik zostaje zablokowane, `ValidateUser` metoda będzie *zawsze* zwracać `false`nawet wtedy, gdy podano poprawnych poświadczeń. Podczas tego zachowania zmniejsza prawdopodobieństwo, że haker spowoduje przerwanie do witryny za pomocą metod siłowych, można zakończyć blokowania limit prawidłowy użytkownik, który zapomniał po prostu swoje hasło lub przypadkowo włączony klawisz Caps Lock na lub napotkał nieprawidłowy dzień pisania.

Niestety nie ma wbudowane narzędzia, do odblokowania konta użytkownika. Aby odblokować konto, można zmodyfikować bazy danych bezpośrednio — Zmień `IsLockedOut` w `aspnet_Membership` tabeli dla odpowiednim kontem użytkownika - lub utworzyć opartych na sieci web interfejs, który zawiera listę blokady konta z opcjami, aby je odblokować. Omówione zostanie Tworzenie interfejsów administracyjnych do wykonywania typowych zadań związanych z kontem i roli użytkownika w przyszłości samouczka.

> [!NOTE]
> Wadą jednego interfejsu z `ValidateUser` metoda jest, że gdy podane poświadczenia są nieprawidłowe, nie zawiera żadnych wyjaśnienie, dlaczego. Poświadczenia może być nieprawidłowa, ponieważ nie istnieje żadne pasującą parę nazwy użytkownika i hasła w magazynie użytkownika lub ponieważ użytkownik nie ma jeszcze zatwierdzone lub użytkownika zostało zablokowane. W kroku 4 przedstawiono sposób wyświetlania bardziej szczegółowy komunikat do użytkownika, gdy ich próba logowania nie powiedzie się.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Krok 2: Zbieranie poświadczeń za pomocą formantu sieci Web logowania

[Kontrolka sieci Web logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) renderuje domyślnego interfejsu użytkownika jest bardzo podobny do utworzyliśmy w <a id="SKM5"> </a> [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-cs.md) samouczka. Używanie formantu logowania zapisuje nam pracy konieczności tworzenia interfejs służący do zbierania poświadczeń s obiekt odwiedzający. Ponadto kontrolka logowania automatycznie zaloguje się użytkownik (przy założeniu, że przesłane poświadczenia są prawidłowe), co zapisywanie nam z konieczności pisania kodu.

Ta funkcja pozwala zaktualizować `Login.aspx`, zastępując ręcznie utworzonego interfejsu i kodu za pomocą formantu logowania. Uruchom przez usunięcie istniejących znaczników i kodu w `Login.aspx`. Możesz go usunąć ostatecznego lub po prostu komentarz go. Aby przekształcić w komentarz deklaratywne znaczników, należy ująć ją z `<%--` i `--%>` ograniczników. Ograniczniki te można wprowadzić ręcznie lub, jak pokazano na rysunku 2, można wybrać tekst komentarz, a następnie kliknij przycisk Komentarz zaznaczonych wierszach ikony na pasku narzędzi. Podobnie można komentarz ikona zaznaczonych wierszach komentarz zaznaczony kod w klasie związanej z kodem.


[![Komentarz istniejących deklaratywne znaczników i kodu źródłowego w Login.aspx](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**Rysunek 2**: komentarz limit istniejących deklaratywne znaczników i kodu źródłowego w `Login.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))


> [!NOTE]
> Komentarz ikona wybranych wierszy jest niedostępna, podczas wyświetlania deklaratywne znaczników w programie Visual Studio 2005. Jeśli nie używasz programu Visual Studio 2008 musisz ręcznie dodać `<%--` i `--%>` ograniczników.


Następnie przeciągnij formant logowania z przybornika do strony i ustawić jej `ID` właściwości `myLogin`. W tym momencie ekranie powinna wyglądać podobnie do rysunek 3. Należy pamiętać, że kontrolka logowania domyślny interfejs zawiera formanty pole tekstowe Nazwa użytkownika i hasło, Zapamiętaj moje następnym wyboru i przycisk w dzienniku. Dostępne są także `RequiredFieldValidator` formantów dla dwóch pól tekstowych.


[![Dodawanie formantu logowanie do strony](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**Rysunek 3**: Dodawanie formantu logowanie do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))


I skończymy! Po kliknięciu formantu logowania w dzienniku przycisku, nastąpi odświeżenie strony i wywoła formantu logowania `Membership.ValidateUser` metody, przekazując wprowadzić nazwę użytkownika i hasło. Jeśli poświadczenia są nieprawidłowe, kontrolka logowania wyświetla komunikat z informacją o takich. Jeśli jednak poświadczenia są prawidłowe, formantu logowania tworzy biletu uwierzytelniania formularzy i przekierowuje użytkownika do odpowiedniej strony.

Kontrolka logowania używa czterech czynnikach w celu określenia przekierowania podczas pomyślnego logowania użytkownika do odpowiedniej strony:

- Określa, czy formant logowania jest na stronie logowania, zgodnie z definicją w `loginUrl` jest wartość domyślna tego ustawienia w konfiguracji uwierzytelniania formularzy. `Login.aspx`
- Obecność `ReturnUrl` parametr querystring
- Wartość formantu logowania [ `DestinationUrl` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- `defaultUrl` Wartość określona w formularzach Ustawienia konfiguracji uwierzytelniania; wartość domyślna to ustawienie jest `Default.aspx`

Rysunek 4 przedstawia sposób formantu logowania korzysta z tych czterech parametrów na decyzję odpowiedniej strony.


[![Dodawanie formantu logowanie do strony](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**Rysunek 4**: Dodawanie formantu logowanie do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))


Poświęć chwilę, aby przetestować formantu logowania, odwiedzając witrynę za pośrednictwem przeglądarki i logowanie jako istniejącego użytkownika w ramach członkostwa.

Interfejs renderowanych formantu logowania jest wysoce można konfigurować. Istnieje wiele właściwości, które wpływają na jego wygląd; Co więcej formantu logowania, można przekonwertować szablon dla ścisła kontrola nad układ elementów interfejsu użytkownika. Pozostała część tego kroku sprawdza Dostosowywanie wyglądu i układu.

### <a name="customizing-the-login-controls-appearance"></a>Dostosowywanie wyglądu formantu logowania

Domyślne ustawienia właściwości formantu logowania renderowania interfejsu użytkownika z tytułu (dziennik), pole tekstowe i etykiety kontroli w przypadku wejść nazwy użytkownika i hasła, pamiętaj o mnie obok czasu wyboru i przycisk Zaloguj. Wystąpienia te elementy są wszystkie konfigurowalne poprzez adresowana do wielu właściwości formantu logowania. Ponadto można dodać dodatkowe elementy interfejsu użytkownika — takie jak łącze do strony, aby utworzyć nowe konto użytkownika — przez ustawienie właściwości lub dwóch.

Poświęć kilka minut gussy się wygląd naszego formantu logowania. Ponieważ `Login.aspx` strona ma już tekstu w górnej części strony, stwierdzający, logowania, tytuł formantu logowania jest zbędny. W związku z tym wyczyszczenie [ `TitleText` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) wartość, aby można było usunąć tytuł formantu logowania.

Nazwa użytkownika: i hasło: etykiety na lewo od dwóch pól tekstowych można dostosować za pomocą [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) i [ `PasswordLabelText` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx)odpowiednio. Ta funkcja pozwala zmienić nazwę użytkownika: Etykieta można odczytać nazwy użytkownika:. Styl etykiety i pola tekstowego są konfigurowane za pomocą [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) i [ `TextBoxStyle` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx)odpowiednio.

Zapamiętaj mnie czasu wyboru następnego tekstu właściwość można ustawić za pomocą formantu logowania [ `RememberMeText property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx), i domyślnie zaznaczone, stanu jest konfigurowany za pomocą [ `RememberMeSet property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (który Wartość domyślna to False). Przejdź dalej i ustawić `RememberMeSet` właściwości na wartość True, dzięki czemu wyboru Zapamiętaj moje dane czasu obok będzie sprawdzana przez domyślny.

Kontrolka logowania oferuje dwie właściwości dostosowania układu jego formantów interfejsu użytkownika. [ `TextLayout property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) Wskazuje, czy nazwa użytkownika: i hasło: etykiety są wyświetlane po lewej stronie z ich odpowiednich pól tekstowych (ustawienie domyślne) lub nad nimi. [ `Orientation property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) Wskazuje, czy dane wejściowe użytkownika i hasło znajduje się w pionie (jeden nad drugim) lub w poziomie. Użyjemy Pozostaw te dwie właściwości ustaw wartości domyślne, ale I zachęca do spróbuj ustawić te dwie właściwości ich wartości innych niż domyślne, aby zobaczyć efekt wynikowy.

> [!NOTE]
> W następnej sekcji Konfigurowanie układ formantu logowania zajmiemy za pomocą szablonów do definiowania precyzyjnej układ elementów interfejsu użytkownika formant układu.


Zawijaj ustawienia właściwości formantu logowania przez ustawienie [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) i [ `CreateUserUrl` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) do nie zarejestrowano jeszcze? Tworzenie konta! i `~/Membership/CreatingUserAccounts.aspx`odpowiednio. To działanie powoduje dodanie hiperłącza do formantu logowania interfejsu wskazuje stronę utworzyliśmy w <a id="SKM6"> </a> [poprzedniego samouczek](creating-user-accounts-cs.md). Kontrolka logowania [ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) i [ `HelpPageUrl` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) i [ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) i [ `PasswordRecoveryUrl` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) pracy w taki sam sposób renderowania łącza strony pomocy i strony odzyskiwania hasła.

Po wprowadzeniu tych zmian właściwości, deklaratywne znaczników i wyglądu formantu logowania powinien wyglądać podobnie jak pokazano na rysunku 5.


[![Wartości właściwości formantu logowania jest wymagane przez jego wyglądu](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**Rysunek 5**: właściwości formantu logowania wartości wymagane przez jego wygląd ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Konfigurowanie układ formantu logowania

Formant sieci Web logowania domyślny interfejs użytkownika wychodzi poza interfejsu w formacie HTML `<table>`. Ale co zrobić, jeśli potrzebujemy bardziej precyzyjną kontrolę nad przetworzonych wyników? Może być chcemy zastąpić `<table>` z serii `<div>` tagów. Lub co w przypadku naszej aplikacji wymaga dodatkowych poświadczeń dla uwierzytelniania? Wiele witryn sieci Web finansowych, na przykład, użytkownicy muszą podać nie tylko nazwę użytkownika i hasło, ale również osobisty numer identyfikacyjny (PIN) lub inne informacje identyfikacyjne. Niezależnie od przyczyny mogą być, jest możliwe do przekonwertowania formantu logowania szablon, w którym firma Microsoft jawnie zdefiniować znaczników deklaracyjnego interfejsu.

Należy wykonać dwie czynności, aby można było zaktualizować formantu logowania do zbierania dodatkowych poświadczeń:

1. Zaktualizuj interfejs formantu logowania uwzględnienie doświadczeniach kontrolnych sieci Web służąca do gromadzenia dodatkowych poświadczeń.
2. Zastąpienie formantu logowania uwierzytelniania wewnętrznego logiki tak, aby użytkownik jest uwierzytelniany tylko, jeśli ich nazwy użytkownika i hasła jest prawidłowa, a ich dodatkowe poświadczenia są prawidłowe, za.

Aby osiągnąć pierwszego zadania, musimy przekonwertować formantu logowania na szablon i Dodaj formanty sieci Web, konieczne. Podobnie jak w przypadku drugiego zadania logika uwierzytelniania formantu logowania może zostać zastąpiony przez tworzenie obsługi zdarzeń dla formantu [ `Authenticate` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx).

Teraz zaktualizuj formantu logowania tak, aby z prośbą o ich nazwy użytkownika, hasło i adres e-mail i uwierzytelnia użytkownika tylko, jeśli adres e-mail podany adres e-mail użytkownika w pliku jest zgodny. Najpierw musimy przekonwertować interfejsu kontroli logowania do szablonu. Tag inteligentny formant logowania wybierz konwersji do opcji szablonu.


[![Konwertowanie formantu logowania do szablonu](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**Rysunek 6**: konwertowanie formantu logowania do szablonu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))


> [!NOTE]
> Aby przywrócić formantu logowania wstępnie template wersji, kliknij łącze resetowania tagu formantu.


Konwertowanie do szablonu formantu logowania dodaje `LayoutTemplate` do znaczników deklaratywne kontrolki z elementów HTML i formantów sieci Web Definiowanie interfejsu użytkownika. Jak pokazano na rysunku 7, konwersja formant na szablon usuwa wiele właściwości w oknie właściwości, takie jak `TitleText`, `CreateUserUrl`, itd., ponieważ wartości tych właściwości są ignorowane przy użyciu szablonu.


[![Mniej właściwości są dostępne podczas kontroli logowania jest konwertować na szablon](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**Rysunek 7**: mniej właściwości są dostępne podczas kontroli logowania jest konwertować na szablon ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))


Kod znaczników HTML w `LayoutTemplate` może być modyfikowany. Podobnie możesz także dodać żadnych nowych formantów sieci Web do szablonu. Jednak ważne jest formantów sieci Web core tego formantu logowania pozostają w szablonie i zachowanie ich przypisanym `ID` wartości. W szczególności nie Usuń lub zmień `UserName` lub `Password` pól tekstowych, `RememberMe` wyboru `LoginButton` przycisku `FailureText` etykietę, lub `RequiredFieldValidator` formantów.

Aby zbierać odwiedzającego adresu e-mail, musimy dodać pole tekstowe do szablonu. Dodaj następujący kod deklaratywne między wiersza tabeli (`<tr>`) zawierający `Password` pole tekstowe i wiersza tabeli, który przechowuje Zapamiętaj mnie obok czasu wyboru:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

Po dodaniu `Email` pole tekstowe, odwiedź stronę za pośrednictwem przeglądarki. Jak pokazano na rysunku 8, interfejs użytkownika logowania formantu zawiera teraz trzecie pole tekstowe.


[![Kontrolka logowania zawiera teraz pole tekstowe adresu E-mail użytkownika](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**Rysunek 8**: formant logowania teraz zawiera pole tekstowe adresu E-mail użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))


W tym momencie nadal używa formantu logowania `Membership.ValidateUser` metody można sprawdzić poprawności podanych poświadczeń. Odpowiednio wartość wprowadzona w `Email` pole tekstowe nie ma wpływu na tego, czy użytkownik może zalogować. W kroku 3 przedstawiono, jak zastąpić logika uwierzytelniania formantu logowania, aby poświadczenia tylko są uznawane za prawidłowe, jeśli nazwa użytkownika i hasło są prawidłowe i adres e-mail podany dopasowania dla adresu e-mail na plik.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Krok 3: Modyfikowanie logika uwierzytelniania formantu logowania

Jeśli użytkownik udostępnia jej realizowany przez jego przepływu pracy uwierzytelniania kontrolę poświadczeń i kliknięć, który ensues przycisk Zaloguj odświeżania strony i nazwy logowania. Przepływ pracy jest uruchamiany przez zwiększenie [ `LoggingIn` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Wszystkie skojarzone z tym zdarzeniem procedury obsługi zdarzeń może anulować dziennika w operacji przez ustawienie `e.Cancel` właściwości `true`.

Jeśli dziennik w operacji nie zostało anulowane, przepływ pracy przechodzi przez zwiększenie [ `Authenticate` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Jeśli program obsługi zdarzeń dla `Authenticate` zdarzeń, to jest odpowiedzialny za określenie, czy podane poświadczenia są prawidłowe. Jeśli określono bez obsługi zdarzeń, używa logowania kontroli `Membership.ValidateUser` metody do sprawdzenia poprawności poświadczeń.

Jeśli podane poświadczenia są prawidłowe, a następnie utworzeniu biletu uwierzytelniania formularzy, [ `LoggedIn` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) jest zgłaszane, a użytkownik zostanie przekierowany do odpowiedniej strony. Miarę, jednak poświadczenia są nieprawidłowe, a następnie [ `LoginError` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) jest wywoływane i zostanie wyświetlony komunikat użytkownik dowie się, że ich poświadczenia były nieprawidłowe. Domyślnie w przypadku niepowodzenia logowania kontroli ustawia jego `FailureText` etykiety formantu właściwości tekst komunikatu o błędzie (próba logowania zakończyła się niepowodzeniem. Spróbuj ponownie). Jednak jeśli formant logowania [ `FailureAction` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) ma ustawioną wartość `RedirectToLoginPage`, następnie problemów formantu logowania `Response.Redirect` do strony logowania dołączanie parametr querystring `loginfailure=1` (co spowoduje, że logowanie formantu, aby wyświetlić komunikat o błędzie).

Rysunek 9 udostępnia schemat blokowy przepływu pracy uwierzytelniania.


[![Przepływ uwierzytelniania formantu logowania](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**Rysunek 9**: przepływ uwierzytelniania logowania kontroli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))


> [!NOTE]
> Jeśli są zastanawiasz się, gdy używasz `FailureAction`w `RedirectToLogin` Strona opcji, należy rozważyć następujący scenariusz. W tej chwili naszych `Site.master` strony wzorcowej ma obecnie tekst Hello, stranger wyświetlany w lewej kolumnie po odwiedzeniu użytkownik anonimowy, ale Wyobraź sobie możemy Zamień tekst formantu logowania. Dzięki temu użytkownik anonimowy zalogować się z dowolnej strony w witrynie, nie wymagając od nich bezpośrednio odwiedź stronę logowania. Jednak jeśli użytkownik nie może zalogować się za pomocą formantu logowania przez strony wzorcowej, może być uzasadnione na przekierowanie do strony logowania (`Login.aspx`), ponieważ prawdopodobnie strony zawiera dodatkowe instrukcje, łączy i innych pomoc — takie jak łącza do utworzenia nowe konto lub pobrać utraconego hasła — które nie zostały dodane do strony wzorcowej.


### <a name="creating-theauthenticateevent-handler"></a>Tworzenie`Authenticate`obsługi zdarzeń

Aby można było dołączyć logiki uwierzytelniania niestandardowego, należy utworzyć program obsługi zdarzeń dla formantu logowania `Authenticate` zdarzeń. Tworzenie programu obsługi zdarzeń dla `Authenticate` zdarzeń wygeneruje następującej definicji programu obsługi zdarzeń:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

Jak widać, `Authenticate` program obsługi zdarzeń jest przekazywany obiekt typu [ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) jako drugi parametr wejściowy. `AuthenticateEventArgs` Klasy zawiera właściwości typu Boolean o nazwie `Authenticated` używany do określenia, czy podane poświadczenia są prawidłowe. Nasze zadania, wówczas można zapisać tutaj kod, który określa, czy podane poświadczenia są prawidłowe i można ustawić `e.Authenticate` właściwości odpowiednio.

### <a name="determining-and-validating-the-supplied-credentials"></a>Określanie i sprawdzanie poprawności podanych poświadczeń

Użyj formantu logowania [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) i [ `Password` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) można określić nazwę użytkownika i hasło poświadczeń wprowadzonych przez użytkownika. Aby było możliwe określenie wartości wprowadzone do wszelkich dodatkowych formantów sieci Web (takie jak `Email` pole tekstowe dodaliśmy w poprzednim kroku), użyj *`LoginControlID`* `.FindControl`("*`controlID`*") można uzyskać programowy odwołanie do formantu sieci Web w szablonie którego `ID` właściwości jest równa *`controlID`*. Na przykład, aby pobrać odwołanie do `Email` pola tekstowego, użyj następującego kodu:

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

Aby zweryfikować poświadczenia użytkownika, należy wykonać dwie czynności:

1. Upewnij się, że podana nazwa użytkownika i hasło są prawidłowe
2. Sprawdź, czy wprowadzony adres e-mail zgodny adres e-mail na potrzeby użytkownika prób zalogowania

Do wykonania wyboru pierwszy, po prostu wykorzystamy `Membership.ValidateUser` metody, takie jak widzieliśmy w kroku 1. Dla drugiego wyboru należy określić adres e-mail użytkownika, dzięki czemu możemy porównaj je z adresu e-mail, wprowadzone w formancie TextBox. Aby uzyskać informacje na temat konkretnego użytkownika, należy użyć `Membership` klasy [ `GetUser` metody](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx).

`GetUser` Metoda ma kilka przeciążeń. Jeśli używane bez przekazywanie żadnych parametrów, zwraca informacje o aktualnie zalogowanego użytkownika. Aby uzyskać informacje na temat konkretnego użytkownika, należy wywołać `GetUser` przekazując ich nazwy użytkownika. W obu przypadkach `GetUser` zwraca [ `MembershipUser` obiektu](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), który zawiera właściwości, takie jak `UserName`, `Email`, `IsApproved`, `IsOnline`i tak dalej.

Poniższy kod implementuje tych dwóch kontroli. Jeśli zarówno pomyślnie, następnie `e.Authenticate` ustawiono `true`, w przeciwnym razie jest ona przypisana `false`.

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

Ten kod w miejscu prób zalogowania się jako prawidłowego użytkownika, wprowadzając prawidłową nazwę użytkownika, hasło i adres e-mail. Spróbuj ponownie, ale tym razem celowo używać niepoprawnego adresu e-mail (zobacz rysunek 10). Na koniec spróbuj go raz trzeci przy użyciu nazwy użytkownika nie istnieje. W pierwszym przypadku użytkownik powinien być pomyślnie zalogowany do lokacji, ale w ostatnich dwóch przypadków powinna zostać wyświetlona wiadomość nieprawidłowe poświadczenia formantu logowania.


[![Tito nie zalogować się w przypadku dostarczenia parametru niepoprawnego adresu E-mail](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**Na rysunku nr 10**: Tito nie dziennika w przypadku podawania niepoprawny adres E-mail ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))


> [!NOTE]
> Zgodnie z opisem w sekcji jak członkostwo w ramach obsługuje nieprawidłowy prób logowania w kroku 1, gdy `Membership.ValidateUser` metoda jest wywoływana i przekazano nieprawidłowe poświadczenia, jego śledzi próba nieprawidłowego logowania i blokuje użytkownika, jeśli przekroczą określony Próg nieudanych prób podania w przedziale czasu. Od naszych wywołania logiki uwierzytelniania niestandardowego `ValidateUser` metody niepoprawnego hasła dla prawidłowej nazwy użytkownika powoduje zwiększenie licznika próba nieprawidłowego logowania, ale ten licznik nie jest zwiększany, w przypadku, gdy nazwa użytkownika i hasło są prawidłowe, ale adres e-mail jest nieprawidłowy. To, to zachowanie jest odpowiedni, ponieważ jest mało prawdopodobne, że haker będzie znać nazwy użytkownika i hasła, ale ma korzystać siłowych metod, aby określić adres e-mail użytkownika.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Krok 4: Zwiększanie formantu logowania nieprawidłowymi poświadczeniami komunikatu

Gdy użytkownik próbuje się zalogować z nieprawidłowymi poświadczeniami, formantu logowania wyświetla komunikat z informacjami o tym, że próba logowania nie powiodła się. W szczególności kontrolka wyświetla komunikat określony przez jego [ `FailureText` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), który ma wartość domyślną równą próba logowania zakończyła się niepowodzeniem. Spróbuj ponownie.

Odwołaj się, że istnieje wiele przyczyn, dlaczego poświadczeń użytkownika może być nieprawidłowy:

- Nazwa użytkownika może nie istnieć.
- Nazwa użytkownika istnieje, ale hasło jest nieprawidłowe
- Nazwa użytkownika i hasło są prawidłowe, ale użytkownik nie jest jeszcze zatwierdzone.
- Nazwa użytkownika i hasło są prawidłowe, ale użytkownik jest zablokowany out (prawdopodobnie ponieważ one przekroczona liczba nieudanych prób logowania w określonym przedziale czasu)

I może być inne przyczyny, używając logiki uwierzytelniania niestandardowego. Na przykład z kodem napisaliśmy w kroku 3, nazwa użytkownika i hasło, może być nieprawidłowa, ale adres e-mail może być nieprawidłowa.

Niezależnie od tego, dlaczego poświadczenia są nieprawidłowe kontrola logowania Wyświetla sam komunikat o błędzie. Ten brak opinii może być mylące dla użytkownika, którego konto nie ma jeszcze zatwierdzone lub który zostało zablokowane. Z niewielki pracy jednak można mamy formantu logowania wyświetli komunikat bardziej odpowiednie.

Zawsze, gdy użytkownik próbuje zalogować się z nieprawidłowymi poświadczeniami zgłasza formantu logowania jego `LoginError` zdarzeń. Przejdź dalej i utworzyć program obsługi zdarzeń dla tego zdarzenia i Dodaj następujący kod:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

Powyższy kod rozpoczyna się przez ustawienie sterowania logowania `FailureText` właściwości na wartość domyślną (próba logowania zakończyła się niepowodzeniem. Spróbuj ponownie). Następnie sprawdza, czy wprowadzona nazwa użytkownika mapy do istniejącego konta użytkownika. Jeśli tak, sprawdza powstałe w ten sposób `MembershipUser` obiektu `IsLockedOut` i `IsApproved` właściwości w celu określenia, czy konto zostało zablokowane i nie ma jeszcze zatwierdzone. W obu przypadkach `FailureText` właściwość została zaktualizowana do odpowiadającej jej wartości.

Do przetestowania tego kodu, celowo spróbować zalogować się jako istniejącego użytkownika, ale niepoprawne hasło. Nie tym pięć razy pod rząd w okresie 10 minut, a konto zostanie zablokowane. Jak przedstawiono rysunek 11, logowania kolejnych prób będzie zawsze zakończyć się niepowodzeniem (nawet w przypadku prawidłowego hasła), ale teraz bardziej opisowe Twoje konto zostało zablokowane z powodu zbyt wielu nieudanych prób logowania. Skontaktuj się z administratorem, aby wiadomości odblokować konto.


[![Tito wykonana zbyt wiele nieudanych prób logowania, a zostało zablokowane](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**Rysunek 11**: Tito wykonywane zbyt wiele nieprawidłowych prób logowania ma został zablokowany do edycji i ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))


## <a name="summary"></a>Podsumowanie

Wcześniejsze kroki tego samouczka, naszą stronę logowania sprawdzić poprawności podanych poświadczeń z wpisaną na stałe listą par nazwa użytkownika i hasło. W tym samouczku Zaktualizowaliśmy stronę, aby zweryfikować poświadczenia w ramach członkostwa. W kroku 1 analizujemy przy użyciu `Membership.ValidateUser` metody programowo. W kroku 2 możemy zastąpione naszych interfejsu ręcznie utworzonego użytkownika i kodu za pomocą formantu logowania.

Kontrolka logowania renderuje interfejsu użytkownika standardowego logowania i automatycznie sprawdza poprawność poświadczeń użytkownika przed framework członkostwa. Ponadto w przypadku prawidłowe poświadczenia formantu logowania loguje się użytkownik za pomocą uwierzytelniania formularzy. Krótko mówiąc środowisko użytkownika funkcjonalnej logowania jest dostępna po prostu przeciągając formantu logowania na strony, nie bardzo deklaratywne znaczników lub kod niezbędne. Co więcej, formantu logowania jest dużym stopniu dostosowywane, co zapewnia małe stopień kontroli nad zarówno renderowanych interfejsu i uwierzytelniania logikę użytkownika.

W tym momencie odwiedzających naszą witrynę sieci Web można utworzyć nowego konta użytkownika i zaloguj do witryny, ale firma Microsoft nie zostały jeszcze przyjrzeć się ograniczenie dostępu do stron na podstawie uwierzytelnionego użytkownika. Obecnie każdy użytkownik, uwierzytelnieni lub anonimowi, można wyświetlić dowolną stronę w naszej witrynie. Wraz z kontroli dostępu do naszej witrynie strony na podstawie użytkownika przez użytkownika, firma Microsoft może być określonych stron, na których funkcji zależy od użytkownika. Następny samouczek wygląda jak ograniczyć dostęp i funkcji w strony na podstawie zalogowanego użytkownika.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Wyświetlanie niestandardowych komunikatów do zablokowane i niezatwierdzonych użytkowników](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Badanie ASP.NET 2.0 na członkostwo, role i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Porady: Tworzenie strony logowania programu ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Dokumentacja techniczna formantu logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Korzystanie z kontrolek logowania](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Bento, Utwórz wiele książek ASP/ASP.NET i twórcę 4GuysFromRolla.com, pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki  *[Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott jest osiągalny w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Teresa Murphy i Michael Olivero. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Poprzednie](creating-user-accounts-cs.md)
> [dalej](user-based-authorization-cs.md)
