---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Samouczek: Host samodzielny SignalR | Dokumentacja firmy Microsoft'
author: pfletcher
description: W tym samouczku pokazano, jak utworzyć samodzielnie hostowany serwer SignalR 2 i jak połączyć się z nim za pomocą klienta języka JavaScript. Wersje oprogramowania używanego w tym samouczku V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 34e307499c7dabcd2afc73780649d79e68c10fdc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379001"
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="5d76b-104">Samouczek: Host samodzielny SignalR</span><span class="sxs-lookup"><span data-stu-id="5d76b-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="5d76b-105">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5d76b-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="5d76b-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="5d76b-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="5d76b-107">W tym samouczku pokazano, jak utworzyć samodzielnie hostowany serwer SignalR 2 i jak połączyć się z nim za pomocą klienta języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5d76b-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5d76b-108">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="5d76b-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="5d76b-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5d76b-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="5d76b-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5d76b-110">.NET 4.5</span></span>
> - <span data-ttu-id="5d76b-111">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="5d76b-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="5d76b-112">Z tego samouczka przy użyciu programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="5d76b-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="5d76b-113">Aby użyć programu Visual Studio 2012 za pomocą tego samouczka, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="5d76b-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="5d76b-114">Aktualizacja usługi [Menedżera pakietów](http://docs.nuget.org/docs/start-here/installing-nuget) do najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="5d76b-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="5d76b-115">Zainstaluj [Instalator platformy sieci Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="5d76b-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="5d76b-116">Instalator platformy sieci Web, wyszukiwanie i instalowanie **platformy ASP.NET i Web Tools 2013.1 dla programu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="5d76b-117">Szablony programu Visual Studio dla klas SignalR spowoduje to zainstalowanie takich jak **Centrum**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="5d76b-118">Niektóre szablony (takie jak **klasy początkowej OWIN**) nie są dostępne; w tym przypadku użyj pliku klasy.</span><span class="sxs-lookup"><span data-stu-id="5d76b-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="5d76b-119">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="5d76b-119">Questions and comments</span></span>
> 
> <span data-ttu-id="5d76b-120">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="5d76b-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5d76b-121">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5d76b-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="5d76b-122">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5d76b-122">Overview</span></span>

<span data-ttu-id="5d76b-123">Serwer biblioteki SignalR zwykle znajduje się w aplikacji ASP.NET w usługach IIS, ale może też być samodzielnie hostowane (na przykład w aplikacji konsoli lub usługa Windows) przy użyciu biblioteki hosta samodzielnego.</span><span class="sxs-lookup"><span data-stu-id="5d76b-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="5d76b-124">Ta biblioteka, podobnie jak wszystkie SignalR 2, jest oparta na OWIN ([Open Web Interface for .NET](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="5d76b-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="5d76b-125">OWIN definiuje abstrakcję między serwerami sieci web platformy .NET i aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="5d76b-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="5d76b-126">OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN jest idealnym rozwiązaniem dla hostingu samodzielnego aplikacji sieci web w swoim własnym procesie, poza usług IIS.</span><span class="sxs-lookup"><span data-stu-id="5d76b-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="5d76b-127">Nie obsługują w usługach IIS przyczyny:</span><span class="sxs-lookup"><span data-stu-id="5d76b-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="5d76b-128">Środowiska, w której usługi IIS są niedostępne lub pożądane, takich jak istniejącej farmy serwerów bez usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="5d76b-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="5d76b-129">Zmniejszenie wydajności programu IIS należy unikać.</span><span class="sxs-lookup"><span data-stu-id="5d76b-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="5d76b-130">Funkcja SignalR jest ma zostać dodany do istniejącego wystąpienia usług aplikacji, która działa w usługę Windows, rola procesu roboczego platformy Azure lub inny proces.</span><span class="sxs-lookup"><span data-stu-id="5d76b-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="5d76b-131">Jeśli to rozwiązanie jest obecnie sporządzana jako hosta samodzielnego ze względu na wydajność, zaleca się również testu, aplikacji hostowanych w usługach IIS, aby określić korzyści w zakresie wydajności.</span><span class="sxs-lookup"><span data-stu-id="5d76b-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="5d76b-132">Ten samouczek zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="5d76b-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="5d76b-133">Tworzenie serwera</span><span class="sxs-lookup"><span data-stu-id="5d76b-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="5d76b-134">Dostęp do serwera przy użyciu klienta języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="5d76b-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="5d76b-135">Tworzenie serwera</span><span class="sxs-lookup"><span data-stu-id="5d76b-135">Creating the server</span></span>

<span data-ttu-id="5d76b-136">W tym samouczku utworzysz serwer, który znajduje się w aplikacji konsoli, ale serwer może być hostowana w dowolny rodzaj procesu, takie jak Windows, usługi lub roli procesu roboczego platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="5d76b-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="5d76b-137">Przykładowy kod do obsługi serwera SignalR, w usłudze Windows, zobacz [Self-Hosting SignalR w usłudze Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="5d76b-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="5d76b-138">Otwórz program Visual Studio 2013 z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="5d76b-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="5d76b-139">Wybierz **pliku**, **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="5d76b-140">Wybierz **Windows** w obszarze **Visual C#** w węźle **szablony** okienka, a następnie wybierz **aplikację Konsolową** szablonu.</span><span class="sxs-lookup"><span data-stu-id="5d76b-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="5d76b-141">Nazwa nowego projektu "SignalRSelfHost", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="5d76b-142">Otwórz konsolę Menedżera pakietów biblioteki, wybierając **narzędzia**, **Menedżer pakietów biblioteki**, **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="5d76b-143">W konsoli Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5d76b-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="5d76b-144">To polecenie dodaje biblioteki SignalR 2 Self-Host do projektu.</span><span class="sxs-lookup"><span data-stu-id="5d76b-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="5d76b-145">W konsoli Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5d76b-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="5d76b-146">To polecenie dodaje bibliotekę Microsoft.Owin.Cors do projektu.</span><span class="sxs-lookup"><span data-stu-id="5d76b-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="5d76b-147">Ta biblioteka będzie służyć do obsługi między domenami, który jest wymagany dla aplikacji, które hostują SignalR i strony sieci web klienta w różnych domenach.</span><span class="sxs-lookup"><span data-stu-id="5d76b-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="5d76b-148">Ponieważ będziesz hostować, SignalR serwera i klienta sieci web na innym, oznacza to, że ten między domenami musi być włączona dla komunikacji między tymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="5d76b-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="5d76b-149">Zastąp zawartości Program.cs następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="5d76b-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="5d76b-150">Powyższy kod obejmuje trzy klasy:</span><span class="sxs-lookup"><span data-stu-id="5d76b-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="5d76b-151">**Program**, w tym **Main** Definiowanie ścieżki podstawowej wykonywania metody.</span><span class="sxs-lookup"><span data-stu-id="5d76b-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="5d76b-152">W przypadku tej metody, aplikacji sieci web typu **uruchamiania** została uruchomiona na określony adres URL (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="5d76b-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="5d76b-153">Jeśli zabezpieczeń jest wymagany w punkcie końcowym, można zaimplementować protokół SSL.</span><span class="sxs-lookup"><span data-stu-id="5d76b-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="5d76b-154">Zobacz [porady: Konfigurowanie portu z certyfikatem SSL](https://msdn.microsoft.com/library/ms733791.aspx) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="5d76b-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="5d76b-155">**Uruchamianie**, klasa zawierająca konfiguracje dla serwera biblioteki SignalR (konfiguracji tylko w tym samouczku używany jest wywołanie `UseCors`) i wywołanie `MapSignalR`, tworzy trasy dla obiektów Centrum w projekcie.</span><span class="sxs-lookup"><span data-stu-id="5d76b-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="5d76b-156">**MyHub**, klasa Centrum SignalR, zapewniające aplikacji dla klientów.</span><span class="sxs-lookup"><span data-stu-id="5d76b-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="5d76b-157">Ta klasa zawiera jedną metodę, **wysyłania**, że klienci wywoła wysyłać wiadomość do wszystkich innych połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="5d76b-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="5d76b-158">Skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="5d76b-158">Compile and run the application.</span></span> <span data-ttu-id="5d76b-159">Adres, który jest uruchomiony serwer powinien być wyświetlony w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="5d76b-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="5d76b-160">Jeśli wykonanie nie powiedzie się z wyjątkiem `System.Reflection.TargetInvocationException was unhandled`, konieczne będzie ponowne uruchomienie programu Visual Studio z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="5d76b-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="5d76b-161">Zatrzymaj aplikację przed przejściem do następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="5d76b-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="5d76b-162">Dostęp do serwera przy użyciu klienta języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="5d76b-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="5d76b-163">W tej sekcji użyjesz tego samego klienta JavaScript z [samouczka Wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="5d76b-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="5d76b-164">Wybierzemy jedną modyfikację do klienta, która umożliwia jawne zdefiniowanie URL koncentratora.</span><span class="sxs-lookup"><span data-stu-id="5d76b-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="5d76b-165">Przy użyciu samodzielnie hostowanej aplikacji serwer może nie koniecznie w ten sam adres co adres URL połączenia (z powodu serwery proxy i moduły równoważenia obciążenia), więc adres URL musi być jawnie zdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="5d76b-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="5d76b-166">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie i wybierz **Dodaj**, **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="5d76b-167">Wybierz **Web** , a następnie wybierz węzeł **aplikacji sieci Web ASP.NET** szablonu.</span><span class="sxs-lookup"><span data-stu-id="5d76b-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="5d76b-168">Nadaj projektowi nazwę "JavascriptClient", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="5d76b-169">Wybierz **pusty** szablonu i pozostaw niezaznaczoną pozostałe opcje.</span><span class="sxs-lookup"><span data-stu-id="5d76b-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="5d76b-170">Wybierz **Utwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="5d76b-171">W konsoli Menedżera pakietów, wybierz projekt "JavascriptClient" **projekt domyślny** listy rozwijanej, a następnie wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5d76b-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="5d76b-172">To polecenie instaluje biblioteki SignalR i JQuery, które będą potrzebne w kliencie.</span><span class="sxs-lookup"><span data-stu-id="5d76b-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="5d76b-173">Kliknij prawym przyciskiem myszy na projekt i wybierz **Dodaj**, **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="5d76b-174">Wybierz **Web** węzłem, a następnie wybierz stronę HTML.</span><span class="sxs-lookup"><span data-stu-id="5d76b-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="5d76b-175">Nazwij stronę **Default.html**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="5d76b-176">Zastąp zawartość strony HTML z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="5d76b-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="5d76b-177">Upewnij się, że odwołania do skryptu w tym miejscu są takie same skryptów w folderze skryptów projektu.</span><span class="sxs-lookup"><span data-stu-id="5d76b-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="5d76b-178">Poniższy kod (wyróżnione w powyższym przykładowym kodzie) jest dodanie zgłaszający klienta używanego w tym samouczku wprowadzenie Stared (oprócz uaktualnienie kodu do SignalR w wersji 2 beta).</span><span class="sxs-lookup"><span data-stu-id="5d76b-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="5d76b-179">Ten wiersz kodu jawnie ustawia adres URL podstawowego połączenia dla elementu SignalR na serwerze.</span><span class="sxs-lookup"><span data-stu-id="5d76b-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="5d76b-180">Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Ustaw projekty startowe...** . Wybierz **wiele projektów startowych** przycisk radiowy, a następnie ustaw oba projekty **akcji** do **Start**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="5d76b-181">Kliknij prawym przyciskiem myszy na "Default.html", a następnie wybierz pozycję **Ustaw jako stronę startową**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="5d76b-182">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="5d76b-182">Run the application.</span></span> <span data-ttu-id="5d76b-183">Zostanie uruchomiony serwer i strony.</span><span class="sxs-lookup"><span data-stu-id="5d76b-183">The server and page will launch.</span></span> <span data-ttu-id="5d76b-184">Może być konieczne ponowne załadowanie strony sieci web (lub wybierz **Kontynuuj** w debugerze) Jeśli strona ładuje się przed rozpoczęciem serwera.</span><span class="sxs-lookup"><span data-stu-id="5d76b-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="5d76b-185">W przeglądarce należy podać nazwę użytkownika po wyświetleniu monitu.</span><span class="sxs-lookup"><span data-stu-id="5d76b-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="5d76b-186">Skopiuj adres URL strony w innej karcie przeglądarki lub w oknie i podaj inną nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5d76b-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="5d76b-187">Będzie mogła wysyłać wiadomości z okienka jednej przeglądarki do innego, tak jak samouczka Wprowadzenie.</span><span class="sxs-lookup"><span data-stu-id="5d76b-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
