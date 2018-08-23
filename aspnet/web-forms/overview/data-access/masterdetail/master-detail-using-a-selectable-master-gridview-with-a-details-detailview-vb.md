---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: Główny/szczegóły korzystający z GridView wzorca można wybierać z DetailView szczegółów (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku zostaną GridView, w której wiersze zawierają nazwę i cenę każdego produktu oraz wybierz służącymi. Kliknięcie przycisku Wybierz dla particu...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: 0bf724741ffbc01aa7611d8b075d89827bf83777
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756598"
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>Główny/szczegóły korzystający z GridView wzorca można wybierać z DetailView szczegółów (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe) lub [Pobierz plik PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> W tym samouczku zostaną GridView, w której wiersze zawierają nazwę i cenę każdego produktu oraz wybierz służącymi. Kliknięcie przycisku Wybierz dla konkretnego produktu spowoduje, że jego szczegółowe informacje do wyświetlenia w kontrolce DetailsView na tej samej stronie.


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczka](master-detail-filtering-across-two-pages-vb.md) widzieliśmy sposób tworzenia raportu wzorzec/szczegół za pomocą dwóch stron sieci web: "główną" strony sieci web, z którego możemy wyświetlonej listy dostawców; i strony sieci web "szczegóły", którego tych produktów, dostarczone przez wybrany na liście Dostawca. Ten format raportu dwie strony może być zmniejszone do jedną stronę. W tym samouczku zostaną GridView, w której wiersze zawierają nazwę i cenę każdego produktu oraz wybierz służącymi. Kliknięcie przycisku Wybierz dla konkretnego produktu spowoduje, że jego szczegółowe informacje do wyświetlenia w kontrolce DetailsView na tej samej stronie.


[![Kliknięcie przycisku Wybierz Wyświetla szczegółowe informacje o produkcie](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**Rysunek 1**: kliknięcie przycisku Wybierz Wyświetla szczegóły produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>Krok 1: Tworzenie można wybierać widoku GridView

Odwołania, który dwustronicowy wzorzec/szczegół zgłosić, że każdy rekord główny uwzględnione hiperłącze, po kliknięciu wysyłane użytkownika do strony szczegółów, przekazując kliknięto wiersz `SupplierID` wartość w zmiennej querystring. Takie hiperłącze został dodany do każdego wiersza w widoku GridView przy użyciu pole hiperłącza HyperLinkField. Dla jednej strony raportu wzorzec/szczegół, musimy przycisku dla każdego widoku GridView wiersz, po kliknięciu pokazuje szczegóły. W kontrolce GridView można skonfigurować, aby uwzględnić przycisku Wybierz dla każdego wiersza, który powoduje odświeżenie strony i oznacza tego wiersza w widoku GridView [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

Rozpocznij od dodania kontrolki widoku siatki do `DetailsBySelecting.aspx` strony w `Filtering` folderu, ustawianie jego `ID` właściwość `ProductsGrid`. Następnie dodaj nowe kontrolki ObjectDataSource, o nazwie `AllProductsDataSource` wywołującej `ProductsBLL` klasy `GetProducts()` metody.


[![Tworzenie kontrolki ObjectDataSource o nazwie AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**Rysunek 2**: tworzenie kontrolki ObjectDataSource o nazwie `AllProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))


[![Korzystanie z klasy ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**Rysunek 3**: Użyj `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))


[![Konfigurowanie kontrolki ObjectDataSource można wywołać metody GetProducts()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**Rysunek 4**: Konfigurowanie kontrolki ObjectDataSource do Invoke `GetProducts()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))


Upravit pole prvku GridView usuwanie wszystkie elementy oprócz `ProductName` i `UnitPrice` BoundFields. Ponadto możesz dostosować te BoundFields zgodnie z potrzebami, takie jak formatowanie `UnitPrice` elementu BoundField jako walutę i zmieniając `HeaderText` właściwości BoundFields. Te kroki można osiągnąć graficznie, klikając łącze Edytowanie kolumn z tagu inteligentnego GridView lub skonfigurować ręcznie składni deklaratywnej.


[![Usuń wszystkie oprócz ProductName i UnitPrice BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**Rysunek 5**: Usuń wszystkie elementy oprócz `ProductName` i `UnitPrice` BoundFields ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))


Jest ostateczny znaczniki dla widoku GridView:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

Następnie należy oznaczyć widoku GridView jako możliwy, która doda wybierz służącymi do każdego wiersza. W tym celu po prostu zaznacz pole wyboru Włącz zaznaczanie w tagu inteligentnego GridView.


[![Przyszłego zaznaczania wierszy w widoku GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**Rysunek 6**: przyszłego zaznaczania wierszy w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))


Opcje umożliwiające wybór sprawdzania dodaje CommandField do `ProductsGrid` GridView z jego `ShowSelectButton` właściwość ustawioną na wartość True. Skutkuje to przycisku Wybierz dla każdego wiersza w widoku GridView, tak jak pokazano na rysunku 6. Domyślnie, wybierz przyciski są renderowane jako LinkButtons, ale można użyć przycisków lub ImageButtons zamiast za pośrednictwem CommandField `ButtonType` właściwości.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

Po kliknięciu przycisku Wybierz wierszu elementu GridView ensues odświeżenie strony i GridView `SelectedRow` zaktualizować właściwości. Oprócz `SelectedRow` , widoku GridView dostarcza [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), i [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) właściwości. `SelectedIndex` Właściwość zwraca indeks wybranego wiersza, natomiast `SelectedValue` i `SelectedDataKey` właściwości zwracają wartości na podstawie GridView [właściwości DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

`DataKeyNames` Właściwość jest używana do skojarzenia z jedną lub więcej wartości, przy czym każdy wiersz i jest najczęściej używany do atrybutu jednoznacznie identyfikujące informacji z danych z każdego wiersza w widoku GridView. `SelectedValue` Właściwość zwraca wartość pierwszego `DataKeyNames` pole danych dla wybranego wiersza w przypadku gdy `SelectedDataKey` właściwości zwracają zaznaczonym wierszu `DataKey` obiekt, który zawiera wszystkie wartości dla określonego klucza pól danych Ten wiersz.

`DataKeyNames` Zostaje automatycznie ustalona pola danych unikatowo identyfikujący wiążąc źródła danych do kontrolki GridView, DetailsView lub FormView za pomocą projektanta. Podczas tej właściwości ustawiono dla nas automatycznie w poprzednich samouczkach, przykładach będą działały bez `DataKeyNames` określona właściwość. Jednak w przypadku wyboru GridView w ramach tego samouczka, a także dla przyszłych samouczków, w których firma Microsoft będzie mieć badanie Wstawianie, aktualizowanie i usuwanie `DataKeyNames` właściwość musi być poprawnie ustawiony. Poświęć chwilę, aby upewnić się, że w widoku GridView `DataKeyNames` właściwość jest ustawiona na `ProductID`.

Teraz wyświetlić postępach dotychczasowych za pośrednictwem przeglądarki. Należy pamiętać, że widoku GridView Wyświetla nazwy i ceny dla wszystkich produktów oraz element LinkButton wybierz. Kliknięcie przycisku Wybierz powoduje odświeżenie strony. W kroku 2 zobaczymy jak odpowiada DetailsView, aby to odświeżenie strony, wyświetlając szczegółowe informacje dotyczące wybranego produktu.


[![Każdy wiersz produktu zawiera wybierz element LinkButton](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**Rysunek 7**: każdy wiersz produktu zawiera element LinkButton wybierz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))


## <a name="highlighting-the-selected-row"></a>Wyróżnianie wybranego wiersza

`ProductsGrid` Ma GridView `SelectedRowStyle` właściwość, która może służyć do dyktowania stylu wizualnego dla wybranego wiersza. Użyto prawidłowo, może to poprawić środowisko użytkownika przez inne wyraźnie materiały, w którym wiersz GridView jest aktualnie wybrany. W tym samouczku Przyjrzyjmy się zaznaczony wiersz wyróżniony żółtym tłem.

Podobnie jak w przypadku samouczków wcześniej Przyjrzyjmy wszelkich starań, aby zachować ustawienia estetycznych powiązane zdefiniowany jako klas CSS. W związku z tym, Utwórz nową klasę CSS w `Styles.css` o nazwie `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

Do zastosowania tej klasy CSS, aby `SelectedRowStyle` właściwość *wszystkich* GridViews w naszej serii samouczków, Edytuj `GridView.skin` skórki w `DataWebControls` motywu, aby uwzględnić `SelectedRowStyle` ustawień, jak pokazano poniżej:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

Dodając ten wybranego wiersza w widoku GridView zostanie wyróżniony kolorem żółtym tłem.


[![Dostosowywanie wyglądu wybranego wiersza przy użyciu właściwości SelectedRowStyle GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**Rysunek 8**: Dostosowywanie za pomocą wygląd zaznaczony wiersz w widoku GridView `SelectedRowStyle` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Krok 2: Wyświetlanie szczegółów wybranego produktu w DetailsView

Za pomocą `ProductsGrid` GridView zakończeniu wszystko, co pozostanie, jest dodanie DetailsView, który wyświetla informacje dotyczące konkretnego produktu wybrane. Dodawanie kontrolki widoku szczegółów powyżej widoku GridView i utworzyć nowe kontrolki ObjectDataSource, o nazwie `ProductDetailsDataSource`. Ponieważ chcemy, aby ten DetailsView, aby wyświetlić określoną informacje dotyczące wybranego produktu, należy skonfigurować `ProductDetailsDataSource` używać `ProductsBLL` klasy `GetProductByProductID(productID)` metody.


[![Wywoływanie metody GetProductByProductID(productID) klasy ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**Rysunek 9**: wywoływanie `ProductsBLL` klasy `GetProductByProductID(productID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))


Masz *`productID`* wartość parametru uzyskane z kontrolki GridView `SelectedValue` właściwości. Jak wspomniano wcześniej, GridView `SelectedValue` właściwość zwraca pierwsze dane klucza dla wybranego wiersza. W związku z tym, należy bezwzględnie, GridView `DataKeyNames` właściwość jest ustawiona na `ProductID`, dzięki czemu wybranego wiersza `ProductID` wartość jest zwracana przez `SelectedValue`.


[![Wartość elementu productID parametru właściwości SelectedValue GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**Na rysunku nr 10**: Ustaw *`productID`* parametr GridView `SelectedValue` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))


Raz `productDetailsDataSource` ObjectDataSource został poprawnie skonfigurowany i powiązane z DetailsView, w tym samouczku została zakończona! Po pierwsze odwiedzenia strony żaden wiersz nie jest zaznaczone, więc GridView `SelectedValue` właściwość zwraca `Nothing`. Ponieważ brak produktów z `NULL` `ProductID` wartości, żadne rekordy nie są zwracane przez `GetProductByProductID(productID)` metody, co oznacza, że nie jest wyświetlana DetailsView (zobacz rysunek 11). Po kliknięciu przycisku Wybierz wierszu elementu GridView ensues odświeżenie strony i DetailsView są odświeżane. Tym razem GridView `SelectedValue` właściwość zwraca `ProductID` wybranego wiersza `GetProductByProductID(productID)` metoda zwraca `ProductsDataTable` z informacjami dotyczącymi tego konkretnego produktu i DetailsView przedstawia te informacje (zobacz rysunek 12).


[![Gdy zostanie wyświetlona pierwszy odwiedzony tylko GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**Rysunek 11**: po raz pierwszy odwiedzony, jest wyświetlany tylko kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))


[![Po wybraniu wiersz, są wyświetlane szczegóły produktu](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**Rysunek 12**: po wybraniu wiersz, są wyświetlane szczegóły produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))


## <a name="summary"></a>Podsumowanie

W tym i w poprzednich samouczkach trzy widzieliśmy kilka technik do wyświetlania raportów wzorzec/szczegół. W tym samouczku zbadaliśmy przy użyciu GridView możliwy do przechowywania rekordów głównych i DetailsView, aby wyświetlić szczegółowe informacje dotyczące wybranego rekordu głównego na tej samej stronie. W samouczkach wcześniej zobaczyliśmy, jak wyświetlać raporty wzorzec/szczegół za pomocą kontrolek DROPDOWNLIST i wyświetlania rekordów głównych w jedną stronę sieci web i rekordy na innym.

W tym samouczku kończy się nasze badania wzorzec/szczegół raportów. Począwszy od następnego samouczka rozpocznie się naszych badań niestandardowe formatowanie przy użyciu GridView DetailsView i FormView. Zobaczymy, jak dostosować wygląd tych formantów na podstawie danych powiązany z nimi, sposób podsumowywania danych w stopce kontrolki GridView i jak za pomocą szablonów w celu uzyskania lepszej kontroli nad układu.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Hilton Giesenow. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-filtering-across-two-pages-vb.md)
