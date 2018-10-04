---
uid: mvc/overview/getting-started/introduction/getting-started
title: Wprowadzenie do ASP.NET MVC 5 | Dokumentacja firmy Microsoft
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 2cc9364b815cae0207fc59784303c6a0906f1b94
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578448"
---
<a name="getting-started-with-aspnet-mvc-5"></a>Wprowadzenie do ASP.NET MVC 5
====================
Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [consider RP](../../../../includes/razor.md)]

W tym samouczku pokazano podstawy tworzenia ASP.NET MVC 5 aplikację internetową przy użyciu [programu Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Kod źródłowy końcowej dla tego samouczka znajduje się na [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).

Ten samouczek został napisany przez [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , a [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

Potrzebujesz konta platformy Azure, aby wdrożyć tę aplikację na platformie Azure:

- Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług platformy Azure, a nawet w przypadku, po ich wyczerpaniu nawet możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- Możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -ramach subskrypcji MSDN daje środki na korzystanie z każdego miesiąca, używanego do płatne usługi platformy Azure.

## <a name="get-started"></a>Wprowadzenie

Rozpocznij od [instalowania programu Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Następnie otwórz program Visual Studio.

Program Visual Studio jest środowiskiem IDE lub zintegrowanego środowiska programistycznego. Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz środowisko IDE do tworzenia aplikacji. W programie Visual Studio znajduje się lista u dołu wyświetlane różne opcje dostępne dla Ciebie. Istnieje również menu, który udostępnia inny sposób wykonywania zadań w środowisku IDE. Na przykład, zamiast zaznaczania **nowy projekt** na **stronę początkową**, możesz użyć paska menu i wybrać **pliku** > **nowy projekt**.

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a>Tworzenie pierwszej aplikacji

Na **stronę początkową**, wybierz opcję **nowy projekt**. W **nowy projekt** okno dialogowe, wybierz opcję **Visual C#** kategorii po lewej stronie, następnie **Web**, a następnie wybierz pozycję **aplikacji sieci Web platformy ASP.NET (.NET Framework)**  szablonu projektu. Nazwij swój projekt "MvcMovie", a następnie wybierz **OK**.

![](getting-started/_static/image2.png)

W **Nowa aplikacja internetowa ASP.NET** okno dialogowe, wybierz **MVC** , a następnie wybierz **OK**.

![](getting-started/_static/image3.png)

Program Visual Studio ulegał szablonu domyślnego dla projektu platformy ASP.NET MVC, który został utworzony, więc teraz utworzono działającą aplikację bez żadnego działania! Jest to proste "Hello World!" Projekt, a jego dobrym miejscem, aby uruchomić aplikację.

![](getting-started/_static/image4.png)

Naciśnij klawisz **F5** można rozpocząć debugowania. Po naciśnięciu klawisza **F5**, uruchomieniu programu Visual Studio [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację sieci web. Następnie, Visual Studio otworzy w przeglądarce i otwiera strony głównej aplikacji. Należy zauważyć, że na pasku adresu przeglądarki mówi `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` zawsze wskazuje własnego komputera lokalnego, co w tym przypadku jest uruchomiona aplikacja właśnie zbudowany. Po uruchomieniu projektu sieci web programu Visual Studio losowy port jest używany dla serwera sieci web. Na poniższej ilustracji numer portu to 1234. Po uruchomieniu aplikacji, zobaczysz inny numer portu.

![](getting-started/_static/image5.png)

Gotową do tego szablonu domyślnego umożliwia `Home`, `Contact`, i `About` stron. Na powyższej ilustracji nie wyświetla **Home**, **o**, i **skontaktuj się z pomocą** łącza. W zależności od rozmiaru okna przeglądarki może być konieczne kliknij ikonę nawigacji, aby zobaczyć te linki.

![](getting-started/_static/image6.png)

Aplikacja obsługuje również zarejestrowania się i zaloguj się. Następnym krokiem jest, aby zmienić sposób działania tej aplikacji i nieco więcej informacji na temat platformy ASP.NET MVC. Zamknij aplikację ASP.NET MVC, a następnie zmienimy kodu.

Aby uzyskać listę bieżącego samouczki, zobacz [MVC zalecane artykuły](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Zobacz tej aplikacji działających na platformie Azure

Czy chcesz w witrynie Zakończono działającego jako aplikacja sieci web? Pełną wersję aplikacji można wdrożyć do konta platformy Azure, po prostu kliknąć poniższy przycisk.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie na platformie Azure. Jeśli nie masz jeszcze konta, użyj jednej z następujących opcji go utworzyć:

- [Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług platformy Azure, a nawet w przypadku, po ich wyczerpaniu nawet możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) -subskrypcji usługi Visual Studio udostępnia środki na korzystanie z każdego miesiąca, używanego do płatne usługi platformy Azure.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
