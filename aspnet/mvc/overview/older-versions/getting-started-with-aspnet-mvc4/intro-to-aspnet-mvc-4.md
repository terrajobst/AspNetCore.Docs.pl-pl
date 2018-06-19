---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Wprowadzenie do platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Zaktualizowanej wersji, jeśli w tym samouczku jest dostępna, w tym miejscu przy użyciu programu Visual Studio 2013. Samouczek nowej używa platformy ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 519bac22ba2607931c5f3123b9b567859a2b3d1c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869052"
---
<a name="intro-to-aspnet-mvc-4"></a>Wprowadzenie do platformy ASP.NET MVC 4
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> Zaktualizowanej wersji, jeśli jest dostępna w tym samouczku [tutaj](../../getting-started/introduction/getting-started.md) przy użyciu [programu Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nowe samouczku platformy ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z tego samouczka.
> 
> W tym samouczku udzieli Ci podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC 4 przy użyciu Microsoft [programu Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) lub Visual Web Developer 2010 Express Service Pack 1. Zaleca się programu Visual Studio 2012, nie trzeba będzie zainstalować niczego na ukończenie tego samouczka. Jeśli używasz programu Visual Studio 2010 należy zainstalować poniższe składniki. Można zainstalować wszystkie z nich, klikając poniższe łącza:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Instalator usługi dla platformy ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [PROGRAM SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, należy zainstalować [usługi Instalator programu ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) i: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> Projekt Visual Web Developer z kodu źródłowego C# jest dostępna powiązany z tym tematem. [Pobierz wersję języka C#](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> W samouczku uruchomieniu aplikacji w programie Visual Studio. Można również udostępnić aplikacji w Internecie przez wdrożenie jej do dostawcy hostingu. Firma Microsoft oferuje usługi hostingu sieci web wolnego maksymalnie 10 witryn sieci web w [bezpłatnego konta wersji próbnej systemu Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Aby uzyskać informacje dotyczące wdrażania projektu sieci web programu Visual Studio do witryny sieci Web systemu Windows Azure, zobacz [tworzenia i wdrażania witryn sieci web ASP.NET i bazy danych SQL z programem Visual Studio](https://docs.microsoft.com/dotnet/azure/). Ten samouczek również przedstawia sposób użycia migracje Code First Framework jednostki do wdrażania bazy danych SQL Server do systemu Windows Azure SQL Database (poprzednio Azure SQL).
> 
> W tym samouczku zapisał Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Jakie będzie kompilacji

> [!NOTE]
> Zaktualizowanej wersji, jeśli jest dostępna w tym samouczku [tutaj](../../getting-started/introduction/getting-started.md) przy użyciu [programu Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nowe samouczku platformy ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z tego samouczka.


Będzie implementuje prostą aplikację listy filmów, która obsługuje tworzenia, edytowania, wyszukiwanie i wyświetlanie filmów z bazy danych. Poniżej przedstawiono dwa zrzuty ekranu aplikacji, która będzie kompilacji. Obejmuje on strona, wyświetlająca listę filmów z bazy danych:

![](intro-to-aspnet-mvc-4/_static/image1.png)

Aplikacja umożliwia także dodawania, edytowania i usuwania, filmów, a także szczegółowe informacje można znaleźć o pojedyncze pliki. Wszystkie scenariusze wprowadzania danych sprawdzania poprawności, aby upewnić się, że dane przechowywane w bazie danych są prawidłowe.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Wprowadzenie

Rozpocząć od uruchomienia programu Visual Studio Express 2012 lub Visual Web Developer 2010 Express. Większość zrzuty ekranu w tej serii użyj programu Visual Studio Express 2012, ale można wykonania kroków tego samouczka z programu Visual Studio 2010/SP1, program Visual Studio 2012, Visual Studio Express 2012 lub Visual Web Developer 2010 Express. Wybierz **nowy projekt** z **Start** strony.

Program Visual Studio jest IDE lub zintegrowane środowisko deweloperskie. Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz IDE do tworzenia aplikacji. W programie Visual Studio jest narzędzi wzdłuż górnej pokazywanie różnych dostępnych opcji. Istnieje również menu, która umożliwia innym sposobem wykonania zadania w środowisku IDE. (Na przykład zamiast zaznaczania **nowy projekt** z **Start** strony, można skorzystać z menu i wybierz **pliku** &gt; **nowy projekt**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Można tworzyć aplikacje przy użyciu języka Visual Basic lub Visual C# jako języka programowania. Wybierz Visual C# po lewej stronie, a następnie wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nazwij swój projekt &quot;MvcMovie&quot; , a następnie kliknij przycisk **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej**. Pozostaw **Razor** jako domyślny aparat widoku.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Kliknij przycisk **OK**. Visual Studio użyty domyślny szablon projektu programu ASP.NET MVC, który właśnie utworzony, więc teraz mieć działającą aplikację bez żadnego działania! Jest to prosty &quot;Hello World!&quot; projektu, a jego dobrym miejscem do uruchomienia aplikacji.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Z **debugowania** menu, wybierz opcję **Rozpocznij debugowanie**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Zwróć uwagę, że skrót klawiaturowy można rozpocząć debugowania jest F5.

F5 powoduje, że Visual Studio, aby uruchomić usługi IIS Express i uruchom aplikację sieci web. Visual Studio następnie uruchamia przeglądarkę i spowoduje otwarcie strony głównej aplikacji. Należy zauważyć, że na pasku adresu przeglądarki mówi `localhost` i nie coś, takich jak `example.com`. Jest to spowodowane tym `localhost` zawsze wskazuje na własnym komputerze lokalnym, działającego w takim przypadku aplikacji zostały utworzone. Po uruchomieniu projektu sieci web programu Visual Studio losowy port jest używany przez serwer sieci web. Na poniższej ilustracji numer portu to 41788. Po uruchomieniu aplikacji widoczny będzie prawdopodobnie inny numer portu.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Dodatkowych zabiegów ten szablon domyślny zawiera strony domowej, kontaktów oraz o. Ponadto zapewnia obsługę rejestracji i logowania, a linki do serwisu Facebook i Twitter. Następnym krokiem jest zmienić sposób działania tej aplikacji i nieco więcej informacji na temat platformy ASP.NET MVC. Zamknij przeglądarkę i Zmieńmy kodu.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
