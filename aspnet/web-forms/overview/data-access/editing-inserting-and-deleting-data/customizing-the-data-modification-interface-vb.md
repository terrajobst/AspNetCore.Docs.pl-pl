---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: Dostosowywanie interfejsu modyfikacji danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: "W tym samouczku wyjaśniono, jak do dostosowania interfejsu można edytować w widoku GridView, zastępując standardowe pole tekstowe i pole wyboru kontrolki z Adres alternaty..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 4f8023efd4d0b32e81dd3aab70e6e7521066fc84
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="customizing-the-data-modification-interface-vb"></a>Dostosowywanie interfejsu modyfikacji danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe) lub [pobierania plików PDF](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> W tym samouczku wyjaśniono, jak do dostosowania interfejsu można edytować w widoku GridView, zastępując standardowe pole tekstowe i pole wyboru formanty z alternatywnych wejściowych formantów sieci Web.


## <a name="introduction"></a>Wprowadzenie

BoundFields i CheckBoxFields używany przez formant widoku GridView i widoku DetailsView uprościć proces modyfikowania danych ze względu na możliwość renderowania tylko do odczytu, można edytować i insertable interfejsów. Te interfejsy mogą być renderowane bez konieczności dodawania dodatkowych deklaratywne znaczników ani kodu innych. Jednak interfejsów elementu BoundField i w polu CheckBoxField brakuje dostosowywalności często potrzebne w rzeczywistych scenariuszach. Aby dostosować interfejsu można edytować lub insertable w widoku GridView lub należy zamiast tego użyć TemplateField widoku DetailsView.

W [poprzedniego samouczek](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) widzieliśmy sposobu dostosowywania interfejsy modyfikacji danych przez dodawanie formantów sieci Web sprawdzania poprawności. W tym samouczku wyjaśniono sposób dostosowywania kolekcji formantów sieci Web rzeczywiste dane, zastępowanie elementu BoundField i w polu CheckBoxField standardowe pole tekstowe i formantów CheckBox z alternatywnych wejściowych formantów sieci Web. W szczególności firma Microsoft będzie kompilacji można edytować widoku GridView umożliwiający produktu nazwa, Kategoria dostawcy i wycofane stanu aktualizacji. Podczas edytowania określonego wiersza, pola kategorii i dostawcy będą zawierały jako DropDownLists, zawierającą zestaw dostępnych kategorii i dostawców do wyboru. Ponadto firma Microsoft będzie zastąpić domyślne polu CheckBoxField wyboru RadioButtonList formant, który oferuje dwie opcje: "Zaprzestać" i "Active".


[![Interfejs edycji w widoku GridView zawiera DropDownLists i elementami RadioButton](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**Rysunek 1**: edytowanie DropDownLists obejmuje interfejs i elementami RadioButton GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Krok 1: Tworzenie odpowiednie`UpdateProduct`przeciążenia

W tym samouczku budujemy można edytować umożliwia edytowanie nazwy produktu, kategoria, dostawca, którą przerywane, stan widoku GridView. Dlatego potrzebujemy `UpdateProduct` przeciążenia, które akceptuje pięć parametrów wejściowych te wartości czterech produktu oraz `ProductID`. W naszym poprzedniej przeciążeń, spowoduje to jeden, takich jak:

1. Pobieranie informacji o produkcie z bazy danych dla określonego `ProductID`,
2. Aktualizacja `ProductName`, `CategoryID`, `SupplierID`, i `Discontinued` , pól i
3. Wyślij żądanie aktualizacji z warstwą dal za pomocą TableAdapter `Update()` metody.

Jednak dla tego konkretnego przeciążenia I został pominięty wyboru reguły biznesowe, które zapewnia, że produkt jest oznaczony jako zaprzestać nie jest tylko produktu oferowanej przez dostawcę. Działanie także dodać go, jeśli wolisz lub, najlepiej, jeśli zrefaktoryzuj limit logiki do oddzielnych metodach.

Poniższy kod przedstawia nowe `UpdateProduct` przeciążenia w `ProductsBLL` klasy:


[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>Krok 2: Obsługuje tworzenie można edytować widoku GridView

Z `UpdateProduct` dodany przeciążenia, jest gotowy do utworzenia naszych można edytować widoku GridView. Otwórz `CustomizedUI.aspx` strony `EditInsertDelete` folderu i Dodaj do projektanta kontrolce GridView. Następnie należy utworzyć nowy element ObjectDataSource z tagów inteligentnych w widoku GridView. Skonfiguruj ObjectDataSource można pobrać informacji o produkcie za pośrednictwem `ProductBLL` klasy `GetProducts()` — metoda i zaktualizować produkt za pomocą danych `UpdateProduct` przeciążenia właśnie utworzyliśmy. Na kartach INSERT i DELETE wybierz z listy rozwijanej (Brak).


[![Skonfiguruj ObjectDataSource do użycia przeciążenia UpdateProduct właśnie utworzony](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**Rysunek 2**: Konfigurowanie ObjectDataSource użyć `UpdateProduct` przeciążenia właśnie utworzony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image6.png))


Jak możemy przedstawiono w całym samouczki modyfikacji danych, deklaratywne składnia ObjectDataSource utworzony przez program Visual Studio przypisuje `OldValuesParameterFormatString` właściwości `original_{0}`. To oczywiście nie będzie działać z naszych warstwy logiki biznesowej od naszych metod oczekują, że oryginalne `ProductID` wartości, należy przesłać. W związku z tym, jak firma Microsoft wykonane w poprzednim samouczki, Poświęć chwilę, aby usunąć to przypisanie właściwości z składni deklaratywnej, lub należy ustawić wartość tej właściwości na `{0}`.

Po tej zmianie znaczników deklaracyjne element ObjectDataSource powinna wyglądać następująco:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

Należy pamiętać, że `OldValuesParameterFormatString` właściwości zostały usunięte oraz że ma `Parameter` w `UpdateParameters` kolekcji dla wszystkich parametrów wejściowych oczekiwany przez naszych `UpdateProduct` przeciążenia.

Gdy element ObjectDataSource jest skonfigurowany do aktualizacji tylko podzestaw wartości produktu, widoku GridView obecnie zawiera *wszystkie* pól produktu. Poświęć chwilę, aby edytować widoku GridView, aby:

- Tylko obejmuje `ProductName`, `SupplierName`, `CategoryName` BoundFields i `Discontinued` CheckBoxField
- `CategoryName` i `SupplierName` pól do wyświetlenia przed (po lewej) `Discontinued` CheckBoxField
- `CategoryName` i `SupplierName` BoundFields `HeaderText` właściwość ma wartość "Kategorii" i "Dostawca", odpowiednio
- Obsługa Edycja jest włączona (pole wyboru Włącz edytowanie w tagu w widoku GridView)

Po wprowadzeniu tych zmian z pokazanej poniżej składni deklaratywnej w widoku GridView będzie wyglądać podobnie do rysunku 3 projektanta.


[![Usunąć niepotrzebne pola z widoku GridView](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**Rysunek 3**: usunąć niepotrzebne pola z widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

W tym momencie zachowanie tylko do odczytu w widoku GridView zostanie zakończona. Podczas przeglądania danych, każdy z produktów jest renderowane jako wiersza w widoku GridView przedstawiający nazwę produktu, kategoria, dostawca i zaprzestać stanu.


[![Interfejs tylko do odczytu w widoku GridView jest pełna](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**Rysunek 4**: interfejs tylko do odczytu w widoku GridView jest pełna ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image12.png))


> [!NOTE]
> Zgodnie z opisem w [samouczek omówienie Wstawianie, aktualizowanie i usuwanie danych](an-overview-of-inserting-updating-and-deleting-data-cs.md), jest zasadnicze znaczenie, że GridView s stan widoku jest włączone (domyślnie). Jeśli ustawisz GridView s `EnableViewState` właściwości `false`, uruchom ryzyko równoczesnych użytkowników przypadkowo usunięcie lub edytowania rekordów. Zobacz [Ostrzeżenie: problem współbieżności z ASP.NET 2.0 GridViews/widoku DetailsView/FormViews tej edycji pomocy technicznej i/lub usunięcie i Whose stan widoku jest wyłączona](http://scottonwriting.net/sowblog/posts/10054.aspx) Aby uzyskać więcej informacji.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Krok 3: Przy użyciu DropDownList dla kategorii i dostawcy edycji interfejsów

Odwołania, który `ProductsRow` zawiera obiekt `CategoryID`, `CategoryName`, `SupplierID`, i `SupplierName` właściwości, które zawierają rzeczywiste wartości Identyfikatora klucza obcego w `Products` bazy danych tabeli i odpowiadający mu `Name` wartości w `Categories` i `Suppliers` tabel. `ProductRow`w `CategoryID` i `SupplierID` może jednocześnie być odczytywane i zapisywane w trakcie `CategoryName` i `SupplierName` właściwości są oznaczone jako tylko do odczytu.

Z powodu stanu tylko do odczytu `CategoryName` i `SupplierName` właściwości miały odpowiednich BoundFields ich `ReadOnly` ustawioną właściwość `True`, uniemożliwia te wartości są modyfikowane, gdy wiersz jest edytowany. Gdy firma Microsoft może ustawić `ReadOnly` właściwości `False`, renderowanie `CategoryName` i `SupplierName` BoundFields jako pola tekstowe podczas edytowania, takie podejście spowoduje Wystąpił wyjątek podczas próby zaktualizowania produktu, ponieważ istnieje nie `UpdateProduct` przeciążenia przyjmującego `CategoryName` i `SupplierName` danych wejściowych. W rzeczywistości nie chcemy utworzyć takie przeciążenia dwóch powodów:

- `Products` Tabela nie ma `SupplierName` lub `CategoryName` pól, ale `SupplierID` i `CategoryID`. W związku z tym chcemy naszych metody do przekazania tych określonych wartości Identyfikatora nie ich tabele odnośników wartości.
- Użytkownik musi wpisz nazwę dostawcy lub kategorii doskonale poniżej, ponieważ wymaga ona użytkownikowi dostępne kategorie i dostawców i ich prawidłowe pisowni.

Pola kategorii i dostawcy powinien być wyświetlany kategorii i nazwy dostawców w trybie tylko do odczytu (jak teraz) i listy rozwijanej odpowiednich opcji podczas edycji. Za pomocą listy rozwijanej, użytkownik końcowy umożliwia szybkie sprawdzenie, jakie kategorie i dostawców są dostępne do wyboru spośród i więcej łatwo wprowadzać ich zaznaczenie.

Aby zapewnić to zachowanie, musimy przekonwertować `SupplierName` i `CategoryName` BoundFields do TemplateFields których `ItemTemplate` emituje `SupplierName` i `CategoryName` wartości i których `EditItemTemplate` używa kontrolki DropDownList, do listy Dostępne kategorie i dostawców.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Dodawanie`Categories`i`Suppliers`DropDownLists

Rozpocznij poprzez konwersję `SupplierName` i `CategoryName` BoundFields do TemplateFields przez: kliknięcie łącza Edytuj kolumny w widoku GridView tagów inteligentnych; wybranie elementu BoundField z listy w lewym dolnym; i klikając pozycję "Konwertuj to pole do Łącze TemplateField". Proces konwersji utworzy TemplateField zarówno `ItemTemplate` i `EditItemTemplate`, jak pokazano w poniższej składni deklaratywnej:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

Ponieważ elementu BoundField została oznaczona jako tylko do odczytu, zarówno `ItemTemplate` i `EditItemTemplate` zawierają Web etykiety kontroli, których `Text` właściwość jest powiązana z polem danych dotyczy (`CategoryName`, w powyższej przykładowej składni). Należy zmodyfikować `EditItemTemplate`, zastępując kontrolka sieci Web etykiety formantu DropDownList.

Jak możemy przedstawiono w poprzednim samouczki, poprzez projektanta, czy bezpośrednio ze składni deklaratywnej można edytować szablonu. Aby edytować go za pomocą projektanta, kliknij łącze Edytuj szablony z tagów inteligentnych w widoku GridView i wybrano pracę z polem kategorii `EditItemTemplate`. Usuwanie formantu etykiety w sieci Web i zastąp go formantu DropDownList, ustawienie dla właściwości Identyfikatora DropDownList `Categories`.


[![Usuń TexBox i Dodaj DropDownList do EditItemTemplate](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**Rysunek 5**: Usuń TexBox i Dodaj DropDownList do `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image15.png))


Obok należy wypełnić DropDownList z dostępnych kategorii. Kliknij łącze Wybierz źródło danych z tagów inteligentnych DropDownList i wybrać opcję, aby utworzyć nowy element ObjectDataSource o nazwie `CategoriesDataSource`.


[![Utwórz nowe kontrolki ObjectDataSource o nazwie CategoriesDataSource](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**Rysunek 6**: Tworzenie nowej kontrolki ObjectDataSource, o nazwie `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image18.png))


Aby ten element ObjectDataSource zwrócić wszystkie kategorie, powiązać `CategoriesBLL` klasy `GetCategories()` metody.


[![Element ObjectDataSource należy powiązać metodę GetCategories() CategoriesBLL](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**Rysunek 7**: powiązać ObjectDataSource do `CategoriesBLL`w `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image21.png))


Na koniec Skonfiguruj ustawienia DropDownList tak, aby `CategoryName` pole jest wyświetlane w każdym DropDownList `ListItem` z `CategoryID` pola używanego jako wartość.


[![Pole CategoryName wyświetlane i CategoryID używany jako wartość](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**Rysunek 8**: ma `CategoryName` pole wyświetlane i `CategoryID` używane jako wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image24.png))


Po wprowadzania tych zmian deklaratywne kod znaczników dla `EditItemTemplate` w `CategoryName` TemplateField obejmuje zarówno DropDownList i ObjectDataSource:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Lista DropDownList na `EditItemTemplate` musi mieć stan widoku włączone. Składnia wiązania z danymi wkrótce zostanie dodany do składni deklaratywnej DropDownList i polecenia wiązania z danymi, takich jak `Eval()` i `Bind()` może występować tylko w formantach, których stan widoku jest włączony.


Powtórz te kroki, aby dodać DropDownList o nazwie `Suppliers` do `SupplierName` na pole TemplateField `EditItemTemplate`. Te obejmują dodawanie DropDownList do `EditItemTemplate` i tworzenie ObjectDataSource innego. `Suppliers` DropDownList przez element ObjectDataSource, jednak należy skonfigurować, aby wywołać `SuppliersBLL` klasy `GetSuppliers()` metody. Ponadto należy skonfigurować `Suppliers` DropDownList do wyświetlenia `CompanyName` pól i użyj `SupplierID` pole jako wartość jego `ListItem` s.

Po dodaniu DropDownLists do dwóch `EditItemTemplate` s, ładowanie strony w przeglądarce i kliknij przycisk Edytuj Jacka Chef Cajun Seasoning produktu. Jak pokazano na rysunku nr 9, produktu kategorii i dostawcy kolumn są renderowane jako zawierające dostępne kategorie i dostawcy, aby wybrać z listy rozwijanej. Jednak należy pamiętać, że *pierwszy* elementów w obu list rozwijanych są wybrane domyślnie (napoje dla kategorii) i ciecze egzotycznych jako dostawcę, nawet jeśli Seasoning Cajun Jacka Chef jest przyprawa dostarczonych przez Cajun Orleans nowy Radości.


[![Pierwszy element listy rozwijanej jest domyślnie zaznaczona](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**Rysunek 9**: pierwszy element listy rozwijanej jest domyślnie zaznaczona ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image27.png))


Ponadto, jeśli klikniesz przycisk aktualizacji, można znaleźć który produktu `CategoryID` i `SupplierID` wartości są ustawiane na `NULL`. Oba te niepożądane zachowania są spowodowane DropDownLists w `EditItemTemplate` s nie są powiązane z pola danych z danych produktu.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Powiązanie DropDownLists do`CategoryID`i`SupplierID`pól danych

Aby przypisać kategorię produktu edytowanych i dostawcy list rozwijanych Ustaw odpowiednie wartości i poproś go o te wartości wysyłane z powrotem do logiki warstwy Biznesowej `UpdateProduct` metody po kliknięciu aktualizacji, należy powiązać DropDownLists `SelectedValue` właściwości `CategoryID` i `SupplierID` pola danych przy użyciu dwukierunkowego wiązania z danymi. W tym z `Categories` DropDownList, możesz dodać `SelectedValue='<%# Bind("CategoryID") %>'` bezpośrednio do składni deklaratywnej.

Alternatywnie można ustawić powiązania danych DropDownList edycji szablonu za pomocą projektanta i klikając przycisk Edytuj powiązania DataBindings łącza DropDownList tagów inteligentnych. Następnie należy wskazać, że `SelectedValue` właściwość powinna być powiązana z `CategoryID` pola używanie dwukierunkowego powiązania danych (zobacz rysunek 10). Powtórz deklaratywne lub projektanta proces powiązać `SupplierID` pole danych do `Suppliers` DropDownList.


[![Powiąż CategoryID do właściwości SelectedValue DropDownList używanie dwukierunkowego powiązania danych](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**Na rysunku nr 10**: powiązać `CategoryID` do DropDownList `SelectedValue` wiązania z danymi dwustronny przy użyciu właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image30.png))


Po powiązania zostały zastosowane do `SelectedValue` właściwości DropDownLists dwóch kolumn edytowanych produktu kategorii i dostawcy domyślnie zostanie ustawiona wartości bieżącego produktu. Po kliknięciu aktualizacji, `CategoryID` i `SupplierID` wartości elementu wybranej listy rozwijanej zostanie przekazany do `UpdateProduct` metody. Rysunek 11 samouczek pokazuje, po dodaniu instrukcje wiązania danych; należy zwrócić uwagę, jak elementy wybranej listy rozwijanej dla Seasoning Cajun Jacka Chef są prawidłowo przyprawa i radości Cajun Orleans nowy.


[![Domyślnie wybrane są produktu edytować bieżącej kategorii i dostawca wartości](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**Rysunek 11**: edytować produktu w bieżącej kategorii i dostawcy wartości, które są wybrane domyślnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image33.png))


## <a name="handlingnullvalues"></a>Obsługa`NULL`wartości

`CategoryID` i `SupplierID` kolumn w `Products` tabela może być `NULL`, jeszcze DropDownLists w `EditItemTemplate` s nie zawierają elementu listy do reprezentowania `NULL` wartość. Ma to dwa konsekwencje:

- Użytkownik nie może użyć naszych interfejsu zmienić kategorii produktu lub dostawcę z innej`NULL` do wartości `NULL` jeden
- Jeśli produkt ma `NULL` `CategoryID` lub `SupplierID`, klikając przycisk Edytuj spowoduje wygenerowanie wyjątku. Jest to spowodowane `NULL` wartość zwrócona przez `CategoryID` (lub `SupplierID`) w `Bind()` instrukcja nie jest zmapowany na wartość w DropDownList (DropDownList zgłasza wyjątek podczas jego `SelectedValue` właściwość jest ustawiona na wartość *nie* w jego Kolekcja elementów listy).

Aby zapewnić obsługę `NULL` `CategoryID` i `SupplierID` wartości, należy dodać kolejne `ListItem` do każdego DropDownList do reprezentowania `NULL` wartość. W [wzorzec/szczegół filtrowania z DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) samouczka widzieliśmy sposób dodawania dodatkowych `ListItem` do danymi DropDownList zaangażowany ustawienie DropDownList `AppendDataBoundItems` właściwości `True` i ręczne dodanie dodatkowych `ListItem`. W tym samouczku poprzedniej, jednak dodaliśmy `ListItem` z `Value` z `-1`. Logika wiązania z danymi w programie ASP.NET, jednak automatycznie przekonwertuje pusty ciąg na `NULL` wartość i a odwrotnie. W związku z tym, w tym samouczku chcemy `ListItem`w `Value` być ciągiem pustym.

Start przez ustawienie obu DropDownLists `AppendDataBoundItems` właściwości `True`. Następnie dodaj `NULL` `ListItem` przez dodanie poniższego `<asp:ListItem>` elementu do każdego DropDownList, dzięki czemu wygląda deklaratywne znaczników takich jak:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

Zawarto Użyj "(None)" jako wartości tekstowej dla tego `ListItem`, ale można ją również być ciąg pusty, jeśli chcesz zmienić.

> [!NOTE]
> Jak widzieliśmy w *wzorzec/szczegół filtrowania z DropDownList* samouczek, `ListItem` s mogą być dodawane do DropDownList poprzez projektanta, klikając DropDownList `Items` właściwości w oknie właściwości (który zostanie wyświetlona `ListItem` edytora kolekcji). Należy jednak pamiętać dodać `NULL` `ListItem` w tym samouczku za pomocą składni deklaratywnej. Jeśli używasz `ListItem` edytora kolekcji wygenerowanego składni deklaratywnej zostaną pominięte `Value` ustawienie całkowicie kiedy przypisane ciąg pusty deklaratywne znaczników, takich jak tworzenie: `<asp:ListItem>(None)</asp:ListItem>`. Gdy wygląda nieszkodliwe, brak wartości powoduje, że lista DropDownList do użycia `Text` wartości właściwości w tym miejscu. Oznacza to, że jeśli to `NULL` `ListItem` jest zaznaczone, wartość "(None)" nastąpi próba można przypisać do `CategoryID`, co spowoduje Wystąpił wyjątek. Jawnie ustawiając `Value=""`, `NULL` można przypisać wartości do `CategoryID` podczas `NULL` `ListItem` jest zaznaczone.


Powtórz te kroki dla DropDownList dostawców.

Te dodatkowe `ListItem`, interfejs do edycji teraz można przypisać `NULL` wartości z produktem `CategoryID` i `SupplierID` pól, jak pokazano na rysunku 12.


[![Wybierz (Brak) można przypisać wartość NULL dla kategorii produktu lub dostawcy](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**Rysunek 12**: Wybierz (Brak) do przypisania `NULL` wartości kategorii produktu lub dostawcy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Krok 4: Korzystanie z elementami RadioButton wycofane stanu

Obecnie produktów `Discontinued` pole danych jest wyrazić przy użyciu elementu CheckBoxField, która renderuje wyłączone pole wyboru dla wierszy tylko do odczytu i włączone wyboru dla wiersza edytowanej. Ten interfejs użytkownika często jest odpowiedni, możemy dostosować go w razie potrzeby, przy użyciu TemplateField. W tym samouczku Zmieńmy polu CheckBoxField na pole TemplateField używającej kontroli RadioButtonList dwie opcje "Zaprzestać" i "Active" z którego użytkownik może określić produktu `Discontinued` wartość.

Rozpocznij od konwertowanie `Discontinued` CheckBoxField na pole TemplateField, co spowoduje utworzenie TemplateField z `ItemTemplate` i `EditItemTemplate`. Zarówno szablony obejmują wyboru z jego `Checked` właściwość powiązana z `Discontinued` pola danych, jedyną różnicą między tymi dwoma, który jest `ItemTemplate`dla tego pola wyboru `Enabled` właściwość jest ustawiona na `False`.

Zastąp pole wyboru w obu `ItemTemplate` i `EditItemTemplate` z formantem RadioButtonList ustawienie obu RadioButtonLists `ID` właściwości `DiscontinuedChoice`. Następnie wskazuje, że RadioButtonLists powinien każdego zawierają dwa przyciski radiowe, co etykietą "Active" o wartości "False" i jeden z etykietą "Zaprzestać" o wartości "True". Można wprowadzić w tym celu `<asp:ListItem>` elementów bezpośrednio za pomocą składni deklaratywnej lub użyj `ListItem` edytora kolekcji przy użyciu projektanta. Przedstawia rysunek 13 `ListItem` edytora kolekcji po opcji przycisku radiowego dwa zostały określone.


[![Dodaj opcje Active i wycofane do RadioButtonList](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**Rysunek 13**: zaprzestać opcje RadioButtonList i Dodaj Active ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image39.png))


Od RadioButtonList w `ItemTemplate` nie powinien być edytowalny, ustaw jej `Enabled` właściwości `False`, zostawianie `Enabled` właściwości `True` (ustawienie domyślne) dla RadioButtonList w `EditItemTemplate`. To spowoduje, że przycisków radiowych w wierszu edytować jako tylko do odczytu, ale umożliwi użytkownikowi w celu zmiany wartości elementu RadioButton wiersza edytowanej.

Nadal trzeba przypisać formantów RadioButtonList `SelectedValue` właściwości tak, aby odpowiedni przycisk radiowy jest zaznaczony na podstawie produktu `Discontinued` pola danych. Podobnie jak w przypadku DropDownLists wcześniej w tym samouczku, albo można dodać tej składni wiązania danych bezpośrednio do deklaratywne znaczników lub za pośrednictwem łącza Edytuj powiązania DataBindings w tagów inteligentnych RadioButtonLists.

Po dodaniu dwóch RadioButtonLists i skonfigurowaniu ich `Discontinued` deklaratywne znaczników na pole TemplateField powinien wyglądać następująco:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

Wprowadzone zmiany `Discontinued` kolumna została przekształcona z listy pól wyboru z listą par przycisk radiowy (patrz rysunek 14). Podczas edytowania produktu, odpowiedni przycisk radiowy jest zaznaczony i stan wycofane produktu może być aktualizowany przez wybranie przycisku radiowego, a następnie klikając polecenie Update.


[![Wycofane pola wyboru zostały zastąpione przez pary przycisku radiowego](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**Rysunek 14**: zaprzestać pola wyboru zostały zastąpione przez pary przycisk radiowy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image42.png))


> [!NOTE]
> Ponieważ `Discontinued` kolumny w `Products` bazy danych nie może mieć `NULL` wartości, firma Microsoft nie trzeba martwić się o przechwytywaniu `NULL` informacji w interfejsie. Jeśli, jednak `Discontinued` kolumna może zawierać `NULL` chcemy dodać innej wartości opcji przycisku do listy z jego `Value` ustawiony na pusty ciąg (`Value=""`), po prostu jak z kategorii i dostawcy DropDownLists.


## <a name="summary"></a>Podsumowanie

Podczas elementu BoundField i CheckBoxField automatycznie renderowania tylko do odczytu, edytowanie, wstawianie interfejsów, mają możliwości dostosowywania. Często, trzeba będzie dostosować edycji lub wstawianie interfejsu, możliwe, że dodawanie formantów weryfikacji (zgodnie z widzieliśmy w poprzednim samouczek) lub dostosowanie interfejsu użytkownika kolekcji danych (jak widzieliśmy w ramach tego samouczka). Dostosowywanie interfejsu z TemplateField można sumowane w poniższych krokach:

1. Dodaj pole TemplateField lub przekonwertować istniejącego elementu BoundField lub CheckBoxField na pole TemplateField
2. Rozszerzyć interfejsu, w razie potrzeby
3. Powiązania pól odpowiednie dane do nowo dodanego formantów sieci Web przy użyciu dwukierunkowego wiązania z danymi

Oprócz za pomocą wbudowanych formantów sieci Web ASP.NET, można również dostosować szablony TemplateField kontrolek niestandardowych, skompilowanych serwera i kontrolki użytkownika.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Poprzednie](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
[dalej](implementing-optimistic-concurrency-vb.md)
