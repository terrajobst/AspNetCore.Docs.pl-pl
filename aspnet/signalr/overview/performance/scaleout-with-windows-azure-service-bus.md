---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Skalowania SignalR z usługi Azure Service Bus | Dokumentacja firmy Microsoft
author: MikeWasson
description: Wersje oprogramowania używanego w tej wersji programu Visual Studio 2013 .NET 4.5 SignalR tematu 2 poprzednie wersje tego tematu, ta wersja 1.x dla SignalR tematu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e6d9e4e6ba2040aa2c6e453aacf0ddca38c4a1a9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "32741543"
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="624e5-103">Skalowania SignalR z usługi Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="624e5-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="624e5-104">przez [Wasson Jan](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="624e5-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="624e5-105">W tym samouczku wdrożysz aplikacji SignalR, do roli sieci Web systemu Windows Azure, przy użyciu płyty montażowej usługi Service Bus do dystrybucji wiadomości do każdego wystąpienia roli.</span><span class="sxs-lookup"><span data-stu-id="624e5-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="624e5-106">(Można również użyć płyty montażowej usługi Service Bus z [sieci web apps w usłudze Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="624e5-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="624e5-107">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="624e5-107">Prerequisites:</span></span>

- <span data-ttu-id="624e5-108">Konto systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="624e5-108">A Windows Azure account.</span></span>
- <span data-ttu-id="624e5-109">[Systemu Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="624e5-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="624e5-110">Program Visual Studio 2012 lub 2013.</span><span class="sxs-lookup"><span data-stu-id="624e5-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="624e5-111">Płyty montażowej magistrali usług jest również zgodna z [Usługa Service Bus dla systemu Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), wersja 1.1.</span><span class="sxs-lookup"><span data-stu-id="624e5-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="624e5-112">Jednak nie jest zgodny z wersją 1.0 Usługa Service Bus dla systemu Windows Server.</span><span class="sxs-lookup"><span data-stu-id="624e5-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="624e5-113">Cennik</span><span class="sxs-lookup"><span data-stu-id="624e5-113">Pricing</span></span>

<span data-ttu-id="624e5-114">Płyty montażowej usługi Service Bus używa tematów do wysłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="624e5-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="624e5-115">Najnowszy cenowa informacji, zobacz [usługi Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="624e5-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="624e5-116">W momencie pisania tego dokumentu możesz wysłać 1 000 000 wiadomości miesięcznie mniej niż 1 $.</span><span class="sxs-lookup"><span data-stu-id="624e5-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="624e5-117">Systemu backplane wysyła komunikat magistrali usług dla każdego wywołania metody koncentratora SignalR.</span><span class="sxs-lookup"><span data-stu-id="624e5-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="624e5-118">Dostępne są także niektóre komunikaty kontroli dla połączeń, rozłączeń łącząca lub zostawianie grup i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="624e5-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="624e5-119">W większości aplikacji większość ruchu komunikat będzie wywołań metod koncentratora.</span><span class="sxs-lookup"><span data-stu-id="624e5-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="624e5-120">Omówienie</span><span class="sxs-lookup"><span data-stu-id="624e5-120">Overview</span></span>

<span data-ttu-id="624e5-121">Przed uzyskujemy do szczegółowy samouczek, w tym miejscu jest to szybki przegląd będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="624e5-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="624e5-122">Umożliwia utworzenie nowej przestrzeni nazw usługi Service Bus w portalu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="624e5-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="624e5-123">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="624e5-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="624e5-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="624e5-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="624e5-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) lub [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="624e5-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="624e5-126">Tworzenie aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="624e5-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="624e5-127">Dodaj następujący kod do pliku Startup.cs do skonfigurowania systemu backplane:</span><span class="sxs-lookup"><span data-stu-id="624e5-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="624e5-128">Ten kod konfiguruje systemu backplane z wartościami domyślnymi dla [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) i [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="624e5-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="624e5-129">Aby uzyskać informacje na temat zmiany tych wartości, zobacz [wydajności SignalR: metryki skalowania](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="624e5-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="624e5-130">Dla każdej aplikacji wybierz inną wartość dla "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="624e5-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="624e5-131">Nie należy używać tej samej wartości dla wielu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="624e5-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="624e5-132">Tworzenie usług platformy Azure</span><span class="sxs-lookup"><span data-stu-id="624e5-132">Create the Azure Services</span></span>

<span data-ttu-id="624e5-133">Tworzenie usługi w chmurze, zgodnie z opisem w [sposobu tworzenia i wdrażania usługi w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="624e5-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="624e5-134">Wykonaj kroki opisane w sekcji "porady: Tworzenie usługi w chmurze przy użyciu szybkie tworzenie".</span><span class="sxs-lookup"><span data-stu-id="624e5-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="624e5-135">W tym samouczku nie trzeba przekazać certyfikat.</span><span class="sxs-lookup"><span data-stu-id="624e5-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="624e5-136">Tworzenie nowej przestrzeni nazw usługi Service Bus, zgodnie z opisem w [jak magistrali usługi do użycia tematy/subskrypcje](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="624e5-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="624e5-137">Wykonaj kroki opisane w sekcji "Tworzenie Namespace usługi".</span><span class="sxs-lookup"><span data-stu-id="624e5-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="624e5-138">Upewnij się wybrać usługę w chmurze i przestrzeni nazw usługi Service Bus tym samym regionie.</span><span class="sxs-lookup"><span data-stu-id="624e5-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="624e5-139">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="624e5-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="624e5-140">Uruchom program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="624e5-140">Start Visual Studio.</span></span> <span data-ttu-id="624e5-141">Z **pliku** menu, kliknij przycisk **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="624e5-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="624e5-142">W **nowy projekt** okna dialogowego rozwiń **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="624e5-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="624e5-143">W obszarze **zainstalowane szablony**, wybierz pozycję **chmury** , a następnie wybierz **usługi w chmurze Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="624e5-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="624e5-144">Zachowaj domyślne .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="624e5-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="624e5-145">Nadaj nazwę aplikacji ChatService, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="624e5-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="624e5-146">W **nowej usługi systemu Windows Azure Cloud** okno dialogowe, wybierz rolę sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="624e5-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="624e5-147">Kliknij przycisk strzałki w prawo (**&gt;**) Dodaj rolę do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="624e5-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="624e5-148">Umieść kursor myszy nad nową rolę więc widoczne ikonę ołówka.</span><span class="sxs-lookup"><span data-stu-id="624e5-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="624e5-149">Kliknij tę ikonę, aby zmienić nazwę roli.</span><span class="sxs-lookup"><span data-stu-id="624e5-149">Click this icon to rename the role.</span></span> <span data-ttu-id="624e5-150">Nazwy roli "SignalRChat" i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="624e5-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="624e5-151">W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **MVC**i kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="624e5-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="624e5-152">Kreator projektu tworzy dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="624e5-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="624e5-153">ChatService: Ten projekt jest aplikacją systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="624e5-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="624e5-154">Definiuje Azure ról i innych opcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="624e5-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="624e5-155">SignalRChat: Ten projekt jest projekt programu ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="624e5-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="624e5-156">Tworzenie aplikacji czatu SignalR</span><span class="sxs-lookup"><span data-stu-id="624e5-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="624e5-157">Aby utworzyć aplikację rozmowy, wykonaj kroki opisane w samouczku [wprowadzenie SignalR i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="624e5-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="624e5-158">Umożliwia instalowanie wymaganych bibliotek NuGet.</span><span class="sxs-lookup"><span data-stu-id="624e5-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="624e5-159">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="624e5-159">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="624e5-160">W **Konsola Menedżera pakietów** okna, wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="624e5-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="624e5-161">Użyj `-ProjectName` możliwość zainstalowania pakietów do projektu programu ASP.NET MVC, a nie projekt systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="624e5-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="624e5-162">Konfiguruj płyty montażowej</span><span class="sxs-lookup"><span data-stu-id="624e5-162">Configure the Backplane</span></span>

<span data-ttu-id="624e5-163">W pliku Startup.cs aplikacji Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="624e5-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="624e5-164">Następnie należy pobrać parametry połączenia magistrali usługi.</span><span class="sxs-lookup"><span data-stu-id="624e5-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="624e5-165">W portalu Azure wybierz przestrzeń nazw magistrali usług, który został utworzony, a następnie kliknij ikonę klucza dostępu.</span><span class="sxs-lookup"><span data-stu-id="624e5-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="624e5-166">Skopiuj parametry połączenia do Schowka, a następnie wklej ją do *connectionString* zmiennej.</span><span class="sxs-lookup"><span data-stu-id="624e5-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="624e5-167">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="624e5-167">Deploy to Azure</span></span>

<span data-ttu-id="624e5-168">W Eksploratorze rozwiązań rozwiń **ról** folder wewnątrz projektu ChatService.</span><span class="sxs-lookup"><span data-stu-id="624e5-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="624e5-169">Kliknij prawym przyciskiem myszy rolę SignalRChat i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="624e5-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="624e5-170">Wybierz **konfiguracji** kartę. W obszarze **wystąpień** wybierz 2.</span><span class="sxs-lookup"><span data-stu-id="624e5-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="624e5-171">Rozmiar maszyny Wirtualnej można również ustawić, **dodatkowe małych**.</span><span class="sxs-lookup"><span data-stu-id="624e5-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="624e5-172">Zapisz zmiany.</span><span class="sxs-lookup"><span data-stu-id="624e5-172">Save the changes.</span></span>

<span data-ttu-id="624e5-173">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt ChatService.</span><span class="sxs-lookup"><span data-stu-id="624e5-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="624e5-174">Wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="624e5-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="624e5-175">Jeśli jest to pierwszy czasu publikowania w systemie Windows Azure, należy pobrać poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="624e5-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="624e5-176">W **publikowania** kreatora, kliknij przycisk "Zaloguj się do pobierania poświadczenia".</span><span class="sxs-lookup"><span data-stu-id="624e5-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="624e5-177">To spowoduje wyświetlenie monitu zaloguj się do portalu Windows Azure i Pobierz plik ustawień publikowania.</span><span class="sxs-lookup"><span data-stu-id="624e5-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="624e5-178">Kliknij przycisk **importu** i wybierz plik ustawień publikowania, który został pobrany.</span><span class="sxs-lookup"><span data-stu-id="624e5-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="624e5-179">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="624e5-179">Click **Next**.</span></span> <span data-ttu-id="624e5-180">W **ustawień publikowania** okna dialogowego, w obszarze **usługi w chmurze**, wybierz usługę w chmurze, który został utworzony wcześniej.</span><span class="sxs-lookup"><span data-stu-id="624e5-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="624e5-181">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="624e5-181">Click **Publish**.</span></span> <span data-ttu-id="624e5-182">Może upłynąć kilka minut, aby wdrożyć aplikację i uruchom maszyny wirtualne.</span><span class="sxs-lookup"><span data-stu-id="624e5-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="624e5-183">Teraz po uruchomieniu aplikacji czatu wystąpień roli komunikują się za pośrednictwem usługi Azure Service Bus, przy użyciu tematu usługi Service Bus.</span><span class="sxs-lookup"><span data-stu-id="624e5-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="624e5-184">Temat jest kolejki komunikatów, która umożliwia wielu subskrybentów.</span><span class="sxs-lookup"><span data-stu-id="624e5-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="624e5-185">Systemu backplane automatycznie tworzy tematu i subskrypcji.</span><span class="sxs-lookup"><span data-stu-id="624e5-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="624e5-186">Aby zobaczyć subskrypcje i działania komunikatu, otwórz Azure portal, wybierz obszar nazw usługi Service Bus i wybierz polecenie "Tematy".</span><span class="sxs-lookup"><span data-stu-id="624e5-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="624e5-187">To, że to potrwać kilka minut dla działania komunikatu wyświetlani na pulpicie nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="624e5-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="624e5-188">SignalR zarządza czasem istnienia tematu.</span><span class="sxs-lookup"><span data-stu-id="624e5-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="624e5-189">Tak długo, jak aplikacja jest wdrażana, nie należy próbować ręcznie usuń tematy lub zmień ustawienia w temacie.</span><span class="sxs-lookup"><span data-stu-id="624e5-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="624e5-190">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="624e5-190">Troubleshooting</span></span>

<span data-ttu-id="624e5-191">**System.InvalidOperationException "Tylko IsolationLevel obsługiwane jest"IsolationLevel.Serializable"."**</span><span class="sxs-lookup"><span data-stu-id="624e5-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="624e5-192">Ten błąd może wystąpić, jeśli poziomu transakcji do operacji ustawiono coś innego niż `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="624e5-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="624e5-193">Upewnij się, że żadne operacje nie są wykonywane z innych poziomach transakcji.</span><span class="sxs-lookup"><span data-stu-id="624e5-193">Verify that no operations are being performed with other transaction levels.</span></span>
