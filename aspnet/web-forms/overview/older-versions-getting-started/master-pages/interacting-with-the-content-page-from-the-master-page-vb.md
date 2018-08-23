---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: Interakcja ze stroną zawartości z poziomu strony wzorcowej (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Zbadamy, jak wywoływać metody, ustaw właściwości, itd. z poziomu strony zawartości z poziomu kodu na stronie wzorcowej.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ace3873ecb525afcb64a0aa144742eab467f8f6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753976"
---
<a name="interacting-with-the-content-page-from-the-master-page-vb"></a>Interakcja ze stroną zawartości z poziomu strony wzorcowej (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)

> Zbadamy, jak wywoływać metody, ustaw właściwości, itd. z poziomu strony zawartości z poziomu kodu na stronie wzorcowej.


## <a name="introduction"></a>Wprowadzenie

Poprzedni Samouczek zbadane jak programowo współdziałać z jego strony wzorcowej strony zawartość. Wycofanie zaktualizowane strony wzorcowej do uwzględnienia w kontrolce GridView wymienionego pięć ostatnio dodane produktów. Następnie utworzyliśmy strony zawartości, w którym użytkownik może dodać nowego produktu. Po dodaniu nowego produktu, poziomu strony zawartości wymagane poinstruować strony wzorcowej, aby odświeżyć jego GridView, tak aby po prostu dodać produktu będzie zawierało. Ta funkcja było wykonywane przez dodanie publiczną metodę strony wzorcowej, że odświeżane dane powiązane z widoku GridView, a następnie wywołania tej metody z poziomu strony zawartości.

Najczęściej używany typ zawartości i interakcji z strony wzorcowej pochodzi ze strony zawartość. Jednak jest możliwe dla strony wzorcowej do rouse bieżącej strony zawartości w działanie i mogą być potrzebne takich funkcji, jeśli strona główna zawiera elementy interfejsu użytkownika, które umożliwiają użytkownikom modyfikowanie danych, który jest również wyświetlany na stronie zawartości. Należy wziąć pod uwagę strony zawartości, że wyświetla informacji o produktach w GridView control i stronę wzorcową, która zawiera przycisk kontrolkę, która, po kliknięciu, podwaja ceny wszystkich produktów. Podobnie jak w przykładzie w poprzednim samouczku widoku GridView potrzebuje zostanie odświeżona po Podwójna cena, który kliknięto przycisk Tak, aby wyświetlał nowe ceny, ale w tym scenariuszu jest stronę wzorcową, którą trzeba rouse strony zawartości w działanie.

W tym samouczku przedstawiono, jak wywoływać funkcje zdefiniowane w stronę zawartości strony wzorcowej.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Podżeganie interakcji programistycznych za pomocą zdarzenia i procedury obsługi zdarzeń

Wywoływanie funkcji strony zawartości strony wzorcowej jest trudniejsze niż odwrotnie. Ponieważ zawartość strony ma jednej strony wzorcowej, gdy podżeganie interakcji programistycznych z poziomu strony zawartości wiemy, właściwości i metody publiczne są nasze dyspozycji. Strony wzorcowej, jednak może mieć wiele różnych stronach zawartości, każdy z swój własny zestaw właściwości i metod. Jak to zrobić następnie napisać kod na stronie głównej na wykonanie akcji w jego zawartości strony, jeśli nie wiesz, jakie zawartość strony zostanie wywołany, aż do czasu?

Należy wziąć pod uwagę formantu sieci Web platformy ASP.NET, takich jak formant przycisku. Formant przycisku mogą być wyświetlane na dowolną liczbę stron ASP.NET i wymaga mechanizm, za pomocą którego może on informować strony została kliknięta. Jest to realizowane przy użyciu *zdarzenia*. W szczególności wywołuje kontrolki przycisku jego `Click` zdarzenie po kliknięciu; strony ASP.NET, która zawiera przycisk opcjonalnie mogą odpowiadać na powiadomienia za pośrednictwem *programu obsługi zdarzeń*.

Ten sam wzorzec może służyć do mają funkcji wyzwalacza strony wzorcowej w stronach zawartości:

1. Dodaj zdarzenie do strony wzorcowej.
2. Wywołaj zdarzenie zawsze wtedy, gdy musi komunikować się z jego strony zawartości strony wzorcowej. Na przykład jeśli strona wzorcowa wymaga do wyzwalania alertu jego strony zawartości, że użytkownik podwoiła ceny, jej zdarzenia będą zgłaszane, natychmiast po mają został podwojony ceny.
3. Utwórz procedurę obsługi zdarzeń w tych stron zawartości, które wymagają wykonania określonego działania.

Ta pozostałej części tego samouczka implementuje przykładzie opisano we wprowadzeniu; to znaczy strony zawartości, która zawiera listę produktów w bazie danych i stronę wzorcową, która zawiera przycisk kontrolować dwukrotnie ceny.

## <a name="step-1-displaying-products-in-a-content-page"></a>Krok 1: Wyświetlanie produktów w zawartości strony

Naszym pierwszym zadaniem biznesowym polega na utworzeniu strony zawartości, która zawiera listę produktów z bazy danych Northwind. (Dodaliśmy do projektu bazy danych Northwind w poprzednim samouczku [ *interakcja ze stroną wzorcową z poziomu strony zawartości*](interacting-with-the-master-page-from-the-content-page-vb.md).) Rozpocznij od dodania nowej strony programu ASP.NET do `~/Admin` folder o nazwie `Products.aspx`i powiązać `Site.master` strony wzorcowej. Rysunek 1 Pokazuje Eksplorator rozwiązań po tej strony została dodana do witryny sieci Web.


[![Dodawanie nowej strony programu ASP.NET do folderu administratora](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)

**Rysunek 01**: Dodawanie nowej strony programu ASP.NET do `Admin` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))


Pamiętaj, że w [ *Określanie tytułu, tagów Meta i innych nagłówków HTML na stronie wzorcowej* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) samouczek tworzenia niestandardowej strony podstawowej klasy o nazwie `BasePage` generujący tytuł strony, jeśli nie jest jawnie ustawione. Przejdź do `Products.aspx` kodem strony klasy i jest pochodną `BasePage` (zamiast z `System.Web.UI.Page`).

Na koniec zaktualizuj `Web.sitemap` plik, aby dołączyć wpis dla tej lekcji. Dodaj następujący kod pod `<siteMapNode>` zawartości do lekcji interakcji strony główne:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

Dodanie tego `<siteMapNode>` element jest widoczny w lekcji listy (zobacz rysunek 5).

Wróć do `Products.aspx`. W formancie zawartości dla `MainContent`, dodawanie kontrolki GridView i nadaj mu nazwę `ProductsGrid`. Powiąż widoku GridView z kontrolką SqlDataSource o nazwie `ProductsDataSource`.


[![Nowe kontrolki SqlDataSource powiązać widoku GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)

**Rysunek 02**: powiązywanie kontrolki GridView nowej kontrolki SqlDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))


Skonfiguruj kreatora, tak aby używał bazy danych Northwind. Jeśli pracy za pomocą poprzedniego samouczka, to konto powinno mieć już parametrów połączenia o nazwie `NorthwindConnectionString` w `Web.config`. Wybierz te parametry połączenia z listy rozwijanej, jak pokazano na rysunku 3.


[![Konfigurowanie SqlDataSource do korzystania z bazy danych Northwind](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)

**Rysunek 03**: Konfigurowanie SqlDataSource do korzystania z bazy danych Northwind ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))


Następnie określ kontroli źródła danych `SELECT` instrukcji przez wybranie tabeli Produkty z listy rozwijanej i zwracanie `ProductName` i `UnitPrice` kolumny (patrz rysunek 4). Kliknij przycisk Dalej, a następnie Zakończ, aby zakończyć działanie kreatora Konfigurowanie źródła danych.


[![Zwracanie ProductName i UnitPrice pola z tabeli Produkty](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)

**Rysunek 04**: zwraca `ProductName` i `UnitPrice` pola z `Products` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))


To wszystko jest do niego! Po zakończeniu pracy Kreatora programu Visual Studio dodaje dwa BoundFields GridView duplikatów dwa pola, które są zwracane przy użyciu kontrolki SqlDataSource. Poniżej kontrolki GridView i użyciu kontrolki SqlDataSource znaczników. Rysunek 5. pokazuje wyniki podczas wyświetlania za pośrednictwem przeglądarki.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]


[![Każdy produkt, a jego ceną znajduje się w widoku GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)

**Rysunek 05**: każdego produktu, a jego ceną znajduje się w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))


> [!NOTE]
> Możesz wyczyścić wyglądu kontrolki GridView. Kilka sugestii obejmują formatowanie wyświetlaną wartość UnitPrice jako walutę i przy użyciu czcionki i kolory tła, aby poprawić wygląd siatki. Aby uzyskać więcej informacji na temat wyświetlania i formatowanie danych w programie ASP.NET, zobacz mój [Praca z serii samouczków danych](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Krok 2: Dodawanie przycisku Double cen do strony wzorcowej

Naszym kolejnym krokiem jest, aby dodać kontrolkę przycisku w sieci Web do poziomu głównego strony, po kliknięciu zostanie double ceny wszystkich produktów w bazie danych. Otwórz `Site.master` strony wzorcowej, a następnie przeciągnij przycisk z przybornika w projektancie, umieszczając je pod `RecentProductsDataSource` kontrolki SqlDataSource dodane w poprzednim samouczku. Ustaw właściwość `ID` właściwości `DoublePrice` i jego `Text` właściwość "Double ceny produktów".

Następnie dodaj kontrolki SqlDataSource strony wzorcowej, nadając mu nazwę `DoublePricesDataSource`. Ta SqlDataSource będzie służyć do wykonywania `UPDATE` instrukcję, aby wszystkie ceny dwukrotnie. W szczególności należy ustawić jego `ConnectionString` i `UpdateCommand` właściwości na ciąg połączenia i `UPDATE` instrukcji. Następnie należy wywołać tej kontrolki SqlDataSource `Update` metody podczas `DoublePrice` przycisku. Aby ustawić `ConnectionString` i `UpdateCommand` właściwości, wybierz kontrolkę kontrolką SqlDataSource, a następnie przejdź do okna właściwości. `ConnectionString` Listy właściwości tych parametrów połączenia przechowywanych już w `Web.config` na liście rozwijanej; wybierz opcję `NorthwindConnectionString` opcji, jak pokazano na rysunku 6.


[![Konfigurowanie SqlDataSource używać NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)

**Rysunek 06**: Konfigurowanie SqlDataSource do użycia `NorthwindConnectionString` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))


Aby ustawić `UpdateCommand` właściwości, znajdź opcję UpdateQuery w oknie dialogowym właściwości. Tę właściwość, po wybraniu Wyświetla przycisk z wielokropkiem; Kliknij ten przycisk, aby wyświetlić okno dialogowe Edytor poleceń i parametrów pokazano na rysunku 7. Wpisz następujące polecenie `UPDATE` w polu tekstowym w oknie dialogowym instrukcji:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

Po wykonaniu tej instrukcji zostanie dwukrotnie `UnitPrice` wartość dla każdego rekordu w `Products` tabeli.


[![Ustaw właściwość elementu UpdateCommand SqlDataSource firmy](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)

**Rysunek 07**: Ustaw SqlDataSource `UpdateCommand` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))


Po ustawieniu tych właściwości, przycisk i użyciu kontrolki SqlDataSource kontrolkom oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

Pozostaje tylko do wywoływania jego `Update` metody podczas `DoublePrice` przycisku. Tworzenie `Click` program obsługi zdarzeń dla `DoublePrice` przycisku i Dodaj następujący kod:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

Aby przetestować tę funkcję, odwiedź stronę `~/Admin/Products.aspx` stronie mamy utworzony w kroku 1, a następnie kliknij przycisk "Double cen produktu". Kliknięcie przycisku powoduje odświeżenie strony i wykonuje `DoublePrice` przycisku `Click` program obsługi zdarzeń, podwajając ceny wszystkich produktów. Następnie ponownie wyrenderowaniu strony i znaczników jest zwracana i ponownie jest wyświetlany w przeglądarce. GridView na stronie zawartości, jednak listy ceny tej samej, jako przed "Double ceny produktów" został kliknięty przycisk. Jest tak, ponieważ jego stan, przechowywane w widoku stanu, dzięki czemu nie są ładowane na ogłaszania zwrotnego, o ile nie zdecyduje, w przeciwnym razie dane wstępnie załadowane w widoku GridView. Jeśli można znaleźć w innej strony, a następnie wróć do `~/Admin/Products.aspx` strony zobaczysz zaktualizowanych cenach.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Krok 3: Wywoływanie zdarzeń ceny połączeń podczas są podwoiła

Ponieważ GridView w `~/Admin/Products.aspx` strony nie odzwierciedla natychmiast podwojenie ceny, użytkownik może uznać understandably, czy klikaj przycisk "Double cen produktu" lub że nie działało. One może spróbuj kliknąć przycisk więcej kilka razy, ponownego podwojenie ceny. Aby rozwiązać ten musimy mieć siatki w zawartości nowych cen wyświetlona strona, natychmiast po, zostaną one podwojone.

Zgodnie z opisem we wcześniejszej części tego samouczka, należy wywołać zdarzenie, na stronie głównej, gdy użytkownik kliknie `DoublePrice` przycisku. Zdarzenie jest sposobem jedną klasę (Wydawca zdarzeń) na powiadomienie drugiego zestawu innych klas (subskrybentów zdarzeń), które coś interesującego wystąpił. W tym przykładzie strony wzorcowej jest wydawcą zdarzeń; zawartość tych stron, które są istotne podczas `DoublePrice` przycisku są subskrybentów.

Klasa subskrybuje zdarzenie, tworząc *programu obsługi zdarzeń*, czyli metody, która jest wykonywana w odpowiedzi na zdarzenie, które są zgłaszane. Wydawca definiuje zdarzenia wywołuje on definiując *delegata zdarzenia*. Delegat wydarzenia określa jakie parametry wejściowe procedury obsługi zdarzeń musi zaakceptować. W .NET Framework delegatów zdarzeń nie zwraca żadnej wartości i akceptuje dwa parametry wejściowe:

- `Object`, Który identyfikuje źródła zdarzeń i
- Klasy pochodzącej od `System.EventArgs`

Drugi parametr przekazany do procedury obsługi zdarzeń może zawierać dodatkowe informacje o zdarzeniu. Podczas gdy podstawowy `EventArgs` klasy nie przekazują wszelkie informacje, program .NET Framework zawiera szereg klas, które rozszerzają `EventArgs` i obejmują dodatkowe właściwości. Na przykład `CommandEventArgs` wystąpienia zostanie przekazany do procedury obsługi zdarzeń, które reagują na wystąpienie `Command` zdarzenia i zawiera dwie właściwości informacyjny: `CommandArgument` i `CommandName`.

> [!NOTE]
> Aby uzyskać więcej informacji na temat tworzenia wywoływania i obsługi zdarzeń, zobacz [zdarzenia i delegatów](https://msdn.microsoft.com/library/17sde2xt.aspx) i [delegatów zdarzeń w języku angielskim — proste](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Aby zdefiniować zdarzenie, użyj następującej składni:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

Ponieważ musimy alert strony zawartości, gdy użytkownik kliknął `DoublePrice` przycisk i nie trzeba przekazać dodatkowe informacje, abyśmy delegata zdarzenia `EventHandler`, definiujący program obsługi zdarzeń, która akceptuje jako jego sekunda Parametr obiektu typu `System.EventArgs`. Aby utworzyć zdarzenie na stronie głównej, należy dodać następujący wiersz kodu do strony wzorcowej osobna klasa kodu:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

Powyższy kod dodaje zdarzenie publiczne strony wzorcowej o nazwie `PricesDoubled`. Musimy teraz Zgłoś to zdarzenie po mają został podwojony ceny. Aby zgłosić zdarzenie, użyj następującej składni:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

Gdzie *nadawcy* i *eventArgs* wartości mają być przekazane do programu obsługi zdarzeń subskrybenta.

Aktualizacja `DoublePrice` `Click` programu obsługi zdarzeń z następującym kodem:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

Tak jak poprzednio `Click` programu obsługi zdarzeń, który rozpoczyna się przez wywołanie metody `DoublePricesDataSource` kontrolki SqlDataSource `Update` metoda dwukrotnie ceny wszystkich produktów. Po, że istnieją dwa dodatków do programu obsługi zdarzeń. Po pierwsze, `RecentProducts` GridView dane są odświeżane. Ta GridView został dodany do strony wzorcowej w poprzednim samouczku i przedstawia pięć najbardziej ostatnio dodane produkty. Musimy odświeżyć tę siatkę, tak aby pokazywał tylko podwoiła ceny tych pięciu produktach. Poniżej `PricesDoubled` zdarzenie jest wywoływane. Odwołanie do samej strony wzorcowej (`Me`) są wysyłane do programu obsługi zdarzeń jako źródło zdarzenia i pusta `EventArgs` obiektu jest wysyłany jako argumenty zdarzenia.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Krok 4: Obsługa zdarzeń w zawartości strony

W tym momencie strony wzorcowej wywołuje jego `PricesDoubled` zdarzenie zawsze wtedy, gdy `DoublePrice` kliknięciu formantu przycisku. Jednak jest to dopiero połowa gruntownie — wciąż potrzebujemy do obsługi zdarzeń w subskrybencie. Ten proces obejmuje dwa kroki: tworzenie obsługi zdarzeń i dodanie kodu okablowania zdarzenia, gdy zostanie wywołane zdarzenie obsługi zdarzenia jest wykonywany.

Rozpocznij od utworzenia programu obsługi zdarzeń o nazwie `Master_PricesDoubled`. Ze względu na sposób zdefiniowaliśmy `PricesDoubled` zdarzenie na stronie głównej dwóch parametrów wejściowych programu obsługi zdarzeń musi być typu `Object` i `EventArgs`, odpowiednio. W wywołaniu procedury obsługi zdarzeń `ProductsGrid` GridView `DataBind` metodę, aby ponownie powiązać dane do siatki.


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

Kod obsługi zdarzeń jest gotowy, ale zostały wykonane następujące kroki jeszcze na połączenie strony wzorcowej `PricesDoubled` zdarzenie, aby ten program obsługi zdarzeń. Subskrybent przewody zdarzenia do obsługi zdarzeń za pomocą następującej składni:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

*Wydawca* jest odwołaniem do obiektu, który oferuje zdarzenia *eventName*, i *methodName* jest nazwą programu obsługi zdarzeń zdefiniowanych w subskrybencie.

Ten kod okablowania zdarzenia musi zostać wykonane w pierwszej wizyty strony i kolejne ogłaszania zwrotnego i powinny być wykonywane w punkcie w cyklu życia strony, poprzedzającym podczas zdarzenia mogą być wywoływane. Jest odpowiedni moment, aby dodać kod okablowania zdarzenia w etapie PreInit występuje bardzo wczesnym etapie cyklu życia strony.

Otwórz `~/Admin/Products.aspx` i utworzyć `Page_PreInit` program obsługi zdarzeń:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

Aby można było ukończyć ten kod okablowania potrzebujemy programowe odwołanie ze stroną wzorcową z poziomu strony zawartości. Jak wspomniano w poprzednim samouczku, istnieją dwa sposoby wykonania tej czynności:

- Przez rzutowanie z typowaniem luźnym `Page.Master` właściwość do typu odpowiednie strony wzorcowej lub
- Dodając `@MasterType` dyrektywy w `.aspx` strony, a następnie używając, że wpisane pogrubieniem `Master` właściwości.

Użyjmy drugie podejście. Dodaj następujący kod `@MasterType` dyrektywę na początku oznaczeniu deklaracyjnym strony:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

Następnie dodaj następujący kod okablowania zdarzeń w `Page_PreInit` programu obsługi zdarzeń:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

Przy użyciu tego kodu w miejscu GridView na stronie zawartości są odświeżane przy każdym `DoublePrice` przycisku.

Rysunki 8 i 9 pokazują to zachowanie. Rysunek 8 przedstawia stronę po raz pierwszy odwiedzony. Należy zauważyć, że cena wartości w obu `RecentProducts` kontrolki GridView (w lewej kolumnie strony wzorcowej) i `ProductsGrid` kontrolki GridView (w stronę zawartości). Przedstawia rysunek 9 takie same ekranu natychmiast po `DoublePrice` został kliknięty przycisk. Jak widać, nowe ceny są natychmiast odzwierciedlane w obu GridViews.


[![Wartości początkowej cen](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)

**Rysunek 08**: początkowej wartości cen ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))


[![Ceny Just-Doubled są wyświetlane w GridViews](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)

**Rysunek 09**: The Just-Doubled ceny są wyświetlane w GridViews ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))


## <a name="summary"></a>Podsumowanie

W idealnym przypadku strony wzorcowej i na stronach zawartości są całkowicie niezależne od siebie nawzajem i wymagają żaden poziom interakcji. Jednak jeśli masz strony wzorcowej lub strony zawartości, która wyświetla dane, który może być modyfikowany ze strony głównej lub strony zawartości, następnie konieczne może być mieć stronę wzorcową alert strony zawartości (lub odwrotnie a) modyfikacji danych, dzięki czemu mogą być aktualizowane wyświetlania. W poprzednim samouczku widzieliśmy jak programowo współdziałać z jego strony głównej; strony zawartości w tym samouczku zobaczyliśmy, jak inicjowania strony wzorcowej interakcji.

Podczas interakcji programistycznych między zawartości i strony wzorcowej mogą pochodzić z zawartości lub strony wzorcowej, wzorzec interakcji, używany jest zależna od pochodzenia. Różnice są wynika z faktu, że zawartość strony ma jednej strony wzorcowej, ale strony wzorcowej może mieć wiele różnych stronach zawartości. Zamiast bezpośrednio współdziałać ze stroną zawartości strony wzorcowej, lepszym rozwiązaniem jest strony wzorcowej wywołać zdarzenie do sygnalizowania, że niektóre akcje miało miejsce. Tych stron zawartości, które interesują akcji można utworzyć procedury obsługi zdarzeń.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Uzyskiwanie dostępu do i aktualizowanie danych w programie ASP.NET:](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Zdarzenia i delegatów](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Przekazywanie informacji między zawartości i stronami wzorcowymi](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Praca z danymi w samouczki platformy ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu ASP/ASP.NET książki i założyciel 4GuysFromRolla.com pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Suchi Banerjee. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](interacting-with-the-master-page-from-the-content-page-vb.md)
> [dalej](master-pages-and-asp-net-ajax-vb.md)
