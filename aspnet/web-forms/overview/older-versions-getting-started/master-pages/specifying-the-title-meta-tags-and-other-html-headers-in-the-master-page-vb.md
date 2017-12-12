---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: "Określenie tytuł, tagi Meta i innych nagłówków HTML na stronie wzorcowej (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Analizuje różnych technik w celu zdefiniowania asortymentach &lt;head&gt; elementy na stronie wzorcowej ze strony zawartość."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 1bbc2efc67d2d828dd0a5c1fcfe95145e8ffb2cb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>Określenie tytuł, tagi Meta i innych nagłówków HTML na stronie wzorcowej (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Analizuje różnych technik w celu zdefiniowania asortymentach &lt;head&gt; elementy na stronie wzorcowej ze strony zawartość.


## <a name="introduction"></a>Wprowadzenie

Domyślnie dwa formanty ContentPlaceHolder nowe strony utworzone w programie Visual Studio 2008 mają: jedną o nazwie `head`i znajduje się w `<head>` element; i jedną o nazwie `ContentPlaceHolder1`, umieszczony w formularzu sieci Web. Celem `ContentPlaceHolder1` jest zdefiniowanie region w formularzu sieci Web, który można dostosować na podstawie strony strona. `head` ContentPlaceHolder umożliwia strony dodać niestandardowe zawartości do `<head>` sekcji. (Oczywiście te dwa elementy ContentPlaceHolders mogą zostać zmodyfikowane lub usunięte, a dodatkowe ContentPlaceHolder mogą być dodawane do strony wzorcowej. Nasze strony wzorcowej `Site.master`, obecnie zawiera cztery kontrolki elementu ContentPlaceHolder.)

Kod HTML `<head>` elementu służy jako repozytorium informacji o dokument strony sieci web, który nie jest częścią dokumentu. Obejmuje to informacje, takie jak tytuł strony sieci web, meta informacje używane przez aparaty wyszukiwania lub wewnętrzny przeszukiwarek i linki do zasobów zewnętrznych, takich jak źródeł danych RSS, JavaScript i CSS plików. Niektóre z tych informacji mogą być przydatne do wszystkich stron w witrynie internetowej. Na przykład możesz chcieć globalnie importować tego samego reguły CSS i JavaScript plików dla każdej strony ASP.NET. Istnieją jednak części `<head>` elementów, które są specyficzne dla strony. Tytuł strony jest podstawowym przykład.

W tym samouczku omówione sposób definiowania globalne i specyficzne dla strony `<head>` sekcji znaczników na stronie głównej i strony z jego zawartością.

## <a name="examining-the-master-pagesheadsection"></a>Badanie strony wzorcowej`<head>`sekcji

Plik strony głównej domyślny utworzony przez program Visual Studio 2008 zawiera następujący kod w jego `<head>` sekcji:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Zwróć uwagę, że `<head>` zawiera element `runat="server"` atrybut, który wskazuje, że jest kontrolki serwera (zamiast Statycznych). Wszystkie strony ASP.NET, pochodzi z [ `Page` klasy](https://msdn.microsoft.com/en-us/library/system.web.ui.page.aspx), który znajduje się w `System.Web.UI` przestrzeni nazw. Ta klasa zawiera [ `Header` właściwości](https://msdn.microsoft.com/en-us/library/system.web.ui.page.header.aspx) zapewniający dostęp do tej strony `<head>` regionu. Przy użyciu `Header` właściwości możemy Ustaw tytuł strony platformy ASP.NET, lub Dodaj dodatkowe znaczników do renderowanej `<head>` sekcji. Jest to możliwe, następnie w celu dostosowania strony zawartości `<head>` element pisząc fragmentem kodu na stronie `Page_Load` obsługi zdarzeń. Omówione jak programowo ustawia tytuł strony w kroku 1.

Znaczników pokazano `<head>` powyżej zawiera również element ContentPlaceHolder formantu o nazwie `head`. Ten formant ContentPlaceHolder nie jest to konieczne, jak strony zawartości można dodać niestandardowe zawartości do `<head>` element programowo. Jest przydatne, jednak w sytuacjach, w którym strony zawartości musi dodać statycznych znaczników do `<head>` jako statyczne znaczników można można dodać elementu deklaratywnie do odpowiedniego formantu zawartości, a nie programowo.

Oprócz `<title>` elementu i `head` ContentPlaceHolder, strony wzorcowej dla `<head>` powinien zawierać element `<head>`-znaczniki poziomu, które są wspólne dla wszystkich stron. W naszym witryny sieci Web, wszystkich stron użyj reguł CSS zdefiniowanych w `Styles.css` pliku. W rezultacie Zaktualizowaliśmy `<head>` element [ *tworzenie układu całej lokacji za pomocą stron wzorcowych* ](creating-a-site-wide-layout-using-master-pages-vb.md) samouczkiem, aby dołączyć do odpowiadającego `<link>` elementu. Nasze `Site.master` do bieżącej strony wzorcowej `<head>` znaczników są wyświetlane poniżej.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Krok 1: Ustawienie tytuł strony zawartości

Tytuł strony sieci web jest określona za pomocą `<title>` elementu. Jest ważne, aby ustawić tytuł każdej strony do odpowiedniej wartości. Podczas odwiedzania strony, jego tytuł jest wyświetlany w pasku tytułu w przeglądarce. Ponadto gdy tworzenie zakładek dla strony, przeglądarki używają tytuł strony jako sugerowana nazwa zakładki. Ponadto wiele aparaty wyszukiwania Pokaż tytuł strony w przypadku wyświetlania wyników wyszukiwania.

> [!NOTE]
> Domyślnie program Visual Studio ustawia `<title>` elementu na stronie głównej stroną"bez tytułu". Podobnie, nowych stron ASP.NET ma ich `<title>` zbyt ustawioną "Bez tytułu strony". Ponieważ mogą być łatwo Pamiętaj, aby ustawić tytuł strony do odpowiedniej wartości, istnieje wiele stron w Internecie z tytułem "Bez tytułu strony". Wyszukiwanie Google dla stron sieci web z tym tytułem zwraca około 2,460,000 wyników. Microsoft nawet jest podatny na publikowania stron sieci web z tytułem "Bez tytułu strony". W momencie pisania tego wyszukiwania Google zgłosił 236 tych stron sieci web w domenie Microsoft.com.


Strony ASP.NET można określić jego tytuł w jednym z następujących sposobów:

- Umieszczając wartości bezpośrednio w ramach `<title>` — element
- Przy użyciu `Title` atrybutu w `<%@ Page %>` — dyrektywa
- Programowo ustawienie strony `Title` właściwości przy użyciu kodu, takie jak `Page.Title="title"` lub `Page.Header.Title="title"`.

Zawartość strony nie mają `<title>` elementu, ponieważ jest zdefiniowana na stronie głównej. W związku z tym można ustawić tytułu strony zawartości można użyć `<%@ Page %>` dyrektywy `Title` atrybutu lub ustaw ją programowo.

### <a name="setting-the-pages-title-declaratively"></a>Ustawienie deklaratywnie tytuł strony

Tytuł strony zawartości można ustawić deklaratywnie za pomocą `Title` atrybutu [ `<%@ Page %>` dyrektywy](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx). Tej właściwości można ustawić bezpośrednio modyfikując `<%@ Page %>` dyrektywy lub za pośrednictwem okna właściwości. Przyjrzyjmy się obu podejść.

W widoku źródła zlokalizuj `<%@ Page %>` dyrektywy, który znajduje się na górze strony deklaratywne znaczników. `<%@ Page %>` Dyrektywy dla `Default.aspx` następuje:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

`<%@ Page %>` Dyrektywa określa atrybuty specyficzne dla strony używane przez aparat ASP.NET podczas analizowania i kompilowania strony. W tym jego plik strony głównej, lokalizację pliku jego kodu i jego tytuł, między innymi informacje o.

Domyślnie podczas tworzenia nowej zawartości strony ustawia Visual Studio `Title` atrybutu "Bez tytułu strony". Zmień `Default.aspx`w `Title` atrybutu "Bez tytułu strony" do "Samouczki strony wzorca", a następnie Wyświetl stronę za pośrednictwem przeglądarki. Rysunek 1 pokazuje pasek tytułu w przeglądarce, która odzwierciedla nowy tytuł strony.


![Pojawi się na pasku tytułu w przeglądarce &quot;samouczki strony wzorca&quot; zamiast &quot;bez tytułu strony&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Rysunek 01**: pasek tytułu w przeglądarce pojawi się na "Samouczki strony wzorca" zamiast "Bez tytułu strony"


Tytuł strony mogą także umieszczać w oknie właściwości. W oknie właściwości wybierz dokument z listy rozwijanej do obciążenia poziomu strony właściwości, które obejmuje `Title` właściwości. Na rysunku 2 przedstawiono okno właściwości po `Title` została ustawiona na "Samouczki strony Master".


![Tytuł w oknie właściwości można skonfigurować za](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Rysunek 02**: tytuł w oknie właściwości można skonfigurować za


### <a name="setting-the-pages-title-programmatically"></a>Ustawienie programowo tytuł strony

Strony wzorcowej `<head runat="server">` znaczników jest przekształcana na [ `HtmlHead` klasy](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmlhead.aspx) wystąpienie podczas renderowania strony przez aparat programu ASP.NET. `HtmlHead` Klasa ma [ `Title` właściwości](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) którego wartość ta jest uwzględniana w renderowanym `<title>` elementu. Ta właściwość jest dostępna z klasy związane z kodem strony ASP.NET za pośrednictwem `Page.Header.Title`; tym samym właściwości są dostępne za pośrednictwem `Page.Title`.

Ćwiczenie ustawienie tytuł strony programowo, przejdź do `About.aspx` kodem strony klasy i utworzyć program obsługi zdarzeń dla strony `Load` zdarzeń. Następnie ustaw tytuł strony "samouczki strony wzorca:: o:: *data*", gdzie *data* jest data bieżąca. Po dodaniu tego kodu użytkownika `Page_Load` obsługi zdarzeń powinien wyglądać podobnie do następującego:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

Rysunek 3 przedstawia paska tytułu w przeglądarce podczas odwiedzania `About.aspx` strony.


![Tytuł strony jest programowane Ustawianie i zawiera bieżącą datę](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Rysunek 03**: tytuł strony jest programowane Ustawianie i zawiera bieżącą datę


## <a name="step-2-automatically-assigning-a-page-title"></a>Krok 2: Automatyczne przypisywanie tytuł strony

Jak widzieliśmy w kroku 1, tytuł strony można ustawić deklaratywnie lub programowo. Jeśli zapomnisz jawnie zmienić do coś bardziej opisowy tytuł strony będzie jednak tytuł domyślny "Bez tytułu strony". W idealnym przypadku tytuł strony będzie miał ustawienie automatycznie dla nas w przypadku, gdy jego wartość nie jest jawnie określona. Na przykład jeśli w czasie wykonywania tytuł strony jest "Bez tytułu strony", firma Microsoft może być tytuł automatycznie aktualizowane do być taka sama jak nazwa pliku strony ASP.NET. Dobre wieści jest to, że z niewielki góry pracy jest możliwe title przypisywane automatycznie.

Wszystkie strony sieci web ASP.NET, pochodzi z `Page` klas w przestrzeni nazw System.Web.UI. `Page` Klasy określa minimalne funkcje wymagane przez stronę ASP.NET i udostępnia podstawowych właściwości, takie jak `IsPostBack`, `IsValid`, `Request`, i `Response`, wśród wielu innych. Często każdej strony w aplikacji sieci web wymaga dodatkowych funkcji lub funkcjonalności. Jest to często stosowana metoda dostarczania to można utworzyć klasy niestandardowej strony podstawowej. Klasy niestandardowej strony podstawowej jest tworzenia klasą pochodzącą z `Page` klasy oraz obejmuje dodatkowe funkcje. Po utworzeniu tej klasy podstawowej, program może pochodzić od niego stron ASP.NET (a nie `Page` klasy), a tym samym oferty rozszerzonej funkcjonalności do stron ASP.NET.

W tym kroku utworzymy strony podstawowej, która automatycznie ustawia tytuł strony do nazwy pliku strony ASP.NET, jeśli tytuł nie inaczej została jawnie ustawiona. Krok 3 analizuje ustawienie tytuł strony oparte na mapy witryny.

> [!NOTE]
> Zbadania tworzenie i używanie niestandardowej strony podstawowej klas wykracza poza zakres tego samouczka serii. Aby uzyskać więcej informacji, przeczytaj [przy użyciu niestandardowych klasy podstawowej dla klas CodeBehind Your stron ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Tworzenie klasy strony podstawowej

Naszym pierwszym zadaniem jest tworzenie klasy strony podstawowej, która jest klasa, która rozszerza `Page` klasy. Rozpocznij od dodania `App_Code` folderu do projektu przez kliknięcie prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wybierając pozycję Dodaj Folder programu ASP.NET, a następnie wybierając `App_Code`. Następnie kliknij prawym przyciskiem myszy `App_Code` folderu i Dodaj nową klasę o nazwie `BasePage.vb`. Na rysunku 4 przedstawiono Solution Explorer po `App_Code` folderu i `BasePage.vb` dodano klasy.


![Dodaj w folderze App_Code oraz klasę o nazwie BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Rysunek 04**: Dodaj `App_Code` Folder i klasę o nazwie`BasePage`


> [!NOTE]
> Program Visual Studio obsługuje dwa tryby zarządzania projektem: witryny sieci Web i projektów aplikacji sieci Web. `App_Code` Folder jest przeznaczony do użycia z modelem projekt witryny sieci Web. Jeśli używany jest model projektu aplikacji sieci Web, umieść `BasePage.vb` klasy w folderze o nazwie coś innego niż `App_Code`, takich jak `Classes`. Aby uzyskać więcej informacji na ten temat, zapoznaj się [Migrowanie projekt witryny sieci Web do projektu aplikacji sieci Web](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).


Ponieważ niestandardowe strony podstawowej służy jako klasa podstawowa dla klasy związane z kodem stron ASP.NET, należy ją rozszerzyć `Page` klasy.


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Zawsze, gdy zostanie zażądana strona ASP.NET wykonywany na kolejnych etapach, skutkując żądanej strony renderowany w kodzie HTML. Firma Microsoft może nacisnąć w etapie przez zastąpienie `Page` klasy `OnEvent` metody. W naszym base stronie umożliwia automatycznie Ustaw tytuł, jeśli go nie został jawnie określony przez `LoadComplete` etap (co, ponieważ użytkownik może mieć odgadnąć, ma miejsce po `Load` etapu).

Aby to zrobić, należy zastąpić `OnLoadComplete` metody, a następnie wprowadź poniższy kod:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

`OnLoadComplete` Metoda uruchamia przez określenie, czy `Title` właściwości nie jeszcze została jawnie ustawiona. Jeśli `Title` właściwość jest `Nothing`, ciąg pusty lub ma wartość "Bez tytułu strony", jest przypisany do nazwy pliku żądanej strony ASP.NET. Ścieżka fizyczna do żądanej strony ASP.NET - `C:\MySites\Tutorial03\Login.aspx`, na przykład - jest dostępny za pośrednictwem `Request.PhysicalPath` właściwości. `Path.GetFileNameWithoutExtension` Używana jest metoda Aby wysunąć tylko części nazwy pliku, a ta nazwa pliku jest przypisywana do `Page.Title` właściwości.

> [!NOTE]
> Zaprosić można zwiększyć tę logikę zwiększające format tytułu. Na przykład, jeśli nazwa pliku strony jest `Company-Products.aspx`, powyższy kod utworzy tytuł "Produkty przedsiębiorstwa", ale najlepiej kreska będą zastąpione spacji, jak "Produktów firmy". Należy również rozważyć dodanie miejsce na zawsze, gdy nastąpiła zmiana wielkości. Oznacza to, należy dodać kodu, który przekształca pliku `OurBusinessHours.aspx` do tytułu z "nasze godzinami pracy".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Ułożenie stron zawartości, dziedziczą z klasy strony podstawowej

Teraz należy zaktualizować stron ASP.NET w naszej witrynie pochodzić od niestandardowe strony podstawowej (`BasePage`) zamiast `Page` klasy. Aby osiągnąć przejdź do każdej klasy związane z kodem i zmienić deklaracji klasy z:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

Do:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

Po wykonaniu tej czynności, odwiedź witrynę za pośrednictwem przeglądarki. Jeśli przejdziesz do witryny tytuł której jawnie jest ustawiona, takich jak `Default.aspx` lub `About.aspx`, jawnie podany tytuł jest używany. Jeśli jednak użytkownik odwiedza witrynę strony, których tytuł nie została zmieniona z domyślnego ("bez tytułu strony"), klasy podstawowej strony Ustawia tytuł, aby nazwa pliku strony.

Rysunek 5. pokazuje `MultipleContentPlaceHolders.aspx` strony podczas wyświetlania za pośrednictwem przeglądarki. Należy pamiętać, że tytuł dokładnie strony filename (mniej rozszerzenia), "MultipleContentPlaceHolders".


[![Jeśli tytuł nie jawnie określone, nazwa pliku strony jest automatycznie używany](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Rysunek 05**: Jeśli tytuł nie jawnie określone, nazwa pliku strony jest automatycznie używany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Krok 3: Utworzenie tytuł strony na mapie witryny

Program ASP.NET oferuje strukturę mapy witryny niezawodne, która umożliwia deweloperom strona Definiowanie mapy witryny hierarchiczna w zewnętrznych zasobów (na przykład tabeli pliku lub bazy danych XML) oraz formanty sieci Web do wyświetlania informacji na temat mapy witryny (na przykład ścieżki mapy witryny, Menu i kontrolki TreeView).

Struktura mapy witryny również można uzyskać programistycznie z klasy związane z kodem strony platformy ASP.NET. W ten sposób firma Microsoft automatycznie Ustaw tytuł strony do tytułu ich odpowiednich węzeł mapy witryny. Ta funkcja pozwala zwiększyć `BasePage` klasy utworzony w kroku 2, dzięki czemu zapewnia tę funkcję. Ale najpierw należy utworzyć mapy witryny.

> [!NOTE]
> W tym samouczku założono, że czytnik jest już znasz ASP. Funkcje mapy witryny w sieci. Aby uzyskać więcej informacji na temat używania mapy witryny, zapoznaj się Moje serii wieloczęściowych artykułu [badanie ASP. NET w lokacji nawigacji](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Tworzenie mapy witryny

System mapy witryny została stworzona [modelu dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), która oddziela mapy witryny interfejsu API z logiki, który serializuje mapy witryny między pamięcią i magazynu trwałego. .NET Framework jest dostarczany z [ `XmlSiteMapProvider` klasy](https://msdn.microsoft.com/en-us/library/system.web.xmlsitemapprovider.aspx), czyli domyślnego dostawcy mapy witryny. Jak jego nazwa wskazuje, `XmlSiteMapProvider` używa pliku XML jako jego magazynu mapy witryny. Teraz Użyj tego dostawcy do definiowania naszych mapy witryny.

Rozpocznij od utworzenia pliku mapy witryny w folderze głównym witryny sieci Web o nazwie `Web.sitemap`. W tym celu kliknij prawym przyciskiem myszy nazwę witryny sieci Web w Eksploratorze rozwiązań, wybierz polecenie Dodaj nowy element, a następnie wybierz szablon mapy witryny. Upewnij się, że plik ma nazwę `Web.sitemap` i kliknij przycisk Dodaj.


[![Dodaj plik o nazwie Web.sitemap folder główny witryny sieci Web](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Rysunek 06**: Dodaj plik o nazwie `Web.sitemap` folder główny witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


Dodaj następujący kod XML `Web.sitemap` pliku:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Plik XML definiuje strukturę mapy witryny hierarchiczna pokazano na rysunku 7.


![Mapa witryny nie jest obecnie składa się z trzech lokacji mapy węzłów](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Rysunek 07**: mapy witryny jest obecnie składa się z trzech lokacji mapy węzłów


Struktura mapy witryny będą aktualizowane w przyszłości samouczki przy wdrażaniu nowych przykłady.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Aktualizowanie strony wzorcowej, aby uwzględnić kontrolki sieci Web

Teraz, gdy mamy mapy witryny sieci Web zdefiniowany teraz zaktualizować strony wzorcowej uwzględnienie kontrolki sieci Web. W szczególności Dodajmy formant ListView do lewej kolumnie w sekcji lekcje, który renderuje Lista nieuporządkowana z elementu listy, dla każdego węzła zdefiniowane mapy witryny.

> [!NOTE]
> ListView — formant jest nowym składnikiem programu ASP.NET w wersji 3.5. Jeśli używasz poprzednich wersji programu ASP.NET, należy użyć kontrolce elementu powtarzanego. Aby uzyskać więcej informacji w formancie ListView, zobacz [ListView i formanty DataPager za pomocą programu ASP.NET 3.5 na](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Rozpocznij od usuwanie istniejącego znacznika Lista nieuporządkowana z sekcji lekcje. Następnie przeciągnij formant ListView z przybornika i upuść ją poniżej wnioski nagłówka. Element ListView znajduje się w sekcji danych przybornika równolegle z innymi kontrolki widoku: GridView widoku DetailsView i FormView. Ustaw element ListView `ID` właściwości `LessonsList`.

W Kreatorze konfiguracji źródła danych wybierz powiązać element ListView do formantu SiteMapDataSource o nazwie `LessonsDataSource`. Formant SiteMapDataSource zwraca strukturę hierarchiczną z systemu mapy witryny.


[![Powiązanie formantu SiteMapDataSource do formantu LessonsList ListView](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Rysunek 08**: powiązanie formantu SiteMapDataSource do formantu ListView LessonsList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


Po utworzeniu formantu SiteMapDataSource, musimy zdefiniować szablonów elementu ListView renderowania Lista nieuporządkowana z elementu listy, dla każdego węzła zwrócony przez formant SiteMapDataSource. Można to zrobić przy użyciu następujących znaczników szablonu:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

`LayoutTemplate` Generuje kod znaczników dla nieuporządkowaną listę (`<ul>...</ul>`) podczas `ItemTemplate` renderuje każdego elementu zwrócony przez SiteMapDataSource jako element listy (`<li>`) zawierającego łącze do konkretnej lekcji.

Po skonfigurowaniu szablonów elementu ListView, odwiedź witrynę sieci Web. Jak pokazano na rysunku nr 9, sekcja — lekcje zawiera pojedynczy element listy punktowane głównej. Gdzie znajdują się informacje i przy użyciu elementu ContentPlaceHolder wiele formantów — lekcje? SiteMapDataSource jest przeznaczony do zwrócenia hierarchiczną zestawu danych, ale formant ListView mogą być wyświetlane tylko jeden poziom w hierarchii. W związku z tym wyświetlane jest tylko pierwszy poziom zwrócony przez SiteMapDataSource węzły mapy witryny.


[![Sekcja — lekcje zawiera pojedynczego elementu listy](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Rysunek 09**: sekcja — lekcje zawiera pojedynczy element listy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


Do wyświetlania na różnych poziomach możemy zagnieździć wielu widokach listy w `ItemTemplate`. Ta technika zostały sprawdzone w [ *stron wzorcowych i nawigacji w witrynie* samouczek](../../data-access/introduction/master-pages-and-site-navigation-vb.md) z mojej [pracy z samouczkiem serii danych](../../data-access/index.md). Jednak dla tej serii samouczek naszych mapy witryny będzie zawierać dwa poziomy: głównej (najwyższego poziomu); i każdej lekcji jako element podrzędny głównej. Zamiast obsługuje tworzenie zagnieżdżonych ListView, firma Microsoft zamiast nakazać SiteMapDataSource, aby nie zwracał węzeł początkowy przez ustawienie jej [ `ShowStartingNode` właściwości](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) do `False`. Net powoduje, że SiteMapDataSource zaczyna się zwracając z drugiej warstwy węzły mapy witryny.

Dzięki tej zmianie ListView Wyświetla punktów na temat i przy użyciu wielu formantów elementu ContentPlaceHolder lekcje, ale pominięto elementu punktor dla strony głównej. Aby rozwiązać ten problem, jawnie dodamy elementu punktor dla strony głównej w `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

Konfigurując SiteMapDataSource, aby pominąć węzeł początkowy, a następnie jawnie dodawania elementu punktor głównej w sekcji — lekcje Wyświetla wyjściowym.


[![Sekcja — lekcje zawiera element punktor w domu i każdego węzła podrzędnego](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Na rysunku nr 10**: sekcja — lekcje zawiera element punktor w domu i każdego węzła podrzędnego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Ustawienie Tytuł oparte na mapy witryny

Z mapy witryny w miejscu, możemy zaktualizować naszych `BasePage` klasy do używania tytułu określony mapy witryny. Jak robiliśmy w kroku 2, będą tylko używać tytułu węzeł mapy witryny, jeśli tytuł strony nie została jawnie ustawiona przez dewelopera strony. Jeśli żądanej strony nie miały jawnie ustawiony tytuł strony i nie został znaleziony w mapy witryny, a następnie firma Microsoft będzie wrócić do korzystania filename żądanej strony (mniej rozszerzenia), jak robiliśmy w kroku 2. Rysunek 11 przedstawia proces tej decyzji.


![W przypadku braku jawnie Ustaw tytuł strony służy tytuł odpowiedniego węzeł mapy witryny](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Rysunek 11**: W przypadku braku jawnie Ustaw tytuł strony służy tytuł odpowiedniego węzeł mapy witryny


Aktualizacja `BasePage` klasy `OnLoadComplete` metodę w celu uwzględnienia następujący kod:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Jak wcześniej `OnLoadComplete` metoda uruchamia przez określenie, czy tytuł strony została jawnie ustawiona. Jeśli `Page.Title` jest `Nothing`, ciągiem pustym lub jest przypisywana wartość "Bez tytułu strony", a następnie kod automatycznie przypisuje wartość do `Page.Title`.

Aby określić tytuł, aby użyć, odwołując zaczyna się kod [ `SiteMap` klasy](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx)w [ `CurrentNode` właściwości](https://msdn.microsoft.com/en-us/library/system.web.sitemap.currentnode.aspx). `CurrentNode`Zwraca [ `SiteMapNode` ](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.aspx) wystąpienia mapy witryny, umożliwiająca obecnie żądanej strony. Zakładając, że obecnie żądanej strony znajduje się w obrębie mapy witryny `SiteMapNode`w `Title` właściwości jest przypisany do tytułu strony. Jeśli obecnie żądana strona nie ma mapy witryny `CurrentNode` zwraca `Nothing` filename żądanej strony służy jako tytuł (jak to zostało zrobione w kroku 2).

Przedstawia rysunek 12 `MultipleContentPlaceHolders.aspx` strony podczas wyświetlania za pośrednictwem przeglądarki. Ponieważ tytułu tej strony nie jest jawnie ustawiona, zamiast tego użyć jej odpowiednie węzeł mapy witryny w tytule.


![Tytuł strony MultipleContentPlaceHolders.aspx są pobierane z mapy witryny](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Rysunek 12**: tytuł strony MultipleContentPlaceHolders.aspx są pobierane z mapy witryny


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Krok 4: Dodawanie innych znaczników specyficznym dla strony do`<head>`sekcji

Kroki 1, 2 i 3 przeglądał dostosowywanie `<title>` element na podstawie strony strona. Oprócz `<title>`, `<head>` może zawierać sekcji `<meta>` elementów i `<link>` elementy. Jak wspomniano wcześniej w tym samouczku `Site.master`w `<head>` sekcja zawiera `<link>` elementu `Styles.css`. Ponieważ to `<link>` w ramach strony wzorcowej jest zdefiniowany element, znajduje się on w `<head>` sekcji dla wszystkich stron zawartości. Jednak jak rozszerzana o dodawaniu `<meta>` i `<link>` elementy na podstawie strony strona?

Najprostszym sposobem dodania specyficznym dla strony zawartości do `<head>` sekcji jest tworzenie formantu elementu ContentPlaceHolder na stronie głównej. Mamy już element ContentPlaceHolder (o nazwie `head`). W związku z tym aby dodać niestandardowe `<head>` znaczników, utworzyć odpowiedni zawartości kontrolki na stronie i umieść kod znaczników.

Aby zilustrować dodawania niestandardowych `<head>` znaczników do strony, obejmują teraz `<meta>` opis elementu do naszej bieżącego zestawu stron zawartości. `<meta>` Opis elementu zawiera krótki opis strony sieci web; Większość wyszukiwarek dołączyć tę informację do jakiegoś podczas wyświetlania wyników wyszukiwania.

A `<meta>` opis element ma następujący format:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Dodaj ten kod znaczników do strony zawartości, należy dodać do formantu zawartości, który jest mapowany na stronie głównej powyżej tekst `head` ContentPlaceHolder. Na przykład, aby zdefiniować `<meta>` opis elementu `Default.aspx`, Dodaj następujący kod:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Ponieważ `head` ContentPlaceHolder nie mieści się w treści strony HTML, znaczników dodane do kontroli zawartości nie jest wyświetlany w widoku Projekt. Aby wyświetlić `<meta>` opisu wizyta element `Default.aspx` za pośrednictwem przeglądarki. Po załadowaniu strony, Wyświetl źródło i należy pamiętać, że `<head>` sekcja zawiera znaczników określony w formancie zawartości.

Poświęć chwilę, aby dodać `<meta>` elementy opis `About.aspx`, `MultipleContentPlaceHolders.aspx`, i `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Programowe Dodawanie znaczników do`<head>`regionu

`head` ContentPlaceHolder pozwala deklaratywnie Dodawanie znaczników niestandardowej strony wzorcowej `<head>` regionu. Niestandardowy kod znaczników mogą być dodawane programowo. Odwołania, który `Page` klasy `Header` zwraca `HtmlHead` zdefiniowane na stronie głównej wystąpienia ( `<head runat="server">`).

Możliwość programowane Dodawanie zawartości do `<head>` region jest przydatne, gdy zawartość do dodania jest dynamiczny. Być może jest ona oparta na użytkownika, odwiedzając stronę; być może jest jest pobierane z bazy danych. Niezależnie od przyczyny, można dodać zawartości do `HtmlHead` przez dodawanie formantów do jego `Controls` kolekcji w następujący sposób:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

Powyższy kod dodaje `<meta>` elementu słowa kluczowe do `<head>` regionu, który zawiera listę słów kluczowych, które opisują strony rozdzielonych przecinkami. Należy pamiętać, że aby dodać `<meta>` tag tworzenia [ `HtmlMeta` ](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmlmeta.aspx) wystąpienia, ustaw jej `Name` i `Content` właściwości, a następnie dodaj go do `Header`w `Controls` kolekcji. Podobnie można dodać programistycznie `<link>` elementu, Utwórz [ `HtmlLink` ](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmllink.aspx) obiekt, ustaw jej właściwości, a następnie dodaj go do `Header`w `Controls` kolekcji.

> [!NOTE]
> Aby dodać dowolny kod znaczników, Utwórz [ `LiteralControl` ](https://msdn.microsoft.com/en-us/library/system.web.ui.literalcontrol.aspx) wystąpienia, należy ustawić jej `Text` właściwości, a następnie dodaj go do `Header`w `Controls` kolekcji.


## <a name="summary"></a>Podsumowanie

W tym samouczku analizujemy na wiele sposobów, aby dodać `<head>` znaczników region na podstawie strony strona. Strona wzorcowa powinna zawierać `HtmlHead` wystąpienia (`<head runat="server">`) z elementu ContentPlaceHolder. `HtmlHead` Wystąpienia umożliwia strony zawartości w celu programowego dostępu `<head>` regionu i deklaratywnie i programowo ustawić stronę jego tytuł; kontrola ContentPlaceHolder umożliwia niestandardowych znaczników, które mają zostać dodane do `<head>` sekcja deklaratywnie za pomocą formantu zawartości.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Dynamicznie ustawienie tytuł strony w programie ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Badanie ASP. Nawigowanie po witrynie przez sieć](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Jak używać tagów HTML Meta](http://searchenginewatch.com/showPage.html?page=2167931)
- [Stron wzorcowych w programie ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Użycie programu ASP.NET 3.5 na ListView i formanty DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Za pomocą niestandardowej klasy podstawowej dla klas CodeBehind stron ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora wielu książek ASP/ASP.NET i twórcę 4GuysFromRolla.com pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott jest osiągalny w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Kowalski Zack i Suchi Banerjee. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Poprzednie](multiple-contentplaceholders-and-default-content-vb.md)
[dalej](urls-in-master-pages-vb.md)
