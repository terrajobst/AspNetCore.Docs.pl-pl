---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: "Wyświetlanie danych z DataList i kontrolki elementu powtarzanego (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W poprzednim samouczki użyliśmy kontrolki widoku siatki do wyświetlania danych. Począwszy od tego samouczka przyjrzymy się tworzenie typowe wzorce raportowania z..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 1f626731e79d83785057498c53cdf49aecb90261
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Wyświetlanie danych z DataList i kontrolki elementu powtarzanego (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) lub [pobierania plików PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> W poprzednim samouczki użyliśmy kontrolki widoku siatki do wyświetlania danych. Począwszy od tego samouczka przyjrzymy się tworzenie typowe wzorce raportowania DataList i powtarzanego kontroli, począwszy od podstawy wyświetlanie danych z tych kontrolek.


## <a name="introduction"></a>Wprowadzenie

We wszystkich przykładów w ciągu ostatnich 28 samouczki, jeśli firma Microsoft niezbędne do wyświetlania wielu rekordów ze źródłem danych, firma Microsoft z kontrolki widoku siatki. Widoku GridView renderuje wiersz dla każdego rekordu w źródle danych, wyświetlania pól danych rekordu s w kolumnach. Widoku GridView ułatwia przyciąganie do wyświetlania, strona za pomocą sortowania, edytowania i usuwania danych, jej wygląd jest nieco boxy. Ponadto odpowiedzialny znaczników struktury s widoku GridView jest stała go zawiera HTML `<table>` z wiersza tabeli (`<tr>`) dla każdego rekordu i komórki tabeli (`<td>`) dla każdego pola.

Aby zapewnić wysoką dostosowanie wyglądu i renderowanego kodu znaczników przy wyświetlaniu wielu rekordów, ASP.NET 2.0 oferuje formant DataList i powtarzanego (które były również dostępne w wersji platformy ASP.NET 1.x). Formant DataList i powtarzanego renderowania ich zawartość przy użyciu szablonów zamiast BoundFields CheckBoxFields, ButtonFields i tak dalej. Podobnie jak GridView elementu DataList renderuje jako kodu HTML `<table>`, ale zezwala na wiele danych źródła rekordów mają być wyświetlane w wierszach tabeli. Powtarzanego, renderuje z drugiej strony, nie dodatkowe znaczników niż co należy jawnie określić, a jest idealny kandydujących, gdy konieczne ścisła kontrola nad znaczników wysyłanego.

Za pośrednictwem samouczków obok dozen lub tak przyjrzymy tworzenia typowe wzorce raportowania DataList i powtarzanego kontroli, począwszy od podstawy wyświetlanie danych z tych szablonów kontrolki. Zajmiemy się tym sposób formatowania tych kontrolek, jak zmienić układ rekordów źródła danych w DataList, typowe scenariusze głównych/szczegółów, sposoby do edytowania i usuwania danych, jak przeglądanie rekordów i tak dalej.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Krok 1: Dodawanie DataList i powtarzanego Samouczek stron sieci Web

Przed Rozpoczniemy w tym samouczku umożliwiają s najpierw Poświęć chwilę, aby dodać stron ASP.NET, które będą potrzebne dla tego samouczka i dalej kilka samouczki dotyczące wyświetlania danych przy użyciu DataList i elementu powtarzanego. Rozpocznij od utworzenia nowego folderu do projektu o nazwie `DataListRepeaterBasics`. Następnie dodaj następujące pięć stron ASP.NET w tym folderze każdego z nich jest skonfigurowany do używania strony wzorcowej `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Utwórz DataListRepeaterBasics Folder i dodać strony samouczek platformy ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Rysunek 1**: tworzenie `DataListRepeaterBasics` folderu i dodać strony samouczek platformy ASP.NET


Otwórz `Default.aspx` strony i przeciągnij `SectionLevelTutorialListing.ascx` kontrolki użytkownika z `UserControls` folderu na powierzchnię projektu. Ten formant użytkownika, który utworzono w [stron wzorcowych i nawigacji w witrynie](../introduction/master-pages-and-site-navigation-cs.md) samouczek, wylicza mapy witryny i wyświetla samouczków, z sekcji bieżącej listy punktowanej.


[![Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` formantu użytkownika `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))


W celu wyświetlania listy punktowanej DataList i powtarzanego samouczki, firma Microsoft będzie tworzony, należy dodać je do mapy witryny. Otwórz `Web.sitemap` i Dodaj następujący kod po znaczników węzeł mapy witryny dodawanie przycisków niestandardowych:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![Zaktualizuj mapy witryny w celu uwzględnienia nowych stron ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Rysunek 3**: zaktualizuj mapy witryny w celu uwzględnienia nowych stron ASP.NET


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Krok 2: Wyświetlanie informacji o produkcie z elementu DataList

Podobnie jak w widoku FormView, formant DataList s renderowania danych wyjściowych zależy od szablonów zamiast BoundFields, CheckBoxFields i tak dalej. W odróżnieniu od FormView elementu DataList jest przeznaczony do wyświetlania zestawu rekordów, a nie solitary jeden. Let s rozpocząć tego samouczka z przyjrzeć powiązanie informacji o produkcie do elementu DataList. Uruchamianie przez otwarcie `Basics.aspx` strony `DataListRepeaterBasics` folderu. Następnie przeciągnij DataList z przybornika do projektanta. Jak pokazano na rysunku 4, przed określeniem szablony DataList s projektanta wyświetla go jako szare pole.


[![Przeciągnij z przybornika do konstruktora elementu DataList](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Rysunek 4**: przeciągnij DataList z przybornika do projektanta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))


Tagów inteligentnych z DataList s, Dodaj nowy element ObjectDataSource i skonfigurować go do używania `ProductsBLL` klasy s `GetProducts` metody. Możemy re tworzenie DataList tylko do odczytu w tym samouczku ustawiony listy rozwijanej (Brak) w Kreatorze s WSTAWIANIA, AKTUALIZOWANIA i usuwania karty.


[![OPT, aby utworzyć nowy element ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Rysunek 5**: zdecydować się na tworzenie nowego elementu ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))


[![Skonfiguruj ObjectDataSource do użycia klasy ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Rysunek 6**: Konfigurowanie ObjectDataSource użyć `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))


[![Pobierz informacje o wszystkich produktów przy użyciu metody GetProducts](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Rysunek 7**: pobieranie informacji o wszystkich używania produktów `GetProducts` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))


Po skonfigurowaniu ObjectDataSource i kojarzenie go z DataList za pośrednictwem jego tagów inteligentnych, Visual Studio automatycznie utworzy `ItemTemplate` w DataList, który wyświetla nazwę i wartość każdego pola danych zwróconych przez źródło danych (zobacz Kod znaczników poniżej). To ustawienie domyślne `ItemTemplate` wygląd s jest identyczna jak szablony tworzone automatycznie podczas tworzenia wiązania źródła danych FormView za pomocą projektanta.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> Odwołaj podczas wiązania źródła danych w formancie FormView za pomocą tagów inteligentnych s FormView, Visual Studio utworzony `ItemTemplate`, `InsertItemTemplate`, i `EditItemTemplate`. Z DataList, jednak tylko `ItemTemplate` jest tworzony. Jest to spowodowane elementu DataList nie ma tego samego wbudowane edycji i wstawianie pomocy technicznej oferowanej przez FormView. Elementu DataList zawiera zdarzenia dotyczące edytowania i usuwania i edytowanie i usuwanie pomocy technicznej można dodać z bitowego kodu, ale brak s nie poza pole prosta jako z FormView. Firma Microsoft wyświetlony sposobu uwzględniania, edytowanie i usuwanie obsługi z elementu DataList w przyszłości samouczka.


Let s Poświęć chwilę, aby poprawić wygląd tego szablonu. Wszystkie pola danych zamiast umożliwiają s Wyświetl tylko nazwy s produktu, dostawca, kategorii, ilość na jednostkę oraz cenie jednostkowej. Ponadto s umożliwiają wyświetlanie nazwy w `<h4>` nagłówka i układ pozostałe pola przy użyciu `<table>` pod nagłówkiem.

Aby wprowadzić te zmiany, które możesz albo szablon funkcji w Projektancie z DataList s inteligentne tag, kliknij łącze Edytuj szablony, lub możesz zmodyfikować szablon ręcznie za pomocą składni deklaratywnej s strony edycji. Jeśli używasz opcji Edytuj szablony w projektancie, Twoje znaczników wynikowy może nie odpowiadać następujący kod znaczników dokładnie, ale podczas wyświetlania za pośrednictwem przeglądarki powinien wyglądają bardzo podobnie do ekranu zrzut pokazano na rysunku 8.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> Przykład powyżej używa kontrolki sieci Web etykiety którego `Text` właściwości jest przypisywana wartość składni wiązania z danymi. Alternatywnie firma Microsoft może mieć pominięcia etykiet, wpisanie składnię wiązania z danymi. Oznacza to, że zamiast `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` można zamiast tego użyliśmy składni deklaratywnej `<%# Eval("CategoryName") %>`.


Jednak pozostawienie w formantach sieci Web etykiety, oferuje dwie zalety. Najpierw zapewnia oznacza łatwiejsze do formatowania danych na podstawie danych, ponieważ zajmiemy się w następnym samouczku. Po drugie opcja Edytuj szablony w Projektancie t wyświetlania deklaratywne wiązania z danymi składni wyświetlonym poza kontrolą niektórych sieci Web. Zamiast tego interfejsu Edytuj szablony zaprojektowano w celu ułatwienia pracy z znaczników statyczne i sieci Web formanty i przyjęto założenie, że wszystkie wiązania z danymi zostanie wykonane za pomocą okna dialogowego Edycja powiązania danych, która jest dostępna z tagami inteligentnymi formantów sieci Web.

W związku z tym podczas pracy z DataList, które udostępnia opcję edycję szablonów przy użyciu narzędzia Projektant, chcę użyć formantów etykiet w sieci Web tak, aby zawartość jest dostępna za pośrednictwem interfejsu Edytuj szablony. Zajmiemy się wkrótce, powtarzanego wymaga edytować zawartość szablonu s z widoku źródła. W związku z tym gdy obsługuje tworzenie szablonów s elementu powtarzanego I będzie często Pomiń Web etykiety kontrolki, jeśli wiadomo, że będzie należy sformatować wygląd danych powiązane tekstu opartych na logiki.


[![Każdy produkt wyjściowy s jest renderowany przy użyciu DataList ItemTemplate s](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Rysunek 8**: s każdy produkt wyjściowy jest renderowany przy użyciu DataList s `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Krok 3: Zwiększanie wygląd elementu DataList

Jak GridView, elementu DataList oferuje kilka właściwości związanych z stylu, takich jak `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`i tak dalej. Podczas pracy z kontrolki GridView i widoku DetailsView, utworzyliśmy plikach skórki w `DataWebControls` motywu, który wstępnie zdefiniowany `CssClass` właściwości dla tych dwóch formantów i `CssClass` właściwości kilka ich właściwości (`RowStyle` `HeaderStyle`i tak dalej). Let s wykonaj te same czynności dla elementu DataList.

Zgodnie z opisem w [wyświetlanie danych z ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) pliku skórki określa domyślną właściwości powiązane z wyglądem formantu sieci Web; motyw to kolekcja plików karnacji, obrazu, CSS i JavaScript, które definiują samouczka określonego wyglądu i działania witryny sieci Web. W *wyświetlanie danych z ObjectDataSource* samouczek, utworzyliśmy `DataWebControls` motywu (który jest zaimplementowany jako folder w `App_Themes` folder) mający, obecnie dwóch plikach skórki - `GridView.skin` i `DetailsView.skin`. Let s dodać innego pliku skórki, aby określić ustawienia wstępnie zdefiniowany styl elementu DataList.

Aby dodać plik skórki, kliknij prawym przyciskiem myszy `App_Themes/DataWebControls` folderu, wybierz polecenie Dodaj nowy element i wybierz z listy opcję pliku skórki. Nadaj nazwę plikowi `DataList.skin`.


[![Utwórz nowy plik skórki o nazwie DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Rysunek 9**: Tworzenie nowego pliku skórki, o nazwie `DataList.skin` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))


Użyj następującego kodu znaczników dla `DataList.skin` pliku:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Te ustawienia przypisywać tej samej klasy CSS do odpowiedniej właściwości DataList były używane z kontrolki GridView i widoku DetailsView. Klasy CSS używany tutaj `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`i tak dalej są zdefiniowane w `Styles.css` plików i zostały dodane w poprzednim samouczki.

Dodając ten plik skórki wygląd s DataList jest aktualizowana w Projektancie (może być konieczne Odśwież widok projektanta, aby zobaczyć efekty nowy plik skórki; z menu Widok, kliknij przycisk Odśwież). Jak pokazano na rysunku nr 10, każdy z produktów przemiennych ma jasny różowym kolor tła.


[![Utwórz nowy plik skórki o nazwie DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Na rysunku nr 10**: Tworzenie nowego pliku skórki, o nazwie `DataList.skin` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Krok 4: Eksploracji DataList s innych szablonów

Oprócz `ItemTemplate`, DataList obsługuje sześciu szablony opcjonalne:

- `HeaderTemplate`Jeśli zostanie podana, dodaje wiersz nagłówka do danych wyjściowych i jest używany do renderowania tego wiersza
- `AlternatingItemTemplate`używany do renderowania elementów przemiennych
- `SelectedItemTemplate`używany do renderowania wybranego elementu; wybrany element to element, którego indeks odpowiada DataList s [ `SelectedIndex` właściwości](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate`używany do renderowania elementu edytowany
- `SeparatorTemplate`Jeśli zostanie podana, dodaje separatora między każdym z elementów i jest używany do renderowania tego separatora
- `FooterTemplate`— w przypadku dodaje wiersz stopki dane wyjściowe i jest używany do renderowania tego wiersza

Podczas określania `HeaderTemplate` lub `FooterTemplate`, DataList dodaje dodatkowy wiersz nagłówka lub stopki do przetworzonych wyników. Podobnie jak z widoku GridView s nagłówku i stopce wierszy, nagłówku i stopce w DataList nie są powiązane z danych. W związku z tym sformatowanej składni wiązania z danymi w `HeaderTemplate` lub `FooterTemplate` próbuje uzyskać dostęp do danych związane zwraca pusty ciąg.

> [!NOTE]
> Jak widzieliśmy w [wyświetlanie informacji podsumowania w widoku GridView s stopki](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) samouczek, gdy ADAM wierszy w nagłówku i stopce składnia wiązania z danymi t pomocy technicznej, informacje specyficzne dla danych mogą zostać dodane bezpośrednio do tych wierszy z Element GridView s `RowDataBound` program obsługi zdarzeń. Ta technika może być używana do obu uruchomionych obliczeń lub innych informacji z dane powiązane z formantem a także przypisać te informacje do stopki. Tego samego pojęcia można zastosować do kontroli DataList i powtarzanego; Jedyną różnicą jest to, że na DataList i powtarzanego utworzyć programu obsługi zdarzeń dla `ItemDataBound` zdarzenia (a nie dla `RowDataBound` zdarzeń).


W naszym przykładzie umożliwiają s mają tytuł informacji o produkcie wyświetlany u góry s DataList skutkuje `<h3>` nagłówka. W tym celu należy dodać `HeaderTemplate` z odpowiednią znaczników. Przy użyciu projektanta, można to osiągnąć klikając łącze Edytuj szablony w tagu DataList s, wybieranie szablonów nagłówka z listy rozwijanej i wpisując w tekście po wybraniu opcji Nagłówek 3 z styl listy rozwijanej liście (patrz rysunek 11).


[![Dodaj właściwość HeaderTemplate z informacji o produkcie tekstu](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Rysunek 11**: Dodaj `HeaderTemplate` z informacji o produkcie tekst ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))


Alternatywnie, to można dodać deklaratywnie, wprowadzając następujące znaczników w `<asp:DataList>` tagów:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Aby dodać nieco miejsca między listę każdego produktu, let s dodać `SeparatorTemplate` zawierającą linii między każdej sekcji. Tag linia pozioma (`<hr>`), dodaje takie podziału. Utwórz `SeparatorTemplate` , aby miała następujący kod:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Podobnie jak `HeaderTemplate` i `FooterTemplates`, `SeparatorTemplate` nie jest powiązany ze źródła danych żadnych rekordów i dlatego nie może bezpośrednio dostęp do źródła danych, rekordy powiązany z elementu DataList.


Po wprowadzeniu to dodawanie, podczas wyświetlania strony za pośrednictwem przeglądarki powinien wyglądać podobnie do rysunku 12. Należy pamiętać, wiersz nagłówka i linii między listę każdego produktu.


[![Elementu DataList zawiera wiersz nagłówka i poziomą między listę każdego produktu](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Rysunek 12**: elementu DataList zawiera wiersz nagłówka i poziomy reguły między każdą listy produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Krok 5: Renderowanie oznaczenia o kontrolce elementu powtarzanego

Jeśli zrobisz widoku/źródła w przeglądarce podczas odwiedzania przykład DataList z rysunku 12, zobaczysz, że elementu DataList emituje HTML `<table>` zawiera wiersz tabeli (`<tr>`) z jedną komórkę (`<td>`) dla każdego elementu powiązany z DataList. Te dane wyjściowe w rzeczywistości jest taki sam jak co będzie emitowany z widoku GridView z jednym TemplateField. Jak zajmiemy się w przyszłości samouczek, elementu DataList można dostosowywać dalsze dane wyjściowe, co pozwala na wyświetlanie wielu rekordów źródła danych w wierszu tabeli.

Co zrobić, jeśli zrobisz chcesz t emitowanie kodu HTML `<table>`, ale? Dla całkowitej i pełną kontrolę nad znacznika generowany przez formant danych sieci Web możemy użyć kontrolce elementu powtarzanego. Podobnie jak DataList powtarzanego jest tworzony, oparte na szablonach. Powtarzanego, jednak tylko udostępnia następujących pięć szablonów:

- `HeaderTemplate`Jeśli zostanie podana, dodaje określony znaczników przed elementów
- `ItemTemplate`używany do renderowania elementów
- `AlternatingItemTemplate`Jeśli zostanie podana, używany do renderowania elementów przemiennych
- `SeparatorTemplate`Jeśli zostanie podana, dodaje określony znaczników między każdym z elementów
- `FooterTemplate`— w przypadku dodaje określonego znacznika po elementów

W programie ASP.NET 1.x, powtarzanego kontrolę został najczęściej używanych do wyświetlania listy punktowanej, którego dane pochodzą z określonego źródła danych. W takim przypadku `HeaderTemplate` i `FooterTemplates` zawierałoby otwarcia i zamknięcia `<ul>` tagów, odpowiednio, podczas gdy `ItemTemplate` zawierałoby `<li>` elementów ze składnią wiązania z danymi. Takie podejście nadal można zastosować w programie ASP.NET 2.0 widzieliśmy w dwóch przykładach w [stron wzorcowych i nawigacji w witrynie](../introduction/master-pages-and-site-navigation-cs.md) samouczek:

- W `Site.master` strony wzorcowej elementu powtarzanego był używany do wyświetlania listy punktowanej zawartości mapy witryny najwyższego poziomu (podstawowym raportowaniem, filtrowania raportów dostosowane formatowanie i tak dalej); inny, zagnieżdżonego elementu powtarzanego był używany do wyświetlania sekcji elementy podrzędne sekcje najwyższego poziomu
- W `SectionLevelTutorialListing.ascx`, powtarzanego był używany do wyświetlania listy punktowanej sekcji dzieci w bieżącej sekcji mapy witryny

> [!NOTE]
> ASP.NET 2.0 wprowadzono nowe [formantu listy BulletedList](https://msdn.microsoft.com/en-us/library/ms228101.aspx), która może być powiązana z kontroli źródła danych w celu wyświetlenia listy punktowanej proste. Za pomocą formantu listy BulletedList nie należy określić dowolne HTML związanych z listy; Zamiast tego możemy wskazują po prostu pola danych, który będzie wyświetlany jako tekst dla każdego elementu listy.


Powtarzanego służy jako instrukcji catch wszystkich danych formantu sieci Web. Jeśli nie jest formant generujący znaczników potrzebne, można w kontrolce elementu powtarzanego. Aby zilustrować, przy użyciu powtarzanego, umożliwiają s listy Kategorie wyświetlane powyżej elementu DataList informacji produktu, które zostały utworzone w kroku 2. W szczególności let s ma kategorie wyświetlane w HTML pojedynczy wiersz `<table>` z każdej kategorii wyświetlane jako kolumny w tabeli.

W tym celu uruchom przeciągając je z przybornika do projektanta, powyżej DataList informacji produktu kontrolce elementu powtarzanego. Podobnie jak w przypadku DataList powtarzanego początkowo wyświetlane jako szare pole aż do jego szablony zostały zdefiniowane.


[![Dodawanie elementu powtarzanego do projektanta](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Rysunek 13**: Dodawanie elementu powtarzanego do projektanta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))


Dostępne są s tylko jedną opcję w tagu elementu powtarzanego s: Wybierz źródło danych. OPT, aby utworzyć nowy element ObjectDataSource i skonfigurować go do użycia `CategoriesBLL` klasy s `GetCategories` metody.


[![Utwórz nowy element ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Rysunek 14**: Utwórz nowy element ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))


[![Skonfiguruj ObjectDataSource do użycia klasy CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Rysunek 15**: Konfigurowanie ObjectDataSource użyć `CategoriesBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))


[![Pobierz informacje o wszystkich kategorii przy użyciu metody GetCategories](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**Rysunek 16**: pobieranie informacji o wszystkich użycia kategorii `GetCategories` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))


W odróżnieniu od DataList Visual Studio nie tworzy automatycznie ItemTemplate dla powtarzanego po jego powiązanie ze źródłem danych. Ponadto szablony s elementu powtarzanego nie można skonfigurować za pomocą projektanta i musi być określona deklaratywnie.

Aby wyświetlić kategorie jako pojedynczy wiersz `<table>` z kolumną w każdej kategorii, potrzebujemy powtarzanego można wyemitować kodu znaczników podobny do następującego:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Ponieważ `<td>Category X</td>` tekst jest powtarzany, będzie ona widoczna w elemencie powtarzanym s ItemTemplate. Kod znaczników, który pojawi się przed nim - `<table><tr>` -zostaną umieszczone w `HeaderTemplate` podczas końcowego znacznika - `</tr></table>` — zostanie umieszczona w `FooterTemplate`. Aby wprowadzić te ustawienia szablonu, przejdź do deklaratywne części strony ASP.NET, klikając przycisk źródła w lewym dolnym rogu i typu w następującej składni:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Powtarzanego emituje dokładne znaczników określony przez jego szablony, nic więcej, nic nie mniejsza. Rysunek 17 przedstawiono dane s elementu powtarzanego widzianego za pośrednictwem przeglądarki.


[![HTML pojedynczy wiersz &lt;tabeli&gt; zawiera listę kategorii każdy w oddzielnej kolumnie](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Rysunek 17**: A pojedynczy wiersz HTML `<table>` zawiera listę kategorii każdy w oddzielnej kolumnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Krok 6: Zwiększanie wygląd elementu powtarzanego

Ponieważ powtarzanego emituje dokładnie kod znaczników, określony przez jego szablony, go jako niespodziewanego nie powinna pochodzić, że nie ma żadnych powiązanych styl właściwości dla elementu powtarzanego. Aby zmienić wygląd zawartości generowanej przez powtarzanego, możemy ręcznie dodać bezpośrednio do szablonów elementu powtarzanego s potrzebnej zawartości HTML i CSS.

W naszym przykładzie umożliwiają s kolumnach kategorii alternatywny kolory tła, takich jak z przemiennych wierszy w elementu DataList. Aby to zrobić, należy przypisać `RowStyle` klasy CSS do każdego elementu powtarzanego i `AlternatingRowStyle` klasy CSS do każdego przemiennych elementu powtarzanego za pośrednictwem `ItemTemplate` i `AlternatingItemTemplate` szablony, w następujący sposób:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Umożliwiają również dodać wiersz nagłówka do danych wyjściowych z tekstem kategorie produktów s. Ponieważ firma Microsoft ADAM wiem, liczbę kolumn naszych co `<table>` znajdą się Najprostszym sposobem, aby wygenerować nagłówek, który może obejmować wszystkie kolumny, jest użycie *dwóch* `<table>` s. Pierwszy `<table>` będzie zawierać dwa wiersze, wiersz nagłówka i wiersza, który będzie zawierać drugiego, pojedynczy wiersz `<table>` z kolumny, dla każdej kategorii w systemie. Oznacza to chcemy Dodaj następujący kod:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

Następujące `HeaderTemplate` i `FooterTemplate` spowodować żądaną znaczników:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

18 rysunek przedstawia powtarzanego po dokonaniu zmiany.


[![Kolumny kategorii alternatywny w kolor tła i zawiera wiersz nagłówka](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**Rysunek 18**: alternatywne kolumny kategorii koloru tła i obejmuje wiersz nagłówka ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))


## <a name="summary"></a>Podsumowanie

Gdy formant widoku GridView ułatwia wyświetlanie, edycji, usuwania, sortowania i przeglądanie danych, wygląd jest bardzo boxy i siatki. Aby uzyskać większą kontrolę nad wyglądem musimy włączyć do formantów DataList albo elementu powtarzanego. Oba formanty wyświetlania zestawu rekordów przy użyciu szablonów zamiast BoundFields, CheckBoxFields i tak dalej.

Renderuje elementu DataList jako kodu HTML `<table>` , domyślnie wyświetla każdy rekord źródła danych w wierszu pojedynczej tabeli, podobnie jak w widoku GridView z jednym TemplateField. Jak zostanie wyświetlone w przyszłości samouczek, jednak elementu DataList zezwala na wiele rekordów mają być wyświetlane w wierszach tabeli. Powtarzanego, z drugiej strony, ściśle emituje znaczników określony w jej szablonów; nie dodaje żadnych dodatkowych znaczników i dlatego jest najczęściej używany do wyświetlania danych w elementów HTML innych niż `<table>` (takie jak listy punktowanej).

Gdy DataList i powtarzanego oferują większą elastyczność podczas ich przetworzonych wyników, mają wiele wbudowanych funkcji w widoku GridView. Jak zajmiemy się w kolejnych samouczkach, niektóre z tych funkcji można podłączyć ponownie bez zbyt wiele wysiłku, ale należy pamiętać, że korzystać elementu DataList lub elementu powtarzanego zamiast widoku GridView ograniczyć funkcje, których można używać bez konieczności implementowania tych funkcji samodzielnie.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Yaakov Ellis, Liz Shulok Randy Schmidt i wstrzymywanie Stacy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Dalej](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
