---
uid: mvc/overview/getting-started/introduction/getting-started
title: Wprowadzenie do platformy ASP.NET MVC 5 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Uwaga: Dostępne w tym miejscu przy użyciu programu Visual Studio 2015 jest zaktualizowana wersja tego samouczka. Samouczek nowej używa platformy ASP.NET Core MVC 6, która oferuje wiele improvem..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 1616b238612fa9f519418f583c40a46b9d81d8ce
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-mvc-5"></a>Wprowadzenie do korzystania z wzorca ASP.NET MVC 5
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[consider RP](../../../../includes/razor.md)]

 
 W tym samouczku udzieli Ci podstawy ASP.NET MVC 5 sieci web aplikacji za pomocą [programu Visual Studio 2017](https://www.visualstudio.com/). Ostateczne źródło samouczek znajdujących się na [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)
 
 
 W tym samouczku, została napisana przy [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , a [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )
 
 Potrzebne jest konto platformy Azure, aby wdrożyć tę aplikację na platformie Azure:
 
 - Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług Azure, a nawet po wyczerpaniu kredytu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
 - Możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -subskrypcji Your MSDN otrzymasz kredyt, co miesiąc, używanego programu płatnych usług Azure.


## <a name="getting-started"></a>Wprowadzenie

Rozpocznij od instalowania i uruchamiania [programu Visual Studio 2017](https://www.visualstudio.com/).

Program Visual Studio jest IDE lub zintegrowane środowisko deweloperskie. Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz IDE do tworzenia aplikacji. W programie Visual Studio znajduje się lista wzdłuż dolnej pokazywanie różnych dostępnych opcji. Istnieje również menu, która umożliwia innym sposobem wykonania zadania w środowisku IDE. (Na przykład zamiast zaznaczania **nowy projekt** z **Start** strony, można skorzystać z menu i wybierz **pliku** &gt; **nowy projekt**.)

   
![](getting-started/_static/image1.png)  
 

## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Kliknij przycisk **nowy projekt**, następnie wybierz Visual C# po lewej stronie, następnie **Web** , a następnie wybierz **aplikacji sieci Web platformy ASP.NET (.NET Framework)**. Nazwa projektu "MvcMovie", a następnie kliknij przycisk **OK**.

![](getting-started/_static/image2.png)

W **nowy projekt ASP.NET** okna dialogowego, kliknij przycisk **MVC** , a następnie kliknij przycisk **OK**.

![](getting-started/_static/image3.png)

Visual Studio użyty domyślny szablon projektu programu ASP.NET MVC, który właśnie utworzony, więc teraz mieć działającą aplikację bez żadnego działania! Jest to prosty "Witaj świecie!" Projekt, a jego dobrym miejscem do uruchomienia aplikacji.

![](getting-started/_static/image4.png)

Kliknij przycisk F5, aby rozpocząć debugowania. Visual Studio, aby uruchomić powoduje F5 [usług IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienia aplikacji sieci web. Visual Studio następnie uruchamia przeglądarkę i spowoduje otwarcie strony głównej aplikacji. Należy zauważyć, że na pasku adresu przeglądarki mówi `localhost:port#` i nie coś, takich jak `example.com`. Jest to spowodowane tym `localhost` zawsze wskazuje na własnym komputerze lokalnym, działającego w takim przypadku aplikacji zostały utworzone. Po uruchomieniu projektu sieci web programu Visual Studio losowy port jest używany przez serwer sieci web. Na poniższej ilustracji numer portu to 1234. Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.

![](getting-started/_static/image5.png)

Dodatkowych zabiegów ten szablon domyślny zawiera strony domowej, kontaktów oraz o. Powyższy obraz nie wyświetla **Home**, **o** i **skontaktuj się z** łącza. W zależności od rozmiaru okna przeglądarki konieczne może być kliknij ikonę nawigacji, aby wyświetlić te łącza.

![](getting-started/_static/image6.png)  

Aplikacja udostępnia również obsługę rejestracji i logowania. Następnym krokiem jest zmienić sposób działania tej aplikacji i nieco więcej informacji na temat platformy ASP.NET MVC. Zamknij aplikację ASP.NET MVC i Zmieńmy kodu.

Lista samouczków bieżący, zobacz [MVC zalecane artykuły](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Zobacz tej aplikacji działających na platformie Azure

Czy chcesz w witrynie Zakończono uruchomione jako aplikacji sieci web? Pełną wersję aplikacji można wdrożyć do konta platformy Azure w celu wystarczy kliknąć poniższy przycisk.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie do platformy Azure. Jeśli nie masz już konto, dostępne są następujące opcje:

- [Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług Azure, a nawet po wyczerpaniu kredytu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -subskrypcji Your MSDN otrzymasz kredyt, co miesiąc, używanego programu płatnych usług Azure.

>[!div class="step-by-step"]
[Next](adding-a-controller.md)
