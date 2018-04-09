---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: Dodawanie kolumn do DataTable dodatkowe (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Podczas korzystania z Kreatora TableAdapter można utworzyć zestawu danych typu, odpowiedniego elementu DataTable zawiera kolumny zwracane przez zapytanie głównej bazy danych. Jednak brak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c2232c9867fc605a5b5d3973d4dbe31895841ca
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-additional-datatable-columns-c"></a>Dodawanie kolumn do DataTable dodatkowe (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip) lub [pobierania plików PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> Podczas korzystania z Kreatora TableAdapter można utworzyć zestawu danych typu, odpowiedniego elementu DataTable zawiera kolumny zwracane przez zapytanie głównej bazy danych. Ale w przypadku sytuacji, gdy element DataTable musi zawierać dodatkowe kolumny. W tym samouczku będziemy Dowiedz się, dlaczego procedur składowanych jest zalecany w przypadku potrzebujemy dodatkowych kolumn elementu DataTable.


## <a name="introduction"></a>Wprowadzenie

Podczas dodawania TableAdapter z zestawem danych wpisane, odpowiedni schemat s DataTable jest określana przez główne zapytanie TableAdapter s. Na przykład, jeśli główne zapytanie zwraca pola danych *A*, *B*, i *C*, DataTable ma trzy odpowiednie kolumny o nazwie *A*, *B*, i *C*. Oprócz jej główne zapytanie TableAdapter może zawierać dodatkowe zapytań zwracających, być może, podzbiór danych na podstawie niektórych parametru. Na przykład w uzupełnieniu do `ProductsTableAdapter` s główne zapytanie, które zwraca informacje o wszystkich produktów, zawiera również metody, takie jak `GetProductsByCategoryID(categoryID)` i `GetProductByProductID(productID)`, które zwracają informacje określonego produktu na podstawie dostarczonego parametru.

Model o schemacie s DataTable odzwierciedlają główne zapytanie TableAdapter s działa również w przypadku, gdy wszystkie metody TableAdapter s zwracają takie samo lub mniej niż określone w głównym zapytaniu pól. Jeśli metody TableAdapter musi zwracać dodatkowe pola danych, następnie możemy rozszerzyć schematu s DataTable odpowiednio. W [wzorca/Detail, za pomocą listy punktowanej z rekordu głównego z DataList szczegóły](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) samouczek dodaliśmy metodę `CategoriesTableAdapter` zwróconą `CategoryID`, `CategoryName`, i `Description` pól danych zdefiniowanych w główne zapytanie plus `NumberOfProducts`, pola dodatkowe dane, które zgłosił liczba skojarzone z każdej kategorii produktów. Ręcznie dodaliśmy nowe kolumny do `CategoriesDataTable` w celu przechwycenia `NumberOfProducts` wartość z tej nowej metody pola danych.

Zgodnie z opisem w [przesyłanie plików](../working-with-binary-files/uploading-files-cs.md) samouczek, dużą należy zadbać o TableAdapters używające ad hoc instrukcji SQL i metody, których pola danych nie pasują dokładnie głównego zapytania. Jeśli Uruchom ponownie Kreatora konfiguracji TableAdapter, jego zaktualizuje wszystkie metody TableAdapter s tak, aby ich lista pól danych zgodne główne zapytanie. W rezultacie żadnych metod z listami dostosowane kolumny powrócić do listy kolumn główne zapytanie s i nie zwracać oczekiwane dane. Ten problem nie występuje, gdy przy użyciu procedur składowanych.

W tym samouczku przedstawiono, jak rozszerzyć schemat s DataTable w celu uwzględnienia dodatkowych kolumn. Z powodu kruchości TableAdapter, korzystając z instrukcji SQL ad-hoc w tym samouczku używamy procedur składowanych. Odwoływać się do [Tworzenie nowej procedury składowanej s wpisane DataSet TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) i [za pomocą istniejących procedur składowanych s wpisane DataSet TableAdapters](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs) samouczki, aby uzyskać więcej informacji na temat Konfigurowanie TableAdapter, aby użyć procedur składowanych.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Krok 1: Dodawanie`PriceQuartile`kolumny`ProductsDataTable`

W *Tworzenie nowej procedury składowanej s wpisane DataSet TableAdapters* samouczek utworzyliśmy zestawu danych typu o nazwie `NorthwindWithSprocs`. Ten zestaw danych zawiera obecnie dwie DataTables: `ProductsDataTable` i `EmployeesDataTable`. `ProductsTableAdapter` Ma następujące trzy metody:

- `GetProducts` -główne zapytanie zwraca wszystkie rekordy z `Products` tabeli
- `GetProductsByCategoryID(categoryID)` -Zwraca wszystkie produkty z określonym *categoryID*.
- `GetProductByProductID(productID)` — Zwraca iloczyn danego z określonym *productID*.

Główne zapytanie i dwie dodatkowe metody wszystkie zwracać ten sam zestaw pól danych, czyli wszystkich kolumn z `Products` tabeli. Nie ma żadnych skorelowane podzapytania lub `JOIN` s ściąganie danych powiązanych z `Categories` lub `Suppliers` tabel. W związku z tym `ProductsDataTable` ma odpowiadającej mu kolumny dla każdego pola w `Products` tabeli.

W tym samouczku s umożliwiają dodanie metody `ProductsTableAdapter` o nazwie `GetProductsWithPriceQuartile` zwracającą wszystkie produkty. Oprócz standardowych produktu pola danych `GetProductsWithPriceQuartile` będą także obejmować `PriceQuartile` pola danych, która wskazuje, w których kwartyl cena produktu s mieści się. Na przykład, będzie mieć tych produktów, których ceny znajdują się w najbardziej kosztownych 25% `PriceQuartile` wartość 1, gdy te, których ceny mieszczą się w dolnej 25% będzie miał wartość 4. Zanim firma Microsoft martwić tworzenia procedury składowanej, aby przywrócić te informacje, jednak najpierw musimy zaktualizować `ProductsDataTable` aby zawierała kolumnę do przechowywania `PriceQuartile` wyniki podczas `GetProductsWithPriceQuartile` metoda jest używana.

Otwórz `NorthwindWithSprocs` zestawu danych i kliknij prawym przyciskiem myszy `ProductsDataTable`. Z menu kontekstowego wybierz polecenie Dodaj, a następnie wybierz kolumnę.


[![Dodaj nową kolumnę do ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**Rysunek 1**: Dodaj nową kolumnę do `ProductsDataTable` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-cs/_static/image3.png))


Spowoduje to dodanie nowej kolumny do tabeli DataTable o nazwie Kolumna1 typu `System.String`. Należy zaktualizować to nazwa kolumny s PriceQuartile i jej typu na `System.Int32` ponieważ będą używane do przechowywania liczbą z zakresu od 1 do 4. Wybierz kolumnę nowo dodanych w `ProductsDataTable` i w oknie właściwości ustaw `Name` właściwości PriceQuartile i `DataType` właściwości `System.Int32`.


[![Nowa nazwa s kolumny i właściwości typu danych](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**Rysunek 2**: Ustaw s nową kolumnę `Name` i `DataType` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-cs/_static/image6.png))


Jak pokazano na rysunku 2, istnieją dodatkowe właściwości, które można ustawić, takie jak czy muszą być unikatowe, jeśli kolumna jest kolumną automatycznego przyrostu wartości w kolumnie, czy baza danych `NULL` wartości są dozwolone i tak dalej. Pozostaw te wartości domyślne.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Krok 2: Tworzenie`GetProductsWithPriceQuartile`— metoda

Teraz, gdy `ProductsDataTable` została zaktualizowana w celu uwzględnienia `PriceQuartile` kolumny, możemy przystąpić do tworzenia `GetProductsWithPriceQuartile` metody. Uruchom prawym przyciskiem myszy na obiekt TableAdapter i wybierając pozycję Dodaj zapytanie z menu kontekstowego. Spowoduje to wyświetlenie Kreatora konfiguracji zapytania TableAdapter, w którym najpierw żąda nam określające, czy chcemy użyć ad hoc instrukcji SQL lub procedurę składowaną nowego lub istniejącego. Ponieważ firma Microsoft ADAM t ma jeszcze procedury przechowywanej, która zwraca dane kwartyl cen, umożliwiają s Zezwalaj TableAdapter utworzyć tę procedurę składowaną firmie Microsoft. Wybierz opcję Utwórz nowe procedury składowanej, a następnie kliknij przycisk Dalej.


[![TableAdapter Kreator można utworzyć procedury składowanej firmie Microsoft, poinstruuj](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**Rysunek 3**: poinstruować kreatora TableAdapter, aby utworzyć przechowywane procedury dla nas ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-cs/_static/image9.png))


Na ekranie kolejnych pokazano na rysunku 4 Kreator zapyta, nam jakiego typu zapytania do dodania. Ponieważ `GetProductsWithPriceQuartile` metoda zwróci wszystkie kolumny i rekordy z `Products` tabeli, wybierz SELECT, która zwraca wiersze opcję i kliknij przycisk Dalej.


[![Zapytanie będzie instrukcję SELECT tego zwraca wiele wierszy](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**Rysunek 4**: nasze zapytanie zostanie `SELECT` instrukcji tego zwraca wiele wierszy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-cs/_static/image12.png))


Następnie możemy monit o podanie `SELECT` zapytania. W kreatorze należy wprowadzić następujące zapytanie:


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

Powyższe zapytanie używa programu SQL Server 2005 s nowe [ `NTILE` funkcja](https://msdn.microsoft.com/library/ms175126.aspx) do dzielenia wyników na czterech grup, w której grupy jest określana przez `UnitPrice` wartości posortowane w kolejności malejącej.

Niestety, Konstruktor kwerend nie wie jak przeanalizować `OVER` — słowo kluczowe i zostanie wyświetlony błąd podczas analizowania powyższym zapytaniu. W związku z tym wprowadź powyżej zapytanie bezpośrednio w polu tekstowym w Kreatorze bez przy użyciu konstruktora zapytań.

> [!NOTE]
> Aby uzyskać więcej informacji na temat s NTILE i SQL Server 2005 inne funkcje klasyfikacji, zobacz [zwracanie wyniki związane z programu Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) i [sekcji funkcje klasyfikacji](https://msdn.microsoft.com/library/ms189798.aspx) z [SQL Server 2005 — książki Online](https://msdn.microsoft.com/library/ms189798.aspx).


Po wprowadzeniu `SELECT` zapytań i klikając przycisk Dalej, Kreator zapyta nas o podanie nazwy dla procedury składowanej, zostanie utworzony. Nazwa nowej procedury składowanej `Products_SelectWithPriceQuartile` i kliknij przycisk Dalej.


[![Nazwa procedury składowanej Products_SelectWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**Rysunek 5**: Nazwa procedury składowanej `Products_SelectWithPriceQuartile` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-cs/_static/image15.png))


Ponadto możemy się monit o podanie nazwy metody TableAdapter. Pozostaw zarówno wypełnienia DataTable i zwróć pola wyboru DataTable sprawdzić i nazwę metody `FillWithPriceQuartile` i `GetProductsWithPriceQuartile`.


[![Nazwa TableAdapter s metody i kliknij przycisk Zakończ](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**Rysunek 6**: Nazwa metody TableAdapter s, a następnie kliknij przycisk Zakończ ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-cs/_static/image18.png))


Z `SELECT` zapytanie określone i procedur składowanych i metod TableAdapter o nazwie, kliknij przycisk Zakończ, aby zakończyć pracę kreatora. Na tym etapie może wystąpić ostrzeżenie lub dwa z informacją, że kreator `OVER` SQL konstrukcja lub instrukcja nie jest obsługiwana. Można zignorować ostrzeżenia.

Po zakończeniu pracy kreatora, powinny zawierać TableAdapter `FillWithPriceQuartile` i `GetProductsWithPriceQuartile` metod i baza danych powinna zawierać procedury składowanej o nazwie `Products_SelectWithPriceQuartile`. Poświęć chwilę, aby sprawdzić, czy TableAdapter rzeczywiście zawiera ta nowa metoda i czy procedura składowana została poprawnie dodane do bazy danych. Podczas sprawdzania bazy danych, jeśli nie widzisz procedury składowanej spróbuj prawym przyciskiem myszy folder procedur składowanych i wybierając odświeżania.


![Sprawdź, czy nowa metoda został dodany do elementu TableAdapter](adding-additional-datatable-columns-cs/_static/image19.png)

**Rysunek 7**: Sprawdź, czy nowa metoda został dodany do elementu TableAdapter


[![Upewnij się, że baza danych zawiera Products_SelectWithPriceQuartile procedury składowanej](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**Rysunek 8**: Upewnij się, że baza danych zawiera `Products_SelectWithPriceQuartile` procedury składowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> Jest jedną z zalet za pomocą procedur składowanych zamiast instrukcji SQL ad-hoc, czy ponowne uruchomienie Kreatora konfiguracji TableAdapter nie zmodyfikuje listę kolumn procedur składowanych. Potwierdza prawym przyciskiem myszy w metodzie TableAdapter, wybranie opcji konfiguracji z menu kontekstowego, aby uruchomić kreatora i klikając przycisk Zakończ, aby ją zakończyć. Następnie przejdź do bazy danych i widok `Products_SelectWithPriceQuartile` procedury składowanej. Należy pamiętać, że jego lista kolumn nie został zmodyfikowany. Ma możemy zostały za pomocą instrukcji SQL ad hoc, ponowne uruchomienie Kreatora konfiguracji TableAdapter będzie mieć cofnięte tej listy kolumny zapytania s odpowiada liście kolumn główne zapytanie, usuwając instrukcji NTILE z zapytania używanego przez `GetProductsWithPriceQuartile` metody.


Gdy s Warstwa dostępu do danych `GetProductsWithPriceQuartile` wywołania metody, wykonuje TableAdapter `Products_SelectWithPriceQuartile` procedury składowanej i dodaje do `ProductsDataTable` dla każdego zwrócił rekordu. Pola danych zwracanych przez procedurę składowaną są mapowane na `ProductsDataTable` s kolumn. Ponieważ `PriceQuartile` pola danych zwracane z procedury składowanej, jego wartość jest przypisywana do `ProductsDataTable` s `PriceQuartile` kolumny.

Dla tych metod TableAdapter, których kwerendy nie zwracać `PriceQuartile` pola danych `PriceQuartile` wartość kolumny s jest wartość określoną przez jego `DefaultValue` właściwości. Jak pokazano na rysunku 2, ta wartość jest równa `DBNull`, wartość domyślna. Jeśli chcesz użyć innej wartości domyślnej, wystarczy ustawić `DefaultValue` właściwości odpowiednio. Upewnij się, że `DefaultValue` wartość jest prawidłowa podane kolumny s `DataType` (tj. `System.Int32` dla `PriceQuartile` kolumny).

W tym momencie zostały wykonane kroki niezbędne do dodawania dodatkowych kolumn do DataTable. Aby sprawdzić, czy ta kolumna dodatkowe działa zgodnie z oczekiwaniami, umożliwiają s tworzyć wyświetlający każdego produktu, Nazwa s, ceny i kwartyl cen strony ASP.NET. Zanim przejdziemy, jednak najpierw musimy zaktualizować warstwę logiki biznesowej, aby uwzględnić metody, która wywołuje w dół do DAL s `GetProductsWithPriceQuartile` metody. Firma Microsoft będzie następnie zaktualizuj logiki warstwy Biznesowej w kroku 3, a następnie utwórz strony ASP.NET w kroku 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Krok 3: Rozbudować warstwy logiki biznesowej

Przed możemy korzystać z nowych `GetProductsWithPriceQuartile` metody z warstwy prezentacji należy najpierw dodamy odpowiedniej metody do logiki warstwy Biznesowej. Otwórz `ProductsBLLWithSprocs` klasy plików i Dodaj następujący kod:


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

Innych pobierania danych, metod, takich jak `ProductsBLLWithSprocs`, `GetProductsWithPriceQuartile` metoda po prostu wywołuje warstwę DAL odpowiadającego s `GetProductsWithPriceQuartile` metodę i zwraca wyniki.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Krok 4: Wyświetlanie informacje o cenach kwartyl strony sieci Web ASP.NET

Z dodatkiem logiki warstwy Biznesowej ukończyć możemy re gotowy do utworzenia strony platformy ASP.NET, pokazujący kwartyl ceny dla każdego produktu. Otwórz `AddingColumns.aspx` strony `AdvancedDAL` folder i przeciągnij element GridView z przybornika do projektanta, ustawienie jej `ID` właściwości `Products`. Z tagów inteligentnych s GridView powiązać go z nowego elementu ObjectDataSource o nazwie `ProductsDataSource`. Element ObjectDataSource umożliwia konfigurowanie `ProductsBLLWithSprocs` klasy s `GetProductsWithPriceQuartile` metody. Ponieważ są to siatki tylko do odczytu zestawu list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak).


[![Skonfiguruj ObjectDataSource do użycia klasy ProductsBLLWithSprocs](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**Rysunek 9**: Konfigurowanie ObjectDataSource użyć `ProductsBLLWithSprocs` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-cs/_static/image25.png))


[![Pobieranie informacji o produkcie z metody GetProductsWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**Na rysunku nr 10**: pobieranie informacji o produkcie z `GetProductsWithPriceQuartile` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-cs/_static/image28.png))


Po zakończeniu pracy Kreatora konfigurowania źródła danych programu Visual Studio automatycznie doda elementu BoundField lub CheckBoxField do widoku GridView dla każdego pola danych zwróconych przez metodę. Jeden z tych pól danych jest `PriceQuartile`, czyli kolumny dodane do `ProductsDataTable` w kroku 1.

Edytuj pola s GridView, usuwając wszystkie elementy oprócz `ProductName`, `UnitPrice`, i `PriceQuartile` BoundFields. Skonfiguruj `UnitPrice` elementu BoundField może, a jego wartość jako walutę sformatować `UnitPrice` i `PriceQuartile` BoundFields prawej - i wyśrodkowania odpowiednio. Na koniec zaktualizuj pozostałych BoundFields `HeaderText` właściwości produktu, ceny i kwartyl cen odpowiednio. Ponadto pole wyboru Włącz sortowanie z tagów inteligentnych s widoku GridView.

Po tych zmian znaczników deklaratywne s GridView i ObjectDataSource powinna wyglądać następująco:


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

Rysunek 11 przedstawia tę stronę po odwiedzeniu za pośrednictwem przeglądarki. Należy pamiętać, że początkowo produkty są uporządkowane według ich cen w kolejności malejącej w poszczególnych produktach przypisane odpowiednie `PriceQuartile` wartość. Oczywiście te dane można sortować według innych kryteriów z cen kolumny kwartyl nadal odzwierciedlające klasyfikacji produktu s względem cen (patrz rysunek 12).


[![Te produkty są uporządkowane według ich ceny](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**Rysunek 11**: produkty są uporządkowane według ich ceny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-cs/_static/image31.png))


[![Te produkty są uporządkowane według nazw](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**Rysunek 12**: produkty są uporządkowane według nazwy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-cs/_static/image34.png))


> [!NOTE]
> Przy użyciu kilku wierszy kodu firma Microsoft może rozszerzyć widoku GridView tak, aby go pokolorowane produktu wierszy na podstawie ich `PriceQuartile` wartość. Firma Microsoft może kolorów te produkty w pierwszy kwartyl jasnozielony, w drugim kwartyl światła żółty i tak dalej. I zachęca Poświęć chwilę, aby dodać tę funkcję. Jeśli potrzebujesz odświeżacz formatowanie Element GridView, należy skontaktować się [niestandardowe formatowanie oparte na danych](../custom-formatting/custom-formatting-based-upon-data-cs.md) samouczka.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Informacje o innym podejściu — tworzenie innego TableAdapter

Jak widzieliśmy w tym samouczku, gdy Dodawanie metody do TableAdapter, która zwraca pola danych innych niż te, wskazane przez główne zapytanie, można dodać kolumny do elementu DataTable. Takie podejście, jednak działa dobrze tylko, jeśli istnieje kilka metod w TableAdapter, które zwracają różnych pól danych i tych pól danych nie zmieniają się zbyt szybko z głównego zapytania.

Zamiast Dodawanie kolumn do DataTable, zamiast tego można dodać TableAdapter innego do zestawu danych, który zawiera metody z pierwszego TableAdapter, które zwracają różnych pól danych. W tym samouczku zamiast dodawać `PriceQuartile` kolumny `ProductsDataTable` (gdy jest używany wyłącznie przez `GetProductsWithPriceQuartile` — metoda), można dodano dodatkowe TableAdapter z zestawem danych o nazwie `ProductsWithPriceQuartileTableAdapter` używany `Products_SelectWithPriceQuartile` przechowywane procedury w razie jego główne zapytanie. Strony ASP.NET, które niezbędne do pobrania informacji o produkcie z kwartyl cen użyje `ProductsWithPriceQuartileTableAdapter`, a te, które nie można nadal używać `ProductsTableAdapter`.

Dodając nowy obiekt TableAdapter DataTables pozostają untarnished i ich kolumny dokładnie duplikatów pól danych zwróconych przez ich metod s TableAdapter. Jednak dodatkowe TableAdapters mogą stać się powtarzających się zadań i funkcji. Na przykład, jeśli te ASP.NET strony wyświetlanej `PriceQuartile` kolumny również konieczne zapewnienie wstawiania, aktualizowania i usuwania pomocy technicznej, `ProductsWithPriceQuartileTableAdapter` musi dysponować jego `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości poprawnie skonfigurowane. Gdy te właściwości będą duplikatów `ProductsTableAdapter` s, ta konfiguracja wprowadza dodatkowego kroku. Ponadto istnieją teraz dwa sposoby aktualizacji, usuwać lub dodawać produktu do bazy danych — za pośrednictwem `ProductsTableAdapter` i `ProductsWithPriceQuartileTableAdapter` klasy.

Pobranie, w tym samouczku obejmuje `ProductsWithPriceQuartileTableAdapter` klasy w `NorthwindWithSprocs` zestawu danych, która ilustruje ta metoda alternatywna.

## <a name="summary"></a>Podsumowanie

W większości przypadków wszystkie metody w TableAdapter będzie zwracać ten sam zestaw pól danych, ale istnieją momenty, gdy konkretnej metody lub dwóch może być konieczne zwrócić dodatkowego pola. Na przykład w [wzorca/Detail, za pomocą listy punktowanej z rekordu głównego z DataList szczegóły](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) samouczek dodaliśmy metodę `CategoriesTableAdapter` że oprócz pola danych główne zapytanie s zwrócił `NumberOfProducts` pole zgłoszone liczba skojarzone z każdej kategorii produktów. W tym samouczku analizujemy dodanie metody w `ProductsTableAdapter` zwróconą `PriceQuartile` pola oprócz pól danych główne zapytanie s. Do przechwytywania danych dodatkowych pól zwrócony przez metody TableAdapter s musimy dodać kolumny do elementu DataTable.

Jeśli planujesz ręczne dodawanie kolumn do DataTable, zalecane jest używane procedury składowane w metodzie TableAdapter. Jeśli TableAdapter używa instrukcji SQL ad-hoc, ilekroć Kreatora konfiguracji TableAdapter jest uruchamiane wszystkie metody przywrócić listy pól danych do pól danych zwrócony przez główne zapytanie. Ten problem nie obejmuje procedury składowane, dlatego zaleca się i były używane w tym samouczku.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Randy Schmidt, Artur Goor Bernadette Leigh i Hilton Giesenow. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](updating-the-tableadapter-to-use-joins-cs.md)
> [dalej](working-with-computed-columns-cs.md)
