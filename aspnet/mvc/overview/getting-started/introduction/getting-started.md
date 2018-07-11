---
uid: mvc/overview/getting-started/introduction/getting-started
title: Wprowadzenie do ASP.NET MVC 5 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Zaktualizowaną wersję w tym samouczku jest dostępna, w tym miejscu przy użyciu programu Visual Studio 2015. Nowe samouczku platformy ASP.NET Core MVC 6, która oferuje wiele improvem...'
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 85d5d3292ff99ade6995c710e2728c41255def4c
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38211751"
---
<a name="getting-started-with-aspnet-mvc-5"></a>Wprowadzenie do korzystania z wzorca ASP.NET MVC 5
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 Ta seria samouczków obejmuje podstawy tworzenia ASP.NET MVC 5 aplikację internetową przy użyciu [programu Visual Studio 2017](https://www.visualstudio.com/). Ostateczne źródło samouczek [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 Ten samouczek został napisany przez [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , a [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 Potrzebujesz konta platformy Azure, aby wdrożyć tę aplikację na platformie Azure:

 - Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług platformy Azure, a nawet w przypadku, po ich wyczerpaniu nawet możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
 - Możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -ramach subskrypcji MSDN daje środki na korzystanie z każdego miesiąca, używanego do płatne usługi platformy Azure.


## <a name="getting-started"></a>Wprowadzenie

Rozpocznij od instalowania i uruchamiania [programu Visual Studio 2017](https://www.visualstudio.com/).

Program Visual Studio jest środowiskiem IDE lub zintegrowanego środowiska programistycznego. Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz środowisko IDE do tworzenia aplikacji. W programie Visual Studio znajduje się lista u dołu wyświetlane różne opcje dostępne dla Ciebie. Istnieje również menu, który udostępnia inny sposób wykonywania zadań w środowisku IDE. (Na przykład, zamiast zaznaczania **nowy projekt** z **Start** strony, można użyć menu i wybrać **pliku** &gt; **nowy projekt**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Kliknij przycisk **nowy projekt**, następnie wybierz pozycję Visual C# po lewej stronie, następnie **Web** , a następnie wybierz **aplikacji sieci Web platformy ASP.NET (.NET Framework)**. Nazwij swój projekt "MvcMovie", a następnie kliknij przycisk **OK**.

![](getting-started/_static/image2.png)

W **nowy projekt ASP.NET** okno dialogowe, kliknij przycisk **MVC** a następnie kliknij przycisk **OK**.

![](getting-started/_static/image3.png)

Program Visual Studio ulegał szablonu domyślnego dla projektu platformy ASP.NET MVC, który został utworzony, więc teraz utworzono działającą aplikację bez żadnego działania! Jest to proste "Hello World!" Projekt, a jego dobrym miejscem, aby uruchomić aplikację.

![](getting-started/_static/image4.png)

Kliknij przycisk F5, aby rozpocząć debugowanie. F5 powoduje, że Visual Studio rozpocząć [usług IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) i uruchom aplikację sieci web. Następnie, Visual Studio otworzy w przeglądarce i otwiera strony głównej aplikacji. Należy zauważyć, że na pasku adresu przeglądarki mówi `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` zawsze wskazuje własnego komputera lokalnego, co w tym przypadku jest uruchomiona aplikacja właśnie zbudowany. Po uruchomieniu projektu sieci web programu Visual Studio losowy port jest używany dla serwera sieci web. Na poniższej ilustracji numer portu to 1234. Po uruchomieniu aplikacji, zobaczysz inny numer portu.

![](getting-started/_static/image5.png)

Gotową do tego szablonu domyślnego zapewnia macierzystego, skontaktuj się z pomocą i o stron. Na powyższej ilustracji nie wyświetla **Home**, **o** i **skontaktuj się z pomocą** łącza. W zależności od rozmiaru okna przeglądarki może być konieczne kliknij ikonę nawigacji, aby zobaczyć te linki.

![](getting-started/_static/image6.png)  

Aplikacja obsługuje również zarejestrowania się i zaloguj się. Następnym krokiem jest, aby zmienić sposób działania tej aplikacji i nieco więcej informacji na temat platformy ASP.NET MVC. Zamknij aplikację ASP.NET MVC, a następnie zmienimy kodu.

Aby uzyskać listę bieżącego samouczki, zobacz [MVC zalecane artykuły](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Zobacz tej aplikacji działających na platformie Azure

Czy chcesz w witrynie Zakończono działającego jako aplikacja sieci web? Pełną wersję aplikacji można wdrożyć do konta platformy Azure, po prostu kliknąć poniższy przycisk.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie na platformie Azure. Jeśli nie masz już konto, masz następujące opcje:

- [Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług platformy Azure, a nawet w przypadku, po ich wyczerpaniu nawet możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -ramach subskrypcji MSDN daje środki na korzystanie z każdego miesiąca, używanego do płatne usługi platformy Azure.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
