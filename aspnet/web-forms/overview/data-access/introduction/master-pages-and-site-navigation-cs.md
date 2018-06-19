---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Strony wzorcowej i nawigacji w witrynie (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Jedną wspólną cechą witryn sieci Web przyjaznych dla użytkownika jest, że schemat nawigacji i układ strony spójny, całej lokacji. W tym samouczku wygląda jak y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: 7bf834373c6f44272a9c4c816e649fefe3247a4e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887603"
---
<a name="master-pages-and-site-navigation-c"></a>Strony wzorcowej i nawigacji w witrynie (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) lub [pobierania plików PDF](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Jedną wspólną cechą witryn sieci Web przyjaznych dla użytkownika jest, że schemat nawigacji i układ strony spójny, całej lokacji. W tym samouczku analizuje tworzenia spójny wygląd i zachowanie we wszystkich stron, które można łatwo aktualizować.


## <a name="introduction"></a>Wprowadzenie

Jedną wspólną cechą witryn sieci Web przyjaznych dla użytkownika jest, że schemat nawigacji i układ strony spójny, całej lokacji. Platforma ASP.NET 2.0 wprowadzono dwie nowe funkcje, które znacznie upraszcza wdrażanie zarówno całej lokacji, strony układu i nawigacja schemat: strony wzorcowe i lokacji nawigacji. Strony wzorcowe umożliwiają deweloperom tworzenie szablonu całej lokacji z wyznaczonych edytowalnych. Następnie można zastosować ten szablon do stron ASP.NET w witrynie. Takie stron ASP.NET wystarczy zdefiniować zawartości dla strony wzorcowej do określonych edytowalnych innych znaczników na stronie głównej jest identyczne we wszystkich stron ASP.NET, które używają strony wzorcowej. Ten model umożliwia deweloperom zdefiniuj oraz na scentralizowanie układ strony w całej lokacji, a tym samym co ułatwia tworzenie spójny wygląd i zachowanie we wszystkich stron, które można łatwo aktualizować.

[Nawigacji systemu lokacji](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) zapewnia zarówno mechanizm dla deweloperów strony zdefiniować mapy witryny sieci Web i interfejsu API dla tej mapy witryny do programowego można wykonać zapytania. Nowe formanty sieci Web nawigacji, który Menu, TreeView i ścieżki mapy witryny ułatwiają renderować całości lub części mapy witryny w typowych nawigacji elementu interfejsu użytkownika. Będziemy używać domyślnego lokacji nawigacji dostawcy, co oznacza, że nasze mapy witryny zostanie zdefiniowana w pliku w formacie XML.

Aby zilustrować tych pojęć i naszych samouczków witryny sieci Web bardziej użyteczne, Poświęć tej lekcji Definiowanie układ strony w całej lokacji, implementacja mapy witryny sieci Web i dodawanie nawigacji interfejsu użytkownika. Na koniec tego samouczka mamy będzie dopracowany witryny sieci Web projektu do tworzenia samouczek stronach sieci web.


[![W rezultacie tego samouczka](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Rysunek 1**: końcowego wyniku z tego samouczka ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>Krok 1: Tworzenie strony wzorcowej

Pierwszym krokiem jest utworzenie strony wzorcowej dla witryny. Obecnie naszą witrynę sieci Web składa się tylko wpisane zestawu danych (`Northwind.xsd`w `App_Code` folderu), klasy logiki warstwy Biznesowej (`ProductsBLL.cs`, `CategoriesBLL.cs`i tak dalej wszystkie w `App_Code` folderu), bazy danych (`NORTHWND.MDF`w `App_Data` folder), plik konfiguracji (`Web.config`), a plik arkusza stylów CSS (`Styles.css`). Po usunięciu te strony i pliki prezentacja za pomocą warstwy DAL i logiki warstwy Biznesowej z dwóch pierwszych samouczki, ponieważ firma Microsoft będzie można Rewidowanie tych przykładów bardziej szczegółowo w przyszłości samouczki.


![Pliki w naszym projektu](master-pages-and-site-navigation-cs/_static/image4.png)

**Rysunek 2**: pliki w naszym projektu


Aby utworzyć strony wzorcowej, kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz polecenie Dodaj nowy element. Następnie wybierz typ strony wzorcowej z listy szablonów i nadaj mu nazwę `Site.master`.


[![Dodaj nową stronę wzorcową do witryny sieci Web](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Rysunek 3**: Dodaj nową stronę wzorcową do witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image7.png))


Zdefiniuj tutaj układ strony całej lokacji, na stronie głównej. Można użyć widoku projektu i Dodaj formanty niezależnie od układu lub sieci Web należy, lub można ręcznie dodać kod znaczników ręcznie w widoku źródła. Na stronie głównej w programie używać [kaskadowych arkuszy stylów](http://www.w3schools.com/css/default.asp) pozycjonowanie i style CSS ustawienia zdefiniowane w pliku zewnętrznym `Style.css`. Gdy nie można ustalić, z poziomu znacznika pokazano poniżej, reguły CSS są zdefiniowane tak, aby nawigacji `<div>`w zawartości jest bezwzględnego, który pojawia się po lewej stronie i ma stałą szerokość 200 pikseli.

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Strona wzorcowa definiuje zarówno układ strony statyczne, jak i regionów, które mogą być edytowane przez stron ASP.NET, które używają strony wzorcowej. Te zawartości edytowalnych są wskazywane przez formantu elementu ContentPlaceHolder może być widoczny w obrębie zawartości `<div>`. Pojedynczy element ContentPlaceHolder ma naszej strony wzorcowej (`MainContent`), ale strony wzorcowej mogą mieć wiele Elementy ContentPlaceHolders.

Adiustację wprowadzony powyżej przełączając do widoku projektu zawiera układ strony wzorcowej. Żadnych stron ASP.NET, które używają tej strony wzorcowej będzie mieć tego układu jednolite, możliwość określenia kod znaczników dla `MainContent` regionu.


[![Strona wzorcowa, podczas wyświetlania za pośrednictwem widoku Projekt](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Rysunek 4**: strona wzorcowa, podczas wyświetlania za pośrednictwem widoku Projekt ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>Krok 2: Dodawanie strony głównej witryny sieci Web

Ze stroną wzorcową zdefiniowane już wszystko gotowe do dodania stron ASP.NET dla witryny sieci Web. Zacznijmy od dodania `Default.aspx`, strony głównej naszą witrynę sieci Web. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz polecenie Dodaj nowy element. Wybierz opcję formularza sieci Web z listy szablonów i nazwa pliku `Default.aspx`. Ponadto zaznacz pole wyboru "Wybierz strony głównej".


[![Dodaj nowy formularz sieci Web sprawdzanie strony wzorcowej zaznacz pole wyboru](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Rysunek 5**: Dodaj nowy formularz sieci Web sprawdzanie zaznacz pole wyboru strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image13.png))


Po kliknięciu przycisku OK, firma Microsoft jest wyświetlony monit o wybranie jakie strony wzorcowej tej nowej strony ASP.NET należy używać. Natomiast w projekcie, może mieć wiele stron wzorcowych, będziemy mieć tylko jeden.


[![Wybierz strony wzorcowej, której ma używać tej strony ASP.NET](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Rysunek 6**: Wybierz strony wzorcowej to zastosowanie powinien strony ASP.NET ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image16.png))


Po wybraniu strony wzorcowej, nowych stron ASP.NET będzie zawierać następujący kod:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

W `@Page` dyrektywa jest odwołaniem do pliku strony głównej używanego (`MasterPageFile="~/Site.master"`), i znaczników strony ASP.NET zawiera formant zawartości dla każdego elementu ContentPlaceHolder formantów zdefiniowane na stronie głównej, z formantu `ContentPlaceHolderID` Mapowanie zawartości do określonego elementu ContentPlaceHolder kontrolować. Formant zawartości jest, gdzie umieścić znaczniki mają być wyświetlane w odpowiedniego elementu ContentPlaceHolder. Ustaw `@Page` dyrektywy `Title` atrybutu do strony głównej i dodać niektóre powitalnego zawartości do formantu zawartości:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

`Title` Atrybutu w `@Page` dyrektywy pozwala ustawić tytuł strony ze strony programu ASP.NET, mimo że `<title>` jest zdefiniowany element na stronie głównej. Firma Microsoft może także ustawić tytuł programowo, przy użyciu `Page.Title`. Należy również zauważyć, że strony wzorcowej odwołania do arkuszy stylów (takie jak `Style.css`) są automatycznie aktualizowane, aby mogły działać w dowolnej strony ASP.NET, niezależnie od tego, jakie katalogu strony ASP.NET jest względem strony wzorcowej.

Przełączanie do widoku projektu, że możemy stwierdzić wygląd strony w przeglądarce. Należy pamiętać, że w projekcie wyświetlania dla strony ASP.NET, czy można edytować tylko zawartości edytowalnych znaczników z systemem innym niż ContentPlaceHolder zdefiniowane na stronie głównej jest szary.


[![Widok projektu dla strony ASP.NET zawiera edycji i nie można edytować regionów](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Rysunek 7**: widok projektu dla platformy ASP.NET strony zawiera zarówno edytowalna i regiony Non-edytowalna ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image19.png))


Gdy `Default.aspx` odwiedzenia strony w przeglądarce, aparatu ASP.NET automatycznie scala strony zawartości strony wzorcowej i ASP. NET zawartość i renderuje zawartość scalone do końcowego kodu HTML, który jest przesyłany do przeglądarki. Po zaktualizowaniu zawartości strony wzorcowej wszystkich stron ASP.NET, które używają tej strony wzorcowej ma ich zawartość remerged z nowej zawartości strony wzorcowej podczas następnego żądania. Krótko mówiąc, model strony wzorcowej pozwala na jednej stronie szablon układu, aby być zdefiniowane (strony wzorcowej) której zmiany są natychmiast odzwierciedlone w całej lokacji.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Dodawanie stron ASP.NET dodatkowe witryny sieci Web

Załóżmy Poświęć chwilę, aby dodać dodatkowe klas zastępczych strony ASP.NET do lokacji, w której będą przechowywane ostatecznie różnych pokazy raportowania. Będzie więcej niż 35 pokazy łącznie tak zamiast tworzenia wszystkie strony stub umożliwia po prostu utworzyć kilka pierwszy. Ponieważ będzie również wiele kategorii pokazów, aby lepiej zarządzać pokazy dodać folder dla kategorii. Dodaj następujące trzy foldery teraz:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Na koniec Dodaj nowych plików, jak pokazano w Eksploratorze rozwiązań na rysunku 8. Podczas dodawania każdego pliku, pamiętaj, aby zaznaczyć pole wyboru "Wybierz strony wzorcowej".


![Dodaj następujące pliki](master-pages-and-site-navigation-cs/_static/image20.png)

**Rysunek 8**: Dodaj poniższe pliki


## <a name="step-2-creating-a-site-map"></a>Krok 2: Tworzenie mapy witryny sieci Web

Jednym z wyzwań związanych z zarządzaniem składa się z więcej niż kilka stron witryny sieci Web oferuje łatwe dla użytkowników przejść do witryny. Najpierw należy zdefiniować struktura nawigacji w witrynie. Następnie ta struktura musi można przetłumaczyć elementów interfejsu użytkownika można nawigować, np. menu lub nawigacji. Na koniec całego tego procesu musi być obsługiwany i aktualizowane w miarę dodawania nowych stron do lokacji i istniejące usunięte. Przed składnika ASP.NET 2.0 deweloperzy były na ich własnych dla tworzenia struktury nawigacji witryny, utrzymując i tłumaczenie go na elementy interfejsu użytkownika można nawigować. Z programem ASP.NET 2.0, deweloperzy mogą korzystać z bardzo elastyczne wbudowanej w system nawigacji witryny.

System nawigacji witryny ASP.NET 2.0 umożliwia projektantowi do definiowania mapy witryny sieci Web, a następnie dostęp do informacji za pośrednictwem programowe interfejsu API. Program ASP.NET jest dostarczany z dostawcy mapy witryny, który oczekuje na dane mapy witryny mają być przechowywane w pliku XML w określony sposób. Jednak ponieważ nawigacji systemu lokacji jest zbudowany [modelu dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) może zostać rozszerzony do obsługi alternatywnych metod dla serializacji mapy witryny. Artykuł Jeff Prosise [SQL lokacji mapy dostawcy możesz już został do](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) pokazuje, jak utworzyć dostawcy mapy witryny który przechowuje mapy witryny w bazie danych programu SQL Server; innym rozwiązaniem jest utworzenie [na podstawie dostawcy mapy witryny struktury systemu plików](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

W tym samouczku jednak Użyjmy domyślnego dostawcę mapy witryny, który jest dostarczany z programem ASP.NET 2.0. Aby utworzyć mapy witryny, po prostu kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wybierz polecenie Dodaj nowy element i wybierz opcję mapy witryny. Pozostaw nazwę jako `Web.sitemap` i kliknij przycisk Dodaj.


[![Dodaj mapy witryny sieci Web do projektu](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Rysunek 9**: Dodaj mapy witryny sieci Web do projektu Your ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image23.png))


Plik mapy witryny jest plikiem XML. Należy pamiętać, że program Visual Studio udostępnia IntelliSense dla struktury mapy witryny. Plik mapy witryny musi mieć `<siteMap>` węzeł jako węzeł główny, który musi zawierać dokładnie jeden `<siteMapNode>` element podrzędny. Pierwsza `<siteMapNode>` element następnie może zawierać dowolną liczbę podrzędny `<siteMapNode>` elementów.

Zdefiniuj mapy witryny, aby naśladował strukturę systemu plików. Oznacza to, Dodaj `<siteMapNode>` elementu dla każdego z trzech folderów i podrzędne `<siteMapNode>` elementy dla każdej strony ASP.NET w tych folderach w następujący sposób:

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

Mapy witryny definiuje struktury nawigacji witryny sieci Web, która jest hierarchii, która zawiera opis różnych sekcji witryny. Każdy `<siteMapNode>` element `Web.sitemap` reprezentuje sekcję w strukturze nawigacji witryny.


[![Mapy witryny reprezentuje strukturę hierarchiczną nawigacji](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Na rysunku nr 10**: mapy witryny reprezentuje strukturę hierarchiczną nawigacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image26.png))


Program ASP.NET udostępnia struktury mapy witryny za pomocą programu .NET Framework [klasy SiteMap](https://msdn.microsoft.com/library/system.web.sitemap.aspx). Ta klasa ma `CurrentNode` właściwość, która zwraca informacje o sekcji, użytkownik jest obecnie odwiedzający; `RootNode` właściwość Zwraca pierwiastek mapy witryny (głównej, w naszym mapy witryny). Zarówno `CurrentNode` i `RootNode` return właściwości [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) wystąpienia, które mają właściwości takich jak `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`i tak dalej umożliwiające mapy witryny Hierarchia można udał.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Krok 3: Wyświetlanie Menu oparty na mapie witryny

Uzyskiwanie dostępu do danych w programie ASP.NET 2.0 można wykonać programowo, takich jak w programie ASP.NET 1.x, lub deklaratywnie, za pomocą nowego [kontroli źródła danych](https://msdn.microsoft.com/library/ms227679.aspx). Istnieje kilka kontrolki źródła danych wbudowanych, takich jak kontrola SqlDataSource, do uzyskiwania dostępu do danych relacyjnych baz danych, kontrolki ObjectDataSource, uzyskać dostęp do danych z klasy i inne. Można też tworzyć własne [źródła danych niestandardowych formantów](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

Kontrolki źródła danych służą jako serwer proxy między strony ASP.NET i danych. Aby wyświetlić dane pobrane z kontroli źródła danych, firma Microsoft będzie zwykle na stronie Dodaj inny formant sieci Web i powiązać ją z kontroli źródła danych. Aby powiązać formant sieci Web do kontroli źródła danych, wystarczy ustawić formant sieci Web `DataSourceID` na wartość formantu źródła danych `ID` właściwości.

W celu ułatwienia pracy z danymi mapy witryny, program ASP.NET zawiera kontrolki SiteMapDataSource, który umożliwia powiązanie formantu względem naszego witryny sieci Web mapy witryny sieci Web. Dwóch formantów sieci Web TreeView i Menu są często używane do udostępniają interfejs użytkownika nawigacji. Aby powiązać dane mapy witryny do jednej z tych dwóch formantów, po prostu Dodaj SiteMapDataSource do strony wraz z elementu TreeView lub Menu kontroli, których `DataSourceID` właściwość jest ustawiona odpowiednio. Na przykład możemy dodać kontrolkę Menu, strony wzorcowej przy użyciu następujących znaczników:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Dla dokładniejszą kontrolę nad emitowany HTML, możemy powiązanie formantu SiteMapDataSource na kontrolce elementu powtarzanego w następujący sposób:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

Formant SiteMapDataSource zwraca poziom hierarchii jednej mapy witryny w czasie, począwszy od główny węzeł mapy witryny (głównej, w naszym mapy witryny), następnie następnego poziomu (podstawowym raportowaniem, filtrowania raportów i dostosować formatowania) i tak dalej. Podczas tworzenia wiązania SiteMapDataSource elementu powtarzanego, wylicza go pierwszy poziom zwrócił i tworzy `ItemTemplate` dla każdego `SiteMapNode` wystąpienia, w tym pierwszy poziom. Można uzyskać dostępu do określonej właściwości elementu `SiteMapNode`, możemy użyć `Eval(propertyName)`, czyli sposób uzyskujemy każdego `SiteMapNode`w `Url` i `Title` właściwości formantu hiperłącza.

W powyższym przykładzie elementu powtarzanego spowoduje, że następujący kod:


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Te węzły mapy witryny (podstawowym raportowaniem, filtrowania raportów i dostosować formatowania) obejmują *drugi* poziomu renderowanego nie pierwszej mapy witryny. Jest to spowodowane SiteMapDataSource `ShowStartingNode` właściwość jest ustawiona na wartość False, powodując SiteMapDataSource do obejścia główny węzeł mapy witryny i zamiast tego rozpocząć zwracając drugiego poziomu w hierarchii mapy witryny.

Do wyświetlenia elementów podrzędnych podstawowym raportowaniem, filtrowania raportów i dostosować formatowania `SiteMapNode` s, można dodać innego elementu powtarzanego do początkowego elementu powtarzanego `ItemTemplate`. Zostanie powiązany tego drugiego elementu powtarzanego `SiteMapNode` wystąpienia `ChildNodes` właściwości, w następujący sposób:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Te dwie wzmacniaki spowodować następujący kod znaczników (niektóre znaczników zostało usunięte w przypadku skrócenia):


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Za pomocą CSS style wybrane z [Rachel Andrew](http://www.rachelandrew.co.uk/)obiektu książki [Anthology CSS: 101 porady ważne wskazówki, &amp; uzyskuje dostęp do](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), `<ul>` i `<li>` elementy są stylem tak, aby Kod znaczników tworzy visual następujące dane wyjściowe:


![Menu składa się z dwóch wzmacniaki i niektóre CSS](master-pages-and-site-navigation-cs/_static/image27.png)

**Rysunek 11**: Menu składa się z dwóch wzmacniaki i niektóre CSS


To menu strony wzorcowej i powiązany z mapy witryny zdefiniowane w `Web.sitemap`, co oznacza że zmiany do mapy witryny zostaną natychmiast odzwierciedlone na wszystkich stronach używające `Site.master` strony wzorcowej.

## <a name="disabling-viewstate"></a>Wyłączanie ViewState

Wszystkie kontrolki ASP.NET Opcjonalnie można utrwalić stanu do [stanu widoku](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), który jest szeregowana jako ukryte pole formularza w renderowanym HTML. Stan widoku jest używany przez formanty do zapamiętania ich programowo zmienić stan między ogłaszania zwrotnego, takich jak dane powiązane formantu danych sieci Web. Gdy stan widoku na to zezwala informacji należy pamiętać, między ogłaszania zwrotnego spowoduje zwiększenie rozmiaru kod znaczników, który należy wysłać do klienta i może spowodować pociągnięcie strony rozrostu Jeśli nie zostanie dokładnie monitorowane. Formanty sieci Web danych szczególnie widoku GridView są szczególnie odpowiedzialne za dodawanie dziesiątki dodatkowe kilobajtów znaczników do strony. Takie zwiększenie może być nieistotne dla użytkowników z połączenia szerokopasmowego lub intranecie, stan widoku można dodać kilka sekund do obiegu dla użytkowników programu dial-up.

Aby zobaczyć wpływ stanu widoku, odwiedź stronę w przeglądarce, a następnie Wyświetl źródło wysyłane przez stronę sieci web (w programie Internet Explorer przejdź do menu Widok i wybierz opcję źródła). Można też włączyć [śledzenie strony](https://msdn.microsoft.com/library/sfbfw58f.aspx) wyświetlić alokacji stan widoku, używane przez każdy z formantów na stronie. Informacje o stanie widoku jest serializowany w ukryte pole o nazwie `__VIEWSTATE`, który znajduje się w `<div>` element natychmiast po otwarciu `<form>` tagu. Stan widoku jest zachowywane tylko w przypadku jest używany; formularzy sieci Web Jeśli nie ma strony ASP.NET `<form runat="server">` w jego składni deklaratywnej nie będzie `__VIEWSTATE` ukryte pole formularza w renderowanego kodu znaczników.

`__VIEWSTATE` Pola formularza, generowane przez strony wzorcowej dodaje około 1800 bajtów do strony wygenerowanego kodu znaczników. Ta dodatkowa rozrostu wynika głównie kontrolce elementu powtarzanego jako zawartości formantu SiteMapDataSource są trwałe, aby wyświetlić stan. Gdy dodatkowych 1800 bajtów nie dość znacznie uzyskanie radością, jeśli element GridView z wielu pól i rekordów, stan widoku można łatwo spęcznienia przez współczynnik 10 lub więcej.

Stan widoku można wyłączyć na poziomie strona lub kontrolka przez ustawienie `EnableViewState` właściwości `false`, a tym samym zmniejszenie rozmiaru renderowanego kodu znaczników. Od stanu widoku danych sieci Web kontroli danych powiązanych z danymi formantu sieci Web w wielu ogłaszania zwrotnego będzie nadal występować gdy wyłączenie stanu widoku danych formant sieci Web danych musi być powiązana każdej strony. W wersji platformy ASP.NET 1.x spadła tę odpowiedzialność na łopatkach developer strony. z programu ASP.NET 2.0 jednak formantów sieci Web danych będzie ponownie powiązać do swoich danych kontroli źródła na każdym ogłaszania zwrotnego w razie potrzeby.

Aby zmniejszyć stan widoku danej strony umożliwia Ustaw kontrolce elementu powtarzanego `EnableViewState` właściwości `false`. Można to zrobić za pomocą okna właściwości w Projektancie lub deklaratywnie w widoku źródła. Po wprowadzeniu tej zmiany znaczników deklaratywne w elemencie powtarzanym powinien wyglądać następująco:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Po tej zmiany, strony w renderowania widoku, że rozmiar danych stanu został zmniejszony do zaledwie 52 bajtów, oszczędności 97% w widoku stanu rozmiar! W samouczkach w tej serii firma Microsoft będzie stan widoku danych formantów sieci Web powodująca domyślne wyłączenie Aby zmniejszyć rozmiar renderowanego kodu znaczników. W większości przykłady `EnableViewState` będzie mieć ustawioną właściwość `false` i zrobione bez uwagi. Tylko razem widok stanu zostaną dokładniej omówione w scenariuszach, w którym musi być włączona w danych do zapewnienia jego funkcjonalność kontrolować sieci Web.

## <a name="step-4-adding-breadcrumb-navigation"></a>Krok 4: Dodawanie nadrzędnych

Aby ukończyć strony wzorcowej, Dodajmy elementu interfejsu użytkownika nawigacją nawigacji do każdej strony. Łączach szybko wyświetlani użytkownicy, ich bieżącą pozycję w hierarchii lokacji. Dodawanie ślad w programie ASP.NET 2.0 jest bardzo łatwo dodać kontrolki ścieżki mapy witryny do strony; Brak kodu jest niezbędne.

Dla witryny, należy dodać ten formant do nagłówka `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

Łączach pokazuje bieżącą stronę odwiedzania w hierarchii mapy witryny, a także tej lokacji węzeł mapy "obiektów nadrzędnych," do katalogu głównego (głównej, w naszym mapy witryny).


![Łączach Wyświetla bieżącą stronę i jego obiektów nadrzędnych w witrynie mapowania hierarchii](master-pages-and-site-navigation-cs/_static/image28.png)

**Rysunek 12**: łączach Wyświetla bieżącą stronę i jego obiektów nadrzędnych w witrynie mapowania hierarchii


## <a name="step-5-adding-the-default-page-for-each-section"></a>Krok 5: Dodawanie domyślnej strony dla każdej sekcji

Samouczki w naszej witrynie są rozkładane na różne kategorie podstawowym raportowaniem, filtrowanie, formatowanie niestandardowe, i tak dalej z folderu dla każdej kategorii i odpowiednie samouczki jako strony ASP.NET w tym folderze. Ponadto każdy folder zawiera `Default.aspx` strony. Na tej stronie domyślne teraz wyświetlić wszystkie samouczków dla bieżącej sekcji. Oznacza to aby uzyskać `Default.aspx` w `BasicReporting` folderu trzeba łącza do `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, i `ProgrammaticParams.aspx`. W tym miejscu, możemy użyć `SiteMap` klas i danych formantu sieci Web, aby wyświetlić te informacje oparte na mapy witryny zdefiniowane w `Web.sitemap`.

Umożliwia wyświetlanie nieuporządkowaną listę ponownie, ale tym razem będzie wyświetlić tytuł i opis samouczków, przy użyciu elementu powtarzanego. Ponieważ znaczników i kodu do wykonania będzie konieczne należy powtórzyć dla każdego `Default.aspx` strony, możemy Hermetyzowanie tego logika interfejsu użytkownika w [kontrolki użytkownika](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Utwórz folder w witrynie sieci Web o nazwie `UserControls` i Dodaj do tego nowego elementu typu kontrolka użytkownika sieci Web o nazwie `SectionLevelTutorialListing.ascx`i Dodaj następujący kod:


[![Dodaj kontrolkę użytkownika sieci Web do folderu elementy UserControls](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Rysunek 13**: dodać nową kontrolkę użytkownika sieci Web do `UserControls` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs


[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

W poprzednim przykładzie elementu powtarzanego możemy powiązany `SiteMap` danych powtarzanego deklaratywnie; `SectionLevelTutorialListing` kontrolki użytkownika, jednak wykonuje programowo. W `Page_Load` program obsługi zdarzeń, dokonuje do upewnij się, że ta strona s adres URL mapowany na węzeł mapy witryny. Użycie tej kontrolki użytkownika na stronie nie ma odpowiadającego `<siteMapNode>` wpisu, `SiteMap.CurrentNode` zwróci `null` i żadne dane nie będą powiązane powtarzanego. Zakładając, że mamy `CurrentNode`, możemy powiązać jej `ChildNodes` kolekcji do elementu powtarzanego. Ponieważ naszych mapy witryny jest skonfigurowany tak, aby `Default.aspx` strony w każdej sekcji jest węzeł nadrzędny wszystkich samouczków w tej sekcji, ten kod będzie wyświetlana łącza do wraz z opisami wszystkich sekcji samouczki, jak pokazano na ekranie zrzut poniżej.

Po utworzeniu tego elementu powtarzanego Otwórz `Default.aspx` stron w foldery, przejdź do widoku projektu i po prostu przeciągnij formant użytkownika z Eksploratora rozwiązań na powierzchnię projektu miejscu samouczek listy pojawią się.


[![Kontrolki użytkownika został dodany do Default.aspx](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Rysunek 14**: kontrolki użytkownika został dodany do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image34.png))


[![Podstawowe samouczki raportowania są wyświetlane.](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Rysunek 15**: znajdują się podstawowe samouczki raportowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-site-navigation-cs/_static/image37.png))


## <a name="summary"></a>Podsumowanie

Mapy witryny zdefiniowany i strony wzorcowej pełną teraz mamy schemat spójne strony układu i nawigacji dla naszych samouczków związanych z danymi. Niezależnie od tego, ile stron dodania do witryny firmy Microsoft aktualizowanie informacji całej lokacji strony układu lub lokacji nawigacji jest szybkie i proste procesu z powodu tych informacji jest scentralizowane. W szczególności informacji o układzie strony jest zdefiniowana na stronie głównej `Site.master` i mapowanie w lokacji `Web.sitemap`. Firma Microsoft nie musisz zapisywać *żadnych* kod, aby osiągnąć ten mechanizm nawigacji i układ strony w całej lokacji, a firma Microsoft przechowywała pełnej WYSIWYG wsparcia projektanta w programie Visual Studio.

Po zakończeniu Warstwa dostępu do danych i warstwy logiki biznesowej i o spójne strony układu i nawigacja w witrynie zdefiniowana, aby rozpocząć eksplorowanie typowe wzorce raportowania. W [dalej](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) trzy samouczki przyjrzymy podstawowe zadania raportowania wyświetlanie dane pobrane z logiki warstwy Biznesowej w widoku DetailsView, w widoku GridView i kontroluje FormView.

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Omówienie stron ASP.NET wzorca](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Stron wzorcowych w programie ASP.NET 2.0](http://odetocode.com/Articles/419.aspx)
- [Szablony projektów 2.0 ASP.NET](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Omówienie nawigacji witryny ASP.NET](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Badanie ASP.NET 2.0 do nawigacji w witrynie](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Funkcje programu ASP.NET 2.0 lokacji nawigacji](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Opis ASP.NET widok stanu](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Porady: Włączanie śledzenia dla strony platformy ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Formanty użytkownika ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Liz Shulok firmy Dennis Patterson i Hilton Giesenow. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](creating-a-business-logic-layer-cs.md)
> [dalej](creating-a-data-access-layer-vb.md)
