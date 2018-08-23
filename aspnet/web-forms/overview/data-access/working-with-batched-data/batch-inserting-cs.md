---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
title: Batch wstawiania (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Dowiedz się, jak wstawić wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika możemy rozszerzyć GridView, aby umożliwić użytkownikowi wprowadzanie wielu n...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: cf025e08-48fc-4385-b176-8610aa7b5565
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
msc.type: authoredcontent
ms.openlocfilehash: 561acc9b473bac7d39e7ed4d511d8b979657131d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751948"
---
<a name="batch-inserting-c"></a>Batch wstawiania (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_CS.zip) lub [Pobierz plik PDF](batch-inserting-cs/_static/datatutorial66cs1.pdf)

> Dowiedz się, jak wstawić wiele rekordów bazy danych w ramach jednej operacji. W warstwie interfejsu użytkownika możemy rozszerzyć GridView, aby umożliwić użytkownikowi wprowadzanie wielu nowych rekordów. W warstwie dostępu do danych Firma opakować wiele operacji wstawiania w ramach transakcji, aby upewnić się, że wszystkie wstawienia powiedzie się lub wstawienia wszystkie zostaną wycofane.


## <a name="introduction"></a>Wprowadzenie

W [aktualizowania wsadowego](batch-updating-cs.md) samouczek przyjrzeliśmy się Dostosowywanie formantu GridView prezentować interfejs, gdzie zostały można edytować wiele rekordów. Użytkownik, odwiedzając stronę może wprowadzić serię zmian i następnie za pomocą kliknięcia jednego przycisku, należy wykonać aktualizację usługi batch. W sytuacjach, w którym użytkownicy często aktualizować wielu rekordów w jednym z rzeczywistym użyciem, taki interfejs można zapisać niezliczone kliknięć i przełączeń kontekstu Mysz klawiatury, w porównaniu do domyślnego — w wierszu funkcje edycji, które zostały najpierw przedstawione w [ Omówienie Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) samouczka.

Takie podejście również będą stosowane, podczas dodawania rekordów. Załóżmy, że w tym miejscu firmie Northwind Traders często otrzymamy wydań od dostawców, które zawierają wiele produktów dla określonej kategorii. Na przykład firma Microsoft może otrzymywać wydania sześciu różnych herbatę i produktów kawy Tokio Traders. Jeśli użytkownik wprowadzi sześć produktów jeden w czasie za pomocą kontrolki widoku szczegółów, należy je wybrać wiele takich samych wartości wielokrotnie: należy wybrać tej samej kategorii (Beverages) tego samego dostawcy (Tokio handlowców), takie same zakończona (wartość FAŁSZ) i tej samej jednostki w kolejności wartości (0). Ten wpis powtarzających się danych jest nie tylko dużo czasu, ale jest podatne na błędy.

Z niewielką ilością pracy możemy utworzyć partię Wstawianie interfejs, który umożliwia użytkownikowi określenie, dostawcy i kategorii raz, wprowadź szereg nazw produktów i cenę jednostkową, a następnie kliknij przycisk służący do dodawania nowych produktów w bazie danych (patrz rysunek 1). Po dodaniu każdego produktu, jego `ProductName` i `UnitPrice` pól danych są przypisane wartości wprowadzone w tych polach tekstowych podczas jego `CategoryID` i `SupplierID` wartości są przypisane wartości z kontrolek DROPDOWNLIST w górnym fo formularza. `Discontinued` i `UnitsOnOrder` wartości są ustawiane na wartości ustaloną `false` i 0, odpowiednio.


[![Interfejs wstawiania wsadowego](batch-inserting-cs/_static/image2.png)](batch-inserting-cs/_static/image1.png)

**Rysunek 1**: interfejs wstawiania wsadowego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image3.png))


W tym samouczku zostanie utworzona strona, która implementuje usługi batch, wstawianie interfejsu przedstawionej na rysunku 1. Jako przy użyciu dwóch poprzednich samouczków, firma Microsoft będzie zawijany wstawienia w zakresie transakcji w celu zapewnienia niepodzielność. Rozpocznij pracę dzięki s!

## <a name="step-1-creating-the-display-interface"></a>Krok 1: Tworzenie interfejsu wyświetlania

W tym samouczku będzie składać się z jednej strony, który jest podzielony na dwie części: obszarem wyświetlania i Wstawianie regionu. Interfejs ekran, który utworzymy w tym kroku przedstawiono produkty w GridView i zawiera przycisk o nazwie wydania produktu procesu. Po kliknięciu tego przycisku interfejsu wyświetlana jest zastępowany Wstawianie interfejsu, który jest pokazany na rysunku 1. Interfejs wyświetlania zwraca po Dodaj produkty z wydania lub kliknięciu przycisku Anuluj. Utworzymy Wstawianie interfejsu w kroku 2.

Podczas tworzenia strony, która ma dwa interfejsy, z których tylko jedna jest widoczny w danym momencie, każdy interfejs zwykle jest umieszczana w [formantu sieci Web Panel](http://www.w3schools.com/aspnet/control_panel.asp), który służy jako kontener dla innych kontrolek. W związku z tym strony ma dwie kontrolki panelu jeden dla każdego interfejsu.

Zacznij od otwarcia `BatchInsert.aspx` stronie `BatchData` folder i przeciągnij panelu z przybornika do konstruktora (patrz rysunek 2). Ustawianie panelu s `ID` właściwość `DisplayInterface`. Podczas dodawania panelu do projektanta, jego `Height` i `Width` właściwości są ustawione na 50 pikseli i 125px, odpowiednio. Czyści wartości tych właściwości w oknie właściwości.


[![Przeciąganie panelu z przybornika do projektanta](batch-inserting-cs/_static/image5.png)](batch-inserting-cs/_static/image4.png)

**Rysunek 2**: przeciągnij panelu z przybornika do projektanta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image6.png))


Następnie przeciągnij formant przycisku i GridView do panelu. Ustaw s `ID` właściwości `ProcessShipment` i jego `Text` właściwości wydania produktu procesu. Ustaw GridView s `ID` właściwości `ProductsGrid` i z jego tag inteligentny powiązać go do nowego elementu ObjectDataSource, o nazwie `ProductsDataSource`. Konfigurowanie kontrolki ObjectDataSource swoich danych z `ProductsBLL` klasy s `GetProducts` metody. Ponieważ GridView ten jest używany tylko do wyświetlania danych, ustaw list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak). Kliknij przycisk Zakończ, aby zakończyć działanie kreatora Konfigurowanie źródła danych.


[![Wyświetl dane zwrócone z metody GetProducts ProductsBLL klasy s](batch-inserting-cs/_static/image8.png)](batch-inserting-cs/_static/image7.png)

**Rysunek 3**: wyświetlane dane zwrócone z `ProductsBLL` klasy s `GetProducts` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image9.png))


[![Ustaw list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak)](batch-inserting-cs/_static/image11.png)](batch-inserting-cs/_static/image10.png)

**Rysunek 4**: Ustaw listy rozwijane w aktualizacji, WSTAWIANIA i usuwania karty (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image12.png))


Po zakończeniu pracy Kreatora ObjectDataSource, Visual Studio doda BoundFields i CheckBoxField dla pól danych produktu. Usuń wszystkie elementy oprócz `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, i `Discontinued` pola. Możesz wprowadzić dowolne dostosowania estetycznych. Czy zdecydowała się formatowania `UnitPrice` pola jako wartość waluty zmieniana pola, zmienić nazwę i kilka pól `HeaderText` wartości. Również skonfigurować GridView obejmujący stronicowanie i sortowanie pomocy technicznej przez zaznaczenie pola wyboru Włączanie stronicowania i włączyć sortowanie w tagu inteligentnego s GridView.

Po dodaniu kontrolki Panel, przycisk, GridView i kontrolki ObjectDataSource i dostosowywanie pól s GridView, Twoje oznaczeniu deklaracyjnym strony s powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](batch-inserting-cs/samples/sample1.aspx)]

Należy zauważyć, że kod znaczników dla przycisku i GridView pojawiają się między otwierającym i zamykającym `<asp:Panel>` tagów. Ponieważ te kontrolki znajdują się w `DisplayInterface` panelu, możemy je ukryć, po prostu ustawić Panel s `Visible` właściwość `false`. Krok 3 analizuje programowe Zmienianie panelu s `Visible` kliknij właściwości w odpowiedzi na przycisku, aby wyświetlić jeden interfejs drugiego są ukryte.

Poświęć chwilę, aby wyświetlić postępach za pośrednictwem przeglądarki. Jak pokazano na rysunku 5, powinien zostać wyświetlony przycisk wydania produktu procesu powyżej GridView zawierającego produkty dziesięciu w danym momencie.


[![Kontrolki GridView zawiera listę produktów oraz oferuje sortowanie i stronicowanie możliwości](batch-inserting-cs/_static/image14.png)](batch-inserting-cs/_static/image13.png)

**Rysunek 5**: widoku GridView zawiera listę produktów i oferuje sortowania i stronicowania możliwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>Krok 2: Tworzenie interfejsu Wstawianie

Za pomocą wyświetlania interfejsu ukończenia, możemy ponownie gotowe do utworzenia Wstawianie interfejsu. Na potrzeby tego samouczka umożliwiają tworzenie Wstawianie interfejs, który wyświetla monit o pojedynczej wartości dostawcy i kategorii, a następnie umożliwia użytkownikowi wprowadzanie maksymalnie pięć nazw produktów i wartości cena jednostki s. Z tym interfejsem użytkownika można dodać do pięciu nowych produktów, wszystkie mają te same kategorii i dostawcy, które mają unikatowe nazwy i ceny.

Rozpocznij od przeciąganie panelu z przybornika w projektancie, umieszczając go poniżej istniejącego `DisplayInterface` panelu. Ustaw `ID` właściwość to nowo dodana panelu `InsertingInterface` i ustaw jego `Visible` właściwość `false`. Dodamy kod, który ustawia `InsertingInterface` panelu s `Visible` właściwości `true` w kroku 3. Również wyczyszczenie panelu s `Height` i `Width` wartości właściwości.

Następnie należy utworzyć Wstawianie interfejs, który został wyświetlony wstecz na rysunku 1. Ten interfejs, można utworzyć za pomocą różnych technik HTML, ale firma Microsoft użyje jednego dość prosta: cztery kolumny, siedem wiersza tabeli.

> [!NOTE]
> Podczas wprowadzania kodu znaczników HTML `<table>` elementów, chcę użyć widoku źródła. Gdy program Visual Studio są narzędzia umożliwiające dodawanie `<table>` elementów za pomocą projektanta, projektanta jest prawdopodobnie wszystko za chęć wstrzyknąć nieproszony dla `style` ustawienia do znaczników. Po użytkownik utworzył `<table>` znaczników, zwykle wrócić do projektanta Aby dodać kontrolki sieci Web i ustawiać ich właściwości. Podczas tworzenia tabel z wstępnie ustaloną kolumnami i wierszami wolę używać Statycznych, a nie od [formantu sieci Web tabeli](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) ponieważ wszystkie formanty Web umieszczone wewnątrz formantu sieci Web tabeli są dostępne wyłącznie przy użyciu `FindControl("controlID")` wzorzec. Jednak używam formantów sieci Web tabeli dla dynamicznie wielkości tabel (tymi którego wierszy lub kolumn, które są oparte na niektóre bazy danych lub kryteria określone przez użytkownika), od sieci Web tabeli kontroli można skonstruować programowo.


Wprowadź następujący kod w ramach `<asp:Panel>` tagi `InsertingInterface` panelu:


[!code-html[Main](batch-inserting-cs/samples/sample2.html)]

To `<table>` znaczników nie zawiera żadnych formantów sieci Web, ale dodamy te chwilowo. Należy pamiętać, że każdy `<tr>` element zawiera konkretnego ustawienia klasy CSS: `BatchInsertHeaderRow` dla wiersza nagłówka, gdy zaczną dostawcy i kategorii kontrolek DROPDOWNLIST; `BatchInsertFooterRow` wiersz stopki, gdy zaczną się Dodaj produkty z wydania i przyciski Anuluj; i przełączanie `BatchInsertRow` i `BatchInsertAlternatingRow` wartości wierszy zawierających produktu i jednostki cena kontrolki pola tekstowego. Czy mogę ve utworzone odpowiednich klas CSS w `Styles.css` plik, aby nadać Wstawianie interfejsu z interfejsem podobnym do kontrolki GridView i DetailsView kontroluje, firma Microsoft ve używane w tych samouczkach. Poniżej przedstawiono te klasy CSS.


[!code-css[Main](batch-inserting-cs/samples/sample3.css)]

Za pomocą tego wprowadzony kod znaczników wróć do widoku projektu. To `<table>` powinien być wyświetlony jako cztery kolumny, siedem wiersz tabeli w projektancie, tak jak pokazano na rysunku 6.


[![Wstawianie interfejsu składa się z cztery kolumny, tabeli Wiersz 7](batch-inserting-cs/_static/image17.png)](batch-inserting-cs/_static/image16.png)

**Rysunek 6**: interfejs Wstawianie składa się z cztery kolumny, tabeli Wiersz 7 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image18.png))


Możemy ponownie teraz gotowy do dodawania formantów sieci Web do Wstawianie interfejsu. Przeciągnij dwóch kontrolek DROPDOWNLIST z przybornika do komórek w tabeli, jeden dla dostawcy i jeden dla kategorii.

Ustaw dostawcy DropDownList s `ID` właściwości `Suppliers` i powiązać ją z nowego elementu ObjectDataSource, o nazwie `SuppliersDataSource`. Konfigurowanie nowego elementu ObjectDataSource można pobrać danych z `SuppliersBLL` klasy s `GetSuppliers` metody i zestaw aktualizacji karty listy rozwijanej s (Brak). Kliknij przycisk Zakończ, aby zakończyć działanie kreatora.


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetSuppliers SuppliersBLL klasy s](batch-inserting-cs/_static/image20.png)](batch-inserting-cs/_static/image19.png)

**Rysunek 7**: Konfigurowanie kontrolki ObjectDataSource do użycia `SuppliersBLL` klasy s `GetSuppliers` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image21.png))


Ma `Suppliers` wyświetlania kontrolki DropDownList `CompanyName` pola danych i użyj `SupplierID` pole danych jako jej `ListItem` wartości s.


[![Wyświetlanie pola dane CompanyName i użyj IDdostawcy jako wartości](batch-inserting-cs/_static/image23.png)](batch-inserting-cs/_static/image22.png)

**Rysunek 8**: wyświetlanie `CompanyName` pola danych i użyj `SupplierID` jako wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image24.png))


Nadaj nazwę drugiej kontrolki DropDownList `Categories` i powiązać ją z nowego elementu ObjectDataSource, o nazwie `CategoriesDataSource`. Konfigurowanie `CategoriesDataSource` ObjectDataSource używać `CategoriesBLL` klasy s `GetCategories` metody; zestaw z listy rozwijanej wyświetla karty aktualizacji i usuwania (Brak) i kliknij przycisk Zakończ, aby zakończyć pracę kreatora. Na koniec mają wyświetlania kontrolki DropDownList `CategoryName` pola danych i użyj `CategoryID` jako wartość.

Po dodaniu tych dwóch kontrolek DROPDOWNLIST i powiązane z ObjectDataSources odpowiednio skonfigurowany, ekran powinien wyglądać podobnie jak rysunek 9.


[![Wiersz nagłówka zawiera teraz dostawców i kontrolek DROPDOWNLIST kategorii](batch-inserting-cs/_static/image26.png)](batch-inserting-cs/_static/image25.png)

**Rysunek 9**: nagłówek wiersza zawiera teraz `Suppliers` i `Categories` kontrolek DROPDOWNLIST ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image27.png))


Teraz musimy utworzyć pola tekstowe, aby zbierać nazwy i ceny dla każdego nowego produktu. Przeciągnij formant TextBox z przybornika do projektanta dla każdego produktu pięć wierszy nazwy i ceny. Ustaw `ID` właściwości pola tekstowe do `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`i tak dalej.

Dodaj CompareValidator po każdym poleceniu ceny jednostkowej pola tekstowe, ustawienie `ControlToValidate` właściwości do odpowiednich `ID`. Również ustawić `Operator` właściwości `GreaterThanEqual`, `ValueToCompare` 0, a `Type` do `Currency`. Te ustawienia poinstruować CompareValidator do upewnij się, że ceny, jeśli wprowadzono, wartość waluty prawidłowe, która jest większa lub równa zero. Ustaw `Text` właściwości \*, i `ErrorMessage` ceny musi być większa lub równa zero. Ponadto można pominąć dowolny symbol waluty.

> [!NOTE]
> Wstawianie interfejsu nawet jeśli nie ma wszystkie formanty RequiredFieldValidator `ProductName` pole `Products` tabeli bazy danych nie zezwala na `NULL` wartości. Jest to spowodowane chcemy umożliwić użytkownikowi wprowadź maksymalnie pięć produktów. Na przykład gdyby użytkownik Podaj nazwę i jednostkę cena produktu dla pierwszych trzech wierszy, pozostawiając puste, ostatnie dwa wiersze d po prostu dodamy trzy nowe produkty do systemu. Ponieważ `ProductName` jest wymagane, jednak firma Microsoft będzie należy programowo upewnij się, aby upewnić się, że jeśli cena jednostkowa jest wpisany podano odpowiednie wartości nazwy produktu. Firma Microsoft będzie czoła tym wyboru w kroku 4.


Podczas sprawdzania poprawności danych wejściowych użytkownika s, CompareValidator raporty nieprawidłowe dane, jeśli wartość zawiera symbol waluty. Dodaj $ przed każdego cena jednostkowa pól tekstowych, która będzie służyć jako wskazówką wizualną, która instruuje użytkownika, aby pominąć symbol waluty, podczas wprowadzania cenę.

Na koniec Dodaj kontrolki podsumowania walidacji `InsertingInterface` panelu ustawienia jego `ShowMessageBox` właściwości `true` i jego `ShowSummary` właściwość `false`. Przy użyciu tych ustawień Jeśli użytkownik wprowadzi wartość ceny nieprawidłową jednostkę, gwiazdką pojawi się obok naruszającym kontrolki pola tekstowego i podsumowania walidacji wyświetli komunikat messagebox po stronie klienta, który pokazuje komunikat o błędzie, który wcześniej określonej.

W tym momencie ekran powinien wyglądać podobnie jak na rysunku nr 10.


[![Wstawianie interfejs zawiera teraz pola tekstowe dla produktów nazwy i ceny](batch-inserting-cs/_static/image29.png)](batch-inserting-cs/_static/image28.png)

**Na rysunku nr 10**: wstawianie interfejsu teraz zawiera pola tekstowe nazw produktów i ceny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image30.png))


Następnie musimy dodać Dodaj produkty z przycisków wydania i Anuluj do wiersza stopki. Przeciągnij dwa formanty przycisków z przybornika w stopce Wstawianie interfejsu ustawienie przyciski `ID` właściwości `AddProducts` i `CancelButton` i `Text` właściwości, aby dodać produkty z wydania i anulowania, odpowiednio. Dodatkowo, ustawienia `CancelButton` formantu s `CausesValidation` właściwość `false`.

Na koniec należy dodać formant etykiety w sieci Web, który spowoduje wyświetlenie komunikatów o stanie dla dwóch interfejsów. Na przykład gdy użytkownik pomyślnie dodaje nowe wydanie produktów, chcemy powrócić do wyświetlania interfejsu i wyświetlony komunikat potwierdzenia. Jeśli jednak użytkownik udostępnia ceny dla nowego produktu, ale pozostawia off nazwę produktu, należy wyświetlić komunikat ostrzegawczy od `ProductName` pole jest wymagane. Ponieważ potrzebujemy ten komunikat do wyświetlenia dla obu interfejsów, należy umieścić go w górnej części strony poza paneli.

Przeciągnij formant etykiety w sieci Web z przybornika do górnej części strony w projektancie. Ustaw `ID` właściwości `StatusLabel`, wyczyść się `Text` właściwości, a następnie ustaw `Visible` i `EnableViewState` właściwości `false`. Jak zauważono w poprzednich samouczkach, ustawienie `EnableViewState` właściwość `false` pozwala nam programowo zmienić wartości właściwości etykiety s i je automatycznie przywrócić wartości domyślne na kolejne zwrotu. Upraszcza to kod do wyświetlania komunikatu o stanie w odpowiedzi na niektóre akcje użytkownika, które zniknie na kolejne zwrotu. Wreszcie, ustaw `StatusLabel` formantu s `CssClass` właściwość ostrzeżenie, jest to nazwa klasy CSS zdefiniowanych w `Styles.css` , tekst jest wyświetlany w dużych, pogrubienie, kursywa czerwony czcionki.

Na ilustracji 11 pokazano projektanta programu Visual Studio po etykieta została dodana i skonfigurowane.


[![Formant StatusLabel powyżej dwóch kontrolek panelu](batch-inserting-cs/_static/image32.png)](batch-inserting-cs/_static/image31.png)

**Rysunek 11**: miejsce `StatusLabel` kontroli nad dwa formanty panelu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Krok 3: Przełączanie się między widokiem interfejsów i wstawiania

W tym momencie ukończyliśmy znaczniki dla naszych wyświetlania i wstawianie interfejsów, ale ponownie nadal pozostał dwa zadania:

- Przełączanie między Wyświetlanie interfejsów i wstawiania
- Dodawanie produktów w wydaniu z bazą danych

Obecnie interfejs wyświetlana jest widoczna, ale interfejs Wstawianie jest ukryty. Jest to spowodowane `DisplayInterface` panelu s `Visible` właściwość jest ustawiona na `true` (wartość domyślna), podczas gdy `InsertingInterface` panelu s `Visible` właściwość jest ustawiona na `false`. Aby przełączać się między dwa interfejsy, należy przełączyć każdy formant s `Visible` wartości właściwości.

Chcemy przenieść z interfejsu wyświetlania interfejsu Wstawianie po kliknięciu przycisku wydania produktu procesu. W związku z tym, utworzyć program obsługi zdarzeń dla przycisku s `Click` zdarzenia, które zawiera następujący kod:


[!code-csharp[Main](batch-inserting-cs/samples/sample4.cs)]

Ten kod po prostu ukrywa `DisplayInterface` panelu i pokazuje `InsertingInterface` panelu.

Następnie należy utworzyć procedury obsługi zdarzeń dla Dodaj produkty z formantów wydania i przycisk Anuluj w interfejsie Wstawianie. Po kliknięciu jednego z tych przycisków, musimy przywrócić interfejsu ekranu. Tworzenie `Click` procedury obsługi zdarzeń dla obu formanty przycisków, dzięki czemu mogą wywołać `ReturnToDisplayInterface`, dodamy chwilowo metody. Oprócz ukrywanie `InsertingInterface` panelu i wyświetlanie `DisplayInterface` panelu `ReturnToDisplayInterface` metoda musi zwracać kontrolki sieci Web do ich edycji wstępnie stanu. Obejmuje to ustawienie kontrolek DROPDOWNLIST `SelectedIndex` właściwości do 0 i wyczyszczenie `Text` właściwości kontrolki pola tekstowego.

> [!NOTE]
> Należy wziąć pod uwagę, co może się zdarzyć, jeśli firma t są zwracają formanty do stanu wstępnie edycji przed zwróceniem do wyświetlania interfejsu. Użytkownik może kliknij przycisk wydania produktu procesu, wprowadź produkty z wydanie a następnie kliknij Dodaj produkty z wydania. Spowoduje to Dodaj produkty i powrót użytkownika do wyświetlania interfejsu. W tym momencie użytkownik może chcieć dodać inny wydania. Po kliknięciu przycisku wydania produktu procesu, które zwracają interfejs Wstawianie, ale metody DropDownList wybrane opcje i wartości pola tekstowego nadal będzie wypełniana przy użyciu ich poprzedniej wartości.


[!code-csharp[Main](batch-inserting-cs/samples/sample5.cs)]

Zarówno `Click` po prostu wywoływanie programów obsługi zdarzeń `ReturnToDisplayInterface` metody, mimo że wrócimy do Dodaj produkty z wydania `Click` programu obsługi zdarzeń w kroku 4 i Dodaj kod, aby zapisać te produkty. `ReturnToDisplayInterface` rozpoczyna się, zwracając `Suppliers` i `Categories` kontrolek DROPDOWNLIST do swojej pierwszej opcji. Dwie stałe `firstControlID` i `lastControlID` oznaczyć początkową i końcową wartości indeksu kontrolki używane w nazwach produktu nazwa i cena jednostkowa pola tekstowe w Wstawianie interfejs i są używane w granicach `for` pętli, która ustawia `Text`właściwości kontrolki TextBox z powrotem do pustego ciągu. Na koniec panele `Visible` właściwości są resetowane w celu Wstawianie interfejs jest ukryty i interfejsu ekranu wyświetlane.

Poświęć chwilę, w celu przetestowania tej strony w przeglądarce. Po raz pierwszy, odwiedzając stronę powinien pojawić się interfejsu wyświetlania, jak zostało pokazane na rysunku 5. Kliknij przycisk wydania produktu procesu. Strona będzie ogłaszanie zwrotne i powinien zostać wyświetlony interfejs Wstawianie, jak pokazano na rysunku 12. Klikając albo Dodaj produkty z przycisków wydania lub Anuluj powrót do wyświetlania interfejsu.

> [!NOTE]
> Podczas wyświetlania Wstawianie interfejsu, Poświęć chwilę, przetestowaniu CompareValidators na cenę jednostkową pól tekstowych. Powinien zostać wyświetlony komunikat messagebox po stronie klienta ostrzeżenie po kliknięciu przycisku Dodaj produkty z przycisku wydania przy użyciu wartości waluty nieprawidłowe lub ceny o wartości mniejszej niż zero.


[![Wstawianie interfejs jest wyświetlany po kliknięciu przycisku procesu wydania produktu](batch-inserting-cs/_static/image35.png)](batch-inserting-cs/_static/image34.png)

**Rysunek 12**: interfejs Wstawianie jest wyświetlany po kliknięciu przycisku procesu wydania produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image36.png))


## <a name="step-4-adding-the-products"></a>Krok 4: Dodanie produktów

Wszystko, który pozostaje w tym samouczku to można zapisać produktów w bazie danych w produktach Dodaj przycisk wydania s `Click` program obsługi zdarzeń. Można to osiągnąć, tworząc `ProductsDataTable` i dodawanie `ProductsRow` wystąpienia dla każdego z podanych nazw produktów. Po tych `ProductsRow` s dodano podejmiemy wywołanie `ProductsBLL` klasy s `UpdateWithTransaction` metody, przekazując `ProductsDataTable`. Pamiętamy `UpdateWithTransaction` metody, która została utworzona w [opakowywanie modyfikacji bazy danych w ramach transakcji](wrapping-database-modifications-within-a-transaction-cs.md) samouczków, przekazuje `ProductsDataTable` do `ProductsTableAdapter` s `UpdateWithTransaction` metody. Z tego miejsca pracy jest transakcja ADO.NET i problemów TableAdatper `INSERT` instrukcji do bazy danych dla każdego dodano `ProductsRow` w elemencie DataTable. Przy założeniu, że wszystkie produkty są dodawane bez błędów, transakcja została zatwierdzona, w przeciwnym razie jego jest wycofywana.

W kodzie Dodaj produkty z przycisku wydania s `Click` program obsługi zdarzeń musi także wykonać nieco sprawdzania błędów. Ponieważ nie ma żadnych RequiredFieldValidators używany w interfejsie Wstawianie, użytkownik może podać dla produktu pomijając jego nazwę. Ponieważ nazwa produktu s jest wymagany, jeśli taki warunek rozwoju musimy ostrzegać użytkowników i operacje wstawiania nie kontynuować. Pełne `Click` następuje kod procedury obsługi zdarzeń:


[!code-csharp[Main](batch-inserting-cs/samples/sample6.cs)]

Program obsługi zdarzeń, który rozpoczyna się poprzez zapewnienie, że `Page.IsValid` właściwość zwraca wartość `true`. Jeśli zostanie zwrócona `false`, a następnie oznacza to jedną lub więcej CompareValidators zgłaszanej nieprawidłowe dane; w takim przypadku firma Microsoft nie powinien być próbować umieścić wprowadzony produktów lub firma Microsoft będzie znajdą się z powodu wyjątku podczas próby przypisania cena jednostkowa wprowadzonych przez użytkownika wartość, która `ProductsRow` s `UnitPrice` właściwości.

Następnie, nowy `ProductsDataTable` tworzone jest wystąpienie (`products`). A `for` pętli jest używana do iteracji produktu nazwa i cena jednostkowa pola tekstowe i `Text` właściwości są odczytywane w zmiennych lokalnych `productName` i `unitPrice`. Jeśli użytkownik wpisze wartość Cena jednostki, ale nie nazwę odpowiedniego produktu `StatusLabel` wyświetla komunikat, jeśli zostaną podane, jednostka ceny można musi również zawierać nazwę produktu i program obsługi zdarzeń jest został zakończony.

Jeśli podano nazwę produktu nowej `ProductsRow` wystąpienia jest tworzony przy użyciu `ProductsDataTable` s `NewProductsRow` metody. Ta nowa `ProductsRow` wystąpienia s `ProductName` właściwość jest ustawiona na bieżącego produktu polu tekstowym podczas `SupplierID` i `CategoryID` właściwości są przypisywane do `SelectedValue` właściwości kontrolek DROPDOWNLIST w nagłówku Wstawianie interfejsów. Jeśli użytkownik wprowadzi wartość dla cena produktu s, jest ona przypisana do `ProductsRow` wystąpienia s `UnitPrice` właściwości; w przeciwnym razie właściwość jest po lewej stronie nieprzypisane, co spowoduje, że `NULL` wartość `UnitPrice` w bazie danych. Na koniec `Discontinued` i `UnitsOnOrder` właściwości są przypisywane do zakodowanych wartości `false` i 0, odpowiednio.

Po właściwości zostały przypisane do `ProductsRow` wystąpienia jest dodawany do `ProductsDataTable`.

Po zakończeniu `for` pętli, możemy sprawdzić, czy wszystkie produkty zostały dodane. Użytkownik może po wszystkich kliknięto Dodaj produkty z wydania przed wpisaniem nazw produktów ani ceny. Jeśli istnieje co najmniej jeden produkt w `ProductsDataTable`, `ProductsBLL` klasy s `UpdateWithTransaction` metoda jest wywoływana. Następnie dane jest odbitych do `ProductsGrid` GridView tak, aby nowo dodane produkty będą wyświetlane w interfejsie wyświetlania. `StatusLabel` Zostanie zaktualizowany i będzie wyświetlony komunikat potwierdzenia i `ReturnToDisplayInterface` zostanie wywołana, ukrywanie Wstawianie interfejsu i wyświetlanie interfejsu ekranu.

Jeśli produkty nie zostały wprowadzone, wstawianie interfejsu nadal jest wyświetlana, ale wiadomości, które produkty nie zostały dodane. Wprowadź nazwy produktów, a jednostka ceny w tych polach tekstowych są wyświetlane.

S rysunku 13, 14 oraz 15 Pokaż, wstawianie i wyświetlić interfejsy w działaniu. Na rysunku 13 użytkownik wpisze wartość Cena jednostki bez odpowiedniej nazwy produktu. Rysunek 14 pokazuje interfejsu wyświetlaną po trzy nowe produkty zostały dodane pomyślnie, rysunek 15 znajdują się dwa produkty nowo dodanych w kontrolce GridView (trzeci znajduje się na poprzedniej stronie).


[![Nazwa produktu jest wymagany po wprowadzeniu cena jednostkowa](batch-inserting-cs/_static/image38.png)](batch-inserting-cs/_static/image37.png)

**Rysunek 13**: Nazwa produktu jest wymagany po wprowadzeniu cena jednostkowa ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image39.png))


[![Zostały dodane trzy nowe Veggies dla dostawcy Mayumi s](batch-inserting-cs/_static/image41.png)](batch-inserting-cs/_static/image40.png)

**Rysunek 14**: trzy nowe Veggies dodano s Mayumi dostawcy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image42.png))


[![Nowe produkty znajduje się na ostatniej stronie widoku GridView](batch-inserting-cs/_static/image44.png)](batch-inserting-cs/_static/image43.png)

**Rysunek 15**: nowe produkty znajdują się na ostatniej stronie kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-inserting-cs/_static/image45.png))


> [!NOTE]
> Batch Wstawianie logikę używaną w tym samouczku opakowuje operacje wstawiania w zakresie transakcji. Aby to sprawdzić, celowo wprowadzono błąd poziomu bazy danych. Na przykład, zamiast Przypisywanie nowego `ProductsRow` wystąpienia s `CategoryID` właściwości wybranej wartości w `Categories` DropDownList i przypisz ją do wartości, takich jak `i * 5`. W tym miejscu `i` indeksatora pętli i ma wartości z zakresu od 1 do 5. W związku z tym, podczas dodawania dwóch lub więcej produktów w usłudze batch wstawić pierwszy produkt będzie zawierać prawidłowy `CategoryID` będzie miał wartość [5], ale kolejne produktów `CategoryID` wartości, które nie pasują do `CategoryID` wartości w `Categories` tabeli. Netto jest to, że podczas pierwszego `INSERT` zakończy się powodzeniem, kolejne zakończy się niepowodzeniem z naruszenie ograniczenia klucza obcego. Ponieważ wstawianie wsadowe są niepodzielne, pierwszy `INSERT` zostanie wycofana, zwracanie rozpoczął się bazy danych do stanu przed proces usługi batch.


## <a name="summary"></a>Podsumowanie

To oraz dwóch poprzednich samouczków utworzyliśmy interfejsy, które umożliwiają aktualizowanie, usuwanie, i wstawiania partii danych, z których wszystkie używane Obsługa transakcji, dodaliśmy do warstwy dostępu do danych w [opakowywanie modyfikacji bazy danych w ramach transakcji](wrapping-database-modifications-within-a-transaction-cs.md) samouczka. W przypadku niektórych scenariuszy, takie interfejsy użytkownika przetwarzania wsadowego znacząco poprawić wydajność użytkownika końcowego poprzez zmniejszenie liczby kliknięć, ogłaszania zwrotnego i przełączeń kontekstu Mysz klawiatury, zachowując integralności danych bazowych.

W tym samouczku kończy naszych przyjrzeć się praca z partiami danych. Kolejny zbiór samouczki przedstawiono szereg zaawansowanych scenariuszy warstwy dostępu do danych, w tym, za pomocą procedur składowanych w metodach s TableAdapter, konfigurowanie ustawień na poziomie połączenia i polecenia w warstwy DAL, szyfrowania parametrów połączenia i nie tylko!

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Prowadzić recenzentów na potrzeby tego samouczka zostały Hilton Giesenow i S ren Jacob Lauritsen. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](batch-deleting-cs.md)
> [dalej](wrapping-database-modifications-within-a-transaction-vb.md)
