---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
title: Wzorzec/szczegół filtrowanie z dwóch DropDownLists (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku rozszerza relacji wzorzec/szczegół, aby dodać warstwę trzeci, za pomocą dwóch formantów DropDownList, aby wybrać żądaną recor nadrzędne i ponadnadrzędny...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11ae4f64-01ba-4823-95f4-a2fe1f84f7d7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
msc.type: authoredcontent
ms.openlocfilehash: ee0232cf8f7c0533703a51a4629522fd887f216f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-filtering-with-two-dropdownlists-vb"></a>Wzorzec/szczegół filtrowanie z dwóch DropDownLists (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_8_VB.exe) lub [pobierania plików PDF](master-detail-filtering-with-two-dropdownlists-vb/_static/datatutorial08vb1.pdf)

> W tym samouczku rozszerza relacji wzorzec/szczegół, aby dodać warstwę trzeci, za pomocą dwóch formantów DropDownList, aby wybrać odpowiednie rekordy nadrzędne i ponadnadrzędny.


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczek](master-detail-filtering-with-a-dropdownlist-vb.md) możemy zbadać sposób wyświetlania raportu proste głównych/szczegółów za pomocą pojedynczego DropDownList wypełniane przy użyciu kategorie i GridView, przedstawiający tych produktów, które należą do wybranej kategorii. Ten wzorzec raportu dobrze działa w przypadku wyświetlania rekordów relacji jeden do wielu, które można z łatwością rozszerzyć działać w przypadku scenariuszy obejmujących wiele relacji jeden do wielu. Na przykład system kolejność wprowadzania musi tabel, które odpowiadają klientami, zamówienia i kolejność pozycji. Dany klient może mieć wielu zamówień z każdej kolejności składające się z wielu elementów. Takie dane widoczne dla użytkowników z dwóch DropDownLists i Element GridView. Pierwszy DropDownList byłyby elementu dla każdego klienta w bazie danych, drugi osoby zawartości jest zamówień dla wybranego odbiorcy. Element GridView listę pozycji z wybranego zamówienia.

Chociaż baza danych Northwind zawiera informacje canonical szczegóły klienta/zamówienia w jego `Customers`, `Orders`, i `Order Details` tabele, te tabele nie są przechwytywane w naszej architektury. Niemniej jednak firma Microsoft może nadal ilustrują przy użyciu dwóch DropDownLists zależnych. Pierwszy DropDownList spowoduje wyświetlenie listy Kategorie, a drugi produktów należących do wybranej kategorii. Element DetailsView zostanie wyświetlona lista szczegółów wybranego produktu.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Krok 1: Tworzenie i wypełnianie DropDownList kategorii

Naszym celem pierwszego jest dodać DropDownList, który zawiera listę kategorii. Te kroki zostały zbadane szczegółowo w poprzednim samouczek, ale są podsumowywane w tym miejscu, aby informacje były kompletne.

Otwórz `MasterDetailsDetails.aspx` strony `Filtering` folder, na stronie, Dodaj DropDownList ustawiony jego `ID` właściwości `Categories`, a następnie kliknij łącze Konfiguruj źródło danych w jego tagów inteligentnych. W Kreatorze konfiguracji źródła danych wybierz można dodać nowego źródła danych.


[![Dodaj nowe źródło danych dla DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image1.png)

**Rysunek 1**: Dodaj nowe źródło danych dla DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image3.png))


Oczywiście nowego źródła danych należy ObjectDataSource. Nazwa tego nowego elementu ObjectDataSource `CategoriesDataSource` i wywołać `CategoriesBLL` obiektu `GetCategories()` metody.


[![Wybierz klasy CategoriesBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image4.png)

**Rysunek 2**: wybierz opcję do użycia `CategoriesBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image6.png))


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetCategories()](master-detail-filtering-with-two-dropdownlists-vb/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image7.png)

**Rysunek 3**: Konfigurowanie ObjectDataSource użyć `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image9.png))


Po skonfigurowaniu ObjectDataSource nadal trzeba określić pole źródła danych, które mają być wyświetlane w `Categories` DropDownList i które z nich powinna być skonfigurowana jako wartość dla elementu listy. Ustaw `CategoryName` pole jako ekran i `CategoryID` jako wartość dla każdego elementu listy.


[![Ma pole CategoryName i użyj CategoryID wyświetlana lista DropDownList jako wartość](master-detail-filtering-with-two-dropdownlists-vb/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image10.png)

**Rysunek 4**: wyświetlone DropDownList `CategoryName` pola i użyj `CategoryID` jako wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image12.png))


W tym momencie mamy formantu DropDownList (`Categories`) który został wypełniony rekordy z `Categories` tabeli. Gdy użytkownik wybierze nową kategorię z DropDownList chcemy odświeżania strony wystąpić, aby można było odświeżyć produktu DropDownList, który zamierzamy utworzyć w kroku 2. W związku z tym, zaznacz opcję Włącz AutoPostBack z `categories` lista DropDownList na tagów inteligentnych.


[![Włącz AutoPostBack dla DropDownList kategorii](master-detail-filtering-with-two-dropdownlists-vb/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image13.png)

**Rysunek 5**: Włącz AutoPostBack dla `Categories` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Krok 2: Wyświetlanie wybranej kategorii produktów w drugim DropDownList

Z `Categories` DropDownList została zakończona, naszych następnym krokiem jest do wyświetlenia DropDownList produktów należących do wybranej kategorii. W tym celu na stronie o nazwie Dodaj inny DropDownList `ProductsByCategory`. Jak `Categories` DropDownList, Utwórz nowy element ObjectDataSource dla `ProductsByCategory` DropDownList o nazwie `ProductsByCategoryDataSource`.


[![Dodaj nowe źródło danych dla ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image16.png)

**Rysunek 6**: Dodaj nowe źródło danych dla `ProductsByCategory` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image18.png))


[![Utwórz nowy element ObjectDataSource o nazwie ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-vb/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image19.png)

**Rysunek 7**: Utwórz nowy składnik o nazwie ObjectDataSource `ProductsByCategoryDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image21.png))


Ponieważ `ProductsByCategory` potrzeb DropDownList do wyświetlenia tylko te produkty należące do wybranej kategorii ma ObjectDataSource wywołania `GetProductsByCategoryID(categoryID)` metody `ProductsBLL` obiektu.


[![Wybierz klasy ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image22.png)

**Rysunek 8**: wybierz opcję do użycia `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image24.png))


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetProductsByCategoryID(categoryID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image25.png)

**Rysunek 9**: Konfigurowanie ObjectDataSource użyć `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image27.png))


W ostatnim kroku kreatora należy określić wartość *`categoryID`* parametru. Ten parametr należy przypisać wybrany element z `Categories` DropDownList.


[![Ściąganie categoryID wartość parametru z DropDownList kategorii](master-detail-filtering-with-two-dropdownlists-vb/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image28.png)

**Na rysunku nr 10**: ściągnięcia *`categoryID`* wartości parametru z `Categories` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image30.png))


Z elementu ObjectDataSource skonfigurowane wszystkie te pozostaje jest do określenia, jakie pola źródła danych służą do wyświetlania i wartość DropDownList elementów. Wyświetl `ProductName` pól i użyj `ProductID` pole jako wartość.


[![Określ elementy ListItems DropDownList tekstu i wartość właściwości pola źródła danych](master-detail-filtering-with-two-dropdownlists-vb/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image31.png)

**Rysunek 11**: Określ użyć pola źródła danych dla DropDownList `ListItem` s " `Text` i `Value` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image33.png))


Z elementu ObjectDataSource i `ProductsByCategory` DropDownList skonfigurowane strony zostaną wyświetlone dwa DropDownLists: pierwszy będzie zawierała listę wszystkich kategorii, podczas gdy druga wyświetla tych produktów należących do wybranej kategorii. Gdy użytkownik wybierze nową kategorię z pierwszego DropDownList, nastąpi odświeżenie strony i być odbitych druga lista DropDownList, przedstawiający tych produktów, które należą do nowo wybranej kategorii. Rysunki 12 i 13 Pokaż `MasterDetailsDetails.aspx` w akcji podczas wyświetlania za pośrednictwem przeglądarki.


[![Podczas odwiedzania najpierw strony, wybrano należące do tej kategorii](master-detail-filtering-with-two-dropdownlists-vb/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image34.png)

**Rysunek 12**: podczas odwiedzania najpierw strony, wybrano należące do tej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image36.png))


[![Wybieranie różnych kategorii wyświetla nowej kategorii produktów](master-detail-filtering-with-two-dropdownlists-vb/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image37.png)

**Rysunek 13**: wybór różnych ekranów kategorii produktów nową kategorię ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image39.png))


Obecnie `productsByCategory` jest lista DropDownList po zmianie *nie* powoduje odświeżenie strony. Jednak należy odświeżania strony wystąpić po dodamy DetailsView, aby wyświetlić szczegóły wybranego produktu (krok 3). W związku z tym, zaznacz pole wyboru Włącz AutoPostBack z `productsByCategory` lista DropDownList na tagów inteligentnych.


[![Włącz funkcję AutoPostBack dla productsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image40.png)

**Rysunek 14**: Włączanie funkcji AutoPostBack dla `productsByCategory` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Krok 3: Element DetailsView przy użyciu Aby wyświetlić szczegóły wybranego produktu

Ostatnim krokiem jest można wyświetlić szczegółów wybranego produktu w widoku DetailsView. Aby to osiągnąć, należy dodać element DetailsView do strony, ustaw jej `ID` właściwości `ProductDetails`i Utwórz nowy element ObjectDataSource dla niego. Skonfiguruj ten element ObjectDataSource jego dane z `ProductsBLL` klasy `GetProductByProductID(productID)` metodę przy użyciu wybranej wartości `ProductsByCategory` DropDownList dla wartości *`productID`* parametru.


[![Wybierz klasy ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image43.png)

**Rysunek 15**: wybierz opcję do użycia `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image45.png))


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetProductByProductID(productID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image46.png)

**Rysunek 16**: Konfigurowanie ObjectDataSource użyć `GetProductByProductID(productID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image48.png))


[![Ściąganie danych z ProductsByCategory DropDownList productID wartość parametru](master-detail-filtering-with-two-dropdownlists-vb/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image49.png)

**Rysunek 17**: ściągnięcia *`productID`* wartości parametru z `ProductsByCategory` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image51.png))


Istnieje możliwość wyświetlenia dostępnych polach w `ProductDetails` widoku DetailsView. I została wybrana opcja usunąć `ProductID`, `SupplierID`, i `CategoryID` pól i kolejności i sformatowany pozostałe pola. Ponadto wyczyścić limit DetailsView `Height` i `Width` właściwości, dzięki czemu do rozszerzenia szerokość niezbędne do wyświetlania najważniejsze dane zamiast go ograniczone do określonego rozmiaru widoku DetailsView. Pełny kod znaczników pojawia się poniżej:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample1.aspx)]

Poświęć chwilę, aby wypróbować `MasterDetailsDetails.aspx` strony w przeglądarce. Na pierwszy rzut oka może występować, że wszystko działa zgodnie z potrzebami, ale występuje niewielkie problem. Po wybraniu nowej kategorii `ProductsByCategory` DropDownList została zaktualizowana do tych produktów dla wybranej kategorii, ale `ProductDetails` widoku DetailsView nadal Pokaż poprzednie informacji o produkcie. Widoku DetailsView jest aktualizowany w przypadku wybrania innego produktu dla wybranej kategorii. Ponadto w przypadku testowania wystarczająco dokładnie, będzie stwierdzisz, że jeśli wybierzesz stale nowych kategorii (np. wybranie napoje z `Categories` DropDownList, a następnie przyprawy, następnie Confections) co inne wybór kategorii spowoduje, że `ProductDetails`Widoku DetailsView jest odświeżenie.

Aby ułatwić skonkretyzować ten problem, Oto przykład określone. Po pierwsze odwiedź stronę należące do tej kategorii jest zaznaczone i pokrewnych produktów są ładowane w `ProductsByCategory` DropDownList. Chai jest wybrany produktu i jego szczegóły są wyświetlane w `ProductDetails` widoku DetailsView, jak pokazano na rysunku 18.


[![Szczegółowe informacje o produkcie wybrane są wyświetlane w widoku DetailsView](master-detail-filtering-with-two-dropdownlists-vb/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image52.png)

**Rysunek 18**: wybrane produktu szczegóły są wyświetlane w widoku DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image54.png))


Jeśli zmienisz zaznaczenie kategorii z napoje przyprawy, występuje odświeżania strony i `ProductsByCategory` DropDownList jest odpowiednio aktualizowany, ale nadal wyświetla szczegóły dla Chai w widoku DetailsView.


[![Poprzednio wybrane produktu szczegóły są nadal wyświetlane](master-detail-filtering-with-two-dropdownlists-vb/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image55.png)

**Rysunek 19**: szczegóły wcześniej wybrane produktu są nadal wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image57.png))


Pobrania nowego produktu z listy odświeża widoku DetailsView, zgodnie z oczekiwaniami. W przypadku wybrania nowej kategorii po zmianie produktu widoku DetailsView ponownie nie odświeżania. Jednak jeśli zamiast nowego produktu wybrano nową kategorię, czy odświeżyć widoku DetailsView. Co w świecie dzieje się w tym miejscu?

Problem jest czasu w cyklu życia strony. Zawsze, gdy strona jest zażądał obejmującego kilka kroków, jako jego renderowania. W jednym z tych kroków ObjectDataSource steruje Sprawdź, czy ich `SelectParameters` wartości zostały zmienione. Jeśli tak, kontrolka sieci Web dane powiązane z ObjectDataSource wie, należy odświeżyć jego wyświetlania. Na przykład, gdy wybrano nową kategorię, `ProductsByCategoryDataSource` ObjectDataSource wykryje, że zmieniono jego wartości parametrów i `ProductsByCategory` DropDownList rebinds, uzyskiwanie produktów dla wybranej kategorii.

Problem, który pojawia się w tej sytuacji jest, że dochodzi do punktu w cyklu życia strony, która ObjectDataSources Sprawdź, czy parametry zmienione *przed* ponownego skojarzone dane zawarte w formantach sieci Web. W związku z tym wybierając nową kategorię `ProductsByCategoryDataSource` ObjectDataSource wykryje zmianę w wartość jego parametru. Element ObjectDataSource, używany przez `ProductDetails` widoku DetailsView, jednak nie należy pamiętać, wszelkie zmiany ponieważ `ProductsByCategory` DropDownList jeszcze nie być odbitych. Nowsze w cyklu życia `ProductsByCategory` DropDownList rebinds do jego ObjectDataSource Przechwytywanie produktów dla nowo wybranej kategorii. Gdy `ProductsByCategory` lista DropDownList na wartość została zmieniona, `ProductDetails` DetailsView ObjectDataSource już przeprowadził wyboru wartość jego parametru; w związku z tym wyświetla jego poprzednie wyniki w widoku DetailsView. Interakcji jest przedstawiona na rysunku 20.


[![Wartość ProductsByCategory DropDownList zmieni się po ObjectDataSource ProductDetails DetailsView sprawdza obecność zmian](master-detail-filtering-with-two-dropdownlists-vb/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image58.png)

**Rysunek 20**: `ProductsByCategory` DropDownList wartość zmiany po `ProductDetails` DetailsView ObjectDataSource sprawdza obecność zmian ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image60.png))


Aby rozwiązać ten należy jawnie ponownie powiązać `ProductDetails` widoku DetailsView po `ProductsByCategory` powiązano DropDownList. Firma Microsoft można to zrobić przez wywołanie metody `ProductDetails` DetailsView `DataBind()` — metoda podczas `ProductsByCategory` lista DropDownList na `DataBound` generowane zdarzenie. Dodaj następujący kod obsługi zdarzeń do `MasterDetailsDetails.aspx` strony klasę kodu (odwoływać się do "[programowane Ustawianie wartości parametrów elementu ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" Aby uzyskać informacje na temat dodawania obsługi zdarzeń):


[!code-vb[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample2.vb)]

Po tym jawnym wywołaniem `ProductDetails` DetailsView `DataBind()` metody został dodany, samouczka działa zgodnie z oczekiwaniami. Rysunek 21 prezentuje sposób zmienione zostaną usunięte naszych wcześniejszych problem.


[![Element ProductDetails DetailsView jest generowane zdarzenie z danymi jawnie odświeżane przy ProductsByCategory lista DropDownList na](master-detail-filtering-with-two-dropdownlists-vb/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image61.png)

**Rysunek 21**: `ProductDetails` widoku DetailsView jest jawnie odświeżane przy `ProductsByCategory` lista DropDownList na `DataBound` generowane zdarzenie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image63.png))


## <a name="summary"></a>Podsumowanie

Lista DropDownList służy jako element interfejsu użytkownika nadaje się doskonale dla raportów wzorzec/szczegół przypadku relacji jeden do wielu między rekordy wzorca i szczegółów. W poprzednim samouczek widzieliśmy jak używać jednego DropDownList do filtrowania wyświetlanych przez wybranej kategorii produktów. W tym samouczku możemy zastąpione widoku GridView produktów DropDownList i umożliwia wyświetlenie szczegółów wybranego produktu w widoku DetailsView. Kwestie omówione w tym samouczku można z łatwością rozszerzyć do modeli danych obejmujących wiele relacji jeden do wielu, takich jak klienci, zamówienia i kolejność elementów. Ogólnie rzecz biorąc można dodać DropDownList dla każdej jednostki "jeden" w relacji jeden do wielu.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Hilton Giesenow. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-filtering-with-a-dropdownlist-vb.md)
> [dalej](master-detail-filtering-across-two-pages-vb.md)
