---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Wzorzec/szczegół filtrowanie z DropDownList (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku przedstawiono sposób wyświetlania raportów wzorzec/szczegół w jednej strony sieci web przy użyciu DropDownLists do wyświetlenia "master" rekordów i DataList displ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: c84902ccf028c976246380abfaebb6a76c573603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Wzorzec/szczegół filtrowanie z DropDownList (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) lub [pobierania plików PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> W tym samouczku przedstawiono sposób wyświetlania raportów wzorzec/szczegół w jednej strony sieci web przy użyciu DropDownLists do wyświetlania rekordów "główny" i DataList do wyświetlenia "szczegóły".


## <a name="introduction"></a>Wprowadzenie

Raport wzorzec/szczegół najpierw utworzyć przy użyciu widoku GridView w wcześniej [wzorzec/szczegół filtrowania z DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) samouczek, rozpoczyna się poprzez wyświetlenie niektórych zestawu rekordów "master". Użytkownik może następnie przejść do jednego z głównym rekordów ten sposób wyświetlania tego rekordu głównego "szczegółowych informacji." Wzorzec/szczegół raporty są idealnym wyborem do wizualizacji relacji jeden do wielu, jak i do wyświetlania szczegółowych informacji z szczególnie tabel "szerokości" (te, które zawiera wiele kolumn). Firma Microsoft zostały przedstawione sposobu wdrażania raportów wzorzec/szczegół za pomocą kontrolki GridView i widoku DetailsView w poprzednim samouczki. W tym samouczku i dwie firma Microsoft będzie reexamine tych pojęć, ale fokus przy użyciu DataList i elementu powtarzanego zamiast kontroluje ją.

W ramach tego samouczka przyjrzymy przy użyciu DropDownList zawiera rekordy "master", wyświetlane w DataList rekordami "szczegóły".

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Krok 1: Dodawanie stron sieci Web z samouczka wzorzec/szczegół

Przed Rozpoczniemy ten samouczek umożliwia najpierw Poświęć chwilę, aby dodać folder i stron ASP.NET, które będą potrzebne dla tego samouczka i dwie dotyczących raportów wzorzec/szczegół za pomocą formantów DataList i elementu powtarzanego. Rozpocznij od utworzenia nowego folderu do projektu o nazwie `DataListRepeaterFiltering`. Następnie dodaj następujące pięć stron ASP.NET w tym folderze każdego z nich jest skonfigurowany do używania strony wzorcowej `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Utwórz DataListRepeaterFiltering Folder i dodać strony samouczek platformy ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Rysunek 1**: tworzenie `DataListRepeaterFiltering` folderu i dodać strony samouczek platformy ASP.NET


Następnie otwórz folder `Default.aspx` strony i przeciągnij `SectionLevelTutorialListing.ascx` kontrolki użytkownika z `UserControls` folderu na powierzchnię projektu. Ten formant użytkownika, który utworzono w [stron wzorcowych i nawigacji w witrynie](../introduction/master-pages-and-site-navigation-cs.md) samouczek, wylicza mapy witryny i wyświetla samouczków, z sekcji bieżącej listy punktowanej.


[![Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` formantu użytkownika `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


W celu wyświetlania listy punktowanej samouczki wzorzec/szczegół, firma Microsoft będzie tworzony, należy dodać je do mapy witryny. Otwórz `Web.sitemap` i Dodaj następujący kod po znaczników węzeł mapy witryny "Wyświetlanie danych z DataList i powtarzanego":

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![Zaktualizuj mapy witryny w celu uwzględnienia nowych stron ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Rysunek 3**: zaktualizuj mapy witryny w celu uwzględnienia nowych stron ASP.NET


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Krok 2: Wyświetlanie kategorii w DropDownList

Nasze raportu wzorzec/szczegół spowoduje wyświetlenie listy kategorii w DropDownList z produktami elementu wybranej listy wyświetlane dodatkowe w dół strony w DataList. Pierwszym zadaniem przed nami, jest następnie kategorie wyświetlane w DropDownList. Uruchamianie przez otwarcie `FilterByDropDownList.aspx` strony `DataListRepeaterFiltering` folder i przeciągnij DropDownList z przybornika do strony projektanta. Następnie należy ustawić DropDownList `ID` właściwości `Categories`. Kliknij łącze Wybierz źródło danych z tagów inteligentnych DropDownList i Utwórz nowy element ObjectDataSource o nazwie `CategoriesDataSource`.


[![Dodaj nowy element ObjectDataSource o nazwie CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Rysunek 4**: Dodaj nowy element ObjectDataSource o nazwie `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


Skonfiguruj nowy element ObjectDataSource tak, aby go wywołuje `CategoriesBLL` klasy `GetCategories()` metody. Po skonfigurowaniu ObjectDataSource nadal trzeba określić, jakie pola źródła danych mają być wyświetlane w DropDownList i jedną powinna być skojarzona jako wartość dla każdego elementu listy. Ma `CategoryName` pole jako ekran i `CategoryID` jako wartość dla każdego elementu listy.


[![Ma pole CategoryName i użyj CategoryID wyświetlana lista DropDownList jako wartość](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Rysunek 5**: wyświetlone DropDownList `CategoryName` pola i użyj `CategoryID` jako wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


W tym momencie mamy DropDownList formant, który jest wypełniana rekordy z `Categories` tabeli (wszystkie dokonane w ciągu około sześciu sekund). Rysunek 6 przedstawia naszych postępu dotychczasowych widzianego za pośrednictwem przeglądarki.


[![Rozwijana lista bieżącej kategorii](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Rysunek 6**: listy rozwijane A bieżącej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Krok 2: Dodawanie DataList produktów

Ostatnim krokiem w naszej raportu wzorzec/szczegół jest aby wyświetlić listę produktów skojarzonych z wybranej kategorii. W tym celu Dodaj DataList do strony i Utwórz nowy element ObjectDataSource o nazwie `ProductsByCategoryDataSource`. Ma `ProductsByCategoryDataSource` kontroli pobrać dane z `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` metody. Ponieważ ten raport wzorzec/szczegół jest tylko do odczytu, wybierz opcję (Brak) na kartach INSERT, UPDATE i DELETE.


[![Wybierz metodę GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Rysunek 7**: Wybierz `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


Po kliknięciu przycisku Dalej, Kreator ObjectDataSource żąda nam źródła wartość `GetProductsByCategoryID(categoryID)` metody *`categoryID`* parametru. Aby użyć wartości wybranych `categories` elementu DropDownList ustawiono parametr źródła kontroli i ControlID do `Categories`.


[![Ustaw categoryID parametru wartość DropDownList kategorii](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Rysunek 8**: Ustaw *`categoryID`* parametru z wartością `Categories` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


Po zakończeniu pracy Kreatora konfigurowania źródła danych programu Visual Studio automatycznie generuje `ItemTemplate` dla DataList, który wyświetla nazwę i wartość każdego pola danych. Załóżmy zwiększenia DataList, aby zamiast tego użyć `ItemTemplate` wyświetlający tylko nazwę produktu, kategoria, dostawca, ilość na jednostkę oraz cen wraz z `SeparatorTemplate` który injects `<hr>` element między każdym z elementów. Będę używać `ItemTemplate` z przykładem w [wyświetlanie danych za pomocą DataList i kontrolki elementu powtarzanego](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) samouczek, ale działanie może używać dowolnego oznaczenia szablonu możesz znaleźć najbardziej atrakcyjność.

Po wprowadzeniu tych zmian, listy DataList i jego ObjectDataSource znaczników powinien wyglądać podobnie do następującego:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Poświęć chwilę, aby zapoznaj się z naszym postęp w przeglądarce. Podczas odwiedzania najpierw strony, tych produktów należących do wybranej kategorii (napoje) są wyświetlane (jak pokazano na rysunku nr 9), ale zmiana DropDownList nie powoduje aktualizacji danych. Jest to spowodowane odświeżania strony musi nastąpić DataList do aktualizacji. Firma Microsoft może być w tym celu ustaw DropDownList `AutoPostBack` właściwości `true` lub dodać kontrolkę przycisku sieci Web ze stroną. W tym samouczku I została wybrana opcja można ustawić DropDownList `AutoPostBack` właściwości `true`.

Rysunki 9 i 10 ilustrują raportu wzorzec/szczegół w akcji.


[![Podczas odwiedzania najpierw strony, są wyświetlane produkty napoju](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Rysunek 9**: podczas odwiedzania najpierw strony, są wyświetlane produkty napoju ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![Automatyczne zaznaczanie nowego produktu (produktu) powoduje odświeżenie strony, aktualizowania elementu DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Na rysunku nr 10**: zaznaczenie nowego produktu (produktu) automatycznie powoduje odświeżenie strony, aktualizowania elementu DataList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Dodawanie elementu listy "--Wybierz kategorię--"

Podczas odwiedzania najpierw `FilterByDropDownList.aspx` strony kategorie lista DropDownList na pierwszy element listy (napoje) jest domyślnie zaznaczona, przedstawiający produktów napoju w elementu DataList. W *wzorzec/szczegół filtrowania z DropDownList* samouczek dodaliśmy użyć opcji "--Wybierz kategorię--" Aby DropDownList, został domyślnie zaznaczone, a następnie wyświetlane po wybraniu *wszystkie* z produkty w bazie danych. Takie podejście była zarządzane podczas wyświetlania produktów w widoku GridView, jak każdy wiersz produktu trwało się niewielkie nieruchomości ekranu. Z DataList jednak informacji każdego produktu zużywa dużo większą fragmentu ekranu. Nadal umożliwia dodać opcji "--Wybierz kategorię--" jest domyślnie zaznaczone, a zamiast je wszystkie produkty po wybraniu Skonfigurujmy go tak, aby pokazywał produktów.

Aby dodać nowy element listy do DropDownList, przejdź do okna właściwości, a następnie kliknij wielokropek w `Items` właściwości. Dodaj nowy element listy z `Text` "--Wybierz kategorię--" i `Value` `0`.


![Dodaj](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Rysunek 11**: Dodawanie elementu listy "--Wybierz kategorię--"


Alternatywnie można dodać elementu listy, dodając następujący kod do DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

Ponadto należy ustawić kontrolę lista DropDownList `AppendDataBoundItems` do `true` ponieważ jeśli jest ustawiona na `false` (domyślnie), gdy kategorie są powiązane z DropDownList z ObjectDataSource będzie zastępują żadnej listy dodane ręcznie elementy.


![Ustaw wartość True dla właściwości AppendDataBoundItems](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Rysunek 12**: Ustaw `AppendDataBoundItems` właściwości na wartość True


Powód Wybraliśmy wartości `0` listy "--Wybierz kategorię--" elementu jest, ponieważ nie ma żadnych kategorii w systemie o wartości `0`, dlatego zostaną zwrócone żadne rekordy produktu po wybraniu elementu listy "--Wybierz kategorię--". Można to potwierdzić, Poświęć chwilę, aby odwiedzić stronę za pośrednictwem przeglądarki. Jak rysunku 13 przedstawiono, przy pierwszym Przeglądanie strony elementu listy "--Wybierz kategorię--" jest zaznaczone i produkty nie są wyświetlane.


[![Gdy](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Rysunek 13**: po wybraniu elementu "--Wybierz kategorię--" listy produktów nie są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


Jeśli zamiast pojawiłyby *wszystkie* produktów po wybraniu opcji "--Wybierz kategorię —" Użyj wartości elementu `-1` zamiast tego. Astute czytnik będzie przypominać tego Wstecz w *wzorzec/szczegół filtrowania z DropDownList* samouczek Zaktualizowaliśmy `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` metody, aby Jeśli *`categoryID`* wartość `-1` przekazano produktu wszystkie rekordy zostały zwrócone.

## <a name="summary"></a>Podsumowanie

Podczas wyświetlania danych dotyczących hierarchicznie, pomaga często zawierają dane przy użyciu wzorzec/szczegół raportów, z których użytkownik może uruchomić perusing danych od góry hierarchii i przejść do szczegółów. W tym samouczku będziemy zbadane, tworzenie prostego główny/szczegółowy raport pokazujący wybranej kategorii produktów. Zostało to zrobić za pomocą DropDownList listy kategorii i DataList produktów należących do wybranej kategorii.

W następnym samouczku przyjrzymy oddzielanie rekordów wzorzec i szczegóły między dwoma stronami. Na pierwszej stronie listę rekordów "główny" będą wyświetlane, Link, aby wyświetlić szczegóły. Kliknięcie łącza zostanie whisk użytkownika do drugiej strony, które będą wyświetlane szczegóły dla wybranego rekordu głównego.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Randy Schmidt. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](master-detail-filtering-acess-two-pages-datalist-cs.md)
