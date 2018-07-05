---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: Batch aktualizowanie (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Dowiedz się, jak aktualizowanie wielu rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika, firma Microsoft tworzy GridView, gdzie każdy wiersz jest edytowalny. W danych...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: 7830e010ea8dbf6ce9bd59154c10eb4c30a3dceb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363316"
---
<a name="batch-updating-vb"></a>Batch aktualizowanie (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) lub [Pobierz plik PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Dowiedz się, jak aktualizowanie wielu rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika, firma Microsoft tworzy GridView, gdzie każdy wiersz jest edytowalny. W warstwie dostępu do danych Firma opakować wiele operacji aktualizacji w ramach transakcji, aby upewnić się, że wszystkie aktualizacje powiedzie się lub wszystkie aktualizacje zostaną wycofane.


## <a name="introduction"></a>Wprowadzenie

W [poprzedni Samouczek](wrapping-database-modifications-within-a-transaction-vb.md) widzieliśmy, jak rozszerzyć warstwę dostępu do danych w celu dodania obsługi dla transakcji bazy danych. Transakcje bazy danych gwarantuje, że szereg instrukcji modyfikacji danych będą traktowane jako jedna operacja niepodzielnego, który zapewnia, że wszystkie modyfikacje zakończy się niepowodzeniem, lub wszystkie zakończy się powodzeniem. Dzięki tej niskiego poziomu warstwy DAL funkcji sposób możemy ponownie gotowe Włącz naszej uwagi do tworzenia interfejsów modyfikacji danych usługi batch.

W tym samouczku utworzymy GridView, gdzie każdy wiersz jest edytowalny (patrz rysunek 1). Ponieważ każdy wiersz jest wyświetlana w interfejsie edycji miejsca s nie ma potrzeby wartości w kolumnie edycji, zaktualizuj i przyciski "Anuluj". Zamiast tego, dostępne są dwa przyciski aktualizacji produktów na stronie, po kliknięciu wyliczyć wierszy GridView i aktualizują bazę danych.


[![Każdy wiersz w widoku GridView jest edytowalna](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Rysunek 1**: każdy wiersz w widoku GridView jest edytowalna ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-vb/_static/image2.png))


Rozpocznij pracę dzięki s!

> [!NOTE]
> W [wykonywanie aktualizacji wsadowych](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) samouczku utworzyliśmy edycji partii interfejs, za pomocą kontrolki DataList. W tym samouczku różni się od poprzedniej w które jest używane GridView i wykonać aktualizację usługi batch w zakresie transakcji. Po ukończeniu tego samouczka zachęcam Cię, aby powrócić do wcześniej samouczek i zaktualizować je, aby korzystać z funkcji związanych z transakcji bazy danych dodane w poprzednim samouczku.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Badanie kroki składania można edytować wszystkie wiersze z GridView

Zgodnie z opisem w [Przegląd Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) samouczku widoku GridView udostępnia wbudowaną obsługę edytowania jego danych źródłowych, na podstawie na wiersz. Wewnętrznie, widoku GridView — informacje o jakie wiersz jest można edytować za pomocą jego [ `EditIndex` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Zgodnie z widoku GridView jest powiązany ze swoim źródłem danych, sprawdza każdy wiersz, aby zobaczyć, jeśli indeks wiersza jest równa wartości `EditIndex`. Jeśli tak, interfejsów wiersza s, które pola są renderowane przy użyciu ich edycji. BoundFields, edytowania interfejsu jest pole tekstowe którego `Text` właściwość jest przypisywana wartość pola danych, określonego przez s elementu BoundField `DataField` właściwości. Dla kontrolek TemplateField `EditItemTemplate` jest używana zamiast `ItemTemplate`.

Pamiętaj, że edycji przepływ pracy jest uruchamiany, gdy użytkownik kliknie przycisk Edytuj wiersz s. To powoduje odświeżenie strony, ustawia GridView s `EditIndex` na indeks wiersza kliknięto s i rebinds danych do siatki. Po kliknięciu przycisku Anuluj wiersz s na zwrot `EditIndex` jest ustawiona na wartość `-1` przed ponowne wiązanie danych do siatki. Ponieważ wierszy s GridView rozpocząć indeksowania od zera, ustawienie `EditIndex` do `-1` powoduje wyświetlanie widoku GridView w trybie tylko do odczytu.

`EditIndex` Właściwość sprawdza się w przypadku edycji na wiersz, ale nie jest przeznaczony do edycji usługi batch. Aby całego widoku GridView można edytować, musimy każdy wiersz renderowania za pomocą jego interfejsu edycji. W tym celu najłatwiej utworzyć, gdzie każde pole można edytować zaimplementowano TemplateField z jego edycji interfejs określony w `ItemTemplate`.

Ciągu kolejnych kilku kroków utworzymy całkowicie edytowalne GridView. W kroku 1 utworzymy Rozpocznij od utworzenia widoku GridView i jego ObjectDataSource i konwertować kontrolek TemplateField jego BoundFields i CheckBoxField. W krokach 2 i 3 zmienimy interfejsów edycji z kontrolek TemplateField `EditItemTemplate` s, aby ich `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Krok 1: Wyświetlanie informacji o produkcie

Zanim firma martwić się o tworzeniu GridView których wiersze są edytowalne, umożliwić s najpierw po prostu wyświetlanie informacji o produkcie. Otwórz `BatchUpdate.aspx` stronie `BatchData` folder i przeciągnij GridView z przybornika do projektanta. Ustaw GridView s `ID` do `ProductsGrid` i z jego tag inteligentny chcesz powiązać nowe kontrolki ObjectDataSource, o nazwie `ProductsDataSource`. Konfigurowanie kontrolki ObjectDataSource można pobrać danych z `ProductsBLL` klasy s `GetProducts` metody.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy ProductsBLL](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**Rysunek 2**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-vb/_static/image4.png))


[![Pobieranie danych produktu przy użyciu metody GetProducts](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Rysunek 3**: pobieranie danych produkt za pomocą `GetProducts` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-vb/_static/image6.png))


Podobnie jak GridView funkcji modyfikacji s ObjectDataSource zostały zaprojektowane do pracy na podstawie na wiersz. Aby zaktualizować zestaw rekordów, będziemy potrzebować do zapisania fragmentem kodu w klasie CodeBehind strony s ASP.NET, która partii dane i przekazuje je do LOGIKI. W związku z tym Ustaw list rozwijanych w ObjectDataSource s aktualizacji, WSTAWIANIA i usuwania karty na (Brak). Kliknij przycisk Zakończ, aby zakończyć działanie kreatora.


[![Ustaw list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Rysunek 4**: Ustaw listy rozwijane w aktualizacji, WSTAWIANIA i usuwania karty (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-vb/_static/image8.png))


Po zakończeniu pracy kreatora Konfigurowanie źródła danych, s ObjectDataSource o oznaczeniu deklaracyjnym powinien wyglądać następująco:


[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

Kończenie pracy kreatora skonfiguruj źródło danych również powoduje, że Visual Studio, aby utworzyć BoundFields i CheckBoxField dla pól danych produktu w widoku GridView. W tym samouczku umożliwiają tylko umożliwia użytkownikowi wyświetlanie i edytowanie Nazwa s produktu, kategorii, ceny i stan nieobsługiwane s. Usuń wszystkie elementy oprócz `ProductName`, `CategoryName`, `UnitPrice`, i `Discontinued` pola i Zmień nazwę `HeaderText` właściwości pierwsze trzy pola produkt, kategoria i ceny, odpowiednio. Na koniec zaznacz włączone stronicowanie i sortowanie Włącz pola wyboru w tagu inteligentnego s GridView.

W tym momencie widoku GridView ma trzy BoundFields (`ProductName`, `CategoryName`, i `UnitPrice`) i CheckBoxField (`Discontinued`). Musimy przekonwertować te cztery pola na kontrolek TemplateField, a następnie przenieść interfejsu edycji z TemplateField s `EditItemTemplate` do jego `ItemTemplate`.

> [!NOTE]
> Rozważyliśmy, tworzenie i Dostosowywanie kontrolek TemplateField w [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) samouczka. Omówimy kroki konwersji BoundFields i CheckBoxField do kontrolek TemplateField i definiowanie ich edycji interfejsy w ich `ItemTemplate` s, ale jeśli pobieranie zablokowana, lub należy odświeżacz don t wahaj się odnoszą się do tego samouczka wcześniej.


Kliknij link Edytuj kolumny, aby otworzyć okno dialogowe pól tagów inteligentnych s GridView. Następnie zaznacz każde pole i kliknij Convert to pole na łącze TemplateField.


![Konwertowanie istniejących BoundFields i CheckBoxField do kontrolek TemplateField](batch-updating-vb/_static/image5.gif)

**Rysunek 5**: Konwertowanie istniejącej BoundFields i CheckBoxField do kontrolek TemplateField


Każde pole jest TemplateField, możemy ponownie gotowe, aby przenieść edycji interfejs z `EditItemTemplate` s `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Krok 2. Tworzenie`ProductName`,`UnitPrice`, i`Discontinued`edycji interfejsów

Tworzenie `ProductName`, `UnitPrice`, i `Discontinued` edycji interfejsy są tematu ten krok i całkiem proste, ponieważ każdy interfejs jest już zdefiniowany w elemencie TemplateField s `EditItemTemplate`. Tworzenie `CategoryName` edycji interfejs jest nieco bardziej skomplikowane, ponieważ musimy utworzyć DropDownList odpowiednich kategorii. To `CategoryName` edytowanie interfejsu, jest opisany w kroku 3.

Let s rozpoczynać `ProductName` TemplateField. Kliknij link Edytuj szablony z tagu inteligentnego s GridView i przejście do `ProductName` TemplateField s `EditItemTemplate`. Wybierz pole tekstowe, skopiuj go do Schowka, a następnie wklej go do `ProductName` TemplateField s `ItemTemplate`. Zmiana TextBox s `ID` właściwość `ProductName`.

Następnie dodaj RequiredFieldValidator do `ItemTemplate` aby upewnić się, że użytkownik udostępnia wartość dla każdej nazwy produktu s. Ustaw `ControlToValidate` Właściwość ProductName, `ErrorMessage` właściwości do Ciebie należy podać nazwę produktu. i `Text` właściwość \*. Po wprowadzeniu te dodatki do `ItemTemplate`, ekran powinien wyglądać podobnie jak rysunek 6.


[![Teraz TemplateField ProductName zawiera pole tekstowe i RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Rysunek 6**: `ProductName` TemplateField zawiera teraz pole tekstowe oraz RequiredFieldValidator ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-vb/_static/image10.png))


Aby uzyskać `UnitPrice` edytowanie interfejsu, zacznij od skopiowania pole tekstowe z `EditItemTemplate` do `ItemTemplate`. Następnie należy umieścić $ przed pole tekstowe i ustaw jego `ID` właściwość UnitPrice i jego `Columns` właściwości do 8.

Również dodać CompareValidator do `UnitPrice` s `ItemTemplate` do upewnij się, że wartości wprowadzone przez użytkownika wartość waluty prawidłowe większe niż lub równe 0,00 USD. Ustaw s modułu sprawdzania poprawności `ControlToValidate` właściwość UnitPrice, jego `ErrorMessage` właściwości do Ciebie należy wprowadzić wartość waluty prawidłowe. Można pominąć wszystkie waluty symboli., jego `Text` właściwości \*, jego `Type` właściwości `Currency`, jego `Operator` właściwości `GreaterThanEqual`i jego `ValueToCompare` właściwości na wartość 0.


[![Dodaj CompareValidator, aby upewnić się, wprowadzone cena to wartość waluty nieujemną.](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Rysunek 7**: Dodaj CompareValidator, aby upewnić się, wprowadzone cena jest wartością nieujemną waluty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-vb/_static/image12.png))


Aby uzyskać `Discontinued` TemplateField można użyć pola wyboru już zdefiniowana w `ItemTemplate`. Wystarczy ustawić dla jego `ID` do wycofany i jego `Enabled` właściwość `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Krok 3: Tworzenie`CategoryName`edytowanie interfejsu

Interfejs edytowania w `CategoryName` TemplateField s `EditItemTemplate` zawiera pole tekstowe, który wyświetla wartość `CategoryName` pola danych. Należy zastąpić tę DropDownList, który wyświetla listę kategorii, możliwe.

> [!NOTE]
> [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) samouczek zawiera bardziej szczegółowe i kompletne dyskusji na temat dostosowywania szablonu, aby uwzględnić metody DropDownList w przeciwieństwie do pola tekstowego. Gdy spełniono opisane w tym miejscu są przedstawione lapidarnie. Dla bardziej przyjrzeć się tworzenie i konfigurowanie kategorii DropDownList, odwołaj się do [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) samouczka.


Przeciągnij kontrolki DropDownList z przybornika do `CategoryName` TemplateField s `ItemTemplate`, ustawiając jego `ID` do `Categories`. Na tym etapie firma Microsoft będzie zazwyczaj Definiowanie kontrolek DROPDOWNLIST s źródła danych za pomocą tagu inteligentnego tworzenia nowego elementu ObjectDataSource. Jednakże, spowoduje to dodanie elementu ObjectDataSource w ramach `ItemTemplate`, co spowoduje wystąpienie kontrolki ObjectDataSource utworzone dla każdego wiersza w widoku GridView. Zamiast tego można pozwolić tworzenia kontrolki ObjectDataSource poza s GridView kontrolek TemplateField s. Zakończ edycję szablonu i przeciągnij kontrolki ObjectDataSource z przybornika w Projektancie pod `ProductsDataSource` ObjectDataSource. Nadaj nazwę nowej kontrolki ObjectDataSource `CategoriesDataSource` i skonfigurować go do używania `CategoriesBLL` klasy s `GetCategories` metody.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy CategoriesBLL](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Rysunek 8**: Konfigurowanie kontrolki ObjectDataSource do użycia `CategoriesBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-vb/_static/image14.png))


[![Pobieranie danych kategorii przy użyciu metody GetCategories](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Rysunek 9**: pobrać za pomocą danych kategorii `GetCategories` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-vb/_static/image16.png))


Ponieważ ten ObjectDataSource jest używany tylko w celu pobrania danych, należy ustawić list rozwijanych w kartach UPDATE i DELETE (Brak). Kliknij przycisk Zakończ, aby zakończyć działanie kreatora.


[![Zestaw list rozwijanych w aktualizacji i usuwania karty (Brak)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Na rysunku nr 10**: Ustaw listy rozwijane w aktualizacji i usuwania karty (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-vb/_static/image18.png))


Po zakończeniu pracy kreatora, `CategoriesDataSource` s oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższego:


[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

Za pomocą `CategoriesDataSource` utworzone i skonfigurowane, wróć do `CategoryName` TemplateField s `ItemTemplate` a DropDownList s tagu inteligentnego, kliknij Link, wybierz źródło danych. W Kreatorze konfiguracji źródła danych wybierz `CategoriesDataSource` opcji z pierwszej listy rozwijanej, a następnie wybrać opcję `CategoryName` używany do wyświetlania i `CategoryID` jako wartość.


[![Powiązywanie metody DropDownList CategoriesDataSource](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Rysunek 11**: powiązywanie kontrolki DropDownList do `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-vb/_static/image20.png))


W tym momencie `Categories` DropDownList Wyświetla listę wszystkich kategorii, ale go jeszcze automatycznie wybierze odpowiednią kategorię dla produktu powiązane z wiersza w widoku GridView. W tym musimy `Categories` DropDownList s `SelectedValue` produktu s `CategoryID` wartość. Kliknij link Edytuj powiązania danych z kontrolki DropDownList tagu inteligentnego s i skojarz `SelectedValue` właściwość o `CategoryID` pole danych, jak pokazano na rysunku 12.


![Wartość CategoryID produktu s należy powiązać właściwości SelectedValue DropDownList s](batch-updating-vb/_static/image12.gif)

**Rysunek 12**: powiązywanie produkt s `CategoryID` wartość s DropDownList `SelectedValue` właściwości


Jeden ostatniego pozostaje problem: Jeśli t produktu `CategoryID` wartość określony instrukcji wiązania z danymi na `SelectedValue` spowodują wyjątek. Jest to spowodowane metody DropDownList zawiera tylko elementy dla kategorii, a nie oferuje opcji dla tych produktów, które mają `NULL` bazy danych wartości `CategoryID`. Aby rozwiązać ten problem, należy ustawić DropDownList s `AppendDataBoundItems` właściwości `True` i Dodaj nowy element do metody DropDownList, pomijając `Value` właściwości ze składni deklaratywnej. Oznacza to, upewnij się, że `Categories` składni deklaratywnej s DropDownList wygląda podobnie do następującej:


[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Uwaga jak `<asp:ListItem Value="">` — wybierz jedną — zawiera jego `Value` atrybut jawnie ustawiony na pusty ciąg. Odwołaj się do [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) samouczek bardziej szczegółowe omówienie dotyczące Dlaczego ten dodatkowy element DropDownList jest wymagana do obsługi `NULL` przypadek i dlaczego przypisywanie `Value` istotne jest właściwość na pusty ciąg.

> [!NOTE]
> Brak potencjalny wydajność i skalowalność problem w tym miejscu, warto zauważyć. Ponieważ każdy wiersz zawiera kontrolki DropDownList, który używa `CategoriesDataSource` jako źródło danych `CategoriesBLL` klasy s `GetCategories` zostanie wywołana metoda *n* odwiedzić razy na każdej stronie, gdzie *n* jest liczbą wierszy w widoku GridView. Te *n* wywołania `GetCategories` spowodować *n* zapytania do bazy danych. Wpływ na bazie danych można zmniejszone, buforując zwrócone kategorie w pamięci podręcznej na żądanie lub przy użyciu warstwy buforowania, przy użyciu języka SQL, buforowania, zależność lub bardzo krótki na podstawie czasu wygaśnięcia. Aby uzyskać więcej informacji na temat danego żądania buforowania opcji, zobacz [ `HttpContext.Items` Store pamięci podręcznej na żądanie](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Krok 4: Kończenie interfejsu edycji

Firma Microsoft ve wprowadzone szereg zmian do szablonów GridView s bez wstrzymywania, aby wyświetlić postępach. Poświęć chwilę, aby wyświetlić postępach za pośrednictwem przeglądarki. Jak pokazano na rysunku 13, każdy wiersz jest renderowany przy użyciu jego `ItemTemplate`, zawierającą s komórki edytowanie interfejsu.


[![Każdy wiersz GridView jest edytowalna](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Rysunek 13**: każdego wiersza w widoku GridView jest edytowalna ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-vb/_static/image22.png))


Brak kilku drobnych kwestii formatowania, które powinny Dbamy o w tym momencie. Po pierwsze należy pamiętać, że `UnitPrice` wartość zawiera cztery separatorów dziesiętnych. Aby rozwiązać ten problem, wróć do `UnitPrice` TemplateField s `ItemTemplate` a tagu inteligentnego s pola tekstowego, kliknij Link Edytuj powiązania danych. Następnie należy określić, że `Text` właściwości powinny być sformatowane jako liczba.


![Format, właściwość tekst jako liczba](batch-updating-vb/_static/image14.gif)

**Rysunek 14**: Format `Text` właściwości jako liczby


Po drugie, umożliwiają s Centrum pola wyboru w `Discontinued` kolumny (zamiast on wyrównany do lewej). Kliknij Edytowanie kolumn z kontrolki GridView tagu inteligentnego s i wybierz pozycję `Discontinued` TemplateField z listy pól w lewym dolnym rogu. Przejdź do szczegółów `ItemStyle` i ustaw `HorizontalAlign` właściwości Centrum, jak pokazano na rysunku 15.


![Centrum nieobsługiwane pola wyboru](batch-updating-vb/_static/image15.gif)

**Rysunek 15**: Centrum `Discontinued` pola wyboru


Następnie na stronie Dodaj kontrolki podsumowania walidacji i ustaw jego `ShowMessageBox` właściwości `True` i jego `ShowSummary` właściwość `False`. Również dodać przycisk w sieci Web, który kontroluje, po kliknięciu, zaktualizuje zmiany s użytkownika. W szczególności należy dodać dwie kontrolki przycisku w sieci Web powyższego widoku GridView i poniższego, ustawienie oba formanty `Text` właściwości, aby aktualizacje produktów.

Ponieważ GridView s edytowanie interfejsu jest zdefiniowany w jej kontrolek TemplateField `ItemTemplate` s, `EditItemTemplate` s są niepotrzebne i mogą zostać usunięte.

Po co powyżej wymienione zmiany formatowania, dodając formanty przycisków i usuwanie niepotrzebnych `EditItemTemplate` s, Twojej składni deklaratywnej strony s powinna wyglądać następująco:


[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

Rysunek 16 pokazuje tej strony, podczas wyświetlania za pośrednictwem przeglądarki, po dodaniu kontrolki przycisku w sieci Web i formatowania zmian.


[![Teraz strona zawiera dwa przyciski produktów Update](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**Rysunek 16**: strona teraz obejmuje dwa aktualizacji produktów przyciski ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-vb/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Krok 5: Aktualizowanie produktów

Użytkownik odwiedzi tę stronę ich modyfikowania a następnie kliknij przycisk jednego z dwóch przycisków aktualizacje produktów. W tym momencie należy zapisać w jakiś sposób wartości wprowadzonych przez użytkownika dla każdego wiersza w `ProductsDataTable` wystąpienia, a następnie przekaż go do metody LOGIKI, która następnie przekaż go `ProductsDataTable` wystąpienia s DAL `UpdateWithTransaction` metody. `UpdateWithTransaction` Metody, które utworzyliśmy w [poprzedni Samouczek](wrapping-database-modifications-within-a-transaction-vb.md), zapewnia, że partii zmian zostaną zaktualizowane jako operację niepodzielną.

Utwórz metodę o nazwie `BatchUpdate` w `BatchUpdate.aspx.vb` i Dodaj następujący kod:


[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Ta metoda rozpoczyna przez pobranie wszystkich produktów w `ProductsDataTable` poprzez wywołanie s LOGIKI `GetProducts` metody. Następnie wylicza `ProductGrid` GridView s [ `Rows` kolekcji](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). `Rows` Kolekcja zawiera [ `GridViewRow` wystąpienia](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) dla każdego wiersza wyświetlane w widoku GridView. Ponieważ firma Microsoft są wyświetlane na maksymalnie dziesięć wierszy na stronie, GridView s `Rows` kolekcja będzie mieć nie więcej niż dziesięć elementów.

Dla każdego wiersza `ProductID` jest pobierany z `DataKeys` zbierania i odpowiednie `ProductsRow` wybrana w zaufanym `ProductsDataTable`. Cztery kontrolki wejściowe TemplateField programowo są określone i ich wartości przypisane do `ProductsRow` wystąpienia właściwości s. Po każdym GridView wartości wierszy s zostały użyte w celu aktualizacji `ProductsDataTable`, jej s przekazany do s LOGIKI `UpdateWithTransaction` metodę, która jako widzieliśmy w poprzednim samouczku, po prostu wywołuje widok w dół do DAL s `UpdateWithTransaction` metody.

Algorytm aktualizacji usługi batch na potrzeby tego samouczka aktualizuje każdego wiersza w `ProductsDataTable` odnosi się do wiersza w widoku GridView, niezależnie od tego, czy informacje o produkcie s został zmieniony. Chociaż takie blind aktualizacje nie są zazwyczaj problem z wydajnością, której jest przeprowadzana inspekcja zmiany do tabeli bazy danych może prowadzić do zbędny rekordów. Ponownie [wykonywanie aktualizacji wsadowych](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) samouczka będziemy zbadano partii Aktualizowanie interfejsu za pomocą kontrolki DataList i dodać kod, który będzie aktualizować jedynie te rekordy, które rzeczywiście zostały zmodyfikowane przez użytkownika. Możesz skorzystać z technik [wykonywanie aktualizacji wsadowych](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) można zaktualizować kod w tym samouczku, w razie potrzeby.

> [!NOTE]
> Podczas tworzenia powiązania ze źródłem danych do kontrolki GridView za pośrednictwem jego tagu inteligentnego, Visual Studio automatycznie przypisuje danych źródła s podstawowej wartości klucza GridView s `DataKeyNames` właściwości. Jeśli użytkownik nie został powiązany kontrolki ObjectDataSource do kontrolki GridView za pośrednictwem tagu inteligentnego s GridView opisany w kroku 1, a następnie należy ręcznie ustawić GridView s `DataKeyNames` właściwości ProductID, aby uzyskać dostęp do `ProductID` wartość dla każdego wiersza za pośrednictwem `DataKeys` kolekcji.


Kod używany w `BatchUpdate` jest podobny do używanych w tym s LOGIKI `UpdateProduct` metody, główną różnicą jest to na `UpdateProduct` metody tylko jeden `ProductRow` wystąpienia są pobierane z architektury. Kod, który przypisuje właściwości `ProductRow` jest taka sama, między `UpdateProducts` metody i kodu w ramach `For Each` pętli w `BatchUpdate`, ponieważ jest ogólny wzorzec.

Do ukończenia tego samouczka, musimy mieć `BatchUpdate` metoda wywoływana, gdy jeden z przycisków aktualizacje produktów po kliknięciu. Utwórz obsługę zdarzeń dla `Click` zdarzenia te dwie kontrolki przycisku i Dodaj następujący kod w procedurze obsługi zdarzeń:


[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

Najpierw następuje wywołanie `BatchUpdate`. Następnie [ `ClientScript` właściwość](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx) służy do iniekcji JavaScript, który wyświetli komunikat messagebox, która odczytuje produkty zostały zaktualizowane.

Nieco potrwać do przetestowania tego kodu. Odwiedź stronę `BatchUpdate.aspx` za pośrednictwem przeglądarki, Edytuj liczbę wierszy i kliknij jeden z przycisków aktualizacje produktów. Przy założeniu, że nie ma żadnych błędów walidacji danych wejściowych, powinien zostać wyświetlony komunikat messagebox, która odczytuje produkty zostały zaktualizowane. Aby sprawdzić, czy niepodzielność aktualizacji, należy rozważyć dodanie losową `CHECK` ograniczenia, takie jak taki, który nie zezwala na `UnitPrice` wartości wartość 1234,56. Następnie z `BatchUpdate.aspx`, edytować wiele rekordów, upewniając się ustawić jeden produkt s `UnitPrice` wartość zabroniona wartość (wartość 1234,56). Powinno to spowodować wystąpienie błędu, gdy kliknięcie aktualizacje produktów z innymi zmianami podczas tej operacji wsadowych z powrotem obniżyć do ich oryginalnych wartości.

## <a name="an-alternativebatchupdatemethod"></a>Zamiast`BatchUpdate`— metoda

`BatchUpdate` Możemy po prostu metodę badania pobiera *wszystkich* produktów z s LOGIKI `GetProducts` metody, a następnie aktualizuje tylko te rekordy, które są wyświetlane w widoku GridView. To podejście jest idealne w przypadku widoku GridView używa stronicowania, ale jeśli istnieje, może być setek, tysięcy lub dziesiątki tysięcy produktów, ale tylko dziesięć wierszy w widoku GridView. W takim przypadku wszystkie produkty z bazy danych tylko do modyfikowania 10 jest mniej niż idealne rozwiązanie.

Dla tych typów sytuacjach należy rozważyć użycie następujących `BatchUpdateAlternate` metody zamiast tego:


[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate` rozpoczyna się od utworzenia nowego pustego `ProductsDataTable` o nazwie `products`. Następnie przeprowadza użytkownika przez proces GridView s `Rows` kolekcji i dla każdego wiersza pobiera informacje o określonym produktem, za pomocą s LOGIKI `GetProductByProductID(productID)` metody. Pobrany `ProductsRow` wystąpienie posiada jego właściwości, aktualizowane w taki sam sposób jak `BatchUpdate`, ale po zaktualizowaniu wiersza są importowane do `products` `ProductsDataTable` za pośrednictwem DataTable s [ `ImportRow(DataRow)` metoda](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Po `For Each` pętli zakończeniu `products` zawiera jeden `ProductsRow` wystąpienia dla każdego wiersza w widoku GridView. Ponieważ każda z `ProductsRow` wystąpienia zostały dodane do `products` (zamiast aktualizacja), jeśli firma Microsoft bezrefleksyjne przekazać go do `UpdateWithTransaction` — metoda `ProductsTableAdatper` podejmie próbę wstawienia poszczególnych rekordów do bazy danych. Zamiast tego należy określić, że każda z tych wierszy została zmodyfikowana (nie dodane).

Można to osiągnąć przez dodanie nowej metody do LOGIKI o nazwie `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, pokazano poniżej, zestawy `RowState` każdego z `ProductsRow` wystąpienia w `ProductsDataTable` do `Modified` i następnie przekazuje `ProductsDataTable` s DAL `UpdateWithTransaction` metody.


[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Podsumowanie

Widoku GridView udostępnia wiersz wbudowane funkcje edycji, ale brakuje obsługi Tworzenie interfejsów pełni edytowalne. Jak widzieliśmy w tym samouczku takie interfejsy są możliwe, ale wymaga nieco pracy. Do utworzenia w kontrolce GridView, gdzie każdy wiersz jest edytowalny, musimy przekonwertować pola s GridView na kontrolek TemplateField i zdefiniuj interfejs edycji, w ramach `ItemTemplate` s. Ponadto należy zaktualizować wszystkie — typ kontrolki przycisku w sieci Web musi zostać dodany do strony, niezależnie od widoku GridView. Przyciski te `Click` procedury obsługi zdarzeń należy wyliczyć GridView s `Rows` zbierania, przechowywanie zmian w `ProductsDataTable`i przekazać zaktualizowane informacje do odpowiedniej metody LOGIKI.

W następnym samouczku Zobaczymy się, jak utworzyć interfejs usuwania usługi batch. W szczególności każdego wiersza w widoku GridView będzie zawierać pola wyboru i zamiast zaktualizowanie wszystkich — wpisz przyciski, będziemy mieć przyciski Usuń wybrane wiersze.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Teresa Murphy i David Suru. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](wrapping-database-modifications-within-a-transaction-vb.md)
> [dalej](batch-deleting-vb.md)
