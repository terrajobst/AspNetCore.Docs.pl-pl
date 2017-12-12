---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Host ASP.NET Web API 2 w roli procesu roboczego platformy Azure | Dokumentacja firmy Microsoft
author: MikeWasson
description: "Ten samouczek pokazuje, jak do obsługi interfejsu API sieci Web platformy ASP.NET w roli procesu roboczego platformy Azure przy użyciu OWIN do hosta samodzielnego strukturę interfejsu API sieci Web. Otwórz interfejs sieci Web dla platformy .NET (OWIN) de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 326c4a4e274dbc1aa6e09f1d07c4d135e4304484
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="7ab56-104">Host ASP.NET Web API 2 w roli procesu roboczego platformy Azure</span><span class="sxs-lookup"><span data-stu-id="7ab56-104">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>
====================
<span data-ttu-id="7ab56-105">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7ab56-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="7ab56-106">Ten samouczek pokazuje, jak do obsługi interfejsu API sieci Web platformy ASP.NET w roli procesu roboczego platformy Azure przy użyciu OWIN do hosta samodzielnego strukturę interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7ab56-106">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="7ab56-107">[Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (OWIN) definiuje abstrakcję między serwerami sieci web .NET i aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="7ab56-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="7ab56-108">OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN idealne rozwiązanie w przypadku samodzielnej obsługi aplikacji sieci web w własnego procesu poza usług IIS — na przykład w roli procesu roboczego platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="7ab56-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="7ab56-109">W tym samouczku użyjesz pakietu Microsoft.Owin.Host.HttpListener zapewniające serwera HTTP używany do hosta samodzielnego aplikacji OWIN.</span><span class="sxs-lookup"><span data-stu-id="7ab56-109">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7ab56-110">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="7ab56-110">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="7ab56-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7ab56-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="7ab56-112">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="7ab56-112">Web API 2</span></span>
> - [<span data-ttu-id="7ab56-113">Zestaw Azure SDK dla platformy .NET w wersji 2.3</span><span class="sxs-lookup"><span data-stu-id="7ab56-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/en-us/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="7ab56-114">Tworzenie projektu platformy Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="7ab56-114">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="7ab56-115">Uruchom program Visual Studio z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="7ab56-115">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="7ab56-116">Aby debugować aplikację lokalnie, przy użyciu emulatora obliczeń platformy Azure, potrzebne są uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="7ab56-116">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="7ab56-117">Na **pliku** menu, kliknij przycisk **nowy**, następnie kliknij przycisk **projektu**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-117">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="7ab56-118">Z **zainstalowane szablony**, w obszarze Visual C#, kliknij przycisk **chmury** , a następnie kliknij przycisk **usługi w chmurze Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-118">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="7ab56-119">Nazwij projekt "AzureApp", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-119">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="7ab56-120">W **nowej usługi systemu Windows Azure Cloud** okna dialogowego, kliknij dwukrotnie **roli procesu roboczego**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-120">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="7ab56-121">Pozostaw nazwę domyślną ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="7ab56-121">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="7ab56-122">Ten krok powoduje dodanie roli procesu roboczego do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="7ab56-122">This step adds a worker role to the solution.</span></span> <span data-ttu-id="7ab56-123">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-123">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="7ab56-124">Rozwiązanie programu Visual Studio, która jest tworzona zawiera dwa projekty:</span><span class="sxs-lookup"><span data-stu-id="7ab56-124">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="7ab56-125">&quot;AzureApp&quot; definiuje ról i konfiguracji aplikacji Azure.</span><span class="sxs-lookup"><span data-stu-id="7ab56-125">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="7ab56-126">&quot;WorkerRole1&quot; zawiera kod roli proces roboczy.</span><span class="sxs-lookup"><span data-stu-id="7ab56-126">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="7ab56-127">Ogólnie rzecz biorąc aplikacja Azure może zawierać wiele ról, chociaż w tym samouczku korzysta z jedną rolę.</span><span class="sxs-lookup"><span data-stu-id="7ab56-127">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="7ab56-128">Dodaj składnik Web API i OWIN pakietów</span><span class="sxs-lookup"><span data-stu-id="7ab56-128">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="7ab56-129">Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki**, następnie kliknij przycisk **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-129">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="7ab56-130">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7ab56-130">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="7ab56-131">Dodawanie punktu końcowego HTTP</span><span class="sxs-lookup"><span data-stu-id="7ab56-131">Add an HTTP Endpoint</span></span>

<span data-ttu-id="7ab56-132">W Eksploratorze rozwiązań rozwiń projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="7ab56-132">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="7ab56-133">Rozwiń węzeł ról, kliknij prawym przyciskiem myszy WorkerRole1, a następnie wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-133">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="7ab56-134">Kliknij przycisk **punkty końcowe**, a następnie kliknij przycisk **Dodawanie punktu końcowego**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-134">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="7ab56-135">W **protokołu** listy rozwijanej wybierz opcję "http".</span><span class="sxs-lookup"><span data-stu-id="7ab56-135">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="7ab56-136">W **Port publiczny** i **Port prywatny**, wpisz 80.</span><span class="sxs-lookup"><span data-stu-id="7ab56-136">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="7ab56-137">Numery portów mogą być różne.</span><span class="sxs-lookup"><span data-stu-id="7ab56-137">These port numbers can be different.</span></span> <span data-ttu-id="7ab56-138">Port publiczny jest co używana przez klientów podczas wysyłania żądania do roli.</span><span class="sxs-lookup"><span data-stu-id="7ab56-138">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="7ab56-139">Skonfiguruj interfejs API sieci Web dla hosta samodzielnego</span><span class="sxs-lookup"><span data-stu-id="7ab56-139">Configure Web API for Self-Host</span></span>

<span data-ttu-id="7ab56-140">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt WorkerRole1 i wybierz **Dodaj** / **klasy** Aby dodać nową klasę.</span><span class="sxs-lookup"><span data-stu-id="7ab56-140">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="7ab56-141">Nazwa klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="7ab56-141">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="7ab56-142">Zastąp cały schematyczny kod w tym pliku następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7ab56-142">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="7ab56-143">Dodawanie kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="7ab56-143">Add a Web API Controller</span></span>

<span data-ttu-id="7ab56-144">Następnie Dodaj klasę kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7ab56-144">Next, add a Web API controller class.</span></span> <span data-ttu-id="7ab56-145">Kliknij prawym przyciskiem myszy projekt WorkerRole1 i wybierz **Dodaj** / **klasy**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-145">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="7ab56-146">Nazwa klasy kontrolera testów.</span><span class="sxs-lookup"><span data-stu-id="7ab56-146">Name the class TestController.</span></span> <span data-ttu-id="7ab56-147">Zastąp cały schematyczny kod w tym pliku następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7ab56-147">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="7ab56-148">Dla uproszczenia tego kontrolera po prostu definiuje dwie metody GET, które zwracają zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="7ab56-148">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="7ab56-149">Uruchom hosta OWIN</span><span class="sxs-lookup"><span data-stu-id="7ab56-149">Start the OWIN Host</span></span>

<span data-ttu-id="7ab56-150">Otwórz plik WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="7ab56-150">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="7ab56-151">Ta klasa definiuje kod, uruchamiany w momencie uruchamiania i zatrzymywania roli procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="7ab56-151">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="7ab56-152">Dodaj następującą instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="7ab56-152">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="7ab56-153">Dodaj **IDisposable** członka `WorkerRole` klasy:</span><span class="sxs-lookup"><span data-stu-id="7ab56-153">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="7ab56-154">W `OnStart` metody, Dodaj następujący kod, aby uruchomić hosta:</span><span class="sxs-lookup"><span data-stu-id="7ab56-154">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="7ab56-155">**WebApp.Start** metoda uruchamia hosta OWIN.</span><span class="sxs-lookup"><span data-stu-id="7ab56-155">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="7ab56-156">Nazwa `Startup` klasy jest parametrem typu metody.</span><span class="sxs-lookup"><span data-stu-id="7ab56-156">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="7ab56-157">Według Konwencji wywoła hosta `Configure` metody tej klasy.</span><span class="sxs-lookup"><span data-stu-id="7ab56-157">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="7ab56-158">Zastąpienie `OnStop` zlikwidować  *\_aplikacji* wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="7ab56-158">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="7ab56-159">W tym miejscu jest kompletny kod dla WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="7ab56-159">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="7ab56-160">Skompiluj rozwiązanie, a następnie naciśnij klawisz F5, aby uruchomić aplikację lokalnie w emulatorze obliczeniowe Azure.</span><span class="sxs-lookup"><span data-stu-id="7ab56-160">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="7ab56-161">W zależności od ustawienia zapory konieczne może być Zezwalaj emulatora przez zaporę.</span><span class="sxs-lookup"><span data-stu-id="7ab56-161">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="7ab56-162">Jeśli otrzymasz wyjątek podobnie do poniższego, można znaleźć pod adresem [ten wpis w blogu](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) obejście tego problemu.</span><span class="sxs-lookup"><span data-stu-id="7ab56-162">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="7ab56-163">"Nie można załadować pliku lub zestawu ' Microsoft.Owin, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35" lub jednej z jego zależności.</span><span class="sxs-lookup"><span data-stu-id="7ab56-163">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="7ab56-164">Definicja manifestu zestawu znajduje nie odpowiada odwołaniu do zestawu.</span><span class="sxs-lookup"><span data-stu-id="7ab56-164">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="7ab56-165">(Wyjątek od HRESULT: 0x80131040) "</span><span class="sxs-lookup"><span data-stu-id="7ab56-165">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="7ab56-166">Emulator obliczeń przypisuje lokalny adres IP punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="7ab56-166">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="7ab56-167">Adres IP można znaleźć, wyświetlając interfejs użytkownika emulatora obliczeń.</span><span class="sxs-lookup"><span data-stu-id="7ab56-167">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="7ab56-168">Kliknij prawym przyciskiem myszy ikonę emulatora w pasku obszaru powiadomień zadań, a następnie wybierz **Pokaż interfejs użytkownika emulatora obliczeń**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-168">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="7ab56-169">Znajdowanie adresu IP w ramach wdrożenia usługi, wdrażania [id], szczegóły usługi.</span><span class="sxs-lookup"><span data-stu-id="7ab56-169">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="7ab56-170">Otwórz przeglądarkę sieci web i przejdź do http://*adres*/test/1, gdzie *adres* to adres IP przypisany przez emulator obliczeń; na przykład `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="7ab56-170">Open a web browser and navigate to http://*address*/test/1, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="7ab56-171">Powinny pojawić się odpowiedzi z kontrolera interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="7ab56-171">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="7ab56-172">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="7ab56-172">Deploy to Azure</span></span>

<span data-ttu-id="7ab56-173">W tym kroku musi mieć konto platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="7ab56-173">For this step, you must have an Azure account.</span></span> <span data-ttu-id="7ab56-174">Jeśli nie masz jeszcze jeden, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut.</span><span class="sxs-lookup"><span data-stu-id="7ab56-174">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="7ab56-175">Aby uzyskać więcej informacji, zobacz [bezpłatna wersja próbna programu Microsoft Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="7ab56-175">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="7ab56-176">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="7ab56-176">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="7ab56-177">Wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-177">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="7ab56-178">Jeśli użytkownik nie jest zalogowany do konta platformy Azure, kliknij przycisk **logowania**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-178">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="7ab56-179">Po zarejestrowaniu w Wybierz subskrypcję i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-179">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="7ab56-180">Wprowadź nazwę usługi w chmurze i wybierz region.</span><span class="sxs-lookup"><span data-stu-id="7ab56-180">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="7ab56-181">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-181">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="7ab56-182">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="7ab56-182">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="7ab56-183">Okno Dziennik aktywności platformy Azure będzie wyświetlany postęp wdrażania.</span><span class="sxs-lookup"><span data-stu-id="7ab56-183">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="7ab56-184">Gdy aplikacja jest wdrożona, przejdź do http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="7ab56-184">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="7ab56-185">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7ab56-185">Additional Resources</span></span>

- [<span data-ttu-id="7ab56-186">Przegląd projektu Katana</span><span class="sxs-lookup"><span data-stu-id="7ab56-186">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="7ab56-187">Projekt Katana w witrynie GitHub</span><span class="sxs-lookup"><span data-stu-id="7ab56-187">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
