---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
title: Wzorzec/szczegół za pomocą listy punktowanej rekordów głównych z kontrolką DataList szczegółów (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku firma Microsoft będzie skompresować dwustronicowy wzorzec/szczegół raport z poprzedniego samouczka w pojedynczej strony przedstawiający listy punktowanej nazw kategorii na t...
ms.author: aspnetcontent
ms.date: 10/17/2006
ms.assetid: c727bb73-7b59-41a1-8dc3-623c6d69e7c2
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 0be6a03b2fbbf71a1f1d1e13bd4b9118981d0715
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841066"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-c"></a>Wzorzec/szczegół za pomocą listy punktowanej rekordów głównych z kontrolką DataList szczegółów (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_CS.exe) lub [Pobierz plik PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/datatutorial35cs1.pdf)

> W tym samouczku firma Microsoft będzie skompresować dwustronicowy wzorzec/szczegół raport z poprzedniego samouczka w pojedynczej strony Wyświetlanie listy punktowanej nazwy kategorii po lewej stronie ekranu i wybranej kategorii produktów, po prawej stronie ekranu.


## <a name="introduction"></a>Wprowadzenie

W [poprzedni Samouczek](master-detail-filtering-acess-two-pages-datalist-cs.md) przyjrzeliśmy się korzystania z oddzielnych raportu rekordu głównego/szczegółów na dwóch stronach. Na stronie głównej użyliśmy kontrolką elementu powtarzanego do renderowania listy punktowanej kategorii. Każda nazwa kategorii został hiperłącze, po kliknięciu spowoduje take użytkownika do strony szczegółów, w którym DataList dwie kolumny wykazało, że te produkty należące do wybranej kategorii.

W tym samouczku firma Microsoft będzie skompresować samouczek dwóch stron w pojedynczej strony Wyświetlanie listy punktowanej nazwy kategorii po lewej stronie ekranu o nazwie każdej kategorii, renderowane jako element LinkButton. Kliknięcie jednego nazwa kategorii LinkButtons wymusza odświeżenie strony i wiąże wybranej kategorii produktów s kontrolką DataList dwie kolumny po prawej stronie ekranu. Oprócz wyświetlania nazwy kategorii s, powtarzanego po lewej stronie pokazuje, ile ma łączna liczba produktów związanych z danej kategorii (patrz rysunek 1).


[![S Nazwa kategorii i całkowitą liczbę produktów, które są wyświetlane po lewej stronie](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image1.png)

**Rysunek 1**: s kategorii nazwa i całkowitą liczbę produktów, które są wyświetlane po lewej stronie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Krok 1: Wyświetlanie Repeater w lewej części ekranu

W tym samouczku musimy mieć listy punktowanej kategorii są wyświetlane po lewej stronie produktów s wybranej kategorii. Zawartość wewnątrz strony sieci web może być umieszczony przy użyciu standardowych elementów akapitu tagów HTML, spacje nierozdzielające `<table>` s i tak dalej lub za pomocą kaskadowych arkuszy stylów (CSS) technik. Wszystkie z naszych samouczków tej pory były używane techniki CSS do położenia. Gdy interfejs użytkownika nawigacji został skompilowany w naszej strony wzorcowej w [strony wzorcowe i nawigacja w witrynie](../introduction/master-pages-and-site-navigation-cs.md) samouczka użyliśmy *pozycjonowanie absolutne*, wskazując przesunięcie dokładny piksel nawigacji Lista i głównej zawartości. Alternatywnie CSS może służyć do pozycji jeden element w prawo lub lewo innego za pomocą *zmiennoprzecinkowy*. Firma Microsoft może mieć listy punktowanej kategorii są wyświetlane po lewej stronie wybranej kategorii produktów s według liczb zmiennoprzecinkowych elementu powtarzanego z lewej strony kontrolki DataList

Otwórz `CategoriesAndProducts.aspx` strony `DataListRepeaterFiltering` folderze i Dodaj do strony DataList i Repeater. Ustaw powtarzanego s `ID` do `Categories` i DataList s do `CategoryProducts`. Przejdź do widoku źródła i umieszczenie kontrolki DataList i Repeater w ramach ich własnych `<div>` elementów. Oznacza to, należy ująć elementu powtarzanego, w ramach `<div>` element pierwszy i następnie DataList we własnym `<div>` element bezpośrednio po elemencie powtarzanym. W tym momencie znaczników powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample1.aspx)]

Aby przestawić elementu powtarzanego z lewej strony kontrolki DataList, musimy użyć `float` atrybut stylu CSS, w następujący sposób:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample2.html)]

`float: left;` Liczby zmiennoprzecinkowe pierwszy `<div>` element na lewo od drugiej. `width` i `padding-right` ustawienia wskazują pierwszy `<div>` s `width` i dodaje dopełnienie ile między `<div>` zawartości elementu s i jego prawy margines. Aby uzyskać więcej informacji na temat liczb zmiennoprzecinkowych elementów w kodzie CSS zapoznaj się [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Zamiast określać ustawienia stylu bezpośrednio za pomocą pierwszego `<p>` element s `style` atrybutu, niech s, zamiast tego utwórz nową klasę CSS w `Styles.css` o nazwie `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample3.css)]

Następnie można zastąpić `<div>` z `<div class="FloatLeft">`.

Po dodaniu klasy CSS i konfigurowanie znaczników w `CategoriesAndProducts.aspx` strony, przejdź do projektanta. Powinien zostać wyświetlony powtarzanego liczb zmiennoprzecinkowych z lewej strony kontrolki DataList (chociaż po prawej stronie zarówno po prostu pojawiają się jako szare pola, ponieważ firma Microsoft ve do konfigurowania swoich źródeł danych lub szablonów).


[![Powtarzanego jest przestawione do lewej strony kontrolki DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image4.png)

**Rysunek 2**: Repeater jest przestawione do lewej strony kontrolki DataList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Krok 2: Określanie liczby produktów dla każdej kategorii

Za pomocą s DataList i Repeater otaczającego kompletny kod znaczników możemy ponownie gotowe, aby powiązać dane kategorii powtarzanego kontrolować. Jednak zgodnie z listy punktowanej kategorii na rysunku 1 przedstawiono, oprócz nazwy kategorii s musimy również wyświetlana jest liczba produktów skojarzonych z tej kategorii. Dostęp do tych informacji możemy:

- **Określ tę informację z klasy CodeBehind s strony ASP.NET.** Biorąc pod uwagę określonego *`categoryID`* określamy liczbę skojarzone produkty przez wywołanie metody `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` metody. Ta metoda zwraca `ProductsDataTable` którego `Count` właściwość wskazuje, ile `ProductsRow` istnieje s, czyli liczbę produktów dla określonego *`categoryID`*. Możemy utworzyć `ItemDataBound` program obsługi zdarzeń dla elementu powtarzanego, wywołująca, dla każdej kategorii, powiązany z elementu powtarzanego, `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` metody oraz jego liczbę w danych wyjściowych.
- **Aktualizacja `CategoriesDataTable` w zestawie danych wpisane, aby uwzględnić `NumberOfProducts` kolumny.** Następnie możemy zaktualizować `GetCategories()` method in Class metoda `CategoriesDataTable` Dołącz tę informację, lub też pozostawić `GetCategories()` jako-się i Utwórz nowy `CategoriesDataTable` metodę o nazwie `GetCategoriesAndNumberOfProducts()`.

Pozwól, s, zapoznaj się z obu tych technik jednocześnie. Pierwszym sposobem jest prostsza do zaimplementowania, ponieważ firma Microsoft don t wymagana aktualizacja warstwy dostępu do danych; jednak wymaga więcej komunikacji z bazą danych. Wywołanie `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` method in Class metoda `ItemDataBound` program obsługi zdarzeń dodaje wywołanie dodatkowe bazy danych dla każdej kategorii wyświetlane w elemencie powtarzanym. Dzięki tej technice istnieją *N* + wywołań 1 bazy danych, gdzie *N* jest liczba kategorii wyświetlane w elemencie powtarzanym. Za pomocą drugiego podejścia liczba produktu jest zwracany za pomocą informacji dotyczących poszczególnych kategorii z `CategoriesBLL` klasy s `GetCategories()` (lub `GetCategoriesAndNumberOfProducts()`) metody, powodując tym tylko jeden podróży w bazie danych.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Określanie liczby produktów w obsłudze zdarzeń ItemDataBound

Określanie liczby produktów dla każdej kategorii w elemencie powtarzanym s `ItemDataBound` program obsługi zdarzeń nie wymaga żadnych modyfikacji na naszych istniejących warstwy dostępu do danych. Wszystkie modyfikacje mogą być wprowadzane bezpośrednio z poziomu `CategoriesAndProducts.aspx` strony. Rozpocznij, dodając nowe kontrolki ObjectDataSource, o nazwie `CategoriesDataSource` za pośrednictwem tagu inteligentnego w elemencie powtarzanym s. Następnie skonfiguruj `CategoriesDataSource` ObjectDataSource, tak że pobiera dane z `CategoriesBLL` klasy s `GetCategories()` metody.


[![Konfigurowanie kontrolki ObjectDataSource, aby użyć klasy CategoriesBLL s GetCategories() — metoda](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image7.png)

**Rysunek 3**: Konfigurowanie kontrolki ObjectDataSource do użycia `CategoriesBLL` klasy s `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image9.png))


Każdy element na `Categories` Repeater musi klikania, a po kliknięciu uniemożliwić `CategoryProducts` DataList do wyświetlenia tych produktów dla wybranej kategorii. Można to zrobić, wprowadzając każdą kategorię hiperłącze połączeń do tej samej strony (`CategoriesAndProducts.aspx`), ale przekazując `CategoryID` za pośrednictwem ciąg zapytania, tak, jak widzieliśmy w poprzednim samouczku. Zaletą tego podejścia jest, że strony z wyświetloną produktów określoną kategorię s może być zakładek i indeksowany przez wyszukiwarki.

Alternatywnie można ułatwiamy każdej kategorii LinkButton, czyli podejście, które będą używane na potrzeby tego samouczka. Element LinkButton powoduje wyświetlenie w przeglądarce użytkownika s jako hiperłącze, ale kliknięcie powoduje odświeżenie strony; na odświeżenie strony DataList s ObjectDataSource musi zostać odświeżona do wyświetlenia tych produktów należących do wybranej kategorii. Na potrzeby tego samouczka przy użyciu hiperłącze sens więcej niż przy użyciu LinkButton; jednak może być inne scenariusze, w których korzystniejsze jest przy użyciu LinkButton. Chociaż podejście hiperłącze będzie idealne rozwiązanie w przypadku tego przykładu, umożliwiają zamiast tego zapoznaj się z pomocą element LinkButton s. Jak zajmiemy się tym, za pomocą LinkButton wprowadza wyzwania, które w przeciwnym razie nie będzie pojawiać z hiperłączem. W związku z tym w tym samouczku przy użyciu LinkButton spowoduje wyróżnić te wyzwania i rozwiązań pomagających organizacjom non-profit dla tych scenariuszy, w których warto używać LinkButton zamiast hiperłącze.

> [!NOTE]
> Zaleca się powtórzyć ten samouczek przy użyciu kontrolki Hiperlinku lub `<a>` element audytów element LinkButton.


Następujący kod przedstawia składni deklaratywnej w elemencie powtarzanym i kontrolki ObjectDataSource. Należy zwrócić uwagę na to, że szablony s Repeater renderować listy punktowanej z każdym elementem jako element LinkButton:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample4.aspx)]

> [!NOTE]
> W tym samouczku powtarzanego musi mieć swój stan widoku, włączone (należy zwrócić uwagę na pominięcie `EnableViewState="False"` ze składni deklaratywnej w elemencie powtarzanym s). W kroku 3 możemy utworzyć program obsługi zdarzeń dla elementu powtarzanego s `ItemCommand` zdarzeń, w którym będziemy aktualizować DataList s ObjectDataSource s `SelectParameters` kolekcji. Elementu powtarzanego s `ItemCommand`, jednak wygrał t fire, jeśli stan widoku jest wyłączone. Zobacz [Stumper pytania ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) i [swoje rozwiązanie](http://scottonwriting.net/sowBlog/posts/1268.aspx) Aby uzyskać więcej informacji o tym, dlaczego stan widoku, należy włączyć Repeater s `ItemCommand` wyzwolenie zdarzenia.


Element LinkButton z `ID` wartość właściwości `ViewCategory` nie ma jej `Text` zestawu właściwości. Jeśli po prostu chcemy była wyświetlana nazwa kategorii, firma Microsoft ma ustawiał właściwość Text deklaratywne, za pomocą składni wiązania danych, w następujący sposób:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample5.aspx)]

Jednak chcemy wyświetlić nazwę kategorii s *i* liczba produktów należących do tej kategorii. Te informacje można pobrać z elementu powtarzanego s `ItemDataBound` programu obsługi zdarzeń przez wywołania do `ProductBLL` klasy s `GetCategoriesByProductID(categoryID)` metody i określenie, ile rekordy są zwracane w wynikowym `ProductsDataTable`, jak poniższy kod przedstawia:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample6.cs)]

Rozpoczęliśmy poprzez zapewnienie, że firma Microsoft odnośnie do pracy z elementu danych (jeden którego `ItemType` jest `Item` lub `AlternatingItem`), a następnie Odwołaj `CategoriesRow` wystąpienia, która właśnie została powiązana z bieżącego `RepeaterItem`. Następnie określamy liczbę produktów dla tej kategorii, tworząc wystąpienie `ProductsBLL` klasy i wywoływania jego `GetCategoriesByProductID(categoryID)` metoda i określanie liczbę rekordów zwróconych, za pomocą `Count` właściwości. Na koniec `ViewCategory` LinkButton ItemTemplate to odwołania i jego `Text` właściwość jest ustawiona na *CategoryName* (*NumberOfProductsInCategory*), gdzie  *NumberOfProductsInCategory* jest w formacie liczbę miejsc dziesiętnych, zerowego.

> [!NOTE]
> Alternatywnie można dodaliśmy *formatowanie funkcja* do klasy związane z kodem strony s ASP.NET, który akceptuje kategorii s `CategoryName` i `CategoryID` wartości i zwraca `CategoryName` połączonych z liczbą produkty w danej kategorii (jak ustalono przez wywołanie metody `GetCategoriesByProductID(categoryID)` metody). Wyniki funkcji formatowania można deklaratywne przypisać s LinkButton właściwości tekstu, zastępując potrzebę `ItemDataBound` programu obsługi zdarzeń. Zapoznaj się [za pomocą kontrolek TemplateField w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) lub [formatowanie elementów DataList i Repeater na podstawie od danych](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) samouczki, aby uzyskać więcej informacji na temat korzystania z funkcji formatowania.


Po dodaniu tej obsługi zdarzeń, Poświęć chwilę, aby przetestować stronę za pośrednictwem przeglądarki. Należy zauważyć, jak każda kategoria znajduje się na liście punktowanej wyświetlanie nazwy kategorii s i liczba produktów skojarzonych z kategorii (zobacz rysunek 4).


[![Każdej kategorii s Nazwa i numer produkty są wyświetlane.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image10.png)

**Rysunek 4**: s każdej kategorii, nazwa i numer produkty są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Aktualizowanie`CategoriesDataTable`i`CategoriesTableAdapter`obejmujący wiele produktów dla każdej kategorii

Zamiast określania liczby produktów dla każdej kategorii, ponieważ s powiązany z elementu powtarzanego, możemy usprawnić tego procesu, dostosowując `CategoriesDataTable` i `CategoriesTableAdapter` w warstwie dostępu do danych, aby dołączyć tę informację w sposób macierzysty. Aby to osiągnąć, można dodać nową kolumnę, aby `CategoriesDataTable` do przechowywania liczby skojarzone produkty. Aby dodać nową kolumnę do elementu DataTable, otwórz element zestawu danych wpisane (`App_Code\DAL\Northwind.xsd`), kliknij prawym przyciskiem myszy na DataTable, aby zmodyfikować i wybierz pozycję Dodaj / kolumny. Dodaj nową kolumnę, aby `CategoriesDataTable` (zobacz rysunek 5).


[![Dodaj nową kolumnę do CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image13.png)

**Rysunek 5**: Dodaj nową kolumnę, aby `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image15.png))


Spowoduje to dodanie nowej kolumny o nazwie `Column1`, które można zmienić po prostu wpisać inną nazwę. Zmień nazwę tej nowej kolumny do `NumberOfProducts`. Następnie należy skonfigurować właściwości tej kolumny s. Kliknij nową kolumnę, a następnie przejdź do okna właściwości. Zmiana kolumny s `DataType` właściwość `System.String` do `System.Int32` i ustaw `ReadOnly` właściwości `True`, jak pokazano na rysunku 6.


![Ustaw typ danych i właściwości tylko do odczytu nowej kolumny.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image16.png)

**Rysunek 6**: Ustaw `DataType` i `ReadOnly` właściwości nowej kolumny.


Gdy `CategoriesDataTable` ma teraz `NumberOfProducts` kolumny, jego wartość nie jest ustawiany przez dowolne odpowiednie zapytań TableAdapter w s. Firma Microsoft może aktualizować `GetCategories()` metody zwracają te informacje, jeśli chcemy, aby takie informacje zwrócone za każdym razem, gdy są pobierane informacje o kategorii. Jeśli jednak potrzebujemy do pobrania liczba skojarzone produkty w ramach kategorii w rzadkich przypadkach (na przykład jako tylko do celów tego samouczka), a następnie firma może opuścić `GetCategories()` jako — jest i utworzenie nowej metody, która zwraca te informacje. Umożliwiają s Użyj ostatniego podejścia, tworząc nową metodę o nazwie `GetCategoriesAndNumberOfProducts()`.

Aby dodać ten nowy `GetCategoriesAndNumberOfProducts()` metody, kliknij prawym przyciskiem myszy `CategoriesTableAdapter` i wybierz opcję nowe zapytanie. Wybranie tej opcji powoduje TableAdapter zapytania Kreatora konfiguracji, które firma Microsoft ve używana wiele razy w poprzednich samouczkach w górę. W przypadku tej metody należy uruchomić kreatora, wskazując, że zapytanie używa instrukcji SQL zapytań ad-hoc, która zwraca wiersze.


[![Utwórz metodę, przy użyciu instrukcji SQL zapytań Ad-Hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image17.png)

**Rysunek 7**: Utwórz metody za pomocą instrukcji SQL zapytań Ad-Hoc ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image19.png))


[![Instrukcja SQL zwraca wiersze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image20.png)

**Rysunek 8**: zwraca wiersze instrukcji SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image22.png))


Na następnym ekranie kreatora monituje o nas na zapytanie do użycia. Do zwrócenia każdej kategorii s `CategoryID`, `CategoryName`, i `Description` pola wraz z liczbą produkty powiązane z danej kategorii, należy użyć następującego `SELECT` instrukcji:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample7.sql)]


[![Określ zapytanie do użycia](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image23.png)

**Rysunek 9**: Określ zapytanie do użycia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image25.png))


Należy pamiętać, że podzapytania, które oblicza liczbę produktów, skojarzone z kategorią aliasowana jako `NumberOfProducts`. To dopasowanie nazw powoduje, że wartość zwrócona przez obiekt tego podzapytania do skojarzenia z `CategoriesDataTable` s `NumberOfProducts` kolumny.

Po wprowadzeniu tego zapytania, ostatnim krokiem jest wybranie nazwę dla nowej metody. Użyj `FillWithNumberOfProducts` i `GetCategoriesAndNumberOfProducts` wypełnienia DataTable i zwrócenia DataTable wzorców, odpowiednio.


[![Nazwa nowego FillWithNumberOfProducts metody s TableAdapter i GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image26.png)

**Na rysunku nr 10**: Nazwa s nowy obiekt TableAdapter metody `FillWithNumberOfProducts` i `GetCategoriesAndNumberOfProducts` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image28.png))


W tym momencie warstwy dostępu do danych został rozszerzony do uwzględnienia liczba produktów według kategorii. Ponieważ nasze warstwy prezentacji kieruje wszystkie połączenia z warstwą dal za pośrednictwem osobnych warstwy logiki biznesowej należy dodać odpowiedni `GetCategoriesAndNumberOfProducts` metody `CategoriesBLL` klasy:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample8.cs)]

DAL i LOGIKI pełną, możemy ponownie gotowe powiązać te dane, aby `Categories` Repeater w `CategoriesAndProducts.aspx`! Jeśli był już utworzony ObjectDataSource elementu powtarzanego z Określanie liczby produktów w `ItemDataBound` części programu obsługi zdarzeń, Usuń ten ObjectDataSource i Repeater s `DataSourceID` właściwości również ustawienie; unwire Elementu powtarzanego s `ItemDataBound` zdarzenia z programu obsługi zdarzeń, usuwając `Handles Categories.OnItemDataBound` składni w klasie CodeBehind ASP.NET.

Za pomocą elementu powtarzanego w pierwotnego stanu, Dodaj nowe kontrolki ObjectDataSource, o nazwie `CategoriesDataSource` za pośrednictwem tagu inteligentnego w elemencie powtarzanym s. Konfigurowanie kontrolki ObjectDataSource używać `CategoriesBLL` klasy, zamiast go używać, ale `GetCategories()` metody zostały przez niego używane na `GetCategoriesAndNumberOfProducts()` zamiast (zobacz rysunek 11).


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image29.png)

**Rysunek 11**: Konfigurowanie kontrolki ObjectDataSource do użycia `GetCategoriesAndNumberOfProducts` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image31.png))


Następnie zaktualizuj `ItemTemplate` tak, aby LinkButton s `Text` właściwość deklaratywne przypisano przy użyciu składni wiązania danych i obejmuje zarówno `CategoryName` i `NumberOfProducts` pola danych. Ukończone w oznaczeniu deklaracyjnym dla powtarzanego i `CategoriesDataSource` wykonaj ObjectDataSource:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample9.aspx)]

Renderowany przez aktualizację DAL, aby dołączyć dane wyjściowe `NumberOfProducts` kolumny jest taka sama jak za pomocą `ItemDataBound` metody obsługi zdarzeń (zobacz rysunek 4, aby wyświetlić ekran zrzut powtarzanego wyświetlane nazwy kategorii i liczba produktów).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Krok 3: Wyświetlanie wybranej kategorii produktów s

W tym momencie mamy `Categories` wyświetlanie listy kategorii wraz z liczbą produktów w każdej kategorii elementu powtarzanego. Powtarzanego używa LinkButton dla każdej kategorii, że kliknięcie powoduje odświeżenie strony, w którym punktu firma Microsoft będzie musiała wyświetlić tych produktów dla wybranej kategorii w `CategoryProducts` DataList.

Jednym z wyzwań połączonego z nami jest jak DataList wyświetlanie tylko tych produktów dla wybranej kategorii. W [Master/szczegółów przy Wybieralna GridView wzorzec DetailsView szczegóły](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) samouczek widzieliśmy, jak tworzyć GridView której wiersze można wybrać, za pomocą wybranego wiersza s szczegółowe informacje są wyświetlane w DetailsView na tej samej stronie. GridView s ObjectDataSource zwrócone informacje o produktach przy użyciu `ProductsBLL` s `GetProducts()` metody podczas DetailsView s ObjectDataSource pobrać informacji o używaniu wybranego produktu `GetProductsByProductID(productID)` metody. *`productID`* Deklaratywne podano wartość parametru przez skojarzenie jej z wartością GridView s `SelectedValue` właściwości. Niestety, nie ma powtarzanego `SelectedValue` właściwości i nie może służyć jako źródło parametru.

> [!NOTE]
> Jest to jedna z tych problemów, które są wyświetlane, gdy za pomocą element LinkButton w elemencie powtarzanym. Miał użyliśmy hiperłącze do przekazywania `CategoryID` za pośrednictwem zmiennej querystring zamiast tego moglibyśmy użyć pole QueryString jako źródło dla wartości s parametru.


Zanim firma martwienia się o braku `SelectedValue` jednak let s najpierw powiązać kontrolki DataList ObjectDataSource i określić właściwości dla elementu powtarzanego, jego `ItemTemplate`.

Za pomocą kontrolek DataList s tagu inteligentnego, zoptymalizowany pod kątem można dodać nowego elementu ObjectDataSource, o nazwie `CategoryProductsDataSource` i skonfigurować go do używania `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` metody. Ponieważ DataList w tym samouczku udostępnia interfejs tylko do odczytu, możesz ustawić list rozwijanych w INSERT, UPDATE i usuwanie kart (Brak).


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetProductsByCategoryID(categoryID) s ProductsBLL, klasa](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image32.png)

**Rysunek 12**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image34.png))


Ponieważ `GetProductsByCategoryID(categoryID)` metoda oczekuje, że parametr wejściowy (*`categoryID`*), Kreator konfigurowania źródła danych pozwala określić źródło s parametru. Miał kategorii został na liście GridView lub kontrolką DataList, d ustawimy listy rozwijanej źródła parametru do kontroli i ControlID do `ID` danych formantu sieci Web. Jednak ponieważ nie ma elementu powtarzanego `SelectedValue` właściwości nie można użyć jako źródła parametru. Jeśli zaznaczysz, znajdziesz, że na liście rozwijanej ControlID zawiera tylko jeden formant `ID``CategoryProducts`, `ID` z kontrolki DataList.

Na razie ustawienie listy rozwijanej źródła parametru brak. Firma Microsoft będzie znajdą się programowo przypisywanie wartości tego parametru, gdy kategorii, który kliknięto element LinkButton w elemencie powtarzanym.


[![Czy Określa źródło parametru categoryID parametru](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image35.png)

**Rysunek 13**: nie określaj parametru źródło *`categoryID`* parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image37.png))


Po zakończeniu pracy kreatora Konfigurowanie źródła danych, program Visual Studio automatycznie generuje DataList s `ItemTemplate`. Zastąpić to ustawienie domyślne `ItemTemplate` przy użyciu szablonu możemy używane w poprzednim samouczku; ustawisz DataList s `RepeatColumns` właściwość 2. Po wprowadzeniu tych zmian oznaczeniu deklaracyjnym listy DataList i jego skojarzonego elementu ObjectDataSource powinien wyglądać następująco:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample10.aspx)]

Obecnie `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* parametru nigdy nie ustawiono, dzięki czemu produkty nie są wyświetlane podczas wyświetlania strony. Co należy zrobić to mają wartości tego parametru, ustaw na podstawie `CategoryID` kliknięto kategorii w elemencie powtarzanym. Wprowadza wyzwanie z dwóch powodów: po pierwsze, jak określamy, gdy element LinkButton w elemencie powtarzanym s `ItemTemplate` został kliknięty; oraz drugiego, jak możemy ustalić, `CategoryID` odpowiedniej kategorii, którego element LinkButton został kliknięty?

Ma element LinkButton, takich jak kontrolki przycisku i ImageButton `Click` zdarzeń i [ `Command` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). `Click` Zdarzeń zaprojektowano w celu po prostu należy pamiętać, że kliknięto element LinkButton. Czasami jednak oprócz biorąc pod uwagę, że kliknięto element LinkButton należy również przekazać kilku dodatkowych informacji do programu obsługi zdarzeń. Jeśli jest to przypadek, LinkButton s [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) i [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) właściwości można przypisać te dodatkowe informacje. Wówczas, gdy kliknięto element LinkButton jego `Command` generowane zdarzenie (zamiast jego `Click` zdarzeń), a program obsługi zdarzeń jest przekazywana wartości `CommandName` i `CommandArgument` właściwości.

Gdy `Command` zdarzenie jest wywoływane z w ramach szablonu w elemencie powtarzanym s elementu powtarzanego [ `ItemCommand` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) aktywuje się i jest przekazywany `CommandName` i `CommandArgument` wartości kliknięto element LinkButton (lub przycisku lub ImageButton). W związku z tym aby określić, gdy kliknięto element LinkButton w elemencie powtarzanym kategorii, należy wykonać następujące czynności:

1. Ustaw `CommandName` właściwość element LinkButton w elemencie powtarzanym s `ItemTemplate` na wartość (I był używany ListProducts). Jeśli ta `CommandName` wartość LinkButton s `Command` zdarzenia generowane, gdy kliknięto element LinkButton.
2. Ustaw LinkButton s `CommandArgument` właściwości wartość elementu bieżącej s `CategoryID`.
3. Utwórz procedurę obsługi zdarzeń dla elementu powtarzanego s `ItemCommand` zdarzeń. W przypadku obsługi zestawie zdarzeń `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametru wartość przekazana do `CommandArgument`.

Następujące `ItemTemplate` kod znaczników dla elementu powtarzanego kategorie implementuje kroki 1 i 2. Uwaga jak `CommandArgument` wartość jest przypisywana element danych s `CategoryID` przy użyciu składni wiązania z danymi:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample11.aspx)]

Przy każdym tworzeniu `ItemCommand` rozsądne zawsze najpierw sprawdzić przychodzącego jest program obsługi zdarzeń `CommandName` wartości, ponieważ *wszelkie* `Command` zdarzenia wygenerowane przez *wszelkie* przycisku, element LinkButton, lub ImageButton w elemencie powtarzanym spowoduje, że `ItemCommand` wyzwolenie zdarzenia. Obecnie tylko mamy jeden taki element LinkButton teraz, w przyszłości firma Microsoft (lub innemu deweloperowi na nasz zespół) może dodać kolejne Web kontrolki przycisków do powtarzanego, po kliknięciu wywołuje takie same `ItemCommand` programu obsługi zdarzeń. W związku z tym, jego s najlepiej zawsze upewnij się, możesz sprawdzić `CommandName` właściwości i kontynuować, tylko przy użyciu swojej logiki programowej dopasowuje do oczekiwaną wartość.

Po upewnieniu się, że przekazany do `CommandName` wartość jest równa ListProducts, program obsługi zdarzeń następnie przypisuje `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametru wartość przekazana do `CommandArgument`. Ta modyfikacja s ObjectDataSource `SelectParameters` automatycznie powoduje, że DataList ponownie powiązać się ze źródłem danych, Wyświetlanie produktów dla nowo wybranej kategorii.


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample12.cs)]

W przypadku te dodatki Nasz samouczek dotyczący jest pełny. Poświęć chwilę, aby przetestować działanie w przeglądarce. Rysunek 14 pokazuje ekran, po raz pierwszy, odwiedzając stronę. Ponieważ kategorii musi jeszcze zostać wybrany, produkty nie są wyświetlane. Kliknięcie kategorii, takich jak produktu, wyświetla tych produktów w kategorii produktów w widoku dwie kolumny (patrz rysunek 15).


[![Produkty nie są wyświetlane podczas pierwszego odwiedzając stronę](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image38.png)

**Rysunek 14**: produkty nie są wyświetlane podczas pierwszego odwiedzając stronę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image40.png))


[![Kliknięcie listy kategorii produktu pasujących produktów po prawej stronie](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image41.png)

**Rysunek 15**: kliknięcie kategorii produktu zawiera listę produktów dopasowanie, po prawej stronie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image43.png))


## <a name="summary"></a>Podsumowanie

Jak widzieliśmy w tym samouczku i poprzednim wzorzec/szczegół raportów mogą być rozłożone na dwóch stronach lub skonsolidowane w jeden. Wyświetlanie raportu głównych/szczegółów na jednej stronie, jednak wprowadza wyzwania, o tym, jak najlepiej układem głównym i rejestruje szczegółowe informacje na stronie. W *Master/szczegółów przy Wybieralna GridView wzorzec DetailsView szczegóły* samouczków, mieliśmy rekordów szczegółów są wyświetlane powyżej rekordy główne; w ramach tego samouczka użyliśmy technik CSS, aby mieć wartość zmiennoprzecinkowa rekordy główne do po lewej stronie szczegółów.

Wraz z wyświetlania raportów wzorzec/szczegół, mieliśmy również możliwość sprawdzenia, jak pobrać liczbę produktów skojarzonych z każdej kategorii, a także, jak przeprowadzić logiki po stronie serwera, gdy element LinkButton (lub przycisku lub ImageButton) zostanie kliknięty z poziomu Elementu powtarzanego.

Nasze badania raportów wzorzec/szczegół za pomocą kontrolek DataList i Repeater kończy się w tym samouczku. Nasz kolejny zbiór samouczki przedstawiają sposób dodawania, edytowania i usuwania funkcji za pomocą kontrolki DataList.

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) samouczek dotyczący liczb zmiennoprzecinkowych elementów CSS przy użyciu CSS
- [Pozycjonowanie CSS](http://www.brainjar.com/css/positioning/) więcej informacji na temat rozmieszczania elementów za pomocą CSS
- [Rozmieszczania się zawartość HTML](http://www.w3schools.com/html/html_layout.asp) przy użyciu `<table>` s i innych elementów kodu HTML do pozycjonowania

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Zack Jones. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-filtering-acess-two-pages-datalist-cs.md)
> [dalej](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
