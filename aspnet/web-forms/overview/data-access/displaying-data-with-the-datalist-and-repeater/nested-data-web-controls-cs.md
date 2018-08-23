---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
title: Zagnieżdżone dane sieci Web kontrolki (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku, który przeanalizujemy sposób używania Repeater zagnieżdżone wewnątrz innego elementu powtarzanego. Przykłady przedstawiają sposobu wypełniania wewnętrzny elementu powtarzanego zarówno d...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: ad3cb0ec-26cf-42d7-b81b-184a34ec9f86
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 032321b5cf5323058c114e652512854f9866d447
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753738"
---
<a name="nested-data-web-controls-c"></a>Zagnieżdżone kontrolki internetowe danych (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_CS.exe) lub [Pobierz plik PDF](nested-data-web-controls-cs/_static/datatutorial32cs1.pdf)

> W tym samouczku, który przeanalizujemy sposób używania Repeater zagnieżdżone wewnątrz innego elementu powtarzanego. Przykłady przedstawiają sposobu wypełniania wewnętrzny elementu powtarzanego w sposób deklaratywny i programowy.


## <a name="introduction"></a>Wprowadzenie

Oprócz statyczny kod HTML i składnia wiązania danych szablony mogą również obejmować kontrolki sieci Web oraz użytkownika. Te kontrolki sieci Web może mieć właściwości, ich przypisany za pomocą składni deklaratywnej, powiązań danych, lub można uzyskać programistycznie w procedurach obsługi odpowiedniego zdarzenia po stronie serwera.

Osadzając kontrolki w szablonie, można dostosować i ulepszania wyglądu i interfejsu użytkownika. Na przykład w [za pomocą kontrolek TemplateField w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) samouczka będziemy pokazaliśmy, jak dostosować wyświetlanie s GridView, dodając kontrolkę kalendarza w TemplateField, aby pokazać pracownika Data zatrudnienia s; w [Dodawanie Kontrolek weryfikacji do edycji i wstawiania interfejsy](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) i [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) samouczki, widzieliśmy sposobu dostosowywania, edycji i wstawianie interfejsy przez dodawanie sprawdzania poprawności formanty, pola tekstowe, kontrolek DROPDOWNLIST i inne kontrolki sieci Web.

Szablony mogą również zawierać inne dane kontrolki sieci Web. Oznacza to, że firma Microsoft może mieć DataList, zawierający inną DataList (lub elementu powtarzanego lub GridView lub DetailsView i tak dalej) w ramach jego szablonów. Żądania z takiego interfejsu jest powiązanie odpowiednie dane do wewnętrznego danych formantu sieci Web. Brak dostępnych kilka różnych metod, począwszy od opcji deklaratywnych za pomocą kontrolki ObjectDataSource tymi programowe.

W tym samouczku, który przeanalizujemy sposób używania Repeater zagnieżdżone wewnątrz innego elementu powtarzanego. Zewnętrzne elementu powtarzanego będzie zawierać element dla każdej kategorii w bazie danych, wyświetlanie kategorii s nazwę i opis. Każdy element kategorii s wewnętrzny elementu powtarzanego wyświetlane są informacje dotyczące poszczególnych produktów należących do tej kategorii (patrz rysunek 1) na liście punktowanej. Nasze przykłady przedstawiają sposobu wypełniania wewnętrzny elementu powtarzanego w sposób deklaratywny i programowy.


[![Każdej kategorii, wraz z jej produktów są wyświetlane.](nested-data-web-controls-cs/_static/image2.png)](nested-data-web-controls-cs/_static/image1.png)

**Rysunek 1**: należą każdej kategorii, wraz z jej produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-data-web-controls-cs/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>Krok 1: Tworzenie listy kategorii

Podczas tworzenia stron, która używa zagnieżdżone kontrolki internetowe danych, I przydatne do projektowania, tworzenie i testowanie formantu sieci Web peryferyjnych danych, najpierw bez nawet martwienia się o wewnętrzną kontrolkę zagnieżdżonych. W związku z tym Niech s początek Instruktaż kroki niezbędne do dodania elementu powtarzanego do strony, która wyświetla nazwę i opis dla każdej kategorii.

Zacznij od otwarcia `NestedControls.aspx` strony w `DataListRepeaterBasics` folderu i Dodaj kontrolką elementu powtarzanego do strony, ustawiając jego `ID` właściwość `CategoryList`. Za pomocą tagu inteligentnego Repeater s zdecydować się na utworzenie nowego elementu ObjectDataSource, o nazwie `CategoriesDataSource`.


[![Nazwa nowego ObjectDataSource CategoriesDataSource](nested-data-web-controls-cs/_static/image5.png)](nested-data-web-controls-cs/_static/image4.png)

**Rysunek 2**: nadaj nazwę nowej kontrolki ObjectDataSource `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-data-web-controls-cs/_static/image6.png))


Skonfigurować kontrolki ObjectDataSource ściągania danych z `CategoriesBLL` klasy s `GetCategories` metody.


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetCategories CategoriesBLL klasy s](nested-data-web-controls-cs/_static/image8.png)](nested-data-web-controls-cs/_static/image7.png)

**Rysunek 3**: Konfigurowanie kontrolki ObjectDataSource do użycia `CategoriesBLL` klasy s `GetCategories` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-data-web-controls-cs/_static/image9.png))


Aby określić szablon elementu powtarzanego s zawartości należy przejść do widoku źródła i ręcznie wprowadzić składni deklaratywnej. Dodaj `ItemTemplate` wyświetlającą nazwę kategorii s w `<h4>` elementu i opis kategorii s w element akapitu (`<p>`). Ponadto umożliwiają s oddziel poszczególnych kategorii linii poziomej (`<hr>`). Po wprowadzeniu tych zmian strony powinna zawierać składni deklaratywnej dla elementu powtarzanego i kontrolki ObjectDataSource, który jest podobny do następującego:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample1.aspx)]

Rysunek 4 pokazuje nasz postęp, podczas wyświetlania za pośrednictwem przeglądarki.


[![Każda kategoria s Nazwa i opis ma na liście, oddzielone linia pozioma](nested-data-web-controls-cs/_static/image11.png)](nested-data-web-controls-cs/_static/image10.png)

**Rysunek 4**: s każdej kategorii, nazwa i opis ma na liście, oddzielone linii poziomej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-data-web-controls-cs/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>Krok 2: Dodawanie elementu powtarzanego zagnieżdżonych produktu

Z kategorią listę pełną, naszym kolejnym krokiem jest dodanie Repeater do `CategoryList` s `ItemTemplate` , wyświetla informacje o tych produktów należących do odpowiedniej kategorii. Istnieją różne sposoby, można pobierać dane dla tego wewnętrzny elementu powtarzanego, dwie z nich przedstawimy wkrótce. Na razie umożliwiają s po prostu Utwórz produktów Repeater w ramach `CategoryList` Repeater s `ItemTemplate`. W szczególności umożliwiają s ma wyświetlania elementu powtarzanego każdego produktu na liście punktowanej dla każdego elementu listy łącznie z nazwą produktu s i cena produktu.

Do utworzenia tego elementu powtarzanego będziemy musieli ręcznie wprowadzić wewnętrzny elementu powtarzanego s składni deklaratywnej i szablony do `CategoryList` s `ItemTemplate`. Dodaj następujący kod w ramach `CategoryList` Repeater s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Krok 3: Powiązywanie powtarzanego ProductsByCategoryList specyficznego dla kategorii produktów

W przypadku odwiedzenia strony za pomocą przeglądarki na tym etapie, ekran będzie wyglądać tak samo, jak rysunek 4 ponieważ możemy ve jeszcze żadnych danych można powiązać elementu powtarzanego. Istnieje kilka sposobów, firma Microsoft Pobierz rekordy odpowiedniego produktu i wiązania ich z elementu powtarzanego, niektóre bardziej wydajne niż inne. Głównym wyzwaniem jest powrót odpowiednich produktów dla określonej kategorii.

Dane można powiązać z wewnętrznego kontrolce elementu powtarzanego albo są dostępne w sposób deklaratywny, za pomocą kontrolki ObjectDataSource w `CategoryList` Repeater s `ItemTemplate`, lub programowo, ze strony związanym z kodem strony ASP.NET. Podobnie, te dane mogą być powiązane z wewnętrzny elementu powtarzanego albo w sposób deklaratywny — za pośrednictwem wewnętrznego elementu powtarzanego s `DataSourceID` właściwości lub za pomocą składni deklaratywne wiązania danych, albo programowo, odwołując się do wewnętrznego elementu powtarzanego w `CategoryList` Repeater s `ItemDataBound` programu obsługi zdarzeń, programowe ustawianie jego `DataSource` właściwości i wywoływania jego `DataBind()` metody. Pozwól, s, zapoznaj się z każdego z tych sposobów.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Uzyskiwanie dostępu do danych w sposób deklaratywny za pomocą kontrolki ObjectDataSource i`ItemDataBound`programu obsługi zdarzeń

Ponieważ firma Microsoft był używany ObjectDataSource często w całej tej serii samouczków najbardziej naturalnym wyborem do uzyskiwania dostępu do danych, w tym przykładzie jest się za pomocą kontrolki ObjectDataSource. `ProductsBLL` Klasa ma `GetProductsByCategoryID(categoryID)` metodę, która zwraca informacje na temat tych produktów, które należą do określonego *`categoryID`*. W związku z tym, można dodać kontrolki ObjectDataSource do `CategoryList` Repeater s `ItemTemplate` i skonfiguruj ją, aby uzyskać dostęp do swoich danych z tej metody klasy s.

Niestety Repeater t zezwala na jego szablonów, które mają być edytowana za pomocą widoku projektu, więc musimy dodać składni deklaratywnej dla tego formantu ObjectDataSource ręcznie. Poniższej składni `CategoryList` Repeater s `ItemTemplate` po dodaniu tej nowej kontrolki ObjectDataSource (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample3.aspx)]

Korzystając z podejścia ObjectDataSource musimy `ProductsByCategoryList` Repeater s `DataSourceID` właściwości `ID` elementu ObjectDataSource (`ProductsByCategoryDataSource`). Ponadto te Zauważ, że nasze ObjectDataSource ma `<asp:Parameter>` element, który określa *`categoryID`* wartości, które zostaną przekazane do `GetProductsByCategoryID(categoryID)` metody. Ale jak możemy określić tę wartość? W idealnym przypadku d będziemy mogli ustawiliśmy `DefaultValue` właściwość `<asp:Parameter>` elementu przy użyciu składni wiązania danych w następujący sposób:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample4.aspx)]

Niestety, składnia wiązania danych jest prawidłowy tylko w kontrolkach, które mają `DataBinding` zdarzeń. `Parameter` Klasa zamknięta nie ma takiego zdarzenia, a w związku z tym powyższej składni jest niedozwolony spowoduje błąd w czasie wykonywania.

Aby ustawić tę wartość, należy utworzyć program obsługi zdarzeń dla `CategoryList` Repeater s `ItemDataBound` zdarzeń. Pamiętamy `ItemDataBound` zdarzeń jest uruchamiana raz dla każdego elementu powtarzanego powiązana. W związku z tym, każdym razem, to zdarzenie jest generowane dla zewnętrznego elementu powtarzanego można przypisywać bieżącego `CategoryID` wartość `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` parametru.

Utwórz procedurę obsługi zdarzeń dla `CategoryList` Repeater s `ItemDataBound` zdarzeń z następującym kodem:


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample5.cs)]

Ta procedura obsługi zdarzeń uruchamia, zapewniając, że możemy ponownie radzenia sobie z danymi elementu zamiast elementu nagłówek, stopka lub separatora. Następnie odwołujemy się rzeczywiste `CategoriesRow` wystąpienia, która właśnie została powiązana z bieżącego `RepeaterItem`. Na koniec mamy odwoływać się do elementu ObjectDataSource w `ItemTemplate` i przypisać jej `CategoryID` wartości parametru `CategoryID` bieżącego `RepeaterItem`.

Z tej obsługi zdarzeń `ProductsByCategoryList` Repeater w każdym `RepeaterItem` jest powiązany z tych produktów w `RepeaterItem` kategorii s. Rysunek 5. pokazuje zrzut ekranu: dane wyjściowe.


[![Zewnętrzne powtarzanego Wyświetla każdej kategorii; Jeden wewnętrzny zawiera listę produktów dla tej kategorii](nested-data-web-controls-cs/_static/image14.png)](nested-data-web-controls-cs/_static/image13.png)

**Rysunek 5**: Repeater Wyświetla każdej kategorii zewnętrznej; Wyświetla jeden wewnętrzny produktów dla tej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-data-web-controls-cs/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Programowe uzyskiwanie dostępu do produktów według kategorii danych

Zamiast pobierać produkty dla bieżącej kategorii za pomocą kontrolki ObjectDataSource, możemy utworzyć metodę w naszej platformy ASP.NET strony s osobna klasa kodu (lub `App_Code` folderu lub w osobnym projekcie Biblioteka klas), zwraca odpowiedni zestaw produkty, gdy dane są przekazywane w `CategoryID`. Wyobraź sobie, firma Microsoft ma taką metodę klasy związane z kodem strony s naszej platformy ASP.NET i że nosiła nazwę `GetProductsInCategory(categoryID)`. Przy użyciu tej metody w miejscu firma Microsoft może powiązać produktów dla bieżącej kategorii wewnętrzny elementu powtarzanego przy użyciu poniższej składni deklaratywnej:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample6.aspx)]

Elementu powtarzanego s `DataSource` właściwość używa składni wiązania danych, aby wskazać, czy jego dane pochodzą od `GetProductsInCategory(categoryID)` metody. Ponieważ `Eval("CategoryID")` zwraca wartość typu `Object`, firma Microsoft rzutować obiekt do `Integer` przed przekazaniem go do `GetProductsInCategory(categoryID)` metody. Należy pamiętać, że `CategoryID` używanych w tym miejscu za pomocą wiązania danych składnia jest `CategoryID` w *zewnętrzne* elementu powtarzanego (`CategoryList`), co ten s powiązany z rekordów w `Categories` tabeli. W związku z tym, wiemy, że `CategoryID` nie może być bazą `NULL` wartości, dlatego firma Microsoft może skutkować nieświadomym rzutowany `Eval` metody bez sprawdzania, czy możemy ponownie zajmowanie się `DBNull`.

W przypadku tej metody, musimy utworzyć `GetProductsInCategory(categoryID)` metody i pobrać odpowiedni zestaw produktów, biorąc pod uwagę na podanie *`categoryID`*. Możemy to zrobić, po prostu zwracanie `ProductsDataTable` zwrócone przez `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` metody. Umożliwiają tworzenie s `GetProductsInCategory(categoryID)` metody w klasie CodeBehind dla naszych `NestedControls.aspx` strony. To zrobić przy użyciu następującego kodu:


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample7.cs)]

Ta metoda po prostu tworzy wystąpienie `ProductsBLL` metodę i zwraca wyniki `GetProductsByCategoryID(categoryID)` metody. Należy pamiętać, że metoda musi być oznaczona `Public` lub `Protected`; Jeśli metoda jest oznaczona jako `Private`, nie będą dostępne w oznaczeniu deklaracyjnym strony ASP.NET.

Po wprowadzeniu tych zmian, aby użyć tej nowej metody, Poświęć chwilę, aby wyświetlić stronę za pośrednictwem przeglądarki. Dane wyjściowe powinny być identyczne dane wyjściowe, gdy za pomocą kontrolki ObjectDataSource i `ItemDataBound` metody obsługi zdarzeń (odnoszą się do rysunku 5, aby wyświetlić zrzut ekranu).

> [!NOTE]
> Może się wydawać pracy, aby utworzyć `GetProductsInCategory(categoryID)` metody w klasie CodeBehind strony ASP.NET. Ta metoda po prostu tworzy wystąpienie `ProductsBLL` klasy i zwraca wyniki jego `GetProductsByCategoryID(categoryID)` metody. Dlaczego nie po prostu wywołać tę metodę bezpośrednio z składnia wiązania z danymi w elemencie powtarzanym wewnętrzne, takie jak: `DataSource='<%# ProductsBLL.GetProductsByCategoryID((int)(Eval("CategoryID"))) %>'`? Chociaż ta składnia wygrał t współpracują z naszych bieżąca implementacja parametru `ProductsBLL` klasy (ponieważ `GetProductsByCategoryID(categoryID)` metoda jest metodą wystąpienia), można zmodyfikować `ProductsBLL` uwzględnianie statycznego `GetProductsByCategoryID(categoryID)` metody lub Klasa statyczna `Instance()` metodę, aby zwrócić nowe wystąpienie klasy `ProductsBLL` klasy.


Podczas modyfikacji wyeliminować potrzebę `GetProductsInCategory(categoryID)` metody w klasie CodeBehind strony ASP.NET, metody klasy CodeBehind daje większą elastyczność w stosowaniu dane pobrane, jak zajmiemy się wkrótce.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Pobieranie wszystkich informacji o produkcie jednocześnie

Dwie techniki poprzedniej możemy ve zbadane pobrania tych produktów dla bieżącej kategorii poprzez wywołanie `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` — metoda (pierwszego podejścia dokonał tego za pośrednictwem ObjectDataSource drugiego do `GetProductsInCategory(categoryID)` method in Class metoda osobna klasa kodu). Każdorazowo ta metoda jest wywoływana, wywołuje warstwę logiki biznesowej do warstwy dostępu do danych, który wysyła zapytanie bazy danych za pomocą instrukcji SQL, która zwraca wiersze z `Products` tabeli, którego `CategoryID` podany parametr wejściowy pasuje do pola.

Biorąc pod uwagę *N* kategorie w systemie, to podejście sieci *N* + 1 wywołania zapytanie jednej bazy danych dla bazy danych do wszystkich kategorii a następnie *N* wywołania można pobrać produktów specyficzne dla każdej kategorii. Firma Microsoft może jednak pobrać wszystkie wymagane dane w jednym wywołaniu wywołania tylko dwie bazy danych do wszystkich kategorii, a drugi do wszystkich produktów. Gdy będziemy już mieć wszystkie produkty, możemy filtrować te produkty więc tylko produkty dopasowania bieżącego `CategoryID` są powiązane z tej kategorii s wewnętrzny elementu powtarzanego.

Do tej funkcji, tylko musimy upewnić niewielkich modyfikacji do `GetProductsInCategory(categoryID)` metody w naszej platformy ASP.NET strony s osobna klasa kodu. Zamiast bezrefleksyjne zwracania wyników z `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` metody, możemy zamiast tego pierwszym uzyskaniu dostępu do *wszystkich* produktów (jeśli one, że t został już dostępne), a następnie powrócić po prostu widok filtrowany produkty oparte na przekazanym `CategoryID`.


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample8.cs)]

Należy pamiętać, dodanie zmiennej na poziomie strony `allProducts`. To są przechowywane informacje dotyczące wszystkich produktów i jest wypełniana po raz pierwszy `GetProductsInCategory(categoryID)` metoda jest wywoływana. Po upewnieniu się, że `allProducts` obiekt został utworzony i wypełniony, metoda filtruje wyniki s DataTable w taki sposób, że tylko te wiersze, w których `CategoryID` jest zgodny z określonym `CategoryID` są dostępne. Takie podejście zmniejsza liczbę razy, baza danych jest dostępny z *N* + 1 do dwóch.

To ulepszenie nie wprowadza żadnych zmian renderowanego kodu znaczników strony ani nie powoduje zwracał mniej rekordów niż inne podejście. Po prostu zmniejsza liczbę wezwań do bazy danych.

> [!NOTE]
> Jeden intuicyjnie przyczyny, że zmniejszenie częstotliwości dostępu do bazy danych będzie assuredly zwiększyć wydajność. Jednak może to nie być wymagane. Jeśli masz dużą liczbę produktów którego `CategoryID` jest `NULL`, aby uzyskać przykład, a następnie wywołania `GetProducts` metoda zwraca liczbę produktów, które nigdy nie są wyświetlane. Ponadto, zwracając wszystkie produkty może być marnotrawstwa Jeśli użytkownik re pokazywane są tylko podzbiór kategorie, które może być w przypadku, jeśli udało Ci się wdrożyć stronicowanie.


Jak zawsze, gdy chodzi o analizowania wydajności dwie techniki, tylko surefire miary jest kontrolowane testy dostosowane do Twojej aplikacji s typowych scenariuszy przypadków.

## <a name="summary"></a>Podsumowanie

W tym samouczku widzieliśmy jak zagnieździć danymi formantu sieci Web w innym ciągu, specjalnie badanie sposobu ustawiania zewnętrznego elementu powtarzanego wyświetlania elementu dla każdej kategorii za pomocą wewnętrznego elementu powtarzanego zawierającą listę produktów dla każdej kategorii, na liście punktowanej. Głównym wyzwaniem w tworzeniu interfejsu użytkownika zagnieżdżonych zaletą dostęp i powiązanie poprawne dane z formantu sieci Web wewnętrzny danych. Istnieją różne techniki dostępne dwie z nich zbadaliśmy w ramach tego samouczka. Pierwszą metodę badania używane ObjectDataSource dane zewnętrzne formantu sieci Web s `ItemTemplate` która została powiązana z wewnętrznego danych kontrolki sieci Web za pośrednictwem jego `DataSourceID` właściwości. Druga metoda dostęp do danych za pomocą metody w klasie CodeBehind s strony ASP.NET. Ta metoda może być powiązana następnie wewnętrzny danych formantu sieci Web s `DataSource` właściwości za pomocą składni wiązania danych.

Interfejs użytkownika zagnieżdżonych, w tym samouczku używane Repeater zagnieżdżony w elemencie powtarzanym, techniki te mogą obejmować inne kontrolki internetowe danych. Można zagnieżdżać Repeater w obrębie GridView lub GridView w kontrolkach DataList i tak dalej.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Zack Jones i Liz Shulok. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
> [dalej](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
