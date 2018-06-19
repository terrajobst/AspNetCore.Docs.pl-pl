---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: Wzorzec/szczegół filtrowania przez dwie strony (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku będziesz wdrożymy ten wzorzec przy użyciu widoku GridView, aby wyświetlić listę dostawców w bazie danych. Każdy dostawca wiersza w widoku GridView będzie zawierać Vie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0858bf9c8dc380898647293825145654ac1dbc34
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887792"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Wzorzec/szczegół filtrowania przez dwie strony (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) lub [pobierania plików PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> W tym samouczku będziesz wdrożymy ten wzorzec przy użyciu widoku GridView, aby wyświetlić listę dostawców w bazie danych. Każdy dostawca wiersza w widoku GridView będzie zawierać łącze Wyświetl produkty, po kliknięciu, potrwa użytkownikowi na osobnej stronie który zawiera listę tych produktów dla wybranego dostawcy.


## <a name="introduction"></a>Wprowadzenie

W poprzednich dwóch samouczki widzieliśmy jak [wyświetlanie raportów wzorzec/szczegół w jednej strony sieci web przy użyciu DropDownLists](master-detail-filtering-with-a-dropdownlist-cs.md) do [wyświetlania rekordów "główny" i formantu widoku GridView lub widoku DetailsView](master-detail-filtering-with-two-dropdownlists-cs.md) do wyświetlenia " szczegółowe informacje." Inny wspólnego wzorca używane w raportach głównych/szczegółów jest głównym rekordów na jednej stronie sieci web oraz dane wyświetlane na innym. Forum witryny sieci Web, takiej jak [fora ASP.NET](https://forums.asp.net/), jest doskonałym przykładem tego wzorca w praktyce. Fora ASP.NET składają się z różnych fora wprowadzenie, formularzy sieci Web, kontrolek prezentacji danych i tak dalej. Każdy forum składa się z wielu wątków i każdy wątek składa się z liczbę wpisów. Na stronie głównej fora programu ASP.NET fora są wyświetlane. Kliknięcie na forum whisks do `ShowForum.aspx` strony, której znajduje się lista wątków na tym forum. Podobnie, klikając w wątku przejście do `ShowPost.aspx`, który zawiera wpisy dla wątku, który został kliknięty.

W tym samouczku będziesz wdrożymy ten wzorzec przy użyciu widoku GridView, aby wyświetlić listę dostawców w bazie danych. Każdy dostawca wiersza w widoku GridView będzie zawierać łącze Wyświetl produkty, po kliknięciu, potrwa użytkownikowi na osobnej stronie który zawiera listę tych produktów dla wybranego dostawcy.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Krok 1: Dodawanie`SupplierListMaster.aspx`i`ProductsForSupplierDetails.aspx`strony do`Filtering`folderu

Podczas definiowania układu strony trzeciej samouczka dodaliśmy wiele stron "starter" w `BasicReporting`, `Filtering`, i `CustomFormatting` folderów. Jednak firma Microsoft nie dodano stronę początkową dla tego samouczka w tym czasie, więc Poświęć chwilę, aby dodać dwie nowe strony do `Filtering` folder: `SupplierListMaster.aspx` i `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` Wyświetla listę rekordów "główny" (dostawcy) podczas `ProductsForSupplierDetails.aspx` wyświetli produktów dla wybranego dostawcy.

Podczas tworzenia tych dwóch nowych stron Zadbaj skojarzyć je z `Site.master` strony wzorcowej.


![Dodaj SupplierListMaster.aspx i ProductsForSupplierDetails.aspx strony do filtrowania folderu](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Rysunek 1**: Dodaj `SupplierListMaster.aspx` i `ProductsForSupplierDetails.aspx` strony do `Filtering` folderu


Podczas dodawania nowej strony do projektu, upewnij się również zaktualizować plik mapy witryny, `Web.sitemap`, w związku z tym. W tym samouczku po prostu Dodaj `SupplierListMaster.aspx` strony do mapy witryny, używając następującej zawartości XML jako element podrzędny filtrowania raportów `<siteMapNode>` elementu:


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Pomaga zautomatyzować proces aktualizowania pliku mapy witryny podczas dodawania nowego ASP.NET strony przy użyciu [K. Scott Allen](http://odetocode.com/Blogs/scott/)do bezpłatnego programu Visual Studio [makra mapy witryny](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Krok 2: Wyświetlanie listy dostawcy`SupplierListMaster.aspx`

Z `SupplierListMaster.aspx` i `ProductsForSupplierDetails.aspx` strony utworzone, naszych następnym krokiem jest tworzenie dostawców w widoku GridView `SupplierListMaster.aspx`. Na stronie Dodaj element GridView i powiązać ją z nowego elementu ObjectDataSource. Należy używać tego elementu ObjectDataSource `SuppliersBLL` klasy `GetSuppliers()` metody do zwrócenia wszystkich dostawców.


[![Wybierz klasę SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Rysunek 2**: Wybierz `SuppliersBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetSuppliers()](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Rysunek 3**: Konfigurowanie ObjectDataSource użyć `GetSuppliers()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image7.png))


Należy uwzględnić łącze do zatytułowany Wyświetl produkty każdego wiersza w widoku GridView, po kliknięciu przyjmuje użytkownikowi `ProductsForSupplierDetails.aspx` przekazując wybranego wiersza `SupplierID` wartość za pośrednictwem ciąg zapytania. Na przykład, jeśli użytkownik kliknie łącze Wyświetl produkty dostawcy Traders Tokio (która zawiera `SupplierID` wartość 4), powinny być przesyłane do `ProductsForSupplierDetails.aspx?SupplierID=4`.

W tym celu należy dodać [pole hiperłącza HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) do widoku GridView, która dodaje hiperłącza do każdego wiersza w widoku GridView. Uruchomić, klikając łącza Edytuj kolumny w widoku GridView tagów inteligentnych. Następnie wybierz pole hiperłącza HyperLinkField z listy w lewym górnym rogu i kliknij przycisk Dodaj, aby dołączyć pole hiperłącza HyperLinkField w oknie Lista pól w widoku GridView.


[![Dodaj pole hiperłącza HyperLinkField do widoku GridView](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Rysunek 4**: Dodaj pole hiperłącza HyperLinkField do widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image10.png))


Pole hiperłącza HyperLinkField może być skonfigurowana do używania tego samego tekstu lub adres URL, wartości łącza w każdym wierszu widoku GridView lub można utworzyć te wartości na dane powiązane z każdego określonego wiersza. Aby określić statyczny wartości we wszystkich wierszach Użyj pole hiperłącza HyperLinkField `Text` lub `NavigateUrl` właściwości. Ponieważ chcemy łącze tekst, który ma być taka sama dla wszystkich wierszy, ustaw pole hiperłącza HyperLinkField `Text` właściwości do widoku produktów.


[![Wartość właściwości Text pole hiperłącza HyperLinkField Wyświetl produkty](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Rysunek 5**: Ustaw pole hiperłącza HyperLinkField `Text` właściwości Wyświetl produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image13.png))


Aby ustawić tekst lub wartości adresu URL opartego na danych powiązane z wiersza elementu GridView, określ tekst pola danych lub adres URL wartości powinny być pobierane z w `DataTextField` lub `DataNavigateUrlFields` właściwości. `DataTextField` może być ustawiony tylko na jedno pole danych; `DataNavigateUrlFields`, jednak może być ustawiony na rozdzielana przecinkami lista pól danych. Musimy często podstawowa tekst lub adres URL na kombinacji wartości pola danych bieżącego wiersza i niektóre statycznych znaczników. W tym samouczku, na przykład chcemy łącza pole hiperłącza HyperLinkField adres URL, który można `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, gdzie *`supplierID`* każdego widoku GridView wiersza `SupplierID` wartość. Powiadomienie, że potrzebujemy obu statyczna i opartych na danych wartości w tym miejscu: `ProductsForSupplierDetails.aspx?SupplierID=` część adresu URL łącza jest statyczny, podczas gdy *`supplierID`* części jest oparte na danych zgodnie z jego wartość wynosi każdego wiersza własnych `SupplierID` wartość.

Aby wskazać kombinację wartości statyczne i opartych na danych, użyj `DataTextFormatString` i `DataNavigateUrlFormatString` właściwości. W tych właściwości wprowadź statyczny znaczników zgodnie z potrzebami, a następnie użyć znacznika `{0}` , w którym ma wartość pola określone w `DataTextField` lub `DataNavigateUrlFields` właściwości, które mają być wyświetlane. Jeśli `DataNavigateUrlFields` właściwość ma wiele pól określone użycie `{0}` w przypadku, gdy chcesz pierwsza wartość pola wstawiony, `{1}` dla drugiej wartości pola i tak dalej.

Stosowanie to się z naszym samouczkiem, musimy ustawić `DataNavigateUrlFields` właściwości `SupplierID`, ponieważ jest w polu danych, dla którego wartość należy dostosować na podstawie na wiersz, i `DataNavigateUrlFormatString` właściwości `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Skonfiguruj pole hiperłącza HyperLinkField uwzględnienie prawidłowego adresu URL łącza ustalane na podstawie IDDostawcy](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Rysunek 6**: Konfigurowanie pole hiperłącza HyperLinkField, aby dołączyć odpowiednie łącze adres URL oparty na `SupplierID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image16.png))


Po dodaniu pole hiperłącza HyperLinkField, możesz dostosować i Zmień kolejność pól w widoku GridView. Następujący kod przedstawia widoku GridView po wprowadzono I pewnych niewielkich dostosowań na poziomie pola.


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Poświęć chwilę, aby wyświetlić `SupplierListMaster.aspx` strony za pośrednictwem przeglądarki. Jak pokazano na rysunku 7, strony obecnie zawiera listę wszystkich dostawców w tym łącze Wyświetl produkty. Kliknięcie Wyświetl produkty łącza spowoduje przejście do `ProductsForSupplierDetails.aspx`, przechodzącą wzdłuż dostawcy `SupplierID` w zmiennej querystring.


[![Każdy dostawca wiersz zawiera Link produktów widoku](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Rysunek 7**: każdy wiersz dostawcy zawiera łącze Wyświetl produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Krok 3: Wyświetlanie listy produktów dostawcy`ProductsForSupplierDetails.aspx`

W tym momencie `SupplierListMaster.aspx` strony wysyła użytkownikom `ProductsForSupplierDetails.aspx`, przekazywanie wybranego dostawcy `SupplierID` w zmiennej querystring. Ostatnim krokiem tego samouczka jest wyświetlić produktów w widoku GridView w `ProductsForSupplierDetails.aspx` których `SupplierID` jest równe `SupplierID` przekazany za pośrednictwem ciąg zapytania. Do wykonania tej rozpoczęcia, dodając element GridView do `ProductsForSupplierDetails.aspx` strony przy użyciu nowej kontrolki ObjectDataSource o nazwie `ProductsBySupplierDataSource` który wywołuje `GetProductsBySupplierID(supplierID)` metody `ProductsBLL` klasy.


[![Dodaj nowy element ObjectDataSource o nazwie ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Rysunek 8**: Dodaj nowy element ObjectDataSource o nazwie `ProductsBySupplierDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![Wybierz klasę ProductsBLL](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Rysunek 9**: Wybierz `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![Ma ObjectDataSource wywołania metody GetProductsBySupplierID(supplierID)](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Na rysunku nr 10**: ma wywołać element ObjectDataSource `GetProductsBySupplierID(supplierID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image28.png))


Ostatni krok kreatora konfigurowania źródła danych zapyta firmie Microsoft w celu zapewnienia źródło `GetProductsBySupplierID(supplierID)` metody *`supplierID`* parametru. Aby użyć wartości querystring, ustaw źródło parametru QueryString, a następnie wprowadź nazwę wartości querystring w polu tekstowym pole QueryStringField (`SupplierID`).


[![Wypełnij IDDostawcy wartości parametru z wartością Querystring IDDostawcy](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Rysunek 11**: wypełnić *`supplierID`* wartości parametru z `SupplierID` wartości Querystring ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image31.png))


To wszystko jest do niego! Przedstawia rysunek 12 `ProductsForSupplierDetails.aspx` strony po odwiedzeniu, klikając łącze Traders Tokio z `SupplierListMaster.aspx`.


[![Produkty dostarczane przez Traders Tokio są wyświetlane.](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Rysunek 12**: przedstawiono produkty dostarczane przez Traders Tokio ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Wyświetlanie informacji o dostawcy w`ProductsForSupplierDetails.aspx`

Jak pokazano na rysunku 12, `ProductsForSupplierDetails.aspx` strona po prostu zawiera listę produktów, które są dostarczane przez `SupplierID` określony w zmiennej querystring. Ktoś wysyłane bezpośrednio do tej strony, jednak nie wiedział, że rysunek 12 jest wyświetlany Traders Tokio produktów. Aby rozwiązać ten problem wyświetlić informacje o dostawcach na tej stronie, a także.

Rozpocznij od dodania FormView powyżej produktów widoku GridView. Utwórz nowe kontrolki ObjectDataSource o nazwie `SuppliersDataSource` który wywołuje `SuppliersBLL` klasy `GetSupplierBySupplierID(supplierID)` metody.


[![Wybierz klasę SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Rysunek 13**: Wybierz `SuppliersBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![Ma ObjectDataSource wywołania metody GetSupplierBySupplierID(supplierID)](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Rysunek 14**: ma wywołać element ObjectDataSource `GetSupplierBySupplierID(supplierID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image40.png))


Jak `ProductsBySupplierDataSource`, ma *`supplierID`* parametru przypisana wartość `SupplierID` wartości querystring.


[![Wypełnij IDDostawcy wartości parametru z wartością Querystring IDDostawcy](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Rysunek 15**: wypełnić *`supplierID`* wartości parametru z `SupplierID` wartości Querystring ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image43.png))


Podczas tworzenia wiązania FormView ObjectDataSource w widoku Projekt, Visual Studio automatycznie utworzy FormView `ItemTemplate`, `InsertItemTemplate`, i `EditItemTemplate` z formantami etykiety i pola tekstowego w sieci Web dla każdego pola danych zwróconych przez Element ObjectDataSource. Ponieważ chcemy tylko do wyświetlania działanie informacji dostawca wolnego usunąć `InsertItemTemplate` i `EditItemTemplate`. Następnie edytuj właściwości ItemTemplate, tak aby wyświetlone nazwy firmy dostawcy w `<h3>` element i adresu, miasta, kraju i numer telefonu poniżej nazwę firmy. Alternatywnie możesz ręcznie ustawić FormView `DataSourceID` i Utwórz `ItemTemplate` znaczników, jak robiliśmy w "[wyświetlanie danych z ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" samouczka.

Po te operacje edycji znaczników deklaratywne FormView powinien wyglądać podobny do następującego:


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

Zrzut ekranu przedstawia rysunek 16 `ProductsForSupplierDetails.aspx` strony po uwzględnione informacje wymienione powyżej.


[![Podsumowanie dotyczące dostawcy zawiera listę produktów](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Rysunek 16**: Lista produktów zawiera podsumowanie dotyczące dostawcy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Stosowanie ostatecznych dotyka dla`ProductsForSupplierDetails.aspx`interfejsu użytkownika

Zwiększające użytkownika obsługi tego raportu są kilka dodatków możemy powinien dokonać `ProductsForSupplierDetails.aspx` strony. Obecnie jedynym sposobem użytkownik może przejść od `ProductsForSupplierDetails.aspx` strony powrót do listy dostawców ma kliknij przycisk Wstecz w przeglądarce. Dodajmy formantu hiperłącza do `ProductsForSupplierDetails.aspx` strona, która łączy z powrotem do `SupplierListMaster.aspx`, zapewniając dla użytkownika powrócić do listy głównej w inny sposób.


[![Dodawanie formantu hiperłącza do przyjęcia użytkownika z powrotem do SupplierListMaster.aspx](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Rysunek 17**: Dodawanie formantu hiperłącza, aby odzyskać użytkownika do `SupplierListMaster.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image49.png))


Gdy użytkownik kliknie łącze Wyświetl produkty dla dostawcy, która nie ma żadnych produktów `ProductsBySupplierDataSource` ObjectDataSource w `ProductsForSupplierDetails.aspx` nie zwróciła żadnych wyników. GridView powiązany element ObjectDataSource nie będzie renderować żadnych znaczników powodujące puste region na stronie w przeglądarce. Aby dokładniej komunikować się z użytkownikiem że nie istnieją żadne produktów skojarzonych z wybranego dostawcy możemy ustawić w widoku GridView `EmptyDataText` właściwości komunikat chcemy wyświetlane, gdy wystąpi taka sytuacja. I ustawiono tę właściwość na "Brak produktów dostarczone przez tego dostawcę"

Domyślnie wszystkich dostawców w bazie danych Northwinds Podaj co najmniej jeden produkt. Jednak w tym samouczku I zostały ręcznie zmodyfikowane `Products` tabeli, dzięki czemu dostawcy Escargots Nouveaux nie jest już skojarzony z żadnym produktów. Rysunek 18 zawiera strony szczegółów Escargots Nouveaux po dokonaniu tej zmiany.


[![Użytkownicy jest także informacja, że dostawca nie zapewnia żadnych produktów](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Rysunek 18**: użytkowników jest także informacja, że dostawca nie zapewnia żadnych produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>Podsumowanie

Gdy wzorzec/szczegół raporty można wyświetlić głównego i szczegółów rekordy na jednej stronie, w wiele witryn sieci Web one są rozdzielone przez dwie strony sieci web. W tym samouczku analizujemy sposobu wdrażania raportu wzorzec/szczegół za dostawców wymienionych w widoku GridView "główny" strony sieci web i skojarzone wymienionego na stronie "szczegóły". Każdy wiersz dostawcy na głównej stronie sieci web zawiera łącze do strony szczegółów, który przekazał wzdłuż wiersza `SupplierID` wartość. Takie łącza określonego wiersza można łatwo dodać przy użyciu pole hiperłącza HyperLinkField w widoku GridView.

Na stronie szczegółów pobierania tych produktów dla określonego dostawcy osiągnąć wywoływania `ProductsBLL` klasy `GetProductsBySupplierID(supplierID)` metody. *`supplierID`* Określono wartość parametru deklaratywnie przy użyciu ciąg zapytania jako źródło parametru. Analizujemy również sposób wyświetlania szczegółów dostawcy na stronie szczegółów za pomocą FormView.

Nasze [następny samouczek](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) jest ostatnim w raportach wzorzec/szczegół. Wyjaśniono, jak wyświetlić listę produktów w widoku GridView, gdzie każdy wiersz zawiera przycisk Wybierz. Kliknięcie przycisku Wybierz są wyświetlane takie szczegóły tego produktu w kontrolce widoku DetailsView na tej samej stronie.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Hilton Giesenow. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-filtering-with-two-dropdownlists-cs.md)
> [dalej](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
