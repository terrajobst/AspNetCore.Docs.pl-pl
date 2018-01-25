---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: "Wzorzec/szczegół za pomocą wybieranych GridView wzorca z DetailView szczegóły (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Ten samouczek ma GridView, w której wiersze zawierają nazwę i ceny każdego produktu oraz przycisk Wybierz. Kliknięcie przycisku Wybierz dla particu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: eae9c07eff7780aab18346815ca410d687789d17
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>Wzorzec/szczegół za pomocą wybieranych GridView wzorca z DetailView szczegóły (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe) lub [pobierania plików PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> Ten samouczek ma GridView, w której wiersze zawierają nazwę i ceny każdego produktu oraz przycisk Wybierz. Kliknięcie przycisku Wybierz dla określonego produktu spowoduje jego pełne szczegóły, które mają być wyświetlane w widoku DetailsView formantu w tej samej stronie.


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczek](master-detail-filtering-across-two-pages-vb.md) widzieliśmy Tworzenie raportu wzorzec/szczegół za pomocą dwóch stron sieci web: "główny" strony sieci web, z którego możemy wyświetlonej listy dostawców; oraz stronę sieci web "szczegóły", który tych produktów dostarczonych przez wybrane na liście dostawcy. Ten format raportu dwie strony można zmniejszenia na jednej stronie. Ten samouczek ma GridView, w której wiersze zawierają nazwę i ceny każdego produktu oraz przycisk Wybierz. Kliknięcie przycisku Wybierz dla określonego produktu spowoduje jego pełne szczegóły, które mają być wyświetlane w widoku DetailsView formantu w tej samej stronie.


[![Kliknięcie przycisku Wybierz Wyświetla szczegółowe informacje o produkcie](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**Rysunek 1**: kliknięcie przycisku Wybierz Wyświetla szczegóły tego produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>Krok 1: Tworzenie wybieranych widoku GridView

Odwołania, który dwustronicowy główny/szczegółowy raport, że każdy rekord główny się hiperłącze, po kliknięciu wysyłane użytkownika do strony szczegółów przekazywanie klikniętej wiersza `SupplierID` wartość w zmiennej querystring. Takie hiperłącze został dodany do każdego wiersza w widoku GridView przy użyciu pole hiperłącza HyperLinkField. Dla jednej strony raportu głównych/szczegółów, Wkrótce skontaktujemy przycisk dla każdego widoku GridView wiersz, który, po kliknięciu przedstawia szczegóły. Kontrolki widoku siatki można skonfigurować do uwzględnienia przycisk Wybierz widoczny dla każdego wiersza, który powoduje odświeżenie strony i oznacza tego wiersza jako w widoku GridView [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

Rozpocznij od dodania formantu widoku GridView `DetailsBySelecting.aspx` strony `Filtering` folderu ustawienie jej `ID` właściwości `ProductsGrid`. Następnie dodaj nowy element ObjectDataSource o nazwie `AllProductsDataSource` który wywołuje `ProductsBLL` klasy `GetProducts()` metody.


[![Utwórz element ObjectDataSource o nazwie AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**Rysunek 2**: Tworzenie elementu ObjectDataSource o nazwie `AllProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))


[![Klasa ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**Rysunek 3**: Użyj `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))


[![Skonfiguruj ObjectDataSource można wywołać metody GetProducts()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**Rysunek 4**: Konfigurowanie ObjectDataSource do elementu Invoke `GetProducts()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))


Edytuj pola w widoku GridView, usuwając wszystkie elementy oprócz `ProductName` i `UnitPrice` BoundFields. Ponadto możesz dostosować te BoundFields zgodnie z potrzebami, takie jak formatowanie `UnitPrice` elementu BoundField jako walutę i zmianę `HeaderText` właściwości BoundFields. Te kroki można wykonać graficznie, klikając łącza Edytuj kolumny w widoku GridView tagów inteligentnych lub skonfigurować ręcznie składni deklaratywnej.


[![Usuń wszystkie oprócz ProductName i UnitPrice BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**Rysunek 5**: Usuń wszystkie elementy oprócz `ProductName` i `UnitPrice` BoundFields ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))


Końcowe kod znaczników dla widoku GridView jest:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

Następnie należy oznaczyć widoku GridView jako możliwy, która spowoduje dodanie przycisku Wybierz do każdego wiersza. W tym celu po prostu zaznacz pole wyboru Włącz zaznaczanie w tagu w widoku GridView.


[![Przyszłego zaznaczania wierszy w widoku GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**Rysunek 6**: przyszłego zaznaczania wierszy w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))


Opcje wyboru włączenie sprawdzania dodaje CommandField do `ProductsGrid` GridView z jego `ShowSelectButton` właściwość, ustaw wartość True. Powoduje to przycisk Wybierz widoczny dla każdego wiersza w widoku GridView, jak pokazano na rysunku 6. Domyślnie, wybierz przyciski są renderowane jako LinkButtons, ale można użyć przycisków lub ImageButtons za pośrednictwem CommandField `ButtonType` właściwości.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

Po kliknięciu przycisku Wybierz wierszu elementu GridView odświeżania strony ensues i w widoku GridView `SelectedRow` właściwość jest aktualizowana. Oprócz `SelectedRow` właściwość, zapewnia widoku GridView [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), i [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) właściwości. `SelectedIndex` Właściwość zwraca indeks wybranego wiersza, podczas gdy `SelectedValue` i `SelectedDataKey` właściwości zwracają wartości oparte na w widoku GridView [właściwości DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

`DataKeyNames` Właściwość jest używana do skojarzenia z jedną lub więcej wartości w każdym wierszu i jest najczęściej używany do jednoznacznego identyfikowania informacji z danych z każdego wiersza w widoku GridView atrybutu. `SelectedValue` Właściwość zwraca wartość pierwszego `DataKeyNames` pola danych dla wybranego wiersza w przypadku gdy `SelectedDataKey` właściwość zwraca wybranego wiersza `DataKey` obiekt, który zawiera wszystkie wartości dla określonego klucza pól danych tego wiersza.

`DataKeyNames` Właściwość jest automatycznie ustawiana pól danych unikatowo identyfikujący podczas można powiązać źródła danych widoku GridView, widoku DetailsView lub FormView za pomocą projektanta. Gdy ta właściwość została ustawiona dla instytucji automatycznie w poprzednim samouczki, przykłady będą działały bez `DataKeyNames` określona właściwość. Niemniej jednak w przypadku wyboru widoku GridView w tym samouczku, a także przyszłe samouczki, w których firma Microsoft będzie można badanie Wstawianie, aktualizowanie i usuwanie `DataKeyNames` właściwości musi być poprawnie ustawiony. Poświęć chwilę, aby upewnić się, że w widoku GridView `DataKeyNames` właściwość jest ustawiona na `ProductID`.

Teraz wyświetlić naszych postępu dotychczasowych za pośrednictwem przeglądarki. Należy pamiętać, że widoku GridView Wyświetla nazwę i cen dla wszystkich produktów oraz element LinkButton wybierz. Kliknięcie przycisku Wybierz powoduje odświeżenie strony. W kroku 2 Firma Microsoft będzie widoczny sposobu ustawiania odpowiedź widoku DetailsView na tym ogłaszania zwrotnego przez wyświetlanie szczegółów wybranego produktu.


[![Każdy wiersz produktu zawiera wybierz LinkButton](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**Rysunek 7**: każdy wiersz produktu zawiera element LinkButton wybierz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))


## <a name="highlighting-the-selected-row"></a>Wyróżnianie wybranego wiersza

`ProductsGrid` Ma GridView `SelectedRowStyle` właściwość, która może służyć do dyktowania stylu wizualnego dla wybranego wiersza. Prawidłowo używana, to poprawić pokazując więcej wyraźnie wiersz, który GridView obecnie jest wybierany przez użytkownika. W tym samouczku ma teraz wybranego wiersza wyróżnione żółtym tle.

Zgodnie z naszymi samouczkami, wcześniej, umożliwia Dokładamy wszelkich starań zachować estetycznych powiązane ustawienia zdefiniowane jako klas CSS. W związku z tym, Utwórz nową klasę CSS w `Styles.css` o nazwie `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

Aby zastosować tę klasę CSS do `SelectedRowStyle` właściwość *wszystkie* GridViews w naszym samouczku serii Edytuj `GridView.skin` skórki w `DataWebControls` motyw do uwzględnienia `SelectedRowStyle` ustawień, jak pokazano poniżej:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

Z tym dodatkiem wybranego wiersza w widoku GridView jest teraz wyróżnione kolorem żółtym tle.


[![Dostosowywanie wyglądu wybranego wiersza za pomocą właściwości SelectedRowStyle w widoku GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**Rysunek 8**: dostosowywanie przy użyciu wygląd zaznaczony wiersz w widoku GridView `SelectedRowStyle` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Krok 2: Wyświetlanie szczegółów produktu wybranego w widoku DetailsView

Z `ProductsGrid` ukończyć GridView, wszystkie punkty, które pozostaje jest dodanie widoku DetailsView, który wyświetla informacje dotyczące określonego produktu wybrane. Dodaj formant widoku DetailsView powyżej widoku GridView i utworzyć nowy element ObjectDataSource o nazwie `ProductDetailsDataSource`. Ponieważ chcemy tego widoku DetailsView do wyświetlenia określonej informacji na temat wybranego produktu, należy skonfigurować `ProductDetailsDataSource` do używania `ProductsBLL` klasy `GetProductByProductID(productID)` metody.


[![Wywoływanie metody GetProductByProductID(productID) klasy ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**Rysunek 9**: wywołanie `ProductsBLL` klasy `GetProductByProductID(productID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))


Ma  *`productID`*  wartość parametru uzyskane z kontrolki widoku siatki `SelectedValue` właściwości. Jak wspomniano wcześniej, w widoku GridView `SelectedValue` właściwość zwraca wartość dla wybranego wiersza klucza pierwsze dane. W związku z tym należy bezwzględnie który w widoku GridView `DataKeyNames` właściwość jest ustawiona na `ProductID`, dzięki czemu wybranego wiersza `ProductID` wartość jest zwracana przez `SelectedValue`.


[![Ustaw productID parametru do właściwości SelectedValue w widoku GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**Na rysunku nr 10**: Ustaw  *`productID`*  parametr w widoku GridView `SelectedValue` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))


Raz `productDetailsDataSource` ObjectDataSource został poprawnie skonfigurowany i powiązany z widoku DetailsView, w tym samouczku została zakończona! Po pierwsze odwiedzenia strony wiersza nie jest zaznaczone, dlatego w widoku GridView `SelectedValue` zwraca `Nothing`. Ponieważ nie ma żadnych produktów z `NULL` `ProductID` wartość, nie zwrócono żadnych rekordów przez `GetProductByProductID(productID)` metody, co oznacza, że nie są wyświetlane w widoku DetailsView (patrz rysunek 11). Po kliknięciu przycisku Wybierz wierszu elementu GridView, ensues odświeżania strony i odświeżeniu widoku DetailsView. Tym razem w widoku GridView `SelectedValue` zwraca właściwości `ProductID` wybranego wiersza `GetProductByProductID(productID)` metoda zwraca `ProductsDataTable` informacje dotyczące tego konkretnego produktu i widoku DetailsView zawiera te szczegóły (patrz rysunek 12).


[![Po pierwszym odwiedzi tylko widoku GridView jest wyświetlana](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**Rysunek 11**: po odwiedzeniu najpierw, zostanie wyświetlony tylko widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))


[![Po zaznaczeniu wiersza, wyświetlane są szczegóły tego produktu](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**Rysunek 12**: po wybraniu wiersza, wyświetlane są szczegóły tego produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))


## <a name="summary"></a>Podsumowanie

W tym i poprzedniego samouczki trzy możemy przedstawiono kilka technik do wyświetlania raportów wzorzec/szczegół. W tym samouczku będziemy zbadać za pomocą wybieralny Element GridView do przechowywania rekordów głównych i DetailsView, aby wyświetlić szczegółowe informacje dotyczące wybranego rekordu głównego na tej samej stronie. W samouczkach wcześniejszych Analizujemy sposób wyświetlania raportów głównych/szczegółów za pomocą DropDownLists i wyświetlanie rekordy główne w jedną stronę sieci web i rekordy na innym.

W tym samouczku kończy się naszego badania wzorzec/szczegół raportów. Począwszy od w następnym samouczku rozpocznie naszych badań dostosowane formatowanie z widoku GridView, widoku DetailsView i FormView. Firma Microsoft zostanie wyświetlone, jak dostosować wygląd tych kontrolek na podstawie danych powiązany z nimi, sposób podsumowywania danych w stopce w widoku GridView i jak używać szablonów w celu uzyskania lepszej kontroli nad układ.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Hilton Giesenow. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](master-detail-filtering-across-two-pages-vb.md)
