---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: Określanie tytułu, tagów Meta i innych nagłówków HTML na stronie wzorcowej (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Patrzy na różnych technik do definiowania asortymentach &lt;head&gt; elementów na stronie wzorcowej, z poziomu strony zawartości.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 139fe0f67ddc067e0e23eed99569d7f6de533482
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380710"
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>Określanie tytułu, tagów Meta i innych nagłówków HTML na stronie wzorcowej (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Patrzy na różnych technik do definiowania asortymentach &lt;head&gt; elementów na stronie wzorcowej, z poziomu strony zawartości.


## <a name="introduction"></a>Wprowadzenie

Masz nowe strony wzorcowe, utworzone w programie Visual Studio 2008, domyślnie dwóch kontrolek ContentPlaceHolder: jeden o nazwie `head`i znajduje się w `<head>` elementu; i jeden o nazwie `ContentPlaceHolder1`, umieszczony w obrębie formularza sieci Web. Celem `ContentPlaceHolder1` polega na zdefiniowaniu regionu w formularzu sieci Web, który można dostosować na podstawie strony strona. `head` ContentPlaceHolder umożliwia strony dodać niestandardową zawartość do `<head>` sekcji. (Oczywiście, można zmodyfikować lub usunąć tych dwóch kontrolek ContentPlaceHolder i dodatkowe ContentPlaceHolder mogą być dodawane do strony wzorcowej. Nasze strony wzorcowej `Site.master`, obecnie ma cztery formanty ContentPlaceHolder.)

Kod HTML `<head>` element służy jako repozytorium informacji o dokumencie strony sieci web, który nie jest częścią samego dokumentu. Obejmuje to informacje, takie jak tytuł strony sieci web, informacji używanych przez aparaty wyszukiwania lub wewnętrzny przeszukiwarkami i linki do zasobów zewnętrznych, takich jak źródła danych RSS, JavaScript i CSS plików. Niektóre z tych informacji może być odpowiednie dla wszystkich stron w witrynie sieci Web. Na przykład można globalnie zaimportować te same reguły CSS i pliki JavaScript na każdej stronie ASP.NET. Istnieją jednak części `<head>` elementów, które są specyficzne dla strony. Tytuł strony jest głównym przykładzie.

W tym samouczku sprawdzamy, jak zdefiniować globalnych i specyficznych dla strony `<head>` sekcji znaczników w strony wzorcowej oraz strony z jego zawartością.

## <a name="examining-the-master-pagesheadsection"></a>Badanie strony wzorcowej`<head>`sekcji

Plik strony głównej domyślnego utworzone przez program Visual Studio 2008 zawiera następujący kod w jego `<head>` sekcji:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Należy zauważyć, że `<head>` element zawiera `runat="server"` atrybut, który wskazuje, że jest kontrolką serwera (zamiast Statycznych). Dziedziczyć wszystkie strony ASP.NET [ `Page` klasy](https://msdn.microsoft.com/library/system.web.ui.page.aspx), który znajduje się w `System.Web.UI` przestrzeni nazw. Ta klasa zawiera [ `Header` właściwość](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) zapewniający dostęp do tej strony `<head>` regionu. Za pomocą `Header` właściwość możemy Ustaw tytuł strony ASP.NET lub dodać dodatkową marżę do renderowanej `<head>` sekcji. Jest możliwe, należy dostosować strony zawartości `<head>` elementu przez napisanie fragmentem kodu na stronie `Page_Load` programu obsługi zdarzeń. Sprawdzamy, jak programowo ustawić tytuł strony w kroku 1.

Znaczniki wyświetlane w `<head>` element powyżej obejmuje również formant ContentPlaceHolder o nazwie `head`. Ten formant ContentPlaceHolder nie jest konieczne, jak strony zawartości można dodać niestandardową zawartość do `<head>` element programowo. Jest to przydatne, jednak w sytuacjach, w którym strony zawartości musi dodać statyczny znaczników w celu `<head>` element jako statyczne znaczników można dodać deklaratywne do odpowiedniej kontroli zawartości, a nie programowo.

Oprócz `<title>` elementu i `head` ContentPlaceHolder, strona główna firmy `<head>` element powinien zawierać dowolne `<head>`-poziomu znaczniki, które są wspólne dla wszystkich stron. W naszej witryny sieci Web, wszystkie strony Użyj reguły CSS zdefiniowanych w `Styles.css` pliku. W związku z tym, Zaktualizowaliśmy `<head>` element [ *tworzenie układu dla całej witryny za pomocą stron wzorcowych* ](creating-a-site-wide-layout-using-master-pages-vb.md) samouczka, aby dołączyć odpowiednie `<link>` elementu. Nasze `Site.master` strony wzorcowej w bieżącym `<head>` znaczników znajdują się poniżej.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Krok 1: Ustawianie tytułu strony zawartości

Tytuł strony sieci web jest określony za pośrednictwem `<title>` elementu. Jest to ważne, aby ustawić tytuł każdej strony do odpowiedniej wartości. Podczas wyświetlania strony, jego tytuł jest wyświetlany na pasku tytułu w przeglądarce. Ponadto podczas zakładek stronę, przeglądarek użyć tytuł strony jako sugerowana nazwa zakładki. Ponadto wielu aparatów wyszukiwania Pokaż tytuł strony, podczas wyświetlania wyników wyszukiwania.

> [!NOTE]
> Domyślnie program Visual Studio ustawia `<title>` elementu na stronie głównej "Bez tytułu strony". Podobnie, nowe strony ASP.NET ma ich `<title>` zbyt równa "Bez tytułu Page". Można łatwo zapomnieć o Ustaw tytuł strony do odpowiedniej wartości, dlatego są liczby stron w Internecie o tytule "Strona bez tytułu". Wyszukiwanie Google dla stron sieci web o tym tytule zwraca około 2,460,000 wyników. Nawet firma Microsoft jest podatny na publikowania stron sieci web o tytule "Strona bez tytułu". W momencie pisania tego dokumentu wyszukiwania Google zgłosił 236 tych stron sieci web w domenie Microsoft.com.


Strony ASP.NET można określić jego tytuł w jednym z następujących sposobów:

- Umieszczając wartość bezpośrednio w ramach `<title>` — element
- Za pomocą `Title` atrybutu w `<%@ Page %>` — dyrektywa
- Programowe Ustawianie strony `Title` właściwości przy użyciu kodu, takich jak `Page.Title="title"` lub `Page.Header.Title="title"`.

Zawartość strony nie mają `<title>` elementu, ponieważ jest zdefiniowana na stronie głównej. W związku z tym, aby ustawić tytuł strony zawartości można użyć `<%@ Page %>` dyrektywy `Title` atrybut lub programowo ustawić.

### <a name="setting-the-pages-title-declaratively"></a>Deklaratywne Ustawianie tytuł strony

Tytuł strony zawartości można ustawić sposób deklaratywny za pomocą `Title` atrybutu [ `<%@ Page %>` dyrektywy](https://msdn.microsoft.com/library/ydy4x04a.aspx). Tę właściwość można ustawić przez bezpośrednie zmodyfikowanie `<%@ Page %>` dyrektywy lub w oknie właściwości. Przyjrzyjmy się oba podejścia.

W widoku źródła Znajdź `<%@ Page %>` dyrektywy, który znajduje się na górze oznaczeniu deklaracyjnym strony. `<%@ Page %>` Dyrektywy dla `Default.aspx` poniżej:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

`<%@ Page %>` Dyrektywa określa atrybuty specyficzne dla strony używanego przez aparat programu ASP.NET, gdy analizowaniem i kompilowaniem strony. W tym jego plik strony głównej, lokalizacji jego pliku kodu i jego tytuł, między innymi informacjami.

Domyślnie podczas tworzenia nowej strony zawartości, program Visual Studio ustawia `Title` atrybutu "Bez tytułu Page". Zmiana `Default.aspx`firmy `Title` atrybut z "Bez tytułu Page" do "Samouczków strony Master", a następnie Wyświetl stronę za pośrednictwem przeglądarki. Rysunek 1 pokazuje pasek tytułu w przeglądarce, która odzwierciedla nowy tytuł strony.


![Pojawi się na pasku tytułu w przeglądarce &quot;Master strony samouczki&quot; zamiast &quot;strona bez tytułu&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Rysunek 01**: pasek tytułu w przeglądarce pojawi się na "Master strony samouczki" zamiast "Bez tytułu Page"


Tytuł strony mogą również ustawiać w oknie właściwości. W oknie właściwości wybierz dokument z listy rozwijanej do obciążenia na poziomie strony właściwości, które obejmuje `Title` właściwości. Na rysunku 2 przedstawiono w oknie właściwości po `Title` została ustawiona na "Samouczki strony Master".


![Możesz skonfigurować zbyt tytuł z okna właściwości](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Rysunek 02**: tytuł w oknie właściwości można skonfigurować za


### <a name="setting-the-pages-title-programmatically"></a>Programowe Ustawianie tytuł strony

Strony wzorcowej `<head runat="server">` znaczników jest tłumaczony na [ `HtmlHead` klasy](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) wystąpienie, gdy strona jest renderowany przez aparat programu ASP.NET. `HtmlHead` Klasa ma [ `Title` właściwość](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) którego wartość jest odzwierciedlana w renderowanym `<title>` elementu. Ta właściwość jest dostępna z klasy CodeBehind strony ASP.NET za pomocą `Page.Header.Title`; te właściwości można także uzyskać dostęp za pośrednictwem `Page.Title`.

Praktyczne, ustawienie tytuł strony programowo, przejdź do `About.aspx` strony związane z kodem klasę i utworzyć program obsługi zdarzeń dla strony `Load` zdarzeń. Następnie ustaw tytuł strony "Master strony samouczki:: o:: *data*", gdzie *data* jest bieżąca data. Po dodaniu tego kodu usługi `Page_Load` programu obsługi zdarzeń powinny wyglądać podobnie do następujących:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

Rysunek 3 przedstawia pasek tytułu w przeglądarce, gdy użytkownik odwiedzi `About.aspx` strony.


![Tytuł strony jest programowe Ustawianie i zawiera bieżącą datą](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Rysunek 03**: tytuł strony jest programowe Ustawianie i zawiera bieżącą datą


## <a name="step-2-automatically-assigning-a-page-title"></a>Krok 2: Automatyczne przypisywanie tytuł strony

Jak widzieliśmy w kroku 1, można ustawić tytuł strony deklaratywne lub programowo. Jeśli nie pamiętasz jawnie Zmień tytuł na coś bardziej opisowe, strona będzie jednak tytuł domyślny "Bez tytułu Page". W idealnym przypadku tytuł strony będzie miał ustawienie automatycznie dla nas w przypadku, gdy firma Microsoft nie jawnie określić jego wartość. Na przykład jeśli w środowisku uruchomieniowym tytuł strony jest "Bez tytułu Page", może być chcemy mieć tytuł automatycznie aktualizowany, aby być taka sama jak nazwa pliku strony ASP.NET. Dobra wiadomość jest taka, wraz z trochę więcej pracy ponoszonych z góry, którą można mieć tytuł przypisywany.

Wszystkich stron ASP.NET web pages pochodzić od `Page` klasy w przestrzeni nazw System.Web.UI. `Page` Klasa definiuje minimalne funkcje wymagane przez strony ASP.NET i udostępnia podstawowe właściwości, takie jak `IsPostBack`, `IsValid`, `Request`, i `Response`, wiele innych. Często każdej strony w aplikacji sieci web wymaga dodatkowych funkcji lub funkcjonalności. Często stosowaną metodą zapewnienia to jest do utworzenia klasy niestandardowej strony podstawowej. Klasa niestandardowa strona podstawowa jest należy utworzyć klasę pochodzącą od `Page` klasy i zawiera dodatkowe funkcje. Po utworzeniu tej klasy bazowej może mieć dziedziczyć po nim stron ASP.NET (a nie `Page` klasy), a tym samym oferuje rozszerzoną funkcjonalność do stron ASP.NET.

W tym kroku utworzymy strony podstawowej, która automatycznie ustawia tytuł strony do strony ASP.NET, nazwa_pliku, jeśli tytuł nie w przeciwnym razie została jawnie ustawiona. Krok 3 patrzy na ustawienie tytuł strony, w oparciu o mapy witryny.

> [!NOTE]
> Tworzenie i używanie niestandardowej strony podstawowej klasy głębszego zbadania wykracza poza zakres tej serii samouczków. Aby uzyskać więcej informacji, przeczytaj [przy użyciu klasy podstawowej niestandardowe dla klasy CodeBehind Your strony ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Tworzenie klasy strony podstawowej

Naszym pierwszym zadaniem jest tworzenie klasy strony podstawowej, która jest klasą, która rozszerza `Page` klasy. Rozpocznij od dodania `App_Code` folderu do projektu, kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wybierając Dodaj Folder programu ASP.NET, a następnie wybierając `App_Code`. Następnie kliknij prawym przyciskiem myszy `App_Code` folderze i Dodaj nową klasę o nazwie `BasePage.vb`. Rysunek 4 przedstawia Solution Explorer po `App_Code` folder i `BasePage.vb` klasy zostały dodane.


![Dodaj folderze App_Code oraz klasę o nazwie BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Rysunek 04**: Dodaj `App_Code` Folder i klasę o nazwie `BasePage`


> [!NOTE]
> Program Visual Studio obsługuje dwa tryby zarządzania projektami: projektów witryny sieci Web i projektów aplikacji sieci Web. `App_Code` Folderu jest przeznaczony do użycia z modelem projektu witryny sieci Web. Jeśli używasz modelu projektu aplikacji sieci Web, należy umieścić `BasePage.vb` klasy w folderze o nazwie coś innego niż `App_Code`, takich jak `Classes`. Aby uzyskać więcej informacji na ten temat, zobacz [migracji projektu witryny sieci Web do projektu aplikacji sieci Web](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).


Ponieważ niestandardowe strony podstawowej służy jako klasa podstawowa dla klasy CodeBehind strony ASP.NET, należy ją rozszerzyć `Page` klasy.


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Przy każdym żądaniu strony ASP.NET przechodzi przez kolejne etapy, skutkując żądanej strony są renderowane w kodzie HTML. Firma Microsoft może korzystać przez zastąpienie etapu `Page` klasy `OnEvent` metody. Dla naszych podstawowego strony umożliwia ustawianie tytułu automatycznie, jeśli go nie został jawnie określony przez `LoadComplete` etapu (, ponieważ użytkownik może mieć złamać, występuje po `Load` etapu).

Aby to osiągnąć, należy zastąpić `OnLoadComplete` metody i wprowadź następujący kod:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

`OnLoadComplete` Metoda uruchamia się przez określenie, czy `Title` właściwości nie jeszcze została jawnie ustawiona. Jeśli `Title` właściwość `Nothing`, ciąg pusty lub ma wartość "Bez tytułu Page", jest przypisany do nazwy pliku żądanej strony ASP.NET. Ścieżka fizyczna do żądanej strony ASP.NET - `C:\MySites\Tutorial03\Login.aspx`, na przykład — jest dostępna za pośrednictwem `Request.PhysicalPath` właściwości. `Path.GetFileNameWithoutExtension` Metoda jest używana, aby wysunąć tylko część nazwy pliku, a ta nazwa pliku jest przypisywany do `Page.Title` właściwości.

> [!NOTE]
> Zapraszam WAS do zwiększenia tę logikę, aby poprawić format tytuł. Na przykład, jeśli nazwa pliku strony `Company-Products.aspx`, powyższy kod generuje tytuł "Firmy produkty", ale najlepiej kreska zostanie zamienione spację, tak jak "Produkty firmy". Należy również rozważyć dodanie spację, zawsze wtedy, gdy nie nastąpiła zmiana wielkości liter. Oznacza to, należy rozważyć dodanie kod, który przekształca, nazwa_pliku `OurBusinessHours.aspx` tytuł elementu "nasze godzinami".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Ułożenie stron zawartości, dziedziczą z klasy bazowej strony

Teraz musimy zaktualizować strony ASP.NET w naszej witrynie, aby dziedziczyć niestandardowe strony podstawowej (`BasePage`) zamiast `Page` klasy. Aby osiągnąć przejdź do każdej klasy związane z kodem i zmień deklarację klasy z:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

Do:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

Po wykonaniu tej czynności, odwiedź witrynę za pośrednictwem przeglądarki. Jeśli odwiedź stronę, na których tytuł jawnie ustawiono, takich jak `Default.aspx` lub `About.aspx`, jest używany jawnie określonego tytułu. Jeśli jednak użytkownik odwiedza strony, których tytuł nie została zmieniona z domyślnego ("bez tytułu Page"), klasy podstawowej strony Ustawia tytuł strony, nazwa_pliku.

Rysunek 5. pokazuje `MultipleContentPlaceHolders.aspx` stronie podczas przeglądania za pośrednictwem przeglądarki. Należy pamiętać, że tytuł dokładnie strony filename (mniej rozszerzenia), "MultipleContentPlaceHolders".


[![Jeśli tytuł nie jawnie określone, nazwa_pliku strony jest używane automatycznie](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Rysunek 05**: Jeśli tytuł nie jawnie określone, nazwa_pliku strony jest używane automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Krok 3: Bazowanie tytuł strony na mapy witryny

Program ASP.NET oferuje architekturę mapy niezawodne witryny, która umożliwia programistom stron do definiowania mapy hierarchiczne witryny sieci Web w zewnętrznego zasobu (np. tabeli pliku lub bazy danych XML) wraz z formantów sieci Web do wyświetlania informacji na temat mapy witryny (na przykład SiteMapPath, Menu i kontrolki TreeView).

Struktura mapy witryny również są dostępne programowo z klasy CodeBehind strony ASP.NET. W ten sposób firma Microsoft automatycznie Ustaw tytuł strony do tytułu jego odpowiedniego węzła mapy witryny. Możemy poprawić `BasePage` klasy utworzone w kroku 2, dzięki czemu oferuje tę funkcję. Ale najpierw należy utworzyć mapy witryny sieci Web dla naszej witrynie.

> [!NOTE]
> W tym samouczku przyjęto założenie, że czytelnik jest już znasz ASP. Funkcje mapy witryny w sieci. Aby uzyskać więcej informacji na temat korzystania z mapy witryny, zapoznaj się z moich wieloczęściową serię artykułów, [badanie ASP. NET w lokacji nawigacji](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Tworzenie mapy witryny

System mapy witryny jest kompilowanych [modelu dostawca](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), który oddziela mapy witryny interfejsu API od logiki, który serializuje informacji mapy witryny między pamięci i magazynu trwałego. .NET Framework, który jest dostarczany z [ `XmlSiteMapProvider` klasy](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), czyli dostawcy mapy witryny domyślnej. Jak sugeruje jej nazwa, `XmlSiteMapProvider` używa pliku XML jako jego magazynu mapy witryny. Definiowanie nasza Mapa witryny, użyjemy tego dostawcy.

Rozpocznij od utworzenia pliku mapy witryny w folderze głównym witryny sieci Web o nazwie `Web.sitemap`. W tym celu kliknij prawym przyciskiem myszy nazwę witryny sieci Web w Eksploratorze rozwiązań, wybierz pozycję Dodaj nowy element, a następnie wybierz szablon mapy witryny. Upewnij się, że plik ma nazwę `Web.sitemap` i kliknij przycisk Dodaj.


[![Dodaj plik o nazwie Web.sitemap folder główny witryny sieci Web](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Rysunek 06**: Dodaj plik o nazwie `Web.sitemap` folder główny witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


Dodaj następujący kod XML do `Web.sitemap` pliku:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Plik XML definiuje strukturę mapy witryny hierarchiczne pokazano na rysunku 7.


![Mapa witryny nie jest obecnie składa się z trzech mapy węzłach lokacji](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Rysunek 07**: mapy witryny jest obecnie składa się z trzech mapy węzłach lokacji


Struktura mapy witryny będą aktualizowane w przyszłości samouczkach ponieważ nieustannie dodajemy nowe przykłady.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Aktualizowanie strony wzorcowej, aby uwzględnić kontrolki sieci Web

Teraz, gdy mamy już mapy witryny sieci Web zdefiniowany, zaktualizuj strony wzorcowej, aby uwzględnić kontrolki sieci Web. W szczególności Dodajmy kontrolki ListView do lewej kolumnie w sekcji lekcje, która renderuje nieuporządkowaną listę przy użyciu elementu listy dla każdego węzła zdefiniowane w mapy witryny.

> [!NOTE]
> Kontrolka ListView jest nowym składnikiem programu ASP.NET w wersji 3.5. Jeśli używasz poprzedniej wersji programu ASP.NET, należy użyć w kontrolce elementu powtarzanego. Aby uzyskać więcej informacji w kontrolce ListView, zobacz [ListView przy użyciu ASP.NET 3.5 firmy i formanty DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Rozpocznij od usuwanie istniejących znaczników listę nieuporządkowaną z sekcji lekcje. Następnie przeciągnij kontrolce ListView z przybornika i upuść go w następnej lekcji nagłówka. ListView znajduje się w sekcji danych przybornika, wraz z innymi kontrolki widoku: GridView DetailsView i FormView. Ustaw ListView `ID` właściwość `LessonsList`.

Z Kreatora konfiguracji źródła danych wybierz powiązać formant SiteMapDataSource o nazwie ListView `LessonsDataSource`. Formant SiteMapDataSource zwraca strukturę hierarchiczną z systemu lokacji w mapie.


[![Powiąż formant SiteMapDataSource kontrolki ListView LessonsList](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Rysunek 08**: powiązanie formantu SiteMapDataSource do formantu ListView LessonsList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


Po utworzeniu kontrolki SiteMapDataSource, należy zdefiniować szablony ListView renderowaniu nieuporządkowaną listę przy użyciu elementu listy dla każdego węzła zwracany przez kontrolę SiteMapDataSource. Można to zrobić za pomocą następujących znaczników szablonu:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

`LayoutTemplate` Wygeneruje kod znaczników dla nieuporządkowaną listę (`<ul>...</ul>`) podczas `ItemTemplate` renderuje każdego elementu zwróconego przez SiteMapDataSource jako element listy (`<li>`) zawierającego łącze do określonej lekcji.

Po skonfigurowaniu szablonów ListView, odwiedź witrynę internetową. Jak pokazano na rysunku nr 9, w sekcji — lekcje zawiera pojedynczy element punktowanej głównej. Gdzie są informacje i za pomocą kontrolek ContentPlaceHolder wielu lekcje? SiteMapDataSource jest przeznaczony do zwrócenia hierarchicznego zestawu danych, ale kontrolki ListView mogą być wyświetlane tylko jeden poziom w hierarchii. W związku z tym tylko pierwszy poziom węzłów mapy witryny zwrócony przez SiteMapDataSource jest wyświetlany.


[![Sekcja lekcje zawiera pojedynczego elementu listy](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Rysunek 09**: sekcja lekcje zawiera pojedynczy element listy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


Do wyświetlania na różnych poziomach możemy zagnieździć wielu ListViews w ramach `ItemTemplate`. Ta technika zostały sprawdzone w [ *strony wzorcowe i nawigacja w witrynie* samouczek](../../data-access/introduction/master-pages-and-site-navigation-vb.md) z moich [Praca z serii samouczków danych](../../data-access/index.md). Jednak w tej serii samouczków nasza Mapa witryny będzie zawierać dwa poziomy: Strona główna (najwyższego poziomu); a Każda lekcja jako element podrzędny głównej. Zamiast tworzenia zagnieżdżonych ListView, firma Microsoft zamiast nakazać SiteMapDataSource nie zwracać węzeł początkowy, ustawiając jego [ `ShowStartingNode` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) do `False`. Efektem sieciowym jest że SiteMapDataSource zaczyna się, zwracając Druga warstwa węzłów mapy witryny.

Dzięki tej zmianie ListView Wyświetla elementy punktor, aby uzyskać informacje, a za pomocą wiele kontrolek ContentPlaceHolder lekcje, ale pomija element punktora dla strony głównej. Aby rozwiązać ten problem, firma Microsoft jawnie dodać punktowany dla strony głównej w `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

Konfigurując SiteMapDataSource, aby pominąć węzeł początkowy i jawne dodanie elementu punktor głównej w sekcji lekcje Wyświetla zamierzonych danych wyjściowych.


[![W sekcji lekcje zawiera element punktor w domu i każdy węzeł podrzędny](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Na rysunku nr 10**: sekcja lekcje zawiera element punktor w domu i każdy węzeł podrzędny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Ustawianie tytułu oparte na mapy witryny

Za pomocą mapy witryny w miejscu, możemy zaktualizować naszych `BasePage` klasy tytuł określony mapy witryny. Ile My mieliśmy w kroku 2 ma być uruchamiany tylko do użycia tytuł węzeł mapy witryny, jeśli tytuł strony nie została jawnie ustawiona przez dewelopera strony. Jeśli trwa żądanej strony nie ma jawnie ustawionej tytuł strony i nie zostanie znaleziony w Mapa witryny, a następnie firma Microsoft będzie wrócić do korzystania filename żądanej strony (mniej rozszerzenia), ile My mieliśmy w kroku 2. Rysunek 11 ilustruje ten proces.


![W przypadku braku jawnie Ustaw tytuł strony zostanie użyty tytuł odpowiedni węzeł mapy witryny](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Rysunek 11**: W przypadku braku jawnie Ustaw tytuł strony zostanie użyty tytuł odpowiedni węzeł mapy witryny


Aktualizacja `BasePage` klasy `OnLoadComplete` metodę, aby zawrzeć następujący kod:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Tak jak poprzednio `OnLoadComplete` metoda uruchamia się po określeniu, czy tytuł strony została jawnie ustawiona. Jeśli `Page.Title` jest `Nothing`, ciąg pusty lub jest przypisywana wartość "Bez tytułu Page", a następnie kod automatycznie przypisuje wartość do `Page.Title`.

Aby ustalić, tytuł, aby używać, odwołując się do zaczyna się kod [ `SiteMap` klasy](https://msdn.microsoft.com/library/system.web.sitemap.aspx)firmy [ `CurrentNode` właściwość](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx). `CurrentNode` Zwraca [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) wystąpienie mapy witryny, która odpowiada aktualnie żądanej strony. Zakładając, że obecnie żądanej strony znajduje się w obrębie mapy witryny `SiteMapNode`firmy `Title` właściwość jest przypisana do tytułu strony. Jeśli obecnie żądana strona nie jest mapa witryny `CurrentNode` zwraca `Nothing` i żądanej strony, nazwa_pliku służy jako tytuł (jak zostało to wykonane w kroku 2).

Przedstawia rysunek 12 `MultipleContentPlaceHolders.aspx` stronie podczas przeglądania za pośrednictwem przeglądarki. Ponieważ nie ustawiono jawnie tytułu tej strony, w zamian jest używana jego odpowiedni węzeł mapy witryny w tytule.


![Tytuł strony MultipleContentPlaceHolders.aspx są pobierane z mapy witryny](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Rysunek 12**: tytuł strony MultipleContentPlaceHolders.aspx są pobierane z mapy witryny


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Krok 4: Dodanie innych znaczników dla strony, aby`<head>`sekcji

Kroki 1, 2 i 3 przyjrzano dostosowywanie `<title>` elementu na podstawie strony strona. Oprócz `<title>`, `<head>` sekcja może zawierać `<meta>` elementy i `<link>` elementów. Jak wspomniano wcześniej w tym samouczku `Site.master`firmy `<head>` sekcja zawiera `<link>` elementu `Styles.css`. Ponieważ to `<link>` element jest zdefiniowany w obrębie strony wzorcowej, znajduje się w `<head>` sekcji dla wszystkich stron zawartości. Ale jak przejdziemy o dodawaniu `<meta>` i `<link>` elementów na podstawie strony strona?

Najprostszym sposobem dodania zawartości dla strony `<head>` sekcji jest utworzenie kontrolki ContentPlaceHolder na stronie głównej. Mamy już takie ContentPlaceHolder (o nazwie `head`). W związku z tym aby dodać niestandardowe `<head>` znaczników, utworzenie odpowiedniego formantu na stronie zawartości i umieścić znaczniki istnieje.

Aby zilustrować dodawania niestandardowego `<head>` znaczników do strony, obejmują teraz `<meta>` description — element do naszej bieżący zestaw stron zawartości. `<meta>` Description — element zawiera krótki opis strony sieci web; Większość wyszukiwarek Zastosuj te informacje w pewnej postaci, podczas wyświetlania wyników wyszukiwania.

A `<meta>` description — element ma następującą postać:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Aby dodać ten kod znaczników do strony zawartości, Dodaj powyżej tekst do formantu zawartości, który jest mapowany do strony wzorcowej `head` ContentPlaceHolder. Na przykład, aby zdefiniować `<meta>` description — element dla `Default.aspx`, Dodaj następujący kod:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Ponieważ `head` ContentPlaceHolder nie mieści się w treści strony HTML, znaczniki dodane do kontroli zawartości nie jest wyświetlana w widoku Projekt. Aby wyświetlić `<meta>` opis elementu odwiedziny `Default.aspx` za pośrednictwem przeglądarki. Po załadowaniu strony, Wyświetl źródło i zwróć uwagę, że `<head>` sekcja zawiera określone w formancie zawartości kodu znaczników.

Poświęć chwilę, aby dodać `<meta>` opis elementów `About.aspx`, `MultipleContentPlaceHolders.aspx`, i `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Programowe Dodawanie znaczników do`<head>`regionu

`head` ContentPlaceHolder pozwala nam na deklaratywne Dodawanie znaczników niestandardowej strony wzorcowej `<head>` regionu. Programowo z również mogą być dodawane do niestandardowych znaczników. Pamiętamy `Page` klasy `Header` właściwość zwraca `HtmlHead` zdefiniowanego na stronie głównej wystąpienia ( `<head runat="server">`).

Możliwość programowe Dodawanie zawartości do `<head>` region jest przydatne w przypadku, gdy zawartość do dodania są dynamiczne. Być może jest on oparty na użytkownika, odwiedzając stronę; może to są pobierane z bazy danych. Niezależnie od przyczyny, można dodać zawartości do `HtmlHead` poprzez dodawanie formantów do jego `Controls` kolekcji w następujący sposób:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

Powyższy kod dodaje `<meta>` keywords — element do `<head>` regionu, który zawiera listę słów kluczowych, które opisują strony rozdzielonych przecinkami. Należy pamiętać, że można dodać `<meta>` tag, możesz utworzyć [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) wystąpienia, ustaw jego `Name` i `Content` właściwości, a następnie dodaj go do `Header`firmy `Controls` kolekcji. Podobnie Aby programowo dodać `<link>` elementu, Utwórz [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) obiektu, ustaw jego właściwości, a następnie dodaj go do `Header`firmy `Controls` kolekcji.

> [!NOTE]
> Aby dodać dowolne znaczników, Utwórz [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) wystąpienia, ustaw jego `Text` właściwości, a następnie dodaj go do `Header`firmy `Controls` kolekcji.


## <a name="summary"></a>Podsumowanie

W tym samouczku przyjrzeliśmy się na różne sposoby, aby dodać `<head>` znaczników regionie na podstawie strony strona. Strona wzorcowa powinna zawierać `HtmlHead` wystąpienia (`<head runat="server">`) przy użyciu ContentPlaceHolder. `HtmlHead` Wystąpienie umożliwia strony z zawartością programistycznie uzyskują dostęp `<head>` region i tytułu w sposób deklaratywny i programowy ustawienie strony; kontrola ContentPlaceHolder umożliwia niestandardowych znaczników, które mają zostać dodane do `<head>` sekcja deklaratywnie przez kontrolkę zawartości.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Dynamiczne Ustawianie tytuł strony w programie ASP.NET:](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Badanie ASP. Nawigacja po witrynie NET firmy](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Jak używać tagów HTML Meta](http://searchenginewatch.com/showPage.html?page=2167931)
- [Strony wzorcowe na platformie ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Za pomocą programu ASP.NET 3.5 w ListView i formanty DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Za pomocą niestandardowej klasy bazowej dla klas związanym z kodem strony ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu ASP/ASP.NET książki i założyciel 4GuysFromRolla.com pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Zack Jones i Suchi Banerjee. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](multiple-contentplaceholders-and-default-content-vb.md)
> [dalej](urls-in-master-pages-vb.md)
