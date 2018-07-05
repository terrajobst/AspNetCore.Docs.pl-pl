---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Wprowadzenie do platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Zaktualizowaną wersję, jeśli w tym samouczku jest dostępna w tym miejscu za pomocą programu Visual Studio 2013. Nowe samouczku ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 51e469e131b083325bc565530d91173887769ab0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395817"
---
<a name="intro-to-aspnet-mvc-4"></a>Wprowadzenie do platformy ASP.NET MVC 4
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Zaktualizowanej wersji, jeśli w tym samouczku jest dostępny [tutaj](../../getting-started/introduction/getting-started.md) przy użyciu [programu Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nowe samouczku ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z tego samouczka.
> 
> Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web programu ASP.NET MVC 4 przy użyciu Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) lub Visual Web Developer 2010 Express Service Pack 1. Zaleca się programu Visual Studio 2012, nie trzeba będzie zainstalować niczego do ukończenia tego samouczka. Jeśli używasz programu Visual Studio 2010 należy zainstalować poniższe składniki. Można zainstalować wszystkie z nich, klikając poniższe linki:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Instalator usługi dla platformy ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [PROGRAM SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, należy zainstalować [WPI Instalator programu ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) i: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> Projekt Visual Web Developer, przy użyciu kodu źródłowego języka C# jest dostępny powiązany z tym tematem. [Pobierz wersję języka C#](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> W tym samouczku możesz uruchomić aplikację w programie Visual Studio. Można także udostępnić aplikację za pośrednictwem Internetu przez wdrożenie jej dostawcy usług hostingowych. Firma Microsoft oferuje bezpłatny internetowy hostowanie do 10 witryn sieci web w [bezpłatne konto wersji próbnej usługi Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Aby uzyskać informacje dotyczące wdrażania projektu sieci web programu Visual Studio z witryny sieci Web do Windows Azure, zobacz [tworzenie i wdrażanie witryny sieci web platformy ASP.NET i SQL Database za pomocą programu Visual Studio](https://docs.microsoft.com/dotnet/azure/). Ten samouczek pokazuje również, jak użyć migracje Code First Framework jednostki do wdrożenia bazy danych programu SQL Server do systemu Windows Azure SQL Database (dawniej SQL Azure).
> 
> Ten samouczek został napisany przez Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Jakie będziesz tworzyć

> [!NOTE]
> Zaktualizowanej wersji, jeśli w tym samouczku jest dostępny [tutaj](../../getting-started/introduction/getting-started.md) przy użyciu [programu Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nowe samouczku ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z tego samouczka.


Będzie implementowana prostą aplikację listy filmów, która obsługuje tworzenie, edytowanie, wyszukiwanie i wyświetlanie listy filmów z bazy danych. Poniżej przedstawiono dwa zrzuty ekranu aplikacji, którą utworzysz. Zawiera ona strona, która wyświetla listę filmów z bazy danych:

![](intro-to-aspnet-mvc-4/_static/image1.png)

Aplikacja umożliwia także dodawanie, edytowanie i usuwanie filmów, a także znaleźć w szczegółach dotyczących poszczególnych z nich. Wszystkie scenariusze wprowadzania danych obejmują sprawdzanie poprawności, aby upewnić się, że dane przechowywane w bazie danych są prawidłowe.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Wprowadzenie

Rozpocznij, uruchamiając program Visual Studio Express 2012 lub Visual Web Developer 2010 Express. Większość zrzutów ekranu w tej serii użyj programu Visual Studio Express 2012, ale można w tym samouczku za pomocą programu Visual Studio 2010/SP1, program Visual Studio 2012, Visual Studio Express 2012 lub Visual Web Developer 2010 Express. Wybierz **nowy projekt** z **Start** strony.

Program Visual Studio jest środowiskiem IDE lub zintegrowanego środowiska programistycznego. Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz środowisko IDE do tworzenia aplikacji. W programie Visual Studio jest narzędzi wzdłuż górnej przedstawiający różne opcje dostępne dla Ciebie. Istnieje również menu, który udostępnia inny sposób wykonywania zadań w środowisku IDE. (Na przykład, zamiast zaznaczania **nowy projekt** z **Start** strony, można użyć menu i wybrać **pliku** &gt; **nowy projekt**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Można tworzyć aplikacje przy użyciu języka Visual Basic lub Visual C# jako języka programowania. Wybierz pozycję Visual C# po lewej stronie, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC 4**. Nazwij swój projekt &quot;MvcMovie&quot; a następnie kliknij przycisk **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej**. Pozostaw **Razor** jako domyślny aparat widoku.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Kliknij przycisk **OK**. Program Visual Studio ulegał szablonu domyślnego dla projektu platformy ASP.NET MVC, który został utworzony, więc teraz utworzono działającą aplikację bez żadnego działania! Jest to prosty &quot;Hello World!&quot; projektu, a jego dobrym miejscem, aby uruchomić aplikację.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Z **debugowania** menu, wybierz opcję **Rozpocznij debugowanie**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Zauważ, że skrót klawiaturowy, aby rozpocząć debugowanie F5.

F5 powoduje, że Visual Studio w celu uruchomienia usług IIS Express i uruchom aplikację sieci web. Następnie, Visual Studio otworzy w przeglądarce i otwiera strony głównej aplikacji. Należy zauważyć, że na pasku adresu przeglądarki mówi `localhost` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` zawsze wskazuje własnego komputera lokalnego, co w tym przypadku jest uruchomiona aplikacja właśnie zbudowany. Po uruchomieniu projektu sieci web programu Visual Studio losowy port jest używany dla serwera sieci web. Na poniższej ilustracji numer portu to 41788. Po uruchomieniu aplikacji, najprawdopodobniej znajdziesz inny numer portu.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Gotową do tego szablonu domyślnego zapewnia macierzystego, skontaktuj się z pomocą i o stron. Ponadto zapewnia obsługę zarejestrowania się i zaloguj się i linki do serwisu Facebook i Twitter. Następnym krokiem jest, aby zmienić sposób działania tej aplikacji i nieco więcej informacji na temat platformy ASP.NET MVC. Zamknij przeglądarkę i zmienimy kodu.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
