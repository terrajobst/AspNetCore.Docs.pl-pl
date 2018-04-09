---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: Wzorzec/szczegół za pomocą listy punktowanej rekordów wzorca z szczegóły elementu DataList (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku firma Microsoft będzie skompresować dwustronicowy wzorzec/szczegół raport poprzedniej samouczka w pojedynczej strony, wyświetlanie listy punktowane nazwy kategorii na t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d87dc7f4fb00e96d9eb2653e6fbc1efb8bb656c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Wzorzec/szczegół za pomocą listy punktowanej rekordów wzorca z szczegóły elementu DataList (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) lub [pobierania plików PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> W tym samouczku firma Microsoft będzie skompresować dwustronicowy wzorzec/szczegół raport poprzedniej samouczka w pojedynczej strony, wyświetlanie listy punktowane nazwy kategorii po lewej stronie ekranu i wybranej kategorii produktów po prawej stronie ekranu.


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczek](master-detail-filtering-acess-two-pages-datalist-vb.md) analizujemy oddzielającego raportu wzorzec/szczegół między dwoma stronami. Na stronie głównej użyliśmy kontrolce elementu powtarzanego do renderowania listy punktowanej kategorii. Każda nazwa kategorii została hiperłącze, że po kliknięciu spowoduje podjęcie użytkownika do strony szczegółów, w którym DataList dwie kolumny wykazało tych produktów należących do wybranej kategorii.

W tym samouczku firma Microsoft będzie skompresować Samouczek stron w pojedynczej strony, wyświetlanie listy punktowane nazwy kategorii po lewej stronie ekranu z każdej renderowane jako element LinkButton nazwy kategorii. Kliknij jeden z nazwy kategorii LinkButtons powoduje odświeżenie strony i wiąże wybranej kategorii produktów s DataList dwie kolumny, z prawej strony ekranu. Oprócz wyświetlania każdej nazwy kategorii s, powtarzanego po lewej stronie pokazuje, ile łączna liczba produktów są przeznaczone dla danej kategorii (zobacz rysunek 1).


[![S Nazwa kategorii i łączna liczba produktów są wyświetlane po lewej stronie](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Rysunek 1**: s kategorii nazwy i łączna liczba produktów są wyświetlane po lewej stronie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Krok 1: Wyświetlanie elementu powtarzanego w lewej części ekranu

W tym samouczku musimy mieć listy punktowanej kategorii są wyświetlane na lewo od wybranej kategorii produktów s. Zawartość na stronie sieci web może być umieszczony za pomocą standardowych elementów akapitu tagów HTML, spacje nierozdzielające `<table>` s i tak dalej lub przy użyciu kaskadowych arkuszy stylów (CSS) technik. Wszystkie z naszych samouczków dotychczasowych były używane techniki CSS do rozmieszczania. Gdy budujemy interfejs użytkownika nawigacji w naszej strony wzorcowej [stron wzorcowych i nawigacji w witrynie](../introduction/master-pages-and-site-navigation-vb.md) samouczek użyliśmy *bezwzględny*, wskazującą przesunięcie pikseli dokładne nawigacji Lista i głównej zawartości. Alternatywnie CSS może służyć do pozycji jeden element do prawej lub lewej innego za pośrednictwem *przestawne*. Firma Microsoft może mieć listy punktowanej kategorii są wyświetlane na lewo od wybranej kategorii produktów s przez zmiennoprzecinkową elementu powtarzanego z lewej strony elementu DataList

Otwórz `CategoriesAndProducts.aspx` strony z `DataListRepeaterFiltering` folderu i Dodaj do strony elementu powtarzanego i DataList. Wartość elementu powtarzanego s `ID` do `Categories` i DataList s do `CategoryProducts`. Przejdź do widoku źródłowego i umieść formanty elementu powtarzanego i DataList w ich własnych `<div>` elementów. Oznacza to, ujmij powtarzanego w `<div>` element pierwszy, a następnie DataList we własnym `<div>` element bezpośrednio po elemencie powtarzanym. W tym momencie z znaczników powinien wyglądać podobnie do następującego:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Aby float elementu powtarzanego z lewej strony elementu DataList, należy użyć `float` atrybutu style CSS, w następujący sposób:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

`float: left;` Pojawia się pierwszy `<div>` element w lewo drugi. `width` i `padding-right` ustawienia wskazują pierwszy `<div>` s `width` i ile dopełnienie dodawany jest odstęp `<div>` zawartość elementu s i jego prawy margines. Aby uzyskać więcej informacji na temat przestawne elementy w kodzie CSS zapoznaj się [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Zamiast Określ ustawienie stylu bezpośrednio przez pierwszy `<p>` elementu s `style` atrybutu, niech s zamiast tego utwórz nową klasę CSS w `Styles.css` o nazwie `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

Następnie firma Microsoft może zastąpić `<div>` z `<div class="FloatLeft">`.

Po dodaniu klasy CSS i skonfigurowaniu znaczników w `CategoriesAndProducts.aspx` strony, przejdź do projektanta. Powinny pojawić się powtarzanego przestawionych do lewej strony elementu DataList (chociaż prawo teraz zarówno właśnie są wyświetlane jako szare pola od nas kolejnych jeszcze, aby skonfigurować ich źródeł danych lub szablonów).


[![Powtarzanego jest przestawione do lewej strony elementu DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Rysunek 2**: elementu powtarzanego jest przestawione do lewej strony elementu DataList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Krok 2: Określenie, ile produktów dla każdej kategorii

Z elementu powtarzanego i DataList s otaczającego znaczników pełną re gotowe, aby powiązać dane kategorii powtarzanego sterować. Jak pokazano na liście punktowanej kategorii na rysunku 1, oprócz nazwy kategorii s również Potrzebujemy jednak wyświetlana liczba produktów skojarzonych z kategorii. Dostęp do informacji możemy:

- **Należy określić te informacje z klasy związane z kodem s strony ASP.NET.** Podane określonego *`categoryID`* możemy określić liczbę produktów skojarzonych przez wywołanie metody `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` metody. Ta metoda zwraca `ProductsDataTable` którego `Count` właściwość wskazuje, ile `ProductsRow` istnieje s, który jest liczba produktów dla określonego *`categoryID`*. Można utworzyć `ItemDataBound` obsługi zdarzenia powtarzanego wywołującą, dla każdej kategorii, powiązany z elementu powtarzanego, `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` — metoda i zawiera jego liczbę w danych wyjściowych.
- **Aktualizacja `CategoriesDataTable` w zestawie danych wpisany, aby uwzględnić `NumberOfProducts` kolumny.** Firma Microsoft może następnie zaktualizuj `GetCategories()` metody w `CategoriesDataTable` Dołącz tę informację, lub też pozostawić `GetCategories()` jako- i utworzyć nowy `CategoriesDataTable` wywołano metodę `GetCategoriesAndNumberOfProducts()`.

Let s Eksploruj obu tych metod. Pierwszym sposobem jest prostsza do zaimplementowania, ponieważ będziemy ADAM t wymagana aktualizacja Warstwa dostępu do danych; jednak wymaga więcej komunikacji z bazą danych. Wywołanie `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` metoda `ItemDataBound` obsługi zdarzeń dodaje wywołanie dodatkowe bazy danych dla każdej kategorii wyświetlany w elemencie powtarzanym. Ta metoda jest *N* + wywołania 1 bazy danych, gdzie *N* jest liczba kategorie wyświetlane w elemencie powtarzanym. Z drugiej metody, liczba produktu, jest zwracany za informacji dotyczących poszczególnych kategorii z `CategoriesBLL` klasy s `GetCategories()` (lub `GetCategoriesAndNumberOfProducts()`) metoda, powodując w podróży tylko jeden z bazą danych.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Określenie, ile produktów w obsłudze zdarzeń ItemDataBound

Określenie, ile produktów dla każdej kategorii w elemencie powtarzanym s `ItemDataBound` program obsługi zdarzeń nie wymaga wszelkie modyfikacje naszych istniejących Warstwa dostępu do danych. Można wprowadzić modyfikacje wszystkich bezpośrednio z poziomu `CategoriesAndProducts.aspx` strony. Rozpocznij od dodania nowego elementu ObjectDataSource o nazwie `CategoriesDataSource` za pomocą tagów inteligentnych s elementu powtarzanego. Następnie skonfiguruj `CategoriesDataSource` ObjectDataSource, tak że pobiera dane z `CategoriesBLL` klasy s `GetCategories()` metody.


[![Skonfiguruj ObjectDataSource do użycia klasy CategoriesBLL s GetCategories() — metoda](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Rysunek 3**: Konfigurowanie ObjectDataSource użyć `CategoriesBLL` klasy s `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))


Każdy element `Categories` elementu powtarzanego musi być aktywne, a po kliknięciu uniemożliwić `CategoryProducts` DataList do wyświetlenia tych produktów dla wybranej kategorii. Można to osiągnąć poprzez każdej kategorii hiperłącze, łączenie do tej samej strony (`CategoriesAndProducts.aspx`), ale przechodzącą `CategoryID` za pośrednictwem querystring, taki sam sposób, jak widzieliśmy w poprzednim samouczka. Zaletą tej metody jest strona wyświetlania produktów określonej kategorii s może być zakładki i indeksowany przez wyszukiwarki.

Alternatywnie, firma Microsoft może wprowadzać każdej kategorii LinkButton, czyli podejście, które będą używane na potrzeby tego samouczka. Element LinkButton renderowania w przeglądarce użytkownika s jako hiperłącze, ale po kliknięciu powoduje odświeżenie strony; strony DataList s ObjectDataSource musi zostać odświeżona do wyświetlenia tych produktów należących do wybranej kategorii. W tym samouczku przy użyciu hiperłącze sens więcej niż przy użyciu LinkButton; jednak może być inne scenariusze, w których przy użyciu element LinkButton jest bardziej korzystne. Podejście hiperłącze może być idealne rozwiązanie w tym przykładzie, pozwolić, aby zamiast tego eksplorowania przy użyciu element LinkButton s. Jak zajmiemy się tym, za pomocą element LinkButton przedstawiono niektóre wyzwania, które w przeciwnym razie nie będą pojawiać z hiperłączem. W związku z tym za pomocą element LinkButton w tym samouczku zostanie zaznacz te problemy i zapewnić rozwiązania tych scenariuszy, w którym firma Microsoft może wystąpić potrzeba użycia element LinkButton zamiast hiperłącza.

> [!NOTE]
> Zaleca się powtórzyć ten samouczek przy użyciu formantu hiperłącza lub `<a>` elementu zamiast element LinkButton.


Następujący kod przedstawia składni deklaratywnej powtarzanego i ObjectDataSource. Należy pamiętać, że szablony s elementu powtarzanego renderowania listy punktowanej z każdym elementem jako element LinkButton:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> W tym samouczku powtarzanego musi mieć stan widoku włączone (należy zwrócić uwagę na pominięcie `EnableViewState="False"` z składni deklaratywnej elementu powtarzanego s). W kroku 3 możemy utworzyć program obsługi zdarzeń dla elementu powtarzanego s `ItemCommand` zdarzeń, w którym firma Microsoft będzie aktualizować DataList s ObjectDataSource s `SelectParameters` kolekcji. Powtarzanego s `ItemCommand`, jednak won t fire, jeśli stan widoku jest wyłączone. Zobacz [Stumper A pytania ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) i [rozwiązanie](http://scottonwriting.net/sowBlog/posts/1268.aspx) Aby uzyskać więcej informacji o tym, dlaczego stan widoku, należy włączyć powtarzanego s `ItemCommand` zdarzenia.


Element LinkButton z `ID` wartość właściwości `ViewCategory` nie ma jej `Text` zestawu właściwości. Czy możemy miał tylko wyświetlić nazwę kategorii, mieć ustawimy usługę· właściwości Text deklaratywnie, za pomocą składni wiązania z danymi w następujący sposób:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

Jednak chcemy wyświetlić nazwę kategorii s *i* liczba produktów należących do tej kategorii. Te informacje można znaleźć w elemencie powtarzanym s `ItemDataBound` obsługi zdarzeń za wywołania do `ProductBLL` klasy s `GetCategoriesByProductID(categoryID)` metody oraz określenie, ile rekordy są zwracane w wynikowym `ProductsDataTable`, jako następujący kod przedstawia:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Rozpoczniemy za zapewnienie, że firma Microsoft odnośnie do pracy z elementu danych (jeden których `ItemType` jest `Item` lub `AlternatingItem`), a następnie odwołania `CategoriesRow` wystąpienia, która właśnie została powiązana z bieżącą `RepeaterItem`. Następnie możemy określić liczba produktów w tej kategorii przez utworzenie wystąpienia `ProductsBLL` klasy, wywoływanie jej `GetCategoriesByProductID(categoryID)` — metoda i określającą liczbę rekordów zwróconych przy użyciu `Count` właściwości. Na koniec `ViewCategory` LinkButton w szablonie ItemTemplate jest odwołania i jego `Text` właściwość jest ustawiona na *CategoryName* (*NumberOfProductsInCategory*), gdzie  *NumberOfProductsInCategory* jest formatowana jako liczba z zerowej liczby miejsc dziesiętnych.

> [!NOTE]
> Alternatywnie można dodaliśmy *formatowania funkcja* do klasy związane z kodem strony s ASP.NET, który akceptuje kategorii s `CategoryName` i `CategoryID` wartości i zwraca `CategoryName` połączonych z liczbą produkty w danej kategorii (zgodnie z ustaleniami wywoływania `GetCategoriesByProductID(categoryID)` metody). Wyniki formatowania funkcja może zostać przypisana deklaratywnie s LinkButton właściwości Text, zastępując potrzebę `ItemDataBound` obsługi zdarzeń. Zapoznaj się [przy użyciu TemplateFields w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) lub [formatowania DataList i powtarzanego oparte na danych](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) samouczki, aby uzyskać więcej informacji na temat używania funkcji formatowania.


Po dodaniu ten program obsługi zdarzeń, Poświęć chwilę, aby przetestować za pośrednictwem przeglądarki. Należy zauważyć, jak każdej kategorii jest wyświetlane na liście punktowanej wyświetlanie nazwy kategorii s i liczba produktów skojarzonych z kategorii (patrz rysunek 4).


[![Każdej kategorii s Nazwa i numer produktów są wyświetlane.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Rysunek 4**: s kategorii każdą nazwę i numer produktów są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Aktualizowanie`CategoriesDataTable`i`CategoriesTableAdapter`do obejmują liczbę produktów dla każdej kategorii

Zamiast określania liczby produktów dla każdej kategorii, ponieważ s powiązany z elementu powtarzanego, możemy usprawnić ten proces przez dostosowanie wartości właściwości `CategoriesDataTable` i `CategoriesTableAdapter` w warstwie dostępu do danych, aby uwzględnić te informacje w sposób macierzysty. Można to osiągnąć, możemy dodać nową kolumnę `CategoriesDataTable` do przechowywania liczba produktów skojarzone. Aby dodać nową kolumnę do elementu DataTable, Otwórz zestaw danych wpisane (`App_Code\DAL\Northwind.xsd`), kliknij prawym przyciskiem myszy na DataTable, aby zmodyfikować i wybierz polecenie Dodaj / kolumny. Dodaj nową kolumnę do `CategoriesDataTable` (patrz rysunek 5).


[![Dodaj nową kolumnę do CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Rysunek 5**: Dodaj nową kolumnę do `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))


Spowoduje to dodanie nowej kolumny o nazwie `Column1`, które można zmienić po prostu wpisując inną nazwę. Zmień nazwę tej nowej kolumny `NumberOfProducts`. Następnie należy skonfigurować właściwości tej kolumny s. Kliknij na nowej kolumny, a następnie przejdź do okna właściwości. Zmień kolumnę s `DataType` właściwość z `System.String` do `System.Int32` i ustaw `ReadOnly` właściwości `True`, jak pokazano na rysunku 6.


![Ustaw typ danych i właściwości tylko do odczytu nowej kolumny.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Rysunek 6**: Ustaw `DataType` i `ReadOnly` właściwości nowej kolumny.


Gdy `CategoriesDataTable` ma teraz `NumberOfProducts` kolumny, jego wartość nie jest ustawiona przy użyciu jednej z odpowiednich zapytań s TableAdapter. Firma Microsoft może aktualizować `GetCategories()` metoda zwraca informację, jeśli chcemy takie informacje zwracane za każdym razem, gdy pobieranie informacji o kategorii. Jeśli jednak potrzebujemy do pobrania liczba produktów skojarzonych dla kategorii w rzadkich przypadkach, (na przykład jako tylko do celów tego samouczka), a następnie firma Microsoft może narazić `GetCategories()` jako — jest i utworzenie nowej metody, która zwraca te informacje. Let s posłuż się tą metodą drugie, utworzenie nowej metody o nazwie `GetCategoriesAndNumberOfProducts()`.

Aby dodać to nowe `GetCategoriesAndNumberOfProducts()` metody, kliknij prawym przyciskiem myszy `CategoriesTableAdapter` i wybierz nową kwerendę. Wybranie tej opcji powoduje TableAdapter zapytania Kreatora konfiguracji, które firma Microsoft kolejnych używana wiele razy w poprzednim samouczki w górę. Dla tej metody należy uruchomić kreatora, wskazując, że zapytanie używa instrukcji SQL ad-hoc, która zwraca wiersze.


[![Create — metoda, za pomocą instrukcji SQL Ad Hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Rysunek 7**: tworzenie przy użyciu metody instrukcji SQL Ad-Hoc ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))


[![Instrukcja SQL zwraca wiersze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Rysunek 8**: zwraca wiersze instrukcji SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))


Na następnym ekranie kreatora monituje o nas dla zapytania do użycia. Aby powrócić do poszczególnych kategorii s `CategoryID`, `CategoryName`, i `Description` pola wraz z liczbą produktów skojarzonych z tą kategorią, należy użyć następującego `SELECT` instrukcji:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]


[![Określ zapytanie do użycia](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Rysunek 9**: określ kwerendę do użycia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))


Należy pamiętać, że podzapytania, który oblicza liczbę produktów skojarzonych z kategorii używane z aliasem jako `NumberOfProducts`. Tego dopasowania nazewnictwa powoduje, że wartość zwrócona przez ten podzapytania ma być skojarzone z `CategoriesDataTable` s `NumberOfProducts` kolumny.

Po wprowadzeniu tego zapytania, ostatnim krokiem jest wybierz nazwę dla nowej metody. Użyj `FillWithNumberOfProducts` i `GetCategoriesAndNumberOfProducts` wypełnienia DataTable i przywrócenie elementu DataTable wzorców, odpowiednio.


[![Nazwa nowej FillWithNumberOfProducts metody s TableAdapter i GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Na rysunku nr 10**: Nazwa s nowy obiekt TableAdapter metody `FillWithNumberOfProducts` i `GetCategoriesAndNumberOfProducts` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))


W tym momencie Warstwa dostępu do danych został rozszerzony do obejmują liczbę produktów dla każdej kategorii. Ponieważ naszych warstwy prezentacji kieruje wszystkie połączenia z warstwą dal za pomocą osobnych warstwy logiki biznesowej należy dodać odpowiednią `GetCategoriesAndNumberOfProducts` metody `CategoriesBLL` klasy:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

DAL i logiki warstwy Biznesowej pełną, możemy re gotowe powiązać te dane do `Categories` elementu powtarzanego w `CategoriesAndProducts.aspx`! Jeśli był już utworzone w elemencie ObjectDataSource dla elementu powtarzanego z Określanie liczby produktów w `ItemDataBound` w sekcji obsługi zdarzeń, usunąć ten element ObjectDataSource i powtarzanego s `DataSourceID` właściwość również ustawienie; unwire Elementu powtarzanego s `ItemDataBound` zdarzeń z obsługi zdarzeń, usuwając `Handles Categories.OnItemDataBound` składni w klasie związanej z kodem ASP.NET.

Z elementu powtarzanego w oryginalnego stanu, Dodaj nowy element ObjectDataSource o nazwie `CategoriesDataSource` za pomocą tagów inteligentnych s elementu powtarzanego. Element ObjectDataSource umożliwia konfigurowanie `CategoriesBLL` klasy, zamiast go używać, ale `GetCategories()` metody ma on używać `GetCategoriesAndNumberOfProducts()` zamiast tego (patrz rysunek 11).


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Rysunek 11**: Konfigurowanie ObjectDataSource użyć `GetCategoriesAndNumberOfProducts` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))


Następnie zaktualizuj `ItemTemplate` , aby LinkButton s `Text` właściwość deklaratywnie przypisano przy użyciu składni wiązania z danymi i obejmuje zarówno `CategoryName` i `NumberOfProducts` pola danych. Zakończenie znaczników deklaratywne dla elementu powtarzanego i `CategoriesDataSource` wykonaj ObjectDataSource:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

Dane wyjściowe renderowane, aktualizując DAL w celu uwzględnienia `NumberOfProducts` kolumny jest taka sama jak przy użyciu `ItemDataBound` metody obsługi zdarzeń (odwołują się do 4 rysunek, aby wyświetlić ekran zrzut powtarzanego wyświetlane nazwy kategorii i liczba produktów).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Krok 3: Wyświetlanie wybranej kategorii produktów s

W tym momencie mamy `Categories` wyświetlanie listy kategorii wraz z liczbą produktów w każdej kategorii elementu powtarzanego. Powtarzanego używa element LinkButton dla każdej kategorii, że kliknięcie powoduje odświeżenie strony, w którym punktu możemy potrzebne do wyświetlenia tych produktów dla wybranej kategorii w `CategoryProducts` DataList.

Jednym z wyzwań ukierunkowane nam jest sposobu ustawiania DataList wyświetlanie tylko tych produktów dla wybranej kategorii. W [wzorca/Detail, przy użyciu wybieranych GridView wzorca z DetailsView szczegóły](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) może zostać wybrany samouczek widzieliśmy jak zbudować Element GridView której wiersze, z wybranego wiersza s Szczegóły są wyświetlane w widoku DetailsView na tej samej stronie. GridView s ObjectDataSource zwrócone informacje o wszystkich produktów za pomocą `ProductsBLL` s `GetProducts()` metoda podczas s widoku DetailsView ObjectDataSource pobrać informacji o używaniu produktu `GetProductsByProductID(productID)` metody. *`productID`* Deklaratywnie podano wartość parametru przez skojarzenie jej z wartością GridView s `SelectedValue` właściwości. Niestety, nie ma powtarzanego `SelectedValue` właściwości i nie może służyć jako źródło parametru.

> [!NOTE]
> Jest to jeden z tych problemów, które jest wyświetlane podczas korzystania z element LinkButton w elementu powtarzanego. Gdyby użyliśmy hiperłącza do przekazania w `CategoryID` za pośrednictwem zmiennej querystring firma Microsoft może Użyj tego pola QueryString jako źródło dla wartości parametru s.


Zanim firma Microsoft martwić się o braku `SelectedValue` jednak let s najpierw powiązać elementu DataList ObjectDataSource i określić właściwości dla elementu powtarzanego, jego `ItemTemplate`.

Z tagów inteligentnych DataList s, należy wybrać opcję Dodaj nowy element ObjectDataSource o nazwie `CategoryProductsDataSource` i skonfigurować go do używania `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` metody. Ponieważ DataList w tym samouczku udostępnia interfejs tylko do odczytu, możesz także list rozwijanych w INSERT, UPDATE i usuwanie kart na (Brak).


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetProductsByCategoryID(categoryID) s ProductsBLL — klasa](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Rysunek 12**: Konfigurowanie ObjectDataSource użyć `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))


Ponieważ `GetProductsByCategoryID(categoryID)` metoda oczekuje parametru wejściowego (*`categoryID`*), Kreator konfigurowania źródła danych pozwala określić źródło s parametru. Ma kategorie zostały wymienione w widoku GridView lub DataList, możemy d ustawiono listy rozwijanej parametru źródło kontroli i ControlID do `ID` danych formantu sieci Web. Jednak ponieważ brakuje elementu powtarzanego `SelectedValue` właściwości nie może być używany jako źródło parametru. Jeśli wybierzesz, można znaleźć że na liście rozwijanej ControlID zawiera tylko jeden formant `ID``CategoryProducts`, `ID` z elementu DataList.

Teraz Ustaw listy rozwijanej źródła parametru None. Firma Microsoft będzie przechodzili programowo przypisywanie wartości tego parametru, gdy kategorii, które LinkButton zostanie kliknięty w elemencie powtarzanym.


[![Czy określono parametr źródło categoryID parametru](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Rysunek 13**: nie określaj parametru źródło *`categoryID`* parametr ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))


Po zakończeniu pracy Kreatora konfigurowania źródła danych programu Visual Studio automatycznie generuje DataList s `ItemTemplate`. Zastąp domyślną `ItemTemplate` z szablonem możemy używany w poprzednim samouczek; ustawisz DataList s `RepeatColumns` właściwości do 2. Po wprowadzeniu tych zmian deklaratywne znaczników dla listy DataList i jego skojarzony element ObjectDataSource powinna wyglądać następująco:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

Obecnie `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* parametru nigdy nie ustawiono, więc produkty nie są wyświetlane podczas wyświetlania strony. Co należy zrobić jest ustawiony na podstawie wartość parametru `CategoryID` klikniętej kategorii w elemencie powtarzanym. Powstaje wyzwanie z dwóch powodów: najpierw, jak możemy określania, kiedy LinkButton w elemencie powtarzanym s `ItemTemplate` został kliknięty; oraz drugiego, jak możemy ustalić `CategoryID` odpowiedniej kategorii, których LinkButton został kliknięty?

Element LinkButton tak, jak przycisk i ImageButton formanty ma `Click` zdarzeń i [ `Command` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). `Click` Zdarzeń zaprojektowano w celu po prostu należy pamiętać, że element LinkButton został kliknięty. Czasami jednak oprócz zauważyć, że został kliknięty element LinkButton również musimy przekazują do obsługi zdarzeń niektóre dodatkowe informacje. Jeśli jest to LinkButton s [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) i [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) właściwości można przypisać te dodatkowe informacje. Następnie, gdy kliknięto element LinkButton jego `Command` generowane zdarzenie (zamiast jego `Click` zdarzeń) i program obsługi zdarzeń jest przekazywany wartości `CommandName` i `CommandArgument` właściwości.

Gdy `Command` zdarzenie jest wywoływane z szablonu w elemencie powtarzanym s elementu powtarzanego [ `ItemCommand` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) wyzwala i jest przekazywany `CommandName` i `CommandArgument` wartości klikniętej LinkButton (lub przycisku lub ImageButton). W związku z tym aby określić, kiedy zostanie kliknięta kategorii LinkButton w elemencie powtarzanym, musimy wykonać następujące czynności:

1. Ustaw `CommandName` właściwości LinkButton w elemencie powtarzanym s `ItemTemplate` niektóre wartości (I był używany ListProducts). To ustawienie `CommandName` wartość LinkButton s `Command` zdarzenia generowane, gdy kliknięto element LinkButton.
2. Ustaw LinkButton s `CommandArgument` na wartość bieżącego elementu s `CategoryID`.
3. Utwórz program obsługi zdarzeń dla elementu powtarzanego s `ItemCommand` zdarzeń. W przypadku obsługi zestawie zdarzeń `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametru w wartości przekazane do `CommandArgument`.

Następujące `ItemTemplate` kod znaczników dla elementu powtarzanego kategorii implementuje kroki 1 i 2. Uwaga jak `CommandArgument` jest przypisana wartość elementu danych s `CategoryID` przy użyciu składni wiązania z danymi:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Zawsze, gdy tworzenie `ItemCommand` rozsądne Sprawdź zawsze najpierw przychodzącego jest program obsługi zdarzeń `CommandName` wartości, ponieważ *żadnych* `Command` zdarzenie zgłaszane przez *żadnych* przycisku, LinkButton, lub ImageButton w elemencie powtarzanym spowoduje, że `ItemCommand` zdarzenia. Chociaż firma Microsoft obecnie tylko jednego takiego LinkButton teraz, w przyszłości firma Microsoft (lub innego dewelopera na nasz zespół) może dodać dodatkowy przycisk formantów sieci Web do powtarzanego, po kliknięciu zgłasza takie same `ItemCommand` obsługi zdarzeń. W związku z tym go s najlepiej zawsze upewnij się, że sprawdzanie `CommandName` właściwości i tylko Kontynuuj Twojej logiki pasujący do oczekiwaną wartość.

Po upewnieniu się, że przekazany do `CommandName` wartość jest równa ListProducts, następnie przypisuje programu obsługi zdarzeń `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parametru w wartości przekazane do `CommandArgument`. Ta modyfikacja s ObjectDataSource `SelectParameters` automatycznie powoduje, że DataList ponownie powiązać się ze źródłem danych, przedstawiający produktów dla nowo wybranej kategorii.


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

Z tych dodatków z naszym samouczkiem jest zakończona Poświęć chwilę, aby przetestować go w przeglądarce. Rysunek 14 pokazuje ekranu podczas najpierw odwiedzania strony. Ponieważ kategorię musi jeszcze zostać wybrany, produkty nie są wyświetlane. Kliknięcie kategorii, takich jak produktu, wyświetla te produkty w danej kategorii produktów w widoku dwie kolumny (patrz rysunek 15).


[![Brak produkty nie są wyświetlane podczas pierwszego odwiedzając stronę](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Rysunek 14**: produkty nie są wyświetlane po pierwszym odwiedzania strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))


[![Kliknięcie przycisku listy kategorii produktu pasujących produktów w prawo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Rysunek 15**: kliknięcie Kategoria produktu zawiera listę produktów dopasowania w prawo ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))


## <a name="summary"></a>Podsumowanie

Jak widzieliśmy w tym samouczku i poprzednim wzorzec/szczegół raportów mogą być rozszerzane na dwie strony lub skonsolidowane w jeden. Wyświetlanie raportu głównych/szczegółów na jednej stronie, jednak wprowadza wyzwania dotyczące najlepiej układu wzorca i rejestruje szczegółowe informacje na stronie. W *wzorca/Detail, przy użyciu wybieranych GridView wzorca z DetailsView szczegóły* samouczka było rekordów szczegółów pojawiają się nad rekordy główne; w tym samouczku użyliśmy techniki CSS mają float wzorca rekordów do po lewej szczegóły.

Wraz z wyświetlania raportów głównych/szczegółów, również było możliwość Eksploruj sposobu pobierania liczby produktów skojarzonych z każdej kategorii, jak również, jak przeprowadzać logiki po stronie serwera, gdy LinkButton (lub przycisku lub ImageButton) zostanie kliknięty z poziomu Elementu powtarzanego.

W tym samouczku wykonuje naszych badania raportów wzorzec/szczegół z DataList i elementu powtarzanego. Nasze dalej zbiór samouczki przedstawiają sposób dodawania, edytowania i usuwania funkcji formant DataList.

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) samouczek dotyczący przestawne elementy CSS z CSS
- [Pozycjonowanie CSS](http://www.brainjar.com/css/positioning/) więcej informacji na temat rozmieszczania elementów za pomocą stylów CSS
- [Tworzenie limit zawartości HTML](http://www.w3schools.com/html/html_layout.asp) przy użyciu `<table>` s i inne elementy HTML do rozmieszczania

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Nowak Zack. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-filtering-acess-two-pages-datalist-vb.md)
