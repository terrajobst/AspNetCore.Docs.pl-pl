---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
title: Filtrowanie rekordu głównego/szczegółów na dwóch stronach (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku będziesz wdrażamy tego wzorca przy użyciu GridView, aby wyświetlić listę dostawców w bazie danych. Każdy wiersz dostawcy w widoku GridView będzie zawierać Vie...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 361d6a44-3f1f-4daf-85df-d4c2b8bf065d
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 290c4eba6f77a6006d424c3f05b77c1c128026b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754231"
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Filtrowanie rekordu głównego/szczegółów na dwóch stronach (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_9_VB.exe) lub [Pobierz plik PDF](master-detail-filtering-across-two-pages-vb/_static/datatutorial09vb1.pdf)

> W tym samouczku będziesz wdrażamy tego wzorca przy użyciu GridView, aby wyświetlić listę dostawców w bazie danych. Każdy wiersz dostawcy w widoku GridView będzie zawierać link Wyświetl produkty, po kliknięciu spowoduje przejście użytkownikowi na osobnej stronie, zawiera listę tych produktów dla wybranego dostawcy.


## <a name="introduction"></a>Wprowadzenie

W poprzednich samouczkach dwóch widzieliśmy sposób wyświetlania raportów wzorzec/szczegół w pojedynczej strony sieci web za pomocą kontrolek DROPDOWNLIST Wyświetl rekordy "główną" oraz [GridView](master-detail-filtering-with-a-dropdownlist-vb.md) lub [DetailsView](master-detail-filtering-with-two-dropdownlists-vb.md) formantu, aby wyświetlić " szczegółowe informacje." Inny typowy wzorzec używany w przypadku raportów wzorzec/szczegół jest głównym rekordów na jedną stronę sieci web i szczegółowe informacje wyświetlane na innej. Forum witryny sieci Web, takiej jak [fora ASP.NET](https://forums.asp.net/), jest doskonałym przykładem tego wzorca, w praktyce. Fora ASP.NET składają się z różnych forów wprowadzenie do formularzy sieci Web, formanty prezentacji danych i tak dalej. Każde forum składa się z wielu wątków i każdy wątek składa się z liczbą wpisów. Na stronie głównej forów ASP.NET fora są wyświetlane. Kliknięcie na forum, na którym można whisks `ShowForum.aspx` strony, która wyświetla listę wątków dla wybranego forum. Podobnie, klikając przycisk w wątku spowoduje przejście do `ShowPost.aspx`, które wyświetla wpisy w wątku, który został kliknięty.

W tym samouczku będziesz wdrażamy tego wzorca przy użyciu GridView, aby wyświetlić listę dostawców w bazie danych. Każdy wiersz dostawcy w widoku GridView będzie zawierać link Wyświetl produkty, po kliknięciu spowoduje przejście użytkownikowi na osobnej stronie, zawiera listę tych produktów dla wybranego dostawcy.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Krok 1. Dodawanie`SupplierListMaster.aspx`i`ProductsForSupplierDetails.aspx`strony do`Filtering`folderu

Podczas definiowania układu strony w to trzeci samouczek dodaliśmy wiele stron "starter" w `BasicReporting`, `Filtering`, i `CustomFormatting` folderów. Jednak firma Microsoft nie dodano stronę początkowy na potrzeby tego samouczka w tym czasie, więc warto poświęcić chwilę, aby dodać dwa nowe strony, aby `Filtering` folder: `SupplierListMaster.aspx` i `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` zostanie wyświetlona lista rekordów "główną" (dostawcami) podczas `ProductsForSupplierDetails.aspx` wyświetli produktów dla wybranego dostawcy.

Podczas tworzenia tych dwóch nowych stron pewni skojarzyć je z `Site.master` strony wzorcowej.


![Dodaj SupplierListMaster.aspx i stron ProductsForSupplierDetails.aspx do filtrowania folderu](master-detail-filtering-across-two-pages-vb/_static/image1.png)

**Rysunek 1**: Dodaj `SupplierListMaster.aspx` i `ProductsForSupplierDetails.aspx` strony do `Filtering` folderu


Podczas dodawania nowych stron do projektu, upewnij się również zaktualizować plik mapy witryny `Web.sitemap`, odpowiednio. Na potrzeby tego samouczka wystarczy dodać `SupplierListMaster.aspx` strony do mapy witryny przy użyciu następującej zawartości XML jako element podrzędny filtrowanie raportów `<siteMapNode>` elementu:


[!code-xml[Main](master-detail-filtering-across-two-pages-vb/samples/sample1.xml)]

> [!NOTE]
> Możesz pomóc zautomatyzować proces aktualizowania pliku mapy witryny, podczas dodawania nowych ASP.NET strony za pomocą [K. Scott Allen](http://odetocode.com/Blogs/scott/)użytkownika bezpłatnego programu Visual Studio [makra mapy witryny](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Krok 2: Wyświetlanie listy dostawcy`SupplierListMaster.aspx`

Za pomocą `SupplierListMaster.aspx` i `ProductsForSupplierDetails.aspx` stron utworzonych, naszym kolejnym krokiem jest utworzenie GridView dostawców w `SupplierListMaster.aspx`. Na stronie Dodaj GridView i powiązać ją z nowego elementu ObjectDataSource. Należy użyć tej kontrolki ObjectDataSource `SuppliersBLL` klasy `GetSuppliers()` metody do zwrócenia wszystkich dostawców.


[![Wybierz klasę SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image3.png)](master-detail-filtering-across-two-pages-vb/_static/image2.png)

**Rysunek 2**: Wybierz `SuppliersBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image4.png))


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetSuppliers()](master-detail-filtering-across-two-pages-vb/_static/image6.png)](master-detail-filtering-across-two-pages-vb/_static/image5.png)

**Rysunek 3**: Konfigurowanie kontrolki ObjectDataSource do użycia `GetSuppliers()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image7.png))


Musimy uwzględnić łącze pod tytułem Wyświetl produkty w każdym wierszu GridView, kliknięcie powoduje otwarcie `ProductsForSupplierDetails.aspx` przekazywania w zaznaczonym wierszu `SupplierID` korzyści dzięki ciąg zapytania. Na przykład, jeśli użytkownik kliknie link Wyświetl produkty dostawcy handlowców Tokio (który ma `SupplierID` wartość 4), powinny być przesyłane do `ProductsForSupplierDetails.aspx?SupplierID=4`.

Aby to zrobić, należy dodać [pole hiperłącza HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) do kontrolki GridView, która dodaje hiperłącze do każdego wiersza w widoku GridView. Rozpocznij, klikając link Edytowanie kolumn z GridView tagu inteligentnego. Następnie wybierz pole hiperłącza HyperLinkField z listy w lewym górnym rogu i kliknij przycisk Dodaj, aby uwzględnić pole hiperłącza HyperLinkField na liście pól w widoku GridView.


[![Dodaj pole hiperłącza HyperLinkField do widoku GridView](master-detail-filtering-across-two-pages-vb/_static/image9.png)](master-detail-filtering-across-two-pages-vb/_static/image8.png)

**Rysunek 4**: Dodaj pole hiperłącza HyperLinkField do kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image10.png))


Pole hiperłącza HyperLinkField można skonfigurować do używania tego samego tekstu lub adres URL wartości łącze w każdym wierszu GridView lub można oprzeć te wartości na podstawie wartości danych powiązany z każdego określonego wiersza. Aby określić statyczny wartości we wszystkich wierszach Użyj pole hiperłącza HyperLinkField `Text` lub `NavigateUrl` właściwości. Ponieważ chcemy, aby tekst łącza być takie same we wszystkich wierszach, ustaw pole hiperłącza HyperLinkField `Text` właściwości Wyświetl produkty.


[![Ustawienie właściwości Text pole hiperłącza HyperLinkField Wyświetl produkty](master-detail-filtering-across-two-pages-vb/_static/image12.png)](master-detail-filtering-across-two-pages-vb/_static/image11.png)

**Rysunek 5**: Ustaw pole hiperłącza HyperLinkField `Text` właściwości Wyświetl produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image13.png))


Aby ustawić tekst lub wartości adresu URL być oparte na danych bazowych, powiązany z wiersza w widoku GridView, określ dane pola, tekst lub wartości adresu URL powinny być ściągane z w `DataTextField` lub `DataNavigateUrlFields` właściwości. `DataTextField` można ustawić tylko na jedno pole danych; `DataNavigateUrlFields`, jednak może być równa rozdzielana przecinkami lista pól danych. Musimy często tekstu lub adres URL na kombinacji wartości pola danych bieżący wiersz i niektóre statyczne znaczników. W tym samouczku, na przykład chcemy, aby adres URL łącza pole hiperłącza HyperLinkField można `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, gdzie *`supplierID`* wiersz każdego GridView `SupplierID` wartość. Zwróć uwagę, że potrzebujemy statycznych i opartych na danych wartości w tym miejscu: `ProductsForSupplierDetails.aspx?SupplierID=` część adresu URL łącza jest statyczna, natomiast *`supplierID`* część jest oparte na danych jako jej wartość jest każdy wiersz na własnych `SupplierID` wartość.

Aby określić kombinacji wartości statyczne i opartych na danych, należy użyć `DataTextFormatString` i `DataNavigateUrlFormatString` właściwości. W ramach tych właściwości wprowadź statyczny znaczników, zgodnie z potrzebami, a następnie za pomocą znacznika `{0}` gdzie ma zostać zwrócona wartość pola określone w `DataTextField` lub `DataNavigateUrlFields` właściwości są wyświetlane. Jeśli `DataNavigateUrlFields` właściwość ma wiele pól określone użycie `{0}` w przypadku, gdy chcesz, aby pierwsza wartość pola wstawiony, `{1}` dla drugiej wartości pola i tak dalej.

Zastosowanie do naszego samouczka, musimy `DataNavigateUrlFields` właściwości `SupplierID`, ponieważ pole danych, którego wartość należy dostosować na podstawie na wiersz i `DataNavigateUrlFormatString` właściwość `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Konfigurowanie pole hiperłącza HyperLinkField w celu uwzględnienia prawidłowego adresu URL łącza na podstawie IDDostawcy](master-detail-filtering-across-two-pages-vb/_static/image15.png)](master-detail-filtering-across-two-pages-vb/_static/image14.png)

**Rysunek 6**: Konfigurowanie pole hiperłącza HyperLinkField, aby dołączyć odpowiednie łącze adres URL oparty na `SupplierID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image16.png))


Po dodaniu pole hiperłącza HyperLinkField, możesz dostosować i zmienianie kolejności pól w widoku GridView. Następujący kod przedstawia widoku GridView po przeze mnie niektóre niewielkich dostosowań na poziomie pola.


[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample2.aspx)]

Poświęć chwilę, aby wyświetlić `SupplierListMaster.aspx` strony za pośrednictwem przeglądarki. Jak pokazano na rysunku 7, strony obecnie zawiera listę wszystkich dostawców, w tym link Wyświetl produkty. Kliknięcie na Wyświetl produkty łącza spowoduje przejście do `ProductsForSupplierDetails.aspx`, przekazując u dostawcy `SupplierID` w zmiennej querystring.


[![Każdy wiersz dostawcy zawiera łącze Wyświetl produkty](master-detail-filtering-across-two-pages-vb/_static/image18.png)](master-detail-filtering-across-two-pages-vb/_static/image17.png)

**Rysunek 7**: każdy wiersz dostawcy zawiera łącze Wyświetl produkty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Krok 3: Wyświetlanie listy produktów dostawcy`ProductsForSupplierDetails.aspx`

W tym momencie `SupplierListMaster.aspx` strony wysyła użytkownikowi `ProductsForSupplierDetails.aspx`, przekazując wybranego dostawcy `SupplierID` w zmiennej querystring. Ostatnim krokiem tego samouczka jest wyświetlanie produktów w GridView w `ProductsForSupplierDetails.aspx` którego `SupplierID` jest równa `SupplierID` przekazany ciąg zapytania. Aby osiągnąć ten start, dodając GridView do `ProductsForSupplierDetails.aspx` stronę, korzystając z nowego formantu ObjectDataSource o nazwie `ProductsBySupplierDataSource` wywołującej `GetProductsBySupplierID(supplierID)` metody z `ProductsBLL` klasy.


[![Dodawanie nowego elementu ObjectDataSource, o nazwie ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-vb/_static/image21.png)](master-detail-filtering-across-two-pages-vb/_static/image20.png)

**Rysunek 8**: Dodaj nazwę nowej kontrolki ObjectDataSource `ProductsBySupplierDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image22.png))


[![Wybierz klasę ProductsBLL](master-detail-filtering-across-two-pages-vb/_static/image24.png)](master-detail-filtering-across-two-pages-vb/_static/image23.png)

**Rysunek 9**: Wybierz `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image25.png))


[![Masz ObjectDataSource, wywołaj metodę GetProductsBySupplierID(supplierID)](master-detail-filtering-across-two-pages-vb/_static/image27.png)](master-detail-filtering-across-two-pages-vb/_static/image26.png)

**Na rysunku nr 10**: mają wywołania elementu ObjectDataSource `GetProductsBySupplierID(supplierID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image28.png))


Ostatnim krokiem w Kreatorze konfigurowania źródła danych prosi nam zapewnić źródło `GetProductsBySupplierID(supplierID)` metody *`supplierID`* parametru. Aby użyć wartości querystring, ustaw źródło parametru QueryString, a następnie wprowadź nazwę wartości querystring w do użycia w polu tekstowym vlastnost QueryStringField (`SupplierID`).


[![Wypełnij IDDostawcy wartości parametru z wartości Querystring w IDDostawcy](master-detail-filtering-across-two-pages-vb/_static/image30.png)](master-detail-filtering-across-two-pages-vb/_static/image29.png)

**Rysunek 11**: Wypełnij *`supplierID`* wartość parametru `SupplierID` wartości Querystring ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image31.png))


To wszystko jest do niego! Przedstawia rysunek 12 `ProductsForSupplierDetails.aspx` stronie podczas odwiedzania, klikając link handlowców Tokio z `SupplierListMaster.aspx`.


[![Produkty dostarczane przez podmioty Tokio są wyświetlane.](master-detail-filtering-across-two-pages-vb/_static/image33.png)](master-detail-filtering-across-two-pages-vb/_static/image32.png)

**Rysunek 12**: przedstawiono produkty dostarczane przez podmioty Tokio ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Wyświetlanie informacji o dostawcy w`ProductsForSupplierDetails.aspx`

Jak pokazano na rysunku 12, `ProductsForSupplierDetails.aspx` strona po prostu zawiera listę produktów, które są dostarczane przez `SupplierID` określone w zmiennej querystring. Ktoś wysyłane bezpośrednio do tej strony, jednak nie wiadomo, że rysunek 12 są wyświetlane Tokio Traders products. Aby rozwiązać ten problem możemy wyświetlić informacje o dostawcach na tej stronie, jak również.

Rozpocznij, dodając FormView powyżej produktów GridView. Tworzenie formantu ObjectDataSource o nazwie `SuppliersDataSource` wywołującej `SuppliersBLL` klasy `GetSupplierBySupplierID(supplierID)` metody.


[![Wybierz klasę SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image36.png)](master-detail-filtering-across-two-pages-vb/_static/image35.png)

**Rysunek 13**: Wybierz `SuppliersBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image37.png))


[![Masz ObjectDataSource, wywołaj metodę GetSupplierBySupplierID(supplierID)](master-detail-filtering-across-two-pages-vb/_static/image39.png)](master-detail-filtering-across-two-pages-vb/_static/image38.png)

**Rysunek 14**: mają wywołania elementu ObjectDataSource `GetSupplierBySupplierID(supplierID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image40.png))


Podobnie jak w przypadku `ProductsBySupplierDataSource`, mają *`supplierID`* parametru przypisana wartość `SupplierID` wartości querystring.


[![Wypełnij IDDostawcy wartości parametru z wartości Querystring w IDDostawcy](master-detail-filtering-across-two-pages-vb/_static/image42.png)](master-detail-filtering-across-two-pages-vb/_static/image41.png)

**Rysunek 15**: Wypełnij *`supplierID`* wartość parametru `SupplierID` wartości Querystring ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image43.png))


Podczas tworzenia wiązania FormView do kontrolki ObjectDataSource w widoku Projekt, Visual Studio automatycznie utworzy FormView `ItemTemplate`, `InsertItemTemplate`, i `EditItemTemplate` za pomocą kontrolki etykiety i pole tekstowe w sieci Web dla każdego pola danych zwracane przez Kontrolki ObjectDataSource. Ponieważ chcemy wyświetlić dostawcy informacji możesz usunąć tylko `InsertItemTemplate` i `EditItemTemplate`. Następnie edytuj właściwości ItemTemplate tak, aby wyświetlał dostawcy nazwy firmy w `<h3>` elementu i adres, Miasto, kraj i numer telefonu, pod nazwą firmy. Alternatywnie, można ręcznie ustawić FormView `DataSourceID` i utworzyć `ItemTemplate` znaczników, ile My mieliśmy w "[wyświetlanie danych za pomocą kontrolki ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" samouczek.

Po te operacje edycji FormView oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample3.aspx)]

Zrzut ekranu pokazuje, rysunek 16 `ProductsForSupplierDetails.aspx` strony po dodano informacje o dostawcach szczegóły przedstawiono powyżej.


[![Lista produktów zawiera podsumowanie dotyczące dostawcy](master-detail-filtering-across-two-pages-vb/_static/image45.png)](master-detail-filtering-across-two-pages-vb/_static/image44.png)

**Rysunek 16**: Lista produktów zawiera podsumowanie dotyczące dostawcy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Stosowanie końcowe dotyka dla`ProductsForSupplierDetails.aspx`interfejsu użytkownika

Aby poprawić użytkownika środowisko dla tego raportu kilka dodatki firma powinna dokonać `ProductsForSupplierDetails.aspx` strony. Obecnie jedynym sposobem użytkownik może przejść od `ProductsForSupplierDetails.aspx` strony, powrót do listy dostawców jest kliknij przycisk Wstecz w swojej przeglądarce. Dodajmy kontrolkę Hiperlinku do `ProductsForSupplierDetails.aspx` strony z linkiem do `SupplierListMaster.aspx`, zapewniając dla użytkownika powrócić do listy wzorca w inny sposób.


[![Dodaj kontrolkę Hiperlinku, aby odzyskać użytkownika do SupplierListMaster.aspx](master-detail-filtering-across-two-pages-vb/_static/image48.png)](master-detail-filtering-across-two-pages-vb/_static/image47.png)

**Rysunek 17**: Dodawanie formantu Hiperlink, aby odzyskać użytkownika do `SupplierListMaster.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image49.png))


Jeśli użytkownik kliknie łącze Wyświetl produkty dla dostawcy, który nie ma żadnych produktów `ProductsBySupplierDataSource` ObjectDataSource w `ProductsForSupplierDetails.aspx` nie zwraca żadnych wyników. GridView powiązany z kontrolki ObjectDataSource nie są renderowane marżę skutkuje puste regionu na stronie w przeglądarce użytkownika. Komunikują się z bardziej precyzyjnie użytkownika brak produktów skojarzone z wybranego dostawcy możemy ustawić GridView `EmptyDataText` właściwości wiadomości, firma Microsoft mają być wyświetlane w przypadku wystąpienia takich sytuacji. Czy mogę ustawiono tę właściwość na "Brak produktów dostarczonych przez tego dostawcę"

Domyślnie przez wszystkich dostawców w bazie danych Northwinds Podaj co najmniej jeden produkt. Jednak na potrzeby tego samouczka I zostały ręcznie zmodyfikowane `Products` tabeli, aby dostawca Escargots Nouveaux nie jest już skojarzony z żadnym z produktów. Rysunek 18 zawiera strony szczegółów Escargots Nouveaux po dokonaniu tej zmiany.


[![Użytkownicy otrzymują, że dostawca nie udostępnia żadnych produktów](master-detail-filtering-across-two-pages-vb/_static/image51.png)](master-detail-filtering-across-two-pages-vb/_static/image50.png)

**Rysunek 18**: użytkownicy widzą komunikat, dostawca nie udostępnia żadnych produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image52.png))


## <a name="summary"></a>Podsumowanie

Gdy wzorzec/szczegół raporty można wyświetlić zarówno węzła głównego, jak i szczegóły rekordów na jednej stronie, w wielu witryn sieci Web one są rozdzielone na dwóch stronach sieci web. W tym samouczku zobaczyliśmy, jak wdrożyć raport wzorzec/szczegół przez dostawców GridView "główną" strony sieci web na liście i skojarzone produkty wymienione na stronie "szczegóły". Każdy wiersz dostawcy wzorcowej strony sieci web zawiera łącze do strony szczegółów, który jest przekazywany z wiersza `SupplierID` wartość. Takie specyficzne dla wierszy można łatwo dodawać linków przy użyciu GridView pole hiperłącza HyperLinkField.

Na stronie szczegółów pobierania tych produktów dla określonego dostawcy było wykonywane za pomocą wywołania `ProductsBLL` klasy `GetProductsBySupplierID(supplierID)` metody. *`supplierID`* Została określona wartość parametru, deklaratywne przy użyciu ciąg zapytania jako źródło parametru. Również zobaczyliśmy, jak wyświetlać szczegółowe informacje o dostawcy na stronie szczegółów przy użyciu kontrolce FormView.

Nasze [następnego samouczka](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) jest ostatnim w raportach wzorzec/szczegół. Omówimy sposób wyświetlania listy produktów w GridView, gdzie każdy wiersz zawiera wybierz przycisk. Kliknięcie przycisku Wybierz spowoduje wyświetlenie szczegółów tego produktu w kontrolce DetailsView na tej samej stronie.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Hilton Giesenow. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-filtering-with-two-dropdownlists-vb.md)
> [dalej](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)
