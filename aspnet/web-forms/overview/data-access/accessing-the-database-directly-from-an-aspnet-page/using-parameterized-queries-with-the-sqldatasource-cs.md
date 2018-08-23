---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
title: Używanie zapytań sparametryzowanych z kontrolką SqlDataSource (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku Będziemy nadal Zaktualizowaliśmy wygląd przy użyciu kontrolki SqlDataSource i Poznaj sposoby definiowania zapytań sparametryzowanych. Parametry można określić obu dekla...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 9128aaac-afe2-449f-84b2-bb1d035083c4
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 654c0ce5520a206e5e8e2fd20bed92ac1075bfe9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754355"
---
<a name="using-parameterized-queries-with-the-sqldatasource-c"></a>Używanie zapytań sparametryzowanych z kontrolką SqlDataSource (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_CS.exe) lub [Pobierz plik PDF](using-parameterized-queries-with-the-sqldatasource-cs/_static/datatutorial48cs1.pdf)

> W tym samouczku Będziemy nadal Zaktualizowaliśmy wygląd przy użyciu kontrolki SqlDataSource i Poznaj sposoby definiowania zapytań sparametryzowanych. Parametry można określić w sposób deklaratywny i programowy i mogą być ściągane z wielu lokalizacji na przykład ciąg zapytania, sesja stanu, innych formantów i innych.


## <a name="introduction"></a>Wprowadzenie

Jak odbierać dane bezpośrednio z bazy danych przy użyciu kontrolki SqlDataSource widzieliśmy w poprzednim samouczku. Za pomocą Kreatora konfigurowania źródła danych, firma Microsoft może wybrać bazy danych, a następnie: Wybierz kolumny do zwrócenia z tabeli lub widoku. Wprowadź instrukcję SQL niestandardowych; lub użyj procedury składowanej. Czy wybranie kolumn z tabeli lub widoku lub wprowadzanie niestandardowych instrukcji SQL, SqlDataSource kontrolować s `SelectCommand` właściwość jest przypisana wynikowy SQL zapytań ad-hoc `SELECT` instrukcji i jest to `SELECT` instrukcję, która jest wykonywane, kiedy SqlDataSource s `Select()` metoda jest wywoływana (programowo lub automatycznie dane formantu sieci Web).

SQL `SELECT` instrukcje używane w poprzednich pokazy samouczek s wykończenia `WHERE` klauzul. W `SELECT` instrukcji `WHERE` klauzuli może służyć do ograniczania wyników zwróconych. Na przykład aby wyświetlić nazwy produktów wyceny ponad 50,00 USD, możemy użyć następujące zapytanie:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample1.sql)]

Zazwyczaj z wartościami użytymi w `WHERE` klauzuli należy określić, przez zewnętrznego źródła, takie jak wartości querystring, zmiennej sesji lub dane wejściowe użytkownika z formantu sieci Web, na stronie. Najlepiej, jeśli takie dane wejściowe są określane za pośrednictwem *parametry*. Za pomocą programu Microsoft SQL Server, parametry są wskazywane za pomocą `@parameterName`, jak w:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample2.sql)]

SqlDataSource obsługuje sparametryzowanych zapytań, zarówno dla `SELECT` instrukcji i `INSERT`, `UPDATE`, i `DELETE` instrukcji. Ponadto wartości parametrów mogą być automatycznie ściągane z różnych źródeł ciąg zapytania, stan sesji formantów na stronie i tak dalej lub można przypisać programowo. W tym samouczku opisano sposób definiowania sparametryzowanych zapytań i jak określić parametr wartości w sposób deklaratywny i programowy.

> [!NOTE]
> W poprzednim samouczku mamy porównywane ObjectDataSource, które zostało naszego narzędzia do wybranego przez najpierw 46 samouczki z kontrolką SqlDataSource, biorąc pod uwagę ich podobieństwa pojęciach. Te podobieństwa także rozszerzać do parametrów. Parametry s ObjectDataSource mapowany do parametrów wejściowych dla metod w warstwy logiki biznesowej. Dzięki użyciu kontrolki SqlDataSource parametry są definiowane bezpośrednio z poziomu zapytania SQL. Obie kontrolki zawiera kolekcji parametrów dla ich `Select()`, `Insert()`, `Update()`, i `Delete()` metody i jednocześnie może mieć następujące wartości parametrów wypełnione z wstępnie zdefiniowanych źródeł (querystring wartości, zmienne sesji i tak dalej ) lub przypisane programowo.


## <a name="creating-a-parameterized-query"></a>Tworzenie zapytania parametrycznego

Kreator konfigurowania źródła danych s kontrolek SqlDataSource oferuje trzy drogi prowadzące do definiowania polecenie do wykonania w celu pobrania rekordów bazy danych:

- Wybieranie kolumny z istniejącej tabeli lub widoku
- Wprowadzając niestandardowych instrukcji SQL, lub
- Wybierając procedury składowanej

Podczas wybierania kolumn z istniejącej tabeli lub widoku, parametry `WHERE` musi być określona klauzula poprzez dodawanie `WHERE` klauzuli, okno dialogowe. Tworząc niestandardową instrukcję SQL, jednak możesz wprowadzić parametry bezpośrednio do `WHERE` — klauzula (przy użyciu `@parameterName` do oznaczania wszystkich parametrów). A [procedury składowanej](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) składa się z jedną lub więcej instrukcji SQL, i mogą być parametryzowane tych instrukcji. Parametry używane w instrukcji SQL, muszą być przekazywane w jako parametry wejściowe procedury składowanej.

Ponieważ tworzenie zapytania parametrycznego zależy SqlDataSource s `SelectCommand` jest określony, pozwól s Spójrz na wszystkich trzech metod. Aby rozpocząć, otwórz `ParameterizedQueries.aspx` strony w `SqlDataSource` folderu, przeciągnij kontrolki SqlDataSource z przybornika do projektanta i ustaw jego `ID` do `Products25BucksAndUnderDataSource`. Następnie kliknij łącze Konfigurowanie źródła danych za pomocą tagu inteligentnego sterowania s. Wybierz bazę danych do użycia (`NORTHWINDConnectionString`) i kliknij przycisk Dalej.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Krok 1. Dodawanie`WHERE`klauzuli podczas wybierania kolumn z tabeli lub widoku

Podczas wybierania danych do zwrócenia z bazy danych przy użyciu kontrolki SqlDataSource, Kreator konfigurowania źródła danych pozwala po prostu wybierz kolumny do zwrócenia z istniejącej tabeli lub wyświetlenia (patrz rysunek 1). Wykonując to automatycznie gromadzone SQL `SELECT` instrukcję, która jest, co jest wysyłane do bazy danych podczas SqlDataSource s `Select()` metoda jest wywoływana. Ile My mieliśmy w poprzednim samouczku wybierz tabelę produkty z listy rozwijanej i sprawdź `ProductID`, `ProductName`, i `UnitPrice` kolumn.


[![Wybierz kolumny do zwrócenia z tabeli lub widoku](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.png)

**Rysunek 1**: Wybierz kolumny do zwrócenia z tabeli lub widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.png))


Obejmujący `WHERE` w klauzuli `SELECT` instrukcji, kliknij przycisk `WHERE` przycisk, który wywołuje polecenie Dodaj `WHERE` klauzuli dialogowego (patrz rysunek 2). Aby dodać parametr, aby ograniczyć wyniki zwrócone przez `SELECT` zapytania o dane, najpierw wybierz kolumnę, aby filtrować dane według. Następnie wybierz operator, który ma być używana w celu filtrowania (=, &lt;, &lt;= &gt;i tak dalej). Na koniec wybierz źródło wartość parametru s, takie jak ze stanu sesji lub ciąg zapytania. Po skonfigurowaniu parametr, kliknij przycisk Dodaj, aby uwzględnić go w `SELECT` zapytania.

W tym przykładzie umożliwiają s zwracać tylko tych wyników, których `UnitPrice` wartość jest mniejsza niż lub równe 25,00. W związku z tym, wybierz `UnitPrice` z listy rozwijanej kolumny i &lt;= z listy rozwijanej operatora. Korzystając z wartości parametru ustaloną (na przykład 25,00) lub wartość parametru jest należy określić programowo, nie zaznaczaj niczego z listy rozwijanej źródeł. Następnie wprowadź wartość parametru zakodowane w polu tekstowym wartość 25,00 i ukończyć proces, klikając przycisk Dodaj.


[![Ogranicz wyniki zwrócone z gdzie dodać klauzulę, okno dialogowe](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.png)

**Rysunek 2**: Ogranicz wyniki zwracane z dodawania `WHERE` klauzuli, okno dialogowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.png))


Po dodaniu parametru, kliknij przycisk OK, aby powrócić do kreatora Konfigurowanie źródła danych. `SELECT` Instrukcji w dolnej części kreatora powinny znajdować się teraz `WHERE` klauzuli za pomocą parametru o nazwie `@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample3.sql)]

> [!NOTE]
> Jeśli określono wiele warunków w `WHERE` klauzuli z dodawania `WHERE` klauzuli okno dialogowe, Kreator łączy je z `AND` operatora. Jeśli konieczne jest uwzględnienie `OR` w `WHERE` klauzuli (takie jak `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`), a następnie musisz utworzyć `SELECT` instrukcji za pomocą niestandardowego ekranu instrukcji SQL.


Ukończenie konfigurowania SqlDataSource (kliknij przycisk Dalej, następnie Zakończ), a następnie sprawdź s SqlDataSource o oznaczeniu deklaracyjnym. Zawiera teraz znaczników `<SelectParameters>` kolekcji opisujemy źródeł dla parametrów w `SelectCommand`.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample4.aspx)]

Gdy SqlDataSource s `Select()` metoda jest wywoływana, `UnitPrice` wartość parametru (25,00) jest stosowany do `@UnitPrice` parametru w `SelectCommand` przed wysłaniem ich do bazy danych. Wynikiem jest tylko tych produktów, które są mniejsze niż lub równe 25,00 są zwracane z `Products` tabeli. Aby potwierdzić, dodać GridView do strony, powiązać go z tym źródłem danych, a następnie wyświetlić stronę za pośrednictwem przeglądarki. Powinien być widoczny tylko tych produktów na liście, które są mniejsze niż lub równe 25,00, ponieważ potwierdza rysunek 3.


[![Są wyświetlane tylko te produkty mniej niż lub równe 25,00](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.png)

**Rysunek 3**: są wyświetlane tylko te produkty mniejszą niż lub równe 25,00 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Krok 2: Dodawanie parametrów do instrukcji SQL niestandardowe

Podczas dodawania niestandardowej instrukcji SQL można wprowadzić `WHERE` klauzuli jawnie lub określ wartość w komórce filtru konstruktora zapytań. Aby to wykazać, niech s wyświetlanie tylko tych produktów w GridView, których ceny są mniejsze niż określony próg. Rozpocznij, dodając pola tekstowego do `ParameterizedQueries.aspx` strony, aby zebrać wartość tego progu przez użytkownika. Ustaw TextBox s `ID` właściwość `MaxPrice`. Dodawanie kontrolki przycisku w sieci Web i ustaw jego `Text` właściwości wyświetlania pasujące produkty.

Następnie przeciągnij GridView na stronie i w tagu inteligentnego zdecydować się na utworzenie nowego SqlDataSource, o nazwie `ProductsFilteredByPriceDataSource`. Za pomocą Kreatora konfigurowania źródła danych, przejdź do Określ niestandardową instrukcję SQL lub procedury składowanej ekranu (zobacz rysunek 4) i wprowadź następujące zapytanie:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample5.sql)]

Po wprowadzeniu zapytania (ręcznie lub za pośrednictwem konstruktora zapytań), kliknij przycisk Dalej.


[![Zwraca tylko te produkty, mniejsza niż wartość parametru](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.png)

**Rysunek 4**: zwracać tylko te produkty mniejszą niż lub równa wartości parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.png))


Ponieważ zapytanie zawiera parametry, następnym ekranie kreatora nam monituje o podanie źródła wartości parametrów. Wybierz formant z listy rozwijanej Źródło parametru i `MaxPrice` (formant pola tekstowego s `ID` wartość) z listy rozwijanej ControlID. Możesz też wprowadzić opcjonalną wartość domyślną do użycia w przypadku, gdy użytkownik nie wprowadził dowolny tekst w `MaxPrice` pola tekstowego. W chwili obecnej, nie należy wprowadzać wartości domyślnej.


[![S MaxPrice w polu tekstowym właściwości tekst jest używany jako źródło parametru](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.png)

**Rysunek 5**: `MaxPrice` TextBox s `Text` właściwość jest używana jako źródło parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.png))


Ukończ pracę kreatora Konfigurowanie źródła danych, klikając przycisk Dalej, a następnie Zakończ. Oznaczeniu deklaracyjnym GridView, pole tekstowe i przycisk oraz SqlDataSource następujące czynności:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample6.aspx)]

Należy pamiętać, że parametr w ramach SqlDataSource s `<SelectParameters>` sekcja jest `ControlParameter`, która obejmuje dodatkowe właściwości, takie jak `ControlID` i `PropertyName`. Gdy SqlDataSource s `Select()` metoda jest wywoływana, `ControlParameter` bierze wartość określonej właściwości formantu sieci Web, a następnie przypisuje go do odpowiedniego parametru w `SelectCommand`. W tym przykładzie `MaxPrice` s właściwość tekst jest używany jako `@MaxPrice` wartość parametru.

Potrwać chwilę, aby wyświetlić tą stronę za pośrednictwem przeglądarki. Po pierwsze, odwiedzając stronę lub po każdym `MaxPrice` TextBox brakuje wartości żadne rekordy nie są wyświetlane w widoku GridView.


[![Żadne rekordy nie są wyświetlane podczas MaxPrice pole tekstowe jest puste](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.png)

**Rysunek 6**: rekordy nie są wyświetlane, gdy `MaxPrice` pole tekstowe jest puste ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.png))


Jest przyczyna produkty nie są wyświetlane, ponieważ domyślnie pusty ciąg jako wartość parametru jest konwertowana na bazę danych `NULL` wartość. Ponieważ to funkcja porównania `[UnitPrice] <= NULL` zawsze jako FAŁSZ, są zwracane żadne wyniki.

Wprowadź wartość w polu tekstowym, takich jak 5.00 i kliknij przycisk Wyświetl produkty dopasowania. Na odświeżenie strony SqlDataSource informuje, że zmieniło się GridView, że jedna z jej źródła. W związku z tym widoku GridView rebinds do SqlDataSource wyświetlania tych produktów mniejszą lub równą 5,00 zł.


[![Produkty mniejszą niż lub równe $5.00 są wyświetlane.](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.png)

**Rysunek 7**: produkty mniejszą niż lub równe $5.00 są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.png))


## <a name="initially-displaying-all-products"></a>Początkowo wyświetlane wszystkie produkty

Zamiast wyświetlania żadnych produktów, po pierwszym załadowaniu strony, warto wyświetlić *wszystkich* produktów. Jednym ze sposobów, aby wyświetlić listę wszystkich produktów zawsze wtedy, gdy `MaxPrice` pole tekstowe jest puste jest, aby ustawić wartość domyślną parametru s na wyjątkowo wysoka wartość, takich jak 1000000, ponieważ s mało prawdopodobne, że Northwind Traders będzie nigdy nie mają spisu którego cena jednostkowa przekracza $ 1 000 000. To podejście jest jednak shortsighted i może nie działać w innych sytuacjach.

W poprzednich samouczkach - [parametry deklaratywne](../basic-reporting/declarative-parameters-cs.md) i [wzorzec/szczegół filtrowanie przy użyciu kontrolki DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) firma Microsoft była w obliczu podobny problem. Nasze rozwiązanie było umieścić tę logikę w warstwy logiki biznesowej. W szczególności LOGIKI badania wartość przychodzącego i, jeśli był `NULL` lub niektóre zastrzeżone wartości, wywołanie był kierowany do metody DAL zwrócone wszystkie rekordy. Jeśli wartość przychodzącego była wartość filtrowania normalna, Wykonano wywołanie do metody DAL, która instrukcja SQL, używanym sparametryzowane `WHERE` klauzula podana wartość.

Niestety firma Microsoft obejścia architektury, korzystając z kontrolką SqlDataSource. Zamiast tego należy dostosować instrukcję SQL, aby inteligentnie Pobierz wszystkie rekordy, jeśli `@MaximumPrice` parametr jest `NULL` lub jakąś wartość zastrzeżone. Na potrzeby tego ćwiczenia umożliwiają s jest tak że jeśli `@MaximumPrice` parametr jest równy `-1.0`, następnie *wszystkich* rekordy mają zostać zwrócone (`-1.0` działa jako zastrzeżonej wartości, ponieważ brak produktu mogą mieć ujemnych `UnitPrice`wartości). W tym celu możemy użyć następującą instrukcję SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample7.sql)]

To `WHERE` klauzula zwraca *wszystkich* rekordy Jeżeli `@MaximumPrice` parametr ma wartość `-1.0`. Jeśli wartość parametru jest `-1.0`, tylko te produkty którego `UnitPrice` jest mniejsza niż lub równa `@MaximumPrice` są zwracane wartości parametru. Ustawiając wartość domyślną `@MaximumPrice` parametr `-1.0`, przy pierwszym ładowaniu strony (lub zawsze, gdy `MaxPrice` pole tekstowe jest puste), `@MaximumPrice` będzie mieć wartość `-1.0` i wszystkie produkty zostaną wyświetlone.


[![Teraz wszystkie produkty są wyświetlane po MaxPrice pole tekstowe jest puste](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.png)

**Rysunek 8**: teraz wszystkie produkty są wyświetlane, gdy `MaxPrice` pole tekstowe jest puste ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image16.png))


Istnieje kilka ostrzeżenia, należy pamiętać, w przypadku tej metody. Najpierw należy pamiętać, że typ danych parametru s jest wnioskowany przez użycie w zapytaniu SQL. Jeśli zmienisz `WHERE` klauzuli z `@MaximumPrice = -1.0` do `@MaximumPrice = -1`, środowisko uruchomieniowe traktuje parametr jako liczba całkowita. Jeśli następnie spróbujesz przypisać `MaxPrice` polu tekstowym na wartość dziesiętną (na przykład 5,00), wystąpi błąd ponieważ 5.00 nie można przekonwertować na liczbę całkowitą. Aby rozwiązać ten problem, albo upewnij się, że używasz `@MaximumPrice = -1.0` w `WHERE` klauzuli lub lepszą jeszcze, ustaw `ControlParameter` obiektu s `Type` właściwości na wartość dziesiętną.

Po drugie, dodając `OR @MaximumPrice = -1.0` do `WHERE` klauzuli aparatu zapytań nie można użyć indeksu na `UnitPrice` (zakładając, że jeden istnieje), powodując skanu tabeli. To może wpłynąć na wydajność, jeśli są wystarczająco dużą liczbę rekordów w `Products` tabeli. Lepszym rozwiązaniem byłoby przenieść tę logikę do procedury składowanej gdzie `IF` albo wykonać instrukcję `SELECT` powstają kwerendy `Products` tabeli bez `WHERE` klauzuli, gdy wszystkie rekordy mają być zwracane lub jeden którego `WHERE` klauzula zawiera po prostu `UnitPrice` kryteria, dzięki czemu można użyć indeksu.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Krok 3: Tworzenie i używanie sparametryzowanej procedury składowane

Procedury składowane może zawierać zestaw parametrów wejściowych, które mogą być następnie używane w instrukcji SQL zdefiniowany w ramach procedury składowanej. Podczas konfigurowania SqlDataSource można użyć procedury składowanej, która akceptuje parametry wejściowe, można określić wartości tych parametrów, przy użyciu tych samych technik, podobnie jak w przypadku instrukcji SQL zapytań ad-hoc.

Aby zilustrować, przy użyciu procedur składowanych w użyciu kontrolki SqlDataSource, umożliwiają s utworzyć nową procedurę składowaną w bazie danych Northwind, o nazwie `GetProductsByCategory`, który akceptuje parametr o nazwie `@CategoryID` i zwraca wszystkie kolumny produktów którego `CategoryID` odpowiada kolumnie `@CategoryID`. Aby utworzyć procedurę przechowywaną, przejdź do Eksploratora serwera, a następnie przejść do szczegółów `NORTHWND.MDF` bazy danych. (Jeśli don t Server Explorer, Połącz, przechodząc do menu widoku i wybranie opcji Eksploratora serwera.)

Z `NORTHWND.MDF` bazy danych, kliknij prawym przyciskiem myszy w folderze procedur składowanych, wybierz pozycję Dodaj nową procedurę przechowywaną i wprowadź następującą składnię:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample8.sql)]

Kliknij przycisk Zapisz ikonę (lub Ctrl + S) można zapisać procedury składowanej. Kliknij prawym przyciskiem myszy w folderze procedur składowanych i wybierając polecenie wykonania, możesz przetestować procedurę składowaną. To spowoduje wyświetlenie monitu dla parametrów procedury składowanej s (`@CategoryID`, w tym wystąpieniu), po której wyniki zostaną wyświetlone w oknie danych wyjściowych.


[![GetProductsByCategory przechowywane procedury, gdy wykonywane za pomocą @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image17.png)

**Rysunek 9**: `GetProductsByCategory` Stored Procedure, gdy wykonywane za pomocą `@CategoryID` 1 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image18.png))


Pozwól, s, użyć tej procedury składowanej, aby wyświetlić wszystkie produkty z kategorii Beverages w GridView. Na stronie Dodaj nowe kontrolki GridView i powiązać ją z nowego SqlDataSource, o nazwie `BeverageProductsDataSource`. W dalszym ciągu Określ niestandardową instrukcję SQL lub procedury składowanej ekranu, wybierz przycisk radiowy procedury składowanej i wybierz `GetProductsByCategory` przechowywane procedury z listy rozwijanej.


[![Wybierz GetProductsByCategory przechowywane procedury z listy rozwijanej](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image19.png)

**Na rysunku nr 10**: Wybierz `GetProductsByCategory` procedury składowanej z listy rozwijanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image20.png))


Ponieważ procedury składowanej akceptuje parametr wejściowy (`@CategoryID`), jeśli klikniesz pozycję dalej monituje NAS, aby określić źródło dla wartości tego parametru s. Beverages `CategoryID` wynosi 1, więc pozostaw Brak listy rozwijanej źródła parametru i wprowadź wartość w polu tekstowym DefaultValue 1.


[![Umożliwia powrót do produktów do kategorii Beverages przez ustaloną wartość 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image21.png)

**Rysunek 11**: Użyj wartości Hard-Coded 1 do zwrócenia produkty z kategorii Beverages ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image22.png))


Jak oznaczeniu deklaracyjnym pokazano, korzystając z procedury składowanej SqlDataSource s `SelectCommand` właściwość jest ustawiona na nazwę procedury składowanej i [ `SelectCommandType` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) ustawiono `StoredProcedure`oznaczający które `SelectCommand` jest nazwa procedury składowanej, a nie instrukcji SQL zapytań ad-hoc.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample9.aspx)]

Przetestuj stronę w przeglądarce. Wyświetlane są tylko te produkty, które należą do kategorii Beverages, mimo że *wszystkich* produktu pola są wyświetlane od `GetProductsByCategory` procedura składowana ma zwracać wszystkie kolumny z `Products` tabeli. Firma Microsoft może, oczywiście, ograniczenia lub dostosować pola wyświetlane w widoku GridView z okna dialogowego Edytowanie kolumn GridView s.


[![Zostaną wyświetlone wszystkie Beverages](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image23.png)

**Rysunek 12**: wyświetlane są wszystkie Beverages ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Krok 4: Wywoływanie programowo SqlDataSource s`Select()`— instrukcja

Przykłady możemy ve widoczne w poprzednim samouczku i w tym samouczku do tej pory ma powiązany kontrolki SqlDataSource bezpośrednio GridView. SqlDataSource danych formantu s, jednak umożliwia programowo uzyskać dostęp i wyliczenia w kodzie. Może to być szczególnie przydatne, gdy należy wykonywać zapytania względem danych sprawdź go, ale don t, trzeba go wyświetlić. Zamiast konieczności zapisywania wszystkich standardowy kod ADO.NET do łączenia z bazą danych, określ polecenie i pobrać wyniki, można pozwolić SqlDataSource obsługi tego monotonii kodu.

Aby zilustrować pracy z tym s SqlDataSource danych programowo, Wyobraź sobie, Twój szef wysyła zbliża się można z żądaniem utworzyć stronę sieci web Wyświetla nazwę losowo wybranej kategorii i jego skojarzone produkty. Oznacza to, gdy użytkownik odwiedzi tę stronę, chcemy losowo wybrać kategorię z `Categories` tabeli, wyświetlana nazwa kategorii i następnie wyświetlanie listy produktów należących do tej kategorii.

W tym celu należy dwie kontrolki SqlDataSource co do pobrania losowe kategorię z `Categories` tabeli, a drugi można pobrać kategorii produktów s. Utworzymy SqlDataSource pobierającego rekord losowe kategorii, w tym kroku; Krok 5 patrzy na tworzeniu SqlDataSource, która pobiera produktów s kategorii.

Rozpocznij od dodania SqlDataSource do `ParameterizedQueries.aspx` i ustaw jego `ID` do `RandomCategoryDataSource`. Skonfiguruj go, tak aby używał następujące zapytanie SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample10.sql)]

`ORDER BY NEWID()` Zwraca rekordy, sortować w kolejności losowej (zobacz [Using `NEWID()` losowo sortowanie rekordów](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1` Zwraca pierwszy rekord z zestawu wyników. Podsumowując, ta kwerenda zwraca `CategoryID` i `CategoryName` wartości kolumn z jednej, losowo wybranej kategorii.

Do wyświetlania kategorii s `CategoryName` wartości, Dodaj kontrolkę etykieta w sieci Web do strony, ustaw jego `ID` właściwości `CategoryNameLabel`i wyczyszczenie jego `Text` właściwości. Aby programowo pobrać dane z kontrolki SqlDataSource, musimy wywołania jego `Select()` metody. [ `Select()` Metoda](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) oczekuje, że jeden parametr wejściowy typu [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), która określa, jak dane powinny być messaged przed zwróceniem. Może zawierać instrukcje dotyczące sortowania i filtrowania danych i jest używany przez dane, które kontrolki sieci Web, podczas sortowania i stronicowania danych z kontrolki SqlDataSource. W naszym przykładzie, możemy don t konieczność dane można zmodyfikować przed zwróceniem i w związku z tym przejdzie w `DataSourceSelectArguments.Empty` obiektu.

`Select()` Metoda zwraca obiekt, który implementuje `IEnumerable`. Dokładny typ zwracany, zależy od wartości kontrolki SqlDataSource s [ `DataSourceMode` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Zgodnie z opisem w poprzednim samouczku, można ustawić tej właściwości na wartość, albo `DataSet` lub `DataReader`. Jeśli ustawiono `DataSet`, `Select()` metoda zwraca [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) obiekt; Jeśli równa `DataReader`, zwraca obiekt, który implementuje [ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Ponieważ `RandomCategoryDataSource` ma SqlDataSource jego `DataSourceMode` właściwością `DataSet` (ustawienie domyślne), firma Microsoft będzie pracował z obiektu widoku danych.

Poniższy kod ilustruje sposób pobierania rekordów z `RandomCategoryDataSource` SqlDataSource jako widoku danych oraz jak odczytać `CategoryName` wartość kolumny z pierwszego wiersza w widoku danych:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample11.cs)]

`randomCategoryView[0]` Zwraca pierwszy `DataRowView` w widoku danych. `randomCategoryView[0]["CategoryName"]` Zwraca wartość `CategoryName` kolumny w tym pierwszego wiersza. Należy pamiętać, że DataView typowaniem luźnym. Aby odwołać się do wartości określonej kolumny musimy przekazać nazwę kolumny w formie ciągu (w tym przypadku CategoryName). Rysunek 13 pokazuje komunikat wyświetlany w `CategoryNameLabel` podczas wyświetlania strony. Oczywiście kategorii rzeczywista nazwa wyświetlana jest wybierane losowo przez `RandomCategoryDataSource` SqlDataSource na każdej wizyty do strony (w tym ogłaszania zwrotnego).


[![S losowo wybranej kategorii, którego nazwa jest wyświetlana](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image25.png)

**Rysunek 13**: s losowo wybranej kategorii jest wyświetlana nazwa ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image26.png))


> [!NOTE]
> Jeśli SqlDataSource kontrolować s `DataSourceMode` właściwość była równa `DataReader`, wartość zwrotną z elementu `Select()` wymagałby metody, można rzutować do `IDataReader`. Aby przeczytać `CategoryName` wartość kolumny z pierwszego wiersza, możemy d za pomocą kodu, takich jak:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample12.cs)]

Dzięki użyciu kontrolki SqlDataSource losowo wybranie kategorii, możemy ponownie gotowe do dodania GridView, który zawiera listę produktów kategorii s.

> [!NOTE]
> Zamiast używania formantu sieci Web etykiety, aby wyświetlić nazwę kategorii s, można dodaliśmy FormView lub DetailsView ze stroną powiązania jej z kontrolką SqlDataSource. Za pomocą etykiety, jednak umożliwiło nam Zbadaj, jak programowo wywołania SqlDataSource s `Select()` instrukcji i pracować z jego dane wynikowe w kodzie.


## <a name="step-5-assigning-parameter-values-programmatically"></a>Krok 5: Przypisywanie wartości parametrów programowe

Wszystkie przykłady możemy ve do tej pory widoczne w tym samouczku użyto wartości parametru ustaloną lub jeden z jednego ze źródeł wstępnie zdefiniowanych parametrów (wartości querystring, formantu sieci Web, na stronie i tak dalej). Jednak parametry s kontrolki SqlDataSource można również ustawić programowo. Aby ukończyć naszym przykładzie bieżąca, potrzebujemy SqlDataSource, które zwraca wszystkie produkty należące do określonej kategorii. Będzie miał ten SqlDataSource `CategoryID` parametr, którego wartość należy ustawić na podstawie `CategoryID` wartość kolumny zwracane przez `RandomCategoryDataSource` SqlDataSource w `Page_Load` programu obsługi zdarzeń.

Rozpocznij od dodania GridView do strony i powiązać ją z nowego SqlDataSource, o nazwie `ProductsByCategoryDataSource`. Podobnie jak zrobiliśmy w kroku 3, skonfiguruj SqlDataSource wywołuje `GetProductsByCategory` procedury składowanej. Pozostaw ustawiony listy rozwijanej źródła parametru na wartość None, ale nie należy wprowadzać wartości domyślnej, ponieważ firma Microsoft będzie programowo ustawić tę wartość domyślną.


[![Nie określaj parametru źródła lub wartość domyślną](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image27.png)

**Rysunek 14**: nie określono źródła parametru lub wartości domyślnej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image28.png))


Po zakończeniu pracy Kreatora SqlDataSource wynikowy oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample13.aspx)]

Firma Microsoft może przypisać `DefaultValue` z `CategoryID` programowo w parametrze `Page_Load` program obsługi zdarzeń:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample14.cs)]

To dodawanie strona zawierająca GridView, który pokazuje produkty powiązane z losowo wybranej kategorii.


[![Nie określaj parametru źródła lub wartość domyślną](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image29.png)

**Rysunek 15**: nie określono źródła parametru lub wartości domyślnej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image30.png))


## <a name="summary"></a>Podsumowanie

SqlDataSource umożliwia deweloperom strony do definiowania sparametryzowanych zapytań, których wartości parametru mogą być zakodowane, pobierane z wstępnie zdefiniowanych parametrów źródeł lub przypisany programowo. W tym samouczku widzieliśmy jak sformułować zapytanie parametryczne za pomocą Kreatora konfigurowania źródła danych dla zapytania SQL ad hoc i procedur składowanych. Zobaczyliśmy także za pomocą parametru ustaloną źródeł, formantu sieci Web jako źródło parametru i programowo określenie wartości parametru.

Podobnie jak za pomocą kontrolki ObjectDataSource, SqlDataSource udostępnia również funkcje do modyfikowania jego danych źródłowych. W następnym samouczku omówimy sposób definiowania `INSERT`, `UPDATE`, i `DELETE` instrukcji z kontrolką SqlDataSource. Po dodaniu tych instrukcji, firma Microsoft może korzystać z wbudowanych, wstawianie, edytowanie i usuwanie funkcje związane z kontrolkami GridView DetailsView i FormView.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Scott Clyde, Randell Schmidt i Krzysztof Pespisa. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](querying-data-with-the-sqldatasource-control-cs.md)
> [dalej](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
