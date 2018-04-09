---
uid: mvc/overview/getting-started/introduction/getting-started
title: Wprowadzenie do platformy ASP.NET MVC 5 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Dostępne w tym miejscu przy użyciu programu Visual Studio 2015 jest zaktualizowana wersja tego samouczka. Samouczek nowej używa platformy ASP.NET Core MVC 6, która oferuje wiele improvem...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 0f1fd2026691d3bc0e81b20a9731879d7a6041bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="a1bf6-104">Wprowadzenie do korzystania z wzorca ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="a1bf6-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="a1bf6-105">przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a1bf6-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 <span data-ttu-id="a1bf6-106">W tym samouczku udzieli Ci podstawy ASP.NET MVC 5 sieci web aplikacji za pomocą [programu Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="a1bf6-107">Ostateczne źródło samouczek znajdujących się na [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="a1bf6-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>


 <span data-ttu-id="a1bf6-108">W tym samouczku, została napisana przy [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , a [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="a1bf6-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>

 <span data-ttu-id="a1bf6-109">Potrzebne jest konto platformy Azure, aby wdrożyć tę aplikację na platformie Azure:</span><span class="sxs-lookup"><span data-stu-id="a1bf6-109">You need an Azure account to deploy this app to Azure:</span></span>

 - <span data-ttu-id="a1bf6-110">Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług Azure, a nawet po wyczerpaniu kredytu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-110">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="a1bf6-111">Możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -subskrypcji Your MSDN otrzymasz kredyt, co miesiąc, używanego programu płatnych usług Azure.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="a1bf6-112">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="a1bf6-112">Getting Started</span></span>

<span data-ttu-id="a1bf6-113">Rozpocznij od instalowania i uruchamiania [programu Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="a1bf6-114">Program Visual Studio jest IDE lub zintegrowane środowisko deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="a1bf6-115">Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz IDE do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="a1bf6-116">W programie Visual Studio znajduje się lista wzdłuż dolnej pokazywanie różnych dostępnych opcji.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="a1bf6-117">Istnieje również menu, która umożliwia innym sposobem wykonania zadania w środowisku IDE.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="a1bf6-118">(Na przykład zamiast zaznaczania **nowy projekt** z **Start** strony, można skorzystać z menu i wybierz **pliku** &gt; **nowy projekt**.)</span><span class="sxs-lookup"><span data-stu-id="a1bf6-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a><span data-ttu-id="a1bf6-119">Tworzenie pierwszej aplikacji</span><span class="sxs-lookup"><span data-stu-id="a1bf6-119">Creating Your First Application</span></span>

<span data-ttu-id="a1bf6-120">Kliknij przycisk **nowy projekt**, następnie wybierz Visual C# po lewej stronie, następnie **Web** , a następnie wybierz **aplikacji sieci Web platformy ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="a1bf6-121">Nazwa projektu "MvcMovie", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="a1bf6-122">W **nowy projekt ASP.NET** okna dialogowego, kliknij przycisk **MVC** , a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="a1bf6-123">Visual Studio użyty domyślny szablon projektu programu ASP.NET MVC, który właśnie utworzony, więc teraz mieć działającą aplikację bez żadnego działania!</span><span class="sxs-lookup"><span data-stu-id="a1bf6-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="a1bf6-124">Jest to prosty "Witaj świecie!"</span><span class="sxs-lookup"><span data-stu-id="a1bf6-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="a1bf6-125">Projekt, a jego dobrym miejscem do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="a1bf6-126">Kliknij przycisk F5, aby rozpocząć debugowania.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-126">Click F5 to start debugging.</span></span> <span data-ttu-id="a1bf6-127">Visual Studio, aby uruchomić powoduje F5 [usług IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienia aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="a1bf6-128">Visual Studio następnie uruchamia przeglądarkę i spowoduje otwarcie strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="a1bf6-129">Należy zauważyć, że na pasku adresu przeglądarki mówi `localhost:port#` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a1bf6-130">Jest to spowodowane tym `localhost` zawsze wskazuje na własnym komputerze lokalnym, działającego w takim przypadku aplikacji zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="a1bf6-131">Po uruchomieniu projektu sieci web programu Visual Studio losowy port jest używany przez serwer sieci web.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a1bf6-132">Na poniższej ilustracji numer portu to 1234.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="a1bf6-133">Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="a1bf6-134">Dodatkowych zabiegów ten szablon domyślny zawiera strony domowej, kontaktów oraz o.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="a1bf6-135">Powyższy obraz nie wyświetla **Home**, **o** i **skontaktuj się z** łącza.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="a1bf6-136">W zależności od rozmiaru okna przeglądarki konieczne może być kliknij ikonę nawigacji, aby wyświetlić te łącza.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="a1bf6-137">Aplikacja udostępnia również obsługę rejestracji i logowania.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="a1bf6-138">Następnym krokiem jest zmienić sposób działania tej aplikacji i nieco więcej informacji na temat platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="a1bf6-139">Zamknij aplikację ASP.NET MVC i Zmieńmy kodu.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="a1bf6-140">Lista samouczków bieżący, zobacz [MVC zalecane artykuły](../mvc-learning-sequence.md).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="a1bf6-141">Zobacz tej aplikacji działających na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="a1bf6-141">See this App Running on Azure</span></span>

<span data-ttu-id="a1bf6-142">Czy chcesz w witrynie Zakończono uruchomione jako aplikacji sieci web?</span><span class="sxs-lookup"><span data-stu-id="a1bf6-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="a1bf6-143">Pełną wersję aplikacji można wdrożyć do konta platformy Azure w celu wystarczy kliknąć poniższy przycisk.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="a1bf6-144">Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie do platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="a1bf6-145">Jeśli nie masz już konto, dostępne są następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="a1bf6-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="a1bf6-146">[Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług Azure, a nawet po wyczerpaniu kredytu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="a1bf6-147">[Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -subskrypcji Your MSDN otrzymasz kredyt, co miesiąc, używanego programu płatnych usług Azure.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a1bf6-148">Next</span><span class="sxs-lookup"><span data-stu-id="a1bf6-148">Next</span></span>](adding-a-controller.md)
