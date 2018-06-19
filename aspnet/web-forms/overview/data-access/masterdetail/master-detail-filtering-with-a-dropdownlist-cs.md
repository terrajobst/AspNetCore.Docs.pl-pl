---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
title: Wzorzec/szczegół filtrowanie z DropDownList (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku będziesz przedstawiono sposób wyświetlania rekordy główne w formancie DropDownList i szczegóły wybranego elementu listy w widoku GridView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 53e659cc-eefb-40c1-a1dc-559481c99443
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 42a6a76b0b05045bed1ada227b7c32a51600b760
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880638"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Wzorzec/szczegół filtrowanie z DropDownList (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_7_CS.exe) lub [pobierania plików PDF](master-detail-filtering-with-a-dropdownlist-cs/_static/datatutorial07cs1.pdf)

> W tym samouczku będziesz przedstawiono sposób wyświetlania rekordy główne w formancie DropDownList i szczegóły wybranego elementu listy w widoku GridView.


## <a name="introduction"></a>Wprowadzenie

Spotykanym typem raportu jest *główny/szczegółowy raport*, w poprzez wyświetlenie niektórych zestawu rekordów "główny" zaczynający raportu. Użytkownik może następnie przejść do jednego z głównym rekordów ten sposób wyświetlania tego rekordu głównego "szczegółowych informacji." Wzorzec/szczegół raporty są idealnym wyborem w przypadku wizualizacja relacje jeden do wielu, takich jak raportu wyświetlane są wszystkie kategorie i następnie Umożliwianie użytkownikowi wybierz określonej kategorii i wyświetlić związane z nimi produkty. Ponadto raporty wzorzec/szczegół są przydatne w przypadku wyświetlania szczegółowych informacji z szczególnie "szerokie" tabele (te, które zawiera wiele kolumn). Na przykład poziom "główny" raport wzorzec/szczegół mogą być wyświetlane tylko produktu nazwy i Jednostka ceny produktów w bazie danych i przechodzenie od określonego produktu czy Pokaż pola dodatkowe produktu (kategorii, dostawca, ilość na jednostkę, i tak dalej).

Istnieje wiele sposobów, które można zaimplementować wzorzec/szczegół raportu. Przez to i trzech kolejnych samouczkach przyjrzymy szerokiej gamy raportów wzorzec/szczegół. W tym samouczku zajmiemy się tym sposób wyświetlania rekordów wzorca [kontroli DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) i szczegóły wybranego elementu listy w widoku GridView. W szczególności w tym samouczku wzorzec/szczegół raportu spowoduje wyświetlenie listy kategorii i informacje o produkcie.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Krok 1: Wyświetlanie kategorii w DropDownList

Nasze raportu wzorzec/szczegół spowoduje wyświetlenie listy kategorii w DropDownList z produktami elementu wybranej listy wyświetlane dodatkowe w dół na stronie w widoku GridView. Pierwszym zadaniem przed nami, jest następnie kategorie wyświetlane w DropDownList. Otwórz `FilterByDropDownList.aspx` strony `Filtering` , przeciągnij z przybornika do strony projektanta DropDownList i ustawić jej `ID` właściwości `Categories`. Następnie kliknij łącze Wybierz źródło danych z DropDownList tagów inteligentnych. Spowoduje to wyświetlenie Kreator konfiguracji źródła danych.


[![Określ źródło danych DropDownList](master-detail-filtering-with-a-dropdownlist-cs/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image1.png)

**Rysunek 1**: Określ źródło danych DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image3.png))


Wybierz dodać nowy element ObjectDataSource o nazwie `CategoriesDataSource` który wywołuje `CategoriesBLL` klasy `GetCategories()` metody.


[![Dodaj nowy element ObjectDataSource o nazwie CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-cs/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image4.png)

**Rysunek 2**: Dodaj nowy element ObjectDataSource o nazwie `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image6.png))


[![Wybierz klasy CategoriesBLL](master-detail-filtering-with-a-dropdownlist-cs/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image7.png)

**Rysunek 3**: wybierz opcję do użycia `CategoriesBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image9.png))


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetCategories()](master-detail-filtering-with-a-dropdownlist-cs/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image10.png)

**Rysunek 4**: Konfigurowanie ObjectDataSource użyć `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image12.png))


Po skonfigurowaniu ObjectDataSource nadal trzeba określić, jakie pola źródła danych mają być wyświetlane w DropDownList i jedną powinna być skojarzona jako wartość dla elementu listy. Ma `CategoryName` pole jako ekran i `CategoryID` jako wartość dla każdego elementu listy.


[![Ma pole CategoryName i użyj CategoryID wyświetlana lista DropDownList jako wartość](master-detail-filtering-with-a-dropdownlist-cs/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image13.png)

**Rysunek 5**: wyświetlone DropDownList `CategoryName` pola i użyj `CategoryID` jako wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image15.png))


W tym momencie mamy DropDownList formant, który jest wypełniana rekordy z `Categories` tabeli (wszystkie dokonane w ciągu około sześciu sekund). Rysunek 6 przedstawia naszych postępu dotychczasowych widzianego za pośrednictwem przeglądarki.


[![Rozwijana lista bieżącej kategorii](master-detail-filtering-with-a-dropdownlist-cs/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image16.png)

**Rysunek 6**: listy rozwijane A bieżącej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Krok 2: Dodawanie widoku GridView produktów

Ten ostatni krok w raporcie naszych wzorzec/szczegół jest na liście produktów skojarzonych z wybranej kategorii. W tym celu Dodaj element GridView do strony i Utwórz nowy element ObjectDataSource o nazwie `productsDataSource`. Ma `productsDataSource` kontroli wybrakowane za pomocą `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` metody.


[![Wybierz metodę GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-cs/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image19.png)

**Rysunek 7**: Wybierz `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image21.png))


Po wybraniu tej metody, Kreator ObjectDataSource nam monituje o podanie wartości dla metody *`categoryID`* parametru. Aby użyć wartości wybranych `categories` elementu DropDownList ustawiono parametr źródła kontroli i ControlID do `Categories`.


[![Ustaw categoryID parametru wartość DropDownList kategorii](master-detail-filtering-with-a-dropdownlist-cs/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image22.png)

**Rysunek 8**: Ustaw *`categoryID`* parametru z wartością `Categories` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image24.png))


Poświęć chwilę, aby zapoznaj się z naszym postęp w przeglądarce. Podczas odwiedzania najpierw strony, tych produktów należy do wybranej kategorii (napoje) są wyświetlane (jak pokazano na rysunku nr 9), ale zmiana DropDownList nie powoduje aktualizacji danych. Jest to spowodowane odświeżania strony musi nastąpić można zaktualizować widoku GridView. W tym celu mamy dwie opcje (które wymaga pisania żadnego kodu):

- **Ustaw kategorie lista DropDownList na**[właściwości AutoPostBack](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**na wartość True.** (Można to zrobić, zaznaczając opcję Włącz AutoPostBack w tagu DropDownList.) To wyzwoli odświeżania strony zawsze, gdy lista DropDownList wybrany element zostanie zmieniona przez użytkownika. W związku z tym gdy użytkownik wybierze nową kategorię z DropDownList nastąpi odświeżenie strony i widoku GridView zostanie zaktualizowany z produktami nowo wybranej kategorii. (Jest to podejście, które wcześniej używanych w ramach tego samouczka).
- **Dodawanie formantu przycisku Web obok DropDownList.** Ustaw jego `Text` właściwości odświeżania lub podobny. Z tej metody użytkownik będzie musiał wybierz nową kategorię, a następnie kliknij przycisk. Kliknięcie przycisku powoduje odświeżenie strony i zaktualizować widoku GridView, aby wyświetlić listę tych produktów wybranej kategorii.

Rysunki 9 i 10 ilustrują raportu wzorzec/szczegół w akcji.


[![Podczas odwiedzania najpierw strony, są wyświetlane produkty napoju](master-detail-filtering-with-a-dropdownlist-cs/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image25.png)

**Rysunek 9**: podczas odwiedzania najpierw strony, są wyświetlane produkty napoju ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image27.png))


[![Automatyczne zaznaczanie nowego produktu (produktu) powoduje odświeżenie strony, aktualizowanie widoku GridView](master-detail-filtering-with-a-dropdownlist-cs/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image28.png)

**Na rysunku nr 10**: zaznaczenie nowego produktu (produktu) automatycznie powoduje odświeżenie strony, aktualizowanie widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Dodawanie elementu listy "--Wybierz kategorię--"

Podczas odwiedzania najpierw `FilterByDropDownList.aspx` strony kategorie lista DropDownList na pierwszy element listy (napoje) jest zaznaczone domyślnie, wyświetlane produkty napoju w widoku GridView. Zamiast przedstawiający pierwszej kategorii produktów, firma Microsoft może być zamiast tego elementu DropDownList wybrane który mówi wyglądać mniej więcej tak, "--Wybierz kategorię--".

Aby dodać nowy element listy do DropDownList, przejdź do okna właściwości, a następnie kliknij wielokropek w `Items` właściwości. Dodaj nowy element listy z `Text` "--Wybierz kategorię--" i `Value` `-1`.


[![Dodaj wybierz kategorię--elementu listy](master-detail-filtering-with-a-dropdownlist-cs/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image31.png)

**Rysunek 11**: Dodaj wybierz kategorię — element listy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image33.png))


Alternatywnie można dodać elementu listy, dodając następujący kod do DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample1.aspx)]

Ponadto należy ustawić kontrolę lista DropDownList `AppendDataBoundItems` na wartość True, ponieważ podczas kategorie są powiązane z DropDownList z elementu ObjectDataSource będzie zastępują żadnych elementów listy dodane ręcznie czy `AppendDataBoundItems` nie ma wartość True.


![Ustaw wartość True dla właściwości AppendDataBoundItems](master-detail-filtering-with-a-dropdownlist-cs/_static/image34.png)

**Rysunek 12**: Ustaw `AppendDataBoundItems` właściwości na wartość True


Po wprowadzeniu tych zmian podczas odwiedzania najpierw na stronie opcji "--Wybierz kategorię--" jest zaznaczone, a produkty nie są wyświetlane.


[![Ładowania strony początkowej produkty nie są wyświetlane](master-detail-filtering-with-a-dropdownlist-cs/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image35.png)

**Rysunek 13**: na początkowej stronie obciążenia nr produkty są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image37.png))


Jest przyczyna produkty nie są wyświetlane po, ponieważ wybrano element listy "--Wybierz kategorię--", ponieważ jego wartość wynosi `-1` i nie ma produktów w bazie danych o `CategoryID` z `-1`. Jeśli jest to zachowanie ma, a następnie gotowe na tym etapie! Jeśli jednak chcesz wyświetlić *wszystkie* kategorii po wybraniu elementu listy "--Wybierz kategorię--", wróć do `ProductsBLL` klasy i dostosować `GetProductsByCategoryID(categoryID)` metodę, tak że wywołuje `GetProducts()` — metoda Jeśli przekazany w *`categoryID`* parametru jest mniejszy od zera:

[!code-csharp[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample2.cs)]

Zastosowanych w tym miejscu jest podobna do metody możemy używany do wyświetlania wszystkich dostawców w [deklaratywne parametry](../basic-reporting/declarative-parameters-cs.md) samouczek, mimo że w tym przykładzie używamy wartość `-1` wskazująca, że wszystkie rekordy powinny być pobrany w przeciwieństwie do `null`. Jest to spowodowane *`categoryID`* parametr `GetProductsByCategoryID(categoryID)` metoda oczekuje jako przekazano wartość całkowitą, natomiast w samouczku deklaratywne parametry zostały możemy przekazując parametr wejściowy ciąg znaków.

Zrzut ekranu przedstawia rysunek 14 `FilterByDropDownList.aspx` po wybraniu opcji "--Wybierz kategorię--". W tym miejscu domyślnie są wyświetlane wszystkie produkty, a użytkownik można ograniczyć wyświetlanie przez wybranie jednej konkretnej kategorii.


[![Wszystkie produkty są teraz wyświetlane domyślnie](master-detail-filtering-with-a-dropdownlist-cs/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image38.png)

**Rysunek 14**: wszystkie produkty są teraz wyświetlane domyślnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-cs/_static/image40.png))


## <a name="summary"></a>Podsumowanie

Podczas wyświetlania danych dotyczących hierarchicznie, pomaga często zawierają dane przy użyciu wzorzec/szczegół raportów, z których użytkownik może uruchomić perusing danych od góry hierarchii i przejść do szczegółów. W tym samouczku będziemy zbadane, tworzenie prostego główny/szczegółowy raport pokazujący wybranej kategorii produktów. Zostało to zrobić za pomocą DropDownList listy kategorii i GridView produktów należących do wybranej kategorii.

W [następny samouczek](master-detail-filtering-with-two-dropdownlists-cs.md) przeniesiemy interfejsu DropDownList w jednym kroku, za pomocą dwóch DropDownLists.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Next](master-detail-filtering-with-two-dropdownlists-cs.md)
