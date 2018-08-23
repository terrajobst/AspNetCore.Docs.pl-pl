---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: SignalR — skalowanie w poziomie za pomocą usługi Azure Service Bus | Dokumentacja firmy Microsoft
author: MikeWasson
description: Wersje oprogramowania używany w tej wersji programu Visual Studio 2013 .NET 4.5 SignalR tematu 2 poprzednie wersje tego tematu dla SignalR 1.x wersję tego tematu...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b87eb9f2df82d92c07ea0c86873849a44660e5c2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756899"
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="957c6-103">SignalR — skalowanie w poziomie za pomocą usługi Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="957c6-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="957c6-104">przez [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="957c6-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="957c6-105">W tym samouczku wdrożysz aplikację SignalR, do roli sieci Web Windows Azure, przy użyciu systemu backplane usługi Service Bus, aby dystrybuować komunikaty do każdego wystąpienia roli.</span><span class="sxs-lookup"><span data-stu-id="957c6-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="957c6-106">(Możesz również użyć płyty montażowej usługi Service Bus za pomocą [aplikacji sieci web w usłudze Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="957c6-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="957c6-107">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="957c6-107">Prerequisites:</span></span>

- <span data-ttu-id="957c6-108">Konto usługi Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="957c6-108">A Windows Azure account.</span></span>
- <span data-ttu-id="957c6-109">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="957c6-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="957c6-110">Visual Studio 2012 lub 2013.</span><span class="sxs-lookup"><span data-stu-id="957c6-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="957c6-111">Płyty montażowej magistrali usług jest również zgodna z [usługi Service Bus dla systemu Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), wersja 1.1.</span><span class="sxs-lookup"><span data-stu-id="957c6-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="957c6-112">Jednak nie jest zgodny z wersją 1.0 usługi Service Bus dla systemu Windows Server.</span><span class="sxs-lookup"><span data-stu-id="957c6-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="957c6-113">Cennik</span><span class="sxs-lookup"><span data-stu-id="957c6-113">Pricing</span></span>

<span data-ttu-id="957c6-114">Płyty montażowej usługi Service Bus używa tematów do wysłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="957c6-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="957c6-115">Najnowsze informacje cenach, zobacz [usługi Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="957c6-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="957c6-116">W momencie pisania tego dokumentu można wysłać wiadomości 1 000 000 na miesiąc dla mniej niż 1 USD.</span><span class="sxs-lookup"><span data-stu-id="957c6-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="957c6-117">Systemu backplane wysyła komunikat usługi service bus dla każdego wywołania metody koncentratora SignalR.</span><span class="sxs-lookup"><span data-stu-id="957c6-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="957c6-118">Istnieją również pewne komunikaty sterujące dla połączeń, odłączenia, przyłączany lub opuścić grupy i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="957c6-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="957c6-119">W większości aplikacji większość ruchu komunikat będzie wywołań metod koncentratora.</span><span class="sxs-lookup"><span data-stu-id="957c6-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="957c6-120">Omówienie</span><span class="sxs-lookup"><span data-stu-id="957c6-120">Overview</span></span>

<span data-ttu-id="957c6-121">Przed przejściem do szczegółowe podręcznika, poniżej przedstawiono krótkie podsumowanie będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="957c6-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="957c6-122">Użyj portalu platformy Windows Azure, aby utworzyć nową przestrzeń nazw usługi Service Bus.</span><span class="sxs-lookup"><span data-stu-id="957c6-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="957c6-123">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="957c6-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="957c6-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="957c6-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="957c6-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) lub [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="957c6-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="957c6-126">Tworzenie aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="957c6-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="957c6-127">Dodaj następujący kod do pliku Startup.cs w celu skonfigurowania systemu backplane:</span><span class="sxs-lookup"><span data-stu-id="957c6-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="957c6-128">Ten kod konfiguruje systemu backplane z wartościami domyślnymi dla [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) i [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="957c6-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="957c6-129">Aby uzyskać informacje na temat zmiany tych wartości, zobacz [wydajność SignalR: metryki skalowania](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="957c6-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="957c6-130">Dla każdej aplikacji wybierz inną wartość dla "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="957c6-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="957c6-131">Nie należy używać tej samej wartości dla wielu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="957c6-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="957c6-132">Tworzenie usług platformy Azure</span><span class="sxs-lookup"><span data-stu-id="957c6-132">Create the Azure Services</span></span>

<span data-ttu-id="957c6-133">Utwórz usługę w chmurze, zgodnie z opisem w [jak utworzyć i wdrożyć usługę w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="957c6-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="957c6-134">Postępuj zgodnie z instrukcjami w sekcji "jak: Tworzenie usługi w chmurze przy użyciu szybkiego tworzenia".</span><span class="sxs-lookup"><span data-stu-id="957c6-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="957c6-135">W tym samouczku nie musisz przekazać certyfikat.</span><span class="sxs-lookup"><span data-stu-id="957c6-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="957c6-136">Utwórz nową przestrzeń nazw usługi Service Bus, zgodnie z opisem w [jak do użycia usługi Service Bus tematów/subskrypcji](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="957c6-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="957c6-137">Postępuj zgodnie z instrukcjami w sekcji "Tworzenie Namespace usługi".</span><span class="sxs-lookup"><span data-stu-id="957c6-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="957c6-138">Upewnij się wybrać ten sam region dla usługi w chmurze i przestrzeni nazw usługi Service Bus.</span><span class="sxs-lookup"><span data-stu-id="957c6-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="957c6-139">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="957c6-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="957c6-140">Uruchom program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="957c6-140">Start Visual Studio.</span></span> <span data-ttu-id="957c6-141">Z **pliku** menu, kliknij przycisk **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="957c6-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="957c6-142">W **nowy projekt** okna dialogowego rozwiń **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="957c6-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="957c6-143">W obszarze **zainstalowane szablony**, wybierz opcję **chmury** , a następnie wybierz **usłudze Windows Azure Cloud Services**.</span><span class="sxs-lookup"><span data-stu-id="957c6-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="957c6-144">Zachowaj domyślne .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="957c6-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="957c6-145">Nazwij aplikację ChatService, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="957c6-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="957c6-146">W **nową usługę w chmurze Azure Windows** okno dialogowe, wybierz rolę sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="957c6-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="957c6-147">Kliknij przycisk strzałki w prawo (**&gt;**) aby dodać rolę do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="957c6-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="957c6-148">Umieść kursor myszy nad nową rolę, więc widoczne ikonę ołówka.</span><span class="sxs-lookup"><span data-stu-id="957c6-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="957c6-149">Kliknij tę ikonę, aby zmienić nazwę roli.</span><span class="sxs-lookup"><span data-stu-id="957c6-149">Click this icon to rename the role.</span></span> <span data-ttu-id="957c6-150">Nazwa roli "SignalRChat", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="957c6-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="957c6-151">W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **MVC**i kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="957c6-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="957c6-152">Kreator projektu tworzy dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="957c6-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="957c6-153">ChatService: Ten projekt jest aplikacją platformy Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="957c6-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="957c6-154">Definiuje ról platformy Azure i inne opcje konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="957c6-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="957c6-155">SignalRChat: Ten projekt jest projektu ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="957c6-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="957c6-156">Tworzenie aplikacji czatu SignalR</span><span class="sxs-lookup"><span data-stu-id="957c6-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="957c6-157">Aby utworzyć aplikację rozmowy, postępuj zgodnie z instrukcjami w tym samouczku [wprowadzenie do SignalR i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="957c6-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="957c6-158">NuGet umożliwia instalowanie wymaganych bibliotek.</span><span class="sxs-lookup"><span data-stu-id="957c6-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="957c6-159">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="957c6-159">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="957c6-160">W **Konsola Menedżera pakietów** okna, wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="957c6-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="957c6-161">Użyj `-ProjectName` opcję, aby zainstalować te pakiety w projekcie ASP.NET MVC, a nie projekt platformy Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="957c6-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="957c6-162">Konfigurowanie systemu Backplane</span><span class="sxs-lookup"><span data-stu-id="957c6-162">Configure the Backplane</span></span>

<span data-ttu-id="957c6-163">W pliku Startup.cs Twojej aplikacji Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="957c6-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="957c6-164">Teraz musisz pobrać parametry połączenia usługi service bus.</span><span class="sxs-lookup"><span data-stu-id="957c6-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="957c6-165">W witrynie Azure portal wybierz przestrzeń nazw magistrali usług, który został utworzony, a następnie kliknij ikonę klucza dostępu.</span><span class="sxs-lookup"><span data-stu-id="957c6-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="957c6-166">Skopiuj parametry połączenia do Schowka, a następnie wklej go do *connectionString* zmiennej.</span><span class="sxs-lookup"><span data-stu-id="957c6-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="957c6-167">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="957c6-167">Deploy to Azure</span></span>

<span data-ttu-id="957c6-168">W Eksploratorze rozwiązań rozwiń **role** folder wewnątrz projektu ChatService.</span><span class="sxs-lookup"><span data-stu-id="957c6-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="957c6-169">Kliknij prawym przyciskiem myszy rolę SignalRChat, a następnie wybierz pozycję **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="957c6-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="957c6-170">Wybierz **konfiguracji** kartę. W obszarze **wystąpień** wybierz opcję 2.</span><span class="sxs-lookup"><span data-stu-id="957c6-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="957c6-171">Rozmiar maszyny Wirtualnej można również ustawić, **wystąpieniom bardzo małe**.</span><span class="sxs-lookup"><span data-stu-id="957c6-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="957c6-172">Zapisz zmiany.</span><span class="sxs-lookup"><span data-stu-id="957c6-172">Save the changes.</span></span>

<span data-ttu-id="957c6-173">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt ChatService.</span><span class="sxs-lookup"><span data-stu-id="957c6-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="957c6-174">Wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="957c6-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="957c6-175">Jeśli jest to pierwszy czasu publikowania na platformie Windows Azure, należy pobrać swoje poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="957c6-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="957c6-176">W **Publikuj** kreatora, kliknij przycisk "Zaloguj się do pobierania poświadczenia".</span><span class="sxs-lookup"><span data-stu-id="957c6-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="957c6-177">To spowoduje wyświetlenie monitu zaloguj się do portalu usługi Windows Azure i Pobierz plik ustawień publikowania.</span><span class="sxs-lookup"><span data-stu-id="957c6-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="957c6-178">Kliknij przycisk **importu** i zaznacz plik ustawień publikowania, który został pobrany.</span><span class="sxs-lookup"><span data-stu-id="957c6-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="957c6-179">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="957c6-179">Click **Next**.</span></span> <span data-ttu-id="957c6-180">W **ustawień publikowania** okna dialogowego, w obszarze **usługi w chmurze**, wybierz usługę w chmurze, która została utworzona wcześniej.</span><span class="sxs-lookup"><span data-stu-id="957c6-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="957c6-181">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="957c6-181">Click **Publish**.</span></span> <span data-ttu-id="957c6-182">Może upłynąć kilka minut, aby wdrożyć aplikację i uruchomić maszyny wirtualne.</span><span class="sxs-lookup"><span data-stu-id="957c6-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="957c6-183">Teraz po uruchomieniu aplikacji rozmów z wystąpień roli komunikują się za pośrednictwem usługi Azure Service Bus, przy użyciu tematu usługi Service Bus.</span><span class="sxs-lookup"><span data-stu-id="957c6-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="957c6-184">Temat jest kolejki komunikatów, która umożliwia wielu subskrybentów.</span><span class="sxs-lookup"><span data-stu-id="957c6-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="957c6-185">Systemu backplane automatycznie tworzy, tematu i subskrypcji.</span><span class="sxs-lookup"><span data-stu-id="957c6-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="957c6-186">Aby zobaczyć subskrypcje i działania komunikatu, otwórz witrynę Azure portal, wybierz przestrzeń nazw usługi Service Bus i wybierz polecenie "Tematów".</span><span class="sxs-lookup"><span data-stu-id="957c6-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="957c6-187">To, że po kilku minutach dla działania komunikatu do wyświetlenia na pulpicie nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="957c6-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="957c6-188">SignalR zarządza czasem istnienia tematu.</span><span class="sxs-lookup"><span data-stu-id="957c6-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="957c6-189">Tak długo, jak Twoja aplikacja zostanie wdrożona, nie należy próbować ręcznie usuń tematy lub zmienić ustawienia w tej dziedzinie.</span><span class="sxs-lookup"><span data-stu-id="957c6-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="957c6-190">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="957c6-190">Troubleshooting</span></span>

<span data-ttu-id="957c6-191">**System.InvalidOperationException "Tylko IsolationLevel obsługiwane jest"IsolationLevel.Serializable"."**</span><span class="sxs-lookup"><span data-stu-id="957c6-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="957c6-192">Ten błąd może wystąpić, jeśli poziom transakcji dla operacji jest równa coś innego niż `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="957c6-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="957c6-193">Sprawdź, czy żadne operacje są wykonywane przy użyciu innych poziomów transakcji.</span><span class="sxs-lookup"><span data-stu-id="957c6-193">Verify that no operations are being performed with other transaction levels.</span></span>
