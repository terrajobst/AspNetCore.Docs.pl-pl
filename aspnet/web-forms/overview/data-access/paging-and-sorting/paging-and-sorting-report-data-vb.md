---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: Stronicowanie i sortowanie danych raportu (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Stronicowanie i sortowanie są dwie funkcje bardzo często, gdy wyświetlanie danych w aplikacji online. W tym samouczku przeniesiemy Pierwsze spojrzenie na dodanie, sortowanie i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 6de1f78bab795ef191553bf0d58b3b29c26d7d85
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387653"
---
<a name="paging-and-sorting-report-data-vb"></a>Stronicowanie i sortowanie danych raportu (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe) lub [Pobierz plik PDF](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> Stronicowanie i sortowanie są dwie funkcje bardzo często, gdy wyświetlanie danych w aplikacji online. W tym samouczku zostaną wykonane Pierwsze spojrzenie na dodawanie sortowania i stronicowania naszych raportów, które następnie utworzymy od samouczków w przyszłości.


## <a name="introduction"></a>Wprowadzenie

Stronicowanie i sortowanie są dwie funkcje bardzo często, gdy wyświetlanie danych w aplikacji online. Na przykład podczas szukania ASP.NET książki w księgarni online, mogą istnieć setki takich książki, ale raport z listą wyników wyszukiwania zawiera maksymalnie dziesięciu dopasowań na każdej stronie. Ponadto można posortować wyników według tytułu, ceny, liczbę stron, imię i nazwisko autora i tak dalej. While przeszłości 23 samouczki zbadaniu sposób tworzenia szerokiej gamy raportów, w tym interfejsy, które zezwala na dodawanie, edytowanie i usuwanie danych, firma Microsoft ve nie przyjrzano się jak sortować dane i tylko stronicowania przykłady możemy ve widoczne zostały DetailsView i FormView kontrolki.

W tym samouczku opisano sposób dodawania sortowania i stronicowania naszych raportów, które można osiągnąć, po prostu sprawdzanie kilku pól wyboru. Niestety ta implementacja uproszczony ma swoje wady, które sortowania interfejsu pozostawia nieco się być wskazane i procedury stronicowania nie są przeznaczone do efektywne stronicowanie dużych zestawów wyników. Samouczki przyszłych Przedstawimy sposób ominąć ograniczenia out-of--box stronicowanie i sortowanie rozwiązania.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Krok 1: Dodawanie stronicowanie i sortowanie Samouczek stron sieci Web

Zanim zaczniemy, w tym samouczku, chętnie s najpierw Poświęć chwilę, aby dodać strony ASP.NET, które będą potrzebne dla tego samouczka i dalej trzy. Rozpocznij od utworzenia nowego folderu w projekcie o nazwie `PagingAndSorting`. Następnie dodaj następujące pięć strony ASP.NET, w tym folderze każdego z nich jest skonfigurowany do używania strony wzorcowej `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![Utwórz PagingAndSorting Folder i dodawanie stron samouczek platformy ASP.NET](paging-and-sorting-report-data-vb/_static/image1.png)

**Rysunek 1**: Utwórz PagingAndSorting Folder i dodawanie stron samouczek platformy ASP.NET


Następnie otwórz `Default.aspx` strony, a następnie przeciągnij `SectionLevelTutorialListing.ascx` kontrolki użytkownika od `UserControls` folder na powierzchnię projektu. Ten formant użytkownika, które utworzyliśmy w [strony wzorcowe i nawigacja w witrynie](../introduction/master-pages-and-site-navigation-vb.md) samouczek, wylicza mapy witryny i wyświetla te samouczki w bieżącej sekcji na liście punktowanej.


![Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](paging-and-sorting-report-data-vb/_static/image2.png)

**Rysunek 2**: Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx


Aby ma na liście punktowanej wyświetlić stronicowanie i sortowanie samouczków, z którymi firma utworzona, należy dodać je do mapy witryny. Otwórz `Web.sitemap` pliku i Dodaj następujący kod po edycji, wstawianie i usuwanie znaczników węzeł mapy witryny:


[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]


![Aktualizacja mapy witryny, aby uwzględnić nowe strony ASP.NET](paging-and-sorting-report-data-vb/_static/image3.png)

**Rysunek 3**: Aktualizacja mapy witryny, aby uwzględnić nowe strony ASP.NET


## <a name="step-2-displaying-product-information-in-a-gridview"></a>Krok 2: Wyświetlanie informacji o produkcie w widoku GridView

Przed faktycznie wdrażamy, stronicowanie i sortowanie możliwości, niech s, należy najpierw utworzyć standardowe innych niż srotable, niestronicowanej GridView, który zawiera informacje o produkcie. Jest to zadanie możemy ve wykonywane wiele razy przed w całej tej serii samouczków więc te kroki należy się zapoznać. Zacznij od otwarcia `SimplePagingSorting.aspx` strony, a następnie przeciągnij kontrolki widoku siatki z przybornika w projektancie, ustawiając jego `ID` właściwość `Products`. Następnie należy utworzyć nowe kontrolki ObjectDataSource, która używa klasy ProductsBLL s `GetProducts()` metodę, aby zwrócić wszystkie informacje o produkcie.


![Pobierz informacje o wszystkich produktów za pomocą metody GetProducts()](paging-and-sorting-report-data-vb/_static/image4.png)

**Rysunek 4**: pobieranie informacji o wszystkich produktów za pomocą metody GetProducts()


Ponieważ ten raport jest tylko do odczytu raportu, s tam nie ma potrzeby mapowania ObjectDataSource s `Insert()`, `Update()`, lub `Delete()` metody odpowiadającej `ProductsBLL` metod; w związku z tym, wybrać (Brak) z listy rozwijanej dla UPDATE, INSERT Usuwanie karty i.


![Wybierz (Brak) opcji listy rozwijanej w aktualizacji, WSTAWIANIA i usuwania karty](paging-and-sorting-report-data-vb/_static/image5.png)

**Rysunek 5**: Wybierz (Brak) opcji listy rozwijanej w aktualizacji, WSTAWIANIA i usuwania karty


Następnie pozwalają dostosować pola s GridView, aby były wyświetlane tylko nazwy produktów, dostawcy, kategorie, ceny i Stany nieobsługiwane s. Ponadto, możesz wprowadzić formatowanie na poziomie pola zmienia się, takich jak dostosowywanie `HeaderText` właściwości lub formatowania cena jako walutę. Po wprowadzeniu tych zmian deklaratywne kontrolki GridView znaczników w s powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

Rysunek 6. pokazuje nasz postęp tej pory, podczas wyświetlania za pośrednictwem przeglądarki. Pamiętaj, że strona zawiera listę wszystkich produktów na jednym ekranie, przedstawiający każdej nazwy produktu s, kategorii, dostawca, ceny, wycofane stanu.


[![Każdy z produktów są wyświetlane](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**Rysunek 6**: każdego produktu są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>Krok 3: Dodawanie obsługi stronicowania

Wyświetlanie listy *wszystkich* produktów na jednym ekranie może doprowadzić do przeciążenia informacji dla użytkownika perusing danych. Aby wyniki łatwiejsze w obsłudze, możemy podzielić dane na mniejsze strony danych i umożliwia użytkownikowi przejść przez jedną stronę danych w czasie. Aby wykonać to po prostu zaznacz pole wyboru Włączanie stronicowania, za pomocą tagu inteligentnego s GridView (to ustawienie GridView s [ `AllowPaging` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) do `true`).


[![Zaznacz pole wyboru stronicowania Włącz, aby dodać obsługę stronicowania](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**Rysunek 7**: Sprawdź Włączanie stronicowania pole wyboru, aby dodać obsługę stronicowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image11.png))


Włączanie stronicowania ogranicza liczbę rekordów wyświetlanych na stronie i dodaje *interfejsu stronicowania* do kontrolki GridView. Domyślny interfejs stronicowania pokazano na rysunku 7 jest szereg numery stron, co pozwala na szybkie przechodzenie z jednej strony danych do innego. Ten interfejs stronicowania powinien wyglądać dobrze znanych, ponieważ ve widać go podczas dodawania kontrolek DetailsView i FormView obsługę stronicowania w poprzednich samouczkach.

Obu DetailsView FormView formantów i wyświetlić tylko jeden rekord na każdej stronie. GridView, jednak konsultować się z jego [ `PageSize` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) do ustalenia, ile rekordy wyświetlanych na stronie (Ta właściwość jest domyślnie ustawiona na wartość 10).

Można dostosować ten GridView DetailsView i FormView s stronicowania interfejs z następującymi właściwościami:

- `PagerStyle` Określa informacje o interfejsie stronicowania; stylu można określić ustawienia takie jak `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`i tak dalej.
- `PagerSettings` zawiera bevy właściwości, które można dostosować funkcjonalności interfejsu stronicowania; `PageButtonCount` wskazuje maksymalną liczbę cyfr stronę liczbowe wyświetlane w interfejsie stronicowania (wartość domyślna wynosi 10); [ `Mode` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) wskazuje, jak działa interfejs stronicowania i może być ustawiona na: 

    - `NextPrevious` Pokazuje przycisków dalej i Wstecz, umożliwiając użytkownikowi krok przodu lub do tyłu po jednej stronie naraz
    - `NextPreviousFirstLast` Oprócz przycisków dalej i Wstecz imię i nazwisko przyciski są również uwzględniane, co pozwala na szybkie przeniesienie do pierwszej lub ostatniej strony danych
    - `Numeric` zawiera szereg numery stron, dzięki czemu użytkownik od razu przejść do dowolnej strony
    - `NumericFirstLast` Oprócz numery stron zawiera przyciski imię i nazwisko, co pozwala na szybkie przeniesienie do pierwszej lub ostatniej strony danych. przyciski pierwszego/ostatniego są wyświetlane tylko jeśli wszystkie liczby stronę liczbowe nie mieści się

Ponadto GridView DetailsView i FormView wszystkie oferty `PageIndex` i `PageCount` właściwości, które wskazują na wyświetlanej stronie bieżącej i łączna liczba stron danych, odpowiednio. `PageIndex` Właściwości są indeksowane począwszy od 0, co oznacza, że podczas wyświetlania na pierwszej stronie danych `PageIndex` będzie równa 0. `PageCount`, z drugiej strony, rozpoczyna zliczanie od 1, co oznacza, że `PageIndex` jest ograniczona do wartości z zakresu od 0 i `PageCount - 1`.

Pozwól, s, Poświęć chwilę, aby poprawić wygląd domyślny naszego interfejsu stronicowania s GridView. W szczególności umożliwiają s interfejs stronicowania wyrównany do prawej światła szarym tle. Zamiast ustawienie tych właściwości bezpośrednio za pomocą GridView s `PagerStyle` właściwości umożliwiają s utworzyć klasę CSS z `Styles.css` o nazwie `PagerRowStyle` , a następnie przypisać `PagerStyle` s `CssClass` właściwości, za pośrednictwem naszego motywu. Zacznij od otwarcia `Styles.css` i dodanie następujących CSS definicję klasy:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

Następnie otwórz `GridView.skin` w pliku `DataWebControls` folder wewnątrz `App_Themes` folderu. Tak jak Omówiliśmy to w *strony wzorcowe i nawigacja w witrynie* samouczek, karnacji plików może służyć do określenia domyślnych wartości właściwości kontrolki sieci Web. W związku z tym, rozszerzyć istniejące ustawienia, aby uwzględnić ustawienie `PagerStyle` s `CssClass` właściwość `PagerRowStyle`. Ponadto należy skonfigurować interfejs stronicowania, aby wyświetlić maksymalnie pięć stronę liczbowe przyciski, za pomocą umożliwiają s `NumericFirstLast` interfejsu stronicowania.


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>Środowisko użytkownika stronicowania

Rysunek 8 przedstawia strony sieci web po odwiedzeniu za pośrednictwem przeglądarki, po dokonaniu wyboru Włączanie stronicowania s GridView kontroli i `PagerStyle` i `PagerSettings` konfiguracji zostały wprowadzone za pośrednictwem `GridView.skin` pliku. Uwaga jak tylko dziesięć rekordy są wyświetlane, a interfejs stronicowania wskazuje, wyświetlania na pierwszej stronie danych.


[![Za pomocą stronicowania włączone tylko podzestaw rekordów są wyświetlane w czasie](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**Rysunek 8**: włączone stronicowanie, tylko podzestaw rekordów wyświetlane są w czasie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image14.png))


Gdy użytkownik kliknie na jednym z numery stron w interfejsie stronicowania, ensues odświeżenie strony i strona ładuje się przedstawiający, że żądana strona s rekordów. Po przystąpieniu do wyświetlania danych na ostatniej stronie rysunku nr 9 przedstawiono wyniki. Należy zauważyć, że ostatnia strona ma tylko jeden rekord. jest to spowodowane istnieją rekordy 81 w całości, co osiem stron 10 rekordów dla określonej strony po jednej stronie pojedynczy rekord.


[![Kliknięcie na numer strony powoduje odświeżenie strony i zawiera odpowiednią podzestaw rekordów](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**Rysunek 9**: kliknięcie na numer strony powoduje odświeżenie strony i przedstawia podzbiór odpowiednie rekordy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>Przepływ pracy po stronie serwera s stronicowania

Gdy użytkownik końcowy kliknie przycisk w interfejsie stronicowania, ensues odświeżenie strony i rozpoczyna się poniższy przepływ pracy po stronie serwera:

1. S kontrolki GridView (lub DetailsView lub FormView) `PageIndexChanging` generowane zdarzenie
2. Kontrolki ObjectDataSource ponownie żąda *wszystkich* danych od LOGIKI; GridView s `PageIndex` i `PageSize` wartości właściwości są używane do określenia rekordów zwracanych z LOGIKI muszą być wyświetlana w widoku GridView
3. GridView s `PageIndexChanged` generowane zdarzenie

W kroku 2 kontrolki ObjectDataSource żąda ponownego wszystkich danych ze źródła danych. Ten styl stronicowania jest często nazywany *stronicowania domyślne*, ponieważ s zachowanie stronicowania, używany domyślnie podczas ustawiania `AllowPaging` właściwość `true`. Domyślne naively stronicowanie danych formantu sieci Web pobiera wszystkie rekordy, dla każdej strony danych, mimo że tylko podzestaw rekordów są faktycznie renderowane w kodzie HTML tego s wysyłany do przeglądarki. Chyba że danych w bazie danych jest buforowany przez LOGIKI lub kontrolki ObjectDataSource, stronicowanie domyślny jest niedziałającym wystarczająco dużych zestawów danych lub aplikacji sieci web z wieloma równoczesnymi użytkownikami.

W następnym samouczku zajmiemy się sposób implementacji *stronicowania niestandardowego*. Za pomocą niestandardowych stronicowania może specjalnie wydać ObjectDataSource w celu pobrania tylko dokładny zestaw rekordów potrzebnych do żądanej strony danych. Jak możesz sobie wyobrazić, stronicowania niestandardowego znacznie poprawia wydajność stronicować dużych zestawów wyników.

> [!NOTE]
> Gdy stronicowania domyślny nie jest odpowiedni w przypadku, gdy stronicowanie za pośrednictwem wystarczająco duży zestaw wyników lub witryny za pomocą wielu równoczesnych użytkowników, należy pamiętać, że stronicowania niestandardowego wymaga więcej zmian i wysiłku, aby zaimplementować i nie jest tak proste, jak zaznaczenia pola wyboru (co jest domyślny stronicowanie). W związku z tym, stronicowania domyślny może być idealnym wyborem dla małych, o małym natężeniu ruchu witryn sieci Web lub gdy stronicowanie za pośrednictwem wyników z niewielką zestawy, ponieważ s znacznie łatwiejsze i szybsze do wdrożenia.


Na przykład gdy wiemy, że nigdy nie odpowiemy od ponad 100 produktów w naszej bazie danych, przyrost wydajności minimalnej przysługujące stronicowania niestandardowego jest prawdopodobnie równoważona nakład pracy wymagany do jej wdrożenia. Jeśli jednak może być jeden dzień mamy tysiące lub dziesiątki tysięcy produktów, *nie* Implementowanie stronicowania niestandardowego z kolei będzie znacznie ograniczać skalowalność naszej aplikacji.

## <a name="step-4-customizing-the-paging-experience"></a>Krok 4: Dostosowywanie stronicowania

Kontrolki internetowe danych zapewniają wiele właściwości, których można użyć w celu poprawienia środowiska użytkownika s stronicowania. `PageCount` Właściwość, na przykład, wskazuje, ile łączna liczba stron są, podczas gdy `PageIndex` właściwość wskazuje bieżącą stronę odwiedzana i można ustawić, aby szybko przejść przez użytkownika do określonej strony. Aby zilustrować, jak użyć tych właściwości, aby ulepszyć środowisko użytkownika s stronicowania, niech s, Dodaj etykietę sieci Web kontrolę naszą stronę, informujący użytkownika, które strony są ponownie obecnie odwiedzający wraz z kontrolki DropDownList, która pozwala na szybkie przejście do dowolnej stronie .

Najpierw należy dodać formant etykiety w sieci Web do strony, ustaw jego `ID` właściwości `PagingInformation`i wyczyszczenie jego `Text` właściwości. Następnie należy utworzyć program obsługi zdarzeń dla GridView s `DataBound` zdarzeń i Dodaj następujący kod:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

Przypisuje ten program obsługi zdarzeń `PagingInformation` etykiety s `Text` właściwość komunikat z informacją użytkownika strony obecnie odwiedzają `Products.PageIndex + 1` poza ile łączna liczba stron `Products.PageCount` (dodamy 1, aby `Products.PageIndex` właściwość ponieważ `PageIndex` są indeksowane począwszy od 0). Wybieram Przypisz tej etykiety s `Text` właściwości w `DataBound` programu obsługi zdarzeń, w przeciwieństwie do `PageIndexChanged` programu obsługi zdarzeń ponieważ `DataBound` zdarzeń jest uruchamiana za każdym razem, gdy powiązania danych z widoku GridView, natomiast `PageIndexChanged` tylko program obsługi zdarzeń generowane, gdy zostanie zmieniona indeks strony. GridView po początkowym danymi powiązanymi na pierwszej stronie odwiedź `PageIndexChanging` zdarzeń t ogień (natomiast `DataBound` zdarzeń jest).

Dodając ten użytkownika są teraz wyświetlane komunikat wskazujący, jaki strony odwiedzają i ile łączna liczba stron danych istnieje.


[![Numer bieżącej strony i łączna liczba stron są wyświetlane](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**Na rysunku nr 10**: numer bieżącej strony i łączna liczba stron są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image20.png))


Oprócz formant etykiety pozwalają również dodać formant DropDownList, który wyświetla numery stron w widoku GridView z aktualnie otwartą stronę wybrane s. W tym miejscu chodzi o to, że użytkownik można szybko przechodzić z bieżącej strony do innego, po prostu wybierając nowy indeks strony z metody DropDownList. Rozpocznij od dodania kontrolki DropDownList do projektanta, ustawiając jego `ID` właściwość `PageList` a zaznaczając opcję włączenia automatycznego ogłaszania zwrotnego w tagu inteligentnego.

Następnie wróć do `DataBound` programu obsługi zdarzeń i Dodaj następujący kod:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

Ten kod, który rozpoczyna się przez wyczyszczenie między elementami `PageList` DropDownList. To może wydawać się zbędne, ponieważ jeden t wouldn oczekiwany numer strony, aby zmienić, ale inni użytkownicy mogą jednocześnie przy użyciu systemu, dodawanie lub usuwanie rekordów z `Products` tabeli. Takie wstawienia lub usunięcia można zmienić liczbę stron danych.

Następnie należy ponownie utworzyć numery stron i ten, który mapuje bieżącego widoku GridView `PageIndex` wybrane domyślnie. Możemy to zrobić za pomocą pętli z zakresu od 0 do `PageCount - 1`, dodawania nowego `ListItem` w każdej iteracji i ustawienie jej `Selected` właściwość na true, jeśli bieżący indeks iteracji jest równe GridView s `PageIndex` właściwości.

Na koniec musimy utworzyć program obsługi zdarzeń dla DropDownList s `SelectedIndexChanged` zdarzenie, które są generowane każdorazowo użytkownika, wybierz inny element z listy. Aby utworzyć ten program obsługi zdarzeń, po prostu kliknij dwukrotnie ikonę kontrolki DropDownList w projektancie, a następnie dodaj następujący kod:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

Jak pokazano na ilustracji 11, jedynie zmiana GridView s `PageIndex` właściwości powoduje, że dane, które mają być odbitych do kontrolki GridView. W tym s GridView `DataBound` programu obsługi zdarzeń, odpowiednie metody DropDownList `ListItem` jest zaznaczone.


[![Użytkownik jest automatycznie otwierana szóstego strony po wybraniu elementu listy rozwijanej 6 strony](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**Rysunek 11**: użytkownik jest automatycznie otwierana szóstego strony po wybraniu elementu listy rozwijanej 6 strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>Krok 5: Dodawanie obsługi sortowania dwukierunkowa

Dodawanie Obsługa sortowania dwukierunkowa jest tak proste. wystarczy dodać obsługę stronicowania po prostu zaznacz opcję Włącz sortowanie tagów inteligentnych s kontrolki GridView (która ustawia GridView s [ `AllowSorting` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) do `true`). To powoduje wyświetlenie wszystkich nagłówków pola s GridView jako LinkButtons, po kliknięciu, powoduje odświeżenie strony i zwracać dane posortowane według kliknięto kolumny w kolejności rosnącej. Ponownie klikając ten sam nagłówek LinkButton ponownie sortując dane w kolejności malejącej.

> [!NOTE]
> Jeśli używasz niestandardowego warstwy dostępu do danych, a nie wpisana zestawu danych nie może mieć opcję Włącz sortowanie w tagu inteligentnego s GridView. Tylko GridViews powiązać ze źródłami danych, które natywnie obsługują sortowanie ma dostępne to pole wyboru. Wpisany zestaw danych zapewnia obsługę sortowania out-of--box, ponieważ zapewnia ADO.NET DataTable `Sort` metody, która przy wywołaniu sortuje s elementu DataTable przy użyciu kryteriów określonych wierszy danych.


Jeśli Twoje DAL nie zwraca obiekty, które natywnie pomocy technicznej, sortowanie, musisz skonfigurować ObjectDataSource do przekazywania informacji sortowania do warstwy logiki biznesowej, które można sortować dane lub dane posortowane przez warstwę DAL. Pokażemy, jak sortować dane na logice biznesowej i warstwy dostępu do danych w przyszłości zapoznać się z samouczkiem.

LinkButtons sortowania są renderowane jako hiperłącza HTML, którego bieżącym kolory (niebieski nieodwiedzonych łącze i Ciemnoczerwony dla odwiedzony Odnośnik) mogą powodować konfliktów z kolor tła w wierszu nagłówka. Zamiast tego umożliwiają s ma wszystkie łącza wiersz nagłówka wyświetlana białe, niezależnie od tego, czy ich ve zostały odwiedzone, czy nie. Można to osiągnąć, dodając następujące polecenie, aby `Styles.css` klasy:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

Ta składnia wskazuje, aby użyć białego tekstu, podczas wyświetlania hiperlinków w obrębie elementu, który używa klasy HeaderStyle.

Po dodaniu tego CSS podczas odwiedzania strony za pośrednictwem przeglądarki ekran powinien wyglądać podobnie jak rysunek 12. W szczególności rysunku 12 przedstawiono wyniki, po kliknięciu łącza nagłówka s pola Cena.


[![Wyniki są posortowane według UnitPrice w kolejności rosnącej](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**Rysunek 12**: wyniki są posortowane według UnitPrice w kolejności rosnącej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Badanie sortowania przepływu pracy

Wszystkie kontrolki GridView pól elementu BoundField, CheckBoxField TemplateField i mieć itd `SortExpression` właściwość, która określa wyrażenie które powinny być używane do sortowania danych, po kliknięciu tego s pola sortowania nagłówka łącza. Ma również widoku GridView `SortExpression` właściwości. Podczas sortowania nagłówka kliknięto element LinkButton widoku GridView przypisuje s tego pola `SortExpression` wartość do jego `SortExpression` właściwości. Następnie dane są ponownie pobierane z kontrolki ObjectDataSource i posortowane zgodnie z tym s GridView `SortExpression` właściwości. Poniżej przedstawiono szczegółową listę sekwencji kroków, które wynika, gdy użytkownik końcowy sortując dane w widoku GridView:

1. GridView s [zdarzeń sortowanie](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) generowane
2. GridView s [ `SortExpression` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) jest ustawiona na `SortExpression` pola, którego sortowania nagłówka kliknął element LinkButton
3. Kontrolki ObjectDataSource ponownie pobiera wszystkie dane z LOGIKI, a następnie sortuje danych za pomocą GridView s `SortExpression`
4. GridView s `PageIndex` właściwość zostaje zresetowana do 0, co oznacza, że podczas sortowania użytkownika jest zwracany do pierwszej strony danych (zakładając, że wdrożono obsługę stronicowania)
5. GridView s [ `Sorted` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) generowane

Jak domyślne stronicowania, domyślne sortowanie opcji ponownie pobiera *wszystkich* rekordy z LOGIKI. Podczas korzystania z sortowania bez stronicowania lub w przypadku korzystania z sortowania z domyślnie stronicowania, miejsca s ma sposobu obejścia tego wydajności trafień (ograniczony buforowanie danych w bazie danych). Jednakże, jak firma Microsoft znajduje się w przyszłości — samouczek, jego s umożliwia wydajne sortowania danych, korzystając z niestandardowego stronicowania.

Podczas tworzenia wiązania kontrolki ObjectDataSource do kontrolki GridView za pomocą listy rozwijanej w tagu inteligentnego s GridView, każde pole GridView automatycznie ma jego `SortExpression` przypisano nazwę pola danych właściwości `ProductsRow` klasy. Na przykład `ProductName` s elementu BoundField `SortExpression` jest ustawiona na `ProductName`, jak pokazano w poniższym oznaczeniu deklaracyjnym:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

Pola można skonfigurować tak, aby go s nie można sortować, czyszcząc jego `SortExpression` właściwości (przypisywanie do pustego ciągu). Na przykład załóżmy, że firma Microsoft po chcesz zezwolić klientom sortować nasze produkty według ceny. `UnitPrice` s elementu BoundField `SortExpression` właściwość może zostać usunięty w oznaczeniu deklaracyjnym, albo za pomocą okna dialogowego pola, (który jest dostępny, klikając łącze Edytowanie kolumn w widoku GridView tagu inteligentnego s).


![Wyniki są posortowane według UnitPrice w kolejności rosnącej](paging-and-sorting-report-data-vb/_static/image27.png)

**Rysunek 13**: wyniki są posortowane według UnitPrice w kolejności rosnącej


Gdy `SortExpression` właściwość została usunięta z `UnitPrice` elementu BoundField, nagłówek jest renderowane jako tekst, a nie jako łącze, zapobiegając w ten sposób użytkownicy z sortowanie danych według ceny.


[![Przez usunięcie właściwości SortExpression, użytkownicy będą mogli już sortować produkty według ceny](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**Rysunek 14**: przez usunięcie właściwości SortExpression, użytkownicy będą mogli już sortować według produktów ceny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>Programowe sortowanie widoku GridView

Można również sortować zawartość widoku GridView programowo przy użyciu GridView s [ `Sort` metoda](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Po prostu przekazać `SortExpression` wartość, aby posortować według wraz z [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` lub `Descending`), GridView s data. zostanie ona ponownie posortowana.

Wyobraź sobie, że przyczyna możemy wyłączona sortowanie według `UnitPrice` była, ponieważ możemy Boisz się, naszych klientów po prostu kupuje tylko produkty najtańsza. Jednakże chcemy zachęcenie ich do kupować produkty najbardziej kosztowne, więc d staramy się udać posortować według ceny, ale tylko z najbardziej kosztowne ceny produktów do najmniej.

Aby wykonać to dodanie kontrolki przycisku w sieci Web do strony, ustaw jego `ID` właściwości `SortPriceDescending`i jego `Text` właściwości Sortuj według ceny. Następnie należy utworzyć procedurę obsługi zdarzeń dla przycisku s `Click` zdarzeń przez dwukrotne kliknięcie formantu przycisku w projektancie. Dodaj następujący kod do tego programu obsługi zdarzeń:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

Kliknięcie tego przycisku zwraca użytkownika do pierwszej strony z produktami, posortowane według ceny z najbardziej kosztowne najniższy (patrz rysunek 15).


[![Kliknięcie przycisku porządkuje produkty z najbardziej kosztowne do najmniej](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**Rysunek 15**: kliknięcie przycisku porządkuje produkty z najbardziej kosztowne do najmniejszej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-and-sorting-report-data-vb/_static/image33.png))


## <a name="summary"></a>Podsumowanie

W tym samouczku widzieliśmy sposób implementacji domyślnej, stronicowanie i sortowanie możliwości które były bardzo proste — wystarczy zaznaczenia pola wyboru! Gdy użytkownik sortuje lub między stronami w ramach danych, otwierany jest podobne przepływu pracy:

1. Ensues ogłaszania zwrotnego
2. Dane formantu sieci Web s wstępnie poziomu generowane zdarzenie (`PageIndexChanging` lub `Sorting`)
3. Wszystkie dane zostaną ponownie pobrane przez kontrolki ObjectDataSource
4. Dane formantu sieci Web s używane po tej operacji na poziomie generowane zdarzenie (`PageIndexChanged` lub `Sorted`)

Implementowanie podstawowych stronicowanie i sortowanie jest szybka i bezproblemowa, więcej nakładu pracy musi być stosowany, korzystanie z bardziej efektywne stronicowanie niestandardowych lub w celu dodatkowego zwiększenia interfejsu stronicowania i sortowania. Samouczki przyszłych przedstawimy następujące tematy.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-customized-sorting-user-interface-cs.md)
> [dalej](efficiently-paging-through-large-amounts-of-data-vb.md)
