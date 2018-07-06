---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
title: Tworzenie warstwy logiki biznesowej (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku zobaczymy, jak centralizować własne reguły biznesowe do Business Logic warstwy (LOGIKI) służąca jako pośrednik wymianę danych między t...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 142e5181-29ce-4bb9-907b-2a0becf7928b
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: eaa91cc46f0d043983c252c1d0adfc413f3b582d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832357"
---
<a name="creating-a-business-logic-layer-vb"></a>Tworzenie warstwy logiki biznesowej (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_2_VB.exe) lub [Pobierz plik PDF](creating-a-business-logic-layer-vb/_static/datatutorial02vb1.pdf)

> W tym samouczku opisano jak centralizować własne reguły biznesowe do Business Logic warstwy (LOGIKI) służąca jako pośrednik wymianę danych między warstwą prezentacji i warstwy DAL.


## <a name="introduction"></a>Wprowadzenie

Warstwa dostępu do danych (DAL) utworzone w [pierwszego samouczka dotyczącego](creating-a-data-access-layer-vb.md) klarownie oddziela dane dostępu logiki od logiki prezentacji. Jednak gdy warstwy DAL nie pozostawia żadnych śladów oddziela szczegóły dostępu do danych z warstwy prezentacji, nie wymusza żadnych reguł biznesowych, które mogą mieć zastosowanie. Na przykład dla naszej aplikacji warto nie zezwalaj na `CategoryID` lub `SupplierID` pola `Products` tabeli do zmodyfikowania kiedy `Discontinued` pole ma wartość 1 lub firma Microsoft może chcieć wymuszania reguł związanych ze stażem pracy zabronienia sytuacje, w których Pracownik jest zarządzana przez kogoś, kto został zatrudniony po nich. Inny typowy scenariusz polega autoryzacji może być tylko użytkownicy w określonej roli można usunąć produktów lub zmienić `UnitPrice` wartość.

W tym samouczku opisano jak centralizować tych reguł biznesowych do Business Logic warstwy (LOGIKI) służąca jako pośrednik wymianę danych między warstwą prezentacji i warstwy DAL. W przypadku aplikacji rzeczywistych LOGIKI powinny zostać wdrożone jako oddzielny projekt biblioteki klas; Jednakże potrzeby tych samouczków firma Microsoft będzie implementowana LOGIKI jako szereg klas w naszym `App_Code` folder w celu uproszczenia struktury projektu. Rysunek 1 przedstawia architektury relacje Warstwa prezentacji, LOGIKI i warstwy DAL.


![LOGIKI warstwy prezentacji są oddzielone od warstwy dostępu do danych i nakłada reguły biznesowe](creating-a-business-logic-layer-vb/_static/image1.png)

**Rysunek 1**: LOGIKI warstwy prezentacji są oddzielone od warstwy dostępu do danych i nakłada reguły biznesowe


Zamiast tworzenia osobnych klas do zaimplementowania naszych [logikę biznesową](http://en.wikipedia.org/wiki/Business_logic), firma Microsoft może umieścić również tę logikę bezpośrednio w wpisany zestaw danych z klasami, częściowe. Na przykład tworzenie i rozszerzanie wpisany zestaw danych odnoszą się do pierwszego samouczka dotyczącego.

## <a name="step-1-creating-the-bll-classes"></a>Krok 1: Tworzenie klas LOGIKI

Nasze LOGIKI będzie składać się z czterech klas, jeden dla każdego TableAdapter w DAL; Każda z tych klas LOGIKI mają metody do pobierania, wstawianie, aktualizowanie i usuwanie z odpowiednich TableAdapter w warstwy DAL stosowanie reguł biznesowych odpowiednie.

Aby bardziej klarownie należy oddzielić klasy związane z warstwy DAL i LOGIKI, Utwórz dwa podfoldery w `App_Code` folderze `DAL` i `BLL`. Po prostu kliknij prawym przyciskiem myszy `App_Code` folder w Eksploratorze rozwiązań i wybierz nowy Folder. Po utworzeniu oba te foldery, Przenieś wpisana zestawu danych utworzonego w pierwszym samouczku do `DAL` podfolderu.

Następnie należy utworzyć cztery pliki klasy LOGIKI w `BLL` podfolderu. W tym celu kliknij prawym przyciskiem myszy `BLL` podfolder, wybierz pozycję Dodaj nowy element, a następnie wybierz szablon klasy. Nazwa klasy cztery `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, i `EmployeesBLL`.


![Dodanie czterech nowych klas w folderze App_Code](creating-a-business-logic-layer-vb/_static/image2.png)

**Rysunek 2**: dodanie czterech nowych klas do `App_Code` folderu


Następnie Dodajmy metody do każdej klasy, po prostu opakowywać metody zdefiniowane dla elementów TableAdapter z pierwszego samouczka. Na razie metody te po prostu wywoła bezpośrednio do warstwy DAL; zostanie zwrócona nowszej, aby dodać wszelka logika biznesowa potrzebne.

> [!NOTE]
> Jeśli używasz programu Visual Studio, Standard Edition lub nowszy (oznacza to, że jesteś *nie* przy użyciu programu Visual Web Developer), opcjonalnie można zaprojektować wizualnie przy użyciu klas [projektanta klas](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Zapoznaj się [blogu Projektant klasy](https://blogs.msdn.com/classdesigner/default.aspx) Aby uzyskać więcej informacji na temat tej nowej funkcji w programie Visual Studio.


Aby uzyskać `ProductsBLL` musimy dodać daje w sumie siedmiu metod klasy:

- `GetProducts()` Zwraca wszystkie produkty
- `GetProductByProductID(productID)` Zwraca iloczyn z Identyfikatorem określony produkt
- `GetProductsByCategoryID(categoryID)` Zwraca wszystkie produkty z określonej kategorii
- `GetProductsBySupplier(supplierID)` Zwraca wszystkie produkty z określonego dostawcy
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` Wstawia nowy produkt do bazy danych przy użyciu wartości przekazywane w; Zwraca `ProductID` wartość nowo wstawionej rekordu
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` aktualizacje istniejącego produktu w bazie danych przy użyciu wartości przekazywane w; Zwraca `True` Jeśli dokładnie jeden wiersz został zaktualizowany, `False` inaczej
- `DeleteProduct(productID)` Usuwa określony produkt z bazy danych

ProductsBLL.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample1.vb)]

Metody, które po prostu zwrócenia danych `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, i `GetProductBySuppliersID` są dość prosta, jak one po prostu wywołanie w dół warstwę DAL. W niektórych scenariuszach może być reguły biznesowe, które powinny być wdrożone na tym poziomie (takie jak reguły autoryzacji na podstawie aktualnie zalogowanego użytkownika lub roli, do której należy użytkownik), po prostu pozostawimy tych metod jako-to. W przypadku tych metod następnie LOGIKI służy jako serwer proxy, przez które Warstwa prezentacji uzyskuje dostęp do danych bazowych z warstwy dostępu do danych.

`AddProduct` i `UpdateProduct` metody zarówno zająć jako parametry wartości różnych pól produktu i dodawanie nowego produktu lub zaktualizować istniejącą, odpowiednio. Od wielu `Product` kolumn w tabeli może akceptować `NULL` wartości (`CategoryID`, `SupplierID`, i `UnitPrice`, kilka), te parametry dla wejścia `AddProduct` i `UpdateProduct` mapowane na kolumny używa [typów dopuszczających wartości zerowe](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Typy dopuszczające wartości zerowe są nowe w .NET 2.0 i zapewniać technika wskazującą, czy typ wartości powinien, zamiast tego można `Nothing`. Zapoznaj się [Paul Vick](http://www.panopticoncentral.net/)firmy wpis w blogu [prawdziwych informacji dotyczących typów dopuszczających wartości zerowe i VB](http://www.panopticoncentral.net/archive/2004/06/04/1180.aspx) i dokumentacji technicznej dla [Nullable](https://msdn.microsoft.com/library/b3h38hb0%28VS.80%29.aspx) struktury, aby uzyskać więcej informacji.

Wszystkie trzy metody zwracać wartość logiczną wskazującą, czy wiersz został wstawione, zaktualizowane lub został usunięty, ponieważ operacja nie może powodować wierszu. Na przykład, jeśli deweloper strona wywołuje `DeleteProduct` przekazując `ProductID` produktu nieistniejącej `DELETE` oświadczenie wydane w bazie danych nie będzie miał znaczenia i w związku z tym `DeleteProduct` metoda zwróci `False`.

Należy pamiętać, że podczas dodawania nowego produktu lub aktualizowania istniejącego podejmujemy w produkcie nowe lub zmodyfikowane wartości pól jako listę wartości skalarnych w przeciwieństwie do akceptowania `ProductsRow` wystąpienia. Ta metoda została wybrana, ponieważ `ProductsRow` klasa pochodzi od ADO.NET `DataRow` klasy, która nie ma domyślnego konstruktora bez parametrów. Aby można było utworzyć nowy `ProductsRow` wystąpienia, należy najpierw trzeba utworzyć `ProductsDataTable` wystąpienia, a następnie wywołaj jej `NewProductRow()` — metoda (co możemy zrobić `AddProduct`). Gdy firma Microsoft, przejdź do wstawiania i aktualizowania produkty za pomocą kontrolki ObjectDataSource, tego braku rears orzechów głową. Krótko mówiąc kontrolki ObjectDataSource podejmie próbę utworzenia wystąpienia parametrów wejściowych. Jeśli metoda LOGIKI oczekuje `ProductsRow` wystąpienie kontrolki ObjectDataSource spróbuje utworzyć, ale nie ze względu na Brak domyślnego konstruktora bez parametrów. Aby uzyskać więcej informacji na temat tego problemu można znaleźć w dwa następujące wpisy na forach platformy ASP.NET: [aktualizowanie ObjectDataSources z zestawami danych Strongly-Typed](https://forums.asp.net/1098630/ShowPost.aspx), i [Problem za pomocą kontrolki ObjectDataSource i Strongly-Typed DataSet](https://forums.asp.net/1048212/ShowPost.aspx).

Następnie w obu `AddProduct` i `UpdateProduct`, kod tworzy `ProductsRow` wystąpienia i wypełnia ją wartościami właśnie przekazany. Podczas przypisywania wartości do elementach DataColumns DataRow może wystąpić różnych testów weryfikacyjnych na poziomie pola. W związku z tym ręczne wprowadzanie się wartości przekazana do DataRow temu ważność danych jest przekazywany do metody LOGIKI. Niestety DataRow silnie typizowanych klas wygenerowanych przez program Visual Studio, nie należy używać typów dopuszczających wartości null. Przeciwnie aby wskazać, że określonego elementu DataColumn w DataRow powinien odpowiadać `NULL` bazy danych wartości, trzeba użyć `SetColumnNameNull()` — metoda.

W `UpdateProduct` możemy najpierw załadować w ramach produktu, aby zaktualizować za pomocą `GetProductByProductID(productID)`. Chociaż może wydawać się od niepotrzebne podróży do bazy danych, to dodatkowe podróży dowiedzie cenna w przyszłości samouczków, przedstawiających optymistycznej współbieżności. Optymistyczna współbieżność jest techniką, aby upewnić się, że dwóch użytkowników, którzy pracują jednocześnie te same dane nie przypadkowe zastąpienie zmian siebie nawzajem. Przechwytywanie cały rekord również ułatwia tworzenie metody aktualizacji w LOGIKI, która modyfikować tylko podzbiór kolumn DataRow. Gdy będziemy eksplorować `SuppliersBLL` zajmiemy się tym przykładem takich klas.

Na koniec należy pamiętać, że `ProductsBLL` klasa ma [atrybut obiektu DataObject](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) zastosowano ( `[System.ComponentModel.DataObject]` składni bezpośrednio przed instrukcją klasy w górnej części pliku) i metody mają [ Atrybuty DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). `DataObject` Atrybut oznacza klasy jako obiekt odpowiednie powiązanie [formantu ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), podczas gdy `DataObjectMethodAttribute` wskazuje na przeznaczenie metody. Jak zobaczymy w przyszłości samouczki, ASP.NET 2.0 ObjectDataSource ułatwia deklaratywne dostępu do danych z klasy. Aby filtrować listę możliwych klas, które można powiązać w Kreatorze ObjectDataSource, domyślnie tylko tych klas, które są oznaczone jako `DataObjects` są wyświetlane na liście rozwijanej kreatora. `ProductsBLL` Klasy będzie działać równie dobrze bez tych atrybutów, ale dodanie ich sprawia, że łatwiej pracować w Kreatorze ObjectDataSource.

## <a name="adding-the-other-classes"></a>Dodawanie innych klas

Za pomocą `ProductsBLL` klasy pełną, nadal należy dodać do klas do pracy z kategorii, dostawców i pracowników. Poświęć chwilę, aby utworzyć następujące klasy i metody przy użyciu koncepcji w powyższym przykładzie:

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

Jedną z metod warte odnotowania jest `SuppliersBLL` klasy `UpdateSupplierAddress` metody. Ta metoda zapewnia interfejs do aktualizowania po prostu dostawcy informacje o adresie. Wewnętrznie ta metoda odczytuje w `SupplierDataRow` obiektu dla określonego `supplierID` (przy użyciu `GetSupplierBySupplierID`), ustawia jego właściwości związanych z adresem, a następnie wywołuje się w dół, do `SupplierDataTable`firmy `Update` metody. `UpdateSupplierAddress` Następujące metody:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample2.vb)]

Można znaleźć w tym artykule do pobrania dla mojego pełną implementację klasy LOGIKI.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Krok 2: Uzyskiwanie dostępu do Typizowanych zestawów danych za pośrednictwem klasy LOGIKI

W pierwszym samouczku widzieliśmy przykłady bezpośredniej pracy z kontrolą typów w zestawie danych programowo, ale dodając naszych zajęć LOGIKI warstwy prezentacji powinien działać w przypadku LOGIKI zamiast tego. W `AllProducts.aspx` przykład z pierwszego samouczka dotyczącego `ProductsTableAdapter` zostało użyte do utworzenia powiązania z listą produktów programu w kontrolce GridView, jak pokazano w poniższym kodzie:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample3.vb)]

Do korzystania z nowej LOGIKI klas, wszystkie, które muszą być zmienione się pierwszy wiersz kodu po prostu zastąpić `ProductsTableAdapter` obiekt z `ProductBLL` obiektu:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample4.vb)]

Klasy LOGIKI również możliwy deklaratywne (jak wpisane w zestawie danych) za pomocą kontrolki ObjectDataSource. Omawiane będą ObjectDataSource bardziej szczegółowo w ramach następujących samouczków.


[![Lista produktów są wyświetlane w widoku GridView](creating-a-business-logic-layer-vb/_static/image4.png)](creating-a-business-logic-layer-vb/_static/image3.png)

**Rysunek 3**: Lista produktów jest wyświetlany w kontrolce GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-business-logic-layer-vb/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Krok 3: Dodawanie walidacji na poziomie pola do klasy DataRow

Weryfikacji na poziomie pola są testy, które odnoszą się do wartości właściwości obiektów biznesowych podczas wstawiania lub aktualizowania. Niektóre reguły walidacji na poziomie pola dla produktów obejmują:

- `ProductName` Pole musi zawierać 40 znaków lub mniej znaków
- `QuantityPerUnit` Pole musi być 20 znaków lub mniej znaków
- `ProductID`, `ProductName`, I `Discontinued` pola są wymagane, ale wszystkie inne pola są opcjonalne
- `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, I `ReorderLevel` pola musi być większa lub równa zero

Te reguły można i powinien zostać przedstawiony na poziomie bazy danych. Limit znaków `ProductName` i `QuantityPerUnit` pola są przechwytywane przez typy danych tych kolumn w `Products` tabeli (`nvarchar(40)` i `nvarchar(20)`odpowiednio). Czy pola są wymagane i opcjonalne są wyrażone w przypadku kolumny tabeli bazy danych umożliwia `NULL` s. Cztery [ograniczenia check](https://msdn.microsoft.com/library/ms188258.aspx) istnieje, upewnij się, że tylko wartości większe niż lub równa zero mogą przesłać go do `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, lub `ReorderLevel` kolumn.

Oprócz Wymuszanie tych zasad w bazie danych są powinien również są wymuszane na poziomie zestawu danych. W rzeczywistości długość pola i tego, czy wartość jest wymagane lub opcjonalne są już przechwytywane dla każdego elementu DataTable zestawu elementach DataColumns. Aby wyświetlić istniejące weryfikacji na poziomie pola dostarczana automatycznie, przejdź do Projektanta obiektów DataSet, wybierz jedną z DataTables pola, a następnie przejdź do okna właściwości. Jak pokazano na rysunku 4, `QuantityPerUnit` DataColumn w `ProductsDataTable` może się składać maksymalnie 20 znaków i umożliwić `NULL` wartości. Jeśli firma Microsoft podejmie próbę ustaw `ProductsDataRow`firmy `QuantityPerUnit` właściwości na wartość ciągu jest dłuższa niż 20 znaków `ArgumentException` zostanie zgłoszony.


[![DataColumn zapewnia podstawową walidację na poziomie pola](creating-a-business-logic-layer-vb/_static/image7.png)](creating-a-business-logic-layer-vb/_static/image6.png)

**Rysunek 4**: DataColumn zapewnia podstawowe pole weryfikacji na poziomie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-business-logic-layer-vb/_static/image8.png))


Niestety, firma Microsoft nie można określić granice kontroli, takie jak `UnitPrice` wartość musi być większa lub równa zero, w oknie właściwości. Aby udostępnić ten typ weryfikacji na poziomie pola musimy utworzyć program obsługi zdarzeń dla tabeli DataTable [columnchanging —](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) zdarzeń. Jak wspomniano w [poprzedni Samouczek](creating-a-data-access-layer-vb.md), obiektów DataSet, DataTable i DataRow, utworzonych przez wpisany zestaw danych można rozszerzyć za pomocą klasy częściowe. Ta technika, możemy utworzyć `ColumnChanging` program obsługi zdarzeń dla `ProductsDataTable` klasy. Rozpocznij od utworzenia klasy w `App_Code` folder o nazwie `ProductsDataTable.ColumnChanging.vb`.


[![Dodaj nową klasę w folderze App_Code](creating-a-business-logic-layer-vb/_static/image10.png)](creating-a-business-logic-layer-vb/_static/image9.png)

**Rysunek 5**: Dodaj nową klasę do `App_Code` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-business-logic-layer-vb/_static/image11.png))


Następnie należy utworzyć program obsługi zdarzeń dla `ColumnChanging` zdarzenia, które zapewnia, że `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, i `ReorderLevel` wartości w kolumnie (w przeciwnym razie `NULL`) są większe niż lub równa zero. Jeśli takie kolumny jest poza zakresem, throw `ArgumentException`.

ProductsDataTable.ColumnChanging.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample5.vb)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Krok 4: Dodawanie niestandardowych reguł biznesowych do LOGIKI klas

Oprócz weryfikacji na poziomie pola mogą być ogólne niestandardowych reguł biznesowych, które obejmują różne jednostki i pojęcia, które nie można wyrazić na poziomie pojedynczej kolumny, takich jak:

- Jeśli produkt jest przerywane, jego `UnitPrice` nie można zaktualizować
- Pracownika miejsca zamieszkania musi być taka sama jak jego menedżera miejsca zamieszkania
- Produkt nie może zostać zakończona, jeśli jest tylko produkty, które są dostarczane przez dostawcę

Klasy LOGIKI powinna zawierać kontrole w celu zapewnienia zgodności z zasadami biznesowymi aplikacji. Te sprawdzenia można dodawać bezpośrednio do metody, których dotyczą.

Wyobraź sobie, reguły biznesowe określają, że produktu nie można oznaczyć nieobsługiwane Jeśli był tylko produkty z danego dostawcy. Oznacza to jeśli produkt *X* została tylko produkty, firma Microsoft zakupić od dostawcy *Y*, firma Microsoft nie można oznaczyć *X* jako zaprzestać; Jeśli jednak dostawca *Y*zasilane nam trzy produkty *A*, *B*, i *C*, firma Microsoft może oznaczyć wszystkie i wszystkie te jako wycofane. Reguła biznesowa nieparzysta, ale reguły biznesowe i zdroworozsądkowe zawsze nie są wyrównane!

Aby wymuszają tę regułę biznesową w `UpdateProducts` Zaczniemy przez sprawdzenie, czy metoda `Discontinued` została ustawiona na `True` i, jeśli tak, możemy wywołać `GetProductsBySupplierID` ustalenie, jak wiele produktów, firma Microsoft zakupić od dostawcy tego produktu. Jeśli tylko jeden produkt zakupu od dostawcy, firma Microsoft throw `ApplicationException`.


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample6.vb)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Odpowiadanie na błędy sprawdzania poprawności w warstwie prezentacji

Podczas wywoływania LOGIKI z warstwą prezentacji możemy zdecydować, czy próba obsługi wszystkich wyjątków, które może zostać wywołane, lub pozwól, aby je będą się pojawiać do platformy ASP.NET (który zgłosi `HttpApplication`firmy `Error` zdarzeń). Do obsługi wyjątku, pracując z LOGIKI programowo, możemy użyć [spróbuj... CATCH](https://msdn.microsoft.com/library/fk6t46tz%28VS.80%29.aspx) bloku, co ilustruje poniższy przykład:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample7.vb)]

Jak zobaczymy samouczki w przyszłości, obsługa wyjątków, występujących LOGIKI, korzystając z danych kontrolki internetowej do wstawiania, aktualizowania, lub usuwania danych są obsługiwane bezpośrednio w obsłudze zdarzeń mających do zawijania kodu, w przeciwieństwie do `Try...Catch` bloków.

## <a name="summary"></a>Podsumowanie

Dobrze zaprojektowanej architektury aplikacji jest specjalnie do różnych warstw, z których każdy hermetyzuje określonej roli. W pierwszym samouczku tej serii artykułu utworzyliśmy warstwy dostępu do danych, przy użyciu wpisanych zestawów danych; w tym samouczku utworzyliśmy warstwy logiki biznesowej jako szereg klas w naszej aplikacji `App_Code` folder, który wywołania w dół w naszym DAL. LOGIKI implementuje pole poziomie biznesowych i logiki dla naszej aplikacji. Oprócz tworzenia oddzielnych LOGIKI, ile My mieliśmy, w tym samouczku innym rozwiązaniem jest rozszerzać metody TableAdapter przy użyciu klas częściowych. Jednak przy użyciu tej metody nie pozwalają na zastąpienie istniejących metod ani nie jest on osobne nasze warstwy DAL i naszych LOGIKI prawidłowo jako podejście, które wykonaliśmy w tym artykule.

DAL i LOGIKI pełną możemy rozpocząć na nasze warstwy prezentacji. W [następnego samouczka](master-pages-and-site-navigation-vb.md) utworzymy zająć krótki przekierowania z tematy dotyczące dostępu do danych i definiowanie układu strony spójne używanych w całej samouczków.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Liz Shulok, Dennis Patterson, Carlos Santos i Hilton Giesenow. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](creating-a-data-access-layer-vb.md)
> [dalej](master-pages-and-site-navigation-vb.md)
