---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
title: Zagnieżdżone danych sieci Web formantów (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku przeanalizujemy sposób użycia elementu powtarzanego zagnieżdżone wewnątrz innego elementu powtarzanego. Przykłady przedstawiają jak wypełnić wewnętrzny elementu powtarzanego zarówno d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 8b7fcf7b-722b-498d-a4e4-7c93701e0c95
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: d8bb5eae2003273fa8d8a06cc4adaa959378f1e2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882965"
---
<a name="nested-data-web-controls-vb"></a>Zagnieżdżone dane formantów sieci Web (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_VB.exe) lub [pobierania plików PDF](nested-data-web-controls-vb/_static/datatutorial32vb1.pdf)

> W tym samouczku przeanalizujemy sposób użycia elementu powtarzanego zagnieżdżone wewnątrz innego elementu powtarzanego. Przykłady przedstawiają sposób wypełnienia wewnętrzny elementu powtarzanego deklaratywnie i programowo.


## <a name="introduction"></a>Wprowadzenie

Oprócz Statycznych i składnia wiązania z danymi szablony mogą również obejmować formanty sieci Web i użytkownika. Te kontrolki sieci Web może mieć właściwości przypisane za pomocą składni deklaratywnej, wiązania z danymi, lub można uzyskać programistycznie w obsłudze odpowiednie zdarzenia po stronie serwera.

Osadzanie formantów w ramach szablonu, można dostosować i ulepszania wygląd i środowisko użytkownika. Na przykład w [przy użyciu TemplateFields w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) samouczka widzieliśmy sposób dostosowywania widoku GridView wyświetlania s dodając kontrolkę kalendarza w TemplateField pokazanie pracownika Data zatrudnienia s; w [Dodawanie Formanty walidacji do edycji i wstawianie interfejsy](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) i [Dostosowywanie interfejs modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) samouczki, widzieliśmy sposobu dostosowywania edycji i wstawianie interfejsy przez dodawanie walidacji Formanty pól tekstowych, DropDownLists i innych formantów sieci Web.

Szablony może także zawierać dane, inne formanty sieci Web. Oznacza to, że firma Microsoft może mieć DataList, zawierający innego DataList (lub elementu powtarzanego lub widoku GridView lub widoku DetailsView i tak dalej) w swoich szablonach. Wyzwanie z takiego interfejsu jest powiązanie odpowiednie dane wewnętrzne danych formantu sieci Web. Brak dostępnych kilka różnych metod, począwszy od deklaratywne opcje za pomocą elementu ObjectDataSource do programowego protokołów.

W tym samouczku przeanalizujemy sposób użycia elementu powtarzanego zagnieżdżone wewnątrz innego elementu powtarzanego. Zewnętrzne powtarzanego zawiera element dla każdej kategorii w bazie danych, wyświetlanie kategorii s nazwę i opis. Każdy element kategorii s wewnętrzny elementu powtarzanego wyświetlane są informacje dotyczące każdego produktu należące do tej kategorii (zobacz rysunek 1) na liście punktowanej. Nasze przykłady przedstawiają sposób wypełnienia wewnętrzny elementu powtarzanego deklaratywnie i programowo.


[![Każdej kategorii, wraz z jego produktów, są wyświetlane.](nested-data-web-controls-vb/_static/image2.png)](nested-data-web-controls-vb/_static/image1.png)

**Rysunek 1**: każdej kategorii, wraz z jego produktów, są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-data-web-controls-vb/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>Krok 1: Tworzenie listy kategorii

Podczas kompilowania strony, która używa zagnieżdżone formantów sieci Web danych, I przyda projektu, Utwórz i przetestowanie peryferyjnych danych formantu sieci Web, nie martwiąc się nawet o wewnętrzny zagnieżdżony formant. W związku z tym umożliwiają s, Rozpocznij od przejście przez kroki niezbędne do dodania elementu powtarzanego do strony, która zawiera nazwę i opis dla każdej kategorii.

Uruchamianie przez otwarcie `NestedControls.aspx` w obszarze `DataListRepeaterBasics` folderu i dodać kontrolce elementu powtarzanego do strony, ustawienie jej `ID` właściwości `CategoryList`. Z tagów inteligentnych elementu powtarzanego s wybrać do utworzenia nowego elementu ObjectDataSource o nazwie `CategoriesDataSource`.


[![Nazwa nowej ObjectDataSource CategoriesDataSource](nested-data-web-controls-vb/_static/image5.png)](nested-data-web-controls-vb/_static/image4.png)

**Rysunek 2**: Nazwa nowego elementu ObjectDataSource `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-data-web-controls-vb/_static/image6.png))


Skonfiguruj ObjectDataSource, dzięki czemu ściągania danych z `CategoriesBLL` klasy s `GetCategories` metody.


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetCategories klasy CategoriesBLL s](nested-data-web-controls-vb/_static/image8.png)](nested-data-web-controls-vb/_static/image7.png)

**Rysunek 3**: Konfigurowanie ObjectDataSource użyć `CategoriesBLL` klasy s `GetCategories` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-data-web-controls-vb/_static/image9.png))


Aby określić szablon elementu powtarzanego s zawartości musimy przejdź do widoku źródłowego i ręcznie wprowadzić składni deklaratywnej. Dodaj `ItemTemplate` który wyświetla nazwę kategorii s w `<h4>` elementu i opis kategorii s w elemencie akapitu (`<p>`). Ponadto pozwalają s oddziel poszczególnych kategorii linia pozioma (`<hr>`). Po wprowadzeniu tych zmian strony powinny zawierać składni deklaratywnej dla elementu powtarzanego i ObjectDataSource, który jest podobny do następującego:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample1.aspx)]

Na rysunku 4 przedstawiono naszych postępu podczas wyświetlania za pośrednictwem przeglądarki.


[![Każda kategoria s nazwę i opis jest wyświetlana, oddzielonych linia pozioma](nested-data-web-controls-vb/_static/image11.png)](nested-data-web-controls-vb/_static/image10.png)

**Rysunek 4**: s kategorii każdą nazwę i opis jest wyświetlana, oddzielonych linia pozioma ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-data-web-controls-vb/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>Krok 2: Dodawanie elementu powtarzanego zagnieżdżonych produktu

Kategorii wyświetlania ukończenia naszego następne zadanie jest dodanie elementu powtarzanego do `CategoryList` s `ItemTemplate` którym wyświetlane są informacje o tych produktów należących do odpowiedniej kategorii. Istnieją różne sposoby, można pobierać dane dla tego wewnętrzny elementu powtarzanego, dwóch, których firma Microsoft będzie Eksploruj wkrótce. Na razie let s wystarczy utworzyć produktów elementu powtarzanego w `CategoryList` elementu powtarzanego s `ItemTemplate`. W szczególności umożliwiają s produkt każdego produktu w postaci listy punktowanej dla każdego elementu listy nazwy produktu s oraz cen wyświetlania elementu powtarzanego.

Do utworzenia tego elementu powtarzanego, należy ręcznie wprowadzić wewnętrzny elementu powtarzanego s składni deklaratywnej i szablony do `CategoryList` s `ItemTemplate`. Dodaj następujący kod w `CategoryList` elementu powtarzanego s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Krok 3: Powiązanie powtarzanego ProductsByCategoryList specyficznego dla kategorii produktów

W przypadku odwiedzenia strony za pośrednictwem przeglądarki w tym momencie ekranu wyglądają tak samo jak rysunek 4 ponieważ firma Microsoft kolejnych jeszcze powiązać żadnych danych z elementu powtarzanego. Istnieje kilka sposobów czy możemy pobrania produktu odpowiednie rekordy i powiązać powtarzanego, niektóre bardziej wydajna niż innym. W tym miejscu głównego żądania jest odzyskać odpowiednie produktów dla określonej kategorii.

Danych, aby powiązać wewnętrzny kontrolce elementu powtarzanego albo dostępne deklaratywnie, przez element ObjectDataSource w `CategoryList` elementu powtarzanego s `ItemTemplate`, lub programowo, na stronie CodeBehind strony ASP.NET. Podobnie, te dane mogą być powiązane z wewnętrzny elementu powtarzanego albo deklaratywnie — za pośrednictwem wewnętrzny elementu powtarzanego s `DataSourceID` właściwości lub za pomocą składni deklaratywnej wiązania z danymi lub programowo wewnętrzny elementu powtarzanego w `CategoryList` elementu powtarzanego s `ItemDataBound` program obsługi zdarzeń, programowane ustawianie jego `DataSource` właściwości i wywołania jego `DataBind()` metody. Let s Eksplorowanie każdego z tych sposobów.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Uzyskiwanie dostępu do danych deklaratywnie z kontrolki ObjectDataSource i`ItemDataBound`obsługi zdarzeń

Ponieważ firma Microsoft był używany element ObjectDataSource często w tej serii samouczek wybór najbardziej fizyczne do uzyskiwania dostępu do danych, w tym przykładzie zostaje przestrzegaj ObjectDataSource. `ProductsBLL` Klasa ma `GetProductsByCategoryID(categoryID)` metodę, która zwraca informacje na temat tych produktów, które należą do określonej *`categoryID`*. W związku z tym można dodać elementu ObjectDataSource do `CategoryList` elementu powtarzanego s `ItemTemplate` i skonfigurowanie dostępu do danych z tej metody klasy s.

Niestety elementu powtarzanego zezwala na jego szablonów można edytować za pomocą widoku Projekt, należy ręcznie dodać składni deklaratywnej dla tej kontrolki ObjectDataSource. Poniżej przedstawiono składnię `CategoryList` elementu powtarzanego s `ItemTemplate` po dodaniu tego nowego elementu ObjectDataSource (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample3.aspx)]

Korzystając z metody ObjectDataSource musimy ustawić `ProductsByCategoryList` elementu powtarzanego s `DataSourceID` właściwości `ID` elementu ObjectDataSource (`ProductsByCategoryDataSource`). Ponadto powiadomienia z naszych ObjectDataSource `<asp:Parameter>` element, który określa *`categoryID`* wartości, które zostaną przekazane do `GetProductsByCategoryID(categoryID)` — metoda. Ale jak możemy podać tę wartość? Najlepiej, możemy d można ustawić tylko `DefaultValue` właściwość `<asp:Parameter>` elementu za pomocą składni wiązania z danymi w następujący sposób:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample4.aspx)]

Niestety, składnia wiązania danych jest prawidłowy tylko w formantach, które mają `DataBinding` zdarzeń. `Parameter` Klasa nie ma takiego zdarzenia i w związku z tym powyższej składni jest niedozwolony i spowoduje błąd w czasie wykonywania.

Aby ustawić tę wartość, należy utworzyć programu obsługi zdarzeń dla `CategoryList` elementu powtarzanego s `ItemDataBound` zdarzeń. Odwołania, który `ItemDataBound` zdarzenia generowane raz dla każdego elementu powiązany z elementu powtarzanego. W związku z tym zawsze to zdarzenie jest generowane dla zewnętrznego elementu powtarzanego możemy można przypisać bieżącego `CategoryID` do wartości `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` parametru.

Tworzenie procedury obsługi zdarzeń dla `CategoryList` elementu powtarzanego s `ItemDataBound` zdarzeń z następującym kodem:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample5.vb)]

Ten program obsługi zdarzeń rozpoczyna się przez zapewnienie, że firma Microsoft re postępowania z danymi elementu zamiast elementu nagłówka, stopki lub separatora. Następnie możemy odwołania rzeczywiste `CategoriesRow` wystąpienia, która właśnie została powiązana z bieżącą `RepeaterItem`. Na koniec mamy odwołania ObjectDataSource w `ItemTemplate` i przypisz jej `CategoryID` wartości parametru `CategoryID` bieżącego `RepeaterItem`.

Z tej obsługi zdarzeń `ProductsByCategoryList` elementu powtarzanego w każdym `RepeaterItem` jest powiązany z tych produktów w `RepeaterItem` kategorii s. Rysunek 5. pokazuje zrzut ekranu: dane wyjściowe.


[![Zewnętrzne powtarzanego zawiera listę poszczególnych kategorii; Jeden wewnętrzny zawiera listę produktów dla tej kategorii](nested-data-web-controls-vb/_static/image14.png)](nested-data-web-controls-vb/_static/image13.png)

**Rysunek 5**: elementu powtarzanego wymieniono każdej kategorii zewnętrznej; list jeden wewnętrzny produktów dla tej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-data-web-controls-vb/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Uzyskiwanie dostępu do produktów przez dane kategorii programowo

Zamiast elementu ObjectDataSource do pobrania dla bieżącej kategorii produktów, można utworzyć metody w naszym klasie związanej z kodem strony ASP.NET (lub w `App_Code` folderu lub w oddzielnym projektu biblioteki klas) zwracająca odpowiedni zestaw produkty, gdy dane są przekazywane w `CategoryID`. Wyobraź sobie Musieliśmy taka metoda naszej klasy związane z kodem strony ASP.NET i że został on nazwę `GetProductsInCategory(categoryID)`. Przy użyciu tej metody w miejscu możemy można powiązać produktów dla bieżącej kategorii wewnętrzny elementu powtarzanego, używając następującej składni deklaratywnej:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample6.aspx)]

Powtarzanego s `DataSource` właściwości używa składni wiązania z danymi, aby wskazać, że jego dane pochodzą z `GetProductsInCategory(categoryID)` metody. Ponieważ `Eval("CategoryID")` zwraca wartość typu `Object`, możemy rzutowanie tego obiektu na `Integer` przed przekazaniem go do `GetProductsInCategory(categoryID)` metody. Należy pamiętać, że `CategoryID` używanych w tym miejscu za pośrednictwem wiązania z danymi składnia jest `CategoryID` w *zewnętrzne* elementu powtarzanego (`CategoryList`), co tego s powiązane rekordy w `Categories` tabeli. W związku z tym wiemy, że `CategoryID` nie może być bazy danych `NULL` wartości, dlatego firma Microsoft może ślepo rzutowania `Eval` metody bez sprawdzania, czy możemy re zajmowanie `DBNull`.

Z tej metody, należy utworzyć `GetProductsInCategory(categoryID)` — metoda i pobrać odpowiedni zestaw produktów, podana podane *`categoryID`*. Firma Microsoft można to zrobić, po prostu zwracanie `ProductsDataTable` zwrócony przez `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` metody. Let s utworzyć `GetProductsInCategory(categoryID)` metody w klasie CodeBehind dla naszych `NestedControls.aspx` strony. To zrobić przy użyciu następującego kodu:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample7.vb)]

Ta metoda po prostu tworzy wystąpienie `ProductsBLL` metodę i zwraca wyniki `GetProductsByCategoryID(categoryID)` metody. Należy pamiętać, że metoda muszą być oznaczone jako `Public` lub `Protected`; Jeśli metoda jest oznaczona jako `Private`, nie będzie dostępny z ASP.NET s deklaratywne znaczników.

Po wprowadzeniu tych zmian, aby użyć tej nowej metody, Poświęć chwilę, aby wyświetlić stronę za pośrednictwem przeglądarki. Dane wyjściowe powinny być identyczne dane wyjściowe, korzystając z elementu ObjectDataSource i `ItemDataBound` metoda obsługi zdarzeń (odwołują się do rysunek 5, aby zobaczyć zrzut ekranu).

> [!NOTE]
> Może się wydawać busywork, aby utworzyć `GetProductsInCategory(categoryID)` metody w klasie związanej z kodem strony ASP.NET. Po wszystkich, ta metoda po prostu tworzy wystąpienie `ProductsBLL` klasy i zwraca wyniki jego `GetProductsByCategoryID(categoryID)` metody. Dlaczego nie tylko tę metodę należy wywołać bezpośrednio z składnia wiązania z danymi w elemencie powtarzanym wewnętrzne, takie jak: `DataSource='<%# ProductsBLL.GetProductsByCategoryID(CType(Eval("CategoryID"), Integer)) %>'`? Mimo że ta składnia won działa z naszych bieżąca implementacja `ProductsBLL` klasy (ponieważ `GetProductsByCategoryID(categoryID)` metoda jest metodą wystąpienia), można zmodyfikować `ProductsBLL` uwzględnienie statycznego `GetProductsByCategoryID(categoryID)` — metoda lub klasa obejmują statycznego `Instance()` metodę, aby zwrócić nowe wystąpienie klasy `ProductsBLL` klasy.


Gdy takich modyfikacji wyeliminować potrzebę `GetProductsInCategory(categoryID)` metody w klasie związanej z kodem strony ASP.NET, metody klasy związane z kodem daje większą elastyczność podczas pracy z dane pobrane, jak zajmiemy się wkrótce.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Pobieranie wszystkich informacji o produkcie jednocześnie

Dwie metody poprzedniej możemy stawienia zbadać pobrania tych produktów dla bieżącej kategorii poprzez wywołanie `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` — metoda (pierwszym sposobem dokonał tego za pośrednictwem ObjectDataSource drugiego do `GetProductsInCategory(categoryID)` metody w klasy związane z kodem). Zawsze ta metoda jest wywoływana, wywołania warstwy logiki biznesowej do warstwy dostępu do danych, który wysyła zapytanie do bazy danych za pomocą instrukcji SQL, która zwraca wiersze z `Products` tabeli, którego `CategoryID` pole zgodne podany parametr wejściowy.

Podane *N* kategorii w systemie, to rozwiązanie sieci *N* + 1 wywołań zapytanie bazy danych jedną bazę danych można pobrać wszystkich kategorii, a następnie *N* połączeń w celu pobrania produktów specyficzne dla każdej kategorii. Można jednak pobrać wszystkich potrzebnych danych w jednym wywołaniu wywołania tylko dwie bazy danych można uzyskać wszystkie kategorie i drugiego do wszystkich produktów. Gdy mamy wszystkie produkty, firma Microsoft można filtrować tych produktów tak jedynie produkty dopasowania bieżącego `CategoryID` są powiązane z tej kategorii s wewnętrzny elementu powtarzanego.

Do tej funkcji, tylko musimy upewnić nieznaczne modyfikację `GetProductsInCategory(categoryID)` metody w klasie CodeBehind strony s naszej platformy ASP.NET. Zamiast ślepo zwracania wyników z `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` metody, możesz zamiast tego najpierw przejść do *wszystkie* produktów (jeśli one informacjami o klientach t został już dostępne), a następnie wróć właśnie filtrowany widok produkty oparte na przekazany do `CategoryID`.


[!code-vb[Main](nested-data-web-controls-vb/samples/sample8.vb)]

Należy pamiętać, Dodawanie zmiennej na poziomie strony `allProducts`. To są przechowywane informacje dotyczące wszystkich produktów i jest wypełniana przy pierwszym `GetProductsInCategory(categoryID)` wywołania metody. Po upewnieniu się, że `allProducts` obiekt został utworzony i wypełniane, metody filtruje wyniki s DataTable w taki sposób, że tylko te wiersze, w których `CategoryID` jest zgodna z określoną `CategoryID` są dostępne. Takie podejście pozwala zmniejszyć liczbę razy bazy danych jest dostępny z *N* + 1 w dół, aby dwa.

To rozszerzenie nie powoduje żadnych zmian do renderowanego kodu znaczników strony ani osiągać rekordów wstecz mniej niż innej metody. Po prostu zmniejsza liczbę wezwań do bazy danych.

> [!NOTE]
> Jeden intuicyjnie przyczyny, że zmniejszenie liczby uzyskuje dostęp do bazy danych będzie assuredly zwiększenia wydajności. Jednak nie może to być wielkość liter. Jeśli masz dużą liczbę produktów których `CategoryID` jest `NULL`, na przykład, a następnie wywołać `GetProducts` metoda zwraca liczbę produktów, które nigdy nie są wyświetlane. Ponadto zwracanie wszystkich produktów może być niepotrzebne Jeśli której wyświetlane są tylko podzbiór kategorie, które może być w przypadku, jeśli zostały zaimplementowane stronicowania.


Jak zawsze, gdy chodzi o analizowanie wydajności dwie metody, tylko surefire miary jest do uruchomienia testów kontrolowane dostosowane do Twojej aplikacji s typowych scenariuszy przypadków.

## <a name="summary"></a>Podsumowanie

W tym samouczku widzieliśmy jak zagnieździć jednego danych formantu sieci Web w innym, w szczególności badanie sposobu ustawiania zewnętrznego elementu powtarzanego wyświetlania elementu dla każdej kategorii z wewnętrzny elementu powtarzanego listę produktów dla każdej kategorii na liście punktowanej. Główne żądania tworzenia interfejsu użytkownika zagnieżdżonych znajduje się w dostęp i powiązanie danych z formantem Web wewnętrzny danych. Istnieją różne metody dostępne, z których dwa możemy zbadane, w tym samouczku. Pierwszym sposobem zbadane używany element ObjectDataSource zewnętrzne danych formantu sieci Web s `ItemTemplate` która została powiązana z kontroli danych wewnętrznej sieci Web za pośrednictwem jego `DataSourceID` właściwości. Druga metoda dostępu do danych za pomocą metody w klasie związanej z kodem s strony ASP.NET. Ta metoda może być powiązana następnie wewnętrzny danych formantu sieci Web s `DataSource` właściwości za pośrednictwem składnia wiązania z danymi.

Interfejs użytkownika zagnieżdżony, w tym samouczku używane elementu powtarzanego zagnieżdżone w obrębie elementu powtarzanego, można rozszerzyć te techniki innych formantów danych sieci Web. Można zagnieździć elementu powtarzanego w widoku GridView lub GridView wewnątrz elementu DataList i tak dalej.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Kowalski Zack i Liz Shulok. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
