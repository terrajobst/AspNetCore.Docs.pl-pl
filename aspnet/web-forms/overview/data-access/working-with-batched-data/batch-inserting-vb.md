---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: Wsadowe Wstawianie (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: "Dowiedz się, jak można wstawić wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika rozbudowujemy widoku GridView, aby umożliwić użytkownikom wprowadzanie wielu n..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: e1e77dde4602350b18508bf5d71dbcd953f8961c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="batch-inserting-vb"></a>Wsadowe Wstawianie (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip) lub [pobierania plików PDF](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> Dowiedz się, jak można wstawić wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika rozbudowujemy widoku GridView umożliwia użytkownikowi wprowadzenie wiele nowych rekordów. W warstwie dostępu do danych Firma Microsoft zawijać wiele operacji wstawiania w ramach transakcji do zapewnienia, że wszystkie wstawienia powiedzie się lub wycofać wszystkie wstawienia.


## <a name="introduction"></a>Wprowadzenie

W [wsadowe aktualizacji](batch-updating-vb.md) samouczek analizujemy Dostosowywanie formantu widoku GridView do prezentowania interfejsu, gdzie zostały można edytować wiele rekordów. Użytkownika, odwiedzając stronę można dokonywania kolejnych zmian a następnie, za pomocą kliknięcia przycisku pojedynczego przeprowadzić aktualizację partii. W sytuacjach, w którym użytkownicy często aktualizować wielu rekordów w jednym Przejdź takiego interfejsu można zapisać niezliczonych kliknięć i przełączenia kontekstu Mysz klawiatury w porównaniu do domyślnej na wierszu funkcje edycji, które zostały najpierw przedstawione w [ Omówienie Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) samouczka.

To pojęcie również będą stosowane, gdy dodawanie rekordów. Załóżmy, że w tym miejscu w firmie Northwind Traders często Otrzymaliśmy wysyłek od dostawców, które zawierają wiele produktów dla określonej kategorii. Na przykład może Otrzymaliśmy wydania sześciu różnych zepołowy i produktów kawy z Tokio Traders. Jeśli użytkownik wprowadzi sześciu produktów co jednocześnie za pomocą formantu widoku DetailsView, będzie musiał wybrać wiele takich samych wartości wielokrotnie: należy wybrać w tej samej kategorii (napoje) tego samego dostawcy (Tokio Traders), zaprzestać takie same (wartość FAŁSZ) i tej samej jednostki na wartość kolejności (0). Ten wpis powtarzających się danych nie jest tylko czasochłonne, ale jest podatne na błędy.

Z małego wysiłku można utworzyć partię Wstawianie interfejs, który umożliwia użytkownikowi na wybranie dostawcy i kategorii, wprowadź serii nazw produktów i ceny jednostki, a następnie kliknij przycisk, aby dodać nowe produkty w bazie danych (zobacz rysunek 1). Po dodaniu każdego produktu, jego `ProductName` i `UnitPrice` pola danych są przypisane wartości wprowadzone w tych polach tekstowych podczas jego `CategoryID` i `SupplierID` wartości zostały przypisane wartości z DropDownLists w górnym fo formularza. `Discontinued` i `UnitsOnOrder` wartości są ustawiane na wartości stałe `False` i 0, odpowiednio.


[![Interfejs Wstawianie wsadowe](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**Rysunek 1**: interfejs Wstawianie wsadowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image3.png))


W tym samouczku utworzymy strona, która implementuje partii Wstawianie interfejsu pokazany na rysunku 1. Jako z poprzednich dwóch samouczki, firma Microsoft będzie zawijany wstawienia w zakresie transakcji w celu zapewnienia niepodzielność. Rozpoczynanie pracy dzięki s!

## <a name="step-1-creating-the-display-interface"></a>Krok 1: Tworzenie interfejsu wyświetlania

W tym samouczku będzie składać się z jednej strony, który jest podzielony na dwie części: obszar wyświetlania i Wstawianie regionu. Wyświetlanie interfejsu, która zostanie utworzone w tym kroku, zawiera produkty, w widoku GridView i zawiera przycisk zatytułowany wydania produktu procesu. Po kliknięciu tego przycisku interfejsu wyświetlana jest zastępowany Wstawianie interfejsu, co przedstawiono na rysunku 1. Interfejs wyświetlania zwraca po Dodawanie produktów z wydania lub kliknięciu przycisku Anuluj. Utworzymy Wstawianie interfejsu w kroku 2.

Podczas tworzenia strony, która ma dwa interfejsy, z których tylko jedna jest widoczny w czasie, każdy interfejs zazwyczaj znajduje się w obrębie [Panel Web sterowania](http://www.w3schools.com/aspnet/control_panel.asp), która służy jako kontener dla innych formantów. W związku z tym naszą stronę ma dwa formanty panelu jednej dla każdego interfejsu.

Uruchamianie przez otwarcie `BatchInsert.aspx` strony `BatchData` folder i przeciągnij Panel z przybornika do projektanta (patrz rysunek 2). Ustawianie panelu s `ID` właściwości `DisplayInterface`. Podczas dodawania panelu do projektanta, jego `Height` i `Width` właściwości są ustawione na 50 pikseli i 125px, odpowiednio. Czyści wartości tych właściwości w oknie właściwości.


[![Przeciągnij z przybornika do projektanta panelu](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**Rysunek 2**: przeciągnij z przybornika do projektanta panelu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image6.png))


Następnie przeciągnij formant przycisku i GridView do panelu. Ustawianie przycisku s `ID` właściwości `ProcessShipment` i jego `Text` właściwości do wydania produktu procesu. Ustaw GridView s `ID` właściwości `ProductsGrid` i z jego tagów inteligentnych powiązać go z nowego elementu ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj ObjectDataSource jego dane z `ProductsBLL` klasy s `GetProducts` metody. Ponieważ tego widoku GridView jest używany tylko do wyświetlania danych, ustaw listy rozwijane w UPDATE, INSERT i usuwanie kart na (Brak). Kliknij przycisk Zakończ, aby zakończyć pracę Kreatora konfigurowania źródła danych.


[![Wyświetlanie danych zwróconych przez metodę GetProducts klasy ProductsBLL s](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**Rysunek 3**: Wyświetl dane zwrócone z `ProductsBLL` klasy s `GetProducts` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image9.png))


[![Ustawianie list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak)](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**Rysunek 4**: wartość listy rozwijane w AKTUALIZOWANIA, WSTAWIANIA i usuwanie kart (None) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image12.png))


Po zakończeniu pracy Kreatora ObjectDataSource Visual Studio spowoduje dodanie BoundFields i CheckBoxField dla pól danych produktu. Usuń wszystkie elementy oprócz `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, i `Discontinued` pól. Możesz także zmienić wszystkie dostosowania estetycznych. I decyzję o formacie `UnitPrice` pole jako wartość walutową kolejności pól i zmieniona kilka pól `HeaderText` wartości. Skonfiguruj również GridView stronicowania i sortowania pomocy technicznej, sprawdzając włączenia stronicowania i włączyć sortowanie pola wyboru w widoku GridView tag inteligentny s.

Po dodaniu kontrolki panelu, przycisk widoku GridView i ObjectDataSource i dostosowywanie pól s GridView, Twoje s deklaratywne znaczników powinien wyglądać podobny do następującego:


[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

Należy pamiętać, że kod znaczników dla przycisku i GridView są umieszczone w nawiasach otwarcia i zamknięcia `<asp:Panel>` tagów. Ponieważ w ramach tych kontrolek `DisplayInterface` panelu, firma Microsoft może je ukryć wystarczy wybrać ustawienie panelu s `Visible` właściwości `False`. Krok 3 analizuje programowo, zmieniając panelu s `Visible` właściwości w odpowiedzi na przycisku kliknij, aby pokazać jeden interfejs innych są ukryte.

Poświęć chwilę, aby wyświetlić postęp naszych za pośrednictwem przeglądarki. Jak pokazano na rysunku nr 5, powinien być widoczny przycisk wydania produktu procesu powyżej widoku GridView, który zawiera listę produktów dziesięć w czasie.


[![Widoku GridView zawiera listę produktów i umożliwia sortowanie i stronicowanie możliwości](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**Rysunek 5**: widoku GridView zawiera listę produktów i umożliwia sortowanie i stronicowanie możliwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>Krok 2: Tworzenie interfejsu Wstawianie

Wyświetl interfejs zakończone, możemy re gotowy do utworzenia Wstawianie interfejsu. W tym samouczku umożliwiają tworzenie Wstawianie interfejs, który wyświetla monit o pojedynczej wartości kategorii i dostawcy, a następnie umożliwia użytkownikowi wprowadzenie do pięciu produktu nazwy i wartości cen jednostki s. Z tym interfejsem użytkownika można dodać do pięciu nowych produktów, wszystkie mają tej samej kategorii i dostawcy, które mają unikatowe nazwy i cenach.

Start, przeciągając je z przybornika do projektanta umieszczenia go pod istniejące panelu `DisplayInterface` panelu. Ustaw `ID` właściwości tego nowo dodanych panelu `InsertingInterface` i ustawić jej `Visible` właściwości `False`. Dodamy kod ustawiający `InsertingInterface` panelu s `Visible` właściwości `True` w kroku 3. Również wyczyszczenie panelu s `Height` i `Width` wartości właściwości.

Następnie należy utworzyć Wstawianie interfejs, który został przedstawiony wstecz na rysunku 1. Ten interfejs mogą być tworzone za pomocą różnych technik HTML, ale używamy jednego bardzo prosta: cztery kolumny, siedmiu wiersza tabeli.

> [!NOTE]
> Podczas wprowadzania kodu znaczników HTML `<table>` elementów, chcę użyć widoku źródła. Chociaż w Visual Studio narzędzia do dodawania `<table>` elementów przy użyciu narzędzia Projektant, Projektant wydaje się wszystkie zbyt chce wstrzyknąć nieproszony dla `style` ustawienia do znaczników. Gdy utworzono `<table>` znaczników, zwykle wrócić do projektanta, aby dodać formantów sieci Web i ustawiania ich właściwości. Podczas tworzenia tabel przy użyciu wstępnie określonych kolumn i wierszy wolę przy użyciu Statycznych zamiast [kontrolka sieci Web tabeli](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.table.aspx) ponieważ wszystkie formanty sieci Web umieścić w formancie tabeli w sieci Web można uzyskać tylko za pomocą `FindControl("controlID")` wzorca. Jednak używam formantów sieci Web tabeli dla tabel dynamicznie o rozmiarze (widocznych kolumn lub wierszy, których są oparte na niektóre bazy danych lub kryteria określone przez użytkownika), od sieci Web tabeli formantu można skonstruować programowo.


Wprowadź następujący kod w `<asp:Panel>` tagi `InsertingInterface` panelu:


[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

To `<table>` znacznika nie zawiera żadnych formantów sieci Web, ale dodamy tych na chwilę. Należy pamiętać, że każdy `<tr>` element zawiera danego ustawienia klasy CSS: `BatchInsertHeaderRow` dla wiersz nagłówka, w którym przejdzie dostawcy i kategorii DropDownLists; `BatchInsertFooterRow` wiersza stopki, gdzie zostaną umieszczone Dodawanie produktów z wysyłki i przyciski Anuluj; i naprzemiennych `BatchInsertRow` i `BatchInsertAlternatingRow` wartości wierszy zawierających produktu i jednostki ceny pól tekstowych. I Zapisz utworzony odpowiedni klas CSS w `Styles.css` plik umożliwiają wstawianie interfejsu interfejsem podobnym do widoku GridView i widoku DetailsView steruje, możemy kolejnych używanych w całym te samouczki. Poniżej przedstawiono te klasy CSS.


[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

Z tego znacznika wprowadzona wróć do widoku projektu. To `<table>` powinny być widoczne jako czterokolumnowe, siedmiu wiersz tabeli w projektancie, jak pokazano na rysunku 6.


[![Wstawianie interfejsu składa się z cztery kolumny, tabeli siedmiu wiersza](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**Rysunek 6**: wstawianie interfejsu składa się z cztery kolumny, tabeli siedmiu wiersza ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image18.png))


Firma Microsoft re teraz gotowe do dodawania formantów sieci Web do Wstawianie interfejsu. Przeciągnij dwa DropDownLists z przybornika do komórek w tabeli, jeden dla dostawcy i jeden dla kategorii.

Ustaw dostawcy DropDownList s `ID` właściwości `Suppliers` i powiązać ją z nowego elementu ObjectDataSource o nazwie `SuppliersDataSource`. Skonfiguruj nowy element ObjectDataSource można pobrać danych z `SuppliersBLL` klasy s `GetSuppliers` — metoda i zestaw aktualizacji karcie listy rozwijanej s (Brak). Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetSuppliers klasy SuppliersBLL s](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**Rysunek 7**: Konfigurowanie ObjectDataSource użyć `SuppliersBLL` klasy s `GetSuppliers` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image21.png))


Ma `Suppliers` wyświetlana lista DropDownList `CompanyName` pola danych i użyj `SupplierID` pola danych jako jego `ListItem` wartości s.


[![Wyświetlanie w polu danych NazwaFirmy i używanie IDdostawcy jako wartość](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**Rysunek 8**: Wyświetl `CompanyName` pola danych i użyj `SupplierID` jako wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image24.png))


Nazwa drugiej DropDownList `Categories` i powiązać ją z nowego elementu ObjectDataSource o nazwie `CategoriesDataSource`. Skonfiguruj `CategoriesDataSource` ObjectDataSource do użycia `CategoriesBLL` klasy s `GetCategories` metod; Ustaw listy rozwijane karty UPDATE i DELETE na (Brak) i kliknij przycisk Zakończ, aby zakończyć pracę kreatora. Na koniec ma wyświetlana lista DropDownList `CategoryName` pola danych i użyj `CategoryID` jako wartość.

Po dodaniu tych dwóch DropDownLists i powiązany z ObjectDataSources odpowiednio skonfigurowany, ekranu powinien wyglądać podobnie do rysunku 9.


[![Wiersz nagłówka zawiera teraz dostawców i DropDownLists kategorii](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**Rysunek 9**: nagłówka wiersza zawiera teraz `Suppliers` i `Categories` DropDownLists ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image27.png))


Teraz musisz utworzyć pól tekstowych, aby zbierać nazwy i ceny dla każdego nowego produktu. Dla każdego produktu pięć wierszy nazwy i cen, przeciągnij kontrolki pola tekstowego z przybornika do projektanta. Ustaw `ID` właściwości pola tekstowe do `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`i tak dalej.

Dodaj CompareValidator po każdym cenie jednostkowej pól tekstowych, ustawienie `ControlToValidate` właściwości do odpowiedniego `ID`. Również ustawić `Operator` właściwości `GreaterThanEqual`, `ValueToCompare` 0 i `Type` do `Currency`. Te ustawienia, poinstruuj CompareValidator w celu zapewnienia cen, jeśli wprowadzony, waluty prawidłową wartość, która jest większa lub równa zero. Ustaw `Text` właściwości \*, i `ErrorMessage` ceny musi być większa lub równa zero. Ponadto należy pominąć żadnych symboli waluty.

> [!NOTE]
> Wstawianie interfejsu mimo tego, że nie ma żadnych formantów RequiredFieldValidator `ProductName` w `Products` tabeli bazy danych nie zezwala na `NULL` wartości. Jest to spowodowane chcemy umożliwić użytkownikowi wprowadź maksymalnie pięć produktów. Na przykład jeśli użytkownik był przewidzieć cenę produktu nazwę i jednostkę pierwsze trzy wiersze, pozostawiając puste, ostatnie dwa wiersze d tylko dodamy trzech nowych produktów do systemu. Ponieważ `ProductName` jest wymagany, jednak musimy programowo upewnij się, aby upewnić się, że jeśli cenie jednostkowej jest wpisany podano odpowiadającą mu wartość nazwy produktu. Firma Microsoft będzie rozwiązania tego wyboru w kroku 4.


Podczas sprawdzania poprawności danych wejściowych użytkownika s, CompareValidator raporty nieprawidłowych danych, jeśli wartość zawiera symbol waluty. Dodaj $ przed poszczególnych pól tekstowych jako wizualnie instruuje użytkownika, aby pominąć symbol waluty, wprowadzając ceny cenie jednostkowej.

Na koniec Dodaj formant ValidationSummary w `InsertingInterface` panelu Ustawienia jej `ShowMessageBox` właściwości `True` i jego `ShowSummary` właściwości `False`. Przy użyciu tych ustawień Jeśli użytkownik wprowadzi wartość ceny nieprawidłową jednostkę, gwiazdka pojawi się obok ataku kontrolki TextBox i ValidationSummary wyświetli messagebox po stronie klienta, który zawiera komunikat o błędzie, który mamy określony wcześniej.

W tym momencie ekranie powinien wyglądać podobnie do rysunek 10.


[![Wstawianie interfejs zawiera teraz pól tekstowych dla produktów nazwy i cen](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**Na rysunku nr 10**: wstawianie interfejsu teraz obejmuje pól tekstowych nazw produktów i ceny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image30.png))


Musimy dodać Dodawanie produktów z przycisków wysyłki i Anuluj do wiersza stopki dalej. Przeciągnij dwa przycisk formanty z przybornika do stopki Wstawianie interfejsu, ustawienie przycisków `ID` właściwości `AddProducts` i `CancelButton` i `Text` właściwości, aby dodać produkty z wysyłki i Anuluj, odpowiednio. Ponadto ustawić `CancelButton` kontroli s `CausesValidation` właściwości `false`.

Na koniec należy dodać kontrolkę etykiety w sieci Web, która spowoduje wyświetlenie komunikatów o stanie dla dwóch interfejsów. Na przykład gdy użytkownik pomyślnie dodaje nowe wydanie produktów, chcemy powrócić do interfejsu wyświetlania i wyświetlony komunikat potwierdzenia. Jeśli jednak użytkownik zapewnia ceny dla nowego produktu, ale pozostawia poza nazwy produktu, musimy wyświetla komunikat ostrzegawczy od `ProductName` pole jest wymagane. Ponieważ potrzebujemy ten komunikat do wyświetlenia dla obu interfejsów, umieść go w górnej części strony poza panele.

Przeciągnij formant sieci Web etykiety z przybornika do górnej części strony w projektancie. Ustaw `ID` właściwości `StatusLabel`, wyczyść limit `Text` właściwości i ustaw `Visible` i `EnableViewState` właściwości `False`. Jak możemy jak już wspomniano w poprzedniej samouczki, ustawienie `EnableViewState` właściwości `False` pozwala programowo zmienić wartości właściwości etykiety s i je automatycznie przywrócić wartości domyślne na kolejnych ogłaszania zwrotnego. Upraszcza to kodu do wyświetlania komunikatu o stanie w odpowiedzi na niektóre akcje użytkownika, które zniknie na kolejnych ogłaszania zwrotnego. Wreszcie, ustaw `StatusLabel` kontroli s `CssClass` właściwości na ostrzeżenie, czyli nazwa klasy CSS zdefiniowanych w `Styles.css` wyświetlającym tekst w czcionki dużych, pogrubienie, kursywa czerwony.

Rysunek 11 zawiera projektanta programu Visual Studio po etykiecie zostało dodane i skonfigurowane.


[![Formant StatusLabel powyżej dwóch kontrolki panelu](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**Rysunek 11**: miejsce `StatusLabel` kontroli nad dwa formanty panelu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Krok 3: Przełączanie między wyświetlania i wstawianie interfejsów

Na tym etapie firma Microsoft ukończył kod znaczników dla naszych wyświetlania i wstawianie interfejsów, ale możemy re nadal pozostawione z dwóch zadań:

- Przełączanie między wyświetlania i wstawianie interfejsów
- Dodawanie produktów w wydaniu do bazy danych

Obecnie interfejs wyświetlana jest widoczne, ale Wstawianie interfejsu jest ukryta. Wynika to z faktu `DisplayInterface` panelu s `Visible` właściwość jest ustawiona na `True` (wartość domyślna), podczas gdy `InsertingInterface` panelu s `Visible` właściwość jest ustawiona na `False`. Aby przełączyć między dwa interfejsy musimy po prostu Przełącz każdego formantu s `Visible` wartości właściwości.

Chcemy przenieść z interfejsu wyświetlania Wstawianie interfejsu po kliknięciu przycisku wydania produktu procesu. W związku z tym tworzenie procedury obsługi zdarzeń dla tego przycisku s `Click` zdarzeń, który zawiera następujący kod:


[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

Ten kod po prostu ukrywa `DisplayInterface` Panel i pokazuje `InsertingInterface` panelu.

Następnie należy utworzyć procedury obsługi zdarzeń dla produktów Dodaj z kontroli wysyłki i przycisk Anuluj w interfejsie Wstawianie. Po kliknięciu jednego z tych przycisków, należy przywrócić interfejs wyświetlania. Utwórz `Click` obsługi zdarzeń dla obu kontrolki przycisku Tak, aby wywołują `ReturnToDisplayInterface`, metody zostanie dodany na chwilę. Oprócz ukrywanie `InsertingInterface` panelu, przedstawiający `DisplayInterface` panelu `ReturnToDisplayInterface` metoda musi zwracać formantów sieci Web do stanu przed edycji. Obejmuje to ustawienie DropDownLists `SelectedIndex` właściwości 0 i wyczyszczenie `Text` właściwości kontrolki TextBox.

> [!NOTE]
> Należy rozważyć, co może się zdarzyć, jeśli firma Microsoft t przywrócić formanty ich stan wstępnie edycji przed zwróceniem do wyświetlania interfejsu. Użytkownik może kliknij przycisk procesu wydania produktu, wprowadź produkty z wydanie, a następnie kliknij przycisk Dodaj produkty z wydania. Spowoduje to dodać produkty i powrócić do wyświetlania interfejsu użytkownika. Użytkownik może być w tym momencie dodać innego wydania. Po kliknięciu przycisk wydania produktu procesu, który zwracają Wstawianie interfejsu, ale lista DropDownList opcji i wartości tekstowe nadal będzie wypełnione poprzedniej wartości.


[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

Zarówno `Click` procedury obsługi zdarzeń po prostu Wywołaj `ReturnToDisplayInterface` metody, mimo że wrócimy do Dodawanie produktów z wydania `Click` obsługi zdarzeń w kroku 4 i Dodaj kod, aby zapisać te produkty. `ReturnToDisplayInterface`rozpoczyna się zwracając `Suppliers` i `Categories` DropDownLists ich pierwszej opcji. Dwie stałe `firstControlID` i `lastControlID` oznaczyć początkową i końcową używać w nazwie produktu nazwy i Jednostka ceny pól tekstowych w Wstawianie interfejsu i są używane w zakresie wartości indeksu kontrolki `For` pętli, która ustawia `Text`właściwości formantów pole tekstowe z powrotem do pustego ciągu. Na koniec panele `Visible` właściwości są resetowane tak, aby Wstawianie interfejsu jest ukryta i interfejs wyświetlania wyświetlane.

Poświęć chwilę, aby przetestować tę stronę w przeglądarce. Podczas odwiedzania najpierw strony interfejsu wyświetlania powinna zostać wyświetlona jak zostało pokazano na rysunku 5. Kliknij przycisk wydania produktu procesu. Strona zostanie ogłaszanie i powinien zostać wyświetlony interfejs Wstawianie, jak pokazano na rysunku 12. Kliknięcie przycisku albo Dodaj produkty z przycisków wydanie lub Anuluj powrót do wyświetlania interfejsu.

> [!NOTE]
> Podczas wyświetlania Wstawianie interfejsu, Poświęć chwilę, aby przetestować CompareValidators w cenie jednostkowej pól tekstowych. Powinny pojawić się ostrzeżenie po kliknięciu przycisku Dodaj produkty z przycisku wydania z wartości waluty nieprawidłowy lub ceny o wartości mniejszej niż zero element messagebox po stronie klienta.


[![Wstawianie interfejsu jest wyświetlany po kliknięciu przycisku wydania produktu procesu](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**Rysunek 12**: wstawianie interfejsu jest wyświetlany po kliknięciu przycisku wydania produktu procesu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image36.png))


## <a name="step-4-adding-the-products"></a>Krok 4: Dodawanie produktów

All, który pozostaje w tym samouczku jest zapisać produktów do bazy danych w produktach dodać z przycisku wydania s `Click` program obsługi zdarzeń. Można to zrobić, tworząc `ProductsDataTable` i dodawanie `ProductsRow` wystąpienia dla każdego z podanych nazw produktów. Raz te `ProductsRow` dodano s, firma Microsoft będzie wprowadzać wywołanie `ProductsBLL` klasy s `UpdateWithTransaction` metody przekazując `ProductsDataTable`. Odwołania, który `UpdateWithTransaction` metodę, która została utworzona w [zawijania modyfikacje bazy danych w ramach transakcji](wrapping-database-modifications-within-a-transaction-vb.md) samouczek, przekazuje `ProductsDataTable` do `ProductsTableAdapter` s `UpdateWithTransaction` metody. Dostępne transakcji ADO.NET została uruchomiona i problemy TableAdatper `INSERT` instrukcji do bazy danych dla każdego dodane `ProductsRow` w elemencie DataTable. Zakładając, że wszystkie produkty są dodawane bez błędów, transakcja została zatwierdzona, w przeciwnym razie go zostanie wycofana.

Kod Dodawanie produktów z przycisku wydania s `Click` program obsługi zdarzeń musi również wykonać nieco sprawdzanie błędów. Ponieważ nie ma żadnych RequiredFieldValidators używany w interfejsie Wstawianie, użytkownik może podać produktu pomijając jego nazwy. Ponieważ nazwy produktu s jest wymagany, jeśli taki warunek otwierany musimy użytkownika, a nie kontynuować operacji wstawienia. Pełną `Click` kod obsługi zdarzenia:


[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

Program obsługi zdarzeń, który uruchamia się za zapewnienie, że `Page.IsValid` właściwość zwraca wartość `True`. Jeśli zmienna zwraca `False`, następnie oznacza to, że jeden lub więcej CompareValidators są raportowanie nieprawidłowe dane; w takim przypadku nie chcemy się próba wstawienia wprowadzone produktów lub firma Microsoft będzie końcowa Wystąpił wyjątek podczas próby przypisać cenie jednostkowej wprowadzonych przez użytkownika wartość, która ma `ProductsRow` s `UnitPrice` właściwości.

Następnie, nowy `ProductsDataTable` jest tworzone wystąpienie (`products`). A `For` pętli jest używany do iterowania po ceny nazwę i jednostkę produktu pola tekstowe i `Text` właściwości są odczytywane do zmiennych lokalnych `productName` i `unitPrice`. Jeśli użytkownik wpisze wartość cenie jednostkowej, ale nie z odpowiednią nazwą produktu `StatusLabel` wyświetla komunikat, jeśli podasz jednostka ceny można również musi zawierać nazwę produktu i obsługi zdarzeń jest zakończony.

Jeśli podano nazwę produktu nowy `ProductsRow` wystąpienia jest tworzony przy użyciu `ProductsDataTable` s `NewProductsRow` metody. Nowy `ProductsRow` wystąpienia s `ProductName` właściwość jest ustawiona na bieżącego produktu Nazwa pola tekstowego podczas `SupplierID` i `CategoryID` właściwości są przypisane do `SelectedValue` właściwości DropDownLists w nagłówku Wstawianie interfejsów. Jeśli użytkownik wprowadził wartość w cenie produktu s, jest przypisany do `ProductsRow` wystąpienia s `UnitPrice` właściwość; w przeciwnym razie wartość właściwości jest nieprzypisane, lewo, co spowoduje `NULL` wartość `UnitPrice` w bazie danych. Na koniec `Discontinued` i `UnitsOnOrder` właściwości są przypisane do zakodowanych wartości `False` i 0, odpowiednio.

Po przypisaniu właściwości do `ProductsRow` wystąpienia jest dodawany do `ProductsDataTable`.

Po zakończeniu `For` pętli, musimy sprawdzić czy dodano wszystkie produkty. Użytkownik może po wszystkich kliknięto Dodawanie produktów z wydania przed wprowadzeniem wszelkie nazwy produktów lub ceny. Jeśli istnieje co najmniej jeden produkt `ProductsDataTable`, `ProductsBLL` klasy s `UpdateWithTransaction` metoda jest wywoływana. Następnie dane jest odbitych do `ProductsGrid` GridView tak, aby nowo dodanego produktów będą wyświetlane w interfejsie wyświetlania. `StatusLabel` Jest aktualizowana w celu wyświetlania potwierdzenia i `ReturnToDisplayInterface` jest wywoływana ukrywanie Wstawianie interfejsu i wyświetlanie interfejsu wyświetlania.

Jeśli nie wprowadzono żadnych produktów, nadal jest wyświetlana ale komunikat nie dodano żadnych produktów Wstawianie interfejsu. Wprowadź nazwy produktu, a jednostka ceny w tych polach tekstowych są wyświetlane.

S rysunek 13, 14 do 15 Pokaż Wstawianie i Wyświetl interfejsów w akcji. Na rysunku 13 użytkownik wprowadził wartość ceny jednostki bez odpowiadającą jej nazwą produktu. Rysunek 14 pokazuje interfejsu wyświetlania po trzech nowych produktów zostały dodane pomyślnie, gdy rysunek 15 pokazuje dwóch produktów nowo dodanych GridView (trzeci znajduje się na poprzedniej stronie).


[![Nazwa produktu jest wymagany po wprowadzeniu cenie jednostkowej](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**Rysunek 13**: Nazwa produktu A jest wymagany po wprowadzeniu cenie jednostkowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image39.png))


[![Dodano trzy Veggies nowego dostawcy Mayumi s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**Rysunek 14**: trzy nowe Veggies dodano s Mayumi dostawcy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image42.png))


[![Nowych produktów znajduje się na ostatniej stronie widoku GridView](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**Rysunek 15**: nowych produktów znajduje się na ostatniej stronie widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-vb/_static/image45.png))


> [!NOTE]
> Wstawianie logikę używaną w tym samouczku partii opakowuje wstawia w zakresie transakcji. Aby to sprawdzić, celowo powodować błąd poziom bazy danych. Na przykład, zamiast przypisywać nowe `ProductsRow` wystąpienia s `CategoryID` właściwości wybranej wartości w `Categories` DropDownList i przypisz go do wartości, takich jak `i * 5`. W tym miejscu `i` indeksatora pętli i ma wartości z zakresu od 1 do 5. W związku z tym podczas dodawania dwóch lub więcej produktów w partii Wstaw pierwszy produkt ma prawidłową `CategoryID` będzie mieć wartość [5], ale kolejnych produktów `CategoryID` wartości, które nie pasują do `CategoryID` wartości w `Categories` tabeli. Net powoduje, że podczas pierwszego `INSERT` powiedzie się, kolejne zakończy się niepowodzeniem z naruszenie ograniczenia klucza obcego. Ponieważ wstawianie wsadowe jest atomic, pierwszy `INSERT` zostanie wycofana, zwracanie rozpoczęcia bazy danych do stanu przed partii procesu.


## <a name="summary"></a>Podsumowanie

Przez to i poprzednich dwóch samouczki utworzono interfejsów, które umożliwiają aktualizowanie, usuwanie, i wstawianie partii danych, z których wszystkie używane obsługi transakcji, dodano do Warstwa dostępu do danych w [zawijania modyfikacje bazy danych w ramach transakcji](wrapping-database-modifications-within-a-transaction-vb.md) samouczka. Dla niektórych scenariuszy, takich interfejsów użytkownika przetwarzania wsadowego znacząco poprawić wydajność użytkownika końcowego przez zmniejszenie liczby kliknięć, ogłaszania zwrotnego i przełączenia kontekstu Mysz klawiatury, zachowując jednocześnie integralność danych.

W tym samouczku wykonuje naszych przyjrzeć się praca z danymi wsadowej. Następny zestaw samouczki Eksploruje różnych zaawansowanych scenariuszy Warstwa dostępu do danych, w tym korzystanie z procedur składowanych w metodach s TableAdapter, konfigurowanie ustawień połączenia i poziom polecenia w warstwę DAL, szyfrowania parametrów połączenia i inne!

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. W tym samouczku prowadzić osoby dokonujące przeglądu zostały ren Hilton Giesenow i S Lauritsen Jacoba. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](batch-deleting-vb.md)
