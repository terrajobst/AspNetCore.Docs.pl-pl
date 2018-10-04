---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: Wprowadzenie do platformy ASP.NET MVC 3 (VB) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express Service Pack 1, czyli...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: f596dbfb534a64169767fb77fb15ecc867466c74
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577187"
---
<a name="intro-to-aspnet-mvc-3-vb"></a>Wprowadzenie do platformy ASP.NET MVC 3 (VB)
====================
Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Program ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/intro-to-aspnet-mvc-3.md) po ukończeniu tego samouczka.


Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:

- [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Program ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)

Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Projekt Visual Web Developer, przy użyciu kodu źródłowego VB jest dostępna powiązany z tym tematem. [Pobierz wersję VB tutaj](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Jeśli wolisz CSharp, przełącz się do [wersji języka CSharp](../cs/intro-to-aspnet-mvc-3.md) po ukończeniu tego samouczka.

## <a name="what-youll-build"></a>Jakie będziesz tworzyć

Będzie implementowana prostą aplikację listy filmów, która obsługuje tworzenia, edytowania i wyświetlania listy filmów z bazy danych. Poniżej przedstawiono dwa zrzuty ekranu aplikacji, którą utworzysz. Zawiera ona strona, która wyświetla listę filmów z bazy danych:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

Aplikacja umożliwia także dodawanie, edytowanie i usuwanie filmów, a także znaleźć w szczegółach dotyczących poszczególnych z nich. Wszystkie scenariusze wprowadzania danych obejmują sprawdzanie poprawności, aby upewnić się, że dane przechowywane w bazie danych są prawidłowe.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Umiejętności, których dowiesz się

Oto, dowiesz się:

- Jak utworzyć nowy projekt ASP.NET MVC
- Jak utworzyć nową bazę danych przy użyciu najpierw kod platformy Entity Framework
- Jak utworzyć platformy ASP.NET MVC, widoków i kontrolerów
- Sposób pobierania i wyświetlania danych
- Jak edytować dane i włączyć sprawdzanie poprawności danych

## <a name="getting-started"></a>Wprowadzenie

Uruchom za pomocą programu Visual Web Developer 2010 Express ("VWD" skrócie) i wybierz **nowy projekt** z **Start** strony.

Visual Web Developer jest środowiskiem IDE lub zintegrowanego środowiska programistycznego. Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz środowisko IDE do tworzenia aplikacji. W Visual Web Developer jest narzędzi wzdłuż górnej przedstawiający różne opcje dostępne dla Ciebie. Istnieje również menu, który udostępnia inny sposób wykonywania zadań w środowisku IDE. (Na przykład, zamiast zaznaczania **nowy projekt** z **Start** strony, można użyć menu i wybrać **pliku** &gt; **nowy projekt**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Można tworzyć aplikacje przy użyciu wybranego języka Visual Basic lub Visual C# jako języka programowania. W tym samouczku po lewej stronie wybierz pozycję Visual Basic, a następnie wybierz **aplikacji sieci Web programu ASP.NET MVC 3**. Nazwij swój projekt "MvcMovie", a następnie kliknij przycisk **OK**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

W **nowego projektu programu ASP.NET MVC 3** okno dialogowe, wybierz opcję **aplikacji internetowej**. Pozostaw **Razor** jako domyślny aparat widoku.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Kliknij przycisk **OK**. Visual Web Developer używać szablonu domyślnego dla projektu platformy ASP.NET MVC, który został utworzony, więc teraz utworzono działającą aplikację bez żadnego działania! Jest to proste "Hello World!" Projekt, a jego dobrym miejscem, aby uruchomić aplikację.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Z **debugowania** menu, wybierz opcję **Rozpocznij debugowanie**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Zauważ, że skrót klawiaturowy, aby rozpocząć debugowanie F5.

F5 powoduje, że Visual Web Developer uruchomić serwera wdrożeniowego sieci web i uruchomić aplikację sieci web. VWD następnie otworzy w przeglądarce i otwiera strony głównej aplikacji. Należy zauważyć, że na pasku adresu przeglądarki mówi `localhost` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` zawsze wskazuje własnego komputera lokalnego, co w tym przypadku jest uruchomiona aplikacja właśnie zbudowany. Po uruchomieniu projektu sieci web VWD losowy port jest używany dla projektu. Na poniższej ilustracji liczba losowy port jest 43246. Projekt prawdopodobnie będziesz używasz inny numer portu.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Gotowych ten szablon domyślny zawiera dwie strony do odwiedzenia i strony logowania podstawowe. Możemy zmienić sposób działania tej aplikacji i Poznaj nieco platformy ASP.NET MVC w procesie. Zamknij przeglądarkę i zmienimy kodu.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
