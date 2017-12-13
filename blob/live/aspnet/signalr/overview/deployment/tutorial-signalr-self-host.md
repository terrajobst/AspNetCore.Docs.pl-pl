---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Samouczek: SignalR hosta samodzielnego | Dokumentacja firmy Microsoft'
author: pfletcher
description: "Ten samouczek pokazuje, jak utworzyć własny hostowanego serwera SignalR 2 oraz sposób nawiązania połączenia z klientem JavaScript. Używane w samouczku V wersje oprogramowania..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 997756ff8d48e41da981491d6154f3107ec7a051
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="2f2a5-104">Samouczek: SignalR Host samodzielny</span><span class="sxs-lookup"><span data-stu-id="2f2a5-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="2f2a5-105">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="2f2a5-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="2f2a5-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="2f2a5-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="2f2a5-107">Ten samouczek pokazuje, jak utworzyć własny hostowanego serwera SignalR 2 oraz sposób nawiązania połączenia z klientem JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2f2a5-108">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="2f2a5-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="2f2a5-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2f2a5-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="2f2a5-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="2f2a5-110">.NET 4.5</span></span>
> - <span data-ttu-id="2f2a5-111">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="2f2a5-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="2f2a5-112">Z tego samouczka przy użyciu programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2f2a5-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="2f2a5-113">Aby używać programu Visual Studio 2012 z tego samouczka, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="2f2a5-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="2f2a5-114">Aktualizacja Twojego [Menedżera pakietów](http://docs.nuget.org/docs/start-here/installing-nuget) do najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="2f2a5-115">Zainstaluj [sieci Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f2a5-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="2f2a5-116">Instalator platformy sieci Web, wyszukiwanie i instalowanie **ASP.NET i 2013.1 narzędzia sieci Web dla programu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="2f2a5-117">Spowoduje to zainstalowanie szablony programu Visual Studio dla biblioteki SignalR klas takich jak **Centrum**.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="2f2a5-118">Niektóre szablony (takich jak **klasy początkowej OWIN**) nie są dostępne; w tym przypadku powinien użyć pliku klasy.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="2f2a5-119">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="2f2a5-119">Questions and comments</span></span>
> 
> <span data-ttu-id="2f2a5-120">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="2f2a5-121">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="2f2a5-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="2f2a5-122">Omówienie</span><span class="sxs-lookup"><span data-stu-id="2f2a5-122">Overview</span></span>

<span data-ttu-id="2f2a5-123">Serwer SignalR zazwyczaj znajduje się w aplikacji ASP.NET w usługach IIS, ale może również być hostowanie Samoobsługowe (na przykład w aplikacji konsoli lub usługi systemu Windows) za pomocą biblioteki hosta samodzielnego.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="2f2a5-124">Ta biblioteka, podobnie jak wszystkie SignalR 2, w oparciu OWIN ([interfejsu Open Web dla platformy .NET](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="2f2a5-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="2f2a5-125">OWIN definiuje abstrakcję między serwerami sieci web .NET i aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="2f2a5-126">OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN idealne rozwiązanie w przypadku samodzielnej obsługi aplikacji sieci web w własnego procesu poza usług IIS.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="2f2a5-127">Nie zawiera w usługach IIS przyczyny:</span><span class="sxs-lookup"><span data-stu-id="2f2a5-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="2f2a5-128">Środowiska, w którym usługi IIS nie jest dostępny lub pożądane, takich jak istniejącej farmy serwerów bez usług IIS.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="2f2a5-129">Zmniejszenie wydajności programu IIS trzeba unikać.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="2f2a5-130">Funkcja SignalR jest mają zostać dodane do exising aplikację, która działa w usługi systemu Windows, rola procesu roboczego platformy Azure lub inny proces.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="2f2a5-131">Jeśli rozwiązanie jest tworzone jako hosta samodzielnego ze względu na wydajność, zaleca się również testu z aplikacją hostowanych w usługach IIS w celu określenia poprawiać wydajność.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="2f2a5-132">Ten samouczek zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="2f2a5-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="2f2a5-133">Tworzenie serwera</span><span class="sxs-lookup"><span data-stu-id="2f2a5-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="2f2a5-134">Dostęp do serwera z klientem JavaScript</span><span class="sxs-lookup"><span data-stu-id="2f2a5-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="2f2a5-135">Tworzenie serwera</span><span class="sxs-lookup"><span data-stu-id="2f2a5-135">Creating the server</span></span>

<span data-ttu-id="2f2a5-136">W tym samouczku utworzysz serwer, który znajduje się w aplikacji konsoli, ale serwer może być obsługiwana przez dowolny rodzaj procesu, taką jak usługa systemu Windows lub roli procesu roboczego platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="2f2a5-137">Przykładowy kod do obsługi serwera SignalR, w usłudze systemu Windows, temacie [Self-Hosting SignalR w usłudze Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="2f2a5-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="2f2a5-138">Otwórz program Visual Studio 2013 z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="2f2a5-139">Wybierz **pliku**, **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="2f2a5-140">Wybierz **Windows** w obszarze **Visual C#** w węźle **szablony** okienka, a następnie wybierz **aplikacji konsoli** szablonu.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="2f2a5-141">Nazwa nowego projektu "SignalRSelfHost", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="2f2a5-142">Otwórz konsolę Menedżera pakietów biblioteki wybierając **narzędzia**, **Menedżer pakietów biblioteki**, **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="2f2a5-143">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2f2a5-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="2f2a5-144">To polecenie dodaje biblioteki SignalR 2 Self-Host do projektu.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="2f2a5-145">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2f2a5-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="2f2a5-146">To polecenie dodaje biblioteki Microsoft.Owin.Cors do projektu.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="2f2a5-147">Ta biblioteka będzie służyć do obsługi między domenami, które jest wymagane dla aplikacji, które hosta SignalR i klient strony sieci web w różnych domenach.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="2f2a5-148">Ponieważ będzie można obsługiwać SignalR serwera i klienta sieci web na inne porty, oznacza to, że tej domeny — musi być włączona dla komunikacji między tymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="2f2a5-149">Zastąp zawartość pliku Program.cs następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="2f2a5-150">Powyższy kod obejmuje trzy klasy:</span><span class="sxs-lookup"><span data-stu-id="2f2a5-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="2f2a5-151">**Program**, takie jak **Main** metody Definiowanie ścieżki podstawowej wykonywania.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="2f2a5-152">W przypadku tej metody, aplikacji sieci web typu **uruchamiania** została uruchomiona z określonym adresem URL (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="2f2a5-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="2f2a5-153">Jeśli zabezpieczeń jest wymagane dla punktu końcowego, można stosować protokół SSL.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="2f2a5-154">Zobacz [porady: Konfigurowanie portu z certyfikatem SSL](https://msdn.microsoft.com/en-us/library/ms733791.aspx) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/en-us/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="2f2a5-155">**Uruchamianie**, klasa zawierająca konfiguracje serwera SignalR (tylko konfiguracja, w tym samouczku używana jest wywołanie `UseCors`) i wywołania w celu `MapSignalR`, co powoduje trasy dla obiektów Centrum w projekcie.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="2f2a5-156">**MyHub**, klasy koncentratora SignalR udostępniającą aplikacji do klientów.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="2f2a5-157">Ta klasa zawiera tylko jedną metodę **wysyłania**, że klienci wywoła wysyłać wiadomości do wszystkich innych połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="2f2a5-158">Kompilowanie i uruchamianie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-158">Compile and run the application.</span></span> <span data-ttu-id="2f2a5-159">Adres, który jest uruchomiony serwer powinien być wyświetlony w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="2f2a5-160">Jeśli wykonanie zakończy się niepowodzeniem z wyjątkiem `System.Reflection.TargetInvocationException was unhandled`, konieczne będzie ponowne uruchomienie programu Visual Studio z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="2f2a5-161">Zatrzymaj aplikację przed kontynuowaniem do następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="2f2a5-162">Dostęp do serwera z klientem JavaScript</span><span class="sxs-lookup"><span data-stu-id="2f2a5-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="2f2a5-163">W tej sekcji użyjesz tego samego klienta JavaScript z [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="2f2a5-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="2f2a5-164">Tylko wybierzemy modyfikację jednego klienta, który jest jawnie określenie adresu URL koncentratora.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="2f2a5-165">Z własnym hostowanej aplikacji serwer może nie koniecznie na ten sam adres URL połączenia (ze względu na odwrotne serwery proxy i moduły równoważenia obciążenia), więc adres URL musi być jawnie zdefiniowane w.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="2f2a5-166">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy w ramach rozwiązania i wybierz **Dodaj**, **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="2f2a5-167">Wybierz **Web** , a następnie wybierz węzeł **aplikacji sieci Web ASP.NET** szablonu.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="2f2a5-168">Nazwij projekt "JavascriptClient", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="2f2a5-169">Wybierz **pusty** szablonu i pozostaw niezaznaczone pozostałe opcje.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="2f2a5-170">Wybierz **Utwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="2f2a5-171">W konsoli Menedżera pakietów, wybierz projekt "JavascriptClient" **domyślny projekt** listy rozwijanej, a następnie wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2f2a5-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="2f2a5-172">To polecenie powoduje zainstalowanie biblioteki SignalR i JQuery, które będą potrzebne w kliencie.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="2f2a5-173">Kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="2f2a5-174">Wybierz **Web** węzeł, a następnie wybierz stronę HTML.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="2f2a5-175">Nazwa strony **Default.html**.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="2f2a5-176">Zastąp zawartość strony HTML z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="2f2a5-177">Sprawdź, czy odwołań do skryptów tutaj odpowiadają skryptów w folderze skryptów projektu.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="2f2a5-178">Poniższy kod (wyróżniony w powyższym przykładowym kodzie) jest wprowadzonych do klienta używany w samouczku pobierania Stared (oprócz uaktualniania kodu do wersji beta programu SignalR w wersji 2).</span><span class="sxs-lookup"><span data-stu-id="2f2a5-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="2f2a5-179">Ten wiersz kodu jawnie ustawia adres URL połączenia podstawowej dla biblioteki SignalR na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="2f2a5-180">Kliknij prawym przyciskiem myszy w ramach rozwiązania, a następnie wybierz **Ustaw projekty startowe...** . Wybierz **wiele projektów startowych** przycisk radiowy i ustaw oba projekty **akcji** do **Start**.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="2f2a5-181">Kliknij prawym przyciskiem "Default.html" i wybierz **Ustaw jako stronę startową**.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="2f2a5-182">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-182">Run the application.</span></span> <span data-ttu-id="2f2a5-183">Spowoduje uruchomienie serwera i strony.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-183">The server and page will launch.</span></span> <span data-ttu-id="2f2a5-184">Może być konieczne ponowne załadowanie tej strony sieci web (lub wybierz **Kontynuuj** w debugerze) Jeśli strony ładuje zanim serwer jest uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="2f2a5-185">W przeglądarce należy podać nazwę użytkownika, po wyświetleniu monitu.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="2f2a5-186">Skopiuj adres URL strony do innej karty przeglądarki lub okna i podaj inną nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="2f2a5-187">Można wysyłać z okienka w przeglądarce jednego do drugiego, tak jak w samouczku wprowadzenie.</span><span class="sxs-lookup"><span data-stu-id="2f2a5-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
