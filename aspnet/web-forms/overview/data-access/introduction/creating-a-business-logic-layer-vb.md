---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
title: Tworzenie warstwy logiki biznesowej (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: "W tym samouczku zajmiemy się tym, jak centralizować reguł biznesowych do firm logiki warstwy (logiki warstwy Biznesowej) służący jako pośrednik wymiany danych między t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 142e5181-29ce-4bb9-907b-2a0becf7928b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 858383203ddbaa9cb895c3368705f90546c8c974
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-business-logic-layer-vb"></a>Tworzenie warstwy logiki biznesowej (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_2_VB.exe) lub [pobierania plików PDF](creating-a-business-logic-layer-vb/_static/datatutorial02vb1.pdf)

> W tym samouczku przedstawiono będzie jak centralizować reguł biznesowych do firm logiki warstwy (logiki warstwy Biznesowej) służący jako pośrednik wymiany danych między warstwą prezentacji a warstwy DAL.


## <a name="introduction"></a>Wprowadzenie

Warstwa dostępu do danych (DAL) utworzone w [pierwszy samouczek](creating-a-data-access-layer-vb.md) bezpośrednio oddziela dane dostęp logiki z logiki prezentacji. Jednak podczas warstwy DAL prawidłowo oddziela szczegółów dostępu do danych z warstwy prezentacji, nie obsługuje on wymuszania żadnych reguł biznesowych, które mogą być zastosowane. Na przykład dla aplikacji może chcemy nie zezwalaj na `CategoryID` lub `SupplierID` pola `Products` tabeli można zmodyfikować po `Discontinued` pole ma wartość 1 lub może chcemy wymuszania reguł starszeństwa zabronienia sytuacje, w których Pracownik jest zarządzana przez osobę, która została dzierżawione po nich. Inny typowy scenariusz polega autoryzacji może tylko użytkownicy w określonej roli można usunąć produktów lub zmienić `UnitPrice` wartość.

W tym samouczku przedstawiono będzie jak centralizować tych reguł biznesowych do firm logiki warstwy (logiki warstwy Biznesowej) służący jako pośrednik wymiany danych między warstwą prezentacji a warstwy DAL. W przypadku aplikacji rzeczywistych logiki warstwy Biznesowej powinny zostać wdrożone jako osobne projektu biblioteki klas; Jednak te samouczki będzie wdrożymy logiki warstwy Biznesowej jako szereg klas w naszym `App_Code` folderu w celu uproszczenia struktury projektu. Rysunek 1 pokazuje architektury relacje warstwę prezentacji, logiki warstwy Biznesowej i warstwy DAL.


![Logiki warstwy Biznesowej oddziela warstwę prezentacji z Warstwa dostępu do danych i nakłada reguły biznesowe](creating-a-business-logic-layer-vb/_static/image1.png)

**Rysunek 1**: logiki warstwy Biznesowej oddziela warstwę prezentacji z Warstwa dostępu do danych i nakłada reguły biznesowe


Zamiast tworzenia osobnych klas do zaimplementowania naszych [logiki biznesowej](http://en.wikipedia.org/wiki/Business_logic), firma Microsoft może umieścić również tę logikę bezpośrednio w DataSet wpisana z częściowa klasy. Na przykład tworzenie i rozszerzanie zestawu danych typu odwołują się do pierwszego samouczka.

## <a name="step-1-creating-the-bll-classes"></a>Krok 1: Tworzenie klasy logiki warstwy Biznesowej

Nasze logiki warstwy Biznesowej będzie składać się z czterech klas, po jednej dla każdego TableAdapter w DAL; Każda z tych klas logiki warstwy Biznesowej ma metod pobierania, wstawianie, aktualizowanie i usuwanie z odpowiednich TableAdapter w warstwy DAL stosowanie reguł biznesowych odpowiednie.

Aby bardziej bezpośrednio Rozdziel klasy związane z warstwy DAL i logiki warstwy Biznesowej, Utwórz dwa podfoldery w `App_Code` folderu, `DAL` i `BLL`. Po prostu kliknij prawym przyciskiem myszy `App_Code` folder w Eksploratorze rozwiązań i wybierz nowy Folder. Po utworzeniu tych dwóch folderów, Przenieś zestawu danych typu utworzony w pierwszym samouczku do `DAL` podfolderu.

Następnie należy utworzyć cztery pliki klasy logiki warstwy Biznesowej w `BLL` podfolderu. W tym celu kliknij prawym przyciskiem myszy `BLL` podfolderu, Dodaj nowy element lub wybrać szablon klasy. Nazwa klasy cztery `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, i `EmployeesBLL`.


![Dodaj cztery nowe klasy w folderze App_Code](creating-a-business-logic-layer-vb/_static/image2.png)

**Rysunek 2**: dodawać nowe klasy cztery do `App_Code` folderu


Następnie możemy dodać metody do każdej klasy, po prostu opakowywać metody zdefiniowane dla TableAdapters z pierwszym samouczku. Na razie tych metod będzie po prostu Wywołaj bezpośrednio do warstwy DAL; zostanie zwrócona później, aby dodać wszelka logika biznesowa potrzebne.

> [!NOTE]
> Jeśli używasz programu Visual Studio, Standard Edition lub nowszy (oznacza to, że *nie* przy użyciu programu Visual Web Developer), opcjonalnie można zaprojektować wizualnie przy użyciu klas [Projektant klas](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Zapoznaj się [blogu Projektant klasy](https://blogs.msdn.com/classdesigner/default.aspx) uzyskać więcej informacji o tej nowej funkcji w programie Visual Studio.


Aby uzyskać `ProductsBLL` musimy dodać łącznie siedem metod klasy:

- `GetProducts()`Zwraca wszystkie produkty
- `GetProductByProductID(productID)`Zwraca iloczyn z Identyfikatorem określonego produktu
- `GetProductsByCategoryID(categoryID)`Zwraca wszystkie produkty z określonej kategorii
- `GetProductsBySupplier(supplierID)`Zwraca wszystkie produkty od określonego dostawcy
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)`Wstawia nowego produktu do bazy danych przy użyciu wartości przekazywane w; Zwraca `ProductID` wartości nowo wstawionej rekordu
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)`aktualizuje istniejącą produktu w bazie danych przy użyciu wartości przekazywane w; Zwraca `True` Jeśli dokładnie jeden wiersz został zaktualizowany, `False` inaczej
- `DeleteProduct(productID)`Usuwa określony produkt z bazy danych

ProductsBLL.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample1.vb)]

Metody, które po prostu zwracają dane `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, i `GetProductBySuppliersID` są bardzo prosta zgodnie z ich po prostu wywołują warstwy DAL. W niektórych scenariuszach może być reguły biznesowe, które muszą zostać wdrożone na tym poziomie (takie jak reguły autoryzacji na podstawie aktualnie zalogowanego użytkownika lub roli, do której należy użytkownik), po prostu pozostanie tych metod jako — jest. Dla tych metod następnie logiki warstwy Biznesowej służy jako serwer proxy, przez który warstwę prezentacji uzyskuje dostęp do danych z Warstwa dostępu do danych.

`AddProduct` i `UpdateProduct` metody zarówno zająć w jako parametry wartości różnych pól produktu i dodawanie nowego produktu lub zaktualizuj istniejącą, odpowiednio. Od wielu `Product` kolumny tabeli może akceptować `NULL` wartości (`CategoryID`, `SupplierID`, i `UnitPrice`, kilka), te parametry dla wejścia `AddProduct` i `UpdateProduct` mapowane na wykorzystanie kolumn [typy dopuszczające wartości zerowe](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Typy dopuszczające wartości zerowe dopiero zaczynasz korzystać z .NET 2.0 i podaj technika wskazującą, czy typ wartości należy zamiast tego można `Nothing`. Zapoznaj się [Vick Pawła](http://www.panopticoncentral.net/)w wpis w blogu [prawdy o typy dopuszczające wartości zerowe i VB](http://www.panopticoncentral.net/archive/2004/06/04/1180.aspx) i dokumentacja techniczna dotycząca [Nullable](https://msdn.microsoft.com/library/b3h38hb0%28VS.80%29.aspx) struktury, aby uzyskać więcej informacji.

Wszystkie trzy metody zwracać wartość logiczną wskazującą, czy wiersz został dodaje, zaktualizowane lub usunięte, ponieważ operacja nie może spowodować w wierszu. Na przykład, jeśli wywołuje developer strony `DeleteProduct` przekazując `ProductID` produktu nieistniejącą `DELETE` oświadczenie wydane w bazie danych nie będzie miał znaczenia i w związku z tym `DeleteProduct` metoda zwróci `False`.

Należy pamiętać, że podczas dodawania nowego produktu lub aktualizowanie istniejącego traktujemy w wartości pól produktu nowe lub zmodyfikowane jako listę wartości skalarne, a nie akceptuje `ProductsRow` wystąpienia. Ta metoda została wybrana, ponieważ `ProductsRow` klasę pochodną ADO.NET `DataRow` klasy, która nie ma domyślnego konstruktora bez parametrów. Aby utworzyć nową `ProductsRow` wystąpienia, należy najpierw utworzymy `ProductsDataTable` wystąpienia, a następnie wywołaj jego `NewProductRow()` — metoda (który czynności wykonane w `AddProduct`). Tego braku rears jego head, gdy firma Microsoft, przejdź do wstawiania i aktualizowania produktów przy użyciu elementu ObjectDataSource. Krótko mówiąc ObjectDataSource podejmie próbę utworzenia wystąpienia parametrów wejściowych. Jeśli metoda logiki warstwy Biznesowej oczekuje `ProductsRow` wystąpienia, ObjectDataSource będzie próbował utworzyć, ale się niepowodzeniem z powodu braku domyślnego konstruktora bez parametrów. Więcej informacji na temat tego problemu można znaleźć w następujących dwóch wpisów fora programu ASP.NET: [aktualizowanie ObjectDataSources z zestawami danych Strongly-Typed](https://forums.asp.net/1098630/ShowPost.aspx), i [Problem z ObjectDataSource i Strongly-Typed DataSet](https://forums.asp.net/1048212/ShowPost.aspx).

Następnie, w obu `AddProduct` i `UpdateProduct`, kod tworzy `ProductsRow` wystąpienia i wypełnia wartości przekazane tylko. Podczas przypisywania wartości elementach DataColumns DataRow może wystąpić różnych kontroli weryfikacji na poziomie pola. W związku z tym ręczne wprowadzanie się wartości przekazana do DataRow zapewnia ważności danych przekazywany do metody logiki warstwy Biznesowej. Niestety klasy DataRow jednoznacznie generowane przez program Visual Studio nie należy używać typów dopuszczających wartości zerowe. Zamiast aby wskazać, że określonego elementu DataColumn w DataRow powinna odpowiadać `NULL` bazy danych musi używamy wartość `SetColumnNameNull()` — metoda.

W `UpdateProduct` możemy najpierw obciążenia w ramach produktu, aby zaktualizować przy użyciu `GetProductByProductID(productID)`. A to może się wydawać niepotrzebnych podróży do bazy danych, to dodatkowe podróży będzie okazać się zastanowić w przyszłości samouczków, z którymi Eksploruj optymistycznej współbieżności. Optymistycznej współbieżności to technika, aby upewnić się, że dwóch użytkowników jednocześnie pracujących na tym samym danych przypadkowo nie zastępuj zmiany siebie nawzajem. Przechwytywanie całego rekordu również ułatwia tworzenie metody aktualizacji w logiki warstwy Biznesowej, które modyfikować tylko podzbiór kolumn DataRow. Gdy firma Microsoft Eksploruj `SuppliersBLL` zajmiemy się tym przykładem takich klasy.

Na koniec należy pamiętać, że `ProductsBLL` klasa ma [atrybutu obiektu DataObject](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) zastosować dla niego ( `[System.ComponentModel.DataObject]` składni bezpośrednio przed instrukcją klasy w górnej części pliku) i metody [ Atrybuty DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). `DataObject` Atrybut oznacza klasy jako odpowiednie dla powiązania obiektu [kontrolki ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), podczas gdy `DataObjectMethodAttribute` wskazuje na przeznaczenie metody. Jak zajmiemy się w przyszłości samouczki, ASP.NET 2.0 ObjectDataSource ułatwia deklaratywnie dostępu do danych z klasy. Aby filtrować listę klas można powiązać w Kreatorze ObjectDataSource, domyślnie tylko klasy oznaczona jako `DataObjects` są wyświetlane na liście rozwijanej kreatora. `ProductsBLL` Klasy będzie działać równie dobrze bez tych atrybutów, ale dodanie ich ułatwia współpracować w elemencie ObjectDataSource kreatora.

## <a name="adding-the-other-classes"></a>Dodawanie innych klas

Z `ProductsBLL` klasy pełną, nadal trzeba dodać klasy do pracy z kategorii, dostawców i pracowników. Poświęć chwilę, aby utworzyć następujące klasy i metody za pomocą pojęcia w powyższym przykładzie:

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

Jest jedna metoda warto zauważyć `SuppliersBLL` klasy `UpdateSupplierAddress` metody. Ta metoda zawiera interfejs umożliwiający aktualizowanie tylko informacji o adresie dostawcy. Wewnętrznie ta metoda odczytuje w `SupplierDataRow` obiektu dla określonego `supplierID` (przy użyciu `GetSupplierBySupplierID`), ustawia jego właściwości związanych z adresem, a następnie wywołuje w dół do `SupplierDataTable`w `Update` metody. `UpdateSupplierAddress` Następujące metody:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample2.vb)]

Zapoznaj się w tym artykule pobierania dla moich pełnej implementacji klasy logiki warstwy Biznesowej.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Krok 2: Uzyskiwanie dostępu do Typizowane zbiory danych za pośrednictwem klas logiki warstwy Biznesowej

W pierwszym samouczku widzieliśmy przykłady Praca bezpośrednio z zestawem danych wpisane programowo, ale dodając nasze klasy logiki warstwy Biznesowej warstwy prezentacji powinny działać względem logiki warstwy Biznesowej zamiast tego. W `AllProducts.aspx` przykład z pierwszym samouczku `ProductsTableAdapter` został użyty do powiązania z listą produktów programu Element GridView, jak pokazano w poniższym kodzie:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample3.vb)]

Do używania nowych logiki warstwy Biznesowej jest po prostu zastąpić pierwszy wiersz kodu klasy, wszystkie które musi zostać zmienione `ProductsTableAdapter` obiekt z `ProductBLL` obiektu:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample4.vb)]

Klasy logiki warstwy Biznesowej można również można uzyskać dostępu do deklaratywnie (jak wpisane w zestawie danych) przy użyciu elementu ObjectDataSource. Firma Microsoft omówione w niniejszym dokumencie ObjectDataSource bardziej szczegółowo w następujących samouczkach.


[![Lista produktów są wyświetlane w widoku GridView](creating-a-business-logic-layer-vb/_static/image4.png)](creating-a-business-logic-layer-vb/_static/image3.png)

**Rysunek 3**: Lista produktów są wyświetlane w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-business-logic-layer-vb/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Krok 3: Dodawanie walidacji na poziomie pola do klasy DataRow

Weryfikacji na poziomie pola są kontroli, które odnoszą się do wartości właściwości obiektów biznesowych podczas uaktualniania. Niektóre reguły weryfikacji na poziomie pola produktów obejmują:

- `ProductName` Pole musi zawierać 40 znaków lub mniej długości
- `QuantityPerUnit` Pole musi być 20 znaków lub mniej długości
- `ProductID`, `ProductName`, I `Discontinued` pola są wymagane, ale pozostałe pola są opcjonalne
- `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, I `ReorderLevel` pola musi być większa lub równa zero

Te reguły można i powinien zostać przedstawiony na poziomie bazy danych. Jest za długa na `ProductName` i `QuantityPerUnit` pola są przechwytywane przez typy danych tych kolumn w `Products` tabeli (`nvarchar(40)` i `nvarchar(20)`odpowiednio). Określa, czy pola są wymagane i opcjonalne są wyrażone według, kolumny tabeli bazy danych umożliwia `NULL` s. Cztery [ograniczenia sprawdzania](https://msdn.microsoft.com/library/ms188258.aspx) istnieje która upewnij się, że tylko wartości większe niż lub równa zeru można tworzyć w `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, lub `ReorderLevel` kolumn.

Oprócz stosować te zasady w bazie danych one należy również wymusić na poziomie zestawu danych. W rzeczywistości długość pola i określa, czy wartość jest wymagany lub opcjonalny są już przechwytywane dla każdego elementu DataTable zestawu elementach DataColumns. Aby wyświetlić istniejące weryfikacji na poziomie pola automatycznie dostarczona, przejdź do Projektanta obiektów DataSet, wybierz jeden z DataTables pola, a następnie przejdź do okna właściwości. Jak pokazano na rysunku 4, `QuantityPerUnit` elementu DataColumn w `ProductsDataTable` ma maksymalną długość wynoszącą 20 znaków i umożliwić `NULL` wartości. Firma Microsoft podejmie próbę ustawić `ProductsDataRow`w `QuantityPerUnit` właściwości na wartość ciągu jest dłuższa niż 20 znaków `ArgumentException` zostanie wygenerowany.


[![Element DataColumn udostępnia podstawowe weryfikacji na poziomie pola](creating-a-business-logic-layer-vb/_static/image7.png)](creating-a-business-logic-layer-vb/_static/image6.png)

**Rysunek 4**: DataColumn udostępnia podstawowe pole weryfikacji na poziomie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-business-logic-layer-vb/_static/image8.png))


Niestety, firma Microsoft nie można określić kontroli granice, takich jak `UnitPrice` wartość musi być większa lub równa zero, przy użyciu okna właściwości. Aby udostępnić ten typ weryfikacji na poziomie pola należy utworzyć program obsługi zdarzeń dla tabeli DataTable [columnchanging —](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) zdarzeń. Jak wspomniano w [poprzedniego samouczek](creating-a-data-access-layer-vb.md), zestawu danych, DataTables i DataRow obiekty utworzone przez zestaw danych wpisane można rozszerzyć za pomocą klasy częściowe. Ta technika, możemy utworzyć `ColumnChanging` programu obsługi zdarzeń dla `ProductsDataTable` klasy. Rozpocznij od utworzenia klasy w `App_Code` folder o nazwie `ProductsDataTable.ColumnChanging.vb`.


[![Dodaj nową klasę w folderze App_Code](creating-a-business-logic-layer-vb/_static/image10.png)](creating-a-business-logic-layer-vb/_static/image9.png)

**Rysunek 5**: Dodaj nową klasę do `App_Code` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-business-logic-layer-vb/_static/image11.png))


Następnie należy utworzyć programu obsługi zdarzeń dla `ColumnChanging` zdarzenie, które zapewnia, że `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, i `ReorderLevel` wartości w kolumnie (Jeśli nie `NULL`) jest większa lub równa zero. Jeśli takie kolumny jest poza zakresem, throw `ArgumentException`.

ProductsDataTable.ColumnChanging.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample5.vb)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Krok 4: Dodawanie niestandardowych reguł biznesowych do klas logiki warstwy Biznesowej

Oprócz weryfikacji na poziomie pola mogą być ogólne niestandardowe zasady biznesowe obejmujących różne jednostki lub koncepcji nie można wyrazić na poziomie jednej kolumny, takich jak:

- Jeśli produkt jest przerywane, jego `UnitPrice` nie można zaktualizować
- Pracownik miejsca zamieszkania musi być taka sama jak jego menedżera miejsca zamieszkania
- Produkt nie przerywane, jeśli jest tylko produktu dostarczony przez dostawcę

Klasy logiki warstwy Biznesowej powinien zawierać kontroli, aby zapewnić zgodność z zasadami biznesowymi aplikacji. Te sprawdzenia można dodać bezpośrednio do metody, których dotyczą.

Załóżmy, że wymagane przez naszych reguł biznesowych, że produktu nie można oznaczyć wycofane Jeśli był tylko produktu od danego dostawcy. Oznacza to jeśli produktu *X* był tylko produktu, firma Microsoft zakupionych od dostawcy *Y*, firma Microsoft nie można oznaczyć *X* jako zaprzestać; Jeśli jednak dostawca *Y*nam dostarczony wraz z trzech produktów *A*, *B*, i *C*, firma Microsoft może oznaczyć any i wszystkie te jako przerywane. Reguła biznesowa nieparzysta, ale reguły biznesowe i rozsądkiem zawsze nie są wyrównane!

Aby wymusić tę regułę biznesową w `UpdateProducts` Rozpoczniemy przez sprawdzenie, czy metoda `Discontinued` ustawiono `True` , a jeśli tak, czy nazywamy `GetProductsBySupplierID` ustalenie, jak wiele produktów, firma Microsoft zakupionych od dostawcy tego produktu. Jeśli tylko jeden produkt jest zakupione od tego dostawcy, możemy throw `ApplicationException`.


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample6.vb)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Odpowiedzi na błędy sprawdzania poprawności w warstwie prezentacji

Podczas wywoływania metody logiki warstwy Biznesowej z warstwą prezentacji firma Microsoft może zdecydować, czy próbę obsługiwać wszystkie wyjątki, które może zostać wywołane lub daj bąbelkowy do platformy ASP.NET (które zgłosi `HttpApplication`w `Error` zdarzeń). Do obsługi wyjątku podczas pracy z logiki warstwy Biznesowej programowo, możemy użyć [spróbuj... CATCH](https://msdn.microsoft.com/library/fk6t46tz%28VS.80%29.aspx) bloku, jak przedstawiono na poniższym przykładzie:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample7.vb)]

Zajmiemy się w przyszłości samouczki, obsługi wyjątków, występujących podczas korzystania z danych logiki warstwy Biznesowej w sieci Web kontroli wstawiania, aktualizacji, lub usuwanie danych mogą być obsługiwane bezpośrednio w obsłudze zdarzeń zamiast konieczności zawijać kodu w `Try...Catch` bloków.

## <a name="summary"></a>Podsumowanie

Na różne warstwy, z których każdy hermetyzuje określonej roli, co jest dobrze zaprojektowanej architektury aplikacji. W pierwszym samouczku tej serii artykułu utworzyliśmy Warstwa dostępu do danych, korzystając z wpisanych zestawów danych; w tym samouczku budujemy warstwy logiki biznesowej jako szereg klas w naszej aplikacji `App_Code` folderu, które wywołują naszych DAL w dół. Logiki warstwy Biznesowej implementuje logiki biznesowej i na poziomie pola do naszej aplikacji. Oprócz tworzenia oddzielnych logiki warstwy Biznesowej, jak robiliśmy w tym samouczku inną opcją jest rozszerzać metody TableAdapters przy użyciu klas częściowych. Jednak ta technika pozwala firmie Microsoft w celu zastąpienia istniejących metod nie jest on rozdzielić naszych DAL i naszych logiki warstwy Biznesowej prawidłowo jako podejście, które firma Microsoft zostało wykonane w tym artykule.

DAL i logiki warstwy Biznesowej pełną jest gotowy do uruchomienia na naszych warstwy prezentacji. W [następny samouczek](master-pages-and-site-navigation-vb.md) firma Microsoft będzie wybranie krótki przekierowania z tematy dotyczące dostępu do danych i zdefiniowanie układ spójne strony do użycia w całym samouczków.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Liz Shulok, firmy Dennis Patterson Santos Artur i Hilton Giesenow. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](creating-a-data-access-layer-vb.md)
[dalej](master-pages-and-site-navigation-vb.md)
