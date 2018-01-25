---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: Raport dla stronicowania i sortowania danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: "Stronicowania i sortowania są dwie funkcje często podczas wyświetlania danych w aplikacji online. W tym samouczku zostaną wykonane pierwszy przyjrzeć się dodanie sortowania i..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 23dbd63110092b2e91b7f3f9f6b602ef917c5527
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="paging-and-sorting-report-data-vb"></a>Stronicowania i sortowania danych raportu (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe) lub [pobierania plików PDF](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> Stronicowania i sortowania są dwie funkcje często podczas wyświetlania danych w aplikacji online. W tym samouczku zostaną wykonane pierwszy przyjrzeć się dodanie sortowania i stronicowania do naszej raporty, które firma Microsoft będzie następnie rozbudowanie samouczków w przyszłości.


## <a name="introduction"></a>Wprowadzenie

Stronicowania i sortowania są dwie funkcje często podczas wyświetlania danych w aplikacji online. Na przykład przy wyszukiwaniu ASP.NET książki w księgarni, może istnieć setki takich książek, ale raport wyświetlania wyników wyszukiwania zawiera maksymalnie dziesięciu dopasowań na każdej stronie. Ponadto wyniki można sortować według tytułu, cen, liczba stron, imię i nazwisko autora i tak dalej. Podczas przeszłości 23 samouczki zbadać sposób tworzenia szerokiej gamy raportów, łącznie z interfejsów, które umożliwiają dodawanie, edytowanie i usuwanie danych, firma Microsoft kolejnych nie wyszukiwanego, w jaki sposób sortować dane i tylko stronicowania przykłady możemy kolejnych widoczne były z widoku DetailsView i FormView formanty.

W tym samouczku będzie przedstawiono sposób dodawania sortowania i stronicowania do naszej raporty, które można wykonać po prostu sprawdzając kilka pól wyboru. Niestety ta implementacja simplistic ma jego wady, które sortowania interfejsu pozostawia typu bit do jest potrzebne i procedury stronicowania nie są przeznaczone do stronicowania wydajnie za pośrednictwem dużych zestawów wyników. Samouczki przyszłych zastosuje jak ominąć ograniczenia out-of--box stronicowania i sortowania rozwiązania.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Krok 1: Dodawanie stronicowania i sortowania Samouczek stron sieci Web

Przed Rozpoczniemy w tym samouczku umożliwiają najpierw Poświęć chwilę, aby dodać stron ASP.NET, które będą potrzebne dla tego samouczka i dalej trzy s. Rozpocznij od utworzenia nowego folderu do projektu o nazwie `PagingAndSorting`. Następnie dodaj następujące pięć stron ASP.NET w tym folderze każdego z nich jest skonfigurowany do używania strony wzorcowej `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![Utwórz PagingAndSorting Folder i dodać strony samouczek platformy ASP.NET](paging-and-sorting-report-data-vb/_static/image1.png)

**Rysunek 1**: Utwórz PagingAndSorting Folder i dodać strony samouczek platformy ASP.NET


Następnie otwórz folder `Default.aspx` strony i przeciągnij `SectionLevelTutorialListing.ascx` kontrolki użytkownika z `UserControls` folderu na powierzchnię projektu. Ten formant użytkownika, który utworzono w [stron wzorcowych i nawigacji w witrynie](../introduction/master-pages-and-site-navigation-vb.md) samouczek, wylicza mapy witryny i wyświetla te samouczki w bieżącej sekcji listy punktowanej.


![Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](paging-and-sorting-report-data-vb/_static/image2.png)

**Rysunek 2**: Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx


W celu wyświetlenia stronicowania i sortowania samouczków, z którymi możemy utworzyć listy punktowanej, należy dodać je do mapy witryny. Otwórz `Web.sitemap` i Dodaj następujący kod po edytowanie, wstawianie i usuwanie znaczników węzeł mapy witryny:


[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]


![Zaktualizuj mapy witryny w celu uwzględnienia nowych stron ASP.NET](paging-and-sorting-report-data-vb/_static/image3.png)

**Rysunek 3**: zaktualizuj mapy witryny w celu uwzględnienia nowych stron ASP.NET


## <a name="step-2-displaying-product-information-in-a-gridview"></a>Krok 2: Wyświetlanie informacji o produkcie, w widoku GridView

Zanim wprowadzania faktycznie stronicowania i sortowania możliwości, chętnie s, najpierw utwórz standardowy z systemem innym niż srotable, niestronicowanej GridView, który zawiera listę informacji o produkcie. Jest to zadanie możemy kolejnych wykonywane wielokrotnie przed w tej serii samouczek więc te kroki należy się zapoznać. Uruchamianie przez otwarcie `SimplePagingSorting.aspx` strony i przeciągnij formant widoku GridView z przybornika do projektanta, ustawienie jej `ID` właściwości `Products`. Następnie należy utworzyć nowy element ObjectDataSource używającej klasy ProductsBLL s `GetProducts()` metody do zwrócenia wszystkich informacji o produkcie.


![Pobierz informacje o wszystkich produktów przy użyciu metody GetProducts()](paging-and-sorting-report-data-vb/_static/image4.png)

**Rysunek 4**: pobieranie informacji na temat wszystkich produktów przy użyciu metody GetProducts()


Ponieważ ten raport jest tylko do odczytu raportu, Brak s nie trzeba mapować ObjectDataSource s `Insert()`, `Update()`, lub `Delete()` metody odpowiadającego `ProductsBLL` metod; w związku z tym wybierz (Brak) z listy rozwijanej dla UPDATE, INSERT i usuwanie kart.


![Wybierz (Brak) opcja na liście rozwijanej w UPDATE, INSERT i usuwanie kart](paging-and-sorting-report-data-vb/_static/image5.png)

**Rysunek 5**: Wybierz (Brak) opcja na liście rozwijanej w UPDATE, INSERT i usuwanie kart


Następnie umożliwiają dostosowywanie pól s widoku GridView, dzięki czemu są wyświetlane tylko nazwy produktów, dostawcy, kategorie, ceny i Stany wycofane s. Ponadto, działania mogą wydać formatowania na poziomie pola zmiany, na przykład dostosowywania `HeaderText` właściwości lub formatowania cen jako walutę. Po wprowadzeniu tych zmian deklaratywne GridView znaczników w s powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

Rysunek 6 przedstawia naszych postępu dotychczasowych widzianego za pośrednictwem przeglądarki. Warto zauważyć, że strona zawiera listę wszystkich produktów w jeden ekran pokazujący każdej nazwy produktu s, kategorii, dostawca, ceny, zaprzestać stanu.


[![Każdy z produktów są wyświetlane](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**Rysunek 6**: każdego produktu są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>Krok 3: Dodawanie obsługi stronicowania

Wyświetlanie listy *wszystkie* produktów na jednym ekranie może prowadzić do przeciążenia informacje dla użytkownika perusing danych. Aby sprawić, że wyniki łatwiejsze w obsłudze, możemy podzielić danych na mniejszym strony danych i umożliwia użytkownikowi kroków opisanych w jedną stronę danych w czasie. Aby wykonać to po prostu zaznacz pole wyboru Włącz stronicowanie w widoku GridView tag inteligentny s (to ustawienie GridView s [ `AllowPaging` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) do `true`).


[![Zaznacz pole wyboru stronicowania Włącz, aby dodać obsługę stronicowania](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**Rysunek 7**: pole wyboru Włącz stronicowanie Aby dodać obsługę stronicowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image11.png))


Włączanie stronicowania ogranicza liczbę rekordów wyświetlanych na stronie i dodaje *interfejsu stronicowania* do widoku GridView. Domyślny interfejs stronicowania, pokazano na rysunku 7 jest szereg numery stron, dzięki czemu można szybko przejść z jednej strony danych do innego użytkownika. Ten interfejs stronicowania powinna wyglądać znajomo, jak firma Microsoft Zapisz go występuje w przypadku dodawania obsługi stronicowania do widoku DetailsView i FormView formantów w ciągu ostatnich samouczkach.

Formanty widoku DetailsView i FormView Pokaż tylko jeden rekord na stronie. Jednak sprawdza widoku GridView, jego [ `PageSize` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) do ustalenia, ile rekordy wyświetlanych na stronie (Ta właściwość domyślnie przyjmowana jest wartość 10).

Tego widoku GridView widoku DetailsView i FormView interfejsu stronicowania s można dostosować z następującymi właściwościami:

- `PagerStyle`Wskazuje informacji o stylu interfejsu stronicowania; można określić ustawień, takich jak `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`i tak dalej.
- `PagerSettings`zawiera bevy właściwości, które można dostosować funkcjonalność interfejsu stronicowania; `PageButtonCount` wskazuje maksymalną liczbę numerów liczbowych strony wyświetlany w interfejsie stronicowania (wartość domyślna wynosi 10); [ `Mode` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) wskazuje, jak działa interfejs stronicowania i może być ustawiony na: 

    - `NextPrevious`Pokazuje przycisków Następny i poprzedni, umożliwiając użytkownikowi krok przodu lub do tyłu jedną stronę w czasie
    - `NextPreviousFirstLast`Oprócz przycisków Następny i poprzedni pierwszy i ostatni przyciski również są uwzględnione, dzięki czemu użytkownik szybko przejść do pierwszej lub ostatniej strony danych
    - `Numeric`przedstawia serię numerów stron, umożliwiając użytkownikowi od razu przejść do określonej strony
    - `NumericFirstLast`oprócz cyfr strony obejmuje pierwszy i ostatni przycisków, dzięki czemu użytkownik szybko przejść do pierwszej lub ostatniej strony danych. przyciski pierwszym i ostatnim są wyświetlane tylko jeśli wszystkie liczby liczbowych strony nie mieści się

Ponadto GridView widoku DetailsView i FormView wszystkie oferty `PageIndex` i `PageCount` właściwości, które wskazują bieżącej strony wyświetlany i całkowita liczba stron danych, odpowiednio. `PageIndex` Właściwość jest indeksowana, zaczynając od 0, co oznacza, że podczas przeglądania danych na pierwszej stronie `PageIndex` będzie równa 0. `PageCount`, z drugiej strony, rozpoczyna zliczanie od 1, co oznacza, że `PageIndex` jest ograniczony do wartości pomiędzy 0 a `PageCount - 1`.

Let s Poświęć chwilę, aby poprawić domyślny wygląd naszego interfejsu stronicowania s widoku GridView. W szczególności umożliwiają s interfejs stronicowania wyrównany światła szarym tle. Zamiast ustawienie tych właściwości bezpośrednio w widoku GridView s `PagerStyle` właściwość let s utworzyć klasę CSS w `Styles.css` o nazwie `PagerRowStyle` , a następnie przypisać `PagerStyle` s `CssClass` właściwości za pośrednictwem naszego motywu. Uruchamianie przez otwarcie `Styles.css` i dodawanie CSS następujące klasy definicji:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

Następnie otwórz folder `GridView.skin` w pliku `DataWebControls` folderze `App_Themes` folderu. Jak wspomniano w *stron wzorcowych i nawigacji w witrynie* samouczek, karnacji plików może służyć do określenia domyślnych wartości właściwości dla formantu sieci Web. W związku z tym uzupełnić istniejące ustawienia, aby uwzględnić ustawienie `PagerStyle` s `CssClass` właściwości `PagerRowStyle`. Ponadto pozwalają s Skonfiguruj interfejs stronicowania pokazanie maksymalnie pięć strony numerycznych przycisków przy użyciu `NumericFirstLast` interfejsu stronicowania.


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>Środowisko użytkownika stronicowania

Rysunek nr 8 przedstawia strony sieci web po odwiedzeniu za pośrednictwem przeglądarki, po sprawdzeniu wyboru Włącz stronicowanie w widoku GridView s i `PagerStyle` i `PagerSettings` konfiguracji zostały wprowadzone za pośrednictwem `GridView.skin` pliku. Uwaga tylko sposób dziesięć rekordów są wyświetlane, a interfejs stronicowania wskazuje wyświetlany na pierwszej stronie danych.


[![Włączone stronicowanie tylko podzestaw rekordów są wyświetlane w czasie](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**Rysunek 8**: włączone stronicowanie, tylko podzestaw rekordów są wyświetlane w czasie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image14.png))


Użytkownik kliknie na jednym z numerów stron w interfejsie stronicowania, ensues odświeżania strony i strony ponowne załadowanie przedstawiający żądanej strony s rekordów. Rysunek 9 pokazuje wyniki po Aby wyświetlić ostatniej strony danych. Zwróć uwagę, że ostatnia strona ma tylko jeden rekord. jest tak, ponieważ ma rekordów 81 w całości, co osiem stron 10 rekordów dla określonej strony jedną stronę z pojedynczy rekord.


[![Kliknięcie numer strony powoduje odświeżenie strony i zawiera odpowiednią podzestaw rekordów](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**Rysunek 9**: kliknięcie numer strony powoduje odświeżenie strony i przedstawia podzbiór odpowiednie rekordy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>Przepływ pracy po stronie serwera s stronicowania

Gdy użytkownik kliknie przycisk w interfejsie stronicowania, ensues odświeżania strony i rozpoczyna się poniższy przepływ pracy po stronie serwera:

1. GridView s (lub widoku DetailsView lub FormView) `PageIndexChanging` generowane zdarzenie
2. Element ObjectDataSource ponownie żądań *wszystkie* danych z logiki warstwy Biznesowej; GridView s `PageIndex` i `PageSize` wartości właściwości są używane do określenia, jakie rekordów zwróconych z logiki warstwy Biznesowej muszą być wyświetlane w widoku GridView
3. GridView s `PageIndexChanged` generowane zdarzenie

W kroku 2 ObjectDataSource żądań ponownie wszystkie dane ze źródła danych. Ten styl stronicowania jest często określana jako *stronicowania domyślne*, ponieważ s zachowanie stronicowania używany domyślnie podczas ustawiania `AllowPaging` właściwości `true`. Domyślne naively stronicowanie danych formantu sieci Web pobiera wszystkie rekordy dotyczące każdej strony danych, mimo że tylko podzestaw rekordów są faktycznie renderowane w kodzie HTML tego s wysyłany do przeglądarki. O ile danych w bazie danych jest buforowana przez element ObjectDataSource lub logiki warstwy Biznesowej, stronicowania domyślny jest niedziałającym wystarczająco dużych zestawów wyników lub aplikacji sieci web za pomocą wielu użytkowników równocześnie.

W następnym samouczku zajmiemy się implementowania *stronicowania niestandardowego*. Stronicowania niestandardowego można w szczególności poinstruować ObjectDataSource tylko pobrać dokładny zestaw rekordów potrzebnych do żądanej strony danych. Jak można wyobrazić sobie stronicowania niestandardowego znacznie poprawia wydajność stronicowania za pośrednictwem dużych zestawów wyników.

> [!NOTE]
> Podczas stronicowania domyślne nie nadaje się podczas stronicowania wielu użytkownikom jednoczesne za pośrednictwem wystarczająco dużych zestawów wyników lub witryny, należy pamiętać, że stronicowania niestandardowego wymaga więcej zmiany i działań zmierzających do wdrożenia i nie jest tak proste, jak sprawdzanie checkbox (ponieważ jest domyślne stronicowanie). W związku z tym domyślna stronicowania może być idealnym wyborem w przypadku małych, mniejszym natężeniu ruchu witryn sieci Web lub gdy stronicowania za pośrednictwem wyników z stosunkowo mały zestawy, ponieważ s znacznie łatwiejsze i szybsze do wdrożenia.


Na przykład jeśli NAS w naszej bazie danych nie będziesz mieć więcej niż 100 produktów, wiemy są bardziej wydajne minimalnego podoba Ci się przez stronicowania niestandardowego jest prawdopodobnie przesunięcia o nakład pracy wymagany do zaimplementowania go. Jeśli jednak może jeden dzień mamy tysięcy lub dziesiątki tysięcy produktów, *nie* Implementowanie stronicowania niestandardowego znacznie może utrudniać skalowalność naszej aplikacji.

## <a name="step-4-customizing-the-paging-experience"></a>Krok 4: Dostosowywanie stronicowania

Formanty sieci Web danych udostępniają wiele właściwości, które mogą służyć do ulepszanie środowiska użytkownika s stronicowania. `PageCount` Właściwości, na przykład wskazuje, ile całkowita liczba stron są, podczas gdy `PageIndex` właściwość wskazuje bieżącą stronę odwiedzana i można ustawić, aby szybko przenieść użytkownika do określonej strony. Ilustrujący sposób używania tych właściwości Aby ulepszyć środowisko użytkownika s stronicowania, let s Dodaj etykietę sieci Web sterowania do strony z informacją, jakie strony one re obecnie odwiedzający wraz z DropDownList formant, który pozwala na szybkie przejście do dowolnej stronie .

Najpierw dodaj formant etykiety w sieci Web do strony, ustaw jej `ID` właściwości `PagingInformation`i wyczyszczenie jego `Text` właściwości. Następnie należy utworzyć programu obsługi zdarzeń dla widoku GridView s `DataBound` zdarzeń i Dodaj następujący kod:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

Ten program obsługi zdarzeń przypisuje `PagingInformation` etykiety s `Text` właściwości komunikat informujący użytkownika strony obecnie odwiedzają `Products.PageIndex + 1` poza liczbę całkowita liczba stron `Products.PageCount` (dodamy od 1 do `Products.PageIndex` właściwości ponieważ `PageIndex` jest indeksowana, zaczynając od 0). Wybrano Przypisz tej etykiety s `Text` właściwości w `DataBound` obsługi zdarzeń w przeciwieństwie do `PageIndexChanged` obsługi zdarzeń ponieważ `DataBound` zdarzenia generowane za każdym razem, gdy danych jest powiązany z widoku GridView, podczas gdy `PageIndexChanged` tylko program obsługi zdarzeń uruchamiany po zmianie indeksu strony. Gdy widoku GridView jest początkowo danymi powiązanymi na pierwszej stronie odwiedzać, `PageIndexChanging` fire t zdarzenia (należy `DataBound` jest zdarzeń).

Z tym dodatkiem użytkownika jest teraz wyświetlany komunikat informujący o jakie strony odwiedzają i liczbę całkowita liczba stron w danych są.


[![Numer bieżącej strony i łączna liczba stron wyświetlanych](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**Na rysunku nr 10**: numer bieżącej strony i łączna liczba stron wyświetlanych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image20.png))


Oprócz formantu etykiety umożliwiają także dodać DropDownList formant, który zawiera numery stron w widoku GridView z aktualnie otwartą stronę wybrane s. W tym miejscu są czy użytkownika można szybko przechodzić z bieżącej strony do innego, po prostu wybierając nowego indeksu strony z DropDownList. Rozpocznij od dodania DropDownList do projektanta, ustawienie jej `ID` właściwości `PageList` i sprawdzanie opcję Włącz AutoPostBack z jego tagów inteligentnych.

Następnie wróć do `DataBound` obsługi zdarzeń i Dodaj następujący kod:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

Ten kod rozpoczyna się po usunięciu zaznaczenia między elementami `PageList` DropDownList. To może wydawać się zbędny, ponieważ jeden t wouldn oczekiwany numer strony, aby zmienić, ale inni użytkownicy mogą jednocześnie przy użyciu systemu, dodawanie lub usuwanie rekordów z `Products` tabeli. Takie wstawienia lub usunięcia można zmienić liczbę stron danych.

Następnie należy ponownie utworzyć numery strony i ma jedną, która mapuje do bieżącego widoku GridView `PageIndex` wybrane domyślnie. Firma Microsoft w tym celu z pętli z zakresu od 0 do `PageCount - 1`, Dodawanie nowej `ListItem` w każdej iteracji i ustawienie jej `Selected` właściwości na wartość true, jeśli bieżący indeks iteracji equals GridView s `PageIndex` właściwości.

Na koniec należy utworzyć programu obsługi zdarzeń dla DropDownList s `SelectedIndexChanged` zdarzenie, które są generowane, zawsze użytkownika, wybierz inny element z listy. Aby utworzyć ten program obsługi zdarzeń, po prostu kliknij dwukrotnie DropDownList w projektancie, a następnie dodaj następujący kod:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

Jak pokazano na rysunku nr 11, zmiana jedynie GridView s `PageIndex` właściwości powoduje, że dane, można odbitych do widoku GridView. W widoku GridView s `DataBound` program obsługi zdarzeń, odpowiednie DropDownList `ListItem` jest zaznaczone.


[![Użytkownik jest automatycznie otwierana szóstego strony po wybraniu elementu listy rozwijanej 6 strony](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**Rysunek 11**: użytkownika jest automatycznie otwierana szóstego strony po wybraniu elementu listy rozwijanej 6 strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>Krok 5: Dodawanie obsługi sortowania dwukierunkowych

Dodawanie sortowania Obsługa dwukierunkowych jest tak proste, jak dodać obsługę stronicowania po prostu zaznacz opcję Włącz sortowanie z tagów inteligentnych s GridView (który ustawia GridView s [ `AllowSorting` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) do `true`). To pozwala każdej nagłówki pól s GridView jako LinkButtons, po kliknięciu, powoduje odświeżenie strony i zwrócić dane posortowane według kolumny klikniętego w kolejności rosnącej. Ponownie ponowne kliknięcie tego samego nagłówka LinkButton sortowania danych w kolejności malejącej.

> [!NOTE]
> Jeśli używasz niestandardowego Warstwa dostępu do danych, a nie typu zestawu danych nie może mieć opcję Włącz sortowanie w widoku GridView tag inteligentny s. Tylko GridViews powiązać ze źródłami danych, które natywnie obsługują sortowanie ma dostępne to pole wyboru. Wpisane DataSet zapewnia obsługę sortowania poza pole, ponieważ zapewnia ADO.NET DataTable `Sort` — metoda, gdy została wywołana, sortuje s DataTable wierszy danych przy użyciu określonych kryteriów.


Jeśli Twoje DAL nie zwraca obiekty, które natywnie Obsługa sortowanie, należy skonfigurować element ObjectDataSource do przekazywania informacji sortowania do warstwy logiki biznesowej, która można sortować dane lub dane sortowane przez warstwę DAL. Firma Microsoft będzie Poznaj sposób sortowania danych w logiki biznesowej i warstwy dostępu do danych w przyszłości samouczka.

LinkButtons sortowania są renderowane jako hiperłącze HTML, którego bieżący kolory (niebieski nieodwiedzonych łącze i ciemny czerwony dla odwiedzonego łącza) mogą powodować konfliktów z kolor tła nagłówka wiersza. Zamiast tego let s ma wszystkie linki wiersz nagłówka wyświetlane jako białe, niezależnie od tego, czy ich kolejnych zostały odwiedzi lub nie. Można to osiągnąć, dodając następujące polecenie, aby `Styles.css` klasy:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

Ta składnia wskazuje, aby użyć białego tekstu, podczas wyświetlania tych hiperłącza w obrębie elementu, który używa klasy HeaderStyle.

Po dodaniu tego CSS podczas odwiedzania strony za pośrednictwem przeglądarki ekranie powinien wyglądać podobnie do rysunku 12. W szczególności rysunku 12 przedstawiono wyniki po kliknięciu łącze nagłówka s pola cen.


[![Wyniki są posortowane według UnitPrice w kolejności rosnącej](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**Rysunek 12**: wyniki są posortowane według UnitPrice w kolejności rosnącej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Sprawdzenie sortowania przepływu pracy

GridView wszystkie pola elementu BoundField, CheckBoxField, TemplateField, i mieć itp `SortExpression` właściwość, która określa wyrażenie, które mają być używane do posortować dane, po kliknięciu tego pola s sortowania nagłówka łącza. Ma również widoku GridView `SortExpression` właściwości. Podczas sortowania nagłówka zostanie kliknięty przycisk łącza widoku GridView przypisuje tego pola s `SortExpression` do wartości jego `SortExpression` właściwości. Następnie ponownie pobrany z elementu ObjectDataSource i posortowane według GridView s danych `SortExpression` właściwości. Poniżej przedstawiono szczegóły wynika, gdy użytkownik końcowy sortuje dane w widoku GridView sekwencja kroków:

1. GridView s [zdarzenia Sorting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) uruchamiany
2. GridView s [ `SortExpression` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) ma ustawioną wartość `SortExpression` pola, którego sortowania nagłówek został kliknięty przycisk łącza
3. Element ObjectDataSource ponownie pobiera wszystkie dane z logiki warstwy Biznesowej i następnie sortuje danych za pomocą s widoku GridView`SortExpression`
4. GridView s `PageIndex` właściwość zostanie zresetowana do 0, co oznacza, że podczas sortowania użytkownika jest zwracana do pierwszej strony danych (przy założeniu, obsługę stronicowania została zaimplementowana)
5. GridView s [ `Sorted` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) uruchamiany

Jak stronicowania domyślne domyślną opcję sortowania ponownie pobiera *wszystkie* rekordy z logiki warstwy Biznesowej. Podczas korzystania z sortowania bez stronicowania lub w przypadku korzystania z sortowania z domyślne stronicowania, że s ma sposobu obejścia tego wydajności trafień (zbyt mała buforowanie danych w bazie danych). Jednak, ponieważ zajmiemy się w przyszłości samouczek go s umożliwia wydajne sortowanie danych podczas używania stronicowania niestandardowego.

Podczas tworzenia wiązania elementu ObjectDataSource GridView za pomocą listy rozwijanej w widoku GridView tag inteligentny s, każde pole widoku GridView automatycznie ma jego `SortExpression` właściwości, przypisane do nazwę pola danych w `ProductsRow` klasy. Na przykład `ProductName` s elementu BoundField `SortExpression` ma ustawioną wartość `ProductName`, jak przedstawiono w następujących deklaratywne kod znaczników:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

Pola można skonfigurować, aby go s nie można sortować, czyszcząc jego `SortExpression` właściwości (przypisywanie do pustego ciągu). Na przykład załóżmy, że firma Microsoft chcesz zezwolić klientom sortować naszych produktów ceny. `UnitPrice` s elementu BoundField `SortExpression` usunąć właściwości z deklaratywne znaczników lub za pomocą okna dialogowego pola (która jest dostępna, klikając łącze Edytuj kolumny w widoku GridView tag inteligentny s).


![Wyniki są posortowane według UnitPrice w kolejności rosnącej](paging-and-sorting-report-data-vb/_static/image27.png)

**Rysunek 13**: wyniki są posortowane według UnitPrice w kolejności rosnącej


Raz `SortExpression` właściwości zostały usunięte przez `UnitPrice` elementu BoundField, nagłówek jest renderowane jako tekst, a nie jako łącze, zapobiegając w ten sposób użytkownicy z sortowania danych przez cenę.


[![Przez usunięcie właściwości SortExpression, użytkownicy mogą już sortować produktów przez cen](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**Rysunek 14**: przez usunięcie właściwości SortExpression, użytkownicy mogą już sortować produktów przez cen ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>Programowo sortowania w widoku GridView

Można także sortować zawartość widoku GridView programowo przy użyciu widoku GridView s [ `Sort` metody](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Po prostu Przekaż `SortExpression` wartość, aby posortować według wraz z [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` lub `Descending`), i dane s GridView będą ponownie sortowane.

Wyobraź sobie powód możemy wyłączenia sortowanie według `UnitPrice` został, ponieważ wystąpiły Boisz, się klientów po prostu kupuje tylko produkty najniższej cenie. Jednak chcemy zachęcić kupić najdroższych produktów, dlatego firma Microsoft d podoba, aby można było sortować produkty cen, ale tylko z najdroższych cen do najmniej.

Celu formantu przycisku sieci Web to dodanie do strony, ustaw jej `ID` właściwości `SortPriceDescending`i jego `Text` właściwości do sortowania według ceny. Następnie należy utworzyć program obsługi zdarzeń dla przycisku s `Click` zdarzeń, klikając dwukrotnie formantu przycisku w projektancie. Dodaj następujący kod, aby ten program obsługi zdarzeń:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

Kliknięcie tego przycisku zwraca użytkownika do pierwszej strony z produktami posortowane według cen z najdroższych do najniższych (patrz rysunek 15).


[![Kliknięcie przycisku porządkuje produktów z najbardziej kosztownych do najmniej](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**Rysunek 15**: kliknięcie przycisku porządkuje produktów z najbardziej kosztowne o najmniejszej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image33.png))


## <a name="summary"></a>Podsumowanie

W tym samouczku widzieliśmy implementowania stronicowania i sortowania możliwości które zostały tak proste, jak sprawdzanie wyboru! Gdy użytkownik sortuje lub strony przy użyciu danych, otwierany podobne przepływu pracy:

1. Ensues odświeżania strony
2. Danych formantu sieci Web s wstępnie poziomu generowane zdarzenie (`PageIndexChanging` lub `Sorting`)
3. Ponownie pobrać wszystkich danych przez element ObjectDataSource
4. Danych formantu sieci Web s po poziomu generowane zdarzenie (`PageIndexChanged` lub `Sorted`)

Podczas implementowania podstawowe stronicowania i sortowania jest ona błyskawicznie, należy użyć więcej wysiłku, mogą korzystać z bardziej wydajne stronicowania niestandardowego lub aby zwiększyć interfejsu stronicowania i sortowania. Samouczki przyszłych może zapoznać się w tych tematach.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Poprzednie](creating-a-customized-sorting-user-interface-cs.md)
[dalej](efficiently-paging-through-large-amounts-of-data-vb.md)
