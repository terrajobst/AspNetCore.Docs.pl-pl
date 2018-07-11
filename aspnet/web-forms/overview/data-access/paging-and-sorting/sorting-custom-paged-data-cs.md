---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: Sortowanie niestandardowo stronicowanych danych (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W poprzednim samouczku dowiedzieliśmy sposób implementacji niestandardowych stronicowania, gdy presentating danych na stronie sieci web. W tym samouczku zobaczymy, jak rozszerzyć poprzednią...
ms.author: aspnetcontent
ms.date: 08/15/2006
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 8057b11f9d2d812b9b2bf8d9e016ed17cff4e2f2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816749"
---
<a name="sorting-custom-paged-data-c"></a>Sortowanie niestandardowo stronicowanych danych (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe) lub [Pobierz plik PDF](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> W poprzednim samouczku dowiedzieliśmy sposób implementacji niestandardowych stronicowania, gdy presentating danych na stronie sieci web. W tym samouczku widzimy, jak rozszerzyć poprzedni przykład obsługę sortowanie stronicowania niestandardowego.


## <a name="introduction"></a>Wprowadzenie

W porównaniu do stronicowania domyślne stronicowania niestandardowego można poprawić wydajność stronicować dane przez wiele rzędów, tworzenie niestandardowych, stronicowanie de facto Wybór implementacji stronicowania, gdy stronicowanie dużych ilości danych. Implementowanie stronicowania niestandardowego jest bardziej skomplikowane niż liczba implementacji domyślnej stronicowanie, jednak, szczególnie w przypadku dodawania sortowanie do mieszanki. W tym samouczku będziemy rozszerzać przykład z elementem po poprzednim obejmują obsługę sortowanie *i* stronicowania niestandardowego.

> [!NOTE]
> Ponieważ ten samouczek opiera się na poprzednim jeden, przed początku Poświęć chwilę, aby skopiować składni deklaratywnej w ramach `<asp:Content>` elementu z poprzedniego samouczka s strony sieci web (`EfficientPaging.aspx`) i wklej go między `<asp:Content>` element `SortParameter.aspx` strony. Odwołaj się do kroku 1 [Dodawanie kontrolek weryfikacji do edycji i wstawiania interfejsy](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) samouczka, aby uzyskać bardziej szczegółowe omówienie dotyczące replikowania funkcjonalność co strona ASP.NET do innego.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>Krok 1: Rewidowanie niestandardowe technika stronicowania

Niestandardowe stronicowanie działało poprawnie, musimy zaimplementować niektórych technika, która może efektywnie Pobierz konkretnego podzbioru rekordów podanymi jako parametry Rozpocznij indeks wiersza i maksymalna liczba wierszy. Istnieje kilka technik, które mogą służyć do osiągnięcia tego celu. W poprzednim samouczku przyjrzeliśmy się to za pomocą programu Microsoft SQL Server 2005 s nowego `ROW_NUMBER()` funkcji klasyfikacja. Krótko mówiąc `ROW_NUMBER()` funkcji Klasyfikacja przypisuje numer wiersza każdy wiersz zwrócony przez zapytanie, które są uszeregowane według określonego sortowania. Następnie uzyskuje się w, zwracając określonej sekcji numerowane wyników odpowiednich podzestaw rekordów. Następujące zapytanie pokazano, jak użyć tej techniki w celu zwracania tych produktów numerowane 11 do 20, gdy klasyfikacja wyniki uporządkowane alfabetycznie według `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

Ta metoda sprawdza się w przypadku stronicowania przy użyciu określonego sortowania (`ProductName` sortowana alfabetycznie, w tym przypadku), ale zapytanie musi zostać zmodyfikowana, aby wyświetlić wyniki posortowane według wyrażenie sortowania. W idealnym przypadku powyższym zapytaniu można dopasować do użycia parametru w `OVER` klauzuli w następujący sposób:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

Niestety, sparametryzowane `ORDER BY` klauzule są niedozwolone. Zamiast tego należy utworzyć procedurę składowaną, która akceptuje `@sortExpression` parametr wejściowy, ale używa jednego z następujących rozwiązań:

- Pisanie zapytań ustaloną dla każdego z wyrażenia sortowania, które mogą być używane; następnie należy użyć `IF/ELSE` instrukcje języka T-SQL, aby określić, która kwerenda do wykonania.
- Użyj `CASE` instrukcję, aby zapewnić dynamiczne `ORDER BY` na podstawie wyrażenia `@sortExpressio` n parametr wejściowy; Zobacz używane w sekcji dynamicznie sortowanie wyników zapytania [Power SQL `CASE` instrukcji](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Aby uzyskać więcej informacji.
- Utwórz odpowiednie zapytanie jako ciąg w procedurze składowanej, a następnie użyj [ `sp_executesql` systemowej procedury składowanej](https://msdn.microsoft.com/library/ms188001.aspx) do wykonania dynamicznego zapytania.

Każda z tych obejść ma pewne wady. Pierwsza opcja nie jest jak obsłudze jako pozostałe dwa, ponieważ wymaga tworzenia kwerendy dla każdego wyrażenia sortowania możliwe. W związku z tym jeśli później zdecydujesz się dodać nowy, sortowanie pola do widoku GridView również należy wrócić i zaktualizować procedury składowanej. Drugie podejście ma pewne precyzyjnie, które wprowadzają problemów z wydajnością, gdy sortowanie według kolumny bazy danych innych niż ciąg, a także cierpi z tych samych problemów, łatwość konserwacji jako pierwszy. I trzecią, która używa dynamiczny język SQL, wprowadza ryzyko ataku polegającego na iniekcji SQL, jeśli osoba atakująca może wykonać procedurę składowaną w wartości parametru wejściowego atakującego.

Gdy żaden z tych metod jest doskonałym rozwiązaniem, myślę, że trzecia opcja to najlepsze trzy. Przy jej użyciu dynamiczny język SQL oferuje stopień elastyczności, które nie obsługują pozostałe dwa. Ponadto ataku polegającego na iniekcji SQL można wykorzystać tylko jeśli atakujący jest w stanie wykonać procedurę składowaną, przekazując parametrów wejściowych przez siebie. Ponieważ warstwy DAL używa sparametryzowanych zapytań, ADO.NET będzie chronić te parametry, które są wysyłane do bazy danych przy użyciu architektury, co oznacza, że atak wstrzyknięcie kodu SQL istnieje tylko jeśli osoba atakująca może bezpośrednie wykonywanie procedury składowanej.

Aby zaimplementować tę funkcję, należy utworzyć nową procedurę składowaną w bazie danych Northwind, o nazwie `GetProductsPagedAndSorted`. Tę procedurę składowaną, należy zaakceptować trzy parametry wejściowe: `@sortExpression`, parametr wejściowy typu `nvarchar(100`) określający, jak wyniki powinny być sortowane i są wstrzykiwane bezpośrednio po `ORDER BY` tekstu w `OVER` klauzula; i `@startRowIndex` i `@maximumRows`, te same parametry wejściowe dwie liczby całkowitej z `GetProductsPaged` badany w poprzednim samouczku procedury składowanej. Utwórz `GetProductsPagedAndSorted` przechowywane procedury przy użyciu następującego skryptu:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

Procedura składowana rozpoczyna się poprzez zapewnienie, że wartość `@sortExpression` określono parametr. Jeśli go brakuje, wyniki są uporządkowane według `ProductID`. Następnie jest tworzony dynamicznego zapytania SQL. Należy zauważyć, że dynamiczne kwerendę SQL w tym miejscu różni się nieco od naszego poprzedniego zapytania używane do pobierania wszystkich wierszy z tabeli Produkty. W poprzednich przykładach możemy uzyskać każdej kategorii skojarzona produktów s s i dostawcy s nazwy podzapytania. Tę decyzję podjęliśmy w [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-cs.md) samouczek i zostało to zrobione, zamiast przy użyciu `JOIN` s ponieważ TableAdapter nie można automatycznie utworzyć skojarzone Wstawianie, aktualizowanie i usuwanie metody dla takich zapytania. `GetProductsPagedAndSorted` Procedury składowanej, musi jednak używać `JOIN` pod kątem wyników, które mają być uporządkowane według nazw kategorii lub dostawcę.

To zapytanie dynamiczne jest tworzony przez złączenie części zapytania statycznych i `@sortExpression`, `@startRowIndex`, i `@maximumRows` parametrów. Ponieważ `@startRowIndex` i `@maximumRows` parametrów liczby całkowitej, muszą zostać przekonwertowane na nvarchars aby można było poprawnie był połączone. Gdy to zapytanie dynamiczne SQL został skonstruowany, jest wykonywane przy użyciu `sp_executesql`.

Poświęć chwilę, aby przetestować tę procedurę składowaną z różnymi wartościami dla `@sortExpression`, `@startRowIndex`, i `@maximumRows` parametrów. Z poziomu Eksploratora serwera kliknij prawym przyciskiem myszy nazwę procedury składowanej i wybierz polecenie Execute. Zostanie wyświetlone okno dialogowe Uruchom procedurę składowaną, w którym możesz wprowadzić parametry wejściowe (patrz rysunek 1). Aby posortować wyników według nazwy kategorii, należy użyć CategoryName dla `@sortExpression` wartość parametru; Aby sortować według nazwy firmy dostawcy s, użyj CompanyName. Po podaniu wartości parametrów, kliknij przycisk OK. Wyniki są wyświetlane w oknie danych wyjściowych. Na rysunku 2 przedstawiono wyniki, podczas zwracania produktów randze spośród wszystkich dokumentów 11 do 20 przypadku porządkowania według `UnitPrice` w kolejności malejącej.


![Wypróbuj różne wartości dla parametrów procedury składowanej s trzech danych wejściowych](sorting-custom-paged-data-cs/_static/image1.png)

**Rysunek 1**: Wypróbuj różne wartości dla parametrów wejściowych procedury składowanej s trzy


[![S procedury składowanej wyniki są wyświetlane w oknie danych wyjściowych](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**Rysunek 2**: procedura składowana s wyniki są wyświetlane w oknie danych wyjściowych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-custom-paged-data-cs/_static/image4.png))


> [!NOTE]
> Gdy klasyfikacja wyniki wg określonego `ORDER BY` kolumny w `OVER` klauzuli programu SQL Server należy sortować wyniki. To to szybka operacja, w przypadku indeksu klastrowanego za pośrednictwem kolumn na liście wyników są szeregowane, lub jeśli jest pokryciem indeksu, ale może być bardziej kosztowne w przeciwnym razie. Aby poprawić wydajność zapytań wystarczająco duże, należy rozważyć dodanie indeksu nieklastrowanego dla kolumny, według której wyniki są uporządkowane według. Zapoznaj się [funkcji Klasyfikacja i wydajności w programie SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) Aby uzyskać więcej informacji.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Krok 2: Rozszerzając dostępu do danych i warstwy logiki biznesowej

Za pomocą `GetProductsPagedAndSorted` tworzenia procedury składowanej, naszym następnym krokiem jest zapewnienie oznacza, że do wykonania tej procedury składowanej za pośrednictwem naszej architektury aplikacji. Powoduje to dodanie odpowiedniej metody do warstwy DAL i LOGIKI. Pozwól, s, Rozpocznij od dodania metody z warstwą dal. Otwórz `Northwind.xsd` wpisana zestawu danych, kliknij prawym przyciskiem myszy `ProductsTableAdapter`i wybierz opcję Dodaj zapytanie z menu kontekstowego. Ile My mieliśmy w poprzednim samouczku, firma Microsoft chce skonfigurować ta nowa metoda warstwy DAL do użycia istniejącą procedurę składowaną - `GetProductsPagedAndSorted`, w tym przypadku. Rozpocznij, wskazując, ma nową metodę TableAdapter do użycia istniejącą procedurę składowaną.


![Wybrać istniejącą procedurę składowaną](sorting-custom-paged-data-cs/_static/image5.png)

**Rysunek 3**: wybrać istniejącą procedurę składowaną


Aby określić procedurę przechowywaną, aby użyć, wybierz `GetProductsPagedAndSorted` przechowywane procedury z listy rozwijanej na następnym ekranie.


![Użyj GetProductsPagedAndSorted procedury składowanej](sorting-custom-paged-data-cs/_static/image6.png)

**Rysunek 4**: Użyj GetProductsPagedAndSorted procedury składowanej


Tę procedurę składowaną zwraca zestaw rekordów, jego wyniki tak, na następnym ekranie wskazują, że zwraca ona dane tabelaryczne.


![Wskazuje, że procedura składowana ma zwracać dane tabelaryczne](sorting-custom-paged-data-cs/_static/image7.png)

**Rysunek 5**: wskazuje, że procedura składowana ma zwracać dane tabelaryczne


Na koniec utwórz metody DAL, które używają zarówno wypełnienia DataTable i zwraca DataTable wzorców nazewnictwa metody `FillPagedAndSorted` i `GetProductsPagedAndSorted`, odpowiednio.


![Wybierz nazwy metod](sorting-custom-paged-data-cs/_static/image8.png)

**Rysunek 6**: Wybierz nazwy metod


Obecnie ten wykonujemy ve rozszerzone DAL, możemy ponownie gotowe, aby włączyć do LOGIKI. Otwórz `ProductsBLL` klasy plików i Dodaj nową metodę `GetProductsPagedAndSorted`. Ta metoda musi zaakceptować, trzech parametrów wejściowych `sortExpression`, `startRowIndex`, i `maximumRows` i po prostu wywołać w dół do DAL s `GetProductsPagedAndSorted` metody, w następujący sposób:


[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Krok 3: Konfigurowanie kontrolki ObjectDataSource przebiegu w parametrze SortExpression

O rozszerzonych DAL i LOGIKI zawierają metody, które wykorzystują `GetProductsPagedAndSorted` procedury składowanej, wszystkie pozostające jest skonfigurowanie ObjectDataSource w `SortParameter.aspx` strony do używania nowej metody LOGIKI i przekazać `SortExpression` na podstawie parametrów Kolumna, która użytkownik zażądał Sortuj wyniki według.

Rozpocznij od zmiany ObjectDataSource s `SelectMethod` z `GetProductsPaged` do `GetProductsPagedAndSorted`. Można to zrobić za pomocą Kreatora konfigurowania źródła danych, w oknie właściwości lub bezpośrednio za pomocą składni deklaratywnej. Następnie należy podać wartość dla ObjectDataSource s [ `SortParameterName` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Jeśli ta właściwość jest ustawiona, kontrolki ObjectDataSource spróbuje przekazać GridView s `SortExpression` właściwość `SelectMethod`. W szczególności kontrolki ObjectDataSource sprawdza, czy parametr wejściowy, którego nazwa jest równa wartości `SortParameterName` właściwości. Ponieważ s LOGIKI `GetProductsPagedAndSorted` metoda ma parametr wejściowy wyrażenia sortowania, o nazwie `sortExpression`, ustaw ObjectDataSource s `SortExpression` właściwość sortExpression.

Po wprowadzeniu tych dwóch zmian, składni deklaratywnej s ObjectDataSource powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> Zgodnie z poprzedniego samouczka, upewnij się, że nie kontrolki ObjectDataSource *nie* dołączyć parametry wejściowe sortExpression, startRowIndex lub maximumRows jego SelectParameters kolekcji.


Aby włączyć sortowanie w widoku GridView, po prostu zaznacz pole wyboru Włącz sortowania w widoku GridView s tagu inteligentnego, który ustawia GridView s `AllowSorting` właściwość `true` i powodują, że tekst nagłówka dla każdej kolumny mógł być renderowany jako element LinkButton. Gdy użytkownik końcowy kliknie jeden nagłówek LinkButtons, ensues odświeżenie strony i orzeczona następujące czynności:

1. Aktualizacje GridView jego [ `SortExpression` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) wartość `SortExpression` pola, do którego łącze nagłówek został kliknięty
2. Kontrolki ObjectDataSource wywołuje s LOGIKI `GetProductsPagedAndSorted` metody, przekazując GridView s `SortExpression` właściwość jako wartość dla metody s `sortExpression` parametr wejściowy (wraz z odpowiednim `startRowIndex` i `maximumRows` wartości parametrów wejściowych)
3. LOGIKI wywołuje DAL s `GetProductsPagedAndSorted` — metoda
4. Wykonuje warstwy DAL `GetProductsPagedAndSorted` procedurą składowaną, zakończone powodzeniem w `@sortExpression` parametru (wraz z `@startRowIndex` i `@maximumRows` wartości parametrów wejściowych)
5. Procedura składowana ma zwracać odpowiedniego podzestawu danych do LOGIKI, która zwraca go do elementu ObjectDataSource; te dane są następnie powiązany z kontrolki GridView, renderowane w kodzie HTML i wysyłane do użytkownika końcowego

Rysunek nr 7 przedstawia pierwszej strony wyniki, gdy są sortowane według `UnitPrice` w kolejności rosnącej.


[![Wyniki są sortowane według UnitPrice](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**Rysunek 7**: wyniki są sortowane według UnitPrice ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-custom-paged-data-cs/_static/image11.png))


Gdy bieżąca implementacja parametru poprawnie można sortować wyniki według nazwy produktu, nazwa kategorii, ilość na jednostkę oraz ceny jednostkowej, próby kolejność wyników przez dostawcę nazwy wyniki w wyjątek czasu wykonywania (zobacz rysunek 8).


![Podjęto próbę Sortuj wyniki według dostawca skutkuje następujący wyjątek czasu wykonywania](sorting-custom-paged-data-cs/_static/image12.png)

**Rysunek 8**: Podjęto próbę Sortuj wyniki według dostawca skutkuje następujący wyjątek czasu wykonywania


Ten wyjątek występuje, ponieważ `SortExpression` s GridView `SupplierName` elementu BoundField jest ustawiona na `SupplierName`. Jednak nazwy dostawcy s `Suppliers` faktycznie nosi nazwę tabeli `CompanyName` byliśmy aliasem nazwy tej kolumny jako `SupplierName`. Jednak `OVER` klauzuli posługują się `ROW_NUMBER()` funkcji nie można użyć aliasu i należy użyć nazwy kolumny rzeczywistych. Dlatego też zmienić `SupplierName` s elementu BoundField `SortExpression` z NazwaDostawcy do CompanyName (patrz rysunek 9). Jak pokazano na rysunku nr 10, po tej zmianie wyniki można sortować przez dostawcę.


![Zmień SortExpression s elementu BoundField NazwaDostawcy CompanyName](sorting-custom-paged-data-cs/_static/image13.png)

**Rysunek 9**: Zmień SortExpression s elementu BoundField NazwaDostawcy CompanyName


[![Teraz można posortować wyników według dostawcy](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**Na rysunku nr 10**: można teraz można posortować wyników według dostawcy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-custom-paged-data-cs/_static/image16.png))


## <a name="summary"></a>Podsumowanie

Implementacja niestandardowa stronicowania, który zbadaliśmy w poprzednim samouczku wymaga określenia kolejności za pomocą której wyniki zostały posortowane w czasie projektowania. Krótko mówiąc oznaczało to, że implementacja niestandardowa stronicowania, którą wdrożyliśmy nie może, w tym samym czasie dostarczyć funkcje sortowania. W tym samouczku będziemy overcame to ograniczenie, rozszerzając procedury składowanej od pierwszego do uwzględnienia `@sortExpression` parametr wejściowy, za pomocą którego można sortować wyniki.

Po tworzenia tego przechowywane procedury i tworzenie nowych metod w DAL i LOGIKI, byliśmy w stanie stosować GridView oferowane zarówno sortowania i niestandardowe, stronicowania, konfigurując ObjectDataSource podawać GridView s bieżącego `SortExpression` właściwość LOGIKI `SelectMethod`.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Carlos Santos. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](efficiently-paging-through-large-amounts-of-data-cs.md)
> [dalej](creating-a-customized-sorting-user-interface-cs.md)
