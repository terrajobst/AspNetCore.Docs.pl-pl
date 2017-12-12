---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: "Iteracja #6 — użyj test-driven development (VB) | Dokumentacja firmy Microsoft"
author: microsoft
description: "W tym szóstego iteracji dodania nowych funkcji do naszej aplikacji najpierw pisania testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b558df9c0b44f5f76115270d361b6022658f9f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="iteration-6--use-test-driven-development-vb"></a>Iteracja #6 — użyj test-driven development (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> W tym szóstego iteracji dodania nowych funkcji do naszej aplikacji najpierw pisania testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.


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

W poprzedniej iteracji aplikacji menedżera kontaktu utworzyliśmy testów jednostkowych do zapewnienia zabezpieczenie na naszego kodu. Motywacją do tworzenia testów jednostkowych było naszego kodu bardziej odporne na zmiany. Testy jednostkowe w miejscu możemy happily wprowadzenie zmian do naszego kodu i od razu wiedzieć, czy możemy przerwane istniejące funkcje.

W tym iteracji używamy testów jednostkowych w zupełnie innego celu. W tej iteracji, testy jednostkowe używany jako część aplikacji zasady projektowania klas o nazwie *test-driven development,*. Ćwiczenie test-driven development, należy najpierw zapisać testy i następnie pisać kod dla testów.

Ściślej, gdy ćwiczenia test-driven development, istnieją trzy kroki, które zostaną wykonane podczas tworzenia kodu (czerwony / zielony/Zrefaktoryzuj):

1. Zapis testu jednostkowego, który zakończy się niepowodzeniem (czerwony)
2. Pisanie kodu, który przekazuje testu jednostkowego (zielony)
3. Zrefaktoryzuj kod (Zrefaktoryzuj)

Najpierw należy zapisać testu jednostkowego. Test jednostkowy powinien express zamiaru zachowanie dla jak ma być kodu. Po utworzeniu testu jednostkowego testu jednostkowego powinna zakończyć się niepowodzeniem. Test powinna zakończyć się niepowodzeniem, ponieważ nie zostały jeszcze zapisane kodu aplikacji, który spełnia testu.

Następnie należy napisać kod wystarczającego w kolejności dla testu jednostkowego do przekazania. Celem jest pisanie kodu w sposób laziest, sloppiest oraz najszybciej możliwe. Należy nie marnować czas planowania architektury aplikacji. Zamiast tego należy skoncentrować się na zapisywanie minimalnej ilości kodu niezbędne do spełnienia zamiar wyrażonych przez test jednostkowy.

Na koniec po wystarczającej ilości kodu zostały zapisane, możesz ponownie krok i należy wziąć pod uwagę ogólna Architektura aplikacji. W tym kroku przepisywania (zrefaktoryzuj) kodu dzięki wykorzystaniu projektowania oprogramowania wzorce — takich jak wzorzec repozytorium — tak, aby kod jest łatwy w obsłudze więcej. Kod w tym kroku można przepisać fearlessly, ponieważ kod jest objęta testów jednostkowych.

Istnieje wiele korzyści wynikających z test-driven development, korzystających z metodologii. Po pierwsze, test-driven development wymusza pozwala skupić się na kod, który faktycznie do zapisania. Ponieważ są stale koncentruje się na właśnie pisanie wystarczającej ilości kodu do przekazania poszczególnego testu, będą mogli wandering do gąszcz i zapisywanie olbrzymich ilości kodu, który nigdy nie będzie używany.

Po drugie metodologię projektowania "test najpierw" wymusza napisać kod z punktu widzenia użycia kodu. Innymi słowy gdy ćwiczenia test-driven development, stale pisania testów z punktu widzenia użytkownika. W związku z tym test-driven development, może spowodować czyszczący i bardziej zrozumiałe interfejsów API.

Na koniec test-driven development, wymusza pisać testy jednostkowe w ramach normalnego procesu pisania aplikacji. Jak zbliża się termin projektu testowania jest zwykle najpierw, który jest przesyłany okno. Podczas ćwiczeń test-driven development z drugiej strony, możesz są bardziej prawdopodobną virtuous temat pisania testów jednostkowych, ponieważ test-driven development, sprawia, że testy jednostkowe centralnej do procesu tworzenia aplikacji.

> [!NOTE] 
> 
> Aby dowiedzieć się więcej na temat test-driven development, I zaleca się przeczytanie książki piór Michael **pracy skutecznie z kodem starszych**.


W tym iteracji dodania do naszej aplikacji Menedżera skontaktuj się z nowej funkcji. Możemy dodać obsługę skontaktuj się z grupy. Można użyć, skontaktuj się z grup do organizowania na kategorie, takie jak firm kontaktów i grup Friend.

Ta nowa funkcja zostanie dodany do naszej aplikacji w procesie projektowania opartego na test. Firma Microsoft będzie najpierw zapisać Nasze testy jednostkowe i firma Microsoft będzie zapisanie wszystkich naszego kodu względem tych testów.

## <a name="what-gets-tested"></a>Pobiera co przetestowane

Jak wspomniano w poprzedniej iteracji, zwykle nie pisania testów jednostkowych dla logika dostępu do danych lub wyświetlić logiki. Zrobisz testów jednostkowych zapisu t logika dostępu do danych, ponieważ uzyskanie dostępu do bazy danych jest stosunkowo wolne działanie. Zrobisz testów jednostkowych zapisu t logiki widoku, ponieważ dostęp do widoku wymaga Obracająca się z serwerem sieci web, który jest stosunkowo wolne działanie. T nie powinien zapisać testu jednostkowego, chyba że testu mogą być wykonywane wielokrotnie bardzo szybkiej

Ponieważ test-driven development, wynikają z testów jednostkowych, możemy skupić się początkowo o pisaniu kontrolera i logiki biznesowej. Firma Microsoft nie dotknięcie bazy danych lub widoków. Firma Microsoft kupione t modyfikacji bazy danych lub Utwórz widoki naszych aż do zakończenia bardzo tego samouczka. Możemy zaczynać co można przetestować.

## <a name="creating-user-stories"></a>Tworzenie przypadków użycia

Podczas ćwiczeń test-driven development, należy zawsze uruchomić pisząc testu. Natychmiast zgłasza to pytanie: jak użytkownik zdecyduje jakie test, aby najpierw zapisać? Aby odpowiedzieć na to pytanie, należy zapisać zestaw [ *scenariuszy użytkownika*](http://en.wikipedia.org/wiki/User_stories).

Scenariusza użytkownika jest bardzo krótki opis wymaganie dotyczące oprogramowania (zazwyczaj jedno zdanie). Należy go nietechnicznych opis wymagań zapisywane z punktu widzenia użytkownika.

S zbiór scenariuszy użytkownika, które opisują funkcje wymagane przez nowe funkcje grupy skontaktuj się z tutaj:

1. Użytkownika można wyświetlić listę grup kontaktów.
2. Użytkownik może utworzyć nowej grupy kontaktów.
3. Użytkownik może usunąć istniejącą grupę kontaktu.
4. Użytkownik może wybrać grupy kontaktów, podczas tworzenia nowego kontaktu.
5. Użytkownik może wybrać grupy kontaktów podczas edycji istniejącego kontaktu.
6. Lista grup kontaktów jest wyświetlany w widoku indeksu.
7. Gdy użytkownik kliknie grupy kontaktów, zostanie wyświetlona lista zgodnych kontaktów.

Należy zauważyć, że ta lista scenariuszy użytkownika jest całkowicie zrozumiały dla klienta. Nie ma nie wymieniono szczegóły implementacji technicznej.

Gdy podczas tworzenia aplikacji, zestaw scenariuszy użytkownika może stać się bardziej precyzyjnych. Wiele wątków (wymagania) może podzielić scenariusza użytkownika. Na przykład można zdecydować, że utworzenie nowej grupy kontaktów powinny obejmować sprawdzania poprawności. Przesyłanie grupy kontaktów bez nazwy powinien zwrócić błąd sprawdzania poprawności.

Po utworzeniu listę scenariuszy użytkownika, możesz przystąpić do pisania Twojego pierwszego testu jednostkowego. Zaczniemy od utworzenia testu jednostkowego do wyświetlania na liście grup kontaktów.

## <a name="listing-contact-groups"></a>Wyświetlanie listy grup kontaktów

Nasze pierwszego scenariusza użytkownika to, czy użytkownik powinien móc wyświetlić listę grup kontaktów. Musimy express tego wątku z testem.

Tworzenie nowego testu jednostkowego, klikając prawym przyciskiem myszy folder kontrolery w projekcie ContactManager.Tests wybranie **Add, przetestuj nowe**i wybierając **testu jednostkowego** szablonu (zobacz rysunek 1). Nazwa nowej jednostki testowanie GroupControllerTest.vb i kliknij przycisk **OK** przycisku.


[![Dodawanie testu jednostkowego GroupControllerTest](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Rysunek 01**: Dodawanie testu jednostkowego GroupControllerTest ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image2.png))


Nasze pierwszego testu jednostkowego znajduje się w 1 wyświetlania. Ten test sprawdza, czy metoda indeks() kontrolera grupy zwraca zbiór grup. Test sprawdza, czy kolekcję grup, jest zwracany w widoku danych.

**Wyświetlanie listy 1 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

Podczas wpisywania tekstu najpierw kod w wyświetlania 1 programu Visual Studio, uzyskasz dużo czerwoną linie dowolnym kształcie. Nie utworzono klasy GroupController lub grupy.

W tym momencie możesz nawet kompilacji t naszej aplikacji, dlatego firma Microsoft można wykonać naszych pierwszego testu jednostkowego. Tego dobrej s. Który traktowana jako niepowodzenie testu. W związku z tym teraz mamy uprawnień, aby rozpocząć pisanie kodu aplikacji. Należy zapisać za mało kod do wykonania naszym teście.

Klasa kontrolera grupy wyświetlania 2 zawiera podstawowe czynności kod wymagany do przekazania testu jednostkowego. Akcja indeks() zwraca listę statycznie kodowane grupy (klasy grupy jest zdefiniowany w wyświetlania 3).

**Wyświetlanie listy 2 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**Wyświetlanie listy 3 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Po dodamy klasy GroupController i grupy do naszej projektu naszych pierwszego testu jednostkowego zakończy się pomyślnie (patrz rysunek 2). Firma Microsoft to zostało zrobione minimalna pracy wymaganej w celu przekazania testu. Nadszedł czas udanej sprzedaży.


[![Powodzenie!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Rysunek 02**: sukcesu! () [Kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image4.png))


## <a name="creating-contact-groups"></a>Tworzenie grup kontaktów

Teraz możesz teraz przystąpić do drugiego scenariusza użytkownika. Potrzebujemy można było tworzyć nowe grupy kontaktów. Musimy express zamiar z testem.

Testu w listę 4 sprawdza, czy wywołanie Create() metody z nową grupą dodaje do listy grup zwracany przez metodę indeks() grupę. Innymi słowy Jeśli I utworzyć nową grupę następnie I powinien móc odzyskać nową grupę z listy grup zwracany przez metodę indeks().

**Wyświetlanie listy 4 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

Testu w listę 4 wywołuje kontrolera grupy Create() metody za pomocą nowego skontaktuj się z grupą. Następnie testu sprawdza, czy wywołanie kontrolera grupy indeks() — metoda zwraca nową grupę w widoku danych.

Zmodyfikowane kontrolera grupy w listę 5 zawiera podstawowe czynności zmiany wymagane do przekazania nowego testu.

**Wyświetlanie listy 5 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Kontroler grupy w listę 5 ma nową akcję Create(). Ta akcja dodaje grupę do kolekcji grup. Zwróć uwagę, że akcja indeks() został zmodyfikowany w celu zwrócenia zawartości kolekcję grup.

Ponownie firma Microsoft wykonali systemu od zera minimalna ilość pracy wymaganej w celu przekazania testu jednostkowego. Po możemy wprowadzić te zmiany do kontrolera grupy, wszystkie jednostki Nasze testy przebiegu.

## <a name="adding-validation"></a>Dodawanie walidacji

To wymaganie nie zostało określone jawnie w scenariusza użytkownika. Istnieje uzasadnione wymagają, aby grupa miał nazwę. W przeciwnym razie organizowanie kontakty do grupy nie będzie bardzo przydatne.

Wyświetlanie listy 6 zawiera określającym zamiar nowego testu. Ten test sprawdza, czy próby utworzenia grupy bez podawania nazwy spowoduje komunikat o błędzie weryfikacji w stanie modelu.

**Wyświetlanie listy 6 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

W celu spełnienia tego testu, należy dodać właściwość nazwy do klasy nasze grupy (zobacz listę 7). Ponadto należy dodać niewielki rozmiar nieco logikę weryfikacji do kontrolera grupy s Create() akcji (patrz lista 8).

**Wyświetlanie listy 7 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**Wyświetlanie listy 8 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Należy zauważyć, że kontroler grupy akcji Create() teraz zawiera logikę zarówno sprawdzania poprawności, jak i bazy danych. Obecnie bazy danych używanej przez grupę kontroler składa się z nic więcej niż kolekcji w pamięci.

## <a name="time-to-refactor"></a>Czas do refaktoryzacji

Trzeci krok w Zrefaktoryzuj-czerwony/zielony jest częścią refaktoryzacji. W tym momencie musimy kroku z naszego kodu i należy wziąć pod uwagę, jak firma Microsoft Refaktoryzuj naszej aplikacji, aby zwiększyć jego projekt. W ramach refaktoryzacji jest na etapie, jaką naszym zdaniem twardym o najlepszym sposobem stosowania zasad projektowania oprogramowania i wzorce.

Firma Microsoft mogą zmodyfikować naszego kodu w żaden sposób wybranego zwiększające projekt kodu. Mamy zabezpieczenie testów jednostkowych, które uniemożliwiają nam fundamentalne istniejące funkcje.

Teraz, kontrolera grupy jest bałagan z punktu widzenia dobre projektu. Kontroler grupy zawiera plątaniną bałagan sprawdzania i kod dostępu do danych. Aby uniknąć naruszenia jednej zasady odpowiedzialności, musimy podzielić te problemy na różnych klas.

Klasy kontrolera nasze refactored grupy znajduje się w wyświetlania 9. Kontroler został zmodyfikowany w celu użycia ContactManager warstwy usługi. Jest to tej samej warstwie usługi korzystające z skontaktuj się z kontrolerem.

Wyświetlanie listy 10 zawiera nowych metod dodane do warstwy usług ContactManager do obsługi sprawdzania poprawności, wyświetlania i tworzenia grup. Interfejs IContactManagerService został zaktualizowano w celu uwzględnienia nowych metod.

Wyświetlanie listy 11 zawiera nową klasę FakeContactManagerRepository, który implementuje interfejs IContactManagerRepository. W odróżnieniu od klasy EntityContactManagerRepository, który też implementuje interfejs IContactManagerRepository naszej nowej klasy FakeContactManagerRepository nie komunikują się z bazą danych. Klasa FakeContactManagerRepository korzysta z kolekcji w pamięci, a jako serwer proxy dla bazy danych. Użyjemy tej klasy w naszych testów jednostkowych jako warstwa fałszywych repozytorium.

**Wyświetlanie listy 9 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**Wyświetlanie listy 10 - Controllers\ContactManagerService.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**Wyświetlanie listy 11 - Controllers\FakeContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]


Modyfikowanie IContactManagerRepository wymaga interfejsu używać do implementowania CreateGroup() i ListGroups() metody w klasie EntityContactManagerRepository. Laziest i najszybszym sposobem na tym jest dodawanie szkieletu metody, które wyglądają następująco:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]


Ponadto te zmiany do projektu aplikacji wymagają nam w tworzeniu modyfikacji niektórych testów jednostkowych. Teraz musisz użyć FakeContactManagerRepository podczas przeprowadzania testów jednostkowych. Zaktualizowano klasy GroupControllerTest znajduje się w wyświetlania 12.

**Wyświetlanie listy 12 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Po wykonujemy wszystkie te zmiany, ponownie, wszystkich naszych przebiegu testów jednostkowych. Firma Microsoft zakończył cały cykl Zrefaktoryzuj-czerwony/zielony. Wdrożyliśmy pierwszy dwóch scenariuszy. Mamy teraz obsługi testów jednostkowych dla wymagań wyrażone w scenariuszy użytkownika. Wdrażanie w pozostałej części scenariuszy użytkownika obejmuje powtarzające się tego samego cyklu Zrefaktoryzuj-czerwony/zielony.

## <a name="modifying-our-database"></a>Modyfikowanie naszej bazie danych

Niestety mimo że firma Microsoft zostały spełnione wszystkie wymagania dotyczące wyrażonych przez nasze testy jednostek, pracę nie została wykonana. Nadal trzeba zmodyfikować naszej bazie danych.

Należy utworzyć nową tabelę bazy danych grupy. Wykonaj następujące kroki:

1. W oknie Eksploratora serwera, kliknij prawym przyciskiem myszy folder Tabele i wybierz opcję menu **Dodaj nową tabelę**.
2. Wprowadź dwie kolumny opisane poniżej w Projektancie tabel.
3. Oznacz jako klucz podstawowy i kolumny tożsamości w kolumnie identyfikator.
4. Zapisz nową tabelę przy użyciu nazwy grup, klikając ikonę dyskietek.

<a id="0.12_table01"></a>


| **Nazwa kolumny** | **Typ danych** | **Dopuszcza wartości null** |
| --- | --- | --- |
| Identyfikator | int | False |
| Nazwa | nvarchar(50) | False |


Następnie należy usunąć wszystkie dane z tabeli kontaktów (w przeciwnym razie możemy won t można utworzyć relacji między tabelami kontaktów i grup). Wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy tabeli kontaktów i wybierz opcję menu **Pokaż dane tabeli**.
2. Usuń wszystkie wiersze.

Następnie należy zdefiniować relacji między tabelą bazy danych grupy i istniejącej tabeli kontaktów w bazie danych. Wykonaj następujące kroki:

1. Kliknij dwukrotnie tabelę Kontakty w oknie Eksploratora serwera, aby otworzyć projektanta tabel.
2. Dodaj nową kolumnę całkowitą do tabeli kontaktów o nazwie identyfikatora grupy.
3. Kliknij przycisk relacji, aby otworzyć okno dialogowe relacje klucza obcego (patrz rysunek 3).
4. Kliknij przycisk Dodaj.
5. Kliknij przycisk wielokropka obok przycisku tabeli i specyfikacja kolumn.
6. W oknie dialogowym tabele i kolumny wybierz grupę jako tabeli klucza podstawowego i identyfikator jako kolumna klucza podstawowego. Wybierz kontakty jako tabela klucza obcego i GroupId jako kolumna klucza obcego (patrz rysunek 4). Kliknij przycisk OK.
7. W obszarze **INSERT i UPDATE specyfikacji**, wybierz wartość **Cascade** dla **Usuń regułę**.
8. Kliknij przycisk Zamknij, aby zamknąć okno dialogowe relacje klucza obcego.
9. Kliknij przycisk Zapisz, aby zapisać zmiany w tabeli kontaktów.


[![Tworzenie relacji tabeli bazy danych](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Rysunek 03**: tworzenie relacji tabeli bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image6.png))


[![Określanie relacji między tabelami](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Rysunek 04**: Określanie relacji między tabelami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image8.png))


### <a name="updating-our-data-model"></a>Aktualizowanie modelu danych

Następnie należy zaktualizować nasz model danych do reprezentowania nowej tabeli bazy danych. Wykonaj następujące kroki:

1. Kliknij dwukrotnie plik ContactManagerModel.edmx w folderze modele, aby otworzyć program Entity Designer.
2. Kliknij prawym przyciskiem myszy powierzchnię projektanta i wybierz opcję menu **modelu aktualizacji z bazy danych**.
3. W Kreatorze aktualizacji wybierz grupy tabeli i kliknij przycisk Zakończ przycisk (patrz rysunek 5).
4. Kliknij prawym przyciskiem myszy obiekt grupy, a następnie wybierz opcję menu **zmienić**. Zmień nazwę *grup* jednostki do *grupy* (w liczbie pojedynczej).
5. Kliknij prawym przyciskiem myszy właściwość nawigacji grupy, która pojawia się w dolnej części obiektu Kontakt. Zmień nazwę *grup* właściwości nawigacji do *grupy* (w liczbie pojedynczej).


[![Aktualizowanie modelu programu Entity Framework z bazy danych](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Rysunek 05**: aktualizowanie modelu programu Entity Framework z bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image10.png))


Po wykonaniu tych kroków modelu danych będzie reprezentować kontaktów i grup tabel. Obie te jednostki powinny być widoczne w programie Entity Designer (patrz rysunek 6).


[![Wyświetlanie grupy i skontaktuj się z programu Entity Designer](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Rysunek 06**: wyświetlanie grupy i skontaktuj się z programu Entity Designer ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Tworzenie klas naszym repozytorium

Następnie należy zaimplementować klasy nasze repozytorium. W trakcie tej iteracji dodano kilka nowych metod interfejsu IContactManagerRepository podczas pisania kodu do zaspokojenia naszych testów jednostkowych. Z ostateczną wersją interfejsu IContactManagerRepository znajduje się w wyświetlania 14.

**Wyświetlanie listy 14 - Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

Firma Microsoft był t faktycznie dowolnej z metod powiązany z pracą z grup kontaktów w naszym rzeczywistych klasy EntityContactManagerRepository zaimplementowana. Klasa EntityContactManagerRepository ma obecnie, metod klasy zastępczej dla każdej z metod grupy kontaktów wyświetlane w interfejsie IContactManagerRepository. Na przykład metoda ListGroups() obecnie wygląda następująco:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

Metody klasy zastępczej włączone nam kompilowania aplikacji i przekazania testów jednostkowych. Jednak teraz nadszedł czas na faktycznie zaimplementować te metody. Z ostateczną wersją klasy EntityContactManagerRepository znajduje się w 13 wyświetlania.

**Wyświetlanie listy 13 - Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Tworzenie widoków

Aplikacji ASP.NET MVC, korzystając z domyślny aparat widoku programu ASP.NET. Tak ADAM t tworzyć widoki w odpowiedzi na test określonej jednostki. Jednakże, ponieważ aplikacja będzie bezużyteczny bez widoków, możemy t zakończyć bez tworzenia i modyfikowania widoków zawartych w aplikacji Menedżera skontaktuj się z tą iterację.

Należy utworzyć następujące nowe widoki zarządzania grup kontaktów (patrz rysunek 7):

- Views\Group\Index.aspx - Wyświetla listę grup kontaktów
- Views\Group\Delete.aspx - Wyświetla formularza Potwierdzenie usuwania grupy kontaktów


[![Widok indeksu grupy](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Rysunek 07**: widok indeksu grupy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image14.png))


Należy zmodyfikować następujące istniejących widoków, aby zawierały grup kontaktów:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Widać zmodyfikowanych widoków analizując aplikacji Visual Studio, dołączony w tym samouczku. Na przykład rysunku 8 przedstawiono widok indeksu kontaktów.


[![Widok indeksu kontaktów](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Rysunek 08**: widok indeksu kontaktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-vb/_static/image16.png))


## <a name="summary"></a>Podsumowanie

W tym iteracji dodaliśmy nowe funkcje do naszej aplikacji kontaktów Menedżerze wykonując test-driven development, metodologię projektowania aplikacji. Możemy uruchomić dzięki utworzeniu zestawu scenariuszy użytkownika. Utworzono zestaw testów jednostkowych zgodne z wymogami wyrażona scenariuszy użytkownika. Na koniec napisaliśmy wystarczającego kodu by spełnić ich wymagań wyrażonych przez testy jednostkowe.

Po zakończeniu pisania kodu za mało by spełnić ich wymagań wyrażonych przez testy jednostek, możemy zaktualizować bazę danych i widoków. Możemy dodać nową tabelę grup do naszej bazie danych i zaktualizować naszych modelu danych programu Entity Framework. Możemy również tworzone i modyfikowane zestaw widoków.

W następnej iteracji — iteracji końcowego--możemy przepisywania naszej aplikacji, aby móc korzystać z technologii Ajax. Dzięki wykorzystaniu technologii Ajax, możemy zwiększyć czas reakcji i wydajności aplikacji, skontaktuj się z Menedżera.

>[!div class="step-by-step"]
[Poprzednie](iteration-5-create-unit-tests-vb.md)
[dalej](iteration-7-add-ajax-functionality-vb.md)
