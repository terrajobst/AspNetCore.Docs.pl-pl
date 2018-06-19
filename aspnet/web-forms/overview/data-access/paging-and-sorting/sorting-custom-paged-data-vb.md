---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
title: Sortowanie niestandardowe stronicowanej danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W poprzednich samouczku opisano implementowania stronicowania niestandardowego, gdy presentating danych na stronie sieci web. W tym samouczku przedstawiono sposób rozszerzyć poprzedniego...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 4823a186-caaf-4116-a318-c7ff4d955ddc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
msc.type: authoredcontent
ms.openlocfilehash: e144b434bd759c4253065e365b1337a3eca82fa7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30883069"
---
<a name="sorting-custom-paged-data-vb"></a>Sortowanie niestandardowe stronicowanej danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_VB.exe) lub [pobierania plików PDF](sorting-custom-paged-data-vb/_static/datatutorial26vb1.pdf)

> W poprzednich samouczku opisano implementowania stronicowania niestandardowego, gdy presentating danych na stronie sieci web. W tym samouczku przedstawiono sposób rozszerzyć w poprzednim przykładzie, aby uwzględnić obsługę stronicowania niestandardowego sortowania.


## <a name="introduction"></a>Wprowadzenie

W porównaniu do stronicowania domyślne stronicowania niestandardowego może poprawić wydajność stronicowania danych przez wiele rzędów, tworzenie niestandardowych stronicowania faktyczne Wybór implementacji stronicowania podczas stronicowania za pośrednictwem dużych ilości danych. Implementowanie stronicowania niestandardowego jest bardziej skomplikowane niż Implementowanie stronicowania domyślne, jednak szczególnie w przypadku dodawania sortowania z różnymi. W tym samouczku będziemy rozszerzać przykład z poprzednim elementem obsługują sortowanie *i* stronicowania niestandardowego.

> [!NOTE]
> Ponieważ w tym samouczku opisano mieszanego, przed rozpoczęciem Poświęć chwilę, aby skopiować składni deklaratywnej w `<asp:Content>` element z poprzedniej strony sieci web samouczek s (`EfficientPaging.aspx`) i wklej go między `<asp:Content>` element `SortParameter.aspx` strony. Odwołaj się do kroku 1 [dodawanie formantów weryfikacji do edycji i wstawianie interfejsy](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) samouczka bardziej szczegółowe omówienie na replikowanie funkcji jedną stronę ASP.NET do innego.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>Krok 1: Rewidowanie niestandardowych technika stronicowania

Firma Microsoft stronicowania niestandardowego działało poprawnie, musisz zaimplementować niektóre metody, która wydajnie chwycić konkretnego podzestawu rekordów podane parametry Start indeks wiersza i maksymalna liczba wierszy. Istnieje kilka metod, które mogą służyć do osiągnięcia tego celu. W poprzednim samouczka analizujemy realizacji tego zadania przy użyciu programu Microsoft SQL Server 2005 s nowe `ROW_NUMBER()` Klasyfikacja funkcji. Krótko mówiąc `ROW_NUMBER()` Klasyfikacja funkcja przypisuje numeru wiersza do każdego wiersza zwróconych przez kwerendę, która jest określana przez kolejność sortowania określona. Odpowiednie podzestaw następnie są uzyskiwane przez zwrócenie określonej sekcji numerowane wyników. Następujące zapytanie ilustruje sposób użycia tej metody w celu uzyskania tych produktów numerowane 11 do 20, gdy klasyfikacja wyniki w kolejności alfabetycznej przez `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample1.sql)]

Ta metoda sprawdza się w przypadku stronicowania przy użyciu określonego sortowania (`ProductName` sortowana alfabetycznie, w tym przypadku), ale zapytania można zmodyfikować, aby wyświetlić wyniki posortowane według wyrażenie sortowania. W idealnym przypadku powyższym zapytaniu może ulegną można użyć parametru w `OVER` klauzuli, w następujący sposób:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample2.sql)]

Niestety, sparametryzowana `ORDER BY` klauzule są niedozwolone. Zamiast tego należy utworzyć procedury przechowywanej, która akceptuje `@sortExpression` parametru wejściowego, ale używa jednego z poniższych rozwiązań:

- Pisać zapytania ustalony dla każdego z wyrażenia sortowania, które mogą być używane; następnie należy użyć `IF/ELSE` instrukcje T-SQL, aby określić, które zapytania do wykonania.
- Użyj `CASE` instrukcji, aby zapewnić dynamiczne `ORDER BY` na podstawie wyrażenia `@sortExpressio` n parametr wejściowy; Zobacz używane w sekcji dynamicznie sortowania wyników zapytania [Power SQL `CASE` instrukcje](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Aby uzyskać więcej informacji.
- Jednostki właściwe zapytania jako ciąg w procedurze składowanej, a następnie użyj [ `sp_executesql` procedury składowanej systemu](https://msdn.microsoft.com/library/ms188001.aspx) można wykonać zapytania dynamicznego.

Każda z tych rozwiązań ma niektóre wady. Pierwsza opcja nie jest jako utrzymaniu jako dwa inne, ponieważ wymaga ona utworzenie zapytania dla każdego wyrażenia sortowania możliwe. W związku z tym jeśli później zdecydujesz się dodać nowe, sortowanie pola do widoku GridView będzie również należy wrócić i zaktualizować procedury składowanej. Drugi podejście charakteryzuje się niektóre precyzyjnie wprowadzenie problemów z wydajnością podczas sortowania według kolumn innych niż ciąg bazy danych, które również odczuwa te same kwestie utrzymanie jako pierwszy. I trzecią, używający dynamiczne SQL wprowadza ryzykiem w przypadku ataku polegającego na iniekcji SQL, jeśli osoba atakująca może wykonać procedurę składowaną, przekazując ich wybieranie wartości parametru wejściowego.

Gdy żaden z tych metod nie jest idealne, myślę, że trzecia opcja to najlepszy trzech. Z jego użyciem SQL dynamicznej oferuje poziom elastyczności, które nie zawierają innych dwa. Ponadto ataku polegającego na iniekcji SQL może być wykorzystywana tylko jeśli atakujący jest w stanie wykonać procedurę składowaną przekazywanie w parametrach wejściowych przez siebie. Ponieważ warstwy DAL korzysta z zapytań sparametryzowanych, ADO.NET będzie chronić tych parametrów, które są wysyłane do bazy danych za pomocą architektury, co oznacza, że SQL iniekcji luki w zabezpieczeniach istnieje tylko jeśli osoba atakująca może bezpośrednio wykonywania procedury składowanej.

Aby zaimplementować tę funkcję, należy utworzyć nową procedurę składowaną w bazie danych Northwind o nazwie `GetProductsPagedAndSorted`. Tę procedurę składowaną powinna obsługiwać trzy parametry wejściowe: `@sortExpression`, parametru wejściowego typu `nvarchar(100`) określa, jak mają być sortowane wyniki i wstrzykuje się bezpośrednio po `ORDER BY` tekst w `OVER` klauzul; i `@startRowIndex` i `@maximumRows`, tej samej całkowitą dwóch parametrów wejściowych z `GetProductsPaged` w poprzednim samouczek procedury składowanej. Utwórz `GetProductsPagedAndSorted` przechowywane procedury przy użyciu następującego skryptu:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample3.sql)]

Procedura składowana uruchamia się za zapewnienie, że wartość `@sortExpression` został określony parametr. Jeśli jest on niedostępny, wyniki są uporządkowane według `ProductID`. Następnie jest tworzony dynamiczne zapytania SQL. Należy pamiętać, że dynamiczne kwerendę SQL w tym miejscu różni się nieco od naszych poprzednich zapytań używane do pobierania wszystkich wierszy z tabeli Produkty. W poprzednich przykładach możemy uzyskać każdej kategorii skojarzona produktu s s i dostawcy nazwy s przy użyciu podzapytania. Ta decyzja została wprowadzona w [tworzenie Warstwa dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) samouczka i wykonano zamiast za pomocą `JOIN` s ponieważ TableAdapter nie można automatycznie utworzyć skojarzone wstawiania, aktualizowania i usuwania metod dla takich zapytania. `GetProductsPagedAndSorted` Procedury składowanej, musi jednak używać `JOIN` s dla wyników można porządkować według nazw kategorii lub dostawcy.

To zapytanie dynamiczny jest tworzony przez łączenie części zapytania statyczne i `@sortExpression`, `@startRowIndex`, i `@maximumRows` parametrów. Ponieważ `@startRowIndex` i `@maximumRows` parametrów liczba całkowita, muszą zostać przekonwertowane na nvarchars Aby zostać poprawnie połączony. Po to dynamiczny zapytanie SQL zostały utworzone, jest wykonywane przy użyciu `sp_executesql`.

Poświęć chwilę, aby przetestować tę procedurę składowaną z różnymi wartościami dla `@sortExpression`, `@startRowIndex`, i `@maximumRows` parametrów. Z poziomu Eksploratora serwera kliknij prawym przyciskiem myszy nazwę procedury składowanej i wybierz polecenie Execute. Zostanie wyświetlone okno dialogowe Uruchom procedurę składowaną, w którym można wprowadzać parametry wejściowe (zobacz rysunek 1). Aby posortować wyników według nazwy kategorii, użyj CategoryName dla `@sortExpression` wartość parametru; Aby sortować według nazwy firmy dostawcy s, użyj NazwaFirmy. Po podaniu wartości parametrów, kliknij przycisk OK. Wyniki są wyświetlane w oknie danych wyjściowych. Na rysunku 2 przedstawiono wyniki, gdy zwracanie produktów uszeregowane 11 do 20 w przypadku porządkowania według `UnitPrice` w kolejności malejącej.


![Spróbuj różne wartości dla procedury składowanej s trzech parametrów wejściowych](sorting-custom-paged-data-vb/_static/image1.png)

**Rysunek 1**: spróbuj różne wartości dla parametrów wejściowych procedury składowanej s trzy


[![S procedury składowanej wyniki są wyświetlane w oknie danych wyjściowych](sorting-custom-paged-data-vb/_static/image3.png)](sorting-custom-paged-data-vb/_static/image2.png)

**Rysunek 2**: s procedury składowanej wyniki są wyświetlane w oknie danych wyjściowych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-custom-paged-data-vb/_static/image4.png))


> [!NOTE]
> Gdy klasyfikacja wyniki według określonego `ORDER BY` kolumny w `OVER` klauzuli, programu SQL Server musi sortowania wyników. To jest szybkie działanie, jeśli istnieje indeks klastrowany kolumn na liście wyników jest porządkowana przez lub jeśli istnieje pokryciem indeksu, ale może być bardziej kosztowne inaczej. Aby poprawić wydajność kwerend wystarczająco duże, należy rozważyć dodanie indeks nieklastrowany za pomocą której wyniki są uporządkowane według kolumny. Zapoznaj się [funkcji klasyfikacji i wydajności w programie SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) więcej szczegółów.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Krok 2: Rozbudować dostęp do danych i warstwy logiki biznesowej

Z `GetProductsPagedAndSorted` tworzenia procedury składowanej, naszych następnym krokiem jest zapewnienie środków do wykonania tej procedury składowanej za pośrednictwem Nasza architektura aplikacji. Powoduje to dodanie odpowiedniej metody do warstwy DAL i logiki warstwy Biznesowej. Let s, Rozpocznij od dodania metodę z warstwą dal. Otwórz `Northwind.xsd` wpisane zestawu danych, kliknij prawym przyciskiem myszy `ProductsTableAdapter`i wybierz opcję Dodaj zapytanie z menu kontekstowego. Jak robiliśmy w poprzednim samouczek chcemy się skonfigurować tej nowej metody warstwy DAL do użycia istniejącą procedurę składowaną - `GetProductsPagedAndSorted`, w tym przypadku. Rozpocznij od wskazujący, że chcesz nowy obiekt TableAdapter metodę istniejącą procedurę składowaną.


![Wybierz istniejącą procedurę składowaną](sorting-custom-paged-data-vb/_static/image5.png)

**Rysunek 3**: Wybierz istniejącą procedurę składowaną


Aby określić procedurę składowaną do użycia, zaznacz `GetProductsPagedAndSorted` przechowywane procedury z listy rozwijanej na następnym ekranie.


![Użyj GetProductsPagedAndSorted procedury składowanej](sorting-custom-paged-data-vb/_static/image6.png)

**Rysunek 4**: Użyj GetProductsPagedAndSorted procedury składowanej


Tę procedurę składowaną zwraca zestaw rekordów, zgodnie z jego wyniki tak, na następnym ekranie wskazują zwraca dane tabelaryczne.


![Wskazuje, że procedura składowana ma zwracać dane tabelaryczne](sorting-custom-paged-data-vb/_static/image7.png)

**Rysunek 5**: wskazuje, że procedura składowana ma zwracać dane tabelaryczne


Na koniec Utwórz DAL metody, które używają obu wypełnienia DataTable i zwraca DataTable w przypadku wzorców nazewnictwa metody `FillPagedAndSorted` i `GetProductsPagedAndSorted`odpowiednio.


![Wybierz nazwy metod](sorting-custom-paged-data-vb/_static/image8.png)

**Rysunek 6**: Wybierz nazwy metod


Teraz tego możemy kolejnych rozszerzony DAL, możemy re gotowe, aby włączyć się do logiki warstwy Biznesowej. Otwórz `ProductsBLL` klasy plik i dodać nową metodę `GetProductsPagedAndSorted`. Ta metoda musi akceptować trzy parametry wejściowe `sortExpression`, `startRowIndex`, i `maximumRows` i po prostu powinny wywoływać w do DAL s `GetProductsPagedAndSorted` metody, w następujący sposób:


[!code-vb[Main](sorting-custom-paged-data-vb/samples/sample4.vb)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Krok 3: Konfigurowanie ObjectDataSource przebiegu w parametrze SortExpression

O rozszerzony DAL i logiki warstwy Biznesowej, aby uwzględnić metody wykorzystujące `GetProductsPagedAndSorted` procedury składowanej, wszystkie który pozostaje jest skonfigurowanie ObjectDataSource w `SortParameter.aspx` strony do używania nowej metody logiki warstwy Biznesowej i przekaż `SortExpression` na podstawie parametru kolumny, która zażądała Sortuj wyniki według użytkownika.

Najpierw zmienić ObjectDataSource s `SelectMethod` z `GetProductsPaged` do `GetProductsPagedAndSorted`. Można to zrobić za pomocą kreatora Konfigurowanie źródła danych z okna właściwości lub bezpośrednio za pomocą składni deklaratywnej. Następnie należy podać wartość dla elementu ObjectDataSource s [ `SortParameterName` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Jeśli ta właściwość jest ustawiona, element ObjectDataSource podejmuje próbę przekazania w widoku GridView s `SortExpression` właściwości `SelectMethod`. W szczególności ObjectDataSource szuka parametr wejściowy, którego nazwa jest równa wartości `SortParameterName` właściwości. Ponieważ s logiki warstwy Biznesowej `GetProductsPagedAndSorted` metoda ma parametr wejściowy wyrażenie sortowania, o nazwie `sortExpression`, ustaw ObjectDataSource s `SortExpression` sortExpression dla właściwości.

Po wprowadzeniu tych zmian dwóch składni deklaratywnej s ObjectDataSource powinien wyglądać podobny do następującego:


[!code-aspx[Main](sorting-custom-paged-data-vb/samples/sample5.aspx)]

> [!NOTE]
> Zgodnie z poprzednim samouczka, upewnij się, że element ObjectDataSource jest *nie* obejmują sortExpression, startRowIndex lub maximumRows parametry wejściowe w jego SelectParameters kolekcji.


Aby włączyć sortowanie w widoku GridView, po prostu zaznacz pole wyboru Włącz sortowania w widoku GridView s tagu inteligentnego, który ustawia GridView s `AllowSorting` właściwości `true` , powodując tekst nagłówka dla każdej kolumny do renderowania jako element LinkButton. Gdy użytkownik końcowy kliknie jeden z nagłówków LinkButtons, ensues odświeżania strony i orzeczona następujące czynności:

1. Aktualizacje GridView jego [ `SortExpression` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) wartość `SortExpression` pola, którego łącze nagłówek został kliknięty
2. Element ObjectDataSource wywołuje s logiki warstwy Biznesowej `GetProductsPagedAndSorted` metody, przekazując GridView s `SortExpression` właściwość jako wartość metody s `sortExpression` parametru wejściowego (wraz z odpowiednią `startRowIndex` i `maximumRows` wartości parametrów wejściowych)
3. Logiki warstwy Biznesowej wywołuje DAL s `GetProductsPagedAndSorted` — metoda
4. Wykonuje warstwy DAL `GetProductsPagedAndSorted` przechowywane procedury, przekazywanie w `@sortExpression` parametr (wraz z `@startRowIndex` i `@maximumRows` wartości parametrów wejściowych)
5. Procedura składowana zwraca odpowiednie podzbiór danych do logiki warstwy Biznesowej, które zwraca go do elementu ObjectDataSource; te dane są następnie powiązany z widoku GridView, renderowane w kodzie HTML i przesyłany do użytkownika końcowego

Rysunek nr 7 przedstawia pierwszej strony wyników, gdy posortowane według `UnitPrice` w kolejności rosnącej.


[![Wyniki są sortowane według UnitPrice](sorting-custom-paged-data-vb/_static/image10.png)](sorting-custom-paged-data-vb/_static/image9.png)

**Rysunek 7**: wyniki są sortowane według UnitPrice ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-custom-paged-data-vb/_static/image11.png))


Gdy bieżąca implementacja poprawnie można sortować wyniki według nazwy produktu, nazwa kategorii, ilość na jednostkę oraz cenie jednostkowej, próby kolejności wyników przez dostawcę wyniki nazwa wyjątek czasu wykonywania (patrz rysunek 8).


![Podjęto próbę Sortuj wyniki według dostawcy skutkuje następujący wyjątek czasu wykonywania](sorting-custom-paged-data-vb/_static/image12.png)

**Rysunek 8**: Podjęto próbę Sortuj wyniki według dostawcy skutkuje następujący wyjątek czasu wykonywania


Ten wyjątek spowodowany `SortExpression` s GridView `SupplierName` ma ustawioną wartość elementu BoundField `SupplierName`. Jednak nazwy dostawcy s `Suppliers` faktycznie nosi nazwę tabeli `CompanyName` byliśmy aliasem tej nazwy kolumny jako `SupplierName`. Jednak `OVER` klauzuli używane przez `ROW_NUMBER()` funkcji nie można użyć aliasu i muszą używać nazwy kolumny rzeczywistych. W związku z tym zmienić `SupplierName` s elementu BoundField `SortExpression` z NazwaDostawcy do NazwaFirmy (patrz rysunek 9). Jak pokazano na rysunku nr 10, ta zmiana może być sortowane wyniki przez dostawcę.


![Zmień SortExpression s elementu BoundField NazwaDostawcy NazwaFirmy](sorting-custom-paged-data-vb/_static/image13.png)

**Rysunek 9**: Zmień SortExpression s elementu BoundField NazwaDostawcy NazwaFirmy


[![Teraz można posortować wyników przez dostawcę](sorting-custom-paged-data-vb/_static/image15.png)](sorting-custom-paged-data-vb/_static/image14.png)

**Na rysunku nr 10**: można teraz można posortować wyników według dostawcy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-custom-paged-data-vb/_static/image16.png))


## <a name="summary"></a>Podsumowanie

Implementacja niestandardowa stronicowania, który mamy się zbadana w poprzednim samouczek wymagane określenia kolejności za pomocą której wyniki były mają być sortowane w czasie projektowania. Krótko mówiąc oznacza to, że implementacja niestandardowa stronicowania, które wprowadziliśmy można, w tym samym czasie, zapewniają funkcje sortowania. W tym samouczku będziemy overcame to ograniczenie przez rozszerzenie procedury składowanej od pierwszego do uwzględnienia `@sortExpression` parametru wejściowego, w którym może być sortowane wyniki.

Po to tworzenie przechowywane procedury i tworzenie nowych metod w DAL i logiki warstwy Biznesowej, możemy możliwość wdrożenia Element GridView oferowane zarówno sortowania i niestandardowych stronicowania przez skonfigurowanie elementu ObjectDataSource do przekazania w widoku GridView s bieżącego `SortExpression` właściwości logiki warstwy Biznesowej `SelectMethod`.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Santos Artur. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](efficiently-paging-through-large-amounts-of-data-vb.md)
> [dalej](creating-a-customized-sorting-user-interface-vb.md)
