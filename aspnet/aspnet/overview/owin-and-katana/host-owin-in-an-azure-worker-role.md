---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Host OWIN w roli procesu roboczego platformy Azure | Dokumentacja firmy Microsoft
author: MikeWasson
description: "W tym samouczku przedstawiono sposób hosta samodzielnego OWIN roli procesu roboczego Microsoft Azure. Otwórz interfejs sieci Web dla platformy .NET (OWIN) definiuje abstrakcję między serwerem sieci web platformy .NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 647514ae5a92b9d729179327fb97bd8005b0a4b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="4501d-104">Host OWIN w roli procesu roboczego platformy Azure</span><span class="sxs-lookup"><span data-stu-id="4501d-104">Host OWIN in an Azure Worker Role</span></span>
====================
<span data-ttu-id="4501d-105">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4501d-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="4501d-106">W tym samouczku przedstawiono sposób hosta samodzielnego OWIN roli procesu roboczego Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="4501d-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
> 
> <span data-ttu-id="4501d-107">[Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (OWIN) definiuje abstrakcję między serwerami sieci web .NET i aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="4501d-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="4501d-108">OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN idealne rozwiązanie w przypadku samodzielnej obsługi aplikacji sieci web w własnego procesu poza usług IIS — na przykład w roli procesu roboczego platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="4501d-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="4501d-109">Z tego samouczka dowiesz się, jak własnym obsługi aplikacji OWIN wewnątrz roli procesu roboczego Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="4501d-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="4501d-110">Aby dowiedzieć się więcej na temat roli proces roboczy, zobacz [modeli wykonywania Azure](https://azure.microsoft.com/en-us/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="4501d-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/en-us/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4501d-111">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="4501d-111">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="4501d-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4501d-112">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [<span data-ttu-id="4501d-113">Zestaw Azure SDK dla platformy .NET w wersji 2.3</span><span class="sxs-lookup"><span data-stu-id="4501d-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/en-us/downloads/)
> - [<span data-ttu-id="4501d-114">Microsoft.Owin.Selfhost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="4501d-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="4501d-115">Tworzenie projektu platformy Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="4501d-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="4501d-116">Uruchom program Visual Studio z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="4501d-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="4501d-117">Aby debugować aplikację lokalnie, przy użyciu emulatora obliczeń platformy Azure, potrzebne są uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="4501d-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="4501d-118">Na **pliku** menu, kliknij przycisk **nowy**, następnie kliknij przycisk **projektu**.</span><span class="sxs-lookup"><span data-stu-id="4501d-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="4501d-119">Z **zainstalowane szablony**, w obszarze Visual C#, kliknij przycisk **chmury** , a następnie kliknij przycisk **usługi w chmurze Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="4501d-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="4501d-120">Nazwij projekt "AzureApp", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4501d-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="4501d-121">W **nowej usługi systemu Windows Azure Cloud** okna dialogowego, kliknij dwukrotnie **roli procesu roboczego**.</span><span class="sxs-lookup"><span data-stu-id="4501d-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="4501d-122">Pozostaw nazwę domyślną ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="4501d-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="4501d-123">Ten krok powoduje dodanie roli procesu roboczego do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="4501d-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="4501d-124">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4501d-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="4501d-125">Rozwiązanie programu Visual Studio, która jest tworzona zawiera dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="4501d-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="4501d-126">&quot;AzureApp&quot; definiuje ról i konfiguracji aplikacji Azure.</span><span class="sxs-lookup"><span data-stu-id="4501d-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="4501d-127">&quot;WorkerRole1&quot; zawiera kod roli proces roboczy.</span><span class="sxs-lookup"><span data-stu-id="4501d-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="4501d-128">Ogólnie rzecz biorąc aplikacja Azure może zawierać wiele ról, chociaż w tym samouczku korzysta z jedną rolę.</span><span class="sxs-lookup"><span data-stu-id="4501d-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="4501d-129">Dodawanie pakietów hosta samodzielnego OWIN</span><span class="sxs-lookup"><span data-stu-id="4501d-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="4501d-130">Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki**, następnie kliknij przycisk **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="4501d-130">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="4501d-131">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="4501d-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="4501d-132">Dodawanie punktu końcowego HTTP</span><span class="sxs-lookup"><span data-stu-id="4501d-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="4501d-133">W Eksploratorze rozwiązań rozwiń projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="4501d-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="4501d-134">Rozwiń węzeł ról, kliknij prawym przyciskiem myszy WorkerRole1, a następnie wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="4501d-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="4501d-135">Kliknij przycisk **punkty końcowe**, a następnie kliknij przycisk **Dodawanie punktu końcowego**.</span><span class="sxs-lookup"><span data-stu-id="4501d-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="4501d-136">W **protokołu** listy rozwijanej wybierz opcję "http".</span><span class="sxs-lookup"><span data-stu-id="4501d-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="4501d-137">W **Port publiczny** i **Port prywatny**, wpisz 80.</span><span class="sxs-lookup"><span data-stu-id="4501d-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="4501d-138">Numery portów mogą być różne.</span><span class="sxs-lookup"><span data-stu-id="4501d-138">These port numbers can be different.</span></span> <span data-ttu-id="4501d-139">Port publiczny jest co używana przez klientów podczas wysyłania żądania do roli.</span><span class="sxs-lookup"><span data-stu-id="4501d-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="4501d-140">Tworzenie klasy początkowej OWIN</span><span class="sxs-lookup"><span data-stu-id="4501d-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="4501d-141">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt WorkerRole1 i wybierz **Dodaj** / **klasy** Aby dodać nową klasę.</span><span class="sxs-lookup"><span data-stu-id="4501d-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="4501d-142">Nazwa klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="4501d-142">Name the class `Startup`.</span></span>

<span data-ttu-id="4501d-143">Zamień wszystkie schematyczny kod poniżej:</span><span class="sxs-lookup"><span data-stu-id="4501d-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="4501d-144">`UseWelcomePage` — Metoda rozszerzenia dodaje prosty strony HTML do aplikacji, aby sprawdzić lokacji działa.</span><span class="sxs-lookup"><span data-stu-id="4501d-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="4501d-145">Uruchom hosta OWIN</span><span class="sxs-lookup"><span data-stu-id="4501d-145">Start the OWIN Host</span></span>

<span data-ttu-id="4501d-146">Otwórz plik WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="4501d-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="4501d-147">Ta klasa definiuje kod, uruchamiany w momencie uruchamiania i zatrzymywania roli procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="4501d-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="4501d-148">Dodaj następującą instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="4501d-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="4501d-149">Dodaj **IDisposable** członka `WorkerRole` klasy:</span><span class="sxs-lookup"><span data-stu-id="4501d-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="4501d-150">W `OnStart` metody, Dodaj następujący kod, aby uruchomić hosta:</span><span class="sxs-lookup"><span data-stu-id="4501d-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="4501d-151">**WebApp.Start** metoda uruchamia hosta OWIN.</span><span class="sxs-lookup"><span data-stu-id="4501d-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="4501d-152">Nazwa `Startup` klasy jest parametrem typu metody.</span><span class="sxs-lookup"><span data-stu-id="4501d-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="4501d-153">Według Konwencji wywoła hosta `Configure` metody tej klasy.</span><span class="sxs-lookup"><span data-stu-id="4501d-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="4501d-154">Zastąpienie `OnStop` zlikwidować  *\_aplikacji* wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="4501d-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="4501d-155">W tym miejscu jest kompletny kod dla WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="4501d-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="4501d-156">Skompiluj rozwiązanie, a następnie naciśnij klawisz F5, aby uruchomić aplikację lokalnie w emulatorze obliczeniowe Azure.</span><span class="sxs-lookup"><span data-stu-id="4501d-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="4501d-157">W zależności od ustawienia zapory konieczne może być Zezwalaj emulatora przez zaporę.</span><span class="sxs-lookup"><span data-stu-id="4501d-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="4501d-158">Emulator obliczeń przypisuje lokalny adres IP punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="4501d-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="4501d-159">Adres IP można znaleźć, wyświetlając interfejs użytkownika emulatora obliczeń.</span><span class="sxs-lookup"><span data-stu-id="4501d-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="4501d-160">Kliknij prawym przyciskiem myszy ikonę emulatora w pasku obszaru powiadomień zadań, a następnie wybierz **Pokaż interfejs użytkownika emulatora obliczeń**.</span><span class="sxs-lookup"><span data-stu-id="4501d-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="4501d-161">Znajdowanie adresu IP w ramach wdrożenia usługi, wdrażania [id], szczegóły usługi.</span><span class="sxs-lookup"><span data-stu-id="4501d-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="4501d-162">Otwórz przeglądarkę sieci web i przejdź do http://*adres*, gdzie *adres* to adres IP przypisany przez emulator obliczeń; na przykład `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="4501d-162">Open a web browser and navigate to http://*address*, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="4501d-163">Powinna zostać wyświetlona strona powitalna OWIN:</span><span class="sxs-lookup"><span data-stu-id="4501d-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="4501d-164">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="4501d-164">Deploy to Azure</span></span>

<span data-ttu-id="4501d-165">W tym kroku musi mieć konto platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="4501d-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="4501d-166">Jeśli nie masz jeszcze jeden, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut.</span><span class="sxs-lookup"><span data-stu-id="4501d-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="4501d-167">Aby uzyskać więcej informacji, zobacz [bezpłatna wersja próbna programu Microsoft Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="4501d-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="4501d-168">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="4501d-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="4501d-169">Wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="4501d-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="4501d-170">Jeśli użytkownik nie jest zalogowany do konta platformy Azure, kliknij przycisk **logowania**.</span><span class="sxs-lookup"><span data-stu-id="4501d-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="4501d-171">Po zarejestrowaniu w Wybierz subskrypcję i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="4501d-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="4501d-172">Wprowadź nazwę usługi w chmurze i wybierz region.</span><span class="sxs-lookup"><span data-stu-id="4501d-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="4501d-173">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="4501d-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="4501d-174">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="4501d-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="4501d-175">Okno Dziennik aktywności platformy Azure będzie wyświetlany postęp wdrażania.</span><span class="sxs-lookup"><span data-stu-id="4501d-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="4501d-176">Gdy aplikacja jest wdrożona, przejdź do `http://appname.cloudapp.net/`, gdzie *appname* to nazwa usługi w chmurze.</span><span class="sxs-lookup"><span data-stu-id="4501d-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4501d-177">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4501d-177">Additional Resources</span></span>

- [<span data-ttu-id="4501d-178">Przegląd projektu Katana</span><span class="sxs-lookup"><span data-stu-id="4501d-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="4501d-179">Projekt Katana w witrynie GitHub</span><span class="sxs-lookup"><span data-stu-id="4501d-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
