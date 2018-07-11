---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Strony wzorcowe i nawigacja w witrynie (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Jedną wspólną cechą witryn sieci Web, przyjazny dla użytkownika jest, że schemat spójne, obejmujące całą lokację strony układu i nawigacji. W tym samouczku pokazano, jak y...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: 0ad86398122f3a47721bda689b7f5afc706047bc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816499"
---
<a name="master-pages-and-site-navigation-c"></a>Strony wzorcowe i nawigacja w witrynie (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) lub [Pobierz plik PDF](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Jedną wspólną cechą witryn sieci Web, przyjazny dla użytkownika jest, że schemat spójne, obejmujące całą lokację strony układu i nawigacji. W tym samouczku przegląda od tego, jak utworzyć spójny wygląd i zachowanie we wszystkich stron, które można łatwo aktualizować.


## <a name="introduction"></a>Wprowadzenie

Jedną wspólną cechą witryn sieci Web, przyjazny dla użytkownika jest, że schemat spójne, obejmujące całą lokację strony układu i nawigacji. Program ASP.NET 2.0 wprowadzono dwie nowe funkcje, które znacznie ułatwiają wdrażanie zarówno strony z normalnej nawigacji i układ schemat: strony wzorcowe i nawigacja lokacji. Strony wzorcowe umożliwiają deweloperom tworzenie szablonu całej witryny za pomocą wyznaczonym edytowalnych. Następnie można zastosować ten szablon do stron ASP.NET w witrynie. Tego rodzaju strony ASP.NET wystarczy zdefiniować zawartości dla strony wzorcowej w określonych regionach można edytować innych znaczników na stronie głównej jest taka sama na wszystkich stronach ASP.NET, korzystających z strony wzorcowej. Ten model umożliwia deweloperom definiowanie i scentralizować układ strony całej lokacji, co ułatwia tworzenie spójny wygląd i zachowanie we wszystkich stron, które można łatwo aktualizować.

[Nawigacji systemu lokacji](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) zawiera zarówno mechanizm programistom stron do definiowania mapy witryny sieci Web i interfejsów API dla tej mapy witryny programowo być badana. Nowe kontrolki sieci Web nawigacji, które Menu widoku drzewa i SiteMapPath ułatwiają renderować całości lub części mapy witryny w typowych nawigacji elementu interfejsu użytkownika. Będziemy używać domyślnego dostawcę usługi site nawigacji, co oznacza, że nasza Mapa witryny są definiowane w pliku w formacie XML.

Aby zilustrować te pojęcia i naszej witryny sieci Web samouczki bardziej użyteczne, Poświęć w tej lekcji Definiowanie układu strony obejmujące całą lokację, implementowanie mapy witryny sieci Web i dodawanie nawigacji interfejsu użytkownika. Do końca tego samouczka odpowiemy na projekt dopracowane witryny sieci Web do tworzenia Nasz samouczek stron sieci web.


[![Wynik końcowy po ukończeniu tego samouczka](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Rysunek 1**: zakończenia wynik z tego samouczka ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>Krok 1: Tworzenie strony wzorcowej

Pierwszym krokiem jest utworzenie strony wzorcowej dla tej witryny. Teraz naszej witryny sieci Web składa się tylko wpisany zestaw danych (`Northwind.xsd`w `App_Code` folderu), klasy LOGIKI (`ProductsBLL.cs`, `CategoriesBLL.cs`i tak dalej w `App_Code` folderu), bazy danych (`NORTHWND.MDF`w `App_Data` folder), plik konfiguracyjny (`Web.config`), a plik arkusza stylów CSS (`Styles.css`). Po usunięciu te strony i pliki ukazujące przy użyciu warstwy DAL i LOGIKI od pierwszych dwóch samouczków, ponieważ firma Microsoft będzie można Rewidowanie tych przykładów większej liczby szczegółów przyszłych samouczków.


![Pliki w projekcie](master-pages-and-site-navigation-cs/_static/image4.png)

**Rysunek 2**: pliki w projekcie


Aby utworzyć stronę wzorcową, kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz polecenie Dodaj nowy element. Następnie wybierz typ strony wzorcowej, z listy szablonów i nadaj mu nazwę `Site.master`.


[![Dodaj nową stronę wzorcową do witryny sieci Web](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Rysunek 3**: Dodaj nową stronę wzorcową do witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image7.png))


Na stronie głównej, należy zdefiniować tutaj układu strony całej lokacji. Możesz użyć widoku projektu i dodawanie kontrolek niezależnie od układu lub sieci Web, należy, lub można ręcznie dodawać znaczniki ręcznie w widoku źródła. Na stronie wzorcowej w programie używam [kaskadowych arkuszy stylów](http://www.w3schools.com/css/default.asp) pozycjonowanie i style za pomocą ustawień CSS zdefiniowanych w pliku zewnętrznym `Style.css`. Gdy nie można odróżnić od znaczników, pokazano poniżej, reguły CSS są zdefiniowane tak, aby nawigacji `<div>`firmy zawartości są pozycjonowane absolutnie, który pojawia się po lewej stronie i ma stałą szerokość 200 pikseli.

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Strona wzorcowa definiuje układ stron statycznych i regiony, które mogą być edytowane przez strony ASP.NET, które używają strony wzorcowej. Te obszary można edytować zawartości są wskazywane przez kontrolki ContentPlaceHolder, które są widoczne w zawartości `<div>`. Nasze strony wzorcowej zawiera pojedynczy ContentPlaceHolder (`MainContent`), ale strona wzorcowa może mieć wiele kontrolek ContentPlaceHolder.

Ze znacznikami podanymi powyżej przełączanie do widoku projektu zawiera układ strony wzorcowej. Wszystkie strony ASP.NET, które używają tej strony wzorcowej będzie miał ten jednolity układ i możliwość określenia znaczniki dla `MainContent` regionu.


[![Strona wzorcowa, podczas wyświetlania za pośrednictwem widoku projektu](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Rysunek 4**: strona wzorcowa, podczas wyświetlania za pośrednictwem widoku projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>Krok 2: Dodawanie określonej strony głównej witryny sieci Web

Ze stroną wzorcową zdefiniowane możemy przystąpić do dodawania stron ASP.NET dla witryny sieci Web. Zacznijmy od dodania `Default.aspx`, strony głównej naszej witryny sieci Web. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz polecenie Dodaj nowy element. Wybierz opcję formularz sieci Web z listy szablonów, a nazwa pliku `Default.aspx`. Ponadto zaznacz pole wyboru "Wybierz stronę wzorcową".


[![Dodaj nowy formularz sieci Web, sprawdzanie, zaznacz pole wyboru strony wzorcowej](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Rysunek 5**: Dodaj nowy formularz sieci Web, sprawdzanie, zaznacz pole wyboru strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image13.png))


Po kliknięciu przycisku OK, firma Microsoft jest wyświetlony monit o wybranie jakie strony wzorcowej, skorzystaj z tej nowej strony programu ASP.NET. Kiedy masz wiele stron wzorcowych w projekcie, mamy tylko jeden.


[![Wybierz stronę wzorzec, do której należy używać tej strony ASP.NET](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Rysunek 6**: Wybierz na stronie wzorcowej to zastosowanie powinien strony ASP.NET ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image16.png))


Po wybraniu strony wzorcowej, nowych stron ASP.NET będzie zawierać następujące znaczniki:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

W `@Page` dyrektywa jest odwołanie do pliku strony wzorcowej używane (`MasterPageFile="~/Site.master"`), i znaczników strony ASP.NET zawiera kontrolkę zawartości dla wszystkich kontrolek ContentPlaceHolder zdefiniowane na stronie głównej, przy użyciu formantu `ContentPlaceHolderID` Mapowanie zawartości do określonych ContentPlaceHolder kontrolować. Formant zawartości jest, gdzie umieścić znaczniki mają być wyświetlane w odpowiedniej ContentPlaceHolder. Ustaw `@Page` dyrektywy `Title` atrybutu do strony głównej i dodamy powitalnego zawartość do formantu zawartości:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

`Title` Atrybutu w `@Page` dyrektywy pozwala ustawić tytuł strony ze strony programu ASP.NET, nawet jeśli `<title>` jest zdefiniowany element na stronie głównej. Firma Microsoft można również ustawić tytuł programowo, za pomocą `Page.Title`. Należy również zauważyć, że odwołania strony wzorcowej do arkuszy stylów (takie jak `Style.css`) są automatycznie aktualizowane, tak aby działają w dowolnej strony ASP.NET, niezależnie od tego, jakie katalogu, strona ASP.NET znajduje się w względem strony wzorcowej.

Przełączanie do widoku projektu, że okaże się, jak wygląda naszą stronę w przeglądarce. Należy zauważyć, że w projekcie wyświetlania dla strony ASP.NET, czy można edytować tylko zawartości edytowalnych znaczników ContentPlaceHolder innego niż zdefiniowane w strony wzorcowej jest wyszarzona.


[![Widok projektu strony ASP.NET zawiera edytowalne i nieedytowalne regionów](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Rysunek 7**: widok projektu ASP.NET strony zawiera zarówno edytowalna i regiony bez edytowalna ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image19.png))


Gdy `Default.aspx` strony jest kontrolowane przez przeglądarkę, automatycznie aparatu ASP.NET Scala zawartość strony wzorcowej strony i ASP. NET zawartości i renderuje zawartość scalone do końcowego kodu HTML, który jest wysyłane do przeglądarki. Po zaktualizowaniu zawartości strony wzorcowej wszystkie strony ASP.NET, które używają tej strony wzorcowej będzie ich zawartość remerged przy użyciu nowej strony wzorcowej zawartości podczas następnego żądania. Krótko mówiąc, model strony wzorcowej pozwala na jednej stronie szablon układu, aby być zdefiniowane (strony wzorcowej) której zmiany są natychmiast odzwierciedlane w całej lokacji.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Dodawanie stron ASP.NET dodatkowe do witryny sieci Web

Poświęćmy chwilę, aby dodać dodatkowe wycinków strony ASP.NET do lokacji, który ostatecznie będzie przechowywać różne pokazy raportowania. Będzie istnieć więcej niż 35 pokazy łącznie, więc zamiast tworzenia wszystkie strony klasy zastępczej teraz po prostu utworzyć pierwsze kilka. Ponieważ również będzie wiele kategorii pokazy, pokazy w lepszym zarządzaniu dodać folder dla kategorii. Teraz, należy dodać następujące trzy foldery:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Na koniec należy dodać nowe pliki, jak pokazano w Eksploratorze rozwiązań na rysunku 8. Podczas dodawania każdego pliku, pamiętaj, aby zaznaczyć pole wyboru "Wybierz stronę wzorcową".


![Dodaj poniższe pliki](master-pages-and-site-navigation-cs/_static/image20.png)

**Rysunek 8**: Dodaj poniższe pliki


## <a name="step-2-creating-a-site-map"></a>Krok 2: Tworzenie mapy witryny sieci Web

Jednym z wyzwań związanych z zarządzaniem składa się z więcej niż kilka stron witryny sieci Web jest zapewnienie prostą metodę dla użytkowników zewnętrznych poruszać się po witrynie. Najpierw struktury nawigacyjnej witryny musi być zdefiniowany. Następnie tej struktury muszą być przetłumaczone na elementy interfejsu użytkownika można nawigować, takie jak menu lub linki do stron nadrzędnych. Na koniec całego tego procesu, musi być aktualizowanej i konserwowanej jako nowe strony są dodawane do lokacji i istniejące usunięte. Przed ASP.NET 2.0 deweloperzy były na ich do tworzenia struktury nawigacyjnej witryny, utrzymywanie jej i tłumaczenie go na elementy interfejsu użytkownika można nawigować. Za pomocą programu ASP.NET 2.0, deweloperzy mogą korzystać z bardzo elastyczny wbudowane w system nawigacji lokacji.

System nawigacji witryny ASP.NET w wersji 2.0 zapewnia środek dewelopera do definiowania mapy witryny sieci Web, a następnie dostęp do informacji za pomocą programowego interfejsu API. ASP.NET jest dostarczany z dostawcy mapy witryny, który oczekuje, że dane mapy witryny ma być przechowywany w pliku XML, który jest sformatowany w określony sposób. Jednak ponieważ nawigacji systemu lokacji jest oparta na [modelu dostawca](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) może zostać rozszerzony do obsługi alternatywne sposoby dla serializacji mapy witryny. Artykuł Jeff Prosise [SQL lokacji mapy dostawcy możesz już został oczekiwania dla](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) pokazuje, jak utworzyć dostawcy mapy witryny mapy witryny, przechowywane w bazie danych programu SQL Server; w innym rozwiązaniem jest utworzenie [na podstawie dostawcy mapy witryny struktury systemu plików](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

W tym samouczku, użyjemy dostawcy mapy witryny domyślnej, który jest dostarczany za pomocą programu ASP.NET 2.0. Do tworzenia mapy witryny, po prostu kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wybierz pozycję Dodaj nowy element i wybierz opcję mapy witryny. Pozostaw nazwę `Web.sitemap` i kliknij przycisk Dodaj.


[![Dodawanie mapy witryny sieci Web do projektu](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Rysunek 9**: Dodawanie mapy witryny sieci Web do projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image23.png))


Plik mapy witryny jest plikiem XML. Należy pamiętać, że program Visual Studio oferuje funkcję IntelliSense dla struktury mapy witryny. Plik mapy witryny musi mieć `<siteMap>` węzeł jako węzeł główny, który musi zawierać dokładnie jeden `<siteMapNode>` elementu podrzędnego. Najpierw `<siteMapNode>` następnie może zawierać dowolną liczbę elementów potomnych `<siteMapNode>` elementów.

Zdefiniuj Mapa witryny, aby mógł naśladować strukturę systemu plików. Oznacza to, Dodaj `<siteMapNode>` elementu dla każdego z trzech folderów, a podrzędny `<siteMapNode>` elementów dla każdej strony ASP.NET w tych folderach, w następujący sposób:

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

Mapa witryny definiuje struktury nawigacyjnej witryny sieci Web, czyli hierarchii, w tym artykule opisano różne sekcje witryny. Każdy `<siteMapNode>` element `Web.sitemap` reprezentuje sekcję w strukturze nawigacji strony.


[![Mapa witryny reprezentuje hierarchicznej struktury nawigacyjnej](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Na rysunku nr 10**: mapy witryny reprezentuje hierarchicznej struktury nawigacyjnej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image26.png))


Program ASP.NET udostępnia struktury mapy witryny za pomocą programu .NET Framework [klasy SiteMap](https://msdn.microsoft.com/library/system.web.sitemap.aspx). Ta klasa ma `CurrentNode` właściwość, która zwraca informacje o aktualnie odwiedzania przez użytkownika; `RootNode` właściwość zwraca głównego mapy witryny (Home, nasze mapy witryny). Zarówno `CurrentNode` i `RootNode` return właściwości [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) wystąpień, które mają właściwości takich jak `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`i tak dalej umożliwiające mapy witryny hierarchię, aby być dodawanym.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Krok 3: Wyświetlanie Menu, w oparciu o mapy witryny

Uzyskiwanie dostępu do danych w programie ASP.NET 2.0 można zrobić programowo, jak pokazano w ASP.NET 1.x, lub w sposób deklaratywny, za pomocą nowego [kontrolki źródła danych](https://msdn.microsoft.com/library/ms227679.aspx). Istnieje kilka kontrolki źródła danych wbudowane, takie jak kontrolki SqlDataSource, umożliwiający dostęp do danych z relacyjnej bazy danych, kontrolka ObjectDataSource, uzyskać dostęp do danych z klasy i inne. Możesz nawet utworzyć własny [kontrolki źródła danych niestandardowych](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

Kontrolki źródła danych służy jako serwer proxy między Twoja strona ASP.NET i danych bazowych. Aby można było wyświetlić dane pobrane z kontroli źródła danych, utworzymy zazwyczaj dodać innego formantu sieci Web do strony i powiązać ją z kontrolą źródła danych. Aby powiązać formant sieci Web do kontroli źródła danych, po prostu ustaw kontrolki sieci Web `DataSourceID` właściwości do wartości danych kontroli źródła `ID` właściwości.

Ułatwiające pracę z danymi mapy witryny, program ASP.NET zawiera formant SiteMapDataSource, który pozwala nam na powiązanie formantu sieci Web przed mapy witryny naszej witryny sieci Web. Dwie kontrolki sieci Web w widoku drzewa i Menu są często używane do udostępniają interfejs użytkownika nawigacji. Aby powiązać dane mapy witryny do jednej z tych dwóch kontrolek, po prostu Dodaj SiteMapDataSource do strony wraz z widoku drzewa lub formant Menu, którego `DataSourceID` właściwość jest ustawiana odpowiednio. Na przykład możemy dodać formant Menu, strony wzorcowej, za pomocą następujących znaczników:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Aby uzyskać lepszą kontrolę nad emitowany kod HTML firma Microsoft można powiązać kontrolki SiteMapDataSource kontrolce elementu powtarzanego w następujący sposób:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

Kontrolka SiteMapDataSource zwraca poziom hierarchii jeden mapy witryny w czasie, począwszy od główny węzeł mapy witryny (Home, nasze mapy witryny), następnie następnego poziomu (podstawowe raportowanie, filtrowanie raportów i dostosować formatowanie) i tak dalej. Podczas tworzenia wiązania SiteMapDataSource do Repeater, wylicza pierwszy poziom zwracane, a następnie tworzy `ItemTemplate` dla każdego `SiteMapNode` wystąpienia w tym pierwszy poziom. Do dostępu do określonej właściwości elementu `SiteMapNode`, możemy użyć `Eval(propertyName)`, czyli jak Pobieramy każdego `SiteMapNode`firmy `Url` i `Title` właściwości kontrolki Hiperlinku.

W powyższym przykładzie elementu powtarzanego spowoduje, że następujące znaczniki:


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

W skład tych lokacji węzłów map (podstawowe raportowanie, filtrowanie raportów i dostosować formatowanie) *drugi* poziomu mapy witryny są renderowane nie pierwszy. Jest to spowodowane SiteMapDataSource `ShowStartingNode` właściwość ma wartość False, powodując SiteMapDataSource obejść główny węzeł mapy witryny i zamiast tego należy rozpocząć od zwracanie drugi poziom w hierarchii mapy witryny.

Aby wyświetlić elementy podrzędne dla podstawowe raportowanie, filtrowanie raportów i dostosować formatowanie `SiteMapNode` s, możemy dodać innego elementu powtarzanego do początkowego elementu powtarzanego `ItemTemplate`. Ten drugi Repeater będą powiązane `SiteMapNode` wystąpienia `ChildNodes` właściwości w następujący sposób:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Te dwa wzmacniaki powoduje następujące znaczniki (niektóre znaczników została usunięta dla zwięzłości):


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Przy użyciu CSS style wybrane z [Rachel Andrew](http://www.rachelandrew.co.uk/)użytkownika Zarezerwuj [Antologia CSS: 101 Essential porad, sztuczek, &amp; hakowanie](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), `<ul>` i `<li>` mają różne elementy tak, aby znaczniki generuje następujące wyniki visual:


![Menu składa się z dwóch wzmacniaki i niektóre CSS](master-pages-and-site-navigation-cs/_static/image27.png)

**Rysunek 11**: Menu składa się z dwóch wzmacniaki i niektóre CSS


To menu znajduje się w stronę wzorcową i powiązane z mapy witryny zdefiniowane w `Web.sitemap`, co oznacza że każda zmiana mapy witryny będą natychmiast odzwierciedlane na wszystkich stronach używające `Site.master` strony wzorcowej.

## <a name="disabling-viewstate"></a>Wyłączanie ViewState

Wszystkie formanty ASP.NET Opcjonalnie można utrwalić stanu do [stanu widoku](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), który jest serializowany jako ukryte pole formularza w postaci HTML. Stan widoku jest używany przez formanty do zapamiętania ich programowo zmienić stan różnych ogłaszania zwrotnego, takie jak dane powiązane z formantu sieci Web danych. Wyświetl stan pozwala na informacje, aby pamiętać różnych ogłaszania zwrotnego, zwiększa się jednak do rozmiaru znaczników, które muszą być wysyłane do klienta i może prowadzić do rozrostu strony poważny, jeśli nie ściśle monitorowane. Kontrolki internetowe danych szczególnie GridView są szczególnie notorious do dodawania wielu dodatkowych kilobajtów znaczników do strony. Takie zwiększenie może być nieistotne dla użytkowników, szerokopasmowych lub intranet, stan widoku można dodać kilka sekund do obiegu dla użytkowników programu dial-up.

Aby zobaczyć wpływ stanu widoku, odwiedź stronę w przeglądarce, a następnie Wyświetl źródło wysyłane przez stronę sieci web (w programie Internet Explorer przejdź do menu widoku i wybierz opcję źródła). Można też włączyć [śledzenie stron](https://msdn.microsoft.com/library/sfbfw58f.aspx) Aby wyświetlić widok stanu alokacji, używanego przez poszczególne formantów na stronie. Informacje o stanie widoku jest serializowana WE ukryte pole formularza o nazwie `__VIEWSTATE`, który znajduje się w `<div>` element natychmiast po otwarciu `<form>` tagu. Stan widoku tylko jest trwały, gdy jest używany; formularz sieci Web Jeśli nie ma strony ASP.NET `<form runat="server">` w jego składni deklaratywnej nie będzie `__VIEWSTATE` ukryte pole formularza w renderowanego kodu znaczników.

`__VIEWSTATE` Pola formularza, generowane przez stronę wzorcową dodaje około 1800 bajtów do strony wygenerowany kod znaczników. Ten dodatkowy rozrostu wynika przede wszystkim kontrolce elementu powtarzanego jako zawartość kontrolki SiteMapDataSource są zachowywane, aby wyświetlić stan. Gdy dodatkowych 1800 bajtów może nie promieniowe wydają się być zbyt wiele, by poznawania, gdy korzystający z kontrolki GridView przy użyciu wielu pól i rekordów, stan widoku można łatwo spęcznienia ośmiokrotnego 10.

Stan widoku można wyłączyć na poziomie strony lub kontrolki, ustawiając `EnableViewState` właściwości `false`, a tym samym zmniejszenie rozmiaru renderowanego kodu znaczników. Od stanu widoku danych formant utrwala dane powiązane z danymi formantu sieci Web w ogłaszania zwrotnego w sieci Web po wyłączeniu stanu widoku danych sieci Web kontroli danych musi być powiązany każdej strony. W programie ASP.NET wersji 1.x siebie tę odpowiedzialność spadł w ucz developer strony. za pomocą programu ASP.NET 2.0 jednak w kontrolkach internetowych danych zostanie ponownie powiązać do kontroli źródła danych, ich każdego odświeżania w razie potrzeby.

Aby zmniejszyć stan widoku danej strony umożliwia ustawianie kontrolce elementu powtarzanego `EnableViewState` właściwość `false`. Można to zrobić w oknie właściwości w projektancie, lub deklaratywnie w widoku źródła. Po wprowadzeniu tej zmiany w elemencie powtarzanym oznaczeniu deklaracyjnym powinien wyglądać:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Po tej zmianie stronę firmy renderowania widoku, że rozmiar danych stanu został zmniejszony do kilku 52 bajtów, 97% oszczędności w widoku stanu rozmiar! W samouczkach w tej serii firma Microsoft będzie stan widoku danych kontrolki sieci Web powodująca domyślne wyłączenie w celu zmniejszenia rozmiaru renderowanego kodu znaczników. W większości przykłady `EnableViewState` będzie można ustawić właściwości `false` i bez wzmiankę zrobione. Jedyną sytuacją, w widoku stanu zostaną omówione znajduje się w scenariuszach, w którym musi być włączona w kolejności dla danych do zapewnienia jego funkcjonalność kontrolować sieci Web.

## <a name="step-4-adding-breadcrumb-navigation"></a>Krok 4. Dodawanie do stron nadrzędnych

Aby ukończyć strony wzorcowej, możemy dodać element interfejsu użytkownika łączy do stron nadrzędnych nawigacji do każdej strony. Łącza do stron nadrzędnych szybko wyświetlane ich bieżącą pozycję w hierarchii lokacji. Na stronie; Dodawanie nawigacji w programie ASP.NET 2.0 jest łatwe po prostu Dodaj kontrolki ścieżki mapy witryny żaden kod nie jest wymagana.

Dla naszej witrynie, dodanie tej kontrolki z nagłówkiem `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

Łączy do stron nadrzędnych pokazuje bieżącą stronę użytkownik odwiedził w hierarchii mapy witryny, a także tej lokacji węzeł mapy "elementów nadrzędnych," do katalogu głównego (główny, w naszym mapy witryny).


![Łączy do stron nadrzędnych Wyświetla bieżącą stronę i jego elementy nadrzędne w witrynie mapowanie hierarchii](master-pages-and-site-navigation-cs/_static/image28.png)

**Rysunek 12**: łączach Wyświetla bieżącą stronę i jego elementy nadrzędne w witrynie mapowanie hierarchii


## <a name="step-5-adding-the-default-page-for-each-section"></a>Krok 5. Dodawanie domyślna strona dla każdej sekcji

Samouczki w naszej witrynie są podzielone na różne kategorie podstawowe raportowanie, filtrowanie, niestandardowe formatowanie, i tak dalej z folderem, dla każdej kategorii i odpowiednie samouczki jako strony ASP.NET, w tym folderze. Ponadto każdy folder zawiera `Default.aspx` strony. Dla tej strony domyślnej umożliwia wyświetlenie wszystkich samouczków dotyczących bieżącej sekcji. Oznacza to aby uzyskać `Default.aspx` w `BasicReporting` folderu czy mamy łącza do `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, i `ProgrammaticParams.aspx`. W tym miejscu, możemy użyć `SiteMap` klas i danych formantu sieci Web, aby wyświetlić te informacje, na podstawie mapy witryny zdefiniowane w `Web.sitemap`.

Umożliwia wyświetlanie nieuporządkowaną listę przy użyciu Repeater ponownie, ale tym razem będzie wyświetlana tytuł i opis samouczków. Ponieważ znaczników i kodu, aby osiągnąć ten należy powtórzyć dla każdego `Default.aspx` stronie mamy hermetyzacji logika interfejsu użytkownika w [kontrolki użytkownika](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Utwórz folder w witrynie sieci Web o nazwie `UserControls` i Dodaj do tego nowego elementu typu kontrolka użytkownika sieci Web o nazwie `SectionLevelTutorialListing.ascx`i Dodaj następujący kod:


[![Dodaj nową kontrolkę użytkownika sieci Web do folderu elementy kontrolki użytkownika](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Rysunek 13**: Dodaj nową kontrolkę użytkownika sieci Web do `UserControls` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs


[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

W poprzednim przykładzie Repeater możemy powiązać `SiteMap` danych powtarzanego deklaratywne; `SectionLevelTutorialListing` kontrolki użytkownika, jednak odbywa się to programowo. W `Page_Load` procedura obsługi zdarzeń, dokonuje do upewnij się, że ta strona s adres URL mapowany na węzeł mapy witryny. Jeśli ten formant użytkownika jest używana w strona, która nie ma odpowiadającego `<siteMapNode>` wpisu `SiteMap.CurrentNode` zwróci `null` i żadne dane nie będą powiązane powtarzanego. Zakładając, że mamy `CurrentNode`, możemy powiązać jej `ChildNodes` kolekcji do elementu powtarzanego. Ponieważ nasza Mapa witryny jest skonfigurowany tak, aby `Default.aspx` strony w każdej sekcji jest węzła nadrzędnego wszystkie samouczki w tej sekcji, ten kod będą wyświetlane łącza do i opisy wszystkich sekcji samouczków, jak pokazano na poniższym zrzucie ekranu.

Po utworzeniu tego elementu powtarzanego Otwórz `Default.aspx` strony we wszystkich folderach, przejdź do widoku projektu i po prostu przeciągnij formant użytkownika za pomocą Eksploratora rozwiązań na powierzchnię projektową miejscu listy samouczków, są wyświetlane.


[![Formant użytkownika ma została dodana do Default.aspx](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Rysunek 14**: formant użytkownika ma została dodana do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image34.png))


[![Podstawowe raportowanie samouczki są wyświetlane.](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Rysunek 15**: wymieniono podstawowe samouczki raportowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image37.png))


## <a name="summary"></a>Podsumowanie

Mapa witryny, zdefiniowane i pełne strony wzorcowej mamy schemat spójne strony układu i nawigacji dla naszych samouczków związanych z danymi. Niezależnie od tego, ile stron, dodamy do naszej witryny aktualizacji informacji nawigacji układu lub witryny strony całej witryny jest szybki i prosty proces ze względu na te informacje są scentralizowane. W szczególności informacji o układzie strony jest zdefiniowana na stronie głównej `Site.master` a lokacją mapy w `Web.sitemap`. Firma Microsoft nie trzeba było pisać *wszelkie* kodu, aby osiągnąć ten mechanizm nawigacji i układ strony całej witryny i przechowujemy WYSIWYG pełne wsparcie w programie Visual Studio.

Po zakończeniu warstwy dostępu do danych i warstwy logiki biznesowej i mieć spójne strony układu i nawigacja w witrynie zdefiniowane, jesteśmy gotowi do rozpocząć eksplorowanie typowych wzorców do raportowania. W [dalej](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) trzech samouczków przyjrzymy podstawowe zadania raportowania, wyświetlanie danych pobranych z LOGIKI w widoku GridView DetailsView, i kontroluje FormView.

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Omówienie stron wzorcową platformy ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Strony wzorcowe na platformie ASP.NET 2.0](http://odetocode.com/Articles/419.aspx)
- [Szablony projektów 2.0 platformy ASP.NET](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Przegląd Nawigacja witryny ASP.NET](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Badanie programu ASP.NET 2.0 w nawigacji po witrynie](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Funkcje nawigacji 2.0 witryny ASP.NET](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Stany widoków środowiska ASP.NET opis](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Porady: Włączanie śledzenia dla strony ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Formanty użytkownika ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Liz Shulok Dennis Patterson i Hilton Giesenow. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](creating-a-business-logic-layer-cs.md)
> [dalej](creating-a-data-access-layer-vb.md)
