---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
title: "Użycie zapytań sparametryzowanych z SqlDataSource (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku możemy kontynuować naszych przyjrzeć kontroli SqlDataSource i Dowiedz się, jak zdefiniować sparametryzowanych zapytań. Parametry można określić zarówno decla..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9128aaac-afe2-449f-84b2-bb1d035083c4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: b66c68b8306b905a800465ab0ed720ae6f9d16b9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="using-parameterized-queries-with-the-sqldatasource-c"></a>Użycie zapytań sparametryzowanych z SqlDataSource (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_CS.exe) lub [pobierania plików PDF](using-parameterized-queries-with-the-sqldatasource-cs/_static/datatutorial48cs1.pdf)

> W tym samouczku możemy kontynuować naszych przyjrzeć kontroli SqlDataSource i Dowiedz się, jak zdefiniować sparametryzowanych zapytań. Parametry można określić zarówno deklaratywnie, jak i programowo i być pobierane z różnych lokalizacji, np. ciąg zapytania, sesji stan innych kontrolek i więcej.


## <a name="introduction"></a>Wprowadzenie

W poprzednich instrukcji widzieliśmy sposobu używania kontroli SqlDataSource do pobierania danych bezpośrednio z bazy danych. Korzystając z Kreatora konfigurowania źródła danych, firma Microsoft może wybrać bazy danych, a następnie: Wybierz kolumny z tabeli lub widoku; Wprowadź niestandardową instrukcję SQL; lub użyj procedury składowanej. Czy Wybieranie kolumn w tabeli lub widoku lub wprowadzanie niestandardowych instrukcji SQL, SqlDataSource formant s `SelectCommand` właściwości przypisano wynikowy SQL ad hoc `SELECT` instrukcji i stanowi to `SELECT` instrukcję, która jest wykonywane, kiedy SqlDataSource s `Select()` wywołania metody (programowo lub automatycznie z danych formantu sieci Web).

SQL `SELECT` instrukcje używane w poprzednich pokazy samouczek s brakuje `WHERE` klauzul. W `SELECT` instrukcji `WHERE` można używać klauzuli Aby ograniczyć wyniki zwrócone. Na przykład aby wyświetlić nazwy produktów, kosztów więcej niż 50,00 USD, możemy użyć następujące zapytanie:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample1.sql)]

Zazwyczaj wartości użyte w `WHERE` klauzuli są określić źródła zewnętrznego, takie jak wartości querystring, zmiennej sesji lub danych wejściowych użytkownika za pomocą formantu na stronie sieci Web. Najlepiej, jeśli takie dane wejściowe są określane za pośrednictwem *parametry*. Microsoft SQL Server, parametry są oznaczane za pomocą `@parameterName`, jak w:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample2.sql)]

SqlDataSource obsługuje sparametryzowanych zapytań, dla `SELECT` instrukcje i `INSERT`, `UPDATE`, i `DELETE` instrukcje. Ponadto wartości parametrów, które może być automatycznie pobierane z różnych źródeł querystring, stan sesji kontrolki na stronie i itd lub można przypisać programowo. W tym samouczku przedstawiono będzie sposób definiowania zapytań sparametryzowanych również, jak określenie parametru wartości deklaratywnie i programowo.

> [!NOTE]
> W poprzednich instrukcji firma Microsoft porównywany ObjectDataSource, które zostało naszych wybranych narzędzi nad najpierw 46 samouczków z SqlDataSource, biorąc pod uwagę ich podobieństwa koncepcyjnego. Te podobieństwa rozszerzać parametrów. Element ObjectDataSource parametry s mapowane na parametry wejściowe dla metody warstwy logiki biznesowej. Z SqlDataSource parametry są definiowane bezpośrednio z poziomu zapytania SQL. Oba formanty zawiera kolekcji parametrów ich `Select()`, `Insert()`, `Update()`, i `Delete()` metod, a jednocześnie może mieć wartości tych parametrów pochodzą z wstępnie zdefiniowanych źródeł (querystring wartości, zmienne sesji i tak dalej ) lub przypisane programowo.


## <a name="creating-a-parameterized-query"></a>Tworzenie zapytania parametrycznego

Kreator konfigurowania źródła danych s formantów SqlDataSource oferuje trzy możliwe do definiowania polecenie do wykonania w celu pobrania rekordów bazy danych:

- Wybieranie kolumn w istniejącej tabeli lub widoku
- Wprowadź instrukcję SQL niestandardowych lub
- Wybierając procedury składowanej

Podczas wybierania kolumn z istniejącej tabeli lub widoku, parametry `WHERE` musi być określona klauzula za pomocą opcji Dodaj `WHERE` klauzuli — okno dialogowe. Tworząc niestandardową instrukcję SQL, jednak można wprowadzić parametry bezpośrednio do `WHERE` klauzuli (przy użyciu `@parameterName` do oznaczania każdego parametru). A [procedury składowanej](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) składa się z co najmniej jeden instrukcji SQL i mogą nadać parametry te instrukcje. Parametry używane w instrukcji SQL, muszą być przekazywane w jako parametry wejściowe procedury składowanej.

Ponieważ tworzenie zapytania parametrycznego zależy sposób SqlDataSource s `SelectCommand` jest określony, pozwalają s Spójrz na wszystkich trzech metod. Aby rozpocząć, otwórz `ParameterizedQueries.aspx` strony `SqlDataSource` , przeciągnij formant SqlDataSource z przybornika do projektanta i ustawić jej `ID` do `Products25BucksAndUnderDataSource`. Następnie kliknij łącze Konfigurowanie źródła danych z tagów inteligentnych s formantu. Wybierz bazę danych do użycia (`NORTHWINDConnectionString`) i kliknij przycisk Dalej.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Krok 1: Dodawanie`WHERE`klauzuli podczas wybierania kolumn z tabeli lub widoku

Podczas wybierania danych do zwrócenia z bazy danych z formantem SqlDataSource, kreatora skonfiguruj źródło danych umożliwia firmie Microsoft w celu po prostu wybierz kolumny do zwrócenia z istniejącej tabeli lub widoku, (zobacz rysunek 1). Czynności to automatycznie tworzy SQL `SELECT` instrukcja, która jest wysyłanych do bazy danych podczas SqlDataSource s `Select()` wywołania metody. Jak robiliśmy poprzedniej samouczka wybierz tabeli Produkty z listy rozwijanej i sprawdź `ProductID`, `ProductName`, i `UnitPrice` kolumn.


[![Wybierz kolumny z tabeli lub widoku](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.png)

**Rysunek 1**: Wybierz kolumny do powrotu z tabeli lub widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.png))


Aby uwzględnić `WHERE` w klauzuli `SELECT` instrukcji, kliknij przycisk `WHERE` przycisku, który wywołuje Dodaj `WHERE` klauzuli dialogowe (patrz rysunek 2). Aby dodać parametr, aby ograniczyć liczbę wyników zwróconych przez `SELECT` kwerendy, najpierw wybierz kolumnę, aby filtrować dane według. Następnie wybierz operator, służące do filtrowania (=, &lt;, &lt;=, &gt;i tak dalej). Na koniec wybierz źródło wartości parametru s, takich jak ze stanu sesji lub ciąg zapytania. Po skonfigurowaniu parametr, kliknij przycisk Dodaj, aby uwzględnić go w `SELECT` zapytania.

W tym przykładzie umożliwiają s zwracać tylko tych wyników gdzie `UnitPrice` wartość jest mniejsza niż 25,00. W związku z tym wybierz `UnitPrice` z listy rozwijanej kolumny i &lt;= z listy rozwijanej operatora. Korzystając z wartości parametru ustalony (na przykład 25,00) lub wartość parametru jest określenie programowo, wybierz opcję Brak z listy rozwijanej źródła. Następnie wprowadź wartość parametru ustalony w polu tekstowym wartość 25,00 i ukończyć proces, klikając przycisk Dodaj.


[![Ogranicz wyniki zwrócone przez dodawanie WHERE klauzuli — okno dialogowe](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.png)

**Rysunek 2**: Ogranicz wyniki zwracane z Dodaj `WHERE` okno dialogowe klauzuli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.png))


Po dodaniu parametru, kliknij przycisk OK, aby powrócić do Kreatora konfigurowania źródła danych. `SELECT` Instrukcja w dolnej części kreatora teraz powinna zawierać `WHERE` klauzuli z parametru o nazwie `@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample3.sql)]

> [!NOTE]
> Jeśli określono wiele warunków w `WHERE` klauzuli z Dodaj `WHERE` okno dialogowe klauzuli, Kreator łączy je za pomocą `AND` operatora. Jeśli konieczne jest uwzględnienie `OR` w `WHERE` klauzuli (takich jak `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`), a następnie trzeba utworzyć `SELECT` instrukcji za pomocą niestandardowych ekranu instrukcji SQL.


Ukończenie konfigurowania SqlDataSource (kliknij przycisk Dalej, następnie Zakończ), a następnie sprawdź znaczników deklaratywne s SqlDataSource. Kod znaczników zawiera teraz `<SelectParameters>` kolekcji, która wskazuje źródeł dla parametrów w `SelectCommand`.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample4.aspx)]

Gdy SqlDataSource s `Select()` wywołaniu metody `UnitPrice` wartość parametru (25,00) jest stosowana do `@UnitPrice` parametru w `SelectCommand` przed wysłaniem do bazy danych. Wynikiem jest jedynie produktów co najwyżej 25,00 są zwracane z `Products` tabeli. Aby potwierdzić, dodać element GridView ze stroną, powiązać go z tego źródła danych, a następnie Wyświetl stronę za pośrednictwem przeglądarki. Tylko powinna zostać wyświetlona na liście produktów, które są mniejsze niż lub równa 25,00, jak potwierdza rysunek 3.


[![Wyświetlane są tylko te produkty mniejsze niż lub równe 25,00](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.png)

**Rysunek 3**: są wyświetlane tylko te produkty mniejsze niż lub równe 25,00 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Krok 2: Dodawanie parametrów do instrukcji SQL niestandardowych

Podczas dodawania niestandardowych instrukcji SQL można wprowadzić `WHERE` klauzuli jawnie lub określ wartość w komórce filtru konstruktora zapytań. Aby zademonstrować, to, umożliwiają wyświetlanie tylko tych produktów w widoku GridView, których ceny jest mniejsza niż określony próg s. Rozpocznij od dodania pola tekstowego do `ParameterizedQueries.aspx` strony, aby zbierać ta wartość progowa od użytkownika. Ustaw pole tekstowe s `ID` właściwości `MaxPrice`. Dodawanie formantu przycisku sieci Web i ustawić jej `Text` właściwości do wyświetlania pasujące produkty.

Następnie, przeciągnij element GridView strony i z jego tagów inteligentnych chce utworzyć nowe SqlDataSource, o nazwie `ProductsFilteredByPriceDataSource`. W Kreatorze skonfiguruj źródło danych, przejdź do Określ niestandardową instrukcję SQL lub procedurę składowaną ekranu (patrz rysunek 4), a następnie wprowadź następujące zapytanie:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample5.sql)]

Po wprowadzeniu kwerendy (ręcznie lub za pośrednictwem konstruktora zapytań), kliknij przycisk Dalej.


[![Zwraca tylko te produkty mniejsza niż wartość parametru](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.png)

**Rysunek 4**: zwraca tylko te produkty mniejsze niż lub równy wartości parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.png))


Ponieważ zapytanie zawiera parametry, następnym ekranie kreatora nam monituje o podanie źródła wartości parametrów. Wybierz z listy rozwijanej Źródło parametru kontroli i `MaxPrice` (formantu TextBox s `ID` wartość) z listy rozwijanej ControlID. Możesz też wprowadzić opcjonalną wartość domyślną do użycia w przypadku, gdy użytkownik nie wprowadził tekst do `MaxPrice` pola tekstowego. W chwili obecnej, nie należy wprowadzać wartości domyślnej.


[![Pole tekstowe MaxPrice s właściwości Text jest używany jako źródło parametru](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.png)

**Rysunek 5**: `MaxPrice` pole tekstowe s `Text` właściwość jest używana jako źródło parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.png))


Ukończ pracę Kreatora konfigurowania źródła danych, klikając przycisk Dalej, a następnie Zakończ. Deklaracyjne kod znaczników dla widoku GridView, pola tekstowego, przycisk i SqlDataSource następująco:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample6.aspx)]

Należy pamiętać, że parametr w elemencie SqlDataSource s `<SelectParameters>` sekcja jest `ControlParameter`, która obejmuje dodatkowe właściwości, takie jak `ControlID` i `PropertyName`. Gdy SqlDataSource s `Select()` wywołaniu metody `ControlParameter` grabs wartość określonej właściwości formantu sieci Web i przypisuje go do odpowiadającego mu parametru w `SelectCommand`. W tym przykładzie `MaxPrice` s właściwość tekst jest używany jako `@MaxPrice` wartość parametru.

Zabrać kilka minut, aby wyświetlić tą stronę za pośrednictwem przeglądarki. Podczas odwiedzania najpierw strony lub gdy `MaxPrice` pole tekstowe brakuje wartości żadne rekordy nie są wyświetlane w widoku GridView.


[![Żadne rekordy nie są wyświetlane po MaxPrice pole tekstowe jest puste.](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.png)

**Rysunek 6**: rekordy nie są wyświetlane, gdy `MaxPrice` pole tekstowe jest pusta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.png))


Jest przyczyna produkty nie są wyświetlane, ponieważ domyślnie pusty ciąg na wartość parametru jest konwertowany na bazie danych `NULL` wartość. Ponieważ porównanie `[UnitPrice] <= NULL` zawsze jako wartość False, żadne wyniki nie są zwracane.

Wprowadź wartość w polu tekstowym, takich jak 5.00 i kliknij przycisk Wyświetl pasujące produkty. Strony SqlDataSource informuje o GridView, że jedna z jego źródła został zmieniony. W rezultacie widoku GridView rebinds do SqlDataSource wyświetlanie tych produktów mniejszą lub równą do wysokości 5,00 $.


[![Produkty mniejsze niż lub równe $5.00 są wyświetlane.](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.png)

**Rysunek 7**: produkty mniejsze niż lub równe $5.00 są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.png))


## <a name="initially-displaying-all-products"></a>Początkowo wyświetlanie wszystkich produktów

Zamiast produktów, gdy strona jest ładowana jako pierwsza, chcemy może wyświetlić *wszystkie* produktów. Aby wyświetlić listę wszystkich produktów zawsze, gdy `MaxPrice` pole tekstowe jest pusty ma ustawioną wartość domyślną parametru s niektórych insanely wysokiej wartości, takich jak 1000000, ponieważ s mało prawdopodobne, że Northwind Traders będzie kiedykolwiek ma spisu którego cenie jednostkowej przekracza $ 1 000 000. Takie podejście jest jednak shortsighted i może nie działać w innych sytuacjach.

W poprzednich samouczki - [deklaratywne parametry](../basic-reporting/declarative-parameters-cs.md) i [wzorzec/szczegół filtrowania z DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) możemy zostały wobec podobny problem. Nasze było umieść logikę tej warstwy logiki biznesowej. W szczególności logiki warstwy Biznesowej zbadać wartości przychodzącej i, jeśli został `NULL` lub niektóre zastrzeżone wartości, wywołanie został skierowany do metody DAL zwrócone wszystkie rekordy. Jeśli wartość przychodzącego była wartością filtrowania normalne, zadzwoniono do wykonywania instrukcji SQL, który używany sparametryzowane metody DAL `WHERE` klauzuli o podanej wartości.

Niestety mamy obejścia architektury, gdy przy użyciu SqlDataSource. Zamiast tego należy dostosować instrukcję SQL do inteligentnie pobrania wszystkich rekordów, jeśli `@MaximumPrice` parametr jest `NULL` lub niektóre zarezerwowanej wartości. W tym ćwiczeniu let s jest więc jeśli `@MaximumPrice` parametr jest równy `-1.0`, następnie *wszystkie* rekordów mają zostać zwrócone (`-1.0` działa jako zarezerwowanej wartości, ponieważ produkt nie może mieć ujemnej `UnitPrice`wartości). W tym celu możemy użyć następującą instrukcję SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample7.sql)]

To `WHERE` klauzula zwraca *wszystkie* rejestruje Jeśli `@MaximumPrice` parametr ma wartość `-1.0`. Jeśli wartość parametru nie jest `-1.0`, tylko te produkty których `UnitPrice` jest mniejsza niż lub równa `@MaximumPrice` są zwracane wartości parametru. Ustawiając wartość domyślną `@MaximumPrice` parametr `-1.0`, pierwszy ładowania strony (lub gdy `MaxPrice` pole tekstowe jest pusta), `@MaximumPrice` będzie mieć wartość `-1.0` i wszystkie produkty zostaną wyświetlone.


[![Teraz wszystkie produkty nie są wyświetlane po MaxPrice pole tekstowe jest puste.](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.png)

**Rysunek 8**: teraz wszystkie produkty nie są wyświetlane, gdy `MaxPrice` pole tekstowe jest pusta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image16.png))


Istnieje kilka ostrzeżenia, należy pamiętać o tej metody. Najpierw należy pamiętać, że typ danych parametru s jest wykryta przez jego użycia s w zapytaniu SQL. Jeśli zmienisz `WHERE` klauzuli z `@MaximumPrice = -1.0` do `@MaximumPrice = -1`, środowisko uruchomieniowe traktuje parametr jako liczba całkowita. Jeśli następnie spróbujesz przypisać `MaxPrice` pole tekstowe do wartości dziesiętnej (na przykład 5,00), wystąpi błąd ponieważ 5.00 nie można przekonwertować na liczbę całkowitą. Aby rozwiązać ten problem, albo upewnij się, że używasz `@MaximumPrice = -1.0` w `WHERE` klauzuli lub lepszą jeszcze, ustaw `ControlParameter` obiektu s `Type` dla właściwości typu Decimal.

Po drugie, dodając `OR @MaximumPrice = -1.0` do `WHERE` klauzuli aparatu zapytań nie można użyć indeksu na `UnitPrice` (zakładając, że jeden istnieje), powodując skanowanie tabeli. Jeśli są wystarczająco dużą liczbę rekordów w może wpłynąć na wydajność `Products` tabeli. Lepszym rozwiązaniem byłoby przenieść tę logikę do procedury składowanej gdzie `IF` albo przeprowadzi instrukcja `SELECT` zapytania z `Products` tabeli bez `WHERE` klauzuli, gdy wszystkie rekordy muszą zostać zwrócone, lub jeden którego `WHERE` klauzula zawiera po prostu `UnitPrice` kryteria, dzięki czemu indeks może być używany.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Krok 3: Tworzenie i korzystanie z procedur składowanych sparametryzowane

Procedury składowane mogą obejmować zestaw parametrów wejściowych, które mogą być następnie używane w instrukcji SQL zdefiniowany w ramach procedury składowanej. Podczas konfigurowania SqlDataSource można użyć procedury składowanej, który akceptuje parametr wejściowy, wartości tych parametrów można określić przy użyciu tych samych metod, podobnie jak w przypadku instrukcji SQL ad hoc.

Aby zilustrować, przy użyciu procedur składowanych w elemencie SqlDataSource, umożliwiają s Utwórz nową procedurę składowaną w bazie danych Northwind o nazwie `GetProductsByCategory`, który akceptuje parametr o nazwie `@CategoryID` i zwraca wszystkie kolumny produktów których `CategoryID` odpowiada kolumnie `@CategoryID`. Aby utworzyć procedury składowanej, przejdź do Eksploratora serwera i przejść do `NORTHWND.MDF` bazy danych. (Jeśli ADAM t Eksploratora serwera, nawiązania połączenia, przechodząc do menu Widok i wybranie opcji Eksploratora serwera.)

Z `NORTHWND.MDF` bazy danych, kliknij prawym przyciskiem myszy folder na procedury składowane, wybierz polecenie Dodaj nowe procedury składowanej i wprowadź następującej składni:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample8.sql)]

Kliknij przycisk Zapisz ikona (lub Ctrl + S) można zapisać procedury składowanej. Można przetestować procedurę składowaną przez kliknięcie prawym przyciskiem myszy w folderze procedur składowanych i wybierając polecenie Execute. To spowoduje wyświetlenie monitu dla parametrów procedury składowanej s (`@CategoryID`, w tym wystąpieniu), po którym wyniki będą wyświetlane w oknie danych wyjściowych.


[![GetProductsByCategory przechowywane procedury, gdy wykonywane z @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image17.png)

**Rysunek 9**: `GetProductsByCategory` procedury składowanej podczas wykonywania z `@CategoryID` 1 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image18.png))


Let s, użyj tej procedury składowanej do wyświetlenia wszystkich produktów w należące do tej kategorii w widoku GridView. Na stronie Dodaj nowy element GridView i powiązać ją z nowego SqlDataSource, o nazwie `BeverageProductsDataSource`. Nadal Określ niestandardową instrukcję SQL lub procedurę składowaną ekranu, wybierz przycisk radiowy procedury przechowywane i wybierz `GetProductsByCategory` przechowywane procedury z listy rozwijanej.


[![Wybierz GetProductsByCategory przechowywane procedury z listy rozwijanej](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image19.png)

**Na rysunku nr 10**: Wybierz `GetProductsByCategory` procedurę składowaną z listy rozwijanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image20.png))


Ponieważ procedury składowanej akceptuje parametr wejściowy (`@CategoryID`), klikając przycisk Dalej monituje nam, aby określić źródło dla tego parametru wartość s. Napoje `CategoryID` 1, umożliwiające pozostaw Brak listy rozwijanej Źródło parametru i wprowadź wartość 1 w polu tekstowym DefaultValue.


[![Umożliwia zwracanie produktów w należące do tej kategorii ustalony wartość 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image21.png)

**Rysunek 11**: umożliwia zwracanie produktów w należące do tej kategorii Hard-Coded wartość 1 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image22.png))


Jak przedstawiono na poniższym deklaratywne znaczników, korzystając z procedury składowanej SqlDataSource s `SelectCommand` właściwość jest ustawiona na nazwę procedury składowanej i [ `SelectCommandType` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) ma ustawioną wartość `StoredProcedure`, wskazujące które `SelectCommand` to nazwa procedury składowanej zamiast instrukcji SQL ad hoc.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample9.aspx)]

Przetestowanie strony w przeglądarce. Te produkty, które należą do należące do tej kategorii są wyświetlane, mimo że *wszystkie* produktu pola są wyświetlane od `GetProductsByCategory` procedury składowanej zwraca wszystkie kolumny z `Products` tabeli. Firma Microsoft może, oczywiście ograniczenia lub dostosować pola wyświetlane w widoku GridView z okna dialogowego Edytowanie kolumn s widoku GridView.


[![Zostaną wyświetlone wszystkie napoje](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image23.png)

**Rysunek 12**: wyświetlane są wszystkie napoje ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Krok 4: Programowane wywoływania SqlDataSource s`Select()`— instrukcja

Przykłady możemy kolejnych widoczne w poprzednim samouczek i w tym samouczku wykonanej do tej pory ma związanego SqlDataSource bezpośrednio w widoku GridView. Dane kontroli s SqlDataSource, jednak można programowo dostęp i wyliczane w kodzie. Może to być szczególnie przydatne, gdy trzeba wykonać zapytania o dane sprawdzić, ale ADAM t trzeba go wyświetlić. Zamiast konieczności zapisanie wszystkich schematyczny kod ADO.NET w celu połączenia z bazą danych, określ polecenie i pobieraniem ich wyników, możesz pozwolić, aby obsłużyć ten kod monotonii SqlDataSource.

Aby zilustrować pracy z SqlDataSource s danych programowo, załóżmy, że Twoje przełożonego zbliża się można z żądaniem utworzyć stronę sieci web, nazwą losowo wybranej kategorii i związane z nimi produkty. Oznacza to, gdy użytkownik odwiedzi tę stronę, chcemy losowo wybierz kategorię z `Categories` tabeli, wyświetlana nazwa kategorii, a następnie listy produktów należących do tej kategorii.

W tym celu należy dwóch formantów SqlDataSource co do pobrania losowe kategorii z `Categories` tabeli i drugiego do pobrania kategorii produktów s. Firma Microsoft będzie kompilacji SqlDataSource pobierający rekord losowe kategorii, w tym kroku; Krok 5 analizuje obsługuje tworzenie SqlDataSource, która pobiera kategorii produktów s.

Rozpocznij od dodania SqlDataSource do `ParameterizedQueries.aspx` i ustawić jej `ID` do `RandomCategoryDataSource`. Skonfiguruj go, tak aby były używane następujące zapytanie SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample10.sql)]

`ORDER BY NEWID()`Zwraca rekordy posortowane w kolejności losowej (zobacz [Using `NEWID()` losowo sortowanie rekordów](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1`Zwraca pierwszy rekord z zestawu wyników. Podsumowując, ta kwerenda zwraca `CategoryID` i `CategoryName` wartości kolumn z jednej, losowo wybranej kategorii.

Aby wyświetlić kategorii s `CategoryName` wartości, Dodaj formant etykiety sieci Web ze stroną, ustaw jego `ID` właściwości `CategoryNameLabel`i wyczyszczenie jego `Text` właściwości. Aby programowo pobierają dane z kontrolką SqlDataSource, musimy wywołania jego `Select()` metody. [ `Select()` Metody](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) oczekuje jednego parametru wejściowego typu [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), który określa, jak dane powinny być messaged przed zwróceniem. Mogą znajdować się instrukcje na sortowanie i filtrowanie danych i jest używany przez dane, które kontroluje sieci Web podczas sortowania i stronicowania za pomocą danych z formantu SqlDataSource. W naszym przykładzie, możemy ADAM t potrzebę danych można zmodyfikować przed zwróceniem i w związku z tym będzie podaj `DataSourceSelectArguments.Empty` obiektu.

`Select()` Metoda zwraca obiekt, który implementuje `IEnumerable`. Dokładny typ zwracany jest zależny od wartości kontrolki SqlDataSource s [ `DataSourceMode` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Zgodnie z opisem w poprzedniej samouczek, tej właściwości można ustawić na wartość albo `DataSet` lub `DataReader`. Jeśli ustawiono `DataSet`, `Select()` metoda zwraca [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) obiektu; w przypadku ustawienia `DataReader`, zwraca obiekt, który implementuje [ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Ponieważ `RandomCategoryDataSource` ma SqlDataSource jego `DataSourceMode` ustawioną właściwość `DataSet` (ustawienie domyślne), firma Microsoft będzie działać z obiektem DataView.

Poniższy kod ilustruje sposób pobrać rekordy z `RandomCategoryDataSource` SqlDataSource jako element DataView oraz sposób odczytywania `CategoryName` wartość kolumny z pierwszego wiersza DataView:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample11.cs)]

`randomCategoryView[0]`Zwraca pierwszy `DataRowView` w widoku danych. `randomCategoryView[0]["CategoryName"]`Zwraca wartość `CategoryName` kolumny w tym pierwszego wiersza. Należy pamiętać, że element DataView typowaniem luźnym. Aby odwołać się do wartości określonej kolumny musimy przekazywać nazwy kolumny jako ciąg (w tym przypadku CategoryName). Rysunek 13 przedstawiono komunikat wyświetlany w `CategoryNameLabel` podczas wyświetlania strony. Oczywiście nazwa kategorii rzeczywiste wyświetlana jest wybierane losowo przez `RandomCategoryDataSource` SqlDataSource każdej wizyty na stronie (w tym ogłaszania zwrotnego).


[![S losowo wybranej kategorii, którego nazwa jest wyświetlana](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image25.png)

**Rysunek 13**: s losowo wybranej kategorii jest wyświetlana nazwa ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image26.png))


> [!NOTE]
> Jeśli SqlDataSource kontrolować s `DataSourceMode` właściwość ma ustawioną `DataReader`, wartość zwrotną z elementu `Select()` metody wymagałby przeznaczenia można rzutować na `IDataReader`. Aby odczytać `CategoryName` wartość kolumny z pierwszego wiersza, możemy d za pomocą kodu, takich jak:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample12.cs)]

Z SqlDataSource losowo Wybieranie kategorii, możemy re gotowe do dodania do widoku GridView, który zawiera listę produktów kategorii s.

> [!NOTE]
> Zamiast przy użyciu formantu etykiety w sieci Web do wyświetlenia nazwę kategorii s, można dodaliśmy FormView lub widoku DetailsView ze stroną powiązania jej z SqlDataSource. Przy użyciu etykiety, jednak dozwolone nam do eksplorowania o wywoływaniu programowo SqlDataSource s `Select()` instrukcji i pracować z jego dane wynikowe w kodzie.


## <a name="step-5-assigning-parameter-values-programmatically"></a>Krok 5: Programowane przypisywanie wartości parametrów

Wszystkie przykłady możemy kolejnych wykonanej do tej pory widoczne w tym samouczku użyto wartości parametru ustalony lub jeden z jednego ze źródeł parametru wstępnie zdefiniowane (wartość ciągu kwerendy, formantu sieci Web na stronie i tak dalej). Jednak parametry s kontroli SqlDataSource można również ustawić programowo. Aby ukończyć naszym przykładzie bieżący, potrzebujemy SqlDataSource, która zwraca wszystkich produktów należących do określonej kategorii. SqlDataSource ten będzie miał `CategoryID` parametru, którego wartość musi być ustawiona na podstawie `CategoryID` zwrócony przez wartość w kolumnie `RandomCategoryDataSource` SqlDataSource w `Page_Load` obsługi zdarzeń.

Rozpocznij od dodania Element GridView do strony i powiązać ją z nowego SqlDataSource, o nazwie `ProductsByCategoryDataSource`. Podobnie jak opisano w kroku 3, skonfigurować SqlDataSource tak, aby go wywołuje `GetProductsByCategory` procedury składowanej. Pozostaw zestaw parametrów listy rozwijanej źródła None, ale nie należy wprowadzać wartość domyślną, jak firma Microsoft będzie programowo Ustaw tę wartość domyślną.


[![Nie określaj parametru źródła lub wartość domyślną](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image27.png)

**Rysunek 14**: nie określono parametru źródła lub wartość domyślną ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image28.png))


Po zakończeniu pracy Kreatora SqlDataSource wynikowy znaczników deklaratywne powinien wyglądać podobny do następującego:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample13.aspx)]

Firma Microsoft może przypisać `DefaultValue` z `CategoryID` parametru programowo w `Page_Load` obsługi zdarzeń:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample14.cs)]

To dodawanie strona zawierająca GridView, pokazujący produktów skojarzonych z losowo wybranej kategorii.


[![Nie określaj parametru źródła lub wartość domyślną](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image29.png)

**Rysunek 15**: nie określono parametru źródła lub wartość domyślną ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-parameterized-queries-with-the-sqldatasource-cs/_static/image30.png))


## <a name="summary"></a>Podsumowanie

SqlDataSource umożliwia deweloperom strona Definiowanie zapytań sparametryzowanych wartości parametrów, których mogą być stałe, pobierane z źródła wstępnie zdefiniowanych parametrów lub przypisane programowo. W tym samouczku widzieliśmy jak sformułować zapytania parametrycznego w Kreatorze skonfiguruj źródło danych dla zapytań ad hoc SQL i procedur składowanych. Również analizujemy za pomocą parametru ustalony źródeł, formantu sieci Web jako źródło parametru i programowo określenie wartości parametru.

Podobnie jak z ObjectDataSource SqlDataSource udostępnia również funkcje do modyfikowania jej odpowiednie dane. W następnym samouczku wyjaśniono, jak zdefiniować `INSERT`, `UPDATE`, i `DELETE` instrukcji z SqlDataSource. Po dodaniu tych instrukcji, firma Microsoft może korzystać z wbudowanych, wstawianie, edytowanie i usuwanie funkcje związane z formantami GridView widoku DetailsView i FormView.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Scott Clyde, Randell Schmidt i Krzysztof Pespisa. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](querying-data-with-the-sqldatasource-control-cs.md)
[dalej](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
