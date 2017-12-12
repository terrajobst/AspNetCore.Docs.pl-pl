---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: Wstawianie nowego rekordu w widoku GridView stopki (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: "Gdy kontrolki widoku siatki nie zapewnia wbudowaną obsługę dla Wstawianie nowego rekordu danych, w tym samouczku pokazano, jak rozszerzyć widoku GridView, aby uwzględnić..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d452e15ced52fd9dcac8201598146cb9ef38d7b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>Wstawianie nowego rekordu w widoku GridView stopki (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe) lub [pobierania plików PDF](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> Gdy kontrolki widoku siatki nie zapewnia wbudowaną obsługę dla Wstawianie nowego rekordu danych, w tym samouczku pokazano, jak rozszerzyć GridView uwzględnienie Wstawianie interfejsu.


## <a name="introduction"></a>Wprowadzenie

Zgodnie z opisem w [omówienie Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) samouczek, dostępne kontrolki każdego widoku GridView widoku DetailsView i FormView Web funkcji modyfikacji danych wbudowanych. W przypadku użycia z kontrolki źródła dane deklaratywne, te trzy formantów sieci Web można szybko i łatwo skonfigurować do modyfikowania danych — w scenariuszach bez konieczności zapisu pojedynczy wiersz kodu. Niestety tylko formanty widoku DetailsView i FormView Podaj wbudowanych Wstawianie, edytowanie i usuwanie funkcji. Widoku GridView umożliwia tylko edytowanie i usuwanie pomocy technicznej. Jednak nieco smarem kątowa, możemy rozszerzyć GridView uwzględnienie Wstawianie interfejsu.

W Dodawanie funkcji wstawianie do widoku GridView, firma Microsoft są zobowiązani do podejmowania decyzji, w jaki sposób nowe rekordy zostaną dodane, tworzenie Wstawianie interfejsu i pisanie kodu, aby wstawić nowy rekord. W tym samouczku przedstawiono dodawanie Wstawianie interfejsu do stopki s GridView wierszy (zobacz rysunek 1). Komórka stopki dla każdej kolumny zawiera odpowiednie dane kolekcji elementu interfejsu użytkownika (pole tekstowe nazwy produktu s, DropDownList dla dostawcy i tak dalej). Potrzebujemy kolumny Dodaj przycisku, gdy kliknięto, będą powodować odświeżenie strony i wstawić nowy rekord w `Products` tabeli, używając wartości podane w wierszu stopki.


[![Wiersz stopki zawiera interfejs umożliwiający dodawanie nowych produktów](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**Rysunek 1**: wiersz stopki zawiera interfejs umożliwiający dodawanie nowych produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>Krok 1: Wyświetlanie informacji o produkcie, w widoku GridView

Zanim firma Microsoft dotyczą nad z tworzeniem Wstawianie interfejsu w stopce s widoku GridView chętnie s pierwszy fokus na dodawanie widoku GridView do strony, która zawiera listę produktów w bazie danych. Uruchamianie przez otwarcie `InsertThroughFooter.aspx` strony `EnhancedGridView` folder i przeciągnij element GridView z przybornika do projektanta, ustawienie GridView s `ID` właściwości `Products`. Następnie użyj tagów inteligentnych s GridView, aby powiązać go z nowego elementu ObjectDataSource o nazwie `ProductsDataSource`.


[![Utwórz nowy element ObjectDataSource o nazwie ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**Rysunek 2**: Utwórz nowy składnik o nazwie ObjectDataSource `ProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))


Element ObjectDataSource umożliwia konfigurowanie `ProductsBLL` klasy s `GetProducts()` metody do pobierania informacji o produkcie. W tym samouczku pozwalają fokus s wyłącznie na dodawanie Wstawianie funkcji i nie martw się o edytowanie i usuwanie. W związku z tym, upewnij się, że listy rozwijanej na karcie Wstawianie jest ustawiona na `AddProduct()` i że list rozwijanych w kartach UPDATE i DELETE są ustawione na (Brak).


[![Mapa metody AddProduct do metody operacji Insert() s ObjectDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**Rysunek 3**: mapy `AddProduct` metodę ObjectDataSource s `Insert()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))


[![Ustawianie list rozwijanych UPDATE i DELETE karty na (Brak)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**Rysunek 4**: Ustaw aktualizacji i usuwania listy rozwijanej karty na (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))


Po zakończeniu pracy kreatora skonfiguruj źródło danych s ObjectDataSource, Visual Studio spowoduje automatyczne dodanie pola do widoku GridView dla każdego z odpowiednich pól danych. Na razie pozostaw wszystkie pola dodane przez program Visual Studio. W dalszej części tego samouczka, firma Microsoft będzie wrócić i usunąć niektóre pola, których wartości ADAM t muszą być określone podczas dodawania nowego rekordu.

Ponieważ blisko 80 produktów w bazie danych, użytkownik będzie musiał przewiń w dół do dolnej części strony sieci web, aby dodać nowy rekord. W związku z tym pozwolić, aby włączyć stronicowanie bardziej widoczne i dostępny Wstawianie interfejsu s. Aby włączyć stronicowanie, po prostu zaznacz pole wyboru Włącz stronicowanie z tagów inteligentnych s widoku GridView.

W tym momencie znaczników deklaratywne s GridView i ObjectDataSource powinien wyglądać podobnie do następującego:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![Wszystkie pola danych produktu są wyświetlane w widoku GridView stronicowanej](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**Rysunek 5**: wszystkie pola danych produktu są wyświetlane w widoku GridView stronicowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>Krok 2: Dodawanie wiersza stopki

Wraz z jego nagłówka i wiersze danych widoku GridView zawiera wiersz stopki. Wiersze nagłówków i stopek są wyświetlane w zależności od wartości GridView s [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) i [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) właściwości. Aby wyświetlić wiersz stopki, po prostu ustaw `ShowFooter` właściwości `True`. Jak pokazano w rysunek 6, ustawienie `ShowFooter` właściwości `True` dodaje stopki do siatki.


[![Aby wyświetlić wiersz stopki, ustaw ShowFooter wartość True](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**Rysunek 6**: Ustaw wiersz stopki do wyświetlenia `ShowFooter` do `True` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))


Należy pamiętać, że wiersz stopki zawiera kolor tła red ciemny. Jest to spowodowane motywu DataWebControls, możemy utworzyć i stosowane do wszystkich stron w [wyświetlanie danych z ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) samouczka. W szczególności `GridView.skin` konfiguruje pliku `FooterStyle` właściwość takie używa `FooterStyle` klasę CSS. `FooterStyle` Klasa jest zdefiniowana w `Styles.css` w następujący sposób:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> Firma Microsoft kolejnych zbadane, korzystania z widoku GridView s stopki wiersza w poprzednim samouczki. Jeśli to konieczne, odwołaj się do [wyświetlanie informacji podsumowania w stopce w widoku GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) samouczka dla odświeżacz.


Po ustawieniu `ShowFooter` właściwości `True`, Poświęć chwilę, aby wyświetlić dane wyjściowe w przeglądarce. Obecnie t wiersza stopki zawierać tekst i formantów sieci Web. W kroku 3 Firma Microsoft będzie zmodyfikować stopkę dla każdego pola w widoku GridView co zawierają one odpowiedni interfejs Wstawianie.


[![Wiersz Pusta stopka jest wyświetlone powyżej stronicowania formantów interfejsu](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**Rysunek 7**: Stopka pusty wiersz jest wyświetlone powyżej stronicowania formantów interfejsu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>Krok 3: Dostosowywanie wiersz stopki

W [przy użyciu TemplateFields w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) samouczek widzieliśmy znacznie dostosowywania wyświetlania określonej kolumny widoku GridView przy użyciu TemplateFields (w przeciwieństwie BoundFields lub CheckBoxFields); w [ Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) analizujemy przy użyciu TemplateFields do dostosowania interfejsu edycji w widoku GridView. Odwołanie składa się z liczbą szablony definiuje kombinację znaczników, formantów sieci Web i składnia wiązania z danymi TemplateField używaną do pewnych typów wierszy. `ItemTemplate`, Na przykład Określa szablon używany do wierszy tylko do odczytu, podczas gdy `EditItemTemplate` definiuje szablonu można edytować wiersza.

Wraz z programem `ItemTemplate` i `EditItemTemplate`, obejmuje także TemplateField `FooterTemplate` zawartości dla stopki wiersza, który określa. W związku z tym można dodawać formanty sieci Web wymagane dla każdego pola s Wstawianie interfejs do `FooterTemplate`. Aby rozpocząć, przekonwertować wszystkie pola w widoku GridView TemplateFields. Można to zrobić kliknięcie łącza Edytuj kolumny w widoku GridView s tagu, wybierając każdego pola w lewym dolnym rogu i kliknięcie przycisku Konwertuj to pole na pole TemplateField łącze.


![Konwertuj każde pole na pole TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**Rysunek 8**: konwertowanie każde pole na pole TemplateField


Kliknięcie przycisku Konwertuj to pole na pole TemplateField włącza bieżącego typu pola w TemplateField równoważne. Na przykład każdego elementu BoundField zastępuje TemplateField z `ItemTemplate` zawiera etykietę, która zawiera odpowiednie pole danych i `EditItemTemplate` który wyświetla pole danych w polu tekstowym. `ProductName` Elementu BoundField został przekonwertowany na następujący kod znaczników TemplateField:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

Podobnie `Discontinued` CheckBoxField został przekonwertowany na pole TemplateField którego `ItemTemplate` i `EditItemTemplate` zawierać formant wyboru sieci Web (o `ItemTemplate` s wyłączone pole wyboru). Tylko do odczytu `ProductID` elementu BoundField został przekonwertowany na pole TemplateField za pomocą formantu etykiety w obu `ItemTemplate` i `EditItemTemplate`. Krótko mówiąc pole na pole TemplateField Konwertowanie istniejącego widoku GridView jest szybki i łatwy sposób przełączyć się do więcej można dostosowywać TemplateField bez utraty istniejących funkcjonalności pola s.

Ponieważ w widoku GridView możemy odnośnie do pracy z edycji Obsługa t możesz usunąć `EditItemTemplate` z każdym TemplateField, pozostawiając tylko `ItemTemplate`. Po wykonaniu tej czynności deklaratywne GridView znaczników w s powinna wyglądać następująco:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

Teraz, gdy każdego pola w widoku GridView został przekonwertowany na pole TemplateField, możemy wprowadź odpowiedni interfejs wstawianie do każdego pola s `FooterTemplate`. Niektóre pola nie zostaną Wstawianie interfejsu (`ProductID`, na przykład); inne osoby będą się różnić w formantach sieci Web, używane do zbierania informacji o produkcie s nowe.

Aby utworzyć interfejs edytowania, wybierz link Edytuj szablony z tagów inteligentnych s widoku GridView. Z listy rozwijanej wybierz odpowiednie pole s `FooterTemplate` i przeciągnij odpowiednie formant z przybornika do projektanta.


[![Dodaj odpowiedni interfejs wstawianie do elementu FooterTemplate każdego pola s](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**Rysunek 9**: dodać odpowiedni interfejs wstawianie do każdego pola s `FooterTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))


Poniższa lista punktowana wylicza pola widoku GridView Określanie Wstawianie interfejsu do dodania:

- `ProductID`Brak.
- `ProductName`Dodaj pole tekstowe i ustaw jego `ID` do `NewProductName`. Dodawanie formantu RequiredFieldValidator również, aby upewnić się, że użytkownik wprowadza wartość dla nowej nazwy produktu s.
- `SupplierID`Brak.
- `CategoryID`Brak.
- `QuantityPerUnit`Dodaj pole tekstowe, ustawienie jej `ID` do `NewQuantityPerUnit`.
- `UnitPrice`Dodaj pole tekstowe o nazwie `NewUnitPrice` i CompareValidator, które powodują wprowadzona wartość jest większa lub równa zero wartości waluty.
- `UnitsInStock`Zastosuj pole tekstowe, którego `ID` ma ustawioną wartość `NewUnitsInStock`. Obejmują CompareValidator, który zapewnia, że wprowadzona wartość jest wartość całkowita większa lub równa zero.
- `UnitsOnOrder`Zastosuj pole tekstowe, którego `ID` ma ustawioną wartość `NewUnitsOnOrder`. Obejmują CompareValidator, który zapewnia, że wprowadzona wartość jest wartość całkowita większa lub równa zero.
- `ReorderLevel`Zastosuj pole tekstowe, którego `ID` ma ustawioną wartość `NewReorderLevel`. Obejmują CompareValidator, który zapewnia, że wprowadzona wartość jest wartość całkowita większa lub równa zero.
- `Discontinued`pola wyboru, ustawienie jej `ID` do `NewDiscontinued`.
- `CategoryName`Dodaj DropDownList i ustaw jej `ID` do `NewCategoryID`. Powiązać go z nowego elementu ObjectDataSource o nazwie `CategoriesDataSource` i skonfigurować go do używania `CategoriesBLL` klasy s `GetCategories()` metody. Ma DropDownList s `ListItem` wyświetlania s `CategoryName` danych pola `CategoryID` pola danych jako ich wartości.
- `SupplierName`Dodaj DropDownList i ustaw jej `ID` do `NewSupplierID`. Powiązać go z nowego elementu ObjectDataSource o nazwie `SuppliersDataSource` i skonfigurować go do używania `SuppliersBLL` klasy s `GetSuppliers()` metody. Ma DropDownList s `ListItem` wyświetlania s `CompanyName` danych pola `SupplierID` pola danych jako ich wartości.

Dla każdego z formanty walidacji wyczyszczenie `ForeColor` właściwości, aby `FooterStyle` kolor biały klasy s CSS będzie używany zamiast domyślnego czerwony. Również użyć `ErrorMessage` właściwość szczegółowy opis, ale wartość `Text` właściwości gwiazdkę. Aby zapobiec tekst formantu s sprawdzania poprawności spowodował Wstawianie interfejsu, który ma być zawijany do dwóch wierszy, ustaw `FooterStyle` s `Wrap` właściwości na wartość false dla każdego `FooterTemplate` s, który formant sprawdzania poprawności. Na koniec Dodaj formant ValidationSummary poniżej widoku GridView i ustaw jej `ShowMessageBox` właściwości `True` i jego `ShowSummary` właściwości `False`.

Podczas dodawania nowego produktu, należy podać `CategoryID` i `SupplierID`. Te informacje są przechwytywane przez DropDownLists w komórkach stopkę dla `CategoryName` i `SupplierName` pól. Został wybrany do użycia tych pól, w przeciwieństwie do `CategoryID` i `SupplierID` TemplateFields, ponieważ w siatce s wiersze danych, użytkownik jest prawdopodobnie bardziej interesują wyświetlanie nazwy kategorii i dostawcy, a nie ich wartości Identyfikatora. Ponieważ `CategoryID` i `SupplierID` teraz przechwytywanych wartości w `CategoryName` i `SupplierName` pola s Wstawianie interfejsów usunąć `CategoryID` i `SupplierID` TemplateFields z widoku GridView.

Podobnie `ProductID` nie jest używany podczas dodawania nowego produktu, więc `ProductID` TemplateField można również usunąć. Jednak umożliwia s pozostaw `ProductID` w siatce. Oprócz pól tekstowych, DropDownLists, pola wyboru i formanty walidacji, wchodzące w skład Wstawianie interfejsu, poprosimy Cię o również Dodawanie przycisku, po kliknięciu wykonuje logiki można dodać nowego produktu do bazy danych. W kroku 4 możemy obejmują przycisk Dodaj w interfejsie wstawianie `ProductID` TemplateField s `FooterTemplate`.

Możesz także poprawić wygląd pola widoku GridView. Na przykład możesz chcieć formatu `UnitPrice` wartości waluty, wyrównanie do prawej `UnitsInStock`, `UnitsOnOrder`, i `ReorderLevel` pól i aktualizacji `HeaderText` wartości TemplateFields.

Po utworzeniu slew wstawiania interfejsy `FooterTemplate` s, usuwanie `SupplierID`, i `CategoryID` TemplateFields i poprawia estetykę siatki za pośrednictwem formatowanie i wyrównywanie TemplateFields Twojego deklaratywne s widoku GridView Kod znaczników powinien wyglądać podobnie do następującego:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

Podczas wyświetlania za pośrednictwem przeglądarki, wiersz stopki s GridView zawiera teraz ukończonej Wstawianie interfejsu (zobacz rysunek 10). W tym momencie Wstawianie t interfejsu obejmują oznacza dla użytkownika wskazać, że s tych wprowadzonych danych dotyczących nowego produktu i chce wstawić nowy rekord w bazie danych. Ponadto firma Microsoft Przenieś jeszcze do rozwiązania, jak dane wprowadzane w stopce przekształci się na nowy rekord w `Products` bazy danych. W kroku 4 wyjaśniono sposób obejmują przycisk Dodaj, aby Wstawianie interfejsu i wykonanie kodu na ogłaszanie, gdy jego s kliknięty. Krok 5 pokazano, jak wstawić nowy rekord przy użyciu danych z stopki.


[![Stopka GridView udostępnia interfejs dla dodawania nowego rekordu](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**Na rysunku nr 10**: Stopka GridView udostępnia interfejs dla dodawania nowego rekordu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Krok 4: W tym przycisk Dodaj w interfejsie Wstawianie

Musimy dołączyć przycisku Dodaj gdzieś w interfejsie Wstawianie ponieważ s wiersza stopki Wstawianie interfejsu obecnie nie ma oznacza, że dla użytkownika wskazać, że zakończyły się wprowadzania nowych informacji o produkcie s. Może to być umieszczone w jeden z istniejących `FooterTemplate` s lub możemy można dodać nowej kolumny do tabeli w tym celu. W tym samouczku let s umieścić przycisk Dodaj w `ProductID` TemplateField s `FooterTemplate`.

Przy użyciu projektanta, kliknij łącze Edytuj szablony w widoku GridView tag inteligentny s, a następnie wybierz pozycję `ProductID` pola s `FooterTemplate` z listy rozwijanej. Dodawanie formantu przycisku sieci Web (lub LinkButton lub ImageButton, jeśli wolisz) do szablonu, ustawienie jej identyfikator na `AddProduct`, jego `CommandName` do wstawienia, a jego `Text` właściwość do dodania, jak pokazano na rysunku nr 11.


[![Umieść przycisk Dodaj w elementu FooterTemplate s ProductID TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**Rysunek 11**: umieść przycisk Dodaj w `ProductID` TemplateField s `FooterTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))


Gdy był dołączony przycisk Dodaj, należy przetestować strony w przeglądarce. Należy pamiętać, że po kliknięciu przycisku Dodaj z nieprawidłowe dane w interfejsie Wstawianie ogłaszania zwrotnego jest krótko circuited oraz formant ValidationSummary wskazuje nieprawidłowe dane (patrz rysunek 12). Z odpowiednią wprowadzone dane klikając przycisk Dodaj powoduje odświeżenie strony. Żaden rekord jest jednak dodane do bazy danych. Firma Microsoft będzie konieczne napisanie nieco kod, aby dokonać insert.


[![Dodawanie przycisku s ogłaszania zwrotnego wynosi krótki Circuited nieprawidłowe dane w interfejsie Wstawianie](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**Rysunek 12**: Dodawanie przycisku s ogłaszania zwrotnego wynosi krótki Circuited nieprawidłowe dane w interfejsie Wstawianie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))


> [!NOTE]
> Formanty walidacji w interfejsie Wstawianie nie zostały przypisane do grupy sprawdzania poprawności. Dobrze działa tak długo, jak wstawianie interfejs jest tylko zestaw formanty walidacji na tej stronie. Jeśli istnieją inne formanty walidacji na stronie (np. formanty walidacji w interfejsie edycji siatki s), formanty walidacji w Wstawianie interfejsu i Dodaj s przycisk `ValidationGroup` można przypisać właściwości taką samą wartość tak, aby skojarzenia tych kontrolek z grupy określonej sprawdzania poprawności. Zobacz [pincety formanty walidacji w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) uzyskać więcej informacji na temat partycjonowania przyciski strony i formanty walidacji w grupach sprawdzania poprawności.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Krok 5: Wstawianie nowych rekordów do`Products`tabeli

Przy użyciu wbudowanych funkcji edytowania GridView, widoku GridView automatycznie obsługuje wszystkie niezbędne do wykonywania aktualizacji pracy. W szczególności, po kliknięciu przycisku Aktualizuj są kopiowane wartości wprowadzone w interfejsie edycji do parametrów w elemencie ObjectDataSource s `UpdateParameters` kolekcji i kopnięć poza aktualizacji przez element ObjectDataSource s `Update()` metody. Ponieważ w widoku GridView nie zapewnia takiego wbudowanej funkcji wstawiania, musimy zaimplementować kod, który wywołuje element ObjectDataSource s `Insert()` — metoda i kopiuje wartości z Wstawianie interfejsu s ObjectDataSource `InsertParameters` kolekcji .

Tę logikę wstawiania powinien być wykonywany po kliknięciu przycisku Dodaj. Zgodnie z opisem w [Dodawanie i reagowanie na przyciski w widoku GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md) samouczek, w dowolnym momencie przycisk, LinkButton lub ImageButton w widoku GridView zostanie kliknięty, GridView s `RowCommand` uruchamiany zdarzenia odświeżania strony. To zdarzenie jest generowane czy przycisku, LinkButton lub ImageButton dodano jawnie takich jak przycisk Dodaj w wierszu stopki, lub jeśli została automatycznie dodana przez widoku GridView (takie jak LinkButtons na początku każdej kolumny, gdy zaznaczono pole wyboru Włącz sortowanie, lub LinkButtons w interfejsie stronicowania po wybraniu włączenia stronicowania).

W związku z tym, aby odpowiedzieć użytkownik, klikając przycisk Dodaj, należy utworzyć programu obsługi zdarzeń dla widoku GridView s `RowCommand` zdarzeń. Ponieważ to zdarzenie jest generowane po każdej zmianie *żadnych* po kliknięciu przycisku, LinkButton lub ImageButton w widoku GridView go s istotne, że będziemy kontynuować, tylko z logiką wstawianie `CommandName` właściwości przekazany do mapowania programu obsługi zdarzeń do `CommandName` wartość przycisk Dodaj (Wstaw). Ponadto firma Microsoft również kontynuować, tylko formanty walidacji raport prawidłowe dane. Aby to umożliwić, należy utworzyć programu obsługi zdarzeń dla `RowCommand` zdarzeń z następującym kodem:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> Możesz się zastanawiać, dlaczego program obsługi zdarzeń bothers sprawdzanie `Page.IsValid` właściwości. Po wszystkich wykorzystanej t ogłaszania zwrotnego można pominąć, jeśli podano nieprawidłowe dane w interfejsie Wstawianie? To założenie jest poprawna, dopóki użytkownik nie ma wyłączone JavaScript lub podjęła kroki obejścia logikę weryfikacji po stronie klienta. Krótko mówiąc jeden nigdy nie będą miały ściśle weryfikacji po stronie klienta; sprawdzenia poprawności po stronie serwera powinny być zawsze wykonywane przed rozpoczęciem pracy z danymi.


W kroku 1 utworzyliśmy `ProductsDataSource` ObjectDataSource tak, aby jego `Insert()` metody jest mapowany na `ProductsBLL` klasy s `AddProduct` metody. Aby wstawić nowy rekord w `Products` tabeli, firma Microsoft może po prostu wywołać ObjectDataSource s `Insert()` metody:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

Teraz, gdy `Insert()` metoda została wywołana, wszystkie punkty, które pozostaje ma na celu Kopiowanie wartości z interfejsu Wstawianie parametry przekazywane do `ProductsBLL` klasy s `AddProduct` metody. Jak widzieliśmy w [badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) samouczek, można to zrobić przez element ObjectDataSource s `Inserting` zdarzeń. W `Inserting` zdarzenia musimy programowo odwołują się do formantów z `Products` stopki s GridView wiersz i przypisać wartości do `e.InputParameters` kolekcji. Jeśli użytkownik pomija wartości, takich jak pozostawienie `ReorderLevel` puste pole tekstowe, należy określić wartość wstawione do bazy danych należy `NULL`. Ponieważ `AddProducts` metoda przyjmuje typy dopuszczające wartości null dla pola wartość null bazy danych, po prostu użyj typu dopuszczającego wartość null i ustaw dla niego wartość `Nothing` w przypadku, gdy zostanie pominięty danych wejściowych użytkownika.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

Z `Inserting` program obsługi zdarzeń jest zakończone, nowe rekordy można dodać do `Products` tabeli bazy danych za pośrednictwem widoku GridView s stopki wiersza. Przejdź dalej i spróbuj dodać kilka nowych produktów.

## <a name="enhancing-and-customizing-the-add-operation"></a>Rozszerzanie i dostosowywanie operacji dodawania

Obecnie w kliknięcie przycisku Dodaj dodaje nowego rekordu do tabeli bazy danych, ale nie zapewnia dowolny rodzaj wizualne czy rekord został pomyślnie dodany. W idealnym przypadku Web etykiety formantu lub po stronie klienta alertu okno może poinformować użytkownika o ich wstawiania została ukończona pomyślnie. I pozostaw to wykonywania dla czytnika.

GridView używane w tym samouczku nie ma zastosowania do listy produktów dowolnej kolejności sortowania ani pozwala użytkownikom końcowym posortuj dane. W rezultacie rekordy są uporządkowane są one w bazie danych przez ich klucz podstawowy. Ponieważ każdego nowego rekordu ma `ProductID` wartość większą niż ostatni, każdym nowym produktem dodaje się go jest przyczepiana końcu elementu siatki. W związku z tym można automatycznie wysłać do użytkownika do ostatniej strony GridView po dodaniu nowego rekordu. Można to osiągnąć, dodając następujący wiersz kodu po wywołaniu `ProductsDataSource.Insert()` w `RowCommand` obsługi zdarzeń, aby wskazać, że użytkownik potrzebuje do wysłania do ostatniej strony po powiązaniu danych widoku GridView:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage`jest zmienną wartości logicznej na poziomie strony, która jest początkowo przypisana wartość `False`. W widoku GridView s `DataBound` program obsługi zdarzeń, jeśli `SendUserToLastPage` ma wartość false, `PageIndex` właściwości jest aktualizowana w celu wysyłania użytkownika do ostatniej strony.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

Powód `PageIndex` właściwość jest ustawiona `DataBound` obsługi zdarzeń (w przeciwieństwie do `RowCommand` obsługi zdarzeń) jest, ponieważ podczas `RowCommand` obsługi zdarzenia generowane, firma Microsoft Przenieś jeszcze do dodawania nowego rekordu `Products` tabeli bazy danych. W związku z tym w `RowCommand` obsługi zdarzeń ostatniego indeksu strony (`PageCount - 1`) reprezentuje ostatni indeks strony *przed* dodano nowego produktu. W większości produktów dodawany ostatniego indeksu strony jest taka sama po dodaniu nowego produktu. Gdy dodanego produktu powoduje ostatniego nowego indeksu strony, niepoprawnie aktualizacji, ale `PageIndex` w `RowCommand` obsługi zdarzeń, a następnie możemy spowoduje przekierowanie do drugiego do ostatniej strony (ostatni indeksu strony przed dodaniem nowego produktu) zamiast nowego ostatniej strony i ndex. Ponieważ `DataBound` obsługi zdarzenia generowane po dodaniu nowego produktu i dane odbitych do siatki, ustawiając `PageIndex` właściwości wiemy, możemy re poprawne ostatniego indeksu strony pobierania.

Na koniec widoku GridView używane w tym samouczku jest dość szeroka, ze względu na liczbę pól, które należy zebrać dodawania nowego produktu. Ze względu na to szerokość może być preferowany układ pionowy s widoku DetailsView. GridView s całkowita szerokość może zostać zmniejszona zbierając mniej danych wejściowych. Możliwe, że firma Microsoft ADAM potrzeba t gromadzenia `UnitsOnOrder`, `UnitsInStock`, i `ReorderLevel` pól podczas dodawania nowego produktu, w tym przypadku te pola można usunąć z widoku GridView.

Aby dostosować zebranych danych, możemy użyć jednej z dwóch metod:

- Nadal używać `AddProduct` metodę, która oczekuje wartości dla `UnitsOnOrder`, `UnitsInStock`, i `ReorderLevel` pól. W `Inserting` program obsługi zdarzeń, podaj stałe, domyślne wartości dla tych danych wejściowych, które zostały usunięte z Wstawianie interfejsu.
- Utwórz nowy przeciążenia `AddProduct` metody w `ProductsBLL` klasy, która nie akceptuje dane wejściowe dla `UnitsOnOrder`, `UnitsInStock`, i `ReorderLevel` pól. Następnie na stronie ASP.NET skonfiguruj ObjectDataSource do używania tego nowego przeciążenia.

Jedną z opcji będzie działać równie również. W przeszłości samouczki użyliśmy tę druga opcję tworzenia wielu przeciążeń dla `ProductsBLL` klasy s `UpdateProduct` metody.

## <a name="summary"></a>Podsumowanie

Widoku GridView nie ma wbudowane funkcje Wstawianie znalezionych w widoku DetailsView i FormView, ale z bitowego nakładu Wstawianie interfejs mogą być dodawane do stopki wiersza. W celu wyświetlenia stopki wiersza w widoku GridView wystarczy ustawić jej `ShowFooter` właściwości `True`. Zawartości wiersza stopki można dostosować dla każdego pola konwertując pole na pole TemplateField i dodawanie Wstawianie interfejsu do `FooterTemplate`. Jak widzieliśmy w tym samouczku `FooterTemplate` może zawierać przycisków, pól tekstowych DropDownLists, pola wyboru, kontrolki źródła danych do zapełniania opartych na danych formantów sieci Web (na przykład DropDownLists) i formanty walidacji. Wraz z służy do zbierania danych wejściowych użytkownika s wymagany jest przycisk Dodaj, LinkButton lub ImageButton.

Po kliknięciu przycisku Dodaj, ObjectDataSource s `Insert()` metoda wywoływana w celu uruchomienia Wstawianie przepływu pracy. ObjectDataSource zostanie następnie wywołaj metodę insert skonfigurowany ( `ProductsBLL` klasy s `AddProduct` metody, w tym samouczku). Firma Microsoft należy skopiować wartości z s GridView Wstawianie interfejsu s ObjectDataSource `InsertParameters` kolekcji przed wywoływana metoda insert. Można to zrobić przez odwołanie programowane wstawianie formantów sieci Web interfejsu w elemencie ObjectDataSource s `Inserting` program obsługi zdarzeń.

W tym samouczku wykonuje naszych przyjrzeć techniki wzmocnienia wygląd s widoku GridView. Następnego zestawu samouczki zbada, jak pracować z danych binarnych, takich jak obrazów, plików PDF, dokumentów programu Word i tak dalej i dane formantów sieci Web.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Bernadette Leigh. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](adding-a-gridview-column-of-checkboxes-vb.md)
