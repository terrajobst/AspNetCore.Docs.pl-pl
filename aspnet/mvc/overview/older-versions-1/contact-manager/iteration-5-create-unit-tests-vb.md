---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: 'Iteracja #5 – testów jednostkowych Utwórz (VB) | Dokumentacja firmy Microsoft'
author: microsoft
description: W piątym iteracji możemy ułatwić naszej aplikacji obsługiwanie i modyfikowanie przez dodanie testów jednostkowych. Firma Microsoft mock naszej klasy modelu danych i tworzenie testów jednostkowych dla o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: fe59792a1e1a7950a318e7e893b3da12d53a8efa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="iteration-5--create-unit-tests-vb"></a>Iteracja #5 — Tworzenie testów jednostkowych (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> W piątym iteracji możemy ułatwić naszej aplikacji obsługiwanie i modyfikowanie przez dodanie testów jednostkowych. Firma Microsoft mock naszej klasy modelu danych i tworzenie testów jednostkowych dla naszych kontrolerów i logiki sprawdzania poprawności.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji ASP.NET MVC kontaktu administracyjnego (VB)

W tej serii samouczków budujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacji skontaktuj się z Menedżera umożliwia przechowywanie informacji kontaktowych - nazwy, numerów telefonów i adresów e-mail — lista osób.

Firma Microsoft kompilowania aplikacji za pośrednictwem przejść przez wiele iteracji. Przy każdej iteracji firma Microsoft stopniowego zwiększenia aplikacji. Celem tego podejścia wiele iteracji jest ułatwia zrozumienie przyczyn, dla każdej zmiany.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy menedżera kontaktu w najprostszym sposobem możliwe. Możemy dodać obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracji #2 — należy Szukaj nieuprzywilejowany aplikacji. W tym iteracji możemy ulepszyć wyglądu aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku programu ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie walidacji formularza. W trzecim iteracji dodamy podstawowej postaci weryfikacji. Firma Microsoft uniemożliwiać przesyłanie formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail i numerów telefonów.

- Iteracji #4 — należy luźno powiązane z aplikacji. W tym trzeci iteracji możemy korzystać z kilku wzorce projektowe oprogramowania do ułatwiają obsługiwanie i modyfikowanie aplikacji Menedżera skontaktuj się z pomocą. Na przykład firma Microsoft Refaktoryzuj naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 - tworzenia testów jednostkowych. W piątym iteracji możemy ułatwić naszej aplikacji obsługiwanie i modyfikowanie przez dodanie testów jednostkowych. Firma Microsoft mock naszej klasy modelu danych i tworzenie testów jednostkowych dla naszych kontrolerów i logiki sprawdzania poprawności.

- Iteracja #6 - użyj test-driven development. W tym szóstego iteracji dodania nowych funkcji do naszej aplikacji najpierw pisania testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 - dodawania funkcjonalności interfejsu Ajax. Siódmego iteracji możemy ulepszyć czas reakcji i wydajności aplikacji, dodając obsługę technologii Ajax.


## <a name="this-iteration"></a>Tej iteracji

W poprzedniej iteracji aplikacji menedżera kontaktu zrefaktoryzowaliśmy aplikacji do luźno sprzężenia. Firma Microsoft rozdzielone aplikacji w różnych kontrolera, usług i warstwy repozytorium. Każda warstwa współdziała z warstwy poniżej za pośrednictwem interfejsów.

Zrefaktoryzowaliśmy aplikacji, aby ułatwić obsługiwanie i modyfikowanie aplikacji. Na przykład jeśli potrzebne do używania nowej technologii dostępu do danych, możemy wystarczy zmienić warstwę repozytorium bez użycia kontrolera lub warstwy usługi. Dokonując Contact Manager luźno, możemy Zapisz wprowadzone aplikacji bardziej odporne na zmiany.

Ale co się stanie, gdy będzie trzeba dodać nową funkcję do Menedżera skontaktuj się z aplikacji? Lub, co się dzieje, gdy firma Microsoft naprawa błędów? Sad, ale również sprawdzonych prawdy pisania kodu jest zawsze, gdy touch kodu utworzyć ryzyko wprowadzenia nowych usterek.

Na przykład jeden dzień poprawnie, Menedżer może poprosić o dodanie nowej funkcji Menedżera skontaktuj się z pomocą. Użytkownik chce, aby dodać obsługę skontaktuj się z grupy. Użytkownik chce, aby umożliwić użytkownikom zorganizować swoje kontakty w grupy znajomych, praca i tak dalej.

W celu wdrożenia tej nowej funkcji, należy zmodyfikować wszystkie trzy warstwy aplikacji menedżera kontaktu. Należy dodać nowe funkcje do kontrolerów, warstwy usług i repozytorium. Jak uruchomić, modyfikowania kodu, istnieje ryzyko, krytyczne funkcje, które działały przed.

Refaktoryzacja naszej aplikacji do osobnych warstw, jak robiliśmy w poprzedniej iteracji została zdaje. Został on zdaje, ponieważ pozwala wprowadzać zmiany do całej warstwy, nie ingerując w pozostałej części aplikacji. Jednak jeśli chcesz ułatwiają obsługiwanie i modyfikowanie kodu w warstwie, musisz utworzyć testy jednostek dla kodu.

Możesz użyć jednostki teście jednostka kodu. Te jednostki kodu są mniejsze niż warstwy całej aplikacji. Zazwyczaj Użyj testu jednostkowego, aby sprawdzić, czy konkretnej metody w kodzie działa w oczekiwany sposób. Na przykład należy utworzyć testu jednostkowego dla metody CreateContact() udostępniane przez klasę ContactManagerService.

Testy jednostkowe dla aplikacji działają tylko takie jak zabezpieczenie na. Zawsze, gdy należy zmodyfikować kod w aplikacji, możesz uruchomić zestaw testów jednostkowych, aby sprawdzić, czy modyfikacja dzieli istniejące funkcje. Testy jednostkowe uczynienia kodu bezpieczne do zmodyfikowania. Testy jednostkowe Sprawdź wszystkie kodu w aplikacji bardziej odporne na zmiany.

W tym iteracji dodania do naszej aplikacji kontaktów Menedżerze testów jednostkowych. W ten sposób w następnej iteracji, firma Microsoft można dodać grupy skontaktuj się z naszej aplikacji bez obaw o fundamentalne istniejące funkcje.

> [!NOTE] 
> 
> Brak różnych platform, na przykład NUnit, xUnit.net i MbUnit testowania jednostek. W tym samouczku używamy jednostki testowania framework dołączony do programu Visual Studio. Jednak może równie łatwo przy użyciu jednej z tych platform alternatywnych.


## <a name="what-gets-tested"></a>Pobiera co przetestowane

W idealnych cały kod byłyby objęte testów jednostkowych. W idealnych trzeba doskonałe zabezpieczenie. Użytkownik będzie mógł zmodyfikować w każdym wierszu kodu w aplikacji i natychmiast będzie znana, wykonując testy jednostkowe, czy zmiana spowodowało przerwanie istniejące funkcje.

Jednak firma Microsoft ADAM t na żywo w idealnych. W praktyce podczas pisania testów jednostkowych skoncentrowane na pisania testów dla logiki biznesowej (na przykład logikę weryfikacji). W szczególności należy *nie* pisanie testów jednostkowych dla danych dostęp logiki lub logiki widoku.

Powinna być użyteczna, testy jednostkowe musi być wykonywany bardzo szybko. Możesz łatwo może spowodować zgromadzenie setki (lub nawet tysiące) Testy jednostkowe dla aplikacji. Jeśli testy jednostkowe zająć dużo czasu do uruchomienia, a następnie będzie uniknąć wykonywania ich. Innymi słowy długotrwałą testy jednostkowe są bezużyteczne kodów dnia na dzień.

Z tego powodu należy zwykle nie zapisuj testy jednostek dla kodu, który współdziała z bazy danych. Uruchamianie setki testy jednostkowe na bazie danych na żywo będzie zbyt wolno. Zamiast tego mock bazy danych i pisania kodu, który współdziała z zasymulować bazy danych (omówimy mocking bazy danych poniżej).

Z powodu podobne zwykle nie pisania testów jednostkowych dla widoków. W celu przetestowania widoku, musi pokrętła serwera sieci web. Ponieważ Obracająca serwera sieci web jest stosunkowo powolne procesu, nie zaleca się tworzenia testów jednostkowych dla widoków.

Jeśli widok zawiera logikę skomplikowane, następnie należy rozważyć przeniesienie logikę do metody pomocnicze. Można pisać testy jednostkowe dla metody pomocnicze, które zostanie wykonane bez Obracająca serwera sieci web.

> [!NOTE] 
> 
> Podczas pisania testów dla logika dostępu do danych lub widok nie jest dobrze podczas pisania testów jednostkowych, te testy może być bardzo przydatne podczas tworzenia funkcjonalności i integracji testów.


> [!NOTE] 
> 
> ASP.NET MVC jest aparat widoku formularzy sieci Web. Gdy aparat widoku formularzy sieci Web jest zależny od serwera sieci web, może nie być innych aparatów widoku.


## <a name="using-a-mock-object-framework"></a>Przy użyciu platformy zasymulować obiektu

Podczas tworzenia testów jednostkowych, prawie zawsze konieczne korzystanie z platformy Mock obiektu. Framework Mock obiektu umożliwia tworzenie mocks i klas zastępczych dla klas w aplikacji.

Na przykład można użyć framework Mock obiektu do generowania wersję makiety klasy repozytorium. W ten sposób można zasymulować repozytorium klasy zamiast klasy rzeczywistych repozytorium w testy jednostkowe. Przy użyciu repozytorium zasymulować pozwala uniknąć wykonywania kodu bazy danych podczas wykonywania testu jednostkowego.

Program Visual Studio nie ma framework Mock obiektu. Istnieją jednak kilka struktur Mock obiektu źródłowego handlowych i otwórz dostępne dla programu .NET framework:

1. Moq — ta struktura jest dostępna w ramach licencji BSD typu open source. Możesz pobrać Moq z [ https://code.google.com/p/moq/ ](https://code.google.com/p/moq/).
2. Rhino Mocks — ta struktura jest dostępny w ramach licencji BSD typu open source. Możesz pobrać Rhino Mocks z [ http://ayende.com/projects/rhino-mocks.aspx ](http://ayende.com/projects/rhino-mocks.aspx).
3. Odłączenia Typemock — jest to komercyjnych framework. Możesz pobrać wersję próbną z [ http://www.typemock.com/ ](http://www.typemock.com/).

W tym samouczku I chcę korzystać z Moq. Jednak może równie łatwo używać Rhino Mocks lub odłączenia Typemock makiety utworzyć obiektów dla aplikacji, skontaktuj się z Menedżera.

Przed użyciem Moq, należy wykonać następujące czynności:

1. .
2. Przed Rozpakuj pobierania, upewnij się, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk oznaczony **Odblokuj** (zobacz rysunek 1).
3. Rozpakuj pobierania.
4. Dodaj odwołanie do zestawu Moq do projektu testowego przez wybranie opcji menu **projekt, Dodaj odwołanie** otworzyć **Dodaj odwołanie** okna dialogowego. Na karcie przeglądania, przejdź do folderu, gdzie można rozpakować Moq i wybierz zestaw Moq.dll. Kliknij przycisk **OK** przycisk (patrz rysunek 2).


[![Odblokowywanie Moq](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Rysunek 01**: odblokowywania Moq ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-5-create-unit-tests-vb/_static/image2.png))


[![Odwołania po dodaniu Moq](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Rysunek 02**: odwołania po dodaniu Moq ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-5-create-unit-tests-vb/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Tworzenie testów jednostkowych dla warstwy usług

Let s, Rozpocznij od utworzenia zestawu testów jednostkowych dla warstwy usług s naszych Menedżera skontaktuj się z aplikacji. Aby sprawdzić logiki sprawdzania poprawności użyjemy tych testów.

Utwórz nowy folder o nazwie modeli w projekcie ContactManager.Tests. Następnie kliknij prawym przyciskiem myszy folder modeli i wybierz **Dodaj, nowego testu**. **Dodaj nowy Test** zostanie wyświetlone okno dialogowe pokazany na rysunku 3. Wybierz **testu jednostkowego** szablonu i nazwy nowego testu ContactManagerServiceTest.vb. Kliknij przycisk **OK** przycisk, aby dodać nowy test do projektu Test.

> [!NOTE] 
> 
> Ogólnie rzecz biorąc mają strukturę folderu projektu testu w taki sposób, aby dopasować struktury folderu projektu programu ASP.NET MVC. Na przykład umieścić kontrolera testów w folderze kontrolerów, testy modelu w folderze modeli i tak dalej.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Rysunek 03**: Models\ContactManagerServiceTest.cs ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-5-create-unit-tests-vb/_static/image6.png))


Początkowo chcemy testowanie metody CreateContact() udostępniane przez klasę ContactManagerService. Utworzymy pięć testów poniżej:

- CreateContact() — testy tego CreateContact() zwraca wartość true, gdy prawidłowy kontakt jest przekazywany do metody.
- CreateContactRequiredFirstName() - testów, że komunikat o błędzie został dodany do stanu modelu, gdy kontakt z Brak imię jest przekazywany do metody CreateContact().
- CreateContactRequredLastName() - testów, że komunikat o błędzie został dodany do stanu modelu, gdy kontakt z Brak nazwisko jest przekazywany do metody CreateContact().
- CreateContactInvalidPhone() - testów, że komunikat o błędzie został dodany do stanu modelu, w przypadku kontaktu z nieprawidłowym numerem telefonu jest przekazywany do metody CreateContact().
- CreateContactInvalidEmail() - testów, że komunikat o błędzie został dodany do stanu modelu, gdy kontakt z nieprawidłowy adres e-mail jest przekazywany do metody CreateContact()...

Pierwszego testu sprawdza prawidłowy kontakt nie generuje błąd sprawdzania poprawności. Pozostałe testy Sprawdź każdy z reguł sprawdzania poprawności.

Kod dla tych testów znajduje się w 1 wyświetlania.

**Listing 1 - Models\ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]


Ponieważ używamy klasy skontaktuj się z 1 wyświetlania musimy Dodaj odwołanie do narzędzia Entity Framework firmy Microsoft do naszych projektu testowego. Dodaj odwołanie do zestawu System.Data.Entity.


Wyświetlanie listy 1 zawiera metodę o nazwie Initialize(), który zostanie nadany atrybut [TestInitialize]. Ta metoda jest wywoływana automatycznie przed uruchomieniem każdego z testów jednostkowych (jest to 5 razy bezpośrednio przed każdym testów jednostkowych). Metodę Initialize() tworzy zasymulować repozytorium, korzystając z poniższego kodu:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Ten wiersz kodu używa Moq framework do generowania interfejsu IContactManagerRepository zasymulować repozytorium. Zasymulować repozytorium jest używany zamiast rzeczywistej EntityContactManagerRepository Aby uniknąć uzyskiwania dostępu do bazy danych, po uruchomieniu każdego z testów jednostkowych. Repozytorium zasymulować implementuje metody interfejsu IContactManagerRepository, ale t Jan metody faktycznie podejmować żadnych działań.

> [!NOTE] 
> 
> Używając Moq framework jest różnica między \_mockRepository i \_mockRepository.Object. Pierwsza odwołuje się do klasy makiety (z IContactManagerRepository), która zawiera metody do określania zachowania zasymulować repozytorium. Drugie odwołuje się do rzeczywistego zasymulować repozytorium, który implementuje interfejs IContactManagerRepository.


Podczas tworzenia wystąpienia klasy ContactManagerService zasymulować repozytorium są objęte metodę Initialize(). Testy jednostkowe poszczególnych używają tego wystąpienia klasy ContactManagerService.

Wyświetlanie listy 1 zawiera pięć metody, które odpowiadają każdego z testów jednostkowych. Każda z tych metod zostanie nadany atrybut [TestMethod]. Po uruchomieniu testów jednostkowych dowolnej metody, która ma ten atrybut jest wywoływana. Innymi słowy dowolnej metody, która zostanie nadany atrybut [TestMethod] jest testu jednostkowego.

Pierwszy testu jednostkowego, o nazwie CreateContact(), sprawdza, czy wywołanie CreateContact() zwraca wartość true, jeśli prawidłowe wystąpienie klasy kontaktu jest przekazywany do metody. Test tworzy wystąpienie klasy kontaktu, wywołuje metodę CreateContact() i sprawdza, czy CreateContact() zwraca wartość true.

Pozostałe testy Sprawdź po wywołaniu metody CreateContact() z nieprawidłową kontaktu następnie metoda zwraca wartość false, a komunikat o błędzie weryfikacji oczekiwano zostanie dodany do stanu modelu. Na przykład CreateContactRequiredFirstName() test tworzy wystąpienie klasy skontaktuj się z pustym ciągiem znaków dla właściwości jego imię. Następnie metoda CreateContact() jest wywoływana z nieprawidłowy kontaktu. Na koniec test sprawdza, czy CreateContact() zwraca wartość false i że stanu modelu zawiera komunikat o błędzie weryfikacji oczekiwano "wymagane jest imię."

Można uruchomić testów jednostkowych w 1 wyświetlania przez wybranie opcji menu **, uruchomienie testu, wszystkie testy z rozwiązania (CTRL + R, A)**. Wyniki testów są wyświetlane w oknie wyników testu (patrz rysunek 4).


[![Wyniki testu](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Rysunek 04**: wyniki testu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-5-create-unit-tests-vb/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Tworzenie testów jednostkowych dla kontrolerów

Aplikacji ASP.NET MVC sterowanie przepływem interakcji z użytkownikiem. Podczas testowania kontrolera, chcesz sprawdzić, czy kontroler zwraca dane wynik i widok prawym akcji. Można również sprawdzić, czy kontroler współdziała z klasami modelu w oczekiwany sposób.

Na przykład wyświetlanie 2 zawiera dwa testów jednostkowych dla kontrolera skontaktuj się z metody Create(). Pierwszy testu jednostkowego sprawdza, czy podczas prawidłowy kontakt jest przekazywany do metody Create(), a następnie przekierowuje metody Create() do akcji indeksu. Innymi słowy po upływie prawidłowy kontakt Create() metoda powinna zwrócić RedirectToRouteResult, reprezentujący akcji indeksu.

Firma Microsoft ADAM t chcesz przetestować ContactManager warstwy usług, gdy testujemy warstwy kontrolera. W związku z tym możemy mock warstwy usług z następujący kod w metodzie zainicjować:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

W CreateValidContact() testu jednostkowego firma Microsoft mock zachowanie wywoływania warstwy usług CreateContact() metody za pomocą następującego kodu:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Ten wiersz kodu powoduje zasymulować usługi ContactManager zwraca wartość true po wywołaniu metody CreateContact(). Przez mocking warstwy usług, firma Microsoft testuje zachowanie kontrolera, bez konieczności wykonywania kodu w warstwie usługi.

Drugi testu jednostkowego sprawdza, czy akcja Create() błędny kontakt jest przekazywany do metody, zwraca Utwórz widok. Firma Microsoft spowodować warstwy usług CreateContact() metoda zwraca wartość false, korzystając z poniższego kodu:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Jeśli Metoda Create() zachowuje się jak oczekujemy, następnie go powinien zwrócić widok Tworzenie warstwy usług zwrócona wartość false. W ten sposób kontroler można wyświetlić komunikaty o błędach weryfikacji w widoku Create, a użytkownik ma możliwość Popraw tego nieprawidłowe właściwości skontaktuj się z pomocą.


Jeśli planujesz tworzenie testów jednostkowych dla kontrolerów należy zwrócić jawne wyświetlanie nazw z akcji kontrolera. Na przykład zwraca widok następująco:

Zwraca View()

Zamiast tego należy zwrócić widok następująco:

Zwraca View("Create")

Jeśli nie ma jawnego wracając widoku właściwość ViewResult.ViewName zwraca pusty ciąg.


**Wyświetlanie listy 2 - Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Podsumowanie

W tym iteracji utworzyliśmy testów jednostkowych dla aplikacji Menedżer kontaktu. Możemy uruchomić te testy jednostkowe w dowolnym momencie, aby sprawdzić, czy naszej aplikacji nadal działa w taki sposób, który oczekuje. Testy jednostkowe działa jako zabezpieczenie, aby nasza aplikacja pozwala nam bezpiecznie modyfikowania aplikacji w przyszłości.

Utworzyliśmy dwa zestawy testów jednostkowych. Najpierw przetestowaliśmy logiki sprawdzania poprawności, tworząc testów jednostkowych dla warstwy naszych usług. Następnie przetestowaliśmy logiki kontroli przepływu, tworząc testów jednostkowych dla naszych warstwy kontrolera. Podczas testowania naszych warstwy usług, firma Microsoft samodzielnie Nasze testy naszych warstwy usług z naszych warstwy repozytorium przez mocking warstwy naszym repozytorium. Podczas testowania warstwy kontrolera, możemy samodzielnie testów dla naszych warstwy kontrolera, przez mocking warstwy usług.

W następnej iteracji firma Microsoft zmodyfikuj aplikacji skontaktuj się z Menedżera tak, aby obsługuje skontaktuj się z grupy. Ta nowa funkcja zostanie dodany do naszej aplikacji przy użyciu o nazwie test-driven development, proces projektowania oprogramowania.

> [!div class="step-by-step"]
> [Poprzednie](iteration-4-make-the-application-loosely-coupled-vb.md)
> [dalej](iteration-6-use-test-driven-development-vb.md)
