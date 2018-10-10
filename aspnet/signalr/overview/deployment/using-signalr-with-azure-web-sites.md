---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Używanie SignalR z usługą Web Apps w usłudze Azure App Service | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym dokumencie opisano sposób konfigurowania aplikacji SignalR, która działa w systemie Microsoft Azure. Wersje oprogramowania używanego w tym samouczku, Visual Studio 2013 lub Vis....
ms.author: riande
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: c5ede2891ef18b622ed269723603dea3b67a135d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912608"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="bd15a-104">Używanie SignalR z usługą Web Apps w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bd15a-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="bd15a-105">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="bd15a-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="bd15a-106">W tym dokumencie opisano sposób konfigurowania aplikacji SignalR, która działa w systemie Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="bd15a-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bd15a-107">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="bd15a-107">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="bd15a-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) lub Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="bd15a-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) or Visual Studio 2012</span></span>
> - <span data-ttu-id="bd15a-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="bd15a-109">.NET 4.5</span></span>
> - <span data-ttu-id="bd15a-110">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="bd15a-110">SignalR version 2</span></span>
> - <span data-ttu-id="bd15a-111">Zestaw Azure SDK 2.3 dla programu Visual Studio 2013 lub 2012</span><span class="sxs-lookup"><span data-stu-id="bd15a-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="bd15a-112">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="bd15a-112">Questions and comments</span></span>
>
> <span data-ttu-id="bd15a-113">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="bd15a-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="bd15a-114">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), lub [fora Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="bd15a-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="bd15a-115">Spis treści</span><span class="sxs-lookup"><span data-stu-id="bd15a-115">Table of Contents</span></span>

- [<span data-ttu-id="bd15a-116">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="bd15a-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="bd15a-117">Wdrażanie aplikacji sieci Web SignalR w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bd15a-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="bd15a-118">Włączanie funkcji WebSockets w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bd15a-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="bd15a-119">Za pomocą systemu Backplane usługi Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="bd15a-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="bd15a-120">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="bd15a-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="bd15a-121">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="bd15a-121">Introduction</span></span>

<span data-ttu-id="bd15a-122">Biblioteki SignalR platformy ASP.NET można przenieść na nowy poziom interakcji między serwerami i sieci web lub klientów programu .NET.</span><span class="sxs-lookup"><span data-stu-id="bd15a-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="bd15a-123">W przypadku hostowanych na platformie Azure, aplikacji SignalR korzystać wysoko dostępnych, skalowalnych i wydajnych środowiska działające w chmurze zapewniająca.</span><span class="sxs-lookup"><span data-stu-id="bd15a-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="bd15a-124">Wdrażanie aplikacji sieci Web SignalR w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bd15a-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="bd15a-125">SignalR nie dodaje żadnych konkretnej kompilacji do wdrażania aplikacji na platformie Azure i wdrażanie na serwerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="bd15a-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="bd15a-126">Aplikacji korzystającej z biblioteki SignalR można hostować na platformie Azure bez konieczności wprowadzania zmian w konfiguracji lub innymi ustawieniami (mimo że obsługę funkcji WebSockets, zobacz [włączenie funkcji WebSockets w usłudze Azure App Service](#websocket) poniżej.) W tym samouczku wdrożysz aplikacja utworzona w [Samouczek wprowadzający](../getting-started/tutorial-getting-started-with-signalr.md) na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="bd15a-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="bd15a-127">\*\*Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="bd15a-127">**Prerequisites**</span></span>

- <span data-ttu-id="bd15a-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="bd15a-128">Visual Studio 2013.</span></span> <span data-ttu-id="bd15a-129">Jeśli nie masz programu Visual Studio, Visual Studio 2013 Express for Web znajduje się w instalacji zestawu SDK usługi Azure.</span><span class="sxs-lookup"><span data-stu-id="bd15a-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="bd15a-130">[Zestaw Azure SDK 2.3 dla programu Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) lub [zestaw Azure SDK 2.3 dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="bd15a-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="bd15a-131">Do ukończenia tego samouczka, potrzebujesz subskrypcji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="bd15a-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="bd15a-132">Możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), lub [logowania do subskrypcji wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bd15a-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="bd15a-133">Wdrażanie aplikacji sieci web SignalR na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="bd15a-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="bd15a-134">Wykonaj [Samouczek wprowadzający](../getting-started/tutorial-getting-started-with-signalr.md), lub pobrać gotowy projekt z [galerii kodów](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="bd15a-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="bd15a-135">W programie Visual Studio, wybierz **kompilacji**, **publikowania SignalR Chat**.</span><span class="sxs-lookup"><span data-stu-id="bd15a-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="bd15a-136">W oknie dialogowym "Publikowanie w sieci Web" Wybierz "Windows Azure Web Sites".</span><span class="sxs-lookup"><span data-stu-id="bd15a-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Wybierz opcję Usługa witryny sieci Web](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="bd15a-138">Jeśli użytkownik nie jest zalogowany do konta Microsoft, kliknij przycisk **Zaloguj...**  w oknie dialogowym "Wybieranie istniejącej witryny sieci Web" i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="bd15a-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Wybierz istniejącą witrynę sieci Web](using-signalr-with-azure-web-sites/_static/image2.png)    ![Logowanie do platformy Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="bd15a-141">W oknie dialogowym "Wybieranie istniejącej witryny sieci Web" kliknij **New**.</span><span class="sxs-lookup"><span data-stu-id="bd15a-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Nową witrynę sieci Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="bd15a-143">W oknie dialogowym "Tworzenie witryny na platformie Windows Azure" Wprowadź unikatową nazwę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd15a-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="bd15a-144">W menu rozwijanym Region, wybierz region najbliżej Ciebie.</span><span class="sxs-lookup"><span data-stu-id="bd15a-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="bd15a-145">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="bd15a-145">Click **Create**.</span></span>

    ![Tworzenie witryny na platformie Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="bd15a-147">W oknie dialogowym "Publikowanie w sieci Web" kliknij **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="bd15a-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Publikowanie witryny](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="bd15a-149">Po jej zakończeniu publikowania aplikacji rozmów SignalR, hostowana w usłudze Azure App Service Web Apps zostanie otwarta w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="bd15a-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Otwieranie w przeglądarce witrynę](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="bd15a-151">Włączanie funkcji WebSockets w usłudze Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="bd15a-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="bd15a-152">Gniazda Websocket musi być jawnie włączone w aplikacji sieci web ma być używany w aplikacji SignalR; w przeciwnym razie używać innych protokołów (zobacz [transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports) Aby uzyskać szczegółowe informacje).</span><span class="sxs-lookup"><span data-stu-id="bd15a-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="bd15a-153">Aby można było używać funkcji WebSockets w usłudze Azure App Service Web Apps, należy ją włączyć w sekcji Konfiguracja aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="bd15a-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="bd15a-154">Aby to zrobić, Otwórz w aplikacji sieci web [portalu zarządzania systemu Azure](https://manage.windowsazure.com/)i wybierz pozycję Konfiguruj.</span><span class="sxs-lookup"><span data-stu-id="bd15a-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Karta Konfigurowanie](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="bd15a-156">W górnej części strony konfiguracji upewnij się, że .NET 4.5 jest używana dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="bd15a-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![Ustawienia programu .NET framework w wersji 4.5](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="bd15a-158">Na stronie konfiguracji w **WebSockets** ustawienie, wybierz **na**.</span><span class="sxs-lookup"><span data-stu-id="bd15a-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Ustawienie funkcji WebSockets: na](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="bd15a-160">W dolnej części strony konfiguracji, wybierz **Zapisz** Aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="bd15a-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Zapisz ustawienia](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="bd15a-162">Za pomocą systemu Backplane usługi Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="bd15a-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="bd15a-163">Jeśli wiele wystąpień jest używana dla aplikacji sieci web, a użytkownicy tych wystąpień, które muszą wchodzić w interakcje ze sobą, (umożliwiając, na przykład wiadomości na czacie utworzone w jednym wystąpieniu można dotrzeć do użytkowników podłączonych do innych wystąpień), [usługi Azure Redis Cache płyty montażowej](../performance/scaleout-with-redis.md) musi zostać wdrożona w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd15a-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="bd15a-164">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="bd15a-164">Next Steps</span></span>

<span data-ttu-id="bd15a-165">Aby uzyskać więcej informacji na temat aplikacji sieci Web w usłudze Azure App Service, zobacz [omówienia usługi Web Apps](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="bd15a-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
