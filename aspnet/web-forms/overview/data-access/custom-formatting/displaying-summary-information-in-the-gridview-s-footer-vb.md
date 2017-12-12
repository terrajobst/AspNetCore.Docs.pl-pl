---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
title: "Wyświetlanie podsumowania w stopce w widoku GridView (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Informacje podsumowania często są wyświetlane w dolnej części raportu w wierszu podsumowania. Kontrolki widoku siatki mogą obejmować wiersza stopki, do których komórek możemy pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 41c818b7-603a-402b-8847-890a63547b6f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: e5b7e39a44d43a857c62842ea3e1dddcacf05c9b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="displaying-summary-information-in-the-gridviews-footer-vb"></a>Wyświetlanie podsumowania w stopce w widoku GridView (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_15_VB.exe) lub [pobierania plików PDF](displaying-summary-information-in-the-gridview-s-footer-vb/_static/datatutorial15vb1.pdf)

> Informacje podsumowania często są wyświetlane w dolnej części raportu w wierszu podsumowania. Kontrolki widoku siatki mogą obejmować wiersza stopki, do których komórek można programowo używamy agregowanie danych. W tym samouczku będzie przedstawiono sposób wyświetlania agregowanie danych, w tym wierszu stopki.


## <a name="introduction"></a>Wprowadzenie

Oprócz wyświetlania każdego ceny te produkty, jednostki w magazynie, jednostki kolejności i poziomy zmiany kolejności, użytkownik może również być zainteresowane agregacji informacje, takie jak średnia cena, liczba jednostek w magazynie i tak dalej. Takie informacje podsumowujące często jest wyświetlane w dolnej części raportu w wierszu podsumowania. Kontrolki widoku siatki mogą obejmować wiersza stopki, do których komórek można programowo używamy agregowanie danych.

To zadanie przedstawia nam trzy wyzwania:

1. Konfigurowanie widoku GridView, aby wyświetlić jego wiersz stopki
2. Określanie danych podsumowania; oznacza to jak możemy obliczenia średniej ceny lub całkowitej liczby jednostek w magazynie?
3. Wstrzykiwania danych podsumowania do komórek wiersza stopki

W tym samouczku będziesz przedstawiono sposób rozwiązać te problemy. W szczególności utworzymy strony, który zawiera listę kategorii z listy rozwijanej z wybranych kategorii produktów wyświetlane w widoku GridView. Widoku GridView będzie zawierać wiersz stopki pokazujący średniej ceny i łączną liczbę jednostek w magazynie i na produkty w danej kategorii.


[![Informacje podsumowania jest wyświetlany w wierszu stopki w widoku GridView](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image1.png)

**Rysunek 1**: Podsumowanie informacje są wyświetlane w widoku GridView stopki wiersza ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image3.png))


Ten samouczek z kategorii produktów wzorzec/szczegół interfejsu, bazując na pojęcia objęte wcześniej [wzorzec/szczegół filtrowania z DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) samouczka. Jeśli nie została jeszcze pracował samouczkiem wcześniej, zrób to przed kontynuowaniem na ten zestaw.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Krok 1: Dodawanie DropDownList kategorie i produktów w widoku GridView

Przed dotyczące nad dodawania informacje podsumowania do stopki w widoku GridView, najpierw po prostu utworzymy wzorzec/szczegół raportu. Po pierwszym kroku została ukończona, wyjaśniono, jak dołączyć dane podsumowujące.

Uruchamianie przez otwarcie `SummaryDataInFooter.aspx` strony `CustomFormatting` folderu. Dodaj kontrolę lista DropDownList i ustawić jej `ID` do `Categories`. Następnie kliknij łącze Wybierz źródło danych z tagów inteligentnych DropDownList i wybrać opcję Dodaj nowy element ObjectDataSource o nazwie `CategoriesDataSource` który wywołuje `CategoriesBLL` klasy `GetCategories()` metody.


[![Dodaj nowy element ObjectDataSource o nazwie CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image4.png)

**Rysunek 2**: Dodaj nowy element ObjectDataSource o nazwie `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image6.png))


[![Ma ObjectDataSource wywołania metody GetCategories() klasy CategoriesBLL](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image7.png)

**Rysunek 3**: ma wywołać element ObjectDataSource `CategoriesBLL` klasy `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image9.png))


Po skonfigurowaniu ObjectDataSource, Kreator wyświetli nam w konfiguracji źródła danych DropDownList kreatora, w którym należy określić, jakie wartości pola danych powinny być wyświetlane i który z nich powinien odpowiadać wartości lista DropDownList na `ListItem` s. Ma `CategoryName` pole wyświetlane i użyj `CategoryID` jako wartość.


[![Użyj pola CategoryID i CategoryName jako tekst i wartość elementy ListItems, odpowiednio](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image10.png)

**Rysunek 4**: Użyj `CategoryName` i `CategoryID` pola jako `Text` i `Value` dla `ListItem` s, odpowiednio ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image12.png))


W tym momencie mamy DropDownList (`Categories`) w systemie który zawiera listę kategorii. Teraz należy dodać element GridView, który zawiera listę produktów, które należą do wybranej kategorii. Zanim przejdziemy, jednak Poświęć chwilę, aby zaznaczyć pole wyboru Włącz AutoPostBack w tagu DropDownList. Zgodnie z opisem w *wzorzec/szczegół filtrowania z DropDownList* samouczek, ustawiając DropDownList `AutoPostBack` właściwości `True` stronie będzie można opublikować ponownie zawsze wartość DropDownList zostanie zmieniona. Spowoduje to GridView należy odświeżyć, przedstawiający tych produktów dla nowo wybranej kategorii. Jeśli `AutoPostBack` właściwość jest ustawiona na `False` (domyślnie), zmiana kategorii nie będzie powodować odświeżenie strony, a więc nie będzie aktualizować wymienionych produktów.


[![Pole wyboru Włącz AutoPostBack w DropDownList tagów inteligentnych](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image13.png)

**Rysunek 5**: pole wyboru Włącz AutoPostBack w tagu DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image15.png))


Dodaj kontrolce GridView do strony, aby wyświetlić produktów dla wybranej kategorii. Ustaw w widoku GridView `ID` do `ProductsInCategory` i powiązać ją z nowego elementu ObjectDataSource o nazwie `ProductsInCategoryDataSource`.


[![Dodaj nowy element ObjectDataSource o nazwie ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image16.png)

**Rysunek 6**: Dodaj nowy element ObjectDataSource o nazwie `ProductsInCategoryDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image18.png))


Skonfiguruj ObjectDataSource, dzięki czemu wywołuje `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` metody.


[![Ma ObjectDataSource wywołania metody GetProductsByCategoryID(categoryID)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image19.png)

**Rysunek 7**: ma wywołać element ObjectDataSource `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image21.png))


Ponieważ `GetProductsByCategoryID(categoryID)` metoda przyjmuje parametr wejściowy w ostatnim kroku kreatora można określić źródło wartości parametru. Aby wyświetlić te produkty z wybranej kategorii, mieć parametr pobierane z `Categories` DropDownList.


[![Pobierz categoryID wartość parametru z DropDownList kategorie wybrane](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image22.png)

**Rysunek 8**: Pobierz  *`categoryID`*  wartości parametru z DropDownList kategorie wybrane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image24.png))


Po zakończeniu pracy kreatora widoku GridView będzie mieć elementu BoundField dla każdej właściwości produktu. Umożliwia czyszczenie, aby tylko te BoundFields `ProductName`, `UnitPrice`, `UnitsInStock`, i `UnitsOnOrder` BoundFields są wyświetlane. Możesz także dodać wszystkie ustawienia na poziomie pola do pozostałych BoundFields (takie jak formatowanie `UnitPrice` jako walutę). Po wprowadzeniu tych zmian, deklaratywne znaczników w widoku GridView powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample1.aspx)]

W tym momencie mamy pełni funkcjonalnej raportu wzorzec/szczegół, który zawiera nazwę cenie jednostkowej, jednostki w magazynie i jednostki w kolejności tych produktów, które należą do wybranej kategorii.


[![Pobierz categoryID wartość parametru z DropDownList kategorie wybrane](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image25.png)

**Rysunek 9**: Pobierz  *`categoryID`*  wartości parametru z DropDownList kategorie wybrane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Krok 2: Wyświetlanie stopki w widoku GridView

Kontrolki widoku siatki można wyświetlić wiersz nagłówka i stopki. Te wiersze są wyświetlane w zależności od wartości `ShowHeader` i `ShowFooter` właściwości, odpowiednio z `ShowHeader` przyjęty `True` i `ShowFooter` do `False`. Aby po prostu Dołącz stopkę w widoku GridView ustaw jego `ShowFooter` właściwości `True`.


[![Wartość True właściwości ShowFooter w widoku GridView](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image28.png)

**Na rysunku nr 10**: Ustaw w widoku GridView `ShowFooter` właściwości `True` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image30.png))


Wiersz stopki ma komórki dla każdego pola zdefiniowane w widoku GridView; Jednak te komórki są domyślnie pusta. Poświęć chwilę, aby wyświetlić postęp naszych w przeglądarce. Z `ShowFooter` teraz ustawioną właściwość `True`, widoku GridView zawiera stopkę pusty wiersz.


[![Wiersz stopki obejmuje teraz widoku GridView](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image31.png)

**Rysunek 11**: widoku GridView zawiera teraz wiersz stopki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image33.png))


Wiersz stopki w rysunek 11 nie wyróżniające, ponieważ mają one białe tło. Utwórzmy `FooterStyle` klasy CSS w `Styles.css` który określa ciemny czerwonym tle, a następnie skonfiguruj `GridView.skin` pliku skórki w `DataWebControls` motywu, aby przypisać tę klasę CSS w widoku GridView `FooterStyle`w `CssClass` właściwości. Jeśli potrzebujesz malowanie skórek i motywów, odwołaj się do [wyświetlanie danych z ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) samouczka.

Rozpocznij od dodania następującej klasy CSS do `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample2.css)]

`FooterStyle` Klasy CSS przypomina w stylu `HeaderStyle` klasy, mimo że `HeaderStyle`na kolor tła jest subtlety ciemniejszego i jego tekst jest wyświetlany pogrubioną czcionką. Ponadto tekst w stopce jest wyrównany konieczne jest wyśrodkowany tekst nagłówka.

Dalej, aby skojarzyć tę klasę CSS z każdego widoku GridView stopki, otwórz `GridView.skin` w pliku `DataWebControls` motywu i zestaw `FooterStyle`w `CssClass` właściwości. Po dodaniu tego pliku znaczników powinien wyglądać następująco:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample3.aspx)]

Jak poniżej przedstawiono zrzut ekranu, ta zmiana powoduje, że stopki wyraźnie więcej.


[![Kolor tła czerwonawego ma teraz wiersz stopki w widoku GridView](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image34.png)

**Rysunek 12**: wiersz stopki GridView ma teraz czerwonawego kolor tła ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>Krok 3: Przetwarzanie danych podsumowania

Z stopki w widoku GridView wyświetlane dalej żądania skierowane do nas jest jak obliczeniowe danych podsumowania. Istnieją dwa sposoby obliczeniowe tych agregacji informacji:

1. Za pomocą kwerendy SQL firma Microsoft może wydać dodatkowe zapytania do bazy danych można obliczyć danych podsumowania dla określonej kategorii. SQL zawiera szereg funkcji agregujących wraz z programem `GROUP BY` klauzuli określające dane, w którym powinien podsumowywania danych. Następujące zapytanie SQL może przywrócić potrzebnych informacji:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample4.sql)]

    Oczywiście nie ma do wysłania tej kwerendy bezpośrednio z `SummaryDataInFooter.aspx` strony, ale raczej, tworząc metody w `ProductsTableAdapter` i `ProductsBLL`.
2. Obliczeniowe te informacje, ponieważ została ona dodana do widoku GridView zgodnie z opisem w [niestandardowe formatowanie oparte na danych](custom-formatting-based-upon-data-cs.md) samouczku, widoku GridView `RowDataBound` obsługi zdarzenia generowane raz dla każdego wiersza dodawane do widoku GridView po jego zostały z danymi. Tworząc program obsługi zdarzeń dla tego zdarzenia możemy Zachowaj uruchomiony całkowitej wartości chcemy agregacji. Po ostatnim wierszu danych została powiązana z widoku GridView mamy sum i informacje niezbędne do obliczenia średniej.

I wykorzystywać zwykle drugi podejście podczas podróży jest zapisywany w bazie danych oraz nakład pracy niezbędne do realizacji funkcji podsumowania Warstwa dostępu do danych i warstwy logiki biznesowej, ale albo podejście będą wystarczające. W tym samouczku umożliwia druga opcja i śledzenie sumy za pomocą `RowDataBound` obsługi zdarzeń.

Utwórz `RowDataBound` programu obsługi zdarzeń dla widoku GridView wybierając widoku GridView w projektancie, klikając ikonę bolt z okna właściwości i dwukrotnie `RowDataBound` zdarzeń. Alternatywnie można wybrać widoku GridView i jego zdarzeń RowDataBound z list rozwijanych w górnej części pliku klasy związane z kodem ASP.NET. Spowoduje to utworzenie nowego obsługi zdarzenia o nazwie `ProductsInCategory_RowDataBound` w `SummaryDataInFooter.aspx` klasie związanej z kodem strony.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample5.vb)]

Aby sumy musimy zdefiniować zmienne poza zasięgiem programu obsługi zdarzeń. Utwórz następujące cztery zmienne na poziomie strony:

- `_totalUnitPrice`, typu`Decimal`
- `_totalNonNullUnitPriceCount`, typu`Integer`
- `_totalUnitsInStock`, typu`Integer`
- `_totalUnitsOnOrder`, typu`Integer`

Następnie należy napisać kod, aby zwiększyć te trzy zmienne dla każdego wiersza danych napotkał w `RowDataBound` obsługi zdarzeń.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample6.vb)]

`RowDataBound` Uruchamia program obsługi zdarzeń przez zapewnienie, że firma Microsoft jest zajmujących się DataRow. Po którym zostały `Northwind.ProductsRow` wystąpienia, która właśnie została powiązana z `GridViewRow` obiektu w `e.Row` jest przechowywana w zmiennej `product`. Następny, uruchomione całkowita zmienne są zwiększany o odpowiednie wartości bieżącego produktu (przy założeniu, że bazy danych nie zawierają one `NULL` wartości). Firma Microsoft zachować informacje o działających `UnitPrice` łącznie i liczbę inną niż`NULL` `UnitPrice` rejestruje ponieważ średniej ceny jest iloraz tych dwóch liczb.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Krok 4: Wyświetlanie danych podsumowania w stopce

Z danych podsumowania sumowane ostatnim krokiem jest Wyświetl ją w wierszu stopki w widoku GridView. To zadanie, można wykonać programowo za pomocą `RowDataBound` obsługi zdarzeń. Odwołania, który `RowDataBound` obsługi zdarzenia generowane dla *co* wiersza, który jest powiązany z widoku GridView, w tym wierszu stopki. Firma Microsoft w związku z tym rozszerzyć naszych obsługi zdarzeń, aby wyświetlić dane w wierszu stopki, używając następującego kodu:


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample7.vb)]

Ponieważ wiersza stopka jest dodawana do widoku GridView po wszystkich wierszy danych zostały dodane, firma Microsoft może być pewność, że w czasie już wszystko gotowe do wyświetlenia danych podsumowania w stopce, który ukończy obliczenia sumy. Ostatni krok, wówczas można ustawić wartości w komórkach stopki.

Do wyświetlania tekstu w komórce określonego stopki, użyj `e.Row.Cells(index).Text = value`, gdzie `Cells` indeksowanie rozpoczyna się od 0. Poniższy kod oblicza średnią cenę (całkowita cena podzielony przez liczbę produktów) i wyświetla je wraz z całkowitą liczbą jednostki w magazynie i jednostki w kolejności w komórkach odpowiednie stopki GridView.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample8.vb)]

Rysunek 13 zawiera raport po dodaniu tego kodu. Uwaga jak `ToString("c")` powoduje, że informacje podsumowujące średniej ceny być sformatowana jak waluty.


[![Kolor tła czerwonawego ma teraz wiersz stopki w widoku GridView](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image37.png)

**Rysunek 13**: wiersz stopki GridView ma teraz czerwonawego kolor tła ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image39.png))


## <a name="summary"></a>Podsumowanie

Wyświetlanie podsumowania danych jest typowym wymaganiem raportu, a kontrolki widoku siatki ułatwia obejmują takie informacje w jego wierszu stopki. Wiersz stopka jest wyświetlany po w widoku GridView `ShowFooter` właściwość jest ustawiona na `True` i może zawierać tekstu w komórkach ustawić programowo za pomocą `RowDataBound` obsługi zdarzeń. Przetwarzanie danych podsumowania albo można wykonać ponowne wykonanie zapytania bazy danych lub przy użyciu kodu w klasie związanej z kodem strony ASP.NET, można programowo obliczyć danych podsumowania.

W tym samouczku kończy się naszego badanie niestandardowe formatowanie kontrolki GridView widoku DetailsView i FormView. Dalej z naszym samouczkiem dotyczącego naszych badań Wstawianie, aktualizowanie i usuwanie danych przy użyciu tych takich samych kontroli.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Poprzednie](using-the-formview-s-templates-vb.md)
