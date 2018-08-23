---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
title: Określanie, które pliki muszą zostać wdrożone (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Pliki muszą zostać wdrożone w środowisku programistycznym do środowiska produkcyjnego w części zależy od tego, czy aplikacja ASP.NET została skompilowana nam...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: ea918f62-c9d6-4a7f-9bc6-e054d3764b2c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
msc.type: authoredcontent
ms.openlocfilehash: 22461b681ea195225c6b7b0306b6f49956a2890b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752487"
---
<a name="determining-what-files-need-to-be-deployed-vb"></a>Określanie, które pliki muszą zostać wdrożone (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_VB.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_vb.pdf)

> Pliki muszą zostać wdrożone w środowisku programistycznym do środowiska produkcyjnego w części zależy od tego, czy aplikacja ASP.NET została skompilowana przy użyciu witryny sieci Web modelu lub Model aplikacji sieci Web. Dowiedz się więcej na temat tych dwóch projektów modeli i wpływ wdrożenia modelu projektu.


## <a name="introduction"></a>Wprowadzenie

Wdrażanie aplikacji sieci web ASP.NET pociąga za sobą kopiowanie plików związanych z ASP.NET ze środowiska projektowego do środowiska produkcyjnego. Pliki związane z ASP.NET obejmują znaczników strony sieci web platformy ASP.NET i pliki obsługi kodu i strony klienta i serwera. Pliki obsługi po stronie klienta są pliki odniesienia do stron sieci web, a następnie wysyłane bezpośrednio w przeglądarce — obrazy, pliki CSS i pliki JavaScript, na przykład. Pliki pomocy technicznej po stronie serwera zawierają te, które są używane do przetwarzania żądania po stronie serwera. W tym pliki konfiguracji, usług sieci web, pliki klas, wpisanych zestawów danych i LINQ do przechowywania plików SQL, między innymi.

Ogólnie rzecz biorąc wszystkie pliki obsługi po stronie klienta mają zostać skopiowane z środowisko programistyczne do środowiska produkcyjnego, ale skopiowania jakie pliki obsługi po stronie serwera zależy od tego, czy jawnie kompilujesz kod po stronie serwera do zestawu ( `.dll` plików) lub jeśli występują tych zestawów generowanych automatycznie. Ten samouczek przedstawia proces, pliki muszą zostać wdrożone, gdy jawnie kompilowanie kodu do zestawu i o tym kroku kompilacji, automatycznie są wykonywane.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Kompilację typu Explicit, a automatyczne kompilowanie

Strony ASP.NET web pages są podzielone na deklaratywne znaczników i kod źródłowy. Część oznaczeniu deklaracyjnym obejmuje HTML, sieci Web, formanty i składnia wiązania danych; fragment kodu zawiera procedury obsługi zdarzeń zapisaną w kodzie języka Visual Basic lub C#. Fragmenty kodu znaczników i kodu zwykle są podzielone na różne pliki: `WebPage.aspx` zawiera oznaczeniu deklaracyjnym podczas `WebPage.aspx.vb` przechowuje kod.

Należy wziąć pod uwagę strony ASP.NET o nazwie `Clock.aspx` zawiera kontrolkę etykiety, którego tekst jest właściwością bieżącą datę i godzinę, o których załadowanie strony. Część oznaczeniu deklaracyjnym (w `Clock.aspx`) zawierałoby kod znaczników dla formantu sieci Web etykiety — `<asp:Label runat="server" id="TimeLabel" />` — podczas fragment kodu (w `Clock.aspx.vb`) będzie zawierał `Page_Load` obsługę zdarzeń z następującym kodem:

[!code-vb[Main](determining-what-files-need-to-be-deployed-vb/samples/sample1.vb)]

Aby aparat programu ASP.NET do obsługi żądania dla tej strony, strony, fragment kodu ( *`WebPage`* `.aspx.vb` pliku) najpierw muszą być skompilowane. Ta kompilacja może się zdarzyć, jawnie lub automatycznie.

Jeśli kompilacja się dzieje w sposób jawny, to kod źródłowy w całej aplikacji jest skompilowany w jeden lub więcej zestawów (`.dll` pliki) znajduje się w aplikacji `Bin` katalogu. Jeśli kompilacja odbywa się automatycznie, a następnie wynikowy wygenerowany automatycznie zestaw jest domyślnie umieszczane w `Temporary ASP.NET Files` folderu, w którym znajduje się w temacie `%WINDOWS%\Microsoft.NET\Framework\<version>`, mimo że ta lokalizacja jest można konfigurować za pomocą [ &lt; Kompilacja&gt; elementu](https://msdn.microsoft.com/library/s10awwz0.aspx) w `Web.config`. Kompilację typu explicit musi mieć niektóre akcje, aby skompilować kod aplikacji ASP.NET do zestawu, a krok ten występuje przed ich wdrożeniem. Za pomocą automatycznej kompilacji procesu kompilacji występuje na serwerze sieci web, gdy najpierw dostępu do zasobu.

Niezależnie od tego, jaki model kompilacji używasz, fragment kodu znaczników wszystkich stron ASP.NET ( `WebPage.aspx` pliki) muszą być kopiowane do środowiska produkcyjnego. Za pomocą kompilację typu explicit należy skopiować się zestawy w `Bin` folder, ale nie trzeba kopiować się fragmentów kodu strony ASP.NET ( `WebPage.aspx.vb` plików). Za pomocą automatycznej kompilacji należy skopiować zapasowej fragment kodu, dzięki czemu kod jest obecny i może być kompilowane automatycznie, gdy odwiedzenia strony. Fragment kodu znaczników każdej strony sieci web platformy ASP.NET zawiera `@Page` dyrektywy z atrybutami, które wskazują, czy już jawnie skompilowanej skojarzonego kodu strony lub tego, czy należy automatycznie kompilację. W rezultacie w środowisku produkcyjnym może bezproblemowo współpracować przy użyciu dowolnego modelu kompilacji i nie należy zastosować ustawienia specjalnej konfiguracji, aby wskazać, że używany jest jawne lub automatycznej kompilacji.

Tabela 1 zawiera podsumowanie różnych plików do wdrożenia przy użyciu kompilację typu explicit, a automatyczne kompilowanie. Należy pamiętać, że niezależnie od tego, kompilacja model używany możesz zawsze wdrażać zestawy w `Bin` folderu, jeśli istnieje w tym folderze. `Bin` Folder zawiera zestawy specyficzne dla aplikacji sieci web, które zawierają kod źródłowy skompilowanych przy użyciu modelu kompilację typu explicit. `Bin` Katalog zawiera także zestawy z innych projektów i dowolne zestawy typu open source lub innych firm, mogą być używane, a te muszą znajdować się na serwerze produkcyjnym. W związku z tym ogólna zasada mówi, skopiuj `Bin` folderu do produkcji, podczas wdrażania. (Jeśli jest używany model automatyczne kompilowanie i nie używają żadnych zestawów zewnętrznych, a następnie nie będzie już `Bin` katalogu -, który jest w porządku!)

| **Kompilacja modelu** | **Wdróż plik fragment kodu znaczników?** | **Wdrażanie pliku źródła kodu?** | **Wdrażanie zestawów w `Bin` katalogu?** |
| --- | --- | --- | --- |
| Kompilację typu Explicit | Tak | Nie | Tak |
| Automatyczne kompilowanie | Tak | Tak | Tak (jeśli istnieje) |

**Tabela 1: Jakie pliki wdrożenia jest zależny od model kompilacji używany.**

## <a name="taking-a-trip-down-memory-lane"></a>Biorąc podróży w dół Lane pamięci

Jakie podejście kompilacji zależy od, w całości, jak aplikacja ASP.NET jest zarządzany w programie Visual Studio. Od czasu. Incepcja NET w roku 2000 były cztery różne wersje programu Visual Studio — programu Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 i Visual Studio 2008. Visual Studio .NET 2002 i 2003 zarządzane aplikacje ASP.NET z wykorzystaniem *projektu aplikacji sieci Web* modelu. Dostępne są następujące kluczowe funkcje modelu projektu aplikacji sieci Web:

- Pliki, że w skład projektu są zdefiniowane w pliku pojedynczego projektu. Wszystkie pliki, które nie są zdefiniowane w pliku projektu nie są uważane za część aplikacji sieci web przez program Visual Studio.
- Używa kompilację typu explicit. Kompilowanie projektu kompiluje pliki kodu w projekcie w jednym zestawie, który znajduje się w `Bin` folderu.

Firma Microsoft opublikowała program Visual Studio 2005 porzucony obsługę modelu projektu aplikacji sieci Web i zastąpiono ją przy użyciu modelu projektu witryny sieci Web. Model projektu witryny sieci Web ze zróżnicowanych gwarantować *projektu aplikacji sieci Web* modelu w następujący sposób:

- Zamiast tego pojedynczego projektu zawierający opisujemy plików projektu, używany jest system plików. Krótko mówiąc wszystkie pliki w folderze aplikacji sieci web (lub podfolderów) są traktowane jako część projektu.
- Kompilowanie projektu w programie Visual Studio nie powoduje utworzenia zestawu w `Bin` katalogu. Zamiast tego kompilowania projektu witryny sieci Web zgłasza błędy kompilacji.
- Obsługa automatycznych kompilacji. Projektów witryny sieci Web są zazwyczaj wdrożone przez skopiowanie znaczników i kod źródłowy do środowiska produkcyjnego, mimo że kod może być wstępnie skompilowanym (kompilację typu explicit).

Microsoft przywrócona modelu projektu aplikacji sieci Web w wydaniu programu Visual Studio 2005 z dodatkiem Service Pack 1. Jednak nadal obsługują tylko model projektu witryny sieci Web dla programu Visual Web Developer. Dobra wiadomość jest taka, ten limit został porzucony z dodatku Service Pack 1 dla programu Visual Web Developer 2008. Obecnie można utworzyć aplikacji ASP.NET w programie Visual Studio (i Visual Web Developer) przy użyciu modelu projektu aplikacji sieci Web lub modelu projektu witryny sieci Web. Oba modele ma swoje zalety i wady. Zapoznaj się [wprowadzenie do projektów aplikacji sieci Web: porównanie projektów witryny sieci Web i projektów aplikacji sieci Web](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) porównanie tych dwóch modeli i zdecydować, jakie modelu projektu działa najlepiej w danej sytuacji.

## <a name="exploring-the-sample-web-application"></a>Eksplorowanie przykładowej aplikacji sieci Web

Pobierania w tym samouczku obejmuje aplikacji programu ASP.NET o nazwie przeglądy książki. Witryna sieci Web naśladuje ktoś może utworzyć witrynę sieci Web hobby udostępnianie ich książki przeglądy dotyczące społeczności online. Ta aplikacja sieci web platformy ASP.NET jest bardzo proste i składa się z następującymi zasobami:

- `Web.config`, pliku konfiguracji aplikacji.
- Strony wzorcowej (`Site.master`).
- Siedem różnych stron ASP.NET:

    - ~/`Default.aspx` — Strona główna witryny.
    - ~/`About.aspx` -"o" witryna.
    - ~/`Fiction/Default.aspx` -strony z listą książek fikcja, które zostały sprawdzone.

        - ~/`Fiction/Blaze.aspx` -Przegląd powieść Richard Bachman *członkowie*.
    - ~/`Tech/Default.aspx` -strony z listą książek technologii, które zostały sprawdzone.

        - ~/`Tech/CYOW.aspx` -Przegląd *Tworzenie własnej witryny internetowej*.
        - ~/`Tech/TYASP35.aspx` -Przegląd *uczyć się ASP.NET 3.5 w ciągu 24 godzin*.
- Trzy różne pliki CSS w `Styles` folderu.
- Cztery pliki obrazów - Powered by ASP.NET logo i obrazy obejmuje trzy książek przeglądu — wszystkie znajdujące się w `Images` folderu.
- A `Web.sitemap` pliku, który definiuje mapy witryny i służy do wyświetlania menu w `Default.aspx` stron w katalogu głównym i `Fiction` i `Tech` folderów.
- Plik klasy o nazwie `BasePage.vb` definiujący podstawowej `Page` klasy. Ta klasa rozszerza funkcjonalność `Page` klasy automatycznie ustawiając `Title` właściwości na podstawie strony położenia na mapie witryny. Mówiąc, dowolnej klasy CodeBehind ASP.NET rozszerzający `BasePage` (zamiast `System.Web.UI.Page`) będzie miał jego tytuł, ustaw wartość w zależności od jego pozycja w mapy witryny. Na przykład podczas przeglądania ~ /`Tech/CYOW.aspx` stronie tytuł jest równa "Strona główna: technologia: Tworzenie własnej witryny internetowej".

Rysunek 1 pokazuje zrzut ekranu, przeglądy książki witryny sieci Web podczas wyświetlania za pośrednictwem przeglądarki. W tym miejscu zostanie wyświetlona strona ~ / Tech/TYASP35.aspx, który przegląda książki *uczyć się ASP.NET 3.5 w ciągu 24 godzin*. Nawigacji, która obejmuje górnej części strony i w menu w lewej kolumnie są oparte na struktura mapy witryny, które są zdefiniowane w `Web.sitemap`. Obraz w prawym górnym rogu jest jednym z okładki książki obrazów znajdujących się w `Images` folderu. Witryny sieci Web wygląd i działanie są definiowane za pomocą kaskadowych reguły arkusza stylów, wskazane przez pliki CSS w `Styles` folderu, chociaż nadrzędna układ strony jest zdefiniowana na stronie głównej `Site.master`.


[![Witryny sieci Web przeglądy książki oferuje recenzje pewną liczbę tytułów](determining-what-files-need-to-be-deployed-vb/_static/image2.png)](determining-what-files-need-to-be-deployed-vb/_static/image1.png)

**Rysunek 1**: przeglądy książki witryny sieci Web oferuje recenzje gamę tytuły ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](determining-what-files-need-to-be-deployed-vb/_static/image3.png))


Ta aplikacja nie korzysta z bazy danych. każdej recenzji jest implementowany jako osobne strony sieci web w aplikacji. W tym samouczku (i dalej kilka samouczków) opisano wdrażanie aplikacji sieci web, który nie ma bazy danych. Jednak w samouczku przyszłości firma Microsoft ulepszenie tej aplikacji w celu przechowywania recenzje, komentarze czytelników i inne informacje w bazie danych i przedstawimy kroki należy wykonać w celu poprawnie wdrażanie aplikacji sieci web opartej na danych.

> [!NOTE]
> Te samouczki skupić się na hostowanie aplikacji ASP.NET u dostawcy usług hosta sieci web i nie Eksplorowanie pomocniczych tematów, takich jak ASP. NET firmy system mapy witryny lub przy użyciu klasy bazowej strony. Aby uzyskać więcej informacji dotyczących tych technologii, a aby uzyskać więcej informacji na inne tematy omówione w samouczku można znaleźć w sekcji dalsze informacje na końcu każdego samouczka.


W tym samouczku pobierania zawiera dwie kopie aplikacji sieci web, każdy zaimplementowane jako innego typu projektu programu Visual Studio: BookReviewsWAP, projekt aplikacji sieci Web i BookReviewsWSP, projekt witryny sieci Web. Oba projekty zostały utworzone za pomocą programu Visual Web Developer 2008 z dodatkiem SP1 i użyć programu ASP.NET 3.5 z dodatkiem SP1. Do pracy z tych projektów uruchomić rozpakowywania zawartość na pulpicie. Aby otworzyć projekt aplikacji sieci Web (BookReviewsWAP), przejdź do `BookReviewsWAP` folder i kliknij dwukrotnie plik rozwiązania `BookReviewsWAP.sln`. Aby otworzyć projekt witryny sieci Web (BookReviewsWSP), uruchom program Visual Studio i następnie wybierz opcję otwarcia witryny sieci Web, w menu Plik, przejdź do `BookReviewsWSP` folderu na komputerze i kliknij przycisk OK.


Pozostałe dwie sekcje w tym Szukaj samouczek na temat plików, które należy skopiować do środowiska produkcyjnego, podczas wdrażania aplikacji. W dwóch następnych samouczków — [ *wdrażanie Twojej witryny przy użyciu protokołu FTP* ](deploying-your-site-using-an-ftp-client-vb.md) i [ *wdrażanie Twojej witryny przy użyciu programu Visual Studio* ](deploying-your-site-using-visual-studio-vb.md) -pokazują różne sposoby Skopiuj te pliki do dostawcy hosta sieci web.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Określanie plików do wdrożenia w projekcie aplikacji sieci Web

Model projektu aplikacji sieci Web używa kompilację typu explicit — kodu źródłowego projektu jest skompilowany w jednym zestawie każdorazowo, gdy kompilujesz aplikację. Tej kompilacji zawiera pliki związane z kodem strony ASP.NET (~ /`Default.aspx.vb`, ~ /`About.aspx.vb`i tak dalej), a także `BasePage.vb` klasy. Wynikowy zestaw o nazwie `BookReviewsWAP.dll` i znajduje się w aplikacji `Bin` katalogu.

Na rysunku 2 przedstawiono pliki, które tworzą projektu aplikacji sieci Web przeglądy książki.


[![Eksplorator rozwiązań zawiera listę plików, wchodzące w skład projektu aplikacji sieci Web.](determining-what-files-need-to-be-deployed-vb/_static/image5.png)](determining-what-files-need-to-be-deployed-vb/_static/image4.png)

**Rysunek 2**: Eksplorator rozwiązań zawiera listę plików, wchodzące w skład projektu aplikacji sieci Web


> [!NOTE]
> Jak pokazano na rysunku 2, plików z kodem strony ASP.NET nie są wyświetlane w Eksploratorze rozwiązań dla projektu aplikacji sieci Web Visual Basic. Aby wyświetlić klasy CodeBehind dla strony, kliknij prawym przyciskiem myszy na stronie w Eksploratorze rozwiązań i wybierz Wyświetl kod.


Aby wdrożyć aplikację ASP.NET opracowanych za pomocą początkowego modelu projektu aplikacji sieci Web, tworząc aplikację tak, aby jawnie skompilować najnowszych kod źródłowy do zestawu. Następnie skopiuj następujące pliki do środowiska produkcyjnego:

- Strona pliki zawierające oznaczeniu deklaracyjnym dla każdej platformy ASP.NET, takich jak ~ /`Default.aspx`, ~ /`About.aspx`i tak dalej. Ponadto kopii zapasowej oznaczeniu deklaracyjnym strony wzorcowe i kontrolki użytkownika.
- Zestawy (`.dll` pliki) w `Bin` folderu. Nie należy skopiować pliki bazy danych programu (`.pdb`) lub pliki XML może się okazać `Bin` katalogu.

Nie trzeba kopiować plików kodu źródłowego stron ASP.NET do środowiska produkcyjnego, ani nie trzeba kopiować `BasePage.vb` pliku klasy.

> [!NOTE]
> Jak pokazano na rysunku 2, `BasePage` klasy jest implementowany jako plik klasy w projekcie, umieszczony w folderze o nazwie `HelperClasses`. Gdy projekt jest skompilowany kod w `BasePage.vb` plik został skompilowany wraz z klasy CodeBehind strony ASP.NET w jednym zestawie `BookReviewsWAP.dll`. Program ASP.NET ma specjalne folder o nazwie `App_Code` przeznaczoną do przechowywania plików klasy dla projektów witryny sieci Web. Kod w `App_Code` folderu automatycznie jest kompilowany i dlatego nie należy używać w projektach aplikacji sieci Web. Zamiast tego należy umieszczać pliki klas aplikacji w normalny folder o nazwie `HelperClasses`, lub `Classes`, lub innego ogranicznika. Alternatywnie można umieścić pliki klas w osobnym projekcie biblioteki klas.


Oprócz kopiowania plików związanych z ASP.NET znaczników i zestawu w `Bin` folder, również należy skopiować pliki obsługi po stronie klienta — obrazy i pliki CSS — a także inne pliki obsługi po stronie serwera, `Web.config` i `Web.sitemap`. Te klienta - i -obsługę po stronie serwera potrzebę pliki będą kopiowane do środowiska produkcyjnego, niezależnie od tego, czy używać jawnych lub automatycznej kompilacji.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Określanie plików do wdrożenia w usłudze pliki projektu witryny sieci Web

Model projektu witryny sieci Web obsługuje automatyczne kompilowanie, funkcja nie jest dostępna przy użyciu modelu projektu aplikacji sieci Web. Za pomocą kompilację typu explicit, należy skompilować kod źródłowy projektu do zestawu i skopiować tego zestawu do środowiska produkcyjnego. Z drugiej strony przy użyciu automatycznej kompilacji możesz po prostu skopiuj kod źródłowy do środowiska produkcyjnego i jest ona kompilowana w czasie wykonywania na żądanie, zgodnie z potrzebami.

Opcja menu kompilacji w programie Visual Studio jest obecny w projektach aplikacji sieci Web i projektów witryny sieci Web. Kompilowanie projektów aplikacji sieci Web kompiluje kodu źródłowego projektu w jednym zestawie znajduje się w `Bin` katalogu; Tworzenie projektu witryny sieci Web sprawdza, czy występują błędy kompilacji, ale nie tworzy żadnych zestawów. Aby wdrożyć aplikację ASP.NET opracowanych za pomocą modelu projektu witryny sieci Web wszystkich należy zrobić to kopia odpowiednie pliki do środowiska produkcyjnego, ale będzie zachęcam Cię do pierwszej kompilacji projektu, aby upewnić się, że nie ma żadnych błędów kompilacji.

Rysunek 3 przedstawia pliki, które składają się projekt witryny sieci Web przeglądy książki.


[![Eksplorator rozwiązań zawiera listę plików, wchodzące w skład projektu witryny sieci Web.](determining-what-files-need-to-be-deployed-vb/_static/image7.png)](determining-what-files-need-to-be-deployed-vb/_static/image6.png)

**Rysunek 3**: Eksplorator rozwiązań zawiera listę plików, wchodzące w skład projektu witryny sieci Web


Wdrażanie projektu witryny sieci Web obejmuje kopiowanie wszystkich plików związanych z ASP.NET do środowiska produkcyjnego - obejmującą strony kodu znaczników dla strony ASP.NET, stron wzorcowych i formanty użytkownika wraz z plikami kodu. Również konieczne skopiowanie zapasowej klasy, takie jak `BasePage.vb`. Należy pamiętać, że `BasePage.vb` plik znajduje się w `App_Code` folder, który jest używany podczas projektów witryny sieci Web dla plików klasy specjalnego folderu ASP.NET. Specjalny folder musi zostać utworzona w środowisku produkcyjnym, jak również jako pliki klas w `App_Code` folderu w środowisku deweloperskim, muszą zostać skopiowane do `App_Code` folderu w środowisku produkcyjnym.

Oprócz kopiowania ASP.NET znaczników i plikami źródła kodu, również należy skopiować pliki obsługi po stronie klienta — obrazy i pliki CSS — a także inne pliki obsługi po stronie serwera, `Web.config` i `Web.sitemap`.

> [!NOTE]
> Projektów witryny sieci Web można również użyć kompilację typu explicit. Samouczek przyszłych zbada sposób jawnego kompilowania projektu witryny sieci Web.


## <a name="summary"></a>Podsumowanie

Wdrażanie aplikacji ASP.NET pociąga za sobą kopiowania plików na potrzeby ze środowiska projektowego w środowisku produkcyjnym. Dokładny zestaw plików, które muszą być synchronizowane z zależy od tego, czy aplikacja ASP.NET jawnie lub automatycznie kompilowania kodu. Strategia kompilacji zatrudnionych ma wpływ tego, czy program Visual Studio jest skonfigurowany do zarządzania aplikacji ASP.NET przy użyciu modelu projektu aplikacji sieci Web lub modelu projektu witryny sieci Web.

Model projektu aplikacji sieci Web używa kompilację typu explicit i kompiluje kod projektu w jednym zestawie w `Bin` folderu. W przypadku wdrażania aplikacji, fragment kodu znaczników stron ASP.NET i zawartość `Bin` folder musi zostać przesunięta do środowiska produkcyjnego; kod źródłowy w aplikacji — pliki kodu i klasy związane z kodem, na przykład — nie ma potrzeby. do skopiowania do środowiska produkcyjnego.

Modelu projektu witryny sieci Web używa domyślnie automatyczne kompilowanie, chociaż można jawnie skompilować projekt witryny sieci Web, jak widać w przyszłości samouczków. Wdrażanie aplikacji ASP.NET, która używa automatycznych kompilacji wymaga, aby fragment adiustację *i* kodu źródłowego, muszą zostać skopiowane do środowiska produkcyjnego. Kod jest kompilowany w środowisku produkcyjnym automatycznie, kiedy jest wykonywana po raz pierwszy.

Teraz, gdy zostały zbadane, które pliki muszą być synchronizowane między środowiskami deweloperskim i produkcyjnym jesteśmy gotowi wdrożyć aplikację przeglądy książki do dostawcy hosta sieci web.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Omówienie kompilacji platformy ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Formanty użytkownika ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Badanie ASP. Nawigacja po witrynie NET firmy](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Wprowadzenie do projektów aplikacji sieci Web](https://msdn.microsoft.com/library/aa730880.aspx)
- [Samouczki strony główne](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Udostępnianie kodu między stronami](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Za pomocą niestandardowej klasy bazowej dla klas związanym z kodem strony ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [System projektu witryny sieci Web w programie Visual Studio 2005: co to jest i dlaczego zrobiliśmy go?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Wskazówki: Konwersji projektu witryny sieci Web do projektu aplikacji sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Poprzednie](asp-net-hosting-options-vb.md)
> [dalej](deploying-your-site-using-an-ftp-client-vb.md)
