---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: Sortowanie danych w kontrolce DataList lub Repeater (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku zajmiemy się, jak dołączyć sortowanie pomocy technicznej w kontrolkach DataList i Repeater, jak również sposób tworzenia DataList lub Repeater, których dane można...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 85b59040cce266165353fe1627ffd983473bdcb6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371642"
---
<a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>Sortowanie danych w kontrolce DataList lub Repeater (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe) lub [Pobierz plik PDF](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> W tym samouczku zajmiemy się, jak dołączyć sortowanie pomocy technicznej w kontrolkach DataList i Repeater, jak również sposób tworzenia DataList lub Repeater, których dane mogą być stronicowana i posortowane.


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczka](paging-report-data-in-a-datalist-or-repeater-control-cs.md) zbadaliśmy, jak dodać obsługę stronicowania do kontrolką DataList. Utworzyliśmy nową metodę w `ProductsBLL` klasy (`GetProductsAsPagedDataSource`) zwrócona `PagedDataSource` obiektu. Powiązany z DataList lub Repeater, DataList lub Repeater zostanie wyświetlony tylko żądanej strony. Ta technika jest podobne do jakich jest używana wewnętrznie przez kontrolki GridView DetailsView i FormView zapewnienie ich domyślnych wbudowanych funkcji stronicowania.

Oprócz obsługę stronicowania, widoku GridView obejmuje również gotowych sortowanie pomocy technicznej. DataList ani elementu powtarzanego zapewnia wbudowaną funkcję sortowania; Jednak funkcje sortowania można dodać z fragmentem kodu. W tym samouczku zajmiemy się, jak dołączyć sortowanie pomocy technicznej w kontrolkach DataList i Repeater, jak również sposób tworzenia DataList lub Repeater, których dane mogą być stronicowana i posortowane.

## <a name="a-review-of-sorting"></a>Przegląd sortowania

Jak widzieliśmy w [stronicowanie i sortowanie danych raportu](../paging-and-sorting/paging-and-sorting-report-data-cs.md) samouczek, w kontrolce GridView zawiera gotowych sortowanie pomocy technicznej. Każde pole GridView może mieć skojarzoną `SortExpression`, co oznacza pole danych, przez którą chcesz posortować dane. Gdy GridView s `AllowSorting` właściwość jest ustawiona na `true`, każde pole GridView, który ma `SortExpression` wartość właściwości ma jej nagłówek renderowane jako element LinkButton. Gdy użytkownik kliknie określonego nagłówka s pola GridView, występuje odświeżenie strony i dane są sortowane według pola kliknięto s `SortExpression`.

Formant widoku GridView ma `SortExpression` także właściwości, która przechowuje `SortExpression` pola GridView dane są sortowane według. Ponadto `SortDirection` właściwość wskazuje, czy dane mają być sortowane w kolejności rosnącej lub malejącej (Jeśli użytkownik kliknie określonego nagłówka pola s GridView łącza dwa razy z rzędu, kolejność sortowania jest włączony).

Gdy widoku GridView jest powiązana z kontroli źródła danych, jej przekazywało jego `SortExpression` i `SortDirection` właściwości do danych kontroli źródła. Kontrola źródła danych, pobiera dane, a następnie sortuje zgodnie z podanym `SortExpression` i `SortDirection` właściwości. Po sortowaniu danych kontroli źródła danych zwraca go do widoku GridView.

Aby zreplikować tę funkcję za pomocą kontrolek DataList lub Repeater, firma Microsoft musi:

- Utwórz interfejs sortowania
- Należy pamiętać, pole danych, aby posortować według i czy mają być sortowane w porządku rosnącym lub malejącym
- Poinstruuj ObjectDataSource sortowanie danych według pola danych

Firma Microsoft będzie czoła te trzy zadania w kroku 3 i 4. Poniżej zajmiemy się, jak dołączyć zarówno stronicowanie i sortowanie pomocy technicznej w kontrolkach DataList lub Repeater.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Krok 2: Wyświetlanie produktów w elemencie powtarzanym

Zanim będziemy zajmować żadnego wykonywania funkcji związanych ze sortowanie, chętnie s zacząć produktów w kontrolce elementu powtarzanego. Zacznij od otwarcia `Sorting.aspx` stronie `PagingSortingDataListRepeater` folderu. Dodaj kontrolką elementu powtarzanego do strony sieci web, ustawiając jego `ID` właściwość `SortableProducts`. Z elementu powtarzanego s tagu inteligentnego, należy utworzyć nowe kontrolki ObjectDataSource, o nazwie `ProductsDataSource` i skonfiguruj ją, aby pobrać dane z `ProductsBLL` klasy s `GetProducts()` metody. Wybierz opcję (Brak) z listy rozwijanej na kartach INSERT, UPDATE i DELETE.


[![Tworzenie kontrolki ObjectDataSource i skonfigurować go przy użyciu metody GetProductsAsPagedDataSource()](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Rysunek 1**: tworzenie kontrolki ObjectDataSource i skonfigurować go do użycia `GetProductsAsPagedDataSource()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))


[![Ustaw listy rozwijane w ramach aktualizacji, WSTAWIANIA i usuwania karty (Brak)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**Rysunek 2**: Ustaw listy rozwijane w ramach aktualizacji, WSTAWIANIA i usuwania karty (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))


W odróżnieniu od za pomocą kontrolek DataList, Visual Studio nie tworzy automatycznie `ItemTemplate` na kontrolce elementu powtarzanego po powiązaniu go ze źródłem danych. Ponadto firma Microsoft należy dodać to `ItemTemplate` deklaratywne, zgodnie z tagu inteligentnego sterowania s Repeater brakuje opcji Edytuj szablony w kontrolkach DataList s. S pozwalają używać tego samego `ItemTemplate` z poprzedniego samouczka, które wyświetlane nazwy produktu s, dostawcy i kategorii.

Po dodaniu `ItemTemplate`, ObjectDataSource i Repeater s oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższego:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

Rysunek 3 przedstawia tej strony, podczas wyświetlania za pośrednictwem przeglądarki.


[![Każdy produkt s nazwę dostawcy i kategorii jest wyświetlana.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Rysunek 3**: s każdego produktu, nazwa, dostawcy i kategoria są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Krok 3: Poinstruowanie ObjectDataSource sortowanie danych

Aby posortować dane wyświetlane w elemencie powtarzanym, należy poinformować ObjectDataSource wyrażenia sortowania, za pomocą którego powinny być sortowane dane. Zanim kontrolki ObjectDataSource pobiera dane, go najpierw generowane jego [ `Selecting` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), zapewniającą szansy sprzedaży dla nas określić wyrażenie sortowania. `Selecting` Programu obsługi zdarzeń jest przekazywany obiekt typu [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), która ma właściwość o nazwie [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) typu [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). `DataSourceSelectArguments` Klasa służy do przekazywania żądań związanych z danymi od konsumenta danych do kontroli źródła danych i obejmuje [ `SortExpression` właściwość](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Aby przekazać sortowania informacje ze strony programu ASP.NET do kontrolki ObjectDataSource, utworzyć program obsługi zdarzeń dla `Selecting` zdarzeń i użyj następującego kodu:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

*SortExpression* można przypisać wartości nazwę pola danych, aby posortować dane, (na przykład ProductName). Nie ma właściwości związane z kierunek sortowania, więc jeśli chcesz posortować dane w kolejności malejącej, Dołącz ciąg DESC do *sortExpression* wartości (na przykład ProductName DESC).

Przejdź dalej i wypróbuj kilka różnych wartości ustaloną dla *sortExpression* i wyników testów w przeglądarce. Jak pokazano na rysunku 4, korzystając z DESC ProductName jako *sortExpression*, produkty są sortowane według nazwy w odwrotnej kolejności alfabetycznej.


[![Produkty są sortowane według nazwy w kolejności alfabetycznej wstecznego](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Rysunek 4**: produkty są sortowane według nazwy w kolejności alfabetycznej odwrotnego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Krok 4: Tworzenie interfejsu sortowania i uzupełnij wyrażenie sortowania i kierunek

Włączanie sortowanie pomocy technicznej w widoku GridView konwertuje tekst nagłówka każdego pola można sortować s LinkButton, po kliknięciu, dane są sortowane w związku z tym. Interfejs sortowania ma sens dla widoku GridView, gdzie dane jest starannego rozmieszczony w kolumnach. W przypadku kontrolek DataList i Repeater jednak innego interfejsu sortowania jest potrzebny. Wspólny interfejs sortowania, aby uzyskać listę danych (w przeciwieństwie do siatki danych), to listy rozwijanej, która zawiera pola, które można sortować dane. Pozwól s zaimplementować ten interfejs na potrzeby tego samouczka.

Dodawanie kontrolki DropDownList Web powyżej `SortableProducts` Repeater i ustaw jego `ID` właściwość `SortBy`. W oknie dialogowym właściwości kliknij wielokropek w `Items` właściwość, aby przełączyć się ListItem Edytor kolekcji. Dodaj `ListItem` s, aby posortować dane według `ProductName`, `CategoryName`, i `SupplierName` pola. Również dodać `ListItem` sortowania produkty według nazwy w odwrotnej kolejności alfabetycznej.

`ListItem` `Text` Właściwości można ustawić na dowolną wartość (takie jak nazwa), ale `Value` właściwości musi być równa nazwę pola danych (na przykład ProductName). Aby posortować wyniki w kolejności malejącej, Dołącz ciąg DESC na nazwę pola danych, takich jak ProductName DESC.


![Dodaj element listy, dla każdego pola danych można sortować](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Rysunek 5**: Dodaj `ListItem` dla każdego pola danych można sortować


Na koniec Dodaj formant sieci Web przycisku z prawej strony metody DropDownList. Ustaw jego `ID` do `RefreshRepeater` i jego `Text` właściwość odświeżania.

Po utworzeniu `ListItem` s i dodawanie przycisku odświeżania składni deklaratywnej s DropDownList i przycisk powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

Za pomocą sortowania metody DropDownList pełną, następnie należy zaktualizować s kontrolki ObjectDataSource `Selecting` programu obsługi zdarzeń, tak że używa wybranej `SortBy``ListItem` s `Value` właściwości, w przeciwieństwie do wyrażenia sortowania zakodowane.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

Na tym etapie, po raz pierwszy, odwiedzając stronę produktów początkowo zostaną posortowane według `ProductName` pola danych, ponieważ s `SortBy` `ListItem` domyślnie zaznaczone (patrz rysunek 6). Wybieranie różnych opcji, takich jak kategoria sortowania i kliknięcia przycisku Odśwież powoduje odświeżenie strony i ponownie posortować dane według nazwy kategorii, jak pokazano na rysunku 7.


[![Produkty są początkowo posortowane według nazwy](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**Rysunek 6**: produkty są początkowo posortowane według nazwy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))


[![Produkty są teraz posortowane według kategorii](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**Rysunek 7**: produkty są teraz posortowane według kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))


> [!NOTE]
> Kliknięcie przycisku Odśwież powoduje, że dane automatycznie ponownie posortowana, ponieważ stan widoku elementu powtarzanego s została wyłączona, powodowanie elementu powtarzanego ponownie powiązać ze swoim źródłem danych na każdym zwrotu. Jeśli był widoku elementu powtarzanego s stan włączony, zmiana sortowania listy rozwijanej listy wygrał t ma żadnego wpływu na kolejność sortowania. Aby rozwiązać ten problem, Utwórz program obsługi zdarzeń dla s przycisk Odśwież `Click` zdarzeń i ponowne wiązanie powtarzanego do źródła danych (przez wywołanie elementu powtarzanego s `DataBind()` metody).


## <a name="remembering-the-sort-expression-and-direction"></a>Uzupełnij wyrażenie sortowania i kierunek

Podczas tworzenia sortowanie DataList lub Repeater na stronie, na których inne niż sortowanie związane z ogłaszania zwrotnego może wystąpić, jego imperatywne s, należy pamiętać wyrażenie sortowania i kierunek w ogłaszania zwrotnego. Na przykład Wyobraź sobie, że Zaktualizowaliśmy powtarzanego, w tym samouczku, aby uwzględnić przycisk Usuń w poszczególnych produktach. Gdy użytkownik kliknie przycisk Usuń d przeprowadzamy kodu, aby usunąć wybrany produkt, a następnie ponownie powiązanie danych do elementu powtarzanego. Jeśli szczegóły sortowania nie są utrwalane w zwrotu, dane wyświetlane na ekranie zostaną przywrócone do oryginalnej kolejności sortowania.

W tym samouczku metody DropDownList niejawnie zapisuje sortowanie wyrażenie i kierunek w swój stan widoku dla nas. Możemy używania innego interfejsu sortowania, jednego z LinkButtons powiedzieć, podanym różne opcje sortowania d należy uważać, aby pamiętać porządek sortowania w ogłaszania zwrotnego. Można to osiągnąć dzięki przechowywaniu sortowania parametrów w stanie widoku strony s, uwzględniając parametr sortowania w zmiennej querystring lub za pomocą niektórych metod trwałości stanu.

Przyszłe przykłady w tym samouczku Poznaj sposoby utrwalania sortowania szczegóły w stanie widoku strony s.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Krok 5: Dodawanie sortowania pomocy technicznej, aby DataList, korzystającą stronicowania domyślne

W [poprzedni Samouczek](paging-report-data-in-a-datalist-or-repeater-control-cs.md) zbadaliśmy sposób implementacji domyślnej stronicowania z kontrolką DataList. Pozwól s rozszerzyć ten poprzedniego przykładu, aby uwzględnić możliwość sortowania stronicowanych danych. Zacznij od otwarcia `SortingWithDefaultPaging.aspx` i `Paging.aspx` stron w `PagingSortingDataListRepeater` folderu. Z `Paging.aspx` kliknij przycisk źródła, aby wyświetlić oznaczeniu deklaracyjnym strony s. Skopiowanie zaznaczonego tekstu (zobacz rysunek 8) i wklej go w oznaczeniu deklaracyjnym `SortingWithDefaultPaging.aspx` między `<asp:Content>` tagów.


[![Replikowanie oznaczeniu deklaracyjnym w &lt;asp: Content&gt; tagów z Paging.aspx SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**Rysunek 8**: replikowanie oznaczeniu deklaracyjnym w `<asp:Content>` tagi z `Paging.aspx` do `SortingWithDefaultPaging.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))


Po skopiowaniu oznaczeniu deklaracyjnym skopiuj metody i właściwości w `Paging.aspx` stronie s osobna klasa kodu do klasy CodeBehind dla `SortingWithDefaultPaging.aspx`. Następnie Poświęć chwilę, aby wyświetlić `SortingWithDefaultPaging.aspx` strony w przeglądarce. Powinna działać, te same funkcje i wygląd jako `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Udoskonalanie ProductsBLL obejmujący domyślne stronicowanie i sortowanie — metoda

W poprzednim samouczku utworzyliśmy `GetProductsAsPagedDataSource(pageIndex, pageSize)` method in Class metoda `ProductsBLL` klasy, która jest zwracana `PagedDataSource` obiektu. To `PagedDataSource` obiekt został wypełniony *wszystkich* produktów (za pośrednictwem s LOGIKI `GetProducts()` metoda), ale podczas wiązania z kontrolki DataList tylko te rekordy, odpowiadających określonym *pageIndex* i *pageSize* były wyświetlane parametry wejściowe.

Wcześniej w tym samouczku Dodaliśmy obsługę sortowania, określając wyrażenia sortowania z ObjectDataSource s `Selecting` programu obsługi zdarzeń. To działa dobrze podczas kontrolki ObjectDataSource jest zwracany obiekt, który można sortować, takie jak `ProductsDataTable` zwrócone przez `GetProducts()` metody. Jednak `PagedDataSource` obiektu zwróconego przez `GetProductsAsPagedDataSource` metoda nie obsługuje sortowania elementów źródła danych wewnętrznych. Zamiast tego, musimy posortować wyniki zwrócone z `GetProducts()` metoda *przed* ujętej w `PagedDataSource`.

Aby to osiągnąć, utworzenie nowej metody w `ProductsBLL` klasy `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Aby posortować `ProductsDataTable` zwrócone przez `GetProducts()` metody, określ `Sort` właściwości domyślnej `DataTableView`:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

`GetProductsSortedAsPagedDataSource` Metoda różni się tylko z `GetProductsAsPagedDataSource` metoda utworzony w poprzednim samouczku. W szczególności `GetProductsSortedAsPagedDataSource` akceptuje dodatkowego parametru wejściowego `sortExpression` i przypisuje tę wartość, aby `Sort` właściwość `ProductDataTable` s `DefaultView`. Kilka wierszy kodu później `PagedDataSource` przypisany jest obiekt s DataSource `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Wywołanie metody GetProductsSortedAsPagedDataSource i określając wartość dla parametru SortExpression dane wejściowe

Za pomocą `GetProductsSortedAsPagedDataSource` metody pełną, następnym krokiem jest podawanie wartości dla tego parametru. Kontrolki ObjectDataSource w `SortingWithDefaultPaging.aspx` jest obecnie skonfigurowany do wywołania `GetProductsAsPagedDataSource` metody i przekazuje w dwóch parametrów wejściowych za pomocą jego dwóch `QueryStringParameters`, które zostały określone w `SelectParameters` kolekcji. Te dwa `QueryStringParameters` wskazują, że źródło `GetProductsAsPagedDataSource` metoda s *pageIndex* i *pageSize* parametry pochodzą z pól querystring `pageIndex` i `pageSize`.

Aktualizuj ObjectDataSource s `SelectMethod` właściwości, tak że wywołuje nowej `GetProductsSortedAsPagedDataSource` metody. Następnie należy dodać nowy `QueryStringParameter` tak, aby *sortExpression* parametr wejściowy jest dostępny z pole querystring `sortExpression`. Ustaw `QueryStringParameter` s `DefaultValue` do ProductName.

Po wprowadzeniu tych zmian s ObjectDataSource o oznaczeniu deklaracyjnym powinien wyglądać:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

W tym momencie `SortingWithDefaultPaging.aspx` strony będzie alfabetycznie sortować wyniki według nazwy produktu (patrz rysunek 9). Jest to spowodowane domyślnie wartość ProductName jest przekazywany jako `GetProductsSortedAsPagedDataSource` metoda s *sortExpression* parametru.


[![Domyślnie wyniki są sortowane według ProductName](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**Rysunek 9**: Domyślnie, wyniki są sortowane według `ProductName` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))


Jeśli ręcznie dodasz `sortExpression` pole querystring, takie jak `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` wyniki będą sortowane według określonego `sortExpression`. Jednak to `sortExpression` parametru nie ma w zmiennej querystring podczas przechodzenia do innej strony danych. W rzeczywistości, klikając przycisk Dalej lub ostatniej strony przyciski przyjmuje nam z powrotem do `Paging.aspx`! Ponadto s występują obecnie sortowania nie interfejs. Jest to jedyny sposób użytkownik może zmienić kolejność sortowania stronicowanych danych przez bezpośrednie manipulowanie ciąg zapytania.

## <a name="creating-the-sorting-interface"></a>Tworzenie interfejsu sortowania

Najpierw należy zaktualizować `RedirectUser` metodę, aby wysłać użytkownikowi `SortingWithDefaultPaging.aspx` (zamiast `Paging.aspx`) i dołączyć `sortExpression` wartość w zmiennej querystring. Tylko do odczytu na poziomie strony o nazwie należy również dodać `SortExpression` właściwości. Ta właściwość jest podobne do `PageIndex` i `PageSize` właściwości utworzony w poprzednim samouczku zwraca wartość `sortExpression` pole querystring, jeśli istnieje, a wartość domyślna wartość (NazwaProduktu) w przeciwnym razie.

Obecnie `RedirectUser` metoda przyjmuje tylko pojedynczy parametr wejściowy elementu indeks strony do wyświetlenia. Jednak może być czasy, kiedy chcemy, aby przekierować użytkownika do określonej strony danych przy użyciu wyrażenie sortowania niż s, jakie określone w zmiennej querystring. Później utworzymy sortowania interfejsu dla tej strony, która będzie zawierać szereg formantów Web przycisk sortowania danych według określonej kolumny. Po kliknięciu jednego z tych przycisków, chcemy przekieruje użytkownika, przekazując wartość wyrażenia sortowania odpowiednie. Aby zapewnić tę funkcję, Utwórz dwie wersje `RedirectUser` metody. Pierwsza z nich powinna obsługiwać tylko indeks strony do wyświetlenia, a druga akceptuje strony wyrażenie indeksu i sortowania.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

W pierwszym przykładzie w tym samouczku utworzyliśmy interfejsu sortowania, przy użyciu kontrolki DropDownList. W tym przykładzie umożliwiają s Użyj trzy kontrolki przycisku w sieci Web umieszczana powyżej DataList, jeden dla sortowanie według `ProductName`, jeden dla `CategoryName`, a drugi dla `SupplierName`. Dodaj trzy kontrolki przycisku w sieci Web, ustawianie ich `ID` i `Text` właściwości odpowiednio:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

Następnie należy utworzyć `Click` programu obsługi zdarzeń dla każdego. Programy obsługi zdarzeń powinny wywoływać `RedirectUser` metody, zwracając użytkownika do pierwszej strony za pomocą wyrażenia sortowania odpowiednie.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Po pierwsze, odwiedzając stronę, dane są sortowane według nazwy produktu alfabetycznie (odnoszą się do rysunek 9). Kliknij przycisk Dalej, przejdź do drugiej strony danych, a następnie kliknij przycisk sortowania według kategorii przycisku. Spowoduje to zwrócenie nam do pierwszej strony dane, posortowane według nazwy kategorii (zobacz rysunek 10). Podobnie kliknięcie przycisku sortowania przez dostawcę przycisk sortowania danych przez dostawcę, zaczynając od pierwszej strony danych. Opcja sortowania zapamiętywane jest jak dane są stronicowane za pośrednictwem. Na ilustracji 11 pokazano strony po sortowanie według kategorii, a następnie przesuwania do 13 strony danych.


[![Produkty są sortowane według kategorii](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**Na rysunku nr 10**: produkty są sortowane według kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))


[![Wyrażenie sortowania są zapamiętywane podczas stronicowanie za pośrednictwem dane](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**Rysunek 11**: wyrażenie sortowania są zapamiętywane podczas stronicowanie za pośrednictwem dane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Krok 6: Niestandardowe stronicować rekordów w elemencie powtarzanym

W przykładzie DataList sprawdzane w kroku 5 stron przy użyciu jego danych korzystające z techniki stronicowania nieefektywne domyślne. Gdy stronicowanie wystarczająco duże ilości danych, konieczne jest stosowanie niestandardowy stronicowania. Ponownie [efektywne stronicowanie za pośrednictwem dużych ilości danych](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) i [sortowanie danych niestandardowych stronicowanej](../paging-and-sorting/sorting-custom-paged-data-cs.md) samouczki, zbadaliśmy różnice między domyślnymi i stronicowania niestandardowego i metody utworzonego w LOGIKI dla Korzystanie z niestandardowych stronicowanie i sortowanie danych stronicowane niestandardowych. W szczególności w tych dwóch poprzednich samouczkach dodaliśmy następujące trzy metody do `ProductsBLL` klasy:

- `GetProductsPaged(startRowIndex, maximumRows)` Zwraca rekordy, począwszy od konkretnego podzbioru *startRowIndex* i nieprzekraczającej *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` Zwraca określony podzbiór rekordów posortowanych według określonego *sortExpression* parametr wejściowy.
- `TotalNumberOfProducts()` udostępnia całkowita liczba rekordów w `Products` tabeli bazy danych.

Tych metod może służyć do wydajnego strony i przeglądanie danych za pomocą kontrolki DataList lub Repeater. Na przykład zezwolić s, Rozpocznij od utworzenia kontrolką elementu powtarzanego z niestandardowych obsługę stronicowania; następnie dodamy funkcje sortowania.

Otwórz `SortingWithCustomPaging.aspx` strony w `PagingSortingDataListRepeater` folder, a na stronie, Dodaj Repeater jego `ID` właściwość `Products`. Z elementu powtarzanego s tagu inteligentnego, należy utworzyć nowe kontrolki ObjectDataSource, o nazwie `ProductsDataSource`. Skonfiguruj ją, aby można było wybrać swoje dane z `ProductsBLL` klasy s `GetProductsPaged` metody.


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetProductsPaged ProductsBLL klasy s](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**Rysunek 12**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsBLL` klasy s `GetProductsPaged` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))


Ustawianie list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak), a następnie kliknij przycisk Dalej. Teraz monituje Kreator konfigurowania źródła danych dla źródła `GetProductsPaged` metoda s *startRowIndex* i *maximumRows* parametrów wejściowych. W rzeczywistości te parametry wejściowe są ignorowane. Zamiast tego *startRowIndex* i *maximumRows* wartości zostaną przekazane w za pośrednictwem `Arguments` właściwości w elemencie ObjectDataSource s `Selecting` procedura obsługi zdarzeń, podobnie jak określonej *sortExpression* w tej wersji demonstracyjnej do pierwszego samouczka s. W związku z tym należy pozostawić źródło parametru list rozwijanych w kreatorze na None.


[![Pozostaw zestaw źródeł parametrów None](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**Rysunek 13**: pozostaw parametr źródeł wartość None ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))


> [!NOTE]
> Czy *nie* zestawu ObjectDataSource s `EnablePaging` właściwość `true`. Spowoduje to ObjectDataSource automatycznie uwzględnić swój własny *startRowIndex* i *maximumRows* parametry `SelectMethod` s istniejącej listy parametrów. `EnablePaging` Właściwość jest przydatna, gdy niestandardowe powiązanie stronicowanych danych do kontrolki GridView, DetailsView lub FormView, ponieważ te kontrolki oczekiwać określone zachowanie z kontrolki ObjectDataSource tego s dostępna tylko wtedy, gdy `EnablePaging` właściwość `true`. Ponieważ mamy ręcznie dodać obsługę stronicowania DataList i Repeater, pozostaw tę właściwość ustawioną na `false` (ustawienie domyślne), ponieważ będzie Zaimplementowaliśmy w funkcje bezpośrednio z poziomu strony ASP.NET.


Na koniec zdefiniuj powtarzanego s `ItemTemplate` tak, aby nazwa produktu s, kategorii i dostawcy. Po wprowadzeniu tych zmian składni deklaratywnej s ObjectDataSource i Repeater powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

Poświęć chwilę, odwiedź stronę za pośrednictwem przeglądarki i należy pamiętać, że nie zwrócono żadnych rekordów. Jest to spowodowane możemy ve jeszcze, aby określić *startRowIndex* i *maximumRows* wartości parametrów; w związku z tym, wartości 0 jest przekazywany w obu. Aby określić te wartości, należy utworzyć program obsługi zdarzeń dla ObjectDataSource s `Selecting` zdarzeń i zestaw tych parametrów wartości programowo do zakodowanych wartości od 0 do 5, odpowiednio:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

Dzięki tej zmianie na stronie, podczas wyświetlania za pośrednictwem przeglądarki, znajdują się pięciu pierwszych produktów.


[![Wyświetlanych jest pierwszych pięć rekordów](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**Rysunek 14**: wyświetlanych jest pierwszych pięć rekordów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))


> [!NOTE]
> Produkty wymienione na rysunku 14 się zdarzyć, mają być sortowane według nazwy produktu, ponieważ `GetProductsPaged` procedury przechowywanej, która wykonuje zapytania efektywne stronicowanie niestandardowego porządkuje wyniki według `ProductName`.


Aby umożliwić użytkownikowi krok na stronach, musimy śledzić indeks wiersza uruchamiania i maksymalna liczba wierszy i Zapamiętaj te wartości w ogłaszania zwrotnego. W przykładzie stronicowania domyślne użyliśmy querystring pola, aby zachować te wartości. dla tej wersji demonstracyjnej umożliwiają s zachować te informacje w stanie widoku strony s. Utwórz dwie poniższe właściwości:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

Następnie zaktualizuj kod w obsłudze zdarzeń wybranie, tak aby używał `StartRowIndex` i `MaximumRows` właściwości zamiast zakodowanych wartości od 0 do 5:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

W tym momencie naszą stronę nadal będzie widoczny tylko pierwszych pięć rekordów. Jednak z tymi właściwościami w miejscu, możemy ponownie gotowe do utworzenia interfejsu stronicowania.

## <a name="adding-the-paging-interface"></a>Dodawanie interfejsu stronicowania

Pozwól s Użyj tego samego pierwszy, Previous, następnie ostatniego stronicowania interfejsu używane w przykładzie stronicowania domyślne etykiety sieci Web jest wyświetlany formantu, który wyświetla stronę, jakich danych w tym oraz ile łączna liczba stron istnieje. Dodaj cztery kontrolki sieci Web przycisk i etykietę pod powtarzanego.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

Następnie należy utworzyć `Click` programy obsługi zdarzeń dla czterech przycisków. Po kliknięciu jednego z tych przycisków, musimy zaktualizować `StartRowIndex` i ponowne powiązywanie danych do elementu powtarzanego. Kod dla przycisków pierwszy, Wstecz i dalej jest prosta, ale dla ostatniego przycisku jak określamy indeks wiersza początkowego dla ostatniej strony danych? Do obliczenia tego indeksu, a także możliwość określenia, czy powinno być włączone przyciski następnej i ostatnio musimy wiedzieć, jak wiele rekordów w sumie są są stronicowane za pośrednictwem. Określamy to przez wywołanie metody `ProductsBLL` klasy s `TotalNumberOfProducts()` metody. S pozwalają utworzyć właściwości tylko do odczytu, na poziomie strony o nazwie `TotalRowCount` zwracającego wyniki `TotalNumberOfProducts()` metody:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

Przy użyciu tej właściwości można teraz określić ostatni indeks wiersza początkowego strony s. W szczególności jego s wyniku liczby całkowitej z `TotalRowCount` minus wynik dzielenia 1 przez `MaximumRows`, pomnożone przez `MaximumRows`. Firma Microsoft może teraz napisać `Click` obsługi zdarzeń dla czterech przycisków interfejsu stronicowania:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

Na koniec należy wyłączyć pierwszy i poprzedni przycisków w interfejsie stronicowania, podczas wyświetlania pierwszej strony danych i przyciski następnej i ostatnio podczas wyświetlania ostatniej strony. Aby to zrobić, Dodaj następujący kod do ObjectDataSource s `Selecting` program obsługi zdarzeń:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

Po dodaniu tych `Click` procedury obsługi zdarzeń i kod, aby włączyć lub wyłączyć elementy interfejsu stronicowania, które są oparte na indeks bieżącego wiersza początkowego przetestować stronę w przeglądarce. Rysunek 15 ilustruje najpierw, odwiedzając stronę pierwszego i przyciski Wstecz zostaną są wyłączone. Jeśli klikniesz pozycję dalej pokazuje drugiej strony danych, podczas gdy kliknięcie ostatni Wyświetla ostatnią stronę (patrz rysunki 16 i 17). Podczas wyświetlania na ostatniej stronie danych przycisków dalej i w ostatniej są wyłączone.


[![Wstecz i ostatniego przyciski są wyłączone podczas wyświetlania strony pierwszy produktów](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**Rysunek 15**: Previous i ostatniego przyciski są wyłączone podczas wyświetlania strony pierwszy produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))


[![Druga strona produkty są Dispalyed](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**Rysunek 16**: drugiej strony produktów są Dispalyed ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))


[![Kliknięcie przycisku Wyświetla ostatni ostatnią stronę danych](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**Rysunek 17**: kliknięcie ostatni powoduje wyświetlenie strony dane końcowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Krok 7: W tym sortowanie pomocy technicznej z niestandardowym stronicowanej Repeater

Teraz, gdy niestandardowe stronicowania zostały wdrożone, ponownie gotowe, aby uwzględnić sortowanie obsługujemy. `ProductsBLL` Klasy s `GetProductsPagedAndSorted` metoda ma taką samą *startRowIndex* i *maximumRows* parametry wejściowe `GetProductsPaged`, ale zezwala na dodatkowe  *sortExpression* parametr wejściowy. Aby użyć `GetProductsPagedAndSorted` metody z `SortingWithCustomPaging.aspx`, należy wykonać następujące czynności:

1. Zmiana ObjectDataSource s `SelectMethod` właściwość `GetProductsPaged` do `GetProductsPagedAndSorted`.
2. Dodaj *sortExpression* `Parameter` obiektu s ObjectDataSource `SelectParameters` kolekcji.
3. Tworzenie prywatnego, poziomu strony `SortExpression` właściwość, która jej wartość są utrwalane w ogłaszania zwrotnego za pośrednictwem stanu widoku strony s.
4. Aktualizuj ObjectDataSource s `Selecting` programu obsługi zdarzeń, aby przypisać ObjectDataSource s *sortExpression* parametru wartość poziomu strony `SortExpression` właściwości.
5. Utwórz interfejs sortowania.

Rozpocznij, aktualizując ObjectDataSource s `SelectMethod` właściwości i dodając *sortExpression* `Parameter`. Upewnij się, że *sortExpression* `Parameter` s `Type` właściwość jest ustawiona na `String`. Po ukończeniu tych dwóch pierwszych zadań s ObjectDataSource o oznaczeniu deklaracyjnym powinien wyglądać następująco:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

Następnie należy poziomu strony `SortExpression` właściwość, której wartość jest serializowana, aby wyświetlić stan. Jeśli nie ustawiono żadnej wartości wyrażenia sortowania, należy użyć ProductName jako domyślny:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

Zanim wywołuje kontrolki ObjectDataSource `GetProductsPagedAndSorted` metoda musimy *sortExpression* `Parameter` wartość `SortExpression` właściwości. W `Selecting` procedura obsługi zdarzeń, Dodaj następujący wiersz kodu:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

Pozostaje tylko do zaimplementowania interfejsu sortowania. Ile My mieliśmy w ostatnim przykładzie umożliwiają s mieć sortowania interfejs implementowany przy użyciu trzech kontrolki przycisku w sieci Web, które umożliwiają użytkownikom posortować wyniki według nazwy produktu, kategorii lub dostawcy.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

Utwórz `Click` programy obsługi zdarzeń dla tych trzech kontrolek przycisku. W przypadku obsługi resetowania `StartRowIndex` 0, należy ustawić `SortExpression` do odpowiedniej wartości i ponowne wiązanie danych do powtarzanego:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

Wszystkie dostępne tego s jest! Gdy wystąpiły kilka kroków, które można pobrać niestandardowych stronicowania i sortowania, zaimplementowane, kroki były bardzo podobne do tych służące do domyślnego stronicowania. Rysunek 18 zawiera produkty, podczas wyświetlania na ostatniej stronie danych, gdy posortowane według kategorii.


[![Ostatnia strona danych, posortowane według kategorii, jest wyświetlany.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**Rysunek 18**: ostatniej strony danych, posortowane według kategorii, jest wyświetlany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))


> [!NOTE]
> W poprzednich przykładach podczas sortowania przez dostawcę, który NazwaDostawcy została użyta jako wyrażenie sortowania. Jednak dla niestandardowych implementacji stronicowania, należy użyć CompanyName. Jest to spowodowane procedury składowanej odpowiedzialnych za wdrażanie niestandardowych stronicowania `GetProductsPagedAndSorted` przekazuje wyrażenia sortowania w `ROW_NUMBER()` — słowo kluczowe, `ROW_NUMBER()` — słowo kluczowe wymaga nazwy kolumny rzeczywiste, a nie jej alias. W związku z tym, trzeba użyć `CompanyName` (nazwa kolumny w `Suppliers` tabeli) zamiast alias użyty w `SELECT` zapytania (`SupplierName`) dla wyrażenia sortowania.


## <a name="summary"></a>Podsumowanie

Ani DataList ani elementu powtarzanego oferuje wbudowaną obsługę sortowania, ale bit kodu i niestandardowy interfejs sortowania, można dodać takich funkcji. Podczas implementowania, sortowanie, ale nie stronicowania, wyrażenie sortowania można określić za pomocą `DataSourceSelectArguments` obiekt przekazany do ObjectDataSource s `Select` metody. To `DataSourceSelectArguments` obiektu s `SortExpression` można przypisać właściwości w elemencie ObjectDataSource s `Selecting` programu obsługi zdarzeń.

Aby dodać funkcje sortowania do DataList lub Repeater już zapewniający obsługę stronicowania, to najłatwiejsza metoda jest dostosowywanie warstwy logiki biznesowej, aby uwzględnić metodę, która akceptuje wyrażenie sortowania. Informacje te można następnie przekazać w za pomocą parametru w ObjectDataSource s `SelectParameters`.

Nasze badania stronicowanie i sortowanie za pomocą kontrolek DataList i Repeater kończy się w tym samouczku. Nasz samouczek dotyczący dalej i jest final zbada, jak dodać kontrolki przycisku w sieci Web do szablonów s w kontrolkach DataList i Repeater zapewnić niektóre funkcje niestandardowe, inicjowane przez użytkownika na podstawie poszczególnych elementów.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został David Suru. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
> [dalej](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
