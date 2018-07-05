---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
title: Filtrowanie rekordu głównego/szczegółów przy użyciu kontrolki DropDownList (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku zobaczymy, jak wyświetlać raporty wzorzec/szczegół w pojedynczej strony sieci web za pomocą kontrolek DROPDOWNLIST do wyświetlania rekordów "master" i kontrolką DataList displ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: ad0f1014-1eff-465f-bdc6-93058de00e44
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: e58448da80f1024c2007e23b07c9a1f676ab0980
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377350"
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Filtrowanie rekordu głównego/szczegółów przy użyciu kontrolki DropDownList (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_VB.exe) lub [Pobierz plik PDF](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/datatutorial33vb1.pdf)

> W tym samouczku widzimy, jak wyświetlać raporty wzorzec/szczegół w pojedynczej strony sieci web za pomocą kontrolek DROPDOWNLIST do wyświetlenia DataList, aby wyświetlić "szczegóły" i "główną" rekordów.


## <a name="introduction"></a>Wprowadzenie

Raportu wzorzec/szczegół pierwszego utworzenia korzystający z kontrolki GridView we wcześniejszych przykładach [wzorzec/szczegół filtrowanie przy użyciu kontrolki DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) samouczek, rozpoczyna się poprzez wyświetlenie niektórych zestawu rekordów "główną". Użytkownika można następnie przejść do jednego z rekordów wzorca ten sposób wyświetlania tego rekordu głównego "szczegółowych informacji." Wzorzec/szczegół raporty są idealnym wyborem dla wizualizacji relacji jeden do wielu i wyświetlanie szczegółowych informacji z szczególnie "szerokiego" tabel (te, które mają wiele kolumn). Po rozważyliśmy, jak wdrożyć raporty wzorzec/szczegół za pomocą kontrolki GridView i DetailsView w poprzednich samouczkach. W tym samouczku i kolejne dwa firma Microsoft będzie reexamine te pojęcia, ale skoncentrować się na za pomocą kontrolek DataList i Repeater kontroluje zamiast tego.

W tym samouczku Zapoznamy się przy użyciu kontrolki DropDownList zawiera rekordy "główną", za pomocą wyświetlanych w kontrolkach DataList rekordów "szczegóły".

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Krok 1: Dodawanie stron sieci Web z samouczka wzorzec/szczegół

Zanim zaczniemy, w tym samouczku, najpierw Spójrzmy chwilę, aby dodać folder i stron ASP.NET, który należy do tego samouczka i kolejne dwa rozwiązywania problemów związanych z raportami wzorzec/szczegół za pomocą kontrolek DataList i Repeater. Rozpocznij od utworzenia nowego folderu w projekcie o nazwie `DataListRepeaterFiltering`. Następnie dodaj następujące pięć strony ASP.NET, w tym folderze każdego z nich jest skonfigurowany do używania strony wzorcowej `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Utwórz DataListRepeaterFiltering Folder i dodawanie stron samouczek platformy ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image1.png)

**Rysunek 1**: tworzenie `DataListRepeaterFiltering` Folder i dodawanie stron samouczek platformy ASP.NET


Następnie otwórz `Default.aspx` strony, a następnie przeciągnij `SectionLevelTutorialListing.ascx` kontrolki użytkownika od `UserControls` folder na powierzchnię projektu. Ten formant użytkownika, które utworzyliśmy w [strony wzorcowe i nawigacja w witrynie](../introduction/master-pages-and-site-navigation-vb.md) samouczek, wylicza mapy witryny i wyświetla samouczków z bieżącej sekcji na liście punktowanej.


[![Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image2.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image4.png))


W celu wyświetlania listy punktowanej samouczki wzorzec/szczegół, firma Microsoft będzie tworzony, należy dodać je do mapy witryny. Otwórz `Web.sitemap` pliku i Dodaj następujący kod po znaczników węzeł mapy witryny "Wyświetlanie danych za pomocą kontrolek DataList i Repeater":

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample1.xml)]


![Aktualizacja mapy witryny, aby uwzględnić nowe strony ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image5.png)

**Rysunek 3**: Aktualizacja mapy witryny, aby uwzględnić nowe strony ASP.NET


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Krok 2: Wyświetlanie kategorii w kontrolki DropDownList

Raport wzorzec/szczegół spowoduje wyświetlenie listy kategorii z kontrolki DropDownList, za pomocą elementu wybranej listy produktów wyświetlane dalej na dół strony w kontrolkach DataList. Pierwsze zadanie w przód od nas, następnie jest kategorie wyświetlane w kontrolki DropDownList. Zacznij od otwarcia `FilterByDropDownList.aspx` stronie `DataListRepeaterFiltering` folder i przeciągnij kontrolki DropDownList z przybornika do projektanta strony. Następnym etapem jest skonfigurowanie DropDownList `ID` właściwość `Categories`. Kliknij łącze Wybierz źródło danych z kontrolki DropDownList tagów inteligentnych i utworzyć nowe kontrolki ObjectDataSource, o nazwie `CategoriesDataSource`.


[![Dodawanie nowego elementu ObjectDataSource, o nazwie CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image6.png)

**Rysunek 4**: Dodaj nazwę nowej kontrolki ObjectDataSource `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image8.png))


Konfigurowanie nowej kontrolki ObjectDataSource w taki sposób, że wywołuje `CategoriesBLL` klasy `GetCategories()` metody. Po skonfigurowaniu ObjectDataSource musimy nadal określić które pole źródła danych powinny być wyświetlane w metody DropDownList i jedną powinna być skojarzona jako wartość dla każdego elementu listy. Masz `CategoryName` pola jako ekran i `CategoryID` jako wartość dla każdego elementu listy.


[![Masz wyświetlana lista DropDownList na pole CategoryName i użyj CategoryID jako wartość](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image9.png)

**Rysunek 5**: wyświetlone DropDownList `CategoryName` pola i użyj `CategoryID` jako wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image11.png))


W tym momencie mamy wypełniony rekordy z kontrolki DropDownList `Categories` tabeli (wszystkie wykonywane w ciągu około sześciu sekund). Rysunek 6. pokazuje nasz postęp tej pory, podczas wyświetlania za pośrednictwem przeglądarki.


[![Menu rozwijane zawiera listę bieżących kategorii](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image12.png)

**Rysunek 6**: Wyświetla listę rozwijaną listę bieżących kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Krok 2: Dodawanie kontrolek DataList produktów

Ostatnim krokiem w naszego raportu wzorzec/szczegół jest aby wyświetlić listę produktów skojarzonych z wybranej kategorii. W tym celu na stronie Dodaj kontrolką DataList i utworzyć nowe kontrolki ObjectDataSource, o nazwie `ProductsByCategoryDataSource`. Ma `ProductsByCategoryDataSource` kontroli pobierania danych z `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` metody. Ponieważ ten raport wzorzec/szczegół jest tylko do odczytu, wybierz opcję (Brak) na kartach INSERT, UPDATE i DELETE.


[![Wybierz metodę GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image15.png)

**Rysunek 7**: Wybierz `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image17.png))


Po kliknięciu przycisku Dalej, Kreator ObjectDataSource nam monituje o podanie źródła wartość `GetProductsByCategoryID(categoryID)` metody *`categoryID`* parametru. Aby użyć wartości wybranych `categories` elementu DropDownList Ustaw źródło parametru do kontroli i ControlID do `Categories`.


[![Ustaw categoryID parametru na wartość DropDownList kategorii](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image18.png)

**Rysunek 8**: Ustaw *`categoryID`* parametru na wartość `Categories` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image20.png))


Po ukończeniu pracy kreatora Konfigurowanie źródła danych, program Visual Studio automatycznie wygeneruje `ItemTemplate` dla DataList, który wyświetla nazwę i wartość każdego pola danych. Możemy poprawić DataList użyć zamiast tego `ItemTemplate` wyświetlającą tylko nazwę produktu, kategorii, dostawca, ilość na jednostkę, a cena wraz z `SeparatorTemplate` , wprowadza `<hr>` element między poszczególne elementy. Zamierzam używać `ItemTemplate` z przykładu w [wyświetlanie danych za pomocą kontrolek DataList i Repeater](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb.md) samouczka, ale możesz użyć dowolnego oznaczenia szablonu, możesz znaleźć najbardziej atrakcyjnego.

Po wprowadzeniu tych zmian, listy DataList i jego ObjectDataSource znaczników powinien wyglądać podobnie do następującego:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample2.aspx)]

Poświęć chwilę, aby wyewidencjonować postępach w przeglądarce. Po raz pierwszy, odwiedzając stronę, tych produktów należących do wybranej kategorii (Beverages) są wyświetlane (jak pokazano na rysunku 9), ale zmiana metody DropDownList nie powoduje aktualizacji danych. Jest to spowodowane ogłaszania zwrotnego musi nastąpić DataList do zaktualizowania. W tym firma Microsoft może albo ustawić DropDownList `AutoPostBack` właściwości `true` lub dodać kontrolkę przycisku w sieci Web do strony. W tym samouczku załączania mogę ustawić DropDownList `AutoPostBack` właściwość `true`.

Rysunki 9 i 10 ilustrują raportu wzorzec/szczegół w działaniu.


[![Po pierwsze, odwiedzając stronę, są wyświetlane produkty spożywczy](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image21.png)

**Rysunek 9**: po pierwsze, odwiedzając stronę, są wyświetlane produkty spożywczy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image23.png))


[![Zaznaczenie nowego produktu (produkty) automatycznie powoduje odświeżenie strony, aktualizowanie kontrolki DataList](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image24.png)

**Na rysunku nr 10**: zaznaczenie nowego produktu (produkty) automatycznie powoduje odświeżenie strony, aktualizowanie kontrolki DataList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Dodawanie elementu listy "— Wybierz kategorię--"

Podczas odwiedzania najpierw `FilterByDropDownList.aspx` stronie kategorii pierwszego elementu listy przez kontrolki DropDownList (Beverages) jest zaznaczona domyślnie, przedstawiający produktów spożywczy w elemencie DataList. W *wzorzec/szczegół filtrowanie przy użyciu kontrolki DropDownList* samouczek dodaliśmy opcję "--Wybierz kategorię--" do metody DropDownList została zaznaczona domyślnie, a następnie po wybraniu wyświetlane *wszystkich* programu produkty w bazie danych. Takie podejście był zarządzany zawierającą listę produktów w GridView, ponieważ każdy wiersz produktu zajmował niewielkiej powierzchnię ekranu. Za pomocą kontrolek DataList jednak informacji każdego produktu zużywa dużo większe fragment ekranu. Nadal teraz dodać opcji "--Wybierz kategorię —" jest domyślnie zaznaczone, a zamiast go Pokaż wszystkie produkty po wybraniu Skonfigurujmy ją tak, aby pokazywał żadnych produktów.

Aby dodać nowy element listy do metody DropDownList, przejdź do okna właściwości, a następnie kliknąć wielokropek w `Items` właściwości. Dodaj nowy element listy z `Text` "— Wybierz kategorię--" i `Value` `0`.


![Dodaj](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image27.png)

**Rysunek 11**: Dodawanie elementu listy "— Wybierz kategorię--"


Alternatywnie można dodać elementu listy, dodając następujący kod do metody DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample3.aspx)]

Ponadto musimy kontrolki DropDownList `AppendDataBoundItems` do `true` ponieważ jeśli jest równa `false` (ustawienie domyślne), w przypadku kategorie są powiązane z metody DropDownList z kontrolki ObjectDataSource zostanie zastąpiona się wszystkie listy dodane ręcznie elementy.


![Ustaw właściwość AppendDataBoundItems na wartość True](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image28.png)

**Rysunek 12**: Ustaw `AppendDataBoundItems` właściwości na wartość True


Przyczyna została wybrana wartość `0` dla listy "— Wybierz kategorię--" elementu jest, ponieważ nie ma żadnych kategorii w systemie o wartości `0`, dlatego zostaną zwrócone żadne rekordy nie produktu po wybraniu elementu listy "— Wybierz kategorię--". Można to potwierdzić, Poświęć chwilę, aby odwiedzić stronę za pośrednictwem przeglądarki. Tak jak pokazano rysunek 13, gdy początkowo wyświetlanie strony elementu listy "— Wybierz kategorię--" jest zaznaczone, a produkty nie są wyświetlane.


[![Gdy](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image29.png)

**Rysunek 13**: po wybraniu elementu listy "— Wybierz kategorię--" produkty nie są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image31.png))


Jeśli wolisz będzie wyświetlane *wszystkich* produktów po wybraniu opcji "--Wybierz kategorię —" Użyj wartości `-1` zamiast tego. Uważni czytnik będzie przypominać tej kopii w *wzorzec/szczegół filtrowanie przy użyciu kontrolki DropDownList* samouczek Zaktualizowaliśmy `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` metody, aby Jeśli *`categoryID`* wartość `-1` przekazaną produktu wszystkie rekordy zostały zwrócone.

## <a name="summary"></a>Podsumowanie

Podczas wyświetlania danych powiązanych hierarchicznie, często pomaga do prezentowania danych za pomocą raportów wzorzec/szczegół, z których użytkownik może uruchomić perusing dane z góry hierarchii i przechodzenie do szczegółów. W tym samouczku zbadaliśmy tworzenia raportu proste wzorzec/szczegół wyświetlanie wybranej kategorii produktów. To było wykonywane przy użyciu kontrolki DropDownList dla listy kategorii i DataList produktów należących do wybranej kategorii.

W następnym samouczku Zapoznamy się oddzielenie wzorzec i szczegóły rekordów na dwóch stronach. Na pierwszej stronie listy rekordów "główną" pojawi się Link, aby wyświetlić szczegóły. Kliknięcie łącza spowoduje whisk użytkownika do drugiej strony wyświetli szczegóły wybranego rekordu głównego.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla...

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Randy Schmidt. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
> [dalej](master-detail-filtering-acess-two-pages-datalist-vb.md)
