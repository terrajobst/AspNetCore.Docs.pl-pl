---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
title: Wyświetlanie danych za pomocą kontrolek DataList i Repeater (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W poprzednich samouczkach użyliśmy kontrolki GridView do wyświetlania danych. Począwszy od w tym samouczku przyjrzymy się tworzenia typowych wzorców raportowania za pomocą...
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: 58618954-a9ed-4ca0-8c2d-95a5ffd9c03e
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 2a6674036500d766e48b33b58f7d7829ea1da28c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840335"
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-vb"></a>Wyświetlanie danych za pomocą kontrolek DataList i Repeater (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_VB.exe) lub [Pobierz plik PDF](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/datatutorial29vb1.pdf)

> W poprzednich samouczkach użyliśmy kontrolki GridView do wyświetlania danych. Począwszy od w tym samouczku przyjrzymy się tworzenia typowych wzorców raportowania za pomocą kontrolek DataList i Repeater, począwszy od podstawy wyświetlanie danych za pomocą tych kontrolek.


## <a name="introduction"></a>Wprowadzenie

We wszystkich przykładach w całej ostatnich 28 samouczki, jeśli potrzebowaliśmy wyświetlić wiele rekordów ze źródła danych, firma Microsoft przekształcane w kontrolce GridView. Widoku GridView renderuje wiersz dla każdego rekordu w źródle danych, wyświetlanie pól rekordu s danych w kolumnach. Chociaż widoku GridView ułatwia staje się wyświetlanie, przeglądanie, sortowanie, edytowania i usuwania danych, jego wygląd jest nieco boxy. Ponadto odpowiada znaczniki struktury s GridView to naprawić zawiera kodu HTML `<table>` przy użyciu wiersza tabeli (`<tr>`) dla każdego wybranego rekordu i komórki tabeli (`<td>`) dla każdego pola.

Zapewnienie większego stopnia dostosowania wyglądu i renderowanego kodu znaczników przy wyświetlaniu wiele rekordów, ASP.NET 2.0 oferuje kontrolek DataList i Repeater (które były również dostępne w wersji platformy ASP.NET 1.x). Kontrolki DataList i Repeater renderowania ich zawartości przy użyciu szablonów, a nie BoundFields CheckBoxFields, ButtonFields i tak dalej. Podobnie jak GridView kontrolki DataList renderowany jako kodu HTML `<table>`, ale pozwala na danych wielu rekordów źródła mają być wyświetlane każdego wiersza tabeli. Powtarzanego, z drugiej strony, renderuje żadnych dodatkowych znaczników niż co należy jawnie określić i jest idealnym rozwiązaniem Release candidate, gdy będziesz potrzebować precyzyjną kontrolę nad znacznikami emitowane.

Za pośrednictwem obok kilkudziesięciu lub tak samouczkach przyjrzymy tworzenia typowych wzorców raportowania za pomocą kontrolek DataList i Repeater, począwszy od podstawy wyświetlanie danych za pomocą tych szablonów kontrolek. Zobaczymy, jak sformatować tych kontrolek, jak zmienić układ rekordów źródła danych w kontrolkach DataList, typowe scenariusze wzorzec/szczegół, sposobów edytowania i usuwania danych, jak do przeglądania rekordów i tak dalej.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Krok 1: Dodawanie kontrolek DataList i Repeater Samouczek stron sieci Web

Zanim zaczniemy, w tym samouczku, umożliwiają najpierw Poświęć chwilę, aby dodać strony ASP.NET, które będą potrzebne w tym samouczku i dalej kilka samouczków, rozwiązywania problemów związanych z wyświetlaniem danych za pomocą kontrolek DataList i Repeater s. Rozpocznij od utworzenia nowego folderu w projekcie o nazwie `DataListRepeaterBasics`. Następnie dodaj następujące pięć strony ASP.NET, w tym folderze każdego z nich jest skonfigurowany do używania strony wzorcowej `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Utwórz DataListRepeaterBasics Folder i dodawanie stron samouczek platformy ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image1.png)

**Rysunek 1**: tworzenie `DataListRepeaterBasics` Folder i dodawanie stron samouczek platformy ASP.NET


Otwórz `Default.aspx` strony, a następnie przeciągnij `SectionLevelTutorialListing.ascx` kontrolki użytkownika od `UserControls` folder na powierzchnię projektu. Ten formant użytkownika, które utworzyliśmy w [strony wzorcowe i nawigacja w witrynie](../introduction/master-pages-and-site-navigation-vb.md) samouczek, wylicza mapy witryny i wyświetla samouczków z bieżącej sekcji na liście punktowanej.


[![Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image2.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image4.png))


W celu wyświetlania listy punktowanej DataList i Repeater samouczków, firma Microsoft będzie tworzony, należy dodać je do mapy witryny. Otwórz `Web.sitemap` pliku i Dodaj następujący kod po znaczników węzeł mapy witryny dodawania przycisków niestandardowych:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample1.xml)]


![Aktualizacja mapy witryny, aby uwzględnić nowe strony ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image5.png)

**Rysunek 3**: Aktualizacja mapy witryny, aby uwzględnić nowe strony ASP.NET


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Krok 2: Wyświetlanie informacji o produkcie za pomocą kontrolki DataList

Podobnie jak w widoku FormView, kontrolki DataList s renderowania danych wyjściowych zależy od szablonów zamiast BoundFields, CheckBoxFields i tak dalej. W odróżnieniu od FormView kontrolki DataList jest przeznaczony do wyświetlania zestawu rekordów zamiast solitary jeden. Pozwól s rozpocząć w tym samouczku przy użyciu przyjrzeć się powiązanie — informacje o produkcie z kontrolką DataList. Zacznij od otwarcia `Basics.aspx` stronie `DataListRepeaterBasics` folderu. Następnie przeciągnij kontrolką DataList z przybornika do projektanta. Jak rysunek 4 przedstawia przed określeniem szablonów DataList s, Projektant wyświetla je jako szary prostokąt.


[![Przeciągnij kontrolki DataList z przybornika do projektanta](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image6.png)

**Rysunek 4**: przeciągnij DataList z przybornika do projektanta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image8.png))


Z DataList s tagów inteligentnych, dodać nowe kontrolki ObjectDataSource i skonfigurować go do używania `ProductsBLL` klasy s `GetProducts` metody. Ponieważ ponowne tworzenie DataList tylko do odczytu, w tym samouczku ustawimy listy rozwijanej (Brak) w Kreatorze s Wstawianie, AKTUALIZOWANIE i usuwanie kart.


[![Wybrać opcję utworzenia nowego elementu ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image9.png)

**Rysunek 5**: zoptymalizowany pod kątem do utworzenia nowego elementu ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image11.png))


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image12.png)

**Rysunek 6**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image14.png))


[![Pobierz informacje o wszystkich produktów za pomocą metody GetProducts](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image15.png)

**Rysunek 7**: pobieranie informacji o wszystkich produktów za pomocą `GetProducts` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image17.png))


Po skonfigurowaniu kontrolki ObjectDataSource i kojarząc ją z DataList za pośrednictwem jego tagu inteligentnego, Visual Studio będzie automatycznie tworzył `ItemTemplate` w elemencie DataList, który wyświetla nazwę i wartość każdego pola danych zwróconych przez źródło danych (zobacz Kod znaczników poniżej). To ustawienie domyślne `ItemTemplate` wygląd jest taka sama jak w przypadku szablonów tworzone automatycznie podczas tworzenia wiązania źródła danych do FormView za pomocą projektanta.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample2.aspx)]

> [!NOTE]
> Pamiętaj, że powiązań źródła danych w kontrolce FormView za pośrednictwem tagu inteligentnego s FormView, Visual Studio stworzył `ItemTemplate`, `InsertItemTemplate`, i `EditItemTemplate`. Za pomocą kontrolek DataList, jednak tylko `ItemTemplate` zostanie utworzony. Jest to spowodowane kontrolki DataList nie ma tych samych wbudowanych, edycji i wstawiania obsługę oferowaną przez FormView. Kontrolki DataList zawierają informacje o zdarzeniach powiązanych edycji i usuwania i edytowania i usuwania pomocy technicznej można dodać z znacznej liczby kod, ale występują s nie out-of--box prosta jako przy użyciu widoku FormView. Zobaczymy, jak dołączyć, edytowania i usuwania pomocy technicznej za pomocą kontrolki DataList w przyszłości zapoznać się z samouczkiem.


Pozwól, s, Poświęć chwilę, aby poprawić wygląd tego szablonu. Zamiast wyświetlić wszystkie pola danych, niech s Wyświetl tylko nazwy s produktu, dostawca, kategorii, ilość na jednostkę oraz ceny jednostkowej. Ponadto umożliwiają s wyświetlania nazwy w `<h4>` nagłówka oraz pozostałe pola, przy użyciu układu `<table>` pod nagłówkiem.

Aby wprowadzić te zmiany, możesz albo szablon funkcji w Projektancie z DataList s inteligentnych tagów kliknij link Edytuj szablony lub można zmodyfikować szablon ręcznie za pomocą składni deklaratywnej s strona edycji. Jeśli używasz opcji Edytuj szablony w Projektancie znaczników wynikowy może nie odpowiadać następującym dokładnie, ale oglądany przez przeglądarkę powinna wyglądać bardzo podobne do ekranu zrzut, jak pokazano na rysunku 8.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample3.aspx)]

> [!NOTE]
> Określa etykietę w sieci Web przykładzie powyżej używa, którego `Text` właściwość jest przypisywana wartość składni wiązania danych. Alternatywnie można pominięto etykiet, wpisując składnię wiązania danych. Oznacza to, że zamiast `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` mogliśmy zamiast tego użyć składni deklaratywnej `<%# Eval("CategoryName") %>`.


Jednak pozostawienie w kontrolkach internetowych etykiety, oferuje dwie korzyści. Po pierwsze zapewnia łatwiejsze oznacza, że formatowanie danych na podstawie danych, jak opisano w następnym samouczku. Po drugie opcja Edytuj szablony w Projektancie t wyświetlania deklaratywne wiązania danych składni, pojawia się poza niektóre kontrolki sieci Web. Zamiast tego interfejs Edytuj szablony zaprojektowano w celu ułatwienia pracy z znaczników statycznych i sieci Web kontroluje i przyjęto założenie, że wszystkie wiązania danych zostanie wykonane za pomocą okna dialogowego Edycja powiązania danych, który jest dostępny z tagów inteligentnych formantów sieci Web.

W związku z tym podczas pracy z DataList, który zapewnia możliwość edytowania szablonów za pomocą projektanta, chcę użyć kontrolki etykiety w sieci Web, więc, że zawartość jest dostępna za pośrednictwem interfejsu Edytuj szablony. Jak zajmiemy się wkrótce, powtarzanego wymaga, że zawartość s szablonu można edytować z poziomu widoku źródła. W związku z tym podczas tworzenia szablonów s Repeater I będzie często pominąć etykiety w sieci Web kontroluje, o ile nie wiem, że potrzebuję sformatować wygląd danych powiązane tekst oparty na logiki.


[![Każdy produkt s danych wyjściowych jest renderowany przy użyciu DataList s właściwości ItemTemplate](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image18.png)

**Rysunek 8**: s każdy produkt wyjściowy jest renderowany przy użyciu DataList s `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Krok 3: Poprawa wyglądu kontrolki DataList

Podobnie jak GridView, kontrolki DataList udostępnia szereg właściwości stylu związane, takie jak `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`i tak dalej. Podczas pracy z kontrolkami GridView i DetailsView, utworzyliśmy pliki Skin w `DataWebControls` motyw, który wstępnie zdefiniowane `CssClass` właściwości dla tych dwóch kontrolek i `CssClass` właściwości dla kilku ich właściwości (`RowStyle` `HeaderStyle`i tak dalej). Pozwól, s, w tym samym elemencie DataList.

Zgodnie z opisem w [wyświetlanie danych za pomocą kontrolki ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) samouczka plik Skin Określa domyślne powiązane z wyglądem właściwości kontrolki sieci Web; motyw to zbiór skórki, obrazów, plików CSS i JavaScript, które definiują określonego wygląd i działanie dla witryny sieci Web. W *wyświetlanie danych za pomocą kontrolki ObjectDataSource* samouczku utworzyliśmy `DataWebControls` motyw (który jest implementowany jako folder w `App_Themes` folder) zawierający, obecnie dwa pliki Skin - `GridView.skin` i `DetailsView.skin`. Pozwól s dodać trzecią plik Skin, aby określić ustawienia wstępnie zdefiniowany styl kontrolki DataList.

Aby dodać plik Skin, kliknij prawym przyciskiem myszy `App_Themes/DataWebControls` folder, kliknij przycisk Dodaj nowy element i wybierz opcję Plik Skin z listy. Nadaj plikowi nazwę `DataList.skin`.


[![Utwórz nowy plik Skin o nazwie DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image21.png)

**Rysunek 9**: Utwórz nowy plik Skin, o nazwie `DataList.skin` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image23.png))


Użyj następujące kodu znaczników dla `DataList.skin` pliku:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample4.aspx)]

Te ustawienia przypisywać tej samej klasy CSS do odpowiedniej właściwości DataList zostały użyte z kontrolkami GridView i DetailsView. Klasy CSS, w tym miejscu użyć `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`i tak dalej są zdefiniowane w `Styles.css` plików i zostały dodane w poprzednich samouczkach.

Dodając ten plik Skin wygląd s DataList jest aktualizowana w projektanta (może być konieczne odświeżenie widoku projektanta, aby zobaczyć skutki nowy plik Skin; z menu Widok, wybierz opcję odświeżania). Jak pokazano na rysunku nr 10, każdy produkt przemienne ma jasny różowy kolor tła.


[![Utwórz nowy plik Skin o nazwie DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image24.png)

**Na rysunku nr 10**: Utwórz nowy plik Skin, o nazwie `DataList.skin` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Krok 4: Poznawanie DataList s innych szablonów

Oprócz `ItemTemplate`, kontrolki DataList obsługuje sześć opcjonalne szablony:

- `HeaderTemplate` Jeśli nie dostarczono, dodaje wiersz nagłówka w danych wyjściowych i używany do renderowania tego wiersza
- `AlternatingItemTemplate` używany do renderowania elementów przemienne
- `SelectedItemTemplate` używany do renderowania wybranego elementu; wybrany element jest element, którego indeks odpowiada DataList s [ `SelectedIndex` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` używany do renderowania elementu edytowany
- `SeparatorTemplate` Jeśli nie dostarczono, dodaje znak oddzielający poszczególne elementy i używany do renderowania tego separatora
- `FooterTemplate` — Jeśli nie dostarczono, dodaje wiersz stopki w danych wyjściowych, a używany do renderowania tego wiersza

Podczas określania `HeaderTemplate` lub `FooterTemplate`, kontrolki DataList dodaje dodatkowy wiersz nagłówka lub stopki do wyniku renderowania. Podobnie jak przy użyciu GridView s nagłówku i stopce wierszy, nagłówek i stopka w kontrolkach DataList nie są powiązane z danych. W związku z tym, wszelkie składnia wiązania danych w `HeaderTemplate` lub `FooterTemplate` próbuje uzyskać dostęp do powiązanych danych zwraca pusty ciąg.

> [!NOTE]
> Jak widzieliśmy w [wyświetlanie informacji podsumowania w kontrolce GridView s stopki](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) samouczek, podczas gdy don wierszy w nagłówku i stopce składni t do wiązania danych pomocy technicznej, informacje specyficzne dla danych może zostać wprowadzona bezpośrednio do tych wierszy z GridView s `RowDataBound` programu obsługi zdarzeń. Ta technika może być używany do obu uruchomionych obliczeń lub inne informacje z danych powiązane z formantem a także przypisać te informacje do stopki. Tę samą koncepcję można zastosować do kontrolek DataList i Repeater Jedyną różnicą jest to, że na kontrolkach DataList i Repeater utworzyć program obsługi zdarzeń dla `ItemDataBound` zdarzeń (a nie dla `RowDataBound` zdarzeń).


W tym przykładzie umożliwiają s ma tytuł wyświetlany u góry s DataList skutkuje informacje o produkcie `<h3>` nagłówka. Aby to zrobić, należy dodać `HeaderTemplate` za pomocą odpowiedniego znacznika. Przy użyciu projektanta, można to zrobić przez kliknięcie linku Edytuj szablony w tagu inteligentnego DataList s, wybierając szablon nagłówka z listy rozwijanej i wpisując tekst po wybraniu opcji Nagłówek 3 ze stylu listy rozwijanej liście (patrz rysunek 11).


[![Dodaj HeaderTemplate przy użyciu informacji o produkcie tekstu](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image27.png)

**Rysunek 11**: Dodaj `HeaderTemplate` przy użyciu informacji o produkcie tekstu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image29.png))


Alternatywnie, to można dodać deklaratywne, wprowadzając następujące znaczniki w ramach `<asp:DataList>` tagi:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample5.html)]

Aby dodać znacznej liczby odstęp między każdym lista produktów, umożliwić s Dodaj `SeparatorTemplate` zawierającej linię między każdej sekcji. Tag linia pozioma (`<hr>`), dodaje pasek podziału. Utwórz `SeparatorTemplate` tak, aby miało następujące znaczniki:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample6.html)]

> [!NOTE]
> Podobnie jak `HeaderTemplate` i `FooterTemplates`, `SeparatorTemplate` nie jest powiązany z dowolny rekord ze źródła danych i w związku z tym nie może mieć bezpośredniego dostępu do rekordów powiązany z kontrolki DataList źródła danych.


Po wprowadzeniu tego dodatku, podczas wyświetlania strony za pośrednictwem przeglądarki powinien wyglądać podobnie do rysunek 12. Należy pamiętać, wiersz nagłówka i wiersz między każdym lista produktów.


[![Kontrolki DataList zawiera wiersz nagłówka i linii poziomej między każdym lista produktów](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image30.png)

**Rysunek 12**: kontrolki DataList zawiera wiersz nagłówka i poziomy reguły między każdej listy produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Krok 5: Renderowanie oznaczenia z kontrolką elementu powtarzanego

Razie widok/źródło w przeglądarce podczas odwiedzania przykład DataList z rysunek 12 zobaczysz, że kontrolki DataList emituje HTML `<table>` zawierającą wiersz tabeli (`<tr>`) z jedną komórkę (`<td>`) dla każdego elementu powiązane z DataList. Te dane wyjściowe, w rzeczywistości jest taka sama, co mogłoby być emitowane z GridView za pomocą pojedynczego TemplateField. Jak zobaczymy w przyszłości zapoznać się z samouczkiem, kontrolki DataList umożliwia dalsze dostosowywanie danych wyjściowych, pozwala na wyświetlanie wielu rekordów źródła danych każdego wiersza tabeli.

Co zrobić, jeśli komputer ma t można wyemitować kodu HTML `<table>`, ale? Firma Microsoft łącznej liczby i pełną kontrolę nad znacznikami, generowane przez formant danych sieci Web, należy użyć w kontrolce elementu powtarzanego. Podobnie jak DataList powtarzanego jest tworzony na podstawie szablonów. Powtarzanego, jednak tylko oferuje pięć następujących szablonów:

- `HeaderTemplate` Jeśli nie dostarczono, dodaje określony znaczników przed elementami
- `ItemTemplate` używany do renderowania elementów
- `AlternatingItemTemplate` Jeśli nie dostarczono, używany do renderowania elementów przemienne
- `SeparatorTemplate` Jeśli nie dostarczono, dodaje określony znaczników, od każdego elementu
- `FooterTemplate` — Jeśli nie dostarczono, dodaje określony znaczników po elementach

W programie ASP.NET: 1.x i Repeater kontroli często był używany do wyświetlania listy punktowanej, którego dane pochodzą z określonego źródła danych. W takim przypadku `HeaderTemplate` i `FooterTemplates` zawierałoby otwierającym i zamykającym `<ul>` tagów, odpowiednio, podczas gdy `ItemTemplate` zawierałoby `<li>` elementów przy użyciu składni wiązania danych. Nadal można używać tej metody w programie ASP.NET 2.0 jako widzieliśmy w dwóch przykładach w [strony wzorcowe i nawigacja w witrynie](../introduction/master-pages-and-site-navigation-vb.md) samouczka:

- W `Site.master` strony wzorcowej Repeater był używany do wyświetlania listy punktowanej lokacji najwyższego poziomu zawartości map (podstawowe raportowanie, filtrowanie raportów, dostosować formatowanie i tak dalej); w innym, zagnieżdżonego elementu powtarzanego był używany do wyświetlania części elementów podrzędnych sekcje najwyższego poziomu
- W `SectionLevelTutorialListing.ascx`, Repeater był używany do wyświetlania listy punktowanej sekcje dzieci w bieżącej sekcji mapy witryny

> [!NOTE]
> ASP.NET 2.0 wprowadzono nowe [kontroli BulletedList](https://msdn.microsoft.com/library/ms228101.aspx), który może być powiązana z kontroli źródła danych w celu wyświetlenia prostej listy punktowanej. Za pomocą kontrolki BulletedList firma Microsoft nie trzeba określić dowolną HTML związane z listy; Zamiast tego po prostu wskazaliśmy pole danych, aby wyświetlić jako tekst dla każdego elementu listy.


Powtarzanego służy jako bloku catch, wszystkie dane formantu sieci Web. Jeśli nie ma istniejącej kontrolki, która generuje potrzebne znaczników, może służyć w kontrolce elementu powtarzanego. Aby zilustrować, za pomocą powtarzanego, niech s ma na liście kategorii wyświetlane powyżej kontrolki DataList informacji produktu, które zostały utworzone w kroku 2. W szczególności umożliwiają s ma kategorie wyświetlane w formacie HTML pojedynczy wiersz tabeli `<table>` z każdej kategorii wyświetlane jako kolumny w tabeli.

W tym celu należy uruchomić, przeciągając kontrolką elementu powtarzanego z przybornika w projektancie, powyżej DataList informacje o produkcie. Podobnie jak w przypadku DataList powtarzanego początkowo wyświetlane są jako szare pole do momentu jego szablony zostały zdefiniowane.


[![Dodaj Repeater do projektanta](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image33.png)

**Rysunek 13**: Dodaj Repeater do projektanta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image35.png))


Tam s tylko jedną opcję w tagu elementu powtarzanego s: Wybierz źródło danych. Zoptymalizowany pod kątem, aby utworzyć nowe kontrolki ObjectDataSource i skonfigurować go do korzystania ze `CategoriesBLL` klasy s `GetCategories` metody.


[![Tworzenie nowego elementu ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image36.png)

**Rysunek 14**: Tworzenie nowego elementu ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image38.png))


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image39.png)

**Rysunek 15**: Konfigurowanie kontrolki ObjectDataSource do użycia `CategoriesBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image41.png))


[![Pobierz informacje o wszystkich kategorii przy użyciu metody GetCategories](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image42.png)

**Rysunek 16**: pobieranie informacji o wszystkich kategorii przy użyciu `GetCategories` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image44.png))


W przeciwieństwie do kontrolki DataList Visual Studio nie tworzy automatycznie ItemTemplate dla powtarzanego po powiązaniu go ze źródłem danych. Ponadto Repeater szablony s nie można skonfigurować za pomocą projektanta i musi być określona w sposób deklaratywny.

Aby wyświetlić kategorie jako pojedynczy wiersz `<table>` kolumnę dla każdej kategorii, potrzebujemy powtarzanego do emitowania znaczników podobny do następującego:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample7.html)]

Ponieważ `<td>Category X</td>` tekst jest fragment, który jest powtarzany, pojawi się w elemencie powtarzanym s właściwości ItemTemplate. Kod znaczników, który pojawia się przed — `<table><tr>` — zostaną umieszczone w `HeaderTemplate` podczas końcowy znaczników - `</tr></table>` -zostaną umieszczone w `FooterTemplate`. Aby wprowadzić te ustawienia szablonu, przejdź do deklaratywne części strony ASP.NET, klikając przycisk źródła, w lewym dolnym rogu i wpisz następującej składni:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample8.aspx)]

Powtarzanego emituje dokładne znaczników określony przez jego szablony, nic więcej, nic nie mniej. Rysunek 17 pokazuje elementu powtarzanego s danych wyjściowych po wyświetleniu za pośrednictwem przeglądarki.


[![Pojedynczy wiersz tabeli HTML &lt;tabeli&gt; Wyświetla każdej kategorii w oddzielnych kolumnach](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image45.png)

**Rysunek 17**: pojedynczy wiersz tabeli HTML `<table>` Wyświetla każdej kategorii w oddzielnych kolumnach ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Krok 6: Poprawa wyglądu powtarzanego

Ponieważ powtarzanego emituje dokładnie znaczników, określony przez jego szablony, go jako Nic dziwnego, że powinna pochodzić, że nie istnieją właściwości stylu związane dla elementu powtarzanego. Aby zmienić wygląd zawartości generowanej przez powtarzanego, możemy ręcznie dodać bezpośrednio do szablonów elementu powtarzanego s potrzebnej zawartości HTML i CSS.

W tym przykładzie umożliwiają s kolumnach kategorii alternatywny kolory tła, takich jak za pomocą przemiennych wierszy w elemencie DataList. Aby to osiągnąć, należy przypisać `RowStyle` klasy CSS do każdego elementu powtarzanego i `AlternatingRowStyle` klasy CSS do każdego przemienne elementu powtarzanego za pośrednictwem `ItemTemplate` i `AlternatingItemTemplate` szablony, w następujący sposób:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample9.aspx)]

Umożliwiają także dodać wiersz nagłówka w danych wyjściowych z tekstem kategorie produktów s. Ponieważ firma Microsoft don wiem liczbę kolumn naszych wynikające `<table>` będzie składał się z dwóch, jest użycie to najprostszy sposób generowania wiersz nagłówka, która może obejmować wszystkie kolumny *dwóch* `<table>` s. Pierwszy `<table>` będzie zawierać dwa wiersze, wiersz nagłówka i wiersza, który będzie zawierać drugi pojedynczy wiersz `<table>` kolumnę dla każdej kategorii, która ma w systemie. Oznacza to chcemy emitować następujące znaczniki:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample10.html)]

Następujące `HeaderTemplate` i `FooterTemplate` spowodować żądaną znaczników:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample11.aspx)]

Rysunek 18 pokazuje powtarzanego po tych zmian.


[![Kolumny kategorii Alternatywny kolor tła i zawiera wiersz nagłówka](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image48.png)

**Rysunek 18**: alternatywne kolumn kategorii koloru tła i zawiera wiersz nagłówka ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image50.png))


## <a name="summary"></a>Podsumowanie

Gdy w kontrolce GridView można łatwo wyświetlić, Edycja, usuwanie, sortowanie i strony za pomocą danych, wygląd jest bardzo boxy i siatka. Aby uzyskać większą kontrolę nad wyglądem musimy włączyć do kontrolek DataList lub Repeater. Oba formanty wyświetlania zestawu rekordów przy użyciu szablonów, zamiast BoundFields, CheckBoxFields i tak dalej.

Kontrolki DataList renderowany jako kodu HTML `<table>` , domyślnie wyświetla każdy rekord źródła danych w wierszu pojedynczej tabeli, podobnie jak za pomocą pojedynczego TemplateField GridView. Jak widać w przyszłości zapoznać się z samouczkiem, jednak kontrolki DataList zezwala na wiele rekordów do wyświetlenia każdego wiersza tabeli. Powtarzanego, z drugiej strony, ściśle emituje znaczników określony w jej szablonów; nie dodaje żadnych dodatkowych znaczników i dlatego jest najczęściej używany do wyświetlania danych w elementy HTML innych niż `<table>` (np. Lista punktowana).

Gdy w kontrolkach DataList i Repeater oferuje większą elastyczność w ich wyniku renderowania, mają wiele wbudowanych funkcji, które znajdują się w widoku GridView. Jak zajmiemy się w kolejnych samouczkach, niektóre z tych funkcji można podłączyć ponownie bez zbyt dużo wysiłku, ale należy pamiętać, który korzystać kontrolki DataList lub Repeater audytów widoku GridView ograniczyć funkcje, których można używać bez konieczności implementowania tych funkcji samodzielnie.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Yaakov Ellis, Liz Shulok, Randy Schmidt i Stacy Park. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](nested-data-web-controls-cs.md)
> [dalej](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
