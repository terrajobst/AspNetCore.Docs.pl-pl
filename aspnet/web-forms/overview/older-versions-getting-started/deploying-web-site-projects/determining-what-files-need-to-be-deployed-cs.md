---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: Określanie pliki muszą być wdrożone (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Pliki potrzebne do wdrożenia środowiska programowania do środowiska produkcyjnego zależy od w części określa, czy aplikacja ASP.NET została skompilowana nam...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: ff5f1d7d156efa12d97382db56211a07c43178fd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="determining-what-files-need-to-be-deployed-c"></a>Określanie pliki muszą być wdrożone (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> Pliki potrzebne do wdrożenia środowiska programowania do środowiska produkcyjnego zależy od w części określa, czy aplikacja ASP.NET została skompilowana przy użyciu witryny sieci Web modelu lub Model aplikacji sieci Web. Dowiedz się więcej o tych dwóch projektu modeli i wpływ modelu projektu wdrożenia.


## <a name="introduction"></a>Wprowadzenie

Wdrażanie aplikacji sieci web ASP.NET wymaga kopiowanie plików związanych z platformy ASP.NET z Środowisko deweloperskie do środowiska produkcyjnego. Pliki związane z ASP.NET obejmują znaczników strony sieci web ASP.NET i kod i strony klienta i serwera plików obsługi. Pliki obsługi po stronie klienta są pliki do stron sieci web, a następnie wysyłane bezpośrednio do przeglądarki - obrazów, plików CSS i JavaScript plików, na przykład. Pliki pomocy technicznej po stronie serwera obejmują te, które są używane do przetwarzania żądania po stronie serwera. W tym pliki konfiguracji, usług sieci web, pliki klas, wpisanych zestawów danych i LINQ do plików SQL, między innymi.

Ogólnie rzecz biorąc wszystkie pliki pomocy technicznej po stronie klienta powinny zostać skopiowane z Środowisko deweloperskie do środowiska produkcyjnego, ale uzyskać skopiowane jakie pliki pomocy technicznej po stronie serwera zależy od tego, czy jawnie kompilacja kodu po stronie serwera do zestawu ( `.dll` pliku) lub jeśli występują tych zestawów generowanych automatycznie. Ten samouczek przedstawia pliki potrzebne do wdrożenia podczas jawnego kompilowania kodu w zespół i ten krok kompilacji, automatycznie są wykonywane po.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Jawne kompilacji i automatyczne kompilowanie

Strony ASP.NET web pages są podzielone na deklaratywne znaczników i kod źródłowy. Fragment znacznika deklaratywne obejmuje HTML, sieci Web, kontrolek i składnia wiązania danych; fragment kodu zawiera procedury obsługi zdarzeń zapisywane w kodzie języka Visual Basic lub C#. Fragmenty kodu znaczników i kodu zwykle są podzielone na różne pliki: `WebPage.aspx` zawiera deklaratywne znaczników podczas `WebPage.aspx.cs` przechowuje kod.

Należy wziąć pod uwagę strony ASP.NET o nazwie Clock.aspx, który zawiera formant etykiety, w których właściwość tekst jest ustawiona na bieżącą datę i godzinę załadowanie strony. Fragment znacznika deklaratywne (w `Clock.aspx`) zawierałoby kod znaczników dla formantu etykiety sieci Web -`<asp:Label runat="server" id="TimeLabel" />` — podczas fragment kodu (w `Clock.aspx.cs`) musi `Page_Load` programu obsługi zdarzeń z następującym kodem:

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

Aby aparatu ASP.NET do obsługi żądania dla tej strony, strony fragment kodu ( `WebPage.aspx.cs` pliku) musi najpierw zostać skompilowany. Ta kompilacja może się zdarzyć, jawnie lub automatycznie.

Jeśli kompilacja jawnie się stanie, a następnie całej aplikacji kod źródłowy jest kompilowany do jednego lub więcej zestawów (`.dll` pliki) znajduje się w aplikacji `Bin` katalogu. Jeśli kompilacja odbywa się automatycznie, a następnie powstałe w ten sposób automatycznego generowania zestaw jest domyślnie umieszczane w `Temporary ASP.NET` folderu plików, w którym można znaleźć w folderze `%WINDOWS%\Microsoft.NET\Framework\`  *&lt;wersji&gt;*, Mimo że można konfigurować za pomocą tej lokalizacji [ `<compilation>` elementu](https://msdn.microsoft.com/library/s10awwz0.aspx) w `Web.config`. Kompilację typu explicit musi mieć niektóre akcje, aby skompilować kod aplikacji ASP.NET do zestawu, a ten krok występuje przed ich wdrożeniem. Z automatycznego tworzenia procesu kompilacji występuje na serwerze sieci web, gdy najpierw dostępu do zasobu.

Niezależnie od tego, jaki model kompilacji w przypadku użycia, znaczników część wszystkich stron ASP.NET ( `WebPage.aspx` pliki) mają zostać skopiowane do środowiska produkcyjnego. Z kompilację typu explicit należy skopiować się zestawów w `Bin` folder, ale nie trzeba kopiować się fragmentów kodu strony ASP.NET ( `WebPage.aspx.cs` plików). Z automatyczne kompilowanie należy skopiować zapasowej części kodu, aby kod jest obecny i mogą być kompilowane automatycznie, gdy odwiedzenia strony. Część znacznika każdej strony sieci web platformy ASP.NET zawiera `@Page` dyrektywy z atrybutami, które wskazują, czy strony skojarzony kod został skompilowany już jawnie lub czy musi być tworzone automatycznie. W związku z tym środowisku produkcyjnym może bezproblemowo współpracować z modelem albo kompilacji i nie trzeba zastosować wszystkie ustawienia konfiguracji specjalne, aby wskazać służy kompilacji jawne lub automatyczne.

Tabela 1 przedstawiono różne pliki do wdrożenia przy użyciu kompilację typu explicit i automatyczne kompilowanie. Należy pamiętać, że niezależnie od kompilacji modelu używane należy zawsze wdrażać zestawów w `Bin` folderu, jeśli istnieje w tym folderze. `Bin` Folder zawiera zestawy specyficzne dla aplikacji sieci web, które obejmują kod źródłowy skompilowany, korzystając z modelu kompilację typu explicit. `Bin` Katalog zawiera również zestawy z innych projektów i dowolne zestawy open source lub innych firm, może być używany, a te muszą znajdować się na serwerze produkcyjnym. W związku z tym ogólne zasadą, skopiuj `Bin` folderu do produkcji, podczas wdrażania. (Jeśli jest używany model automatyczne kompilacji i nie są używane wszystkie zestawy zewnętrzne, nie będziesz mieć `Bin` katalog - OK!)

| **Model kompilacji** | **Wdróż plik części znaczników?** | **Wdrażanie pliku źródła kodu?** | **Wdrażanie zestawów w `Bin` katalogu?** |
| --- | --- | --- | --- |
| Jawne kompilacji | Tak | Nie | Tak |
| Automatyczne kompilowanie | Tak | Tak | Tak (jeśli istnieje) |

**Tabela 1:** jakie pliki wdrożeniem zależy od modelu kompilacji używany.

## <a name="taking-a-trip-down-memory-lane"></a>Biorąc podróży dół Lane pamięci

Jakie podejście kompilacji zależy, w części sposób zarządzania aplikacji ASP.NET w programie Visual Studio. Od czasu. Incepcja NET w roku 2000 były cztery różne wersje programu Visual Studio — Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 i Visual Studio 2008. Visual Studio .NET 2002 i 2003 zarządzanych aplikacji ASP.NET przy użyciu *modelu projektu aplikacji sieci Web*. Najważniejsze funkcje modelu projektu aplikacji sieci Web są:

- Pliki, że w skład projektu są zdefiniowane w pliku pojedynczego projektu. Wszystkie pliki, które nie są zdefiniowane w pliku projektu nie są uważane za część aplikacji sieci web przez program Visual Studio.
- Używa kompilację typu explicit. Tworzenie projektu kompiluje pliki kodu w projekcie do jednego zestawu, który znajduje się w `Bin` folderu.

Firma Microsoft opublikowała program Visual Studio 2005 porzucony obsługę modelu projektu aplikacji sieci Web i zastąpione modelu projekt witryny sieci Web. Projekt witryny sieci Web modelu zróżnicowane sam z modelu projektu aplikacji sieci Web w następujący sposób:

- Zamiast tworzenia projektów, które wskazuje pliki projektu, zamiast niego jest używana do systemu plików. Krótko mówiąc wszystkie pliki w folderze aplikacji sieci web (lub podfolderów) są traktowane jako część projektu.
- Tworzenie projektu programu Visual Studio nie powoduje utworzenia zestawu w `Bin` katalogu. Zamiast tego wówczas skompilowanie projektu witryny sieci Web raporty błędy kompilacji.
- Obsługa automatycznego kompilacji. Projektów witryny sieci Web zwykle są wdrażane przez kopiowanie kodu znaczników i źródła do środowiska produkcyjnego, mimo że można wstępnie skompilowanym (kompilację typu explicit).

Microsoft przywrócona modelu projektu aplikacji sieci Web w wydaniu dodatku Service Pack 1 dla programu Visual Studio 2005. Jednak nadal obsługują tylko model projekt witryny sieci Web programu Visual Web Developer. Dobre wieści jest, że ten limit został porzucony z dodatku Service Pack 1 dla programu Visual Web Developer 2008. Obecnie można utworzyć aplikacji ASP.NET w Visual Studio (i Visual Web Developer) przy użyciu modelu projektu aplikacji sieci Web lub modelu projekt witryny sieci Web. Oba modele ma ich zalet i wad. Zapoznaj się [wprowadzenie do projektów aplikacji sieci Web: porównanie projektów witryny sieci Web i projekty aplikacji sieci Web](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) porównanie dwóch modeli i pomóc zdecydować, jaki model projektu działa najlepiej w danej sytuacji.

## <a name="exploring-the-sample-web-application"></a>Eksploracja przykładowej aplikacji sieci Web

Pobranie w tym samouczku obejmuje aplikacji programu ASP.NET o nazwie przeglądami książki. Witryna sieci Web, którego ktoś może utworzyć witryny sieci Web zainteresowania udostępnianie ich książki przegląda społeczności online. Ta aplikacja sieci web programu ASP.NET jest bardzo prosty i składa się z następujących zasobów:

- `Web.config`, pliku konfiguracji aplikacji.
- Strony wzorcowej (`Site.master`).
- Siedmiu różnych stron ASP.NET: 

    - ~`/Default.aspx`— Strona główna witryny.
    - ~`/About.aspx` -"temat" strony.
    - ~`/Fiction/Default.aspx` -stronę książek fikcja, które zostały sprawdzone. 

        - ~`/Fiction/Blaze.aspx` -Przegląd powieść Richard Bachman *członkowie*.
    - ~/`Tech/Default.aspx` -stronę książek technologii, które zostały sprawdzone. 

        - ~/`Tech/CYOW.aspx`-Przegląd *Tworzenie własnej witryny internetowej*.
        - ~/`Tech/TYASP35.aspx` -Przegląd *nauczyć się ASP.NET 3.5 w ciągu 24 godzin*.
- Trzy różne pliki CSS w folderze style.
- Cztery obrazu — obsługiwane przez program ASP.NET logo i obrazy obejmuje trzy książek je przejrzeć — wszystkie pliki znajdujące się w `Images` folderu.
- A `Web.sitemap` pliku, który definiuje mapy witryny i służy do wyświetlania menu w `Default.aspx` stron w katalogu głównym i `Fiction` i `Tech` folderów.
- Plik klasy o nazwie `BasePage.cs` definiuje podstawowej `Page` klasy. Ta klasa rozszerza funkcjonalność `Page` klasy automatycznie ustawiając `Title` na podstawie właściwości na pozycji strony mapy witryny. Mówiąc, każda klasa CodeBehind ASP.NET który rozszerza `BasePage` (zamiast `System.Web.UI.Page`) będzie miał tytułu ustaw wartość w zależności od jej położenie w mapy witryny. Na przykład podczas przeglądania ~ /`Tech/CYOW.aspx` tytuł strony, ma wartość "Home: technologii: Tworzenie własnej witryny internetowej".

Rysunek 1 pokazuje zrzut ekranu przeglądami książki witryny sieci Web podczas wyświetlania za pośrednictwem przeglądarki. W tym miejscu zostanie wyświetlona strona ~ /`Tech/TYASP35.aspx`, który monitoruje książce *nauczyć się ASP.NET 3.5 w ciągu 24 godzin*. Łączach rozciągających się na górze strony i z menu w lewej kolumnie są oparte na strukturę mapy witryny zdefiniowaną w `Web.sitemap`. Obraz w prawym górnym rogu ekranu jest jednym z okładce książki obrazy znajdujące się w `Images` folderu. Witryny sieci Web wyglądu i działania są definiowane za pomocą wskazane przez pliki CSS w folderze style, podczas gdy nadrzędna układ strony jest zdefiniowana na stronie głównej reguły arkusza stylów kaskadowych `Site.master`.


[![Witryny sieci Web sprawdza książki oferuje przeglądami na gamę tytułów](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Rysunek 1.** witryny sieci Web sprawdza książki oferuje przeglądami na gamę tytułów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](determining-what-files-need-to-be-deployed-cs/_static/image3.png))


Ta aplikacja nie używa bazy danych; każdego przeglądu jest implementowany jako osobne strony sieci web w aplikacji. W tym samouczku (i dalej samouczki kilka) przeprowadzenie wdrażanie aplikacji sieci web, która nie ma bazy danych. Jednak w przyszłości samouczek możemy zwiększy tej aplikacji do przechowywania recenzji, komentarze czytelników i inne informacje w bazie danych i zastosuje, jakie kroki należy wykonać poprawnie wdrażania aplikacji sieci web opartych na danych.

> [!NOTE]
> Te samouczki skupić się na hosting aplikacji ASP.NET z dostawcą hosta sieci web, a nie Eksploruj pomocniczych tematów, takich jak ASP. System mapy witryny w sieci lub za pomocą podstawowej `Page` klasy. Aby uzyskać więcej informacji dotyczących tych technologii oraz więcej tła na inne tematy w samouczku zapoznaj się z rozdziałem dalsze informacje, pod koniec każdego samouczka.


W tym samouczku pobierania ma dwie kopie aplikacji sieci web, każdy zaimplementowane jako inny typ projektu programu Visual Studio: BookReviewsWAP, projekt aplikacji sieci Web i BookReviewsWSP, projekt witryny sieci Web. Oba projekty zostały utworzone za pomocą programu Visual Web Developer 2008 z dodatkiem SP1 i użyj ASP.NET 3.5 z dodatkiem SP1. Do pracy z tymi projektami uruchomić rozpakować zawartość na pulpicie. Aby otworzyć projekt aplikacji sieci Web (BookReviewsWAP), przejdź do folderu BookReviewsWAP i kliknij dwukrotnie plik rozwiązania `BookReviewsWAP.sln`. Aby otworzyć projekt witryny sieci Web (BookReviewsWSP), uruchom program Visual Studio, a następnie z menu Plik wybierz opcję Otwórz witrynę sieci Web, przejdź do `BookReviewsWSP` folder na pulpicie i kliknij przycisk OK.


Pozostałe dwie sekcje tego samouczka wyglądają na jakie pliki należy skopiować do środowiska produkcyjnego, w przypadku wdrażania aplikacji. Następne dwa samouczki - *[wdrażanie Your lokacji przy użyciu FTP](deploying-your-site-using-an-ftp-client-cs.md)* i *[wdrażanie Your lokacji za pomocą programu Visual Studio](deploying-your-site-using-visual-studio-cs.md)* -przedstawiono różne sposoby Skopiuj te pliki do dostawcy hosta sieci web.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Określanie plików do wdrażania projektu aplikacji sieci Web

Model projektu aplikacji sieci Web używa kompilację typu explicit — kod źródłowy projektu jest skompilowany w jednym zestawie zawsze, gdy kompilowania aplikacji. Ta kompilacja obejmuje strony ASP.NET plików z kodem (~ /`Default.aspx.cs`, ~ /`About.aspx.cs`i tak dalej), a także `BasePage.cs` klasy. Wynikowy zestaw o nazwie BookReviewsWAP.dll i znajduje się w aplikacji `Bin` katalogu.

Na rysunku 2 przedstawiono pliki, które tworzą projekt aplikacji sieci Web przeglądami książki.


[![Eksplorator rozwiązań wymieniono pliki wchodzące w skład projektu aplikacji sieci Web](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Rysunek 2**: Solution Explorer wyświetla listę plików, które obejmują projektu aplikacji sieci Web


Aby wdrożyć aplikację ASP.NET, utworzony przy użyciu start modelu projektu aplikacji sieci Web przez tworzenie aplikacji tak, aby jawnie skompilować najnowszych kodu źródłowego do zestawu. Następnie skopiuj następujące pliki do środowiska produkcyjnego:

- Strona plików, które zawierają deklaratywne znaczników dla każdej platformy ASP.NET, takich jak ~ /`Default.aspx`, ~ /`About.aspx`i tak dalej. Ponadto kopii zapasowej deklaratywne znaczników stron wzorcowych i kontrolki użytkownika.
- Zestawy (`.dll` pliki) w `Bin` folderu. Nie trzeba kopiować plików bazy danych programu (`.pdb`) lub pliki XML może się okazać `Bin` katalogu.

Nie trzeba kopiować plików kodu źródłowego stron ASP.NET do środowiska produkcyjnego i nie jest wymagana do skopiowania `BasePage.cs` pliku klasy.

> [!NOTE]
> Jak pokazano na rysunku 2, `BasePage` klasy jest zaimplementowany jako plik klasy w projekcie, umieszczane w folderze o nazwie `HelperClasses`. Podczas kompilowania projektu kodu w `BasePage.cs` skompilować pliku wraz z klasy związane z kodem strony ASP.NET w jednym zestawie `BookReviewsWAP.dll.` ASP.NET ma specjalne folder o nazwie `App_Code` zaprojektowano w celu przechowywania plików klasy dla sieci Web Projekty lokacji. Kod w `App_Code` folderu automatycznie jest skompilowany i dlatego nie powinien być używany z projekty aplikacji sieci Web. Zamiast tego należy umieszczać pliki klasy aplikacji w normalny folder o nazwie `HelperClasses`, lub `Classes`, lub coś podobnego. Alternatywnie można umieścić pliki klas w oddzielnych projektu biblioteki klas.


Oprócz kopiowania plików związanych z ASP.NET znaczników i zestawu w `Bin` folder, również należy skopiować pliki pomocy technicznej po stronie klienta — obrazów i plików CSS — oraz inne pliki obsługi po stronie serwera, `Web.config` i `Web.sitemap`. Te klienta — i po stronie serwera obsługuje potrzeby plików do skopiowania do środowiska produkcyjnego, niezależnie od tego, czy używać kompilacji jawne lub automatyczne.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Określanie plików do wdrożenia dla plików projektu witryny sieci Web

Model projekt witryny sieci Web obsługuje automatyczne kompilowanie, funkcja nie jest dostępny przy użyciu modelu projektu aplikacji sieci Web. Z kompilację typu explicit należy skompilować kod źródłowy projektu do zestawu i skopiuj tego zestawu do środowiska produkcyjnego. Z drugiej strony, z automatyczne kompilowanie możesz po prostu skopiuj kod źródłowy do środowiska produkcyjnego i jest kompilowana w czasie wykonywania na żądanie, w razie potrzeby.

Opcja menu kompilacji programu Visual Studio znajduje się w zarówno projekty aplikacji sieci Web i witryna sieci Web. Kompilowanie projektów aplikacji sieci Web kompiluje kod źródłowy projektu w jednym zestawie znajduje się w `Bin` katalogu; Tworzenie projektu witryny sieci Web sprawdza błędy kompilacji, ale nie powoduje żadnych zestawów. Aby wdrożyć aplikację ASP.NET, utworzony przy użyciu wszystkich modelu projekt witryny sieci Web należy wykonać jest kopiowania odpowiednie pliki do środowiska produkcyjnego, ale I czy zachęca do pierwszej kompilacji projektu, aby upewnić się, że nie ma żadnych błędów kompilacji.

Rysunek 3 przedstawia pliki, które tworzą projekt witryny sieci Web przeglądami książki.


 [![Eksplorator rozwiązań wymieniono pliki wchodzące w skład projekt witryny sieci Web](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Rysunek 3**: Solution Explorer wyświetla listę plików, które obejmują projekt witryny sieci Web


Wdrażanie projektu witryny sieci Web obejmuje kopiowanie wszystkich plików związanych z ASP.NET do środowiska produkcyjnego — zawierającą strony kodu znaczników dla stron ASP.NET, stron wzorcowych i kontrolek użytkownika wraz z ich pliki kodu. Należy także skopiować się żadnych plików klasy, takie jak BasePage.cs. Należy pamiętać, że `BasePage.cs` plik znajduje się w `App_Code` folderu, który jest folderem specjalnym ASP.NET użycia w projektach witryny sieci Web plików klasy. Folderu specjalnego musi zostać utworzona w środowisku produkcyjnym, a także jako pliki klas w `App_Code` folderu w środowisku programistycznym należy skopiować do `App_Code` folderu w środowisku produkcyjnym.

Oprócz kopiowania plików kodu znaczników i źródła programu ASP.NET, należy również skopiować pliki pomocy technicznej po stronie klienta — obrazów i plików CSS — oraz inne pliki obsługi po stronie serwera, `Web.config` i `Web.sitemap`.

> [!NOTE]
> Witryna sieci Web można również użyć kompilację typu explicit. Samouczek przyszłych zbada jak jawnie skompilować projekt witryny sieci Web.


## <a name="summary"></a>Podsumowanie

Wdrażanie aplikacji ASP.NET wymaga kopiowanie niezbędne pliki środowiska programowania do środowiska produkcyjnego. Dokładny zestaw plików, które muszą być zsynchronizowane zależy od tego, czy aplikacja ASP.NET jawnie lub automatycznie kompilowania kodu. Strategia kompilacji zastosować ma wpływ czy Visual Studio jest skonfigurowany do zarządzania aplikacją ASP.NET przy użyciu modelu projektu aplikacji sieci Web lub modelu projekt witryny sieci Web.

Modelu projektu aplikacji sieci Web używa kompilację typu explicit i kompilowaniem kodu projektu w jednym zestawie w `Bin` folderu. W przypadku wdrażania aplikacji, fragment kodu znaczników stron ASP.NET i zawartość `Bin` folder musi zostać przeniesiony do środowiska produkcyjnego; kod źródłowy w aplikacji — pliki kodu i klasy związane z kodem, na przykład — nie ma potrzeby ma zostać skopiowany do środowiska produkcyjnego.

Model projekt witryny sieci Web używa automatyczne kompilowanie domyślnie, mimo że prawdopodobnie jawnie skompilować projekt witryny sieci Web, zostanie wyświetlone w przyszłości samouczki. Wdrażanie aplikacji ASP.NET, która używa automatycznego kompilacji wymaga, aby części znaczników *i* kodu źródłowego należy skopiować do środowiska produkcyjnego. Kod jest automatycznie kompilowana w środowisku produkcyjnym, żądanie po raz pierwszy.

Teraz, gdy zostały zbadane, co pliki muszą być synchronizowane między środowisk projektowania i produkcji możemy są gotowe do wdrożenia aplikacji przeglądami książki dostawcy hosta sieci web.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Omówienie kompilacji programu ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Formanty użytkownika ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Badanie ASP. Nawigowanie po witrynie przez sieć](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Wprowadzenie do projektów aplikacji sieci Web](https://msdn.microsoft.com/library/aa730880.aspx)
- [Samouczki strony wzorca](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Udostępnianie kodu między stronami](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Za pomocą niestandardowej klasy podstawowej dla klas CodeBehind stron ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [System projektu witryny sieci Web w programie Visual Studio 2005: co to jest i dlaczego możemy to zrobić?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Wskazówki: Konwersji projektu witryny sieci Web do projektu aplikacji sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Poprzednie](asp-net-hosting-options-cs.md)
> [dalej](deploying-your-site-using-an-ftp-client-cs.md)
