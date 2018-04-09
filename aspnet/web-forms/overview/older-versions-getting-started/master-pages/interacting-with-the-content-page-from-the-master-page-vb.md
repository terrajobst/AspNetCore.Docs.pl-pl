---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: Interakcji ze strony zawartość z strony wzorcowej (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Sprawdza, czy sposób wywołania metody, ustaw właściwości, etc. zawartości strony z kodu na stronie wzorcowej.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 9274924b441cb21e33eb57de06ff374428fa036b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="interacting-with-the-content-page-from-the-master-page-vb"></a>Interakcji ze strony zawartość z strony wzorcowej (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)

> Sprawdza, czy sposób wywołania metody, ustaw właściwości, etc. zawartości strony z kodu na stronie wzorcowej.


## <a name="introduction"></a>Wprowadzenie

Samouczek poprzedniego zbadać sposobu ustawiania strony zawartości programowo interakcji z jego strony wzorcowej. Odwołania, że Zaktualizowaliśmy stronę wzorcową do uwzględnienia w kontrolce GridView wymienionego pięć ostatnio dodane produktów. Następnie utworzyliśmy strony zawartości, w którym użytkownik może dodać nowego produktu. Po dodaniu nowego produktu, strony zawartości potrzebne, aby poinstruować strony wzorcowej, aby odświeżyć jej GridView tak, aby go to po prostu dodane produktu. Ta funkcja została realizowane przez dodanie publicznej metody strony wzorcowej, aby odświeżyć dane powiązane widoku GridView, a następnie wywołania tej metody na stronie zawartości.

Najczęściej zawartości i interakcji z strony wzorcowej pochodzi ze strony zawartość. Jednak jest możliwe w dla strony wzorcowej do rouse bieżącej strony zawartości do akcji, a tych funkcji mogą być wymagane, jeśli strony wzorcowej zawiera elementy interfejsu użytkownika, które umożliwiają użytkownikom modyfikowanie danych wyświetlanych na stronie zawartości. Należy wziąć pod uwagę strony zawartości, że wyświetla informacji o produktach, w widoku GridView control i strony głównej, która zawiera przycisk kontrolować, po kliknięciu, podwaja ceny wszystkich produktów. Podobnie jak w przykładzie w poprzednim samouczek widoku GridView konieczne jest odświeżenie po Podwójna cena, który przycisk zostanie kliknięty, tak aby wyświetlone nowe ceny, ale w tym scenariuszu jest strony wzorcowej wymagającym rouse strony zawartości do akcji.

W tym samouczku Eksploruje sposobu ustawiania wywołania funkcji zdefiniowane na stronie zawartości strony wzorcowej.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Podżeganie programowe interakcji za pomocą zdarzeń i procedury obsługi zdarzeń

Wywoływanie funkcji strona zawartości ze strony wzorcowej jest trudniejsze niż odwrotnie. Ponieważ zawartość strony ma jednej strony wzorcowej, gdy podżeganie programowe interakcji ze strony zawartość wiemy publicznej metody i właściwości są nasze dyspozycji. Strony wzorcowej, jednak może mieć wiele różnych zawartości stron, a każda z zestawem właściwości i metod. Jak następnie możemy pisać kod na stronie głównej na wykonanie akcji na stronie jej zawartości, jeśli nie wiadomo, jaki strony zawartość zostanie wywołany, aż do środowiska wykonawczego?

Należy wziąć pod uwagę formant sieci Web ASP.NET, takich jak formantu przycisku. Kontrolka przycisku może występować na dowolnej liczbie stron ASP.NET i musi mechanizm, za pomocą której go może generować alerty strony został kliknięty. Jest to realizowane przy użyciu *zdarzenia*. W szczególności zgłasza formantu przycisku jego `Click` zdarzenie po kliknięciu; strony ASP.NET, która zawiera przycisk opcjonalnie może odpowiadać na powiadomienia za pośrednictwem *obsługi zdarzeń*.

Tego samego wzorca może służyć do ma funkcje wyzwalacza strony wzorcowej w jej zawartości strony:

1. Dodawanie zdarzenia strony wzorcowej.
2. Wywołaj zdarzenie, gdy strony wzorcowej musi łączyć się z jego zawartości strony. Na przykład jeśli strony wzorcowej musi alertów jego strony zawartości, czy użytkownik ma podwójny cen, jej zdarzenia będą zgłaszane, natychmiast po ceny zostały podwójny.
3. Utwórz program obsługi zdarzeń w tych strony zawartości, które należy wykonać kilka akcji.

Ta pozostałej części tego samouczka implementację przykładzie opisano we wprowadzeniu; to znaczy strony zawartości, który zawiera listę produktów w bazie danych i strony głównej, która zawiera przycisk kontrolować dwukrotnie ceny.

## <a name="step-1-displaying-products-in-a-content-page"></a>Krok 1: Wyświetlanie produktów na stronie zawartości

Nasza pierwszy firma w kolejności polega na utworzeniu strony zawartości, który zawiera listę produktów z bazy danych Northwind. (Dodaliśmy bazy danych Northwind do projektu w poprzednim samouczka [ *interakcji ze stroną wzorcową ze strony zawartość*](interacting-with-the-master-page-from-the-content-page-vb.md).) Rozpocznij od dodania nowej strony ASP.NET do `~/Admin` folder o nazwie `Products.aspx`, wprowadzania się, że aby powiązać `Site.master` strony wzorcowej. Rysunek 1 Pokazuje Eksplorator rozwiązań po dodaniu tej strony w witrynie sieci Web.


[![Dodaj nową stronę ASP.NET do folderu Admin.](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)

**Rysunek 01**: Dodaj nową stronę ASP.NET do `Admin` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))


Odwołaj w [ *określenie tytułu, tagi Meta i inne nagłówków HTML na stronie wzorcowej* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) samouczek utworzyliśmy klasy niestandardowej strony podstawowej o nazwie `BasePage` generujący tytuł strony, jeśli nie jest jawnie ustawione. Przejdź do `Products.aspx` kodem strony klasy i została ona pochodzić od `BasePage` (a nie z `System.Web.UI.Page`).

Na koniec zaktualizuj `Web.sitemap` pliku, aby uwzględnić wpis dla tej lekcji. Dodaj następujący kod pod `<siteMapNode>` zawartości do lekcji interakcji strony wzorca:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

Dodanie tego `<siteMapNode>` element jest widoczny w wnioski liście (patrz rysunek 5).

Wróć do `Products.aspx`. W zawartości formantu `MainContent`, Dodaj formant widoku GridView i nadaj mu nazwę `ProductsGrid`. Powiązać widoku GridView formant SqlDataSource o nazwie `ProductsDataSource`.


[![Nowy formant SqlDataSource powiązać widoku GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)

**Rysunek 02**: powiązać widoku GridView nowy formant SqlDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))


Skonfiguruj kreatora, tak aby były używane z bazą danych Northwind. Jeśli działał samouczkiem poprzedniej, ma już ciąg połączenia o nazwie `NorthwindConnectionString` w `Web.config`. Wybierz z listy rozwijanej te parametry połączenia, jak pokazano na rysunku 3.


[![Skonfigurować SqlDataSource korzystanie z bazy danych Northwind](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)

**Rysunek 03**: skonfigurować SqlDataSource korzystanie z bazy danych Northwind ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))


Następnie określ kontroli źródła danych `SELECT` instrukcji przez wybranie tabeli Produkty z listy rozwijanej i zwracanie `ProductName` i `UnitPrice` kolumn (patrz rysunek 4). Kliknij przycisk Dalej, a następnie Zakończ, aby zakończyć pracę Kreatora konfigurowania źródła danych.


[![Zwraca ProductName i UnitPrice pola z tabeli Produkty](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)

**Rysunek 04**: zwraca `ProductName` i `UnitPrice` pola z `Products` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))


To wszystko jest do niego! Po zakończeniu działania kreatora programu Visual Studio dodaje dwa BoundFields do widoku GridView dublowanego dwa pola zwrócony przez formant SqlDataSource. Formanty widoku GridView i SqlDataSource znaczników jest zgodna. Rysunek 5. pokazuje wyniki podczas wyświetlania za pośrednictwem przeglądarki.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]


[![W widoku GridView jest wyświetlany każdego produktu i jego ceny](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)

**Rysunek 05**: każdego produktu i jego ceny znajduje się w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))


> [!NOTE]
> Możesz także oczyszczanie widoku GridView. Sugestie obejmują formatowania wyświetlanej wartości UnitPrice jako walutę i przy użyciu czcionki i kolory tła, aby poprawić wygląd siatki. Aby uzyskać więcej informacji na wyświetlanie i formatowania danych w programie ASP.NET odwoływać się do mojej [pracy z samouczkiem serii danych](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Krok 2: Dodawanie przycisku podwójne cen do strony wzorcowej

Nasze następne zadanie jest można dodać formantu przycisku sieci Web do wzorca strony, po kliknięciu będzie Podwójna cena wszystkich produktów w bazie danych. Otwórz `Site.master` strony wzorcowej i przeciągnij element Button z przybornika do projektanta umieszczenia go pod `RecentProductsDataSource` kontroli SqlDataSource dodaliśmy poprzedniej samouczka. Ustaw przycisku `ID` właściwości `DoublePrice` i jego `Text` dla właściwości "Double cen produktu".

Następnie dodaj formant SqlDataSource strony wzorcowej, nadając mu nazwę `DoublePricesDataSource`. SqlDataSource to będzie można używać do wykonywania `UPDATE` instrukcji podwojenia wszystkie ceny. W szczególności należy ustawić jego `ConnectionString` i `UpdateCommand` właściwości na ciąg połączenia i `UPDATE` instrukcji. Następnie należy wywołać ten formant SqlDataSource `Update` metody podczas `DoublePrice` kliknięciu przycisku. Aby ustawić `ConnectionString` i `UpdateCommand` właściwości, wybierz kontrolkę SqlDataSource, a następnie przejdź do okna właściwości. `ConnectionString` Właściwość zawiera listę tych parametrów połączenia przechowywanych w `Web.config` na liście rozwijanej wybierz `NorthwindConnectionString` opcji, jak pokazano na rysunku 6.


[![Skonfigurować SqlDataSource do użycia NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)

**Rysunek 06**: skonfigurować SqlDataSource użyć `NorthwindConnectionString` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))


Aby ustawić `UpdateCommand` właściwości, zlokalizować opcję UpdateQuery w oknie właściwości. Tej właściwości, gdy zaznaczone, wyświetli się przycisk z wielokropkiem; Kliknij ten przycisk, aby wyświetlić okno dialogowe Edytor poleceń i parametrów pokazano na rysunku 7. Wpisz następujące polecenie `UPDATE` instrukcji w oknie dialogowym tekstowym:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

Po wykonaniu tej instrukcji zostanie dwukrotnie `UnitPrice` wartość dla każdego rekordu w `Products` tabeli.


[![Ustaw właściwość elementu UpdateCommand w źródle SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)

**Rysunek 07**: Ustaw SqlDataSource `UpdateCommand` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))


Po ustawieniu właściwości, znaczników deklaratywne przycisk i SqlDataSource formantów powinien wyglądać podobny do następującego:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

Wszystkie opcje, które pozostaje jest wywołanie jego `Update` — metoda podczas `DoublePrice` przycisk zostanie kliknięty. Utwórz `Click` programu obsługi zdarzeń dla `DoublePrice` przycisk i Dodaj następujący kod:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

Aby przetestować tę funkcję, odwiedź stronę `~/Admin/Products.aspx` strony możemy utworzone w kroku 1 i kliknij przycisk "Double cen produktu". Kliknięcie przycisku powoduje odświeżenie strony i wykonuje `DoublePrice` przycisku `Click` program obsługi zdarzeń, podwajając ceny wszystkich produktów. Następnie ponownie realizacją strony i znaczników zwrócił i ponownie wyświetlić w przeglądarce. Na stronie zawartości widoku GridView jednak listy ceny tej samej, jako przed "Double cen produktu" przycisk został kliknięty. Jest to spowodowane stanu przechowywane w stan widoku, więc nie są ładowane na ogłaszania zwrotnego, chyba że poinstruowany miał danych wstępnie załadowane w widoku GridView. Jeśli można znaleźć w innej strony, a następnie wróć do `~/Admin/Products.aspx` strony zobaczysz ceny zaktualizowane.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Krok 3: Wywoływanie zdarzeń ceny połączeń po są podwójny.

Ponieważ w widoku GridView `~/Admin/Products.aspx` strony nie natychmiast odzwierciedla podwojenie cen, użytkownik understandably może założyć, że nie kliknij przycisk "Double cen produktu" lub że nie działa. Może próbują kliknięcie przycisku więcej kilka razy, podwajając ceny wielokrotnie. Aby naprawić to musimy mieć siatki w zawartości Strona wyświetlana nowych cen natychmiast po ich są podwójny.

Zgodnie z opisem we wcześniejszej części tego samouczka, należy wywołać zdarzenie na stronie głównej, gdy użytkownik kliknie `DoublePrice` przycisku. Zdarzenie jest sposobem powiadomienia inny zestaw innych klas (subskrybentów zdarzeń), które coś interesującego wystąpił dla jednej klasy (Wydawca zdarzeń). W tym przykładzie strony wzorcowej jest wydawca zdarzeń; zawartość tych stron, które o czasie `DoublePrice` przycisku są subskrybentów.

Klasa subskrybuje zdarzenia przez utworzenie *obsługi zdarzeń*, czyli metody, która jest wykonywana w odpowiedzi na zdarzenie jest wywoływane. Wydawca definiuje zdarzenia zgłasza on definiując *delegata zdarzenia*. Delegata zdarzenia określa, jakie parametrów wejściowych zaakceptować programu obsługi zdarzeń. W programie .NET Framework zdarzeń delegaty nie zwraca żadnej wartości i dwóch parametrów wejściowych zaakceptować:

- `Object`, Który identyfikuje źródło zdarzenia i
- Klasy pochodne `System.EventArgs`

Drugi parametr przekazany do programu obsługi zdarzeń może zawierać dodatkowe informacje o zdarzeniu. Podczas podstawowym `EventArgs` klasy nie przekazują informacje, .NET Framework zawiera szereg klas, które rozszerzają `EventArgs` i obejmować dodatkowe właściwości. Na przykład `CommandEventArgs` wystąpienie zostanie przekazany do procedury obsługi zdarzeń, które odpowiadają na `Command` zdarzeń i zawiera dwie właściwości informacyjną: `CommandArgument` i `CommandName`.

> [!NOTE]
> Aby uzyskać więcej informacji na temat tworzenia wywoływanie i obsługa zdarzeń, zobacz [zdarzenia i delegatów](https://msdn.microsoft.com/library/17sde2xt.aspx) i [delegatów zdarzeń w prosty angielskim](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Aby zdefiniować zdarzenia, użyj następującej składni:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

Ponieważ musimy alertów strony zawartości, gdy użytkownik kliknął przycisk `DoublePrice` przycisk i nie trzeba przekazać dodatkowe informacje, możemy użyć delegata zdarzenia `EventHandler`, który definiuje program obsługi zdarzeń, który akceptuje jako jego Parametr typu obiektu `System.EventArgs`. Aby utworzyć zdarzenia na stronie głównej, Dodaj następujący wiersz kodu do klasy związane z kodem strony wzorcowej:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

Powyższy kod dodaje zdarzenie publiczne strony wzorcowej o nazwie `PricesDoubled`. Teraz musisz zgłosić to zdarzenie po ceny zostały podwójny. Aby zgłosić zdarzenie, użyj następującej składni:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

Gdzie *nadawcy* i *eventArgs* wartości mają zostać przekazane do programu obsługi zdarzeń abonenta.

Aktualizacja `DoublePrice` `Click` obsługi zdarzeń z następującym kodem:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

Jak wcześniej `Click` obsługi zdarzeń, który rozpoczyna się od wywołania `DoublePricesDataSource` kontroli SqlDataSource `Update` metody podwojenia ceny wszystkich produktów. Po, że istnieją dwa dodatki do obsługi zdarzeń. Najpierw `RecentProducts` odświeżania danych w widoku GridView. Ten element GridView został dodany do strony głównej w poprzednim samouczek i wyświetla pięć produktów najczęściej ostatnio dodane. Musimy odświeżyć tej siatki, tak aby pokazywał just podwójny cen tych produktów pięć. Następujące, `PricesDoubled` zdarzenia. Odwołanie do samej strony wzorcowej (`Me`) są wysyłane do programu obsługi zdarzeń jako źródło zdarzeń i pustą `EventArgs` obiektu jest wysyłany jako argumenty zdarzenia.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Krok 4: Obsługa zdarzeń w zawartości strony

W tym momencie zgłasza strony wzorcowej jego `PricesDoubled` zdarzenia przy każdym `DoublePrice` Button — formant zostanie kliknięty. Jednak jest to tylko połowa bitwy — nadal należy do obsługi zdarzeń w subskrybenta. Ten proces obejmuje dwa kroki: tworzenie obsługi zdarzeń i dodawanie kod okablowania zdarzenia, aby po wywołaniu zdarzenia programu obsługi zdarzeń jest wykonywany.

Rozpocznij od utworzenia programu obsługi zdarzeń o nazwie `Master_PricesDoubled`. Ze względu na sposób zdefiniowanego `PricesDoubled` zdarzenia na stronie głównej dwóch parametrów wejściowych obsługi zdarzeń musi być typu `Object` i `EventArgs`odpowiednio. W wywołaniu procedury obsługi zdarzeń `ProductsGrid` w widoku GridView `DataBind` metodę, aby ponownie powiązać dane do siatki.


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

Kod obsługi zdarzeń jest pełny, ale jeszcze mamy się do strony wzorcowej okablować `PricesDoubled` zdarzenie, aby ten program obsługi zdarzeń. Subskrybent wiążący zdarzenia do obsługi zdarzeń za pomocą następującej składni:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

*Wydawca* jest odwołanie do obiektu, który oferuje zdarzenia *eventName*, i *methodName* to nazwa programu obsługi zdarzeń zdefiniowanych w subskrybenta.

Ten kod okablowania zdarzenia musi zostać wykonana na pierwszej wizyty strony i kolejne ogłaszania zwrotnego i powinny występować w momencie w cyklu życia strony poprzedzający podczas może zostać zgłoszone zdarzenie. Odpowiedni moment, aby dodać kod okablowania zdarzenia jest na etapie PreInit występuje bardzo wczesnym etapie cyklu życia strony.

Otwórz `~/Admin/Products.aspx` i Utwórz `Page_PreInit` obsługi zdarzeń:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

Aby ukończyć ten kod okablowania potrzebujemy programowe odwołanie do strony wzorcowej ze strony zawartość. Jak wspomniano w poprzedniej samouczek, istnieją to zrobić na dwa sposoby:

- Przez rzutowanie z typowaniem luźnym `Page.Master` właściwość do typu odpowiednie strony wzorcowej lub
- Dodając `@MasterType` dyrektywę `.aspx` strony, a następnie użyć jednoznacznie `Master` właściwości.

Użyjmy drugie podejście. Dodaj następujące `@MasterType` dyrektywy do górnej części strony deklaratywne znaczników:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

Następnie dodaj następujący kod okablowania zdarzeń w `Page_PreInit` obsługi zdarzeń:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

Z tego kodu w miejscu widoku GridView na stronie zawartości są odświeżane przy każdym `DoublePrice` kliknięciu przycisku.

Rysunki 8 do 9 zilustrowanie tego zachowania. Rysunek nr 8 przedstawia strony po odwiedzeniu najpierw. Należy pamiętać, że ceny wartości w obu `RecentProducts` GridView (w lewej kolumnie strony głównej) i `ProductsGrid` GridView (na stronie zawartości). Na rysunku nr 9 przedstawiono takie same ekranu natychmiast po `DoublePrice` przycisk został kliknięty. Jak widać, nowych cen natychmiast odzwierciedlone w obu GridViews.


[![Wartości początkowej cen](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)

**Rysunek 08**: początkowej wartości cen ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))


[![Ceny Just-Doubled są wyświetlane w GridViews](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)

**Rysunek 09**: The Just-Doubled ceny są wyświetlane w GridViews ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))


## <a name="summary"></a>Podsumowanie

W idealnym przypadku strony wzorcowej i na stronach zawartości są całkowicie niezależne od siebie i wymagają żaden poziom interakcji. Jednak jeśli strony wzorcowej lub zawartości Strona, wyświetlająca dane, które mogą być modyfikowane z strony wzorcowej lub strony zawartości, następnie konieczne może mieć strony wzorcowej alertów strony zawartości (lub odwrotnie a) modyfikacji danych tak, aby mógł zostać zaktualizowany ekran. W poprzednim samouczek widzieliśmy sposobu ustawiania strony zawartości programowo interakcji z jego strony wzorcowej; w tym samouczku analizujemy sposobu ustawiania inicjowania strony wzorcowej interakcji.

Podczas programowe interakcji między zawartości i strony wzorcowej można pochodzą z zawartości lub strony wzorcowej, wzorzec interakcji używany jest zależna od inicjowanie. Różnice są wynika z faktu, że zawartość strony ma jednej strony wzorcowej, ale strony wzorcowej może mieć wiele różnych stron zawartości. Zamiast bezpośrednio interakcji ze strony zawartości strony wzorcowej lepszym rozwiązaniem jest strony wzorcowej zgłosić zdarzenie, która sygnalizuje, że niektóre akcje, miało miejsce. Te strony zawartości, które interesujących akcji można utworzyć procedury obsługi zdarzeń.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Uzyskiwanie dostępu i aktualizowanie danych w programie ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Zdarzenia i Delegaty](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Przekazywanie informacji między zawartości i stron wzorcowych](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Praca z danymi w samouczkach ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora wielu książek ASP/ASP.NET i twórcę 4GuysFromRolla.com pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott jest osiągalny w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Suchi Banerjee. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](interacting-with-the-master-page-from-the-content-page-vb.md)
> [dalej](master-pages-and-asp-net-ajax-vb.md)
