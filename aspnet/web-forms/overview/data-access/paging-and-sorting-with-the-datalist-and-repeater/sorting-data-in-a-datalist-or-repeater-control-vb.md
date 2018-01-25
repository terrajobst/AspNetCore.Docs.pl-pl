---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: Sortowanie danych w kontrolce elementu powtarzanego (VB) lub DataList | Dokumentacja firmy Microsoft
author: rick-anderson
description: "W tym samouczku zajmiemy się, jak dołączyć sortowanie Obsługa w DataList i powtarzanego, jak również sposób tworzenia DataList lub elementu powtarzanego, których dane mogą..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 0133a74454a7754f4f7087e2121c7387a1aef8a8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>Sortowanie danych w kontrolce elementu powtarzanego (VB) lub DataList
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe) lub [pobierania plików PDF](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> W tym samouczku zajmiemy się, jak dołączyć sortowanie Obsługa w DataList i powtarzanego, jak również sposób tworzenia DataList lub elementu powtarzanego, których dane mogą być stronicowana i sortowane.


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczek](paging-report-data-in-a-datalist-or-repeater-control-vb.md) możemy zbadać, jak dodać obsługę stronicowania do elementu DataList. Utworzyliśmy nową metodę w `ProductsBLL` klasy (`GetProductsAsPagedDataSource`) zwróconą `PagedDataSource` obiektu. Powiązany z DataList lub elementu powtarzanego, DataList lub elementu powtarzanego wyświetla tylko żądanej strony. Ta metoda jest podobna do co to jest używana wewnętrznie przez formanty widoku GridView widoku DetailsView i FormView udostępniane stronicowania wbudowanej domyślnej.

Oprócz oferuje obsługę stronicowania, widoku GridView także fabrycznej sortowanie pomocy technicznej. DataList ani elementu powtarzanego zapewnia wbudowaną funkcję sortowania; Jednak funkcje sortowania można dodać z bitowego kodu. W tym samouczku zajmiemy się, jak dołączyć sortowanie Obsługa w DataList i powtarzanego, jak również sposób tworzenia DataList lub elementu powtarzanego, których dane mogą być stronicowana i sortowane.

## <a name="a-review-of-sorting"></a>Przegląd sortowanie

Jak widzieliśmy w [stronicowania i sortowania danych raportów](../paging-and-sorting/paging-and-sorting-report-data-vb.md) samouczka kontrolki widoku siatki udostępnia fabrycznej sortowanie pomocy technicznej. Każde pole widoku GridView może mieć skojarzone `SortExpression`, co oznacza pole danych, według którego będą sortowane dane. Gdy s widoku GridView `AllowSorting` właściwość jest ustawiona na `true`, każde pole widoku GridView, który ma `SortExpression` jej nagłówek renderowane jako element LinkButton ma wartość właściwości. Po kliknięciu określonego nagłówka s pola widoku GridView występuje odświeżania strony i dane są sortowane według pola klikniętej s `SortExpression`.

Formant widoku GridView ma `SortExpression` również właściwość, która przechowuje `SortExpression` pola widoku GridView dane są sortowane według. Ponadto `SortDirection` właściwość wskazuje, czy dane mają być sortowane w kolejności rosnącej lub malejącej (Jeśli użytkownik klika polecenie określonego nagłówka pola s GridView łącza dwa razy pod rząd, kolejność sortowania jest przełączane).

Gdy widoku GridView jest powiązana z jego kontroli źródła danych, jej przekazuje jego `SortExpression` i `SortDirection` właściwości do danych kontroli źródła. Formant źródła danych pobiera dane, a następnie sortuje ją zgodnie z wybranych `SortExpression` i `SortDirection` właściwości. Po posortowaniu danych, kontroli źródła danych zwraca go do widoku GridView.

Aby zreplikować tej funkcji z formantami DataList lub elementu powtarzanego, firma musi:

- Tworzenie interfejsu sortowania
- Należy pamiętać, aby sortować według pola danych i czy mają być sortowane w kolejności rosnącej lub malejącej
- Poinstruuj ObjectDataSource, aby posortować dane według pola danych

Firma Microsoft będzie rozwiązania te trzy zadania w kroku 3 i 4. Następujące, zajmiemy się, jak dołączyć zarówno stronicowania i sortowania pomocy technicznej w DataList lub elementu powtarzanego.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Krok 2: Wyświetlanie produktów w elementu powtarzanego

Zanim firma Microsoft martwić żadnego wykonywania funkcji związanych z sortowania, chętnie s zacząć produktów w kontrolce elementu powtarzanego. Uruchamianie przez otwarcie `Sorting.aspx` strony `PagingSortingDataListRepeater` folderu. Dodawanie formantu elementu powtarzanego do strony sieci web, ustawienie jej `ID` właściwości `SortableProducts`. W tagu elementu powtarzanego s, utworzyć nowy element ObjectDataSource o nazwie `ProductsDataSource` i skonfigurować go, aby pobrać dane z `ProductsBLL` klasy s `GetProducts()` metody. Wybierz opcję (Brak) z list rozwijanych w kartach INSERT, UPDATE i DELETE.


[![Utwórz element ObjectDataSource i skonfiguruj ją przy użyciu metody GetProductsAsPagedDataSource()](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Rysunek 1**: utworzyć ObjectDataSource i skonfigurować go do użytku `GetProductsAsPagedDataSource()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))


[![Ustaw listy rozwijane w UPDATE, INSERT i usuwanie kart na (Brak)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**Rysunek 2**: Ustaw listy rozwijane w UPDATE, INSERT i usuwanie kart na (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))


W odróżnieniu od z DataList, Visual Studio nie tworzy automatycznie `ItemTemplate` na kontrolce elementu powtarzanego po jego powiązanie ze źródłem danych. Ponadto firma Microsoft należy dodać to `ItemTemplate` deklaratywnie, zgodnie z opcją Edytuj szablony w DataList s nie ma tagu formantu s elementu powtarzanego. Let s używać tego samego `ItemTemplate` z poprzednich samouczek, którego wyświetlana nazwa produktu s, dostawcy i kategorii.

Po dodaniu `ItemTemplate`, znaczników deklaratywne s elementu powtarzanego i ObjectDataSource powinien wyglądać podobnie do następującego:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

Rysunek 3 przedstawia tę stronę, podczas wyświetlania za pośrednictwem przeglądarki.


[![Każdy produkt s Nazwa dostawcy i kategorii jest wyświetlany.](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Rysunek 3**: s każdego produktu, nazwa, dostawcy i kategoria są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Krok 3: Jeśli element ObjectDataSource sortowanie danych

Aby posortować dane wyświetlane w elemencie powtarzanym, należy poinformować ObjectDataSource wyrażenia sortowania, według której mają być sortowane dane. Przed ObjectDataSource pobiera dane, go najpierw generowane jego [ `Selecting` zdarzenia](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), który zapewnia firmie Microsoft w celu określenia wyrażenie sortowania. `Selecting` Program obsługi zdarzeń jest przekazywany obiekt typu [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), który zawiera właściwości o nazwie [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) typu [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). `DataSourceSelectArguments` Klasa służy do przekazywania żądań związanych z danymi od konsumenta danych do kontroli źródła danych i zawiera [ `SortExpression` właściwości](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Aby przekazać informacje sortowania ze strony ASP.NET do ObjectDataSource, utworzyć programu obsługi zdarzeń dla `Selecting` zdarzeń i użyj następującego kodu:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

*SortExpression* można przypisać wartości nazwę pola danych, aby posortować dane przez (na przykład ProductName). Nie ma właściwości związanych z kierunek sortowania, więc jeśli chcesz posortować dane w kolejności malejącej, Dołącz ciąg DESC do *sortExpression* wartości (na przykład ProductName DESC).

Przejdź dalej i spróbuj niektóre inne stałe wartości *sortExpression* i testować wyniki w przeglądarce. Jak na rysunku 4 przedstawiono przy użyciu DESC ProductName jako *sortExpression*, produkty są sortowane według nazwy w odwrotnej kolejności alfabetycznej.


[![Te produkty są sortowane według nazwy w odwrotnej kolejności alfabetycznej](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Rysunek 4**: produkty są sortowane według nazwy w odwrotnej kolejności alfabetycznej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Krok 4: Tworzenie interfejsu sortowania i zapamiętywanie wyrażenie sortowania i kierunek

Włączanie obsługi w widoku GridView sortowanie konwertuje tekst nagłówka każdej sortowania pola s element LinkButton, po kliknięciu, dane są sortowane w związku z tym. Interfejs sortowania sens widoku GridView, w którym dane starannie układu w kolumnach. Formant DataList i powtarzanego jednak innego interfejsu sortowania na potrzeby. Wspólny interfejs sortowania listę danych (w przeciwieństwie do siatki danych), jest listy rozwijanej, która zawiera pola, które można sortować dane. Let s zaimplementować taki interfejs dla tego samouczka.

Dodaj formant sieci DropDownList Web powyżej `SortableProducts` elementu powtarzanego i ustaw jej `ID` właściwości `SortBy`. W oknie właściwości, kliknij przycisk wielokropka w `Items` właściwość, aby przełączyć się Edytor kolekcji elementu listy. Dodaj `ListItem` s, aby posortować dane według `ProductName`, `CategoryName`, i `SupplierName` pól. Dodaj również `ListItem` do sortowania produktów według nazwy w odwrotnej kolejności alfabetycznej.

`ListItem` `Text` Właściwości można ustawić dowolną wartość (takie jak nazwa), ale `Value` właściwości musi mieć ustawioną nazwę pola danych (na przykład ProductName). Aby posortować wyniki w kolejności malejącej, Dołącz ciąg DESC na nazwę pola danych, takich jak ProductName DESC.


![Dodawanie elementu ListItem dla każdego pola Sortowanie danych](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Rysunek 5**: Dodaj `ListItem` dla każdego pola Sortowanie danych


Na koniec dodać kontrolkę Web przycisku z prawej strony DropDownList. Ustaw jego `ID` do `RefreshRepeater` i jego `Text` właściwości do odświeżania.

Po utworzeniu `ListItem` s i dodawanie przycisku Odśwież składni deklaratywnej s DropDownList i przycisk powinien wyglądać podobnie do następującego:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

Z pełną DropDownList sortowania, następnie należy zaktualizować ObjectDataSource s `Selecting` obsługi zdarzeń, tak że używa wybranego `SortBy``ListItem` s `Value` właściwości, w przeciwieństwie do wyrażenia sortowania z wpisaną na stałe.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

Na tym etapie, gdy najpierw odwiedzając stronę produktów początkowo można sortować według `ProductName` pola danych, ponieważ s `SortBy` `ListItem` domyślnie zaznaczone (patrz rysunek 6). Wybranie innego sortowania opcji, takich jak kategorii i kliknięcie przycisku Odśwież powoduje odświeżenie strony i ponownie posortować dane według nazwy kategorii, jak pokazano na rysunku 7.


[![Te produkty nie są początkowo posortowane według nazwy](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**Rysunek 6**: produkty nie są początkowo posortowane według nazwy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))


[![Te produkty nie są teraz posortowane według kategorii](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**Rysunek 7**: produkty nie są teraz posortowane według kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))


> [!NOTE]
> Kliknięcie przycisku Odśwież powoduje, że dane do automatycznie ponownie posortowana ponieważ stan elementu powtarzanego s widoku zostało wyłączone, co powoduje powtarzanego ponownie powiązać ze swoim źródłem danych na każdym ogłaszania zwrotnego. Jeśli był pozostanie widok elementu powtarzanego s stan włączone, zmiana sortowania listy rozwijanej liście won t ma żadnego wpływu na kolejność sortowania. Aby rozwiązać ten problem, należy utworzyć programu obsługi zdarzeń dla przycisku Odśwież s `Click` zdarzeń i ponownie Utwórz wiązanie powtarzanego do źródła danych (przez wywołanie elementu powtarzanego s `DataBind()` metody).


## <a name="remembering-the-sort-expression-and-direction"></a>Zapamiętywanie wyrażenie sortowania i kierunek

Podczas tworzenia sortowanie DataList lub elementu powtarzanego na stronie, których nie sortowania związane z ogłaszania zwrotnego może wystąpić, jego imperatywne s, że wyrażenie sortowania i kierunek zapamiętany między ogłaszania zwrotnego. Załóżmy na przykład, że Zaktualizowaliśmy powtarzanego w tym samouczku, aby uwzględnić przycisk usuwania z każdego produktu. Gdy użytkownik kliknie przycisk Usuń d przeprowadzana niektórych kod, aby usunąć wybrane produktu, a następnie ponownie powiązanie danych do elementu powtarzanego. Jeśli szczegóły sortowania nie są zachowywane między ogłaszania zwrotnego, dane wyświetlane na ekranie zostaną przywrócone do oryginalnego kolejności sortowania.

W tym samouczku DropDownList niejawnie zapisuje sortowanie wyrażenie i kierunek w swój stan widoku firmie Microsoft. Firma Microsoft używania innego interfejsu sortowania, jednego z LinkButtons powiedzieć, że różne opcje sortowania d musimy zajmie się do zapamiętania kolejność sortowania między ogłaszania zwrotnego. Można to osiągnąć, przechowując sortowania parametrów w stanie widoku strony s, dołączając parametr sortowania w ciąg zapytania lub za pośrednictwem niektórych innych technik trwałości stanu.

Przyszłe przykłady w tym samouczku Poznaj jak zachować szczegóły sortowania w stanie widoku strony s.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Krok 5: Dodawanie sortowanie pomocy technicznej do DataList, używającej domyślnej stronicowania

W [poprzedniego samouczek](paging-report-data-in-a-datalist-or-repeater-control-vb.md) możemy zbadać implementowania stronicowania domyślne z elementu DataList. Let s rozszerzenia tego przykładu poprzedniej do obejmują możliwość posortuj dane stronicowanej. Uruchamianie przez otwarcie `SortingWithDefaultPaging.aspx` i `Paging.aspx` stron w `PagingSortingDataListRepeater` folderu. Z `Paging.aspx` strony, kliknij przycisk źródło, aby wyświetlić s deklaratywne znaczników. Kopiowanie zaznaczonego tekstu (patrz rysunek 8) i wklej go w znaczniku deklaratywne `SortingWithDefaultPaging.aspx` między `<asp:Content>` tagów.


[![Replikowanie deklaratywne znaczników w &lt;asp: Content&gt; tagów z Paging.aspx SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**Rysunek 8**: replikowanie deklaratywne znaczników w `<asp:Content>` znaczniki z `Paging.aspx` do `SortingWithDefaultPaging.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))


Po skopiowaniu deklaratywne znaczników, skopiuj metody i właściwości w `Paging.aspx` strony s klasie związanej z kodem klasę CodeBehind dla `SortingWithDefaultPaging.aspx`. Następnie Poświęć chwilę, aby wyświetlić `SortingWithDefaultPaging.aspx` strony w przeglądarce. Powinien mieć, te same funkcje i wyglądu jako `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Udoskonalanie ProductsBLL uwzględnienie domyślne stronicowania i sortowania — metoda

W poprzednich instrukcji utworzyliśmy `GetProductsAsPagedDataSource(pageIndex, pageSize)` metody w `ProductsBLL` klasy zwróconą `PagedDataSource` obiektu. To `PagedDataSource` obiekt został wypełniony *wszystkie* produktów (za pośrednictwem s logiki warstwy Biznesowej `GetProducts()` — metoda), ale podczas wiązania z elementu DataList tylko te rekordy, które są odpowiednie do określonego *pageIndex* i *pageSize* parametry wejściowe były wyświetlane.

Wcześniej w tym samouczku Dodaliśmy obsługę sortowania, określając wyrażenia sortowania z ObjectDataSource s `Selecting` obsługi zdarzeń. Działa to również po elemencie ObjectDataSource jest zwracany obiekt, który można sortować, takie jak `ProductsDataTable` zwrócony przez `GetProducts()` metody. Jednak `PagedDataSource` obiektu zwróconego przez `GetProductsAsPagedDataSource` metoda nie obsługuje sortowania źródła danych wewnętrznych. Zamiast tego należy sortować wyniki zwrócone z `GetProducts()` metody *przed* testujemy `PagedDataSource`.

W tym celu utworzenie nowej metody w `ProductsBLL` klasy `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Aby posortować `ProductsDataTable` zwrócony przez `GetProducts()` metody, określ `Sort` właściwości domyślnej `DataTableView`:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

`GetProductsSortedAsPagedDataSource` Metoda różni się tylko z `GetProductsAsPagedDataSource` metody utworzonej w poprzednim samouczku. W szczególności `GetProductsSortedAsPagedDataSource` akceptuje dodatkowy parametr wejściowy `sortExpression` i przypisuje wartość do `Sort` właściwość `ProductDataTable` s `DefaultView`. Kilka wierszy kodu później, `PagedDataSource` obiekt s źródła danych jest przypisany `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Wywołanie metody GetProductsSortedAsPagedDataSource i określania wartości dla parametru SortExpression dane wejściowe

Z `GetProductsSortedAsPagedDataSource` metody ukończenia, następnym krokiem jest Podaj wartość dla tego parametru. Element ObjectDataSource w `SortingWithDefaultPaging.aspx` jest obecnie skonfigurowany do wywołania `GetProductsAsPagedDataSource` — metoda i przebiegów w dwóch parametrów wejściowych za pośrednictwem jego dwa `QueryStringParameters`, które zostały określone w `SelectParameters` kolekcji. Te dwa `QueryStringParameters` wskazują, że źródło `GetProductsAsPagedDataSource` metody s *pageIndex* i *pageSize* parametry pochodzą z pola querystring `pageIndex` i `pageSize`.

Aktualizuj ObjectDataSource s `SelectMethod` właściwości, tak że wywołuje nowe `GetProductsSortedAsPagedDataSource` — metoda. Następnie dodaj nową `QueryStringParameter` , aby *sortExpression* parametru wejściowego jest dostępny w polu querystring `sortExpression`. Ustaw `QueryStringParameter` s `DefaultValue` do ProductName.

Po wprowadzeniu tych zmian znaczników deklaratywne s ObjectDataSource powinien wyglądać następująco:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

W tym momencie `SortingWithDefaultPaging.aspx` strony będzie alfabetycznie Sortuj wyniki według nazwy produktu (patrz rysunek 9). Wynika to z faktu, domyślnie wartość ProductName jest przekazywany jako `GetProductsSortedAsPagedDataSource` metody s *sortExpression* parametru.


[![Domyślnie wyniki są sortowane według ProductName](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**Rysunek 9**: Domyślnie wyniki są sortowane według `ProductName` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))


Jeśli ręcznie dodać `sortExpression` pole querystring, takich jak `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` wyniki będą sortowane przez określony `sortExpression`. Jednak to `sortExpression` parametru nie wchodzi w zmiennej querystring podczas przenoszenia do innej strony danych. W rzeczywistości, klikając przycisk Dalej lub ostatniej strony przyciski przyjmuje nam z powrotem do `Paging.aspx`! Ponadto s występują obecnie sortowania nie interfejs. Jedynym sposobem, użytkownik może zmienić kolejność sortowania danych stronicowanej jest bezpośrednie manipulowanie ciąg zapytania.

## <a name="creating-the-sorting-interface"></a>Tworzenie interfejsu sortowania

Najpierw należy zaktualizować `RedirectUser` metodę można wysłać użytkownikowi `SortingWithDefaultPaging.aspx` (zamiast `Paging.aspx`) i dołączyć `sortExpression` wartość w zmiennej querystring. Tylko do odczytu na poziomie strony o nazwie należy również dodać `SortExpression` właściwości. Ta właściwość jest podobny do `PageIndex` i `PageSize` właściwości utworzonej w poprzednim samouczku, zwraca wartość `sortExpression` pole querystring, jeśli istnieje oraz domyślną wartość (ProductName) w przeciwnym razie wartość.

Obecnie `RedirectUser` metoda przyjmuje tylko jeden parametr wejściowy indeks strony do wyświetlenia. Jednak może być razy, gdy chcemy, aby przekierować użytkownika do określonej strony danych przy użyciu wyrażenia sortowania niż s, jakie określone w ciąg zapytania. Za chwilę utworzymy interfejsie sortowania dla tej strony, które zawiera szereg formantów sieci Web przycisk sortowania danych w określonej kolumnie. Po kliknięciu jednego z tych przycisków, chcemy użytkownika, przekazując wartość wyrażenia sortowania odpowiedniej przekierowania. Do tej funkcji, należy utworzyć dwie wersje `RedirectUser` metody. Pierwsza z nich powinna obsługiwać tylko indeks strony do wyświetlenia, gdy drugi akceptuje wyrażenie indeksu i sortowanie strony.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

W pierwszym przykładzie w tym samouczku utworzyliśmy sortowania interfejs, za pomocą DropDownList. W tym przykładzie umożliwiają s, formanty trzy przycisk sieci Web znajduje się powyżej jednego elementu DataList sortowania przez `ProductName`, jeden dla `CategoryName`i jedną dla `SupplierName`. Dodawanie formantów sieci Web przycisk trzy, ustawienie ich `ID` i `Text` właściwości odpowiednio:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

Następnie należy utworzyć `Click` obsługi zdarzeń dla każdego. Programy obsługi zdarzeń powinny wywoływać `RedirectUser` metody zwracanie do pierwszej strony przy użyciu wyrażenia sortowania odpowiedniego użytkownika.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Podczas odwiedzania najpierw strony, dane są sortowane według nazwy produktu alfabetycznie (odwołują się do na rysunku nr 9). Kliknij przycisk Dalej, aby przejść do drugiej strony danych, a następnie kliknij przycisk sortowanie według kategorii przycisku. To polecenie zwróci nam do pierwszej strony danych sortowane według nazwy kategorii (zobacz rysunek 10). Podobnie klikając sortowanie przez dostawcę przycisk dane są sortowane przez dostawcę, zaczynając od pierwszej strony danych. Opcja sortowania jest zapamiętany jako danych jest stronicowana za pośrednictwem. Rysunek 11 wyświetla stronę po sortowanie według kategorii, a następnie przejścia do 13 strony danych.


[![Te produkty są sortowane według kategorii](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**Na rysunku nr 10**: produkty są sortowane według kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))


[![Wyrażenie sortowania jest zapamiętanych podczas stronicowania za pośrednictwem danych](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**Rysunek 11**: wyrażenie sortowania są zapamiętywane podczas stronicowania za pośrednictwem dane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Krok 6: Stronicowania niestandardowego rekordów w powtarzanym

W przykładzie DataList zbadać w kroku 5 stron za pośrednictwem jego danych przy użyciu techniki stronicowania nieefektywne domyślne. Gdy stronicowanie za pośrednictwem wystarczająco duże ilości danych, konieczne jest stosowanie stronicowania niestandardowego. W [wydajnie stronicowania za pośrednictwem dużych ilości danych](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) i [sortowanie danych niestandardowych stronicowanej](../paging-and-sorting/sorting-custom-paged-data-vb.md) samouczki, możemy zbadać różnice między domyślnymi i stronicowania niestandardowego i metod utworzony w logiki warstwy Biznesowej dla przy użyciu niestandardowych stronicowania i sortowania danych niestandardowych stronicowanej. W szczególności w tych samouczkach poprzednich dwóch dodaliśmy następujące trzy metody `ProductsBLL` klasy:

- `GetProductsPaged(startRowIndex, maximumRows)`Zwraca określony podzbiór rekordów, zaczynając od *startRowIndex* i nie przekracza *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)`Zwraca określony podzbiór rekordów posortowane według określonego *sortExpression* parametru wejściowego.
- `TotalNumberOfProducts()`Całkowita liczba rekordów w zawiera `Products` tabeli bazy danych.

Te metody mogą służyć do strony i przeszukiwaniem danych przy użyciu formant DataList lub elementu powtarzanego. Na przykład zezwolić s, Rozpocznij od utworzenia kontrolce elementu powtarzanego z obsługę stronicowania niestandardowego; następnie dodamy funkcje sortowania.

Otwórz `SortingWithCustomPaging.aspx` strony `PagingSortingDataListRepeater` folderu i dodać elementu powtarzanego do strony, ustawiając jego `ID` właściwości `Products`. W tagu elementu powtarzanego s, utworzyć nowy element ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj go, aby wybrać dane z `ProductsBLL` klasy s `GetProductsPaged` metody.


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetProductsPaged klasy ProductsBLL s](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**Rysunek 12**: Konfigurowanie ObjectDataSource użyć `ProductsBLL` klasy s `GetProductsPaged` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))


Ustawianie list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak), a następnie kliknij przycisk Dalej. Teraz monituje Kreator konfigurowania źródła danych dla źródła `GetProductsPaged` metody s *startRowIndex* i *maximumRows* parametrów wejściowych. W rzeczywistości te parametry wejściowe są ignorowane. Zamiast tego *startRowIndex* i *maximumRows* wartości zostaną przekazane w za pomocą `Arguments` właściwości w elemencie ObjectDataSource s `Selecting` program obsługi zdarzeń, podobnie jak jak możemy określony *sortExpression* z tego pokazu pierwszy samouczek s. W związku z tym należy pozostawić źródło parametru list rozwijanych w kreatorze na None.


[![Pozostaw zestaw źródeł parametrów None](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**Rysunek 13**: pozostaw źródeł parametr wartość None ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))


> [!NOTE]
> Czy *nie* zestawu ObjectDataSource s `EnablePaging` właściwości `true`. To spowoduje, że element ObjectDataSource uwzględnienie automatycznie własną *startRowIndex* i *maximumRows* parametry `SelectMethod` s istniejącej listy parametrów. `EnablePaging` Właściwość jest przydatna, gdy niestandardowe powiązanie stronicowanej danych widoku GridView, widoku DetailsView lub FormView formantu, ponieważ tych kontrolek spodziewać się pewnych zachowania ObjectDataSource tego s dostępna tylko wtedy, gdy `EnablePaging` jest właściwość `true`. Ponieważ mamy ręcznie dodać obsługę stronicowania DataList i powtarzanego, pozostaw tę właściwość, ustaw `false` (ustawienie domyślne), jak firma Microsoft będzie wypieków (kwesta) w funkcje bezpośrednio z poziomu strony ASP.NET.


Na koniec, zdefiniuj powtarzanego s `ItemTemplate` tak, aby nazwa produktu s, kategorii i dostawcy. Po wprowadzeniu tych zmian składni deklaratywnej s elementu powtarzanego i ObjectDataSource powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

Poświęć chwilę, aby odwiedzić stronę za pośrednictwem przeglądarki i należy pamiętać, że nie zwrócono żadnych rekordów. Jest to spowodowane firma Microsoft kolejnych jeszcze, aby określić *startRowIndex* i *maximumRows* wartości parametrów; w związku z tym wartości 0 jest przekazywany w obu. Aby określić te wartości, należy utworzyć programu obsługi zdarzeń dla elementu ObjectDataSource s `Selecting` zdarzeń i ustaw następujące parametry wartości programowo do zakodowanych wartości od 0 do 5, odpowiednio:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

Dzięki tej zmianie stronie podczas przeglądania za pośrednictwem przeglądarki, pokazuje pierwsze pięć produktów.


[![Pierwsze pięć rekordy są wyświetlane.](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**Rysunek 14**: pierwszy pięć rekordy są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))


> [!NOTE]
> Produkty wymienione na rysunku 14 stanie się można sortować według nazwy produktu, ponieważ `GetProductsPaged` procedury przechowywanej, która wykonuje efektywnej kwerendy stronicowania niestandardowego porządkuje wyniki według `ProductName`.


Aby zezwolić użytkownikowi krok na stronach, musimy śledzić indeks wiersza początkowego i maksymalna liczba wierszy i Pamiętaj te wartości między ogłaszania zwrotnego. W przykładzie stronicowania domyślne użyliśmy querystring pola, aby zachować te wartości. dla tego pokazu pozwolić, aby zachować te informacje w stanie widoku strony s s. Utwórz następujące dwie właściwości:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

Następnie zaktualizuj kod obsługi zdarzenia Selecting, tak aby były używane `StartRowIndex` i `MaximumRows` właściwości zamiast do zakodowanych wartości od 0 do 5:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

W tym momencie naszą stronę nadal zawiera tylko pierwsze pięć rekordy. Jednak z tymi właściwościami w miejscu, możemy re gotowe do tworzenia interfejsu naszych stronicowania.

## <a name="adding-the-paging-interface"></a>Dodawanie interfejsu stronicowania

Let s za pomocą tego samego pierwszej, Previous, dalej stronicowania ostatni interfejs używany w przykładzie stronicowania domyślne etykiety sieci Web jest wyświetlany formant, który wyświetla jakie strony danych w tym i istnieje liczbę całkowita liczba stron. Dodaj cztery kontrolki przycisku w sieci Web i etykiety poniżej elementu powtarzanego.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

Następnie należy utworzyć `Click` programy obsługi zdarzeń dla czterech przycisków. Po kliknięciu jednego z tych przycisków, należy zaktualizować `StartRowIndex` i ponownie powiązać dane do elementu powtarzanego. Kod dla przycisków pierwszej, Wstecz i dalej jest prosta, ale dla przycisku ostatniej jak możemy ustalić indeks wiersza początkowego dla ostatniej strony danych? Do obliczenia tego indeksu, a także możliwość określenia, czy powinno być włączone przycisków Następny i ostatni musimy wiedzieć, ile rekordy w sumie są trwa stronicowanej za pośrednictwem. Można określić, wywołując `ProductsBLL` klasy s `TotalNumberOfProducts()` metody. S umożliwiają tworzenie tylko do odczytu, na poziomie strony właściwości o nazwie `TotalRowCount` które zwraca wyniki `TotalNumberOfProducts()` metody:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

Ta właściwość teraz określić ostatni indeks wiersza początkowego strony s. W szczególności jego s wynik liczby całkowitej z `TotalRowCount` minus wynik dzielenia 1 przez `MaximumRows`, pomnożonych przez `MaximumRows`. Firma Microsoft może teraz zapisać `Click` programy obsługi zdarzeń dla czterech przycisków interfejsu stronicowania:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

Na koniec należy wyłączyć pierwszy i poprzedni przycisków w interfejsie stronicowania podczas wyświetlania na pierwszej stronie danych i przycisków Następny i ostatni podczas wyświetlania na ostatniej stronie. Aby to zrobić, Dodaj następujący kod do ObjectDataSource s `Selecting` obsługi zdarzeń:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

Po dodaniu tych `Click` procedury obsługi zdarzeń i kod, aby włączyć lub wyłączyć elementów interfejsu stronicowania oparte na indeks bieżącego wiersza początkowego testów strony w przeglądarce. Rysunek 15 przedstawiono najpierw odwiedzania strony pierwszy i będzie przyciski Wstecz są wyłączone. Kliknięcie przycisku Dalej pokazuje na drugiej stronie danych, a kliknięcie przycisku ostatniej wyświetli ostatniej strony (patrz rysunki 16 i 17). Podczas przeglądania ostatnich strony danych przycisków Następny i w ostatniej są wyłączone.


[![Wstecz i przyciski ostatniego są wyłączone podczas wyświetlania strony pierwszy produktów](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**Rysunek 15**: ostatni przyciski i Previous są wyłączone podczas wyświetlania strony pierwszy produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))


[![Druga strona produkty są Dispalyed](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**Rysunek 16**: drugiej strony produktów są Dispalyed ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))


[![Wyświetla ostatniego kliknięcie przycisku ostatniej strony danych](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**Rysunek 17**: kliknięcie przycisku ostatniej strony końcowego dane zostaną wyświetlone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Krok 7: W tym sortowanie pomocy technicznej z niestandardowym stronicowanej elementu powtarzanego

Teraz, gdy została ona zaimplementowana stronicowania niestandardowego, re gotowe do dołączenia sortowanie obsługujemy. `ProductsBLL` Klasy s `GetProductsPagedAndSorted` metoda ma taką samą *startRowIndex* i *maximumRows* parametrów jako wejściowych `GetProductsPaged`, ale zezwala na dodatkowe  *sortExpression* parametru wejściowego. Aby użyć `GetProductsPagedAndSorted` metody z `SortingWithCustomPaging.aspx`, należy wykonać następujące czynności:

1. Zmień element ObjectDataSource s `SelectMethod` właściwość z `GetProductsPaged` do `GetProductsPagedAndSorted`.
2. Dodaj *sortExpression* `Parameter` obiektu s ObjectDataSource `SelectParameters` kolekcji.
3. Utwórz prywatne, na poziomie strony `SortExpression` właściwość, która będzie się powtarzał wartość między ogłaszania zwrotnego do stanu widoku strony s.
4. Aktualizuj ObjectDataSource s `Selecting` obsługi zdarzeń, aby przypisać ObjectDataSource s *sortExpression* parametru wartość poziomu strony `SortExpression` właściwości.
5. Utwórz interfejs sortowania.

Uruchom aktualizując ObjectDataSource s `SelectMethod` właściwości i dodać *sortExpression* `Parameter`. Upewnij się, że *sortExpression* `Parameter` s `Type` właściwość jest ustawiona na `String`. Po zakończeniu tych dwóch pierwszych zadań, znaczników deklaratywne s ObjectDataSource powinien wyglądać następująco:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

Następnie należy poziomu strony `SortExpression` właściwości, której wartość jest serializowany do stanu widoku. Jeśli nie ustawiono wartości wyrażenia sortowania, użyj ProductName jako domyślne:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

Przed wywołuje element ObjectDataSource `GetProductsPagedAndSorted` metody należy ustawić *sortExpression* `Parameter` wartość `SortExpression` właściwości. W `Selecting` program obsługi zdarzeń, Dodaj następujący wiersz kodu:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

Wszystkie opcje, które pozostaje jest zaimplementowanie interfejsu sortowania. Jak robiliśmy w ostatnim przykładzie umożliwiają s ma sortowania interfejs implementowany przy użyciu trzech kontrolki przycisku sieci Web, które umożliwiają użytkownikom sortowania wyników według nazwy produktu, kategorii lub dostawcy.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

Utwórz `Click` programy obsługi zdarzeń dla tych trzech kontrolek przycisku. W przypadku resetowania w przypadku obsługi `StartRowIndex` 0, należy ustawić `SortExpression` odpowiednią wartość i ponownie Utwórz wiązanie danych do powtarzanego:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

Wszystkie dostępne tego s jest! Gdy wystąpiły liczbę czynności wymagane do zainstalowania niestandardowych stronicowania i sortowania zaimplementowana, kroki były bardzo podobne do potrzebnych do domyślnego stronicowania. Rysunek 18 zawiera produkty, podczas wyświetlania na ostatniej stronie danych podczas sortowania według kategorii.


[![Ostatnia strona danych, Sorted według kategorii, jest wyświetlana](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**Rysunek 18**: ostatniej strony z danymi, Sorted według kategorii, zostanie wyświetlony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))


> [!NOTE]
> W poprzednich przykładach, podczas sortowania przez dostawcę, który NazwaDostawcy została użyta jako wyrażenie sortowania. Jednak dla niestandardowych implementacji stronicowania, należy użyć NazwaFirmy. Jest to spowodowane procedury składowanej odpowiedzialnych za wdrażanie stronicowania niestandardowego `GetProductsPagedAndSorted` przekazuje wyrażenia sortowania w `ROW_NUMBER()` — słowo kluczowe, `ROW_NUMBER()` — słowo kluczowe wymaga nazwy rzeczywiste kolumny, a nie jej alias. W związku z tym musi używamy `CompanyName` (nazwa kolumny w `Suppliers` tabeli) zamiast alias użyty w `SELECT` kwerendy (`SupplierName`) dla wyrażenia sortowania.


## <a name="summary"></a>Podsumowanie

Ani DataList ani elementu powtarzanego oferują wbudowana obsługa sortowania, ale z bitowego kodu i niestandardowy interfejs sortowania, można dodać tych funkcji. Podczas implementowania sortowanie, ale nie stronicowania, wyrażenie sortowania można określić za pomocą `DataSourceSelectArguments` obiekt przekazany do elementu ObjectDataSource s `Select` metody. To `DataSourceSelectArguments` obiektu s `SortExpression` można przypisać właściwości w elemencie ObjectDataSource s `Selecting` program obsługi zdarzeń.

Aby dodać funkcje sortowania DataList lub elementu powtarzanego już zapewniający obsługę stronicowania, najprostszym rozwiązaniem jest dostosować warstwę logiki biznesowej, aby uwzględnić metody, która akceptuje wyrażenie sortowania. Informacje te można następnie przekazać w za pomocą parametru w elemencie ObjectDataSource s `SelectParameters`.

W tym samouczku wykonuje naszych badania stronicowania i sortowania z formant DataList i elementu powtarzanego. Z naszym samouczkiem dalej i końcowych zbada sposób dodawania kontrolki przycisku w sieci Web do szablonów s DataList i powtarzanego zapewnić niektóre funkcje niestandardowe, inicjowane przez użytkownika na podstawie elementu.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Suru Dominika. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
