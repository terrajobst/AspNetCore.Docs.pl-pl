---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
title: Wsadowe aktualizacji (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Dowiedz się, jak zaktualizować wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika budujemy Element GridView każdego wiersza w przypadku edycji. W danych...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 4e849bcc-c557-4bc3-937e-f7453ee87265
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
msc.type: authoredcontent
ms.openlocfilehash: 9f1bad4f0b58175a8437ebfedf161db057bb2bd2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888555"
---
<a name="batch-updating-c"></a>Wsadowe aktualizacji (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_CS.zip) lub [pobierania plików PDF](batch-updating-cs/_static/datatutorial64cs1.pdf)

> Dowiedz się, jak zaktualizować wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika budujemy Element GridView każdego wiersza w przypadku edycji. W warstwie dostępu do danych Firma Microsoft zawijać wiele operacji aktualizacji w ramach transakcji, aby upewnić się, czy wszystkie aktualizacje powiedzie się lub wszystkie aktualizacje są przywracane.


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczek](wrapping-database-modifications-within-a-transaction-cs.md) widzieliśmy jak rozszerzyć Warstwa dostępu do danych, aby dodać obsługę transakcji bazy danych. Transakcji bazy danych gwarantuje, że szereg instrukcji modyfikacji danych będą traktowane jako jedna operacja atomic, który zapewnia, że wszystkie modyfikacje zakończy się niepowodzeniem lub wszystkie powiedzie się. Z tym niskiego poziomu funkcjach warstwy DAL do końca możemy re przygotowanie włączyć wymagające uwagi do tworzenia partii interfejsów modyfikacji danych.

W tym samouczku będziesz budujemy GridView, w którym każdy wiersz jest edytowalny (zobacz rysunek 1). Ponieważ każdy wiersz jest renderowany w interfejsie edycji tam s kolumnę Edycja nie jest konieczne, zaktualizuj i przyciski "Anuluj". Zamiast tego, dostępne są dwa przyciski aktualizacji produktów na stronie, po kliknięciu wyliczyć wierszach widoku GridView i aktualizowania bazy danych.


[![Każdego wiersza w widoku GridView jest edytowalna](batch-updating-cs/_static/image1.gif)](batch-updating-cs/_static/image1.png)

**Rysunek 1**: każdego wiersza w widoku GridView jest edytowalna ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image2.png))


Rozpoczynanie pracy dzięki s!

> [!NOTE]
> W [przeprowadzania aktualizacji wsadowych](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) samouczek utworzyliśmy edycji partii interfejs, za pomocą formant DataList. W tym samouczku różni się od poprzedniej w którym jest używana Element GridView i wykonać aktualizację partii w zakresie transakcji. Po ukończeniu tego samouczka I zachęca do wróć do wcześniejszych samouczka i zaktualizuj go do korzystania z bazy danych dotyczące transakcji funkcji dodanej w poprzednim samouczka.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Badanie kroki dokonywania można edytować wszystkie wiersze w widoku GridView

Zgodnie z opisem w [omówienie Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) samouczka widoku GridView udostępnia wbudowaną obsługę edytowanie jej odpowiednie dane na podstawie na wiersz. Wewnętrznie widoku GridView notatki wiersza można edytować za pomocą jego [ `EditIndex` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Ponieważ w widoku GridView jest związany ze swoim źródłem danych, sprawdza każdego wiersza, aby zobaczyć, jeśli indeks wiersza jest równa wartości `EditIndex`. Jeśli tak, aby wiersz s, który ich edycję pola mają być renderowane interfejsów. BoundFields, interfejs edytowania jest pole tekstowe których `Text` właściwości jest przypisywana wartość pola danych, określonej przez s elementu BoundField `DataField` właściwości. Dla TemplateFields `EditItemTemplate` jest używany zamiast `ItemTemplate`.

Odwołaj, gdy użytkownik kliknie przycisk Edytuj wiersz s rozpoczyna się edytowania przepływu pracy. To powoduje odświeżenie strony, ustawia GridView s `EditIndex` właściwości indeks wiersza klikniętej s i rebinds danych do siatki. Po kliknięciu przycisku Anuluj wiersza s na ogłaszania zwrotnego `EditIndex` ma ustawioną wartość `-1` przed ponownego wiązania danych do siatki. Ponieważ wierszy s GridView uruchomić indeksowania na zero, ustawienie `EditIndex` do `-1` powoduje wyświetlanie widoku GridView w trybie tylko do odczytu.

`EditIndex` Właściwość sprawdza się w przypadku edycji na wiersz, ale nie jest przeznaczony do edycji partii. Aby całego widoku GridView edytowalny, musimy każdy wiersz renderowania przy użyciu jego edycji interfejsu. Najprostszym sposobem, w tym celu jest utworzenie, gdzie każde pole można edytować zaimplementowano zgodnie z definicją TemplateField z jego edycji interfejs w `ItemTemplate`.

W następnych kilku kroków utworzymy całkowicie można edytować widoku GridView. W kroku 1 nasz Rozpocznij od utworzenia widoku GridView i jego ObjectDataSource i konwertować TemplateFields jego BoundFields i w polu CheckBoxField. Kroki 2 i 3 możemy przeniesienie edycji interfejsów z TemplateFields `EditItemTemplate` s, aby ich `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Krok 1: Wyświetlanie informacji o produkcie

Zanim firma Microsoft martwić się o tworzeniu widoku GridView skutkującej wierszy są edytowalne, let s uruchomić po prostu wyświetlanie informacji o produkcie. Otwórz `BatchUpdate.aspx` strony `BatchData` folder i przeciągnij element GridView z przybornika do projektanta. Ustaw GridView s `ID` do `ProductsGrid` i z jego tagów inteligentnych, wybierz powiązać nowy element ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj ObjectDataSource można pobrać danych z `ProductsBLL` klasy s `GetProducts` metody.


[![Skonfiguruj ObjectDataSource do użycia klasy ProductsBLL](batch-updating-cs/_static/image2.gif)](batch-updating-cs/_static/image3.png)

**Rysunek 2**: Konfigurowanie ObjectDataSource użyć `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image4.png))


[![Pobieranie danych produktu przy użyciu metody GetProducts](batch-updating-cs/_static/image3.gif)](batch-updating-cs/_static/image5.png)

**Rysunek 3**: pobieranie danych produkt za pomocą `GetProducts` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image6.png))


Jak GridView funkcji modyfikacji s ObjectDataSource są przeznaczone do pracy na podstawie na wiersz. Aby można było zaktualizować zestawu rekordów, potrzebujemy zapisać fragmentem kodu w klasie związanej z kodem strony ASP.NET, partii danych i przekazująca je do logiki warstwy Biznesowej. W związku z tym Ustaw list rozwijanych w elemencie ObjectDataSource s UPDATE, INSERT i DELETE karty na (Brak). Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Ustawianie list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak)](batch-updating-cs/_static/image4.gif)](batch-updating-cs/_static/image7.png)

**Rysunek 4**: wartość listy rozwijane w AKTUALIZOWANIA, WSTAWIANIA i usuwanie kart (None) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image8.png))


Po zakończeniu pracy Kreatora konfigurowania źródła danych, znaczników deklaratywne s ObjectDataSource powinien wyglądać następująco:


[!code-aspx[Main](batch-updating-cs/samples/sample1.aspx)]

Kończenie pracy Kreatora konfigurowania źródła danych powoduje także, że Visual Studio, aby utworzyć BoundFields i CheckBoxField dla pól danych produktu w widoku GridView. W tym samouczku umożliwiają s tylko Zezwalaj użytkownikom na wyświetlanie i edytowanie nazwy s produktu, kategoria, ceny i stan wycofane. Usuń wszystkie elementy oprócz `ProductName`, `CategoryName`, `UnitPrice`, i `Discontinued` pól i Zmień nazwę `HeaderText` właściwości pierwsze trzy pola odpowiednio do produktu, kategorii i cenę. Sprawdź też, Włącz stronicowanie i włączyć sortowanie pola wyboru w widoku GridView tag inteligentny s.

W tym momencie widoku GridView ma trzy BoundFields (`ProductName`, `CategoryName`, i `UnitPrice`) i CheckBoxField (`Discontinued`). Musimy przekonwertować czterech pól na TemplateFields, a następnie przesuń interfejs edytowania z TemplateField s `EditItemTemplate` do jego `ItemTemplate`.

> [!NOTE]
> Firma Microsoft zbadane, tworzenie i dostosowywanie TemplateFields w [Dostosowywanie interfejs modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) samouczka. Zostanie omówiony kroki konwertowania BoundFields i CheckBoxField na TemplateFields i definiowanie ich edytowanie interfejsy w ich `ItemTemplate` s, ale jeśli zostać zablokowane lub wymagają odświeżenia, ADAM t się odwołują się do tego samouczka wcześniejszych.


Z widoku GridView s tagu kliknij łącze Edytowanie kolumn, aby otworzyć okno dialogowe pola. Następnie wybierz każdego pola i kliknij przycisk Konwertuj to pole na pole TemplateField łącze.


![Przekonwertuj istniejących BoundFields i CheckBoxField do TemplateField](batch-updating-cs/_static/image5.gif)

**Rysunek 5**: przekonwertować istniejących BoundFields i CheckBoxField na pole TemplateField


Teraz, każde pole jest TemplateField, możemy re wszystko gotowe do przeniesienia edycji interfejsu z `EditItemTemplate` s do `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Krok 2: Tworzenie`ProductName`,`UnitPrice`, i`Discontinued`edycji interfejsów

Tworzenie `ProductName`, `UnitPrice`, i `Discontinued` edycji interfejsy są tematu tego kroku i są bardzo proste, jak każdy interfejs jest już zdefiniowany w elemencie TemplateField s `EditItemTemplate`. Tworzenie `CategoryName` edycji interfejsu jest nieco bardziej skomplikowane, ponieważ należy utworzyć DropDownList odpowiednich kategorii. To `CategoryName` edycji interfejsu, jest opisany w kroku 3.

Let s rozpoczynać `ProductName` TemplateField. Kliknij łącze Edytuj szablony z widoku GridView tag inteligentny s, a następnie Drąż w dół do `ProductName` TemplateField s `EditItemTemplate`. Wybierz pole tekstowe, skopiuj go do Schowka, a następnie wklej go do `ProductName` TemplateField s `ItemTemplate`. Zmień s pole tekstowe `ID` właściwości `ProductName`.

Następnie dodaj RequiredFieldValidator do `ItemTemplate` aby upewnić się, że użytkownik zawiera wartość dla każdej nazwy produktu s. Ustaw `ControlToValidate` właściwości NazwaProduktu, `ErrorMessage` właściwości należy podać nazwę produktu. i `Text` właściwości \*. Po wprowadzeniu tych dodatków do `ItemTemplate`, ekranu powinien wyglądać podobnie do rysunek 6.


[![Teraz TemplateField ProductName zawiera pole tekstowe i RequiredFieldValidator](batch-updating-cs/_static/image6.gif)](batch-updating-cs/_static/image9.png)

**Rysunek 6**: `ProductName` TemplateField zawiera teraz pole tekstowe i RequiredFieldValidator ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image10.png))


Aby uzyskać `UnitPrice` edycji interfejsu, Rozpocznij od kopiowanie pole tekstowe z `EditItemTemplate` do `ItemTemplate`. Następnie należy umieścić $ przed pole tekstowe i ustaw jego `ID` właściwości UnitPrice i jego `Columns` właściwości do 8.

Również dodać CompareValidator do `UnitPrice` s `ItemTemplate` w celu zapewnienia prawidłową walutę wartość większa niż lub równa 0,00 wartości wprowadzonej przez użytkownika. Ustaw s modułu sprawdzania poprawności `ControlToValidate` właściwości UnitPrice, jego `ErrorMessage` właściwości można wprowadzić wartość waluty prawidłowe. Można pominąć wszystkie waluty symboli., jego `Text` właściwości \*, jego `Type` właściwości `Currency`, jego `Operator` właściwości `GreaterThanEqual`i jego `ValueToCompare` właściwości na 0.


[![Dodaj CompareValidator zapewnienie wprowadzona cen to wartość waluty nieujemną.](batch-updating-cs/_static/image7.gif)](batch-updating-cs/_static/image11.png)

**Rysunek 7**: Dodaj CompareValidator zapewnienie wprowadzona cen jest wartość waluty nieujemną ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image12.png))


Aby uzyskać `Discontinued` TemplateField można użyć pola wyboru już zdefiniowana w `ItemTemplate`. Wystarczy ustawić jej `ID` do wycofany i jego `Enabled` właściwości `true`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Krok 3: Tworzenie`CategoryName`edycji — interfejs

Interfejs edytowania w `CategoryName` TemplateField s `EditItemTemplate` zawiera pole tekstowe, który zawiera wartość `CategoryName` pola danych. Należy zastąpić tę DropDownList, który zawiera listę możliwych kategorii.

> [!NOTE]
> [Dostosowywanie interfejs modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) samouczek zawiera bardziej dokładne i kompletne omówione dostosowywania szablonu, aby uwzględnić DropDownList w przeciwieństwie do pola tekstowego. Gdy zakończeniu kroków w tym miejscu są przedstawione lapidarnie. Na pełniejsze przyjrzeć się tworzeniem i konfigurowaniem kategorie lista DropDownList, odwołaj się do [Dostosowywanie interfejs modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) samouczka.


Przeciągnij z przybornika do DropDownList `CategoryName` TemplateField s `ItemTemplate`, ustawienie jej `ID` do `Categories`. Na tym etapie firma Microsoft będzie zazwyczaj zdefiniowanie DropDownLists s źródła danych za pośrednictwem jego tagów inteligentnych Tworzenie nowego elementu ObjectDataSource. Jednakże, spowoduje to dodanie ObjectDataSource w `ItemTemplate`, co spowoduje wystąpienie elementu ObjectDataSource utworzone dla każdego wiersza w widoku GridView. Zamiast tego można pozwolić utworzyć ObjectDataSource poza GridView s TemplateFields s. Zakończyć edytowanie szablonu i przeciągnij element ObjectDataSource z przybornika do projektanta poniżej `ProductsDataSource` ObjectDataSource. Nazwa nowego elementu ObjectDataSource `CategoriesDataSource` i skonfigurować go do używania `CategoriesBLL` klasy s `GetCategories` metody.


[![Skonfiguruj ObjectDataSource do użycia CategoriesBLL Clas](batch-updating-cs/_static/image8.gif)](batch-updating-cs/_static/image13.png)

**Rysunek 8**: Konfigurowanie ObjectDataSource użyć `CategoriesBLL` Clas ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image14.png))


[![Pobieranie danych kategorii przy użyciu metody GetCategories](batch-updating-cs/_static/image9.gif)](batch-updating-cs/_static/image15.png)

**Rysunek 9**: pobrać za pomocą danych kategorii `GetCategories` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image16.png))


Ponieważ ten element ObjectDataSource jest używany tylko w celu pobrania danych, należy ustawić list rozwijanych w kartach UPDATE i DELETE na (Brak). Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Zestaw list rozwijanych w aktualizacjach UPDATE i DELETE karty na (Brak)](batch-updating-cs/_static/image10.gif)](batch-updating-cs/_static/image17.png)

**Na rysunku nr 10**: Ustaw listy rozwijane w aktualizacja i usuwanie kart na (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image18.png))


Po zakończeniu pracy kreatora, `CategoriesDataSource` znaczników deklaratywne s powinna wyglądać następująco:


[!code-aspx[Main](batch-updating-cs/samples/sample2.aspx)]

Z `CategoriesDataSource` utworzone i skonfigurowane, wróć do `CategoryName` TemplateField s `ItemTemplate` i tagów inteligentnych s DropDownList kliknij Link, wybierz źródło danych. W Kreatorze konfiguracji źródła danych, wybierz `CategoriesDataSource` opcję z pierwszej listy rozwijanej i wybrać opcję `CategoryName` używany do wyświetlania i `CategoryID` jako wartość.


[![Powiązać DropDownList CategoriesDataSource](batch-updating-cs/_static/image11.gif)](batch-updating-cs/_static/image19.png)

**Rysunek 11**: powiązać DropDownList do `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image20.png))


W tym momencie `Categories` DropDownList zawiera listę wszystkich kategorii, ale go nie zostały jeszcze automatycznie wybierz odpowiednią kategorię produktu powiązany z wiersza elementu GridView. Należy ustawić w tym celu `Categories` DropDownList s `SelectedValue` produktu s `CategoryID` wartość. Kliknij łącze Edytuj powiązania DataBindings z tagów inteligentnych s DropDownList i skojarz `SelectedValue` właściwości o `CategoryID` pola danych, jak pokazano na rysunku 12.


![Powiązać wartości CategoryID produktu s właściwości SelectedValue DropDownList s](batch-updating-cs/_static/image12.gif)

**Rysunek 12**: powiązać produktu s `CategoryID` wartość s DropDownList `SelectedValue` właściwości


Jeden ostatniego pozostaje problem: Jeśli t produktu `CategoryID` określona wartość następnie instrukcji wiązania z danymi na `SelectedValue` spowoduje wygenerowanie wyjątku. Wynika to z faktu DropDownList zawiera tylko elementy dla kategorii i nie oferuje opcja dla tych produktów, które mają `NULL` bazy danych wartości `CategoryID`. Aby rozwiązać ten problem, ustaw DropDownList s `AppendDataBoundItems` właściwości `true` i Dodaj nowy element do DropDownList, pomijając `Value` właściwość ze składni deklaratywnej. Oznacza to, upewnij się, że `Categories` składni deklaratywnej s DropDownList wygląda podobnie do następującej:


[!code-aspx[Main](batch-updating-cs/samples/sample3.aspx)]

Uwaga jak `<asp:ListItem Value="">` — wybierz jedną — zawiera jego `Value` atrybutu jawnie ustawiona na pusty ciąg. Odwołaj się do [Dostosowywanie interfejs modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) samouczka bardziej szczegółowe omówienie na dlaczego tego dodatkowe elementu DropDownList jest potrzebne do obsługi `NULL` przypadku i dlaczego przypisanie `Value` istotne jest pusty ciąg dla właściwości.

> [!NOTE]
> Istnieje potencjalne wydajności i skalowalności problem tutaj który warto zauważyć. Ponieważ każdy wiersz zawiera DropDownList, która używa `CategoriesDataSource` jako źródło danych, `CategoriesBLL` klasy s `GetCategories` metoda zostanie wywołana *n* razy na stronie odwiedzać, gdzie *n* jest liczbą wierszy w widoku GridView. Te *n* wywołań `GetCategories` spowodować *n* zapytania do bazy danych. Tej wpływa na bazie danych może to buforując zwrócony kategorii w pamięci podręcznej na żądanie lub przez warstwę buforowanie przy użyciu SQL buforowanie zależności lub bardzo krótkim na podstawie czasu wygaśnięcia. Aby uzyskać więcej informacji na żądanie na buforowanie opcji, zobacz [ `HttpContext.Items` magazynu pamięci podręcznej na żądanie](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Krok 4: Kończenie edycji interfejsu

Firma Microsoft Zapisz wprowadzone liczbę zmian w szablonach s GridView bez wstrzymywania, aby wyświetlić postęp naszych. Poświęć chwilę, aby wyświetlić postęp naszych za pośrednictwem przeglądarki. Jak pokazano na rysunku 13, każdy wiersz jest renderowany przy użyciu jego `ItemTemplate`, zawierającą s komórki edycji interfejsu.


[![Każdego wiersza w widoku GridView jest edytowalna](batch-updating-cs/_static/image13.gif)](batch-updating-cs/_static/image21.png)

**Rysunek 13**: każdego wiersza w widoku GridView jest edytowalna ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image22.png))


Istnieje kilka drobne problemy formatowania, które powinien Dbamy o w tym momencie. Najpierw należy pamiętać, że `UnitPrice` wartość zawiera cztery miejsc dziesiętnych. Aby rozwiązać ten problem, wróć do `UnitPrice` TemplateField s `ItemTemplate` i z tagów inteligentnych s pola tekstowego, kliknij łącze Edytuj powiązania danych. Następnie należy określić, że `Text` właściwości powinien być sformatowany jako liczby.


![Właściwość tekstu w formacie numeru](batch-updating-cs/_static/image14.gif)

**Rysunek 14**: Format `Text` właściwości jako liczby


Drugie let s Centrum checkbox w `Discontinued` kolumny (zamiast jego wyrównane do lewej). Kliknij Edytuj kolumny w widoku GridView tag inteligentny s i wybierz pozycję `Discontinued` TemplateField z listy pól w lewym dolnym rogu. Przejdź do szczegółów `ItemStyle` i ustaw `HorizontalAlign` właściwości Centrum, jak pokazano na rys. 15.


![Centrum wycofane wyboru](batch-updating-cs/_static/image15.gif)

**Rysunek 15**: Center `Discontinued` wyboru


Następnie na stronie Dodaj formant ValidationSummary i ustawić jej `ShowMessageBox` właściwości `true` i jego `ShowSummary` właściwości `false`. Również dodać Web przycisk formantów, które, po kliknięciu, nastąpi aktualizacja zmiany s użytkownika. W szczególności należy dodać dwóch formantów sieci Web przycisk jeden nad widoku GridView i jeden poniżej zarówno kontroluje `Text` właściwości do aktualizacji produktów.

Ponieważ GridView s edycji interfejsu jest zdefiniowany w jego TemplateFields `ItemTemplate` s, `EditItemTemplate` s są zbędny i mogą zostać usunięte.

Po co powyższych wymienione zmiany formatowania, dodawanie kontrolek przycisków i usuwanie niepotrzebne `EditItemTemplate` s, składni deklaratywnej Twojej stronie s powinna wyglądać następująco:


[!code-aspx[Main](batch-updating-cs/samples/sample4.aspx)]

Rysunek 16 zawiera tę stronę, podczas wyświetlania za pośrednictwem przeglądarki, po dodaniu formantów sieci Web przycisk i formatowania zmian.


[![Teraz strony zawiera dwa przyciski produktów aktualizacji](batch-updating-cs/_static/image16.gif)](batch-updating-cs/_static/image23.png)

**Rysunek 16**: strona teraz obejmuje dwa aktualizacji produktów przyciski ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-updating-cs/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Krok 5: Aktualizowanie produktów

Gdy użytkownik odwiedzi tę stronę one będzie wprowadzić swoje zmiany, a następnie kliknij jeden z dwóch przycisków aktualizacji produktów. W takiej sytuacji należy zapisać w jakiś sposób wprowadzonych przez użytkownika wartości dla każdego wiersza w `ProductsDataTable` wystąpienia, a następnie przekazać który metodę logiki warstwy Biznesowej, który następnie przekazuje `ProductsDataTable` wystąpienia s DAL `UpdateWithTransaction` metody. `UpdateWithTransaction` Metodę, która utworzyliśmy w [poprzedniego samouczek](wrapping-database-modifications-within-a-transaction-cs.md), zapewnia aktualizowanie partii zmiany jako operacją niepodzielną.

Utwórz metodę o nazwie `BatchUpdate` w `BatchUpdate.aspx.cs` i Dodaj następujący kod:


[!code-csharp[Main](batch-updating-cs/samples/sample5.cs)]

Ta metoda rozpoczyna się od pobrania wszystkich produktów w `ProductsDataTable` rozmów na s logiki warstwy Biznesowej `GetProducts` metody. Następnie wylicza `ProductGrid` GridView s [ `Rows` kolekcji](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). `Rows` Kolekcja zawiera [ `GridViewRow` wystąpienia](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) dla każdego wiersza wyświetlany w widoku GridView. Ponieważ firma Microsoft są wyświetlane maksymalnie dziesięciu wierszy na stronie, GridView s `Rows` kolekcji nie odniesie nie więcej niż dziesięć elementów.

Dla każdego wiersza `ProductID` jest pobierany z `DataKeys` kolekcji i odpowiednie `ProductsRow` wybrano `ProductsDataTable`. Programowo odwołuje się cztery kontrolki wejściowe TemplateField i ich wartości przypisane do `ProductsRow` wystąpienia właściwości s. Po każdym widoku GridView wartości wierszy s zostały już użyte do zaktualizowania `ProductsDataTable`, go s przekazany do s logiki warstwy Biznesowej `UpdateWithTransaction` metodę, która jako widzieliśmy w poprzednim samouczek, po prostu wywołuje w dół do DAL s `UpdateWithTransaction` metody.

Algorytm aktualizacji partii używany na potrzeby tego samouczka aktualizuje każdego wiersza w `ProductsDataTable` odpowiadającej wiersza w widoku GridView, niezależnie od tego, czy zmieniono informacji o produkcie s. Gdy blind takie aktualizacje nie są zazwyczaj problem z wydajnością, może prowadzić do rekordów zbędny w przypadku której inspekcja zmiany do tabeli bazy danych. W [przeprowadzania aktualizacji wsadowych](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) samouczku będziemy przedstawione partii aktualizowania interfejsu za pomocą elementu DataList i dodaje kod, który zaktualizuje tylko te rekordy, które faktycznie zostały zmodyfikowane przez użytkownika. Możesz korzystać z metod z [przeprowadzania aktualizacji wsadowych](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) można zaktualizować kodu w tym samouczku, w razie potrzeby.

> [!NOTE]
> Podczas wiązania ze źródłem danych widoku GridView za pośrednictwem jego tagów inteligentnych, Visual Studio automatycznie przypisuje danych źródła s głównej wartości klucza do widoku GridView s `DataKeyNames` właściwości. Jeśli użytkownik nie został powiązany element ObjectDataSource do widoku GridView za pomocą tagów inteligentnych s GridView w sposób opisany w kroku 1, a następnie konieczne będzie ręczne ustawienie GridView s `DataKeyNames` ProductID w celu uzyskania dostępu do właściwości `ProductID` wartość dla każdego wiersza za pomocą `DataKeys` kolekcji.


Kod używany w `BatchUpdate` przebiega podobnie jak w s logiki warstwy Biznesowej `UpdateProduct` metody, główną różnicą jest, że w `UpdateProduct` metody tylko jeden `ProductRow` wystąpienia są pobierane z architektury. Kod, który przypisuje właściwości `ProductRow` jest taka sama między `UpdateProducts` metody i kod w `foreach` pętli w `BatchUpdate`, ponieważ jest ogólny model.

Do ukończenia tego samouczka, musimy mieć `BatchUpdate` metoda wywoływana, gdy jeden z przycisków produktów aktualizacji zostanie kliknięty. Tworzenie obsługi zdarzeń dla `Click` zdarzenia tych dwóch przycisk i Dodaj następujący kod w obsłudze zdarzeń:


[!code-csharp[Main](batch-updating-cs/samples/sample6.cs)]

Najpierw połączenie jest nawiązywane w przypadku `BatchUpdate`. Następnie `ClientScript property` służy do dodania JavaScript wyświetlające element messagebox odczytujący produkty zostały zaktualizowane.

Zabrać kilka minut, aby przetestować ten kod. Odwiedź stronę `BatchUpdate.aspx` za pośrednictwem przeglądarki, Edytuj liczbę wierszy i kliknij jeden z przycisków aktualizacji produktów. Zakładając, że nie ma żadnych błędów sprawdzania poprawności danych wejściowych, powinien zostać wyświetlony element messagebox odczytujący się, że produkty zostały zaktualizowane. Aby sprawdzić niepodzielność aktualizacji, należy rozważyć dodanie losowe `CHECK` ograniczenia, takie jak, który nie zezwala na `UnitPrice` wartości wartość 1234,56. Następnie z `BatchUpdate.aspx`, edytować wiele rekordów, upewniając się ustawić jeden produkt s `UnitPrice` niedozwolonych wartość (wartość 1234,56). Powinno to spowodować błąd po kliknięciu aktualizacji produktów z innych zmian podczas tej operacji zbiorczej z powrotem obniżyć do ich oryginalnych wartości.

## <a name="an-alternativebatchupdatemethod"></a>Zamiast`BatchUpdate`— metoda

`BatchUpdate` Metody możemy just zbadane pobiera *wszystkie* produktów z s logiki warstwy Biznesowej `GetProducts` metody, a następnie aktualizuje tylko te rekordy, które są wyświetlane w widoku GridView. Takie podejście jest idealny w widoku GridView nie używa stronicowania, ale jeśli tak, może istnieć setki tysięcy i dziesiątki tysięcy produktów, ale tylko dziesięć wierszy w widoku GridView. W takim przypadku pobieranie wszystkich produktów z bazy danych tylko do modyfikowania 10 z nich jest mniejsza niż idealny.

Dla tych typów sytuacjach należy rozważyć użycie następujących `BatchUpdateAlternate` metody zamiast tego:


[!code-csharp[Main](batch-updating-cs/samples/sample7.cs)]

`BatchMethodAlternate` Uruchamia, tworząc nowe puste `ProductsDataTable` o nazwie `products`. Następnie kroki do widoku GridView s `Rows` kolekcji i dla każdego wiersza pobiera informacje o określonym produktem przy użyciu logiki warstwy Biznesowej s `GetProductByProductID(productID)` metody. Pobranej `ProductsRow` wystąpienie ma właściwości zaktualizowane w taki sam sposób jak `BatchUpdate`, ale po zaktualizowaniu wiersza jest importowany do `products``ProductsDataTable` za pośrednictwem DataTable s [ `ImportRow(DataRow)` — metoda](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Po `foreach` pętli zakończeniu `products` zawiera jeden `ProductsRow` wystąpienia dla każdego wiersza w widoku GridView. Ponieważ każdy z `ProductsRow` wystąpienia zostały dodane do `products` (zamiast aktualizacji), jeśli ślepo jest przekazywana do `UpdateWithTransaction` — metoda `ProductsTableAdatper` podejmie próbę wstawienia każdego rekordu do bazy danych. Zamiast tego należy określić, że każdy z tych wierszy została zmodyfikowana (nie dodany).

Można to osiągnąć przez dodanie nowej metody do logiki warstwy Biznesowej o nazwie `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, pokazano poniżej, ustawia `RowState` poszczególnych `ProductsRow` wystąpień w `ProductsDataTable` do `Modified` , a następnie przekazuje `ProductsDataTable` s DAL `UpdateWithTransaction` — metoda.


[!code-csharp[Main](batch-updating-cs/samples/sample8.cs)]

## <a name="summary"></a>Podsumowanie

Widoku GridView udostępnia funkcje edycji wiersza wbudowanych, ale nie ma obsługi tworzenia interfejsów pełni edytowalne. Jak widzieliśmy w tym samouczku interfejsy są możliwe, ale wymaga nieco pracy. Aby utworzyć element GridView, gdzie każdy wiersz jest edytowalny, musimy przekonwertować pola s GridView TemplateFields i zdefiniować edycji interfejs w ramach `ItemTemplate` s. Ponadto aktualizacji wszystkich — typ kontrolki przycisku w sieci Web musi zostać dodany do strony, niezależnie od widoku GridView. Tych przycisków `Click` procedury obsługi zdarzeń należy wyliczyć GridView s `Rows` kolekcji, zapisać zmiany w `ProductsDataTable`, a następnie przekaż zaktualizowane informacje do odpowiedniej metody logiki warstwy Biznesowej.

W następnym samouczku przedstawiono będzie jak utworzyć interfejs usuwania partii. W szczególności każdego wiersza w widoku GridView będzie zawierać pola wyboru i zamiast Aktualizuj wszystkie — typ przycisków, firma Microsoft zapewnia przycisków usunąć zaznaczone wiersze.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Teresa Murphy i Suru Dominika. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](wrapping-database-modifications-within-a-transaction-cs.md)
> [dalej](batch-deleting-cs.md)
