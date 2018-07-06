---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: Dostosowywanie interfejsu modyfikacji danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku przyjrzymy Dostosowywanie interfejsu edycji kontrolki GridView, zastępując standardowego pola tekstowego, a pola wyboru steruje się za pomocą Adres alternaty...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 991f8d07c12c13b1477c2df072847b3730bc1051
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840771"
---
<a name="customizing-the-data-modification-interface-vb"></a>Dostosowywanie interfejsu modyfikacji danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe) lub [Pobierz plik PDF](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> W tym samouczku przyjrzymy Dostosowywanie interfejsu edycji kontrolki GridView, zastępując standardowego pola tekstowego, a pola wyboru steruje się za pomocą alternatywnych kontrolek wejściowych w sieci Web.


## <a name="introduction"></a>Wprowadzenie

BoundFields i używane przez kontrolki GridView i DetailsView CheckBoxFields upraszcza proces modyfikowanie danych ze względu na możliwość renderowania tylko do odczytu, można edytować i które można wstawić interfejsów. Interfejsy te mogą być renderowane bez konieczności dodawania dodatkowych oznaczeniu deklaracyjnym ani kodu. Jednak interfejsów elementu BoundField i jego CheckBoxField braku dostosowywalności często wymagane w przypadku scenariuszy w rzeczywistych warunkach. Aby dostosować interfejs atrybutu do edycji lub które można wstawić w widoku GridView lub DetailsView należy zamiast tego użyć TemplateField.

W [poprzedni Samouczek](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) widzieliśmy sposobu dostosowywania interfejsy modyfikacji danych, dodając formanty Web sprawdzania poprawności. W tym samouczku Zapoznamy się jak dostosować kontrolki sieci Web kolekcji rzeczywiste dane elementu BoundField i firmy CheckBoxField standardowego pola tekstowego i formantów CheckBox przy użyciu alternatywnych kontrolek wejściowych w sieci Web. W szczególności utworzymy GridView można edytować, umożliwiająca produktu nazwy, kategorii, dostawcy i stan nieobsługiwane do zaktualizowania. Podczas edytowania określonego wiersza, pola kategorii i dostawcy będą renderowane jako kontrolek DropDownList, zawierającą zestaw dostępnych kategorii i dostawców do wyboru. Ponadto firma Microsoft będzie Zamień CheckBoxField domyślne pola wyboru formant RadioButtonList, który oferuje dwie opcje: "Aktywny" i "Zakończona".


[![Interfejsu edycji kontrolki GridView obejmuje kontrolek DROPDOWNLIST i przyciski radiowe](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**Rysunek 1**: do edycji kontrolek DROPDOWNLIST obejmuje interfejs i przyciski radiowe kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Krok 1: Tworzenie odpowiedniej`UpdateProduct`przeciążenia

W tym samouczku utworzymy GridView można edytować, umożliwia edytowanie nazwy produktu, kategorii, dostawcy i wycofane stanu. W związku z tym, potrzebujemy `UpdateProduct` przeciążenie, które akceptuje 5 parametrów wejściowych wartości tych czterech produktu oraz `ProductID`. W naszym poprzedniego przeciążeń, co spowoduje, takich jak:

1. Pobierz informacje o produkcie z bazy danych dla określonego `ProductID`,
2. Aktualizacja `ProductName`, `CategoryID`, `SupplierID`, i `Discontinued` pola, a
3. Wyślij żądanie aktualizacji z warstwą dal za pomocą TableAdapter `Update()` metody.

Celu skrócenia programu dla tego określonego przeciążenia I zostały pominięte czy reguły biznesowej gwarantuje, że produkt jest oznaczony jako wycofane nie jest tylko produkty oferowane przez dostawcę. Możesz dodać go, jeśli użytkownik sobie tego życzy lub, w idealnym przypadku zrefaktoryzuj się logikę do oddzielnych metodach.

Poniższy kod przedstawia nową `UpdateProduct` przeciążenia w `ProductsBLL` klasy:


[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>Krok 2: Tworzenie można edytować kontrolki GridView

Za pomocą `UpdateProduct` dodano przeciążenia, możemy przystąpić do tworzenia naszej edytowalne GridView. Otwórz `CustomizedUI.aspx` stronie `EditInsertDelete` folder i dodać kontrolki widoku siatki do projektanta. Następnie należy utworzyć nowe kontrolki ObjectDataSource z GridView tagu inteligentnego. Konfigurowanie kontrolki ObjectDataSource można pobrać informacji o produkcie za pośrednictwem `ProductBLL` klasy `GetProducts()` metody i aktualizowanie danych za pomocą produktu `UpdateProduct` przeciążenia, którą właśnie utworzyliśmy. Z karty Wstawianie i usuwanie wybierz z listy rozwijanej (Brak).


[![Konfigurowanie kontrolki ObjectDataSource do użycia przeciążenia UpdateProduct właśnie utworzony](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**Rysunek 2**: Konfigurowanie kontrolki ObjectDataSource do użycia `UpdateProduct` przeciążenia właśnie utworzony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image6.png))


Jak widzieliśmy w całym samouczki modyfikacji danych, przypisuje składni deklaratywnej dla elementu ObjectDataSource utworzone przez program Visual Studio `OldValuesParameterFormatString` właściwość `original_{0}`. To, oczywiście, nie będzie działać z naszych warstwy logiki biznesowej od naszych metody oczekują, że oryginalna `ProductID` wartości, które zostaną przekazane w. W związku z tym, podobnie jak w poprzednich samouczkach, Poświęć chwilę, aby usunąć to przypisanie właściwości ze składni deklaratywnej lub, zamiast tego należy ustawić wartość tej właściwości na `{0}`.

Po tej zmianie ObjectDataSource oznaczeniu deklaracyjnym powinien wyglądać następująco:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

Należy pamiętać, że `OldValuesParameterFormatString` właściwości zostały usunięte oraz że ma `Parameter` w `UpdateParameters` kolekcji dla każdego z parametrów wejściowych, oczekiwany przez naszych `UpdateProduct` przeciążenia.

Gdy kontrolki ObjectDataSource jest skonfigurowany do aktualizacji tylko podzbiór wartości produktu, widoku GridView przedstawia obecnie *wszystkich* pól produktu. Poświęć chwilę, aby edytować kontrolki GridView tak, aby:

- Zawierają one tylko `ProductName`, `SupplierName`, `CategoryName` BoundFields i `Discontinued` CheckBoxField
- `CategoryName` i `SupplierName` pola do umieszczenia przed (po lewej stronie) `Discontinued` CheckBoxField
- `CategoryName` i `SupplierName` BoundFields `HeaderText` właściwość jest ustawiona na "Category" i "Dostawca" odpowiednio
- Jest włączona obsługa edycji (sprawdzenie, czy pole wyboru Włącz edytowanie w tagu inteligentnego GridView)

Po wprowadzeniu tych zmian Projektant będzie wyglądać podobnie do rysunku 3, za pomocą składni deklaratywnej GridView, pokazano poniżej.


[![Usuń niepotrzebne pola z widoku GridView](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**Rysunek 3**: Usuń zbędne pola z kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

W tym momencie GridView zachowanie tylko do odczytu zostało ukończone. Podczas przeglądania danych, jest renderowany jako wiersz w widoku GridView przedstawiający nazwę produktu, kategorii, dostawcy i wycofane stan każdego produktu.


[![Interfejs tylko do odczytu w widoku GridView zostało ukończone](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**Rysunek 4**: zakończeniu GridView tylko do odczytu interfejsu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image12.png))


> [!NOTE]
> Zgodnie z opisem w [samouczek Przegląd Wstawianie, aktualizowanie i usuwanie danych](an-overview-of-inserting-updating-and-deleting-data-cs.md), jest wszechobecne GridView można s widok stanu włączenia (zachowanie domyślne). Jeśli ustawisz GridView s `EnableViewState` właściwości `false`, uruchom ryzyko, że liczba równoczesnych użytkowników przypadkowo usuwanie i edytowanie rekordów. Zobacz [Ostrzeżenie: problem współbieżności przy użyciu platformy ASP.NET 2.0 GridViews/DetailsView/FormViews tej edycji pomocy technicznej i/lub usuwanie i Whose stan widoku jest wyłączona](http://scottonwriting.net/sowblog/posts/10054.aspx) Aby uzyskać więcej informacji.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Krok 3: Przy użyciu kontrolki DropDownList kategorii i dostawcy edycji interfejsów

Pamiętamy `ProductsRow` obiekt zawiera `CategoryID`, `CategoryName`, `SupplierID`, i `SupplierName` właściwości, które zawierają rzeczywiste wartości Identyfikatora klucza obcego w `Products` bazy danych w tabeli i odpowiedni `Name` wartości w `Categories` i `Suppliers` tabel. `ProductRow`Firmy `CategoryID` i `SupplierID` można zarówno odczytywane i zapisywane, podczas gdy `CategoryName` i `SupplierName` właściwości są oznaczone jako tylko do odczytu.

Z powodu stanu tylko do odczytu `CategoryName` i `SupplierName` właściwości mieli odpowiednich BoundFields ich `ReadOnly` właściwością `True`, uniemożliwiając modyfikowany podczas edycji wiersz te wartości. Gdy firma Microsoft można ustawić `ReadOnly` właściwości `False`, renderowanie `CategoryName` i `SupplierName` BoundFields jako pola tekstowe podczas edycji, takie podejście spowodują wyjątek po użytkownik próbuje zaktualizować produkt, ponieważ ma nie `UpdateProduct` przeciążenia, które przyjmuje `CategoryName` i `SupplierName` danych wejściowych. W rzeczywistości nie chcemy tworzyć takie przeciążenia dwóch powodów:

- `Products` Tabela nie ma `SupplierName` lub `CategoryName` pól, ale `SupplierID` i `CategoryID`. W związku z tym chcemy, aby nasz metody do przekazania tych konkretnych wartości Identyfikatora nie ich tabel odnośników wartości.
- Od użytkownika, wpisz nazwę dostawcy lub kategorii jest mniej niż idealne rozwiązanie, ponieważ wymaga, aby użytkownik znał dostępne kategorie i dostawców oraz ich prawidłowe pisowni.

Pola dostawcy i kategorii powinien być wyświetlany w danej kategorii i nazw dostawców w trybie tylko do odczytu (co jest teraz wykonywane) i listę rozwijaną listę odpowiednich opcji podczas edycji. Przy użyciu listy rozwijanej użytkownika końcowego można szybko wyświetlić, jakie kategorie i dostawców są dostępne do wyboru i więcej łatwo wprowadzać ich zaznaczenia.

Aby zapewnić to zachowanie, musimy przekonwertować `SupplierName` i `CategoryName` BoundFields do kontrolek TemplateField którego `ItemTemplate` emituje `SupplierName` i `CategoryName` wartości i którego `EditItemTemplate` korzysta z kontrolki DropDownList do listy Dostępne kategorie i dostawców.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Dodawanie`Categories`i`Suppliers`kontrolek DROPDOWNLIST

Uruchom za pomocą konwersji `SupplierName` i `CategoryName` BoundFields do kontrolek TemplateField przez: klikając łącze Edytowanie kolumn z tagu inteligentnego GridView; Wybieranie elementu BoundField z listy w lewym dolnym rogu; i klikając przycisk "Konwertuj to pole do Łącze TemplateField". Proces konwersji utworzy TemplateField zarówno `ItemTemplate` i `EditItemTemplate`, jak pokazano na poniższej składni deklaratywnej:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

Ponieważ elementu BoundField została oznaczona jako tylko do odczytu, zarówno `ItemTemplate` i `EditItemTemplate` zawierają Web etykiety formant, którego `Text` właściwość jest powiązana z pola danych dotyczy (`CategoryName`, w powyższej przykładowej składni). Należy zmodyfikować `EditItemTemplate`, zastępując formant etykiety w sieci Web przy użyciu kontrolki DropDownList.

Jak widzieliśmy w poprzednich samouczkach szablonu można edytować za pomocą projektanta lub bezpośrednio z poziomu składni deklaratywnej. Aby edytować go za pomocą projektanta, kliknij link Edytuj szablony z tagu inteligentnego GridView i pracy za pomocą pola kategorii `EditItemTemplate`. Usuń kontrolka etykiety w sieci Web i zastąp go przy użyciu kontrolki DropDownList, ustawienie dla właściwości identyfikator DropDownList `Categories`.


[![Usuń TexBox i Dodaj kontrolki DropDownList do EditItemTemplate](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**Rysunek 5**: Usuń TexBox i Dodaj do kontrolki DropDownList `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image15.png))


Następnie należy wypełnić DropDownList z dostępnych kategorii. Kliknij Link wybierz źródło danych z kontrolki DropDownList tagu inteligentnego, a następnie wybrać opcję utworzenia nowego elementu ObjectDataSource, o nazwie `CategoriesDataSource`.


[![Tworzenie formantu ObjectDataSource o nazwie CategoriesDataSource](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**Rysunek 6**: Tworzenie nowego formantu ObjectDataSource, o nazwie `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image18.png))


Aby to ObjectDataSource zwrócić wszystkie kategorie, powiązać `CategoriesBLL` klasy `GetCategories()` metody.


[![Powiązanie kontrolki ObjectDataSource metodą GetCategories() CategoriesBLL](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**Rysunek 7**: powiązywanie kontrolki ObjectDataSource do `CategoriesBLL`firmy `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image21.png))


Na koniec Skonfiguruj ustawienia DropDownList tak, aby `CategoryName` pole jest wyświetlane w każdej metody DropDownList `ListItem` z `CategoryID` używana jako wartość pola.


[![Pole CategoryName wyświetlane i CategoryID używana jako wartość](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**Rysunek 8**: mają `CategoryName` pole wyświetlane i `CategoryID` używana jako wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image24.png))


Po wprowadzisz te zmiany w oznaczeniu deklaracyjnym dla `EditItemTemplate` w `CategoryName` TemplateField zostaną uwzględnione w elementu ObjectDataSource i kontrolki DropDownList:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Lista DropDownList na platformie `EditItemTemplate` musi mieć swój stan widoku, włączone. Składnia wiązania danych wkrótce zostanie dodany do składni deklaratywnej DropDownList i polecenia wiązania danych, takich jak `Eval()` i `Bind()` może wystąpić tylko w kontrolkach, którego stan widoku jest włączony.


Powtórz te kroki, aby dodać kontrolki DropDownList o nazwie `Suppliers` do `SupplierName` firmy TemplateField `EditItemTemplate`. Działania te obejmują dodawanie do kontrolki DropDownList `EditItemTemplate` i utworzenie innego elementu ObjectDataSource. `Suppliers` ObjectDataSource DropDownList firmy, jednak należy skonfigurować do wywołania `SuppliersBLL` klasy `GetSuppliers()` metody. Ponadto należy skonfigurować `Suppliers` DropDownList, aby wyświetlić `CompanyName` pola, a następnie użyć `SupplierID` pola jako wartość jej `ListItem` s.

Po dodaniu kontrolek DROPDOWNLIST do dwóch `EditItemTemplate` s, ładowania strony w przeglądarce i kliknij przycisk Edytuj Jacka Chef Cajun Seasoning produktu. Jak pokazano na rysunku nr 9, kolumn kategorii i dostawcy, produktu są renderowane jako listy rozwijanej zawierające dostępne kategorie i dostawców do wyboru. Jednak należy pamiętać, że *pierwszy* elementów w obu list rozwijanych są domyślnie zaznaczone (Beverages kategorii) i egzotycznych płynów jako dostawcę, mimo że Seasoning Cajun Jacka Chef jest dostarczane przez nowy Orlean Cajun przyprawa Radości.


[![Pierwszy element listy rozwijanej jest domyślnie zaznaczone](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**Rysunek 9**: pierwszy element listy rozwijanej jest domyślnie zaznaczone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image27.png))


Ponadto, jeśli kliknięciu przycisku Aktualizuj znajdziesz, produktu `CategoryID` i `SupplierID` wartości są ustawiane na `NULL`. Oba te niepożądane zachowania są spowodowane tym, że kontrolek DROPDOWNLIST w `EditItemTemplate` s nie są powiązane z dowolnego pola danych z danych produktu.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Powiązanie kontrolek DROPDOWNLIST do`CategoryID`i`SupplierID`pól danych

Aby kategorii produktu edytowany i dostawcy list rozwijanych Ustaw odpowiednie wartości i mogą mieć tych wartości wysyłane z powrotem do LOGIKI `UpdateProduct` metody po kliknięciu aktualizacji, należy powiązać kontrolek DROPDOWNLIST `SelectedValue` właściwości `CategoryID` i `SupplierID` pola danych za pomocą dwukierunkowego wiązania danych. Aby wykonać to za pomocą `Categories` DropDownList, można dodać `SelectedValue='<%# Bind("CategoryID") %>'` bezpośrednio do składni deklaratywnej.

Alternatywnie można ustawić DropDownList powiązania danych edytowanie szablonu za pomocą projektanta i klikając link Edytuj powiązania danych z kontrolki DropDownList tagu inteligentnego. Następnie należy wskazać, że `SelectedValue` właściwość powinna być powiązana z `CategoryID` pola przy użyciu dwukierunkowego wiązania danych (zobacz rysunek 10). Powtórz zaznacza deklaratywne lub projektanta procedurę można powiązać `SupplierID` pole danych, aby `Suppliers` DropDownList.


[![Powiązywanie CategoryID właściwości SelectedValue DropDownList przy użyciu dwukierunkowego wiązania danych](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**Na rysunku nr 10**: powiązywanie `CategoryID` do metody DropDownList `SelectedValue` Databinding dwustronny przy użyciu właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image30.png))


Po zastosowaniu powiązania w celu `SelectedValue` właściwości dwóch kontrolek DropDownList, edytowany produktu kolumny kategorii i dostawcy będą domyślnie wartości bieżącego produktu. Po kliknięciu aktualizacji `CategoryID` i `SupplierID` wartości elementu wybranej liście rozwijanej zostaną przekazane do `UpdateProduct` metody. Na ilustracji 11 pokazano samouczka po dodaniu instrukcji wiązania danych; należy pamiętać o tym, jak elementy wybranej listy rozwijanej dla Seasoning Cajun Jacka Chef są poprawnie przyprawa i nowy Orlean Cajun radości.


[![Domyślnie wybrane są bieżącej kategorii produktu edytować i dostawca wartości](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**Rysunek 11**: edytować bieżącej kategorii i produktu dostawcy wartości, które są wybrane domyślnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image33.png))


## <a name="handlingnullvalues"></a>Obsługa`NULL`wartości

`CategoryID` i `SupplierID` kolumn w `Products` tabela może być `NULL`, jeszcze kontrolek DROPDOWNLIST w `EditItemTemplate` s nie dołączaj elementu listy do reprezentowania `NULL` wartość. To ma dwa skutki:

- Użytkownik nie może używać naszego interfejsu, aby zmienić kategorii produktu lub dostawcę z innej niż`NULL` wartość `NULL` jeden
- Jeśli produkt ma `NULL` `CategoryID` lub `SupplierID`, klikając przycisk Edytuj spowodują wyjątek. Jest to spowodowane `NULL` wartość zwrócona przez obiekt `CategoryID` (lub `SupplierID`) w `Bind()` instrukcja nie jest mapowany na wartość metody DropDownList (metody DropDownList zgłasza wyjątek podczas jego `SelectedValue` właściwość jest ustawiona na wartość *nie* w jego Kolekcja elementów listy).

Aby można było obsługiwać `NULL` `CategoryID` i `SupplierID` wartości, należy dodać inny `ListItem` do każdej metody DropDownList do reprezentowania `NULL` wartość. W [wzorzec/szczegół filtrowanie przy użyciu kontrolki DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) samouczka będziemy pokazaliśmy, jak dodać kolejny `ListItem` się z danymi DropDownList zaangażowane ustawienie DropDownList `AppendDataBoundItems` właściwość `True` i ręczne dodanie dodatkowych `ListItem`. W poprzednim samouczku, dodaliśmy `ListItem` z `Value` z `-1`. Jednak logika wiązania danych w programie ASP.NET: zostanie automatycznie przekonwertowana na ciąg pusty, aby `NULL` wartość i na odwrót. W związku z tym, w tym samouczku chcemy `ListItem`firmy `Value` być ciągiem pustym.

Rozpocznij od ustawienie oba kontrolek DROPDOWNLIST `AppendDataBoundItems` właściwość `True`. Następnie dodaj `NULL` `ListItem` przez dodanie poniższego `<asp:ListItem>` elementu do każdej metody DropDownList, dzięki czemu wygląda w oznaczeniu deklaracyjnym takich jak:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

Zawarto Użyj "(None)" jako wartości tekstowej w tym `ListItem`, ale można ją również oczekiwany jest ciąg pusty, jeśli chcesz zmienić.

> [!NOTE]
> Jak widzieliśmy w *wzorzec/szczegół filtrowanie przy użyciu kontrolki DropDownList* samouczku `ListItem` s mogą być dodawane do kontrolki DropDownList za pomocą projektanta, klikając DropDownList `Items` właściwości w oknie dialogowym właściwości (który zostanie wyświetlona `ListItem` — Edytor kolekcji). Należy jednak pamiętać dodać `NULL` `ListItem` na potrzeby tego samouczka przy użyciu składni deklaratywnej. Jeśli używasz `ListItem` Edytor kolekcji zostaną pominięte w wygenerowanym składni deklaratywnej `Value` całkowicie ustawienie, jeśli przypisany ciąg pusty, tworzenie oznaczeniu deklaracyjnym, takich jak: `<asp:ListItem>(None)</asp:ListItem>`. Chociaż może to wyglądać nieszkodliwe, brak wartości powoduje, że metody DropDownList używać `Text` wartości właściwości w tym miejscu. Oznacza to, że jeśli to `NULL` `ListItem` jest zaznaczone, wartość "(None)" nastąpi próba można przypisać do `CategoryID`, co spowoduje wyjątek. Poprzez jawne ustawienie `Value=""`, `NULL` można przypisać wartości do `CategoryID` podczas `NULL` `ListItem` jest zaznaczone.


Powtórz te czynności dla metody DropDownList dostawców.

Dzięki temu dodatkowe `ListItem`, interfejs edytowania można teraz przypisywać `NULL` wartości z produktem `CategoryID` i `SupplierID` pola, jak pokazano na rysunku 12.


[![Wybierz (Brak) można przypisać wartości NULL dla kategorii produktu lub dostawcy](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**Rysunek 12**: Wybierz (Brak) do przypisania `NULL` wartości kategorii produktu lub dostawcy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Krok 4: Przy użyciu elementów RadioButton nieobsługiwane stanu

Obecnie produktów `Discontinued` pole danych jest wyrażona za pomocą CheckBoxField, która renderuje wyłączone pole wyboru dla wierszy tylko do odczytu i włączone pole wyboru dla wiersza, edytowany. Podczas tego interfejsu użytkownika często jest odpowiednia, możemy dostosować ją w razie potrzeby przy użyciu TemplateField. W tym samouczku zmienimy CheckBoxField do TemplateField, używający kontroli RadioButtonList dwie opcje "Zakończona" i "Active" z którego użytkownik może określić produktu `Discontinued` wartość.

Rozpocznij od konwertowanie `Discontinued` CheckBoxField do TemplateField, które spowoduje utworzenie TemplateField z `ItemTemplate` i `EditItemTemplate`. Oba szablony zawierają pola wyboru przy użyciu jego `Checked` właściwość powiązana `Discontinued` pole danych, jedyną różnicą między tymi dwoma, który jest `ItemTemplate`firmy składnika CheckBox `Enabled` właściwość jest ustawiona na `False`.

Zastąp pole wyboru w obu `ItemTemplate` i `EditItemTemplate` z kontrolką RadioButtonList ustawienie oba RadioButtonLists `ID` właściwości `DiscontinuedChoice`. Następnie wskazuje, że RadioButtonLists powinien każdego zawierają dwa przyciski radiowe, jedną etykietą "aktywny" o wartości "False" i jednym z etykietą "Zakończona" o wartości "True". Można wprowadzić w tym celu `<asp:ListItem>` elementów bezpośrednio za pomocą składni deklaratywnej lub użyj `ListItem` — Edytor kolekcji przy użyciu projektanta. Przedstawia rysunek 13 `ListItem` — Edytor kolekcji po dwóch radiowy opcji przycisku, które zostały określone.


[![Dodaj aktywnych i nieobsługiwane opcje do RadioButtonList](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**Rysunek 13**: Dodaj aktywnych i wycofane funkcje umożliwiające RadioButtonList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image39.png))


Ponieważ RadioButtonList w `ItemTemplate` nie można edytować, ustaw jego `Enabled` właściwości `False`, opuścić `Enabled` właściwości `True` (ustawienie domyślne) dla RadioButtonList w `EditItemTemplate`. To spowoduje, że przyciski radiowe w wierszu innego niż edytować jako tylko do odczytu, ale pozwoli użytkownikowi na zmianę wartości RadioButton edytowanych wiersza.

Nadal trzeba przypisać kontrolek RadioButtonList `SelectedValue` właściwości tak, że wybrano odpowiedni przycisk radiowy na podstawie produktu `Discontinued` pola danych. Podobnie jak w przypadku kontrolek DROPDOWNLIST zbadane we wcześniejszej części tego samouczka, albo można dodać tej składni wiązania danych bezpośrednio w oznaczeniu deklaracyjnym lub za pomocą linku Edytuj powiązania danych w tagów inteligentnych RadioButtonLists.

Po dodaniu dwóch RadioButtonLists i konfigurowania ich, `Discontinued` TemplateField w oznaczeniu deklaracyjnym powinien wyglądać następująco:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

Za pomocą tych zmian `Discontinued` kolumna została przekształcona z listy pól wyboru do listy par przycisku radiowego (zobacz rysunek 14). Podczas edytowania produktu, odpowiedni przycisk radiowy jest zaznaczone, a stan nieobsługiwane produktu mogą być aktualizowane, wybierając przycisk radiowy i klikając aktualizacji.


[![Nieobsługiwane pola wyboru zostały zastąpione przez pary przycisku radiowego](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**Rysunek 14**: zakończona pola wyboru zostały zastąpione przez pary przycisku radiowego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-data-modification-interface-vb/_static/image42.png))


> [!NOTE]
> Ponieważ `Discontinued` kolumny w `Products` bazy danych nie może mieć `NULL` wartości, nie musimy martwić się o przechwytywaniu `NULL` informacji w interfejsie. Jeśli jednak `Discontinued` kolumna może zawierać `NULL` przycisk, do listy przy użyciu opcji wartości, które chcemy dodać trzecią jego `Value` ustawiony na pusty ciąg (`Value=""`), podobnie jak za pomocą kategorii i dostawcy kontrolek DROPDOWNLIST.


## <a name="summary"></a>Podsumowanie

Gdy elementu BoundField i CheckBoxField automatycznie renderowane tylko do odczytu, edycji i wstawiania interfejsów, brak możliwości dostosowania. Często jednak trzeba będzie dostosować edycji lub wstawianie interfejsu, może być dodawanie kontrolek weryfikacji (zgodnie z widzieliśmy w poprzednim samouczku) lub przez dostosowanie interfejsu użytkownika kolekcji danych (zgodnie z widzieliśmy w ramach tego samouczka). Dostosowywanie interfejsu z TemplateField można sumowane w poniższych krokach:

1. Dodawanie TemplateField lub przekonwertować istniejącego elementu BoundField lub CheckBoxField na TemplateField
2. Rozszerzyć interfejs stosownie do potrzeb
3. Powiązywanie pól danych odpowiednie nowo dodane kontrolki sieci Web przy użyciu dwukierunkowego wiązania danych

Oprócz wbudowanych formantów sieci Web platformy ASP.NET, można również dostosować szablony TemplateField za pomocą kontrolki niestandardowej, skompilowany serwer oraz użytkownika.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
> [dalej](implementing-optimistic-concurrency-vb.md)
