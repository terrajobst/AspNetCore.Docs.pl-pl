---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
title: Wstawianie nowego rekordu w stopce kontrolki GridView (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Podczas kontrolki GridView nie zapewnia wbudowaną obsługę do wstawiania nowego rekordu danych, w tym samouczku pokazano, jak rozszerzyć GridView obejmujący...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 49545652-98af-46ba-9dbc-9ab529805d9b
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: ae27b25a0a7f37cc273d88d7165c8be33ce2f6cc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754344"
---
<a name="inserting-a-new-record-from-the-gridviews-footer-c"></a>Wstawianie nowego rekordu w stopce kontrolki GridView (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_CS.exe) lub [Pobierz plik PDF](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/datatutorial53cs1.pdf)

> Gdy kontrolki GridView nie zapewnia wbudowaną obsługę do wstawiania nowego rekordu danych, w tym samouczku pokazano, jak rozszerzyć GridView obejmujący Wstawianie interfejsu.


## <a name="introduction"></a>Wprowadzenie

Zgodnie z opisem w [Przegląd Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) samouczek kontrolek każdego widoku GridView, DetailsView i FormView Web obejmują możliwości modyfikacji danych wbudowane. W przypadku użycia z kontrolki źródła danych deklaratywne, te trzy kontrolki sieci Web można szybko i łatwo skonfigurować do modyfikowania danych — i w scenariuszach bez konieczności pisania nawet wiersza kodu. Niestety tylko formanty DetailsView i FormView zawierają wbudowane wstawiania, edytowanie i usuwanie możliwości. Kontrolki GridView tylko ofert, edytowania i usuwania pomocy technicznej. Jednak nieco smarem kątowa, możemy rozszerzyć GridView obejmujący Wstawianie interfejsu.

Dodanie możliwości wstawianie do kontrolki GridView, odpowiadamy przy wyborze rozwiązania, w jaki sposób nowe rekordy zostaną dodane, tworzenia interfejsu podano Wstawianie i pisanie kodu, aby wstawić nowy rekord. W tym samouczku przyjrzymy się dodanie interfejsu wstawianie w stopce kontrolki GridView s wierszy (patrz rysunek 1). Komórka stopki dla każdej kolumny zawiera odpowiednie dane kolekcji element interfejsu użytkownika (pole tekstowe nazwy produktu s, kontrolki DropDownList dla dostawcy i tak dalej). Potrzebujemy kolumny Dodaj przycisk, po kliknięciu zostanie powoduje odświeżenie strony i wstawić nowy rekord do `Products` tabeli, używając wartości podane w wierszu stopki.


[![Wiersz stopki udostępnia interfejs do dodawania nowych produktów](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.png)

**Rysunek 1**: wiersz stopki udostępnia interfejs dla dodawania nowych produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>Krok 1: Wyświetlanie informacji o produkcie w widoku GridView

Zanim możemy określić główną przyczynę dotyczą tworzenia interfejsu wstawianie w stopce kontrolki GridView s, chętnie s pierwszy koncentracji uwagi na temat dodawania GridView do strony, która zawiera listę produktów w bazie danych. Zacznij od otwarcia `InsertThroughFooter.aspx` strony w `EnhancedGridView` folder i przeciągnij GridView z przybornika w projektancie, ustawienie GridView s `ID` właściwość `Products`. Następnie użyj tagu inteligentnego s GridView, aby powiązać nowe kontrolki ObjectDataSource, o nazwie `ProductsDataSource`.


[![Tworzenie nowego elementu ObjectDataSource, o nazwie ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.png)

**Rysunek 2**: Utwórz nowy o nazwie elementu ObjectDataSource `ProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.png))


Konfigurowanie kontrolki ObjectDataSource używać `ProductsBLL` klasy s `GetProducts()` metodę, aby pobrać informacje o produkcie. W tym samouczku pozwalają fokus s wyłącznie na dodawanie funkcji Wstawianie i nie martw się o edytowania i usuwania. Dlatego upewnij się, że listy rozwijanej na karcie Wstawianie jest ustawiona na `AddProduct()` i że list rozwijanych w karty aktualizacji i usuwania są ustawione na (Brak).


[![Map, metoda AddProduct metody Insert() s ObjectDataSource](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.png)

**Rysunek 3**: mapy `AddProduct` metoda s ObjectDataSource `Insert()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.png))


[![Ustawianie list rozwijanych aktualizacji i usuwania karty (Brak)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.png)

**Rysunek 4**: Ustaw aktualizacji i usuwania listy rozwijane karty (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.png))


Po zakończeniu pracy kreatora Konfigurowanie źródła danych s ObjectDataSource, Visual Studio spowoduje automatyczne dodanie pola do widoku GridView dla każdego odpowiedniego pola danych. Na razie pozostaw wszystkie pola, który został dodany przez program Visual Studio. Później w tym samouczku utworzymy możesz wrócić i usunąć niektóre pola, których wartości don t, muszą być określone, podczas dodawania nowego rekordu.

Ponieważ blisko 80 produktów w bazie danych, użytkownik będzie miał być przewinięcie w dół aż do dolnej części strony sieci web aby można było dodać nowy rekord. W związku z tym Niech s Włączanie stronicowania umożliwiają wstawianie interfejsu bardziej widoczne i dostępne. Aby włączyć funkcję stronicowania, po prostu zaznacz pole wyboru włączenia stronicowania z tagu inteligentnego s GridView.

W tym momencie kontrolkami GridView i kontrolki ObjectDataSource s oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample1.aspx)]


[![Wszystkie pola danych produktu są wyświetlane w widoku GridView stronicowanej](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.png)

**Rysunek 5**: wszystkie pola danych produktu są wyświetlane w widoku GridView stronicowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>Krok 2: Dodawanie wierszy stopki

Wraz z jego nagłówka i wiersze danych widoku GridView zawiera wiersz stopki. Wiersze nagłówki i stopki są wyświetlane w zależności od wartości GridView s [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) i [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) właściwości. Aby wyświetlić wiersz stopki, po prostu ustaw `ShowFooter` właściwość `true`. Tak jak pokazano w rysunek 6 ustawienie `ShowFooter` właściwość `true` dodaje wiersz stopki do siatki.


[![Aby wyświetlić wiersz stopki, ustaw ShowFooter wartość True](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.png)

**Rysunek 6**: Ustaw wierszu stopki do wyświetlenia `ShowFooter` do `True` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.png))


Należy pamiętać o tym, czy wiersz stopki ma kolor ciemnym tle czerwony. Jest to spowodowane motyw DataWebControls, możemy utworzyć i stosowane do wszystkich stron w [wyświetlanie danych za pomocą kontrolki ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) samouczka. W szczególności `GridView.skin` plik konfiguruje `FooterStyle` właściwość takich zastosowań `FooterStyle` klasę CSS. `FooterStyle` Klasa jest zdefiniowana w `Styles.css` w następujący sposób:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample2.css)]

> [!NOTE]
> Firma Microsoft ve przedstawione w poprzednich samouczkach przy użyciu wiersza stopce kontrolki GridView s. Jeśli to konieczne, odwołaj się do [wyświetlanie informacji podsumowania w stopce kontrolki GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) samouczków dla przypomnienia informacji.


Po ustawieniu `ShowFooter` właściwości `true`, Poświęć chwilę, aby wyświetlić dane wyjściowe w przeglądarce. Obecnie t wiersz stopki zawierać tekst i formantów sieci Web. W kroku 3 zmodyfikujemy stopkę dla każdego pola GridView aby obejmowała odpowiedni interfejs Wstawianie.


[![Wiersz pusty stopka jest wyświetlone powyżej stronicowania kontrolek interfejsu](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image13.png)

**Rysunek 7**: Stopka pusty wiersz jest wyświetlone powyżej stronicowania kontrolek interfejsu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>Krok 3: Dostosowywanie wiersz stopki

Ponownie [za pomocą kontrolek TemplateField w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) samouczek widzieliśmy znacznie dostosowywania wyświetlania konkretnej kolumny GridView używanie kontrolek TemplateField (w przeciwieństwie do BoundFields lub CheckBoxFields); w [ Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) przyjrzeliśmy się używanie kontrolek TemplateField Dostosowywanie interfejsu edycji, z GridView. Odwołania, że TemplateField składa się z wiele szablonów określa kombinacja znaczników, kontrolki sieci Web i składnia wiązania danych używaną do pewnych typów wierszy. `ItemTemplate`, Na przykład Określa szablon używany do wierszy tylko do odczytu, podczas gdy `EditItemTemplate` Określa szablon, który można edytować wiersza.

Wraz z `ItemTemplate` i `EditItemTemplate`, obejmuje także TemplateField `FooterTemplate` określający zawartość wiersza stopki. W związku z tym, można dodać kontrolki sieci Web, potrzebne dla każdego pola s, wstawianie interfejsu do `FooterTemplate`. Aby rozpocząć, przekonwertować wszystkie pola w widoku GridView kontrolek TemplateField. Można to zrobić kliknięcie linku Edytowanie kolumn w kontrolce GridView s tagu inteligentnego, wybierając każdego pola w lewym dolnym rogu, a po kliknięciu łącza TemplateField Convert to pole.


![Każde pole należy konwertować TemplateField](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.gif)

**Rysunek 8**: każde pole należy konwertować TemplateField


Kliknięcie przycisku Konwertuj tego pola do TemplateField włącza bieżącego typu pola w TemplateField równoważne. Na przykład każdego elementu BoundField zastępuje TemplateField z `ItemTemplate` zawierający etykietę, która wyświetla odpowiednie pole danych i `EditItemTemplate` wyświetlającą pola danych w polu tekstowym. `ProductName` Elementu BoundField został przekształcony w niej następujące znaczniki TemplateField:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample3.aspx)]

Podobnie `Discontinued` CheckBoxField został przekształcony w TemplateField którego `ItemTemplate` i `EditItemTemplate` zawierać formant pola wyboru w sieci Web (przy użyciu `ItemTemplate` s wyboru wyłączone). Tylko do odczytu `ProductID` elementu BoundField został przekształcony w TemplateField za pomocą kontrolki etykiety w obu `ItemTemplate` i `EditItemTemplate`. Krótko mówiąc pole TemplateField Konwertowanie istniejących GridView jest szybka i łatwa metoda, aby przełączyć się do szersze TemplateField bez utraty istniejących funkcji pola s.

Ponieważ widoku GridView możemy ponownie Praca z edycji pomocy technicznej t możesz usunąć `EditItemTemplate` z każdego TemplateField, pozostawiając tylko `ItemTemplate`. Po wykonaniu tego, deklaratywne kontrolki GridView znaczników w s powinna wyglądać następująco:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample4.aspx)]

Teraz, że każde pole GridView został przekształcony w TemplateField, możemy wprowadzić odpowiedni interfejs wstawianie do każdego pola s `FooterTemplate`. Niektóre pola nie zostaną Wstawianie interfejsu (`ProductID`, na przykład); inne osoby będą się różnić w kontrolkach internetowych, używane do zbierania nowe informacje o produkcie s.

Aby utworzyć interfejs edycji, wybierz łącze Edytuj szablony z tagu inteligentnego s GridView. Z listy rozwijanej, wybierz odpowiednie pole s `FooterTemplate` i przeciągnij odpowiednie kontrolki z przybornika do projektanta.


[![Dodaj do każdego pola s FooterTemplate odpowiedni interfejs Wstawianie](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image15.png)

**Rysunek 9**: dodać odpowiedni interfejs wstawiania do każdego pola s `FooterTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image16.png))


Poniższej liście punktowanej wylicza pola GridView, określając Wstawianie interfejsu do dodania:

- `ProductID` Brak.
- `ProductName` Dodaj pole tekstowe i ustaw jego `ID` do `NewProductName`. Dodać formant RequiredFieldValidator, który jest również, aby upewnić się, że użytkownik musi wprowadzić wartość dla nowej nazwy produktu s.
- `SupplierID` Brak.
- `CategoryID` Brak.
- `QuantityPerUnit` Dodawanie pola tekstowego, ustawiając jego `ID` do `NewQuantityPerUnit`.
- `UnitPrice` Dodaj pole tekstowe o nazwie `NewUnitPrice` i CompareValidator, które gwarantuje, że wprowadzona wartość jest większa lub równa zero wartości waluty.
- `UnitsInStock` Użyj pola tekstowego którego `ID` ustawiono `NewUnitsInStock`. Obejmują CompareValidator, który zapewnia, że wprowadzona wartość jest liczbą całkowitą większą niż lub równa zero.
- `UnitsOnOrder` Użyj pola tekstowego którego `ID` ustawiono `NewUnitsOnOrder`. Obejmują CompareValidator, który zapewnia, że wprowadzona wartość jest liczbą całkowitą większą niż lub równa zero.
- `ReorderLevel` Użyj pola tekstowego którego `ID` ustawiono `NewReorderLevel`. Obejmują CompareValidator, który zapewnia, że wprowadzona wartość jest liczbą całkowitą większą niż lub równa zero.
- `Discontinued` Dodawanie pola wyboru, ustawiając jego `ID` do `NewDiscontinued`.
- `CategoryName` Dodawanie kontrolki DropDownList i ustaw jego `ID` do `NewCategoryID`. Powiązywanie nowe kontrolki ObjectDataSource, o nazwie `CategoriesDataSource` i skonfigurować go do używania `CategoriesBLL` klasy s `GetCategories()` metody. Ma DropDownList s `ListItem` wyświetlania s `CategoryName` danych pola, przy użyciu `CategoryID` pola danych jako ich wartości.
- `SupplierName` Dodawanie kontrolki DropDownList i ustaw jego `ID` do `NewSupplierID`. Powiązywanie nowe kontrolki ObjectDataSource, o nazwie `SuppliersDataSource` i skonfigurować go do używania `SuppliersBLL` klasy s `GetSuppliers()` metody. Ma DropDownList s `ListItem` wyświetlania s `CompanyName` danych pola, przy użyciu `SupplierID` pola danych jako ich wartości.

Dla każdego z kontrolkami walidacji, wyczyść `ForeColor` właściwość tak, aby `FooterStyle` kolor biały klasy s CSS, będzie używany zamiast domyślnego czerwony. Również użyć `ErrorMessage` szczegółowy opis, ale właściwością `Text` właściwość gwiazdki. Aby zapobiec powoduje wstawianie interfejsu opakowywać w dwóch wierszach tekst kontrolki s sprawdzania poprawności, należy ustawić `FooterStyle` s `Wrap` właściwości na wartość false dla każdego `FooterTemplate` s, używanego przez formant sprawdzania poprawności. Na koniec Dodaj kontrolki podsumowania walidacji poniżej GridView i ustaw jego `ShowMessageBox` właściwości `true` i jego `ShowSummary` właściwość `false`.

Podczas dodawania nowego produktu, należy podać `CategoryID` i `SupplierID`. Te informacje są przechwytywane za pomocą kontrolek DROPDOWNLIST w komórkach stopkę dla `CategoryName` i `SupplierName` pola. Wybrano do użycia tych pól, w przeciwieństwie do `CategoryID` i `SupplierID` kontrolek TemplateField w siatce s wiersze danych, użytkownik jest prawdopodobnie bardziej zainteresowani wyświetlanie nazwy kategorii i dostawcy, a nie ich wartości Identyfikatora. Ponieważ `CategoryID` i `SupplierID` teraz przechwytywanych wartości w `CategoryName` i `SupplierName` pola s Wstawianie interfejsów, możemy usunąć `CategoryID` i `SupplierID` kontrolek TemplateField w kontrolce GridView.

Podobnie `ProductID` nie jest używany podczas dodawania nowego produktu, więc `ProductID` TemplateField można również usunąć. Jednak umożliwia s pozostaw `ProductID` pola w siatce. Oprócz pól tekstowych, kontrolek DropDownList, pola wyboru i formanty sprawdzania poprawności, które tworzą interfejs Wstawianie, będziemy również potrzebować Dodaj przycisk, który, po kliknięciu wykonuje logiki, aby dodać nowy produkt do bazy danych. W kroku 4 dołączamy będzie przycisk dodawania w interfejsie wstawianie `ProductID` TemplateField s `FooterTemplate`.

Możesz poprawić wygląd różnych pól GridView. Na przykład możesz chcieć formatowania `UnitPrice` wartości jako waluta, wyrównanie do prawej `UnitsInStock`, `UnitsOnOrder`, i `ReorderLevel` pola i aktualizacji `HeaderText` wartości kontrolek TemplateField.

Po utworzeniu slew wstawiania interfejsy w `FooterTemplate` s, usuwając `SupplierID`, i `CategoryID` kontrolek TemplateField i poprawy wyglądu siatki za pomocą formatowania i wyrównywanie kontrolek TemplateField, Twoje deklaratywne s GridView Kod znaczników powinien wyglądać podobnie do następującego:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample5.aspx)]

Po wyświetleniu za pośrednictwem przeglądarki, wiersz stopce kontrolki GridView s zawiera teraz gotowy Wstawianie interfejsu (zobacz rysunek 10). W tym momencie Wstawianie t interfejs obejmują oznacza, że dla użytkownika wskazać, że s nagrywa wprowadzić dane dla nowego produktu i chce, aby wstawić nowy rekord do bazy danych. Ponadto firma Microsoft ve jeszcze umożliwiającą, jak dane wprowadzone w stopce będzie przekłada się na nowy rekord w `Products` bazy danych. W kroku 4, omówimy sposób obejmują przycisk dodawania do interfejsu Wstawianie oraz wykonanie kodu na ogłaszanie zwrotne po jego kliknięciu s. Krok 5 przedstawia sposób wstawiania nowego rekordu przy użyciu danych z stopki.


[![W stopce kontrolki GridView udostępnia interfejs dla dodawania nowego rekordu](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image17.png)

**Na rysunku nr 10**: stopce kontrolki GridView udostępnia interfejs dla dodawania nowego rekordu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Krok 4: W tym przycisk dodawania w interfejsie Wstawianie

Musimy uwzględnić przycisk dodawania gdzieś w interfejsie Wstawianie ponieważ s wiersz stopki Wstawianie interfejsu obecnie nie ma oznacza, że dla użytkownika wskazać, że ich zostały ukończone, wprowadzając nowe informacje o produkcie s. To może być umieszczone w jednej z istniejących `FooterTemplate` s lub firma Microsoft może dodać nową kolumnę do siatki w tym celu. W tym samouczku umożliwiają s umieścić przycisk Dodaj w `ProductID` TemplateField s `FooterTemplate`.

Przy użyciu projektanta, kliknij link Edytuj szablony w tagu inteligentnego s GridView, a następnie wybierz `ProductID` s pola `FooterTemplate` z listy rozwijanej. Dodawanie kontrolki przycisku w sieci Web (lub element LinkButton lub ImageButton, jeśli użytkownik sobie tego życzy) do szablonu, ustawiając jej identyfikator na `AddProduct`, jego `CommandName` do wstawiania, a jego `Text` właściwość do dodania, jak pokazano na ilustracji 11.


[![Umieść przycisk Dodaj w FooterTemplate s ProductID TemplateField](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image19.png)

**Rysunek 11**: Umieść Dodaj przycisk w `ProductID` TemplateField s `FooterTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image20.png))


Po był dołączony przycisk Dodaj, należy przetestować stronę w przeglądarce. Należy pamiętać, że po kliknięciu przycisku Dodaj przy użyciu nieprawidłowych danych w interfejsie Wstawianie zwrotu jest krótko circuited i kontrolki podsumowania walidacji wskazuje nieprawidłowe dane (zobacz rysunek 12). Wprowadzić odpowiednie dane klikając przycisk Dodaj powoduje odświeżenie strony. Brak rekordu jest dodawany do bazy danych, jednak. Firma Microsoft będzie konieczne napisanie ilość kodu, aby faktycznie wykonać insert.


[![S Dodaj przycisk odświeżania wynosi krótki Circuited nieprawidłowe dane w interfejsie wstawiania](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image21.png)

**Rysunek 12**: s, Dodaj przycisk odświeżania wynosi krótki Circuited nieprawidłowe dane w interfejsie wstawiania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image22.png))


> [!NOTE]
> Formanty sprawdzania poprawności w interfejsie Wstawianie nie zostały przypisane do grupy sprawdzania poprawności. Działa to prawidłowo, tak długo, jak wstawianie interfejs jest ustawione tylko sprawdzania poprawności formantów na stronie. Jeśli istnieją inne formanty sprawdzania poprawności na stronie (na przykład formanty sprawdzania poprawności w interfejsie edycji siatki s), formanty sprawdzania poprawności w Wstawianie interfejs i dodać przycisk s `ValidationGroup` właściwości powinien być przypisany do tej samej wartości tak, aby Te kontrolki należy skojarzyć z grupą określonego sprawdzania poprawności. Zobacz [analiza formanty sprawdzania poprawności w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) więcej informacji na temat partycjonowania formanty sprawdzania poprawności i przyciski na stronie do sprawdzania poprawności grupy.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Krok 5: Wstawianie nowego rekordu w`Products`tabeli

Po korzystanie z wbudowanych funkcji edycji GridView widoku GridView automatycznie obsługuje wszystkie zadania niezbędne do wykonywania aktualizacji. W szczególności, po kliknięciu przycisku Aktualizuj kopiuje wartości wprowadzone z interfejsu edycji do parametrów w elemencie ObjectDataSource s `UpdateParameters` zbierania i kopnięć wyłączyć aktualizacji za pomocą wywołania ObjectDataSource s `Update()` metody. Ponieważ w widoku GridView nie zapewnia takiego wbudowanej funkcji wstawiania, musimy zaimplementować kod, który wywołuje ObjectDataSource s `Insert()` metody i kopiuje wartości z Wstawianie interfejsu s ObjectDataSource `InsertParameters` kolekcji .

Tę logikę wstawiania powinien być wykonywany po kliknięciu przycisku Dodaj. Zgodnie z opisem w [dodawanie przycisków i reagowanie na w GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs.md) samouczek, w dowolnym momencie przycisk, element LinkButton lub ImageButton w GridView po kliknięciu GridView s `RowCommand` generowane zdarzenie na zwrot. To zdarzenie jest uruchamiany, czy przycisk, element LinkButton lub ImageButton dodano jawnie takich jak przycisk Dodaj w wierszu stopki, lub jeśli została automatycznie dodana przez kontrolki GridView (takie jak LinkButtons u góry każdej kolumny, gdy zaznaczono pole wyboru Włącz sortowanie, lub LinkButtons w interfejsie stronicowania, jeśli wybrano włączenia stronicowania).

W związku z tym, aby odpowiedzieć użytkownik, kliknij przycisk Dodaj, musimy utworzyć program obsługi zdarzeń dla GridView s `RowCommand` zdarzeń. Ponieważ to zdarzenie jest generowane każdorazowo *wszelkie* po kliknięciu przycisku, element LinkButton lub ImageButton w widoku GridView go s istotne, że będziemy kontynuować, tylko z logiką wstawianie `CommandName` właściwość przekazany do mapowania programu obsługi zdarzeń do `CommandName` wartość przycisk Dodaj (Wstaw). Ponadto możemy również Kontynuuj tylko wtedy, jeśli prawidłowe dane zgłasza, formanty sprawdzania poprawności. Aby to umożliwić, należy utworzyć program obsługi zdarzeń dla `RowCommand` zdarzeń z następującym kodem:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample6.cs)]

> [!NOTE]
> Możesz się zastanawiać, dlaczego program obsługi zdarzeń bothers sprawdzanie `Page.IsValid` właściwości. Gdy wszystkie uzyskany t zwrotu można pominąć, jeśli podano nieprawidłowe dane w interfejsie Wstawianie? To założenie jest poprawna, dopóki użytkownik nie wyłączył JavaScript lub podjęte kroki w celu obejścia logiki weryfikacji po stronie klienta. Krótko mówiąc jeden nigdy nie będą miały ściśle weryfikacji po stronie klienta; sprawdzenia poprawności po stronie serwera powinny być zawsze realizowane przed rozpoczęciem pracy z danymi.


W kroku 1 utworzyliśmy `ProductsDataSource` ObjectDataSource taki sposób, że jego `Insert()` metody jest mapowany na `ProductsBLL` klasy s `AddProduct` metody. Aby wstawić nowy rekord do `Products` tabeli, można po prostu wywołać ObjectDataSource s `Insert()` metody:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample7.cs)]

Teraz, gdy `Insert()` wywołaniu metody, wszystkie opcje, które pozostaje tylko skopiuj wartości z interfejsu Wstawianie parametry przekazywane do `ProductsBLL` klasy s `AddProduct` metody. Jak widzieliśmy w [badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) samouczków, można to zrobić za pośrednictwem ObjectDataSource s `Inserting` zdarzeń. W `Inserting` zdarzeń, musimy programowo odwołują się do formantów z `Products` stopki s GridView wiersza, a ich wartości, aby przypisać `e.InputParameters` kolekcji. Jeśli użytkownik pomija wartością, taką jak opuszczania `ReorderLevel` puste pole tekstowe, należy określić wartość wstawione do bazy danych należy `NULL`. Ponieważ `AddProducts` metoda przyjmuje typy dopuszczające wartości null dla pól bazy danych dopuszcza wartości null, po prostu użyć typu dopuszczającego wartość null i ustawić jej wartość na `null` w przypadku, gdy dane wejściowe użytkownika zostanie pominięty.


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample8.cs)]

Za pomocą `Inserting` programu obsługi zdarzeń jest zakończone, nowe rekordy można dodać do `Products` tabeli bazy danych za pośrednictwem wiersza stopce kontrolki GridView s. Przejdź dalej i spróbuj dodać kilka nowych produktów.

## <a name="enhancing-and-customizing-the-add-operation"></a>Udoskonalanie i dostosowywanie operacji dodawania

Obecnie w kliknij przycisk Dodaj dodaje nowy rekord do tabeli bazy danych, ale nie zapewnia dowolny rodzaj wizualną opinię, rekord został pomyślnie dodany. W idealnym przypadku Web etykiety formantu lub po stronie klienta alertu polem będzie poinformować użytkownika o ich wstawiania została ukończona pomyślnie. Można pozostawić to w charakterze ćwiczenia dla czytnika.

GridView używane w tym samouczku nie dotyczą żadnych kolejność sortowania listy produktów, ani nie zezwala użytkownikowi końcowemu sortowania danych. W rezultacie rekordy są uporządkowane jako znajdują się w bazie danych przez ich klucz podstawowy. Ponieważ każdy nowy rekord ma `ProductID` wartość większą niż ostatni z nich, za każdym razem, gdy nowy produkt jest dodawana go jest kumulowany końcu siatki. W związku z tym można automatycznie wysłać użytkownika do ostatniej strony widoku GridView po dodaniu nowego rekordu. Można to osiągnąć, dodając następujący wiersz kodu po wywołaniu `ProductsDataSource.Insert()` w `RowCommand` programu obsługi zdarzeń, aby wskazać, że użytkownik potrzebuje do wysłania do ostatniej strony po powiązaniu danych z widoku GridView:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample9.cs)]

`SendUserToLastPage` jest na poziomie strony zmiennej typu Boolean, która jest początkowo przypisana wartość `false`. W tym s GridView `DataBound` procedura obsługi zdarzeń, jeśli `SendUserToLastPage` ma wartość FAŁSZ, `PageIndex` właściwość zostanie zaktualizowany i będzie wysłać użytkownika do ostatniej strony.


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample10.cs)]

Przyczyna `PageIndex` właściwość jest ustawiona `DataBound` programu obsługi zdarzeń (w przeciwieństwie do `RowCommand` programu obsługi zdarzeń) jest, ponieważ podczas `RowCommand` programu obsługi zdarzeń uruchamia, firma Microsoft ve jeszcze, aby dodać nowy rekord do `Products` tabeli bazy danych. Dlatego w `RowCommand` programu obsługi zdarzeń ostatni indeks strony (`PageCount - 1`) reprezentuje ostatni indeks strony *przed* został dodany nowy produkt. Dla większości produktów dodawany ostatni indeks strony jest taka sama po dodaniu nowego produktu. Ale kiedy dodany produkt wyników w nowych ostatni indeks strony, jeśli niepoprawnie zaktualizować `PageIndex` w `RowCommand` programu obsługi zdarzeń, a następnie możemy nastąpi przekierowanie do drugiego do ostatniej strony (ostatni indeks strony przed dodaniem nowego produktu) w przeciwieństwie do nowej strony ostatnich i ndex. Ponieważ `DataBound` obsługi zdarzenia generowane po został dodany nowy produkt i dane odbitych do siatki, ustawiając `PageIndex` właściwości wiemy, że firma Microsoft ponowne wprowadzenie poprawne ostatni indeks strony.

Na koniec GridView używane w tym samouczku jest dość szeroka, ze względu na liczbę pól, które należy zebrać, aby dodać nowy produkt. Ze względu na to szerokość DetailsView układ pionowy s może być preferowane. GridView s całkowita szerokość może zostać zmniejszona przy zbieraniu mniejszej liczby danych wejściowych. Być może mamy don t nad koniecznością zbierania następujących `UnitsOnOrder`, `UnitsInStock`, i `ReorderLevel` pól podczas dodawania nowego produktu, w którym to przypadku można usunąć te pola w z widoku GridView.

Aby dostosować zebranych danych, możemy użyć jednej z dwóch metod:

- W dalszym ciągu używać `AddProduct` metodę, która oczekuje wartości `UnitsOnOrder`, `UnitsInStock`, i `ReorderLevel` pola. W `Inserting` procedura obsługi zdarzeń, podaj zakodowane, domyślne wartości dla tych danych wejściowych, które zostały usunięte z Wstawianie interfejsu.
- Utwórz nowe przeciążenia `AddProduct` method in Class metoda `ProductsBLL` klasę, która nie akceptuje dane wejściowe dla `UnitsOnOrder`, `UnitsInStock`, i `ReorderLevel` pola. Następnie na stronie ASP.NET, skonfiguruj kontrolki ObjectDataSource, aby użyć tego nowego przeciążenia.

Jedną z opcji będzie działać równie również. W przeszłości samouczki użyliśmy druga opcję tworzenia wielu przeciążenia `ProductsBLL` klasy s `UpdateProduct` metody.

## <a name="summary"></a>Podsumowanie

Kontrolki GridView nie posiada wbudowanej funkcji Wstawianie znalezionych w DetailsView i FormView, ale nieco nakładu pracy interfejs Wstawianie — można dodać wiersz stopki. Aby wyświetlić wiersz stopki w GridView po prostu ustawić jej `ShowFooter` właściwość `true`. Można dostosować zawartość wiersza stopkę dla każdego pola, konwertując pole TemplateField i dodawania, wstawiania współpracować w celu `FooterTemplate`. Jak widzieliśmy w tym samouczku `FooterTemplate` może zawierać przyciski, pola tekstowe, kontrolek DropDownList, pola wyboru, kontrolki źródła danych do wypełniania danymi kontrolki sieci Web (na przykład kontrolek DROPDOWNLIST) i kontrolkami walidacji. Wraz z służy do zbierania danych wejściowych użytkownika s Dodaj przycisk, element LinkButton lub ImageButton są potrzebne.

Po kliknięciu przycisku Dodaj, ObjectDataSource s `Insert()` metoda jest wywoływana, aby uruchomić Wstawianie przepływ pracy. Kontrolki ObjectDataSource zostanie następnie wywołaj metodę skonfigurowanego wstawiania ( `ProductsBLL` klasy s `AddProduct` metody, w ramach tego samouczka). Firma Microsoft musi kopiować wartości z s GridView Wstawianie interfejsu s ObjectDataSource `InsertParameters` kolekcji przed wywoływana metoda insert. Można to osiągnąć przez odwołanie programowe Wstawianie kontrolki sieci Web interfejs, w tym s kontrolki ObjectDataSource `Inserting` programu obsługi zdarzeń.

W tym samouczku kończy naszych poznać techniki poprawa wyglądu s GridView. Kolejny zbiór samouczki zbada, jak pracować z danymi binarnymi, taką jak obrazy, pliki PDF, dokumentów programu Word i tak dalej i dane kontrolki sieci Web.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Bernadette Leigh. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](adding-a-gridview-column-of-checkboxes-cs.md)
> [dalej](adding-a-gridview-column-of-radio-buttons-vb.md)
