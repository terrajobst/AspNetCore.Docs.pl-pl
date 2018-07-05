---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
title: Dodawanie dodatkowych kolumn tabeli DataTable (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Podczas korzystania z Kreatora TableAdapter, aby utworzyć zestaw danych z kontrolą typów, odpowiedniego elementu DataTable zawiera kolumny zwracane przez zapytanie głównej bazy danych. Ale tam...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 1e8e65f9-fe3e-4250-810b-c90227786bed
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 84cb5f267a79fe7787ea23f1dc924e10e4d6fe13
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402788"
---
<a name="adding-additional-datatable-columns-vb"></a>Dodawanie dodatkowych kolumn tabeli DataTable (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_VB.zip) lub [Pobierz plik PDF](adding-additional-datatable-columns-vb/_static/datatutorial70vb1.pdf)

> Podczas korzystania z Kreatora TableAdapter, aby utworzyć zestaw danych z kontrolą typów, odpowiedniego elementu DataTable zawiera kolumny zwracane przez zapytanie głównej bazy danych. Jednak istnieją sytuacje, gdy DataTable musi zawierać dodatkowe kolumny. W tym samouczku dowie się, dlaczego procedury składowane są zalecane, jeśli zaistnieje taka potrzeba dodatkowych kolumn tabeli DataTable.


## <a name="introduction"></a>Wprowadzenie

Podczas dodawania TableAdapter z zestawem danych z kontrolą typów, odpowiedni schemat s DataTable jest określany przez główne zapytanie TableAdapter s. Na przykład, jeśli główne zapytanie zwraca pola danych *A*, *B*, i *C*, DataTable będzie mieć trzy odpowiednie kolumny o nazwie *A*, *B*, i *C*. Oprócz jego główne zapytanie TableAdapter może zawierać dodatkowe zapytania, które zwracają, podzbiór danych na podstawie niektórych parametru. Na przykład, w uzupełnieniu do `ProductsTableAdapter` główne zapytanie s, która zwraca informacje o produktach, zawiera ona także metod, takich jak `GetProductsByCategoryID(categoryID)` i `GetProductByProductID(productID)`, które zwracają informacje określonego produktu, oparte na podany parametr.

Model o schematu s DataTable odzwierciedlają TableAdapter s główne zapytanie działa dobrze, jeśli wszystkie metody TableAdapter s zwracać taki sam lub mniej niż określone w głównym zapytaniu pól. Jeśli metoda TableAdapter musi zwrócić dodatkowe pola danych, następnie należy rozwijania schematu elementu DataTable s odpowiednio. W [Master/szczegółów przy użyciu punktowanej listy z rekordów głównych z kontrolką DataList szczegółów](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) samouczek dodaliśmy metodę `CategoriesTableAdapter` zwróconą `CategoryID`, `CategoryName`, i `Description` pól danych zdefiniowanych w główne zapytanie plus `NumberOfProducts`, pola dodatkowe dane, które podać liczbę produktów skojarzonych z każdej kategorii. Ręcznie dodaliśmy nową kolumnę, aby `CategoriesDataTable` w celu przechwycenia `NumberOfProducts` wartość z tej nowej metody pola danych.

Zgodnie z opisem w [przesyłanie plików](../working-with-binary-files/uploading-files-vb.md) samouczek, doskonałe należy uważać za pomocą adapterów TableAdapter, użyj instrukcji SQL zapytań ad-hoc, która ma metody pól danych, których nie są dokładnie zgodne główne zapytanie. Jeśli Kreator konfiguracji TableAdapter zostanie ponownie uruchomione, jego zaktualizuje wszystkie metody TableAdapter s tak, aby ich lista pól danych odpowiada główne zapytanie. W związku z tym wszystkie metody, przy użyciu kolumn niestandardowych list powrócić do listy kolumn główne zapytanie s i nie zwraca oczekiwanych danych. Ten problem nie występuje, gdy za pomocą procedur składowanych.

W tym samouczku przedstawiony zostanie sposób rozszerzyć schemat s DataTable w celu uwzględnienia dodatkowych kolumn. Z powodu kruchości TableAdapter, korzystając z instrukcji SQL zapytań ad-hoc w tym samouczku użyjemy procedur składowanych. Zapoznaj się [tworzenie nowych procedur składowanych dla s wpisany zestaw danych TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) i [za pomocą istniejących procedur składowanych dla s wpisany zestaw danych TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) samouczki, aby uzyskać więcej informacji na temat Konfigurowanie TableAdapter na używanie procedur składowanych.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Krok 1. Dodawanie`PriceQuartile`kolumny`ProductsDataTable`

W *tworzenie nowych procedur składowanych dla s wpisany zestaw danych TableAdapters* samouczku utworzyliśmy wpisany zestaw danych o nazwie `NorthwindWithSprocs`. Ten zestaw danych zawiera obecnie dwie DataTable: `ProductsDataTable` i `EmployeesDataTable`. `ProductsTableAdapter` Ma następujące trzy metody:

- `GetProducts` -główne zapytanie zwraca wszystkie rekordy z `Products` tabeli
- `GetProductsByCategoryID(categoryID)` -Zwraca wszystkie produkty z określonym *categoryID*.
- `GetProductByProductID(productID)` — Zwraca iloczyn danego z określonym *productID*.

Główne zapytanie i dwie dodatkowe metody wszystkie zwracać ten sam zestaw pól danych, a mianowicie wszystkie kolumny z `Products` tabeli. Nie ma żadnych skorelowany podzapytania lub `JOIN` s ściągania danych powiązanych z `Categories` lub `Suppliers` tabel. W związku z tym `ProductsDataTable` ma odpowiadającej mu kolumny dla każdego pola w `Products` tabeli.

W tym samouczku umożliwiają s Dodaj metodę, która `ProductsTableAdapter` o nazwie `GetProductsWithPriceQuartile` zwracającego wszystkie produkty. Oprócz pól danych Standardowy produkt w `GetProductsWithPriceQuartile` będą również zawierać `PriceQuartile` pola danych, która wskazuje, w ramach której kwartyl cena produktu s mieści się. Na przykład mieć tych produktów, których ceny są podane w najbardziej kosztowne 25% `PriceQuartile` wartość 1, natomiast te, których ceny mieszczą się w dolnej 25% będzie miał wartość 4. Zanim firma martwić o Tworzenie procedury składowanej do zwracania tych informacji, jednak najpierw musimy zaktualizować `ProductsDataTable` aby zawierała kolumnę zawierającą `PriceQuartile` wyniki podczas `GetProductsWithPriceQuartile` metoda jest używana.

Otwórz `NorthwindWithSprocs` zestawu danych i kliknij prawym przyciskiem myszy `ProductsDataTable`. Z menu kontekstowego wybierz polecenie Dodaj, a następnie wybierz kolumnę.


[![Dodaj nową kolumnę do ProductsDataTable](adding-additional-datatable-columns-vb/_static/image2.png)](adding-additional-datatable-columns-vb/_static/image1.png)

**Rysunek 1**: Dodaj nową kolumnę, aby `ProductsDataTable` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image3.png))


Spowoduje to dodanie nowej kolumny do DataTable o nazwie Kolumna1 typu `System.String`. Należy zaktualizować nazwę tej kolumny s PriceQuartile i jej typ `System.Int32` ponieważ będą używane do przechowywania liczbą z przedziału od 1 do 4. Wybierz kolumnę nowo dodane w `ProductsDataTable` i w oknie właściwości ustaw `Name` właściwości PriceQuartile i `DataType` właściwość `System.Int32`.


[![Ustaw nową nazwę s kolumny i właściwości typu danych](adding-additional-datatable-columns-vb/_static/image5.png)](adding-additional-datatable-columns-vb/_static/image4.png)

**Rysunek 2**: Set s nową kolumnę `Name` i `DataType` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image6.png))


Jak pokazano na rysunku 2, istnieją dodatkowe właściwości, które można ustawić, takie jak czy muszą być unikatowe, jeśli kolumna jest kolumną automatycznego przyrostu wartości w kolumnie, czy baza danych `NULL` wartości są dozwolone i tak dalej. Należy pozostawić te wartości ustawione na ich wartości domyślne.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Krok 2. Tworzenie`GetProductsWithPriceQuartile`— metoda

Teraz, gdy `ProductsDataTable` został zaktualizowany w celu uwzględnienia `PriceQuartile` kolumny, możemy przystąpić do tworzenia `GetProductsWithPriceQuartile` metody. Rozpocznij, klikając prawym przyciskiem myszy na obiekt TableAdapter i wybierając pozycję Dodaj zapytanie, z menu kontekstowego. Otwarte Kreatora konfiguracji zapytania TableAdapter najpierw żąda nam do tego, czy chcemy użyć instrukcji SQL zapytań ad-hoc lub nowej lub istniejącej procedury składowanej. Ponieważ firma Microsoft don t jeszcze mają procedury przechowywanej, która zwraca dane kwartyl ceny, umożliwiają s Zezwalaj TableAdapter utworzyć tę procedurę składowaną dla nas. Wybierz opcję tworzenia nowej procedury składowanej, a następnie kliknij przycisk Dalej.


[![Poinstruuj kreatora TableAdapter, aby utworzyć procedurę składowaną dla nas](adding-additional-datatable-columns-vb/_static/image8.png)](adding-additional-datatable-columns-vb/_static/image7.png)

**Rysunek 3**: poinstruować kreatora TableAdapter w celu utworzenia przechowywane procedury dla nas ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image9.png))


Kolejne na ekranie pokazano na rysunku 4 Kreator zapyta, nam jakiego typu zapytania do dodania. Ponieważ `GetProductsWithPriceQuartile` metoda zwróci wszystkie kolumny i rekordy z `Products` tabeli, wybierz pozycję, która zwraca wiersze opcji, a następnie kliknij przycisk Dalej.


[![Zapytanie będzie instrukcję SELECT tego zwraca wiele wierszy](adding-additional-datatable-columns-vb/_static/image11.png)](adding-additional-datatable-columns-vb/_static/image10.png)

**Rysunek 4**: nasze zapytania będą `SELECT` instrukcji tego zwraca wiele wierszy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image12.png))


Następnie możemy monit o podanie `SELECT` zapytania. W kreatorze, wprowadź następujące zapytanie:


[!code-sql[Main](adding-additional-datatable-columns-vb/samples/sample1.sql)]

Powyższe zapytanie używa programu SQL Server 2005 s nowego [ `NTILE` funkcja](https://msdn.microsoft.com/library/ms175126.aspx) podzielić wyniki na cztery grupy, w której grupy są określane przez `UnitPrice` wartości posortowane w kolejności malejącej.

Niestety, Konstruktor kwerend nie wie jak przeanalizować `OVER` — słowo kluczowe i zostanie wyświetlony błąd podczas analizowania powyższym zapytaniu. W związku z tym należy wprowadzić powyżej zapytania bezpośrednio w polu tekstowym w kreatorze, bez przy użyciu konstruktora zapytań.

> [!NOTE]
> Więcej informacji na temat s NTILE i SQL Server 2005 innych funkcji klasyfikacji, zobacz [zwracanie wyników randze spośród wszystkich dokumentów przy użyciu programu Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) i [sekcji funkcji Klasyfikacja](https://msdn.microsoft.com/library/ms189798.aspx) z [SQL Server 2005 — książki Online](https://msdn.microsoft.com/library/ms189798.aspx).


Po wprowadzeniu `SELECT` zapytań i kliknięciu przycisku Dalej, Kreator zapyta nam podać nazwę dla procedury składowanej, zostanie utworzony. Nadaj nazwę nowej procedury składowanej `Products_SelectWithPriceQuartile` i kliknij przycisk Dalej.


[![Nazwa Products_SelectWithPriceQuartile procedury składowanej](adding-additional-datatable-columns-vb/_static/image14.png)](adding-additional-datatable-columns-vb/_static/image13.png)

**Rysunek 5**: Nazwa procedury składowanej `Products_SelectWithPriceQuartile` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image15.png))


Ponadto firma Microsoft jest monit o podanie nazwy metody TableAdapter. Pozostaw zarówno wypełnienia DataTable i zwraca DataTable pól wyboru zaznaczone i nazwę metody `FillWithPriceQuartile` i `GetProductsWithPriceQuartile`.


[![Nazwa TableAdapter s metody i kliknij przycisk Zakończ](adding-additional-datatable-columns-vb/_static/image17.png)](adding-additional-datatable-columns-vb/_static/image16.png)

**Rysunek 6**: Nazwa metody s TableAdapter, a następnie kliknij przycisk Zakończ ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image18.png))


Za pomocą `SELECT` określone zapytanie i procedur składowanych i metody TableAdapter, nosi nazwę, kliknij przycisk Zakończ, aby zakończyć pracę kreatora. W tym momencie możesz otrzymać ostrzeżenie lub dwóch od kreatora informacją, że `OVER` instrukcji lub konstrukcji SQL nie jest obsługiwane. Można zignorować te ostrzeżenia.

Po ukończeniu kreatora, powinien zawierać TableAdapter `FillWithPriceQuartile` i `GetProductsWithPriceQuartile` metod i baza danych powinna zawierać procedury składowanej o nazwie `Products_SelectWithPriceQuartile`. Poświęć chwilę, aby sprawdzić, że TableAdapter w rzeczywistości zawiera ta nowa metoda i czy procedura składowana została poprawnie dodane do bazy danych. Podczas sprawdzania bazy danych, jeśli nie widzisz procedury składowanej spróbuj kliknij prawym przyciskiem myszy folder procedur składowanych i wybieranie pozycji Odśwież.


![Sprawdź, czy nowa metoda został dodany do elementu TableAdapter](adding-additional-datatable-columns-vb/_static/image19.png)

**Rysunek 7**: Sprawdź, czy nowa metoda został dodany do elementu TableAdapter


[![Upewnij się, że baza danych zawiera Products_SelectWithPriceQuartile procedury składowanej](adding-additional-datatable-columns-vb/_static/image21.png)](adding-additional-datatable-columns-vb/_static/image20.png)

**Rysunek 8**: Upewnij się, że baza danych zawiera `Products_SelectWithPriceQuartile` Stored Procedure ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image22.png))


> [!NOTE]
> Jest jedną z zalet przy użyciu procedur składowanych zamiast instrukcji SQL zapytań ad-hoc, że ponowne uruchomienie Kreatora konfiguracji TableAdapter nie zmodyfikuje listę kolumn procedur składowanych. To sprawdzić przez kliknięcie prawym przyciskiem myszy na obiekt TableAdapter, wybranie opcji konfiguracji z menu kontekstowego, aby uruchomić kreatora, a następnie klikając Zakończ, aby go ukończyć. Następnie przejdź do bazy danych i widok `Products_SelectWithPriceQuartile` procedury składowanej. Należy pamiętać, że jego lista kolumn nie zostały zmodyfikowane. Miał zostały używamy instrukcji SQL zapytań ad-hoc, ponownego uruchamiania Kreatora konfiguracji TableAdapter będzie mieć przywrócono tej listy kolumny zapytania s odpowiada liście kolumn główne zapytanie, usuwając instrukcji NTILE z zapytania używanego przez `GetProductsWithPriceQuartile` metody.


Gdy s warstwy dostępu do danych `GetProductsWithPriceQuartile` metoda jest wywoływana, wykonuje TableAdapter `Products_SelectWithPriceQuartile` procedury składowanej i dodaje wiersz, aby `ProductsDataTable` dla każdego zwrócił rekordu. Pola danych zwracane przez procedurę składowaną są mapowane do `ProductsDataTable` s kolumn. Ponieważ `PriceQuartile` pola danych zwracane z procedury składowanej, jego wartość jest przypisywana do `ProductsDataTable` s `PriceQuartile` kolumny.

Te metody TableAdapter, których zapytania nie zwracają `PriceQuartile` pola danych `PriceQuartile` wartość kolumny s jest wartość określoną przez jego `DefaultValue` właściwości. Jak pokazano na rysunku 2, ta wartość jest równa `DBNull`, wartość domyślna. Jeśli chcesz użyć innej wartości domyślnej, po prostu ustaw `DefaultValue` właściwość odpowiednio. Po prostu upewnij się, że `DefaultValue` wartość jest prawidłowa, biorąc pod uwagę kolumny s `DataType` (czyli `System.Int32` dla `PriceQuartile` kolumny).

Na tym etapie firma Microsoft wykonano czynności niezbędne do dodawania dodatkową kolumnę do elementu DataTable. Aby sprawdzić, czy w tej kolumnie dodatkowe działa zgodnie z oczekiwaniami, zezwala s Tworzenie strony ASP.NET, która wyświetla każdego produktu, Nazwa s, ceny i kwartyl ceny. Zanim do tego, jednak najpierw musimy zaktualizować warstwę logiki biznesowej do uwzględnienia metodą, która wywołuje w dół do DAL s `GetProductsWithPriceQuartile` metody. Firma Microsoft następnie aktualizowane LOGIKI w kroku 3 i następnie utwórz strony programu ASP.NET w kroku 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Krok 3: Przestarzałe, warstwę logiki biznesowej

Zanim użyjemy nowy `GetProductsWithPriceQuartile` metody z warstwy prezentacji, należy najpierw dodamy odpowiedniej metody do LOGIKI. Otwórz `ProductsBLLWithSprocs` klasy plików i Dodaj następujący kod:


[!code-vb[Main](adding-additional-datatable-columns-vb/samples/sample2.vb)]

Inne pobierania danych, metody, takie jak `ProductsBLLWithSprocs`, `GetProductsWithPriceQuartile` metoda po prostu wywołuje warstwę DAL odpowiadającego s `GetProductsWithPriceQuartile` metodę i zwraca wyniki.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Krok 4: Wyświetlania informacji o kwartyl ceny na stronie sieci Web platformy ASP.NET

Dodając LOGIKI ukończyć możemy ponownie gotowe do tworzenia strony ASP.NET, która pokazuje kwartyl cena dla każdego produktu. Otwórz `AddingColumns.aspx` strony w `AdvancedDAL` folder i przeciągnij GridView z przybornika w projektancie, ustawiając jego `ID` właściwość `Products`. Za pomocą tagu inteligentnego s GridView powiązać go do nowego elementu ObjectDataSource, o nazwie `ProductsDataSource`. Konfigurowanie kontrolki ObjectDataSource używać `ProductsBLLWithSprocs` klasy s `GetProductsWithPriceQuartile` metody. Ponieważ są to siatki tylko do odczytu zestawu list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak).


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy ProductsBLLWithSprocs](adding-additional-datatable-columns-vb/_static/image24.png)](adding-additional-datatable-columns-vb/_static/image23.png)

**Rysunek 9**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsBLLWithSprocs` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image25.png))


[![Pobieranie informacji o produkcie z metody GetProductsWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image27.png)](adding-additional-datatable-columns-vb/_static/image26.png)

**Na rysunku nr 10**: pobieranie informacji o produkcie z `GetProductsWithPriceQuartile` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image28.png))


Po zakończeniu pracy kreatora Konfigurowanie źródła danych, program Visual Studio automatycznie doda elementu BoundField lub CheckBoxField do kontrolki GridView dla każdego pola danych zwracany przez metodę. Jedno z tych pól danych jest `PriceQuartile`, czyli kolumny, dodaliśmy do `ProductsDataTable` w kroku 1.

Edytuj pola s GridView, usuwając wszystkie elementy oprócz `ProductName`, `UnitPrice`, i `PriceQuartile` BoundFields. Konfigurowanie `UnitPrice` elementu BoundField sformatować wartość jako walutę i `UnitPrice` i `PriceQuartile` BoundFields i Centrum — wyrównany do prawej, odpowiednio. Na koniec zaktualizuj pozostałe BoundFields `HeaderText` właściwości do produktu, ceny i cena KWARTYL, odpowiednio. Sprawdź także włączyć sortowanie pola wyboru tagów inteligentnych s GridView.

Po tych zmian kontrolkami GridView i kontrolki ObjectDataSource s oznaczeniu deklaracyjnym powinien wyglądać następująco:


[!code-aspx[Main](adding-additional-datatable-columns-vb/samples/sample3.aspx)]

Na ilustracji 11 pokazano tej strony po odwiedzeniu za pośrednictwem przeglądarki. Należy pamiętać, że początkowo produkty są uporządkowane według ich ceny w kolejności malejącej w poszczególnych produktach przypisaną odpowiednią `PriceQuartile` wartość. Oczywiście te dane można sortować według innych kryteriów z wartością kolumny kwartyl cena nadal odzwierciedlający klasyfikacji produktu s względem cen (zobacz rysunek 12).


[![Produkty są uporządkowane według ich cen.](adding-additional-datatable-columns-vb/_static/image30.png)](adding-additional-datatable-columns-vb/_static/image29.png)

**Rysunek 11**: produkty są uporządkowane według ich ceny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image31.png))


[![Produkty są uporządkowane według ich nazw](adding-additional-datatable-columns-vb/_static/image33.png)](adding-additional-datatable-columns-vb/_static/image32.png)

**Rysunek 12**: produkty są uporządkowane według ich nazw ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-additional-datatable-columns-vb/_static/image34.png))


> [!NOTE]
> Przy użyciu kilku wierszy kodu firma Microsoft może rozszerzyć widoku GridView tak, aby go pokolorowane wierszy produktów na podstawie ich `PriceQuartile` wartość. Firma Microsoft może kolor tych produktów w pierwszy kwartyl jasnozielony, w drugim kwartyl żółty światła i tak dalej. Zachęcam Cię do Poświęć chwilę, aby dodać tę funkcję. Jeśli potrzebujesz przypomnienia informacji na temat formatowania w kontrolce GridView, zapoznaj się z [niestandardowe formatowanie oparte na danych](../custom-formatting/custom-formatting-based-upon-data-vb.md) samouczka.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Alternatywnym podejściem — tworzenie innego TableAdapter

Jak możemy opisany w tym samouczku, gdy dodanie metody do TableAdapter, która zwraca pola danych niż te, województw przez główne zapytanie, możemy dodać odpowiednich kolumn do DataTable. Takie podejście, jednak działa dobrze, tylko wtedy, gdy istnieje kilka metod w TableAdapter, które zwracają różnych pól danych, a jeśli tych pól danych różnią się zbytnio od głównego zapytania.

Zamiast Dodawanie kolumn do DataTable, możesz zamiast tego dodać inny obiekt TableAdapter do zestawu danych, który zawiera metody z pierwszym TableAdapter, które zwracają różnych pól danych. Na potrzeby tego samouczka zamiast dodawać `PriceQuartile` kolumny `ProductsDataTable` (gdy jest on używany tylko przez `GetProductsWithPriceQuartile` metoda), można dodaliśmy dodatkowe TableAdapter do zestawu danych o nazwie `ProductsWithPriceQuartileTableAdapter` używanym `Products_SelectWithPriceQuartile` przechowywane Procedura jako jego główne zapytanie. Użyj strony ASP.NET, które wymagane można pobrać informacji o produkcie przy użyciu kwartyl cena `ProductsWithPriceQuartileTableAdapter`, natomiast te, które nie może w dalszym ciągu używać `ProductsTableAdapter`.

Dodając nowy obiekt TableAdapter, DataTables pozostają untarnished i kolumn dokładnie takie same jak stosowane pola danych zwracane przez metody TableAdapter s. Jednak dodatkowe TableAdapters wprowadzić powtarzających się zadań i funkcje. Na przykład, jeśli te strony ASP.NET, które wyświetlane `PriceQuartile` kolumny również potrzebne zapewnienie wstawiania, aktualizowania i usuwania pomocy technicznej, `ProductsWithPriceQuartileTableAdapter` muszą znaleźć się jego `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości prawidłowo skonfigurowane. Chociaż te właściwości będą takie same jak stosowane `ProductsTableAdapter` s, ta konfiguracja wprowadza dodatkowego kroku. Ponadto obecnie istnieją dwa sposoby, aby zaktualizować, usuń lub Dodaj produkt do bazy danych — za pośrednictwem `ProductsTableAdapter` i `ProductsWithPriceQuartileTableAdapter` klasy.

W tym samouczku do pobrania obejmują programy `ProductsWithPriceQuartileTableAdapter` klasy w `NorthwindWithSprocs` zestawu danych, który ilustruje takie podejście alternatywne.

## <a name="summary"></a>Podsumowanie

W większości przypadków wszystkie metody w TableAdapter zwróci ten sam zestaw pól danych, ale istnieją momenty, gdy konkretną metodę lub dwóch może być konieczne zwrócić dodatkowe pola. Na przykład w [Master/szczegółów przy użyciu punktowanej listy z rekordów głównych z kontrolką DataList szczegółów](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) samouczek dodaliśmy metodę `CategoriesTableAdapter` , oprócz pól danych główne zapytanie s zwrócił `NumberOfProducts` pola, które podać liczbę produktów skojarzonych z każdej kategorii. W tym samouczku przyjrzeliśmy się dodanie metody w `ProductsTableAdapter` zwróconą `PriceQuartile` pola oprócz pól danych główne zapytanie s. Do przechwytywania dodatkowych danych pola zwracane przez metody TableAdapter s musimy dodać odpowiednich kolumn do DataTable.

Jeśli planujesz ręczne dodawanie kolumn do DataTable, zaleca się, że TableAdapter używanie procedur składowanych. Jeśli element TableAdapter używa instrukcji SQL zapytań ad-hoc, ilekroć Kreator konfiguracji TableAdapter jest uruchamiana wszystkie metody pól danych zwrócony przez główne zapytanie przywrócić list pól danych. Ten problem nie jest rozszerzana procedur przechowywanych, dlatego zaleca się i zostały użyte w tym samouczku.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Randy Schmidt, Jacky Goor, Bernadette Leigh i Hilton Giesenow. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](updating-the-tableadapter-to-use-joins-vb.md)
> [dalej](working-with-computed-columns-vb.md)
