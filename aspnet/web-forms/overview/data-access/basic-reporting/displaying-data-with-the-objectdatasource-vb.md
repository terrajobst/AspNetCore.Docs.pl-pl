---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: Wyświetlanie danych za pomocą kontrolki ObjectDataSource (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten samouczek analizuje kontrolka ObjectDataSource, za pomocą tego formantu można powiązać dane pobrane z LOGIKI utworzonej w poprzednim samouczku bez havi...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: f49cbf19b090917c170b025c269f825cf486c31a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755632"
---
<a name="displaying-data-with-the-objectdatasource-vb"></a>Wyświetlanie danych za pomocą kontrolki ObjectDataSource (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe) lub [Pobierz plik PDF](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> Ten samouczek analizuje kontrolka ObjectDataSource, za pomocą tego formantu można powiązać dane pobrane z LOGIKI utworzony w poprzednim samouczku, bez konieczności pisania nawet wiersza kodu!


## <a name="introduction"></a>Wprowadzenie

Z naszych aplikacji architektury i witryny sieci Web układu strony ukończone możemy rozpocząć eksplorowanie sposób wykonywania różnych typowych zadań i raportowanie związanych z danymi. W poprzednich samouczkach pokazaliśmy już, jak programowo wiązanie danych z warstwy DAL i logiki warstwy Biznesowej z danymi formantu sieci Web, na stronie ASP.NET. Ta składnia przypisywanie sieci Web kontroli danych `DataSource` właściwości do danych do wyświetlania, a następnie wywołując formantu `DataBind()` metoda została wzorzec używany w aplikacjach 1.x ASP.NET i mogą w dalszym ciągu można używać w aplikacji w wersji 2.0. Oferuje natomiast wraz ze nowe kontrolki źródła danych programu ASP.NET 2.0 deklaratywne, aby pracować z danymi. Za pomocą tych kontrolek można powiązać dane pobrane z LOGIKI utworzony w poprzednim samouczku, bez konieczności pisania nawet wiersza kodu!

Program ASP.NET 2.0, który jest dostarczany z pięć formantów źródła danych wbudowane [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [elementu XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), i [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx) mimo, że możesz tworzyć własne [kontrolki źródła danych niestandardowych](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), jeśli to konieczne. Ponieważ opracowaliśmy architekturę dla naszych samouczków aplikacji, będziemy używać kontrolki ObjectDataSource względem naszych zajęć LOGIKI.


![Platforma ASP.NET 2.0 obejmuje pięć formantów źródła danych wbudowane](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**Rysunek 1**: Platforma ASP.NET 2.0 obejmuje pięć formantów źródła danych wbudowane


Kontrolki ObjectDataSource służy jako serwer proxy do pracy z innego obiektu. Aby skonfigurować kontrolki ObjectDataSource określamy to podstawowe obiektu i sposobu jego metody mapowania na ObjectDataSource `Select`, `Insert`, `Update`, i `Delete` metody. Gdy ten obiekt został określony i jego metod mapowane na ObjectDataSource, firma Microsoft następnie powiązać kontrolki ObjectDataSource z danymi formantu sieci Web. ASP.NET jest dostarczany z danymi wielu formantów sieci Web, w tym widoku GridView, DetailsView, RadioButtonList i DropDownList, między innymi. Podczas cyklu życia strony, dane formantu sieci Web może być konieczne dostęp do danych jest powiązany, co pozwoli wykonać za pomocą jego elementu ObjectDataSource `Select` metody; czy danych formantu sieci Web obsługuje, wstawianie, aktualizowanie, usuwanie, wywołania można dokonywać jego ObjectDataSource `Insert`, `Update`, lub `Delete` metody. Te wywołania następnie są kierowane przez kontrolki ObjectDataSource do odpowiedniego obiektu bazowego metod, tak jak pokazano na poniższym diagramie.


[![Kontrolki ObjectDataSource służy jako serwer Proxy](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**Rysunek 2**: ObjectDataSource służy jako serwer Proxy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image4.png))


Gdy kontrolki ObjectDataSource może służyć do wywołania metod wstawiania, aktualizowania lub usuwania danych, możemy skupić się na zwracanie danych; Samouczki przyszłych przedstawimy, za pomocą kontrolki ObjectDataSource i danych kontrolki sieci Web, które modyfikują dane.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Krok 1: Dodawanie i konfigurowanie kontrolki ObjectDataSource

Zacznij od otwarcia `SimpleDisplay.aspx` stronie `BasicReporting` folderu, przejdź do widoku projektu, a następnie przeciągnij formantu ObjectDataSource z przybornika na powierzchni projektowej strony. Kontrolki ObjectDataSource pojawia się jako szary prostokąt na powierzchni projektowej, ponieważ nie generuje żadnych znaczników; ją po prostu uzyskuje dostęp do danych przez wywołanie metody od określonego obiektu. Dane zwrócone przez kontrolki ObjectDataSource mogą być wyświetlane przez dane kontrolki sieci Web, na przykład GridView, DetailsView, FormView i tak dalej.

> [!NOTE]
> Alternatywnie możesz najpierw Dodaj dane formantu sieci Web do strony, a następnie w tagu inteligentnego, wybierz &lt;nowe źródło danych&gt; opcję z listy rozwijanej.


Aby określić obiekt źródłowy i jak metody tego obiektu mapowania ObjectDataSource ObjectDataSource, kliknij link Konfigurowanie źródła danych z ObjectDataSource tagu inteligentnego.


[![Kliknij przycisk Konfiguruj łącze do źródła danych za pomocą tagu inteligentnego](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**Rysunek 3**: kliknij przycisk Konfiguruj łącze do źródła danych za pomocą tagu inteligentnego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image7.png))


Otwarte kreatora Konfigurowanie źródła danych. Po pierwsze firma Microsoft należy określić obiekt, który jest pracować z kontrolki ObjectDataSource. Jeśli zaznaczono pole wyboru "Pokaż tylko składniki danych", listy rozwijanej na tym ekranie wyświetla tylko te obiekty, które mają ozdobione `DataObject` atrybutu. Obecnie nasz lista zawiera elementów TableAdapter wpisany zestaw danych i klas LOGIKI, utworzonego w poprzednim samouczku. Jeśli nie pamiętasz dodać `DataObject` atrybutu dla klasy warstwy logiki biznesowej nie zobaczysz je na tej liście. W takim przypadku należy usunąć zaznaczenie pola wyboru "Pokaż tylko składniki danych", aby wyświetlić wszystkie obiekty, które powinny zawierać klasy LOGIKI (wraz z pierwszą klasą w wpisany zestaw danych DataTable, dotyczy to również utworzeń i tak dalej).

Z tym pierwszym ekranie Wybierz `ProductsBLL` klasy z listy rozwijanej, a następnie kliknij przycisk Dalej.


[![Określ obiekt do użycia za pomocą kontrolki ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**Rysunek 4**: Określ obiekt do użycia za pomocą kontrolki ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image10.png))


Następnym ekranie kreatora zostanie wyświetlony monit o wybranie kontrolki ObjectDataSource należy wywołać metody. Lista rozwijana zawiera listę tych metod, które zwracają dane w obiekcie wybrana w zaufanym poprzedni ekran. Tutaj widzimy `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, i `GetProductsBySupplierID`. Wybierz `GetProducts` metodę z listy rozwijanej i kliknij przycisk Zakończ (Jeśli dodano `DataObjectMethodAttribute` do `ProductBLL`przez metody, jak pokazano w poprzednim samouczku ta opcja zostanie wybrana domyślnie).


[![Wybierz metodę zwracający dane na karcie Wybierz OPCJĘ](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**Rysunek 5**: wybór metody używanej do zwracania danych na karcie Wybierz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Ręczne konfigurowanie kontrolki ObjectDataSource

Kreator konfigurowania źródła danych ObjectDataSource oferuje szybki sposób, aby określić obiekt, który używa i skojarzyć wywoływania metod obiektu. Można jednak skonfigurować ObjectDataSource za pośrednictwem jego właściwości w oknie właściwości lub bezpośrednio w oznaczeniu deklaracyjnym. Po prostu ustaw `TypeName` właściwości typu obiektu podstawowego, który ma być używany oraz `SelectMethod` metody do wywołania podczas pobierania danych.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

Nawet jeśli wolisz kreatora Konfigurowanie źródła danych, które mogą wystąpić sytuacje, kiedy należy ręcznie skonfigurować ObjectDataSource, jak Kreator wyświetla tylko utworzone przez projektanta klas. Jeśli chcesz powiązać kontrolki ObjectDataSource do klasy w .NET Framework, takich jak [klasa członkowska](https://msdn.microsoft.com/library/system.web.security.membership.aspx), aby dostęp do informacji o koncie użytkownika lub [klasy katalogu](https://msdn.microsoft.com/library/system.io.directory.aspx) do pracy z informacje o systemie plików należy ręcznie ustawić właściwości ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Krok 2: Dodawanie formantu sieci Web danych i powiązywanie kontrolki ObjectDataSource

Po dodał do strony i skonfigurowaniu kontrolki ObjectDataSource Służymy kontrolki sieci Web do wskazywania strony, aby wyświetlić dane zwrócone przez ObjectDataSource `Select` metody. Wszelkie dane formantu sieci Web może być powiązana z kontrolki ObjectDataSource; Przyjrzyjmy się wyświetlanie ObjectDataSource danych w kontrolce GridView, DetailsView i FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Powiązanie kontrolki ObjectDataSource GridView

Dodawanie kontrolki GridView z przybornika, aby `SimpleDisplay.aspx`na powierzchni projektowej. W tagu inteligentnego GridView wybierz kontrolka ObjectDataSource, dodane w kroku 1. Automatycznie spowoduje to utworzenie elementu BoundField w widoku GridView dla każdej właściwości, na podstawie danych zwróciło ObjectDataSource `Select` — metoda (to znaczy, właściwości zdefiniowane przez DataTable produktów).


[![GridView został dodany do strony i powiązane z kontrolki ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**Rysunek 6**: GridView został dodany do strony i powiązana kontrolki ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image16.png))


Możesz następnie dostosować, zmienić lub usunąć GridView BoundFields, klikając opcję Edytuj kolumny z tagu inteligentnego.


[![Zarządzanie BoundFields GridView za pomocą okna dialogowego Edycja kolumn](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**Rysunek 7**: Zarządzanie GridView BoundFields za pomocą kolumny okno dialogowe Edytowanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image19.png))


Poświęć chwilę, aby zmodyfikować BoundFields GridView, usuwając `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, i `ReorderLevel` BoundFields. Po prostu wybierz elementu BoundField z listy w lewym dolnym i kliknij przycisk Usuń (czerwony znak X) usuń je. Następnie rozmieszczanie BoundFields tak, aby `CategoryName` i `SupplierName` poprzedzać BoundFields `UnitPrice` elementu BoundField, wybierając te BoundFields i klikając strzałkę w górę. Ustaw `HeaderText` właściwości pozostałe BoundFields do `Products`, `Category`, `Supplier`, i `Price`, odpowiednio. Następnie, Spowoduj `Price` elementu BoundField w formacie waluty przez ustawienie elementu BoundField `HtmlEncode` wartość False dla właściwości i jego `DataFormatString` właściwość `{0:c}`. Na koniec wyrównanie w poziomie `Price` po prawej stronie i `Discontinued` pola wyboru w środku za pośrednictwem `ItemStyle` / `HorizontalAlign` właściwości.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]


[![Zostały dostosowane GridView BoundFields](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**Rysunek 8**: GridView BoundFields zostały dostosowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Używanie motywów do spójnego wyglądu

Te samouczki Dokładamy wszelkich starań usunąć wszelkie ustawienia stylu poziom kontroli, zamiast przy użyciu kaskadowych arkuszy stylów zdefiniowanych w pliku zewnętrznego, jeśli to możliwe. `Styles.css` Plik zawiera `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, i `AlternatingRowStyle` kontrolek używanych w tych samouczkach klas CSS, które powinny być używane do dyktowania wygląd danych w sieci Web. Aby to osiągnąć, możemy ustawić GridView `CssClass` właściwości `DataWebControlStyle`i jego `HeaderStyle`, `RowStyle`, i `AlternatingRowStyle` właściwości `CssClass` właściwości odpowiednio.

Jeśli ustawimy te `CssClass` właściwości na kontrolki sieci Web, firma Microsoft musiałaby Pamiętaj, aby jawnie ustawić te wartości właściwości dla każdego danych sieci Web kontroli dodany do naszych samouczków. Łatwiejsza w zarządzaniu podejście jest określenie właściwości związane z CSS domyślnych DetailsView, w widoku GridView i FormView kontroluje, zastosowanie motywu. Motyw to zbiór ustawień właściwości poziom kontroli, obrazy i klas CSS, które mogą być stosowane do stron w witrynie, aby wymusić wspólne wygląd i działanie.

Nasze motyw nie będą zawierać wszelkie obrazy i pliki CSS (pozostawimy arkusza stylów `Styles.css` jako — zdefiniowano w folderze głównym aplikacji sieci web), ale będzie zawierać dwa skórki. Powłoki jest plikiem, który definiuje właściwości domyślne dla formantu sieci Web. W szczególności odpowiemy plik Skin do kontrolki GridView i DetailsView wskazujący domyślne `CssClass`-powiązane właściwości.

Najpierw Dodaj nowy plik Skin do projektu o nazwie `GridView.skin` , klikając prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierając pozycję Dodaj nowy element.


[![Dodaj plik Skin o nazwie GridView.skin](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**Rysunek 9**: Dodaj plik Skin o nazwie `GridView.skin` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image25.png))


Pliki skin muszą być umieszczone w motywie, znajdujących się w `App_Themes` folderu. Ponieważ jeszcze nie masz taki folder, potrzebna będzie oferować Zrób to dla nas podczas dodawania naszym pierwszym skórki programu Visual Studio. Kliknij przycisk Tak, aby utworzyć `App_Theme` folder i umieść nowe `GridView.skin` plik istnieje.


[![Program Visual Studio utworzy App_Theme Folder](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**Na rysunku nr 10**: Let Visual Studio utworzyć `App_Theme` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image28.png))


Spowoduje to utworzenie nowego motywu w `App_Themes` folder o nazwie GridView plik Skin `GridView.skin`.


![Motyw GridView ma został dodany do folderu App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**Rysunek 11**: motyw GridView ma została dodana do `App_Theme` folderu


Zmień motyw GridView DataWebControls (kliknij prawym przyciskiem myszy w folderze GridView w `App_Theme` folder i wybierz polecenie Zmień nazwę). Następnie wprowadź następujące znaczniki do `GridView.skin` pliku:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

Definiuje właściwości domyślne `CssClass`— właściwości powiązanych z dowolnym GridView dowolnej stronie, która używa motywu DataWebControls. Dodajmy inną skórki dla DetailsView danych formantu sieci Web, która będzie używana wkrótce. Dodaj nowe skórki do motywu DataWebControls o nazwie `DetailsView.skin` i Dodaj następujący kod:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

Z naszych motywu zdefiniowane ostatnim krokiem jest do stosowania motywu do strony programu ASP.NET. Motyw mogą być stosowane na podstawie strony strona lub dla wszystkich stron w witrynie sieci Web. Użyjemy tego motywu dla wszystkich stron w witrynie sieci Web. Aby to zrobić, Dodaj następujący kod do `Web.config`firmy `<system.web>` sekcji:


[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

To wszystko jest do niego! `styleSheetTheme` Ustawienie wskazuje, że określono w motywie właściwości powinny *nie* zastąpienie właściwości określonego na poziomie kontroli. Aby określić, że ustawienia kompozycji powinien trump ustawienia kontroli, użyj `theme` atrybutu zamiast `styleSheetTheme`; Niestety ustawień motywu nie są wyświetlane w widoku projektu usługi Visual Studio. Zapoznaj się [omówienie skórek i motywów programu ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx) i [motywy przy użyciu stylów po stronie serwera](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) Aby uzyskać więcej informacji na temat motywy i skórek; zobacz [How to: stosowanie motywów programu ASP.NET](https://msdn.microsoft.com/library/0yy5hxdk%28VS.80%29.aspx) Aby uzyskać więcej informacji na temat Konfigurowanie strony do użycia motywu.


[![Kontrolki GridView Wyświetla nazwy, kategorii, dostawca, ceny i nieobsługiwane informacje produktu](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**Rysunek 12**: widoku GridView Wyświetla nazwy, kategorii, dostawca, ceny i wycofane informacji produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Wyświetlanie jednego rekordu w czasie w DetailsView

Kontrolki GridView Wyświetla jeden wiersz dla każdego rekordu zwróconego przez formant źródła danych, z którą jest powiązany. Brak sytuacji, kiedy warto wyświetlić jedynego rekordu lub tylko do jednego rekordu w danym momencie. [Kontrolce DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) oferuje tę funkcję, renderowania w formacie HTML `<table>` z dwiema kolumnami i jeden wiersz dla każdej kolumny lub właściwości powiązane z formantem. DetailsView można traktować jako GridView za pomocą pojedynczego rekordu obracany o 90 stopni.

Rozpocznij od dodania kontrolce DetailsView *powyżej* GridView w `SimpleDisplay.aspx`. Następnie powiązać go do tej samej kontrolki ObjectDataSource jako widoku GridView. Tak, jak przy użyciu GridView elementu BoundField zostaną dodane do DetailsView dla każdej właściwości w obiekcie zwracanym przez ObjectDataSource `Select` metody. Jedyna różnica polega na tym, że DetailsView BoundFields są ułożone poziomo, a nie w pionie.


[![Na stronie Dodaj DetailsView i powiązać ją z kontrolki ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**Rysunek 13**: Dodaj element DetailsView do strony i powiązać ją z kontrolki ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image35.png))


Podobnie jak GridView DetailsView BoundFields można tweaked zapewnienie dostosować wyświetlanie danych zwróconych przez kontrolki ObjectDataSource. Rysunek 14 pokazuje DetailsView po jego BoundFields i `CssClass` właściwości zostały skonfigurowane się podobne do kontrolki GridView jego wyglądu.


[![DetailsView zawiera jeden rekord](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**Rysunek 14**: DetailsView zawiera jeden rekord ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image38.png))


Należy pamiętać, że DetailsView są wyświetlane tylko pierwszy rekord zwrócony przez źródło danych. Aby umożliwić użytkownikowi przejść przez wszystkie rekordy, pojedynczo, musimy włączyć stronicowanie dla DetailsView. Aby to zrobić, wróć do programu Visual Studio i zaznacz pole wyboru Włącz stronicowania w DetailsView tagu inteligentnego.


[![Włączanie stronicowania w kontrolce DetailsView](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**Rysunek 15**: Włączanie stronicowania w kontrolce DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image41.png))


[![Oferuje włączono stronicowania DetailsView umożliwia użytkownikowi wyświetlanie produktów](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**Rysunek 16**: oferuje włączone stronicowanie DetailsView umożliwia użytkownikowi wyświetlanie produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image44.png))


Omówimy więcej informacji na temat stronicowania w przyszłości samouczków.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Bardziej elastyczny układ przedstawiająca jeden rekord jednocześnie

DetailsView jest dość sztywne w sposób wyświetlania każdego rekordu zwróconego z kontrolki ObjectDataSource. Firma Microsoft może być bardziej elastyczne widok danych. Na przykład, zamiast wyświetlać nazwę produktu, kategorii, dostawca, ceny i nieobsługiwane informacje w oddzielnym wierszu, może być chcemy wyświetlić nazwę produktu i ceny w `<h4>` nagłówka z informacjami o kategorii i dostawcy, pojawiają się poniżej nazwy i ceny w mniejszej czcionki. A firma Microsoft może obchodzi pokazać nazwy właściwości (produkt, kategoria i tak dalej) obok wartości.

[Kontroli FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) zawiera dostosowania na tym poziomie. Zamiast używania pól (jak kontrolkami GridView i DetailsView), FormView korzysta z szablonów, które pozwalają na kombinacji kontrolki sieci Web, statyczny kod HTML, a [składnia wiązania danych](http://www.15seconds.com/issue/040630.htm). Jeśli dopiero zaczynasz z kontrolką elementu powtarzanego z ASP.NET 1.x, można traktować FormView jako elementu powtarzanego do wyświetlania jeden rekord.

Dodawanie do formantu FormView `SimpleDisplay.aspx` strony powierzchni projektowej. Początkowo FormView wyświetlane jako szary bloku informująca, należy podać co najmniej formantu `ItemTemplate`.


[![Musisz FormView zawierają właściwości ItemTemplate](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**Rysunek 17**: musi zawierać FormView `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image47.png))


Można powiązać FormView bezpośrednio do kontroli źródła danych za pomocą tagu inteligentnego FormView, która spowoduje to utworzenie domyślnych `ItemTemplate` automatycznie (wraz z `EditItemTemplate` i `InsertItemTemplate`, jeśli kontrolka ObjectDatatSource `InsertMethod` i `UpdateMethod` właściwości są ustawione). Jednak w tym przykładzie załóżmy powiązać dane FormView i określ jej `ItemTemplate` ręcznie. Rozpocznij od FormView `DataSourceID` właściwości `ID` formantu ObjectDataSource `ObjectDataSource1`. Następnie należy utworzyć `ItemTemplate` tak, aby wyświetlał nazwę produktu i cenę w `<h4>` elementu i kategorii shipper nazwy i umieszczone pod wpisami, które w mniejszej czcionki.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]


[![Pierwszy produkt (Chai) jest wyświetlana w formacie niestandardowe](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**Rysunek 18**: pierwszy produkt (Chai) jest wyświetlana w formacie niestandardowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-objectdatasource-vb/_static/image50.png))


`<%# Eval(propertyName) %>` Jest składnia wiązania danych. `Eval` Metoda zwraca wartość określonej właściwości bieżącego obiektu, który jest powiązany z kontrolą FormView. Zapoznaj się artykułem Alex Homer [uproszczony i rozszerzone dane powiązanie składni w programie ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) Aby uzyskać więcej informacji na temat upewnimy wiązania danych.

Podobnie jak DetailsView FormView wyświetla tylko pierwszy rekord zwrócony z kontrolki ObjectDataSource. Można włączyć stronicowania w FormView, aby umożliwić użytkownikom zewnętrznym krokowo produktów jednego naraz.

## <a name="summary"></a>Podsumowanie

Uzyskiwanie dostępu do i wyświetlanie danych za pomocą warstwy logiki biznesowej można osiągnąć bez konieczności pisania choćby wiersza kodu dzięki formantu ObjectDataSource ASP.NET 2.0. Kontrolki ObjectDataSource wywołuje określoną metodę w klasie i zwraca wyniki. Te wyniki mogą być wyświetlane w danych formantu sieci Web, który jest powiązany z kontrolki ObjectDataSource. W tym samouczku przyjrzeliśmy się powiązywanie kontrolki GridView DetailsView i FormView kontrolki ObjectDataSource.

Dotąd przedstawiono tylko jak za pomocą kontrolki ObjectDataSource wywołania metody bez parametrów, ale co zrobić, jeśli chcemy wywołać metodę, która oczekuje wprowadzania parametrów, takich jak `ProductBLL` klasy `GetProductsByCategoryID(categoryID)`? Aby można było wywołać metodę, która oczekuje co najmniej jeden parametr możemy skonfigurować ObjectDataSource, aby określić wartości dla parametrów. Zobaczymy, jak w tym w naszym [następnego samouczka](declarative-parameters-vb.md).

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Utwórz własne kontrolki źródła danych](https://msdn.microsoft.com/library/ms364049.aspx)
- [GridView przykładów dla platformy ASP.NET 2.0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Uproszczone i rozszerzone powiązania danych składni w programie ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm)
- [Motywy na platformie ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Style po stronie serwera za pomocą motywów](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Instrukcje: Programowe stosowanie motywów programu ASP.NET](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Hilton Giesenow. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
> [dalej](declarative-parameters-vb.md)
