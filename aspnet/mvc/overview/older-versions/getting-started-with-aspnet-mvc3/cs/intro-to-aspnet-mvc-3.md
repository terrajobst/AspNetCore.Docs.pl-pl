---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Wprowadzenie do platformy ASP.NET MVC 3 (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express Service Pack 1, czyli...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 53306318ab1a782d3605876aac53ec903d053269
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576290"
---
<a name="intro-to-aspnet-mvc-3-c"></a>Wprowadzenie do platformy ASP.NET MVC 3 (C#)
====================
Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.
> 
> 
> Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Program ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer, przy użyciu kodu źródłowego języka C# jest dostępny powiązany z tym tematem. [Pobierz wersję języka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz języka Visual Basic, przełącz się do [wersji języka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) po ukończeniu tego samouczka.


## <a name="what-youll-build"></a>Jakie będziesz tworzyć

Będzie implementowana prostą aplikację listy filmów, która obsługuje tworzenia, edytowania i wyświetlania listy filmów z bazy danych. Poniżej przedstawiono dwa zrzuty ekranu aplikacji, którą utworzysz. Zawiera ona strona, która wyświetla listę filmów z bazy danych:

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

Aplikacja umożliwia także dodawanie, edytowanie i usuwanie filmów, a także znaleźć w szczegółach dotyczących poszczególnych z nich. Wszystkie scenariusze wprowadzania danych obejmują sprawdzanie poprawności, aby upewnić się, że dane przechowywane w bazie danych są prawidłowe.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Umiejętności, których dowiesz się

Oto, dowiesz się:

- Jak utworzyć nowy projekt ASP.NET MVC.
- Jak utworzyć platformy ASP.NET MVC, widoków i kontrolerów.
- Jak utworzyć nową bazę danych przy użyciu modelu Entity Framework Code First.
- Jak pobrać i wyświetlić dane.
- Jak edytować dane i włączyć sprawdzanie poprawności danych.

## <a name="getting-started"></a>Wprowadzenie

Uruchom za pomocą programu Visual Web Developer 2010 Express ("Visual Web Developer" skrócie) i wybierz **nowy projekt** z **Start** strony.

Visual Web Developer jest środowiskiem IDE lub zintegrowanego środowiska programistycznego. Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz środowisko IDE do tworzenia aplikacji. W Visual Web Developer jest narzędzi wzdłuż górnej przedstawiający różne opcje dostępne dla Ciebie. Istnieje również menu, który udostępnia inny sposób wykonywania zadań w środowisku IDE. (Na przykład, zamiast zaznaczania **nowy projekt** z **Start** strony, można użyć menu i wybrać **pliku** &gt; **nowy projekt**.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Można tworzyć aplikacje przy użyciu języka Visual Basic lub Visual C# jako języka programowania. Wybierz pozycję Visual C# po lewej stronie, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC 3**. Nazwij swój projekt "MvcMovie", a następnie kliknij przycisk **OK**. (Jeśli wolisz języka Visual Basic, przełącz się do [wersji języka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) po ukończeniu tego samouczka.)

![](intro-to-aspnet-mvc-3/_static/image5.png)

W **nowego projektu programu ASP.NET MVC 3** okno dialogowe, wybierz opcję **aplikacji internetowej**. Sprawdź **HTML5 użycia znaczników** i pozostawić **Razor** jako domyślny aparat widoku.

![](intro-to-aspnet-mvc-3/_static/image6.png)

Kliknij przycisk **OK**. Visual Web Developer używać szablonu domyślnego dla projektu platformy ASP.NET MVC, który został utworzony, więc teraz utworzono działającą aplikację bez żadnego działania! Jest to proste "Hello World!" Projekt, a jego dobrym miejscem, aby uruchomić aplikację.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

Z **debugowania** menu, wybierz opcję **Rozpocznij debugowanie**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Zauważ, że skrót klawiaturowy, aby rozpocząć debugowanie F5.

F5 powoduje, że Visual Web Developer uruchomić serwera wdrożeniowego sieci web i uruchomić aplikację sieci web. Visual Web Developer następnie otworzy w przeglądarce i otwiera strony głównej aplikacji. Należy zauważyć, że na pasku adresu przeglądarki mówi `localhost` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` zawsze wskazuje własnego komputera lokalnego, co w tym przypadku jest uruchomiona aplikacja właśnie zbudowany. Po uruchomieniu projektu sieci web dla programu Visual Web Developer losowy port jest używany dla serwera sieci web. Na poniższej ilustracji liczba losowy port jest 43246. Po uruchomieniu aplikacji, najprawdopodobniej znajdziesz inny numer portu.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Gotową do tego szablonu domyślnego zapewnia dwie strony, aby odwiedzić i strony logowania podstawowe. Następnym krokiem jest zmienić sposób działania tej aplikacji i Poznaj nieco platformy ASP.NET MVC w procesie. Zamknij przeglądarkę i zmienimy kodu.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
