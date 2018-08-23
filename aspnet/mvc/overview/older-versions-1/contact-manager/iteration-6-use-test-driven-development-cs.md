---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: 'Iteracja #6 — korzystanie z projektowania opartego na testach (C#) | Dokumentacja firmy Microsoft'
author: microsoft
description: W tym szóstego iteracji dodamy nowe funkcje do naszej aplikacji, najpierw pisanie testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: 3c4358a1b979ab95d8ac25551e21ee95d75e5eae
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752289"
---
<a name="iteration-6--use-test-driven-development-c"></a>Iteracja #6 — korzystanie z projektowania opartego na testach (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz program Code](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> W tym szóstego iteracji dodamy nowe funkcje do naszej aplikacji, najpierw pisanie testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Tworzenie aplikacji zarządzania kontaktami platformy ASP.NET MVC (C#)
  

W tej serii samouczków wbudowujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazwy, numerów telefonów i adresów e-mail — lista osób.

Firma Microsoft tworzy aplikację za pośrednictwem wiele iteracji. Z każdą iteracją można stopniowo ulepszyć aplikację. Celem tego wielu podejścia iteracji jest, aby umożliwić Ci zrozumienie przyczyn wprowadzenia poszczególnych zmian.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy Contact Manager w najprostszym sposobem możliwe. Dodano obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracja 2 # — należy wyglądu nieuprzywilejowany aplikacji. W tej iteracji możemy poprawić wygląd aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku platformy ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie weryfikacji formularza. W trzecim iteracji dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail oraz numerów telefonów.

- Iteracja 4 # — należy luźne sprzężenie aplikacji. W tym czwarty iteracji możemy korzystać z kilku wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład możemy refaktoryzować naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 — Tworzenie testów jednostkowych. W piątej iteracji ułatwiamy naszej aplikacji ułatwia konserwację i modyfikowanie, dodając testów jednostkowych. Firma Microsoft testowanie naszych zajęć modelu danych i tworzenie testów jednostkowych dla naszych kontrolery i logikę weryfikacji.

- Iteracja #6 — korzystanie z projektowania opartego na testach. W tym szóstego iteracji dodamy nowe funkcje do naszej aplikacji, najpierw pisanie testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 — dodawanie funkcji Ajax. W siódmej iteracji można ulepszyć czas odpowiedzi i wydajności naszych aplikacji przez dodanie obsługi technologii AJAX.

## <a name="this-iteration"></a>Tej iteracji

W poprzedniej iteracji aplikacji Contact Manager utworzyliśmy testów jednostkowych w celu zapewnienia zabezpieczenie naszego kodu. Motywacją do tworzenia testów jednostkowych było naszego kodu bardziej odporne na zmiany. Przy użyciu testów jednostkowych w miejscu możemy trafem korzysta wprowadzać zmian do naszego kodu i od razu wiedzieć, czy możemy przerwane istniejących funkcji.

W tej iteracji testów jednostkowych jest używana w zupełnie innego celu. W tej iteracji używamy testy jednostkowe, jako część filozofia projektowania aplikacji o nazwie *programowania sterowanego testami*. Podczas programowania sterowanego testami praktyczne w sytuacji, należy najpierw pisania testów, a następnie napisz kod dla testów.

Mówiąc ściślej, gdy ćwiczenia programowania sterowanego testami, istnieją trzy kroki, które zostaną wykonane podczas tworzenia kodu (czerwony / zielony/Zrefaktoryzuj):

1. Napisać test jednostkowy, który zakończy się niepowodzeniem (czerwony)
2. Napisać kod, który przekazuje testu jednostkowego (zielony)
3. Refaktoryzacja kodu (Refaktoryzacja)

Po pierwsze można napisać test jednostkowy. Test jednostkowy powinien wyrażać zamiaru dla jak oczekujesz, że Twój kod będzie działać. Po utworzeniu testu jednostkowego test jednostkowy powinna zakończyć się niepowodzeniem. Test powinna zakończyć się niepowodzeniem, ponieważ nie zostały jeszcze zapisane wszelki kod aplikacji, który spełnia testu.

Następnie zapisz wystarczający tylko kod, aby test jednostkowy do przekazania. Celem jest pisanie kodu w sposób laziest, sloppiest i najszybszych możliwe. Należy nie tracić czasu myśleć o architekturze aplikacji. Zamiast tego należy skoncentrować się na pisaniu minimalnej ilości kodu, które są niezbędne do spełnienia zamiar wyrażona przez test jednostkowy.

Na koniec po napisaniu wystarczającej ilości kodu można krok do tyłu i należy wziąć pod uwagę ogólna Architektura aplikacji. W tym kroku edycji (Refaktoryzacja) kodu, wykorzystując projektowania oprogramowania wzorców — takich jak wzorzec repozytorium — tak, aby Twój kod będzie łatwiejszy w utrzymaniu. Odważnie można przepisać kod w tym kroku, ponieważ kodu jest objęta testów jednostkowych.

Istnieje wiele korzyści wynikających z ćwiczenia programowania sterowanego testami. Po pierwsze, oparte na testach programowanie wymusza pozwala skupić się na kod, który faktycznie potrzebny do zapisania. Ponieważ są stale koncentruje się na tylko pisania wystarczającej ilości kodu do przekazania konkretnego testu, będą mogli się wandering do gąszcz i zapisywanie ogromnych ilości kodu, który nigdy nie będą używane.

Po drugie metodologię projektowania "test najpierw" wymusza pisanie kodu pod względem sposobu użycia kodu. Innymi słowy gdy ćwiczenia programowania sterowanego testami, stale pisania testów z punktu widzenia użytkownika. W związku z tym programowania sterowanego testami może spowodować bardziej przejrzyste i bardziej zrozumiały interfejsów API.

Na koniec programowania sterowanego testami wymusza pisanie testów jednostkowych w ramach normalnego procesu pisania aplikacji. Jak zbliża się termin projektu badania jest zazwyczaj pierwszą rzeczą, która trafia do okna. Podczas ćwiczeń programowania sterowanego testami z drugiej strony, należy prawdopodobnie bardziej składa dotyczące pisania testów jednostkowych, ponieważ programowania sterowanego testami sprawia, że testy jednostkowe centralnej do procesu tworzenia aplikacji.

> [!NOTE] 
> 
> Aby dowiedzieć się więcej na temat programowania sterowanego testami, I zaleca się przeczytanie książki Michael Feathers **działa wydajnie z kodem Legacy**.


W tym iteracji dodamy nową funkcję do naszej aplikacji Contact Manager. Dodajemy obsługę skontaktuj się z grup. Można użyć, skontaktuj się z grup w celu zorganizowania kontaktów na kategorie, takie jak firmy i grupy Friend.

Dodamy tej nowej funkcji do naszej aplikacji postępując zgodnie z procesem programowania sterowanego testami. Nasze testy jednostkowe będziemy pisać najpierw oboje będziemy pisać wszystkich naszych kodować te testy.

## <a name="what-gets-tested"></a>Co pobiera przetestowane

Jak wspomniano w poprzedniej iteracji, zazwyczaj nie pisać testy jednostkowe dla logiką dostępu do danych ani wyświetlić logiki. Komputer t pisanie testów jednostkowych logiką dostępu do danych, ponieważ dostęp do bazy danych jest stosunkowo wolne działanie. Komputer testów jednostkowych zapisu t logiki widoku, ponieważ uzyskanie dostępu do widoku wymaga uruchamiając serwera sieci web, która jest stosunkowo wolne działanie. T nie powinien napisać test jednostkowy, chyba że testu mogą być wykonywane wielokrotnie bardzo szybko

Ponieważ programowania sterowanego testami jest wymuszany przez testy jednostkowe, możemy skoncentrować się początkowo na pisaniu kontrolera i logiki biznesowej. Firma Microsoft należy unikać modyfikowania bazy danych lub widoki. Zdobyliśmy t modyfikacji bazy danych lub Utwórz naszych widoków do bardzo końca tego samouczka. Rozpoczniemy pracę co mogą być testowane.

## <a name="creating-user-stories"></a>Tworzenie przypadków użycia

Podczas ćwiczeń programowania sterowanego testami należy zawsze rozpoczyna się od pisania testu. Natychmiast generuje to pytanie: decyzja dotycząca określenia jakiego test, aby najpierw zapisać? Aby odpowiedzieć na to pytanie, należy napisać zestaw [ **przypadków**](http://en.wikipedia.org/wiki/User_stories).

Scenariusz użycia jest bardzo krótki opis (zwykle w jednym zdaniu) wymagania dotyczące oprogramowania. Powinna to być Nietechniczne opis wymaganie zapisywane z punktu widzenia użytkownika.

Oto s zestaw przypadków użycia, które opisano funkcje wymagane przez nowe funkcje skontaktuj się z grupy:

1. Użytkownik może wyświetlić listę grup kontaktów.
2. Użytkownik może utworzyć nową grupę skontaktuj się z pomocą.
3. Użytkownik może usunąć istniejącą grupę skontaktuj się z pomocą.
4. Użytkownik może wybrać grupy kontaktów, podczas tworzenia nowego kontaktu.
5. Użytkownik może wybrać grupy kontaktów podczas edycji istniejącego kontaktu.
6. Lista grup skontaktuj się z pomocą jest wyświetlany w widoku indeksu.
7. Gdy użytkownik kliknie skontaktuj się z grupy, zostanie wyświetlona lista pasujących kontaktów.

Należy zauważyć, że ta lista historii użytkowników jest całkowicie zrozumiały dla klienta. Nie ma żadnych informację o szczegóły implementacji technicznej.

Gdy procesie tworzenia aplikacji, zestaw przypadków użycia może stać się bardziej precyzyjnych. Scenariusz użycia może podzielić na wiele wątków (wymagania). Na przykład można zdecydować, czy tworzenie nowej grupy skontaktuj się z pomocą powinny obejmować sprawdzania poprawności. Przesyłanie skontaktuj się z grupy bez nazwy powinien zwrócić błąd sprawdzania poprawności.

Po utworzeniu listy historii użytkowników, można przystąpić do pisania pierwszego testu jednostkowego. Zaczniemy od utworzenia testu jednostkowego do wyświetlania listy grup skontaktuj się z pomocą.

## <a name="listing-contact-groups"></a>Wyświetlanie listy grup kontaktów

Nasz pierwszy Historia użytkownika jest, czy użytkownik powinien móc wyświetlić listę grup kontaktów. Musimy express tego wątku z testem.

Tworzenie nowego testu jednostkowego, klikając prawym przyciskiem myszy folder kontrolerów w projekcie ContactManager.Tests wybierając **Dodaj, testowanie nowych**i wybierając polecenie **testów jednostkowych** szablonu (patrz rysunek 1). Nazwa nowej jednostki testowania GroupControllerTest.cs i kliknij przycisk **OK** przycisku.


[![Dodawanie testu jednostkowego GroupControllerTest](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)

**Rysunek 01**: Dodawanie GroupControllerTest testu jednostkowego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-cs/_static/image2.png))


Pierwszy test jednostki znajduje się w ofercie 1. Ten test sprawdza, czy metoda indeks() kontrolera grupy zwraca zbiór grup. Ten test sprawdza, czy zbiór grup jest zwracany w widoku danych.

**Wyświetlanie listy 1 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

Podczas wpisywania tekstu najpierw kod w ofercie 1 w programie Visual Studio, zapewnisz sobie wiele czerwoną linią falistą. Nie utworzono GroupController lub grupę klas.

W tym momencie możemy kompilacji nawet t naszej aplikacji, dzięki czemu możemy t wykonać pierwszy test jednostki. Tego s jest dobra. Który jest traktowany jako niepowodzenie testu. W związku z tym, w efekcie powstał uprawnienia do rozpoczęcia pisania kodu aplikacji. Potrzebujemy do zapisu do wykonania naszym teście wystarczającej ilości kodu.

Klasa kontrolera grupy w ofercie 2 zawiera absolutnego minimum wymagane do przekazania testu jednostkowego kodu. Akcja indeks() zwraca statycznie kodowane listę grup (Klasa grupy jest zdefiniowana w ofercie 3).

**Wyświetlanie listy 2 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

**Wyświetlanie listy 3 - Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

Po dodamy klasy GroupController i grupy do naszego projektu pierwszy test jednostki zakończy się pomyślnie (patrz rysunek 2). Wykonaliśmy minimalne pracę wymaganą do przekazania do testu. Nadszedł czas na Świętuj.


[![SUKCES!](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)

**Rysunek 02**: Sukces! () [Kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-cs/_static/image4.png))


## <a name="creating-contact-groups"></a>Tworzenie grup kontaktów

Teraz można przystąpić do drugiego wątku użytkownika. Potrzebujemy można było tworzyć nowe grupy skontaktuj się z pomocą. Musimy express zamiar testu.

Testowanie w ofercie 4 — sprawdza, czy wywołanie Create() metody za pomocą nowej grupy dodaje do listy grup zwracany przez metodę indeks() grupę. Oznacza to czy jeśli utworzę nową grupę następnie I powinno być możliwe do wrócić nową grupę z listy grup zwracany przez metodę indeks().

**Wyświetlanie listy 4 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

Testowanie w ofercie 4 wywołuje kontrolera grupy Create() metody za pomocą nowego kontaktu grupy. Następnie ten test sprawdza, wywołanie kontrolera grupy metoda indeks() nową grupę w zwracanych danych widoku.

Zmodyfikowane kontrolera grupy w ofercie 5 zawiera podstawowe czynności zmiany wymagane w celu przekazania nowego testu.

**Wyświetlanie listy 5 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Kontroler grupy w ofercie 5 ma nową akcję Create(). Ta akcja dodaje grupę na kolekcję grup. Należy zauważyć, że akcja indeks() została zmodyfikowana, aby zwrócić zawartość kolekcję grup.

Jeszcze raz zostały wykonane bez minimalnej ilości pracy wymaganej do przekazania test jednostkowy. Po możemy wprowadzić te zmiany do kontrolera grupy, wszystkie jednostki Nasze testy zostaną zaliczone.

## <a name="adding-validation"></a>Dodawanie walidacji

To wymaganie nie zostało podane jawnie w historii użytkownika. Jednak jest uzasadnione wymagają, że grupa ma nazwy. W przeciwnym razie organizowanie kontaktów w grupach nie byłoby bardzo przydatne.

Wyświetlanie listy 6 zawiera nowy test, który wyraża swój zamiar. Ten test sprawdza, czy próba utworzenia grupy bez podawania nazwy skutkuje komunikat o błędzie weryfikacji w stanie modelu.

**Wyświetlanie listy 6 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

Aby spełnić wymagania tego testu, musimy dodać właściwość Nazwa klasy naszej grupy (zobacz listę 7). Ponadto należy dodać bitu niewielką logikę walidacji do kontrolera grupy s Create() akcji (patrz lista 8).

**Wyświetlanie listy 7 - Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

**Wyświetlanie listy 8 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

Należy zauważyć, że kontroler grupy akcji Create() teraz zawiera logikę weryfikacji i bazy danych. Obecnie usługa bazy danych używane przez kontroler grupy składa się z nic więcej niż kolekcji w pamięci.

## <a name="time-to-refactor"></a>Czas, Refaktoryzacja

Trzeci krok w czerwony/zielony/Zrefaktoryzuj jest częścią refaktoryzacji. W tym momencie należy kroku powrót po awarii z naszego kodu i należy wziąć pod uwagę, jak możemy refaktoryzować naszej aplikacji, aby zwiększyć jego zamysłem. Etap Zrefaktoryzuj jest etapu, jaką uważamy, że twarde o najlepszym sposobem stosowania zasad projektowania oprogramowania i wzorce.

Jesteśmy swobodnie modyfikować naszego kodu w dowolny sposób, którego możemy poprawić projektu kodu. Mamy projektowi testów jednostkowych, które uniemożliwiają nam przed przerwaniem działania istniejących funkcji.

W tej chwili, kontrolera grupy Miasto jest zakorkowane. z punktu widzenia projektowania oprogramowania dobre. Kontroler grupa zawiera plątaniną MES sprawdzania i kod dostępu do danych. Aby uniknąć naruszenia jednej zasady odpowiedzialności, należy rozdzielić te problemy na różnych klas.

Nasze refaktoryzować klasy kontrolera grupy znajduje się w ofercie 9. Kontroler został zmodyfikowany w celu użycia ContactManager warstwy usług. Jest to ten sam warstwy usług korzystających z skontaktuj się z kontrolerem.

Wyświetlanie listy 10 zawiera nowe metody, które są dodawane do warstwy usług ContactManager do obsługi sprawdzania poprawności, wyświetlanie listy i tworzenia grup. Interfejs IContactManagerService został zaktualizowany w celu uwzględnienia nowych metod.

Wyświetlanie listy 11 zawiera nową klasę FakeContactManagerRepository, który implementuje interfejs IContactManagerRepository. W odróżnieniu od klasy EntityContactManagerRepository, który także implementuje interfejs IContactManagerRepository naszej nowej klasy FakeContactManagerRepository nie komunikuje się z bazą danych. Klasa FakeContactManagerRepository korzysta z kolekcji w pamięci, a jako serwer proxy dla bazy danych. Użyjemy tej klasy w naszych testach jednostkowych jako warstwa fałszywych repozytorium.

**Wyświetlanie listy 9 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

**Lista 10 - Controllers\ContactManagerService.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

**Wyświetlanie listy 11 — Controllers\FakeContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

Modyfikowanie IContactManagerRepository wymaga interfejsu przy użyciu metody CreateGroup() i ListGroups() w klasie EntityContactManagerRepository. Laziest i najszybszy sposób to zrobić, jest Dodaj metody klasy zastępczej, które wyglądają następująco:   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]


Na koniec te zmiany w naszej aplikacji wymagać wprowadzenie pewnych zmian do naszych testów jednostkowych przez firmę Microsoft. Teraz musisz użyć FakeContactManagerRepository podczas przeprowadzania testów jednostkowych. Zaktualizowano klasy GroupControllerTest znajduje się w ofercie 12.

**Wyświetlanie listy 12 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

Po wszystkich tych udostępnimy zmienia, jeszcze raz wszystkich naszych przebieg testów jednostkowych. Ukończyliśmy całego cyklu czerwony/zielony/refaktoryzacji. Zaimplementowano rozwiązania tlp pierwsze dwa przypadki. W efekcie powstał Obsługa testów jednostkowych dla wymagań wyrażony w historii użytkowników. Implementowanie w pozostałej części przypadków użycia obejmuje powtarzające się tego samego cyklu czerwony/zielony/refaktoryzacji.

## <a name="modifying-our-database"></a>Modyfikowanie w naszej bazie danych

Niestety mimo że firma Microsoft zostały spełnione wszystkie wymagania dotyczące wyrażona przez nasze testy jednostkowe, praca odbywa się. Firma Microsoft nadal należy zmodyfikować w naszej bazie danych.

Należy utworzyć nową tabelę bazy danych grupy. Wykonaj następujące kroki:

1. W oknie Eksploratora serwera, kliknij prawym przyciskiem myszy folder Tabele i wybierz opcję menu **Dodaj nową tabelę**.
2. Wprowadź dwie kolumny, które są opisane poniżej w Projektancie tabel.
3. Oznacz jako klucz podstawowy i kolumny tożsamości kolumny identyfikatora.
4. Zapisz nową tabelę przy użyciu grup nazwy, klikając ikonę dyskietek.

<a id="0.11_table01"></a>


| **Nazwa kolumny** | **Typ danych** | **Zezwalaj na wartości null** |
| --- | --- | --- |
| Id | int | False |
| Nazwa | nvarchar(50) | False |


Następnie należy usunąć wszystkie dane z tabeli kontaktów (w przeciwnym razie zdobyliśmy t można było utworzyć relację między tabelami kontaktów i grup). Wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy tabeli kontaktów i wybierz opcję menu **Pokaż dane tabeli**.
2. Usuń wszystkie wiersze.

Następnie należy zdefiniować relacje między grupami tabeli bazy danych i istniejącej tabeli kontaktów w bazie danych. Wykonaj następujące kroki:

1. Kliknij dwukrotnie tabeli kontaktów w oknie Eksploratora serwera, aby otworzyć projektanta tabel.
2. Dodaj nowe kolumna liczb całkowitych do tabeli kontaktów, o nazwie GroupId.
3. Kliknij przycisk relacji, aby otworzyć okno dialogowe relacje klucza obcego (zobacz rysunek 3).
4. Kliknij przycisk Dodaj.
5. Kliknij przycisk wielokropka, który pojawia się obok przycisku tabel i kolumn specyfikacji.
6. W oknie dialogowym tabele i kolumny wybierz grupy jako tabeli klucza podstawowego i identyfikator jako kolumna klucza podstawowego. Wybierz kontakty jako tabela klucza obcego i GroupId jako kolumna klucza obcego (zobacz rysunek 4). Kliknij przycisk OK.
7. W obszarze **INSERT i UPDATE specyfikacji**, wybierz wartość **Cascade** dla **Usuń regułę**.
8. Kliknij przycisk Zamknij, aby zamknąć okno dialogowe relacje klucza obcego.
9. Kliknij przycisk Zapisz, aby zapisać zmiany do tabeli kontaktów.


[![Tworzenie relacji tabeli bazy danych](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)

**Rysunek 03**: tworzenie relacji tabeli bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-cs/_static/image6.png))


[![Określanie relacji między tabelami](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)

**Rysunek 04**: Określanie relacji między tabelami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-cs/_static/image8.png))


### <a name="updating-our-data-model"></a>Aktualizowanie nasz Model danych

Następnie należy zaktualizować nasz model danych do reprezentowania nowej tabeli bazy danych. Wykonaj następujące kroki:

1. Kliknij dwukrotnie plik ContactManagerModel.edmx w folderze modele, aby otworzyć projektanta jednostek.
2. Kliknij prawym przyciskiem myszy na powierzchnię projektanta i wybierz opcję menu **Model aktualizacji z bazy danych**.
3. W Kreatorze aktualizacji wybierz grupy tabeli i kliknij przycisk Zakończ przycisku (zobacz rysunek 5).
4. Kliknij prawym przyciskiem myszy jednostkę grupy, a następnie wybierz opcję menu **Zmień nazwę**. Zmień nazwę *grup* jednostki do *grupy* (pojedynczą).
5. Kliknij prawym przyciskiem myszy właściwość nawigacji grupy, która pojawia się w dolnej części jednostki Contact. Zmień nazwę *grup* właściwość nawigacji do *grupy* (pojedynczą).


[![Aktualizowanie modelu Entity Framework z bazy danych](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)

**Rysunek 05**: aktualizowanie modelu Entity Framework z bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-cs/_static/image10.png))


Po wykonaniu tych kroków, od modelu danych będzie reprezentować tabel kontaktów i grup. W Projektancie jednostki powinny być widoczne oba jednostki (patrz rysunek 6).


[![Projektant ekranu, wyświetlania grupy i skontaktuj się z](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)

**Rysunek 06**: Projektant jednostki wyświetlania grupy i skontaktuj się z pomocą ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-cs/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Tworzenie klas w naszym repozytorium

Następnie należy do zaimplementowania klasy nasze repozytorium. W trakcie tej iteracji dodaliśmy kilka nowych metod interfejsu IContactManagerRepository podczas pisania kodu do zaspokojenia Nasze testy jednostkowe. Ostateczna wersja interfejsu IContactManagerRepository znajduje się w ofercie 14.

**Wyświetlanie listy 14 - Models\IContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

Firma Microsoft haven t faktycznie implementowany dowolnej z metod związanych z pracą z grup kontaktów. Obecnie klasy EntityContactManagerRepository ma metody klasy zastępczej dla wszystkich metod kontaktu grupy wyświetlane w interfejsie IContactManagerRepository. Na przykład metoda ListGroups() obecnie wygląda następująco:

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

Metody klasy zastępczej pomogła nam do kompilowania aplikacji i przekazać testów jednostkowych. Jednak teraz nadszedł czas na faktycznego wdrażania tych metod. Ostateczna wersja klasy EntityContactManagerRepository znajduje się w ofercie 13.

**Wyświetlanie listy 13 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a>Tworzenie widoków

Aplikacja platformy ASP.NET MVC, korzystając z domyślny aparat widoku programu ASP.NET. Tak don t tworzyć widoki w odpowiedzi na test określonej jednostki. Jednakże, ponieważ aplikacja będzie bezużyteczne, widoki, możemy t ukończyć tę iterację bez tworzenia i modyfikowania widoków zawartych w aplikacji Contact Manager.

Należy utworzyć następujące nowych widoków do zarządzania grupami kontaktu (zobacz rysunek 7):

- Views\Group\Index.aspx — Wyświetla listę grup kontaktów
- Views\Group\Delete.aspx - Wyświetla formularz potwierdzenia związanych z usuwaniem grup kontaktu


[![Wyświetl indeks grupy](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)

**Rysunek 07**: indeks grupy widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-cs/_static/image14.png))


Należy zmodyfikować następujące istniejących widoków, aby zawierały grup kontaktów:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Możesz zobaczyć zmodyfikowanych widoków, analizując aplikacji programu Visual Studio, który towarzyszy w tym samouczku. Na przykład rysunek 8 przedstawia widok skontaktuj się z indeksu.


[![Widok indeksu kontaktów](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)

**Rysunek 08**: widoku indeksu kontaktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-6-use-test-driven-development-cs/_static/image16.png))


## <a name="summary"></a>Podsumowanie

W tej iteracji dodaliśmy nowe funkcje do naszej aplikacji Contact Manager postępując zgodnie z metodologii programowania sterowanego testami projektowania aplikacji. Zaczęliśmy, tworząc zestaw przypadków użycia. Utworzyliśmy zestaw testów jednostkowych, który odnosi się do wymagań wyrażone według przypadków użycia. Na koniec napisaliśmy wystarczający tylko kod w celu spełnienia wymagań wyrażony przez testy jednostek.

Po zakończeniu pisania wystarczającej ilości kodu w celu spełnienia wymagań wyrażony przez testy jednostek, Zaktualizowaliśmy bazę danych i widoków. Firma Microsoft dodano nową tabelę grup do naszej bazie danych i naszym modelu danych Entity Framework zaktualizowane. Możemy również tworzone i modyfikowane zestaw widoków.

W następnej iteracji — iteracji końcowego--możemy napisać ponownie naszej aplikacji, aby móc korzystać z technologii Ajax. Dzięki wykorzystaniu technologii Ajax, możemy poprawić czas odpowiedzi i wydajność aplikacji Contact Manager.

> [!div class="step-by-step"]
> [Poprzednie](iteration-5-create-unit-tests-cs.md)
> [dalej](iteration-7-add-ajax-functionality-cs.md)
