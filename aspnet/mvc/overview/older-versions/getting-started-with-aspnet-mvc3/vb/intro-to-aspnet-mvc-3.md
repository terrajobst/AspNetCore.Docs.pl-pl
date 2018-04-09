---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: Wprowadzenie do platformy ASP.NET MVC 3 (VB) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 3be0de6ea6d49f9c0de659398190b71c36ba222a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-3-vb"></a>Wprowadzenie do platformy ASP.NET MVC 3 (VB)
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)
> 
> Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/intro-to-aspnet-mvc-3.md) tego samouczka.


Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:

- [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)

Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Projekt Visual Web Developer z kodem źródłowym VB jest dostępna powiązany z tym tematem. [Pobierz wersję VB tutaj](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Jeśli wolisz CSharp, przełącz się do [wersji języka CSharp](../cs/intro-to-aspnet-mvc-3.md) tego samouczka.

## <a name="what-youll-build"></a>Jakie będzie kompilacji

Będzie implementuje prostą aplikację listy filmów, która obsługuje tworzenie, edytowanie i wyświetlanie filmów z bazy danych. Poniżej przedstawiono dwa zrzuty ekranu aplikacji, która będzie kompilacji. Obejmuje on strona, wyświetlająca listę filmów z bazy danych:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

Aplikacja umożliwia także dodawania, edytowania i usuwania, filmów, a także szczegółowe informacje można znaleźć o pojedyncze pliki. Wszystkie scenariusze wprowadzania danych sprawdzania poprawności, aby upewnić się, że dane przechowywane w bazie danych są prawidłowe.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Dowiesz się umiejętności

Oto dowiesz się:

- Jak utworzyć nowy projekt ASP.NET MVC
- Jak utworzyć nową bazę danych przy użyciu pierwszego kodu programu Entity Framework
- Jak utworzyć ASP.NET MVC, kontrolery i widoki
- Jak pobrać i wyświetlanie danych
- Jak edytować dane i włączyć sprawdzanie poprawności danych

## <a name="getting-started"></a>Wprowadzenie

Zacznij od uruchomienia programu Visual Web Developer 2010 Express ("VWD" skrócie) i wybierz **nowy projekt** z **Start** strony.

Visual Web Developer jest IDE lub zintegrowane środowisko deweloperskie. Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz IDE do tworzenia aplikacji. Visual Web Developer ma narzędzi wzdłuż górnej pokazywanie różnych dostępnych opcji. Istnieje również menu, która umożliwia innym sposobem wykonania zadania w środowisku IDE. (Na przykład zamiast zaznaczania **nowy projekt** z **Start** strony, można skorzystać z menu i wybierz **pliku** &gt; **nowy projekt**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Można tworzyć aplikacje przy użyciu wybór języka Visual Basic lub Visual C# jako języka programowania. W tym samouczku, wybierz pozycję Visual Basic po lewej stronie, a następnie wybierz **aplikacji sieci Web programu ASP.NET MVC 3**. Nazwa projektu "MvcMovie", a następnie kliknij przycisk **OK**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

W **nowy projekt programu ASP.NET MVC 3** okno dialogowe, wybierz opcję **aplikacji internetowej**. Pozostaw **Razor** jako domyślny aparat widoku.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Kliknij przycisk **OK**. Visual Web Developer używany szablon domyślny projektu programu ASP.NET MVC, który właśnie utworzony, więc w tej chwili oferuje działającą aplikację bez żadnego działania! Jest to prosty "Witaj świecie!" Projekt, a jego dobrym miejscem do uruchomienia aplikacji.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Z **debugowania** menu, wybierz opcję **Rozpocznij debugowanie**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Zwróć uwagę, że skrót klawiaturowy można rozpocząć debugowania jest F5.

F5 powoduje, że Visual Web Developer uruchomić serwera wdrożeniowego sieci web i uruchom aplikację sieci web. Następnie VWD spowoduje uruchomienie przeglądarki i spowoduje otwarcie strony głównej aplikacji. Należy zauważyć, że na pasku adresu przeglądarki mówi `localhost` i nie coś, takich jak `example.com`. Jest to spowodowane tym `localhost` zawsze wskazuje na własnym komputerze lokalnym, działającego w takim przypadku aplikacji zostały utworzone. Po uruchomieniu projektu sieci web VWD losowy port jest używany dla projektu. Na poniższej ilustracji numer portu losowe jest 43246. Projekt użyje prawdopodobnie inny numer portu.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Poza pole ten szablon domyślny zawiera dwie strony do odwiedzenia i strony logowania podstawowe. Teraz zmienić sposób działania tej aplikacji i uzyskiwanie nieco ASP.NET MVC w procesie. Zamknij przeglądarkę i Zmieńmy kodu.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
