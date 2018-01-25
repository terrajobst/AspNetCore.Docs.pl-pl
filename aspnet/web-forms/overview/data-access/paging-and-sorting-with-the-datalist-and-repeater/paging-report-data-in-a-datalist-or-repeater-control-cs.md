---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
title: Stronicowanie danych raportu w kontrolce elementu powtarzanego (C#) lub DataList | Dokumentacja firmy Microsoft
author: rick-anderson
description: "Podczas automatycznego stronicowania oferta DataList ani elementu powtarzanego lub sortowania pomocy technicznej w tym samouczku pokazano, jak dodać obsługę stronicowania DataList lub elementu powtarzanego..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: e8e0809b-25c4-4c3b-8d12-9a17048148ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 4952adff752ec834b8be5f190181be98a034ccfd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-c"></a>Dane raportu stronicowania DataList lub w kontrolce elementu powtarzanego (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_CS.exe) lub [pobierania plików PDF](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial44cs1.pdf)

> Gdy oferta DataList ani elementu powtarzanego automatyczne stronicowania i sortowania pomocy technicznej, w tym samouczku pokazano, jak dodać obsługę stronicowania DataList lub elementu powtarzanego, co umożliwia bardziej elastyczne stronicowania i danych interfejsy wyświetlania.


## <a name="introduction"></a>Wprowadzenie

Stronicowania i sortowania są dwie funkcje często podczas wyświetlania danych w aplikacji online. Na przykład przy wyszukiwaniu ASP.NET książki w księgarni, może istnieć setki takich książek, ale raport wyświetlania wyników wyszukiwania zawiera maksymalnie dziesięciu dopasowań na każdej stronie. Ponadto wyniki można sortować według tytułu, cen, liczba stron, imię i nazwisko autora i tak dalej. Jak wspomniano w [stronicowania i sortowania danych raportów](../paging-and-sorting/paging-and-sorting-report-data-cs.md) — samouczek, kontrolki GridView widoku DetailsView i FormView oferować wbudowaną obsługę stronicowania, który można włączyć w znaczników jest pole wyboru. Widoku GridView także sortowanie pomocy technicznej.

Niestety DataList ani elementu powtarzanego oferują automatyczne stronicowania i sortowania pomocy technicznej. W tym samouczku zajmiemy się, jak dodać obsługę stronicowania DataList lub elementu powtarzanego. Firma Microsoft musi ręcznie utworzyć interfejs stronicowania, Wyświetl stronę odpowiednich rekordów i pamiętaj odwiedzana między ogłaszania zwrotnego strony. Gdy to zająć więcej czasu i kodu niż przy użyciu widoku GridView, widoku DetailsView lub FormView, DataList i elementu powtarzanego umożliwiają bardziej elastyczne stronicowania i danych interfejsy wyświetlania.

> [!NOTE]
> Ten samouczek koncentruje się wyłącznie na stronicowania. W następnym samouczku firma Microsoft będzie Włącz wymagające uwagi do dodawania funkcji sortowania.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Krok 1: Dodawanie stronicowania i sortowania Samouczek stron sieci Web

Przed Rozpoczniemy w tym samouczku umożliwiają najpierw Poświęć chwilę, aby dodać stron ASP.NET, które będą potrzebne dla tego samouczka i kolejnego s. Rozpocznij od utworzenia nowego folderu do projektu o nazwie `PagingSortingDataListRepeater`. Następnie dodaj następujące pięć stron ASP.NET w tym folderze każdego z nich jest skonfigurowany do używania strony wzorcowej `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![Utwórz PagingSortingDataListRepeater Folder i dodać strony samouczek platformy ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Rysunek 1**: tworzenie `PagingSortingDataListRepeater` folderu i dodać strony samouczek platformy ASP.NET


Następnie otwórz folder `Default.aspx` strony i przeciągnij `SectionLevelTutorialListing.ascx` kontrolki użytkownika z `UserControls` folderu na powierzchnię projektu. Ten formant użytkownika, który utworzono w [stron wzorcowych i nawigacji w witrynie](../introduction/master-pages-and-site-navigation-cs.md) samouczek, wylicza mapy witryny i wyświetla te samouczki w bieżącej sekcji listy punktowanej.


[![Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` formantu użytkownika `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image4.png))


W celu wyświetlenia stronicowania i sortowania samouczków, z którymi możemy utworzyć listy punktowanej, należy dodać je do mapy witryny. Otwórz `Web.sitemap` i Dodaj następujący kod po edytowanie i usuwanie z znaczników węzeł mapy witryny DataList:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample1.xml)]


![Zaktualizuj mapy witryny w celu uwzględnienia nowych stron ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)

**Rysunek 3**: zaktualizuj mapy witryny w celu uwzględnienia nowych stron ASP.NET


## <a name="a-review-of-paging"></a>Przegląd stronicowania

W samouczkach poprzedniej widzieliśmy sposobu przeglądania danych w kontrolkach GridView widoku DetailsView i FormView. Te trzy formanty zapewniają prosty formularz stronicowania o nazwie *stronicowania domyślne* który może być zaimplementowany przez po prostu Sprawdzanie opcji Włącz stronicowanie w tagu formantu s. Z domyślnego stronicowania, zawsze zostanie zażądana strona danych zarówno na pierwszej stronie odwiedź stronę lub gdy użytkownik przechodzi do innej strony danych widoku GridView, widoku DetailsView, lub FormView ponownie kontrolki *wszystkie* danych z Element ObjectDataSource. Go następnie wycinków limitu określonego zestawu rekordów do wyświetlenia żądanej strony indeksu i liczbie rekordów wyświetlanych na każdej stronie. Omówiono stronicowania domyślne szczegółowo w [stronicowania i sortowania danych raportów](../paging-and-sorting/paging-and-sorting-report-data-cs.md) samouczka.

Ponieważ domyślne stronicowania zażąda ponownie wszystkie rekordy na każdej stronie, nie jest praktyczne podczas stronicowania za pośrednictwem wystarczająco duże ilości danych. Załóżmy na przykład stronicowania do 50 000 rekordów o rozmiarze 10 strony. Zawsze, gdy użytkownik przechodzi do nowej strony, wszystkich 50 000 rekordów musi zostać pobrana z bazy danych, nawet jeśli są wyświetlane tylko dziesięć z nich.

*Stronicowania niestandardowego* rozwiązuje problemy wydajności domyślnej stronicowania przez Przechwytywanie dokładne podzbiór rekordów do wyświetlenia w żądanej strony. Podczas implementowania stronicowania niestandardowego, możemy napisać zapytanie SQL, które skutecznie zwróci tylko poprawny zestaw rekordów. Widzieliśmy jak utworzyć takie zapytanie, używając nowego programu SQL Server 2005 s [ `ROW_NUMBER()` — słowo kluczowe](http://www.4guysfromrolla.com/webtech/010406-1.shtml) w [wydajnie stronicowania za pośrednictwem dużych ilości danych](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) samouczka.

Aby zaimplementować domyślnego stronicowania w formantach DataList lub elementu powtarzanego, możemy użyć [ `PagedDataSource` klasy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) jako otokę `ProductsDataTable` są trwa stronicowanej których zawartość. `PagedDataSource` Klasa ma `DataSource` właściwość, którą można przypisać do dowolnego obiektu wyliczalny i [ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) i [ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) właściwości, które wskazują, jak wiele rekordów do Pokaż na stronie i indeks bieżącej strony. Po ustawieniu właściwości `PagedDataSource` mogą być używane jako źródło danych danych formantu sieci Web. `PagedDataSource`, Gdy wyliczone, będą zwracane tylko odpowiednie podzbiór rekordów jego wewnętrzny `DataSource` na podstawie `PageSize` i `CurrentPageIndex` właściwości. Rysunek 4 przedstawia funkcjonalność `PagedDataSource` klasy.


![Obiekt Wyliczalny z interfejsem stronicowalnej Opakowuje PagedDataSource](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image6.png)

**Rysunek 4**: `PagedDataSource` Opakowuje obiekt Wyliczalny z interfejsem stronicowalnej


`PagedDataSource` Obiekt może być utworzony i skonfigurowany bezpośrednio z warstwy logiki biznesowej i powiązany z DataList lub elementu powtarzanego przez element ObjectDataSource, lub można tworzyć i skonfigurować bezpośrednio w klasie związanej z kodem strony ASP.NET. Jeśli drugie podejście jest używany, możemy zrezygnujesz z używania ObjectDataSource, a zamiast tego należy powiązać stronicowanych danych DataList lub elementu powtarzanego programowo.

`PagedDataSource` Obiekt ma również właściwości do obsługi stronicowania niestandardowego. Firma Microsoft jednak pominąć przy użyciu `PagedDataSource` dla niestandardowych stronicowania, ponieważ istnieje już metody logiki warstwy Biznesowej `ProductsBLL` klasy przeznaczone do stronicowania niestandardowego, który zwraca dokładne rekordów do wyświetlenia.

W ramach tego samouczka przyjrzymy Implementowanie stronicowania domyślne w DataList przez dodanie nowej metody do `ProductsBLL` klasy, która zwraca odpowiednio skonfigurowany `PagedDataSource` obiektu. W następnym samouczku firma Microsoft będzie widoczny sposobu użycia stronicowania niestandardowego.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Krok 2: Dodawanie metody stronicowania domyślne warstwy logiki biznesowej

`ProductsBLL` Klasa ma obecnie metody dla zwracania wszystkie informacje o produkcie `GetProducts()` i jeden dla zwracania określony podzbiór produktów na indeks początkowy `GetProductsPaged(startRowIndex, maximumRows)`. Z stronicowania domyślnego widoku GridView widoku DetailsView i FormView steruje użyciem wszystkich `GetProducts()` metodę, aby pobrać wszystkie produkty, ale następnie użyć `PagedDataSource` wewnętrznie, aby wyświetlić poprawne podzestaw rekordów. Aby zreplikować tej funkcji z formantami DataList i powtarzanego, możemy utworzyć nowej metody w logiki warstwy Biznesowej, które symuluje to zachowanie.

Dodaj metodę `ProductsBLL` klasy o nazwie `GetProductsAsPagedDataSource` który przyjmuje dwa parametry wejściowe całkowitą:

- `pageIndex`Indeks strony do wyświetlenia, indeksowane na zero, a
- `pageSize`Liczba rekordów wyświetlanych na każdej stronie.

`GetProductsAsPagedDataSource`rozpoczyna się przez pobranie *wszystkie* rekordy z `GetProducts()`. Następnie tworzy `PagedDataSource` obiektu ustawienie jej `CurrentPageIndex` i `PageSize` właściwości do wartości przekazane do `pageIndex` i `pageSize` parametrów. Metoda stwierdza, zwracając to skonfigurowany `PagedDataSource`:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Krok 3: Wyświetlanie informacji o produkcie w DataList, przy użyciu domyślnego stronicowania

Z `GetProductsAsPagedDataSource` dodane do metody `ProductsBLL` klasy, można teraz utworzyć DataList lub elementu powtarzanego udostępniający domyślne stronicowania. Uruchamianie przez otwarcie `Paging.aspx` strony `PagingSortingDataListRepeater` folder i przeciągnij DataList z przybornika do projektanta, ustawienie DataList s `ID` właściwości `ProductsDefaultPaging`. Z tagów inteligentnych DataList s, Utwórz nowy element ObjectDataSource o nazwie `ProductsDefaultPagingDataSource` i skonfiguruj ją tak, aby pobierania danych przy użyciu `GetProductsAsPagedDataSource` metody.


[![Utwórz element ObjectDataSource i skonfigurować go do używania () GetProductsAsPagedDataSource — metoda](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Rysunek 5**: utworzyć ObjectDataSource i skonfigurować go do użytku `GetProductsAsPagedDataSource` `()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


Ustawianie list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak).


[![Ustaw listy rozwijane w UPDATE, INSERT i usuwanie kart na (Brak)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Rysunek 6**: Ustaw listy rozwijane w UPDATE, INSERT i usuwanie kart na (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


Ponieważ `GetProductsAsPagedDataSource` metoda oczekuje dwóch parametrów wejściowych, Kreator wyświetli nam źródła wartości tych parametrów.

Indeks strony i wartości rozmiar strony musi zapamiętany między ogłaszania zwrotnego. One mogą być przechowywane w widoku stanu utrwalony na ciąg zapytania, przechowywane w zmiennych sesji albo zapamiętanych przy użyciu niektórych innych technik. W tym samouczku użyjemy ciąg zapytania, który ma możliwość określonej strony danych można tworzyć zakładek.

W szczególności, użyj querystring pola pageIndex i pageSize dla `pageIndex` i `pageSize` parametry, odpowiednio (patrz rysunek 7). Poświęć chwilę, aby ustawić wartości domyślne dla tych parametrów, jak wartości querystring won t można wtedy, gdy użytkownik odwiedza najpierw tej strony. Dla `pageIndex`, ustaw wartość domyślna 0 (co spowoduje wyświetlenie pierwszej strony danych) i `pageSize` s wartość domyślną do 4.


[![Ciąg zapytania jako źródła użyć parametrów pageIndex i pageSize](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Rysunek 7**: Użyj ciąg zapytania jako źródło dla `pageIndex` i `pageSize` parametrów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image15.png))


Po skonfigurowaniu ObjectDataSource, automatycznie tworzy Visual Studio `ItemTemplate` dla elementu DataList. Dostosowywanie `ItemTemplate` tak, aby tylko nazwa produktu s, kategorii i dostawcy. Również ustawić DataList s `RepeatColumns` właściwości 2, jego `Width` 100% i jego `ItemStyle` s `Width` do 50%. Te ustawienia szerokość zapewni równe odstępy dla dwóch kolumn.

Po wprowadzeniu tych zmian, DataList i ObjectDataSource znaczników s powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

> [!NOTE]
> Ponieważ firma Microsoft nie wykonuje żadnej aktualizacji lub usuwania funkcji w tym samouczku, możesz wyłączyć stan widoku DataList s, aby zmniejszyć rozmiar renderowanej strony.


Przy początkowo tę stronę za pośrednictwem przeglądarki, ani `pageIndex` ani `pageSize` znajdują się parametry querystring. W związku z tym są używane wartości domyślne od 0 do 4. Jak pokazano na rysunku 8, powoduje to DataList, który przedstawia pierwsze cztery produkty.


[![Pierwsze cztery produkty wymienione](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image16.png)

**Rysunek 8**: pierwszy produkty cztery wymienione ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image18.png))


Bez interfejsu stronicowania, tam s aktualnie nie proste oznacza, że dla użytkownika przejść do drugiej strony danych. Utworzymy interfejsu stronicowania w kroku 4. Na razie jednak stronicowania można wykonywać tylko bezpośrednio określając kryteria stronicowania w zmiennej querystring. Na przykład, aby wyświetlić na drugiej stronie, Zmień adres URL na pasku adresu przeglądarki s z `Paging.aspx` do `Paging.aspx?pageIndex=2` i naciśnij Enter. Powoduje to, że dane mają być wyświetlane na drugiej stronie (patrz rysunek 9).


[![Druga strona dane są wyświetlane](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image19.png)

**Rysunek 9**: drugiej stronie z dane są wyświetlone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>Krok 4: Tworzenie interfejsu stronicowania

Istnieje wiele różnych interfejsów stronicowania, które można zastosować. Formanty widoku GridView widoku DetailsView i FormView udostępniają czterech różnych interfejsach wyboru spośród:

- **Następnego, poprzedniego** użytkownicy mogą przechodzić jedną stronę w czasie, do następnej lub poprzedniej skrótu.
- **Następnie Wstecz, First, Last** oprócz przycisków Następny i poprzedni, ten interfejs zawiera pierwszy i ostatni przyciski przenoszenia do pierwszego lub bardzo ostatniej strony.
- **Liczbowa** Wyświetla listę numerów strony w interfejsie stronicowania, umożliwiając użytkownikowi szybko przeskoczyć do określonej strony.
- **Liczbowe, najpierw ostatni** oprócz cyfr liczbowych strony zawiera przyciski przenoszenia do pierwszego lub bardzo ostatniej strony.

DataList i powtarzanego firma Microsoft są zobowiązani do podejmowania decyzji interfejsu stronicowania i realizowania. Obejmuje to tworzenie wymaganych formantów sieci Web na stronie i wyświetlanie żądanej strony, po kliknięciu określonego przycisku interfejsu stronicowania. Ponadto niektóre formanty interfejsu stronicowania może być konieczne wyłączone. Na przykład podczas przeglądania na pierwszej stronie danych przy użyciu następnego, poprzedni, najpierw ostatni interfejsu, przycisków Poprzednia i pierwszy zostanie wyłączone.

W tym samouczku s pozwalają stosować dalej, poprzedni, najpierw ostatnio interfejs. Na stronie Dodaj cztery kontrolki przycisku w sieci Web i ustawić ich `ID` s, aby `FirstPage`, `PrevPage`, `NextPage`, i `LastPage`. Ustaw `Text` właściwości &lt; &lt; najpierw &lt; poprzedni, obok &gt;i &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample4.aspx)]

Następnie należy utworzyć `Click` programu obsługi zdarzeń dla każdego z tych przycisków. Za chwilę zostanie dodany kod konieczne wyświetlenie żądanej strony.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Całkowita liczba rekordów jest stronicowana za pośrednictwem zapamiętywanie

Niezależnie od wybrany interfejs stronicowania potrzebujemy na potrzeby obliczania i pamiętaj, całkowita liczba rekordów jest stronicowana za pośrednictwem. Całkowita liczba wierszy (w połączeniu z rozmiarem strony) określa, ile całkowita liczba stron w danych są trwa stronicowanej za pośrednictwem, która określa co kontrolek interfejsu stronicowania są dodawane ani są włączone. W dalej, poprzednio, pierwszy ostatni interfejsu możemy budowania liczba stron służy na dwa sposoby:

- Aby ustalić, czy możemy wyświetlanych ostatniej strony, w którym to przypadku przycisków Następny i ostatni są wyłączone.
- Kliknięcie przycisku ostatniej musimy whisk je do ostatniej strony, którego indeks jest mniejszy od strony count.

Liczba stron jest obliczany jako limitu całkowitej liczby wierszy podzielonej przez rozmiar strony. Na przykład, jeśli firma Microsoft są stronicowanie za pośrednictwem 79 rekordy z czterech rekordów na stronie, następnie liczba stron wynosi 20 (limitu 79 / 4). Jeśli użyto liczbowych interfejsu stronicowania, te informacje poinformuje, co ile przyciski numeryczne strony, aby wyświetlić; Jeśli interfejs naszych stronicowania zawiera przyciski następnego lub ostatni, liczba stron służy do określenia, kiedy można wyłączyć przyciski następnego lub ostatni.

Jeśli interfejs stronicowania zawiera przycisku ostatniej, konieczne jest że całkowita liczba rekordów jest stronicowana za pośrednictwem zapamiętany między ogłaszania zwrotnego tak, aby po kliknięciu przycisku ostatniej możemy określić ostatni indeks strony. Ułatwia to tworzenie `TotalRowCount` właściwości klasy związane z kodem strony s ASP.NET, który będzie się powtarzać, jego wartość, aby wyświetlić stan:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Oprócz `TotalRowCount`, po upływie kilku minut utworzyć poziomu strony właściwości tylko do odczytu do łatwego uzyskiwania dostępu do strony indeksu, rozmiar strony, a liczba stron:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample6.cs)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Całkowita liczba rekordów jest stronicowana za pomocą określania

`PagedDataSource` Zwróciła obiekt ObjectDataSource s `Select()` metoda ma w niej *wszystkie* rekordów produktu, mimo że tylko ich podzbiór są wyświetlane w elementu DataList. `PagedDataSource` s [ `Count` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) zwraca liczbę elementów, które będą wyświetlane w DataList; [ `DataSourceCount` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) zwraca całkowitą liczbę elementów w `PagedDataSource`. W związku z tym należy przypisać ASP.NET strony s `TotalRowCount` wartość właściwości z `PagedDataSource` s `DataSourceCount` właściwości.

W tym celu należy utworzyć programu obsługi zdarzeń dla elementu ObjectDataSource s `Selected` zdarzeń. W `Selected` obsługi zdarzeń mamy dostęp do wartość zwracaną przez element ObjectDataSource s `Select()` metody w tym przypadku `PagedDataSource`.


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

## <a name="displaying-the-requested-page-of-data"></a>Wyświetlanie danych żądanej strony

Gdy użytkownik kliknie jeden z przycisków w interfejsie stronicowania, należy wyświetlić stronę żądanych danych. Ponieważ parametry stronicowania są określone za pomocą ciągu kwerendy, można wyświetlić żądanej strony danych użycia `Response.Redirect(url)` mieć użytkownik s przeglądarki ponownie zażądać `Paging.aspx` strony z odpowiednimi parametrami stronicowania. Na przykład, aby wyświetlić na drugiej stronie danych, firma Microsoft przekieruje użytkownika do `Paging.aspx?pageIndex=1`.

Ułatwia to tworzenie `RedirectUser(sendUserToPageIndex)` metodę, która przekierowuje użytkownika do `Paging.aspx?pageIndex=sendUserToPageIndex`. Następnie należy wywołać tej metody z czterech przycisk `Click` procedury obsługi zdarzeń. W `FirstPage` `Click` obsługi zdarzeń, wywołaj `RedirectUser(0)`, aby wysłać je do pierwszej strony; w `PrevPage` `Click` program obsługi zdarzeń, użyj `PageIndex - 1` jako indeks strony; i tak dalej.


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample8.cs)]

Z `Click` zakończenie obsługi zdarzenia, rekordy DataList s może być stronicowana za pośrednictwem za pomocą przycisków. Poświęć chwilę, aby wypróbować jej możliwości!

## <a name="disabling-paging-interface-controls"></a>Wyłączenie stronicowania formantów interfejsu

Obecnie wszystkie cztery przyciski są włączone, bez względu na wyświetlanej stronie. Jednak chcemy wyłączyć przyciski pierwszy i poprzedni przy wyświetlaniu pierwszej strony danych i przycisków Następny i ostatni przy wyświetlaniu ostatniej strony. `PagedDataSource` Obiektu zwróconego przez element ObjectDataSource s `Select()` metoda ma właściwości [ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) i [ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) który omówione można określić możemy wyświetlania pierwszej lub ostatniej strony danych.

Dodaj następującą wartość do ObjectDataSource s `Selected` obsługi zdarzeń:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Z tym dodatkiem przycisków pierwszy i poprzedni zostanie wyłączona podczas przeglądania pierwszej strony, gdy przycisków Następny i ostatni zostanie wyłączona podczas przeglądania ostatniej strony.

Let s ukończyć interfejsu stronicowania informuje użytkownika co strony one dotyczy aktualnie wyświetlana i istnieje liczbę całkowita liczba stron. Na stronie Dodaj formant sieci Web etykiety i ustawić jej `ID` właściwości `CurrentPageNumber`. Ustaw jego `Text` w elemencie ObjectDataSource s wybrane programu obsługi zdarzeń takich właściwości, że zawiera on bieżącej strony wyświetlany (`PageIndex + 1`) i łączną liczbę stron (`PageCount`).


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample10.cs)]

Rysunek nr 10 przedstawia `Paging.aspx` po odwiedzeniu najpierw. Ponieważ ciąg zapytania jest pusta, domyślnie DataList przedstawiający pierwsze cztery produktów; przyciski pierwszy i poprzedni są wyłączone. Kliknięcie przycisku Dalej wyświetla obok cztery rekordy (patrz rysunek 11); przyciski pierwszy i poprzedni są teraz włączone.


[![Zostanie wyświetlony pierwszy strony danych](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image22.png)

**Na rysunku nr 10**: pierwszej strony z dane są wyświetlone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image24.png))


[![Druga strona dane są wyświetlane](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image25.png)

**Rysunek 11**: drugiej stronie z dane są wyświetlone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image27.png))


> [!NOTE]
> Interfejs stronicowania może zostać poprawione dalsze, umożliwiając użytkownikowi na określenie, ile strony, aby wyświetlić na stronie. Na przykład lista DropDownList można dodać listę opcji rozmiaru strony jak 5, 10, 25, 50 i wszystkie. Po wybraniu rozmiar strony, użytkownik musi zostać przekierowany z powrotem do `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Pozostaw I Implementowanie to rozszerzenie jako wykonywania dla czytnika.


## <a name="using-custom-paging"></a>Przy użyciu stronicowania niestandardowego

Na stronach DataList za pośrednictwem jego danych przy użyciu techniki stronicowania nieefektywne domyślne. Gdy stronicowanie za pośrednictwem wystarczająco duże ilości danych, konieczne jest stosowanie stronicowania niestandardowego. Mimo że szczegóły implementacji różnić się nieznacznie, pojęć dotyczących Implementowanie stronicowania niestandardowego w DataList są takie same jak z domyślnego stronicowania. Stronicowania niestandardowego za pomocą `ProductBLL` klasy s `GetProductsPaged` — metoda (zamiast `GetProductsAsPagedDataSource`). Zgodnie z opisem w [wydajnie stronicowania za pośrednictwem dużych ilości danych](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) samouczka `GetProductsPaged` muszą być przekazywane start wiersza indeksu i maksymalną liczbę wierszy do zwrócenia. Te parametry mogą być obsługiwane za pośrednictwem like tylko querystring `pageIndex` i `pageSize` parametrami używanymi w domyślnym stronicowania.

Ponieważ brak s nie `PagedDataSource` z stronicowania niestandardowego alternatywnych technik musi posłużyć do określenia całkowita liczba rekordów jest stronicowana za pomocą oraz tego, czy możemy re wyświetlanie pierwszej lub ostatniej strony danych. `TotalNumberOfProducts()` Metoda `ProductsBLL` klasy zwraca łączną liczbę produktów, jest stronicowana za pośrednictwem. Aby ustalić, czy jest wyświetlany na pierwszej stronie danych, Sprawdź indeks wiersza początkowego, gdy jest równa 0, a następnie pierwsza strona jest wyświetlana. Jeśli indeks wiersza początkowego plus maksymalna liczba wierszy do zwrócenia jest większa niż lub równa całkowita liczba rekordów jest stronicowana za pośrednictwem ostatnia strona jest wyświetlana.

Firma Microsoft będzie Poznaj Implementowanie stronicowania niestandardowego bardziej szczegółowo w następnej instrukcji.

## <a name="summary"></a>Podsumowanie

Gdy DataList ani elementu powtarzanego oferuje poza znaleziono pole obsługę stronicowania w widoku DetailsView, w widoku GridView i FormView formanty, takie funkcje można dodać przy minimalnym nakładzie pracy. Najprostszym sposobem do implementowania stronicowania domyślny jest zawijany cały zestaw produktów w ramach `PagedDataSource` , a następnie powiązać `PagedDataSource` DataList lub elementu powtarzanego. W tym samouczku dodaliśmy `GetProductsAsPagedDataSource` metodę `ProductsBLL` służącą do zwracania `PagedDataSource`. `ProductsBLL` Klasy zawiera już metody wymagane dla stronicowania niestandardowego `GetProductsPaged` i `TotalNumberOfProducts`.

Wraz z pobierania dokładny zestaw rekordów do wyświetlenia dla stronicowania niestandardowego albo wszystkie rekordy w `PagedDataSource` domyślne stronicowanie, również należy ręcznie dodać interfejs stronicowania. W tym samouczku utworzyliśmy następny, poprzedni, najpierw ostatni interfejsu z czterech kontrolki przycisku w sieci Web. Ponadto dodano formantu etykiety wyświetlanie numer bieżącej strony i łączną liczbę stron.

W następnym samouczku będzie przedstawiono sposób dodawania obsługi sortowania DataList i elementu powtarzanego. Firma Microsoft widoczne jest również sposób tworzenia DataList, która może być zarówno stronicowana i sortowane (wraz z przykładami przy użyciu domyślnego i stronicowania niestandardowego).

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Liz Shulok, Krzysztof Pespisa i Bernadette Leigh. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Next](sorting-data-in-a-datalist-or-repeater-control-cs.md)
