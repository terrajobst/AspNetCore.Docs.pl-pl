---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: "Wyświetlanie danych z elementu ObjectDataSource (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku analizuje kontrolki ObjectDataSource przy użyciu tego formantu można powiązać dane pobrane z logiki warstwy Biznesowej utworzonej w poprzednim samouczku bez havi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: d575c8f597bcb5d2a5d2e27e1145d39110daabe1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="displaying-data-with-the-objectdatasource-vb"></a>Wyświetlanie danych z elementu ObjectDataSource (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe) lub [pobierania plików PDF](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> W tym samouczku analizuje kontrolki ObjectDataSource przy użyciu tego formantu można powiązać dane pobrane z logiki warstwy Biznesowej utworzony w poprzednim samouczek bez konieczności pisania wiersz kodu!


## <a name="introduction"></a>Wprowadzenie

Z naszej aplikacji architektury i witryny sieci Web układ strony pełną jest już rozpocząć eksplorowanie sposobu wykonywania różnych zadań i raportowania związanych z danymi. W poprzednich samouczki możemy przedstawiono sposób programowo wiązanie danych z warstwy DAL i logiki warstwy Biznesowej danych formantu sieci Web strony ASP.NET. Ta składnia przypisywanie kontrolka sieci Web danych `DataSource` właściwości do danych do wyświetlenia i następnie wywoływanie formantu `DataBind()` metoda układ używany w aplikacjach ASP.NET 1.x i mogą w dalszym ciągu można użyć w aplikacjach 2.0. Jednak nowe kontrolki źródła danych programu ASP.NET 2.0 oferuje deklaratywne pracować z danymi. Za pomocą tych kontrolek można powiązać dane pobrane z logiki warstwy Biznesowej utworzony w poprzednim samouczek bez konieczności pisania wiersz kodu!

Platforma ASP.NET 2.0 jest dostarczany z pięciu kontrolki źródła danych wbudowanych [SqlDataSource](https://msdn.microsoft.com/en-us/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/en-us/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/en-us/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/en-us/library/e8d8587a%28en-US,VS.80%29.aspx), i [SiteMapDataSource](https://msdn.microsoft.com/en-us/library/5ex9t96x%28en-US,VS.80%29.aspx) mimo że można tworzyć własne [źródła danych niestandardowych formantów](https://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnvs05/html/DataSourceCon1.asp), jeśli to konieczne. Ponieważ opracowaliśmy architekturę dla naszej aplikacji samouczek, będziemy używać ObjectDataSource względem naszego klasy logiki warstwy Biznesowej.


![Program ASP.NET 2.0 zawiera pięć kontrolki źródła danych wbudowane](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**Rysunek 1**: program ASP.NET 2.0 zawiera pięć kontrolki źródła danych wbudowane


Element ObjectDataSource służy jako serwer proxy do pracy z innego obiektu. Aby skonfigurować ObjectDataSource możemy określić to podstawowy obiektu oraz sposobu mapowania ich metod w elemencie ObjectDataSource `Select`, `Insert`, `Update`, i `Delete` metody. Gdy ten obiekt nie został określony i jego metody zamapowane na element ObjectDataSource, możemy następnie powiązać ObjectDataSource danych formantu sieci Web. Program ASP.NET jest dostarczany z danych wiele formantów sieci Web, w tym widoku GridView, widoku DetailsView, RadioButtonList i DropDownList, między innymi. Podczas cyklu życia strony, danych formantu sieci Web może wymagać dostępu do danych jest powiązany, będzie osiągnąć wywoływanie jej ObjectDataSource `Select` metody; czy danych formantu sieci Web obsługuje, wstawianie, aktualizowanie, usuwanie, wywołania mogą być nawiązywane do jego W elemencie ObjectDataSource `Insert`, `Update`, lub `Delete` metody. Te wywołania następnie są kierowane przez element ObjectDataSource metod odpowiedni obiekt źródłowy, jak na poniższym diagramie przedstawiono.


[![Element ObjectDataSource służy jako serwer Proxy](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**Rysunek 2**: element ObjectDataSource służy jako serwer Proxy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image4.png))


Chociaż ObjectDataSource mogą być używane do wywoływania metod wstawiania, aktualizowania lub usuwania danych, umożliwia po prostu skupić się na zwracanie danych; Samouczki przyszłych może zapoznać się przy użyciu elementu ObjectDataSource i dane w formantach sieci Web, które modyfikują dane.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Krok 1: Dodawanie i konfigurowanie kontrolki ObjectDataSource

Uruchamianie przez otwarcie `SimpleDisplay.aspx` strony `BasicReporting` folderu, Przełącz do widoku projektu, a następnie przeciągnij kontrolki ObjectDataSource z przybornika na powierzchni projektu strony. Element ObjectDataSource pojawia się jako szary prostokąt na powierzchni projektu, ponieważ nie tworzy żadnych znaczników; po prostu uzyskuje dostęp do danych przez wywołanie metody od określonego obiektu. Dane zwrócone przez element ObjectDataSource mogą być wyświetlane na danych formantu sieci Web, takich jak GridView, widoku DetailsView FormView i tak dalej.

> [!NOTE]
> Alternatywnie, należy najpierw dodać dane kontrolka sieci Web do strony, a następnie, w tagu inteligentnego wybierz &lt;nowe źródło danych&gt; opcję z listy rozwijanej.


Aby określić obiekt ObjectDataSource oraz sposobu mapowania ObjectDataSource metod tego obiektu, kliknij łącze Konfigurowanie źródła danych z elementu ObjectDataSource tagów inteligentnych.


[![Kliknij przycisk skonfiguruj łącza źródła danych z tagów inteligentnych](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**Rysunek 3**: kliknij łącze źródła danych skonfigurować za pomocą tagu inteligentnego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image7.png))


Spowoduje to wyświetlenie Kreatora konfigurowania źródła danych. Firma Microsoft, należy określić obiekt, który jest ObjectDataSource do pracy z. Jeśli zaznaczono pole wyboru "Pokaż tylko składniki danych", liście rozwijanej na tym ekranie wyświetla tylko te obiekty, które zostały oznaczone `DataObject` atrybutu. Obecnie naszej listy zawiera TableAdapters w DataSet wpisany oraz klasy logiki warstwy Biznesowej utworzone w poprzednich instrukcji. Jeśli nie pamiętasz dodać `DataObject` atrybutu dla klasy warstwy logiki biznesowej nie zobaczysz je na tej liście. W takim przypadku usunąć zaznaczenie pola wyboru "Pokaż tylko składniki danych", aby wyświetlić wszystkie obiekty, które powinny zawierać klasy logiki warstwy Biznesowej (wraz z innych klas DataSet wpisany DataTables, elementów DataRows i tak dalej).

Na tym ekranie pierwszy wybierz `ProductsBLL` klasę z listy rozwijanej, a następnie kliknij przycisk Dalej.


[![Określ obiekt do użycia z kontrolki ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**Rysunek 4**: Określ obiekt do użycia z kontrolki ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image10.png))


Następnym ekranie kreatora zostanie wyświetlony monit o wybranie metody należy wywołać element ObjectDataSource. Lista rozwijana zawiera listę tych metod, które zwracają dane w obiekcie z poprzedniego ekranu. Tutaj przedstawiono `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, i `GetProductsBySupplierID`. Wybierz `GetProducts` metodę z listy rozwijanej i kliknij przycisk Zakończ (Jeśli dodano `DataObjectMethodAttribute` do `ProductBLL`dla metod, jak pokazano w poprzednim samouczek, ta opcja będzie domyślnie wybierany).


[![Wybierz metodę zwracać dane z wybierz kartę](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**Rysunek 5**: Wybierz metodę dla zwracanie danych na karcie Wybierz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Należy ręcznie skonfigurować ObjectDataSource

Kreator konfigurowania źródła danych przez element ObjectDataSource pozwala szybko określić obiekt, który używa i skojarzyć, jakie metody obiektu są wywoływane. Można jednak skonfigurować ObjectDataSource za pośrednictwem jego właściwości, za pomocą okna właściwości lub bezpośrednio w deklaratywne znaczników. Wystarczy ustawić `TypeName` dla właściwości typu obiektu podstawowego, które mają być używane i `SelectMethod` do metody do wywołania, gdy trwa pobieranie danych.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

Nawet jeśli wolisz kreatora skonfiguruj źródło danych może być czas, kiedy trzeba ręcznie skonfigurować ObjectDataSource, jak Kreator wyświetla tylko klasy utworzonych przez dewelopera. Jeśli chcesz powiązać ObjectDataSource klasę w programie .NET Framework, takich jak [klasy członkostwa](https://msdn.microsoft.com/en-us/library/system.web.security.membership.aspx), aby uzyskać dostępu do informacji o koncie użytkownika, lub [klasy katalogu](https://msdn.microsoft.com/en-us/library/system.io.directory.aspx) do pracy z informacje o systemie plików należy ręcznie ustawić właściwości w elemencie ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Krok 2: Dodawanie formantu danych sieci Web i powiązania jej z ObjectDataSource

Po elemencie ObjectDataSource został dodany do strony i skonfigurowany, już wszystko gotowe do dodania danych formantów sieci Web do strony, aby wyświetlić dane zwrócone przez element ObjectDataSource `Select` metody. Wszystkie dane kontrolka sieci Web może być powiązana z ObjectDataSource; Przyjrzyjmy się wyświetlanie danych w elemencie ObjectDataSource w widoku GridView, widoku DetailsView i FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Powiązanie Element GridView z ObjectDataSource

Dodawanie formantu widoku GridView z przybornika do `SimpleDisplay.aspx`na powierzchnię projektu. W widoku GridView tagów inteligentnych wybierz kontrolki ObjectDataSource, dodane w kroku 1. Automatycznie spowoduje to utworzenie elementu BoundField w widoku GridView dla każdej właściwości zwrócony przez dane z ObjectDataSource `Select` — metoda (to znaczy, właściwości zdefiniowane przez DataTable produktów).


[![Element GridView został dodany do strony, a powiązany element ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**Rysunek 6**: A GridView został dodany do strony i powiązany z ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image16.png))


Możesz następnie dostosować, zmienić lub usunąć BoundFields w widoku GridView, klikając opcję Edytowanie kolumn z tagów inteligentnych.


[![Zarządzanie w widoku GridView BoundFields za pomocą okna dialogowego Edytowanie kolumn](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**Rysunek 7**: zarządzanie w widoku GridView BoundFields poprzez Edytowanie kolumn — okno dialogowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image19.png))


Poświęć chwilę, aby zmodyfikować BoundFields w widoku GridView, usuwanie `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, i `ReorderLevel` BoundFields. Wybierz z listy w lewym dolnym elementu BoundField i kliknij przycisk Usuń (czerwony znak X) je usunąć. Następnie należy zmienić kolejność BoundFields, aby `CategoryName` i `SupplierName` BoundFields poprzedzać `UnitPrice` elementu BoundField zaznaczając te BoundFields i klikając przycisk strzałki w górę. Ustaw `HeaderText` właściwości pozostałych BoundFields do `Products`, `Category`, `Supplier`, i `Price`odpowiednio. Następnie ma `Price` elementu BoundField formacie waluty przez ustawienie elementu BoundField `HtmlEncode` wartość False dla właściwości i jego `DataFormatString` właściwości `{0:c}`. Na koniec wyrównanie w poziomie `Price` w prawo i `Discontinued` wyboru w środku za pośrednictwem `ItemStyle` / `HorizontalAlign` właściwości.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]


[![W widoku GridView BoundFields zostały dostosowane](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**Rysunek 8**: GridView BoundFields zostały dostosowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Aby uzyskać spójny wygląd przy użyciu motywów

Te samouczki starań, aby usunąć ustawienia styl formantu, zamiast niego przy użyciu kaskadowych arkuszy stylów zdefiniowane w pliku zewnętrznym, jeśli to możliwe. `Styles.css` Plik zawiera `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, i `AlternatingRowStyle` kontrolek używanych w tych samouczkach klasy CSS, które mają być używane do dyktowania wygląd danych w sieci Web. W tym celu możemy można ustawić w widoku GridView `CssClass` właściwości `DataWebControlStyle`, a jego `HeaderStyle`, `RowStyle`, i `AlternatingRowStyle` właściwości `CssClass` właściwości odpowiednio.

Jeśli te ustawiliśmy `CssClass` właściwości na formant sieci Web, czy musimy Pamiętaj, aby jawnie ustawić te wartości właściwości dla każdego danych sieci Web dodane do naszych samouczków formantu. Łatwiejsza w zarządzaniu podejście jest określenie właściwości związane z CSS domyślne dla widoku DetailsView, w widoku GridView i formanty FormView za pomocą motywu. Motyw to zbiór właściwości poziom kontroli, obrazy i klasy CSS, które można stosować do stron w całej lokacji w celu wymuszenia wspólnej wyglądu i działania.

Naszych motyw nie będzie zawierać wszystkie obrazy lub pliki CSS (pozostanie arkusza stylów `Styles.css` jako — zdefiniowanych w katalogu głównym aplikacji sieci web), ale obejmuje dwa karnacji. Karnacji jest plik definiujący właściwości domyślnej dla formantu sieci Web. W szczególności firma Microsoft będziesz mieć plik skórki dla kontrolki GridView i widoku DetailsView wskazujący domyślne `CssClass`-powiązanych właściwości.

Rozpocznij od dodania nowego pliku skórki do projektu o nazwie `GridView.skin` prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierając pozycję Dodaj nowy element.


[![Dodaj plik skórki o nazwie GridView.skin](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**Rysunek 9**: Dodawanie pliku skórki o nazwie `GridView.skin` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image25.png))


Pliki karnacji muszą być umieszczone w motywie, znajdujących się w `App_Themes` folderu. Ponieważ nie mamy jeszcze takiego folderu, potrzebna będzie oferować utworzona przez nas podczas dodawania naszym pierwszym skórki Visual Studio. Kliknij przycisk Tak, aby utworzyć `App_Theme` folderu i umieść nowe `GridView.skin` plik istnieje.


[![Program Visual Studio utworzy App_Theme Folder](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**Na rysunku nr 10**: Let Visual Studio utworzyć `App_Theme` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image28.png))


Spowoduje to utworzenie nowego motywu w `App_Themes` folder o nazwie GridView z pliku skórki `GridView.skin`.


![Motyw GridView ma został dodany do folderu App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**Rysunek 11**: motyw GridView ma został dodany do `App_Theme` folderu


Zmień motyw GridView DataWebControls (kliknij prawym przyciskiem folder widoku GridView w `App_Theme` folder i wybierz polecenie Zmień nazwę). Następnie wprowadź następujący kod do `GridView.skin` pliku:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

Definiuje właściwości domyślne `CssClass`-właściwości powiązanych z dowolnym widoku GridView w dowolnej strony użyciu motywu DataWebControls. Dodajmy inną karnację dla danych formantu sieci Web, który będzie używany wkrótce w widoku DetailsView. Dodawanie nowej karnacji do motywu DataWebControls o nazwie `DetailsView.skin` i Dodaj następujący kod:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

Z naszych motywu zdefiniowane ostatnim krokiem jest do stosowania motywu do strony ASP.NET. Na podstawie strony strona lub dla wszystkich stron w witrynie sieci Web można zastosować motyw. Użyjmy ten motyw dla wszystkich stron w witrynie internetowej. Aby to zrobić, Dodaj następujący kod do `Web.config`w `<system.web>` sekcji:


[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

To wszystko jest do niego! `styleSheetTheme` Ustawienie wskazuje, że właściwości określone w motywie powinien *nie* zastąpienie właściwości określonego na poziomie formantu. Aby określić, że ustawienia motywu powinien trump ustawienia kontroli, użyj `theme` atrybutu zamiast `styleSheetTheme`; Niestety, ustawienia kompozycji nie są wyświetlane w widoku Projekt usługi Visual Studio. Zapoznaj się [omówienie skórek i kompozycji ASP.NET](https://msdn.microsoft.com/en-us/library/ykzx33wh.aspx) i [motywów przy użyciu stylów po stronie serwera](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) Aby uzyskać więcej informacji na temat kompozycje i karnacji; zobacz [jak: Zastosuj kompozycji ASP.NET](https://msdn.microsoft.com/en-us/library/0yy5hxdk%28VS.80%29.aspx) Aby uzyskać więcej informacji na temat Konfigurowanie strony do użycia motywu.


[![Wyświetla widoku GridView produktu nazwa, Kategoria dostawcy, ceny i wycofane informacji](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**Rysunek 12**: Wyświetla widoku GridView produktu nazwa, Kategoria dostawcy, ceny i zaprzestać informacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Wyświetlanie jeden rekord naraz w widoku DetailsView

Widoku GridView Wyświetla jeden wiersz dla każdego rekordu zwróconego przez formant źródła danych, z którą jest powiązany. Brak sytuacji, gdy firma Microsoft może mają być wyświetlane wyłącznie rekordu lub tylko jeden rekord naraz. [Formantu widoku DetailsView](https://msdn.microsoft.com/en-us/library/s3w1w7t4.aspx) oferuje tę funkcję, renderowania w formacie HTML `<table>` z kolumnami i jeden wiersz dla każdej kolumny lub właściwości powiązane z formantem. Widoku DetailsView można traktować jako element GridView z pojedynczego rekordu obrócony o 90 stopni.

Rozpocznij od dodania formantu widoku DetailsView *powyżej* w widoku GridView `SimpleDisplay.aspx`. Następnie powiązać go z tej samej kontrolki ObjectDataSource jako widoku GridView. Jak z widoku GridView, elementu BoundField zostanie dodany do widoku DetailsView dla każdej właściwości w obiekcie zwracanym przez element ObjectDataSource `Select` metody. Jedyna różnica polega na tym, że DetailsView BoundFields zostały przedstawione w poziomie, a nie w pionie.


[![Na stronie Dodaj element DetailsView i powiązać ją z ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**Rysunek 13**: na stronie Dodaj element DetailsView i powiązać ją z ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image35.png))


Jak GridView aby zapewnić bardziej dostosowanego wyświetlania danych zwróconych przez element ObjectDataSource można można tweaked DetailsView BoundFields. Rysunek 14 pokazuje widoku DetailsView po jego BoundFields i `CssClass` właściwości zostały skonfigurowane, aby dopasować jego wygląd podobny do widoku GridView.


[![Pojedynczy rekord zawiera widoku DetailsView](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**Rysunek 14**: widoku DetailsView zawiera jeden rekord ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image38.png))


Należy pamiętać, że widoku DetailsView są wyświetlane tylko pierwszy rekord zwrócony przez źródło danych. Umożliwia użytkownikom do wykonania kroków opisanych wszystkich rekordów, pojedynczo, możemy Włącz stronicowanie w widoku DetailsView. Aby to zrobić, wróć do programu Visual Studio i zaznacz pole wyboru Włącz stronicowanie w widoku DetailsView tagów inteligentnych.


[![Włącz stronicowanie w widoku DetailsView formantu](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**Rysunek 15**: Włącz stronicowanie w widoku DetailsView formantu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image41.png))


[![Z włączono stronicowania widoku DetailsView umożliwia użytkownikowi przeglądanie produktów](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**Rysunek 16**: Z włączone stronicowanie, widoku DetailsView umożliwia użytkownikowi przeglądanie produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image44.png))


Firma Microsoft będzie komunikować więcej informacji o stronicowaniu w przyszłości samouczki.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Elastyczny układ przedstawiający jeden rekord naraz

Widoku DetailsView jest dość sztywnego w sposób jego wyświetlania każdego rekordu zwrócone przez element ObjectDataSource. Firma Microsoft może być bardziej elastyczne widoku danych. Na przykład zamiast przedstawiający nazwę produktu, Kategoria dostawcy, ceny i wycofane informacji w oddzielnym wierszu, może chcemy się widoczna nazwa produktu i ceny w `<h4>` pozycji z kategorii i Dostawca informacji pojawiających się poniżej nazwę i cen w mniejszej czcionki. I nie może Dbamy można wyświetlić nazwy właściwości (produktu, kategorii i tak dalej) obok wartości.

[Kontroli FormView](https://msdn.microsoft.com/en-US/library/fyf1dk77.aspx) zawiera ten poziom dostosowania. Zamiast przy użyciu pól (np. opcji widoku GridView i widoku DetailsView nie), FormView korzysta z szablonów, które umożliwiają różnych formantów sieci Web, statyczne HTML i [składnia wiązania z danymi](http://www.15seconds.com/issue/040630.htm). Jeśli znasz kontrolce elementu powtarzanego z platformy ASP.NET 1.x, możesz traktować FormView jako elementu powtarzanego do wyświetlania pojedynczego rekordu.

Dodawanie do formantu FormView `SimpleDisplay.aspx` powierzchni projektu strony. Początkowo FormView wyświetlane jako blok szarego informowania potrzebujemy zapewnić co najmniej formantu `ItemTemplate`.


[![FormView musi zawierać element ItemTemplate](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**Rysunek 17**: musi zawierać FormView `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image47.png))


FormView można powiązać bezpośrednio do kontroli źródła danych za pomocą tagów inteligentnych FormView, co spowoduje utworzenie domyślnego `ItemTemplate` automatycznie (wraz z `EditItemTemplate` i `InsertItemTemplate`, jeśli formant ObjectDatatSource `InsertMethod` i `UpdateMethod` właściwości są ustawione). Jednak w tym przykładzie załóżmy powiązać danych FormView i określ jej `ItemTemplate` ręcznie. Rozpocznij od ustawienie FormView `DataSourceID` właściwości `ID` kontrolki ObjectDataSource `ObjectDataSource1`. Następnie należy utworzyć `ItemTemplate` tak, aby Wyświetla nazwę produktu i cen w `<h4>` elementu oraz nazwy kategorii i wysyłającej pod który mniejszy rozmiar czcionki.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]


[![Pierwszy produktu (Chai) jest wyświetlana w formacie niestandardowe](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**Rysunek 18**: pierwszy produkt (Chai) jest wyświetlana w formacie niestandardowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image50.png))


`<%# Eval(propertyName) %>` Jest składnia wiązania z danymi. `Eval` Metoda zwraca wartość określonej właściwości dla bieżącego obiektu jest powiązany z kontrolą FormView. Zapoznaj się z artykułem Alexowi Homer [uproszczonym i rozszerzone danych powiązania w ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) uzyskać więcej informacji dotyczących dokumentów i ins powiązania danych.

Podobnie jak DetailsView FormView wyświetlane są tylko pierwszy rekord zwrócone przez element ObjectDataSource. Można włączyć stronicowanie w widoku FormView, aby umożliwić użytkownikom zewnętrznym do wykonania kroków opisanych produktów jednym naraz.

## <a name="summary"></a>Podsumowanie

Uzyskiwanie dostępu i wyświetlanie danych z warstwy logiki biznesowej można wykonywać bez pisania wiersz kodu dzięki użyciu kontrolki ObjectDataSource ASP.NET 2.0. Element ObjectDataSource wywołuje metodę określonej klasy i zwraca wyniki. Wyniki te można wyświetlić w danych formantu sieci Web, który jest powiązany element ObjectDataSource. W tym samouczku analizujemy powiązywanie kontrolek GridView widoku DetailsView i FormView ObjectDataSource.

Do tej pory możemy tylko przedstawiono sposób użycia ObjectDataSource można wywołać metody bez parametrów, ale co zrobić, jeśli chcemy wywołać metodę, która oczekuje wprowadź parametry, takie jak `ProductBLL` klasy `GetProductsByCategoryID(categoryID)`? Aby wywołać metodę, która oczekuje co najmniej jeden parametr możemy skonfigurować ObjectDataSource, aby określić wartości tych parametrów. Zajmiemy się tym, jak w tym w naszym [następny samouczek](declarative-parameters-vb.md).

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Utwórz własne kontrolki źródła danych](https://msdn.microsoft.com/en-us/library/ms364049.aspx)
- [Element GridView przykłady dla programu ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/aa479339.aspx)
- [Uproszczone i rozszerzone powiązania danych składni w programie ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm)
- [Motywy w programie ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Style po stronie serwera za pomocą motywów](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Porady: Programowane zastosować kompozycji ASP.NET](https://msdn.microsoft.com/en-us/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Hilton Giesenow. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
[dalej](declarative-parameters-vb.md)
