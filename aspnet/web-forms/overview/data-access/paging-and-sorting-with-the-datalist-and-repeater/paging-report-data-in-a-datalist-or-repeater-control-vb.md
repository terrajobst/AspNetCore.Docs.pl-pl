---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: Stronicowanie danych raportu w kontrolce DataList lub Repeater (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Podczas DataList ani elementu powtarzanego oferty stronicowania automatyczne lub Obsługa sortowania w tym samouczku pokazano, jak dodać obsługę stronicowania do DataList lub Repeater...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: da3c851a8752d4ef5c210a6d8fe552412ecbc9b0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385395"
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Stronicowanie danych raportu w kontrolce DataList lub Repeater (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) lub [Pobierz plik PDF](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Gdy oferta DataList ani elementu powtarzanego automatyczne, stronicowanie i sortowanie pomocy technicznej, w tym samouczku pokazano, jak dodać obsługę stronicowania do DataList lub Repeater, co pozwala znacznie bardziej elastyczne stronicowania i dane wyświetlania interfejsów.


## <a name="introduction"></a>Wprowadzenie

Stronicowanie i sortowanie są dwie funkcje bardzo często, gdy wyświetlanie danych w aplikacji online. Na przykład podczas szukania ASP.NET książki w księgarni online, mogą istnieć setki takich książki, ale raport z listą wyników wyszukiwania zawiera maksymalnie dziesięciu dopasowań na każdej stronie. Ponadto można posortować wyników według tytułu, ceny, liczbę stron, imię i nazwisko autora i tak dalej. Tak jak Omówiliśmy to w [stronicowanie i sortowanie danych raportu](../paging-and-sorting/paging-and-sorting-report-data-vb.md) — samouczek, kontrolki GridView DetailsView i FormView oferować wbudowanej obsługi stronicowania, który może być włączone znaczników pola wyboru. Kontrolki GridView obejmuje również sortowanie pomocy technicznej.

Niestety DataList ani elementu powtarzanego oferują automatyczne stronicowanie i sortowanie pomocy technicznej. W tym samouczku zajmiemy się, jak dodać obsługę stronicowania DataList lub Repeater. Firma Microsoft musi ręcznie utworzyć interfejs stronicowanie, wyświetlanie strony odpowiednich rekordów i Zapamiętaj strony odwiedzana między ogłaszania zwrotnego. Gdy to zająć więcej czasu i kodu niż przy użyciu GridView, DetailsView lub FormView, kontrolek DataList i Repeater umożliwiają znacznie bardziej elastyczne stronicowania i danych wyświetlania interfejsów.

> [!NOTE]
> Ten samouczek koncentruje się wyłącznie na stronicowania. W następnym samouczku firma Microsoft będzie włączyć naszej uwagi do dodawania funkcji sortowania.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Krok 1: Dodawanie stronicowanie i sortowanie Samouczek stron sieci Web

Zanim zaczniemy, w tym samouczku, umożliwiają najpierw Poświęć chwilę, aby dodać strony ASP.NET, które będą potrzebne w tym samouczku i kolejny s. Rozpocznij od utworzenia nowego folderu w projekcie o nazwie `PagingSortingDataListRepeater`. Następnie dodaj następujące pięć strony ASP.NET, w tym folderze każdego z nich jest skonfigurowany do używania strony wzorcowej `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![Utwórz PagingSortingDataListRepeater Folder i dodawanie stron samouczek platformy ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Rysunek 1**: tworzenie `PagingSortingDataListRepeater` Folder i dodawanie stron samouczek platformy ASP.NET


Następnie otwórz `Default.aspx` strony, a następnie przeciągnij `SectionLevelTutorialListing.ascx` kontrolki użytkownika od `UserControls` folder na powierzchnię projektu. Ten formant użytkownika, które utworzyliśmy w [strony wzorcowe i nawigacja w witrynie](../introduction/master-pages-and-site-navigation-vb.md) samouczek, wylicza mapy witryny i wyświetla te samouczki w bieżącej sekcji na liście punktowanej.


[![Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))


Aby ma na liście punktowanej wyświetlić stronicowanie i sortowanie samouczków, z którymi firma utworzona, należy dodać je do mapy witryny. Otwórz `Web.sitemap` pliku i Dodaj następujący kod po edycji i usuwania ze znacznikami węzeł mapy witryny DataList:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]


![Aktualizacja mapy witryny, aby uwzględnić nowe strony ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Rysunek 3**: Aktualizacja mapy witryny, aby uwzględnić nowe strony ASP.NET


## <a name="a-review-of-paging"></a>Przegląd stronicowania

W poprzednich samouczkach widzieliśmy sposobu przeglądania danych w kontrolkach GridView DetailsView i FormView. Te trzy kontrolki oferują prosty formularz stronicowania o nazwie *stronicowania domyślne* który może być implementowany przez po prostu zaznaczenie opcji Włącz stronicowania w tagu inteligentnego sterowania s. Za pomocą stronicowania domyślne każdym żądaniu strony danych, zarówno na pierwszej stronie odwiedź po użytkownik przechodzi do innej strony danych GridView DetailsView, lub FormView ponownie kontrolki *wszystkich* danych z Kontrolki ObjectDataSource. Go następnie wycinków limitu określonego zestawu rekordów do wyświetlenia indeksu żądanej strony i liczbie rekordów do wyświetlenia na jednej stronie. Omówiliśmy stronicowania domyślne szczegółowo w temacie [stronicowanie i sortowanie danych raportu](../paging-and-sorting/paging-and-sorting-report-data-vb.md) samouczka.

Ponieważ domyślne stronicowania zażąda ponownie wszystkie rekordy na każdej stronie, nie jest praktyczne podczas stronicowanie wystarczająco duże ilości danych. Załóżmy na przykład, stronicować 50 000 rekordów o rozmiarze strony 10. Każdorazowo, gdy użytkownik przesuwa się do nowej strony, wszystkich 50 000 rekordów musi zostać pobrany z bazy danych, mimo że są wyświetlane tylko dziesięć z nich.

*Niestandardowe stronicowania* rozwiązuje problemy wydajności domyślnej stronicowania przez przechwytywanie tylko dokładne podzestaw rekordów do wyświetlenia na żądanej strony. Podczas implementowania niestandardowych stronicowania, możemy napisać zapytanie SQL, które efektywnie zwróci tylko odpowiedni zestaw rekordów. Firma Microsoft pokazaliśmy, jak utworzyć zapytanie, za pomocą programu SQL Server 2005 s nowego [ `ROW_NUMBER()` — słowo kluczowe](http://www.4guysfromrolla.com/webtech/010406-1.shtml) w [efektywne stronicowanie za pośrednictwem dużych ilości danych](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) samouczka.

Aby zaimplementować domyślne stronicowania w kontrolkach DataList lub Repeater, możemy użyć [ `PagedDataSource` klasy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) opakowanie `ProductsDataTable` których zawartość jest on stronicowanej. `PagedDataSource` Klasa ma `DataSource` właściwość, którą można przypisać do dowolnego obiektu wyliczalny i [ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) i [ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) właściwości, które wskazują, jak wiele rekordów Pokaż na jednej stronie i indeks bieżącej strony. Po ustawieniu tych właściwości `PagedDataSource` może służyć jako źródło danych dowolnych danych formantu sieci Web. `PagedDataSource`, Gdy wyliczenia, zostanie tylko zwrócenia odpowiedniego podzestawu rekordów jego wewnętrzny `DataSource` na podstawie `PageSize` i `CurrentPageIndex` właściwości. Rysunek 4 przedstawia funkcje `PagedDataSource` klasy.


![PagedDataSource otacza obiekt Wyliczalny z interfejsem stronicowanej](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Rysunek 4**: `PagedDataSource` otacza obiekt Wyliczalny z interfejsem stronicowanej


`PagedDataSource` Obiekt może być tworzone i konfigurowane bezpośrednio z warstwy logiki biznesowej i powiązane z DataList lub Repeater za pomocą kontrolki ObjectDataSource, lub można można tworzyć i konfigurować bezpośrednio w klasie CodeBehind strony ASP.NET. Jeśli drugie podejście jest używany, firma Microsoft zrezygnujesz z używania kontrolki ObjectDataSource, a zamiast tego powiązać stronicowanych danych DataList lub Repeater programowo.

`PagedDataSource` Obiekt również ma właściwości, aby obsługiwać stronicowanie niestandardowych. Możemy jednak pominąć przy użyciu `PagedDataSource` dla niestandardowych stronicowania, ponieważ już metody LOGIKI w `ProductsBLL` klasę zaprojektowaną stronicowanie niestandardowych, które zwracają dokładne rekordy do wyświetlenia.

W tym samouczku przyjrzymy Implementowanie stronicowania domyślne w kontrolkach DataList, dodając nową metodę `ProductsBLL` klasy, która zwraca odpowiednio skonfigurowany `PagedDataSource` obiektu. W następnym samouczku opisano sposób używania stronicowania niestandardowego.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Krok 2: Dodawanie metody stronicowania domyślne w warstwy logiki biznesowej

`ProductsBLL` Klasa ma obecnie metodą zwracającą wszystkie informacje o produkcie `GetProducts()` i jeden dla zwracania określony podzbiór produktów na indeks początkowy `GetProductsPaged(startRowIndex, maximumRows)`. Za pomocą stronicowania domyślne GridView DetailsView i FormView kontroluje wszystkie przypadki użycia `GetProducts()` metodę, aby pobrać wszystkie produkty, ale następnie za pomocą `PagedDataSource` wewnętrznie, aby wyświetlić poprawne podzestaw rekordów. Aby zreplikować tę funkcję za pomocą kontrolek DataList i Repeater, możemy utworzyć nowej metody w LOGIKI, które symuluje to zachowanie.

Dodaj metodę, która `ProductsBLL` klasę o nazwie `GetProductsAsPagedDataSource` przyjmującej w dwóch parametrów wejściowych liczbą całkowitą:

- `pageIndex` Indeks strony, aby wyświetlić, indeksowane od zera, a
- `pageSize` Liczba rekordów do wyświetlenia na jednej stronie.

`GetProductsAsPagedDataSource` rozpoczyna się poprzez pobranie *wszystkich* rekordy z `GetProducts()`. Następnie tworzy `PagedDataSource` obiektu, ustawiając jego `CurrentPageIndex` i `PageSize` właściwości do wartości przekazane do `pageIndex` i `pageSize` parametrów. Metoda stwierdza, zwracając skonfigurowania tej `PagedDataSource`:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Krok 3: Wyświetlanie informacji o produkcie w kontrolką DataList przy użyciu domyślnego stronicowania

Za pomocą `GetProductsAsPagedDataSource` dodane do metody `ProductsBLL` klasy, można teraz utworzyć DataList lub Repeater zapewniająca stronicowania domyślne. Zacznij od otwarcia `Paging.aspx` strony w `PagingSortingDataListRepeater` folder i przeciągnij kontrolką DataList z przybornika w projektancie, ustawienie DataList s `ID` właściwość `ProductsDefaultPaging`. Za pomocą kontrolek DataList s tagu inteligentnego, należy utworzyć nowe kontrolki ObjectDataSource, o nazwie `ProductsDefaultPagingDataSource` i skonfiguruj ją tak, pobiera dane przy użyciu `GetProductsAsPagedDataSource` metody.


[![Tworzenie kontrolki ObjectDataSource i skonfigurować go do używania () GetProductsAsPagedDataSource — metoda](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Rysunek 5**: tworzenie kontrolki ObjectDataSource i skonfigurować go do użycia `GetProductsAsPagedDataSource` `()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


Ustawianie list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak).


[![Ustaw listy rozwijane w ramach aktualizacji, WSTAWIANIA i usuwania karty (Brak)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Rysunek 6**: Ustaw listy rozwijane w ramach aktualizacji, WSTAWIANIA i usuwania karty (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


Ponieważ `GetProductsAsPagedDataSource` metoda oczekuje dwóch parametrów wejściowych, Kreator wyświetli nam źródła te wartości parametrów.

Indeks strony i wartości rozmiaru strony należy pamiętać różnych ogłaszania zwrotnego. One może znajdować się w widoku stanu, utrwalone w zmiennej querystring, przechowywane w zmiennych sesji lub zapamiętanych przy użyciu niektórych innych technik. W tym samouczku użyjemy ciąg zapytania, który ma tę zaletę, dzięki czemu określonej strony danych do zakładek.

W szczególności użyj pageIndex pola querystring i pageSize dla `pageIndex` i `pageSize` parametrów, odpowiednio (zobacz rysunek 7). Poświęć chwilę, aby ustawić wartości domyślne dla tych parametrów, jako wartości querystring wygrał t być obecna, gdy użytkownik najpierw odwiedzi tę stronę. Aby uzyskać `pageIndex`, ustawianie wartości domyślnej 0 (co spowoduje wyświetlenie pierwszej strony danych) i `pageSize` s domyślną wartość 4.


[![Używanie ciąg zapytania jako źródła dla parametrów pageIndex i pageSize](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Rysunek 7**: Użyj ciąg zapytania jako źródło dla `pageIndex` i `pageSize` parametrów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))


Po skonfigurowaniu kontrolki ObjectDataSource, program Visual Studio automatycznie tworzy `ItemTemplate` dla kontrolki DataList. Dostosowywanie `ItemTemplate` aby pokazać tylko nazwy produktu s, kategorii i dostawcy. Również ustawić DataList s `RepeatColumns` właściwość 2, jego `Width` do 100% i jego `ItemStyle` s `Width` do 50%. Te ustawienia szerokość zapewni równe odstępy dla dwóch kolumn.

Po wprowadzeniu tych zmian, znaczników s DataList i kontrolki ObjectDataSource powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Ponieważ firma Microsoft nie wykonuje żadnych aktualizacji lub usuwania funkcji w ramach tego samouczka, możesz wyłączyć stan widoku DataList s, aby zmniejszyć rozmiar renderowanej strony.


Podczas początkowego wizyt u klientów tę stronę za pośrednictwem przeglądarki, ani `pageIndex` ani `pageSize` znajdują się parametry querystring. W związku z tym są używane wartości domyślne, od 0 do 4. Jak pokazano na rysunku 8, powoduje to DataList, który wyświetla produktów pierwsze cztery.


[![Pierwsze cztery produkty są wymienione.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Rysunek 8**: pierwsze cztery produkty są wymienione ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))


Bez interfejsu stronicowania, tam aktualnie nie jest prostym s oznacza, że dla użytkownika przejść do drugiej strony danych. Utworzymy interfejsu stronicowania w kroku 4. Na razie jednak stronicowania można wykonywać tylko bezpośrednio określając kryteria stronicowania w zmiennej querystring. Na przykład, aby wyświetlić drugiej strony, zmienić adres URL w pasku adresu przeglądarki s `Paging.aspx` do `Paging.aspx?pageIndex=2` i naciśnij klawisz Enter. Powoduje to, że drugiej strony danych do wyświetlenia (patrz rysunek 9).


[![Druga strona dane są wyświetlane](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Rysunek 9**: drugiej strony z wyświetleniem danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>Krok 4: Tworzenie interfejsu stronicowania

Istnieje szereg różnych interfejsów stronicowania, które można zaimplementować. Kontrolki GridView DetailsView i FormView zawierają cztery różne interfejsy wyboru spośród oferty:

- **Dalej, poprzedniego** użytkownicy mogą przechodzić po jednej stronie naraz, do następnej lub poprzedniej jeden.
- **Następnie Wstecz, pierwsza, ostatnia** oprócz przycisków dalej i Wstecz, ten interfejs zawiera imię i nazwisko przycisków przechodzenia do bardzo pierwszej lub ostatniej strony.
- **Liczbowe** Wyświetla numery stron w interfejsie stronicowania, pozwalając użytkownikom na szybko przechodzić do określonej strony.
- **Numeryczne, najpierw ostatni** oprócz cyfr stronę liczbowe, zawiera przyciski służące do przechodzenia do bardzo pierwszej lub ostatniej strony.

Dla kontrolek DataList i Repeater odpowiadamy za decydowanie o interfejsu stronicowanie i realizowania. Obejmuje to tworzenie wymaganych formantów sieci Web na tej stronie i wyświetlanie żądanej strony, kliknięcie określonego przycisku interfejsu stronicowania. Ponadto niektóre formanty interfejsu stronicowania może być konieczne można wyłączyć. Na przykład podczas wyświetlania na pierwszej stronie danych za pomocą następnego, Previous, najpierw ostatnio interfejsu, będzie można wyłączyć przyciski pierwszą i Wstecz.

W tym samouczku s pozwalają stosować dalej, Previous, najpierw ostatnio interfejsu. Dodaj cztery kontrolki przycisku w sieci Web do strony i ustaw ich `ID` s `FirstPage`, `PrevPage`, `NextPage`, i `LastPage`. Ustaw `Text` właściwości &lt; &lt; najpierw &lt; Wstecz, obok &gt;i ostatnie &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

Następnie należy utworzyć `Click` program obsługi zdarzeń dla każdego z tych przycisków. Później dodasz kod wymagany do wyświetlić żądanej strony.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Całkowita liczba rekordów, które są stronicowane za pośrednictwem zapamiętywanie

Niezależnie od interfejsu stronicowania musimy obliczeń i zapamiętać całkowita liczba rekordów, które są stronicowane za pośrednictwem. Całkowita liczba wierszy (w połączeniu z rozmiarem strony) określa, ile łączna liczba stron w danych są są stronicowane, który decyduje o tym co stronicowania kontrolek interfejsu są dodawane lub są włączone. W następnej, Previous, pierwszy interfejs ostatnich, który ucieszyliśmy się liczba wyświetleń stron jest używany na dwa sposoby:

- Aby ustalić, czy możemy przeglądasz ostatniej strony, w którym to przypadku są wyłączone, przyciski następnej i ostatnie.
- Jeśli użytkownik kliknie przycisk Ostatnia musimy whisk je do ostatniego liczba strony, której indeks jest mniejsza niż strony.

Liczba wyświetleń stron jest obliczany limitu łącznej liczby wierszy podzielonej przez rozmiar strony. Na przykład, jeśli firma Microsoft stronicowanie 79 rekordy z czterech rekordy na każdej stronie, następnie liczbę stron wynosi 20 (Zaokrąglenie w górę 79 / 4). Jeśli używamy liczbowych interfejsu stronicowania, te informacje informuje NAS co ile przyciski stronę liczbowe, aby wyświetlić; Jeśli Nasz interfejs stronicowania zawiera przyciski Dalej lub ostatni, liczba wyświetleń stron jest używany do określenia, kiedy można wyłączyć przyciski Dalej lub ostatni.

Jeśli interfejs stronicowania zawiera ostatni przycisk, konieczne jest, całkowita liczba rekordów, które są stronicowane za pośrednictwem zapamiętane między ogłaszania zwrotnego tak, aby po kliknięciu przycisku ostatniego określimy, której ostatni indeks strony. Aby ułatwić to zadanie, należy utworzyć `TotalRowCount` właściwości w klasie CodeBehind strony s ASP.NET, która będzie się powtarzać, jego wartość, aby wyświetlić stan:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

Oprócz `TotalRowCount`potrwać chwilę, aby utworzyć prosty sposób uzyskiwania dostępu do strony indeksu, rozmiar strony tylko do odczytu właściwości na poziomie strony oraz liczba stron:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Całkowita liczba rekordów, które są stronicowane za pomocą określania

`PagedDataSource` Obiekt zwrócony od ObjectDataSource s `Select()` metoda ma w niej *wszystkich* rekordów produktu, nawet jeśli tylko część z nich są wyświetlane w elemencie DataList. `PagedDataSource` s [ `Count` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) zwraca liczbę elementów, które będą wyświetlane w elemencie DataList; [ `DataSourceCount` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) zwraca łączna liczba elementów w obrębie `PagedDataSource`. W związku z tym, należy przypisać ASP.NET strony s `TotalRowCount` właściwości wartość elementu `PagedDataSource` s `DataSourceCount` właściwości.

W tym celu należy utworzyć program obsługi zdarzeń dla ObjectDataSource s `Selected` zdarzeń. W `Selected` programu obsługi zdarzeń, które mamy dostęp do wartość zwracaną ObjectDataSource s `Select()` metody w tym przypadku `PagedDataSource`.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>Wyświetlanie żądanej strony danych

Gdy użytkownik kliknie jeden z przycisków w interfejsie stronicowania, należy wyświetlić żądanej strony danych. Ponieważ określono parametry stronicowanie za pośrednictwem ciąg zapytania, aby wyświetlić żądanej strony użycie danych `Response.Redirect(url)` mieć użytkownik s przeglądarki ponownie zażądać `Paging.aspx` strony z odpowiednimi parametrami stronicowania. Na przykład, aby wyświetlić drugiej strony danych, firma Microsoft przekierowania użytkownika `Paging.aspx?pageIndex=1`.

Aby ułatwić to zadanie, należy utworzyć `RedirectUser(sendUserToPageIndex)` metodę, która przekierowuje użytkownika do `Paging.aspx?pageIndex=sendUserToPageIndex`. Następnie należy wywołać tę metodę, przy użyciu czterech przycisku `Click` procedury obsługi zdarzeń. W `FirstPage` `Click` programu obsługi zdarzeń, wywołanie `RedirectUser(0)`, aby wysłać je do pierwszej strony; w `PrevPage` `Click` programu obsługi zdarzeń, użyj `PageIndex - 1` jako indeks strony; i tak dalej.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Za pomocą `Click` zakończenia procedury obsługi zdarzeń, rekordy DataList s może być stronicowana za pośrednictwem za pomocą przycisków. Poświęć chwilę, aby wypróbować tę funkcję!

## <a name="disabling-paging-interface-controls"></a>Wyłączanie stronicowania kontrolek interfejsu

Obecnie wszystkie cztery przyciski są włączone, bez względu na wyświetlanej stronie. Jednak firma Microsoft chcesz wyłączyć przyciski pierwszy i Wstecz w przypadku wyświetlania pierwszej strony danych i przyciski następnej i ostatnie, gdy są wyświetlane na ostatniej stronie. `PagedDataSource` Obiektu zwróconego przez ObjectDataSource s `Select()` metoda ma właściwości [ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) i [ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) , firma Microsoft można sprawdzić, aby ustalić, firma Microsoft wyświetlania pierwszej lub ostatniej strony danych.

Dodaj następujący kod do ObjectDataSource s `Selected` program obsługi zdarzeń:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Dodając ten przyciski pierwszy i poprzedni zostanie wyłączona podczas przeglądania pierwszej strony, podczas gdy przyciski następnej i ostatnio zostanie wyłączona podczas przeglądania na ostatniej stronie.

Umożliwiają s wykonaj interfejsu stronicowania, informujący użytkownika, co stronie one ponownie obecnie oglądany, i istnieje ile łączna liczba stron. Dodaj kontrolkę etykieta w sieci Web do strony i ustaw jego `ID` właściwość `CurrentPageNumber`. Ustaw jego `Text` właściwości kontrolki ObjectDataSource s wybrany program obsługi zdarzeń takich zawiera bieżącą stronę przeglądany (`PageIndex + 1`) oraz łączną liczbę stron (`PageCount`).


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

Na rysunku nr 10 przedstawiono `Paging.aspx` po raz pierwszy odwiedzony. Ponieważ ciąg zapytania jest pusta, kontrolki DataList domyślnie przedstawia produktów pierwsze cztery; Pierwszy i poprzedni przyciski są wyłączone. Jeśli klikniesz pozycję dalej Wyświetla następnych czterech rekordy (zobacz rysunek 11); Pierwszy i poprzedni przyciski są teraz włączone.


[![Strona pierwszego dane są wyświetlane](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Na rysunku nr 10**: pierwszej strony z wyświetleniem danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))


[![Druga strona dane są wyświetlane](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Rysunek 11**: drugiej strony z wyświetleniem danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))


> [!NOTE]
> Interfejs stronicowania może zostać poprawione dalsze, umożliwiając użytkownikowi na określenie, ile strony, aby wyświetlić na stronie. Na przykład kontrolki DropDownList można dodać opcje rozmiaru strony listy, takich jak 5, 10, 25, 50 i wszystkie. Po wybraniu rozmiar strony, użytkownik musi zostać przekierowany z powrotem do `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Opuścić implementacja to rozszerzenie w charakterze ćwiczenia dla czytnika.


## <a name="using-custom-paging"></a>Przy użyciu niestandardowych stronicowania

Strony DataList przy użyciu jego danych korzystające z techniki stronicowania domyślne nieefektywne. Gdy stronicowanie wystarczająco duże ilości danych, konieczne jest stosowanie niestandardowy stronicowania. Chociaż szczegóły implementacji różnią się nieznacznie, pojęć dotyczących implementowania stronicowania niestandardowe w kontrolkach DataList są taka sama jak za pomocą domyślnego stronicowania. Za pomocą niestandardowych stronicowanie, należy użyć `ProductBLL` klasy s `GetProductsPaged` — metoda (zamiast `GetProductsAsPagedDataSource`). Zgodnie z opisem w [efektywne stronicowanie za pośrednictwem dużych ilości danych](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) samouczku `GetProductsPaged` muszą być przekazywane start wiersz indeksu i maksymalną liczbę wierszy do zwrócenia. Te parametry mogą być obsługiwane za pośrednictwem podobnie jak w ciągu kwerendy `pageIndex` i `pageSize` parametrami używanymi w domyślnym stronicowania.

Ponieważ tam s nie `PagedDataSource` przy użyciu niestandardowych stronicowania alternatywnych metod musi służyć do określenia całkowita liczba rekordów, które są stronicowane, za pomocą oraz tego, czy możemy ponownie wyświetlanie pierwszej lub ostatniej strony danych. `TotalNumberOfProducts()` Method in Class metoda `ProductsBLL` Klasa zwraca sumę produkty są stronicowane za pośrednictwem. Aby określić, jeśli pierwsza strona data jest wyświetlana, Sprawdź indeks wiersza początkowego, gdy jest równa 0, pierwsza strona jest wyświetlana, a następnie. Ostatnia strona jest wyświetlana, jeśli indeks wiersza początkowego oraz maksymalna liczba wierszy do zwrócenia jest większa lub równa całkowita liczba rekordów, które są stronicowane za pośrednictwem.

Implementowanie stronicowania niestandardowego bardziej szczegółowo w następnym samouczku przyjrzymy się.

## <a name="summary"></a>Podsumowanie

Natomiast DataList ani elementu powtarzanego oferuje poza obsługę stronicowania pola znaleziony w DetailsView, w widoku GridView i FormView kontroluje, takie funkcje można dodać przy minimalnym nakładzie pracy. Najłatwiejszym sposobem realizowania stronicowania domyślny jest opakowanie cały zestaw produktów w ramach `PagedDataSource` , a następnie wiążą `PagedDataSource` DataList lub Repeater. W tym samouczku dodaliśmy `GetProductsAsPagedDataSource` metody `ProductsBLL` klasy w celu zwracania `PagedDataSource`. `ProductsBLL` Klasy już zawiera metody służące do niestandardowych stronicowania `GetProductsPaged` i `TotalNumberOfProducts`.

Wraz z pobierania dokładny zestaw rekordów do wyświetlenia w przypadku stronicowania niestandardowego albo wszystkie rekordy w `PagedDataSource` domyślne stronicowanie, należy również ręcznie dodać interfejs stronicowania. W tym samouczku utworzyliśmy dalej, Previous, najpierw zostaną ostatnio interfejs cztery kontrolki przycisku w sieci Web. Ponadto dodano kontrolkę typu etykieta wyświetlanie numer bieżącej strony i łączna liczba stron.

W następnym samouczku Zobaczymy się, jak dodać obsługę sortowania do kontrolek DataList i Repeater. Zobaczymy również sposób tworzenia DataList, które mogą być stronicowana i sortowane (wraz z przykładami, przy użyciu domyślnych i niestandardowych stronicowania).

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Liz Shulok, Krzysztof Pespisa i Bernadette Leigh. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](sorting-data-in-a-datalist-or-repeater-control-cs.md)
> [dalej](sorting-data-in-a-datalist-or-repeater-control-vb.md)
