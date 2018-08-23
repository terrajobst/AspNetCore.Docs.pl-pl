---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Hostowanie OWIN w roli procesu roboczego platformy Azure | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym samouczku pokazano, jak na potrzeby samodzielnego hostowania OWIN w roli procesu roboczego Microsoft Azure. Open Web Interface for .NET (OWIN) definiuje abstrakcję między serwerem sieci web platformy .NET...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 6bead915491c62de809b8625d8071a63c70a6ef5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756223"
---
<a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="bca94-104">Hostowanie OWIN w roli procesu roboczego platformy Azure</span><span class="sxs-lookup"><span data-stu-id="bca94-104">Host OWIN in an Azure Worker Role</span></span>
====================
<span data-ttu-id="bca94-105">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bca94-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="bca94-106">W tym samouczku pokazano, jak na potrzeby samodzielnego hostowania OWIN w roli procesu roboczego Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="bca94-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
> 
> <span data-ttu-id="bca94-107">[Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (OWIN) definiuje abstrakcję między serwerami sieci web platformy .NET i aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="bca94-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="bca94-108">OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN jest idealnym rozwiązaniem dla hostingu samodzielnego aplikacji sieci web w swoim własnym procesie, poza programem IIS — na przykład w roli procesu roboczego platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="bca94-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="bca94-109">W tym samouczku dowiesz się, jak na potrzeby samodzielnego hostowania aplikacji OWIN w roli procesu roboczego Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="bca94-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="bca94-110">Aby dowiedzieć się więcej o rolach procesów roboczych, zobacz [modele wykonywania Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="bca94-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bca94-111">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="bca94-111">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="bca94-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bca94-112">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [<span data-ttu-id="bca94-113">Zestaw Azure SDK dla platformy .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="bca94-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)
> - [<span data-ttu-id="bca94-114">Microsoft.Owin.Selfhost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="bca94-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="bca94-115">Tworzenie projektu platformy Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="bca94-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="bca94-116">Uruchom program Visual Studio z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="bca94-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="bca94-117">Aby debugować aplikację lokalnie, za pomocą emulatora obliczeń platformy Azure potrzebne są uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="bca94-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="bca94-118">Na **pliku** menu, kliknij przycisk **New**, następnie kliknij przycisk **projektu**.</span><span class="sxs-lookup"><span data-stu-id="bca94-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="bca94-119">Z **zainstalowane szablony**, w obszarze Visual C#, kliknij przycisk **chmury** a następnie kliknij przycisk **usłudze Windows Azure Cloud Services**.</span><span class="sxs-lookup"><span data-stu-id="bca94-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="bca94-120">Nadaj projektowi nazwę "AzureApp", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="bca94-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="bca94-121">W **nową usługę w chmurze Azure Windows** okno dialogowe, kliknij dwukrotnie **roli procesu roboczego**.</span><span class="sxs-lookup"><span data-stu-id="bca94-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="bca94-122">Pozostaw nazwę domyślną ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="bca94-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="bca94-123">Ten krok powoduje dodanie roli procesu roboczego do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="bca94-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="bca94-124">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="bca94-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="bca94-125">Rozwiązanie programu Visual Studio, który jest tworzony zawiera dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="bca94-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="bca94-126">&quot;AzureApp&quot; definiuje ról i konfiguracji dla aplikacji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="bca94-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="bca94-127">&quot;WorkerRole1&quot; zawiera kod dla roli procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="bca94-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="bca94-128">Ogólnie rzecz biorąc aplikacją platformy Azure może zawierać wiele ról, chociaż w tym samouczku korzysta z jedną rolę.</span><span class="sxs-lookup"><span data-stu-id="bca94-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="bca94-129">Dodaj pakiety hosta samodzielnego OWIN</span><span class="sxs-lookup"><span data-stu-id="bca94-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="bca94-130">Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki**, następnie kliknij przycisk **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="bca94-130">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="bca94-131">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bca94-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="bca94-132">Dodaj punkt końcowy HTTP</span><span class="sxs-lookup"><span data-stu-id="bca94-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="bca94-133">W Eksploratorze rozwiązań rozwiń projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="bca94-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="bca94-134">Rozwiń węzeł ról, kliknij prawym przyciskiem myszy WorkerRole1, a następnie wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="bca94-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="bca94-135">Kliknij przycisk **punktów końcowych**, a następnie kliknij przycisk **Dodaj punkt końcowy**.</span><span class="sxs-lookup"><span data-stu-id="bca94-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="bca94-136">W **protokołu** listy rozwijanej wybierz opcję "http".</span><span class="sxs-lookup"><span data-stu-id="bca94-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="bca94-137">W **Port publiczny** i **Port prywatny**, wpisz 80.</span><span class="sxs-lookup"><span data-stu-id="bca94-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="bca94-138">Te numery portów mogą być różne.</span><span class="sxs-lookup"><span data-stu-id="bca94-138">These port numbers can be different.</span></span> <span data-ttu-id="bca94-139">Port publiczny jest o tym, co klienci używają wysłanie żądania do roli.</span><span class="sxs-lookup"><span data-stu-id="bca94-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="bca94-140">Tworzenie klasy początkowej OWIN</span><span class="sxs-lookup"><span data-stu-id="bca94-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="bca94-141">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt WorkerRole1, a następnie wybierz **Dodaj** / **klasy** Aby dodać nową klasę.</span><span class="sxs-lookup"><span data-stu-id="bca94-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="bca94-142">Nazwa klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="bca94-142">Name the class `Startup`.</span></span>

<span data-ttu-id="bca94-143">Zastąp cały kod standardowy następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="bca94-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="bca94-144">`UseWelcomePage` — Metoda rozszerzenia dodaje prostą stronę HTML do aplikacji, aby sprawdzić lokacji działa.</span><span class="sxs-lookup"><span data-stu-id="bca94-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="bca94-145">Uruchom hosta OWIN</span><span class="sxs-lookup"><span data-stu-id="bca94-145">Start the OWIN Host</span></span>

<span data-ttu-id="bca94-146">Otwórz plik WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="bca94-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="bca94-147">Ta klasa definiuje kod, który jest uruchamiany podczas uruchamiania i zatrzymywania roli procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="bca94-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="bca94-148">Dodaj następującą instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="bca94-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="bca94-149">Dodaj **interfejsu IDisposable** członka do `WorkerRole` klasy:</span><span class="sxs-lookup"><span data-stu-id="bca94-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="bca94-150">W `OnStart` metody, Dodaj następujący kod, aby uruchomić hosta:</span><span class="sxs-lookup"><span data-stu-id="bca94-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="bca94-151">**WebApp.Start** metoda uruchamia hosta OWIN.</span><span class="sxs-lookup"><span data-stu-id="bca94-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="bca94-152">Nazwa `Startup` klasy jest parametrem typu w metodzie.</span><span class="sxs-lookup"><span data-stu-id="bca94-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="bca94-153">Zgodnie z Konwencją, host będzie wywoływać `Configure` metody tej klasy.</span><span class="sxs-lookup"><span data-stu-id="bca94-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="bca94-154">Zastąp `OnStop` do rozporządzania  *\_aplikacji* wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="bca94-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="bca94-155">Oto kompletny kod dla WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="bca94-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="bca94-156">Skompiluj rozwiązanie, a następnie naciśnij klawisz F5, aby uruchomić aplikację lokalnie w emulatorze obliczeń systemu Azure.</span><span class="sxs-lookup"><span data-stu-id="bca94-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="bca94-157">W zależności od ustawień zapory konieczne może być Zezwalaj na emulator przez zaporę.</span><span class="sxs-lookup"><span data-stu-id="bca94-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="bca94-158">Emulator obliczeń przypisuje lokalny adres IP punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="bca94-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="bca94-159">Adres IP można znaleźć, wyświetlając interfejs użytkownika emulatora obliczeń.</span><span class="sxs-lookup"><span data-stu-id="bca94-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="bca94-160">Kliknij prawym przyciskiem myszy ikonę emulatora w zadaniu paska w obszarze powiadomień, a następnie wybierz pozycję **Pokaż interfejs użytkownika emulatora obliczeń**.</span><span class="sxs-lookup"><span data-stu-id="bca94-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="bca94-161">Znajdowanie adresu IP, w ramach wdrożenia usług, wdrożenia [identyfikator], szczegóły usługi.</span><span class="sxs-lookup"><span data-stu-id="bca94-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="bca94-162">Otwórz przeglądarkę internetową i przejdź do http://<em>adres</em>, gdzie <em>adres</em> jest adres IP przypisany przez emulator obliczeń; na przykład `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="bca94-162">Open a web browser and navigate to http://<em>address</em>, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="bca94-163">Powinny pojawić się Strona powitalna OWIN:</span><span class="sxs-lookup"><span data-stu-id="bca94-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="bca94-164">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="bca94-164">Deploy to Azure</span></span>

<span data-ttu-id="bca94-165">W tym kroku musisz mieć konto platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="bca94-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="bca94-166">Jeśli nie masz jeszcze jeden, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut.</span><span class="sxs-lookup"><span data-stu-id="bca94-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="bca94-167">Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="bca94-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="bca94-168">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="bca94-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="bca94-169">Wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="bca94-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="bca94-170">Jeśli nie jest zalogowany do konta platformy Azure, kliknij przycisk **Sign In**.</span><span class="sxs-lookup"><span data-stu-id="bca94-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="bca94-171">Po użytkownik jest zalogowany, wybierz subskrypcję i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="bca94-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="bca94-172">Wprowadź nazwę usługi w chmurze, a następnie wybierz region.</span><span class="sxs-lookup"><span data-stu-id="bca94-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="bca94-173">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="bca94-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="bca94-174">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="bca94-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="bca94-175">Okno Dziennik aktywności platformy Azure Pokazuje postęp wdrażania.</span><span class="sxs-lookup"><span data-stu-id="bca94-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="bca94-176">Gdy aplikacja jest wdrożona, przejdź do `http://appname.cloudapp.net/`, gdzie *appname* to nazwa usługi w chmurze.</span><span class="sxs-lookup"><span data-stu-id="bca94-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bca94-177">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bca94-177">Additional Resources</span></span>

- [<span data-ttu-id="bca94-178">Omówienie projektu Katana</span><span class="sxs-lookup"><span data-stu-id="bca94-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="bca94-179">Projektu Katana w usłudze GitHub</span><span class="sxs-lookup"><span data-stu-id="bca94-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
