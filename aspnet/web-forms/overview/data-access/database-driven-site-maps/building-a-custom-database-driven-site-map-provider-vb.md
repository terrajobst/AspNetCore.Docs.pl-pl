---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
title: Tworzenie dostawcy mapy witryny opartego na bazie danych niestandardowych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Dostawcy mapy witryny domyślnej w programie ASP.NET 2.0 pobiera dane z pliku XML statycznego. Gdy dostawca oparty na składni XML będzie odpowiednia dla wielu małych, jak i średni rozmiar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: f904cd2c-a408-4484-9324-8b8d7fe33893
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
msc.type: authoredcontent
ms.openlocfilehash: 8e041a5a9163c7f9fe55c6aa06f35301cbdb48a8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393973"
---
<a name="building-a-custom-database-driven-site-map-provider-vb"></a>Tworzenie dostawcy mapy witryny opartego na bazie danych niestandardowych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_VB.zip) lub [Pobierz plik PDF](building-a-custom-database-driven-site-map-provider-vb/_static/datatutorial62vb1.pdf)

> Dostawcy mapy witryny domyślnej w programie ASP.NET 2.0 pobiera dane z pliku XML statycznego. Dostawca opartych na języku XML nadaje się do wielu witryn sieci Web małe i średnie, większej aplikacji sieci Web wymagają mapy witryny sieci Web bardziej dynamiczne. W tym samouczku utworzymy niestandardowego dostawcy mapy witryny, która pobiera dane z warstwy logiki biznesowej, która z kolei pobiera dane z bazy danych.


## <a name="introduction"></a>Wprowadzenie

Program ASP.NET 2.0 funkcji mapy witryny s temu deweloper strony do definiowania mapy witryny sieci web aplikacji s niektóre nośnika trwałego, takie jak w pliku XML. Po zdefiniowaniu dane mapy witryny są dostępne programowo za pośrednictwem [ `SiteMap` klasy](https://msdn.microsoft.com/library/system.web.sitemap.aspx) w [ `System.Web` przestrzeni nazw](https://msdn.microsoft.com/library/system.web.aspx) lub przy użyciu różnych nawigacji sieci Web formanty, takie jak Formanty SiteMapPath, Menu i widoku drzewa. System mapy witryny używa [modelu dostawca](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) tak, aby implementacje serializacji mapy innej lokacji można tworzyć i podłączony do aplikacji sieci web. Dostawcy mapy witryny domyślnej, który jest dostarczany z programem ASP.NET 2.0 są utrwalane struktury mapy witryny w pliku XML. Ponownie [strony wzorcowe i nawigacja w witrynie](../introduction/master-pages-and-site-navigation-vb.md) samouczku utworzyliśmy plik o nazwie `Web.sitemap` , zawarty tej struktury i jego XML został Trwa aktualizowanie za pomocą każdej nowej sekcji samouczka.

Dostawcy mapy witryny opartego na języku XML domyślne działa dobrze, jeśli struktura s mapy witryny jest stosunkowo statycznych, takich jak potrzeby tych samouczków. W wielu scenariuszach jednak bardziej dynamiczne mapy witryny jest wymagany. Należy wziąć pod uwagę Mapa witryny, przedstawionej na rysunku 1, w którym każdej kategorii i produktu są wyświetlane jako sekcje w strukturze s witryny sieci Web. Za pomocą tej mapy witryny odwiedzając stronę sieci web, odpowiadający węzeł główny może wyświetlić listę wszystkich kategorii, odwiedzając stronę sieci web określonej kategorii s zwróciłaby listę produktów s tej kategorii i wyświetlania strony sieci web określonym produktem s pokazywałaby produktu szczegóły s.


[![Kategorie i produktów w skład struktury s mapy witryny](building-a-custom-database-driven-site-map-provider-vb/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image1.png)

**Rysunek 1**: kategorie i produkty korzeń s mapy witryny struktury ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image2.png))


Ta struktura według kategorii i produktu może być ustalone w `Web.sitemap` pliku, plik musi być aktualizowane za każdym razem w kategorii lub dodane, usunięte lub zmieniono jego nazwę produktu. W związku z tym obsługa map lokacji będzie znacznie uprościć jeśli jego strukturę została pobrana z bazy danych, lub najlepiej, jeśli z warstwy logiki biznesowej aplikacji architektury s. W ten sposób, jak produkty i kategorie zostały dodane, zmieniono nazwę lub usunięto, Mapa witryny będzie automatycznie aktualizowana w celu odzwierciedlenia tych zmian.

Ponieważ serializacji mapy witryny s ASP.NET 2.0 jest kompilowanych na podstawie modelu dostawców, możemy utworzyć własną niestandardowego dostawcy mapy witryny, bierze jego dane z magazynu danych, takich jak bazy danych lub architektury. W tym samouczku utworzymy niestandardowego dostawcy, która pobiera dane z LOGIKI. Rozpocznij pracę dzięki s!

> [!NOTE]
> Niestandardowego dostawcy mapy witryny utworzonych w tym samouczku jest ściśle powiązany aplikacji s architektury i danych modelu. Jeff Prosise s [przechowywania mapy witryny w programie SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) i [dostawcy mapy witryny SQL był dawna oczekiwane](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) artykuły zbadać uogólnionego podejścia do przechowywania danych mapy witryny w programie SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Krok 1: Tworzenie stron sieci Web uzyskiwania informacji na temat dostawcy mapy witryny niestandardowej

Zanim zaczniemy Tworzenie niestandardowego dostawcy mapy witryny, należy umożliwić s najpierw dodać strony ASP.NET, które będą potrzebne w ramach tego samouczka. Rozpocznij od dodania nowy folder o nazwie `SiteMapProvider`. Następnie dodaj następujące strony ASP.NET do tego folderu, upewniając się skojarzyć każdą stronę z `Site.master` strona główna:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Również dodać `CustomProviders` podfolder na potrzeby `App_Code` folderu.


![Dodawanie stron ASP.NET samouczki dotyczące dostawcy mapy witryny](building-a-custom-database-driven-site-map-provider-vb/_static/image2.gif)

**Rysunek 2**: Dodawanie strony ASP.NET dla witryny mapowanie związane z dostawcą samouczki


Ponieważ istnieje tylko jeden samouczek dla tej sekcji, firma Microsoft don potrzebę t `Default.aspx` Aby wyświetlić listę samouczków sekcji s. Zamiast tego `Default.aspx` będą wyświetlane kategorie w kontrolce GridView. Firma Microsoft będzie rozwiązano ten problem w kroku 2.

Następnie zaktualizuj `Web.sitemap` obejmujący odwołaniem do `Default.aspx` strony. W szczególności należy dodać następujące znaczniki po Caching `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web w samouczkach, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz element samouczek dostawcy mapy witryny wyłącznie.


![Mapa witryny zawiera teraz wpis dla samouczka dostawcy mapy witryny](building-a-custom-database-driven-site-map-provider-vb/_static/image3.gif)

**Rysunek 3**: mapy witryny zawiera teraz wpis dla samouczka dostawcy mapy witryny


Ten samouczek s głównego koncentruje się na ilustrują Tworzenie niestandardowego dostawcy mapy witryny i konfigurowanie aplikacji sieci web do używania tego dostawcy. W szczególności będziemy tworzyć dostawcy, która zwraca Mapa witryny, która zawiera węzeł główny wraz z węzła dla każdej kategorii i produktu, jak pokazano na rysunku 1. Ogólnie rzecz biorąc każdy węzeł na mapie witryny może określić adres URL. Dla naszych mapy witryny będzie główny adres URL węzła s `~/SiteMapProvider/Default.aspx`, która spowoduje wyświetlenie listy wszystkich kategorii w bazie danych. Każdy węzeł kategorii, mapy witryny będzie mieć adres URL, który wskazuje na `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, która spowoduje wyświetlenie listy wszystkich produktów w określonym *categoryID*. Ponadto każdy węzeł mapy witryny produktu wskaże `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, co spowoduje wyświetlenie szczegółów konkretnego produktu s.

Aby rozpocząć musimy utworzyć `Default.aspx`, `ProductsByCategory.aspx`, i `ProductDetails.aspx` stron. Strony te są wykonywane w kroku 2, 3 i 4, odpowiednio. Ponieważ ciągu po ukończeniu tego samouczka znajduje się w dostawcy mapy witryny i poprzednich samouczków Omówiliśmy już tworzenia raportów tego rodzaju wielostronicowych wzorzec/szczegół, firma Microsoft będzie Pospiesz się za pośrednictwem kroki 2 do 4. Jeśli potrzebujesz przypomnienia informacji na temat tworzenia raportów wzorzec/szczegół, obejmujących wiele stron, odwołaj się do [wzorzec/szczegół filtrowania na dwóch stronach](../masterdetail/master-detail-filtering-across-two-pages-vb.md) samouczka.

## <a name="step-2-displaying-a-list-of-categories"></a>Krok 2: Wyświetlanie listy kategorii

Otwórz `Default.aspx` strony w `SiteMapProvider` folder i przeciągnij GridView z przybornika w projektancie, ustawiając jego `ID` do `Categories`. Za pomocą tagu inteligentnego s GridView powiązać go do nowego elementu ObjectDataSource, o nazwie `CategoriesDataSource` i skonfiguruj ją tak, pobiera jego dane za pomocą `CategoriesBLL` klasy s `GetCategories` metody. Ponieważ ten GridView po prostu Wyświetla kategorie i nie zapewnia możliwości modyfikacji danych, zestawu list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak).


[![Konfigurowanie kontrolki ObjectDataSource do zwrócenia kategorii przy użyciu metody GetCategories](building-a-custom-database-driven-site-map-provider-vb/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image3.png)

**Rysunek 4**: Konfigurowanie kontrolki ObjectDataSource powrócić za pomocą kategorii `GetCategories` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image4.png))


[![Ustaw list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.png)

**Rysunek 5**: Ustaw listy rozwijane w aktualizacji, WSTAWIANIA i usuwania karty (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image6.png))


Po zakończeniu pracy kreatora Konfigurowanie źródła danych, program Visual Studio spowoduje dodanie elementu BoundField dla `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, i `BrochurePath`. Edytowanie widoku GridView tak, aby zawierała tylko `CategoryName` i `Description` BoundFields i zaktualizuj `CategoryName` s elementu BoundField `HeaderText` właściwości do kategorii.

Następnie dodaj pole hiperłącza HyperLinkField i umieść ją tak że s pola najdalej po lewej stronie. Ustaw `DataNavigateUrlFields` właściwości `CategoryID` i `DataNavigateUrlFormatString` właściwość `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Ustaw `Text` właściwości Wyświetl produkty.


![Dodaj pole hiperłącza HyperLinkField do kontrolki GridView kategorii](building-a-custom-database-driven-site-map-provider-vb/_static/image6.gif)

**Rysunek 6**: Dodaj pole hiperłącza HyperLinkField do `Categories` GridView


Po utworzeniu kontrolki ObjectDataSource i dostosowywanie pól s GridView, dwie kontrolki o oznaczeniu deklaracyjnym będzie wyglądać następująco:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample2.aspx)]

Rysunek nr 7 przedstawia `Default.aspx` podczas wyświetlania za pośrednictwem przeglądarki. Kliknięcie kategorii s Wyświetl produkty link umożliwia przejście do `ProductsByCategory.aspx?CategoryID=categoryID`, który utworzymy w kroku 3.


[![Każda kategoria jest wyświetlane wraz z Linkiem umożliwiającym wyświetlanie produktów](building-a-custom-database-driven-site-map-provider-vb/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image7.png)

**Rysunek 7**: każdej kategorii są wyświetlane wraz z Linkiem umożliwiającym wyświetlanie produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Krok 3: Wyświetlanie listy produktów s wybranej kategorii

Otwórz `ProductsByCategory.aspx` strony, a następnie dodaj GridView nadawania mu nazwy `ProductsByCategory`. W tagu inteligentnego, należy powiązać widoku GridView nowe kontrolki ObjectDataSource, o nazwie `ProductsByCategoryDataSource`. Konfigurowanie kontrolki ObjectDataSource używać `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` metody i zestaw z listy rozwijanej listy (Brak) na kartach UPDATE, INSERT i DELETE.


[![Użyj metody GetProductsByCategoryID(categoryID) s ProductsBLL, klasa](building-a-custom-database-driven-site-map-provider-vb/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image9.png)

**Rysunek 8**: Użyj `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image10.png))


Ostatni krok w Kreatorze konfigurowania źródła danych, który wyświetla monit dotyczący źródło parametru *categoryID*. Ponieważ te informacje są przesyłane za pośrednictwem pole querystring `CategoryID`, z listy rozwijanej wybierz QueryString i wprowadź w polu tekstowym vlastnost QueryStringField CategoryID, jak pokazano na rysunku 9. Kliknij przycisk Zakończ, aby zakończyć działanie kreatora.


[![Na użytek pole Querystring CategoryID categoryID parametru](building-a-custom-database-driven-site-map-provider-vb/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image11.png)

**Rysunek 9**: Użyj `CategoryID` pole Querystring *categoryID* parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image12.png))


Po zakończeniu działania kreatora programu Visual Studio spowoduje to dodanie odpowiednich BoundFields i CheckBoxField do kontrolki GridView dla pól danych produktu. Usuń wszystkie elementy oprócz `ProductName`, `UnitPrice`, i `SupplierName` BoundFields. Dostosuj te trzy BoundFields `HeaderText` znaków do odczytania produktu, ceny i dostawcy, odpowiednio. Format `UnitPrice` elementu BoundField jako walutę.

Następnie dodaj pole hiperłącza HyperLinkField i przenieś ją do pozycji najdalej po lewej stronie. Ustaw jego `Text` właściwości, aby wyświetlić szczegóły jego `DataNavigateUrlFields` właściwości `ProductID`i jego `DataNavigateUrlFormatString` właściwość `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Dodaj widok szczegółów pole hiperłącza HyperLinkField wskazujący ProductDetails.aspx](building-a-custom-database-driven-site-map-provider-vb/_static/image10.gif)

**Na rysunku nr 10**: Dodaj widok szczegółów pole hiperłącza HyperLinkField, który wskazuje na `ProductDetails.aspx`


Po wykonaniu tych dostosowań, kontrolkami GridView i kontrolki ObjectDataSource s oznaczeniu deklaracyjnym powinien wyglądać w następujący sposób:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample3.aspx)]

Wróć do wyświetlania `Default.aspx` za pośrednictwem przeglądarki i kliknij przycisk Wyświetl produkty łączy dla Beverages. Spowoduje to przejście do `ProductsByCategory.aspx?CategoryID=1`, wyświetlanie nazw, cen i dostawców produktów w bazie danych Northwind, które należą do kategorii Beverages (zobacz rysunek 11). Możesz rozszerzyć tę stronę, aby uwzględnić łącze do zwrócenia użytkowników na stronę listy kategorii (`Default.aspx`) i DetailsView lub FormView formant, który wyświetla wybraną kategorię s nazwę i opis.


[![Są wyświetlane nazwy Beverages, ceny i dostawcami](building-a-custom-database-driven-site-map-provider-vb/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image13.png)

**Rysunek 11**: są wyświetlane nazwy Beverages, ceny i dostawców ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Krok 4: Przedstawiający szczegóły s produktu

Strony końcowe `ProductDetails.aspx`, są wyświetlane szczegóły wybranych produktów. Otwórz `ProductDetails.aspx` i przeciągnij element DetailsView z przybornika do projektanta. Ustaw DetailsView s `ID` właściwości `ProductInfo` i wyczyszczenie jego `Height` i `Width` wartości właściwości. W tagu inteligentnego, należy powiązać DetailsView nowe kontrolki ObjectDataSource, o nazwie `ProductDataSource`, konfigurowanie kontrolki ObjectDataSource swoich danych z `ProductsBLL` klasy s `GetProductByProductID(productID)` metody. Podobnie jak w przypadku poprzednich stron sieci web utworzonych w krokach 2 i 3 zestawu list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak).


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetProductByProductID(productID)](building-a-custom-database-driven-site-map-provider-vb/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.png)

**Rysunek 12**: Konfigurowanie kontrolki ObjectDataSource do użycia `GetProductByProductID(productID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image16.png))


Ostatni krok w Kreatorze konfigurowania źródła danych, który wyświetla monit dotyczący źródła *productID* parametru. Ponieważ te dane przechodzą przez pole querystring `ProductID`, ustaw listy rozwijanej na ciąg zapytania i pole tekstowe vlastnost QueryStringField do elementu ProductID. Na koniec kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Konfigurowanie productID parametru, aby ściągnąć jego wartość z pola Querystring ProductID](building-a-custom-database-driven-site-map-provider-vb/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image17.png)

**Rysunek 13**: Konfigurowanie *productID* parametru, aby ściągnąć jego wartość z `ProductID` pole Querystring ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image18.png))


Po zakończeniu pracy kreatora Konfigurowanie źródła danych, Visual Studio utworzy odpowiednie BoundFields i CheckBoxField w DetailsView dla pól danych produktu. Usuń `ProductID`, `SupplierID`, i `CategoryID` BoundFields i skonfiguruj pozostałe pola, zgodnie z potrzebami. Po kilku estetycznych konfiguracje Moje DetailsView i kontrolki ObjectDataSource s oznaczeniu deklaracyjnym oglądałem się podobne do następującego:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample4.aspx)]

Aby przetestować tę stronę, wróć do `Default.aspx` i kliknij pozycję Wyświetl produkty dla kategorii Beverages. Z listy spożywczy produktów, kliknij link Wyświetl szczegóły Chai herbaty. Spowoduje to przejście do `ProductDetails.aspx?ProductID=1`, który wskazuje s herbaty Chai więcej (zobacz rysunek 14).


[![Jest wyświetlana herbaty Chai s dostawca, kategoria, ceny i inne informacje](building-a-custom-database-driven-site-map-provider-vb/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image19.png)

**Rysunek 14**: herbaty Chai s dostawca, kategoria, ceny i inne informacje jest wyświetlany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Krok 5: Opis wewnętrznego działania dostawcy mapy witryny

Mapa witryny jest reprezentowana w pamięci serwera s sieci web jako kolekcja `SiteMapNode` wystąpień, które tworzą hierarchię. Musi istnieć dokładnie jeden katalog główny, wszystkie węzły inne niż główny musi mieć dokładnie jeden węzeł nadrzędny, a wszystkie węzły mogą mieć dowolną liczbę elementów podrzędnych. Każdy `SiteMapNode` obiekt reprezentuje sekcję w strukturze witryny sieci Web s; te sekcje często ma odpowiadającą mu stronę sieci web. W związku z tym [ `SiteMapNode` klasy](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) ma właściwości, takie jak `Title`, `Url`, i `Description`, które zawierają informacje o sekcji `SiteMapNode` reprezentuje. Istnieje również `Key` właściwość, która jednoznacznie identyfikuje każdy `SiteMapNode` w hierarchii, a także właściwości, używany do ustanawiania tej hierarchii `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`, i tak dalej.

Rysunek 15 przedstawia witrynę ogólną strukturę mapy z rysunku 1, ale przy użyciu szczegółów implementacji określiła bardziej szczegółowo.


[![Każdy SiteMapNode ma właściwości, takie jak tytuł, adres Url, klucz itd.](building-a-custom-database-driven-site-map-provider-vb/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.gif)

**Rysunek 15**: każdy `SiteMapNode` ma właściwości takich jak `Title`, `Url`, `Key`i tak dalej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image17.gif))


Mapa witryny jest dostępny za pośrednictwem [ `SiteMap` klasy](https://msdn.microsoft.com/library/system.web.sitemap.aspx) w [ `System.Web` przestrzeni nazw](https://msdn.microsoft.com/library/system.web.aspx). Ta klasa s `RootNode` właściwość zwraca lokacji głównej mapy s `SiteMapNode` wystąpienia klasy `CurrentNode` zwraca `SiteMapNode` którego `Url` właściwość zgodny z adresem URL obecnie żądanej strony. Ta klasa jest używana wewnętrznie przez kontrolki sieci Web programu ASP.NET 2.0 s.

Gdy `SiteMap` właściwości klasy s są dostępne, musi go serializować struktura mapy witryny z niektórych nośnika trwałego do pamięci. Logika serializacji mapy witryny nie jest jednak twardych zakodowanej w `SiteMap` klasy. Zamiast tego w czasie wykonywania `SiteMap` klasa określa, które mapy witryny *dostawcy* na potrzeby serializacji. Domyślnie [ `XmlSiteMapProvider` klasy](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) jest używany, która odczytuje struktury mapy s lokacji z pliku XML niepoprawnie sformatowany. Jednak za pomocą trochę pracy możemy utworzyć własną niestandardowego dostawcy mapy witryny.

Wszyscy dostawcy mapy witryny musi pochodzić od [ `SiteMapProvider` klasy](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), która obejmuje podstawowe metody i właściwości służące do lokacji mapować dostawcy, ale pomija wiele szczegółów implementacji. Druga klasa [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), rozszerza `SiteMapProvider` klasy i zawiera bardziej niezawodne wdrożenia wymaganych funkcji. Wewnętrznie `StaticSiteMapProvider` przechowuje `SiteMapNode` wystąpień witryny mapy w `Hashtable` i dostarcza metod, takich jak `AddNode(child, parent)`, `RemoveNode(siteMapNode),` i `Clear()` , dodawać i usuwać `SiteMapNode` s do wewnętrznego `Hashtable`. `XmlSiteMapProvider` jest tworzony na podstawie `StaticSiteMapProvider`.

Podczas tworzenia niestandardowego dostawcy mapy witryny, która rozszerza `StaticSiteMapProvider`, istnieją dwie metody abstrakcyjne, które musi zostać zastąpiona: [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) i [ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, jak sama nazwa wskazuje, jest odpowiedzialne za ładowanie struktury mapy witryny z magazynu trwałego i tworzenia go w pamięci. `GetRootNodeCore` Zwraca węzeł główny mapy witryny.

Przed sieci web aplikacji można użyć dostawcy mapy witryny, które muszą być zarejestrowane w konfiguracji s aplikacji. Domyślnie `XmlSiteMapProvider` klasa jest zarejestrowana przy użyciu nazwy `AspNetXmlSiteMapProvider`. Aby zarejestrować dostawcy mapy witryny dodatkowe, Dodaj następujący kod do `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample5.xml)]

*Nazwa* przypisuje wartość zrozumiałą nazwę dostawcy podczas *typu* Określa nazwę dostawcy mapy witryny w pełni kwalifikowanego typu. Przedstawimy zagadnienia związane z konkretnych wartości *nazwa* i *typu* wartości w kroku 7 po możemy ve utworzony naszych niestandardowego dostawcy mapy witryny.

Klasa dostawcy mapy witryny zostanie uruchomiony po raz pierwszy jest dostępny z `SiteMap` klasy i pozostaje w pamięci dla cyklu życia aplikacji sieci web. Ponieważ istnieje tylko jedno wystąpienie dostawcy mapy witryny, które mogą być wywoływane z wielu jednoczesnych osoby odwiedzające, jest konieczne, aby móc wywoływać metody dostawcy s *wątkowo*.

Wydajność i skalowalność powodów jego ważne, firma Microsoft pamięci podręcznej witryny w pamięci mapowania struktury i zwracać buforowane to struktura, a nie odtworzenia go za każdym razem, gdy `BuildSiteMap` metoda jest wywoływana. `BuildSiteMap` może zostać wywołana wiele razy na żądanie strony dla każdego użytkownika, w zależności od formantów nawigacji używana na stronie i głębokość struktura mapy witryny. W każdym przypadku, jeśli firma Microsoft nie buforują struktury mapy witryny w `BuildSiteMap` każdym razem jest wywoływana, firma Microsoft będzie musiała ponownie pobrać produktu oraz informacje o kategorii z architektury (co mogłoby spowodować zapytania do bazy danych). Jak wspomniano w poprzednich samouczkach buforowania danych w pamięci podręcznej mogą stać się przestarzałe. Aby walczyć z tym, możemy użyć czasu lub expiries na podstawie zależności pamięci podręcznej SQL.

> [!NOTE]
> Dostawcy mapy witryny opcjonalnie mogą zastępować [ `Initialize` metoda](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` jest wywoływane, gdy dostawcy mapy witryny najpierw zostanie uruchomiony i jest przekazywany wszelkie atrybuty niestandardowe, które zostały przypisane do dostawcy w `Web.config` w `<add>` elementów, takich jak: `<add name="name" type="type" customAttribute="value" />`. Jest to przydatne, jeśli chcesz zezwolić na dewelopera strony określić różne ustawienia mapy witryny związane z dostawcą bez konieczności modyfikowania kodu s dostawcy. Na przykład, jeśli firma Microsoft była odczytu kategorii i produktów danych bezpośrednio z bazy danych, w przeciwieństwie do przy użyciu architektury, możemy d prawdopodobnie chcesz zezwolić strony dla deweloperów, określ parametry połączenia bazy danych za pośrednictwem `Web.config` zamiast przy użyciu zakodowanego wartość w kodzie dostawcy s. Niestandardowego dostawcy mapy witryny będziemy tworzyć w kroku 6 nie zastępuje to `Initialize` metody. Na przykład za pomocą `Initialize` metody, można znaleźć [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [przechowywania mapy witryny w programie SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) artykułu.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Krok 6: Tworzenie niestandardowego dostawcy mapy witryny

Aby utworzyć niestandardowego dostawcy mapy witryny, która tworzy mapowanie witryny z kategorii i produktów w bazie danych Northwind, należy utworzyć klasę, która rozszerza `StaticSiteMapProvider`. W kroku 1 pojawia się pytanie, aby dodać `CustomProviders` folderu w `App_Code` folder — Dodaj nową klasę do tego folderu o nazwie `NorthwindSiteMapProvider`. Dodaj następujący kod do `NorthwindSiteMapProvider` klasy:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample6.vb)]

Pozwól s rozpocząć eksplorowanie tej klasy s `BuildSiteMap` metody, która rozpoczyna się od [ `lock` instrukcji](https://msdn.microsoft.com/library/c5kehkcz.aspx). `lock` Instrukcji może składać się jednemu wątkowi w czasie, aby wprowadzić, a tym samym dostęp do jej kodu serializacji co uniemożliwia przechodzenie krok po kroku na siebie nawzajem s palcach jednej przez dwa współbieżnych wątków.

Klasa poziomie `SiteMapNode` zmiennej `root` służy do buforowania struktura mapy witryny. Gdy mapy witryny jest tworzony po raz pierwszy lub po raz pierwszy po zmodyfikowaniu danych bazowych `root` będzie `Nothing` i będzie można konstruować struktury mapy witryny. Węzeł główny mapy s witryny jest przypisany do `root` podczas tworzenia procesu, aby następnym razem ta metoda jest wywoływana, `root` nie będzie `Nothing`. W związku z tym tak długo, jak `root` nie `Nothing` struktura mapy witryny zostanie zwrócony do obiektu wywołującego, bez konieczności ponownego tworzenia.

Jeśli katalog główny jest `Nothing`, struktura mapy witryny jest tworzona na podstawie produktu oraz informacje o kategorii. Mapa witryny jest wbudowana, tworząc `SiteMapNode` wystąpień, a następnie tworzących hierarchię poprzez wywołania `StaticSiteMapProvider` klasy s `AddNode` metody. `AddNode` wykonuje wewnętrznej księgowości, przechowywania są formatowane przy `SiteMapNode` wystąpienia w `Hashtable`. Zanim zaczniemy, tworzenia hierarchii, na początek, wywołując `Clear` metody, która usuwa elementów z wewnętrznego `Hashtable`. Następnie `ProductsBLL` klasy s `GetProducts` metody i wynikowy `ProductsDataTable` są przechowywane w zmiennych lokalnych.

Konstrukcja mapy s lokacji rozpoczyna się od Tworzenie węzła głównego i przypisywanie jej do `root`. Przeciążenia [ `SiteMapNode` Konstruktor s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) używane w tym miejscu i przez cały ten `BuildSiteMap` jest przekazywany następujące informacje:

- Odwołanie do dostawcy mapy witryny (`Me`).
- `SiteMapNode` s `Key`. To wymagane, wartość musi być unikatowa dla każdej `SiteMapNode`.
- `SiteMapNode` s `Url`. `Url` jest opcjonalna, ale jeśli nie dostarczono, każdy `SiteMapNode` s `Url` wartość musi być unikatowa.
- `SiteMapNode` s `Title`, co jest wymagane.

`AddNode(root)` Dodaje wywołania metody `SiteMapNode` `root` do mapy witryny jako katalogu głównego. Następnie każdy `ProductRow` w `ProductsDataTable` wyliczenia. Jeśli istnieje już `SiteMapNode` dla bieżącej kategorii produktu s jest przywoływany. W przeciwnym razie nowy `SiteMapNode` dla kategorii zostanie utworzony i dodany jako element podrzędny elementu `SiteMapNode``root` za pośrednictwem `AddNode(categoryNode, root)` wywołania metody. Po odpowiedniej kategorii `SiteMapNode` węzła zostało znalezione lub utworzone, `SiteMapNode` zostanie utworzony dla bieżącego produktu i dodany jako element podrzędny kategorii `SiteMapNode` za pośrednictwem `AddNode(productNode, categoryNode)`. Należy pamiętać, że kategoria `SiteMapNode` s `Url` wartość właściwości jest `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` podczas produktu `SiteMapNode` s `Url` właściwość jest przypisana `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Te produkty, które mają bazy danych `NULL` wartość ich `CategoryID` są zgrupowane w obszarze kategorii `SiteMapNode` którego `Title` właściwość jest ustawiona na brak i których `Url` ustawioną na pusty ciąg. Czy mogę zdecydowała się ustawić `Url` na ciąg pusty, ponieważ `ProductBLL` klasy s `GetProductsByCategory(categoryID)` metoda obecnie nie ma możliwości, aby zwrócić tylko te produkty z `NULL` `CategoryID` wartości. Ponadto potrzebowała zademonstrować sposób renderowania formantów nawigacji `SiteMapNode` , brakuje wartości dla swojej `Url` właściwości. Zachęcam Cię do rozszerzać ten samouczek, aby Brak `SiteMapNode` s `Url` właściwość wskazuje `ProductsByCategory.aspx`, tylko Wyświetla produkty z `NULL` `CategoryID` wartości.


Po tworzenia mapy witryny, dowolnego obiektu zostanie dodany do pamięci podręcznej danych używanie zależności pamięci podręcznej SQL na `Categories` i `Products` tabele za pośrednictwem `AggregateCacheDependency` obiektu. Rozważyliśmy używanie zależności pamięci podręcznej SQL w poprzednim samouczku *przy użyciu zależności pamięci podręcznej SQL*. Niestandardowego dostawcy mapy witryny, jednak używane jest przeciążenie pamięci podręcznej danych s `Insert` metoda nim ve jeszcze do zbadania. To przeciążenie akceptuje jako jego ostatni parametr wejściowy delegata, która jest wywoływana, gdy obiekt zostanie usunięty z pamięci podręcznej. W szczególności przekazanie nowego [ `CacheItemRemovedCallback` delegować](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) wskazującej `OnSiteMapChanged` metoda zdefiniowana dalszych szczegółów w `NorthwindSiteMapProvider` klasy.

> [!NOTE]
> Reprezentacja w pamięci mapy witryny jest buforowana za pośrednictwem zmiennej poziomu klasa `root`. Ponieważ istnieje tylko jedno wystąpienie, klasa dostawcy mapy witryny niestandardowej i ponieważ to wystąpienie jest współużytkowana przez wszystkie wątki w aplikacji sieci web, zmienna ta klasa służy jako pamięci podręcznej. `BuildSiteMap` Metoda również korzysta z pamięci podręcznej danych, jednak tylko w sposób, aby otrzymać powiadomienie, gdy podstawowe dane z bazy danych w `Categories` lub `Products` tabel zmian. Należy pamiętać, że wartość umieszczane w pamięci podręcznej danych tylko bieżącą datę i godzinę. Dane mapy rzeczywistej lokacji *nie* umieścić w pamięci podręcznej danych.


`BuildSiteMap` Metoda kończy się, zwracając węzła głównego mapy witryny.

Pozostałe metody są dość prosta. `GetRootNodeCore` jest odpowiedzialna za zwrócenie węzła głównego. Ponieważ `BuildSiteMap` zwraca katalog główny, `GetRootNodeCore` po prostu zwraca `BuildSiteMap` s zwracają wartość. `OnSiteMapChanged` Metody ustawia `root` do `Nothing` po usunięciu elementu pamięci podręcznej. Z certyfikatem głównym powraca do `Nothing`, przy następnym `BuildSiteMap` jest wywołana, struktura mapy witryny zostanie odbudowany. Na koniec `CachedDate` właściwość zwraca wartość daty i godziny, przechowywane w pamięci podręcznej danych, jeśli istnieje taka wartość. Tej właściwości może służyć przez dewelopera strony, aby określić, kiedy ostatnio buforowano dane mapy witryny.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Krok 7: Rejestrowanie`NorthwindSiteMapProvider`

Aby nasza aplikacja sieci web do użycia `NorthwindSiteMapProvider` utworzony w kroku 6 dostawcy mapy witryny, należy zarejestrować go w `<siteMap>` części `Web.config`. W szczególności należy dodać następujący kod w ramach `<system.web>` element `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample7.xml)]

Ten kod znaczników wykonuje dwie czynności: po pierwsze, oznacza to, że wbudowane `AspNetXmlSiteMapProvider` jest dostawcy mapy witryny domyślnej; po drugie, rejestruje niestandardowego dostawcy mapy witryny utworzony w kroku 6 za pomocą nazw przyjaznych dla człowieka Northwind.

> [!NOTE]
> Dla dostawcy mapy witryny s aplikacji na terenie `App_Code` folderu, wartość `type` atrybutu jest po prostu nazwą klasy. Alternatywnie niestandardowego dostawcy mapy witryny można zostały utworzone w osobnym projekcie Biblioteka klas przy użyciu skompilowanego zestawu umieszczone w elemencie s aplikacji sieci web `/Bin` katalogu. W takim przypadku `type` wartość atrybutu będzie *Namespace*. *ClassName*, *AssemblyName* .


Po zaktualizowaniu `Web.config`, Poświęć chwilę, aby wyświetlić wszystkie strony z samouczków w przeglądarce. Pamiętaj, że interfejs nawigacji po lewej stronie nadal pokazuje sekcje i samouczki zdefiniowane w `Web.sitemap`. Jest to spowodowane pozostawiliśmy `AspNetXmlSiteMapProvider` jako domyślnego dostawcę. Aby można było utworzyć nawigacji elementu interfejsu użytkownika, który używa `NorthwindSiteMapProvider`, musisz jawnie określić, że dostawcy mapy witryny Northwind powinien być używany. Zobaczymy, jak to zrobić w kroku 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Krok 8: Wyświetlanie informacji mapy witryny przy użyciu niestandardowego dostawcy mapy witryny

Przy użyciu niestandardowej witryny dostawcy mapy utworzony i zarejestrowany w `Web.config`, możemy ponownie gotowy do dodawania formantów nawigacji do `Default.aspx`, `ProductsByCategory.aspx`, i `ProductDetails.aspx` stron w `SiteMapProvider` folderu. Zacznij od otwarcia `Default.aspx` strony, a następnie przeciągnij `SiteMapPath` z przybornika do projektanta. Kontrolki ścieżki mapy witryny znajduje się w sekcji nawigacji przybornika.


[![Dodaj ścieżki mapy witryny do Default.aspx](building-a-custom-database-driven-site-map-provider-vb/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image18.gif)

**Rysunek 16**: Dodaj SiteMapPath do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image20.gif))


Kontrolki ścieżki mapy witryny Wyświetla ślad, wskazujący bieżącą lokalizację s strony w obrębie mapy witryny. Dodaliśmy SiteMapPath na górze strony wzorcowej w *strony wzorcowe i nawigacja w witrynie* samouczka.

Poświęć chwilę, aby wyświetlić tą stronę za pośrednictwem przeglądarki. SiteMapPath dodane w rysunek 16 używa domyślnego dostawcy mapy witryny, ściąganie danych z `Web.sitemap`. W związku z tym, linków do stron nadrzędnych strona główna pokazuje &gt; Dostosowywanie mapy witryny, podobnie jak nawigacji w prawym górnym rogu.


[![Łącza do stron nadrzędnych używa domyślnego dostawcy mapy witryny](building-a-custom-database-driven-site-map-provider-vb/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.gif)

**Rysunek 17**: łączach używa dostawcy mapy witryny domyślnej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image23.gif))


Aby SiteMapPath dodane w rysunek 16, użyć niestandardowego dostawcy mapy witryny utworzonego w kroku 6, ustaw jego [ `SiteMapProvider` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) do Northwind, przypisaną nazwę, firma Microsoft `NorthwindSiteMapProvider` w `Web.config`. Niestety Projektant będzie później nadal używać domyślnego dostawcy mapy witryny, ale jeśli odwiedź stronę za pośrednictwem przeglądarki po wprowadzeniu tej zmiany właściwości zobaczysz, że łączy do stron nadrzędnych teraz używa niestandardowego dostawcy mapy witryny.


[![Łącza do stron nadrzędnych używa teraz NorthwindSiteMapProvider dostawcy mapy witryny niestandardowej](building-a-custom-database-driven-site-map-provider-vb/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image24.gif)

**Rysunek 18**: łączach korzysta z dostawcy mapy witryny niestandardowe `NorthwindSiteMapProvider` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image26.gif))


Kontrolki ścieżki mapy witryny przedstawia bardziej funkcjonalnego interfejsu użytkownika w `ProductsByCategory.aspx` i `ProductDetails.aspx` stron. Dodaj ścieżki mapy witryny z tymi stronami, ustawienie `SiteMapProvider` właściwość zarówno do Northwind. Z `Default.aspx` kliknij łącze Wyświetl produkty dla Beverages, a następnie łącze Wyświetl szczegóły dla Chai herbaty. Jak pokazano na rysunku 19, linków do stron nadrzędnych obejmuje bieżącej lokacji mapy (Chai herbaty) i jego elementów nadrzędnych: Beverages i wszystkich kategorii.


[![Łącza do stron nadrzędnych używa teraz NorthwindSiteMapProvider dostawcy mapy witryny niestandardowej](building-a-custom-database-driven-site-map-provider-vb/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.png)

**Rysunek 19**: łączach korzysta z dostawcy mapy witryny niestandardowe `NorthwindSiteMapProvider` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image22.png))


Inne elementy interfejsu użytkownika nawigacji służy oprócz SiteMapPath, takich jak kontrolki Menu i widoku drzewa. `Default.aspx`, `ProductsByCategory.aspx`, I `ProductDetails.aspx` stron do pobrania na potrzeby tego samouczka, na przykład, obejmują formanty Menu (patrz rysunek 20). Zobacz [s badanie ASP.NET 2.0 funkcji nawigacji w witrynie](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) i [za pomocą kontrolki witryny](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) części [przewodników Szybki Start platformy ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/) dla bardziej szczegółowo poznać kontrolki i system mapy witryny ASP.NET w wersji 2.0.


[![Formant Menu zawiera listę kategorii i produkty](building-a-custom-database-driven-site-map-provider-vb/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image28.gif)

**Rysunek 20**: Menu kontrolki listy każdej kategorii i produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-vb/_static/image30.gif))


Jak wspomniano wcześniej w tym samouczku, struktura mapy witryny są dostępne programowo za pośrednictwem `SiteMap` klasy. Poniższy kod zwraca katalog główny `SiteMapNode` domyślnego dostawcy:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample8.vb)]

Ponieważ `AspNetXmlSiteMapProvider` jest dostawcą domyślnym dla naszej aplikacji powyższy kod zwróci zdefiniowany w węźle głównym `Web.sitemap`. Aby odwoływać się do dostawcy mapy witryny, inny niż domyślny, należy użyć `SiteMap` klasy s [ `Providers` właściwość](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) w następujący sposób:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample9.vb)]

Gdzie *nazwa* nazywa się niestandardowego dostawcy mapy witryny (Northwind dla naszej aplikacji internetowej).

Aby uzyskać dostęp do składowej, które są specyficzne dla dostawcy mapy witryny, należy użyć `SiteMap.Providers["name"]` można pobrać wystąpienia dostawcy, a następnie przerzuć go do odpowiedniego typu. Na przykład, aby wyświetlić `NorthwindSiteMapProvider` s `CachedDate` właściwości na stronie ASP.NET, użyj następującego kodu:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample10.vb)]

> [!NOTE]
> Pamiętaj przetestowania funkcji zależności pamięci podręcznej SQL. Po odwiedzając `Default.aspx`, `ProductsByCategory.aspx`, i `ProductDetails.aspx` stron, przejdź do jednego z samouczków w edytowanie, wstawianie i usuwanie sekcji i umożliwia edytowanie nazwy, kategorii lub produktu. Następnie wróć do jednej ze stron w `SiteMapProvider` folderu. Zakładając, że dla mechanizmu sondowania należy pamiętać, zmiany z podstawową bazą danych minęło wystarczająco dużo czasu, Mapa witryny, powinien zostać zaktualizowany do wyświetlenia nowego produktu lub nazwa kategorii.


## <a name="summary"></a>Podsumowanie

Program ASP.NET 2.0 obejmuje funkcje mapy witryny s `SiteMap` klasy, określa liczbę nawigacyjny wbudowanych w sieci Web i dostawcy mapy witryny domyślnej, który oczekuje, że dane mapy witryny utrwalone w pliku XML. Aby można było używać mapy witryny z innego źródła takie jak bazy danych, architektura s lub zdalnej usługi sieci Web, należy utworzyć niestandardowego dostawcy mapy witryny. Obejmuje to tworzenie klasy, która pochodzi bezpośrednio lub pośrednio z `SiteMapProvider` klasy.

W tym samouczku widzieliśmy sposób tworzenia niestandardowego dostawcy mapy witryny, Mapa witryny na podstawie produktów i kategorii informacji ubitych z architektury aplikacji. U naszego dostawcy rozszerzone `StaticSiteMapProvider` klasy i związanych z tworzenia `BuildSiteMap` metodę, która pobierane są dane, skonstruowany hierarchii mapy witryny i buforowane strukturze wynikowe w zmiennej na poziomie klasy. Użyliśmy zależności pamięci podręcznej SQL za pomocą funkcji wywołania zwrotnego do unieważnienia pamięci podręcznej struktury, gdy podstawowe `Categories` lub `Products` danych zostanie zmodyfikowany.

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Przechowywanie mapy witryny w programie SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) i [dostawcy mapy witryny SQL był dawna oczekiwane](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Model dostawcy s przyjrzeć platformy ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Zestaw narzędzi dostawcy](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Badanie programu ASP.NET 2.0 funkcji nawigacji w witrynie s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Dave Gardner, Zack Jones, Teresa Murphy i Bernadette Leigh. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](building-a-custom-database-driven-site-map-provider-cs.md)
