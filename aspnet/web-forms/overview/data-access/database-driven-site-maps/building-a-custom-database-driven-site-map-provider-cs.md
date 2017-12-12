---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: Tworzenie dostawcy mapy witryny opartej na bazie danych niestandardowych (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: "Domyślny dostawca mapy witryny w programie ASP.NET 2.0 pobiera dane ze statycznego pliku XML. Gdy dostawca opartych na języku XML nadaje się do wielu w małych i średnich rozmiar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: f09d8d24cea43e80e66b0d8ce50911fcb978c85e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="building-a-custom-database-driven-site-map-provider-c"></a>Tworzenie dostawcy mapy witryny opartej na bazie danych niestandardowych (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip) lub [pobierania plików PDF](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> Domyślny dostawca mapy witryny w programie ASP.NET 2.0 pobiera dane ze statycznego pliku XML. Gdy dostawca opartych na języku XML nadaje się do wielu witryn sieci Web małych i średnich, dużych aplikacji sieci Web wymagają bardziej dynamiczne mapy witryny. W tym samouczku, który będzie budujemy dostawcy mapy niestandardowej witryny, która pobiera dane z warstwy logiki biznesowej, które z kolei pobiera dane z bazy danych.


## <a name="introduction"></a>Wprowadzenie

Platforma ASP.NET 2.0 funkcja mapy witryny s umożliwia deweloperom strony zdefiniowania mapy witryny sieci web i aplikacji s niektóre stałe nośnika, na przykład w pliku XML. Po zdefiniowaniu, dane mapy witryny można uzyskać dostęp programowo za pomocą [ `SiteMap` klasy](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx) w [ `System.Web` przestrzeni nazw](https://msdn.microsoft.com/en-us/library/system.web.aspx) lub za pomocą różnych nawigacji formanty sieci Web, takich jak Kontrolki ścieżki mapy witryny, Menu i TreeView. System mapy witryny używa [modelu dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) tak, aby implementacje serializacji mapy innej lokacji można tworzyć i podłączony do aplikacji sieci web. Domyślny dostawca mapy witryny dostarczaną z programem ASP.NET 2.0 utrzymuje struktury mapy witryny w pliku XML. W [stron wzorcowych i nawigacji w witrynie](../introduction/master-pages-and-site-navigation-cs.md) samouczek utworzyliśmy plik o nazwie `Web.sitemap` czy zawarte tej struktury i mieć aktualizuje jego XML z każdej nowej sekcji samouczka.

Domyślnego dostawcy mapy witryny opartych na języku XML działa dobrze w przypadku, gdy struktura s mapy witryny jest dość statycznych, takich jak te samouczki. W wielu scenariuszach jednak bardziej dynamiczne mapy witryny jest wymagany. Należy wziąć pod uwagę przedstawione na rysunku 1, w którym każdej kategorii i produktu są wyświetlane jako części strukturę s witryny sieci Web mapy witryny. Z tym mapy witryny odwiedzania strony sieci web odpowiadający węzła głównego mogą zawierać wszystkie kategorie, odwiedzając stronę sieci web określonej kategorii s listę produktów s tej kategorii i wyświetlanie strony sieci web określonego produktu s czy Pokaż tego produktu s szczegóły.


[![Kategorie i produktów w skład struktury s mapy witryny](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**Rysunek 1**: kategorie i produktów w skład s mapy witryny struktury ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))


Ta struktura według kategorii i produktu może być ustalony w `Web.sitemap` pliku, plik musi zostać zaktualizowany w każdym razem, kiedy kategorii lub dodane, usunięte lub zmieniono jego nazwę produktu. W rezultacie konserwacji mapy witryny może znacznie uprościć jeśli jego struktury został pobrany z bazy danych lub, najlepiej z warstwy logiki biznesowej architektura s. W ten sposób, jak produktów i kategorie zostały dodane, zmieniono nazwę lub usunięto, aby uwzględnić te zmiany automatycznie zaktualizuje mapy witryny.

Ponieważ serializacji mapy witryny s ASP.NET 2.0 została stworzona z modelu dostawcy, można utworzyć dostawcy mapy własnej niestandardowej witryny, który grabs dane z magazynu danych, takich jak bazy danych lub architektury. W tym samouczku będziesz mamy utworzyć niestandardowego dostawcę, który pobiera dane z logiki warstwy Biznesowej. Rozpoczynanie pracy dzięki s!

> [!NOTE]
> Dostawcy mapy niestandardowej witryny utworzonej w tym samouczku jest ściśle powiązane do aplikacji s architektury i danych modelu. S Jeff Prosise [przechowywania map witryn w programie SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) i [dostawcy mapy witryny SQL był czas oczekiwania](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) artykuły zbadać uogólniony podejście do przechowywania danych mapy witryny w programie SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Krok 1: Tworzenie stron sieci Web uzyskiwania informacji na temat dostawcy mapy niestandardowej witryny

Zanim możemy rozpocząć tworzenie dostawcy mapy witryny niestandardowej, chętnie s najpierw dodać stron ASP.NET, które będą potrzebne w tym samouczku. Rozpocznij od dodania nowy folder o nazwie `SiteMapProvider`. Następnie dodaj do tego folderu, upewniając się skojarzyć każdą stronę z następujących stron ASP.NET `Site.master` strony głównej:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Dodaj również `CustomProviders` podfolderu, aby `App_Code` folderu.


![Dodawanie stron ASP.NET samouczki dotyczące dostawcy mapy witryny](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**Rysunek 2**: Dodawanie stron ASP.NET dla witryny mapy samouczki związane z dostawcą


Ponieważ istnieje tylko jeden samouczek dla tej sekcji, możemy ADAM potrzeby t `Default.aspx` do listy samouczki sekcji s. Zamiast tego `Default.aspx` wyświetli kategorii w kontrolce GridView. Firma Microsoft będzie rozwiązano ten problem w kroku 2.

Następnie zaktualizuj `Web.sitemap` załączanie odwołań do `Default.aspx` strony. W szczególności, Dodaj następujący kod po buforowanie `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web samouczki, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementu samouczek dostawcy mapy witryny wyłącznie.


![Mapy witryny zawiera teraz wpis samouczek dostawcy mapy witryny](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**Rysunek 3**: mapy witryny zawiera teraz wpis samouczek dostawcy mapy witryny


Głównym celem tego samouczka s jest aby zilustrować Tworzenie dostawcy mapy witryny niestandardowej i konfigurowanie aplikacji sieci web do korzystania z tego dostawcy. W szczególności firma Microsoft będzie kompilacji dostawcy, która zwraca mapy witryny, która zawiera węzeł główny wraz z węzłem dla każdej kategorii i produktu, jak pokazano na rysunku 1. Ogólnie rzecz biorąc każdy węzeł mapy witryny może określić adres URL. Dla naszych mapy witryny będą głównego adresu URL węzła s `~/SiteMapProvider/Default.aspx`, który znajduje się lista wszystkich kategorii w bazie danych. Każdy węzeł kategorii mapy witryny będą mieć adres URL, który wskazuje `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, która będzie zawierała listę wszystkich produktów w określonej *categoryID*. Ponadto każdy węzeł mapy witryny produktu wskaże `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, które będą wyświetlane szczegóły określonego produktu s.

Aby uruchomić należy utworzyć `Default.aspx`, `ProductsByCategory.aspx`, i `ProductDetails.aspx` stron. Te strony są wykonywane w kroku 2, 3 i 4, odpowiednio. Ponieważ impuls w tym samouczku znajduje się w dostawcy mapy witryny i samouczki ostatnich ma pasuje do tworzenia raportów tego rodzaju wielostronicowych wzorzec/szczegół, firma Microsoft będzie Pospiesz się za pośrednictwem kroki 2 do 4. Jeśli potrzebujesz odświeżacz o tworzeniu raportów wzorzec/szczegół, obejmującej wiele stron, odwołaj się do [wzorzec/szczegół filtrowania między dwoma stronami](../masterdetail/master-detail-filtering-across-two-pages-cs.md) samouczka.

## <a name="step-2-displaying-a-list-of-categories"></a>Krok 2: Wyświetlanie listy kategorii

Otwórz `Default.aspx` strony `SiteMapProvider` folder i przeciągnij element GridView z przybornika do projektanta, ustawienie jej `ID` do `Categories`. Z tagów inteligentnych s GridView powiązać go z nowego elementu ObjectDataSource o nazwie `CategoriesDataSource` i skonfiguruj ją tak, aby pobiera, jego danych przy użyciu `CategoriesBLL` klasy s `GetCategories` metody. Ponieważ właśnie tego widoku GridView przedstawia kategorie i nie ma możliwości zmiany danych, ustaw list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak).


[![Skonfiguruj ObjectDataSource do zwrócenia kategorii przy użyciu metody GetCategories](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**Rysunek 4**: Konfigurowanie ObjectDataSource powrócić przy użyciu kategorii `GetCategories` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))


[![Ustawianie list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**Rysunek 5**: wartość listy rozwijane w AKTUALIZOWANIA, WSTAWIANIA i usuwanie kart (None) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))


Po zakończeniu pracy Kreatora konfigurowania źródła danych programu Visual Studio spowoduje dodanie elementu BoundField dla `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, i `BrochurePath`. Edytowanie widoku GridView, tak aby zawierała tylko `CategoryName` i `Description` BoundFields i zaktualizuj `CategoryName` s elementu BoundField `HeaderText` właściwości do kategorii.

Następnie dodaj pole hiperłącza HyperLinkField i umieść go tak że s pole lewej. Ustaw `DataNavigateUrlFields` właściwości `CategoryID` i `DataNavigateUrlFormatString` właściwości `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Ustaw `Text` właściwości do widoku produktów.


![Dodaj pole hiperłącza HyperLinkField do widoku GridView kategorii](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**Rysunek 6**: Dodaj pole hiperłącza HyperLinkField do `Categories` widoku GridView


Po utworzeniu elementu ObjectDataSource i dostosowywanie pól s GridView, znaczników deklaratywne dwóch formantów będzie wyglądać następująco:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

Rysunek nr 7 przedstawia `Default.aspx` podczas wyświetlania za pośrednictwem przeglądarki. Kliknięcie kategorii s Wyświetl produkty łącze prowadzi do `ProductsByCategory.aspx?CategoryID=categoryID`, które firma Microsoft będzie kompilowana w kroku 3.


[![Każda kategoria to wymienione wraz z łączem produktów widoku](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**Rysunek 7**: każdej kategorii jest wymienione wraz z łączem produktów widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Krok 3: Wyświetlanie wybranej kategorii produktów s

Otwórz `ProductsByCategory.aspx` i dodać element GridView nazw `ProductsByCategory`. W tagu inteligentnego powiązać widoku GridView nowy element ObjectDataSource o nazwie `ProductsByCategoryDataSource`. Element ObjectDataSource umożliwia konfigurowanie `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` — metoda i zestaw listy rozwijanej listy (Brak), na kartach UPDATE, INSERT i DELETE.


[![Użyj metody GetProductsByCategoryID(categoryID) s ProductsBLL — klasa](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**Rysunek 8**: Użyj `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))


Ostatnim krokiem w Kreatorze skonfiguruj źródło danych wyświetla monit o źródło parametru *categoryID*. Ponieważ te informacje są przesyłane za pomocą pola querystring `CategoryID`, wybierz ciąg zapytania z listy rozwijanej i wprowadź w polu tekstowym pole QueryStringField CategoryID, jak pokazano na rysunku 9. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Użyj pola Querystring CategoryID dla categoryID parametru](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**Rysunek 9**: Użyj `CategoryID` pola Querystring *categoryID* parametr ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))


Po zakończeniu działania kreatora programu Visual Studio zostaną dodane do widoku GridView dla pól danych produktu odpowiednie BoundFields i CheckBoxField. Usuń wszystkie elementy oprócz `ProductName`, `UnitPrice`, i `SupplierName` BoundFields. Dostosuj te trzy BoundFields `HeaderText` właściwości do odczytu produktu, ceny i dostawcy, odpowiednio. Format `UnitPrice` elementu BoundField jako walutę.

Następnie dodaj pole hiperłącza HyperLinkField i przenieś ją do lewej położenia. Ustaw jego `Text` właściwości, aby wyświetlić szczegóły jego `DataNavigateUrlFields` właściwości `ProductID`i jego `DataNavigateUrlFormatString` właściwości `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Dodaj widok szczegółów pole hiperłącza HyperLinkField wskazujące ProductDetails.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**Na rysunku nr 10**: Dodawanie widoku szczegółów pole hiperłącza HyperLinkField, który wskazuje`ProductDetails.aspx`


Po wykonaniu tych dostosowań, znaczników deklaratywne s GridView i ObjectDataSource powinna wyglądać w następujący sposób:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

Wróć do wyświetlania `Default.aspx` za pośrednictwem przeglądarki i kliknij pozycję Wyświetl produkty łączy dla napoje. Spowoduje to przejście do `ProductsByCategory.aspx?CategoryID=1`, wyświetlanie nazw, ceny i dostawców produktów w bazie danych Northwind, który należy do należące do tej kategorii (patrz rysunek 11). Możesz zwiększyć tę stronę, aby uwzględnić łącze do zwrócenia użytkowników na stronę listy kategorii (`Default.aspx`) i wyświetla wybraną kategorię s nazwę i opis formantu widoku DetailsView lub FormView.


[![Są wyświetlane nazwy napoje, ceny i dostawcami](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**Rysunek 11**: są wyświetlane nazwy napoje, ceny i dostawców ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Krok 4: Pokazywanie szczegółów s produktu

Ostatnia strona `ProductDetails.aspx`, wyświetlane są szczegóły zaznaczonych produktów. Otwórz `ProductDetails.aspx` i przeciągnij element DetailsView z przybornika do projektanta. Ustaw s widoku DetailsView `ID` właściwości `ProductInfo` i wyczyszczenie jego `Height` i `Width` wartości właściwości. W tagu inteligentnego powiązać widoku DetailsView nowy element ObjectDataSource o nazwie `ProductDataSource`, konfigurowanie ObjectDataSource jego dane z `ProductsBLL` klasy s `GetProductByProductID(productID)` metody. Podobnie jak w przypadku poprzednich stron sieci web utworzony w kroki 2 i 3 zestawu list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak).


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetProductByProductID(productID)](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**Rysunek 12**: Konfigurowanie ObjectDataSource użyć `GetProductByProductID(productID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))


Ostatni krok kreatora konfigurowania źródła danych monit o podanie źródła *productID* parametru. Ponieważ te dane jest dostarczany za pomocą pola querystring `ProductID`, ustaw listy rozwijanej QueryString i pole tekstowe pole QueryStringField do ProductID. Na koniec kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Skonfiguruj productID parametru ściągania jego wartość z pola ProductID ciąg zapytania](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**Rysunek 13**: Konfigurowanie *productID* parametru ściągania jego wartość z `ProductID` pole Querystring ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))


Po zakończeniu pracy Kreatora konfigurowania źródła danych, Visual Studio utworzy odpowiedni BoundFields i CheckBoxField w widoku DetailsView dla pól danych produktu. Usuń `ProductID`, `SupplierID`, i `CategoryID` BoundFields i skonfiguruj pozostałe pola zgodnie z własnymi potrzebami. Po kilku estetycznych konfiguracje Moje widoku DetailsView i ObjectDataSource s deklaratywne znaczników wyszukiwanego podobne do poniższych:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

Aby przetestować tę stronę, wróć do `Default.aspx` i kliknij polecenie Wyświetl produkty dla należące do tej kategorii. Z listą produktów napoju, kliknij Link Wyświetl szczegóły, aby Zepołowy Chai. Spowoduje to przejście do `ProductDetails.aspx?ProductID=1`, który wskazuje s Zepołowy Chai szczegóły (patrz rysunek 14).


[![Zostanie wyświetlony Chai Zepołowy s dostawca, kategoria, ceny i inne informacje](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**Rysunek 14**: Zepołowy Chai s dostawca, kategoria, ceny i inne informacje są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Krok 5: Opis przebiega dostawcy mapy witryny

Mapy witryny jest reprezentowana w pamięci s serwera sieci web jako kolekcja `SiteMapNode` wystąpień, które tworzą hierarchię. Musi istnieć dokładnie jeden katalog główny, wszystkie węzły z systemem innym niż główny musi mieć dokładnie jeden węzeł nadrzędny i wszystkich węzłów może mieć dowolną liczbę elementów podrzędnych. Każdy `SiteMapNode` obiekt reprezentuje sekcję w strukturze witryny sieci Web s; te sekcje często mają odpowiednie strony sieci web. W rezultacie [ `SiteMapNode` klasy](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.aspx) ma właściwości, takie jak `Title`, `Url`, i `Description`, które zapewniają informacje o sekcji `SiteMapNode` reprezentuje. Istnieje również `Key` właściwości, który unikatowo identyfikuje każdego `SiteMapNode` w hierarchii, jak również właściwości używany do ustanawiania tej hierarchii `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`, itd.

Rysunek 15 zawiera struktura mapy witryny ogólne z rysunku 1, ale określiła szczegółowo bardziej precyzyjną szczegóły implementacji.


[![Każdy SiteMapNode ma właściwości, takie jak tytuł, adres Url, klucz itd.](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**Rysunek 15**: każdego `SiteMapNode` ma właściwości jak `Title`, `Url`, `Key`i tak dalej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))


Mapy witryny jest dostępna za pośrednictwem [ `SiteMap` klasy](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx) w [ `System.Web` przestrzeni nazw](https://msdn.microsoft.com/en-us/library/system.web.aspx). Ta klasa s `RootNode` właściwość zwraca lokacji głównej mapy s `SiteMapNode` wystąpienia; `CurrentNode` zwraca `SiteMapNode` których `Url` właściwości odpowiada adresowi URL obecnie żądanej strony. Ta klasa jest używana wewnętrznie przez formanty sieci Web programu ASP.NET 2.0 s nawigacji.

Gdy `SiteMap` właściwości klasy s są dostępne, musi on serializować struktury mapy witryny niektóre stałe nośnik do pamięci. Jednak logiki serializacji mapy witryny nie jest trudne w `SiteMap` klasy. Zamiast tego w czasie wykonywania `SiteMap` klasy określa, które mapy witryny *dostawcy* do użycia na potrzeby serializacji. Domyślnie [ `XmlSiteMapProvider` klasy](https://msdn.microsoft.com/en-us/library/system.web.xmlsitemapprovider.aspx) jest używana, która odczytuje struktura s mapy witryny z pliku XML niepoprawnie sformatowany. Jednak z niewielki pracy można utworzyć własną niestandardową witrynę dostawcy mapy.

Wszystkich dostawców mapy witryny musi pochodzić od [ `SiteMapProvider` klasy](https://msdn.microsoft.com/en-us/library/system.web.sitemapprovider.aspx), w tym podstawowych metod i właściwości wymagane dla lokacji mapy dostawców, ale pominięto wiele szczegóły implementacji. W drugiej klasy [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/en-us/library/system.web.staticsitemapprovider.aspx), rozszerza `SiteMapProvider` klasy i zawiera bardziej niezawodne implementacja funkcje. Wewnętrznie `StaticSiteMapProvider` przechowuje `SiteMapNode` wystąpień witryny mapy w `Hashtable` i udostępnia metody, takich jak `AddNode(child, parent)`, `RemoveNode(siteMapNode),` i `Clear()` który Dodawanie i usuwanie `SiteMapNode` s do wewnętrznej `Hashtable`. `XmlSiteMapProvider`jest pochodną `StaticSiteMapProvider`.

Jeśli tworzenie dostawcy mapy niestandardowej witryny, która rozszerza `StaticSiteMapProvider`, istnieją dwie metody abstrakcyjnej, które musi zostać zastąpiona: [ `BuildSiteMap` ](https://msdn.microsoft.com/en-us/library/system.web.staticsitemapprovider.buildsitemap.aspx) i [ `GetRootNodeCore` ](https://msdn.microsoft.com/en-us/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, jak jego nazwa wskazuje, jest odpowiedzialny za ładowanie struktury mapy witryny z magazynu trwałego i tworzenia go w pamięci. `GetRootNodeCore`Zwraca główny węzeł mapy witryny.

Przed sieci web aplikacji można użyć dostawcy mapy witryny, który musi być zarejestrowany w konfiguracji aplikacji s. Domyślnie `XmlSiteMapProvider` klasa jest zarejestrowana przy użyciu nazwy `AspNetXmlSiteMapProvider`. Aby zarejestrować lokacji dodatkowych dostawców mapy, Dodaj następujący kod do `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

*Nazwa* wartość przypisuje nazwę zrozumiałą dla dostawcy podczas *typu* pełni kwalifikowanego typu nazwa dostawcy mapy witryny. Firma Microsoft będzie Poznaj konkretne wartości *nazwa* i *typu* wartości w kroku 7 po możemy kolejnych utworzone naszych dostawcy mapy niestandardowej witryny.

Klasa dostawcy mapy witryny zostanie uruchomiony po raz pierwszy jest dostępny z `SiteMap` klasy i pozostaje w pamięci przez czas ich istnienia aplikacji sieci web. Ponieważ istnieje tylko jedno wystąpienie dostawcy mapy witryny, który może wywołać z wielu równoczesnych osoby odwiedzające, konieczne jest konieczności metod dostawcy s *wątkowo*.

Wydajność i skalowalność powodów jego s ważne, możemy pamięci podręcznej w pamięci lokacji mapowania struktury i zwracać buforowane to struktura zamiast konieczności jej ponownego tworzenia zawsze `BuildSiteMap` wywołania metody. `BuildSiteMap`może zostać wywołana kilka razy na żądanie dostępu do strony dla poszczególnych użytkowników, w zależności od formantów nawigacji używany na stronie i głębokość struktury mapy witryny. W każdym przypadku, jeśli firma Microsoft nie buforują struktury mapy witryny w `BuildSiteMap` , a następnie zawsze jest wywoływana, czy należy ponownie pobrać produktu oraz informacje o kategorii z architektury (co spowoduje powstanie zapytania do bazy danych). Jak wspomniano w poprzedniej samouczki buforowania danych z pamięci podręcznej może stać się przestarzałe. To zwalczania, firma Microsoft może używać czas - lub expiries na podstawie zależności buforu SQL.

> [!NOTE]
> Dostawcy mapy witryny może opcjonalnie zastąpić [ `Initialize` metody](https://msdn.microsoft.com/en-us/library/system.web.sitemapprovider.initialize.aspx). `Initialize`wywoływane, gdy dostawcy mapy witryny jest najpierw wystąpienie zostało utworzone i jest przekazywany dowolne atrybuty niestandardowe przypisane do dostawcy w `Web.config` w `<add>` elementów, takich jak: `<add name="name" type="type" customAttribute="value" />`. Jest przydatne, jeśli chcesz zezwolić dewelopera strony określić różne ustawienia mapy witryny związane z dostawcą bez konieczności modyfikowania kodu s dostawcy. Na przykład, jeśli firma Microsoft zostały odczytu kategorii i produktów danych bezpośrednio z bazy danych, w przeciwieństwie do za pomocą architektury, możemy d prawdopodobnie chcesz zezwolić developer strony, określ odpowiedni ciąg połączenia bazy danych za pośrednictwem `Web.config` zamiast przy użyciu zakodowanego wartość w kodzie dostawcy s. Firma Microsoft będzie kompilacji w kroku 6 dostawcy mapy niestandardowej witryny nie zastępuje to `Initialize` metody. Przykład za pomocą `Initialize` metody, zapoznaj się [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [przechowywania map witryn w programie SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) artykułu.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Krok 6: Tworzenie dostawcy mapy witryny niestandardowej

Aby utworzyć niestandardową witrynę dostawcy mapy, który kompiluje mapy witryny z kategorii i produktów w bazie danych Northwind, należy utworzyć klasę, która rozszerza `StaticSiteMapProvider`. W kroku 1 pojawia się pytanie, można dodać `CustomProviders` folderu w `App_Code` folderu - Dodaj nową klasę do tego folderu o nazwie `NorthwindSiteMapProvider`. Dodaj następujący kod do `NorthwindSiteMapProvider` klasy:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

Let s rozpoczynać się od tej klasy s eksploracji `BuildSiteMap` metodę, która rozpoczyna się od [ `lock` instrukcji](https://msdn.microsoft.com/en-us/library/c5kehkcz.aspx). `lock` Instrukcji zezwala tylko jeden wątek w momencie wprowadzenia, tym samym dostęp do jej kodu serializacji i uniemożliwia wykonywanie krok po kroku na siebie s roku dwa współbieżnych wątków.

Klasa poziomie `SiteMapNode` zmiennej `root` służy do buforowania struktury mapy witryny. Gdy mapy witryny jest tworzony po raz pierwszy lub po raz pierwszy po zmodyfikowaniu danych `root` będzie `null` i będzie można utworzyć struktury mapy witryny. Węzeł główny mapy s witryny jest przypisany do `root` podczas konstruowania procesu, aby następnym razem ta metoda jest wywoływana, `root` nie będzie `null`. W związku z tym tak długo, jak `root` nie jest `null` struktury mapy witryny zostanie zwrócony do obiektu wywołującego bez konieczności ponownego tworzenia.

Jeśli główny jest `null`, struktura mapy witryny jest tworzona na podstawie informacji dotyczących produktu i kategorii. Mapy witryny jest konstruowany przez tworzenie `SiteMapNode` wystąpień i następnie tworząca hierarchii za pomocą wywołania `StaticSiteMapProvider` klasy s `AddNode` metody. `AddNode`wykonuje wewnętrznej księgowości, przechowywanie różne `SiteMapNode` wystąpień w `Hashtable`. Przed rozpoczęciem możemy tworzenia hierarchii, Rozpoczniemy przez wywołanie metody `Clear` metodę, która powoduje elementów wewnętrznych `Hashtable`. Następnie `ProductsBLL` klasy s `GetProducts` — metoda i powstałe w ten sposób `ProductsDataTable` są przechowywane w zmiennych lokalnych.

Konstrukcja mapy s lokacji rozpoczyna się od tworzenia węzła głównego i przypisywania go do `root`. Przeciążenia [ `SiteMapNode` Konstruktor s](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.sitemapnode.aspx) używane w tym miejscu i przez to `BuildSiteMap` jest przekazywany następujące informacje:

- Odwołanie do dostawcy mapy witryny (`this`).
- `SiteMapNode` s `Key`. To wymagane, wartość musi być unikatowa dla każdej `SiteMapNode`.
- `SiteMapNode` s `Url`. `Url`jest opcjonalna, ale w przypadku każdego `SiteMapNode` s `Url` wartość musi być unikatowa.
- `SiteMapNode` s `Title`, co jest wymagane.

`AddNode(root)` Dodaje wywołania metody `SiteMapNode` `root` do mapy witryny jako katalogu głównego. Następnie każdego `ProductRow` w `ProductsDataTable` wyliczeniu. Jeśli istnieje już `SiteMapNode` dla bieżącej kategorii produktów s odwołuje się do. W przeciwnym razie nowy `SiteMapNode` dla kategorii jest tworzony i dodawany jako element podrzędny `SiteMapNode``root` za pośrednictwem `AddNode(categoryNode, root)` wywołania metody. Po odpowiedniej kategorii `SiteMapNode` węzeł został znalezione lub utworzone, `SiteMapNode` jest tworzony dla bieżącego produktu i dodawany jako element podrzędny kategorii `SiteMapNode` za pośrednictwem `AddNode(productNode, categoryNode)`. Należy pamiętać, że kategoria `SiteMapNode` s `Url` wartość właściwości jest `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` podczas produktu `SiteMapNode` s `Url` przypisano właściwości `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Te produkty, które mają bazy danych `NULL` wartość ich `CategoryID` są pogrupowane w kategorii `SiteMapNode` których `Title` właściwość ma wartość None i których `Url` właściwość jest ustawiona na pusty ciąg. I decyzję ustawić `Url` na pusty ciąg od `ProductBLL` klasy s `GetProductsByCategory(categoryID)` metody obecnie nie ma możliwości, aby przywrócić tylko te produkty z `NULL` `CategoryID` wartości. Ponadto miała być pokazują, jak renderowania formantów nawigacji `SiteMapNode` bez wartości dla jego `Url` właściwości. I zachęca do rozszerzać ten samouczek, aby Brak `SiteMapNode` s `Url` wskazuje `ProductsByCategory.aspx`, tylko zawiera produkty z `NULL` `CategoryID` wartości.


Po tworzenia mapy witryny, dowolny obiekt jest dodawany do pamięci podręcznej danych przy użyciu zależności bufora SQL na `Categories` i `Products` tabel za pomocą `AggregateCacheDependency` obiektu. Firma Microsoft zbadane, przy użyciu zależności buforu SQL w poprzednim samouczka *przy użyciu zależności buforu SQL*. Niestandardowa witryna dostawcy mapy, jednak używane jest przeciążenie pamięci podręcznej danych s `Insert` — metoda tego możemy kolejnych jeszcze, aby zapoznać się z. To przeciążenie akceptuje jako jego ostatni parametr wejściowy delegata, który jest wywoływany, gdy obiekt jest usunięty z pamięci podręcznej. W szczególności jest przekazywana w nowym [ `CacheItemRemovedCallback` delegować](https://msdn.microsoft.com/en-us/library/system.web.caching.cacheitemremovedcallback.aspx) wskazującego `OnSiteMapChanged` metoda zdefiniowana dalszych szczegółów w `NorthwindSiteMapProvider` klasy.

> [!NOTE]
> Reprezentacja w pamięci mapy witryny jest buforowana za pośrednictwem zmiennej poziomie klasy `root`. Ponieważ istnieje tylko jedno wystąpienie klasy dostawcy mapy witryny niestandardowej i ponieważ to wystąpienie jest współdzielona przez wszystkie wątki w aplikacji sieci web, ta zmienna klasy służy jako pamięci podręcznej. `BuildSiteMap` Metoda również używa pamięci podręcznej danych, jednak tylko w sposób, aby otrzymać powiadomienie, gdy podstawowa z bazy danych w `Categories` lub `Products` tabel zmian. Należy pamiętać, że wartość umieszczane w pamięci podręcznej danych tylko bieżącą datę i godzinę. Dane mapy lokacji jest *nie* umieścić w pamięci podręcznej danych.


`BuildSiteMap` Ukończeniu metody zwracając główny węzeł mapy witryny.

Pozostałe metody są bardzo prosta. `GetRootNodeCore`jest odpowiedzialny za zwrócenie węzła głównego. Ponieważ `BuildSiteMap` zwraca główny `GetRootNodeCore` po prostu zwraca `BuildSiteMap` s zwracają wartość. `OnSiteMapChanged` Zestawy metody `root` do `null` po usunięciu elementu pamięci podręcznej. Z certyfikatem głównym ustawiana dla `null`, przy następnym `BuildSiteMap` jest wywołane, struktura mapy witryny zostanie odbudowany. Ponadto `CachedDate` właściwość zwraca wartość daty i godziny przechowywane w pamięci podręcznej danych, jeśli istnieje takich wartości. Ta właściwość umożliwia przez dewelopera strony podczas ostatniego został buforowane dane mapy witryny.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Krok 7: Rejestrowanie`NorthwindSiteMapProvider`

Aby naszej aplikacji sieci web do używania `NorthwindSiteMapProvider` utworzony w kroku 6 dostawcy mapy witryny, należy zarejestrować je w `<siteMap>` części `Web.config`. W szczególności, Dodaj następujący kod w `<system.web>` element `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

Ten kod znaczników wykonuje dwie czynności: najpierw, oznacza to, że wbudowane `AspNetXmlSiteMapProvider` domyślny dostawca mapy witryny; drugie, rejestruje dostawcy mapy niestandardowej witryny utworzony w kroku 6 o nazwie przyjaznych dla człowieka Northwind.

> [!NOTE]
> Dla dostawcy map witryn znajdujących się w aplikacji s `App_Code` folderu, wartość `type` atrybut jest po prostu nazwę klasy. Alternatywnie dostawcy mapy witryny niestandardowych można zostały utworzone w oddzielnych projektu biblioteki klas z skompilowanego zestawu umieszczone w s aplikacji sieci web `/Bin` katalogu. W takim przypadku `type` wartość atrybutu będzie *Namespace*. *ClassName*, *AssemblyName* .


Po zaktualizowaniu `Web.config`, Poświęć chwilę, aby wyświetlić dowolną stronę z samouczków w przeglądarce. Zauważ, że interfejs nawigacji po lewej stronie nadal zawiera sekcje i samouczki zdefiniowane w `Web.sitemap`. Jest to spowodowane możemy pozostałych `AspNetXmlSiteMapProvider` jako domyślny dostawca. Aby można było utworzyć nawigacji elementu interfejsu użytkownika, który używa `NorthwindSiteMapProvider`, musisz jawnie określić, że dostawca mapy witryny Northwind powinny być używane. Będzie przedstawiono, jak to zrobić w kroku 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Krok 8: Wyświetlanie informacji mapy witryny przy użyciu dostawcy mapy witryny niestandardowej

Z niestandardowej witryny dostawcy mapy utworzona i zarejestrowana w `Web.config`, możemy re gotowe do dodawania formantów nawigacji do `Default.aspx`, `ProductsByCategory.aspx`, i `ProductDetails.aspx` stron w `SiteMapProvider` folderu. Uruchamianie przez otwarcie `Default.aspx` strony i przeciągnij `SiteMapPath` z przybornika do projektanta. Kontrolki ścieżki mapy witryny znajduje się w sekcji nawigacji przybornika.


[![Dodawanie ścieżki mapy witryny do Default.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**Rysunek 16**: ścieżki mapy witryny, aby dodać `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))


Kontrolki ścieżki mapy witryny Wyświetla ślad, wskazujący bieżącą lokalizację s strony w ramach mapy witryny. Dodaliśmy ścieżki mapy witryny na początku strony wzorcowej w *stron wzorcowych i nawigacji w witrynie* samouczka.

Poświęć chwilę, aby wyświetlić tą stronę za pośrednictwem przeglądarki. Ścieżki mapy witryny dodane na rysunku 16 używa dostawcy mapy witryny domyślnej, ściąganie danych z `Web.sitemap`. W związku z tym łączach pokazuje Home &gt; Dostosowywanie mapy witryny, podobnie jak w łączach w prawym górnym rogu.


[![Łączach korzysta z domyślnego dostawcy mapy witryny](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**Rysunek 17**: łączach używa dostawcy mapy witryny domyślnej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))


Aby ścieżki mapy witryny dodane na rysunku 16 korzystają z dostawcy mapy witryny niestandardowej utworzyliśmy w kroku 6, ustaw jej [ `SiteMapProvider` właściwości](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) do Northwind, nazwa możemy przypisana do `NorthwindSiteMapProvider` w `Web.config`. Niestety Projektant w dalszym ciągu używa domyślnego dostawcy mapy witryny, ale w przypadku odwiedzenia strony za pośrednictwem przeglądarki po wprowadzeniu tej zmiany właściwości zobaczysz, że łączach teraz używa dostawcy mapy witryny niestandardowej.


[![Łączach korzysta z NorthwindSiteMapProvider dostawcy mapy witryny niestandardowej](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**Rysunek 18**: łączach korzysta z dostawcy mapy witryny niestandardowe `NorthwindSiteMapProvider` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))


Kontrolki ścieżki mapy witryny Wyświetla więcej funkcjonalności interfejsu użytkownika w `ProductsByCategory.aspx` i `ProductDetails.aspx` stron. Dodawanie ścieżki mapy witryny na tych stronach ustawienie `SiteMapProvider` właściwości zarówno do Northwind. Z `Default.aspx` kliknij łącze Wyświetl produkty napojami, a następnie Link Wyświetl szczegóły, aby Zepołowy Chai. Jak pokazano na rysunku 19, łączach obejmuje sekcji mapy bieżącej lokacji (Chai Zepołowy) i jego elementy nadrzędne: napoje i wszystkich kategorii.


[![Łączach korzysta z NorthwindSiteMapProvider dostawcy mapy witryny niestandardowej](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**Rysunek 19**: łączach korzysta z dostawcy mapy witryny niestandardowe `NorthwindSiteMapProvider` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))


Elementy interfejsu użytkownika innych nawigacji służy oprócz ścieżki mapy witryny, takie jak kontrolki TreeView i Menu. `Default.aspx`, `ProductsByCategory.aspx`, I `ProductDetails.aspx` strony pobierania w tym samouczku, na przykład obejmują kontrolek Menu (patrz rysunek 20). Zobacz [s badanie ASP.NET 2.0 funkcji nawigacji witryny](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) i [za pomocą kontrolki witryny](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) sekcji [skrócone podręczniki platformy ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/) na pełniejsze przyjrzeć się formanty nawigacji i system mapy witryny w programie ASP.NET 2.0.


[![Formant Menu zawiera listę kategorii i produkty](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**Rysunek 20**: Menu sterowania wyświetla każdy kategorie i produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))


Jak wspomniano wcześniej w tym samouczku, struktura mapy witryny można uzyskać dostęp programowo za pomocą `SiteMap` klasy. Poniższy kod Zwraca pierwiastek `SiteMapNode` domyślnego dostawcy:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

Ponieważ `AspNetXmlSiteMapProvider` jest dostawcą domyślnym dla aplikacji, kodu powyżej zwróci węzeł główny zdefiniowany w `Web.sitemap`. Aby odwołać dostawcy mapy witryny innej niż domyślna, należy użyć `SiteMap` klasy s [ `Providers` właściwości](https://msdn.microsoft.com/en-us/library/system.web.sitemap.providers.aspx) w następujący sposób:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

Gdzie *nazwa* jest nazwa dostawcy mapy niestandardowej witryny (Northwind dla aplikacji sieci web).

Aby uzyskiwanie dostępu do członka, które są specyficzne dla dostawcy mapy witryny, należy użyć `SiteMap.Providers["name"]` można pobrać wystąpienia dostawcy i rzutować go do odpowiedniego typu. Na przykład, aby wyświetlić `NorthwindSiteMapProvider` s `CachedDate` właściwości strony ASP.NET, użyj następującego kodu:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> Należy przetestować funkcji zależności buforu SQL. Po przejściu na stronę `Default.aspx`, `ProductsByCategory.aspx`, i `ProductDetails.aspx` stron, przejdź do jednej z samouczków w edytowanie, wstawianie i usuwanie sekcji i Zmień nazwę kategorii lub produktu. Następnie wróć do jednej ze stron w `SiteMapProvider` folderu. Zakładając, że dla mechanizmu sondowania zmian do podstawowej bazy danych należy pamiętać, upłynęło dostatecznie dużo czasu, mapy witryny powinny zostać uaktualnione do wyświetlenia nowego produktu lub nazwa kategorii.


## <a name="summary"></a>Podsumowanie

Platforma ASP.NET 2.0 obejmuje funkcje mapy witryny s `SiteMap` klasy, określa liczbę wbudowanych nawigacji sieci Web i domyślnego dostawcy mapy witryny oczekuje mapy witryny utrwalone w pliku XML. Aby można było użyć mapy witryny z z innego źródła takich jak z bazy danych, aplikacja — architektura s lub zdalnej usługi sieci Web, należy utworzyć niestandardową witrynę mapy dostawcy. Obejmuje to tworzenie klasy pochodnej, bezpośrednio lub pośrednio, z `SiteMapProvider` klasy.

W tym samouczku widzieliśmy jak utworzyć niestandardową witrynę mapy dostawcy mapy witryny na podstawie produktów i kategorii informacji ubitych z architektury aplikacji. Nasze Dostawca rozszerzony `StaticSiteMapProvider` klasy i wiązało się tworzenie `BuildSiteMap` metodę, która pobrać dane, skonstruowany hierarchii mapy witryny i strukturze wynikowe w zmiennej poziomie klasy w pamięci podręcznej. Użyliśmy zależności bufora SQL przy użyciu funkcji wywołania zwrotnego w celu unieważnienia zapisane w pamięci podręcznej struktury, kiedy podstawowa `Categories` lub `Products` dane są modyfikowane.

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Zapisywanie map witryn w programie SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) i [dostawcy mapy witryny SQL był czas oczekiwania](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Wzór dostawcy s przyjrzeć się ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Zestaw narzędzi do dostawcy](https://msdn.microsoft.com/en-us/asp.net/aa336558.aspx)
- [Badanie ASP.NET 2.0 funkcji nawigacji witryny s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Dave Gardner, Nowak Zack Teresa Murphy i Bernadette Leigh. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Dalej](building-a-custom-database-driven-site-map-provider-vb.md)
