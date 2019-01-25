---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: SignalR — skalowanie w poziomie za pomocą usługi Azure Service Bus (SignalR 1.x) | Dokumentacja firmy Microsoft
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 77186f43b38a8423a1cbd4cf42723c5b9ccdd953
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837913"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="2ed37-102">SignalR — skalowanie w poziomie z użyciem usługi Azure Service Bus (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="2ed37-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="2ed37-103">przez [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="2ed37-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="2ed37-104">W tym samouczku wdrożysz aplikację SignalR, do roli sieci Web Windows Azure, przy użyciu systemu backplane usługi Service Bus, aby dystrybuować komunikaty do każdego wystąpienia roli.</span><span class="sxs-lookup"><span data-stu-id="2ed37-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="2ed37-105">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="2ed37-105">Prerequisites:</span></span>

- <span data-ttu-id="2ed37-106">Konto usługi Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="2ed37-106">A Windows Azure account.</span></span>
- <span data-ttu-id="2ed37-107">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="2ed37-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="2ed37-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="2ed37-108">Visual Studio 2012.</span></span>

<span data-ttu-id="2ed37-109">Płyty montażowej magistrali usług jest również zgodna z [usługi Service Bus dla systemu Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), wersja 1.1.</span><span class="sxs-lookup"><span data-stu-id="2ed37-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="2ed37-110">Jednak nie jest zgodny z wersją 1.0 usługi Service Bus dla systemu Windows Server.</span><span class="sxs-lookup"><span data-stu-id="2ed37-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="2ed37-111">Cennik</span><span class="sxs-lookup"><span data-stu-id="2ed37-111">Pricing</span></span>

<span data-ttu-id="2ed37-112">Płyty montażowej usługi Service Bus używa tematów do wysłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="2ed37-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="2ed37-113">Najnowsze informacje cenach, zobacz [usługi Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="2ed37-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="2ed37-114">W momencie pisania tego dokumentu można wysłać wiadomości 1 000 000 na miesiąc dla mniej niż 1 USD.</span><span class="sxs-lookup"><span data-stu-id="2ed37-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="2ed37-115">Systemu backplane wysyła komunikat usługi service bus dla każdego wywołania metody koncentratora SignalR.</span><span class="sxs-lookup"><span data-stu-id="2ed37-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="2ed37-116">Istnieją również pewne komunikaty sterujące dla połączeń, odłączenia, przyłączany lub opuścić grupy i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="2ed37-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="2ed37-117">W większości aplikacji większość ruchu komunikat będzie wywołań metod koncentratora.</span><span class="sxs-lookup"><span data-stu-id="2ed37-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="2ed37-118">Omówienie</span><span class="sxs-lookup"><span data-stu-id="2ed37-118">Overview</span></span>

<span data-ttu-id="2ed37-119">Przed przejściem do szczegółowe podręcznika, poniżej przedstawiono krótkie podsumowanie będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="2ed37-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="2ed37-120">Użyj portalu platformy Windows Azure, aby utworzyć nową przestrzeń nazw usługi Service Bus.</span><span class="sxs-lookup"><span data-stu-id="2ed37-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="2ed37-121">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="2ed37-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="2ed37-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="2ed37-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="2ed37-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="2ed37-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="2ed37-124">Tworzenie aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="2ed37-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="2ed37-125">Dodaj następujący kod do pliku Global.asax w celu skonfigurowania systemu backplane:</span><span class="sxs-lookup"><span data-stu-id="2ed37-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="2ed37-126">Dla każdej aplikacji wybierz inną wartość dla "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="2ed37-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="2ed37-127">Nie należy używać tej samej wartości dla wielu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2ed37-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="2ed37-128">Tworzenie usług platformy Azure</span><span class="sxs-lookup"><span data-stu-id="2ed37-128">Create the Azure Services</span></span>

<span data-ttu-id="2ed37-129">Utwórz usługę w chmurze, zgodnie z opisem w [jak utworzyć i wdrożyć usługę w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="2ed37-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="2ed37-130">Postępuj zgodnie z instrukcjami w sekcji "jak: Utwórz usługę w chmurze przy użyciu szybkie tworzenie".</span><span class="sxs-lookup"><span data-stu-id="2ed37-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="2ed37-131">W tym samouczku nie musisz przekazać certyfikat.</span><span class="sxs-lookup"><span data-stu-id="2ed37-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="2ed37-132">Utwórz nową przestrzeń nazw usługi Service Bus, zgodnie z opisem w [jak do użycia usługi Service Bus tematów/subskrypcji](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="2ed37-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="2ed37-133">Postępuj zgodnie z instrukcjami w sekcji "Tworzenie Namespace usługi".</span><span class="sxs-lookup"><span data-stu-id="2ed37-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="2ed37-134">Upewnij się wybrać ten sam region dla usługi w chmurze i przestrzeni nazw usługi Service Bus.</span><span class="sxs-lookup"><span data-stu-id="2ed37-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="2ed37-135">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ed37-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="2ed37-136">Uruchom program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2ed37-136">Start Visual Studio.</span></span> <span data-ttu-id="2ed37-137">Z **pliku** menu, kliknij przycisk **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="2ed37-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="2ed37-138">W **nowy projekt** okna dialogowego rozwiń **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="2ed37-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="2ed37-139">W obszarze **zainstalowane szablony**, wybierz opcję **chmury** , a następnie wybierz **usłudze Windows Azure Cloud Services**.</span><span class="sxs-lookup"><span data-stu-id="2ed37-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="2ed37-140">Zachowaj domyślne .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="2ed37-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="2ed37-141">Nazwij aplikację ChatService, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="2ed37-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="2ed37-142">W **nową usługę w chmurze Azure Windows** okno dialogowe, wybierz rolę sieci Web platformy ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="2ed37-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="2ed37-143">Kliknij przycisk strzałki w prawo (**&gt;**) aby dodać rolę do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="2ed37-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="2ed37-144">Umieść kursor myszy nad nową rolę, więc widoczne ikonę ołówka.</span><span class="sxs-lookup"><span data-stu-id="2ed37-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="2ed37-145">Kliknij tę ikonę, aby zmienić nazwę roli.</span><span class="sxs-lookup"><span data-stu-id="2ed37-145">Click this icon to rename the role.</span></span> <span data-ttu-id="2ed37-146">Nazwa roli "SignalRChat", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="2ed37-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="2ed37-147">W **nowego projektu programu ASP.NET MVC 4** kreatora wybierz **aplikacji internetowej**.</span><span class="sxs-lookup"><span data-stu-id="2ed37-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="2ed37-148">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="2ed37-148">Click **OK**.</span></span> <span data-ttu-id="2ed37-149">Kreator projektu tworzy dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="2ed37-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="2ed37-150">ChatService: Ten projekt to aplikacja Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="2ed37-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="2ed37-151">Definiuje ról platformy Azure i inne opcje konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="2ed37-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="2ed37-152">SignalRChat: Ten projekt jest projekt platformy ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="2ed37-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="2ed37-153">Tworzenie aplikacji czatu SignalR</span><span class="sxs-lookup"><span data-stu-id="2ed37-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="2ed37-154">Aby utworzyć aplikację rozmowy, postępuj zgodnie z instrukcjami w tym samouczku [wprowadzenie do SignalR i MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="2ed37-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="2ed37-155">NuGet umożliwia instalowanie wymaganych bibliotek.</span><span class="sxs-lookup"><span data-stu-id="2ed37-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="2ed37-156">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="2ed37-156">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="2ed37-157">W **Konsola Menedżera pakietów** okna, wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="2ed37-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="2ed37-158">Użyj `-ProjectName` opcję, aby zainstalować te pakiety w projekcie ASP.NET MVC, a nie projekt platformy Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="2ed37-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="2ed37-159">Konfigurowanie systemu Backplane</span><span class="sxs-lookup"><span data-stu-id="2ed37-159">Configure the Backplane</span></span>

<span data-ttu-id="2ed37-160">W pliku Global.asax Twojej aplikacji Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="2ed37-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="2ed37-161">Teraz musisz pobrać parametry połączenia usługi service bus.</span><span class="sxs-lookup"><span data-stu-id="2ed37-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="2ed37-162">W witrynie Azure portal wybierz przestrzeń nazw magistrali usług, który został utworzony, a następnie kliknij ikonę klucza dostępu.</span><span class="sxs-lookup"><span data-stu-id="2ed37-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="2ed37-163">Skopiuj parametry połączenia do Schowka, a następnie wklej go do *connectionString* zmiennej.</span><span class="sxs-lookup"><span data-stu-id="2ed37-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="2ed37-164">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="2ed37-164">Deploy to Azure</span></span>

<span data-ttu-id="2ed37-165">W Eksploratorze rozwiązań rozwiń **role** folder wewnątrz projektu ChatService.</span><span class="sxs-lookup"><span data-stu-id="2ed37-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="2ed37-166">Kliknij prawym przyciskiem myszy rolę SignalRChat, a następnie wybierz pozycję **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="2ed37-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="2ed37-167">Wybierz **konfiguracji** kartę. W obszarze **wystąpień** wybierz opcję 2.</span><span class="sxs-lookup"><span data-stu-id="2ed37-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="2ed37-168">Rozmiar maszyny Wirtualnej można również ustawić, **wystąpieniom bardzo małe**.</span><span class="sxs-lookup"><span data-stu-id="2ed37-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="2ed37-169">Zapisz zmiany.</span><span class="sxs-lookup"><span data-stu-id="2ed37-169">Save the changes.</span></span>

<span data-ttu-id="2ed37-170">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt ChatService.</span><span class="sxs-lookup"><span data-stu-id="2ed37-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="2ed37-171">Wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="2ed37-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="2ed37-172">Jeśli jest to pierwszy czasu publikowania na platformie Windows Azure, należy pobrać swoje poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="2ed37-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="2ed37-173">W **Publikuj** kreatora, kliknij przycisk "Zaloguj się do pobierania poświadczenia".</span><span class="sxs-lookup"><span data-stu-id="2ed37-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="2ed37-174">To spowoduje wyświetlenie monitu zaloguj się do portalu usługi Windows Azure i Pobierz plik ustawień publikowania.</span><span class="sxs-lookup"><span data-stu-id="2ed37-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="2ed37-175">Kliknij przycisk **importu** i zaznacz plik ustawień publikowania, który został pobrany.</span><span class="sxs-lookup"><span data-stu-id="2ed37-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="2ed37-176">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="2ed37-176">Click **Next**.</span></span> <span data-ttu-id="2ed37-177">W **ustawień publikowania** okna dialogowego, w obszarze **usługi w chmurze**, wybierz usługę w chmurze, która została utworzona wcześniej.</span><span class="sxs-lookup"><span data-stu-id="2ed37-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="2ed37-178">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="2ed37-178">Click **Publish**.</span></span> <span data-ttu-id="2ed37-179">Może upłynąć kilka minut, aby wdrożyć aplikację i uruchomić maszyny wirtualne.</span><span class="sxs-lookup"><span data-stu-id="2ed37-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="2ed37-180">Teraz po uruchomieniu aplikacji rozmów z wystąpień roli komunikują się za pośrednictwem usługi Azure Service Bus, przy użyciu tematu usługi Service Bus.</span><span class="sxs-lookup"><span data-stu-id="2ed37-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="2ed37-181">Temat jest kolejki komunikatów, która umożliwia wielu subskrybentów.</span><span class="sxs-lookup"><span data-stu-id="2ed37-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="2ed37-182">Systemu backplane automatycznie tworzy, tematu i subskrypcji.</span><span class="sxs-lookup"><span data-stu-id="2ed37-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="2ed37-183">Aby zobaczyć subskrypcje i działania komunikatu, otwórz witrynę Azure portal, wybierz przestrzeń nazw usługi Service Bus i wybierz polecenie "Tematów".</span><span class="sxs-lookup"><span data-stu-id="2ed37-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="2ed37-184">To, że po kilku minutach dla działania komunikatu do wyświetlenia na pulpicie nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="2ed37-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="2ed37-185">SignalR zarządza czasem istnienia tematu.</span><span class="sxs-lookup"><span data-stu-id="2ed37-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="2ed37-186">Tak długo, jak Twoja aplikacja zostanie wdrożona, nie należy próbować ręcznie usuń tematy lub zmienić ustawienia w tej dziedzinie.</span><span class="sxs-lookup"><span data-stu-id="2ed37-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
