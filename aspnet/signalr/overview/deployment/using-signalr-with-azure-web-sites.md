---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Przy użyciu SignalR z aplikacjami sieci Web w usłudze Azure App Service | Dokumentacja firmy Microsoft
author: pfletcher
description: Ten dokument zawiera opis sposobu konfigurowania aplikacji SignalR, która działa w systemie Microsoft Azure. Wersje oprogramowania używany w samouczku Visual Studio 2013 lub Vis....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043210"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="9cfe6-104">Przy użyciu SignalR z aplikacjami sieci Web w usłudze aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="9cfe6-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="9cfe6-105">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="9cfe6-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="9cfe6-106">Ten dokument zawiera opis sposobu konfigurowania aplikacji SignalR, która działa w systemie Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9cfe6-107">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="9cfe6-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9cfe6-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) lub Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="9cfe6-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) or Visual Studio 2012</span></span>
> - <span data-ttu-id="9cfe6-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="9cfe6-109">.NET 4.5</span></span>
> - <span data-ttu-id="9cfe6-110">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="9cfe6-110">SignalR version 2</span></span>
> - <span data-ttu-id="9cfe6-111">Zestaw Azure SDK 2.3 dla programu Visual Studio 2013 lub 2012</span><span class="sxs-lookup"><span data-stu-id="9cfe6-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="9cfe6-112">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="9cfe6-112">Questions and comments</span></span>
> 
> <span data-ttu-id="9cfe6-113">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="9cfe6-114">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), lub [fora Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="9cfe6-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="9cfe6-115">Spis treści</span><span class="sxs-lookup"><span data-stu-id="9cfe6-115">Table of Contents</span></span>

- [<span data-ttu-id="9cfe6-116">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="9cfe6-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="9cfe6-117">Wdrażanie aplikacji sieci SignalR Web w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9cfe6-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="9cfe6-118">Włączanie Websocket w usłudze aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="9cfe6-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="9cfe6-119">Przy użyciu płyty montażowej pamięci podręcznej Azure Redis</span><span class="sxs-lookup"><span data-stu-id="9cfe6-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="9cfe6-120">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="9cfe6-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="9cfe6-121">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="9cfe6-121">Introduction</span></span>

<span data-ttu-id="9cfe6-122">Biblioteka SignalR platformy ASP.NET można przełączyć nowy poziom interakcji między serwerami i sieci web lub klientów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="9cfe6-123">Gdy hostowana na platformie Azure, aplikacji SignalR można korzystać z wysokiej dostępności, skalowalności i wydajności środowisko, która działa w chmurze zapewnia.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="9cfe6-124">Wdrażanie aplikacji sieci SignalR Web w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9cfe6-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="9cfe6-125">SignalR nie dodaje żadnych komplikacji określonego do wdrażania aplikacji na platformie Azure i wdrażanie na serwerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="9cfe6-126">Aplikację, która używa SignalR może być hostowana na platformie Azure, bez wprowadzania żadnych zmian w konfiguracji lub inne ustawienia (mimo że obsługę protokołu WebSockets, zobacz [włączenie Websocket w usłudze Azure App Service](#websocket) poniżej.) W tym samouczku utworzone w aplikacja zostanie wdrożona [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md) na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="9cfe6-127">**Wymagania wstępne**</span><span class="sxs-lookup"><span data-stu-id="9cfe6-127">**Prerequisites**</span></span>

- <span data-ttu-id="9cfe6-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-128">Visual Studio 2013.</span></span> <span data-ttu-id="9cfe6-129">Jeśli nie masz programu Visual Studio, Visual Studio 2013 Express for Web znajduje się w instalacji zestawu SDK platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="9cfe6-130">[Zestaw Azure SDK 2.3 dla programu Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) lub [Azure SDK 2.3 dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="9cfe6-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="9cfe6-131">Do ukończenia tego samouczka należy subskrypcji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="9cfe6-132">Możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), lub [Załóż subskrypcji wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9cfe6-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="9cfe6-133">Wdrażanie aplikacji sieci web SignalR na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="9cfe6-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="9cfe6-134">Zakończenie [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md), lub pobrać gotowego projektu z [galerii kodu](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="9cfe6-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="9cfe6-135">W programie Visual Studio, wybierz **kompilacji**, **publikowania rozmowę SignalR**.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="9cfe6-136">W oknie dialogowym "Publikowanie w sieci Web" Wybierz "Windows Azure Web Sites".</span><span class="sxs-lookup"><span data-stu-id="9cfe6-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Wybierz witryny sieci Web Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="9cfe6-138">Jeśli użytkownik nie jest obecnie zalogowany do konta Microsoft, kliknij przycisk **logowania...**  w oknie dialogowym "Wybierz istniejącą witrynę sieci Web" i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Wybierz istniejącą witrynę sieci Web](using-signalr-with-azure-web-sites/_static/image2.png)    ![Logowanie do platformy Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="9cfe6-141">W oknie dialogowym "Wybierz istniejącą witrynę sieci Web", kliknij przycisk **nowy**.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Nową witrynę sieci Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="9cfe6-143">W oknie dialogowym "Utwórz witrynę w systemie Windows Azure" Wprowadź unikatowej nazwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="9cfe6-144">Wybierz region znajdujący się najbliżej użytkownika na liście rozwijanej regionu.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="9cfe6-145">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-145">Click **Create**.</span></span>

    ![Tworzenie witryny na platformie Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="9cfe6-147">W oknie dialogowym "Publikowanie w sieci Web", kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Publikowanie witryny](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="9cfe6-149">Po ukończeniu publikowania aplikacji rozmów SignalR hostowanej aplikacji w aplikacji sieci Web usługi aplikacji Azure zostanie otwarty w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Witryna otwierania w przeglądarce](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="9cfe6-151">Włączanie Websocket aplikacji sieci Web usługi aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="9cfe6-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="9cfe6-152">Protokół WebSockets musi być jawnie włączone w aplikacji sieci web do użycia w aplikacji SignalR; w przeciwnym razie należy używać innych protokołów (zobacz [transportu i przejścia](../getting-started/introduction-to-signalr.md#transports) szczegółowe informacje).</span><span class="sxs-lookup"><span data-stu-id="9cfe6-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="9cfe6-153">Aby można było używać Websocket aplikacji sieci Web usługi aplikacji Azure, należy ją włączyć w sekcji konfiguracji aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="9cfe6-154">Aby to zrobić, Otwórz aplikację sieci web w [portalu zarządzania Azure](https://manage.windowsazure.com/)i wybierz pozycję Konfiguruj.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Karta Konfigurowanie](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="9cfe6-156">W górnej części strony konfiguracji upewnij się, że programy .NET 4.5 jest używany przez aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![Ustawienie wersji 4.5 programu .NET framework](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="9cfe6-158">Na stronie konfiguracji w **Websocket** wybierz pozycję **na**.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Ustawienie WebSockets: na](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="9cfe6-160">W dolnej części strony konfiguracji, wybierz **zapisać** Aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Zapisz ustawienia](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="9cfe6-162">Przy użyciu płyty montażowej pamięci podręcznej Azure Redis</span><span class="sxs-lookup"><span data-stu-id="9cfe6-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="9cfe6-163">Jeśli używasz wielu wystąpień dla aplikacji sieci web i użytkowników z tych wystąpień konieczne współdziałać ze sobą (umożliwiając, na przykład wiadomości utworzone w jednym wystąpieniu może nawiązać połączenie użytkowników podłączonych do innych wystąpień), [pamięć podręczna Redis Azure płyty montażowej](../performance/scaleout-with-redis.md) musi zostać wdrożona w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9cfe6-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="9cfe6-164">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="9cfe6-164">Next Steps</span></span>

<span data-ttu-id="9cfe6-165">Aby uzyskać więcej informacji dotyczących aplikacji sieci Web w usłudze Azure App Service, zobacz [Omówienie aplikacji sieci Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="9cfe6-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
