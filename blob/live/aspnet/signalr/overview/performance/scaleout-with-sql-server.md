---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Skalowania SignalR z programem SQL Server | Dokumentacja firmy Microsoft
author: MikeWasson
description: "Wersje oprogramowania używane w tym temacie Visual Studio 2013 .NET 4.5 SignalR w wersji 2 poprzednie wersje tego tematu informacji o wcześniejszych wersji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 5bf625a1ef8cc8ceab0014fadfab0c8a23dbc8da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="94493-103">Skalowania SignalR z programem SQL Server</span><span class="sxs-lookup"><span data-stu-id="94493-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="94493-104">przez [Wasson Jan](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="94493-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="94493-105">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="94493-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="94493-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="94493-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="94493-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="94493-107">.NET 4.5</span></span>
> - <span data-ttu-id="94493-108">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="94493-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="94493-109">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="94493-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="94493-110">Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="94493-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="94493-111">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="94493-111">Questions and comments</span></span>
> 
> <span data-ttu-id="94493-112">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="94493-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="94493-113">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="94493-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="94493-114">W tym samouczku użyjesz programu SQL Server do dystrybucji wiadomości w aplikacji SignalR, które zostało wdrożone w dwóch osobnych wystąpień usług IIS.</span><span class="sxs-lookup"><span data-stu-id="94493-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="94493-115">W tym samouczku można również uruchomić na maszynie pojedynczy test, ale uzyskanie pełnego efektu, należy wdrożyć co najmniej dwa serwery aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="94493-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="94493-116">SQL Server należy również zainstalować na jednym serwerze lub na osobnym serwerze dedykowanym.</span><span class="sxs-lookup"><span data-stu-id="94493-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="94493-117">Innym rozwiązaniem jest Uruchamianie samouczka przy użyciu maszyn wirtualnych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="94493-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="94493-118">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="94493-118">Prerequisites</span></span>

<span data-ttu-id="94493-119">Microsoft SQL Server 2005 lub nowszego.</span><span class="sxs-lookup"><span data-stu-id="94493-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="94493-120">Systemu backplane obsługuje zarówno stacjonarnych i serwerów wersje programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="94493-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="94493-121">Nie obsługuje programu SQL Server Compact Edition lub baza danych SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="94493-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="94493-122">(Jeśli aplikacja jest hostowana na platformie Azure, należy wziąć pod uwagę płyty montażowej usługi Service Bus zamiast.)</span><span class="sxs-lookup"><span data-stu-id="94493-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="94493-123">Omówienie</span><span class="sxs-lookup"><span data-stu-id="94493-123">Overview</span></span>

<span data-ttu-id="94493-124">Przed uzyskujemy do szczegółowy samouczek, w tym miejscu jest to szybki przegląd będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="94493-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="94493-125">Tworzenie nowej pustej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="94493-125">Create a new empty database.</span></span> <span data-ttu-id="94493-126">Systemu backplane utworzy potrzebne tabele w tej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="94493-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="94493-127">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="94493-127">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="94493-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="94493-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="94493-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="94493-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="94493-130">Tworzenie aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="94493-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="94493-131">Dodaj następujący kod do pliku Startup.cs do skonfigurowania systemu backplane:</span><span class="sxs-lookup"><span data-stu-id="94493-131">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

 <span data-ttu-id="94493-132">Ten kod konfiguruje systemu backplane z wartościami domyślnymi dla [TableCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) i [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="94493-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="94493-133">Aby uzyskać informacje na temat zmiany tych wartości, zobacz [wydajności SignalR: metryki skalowania](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="94493-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span> 

## <a name="configure-the-database"></a><span data-ttu-id="94493-134">Skonfiguruj bazę danych</span><span class="sxs-lookup"><span data-stu-id="94493-134">Configure the Database</span></span>

<span data-ttu-id="94493-135">Zdecyduj, czy aplikacja będzie używać uwierzytelniania systemu Windows lub uwierzytelniania programu SQL Server dostęp do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="94493-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="94493-136">Niezależnie od tego upewnij się, że użytkownik bazy danych ma uprawnienia do logowania, Utwórz schematy i tworzenie tabel.</span><span class="sxs-lookup"><span data-stu-id="94493-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="94493-137">Utwórz nową bazę danych do płyty montażowej do użycia.</span><span class="sxs-lookup"><span data-stu-id="94493-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="94493-138">Bazy danych można nadać dowolną nazwę.</span><span class="sxs-lookup"><span data-stu-id="94493-138">You can give the database any name.</span></span> <span data-ttu-id="94493-139">Nie trzeba tworzyć żadnych tabel w bazie danych. systemu backplane utworzy potrzebne tabele.</span><span class="sxs-lookup"><span data-stu-id="94493-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="94493-140">Włącz brokera usług</span><span class="sxs-lookup"><span data-stu-id="94493-140">Enable Service Broker</span></span>

<span data-ttu-id="94493-141">Zalecane jest, aby włączyć brokera usługi dla bazy danych systemu backplane.</span><span class="sxs-lookup"><span data-stu-id="94493-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="94493-142">Usługa Service Broker zapewnia macierzystą obsługę dla obsługi wiadomości i kolejkowania w programie SQL Server, co pozwala montażowa wydajniej otrzymywać aktualizacje.</span><span class="sxs-lookup"><span data-stu-id="94493-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="94493-143">(Jednak systemu backplane również działa bez programu Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="94493-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="94493-144">Aby sprawdzić, czy Usługa Service Broker jest włączona, zapytanie **jest\_brokera\_włączone** kolumny w **sys.databases** widoku katalogu.</span><span class="sxs-lookup"><span data-stu-id="94493-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="94493-145">Aby włączyć brokera usługi, użyj następującej kwerendy SQL:</span><span class="sxs-lookup"><span data-stu-id="94493-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="94493-146">Jeśli to zapytanie wydaje się do zakleszczenia, upewnij się, że nie ma żadnych aplikacji, połączony z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="94493-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="94493-147">Jeśli śledzenie jest włączone, dane śledzenia zostaną wyświetlone również czy Service Broker jest włączona.</span><span class="sxs-lookup"><span data-stu-id="94493-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="94493-148">Tworzenie aplikacji SignalR</span><span class="sxs-lookup"><span data-stu-id="94493-148">Create a SignalR Application</span></span>

<span data-ttu-id="94493-149">Utwórz aplikację SignalR, wykonując jedną z tych samouczki:</span><span class="sxs-lookup"><span data-stu-id="94493-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="94493-150">Wprowadzenie do korzystania z SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="94493-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="94493-151">Wprowadzenie do korzystania z SignalR 2.0 i MVC 5</span><span class="sxs-lookup"><span data-stu-id="94493-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="94493-152">Firma Microsoft będzie następnie zmodyfikować rozmów aplikację do obsługi skalowania w poziomie z programem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="94493-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="94493-153">Najpierw Dodaj pakiet SignalR.SqlServer NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="94493-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="94493-154">W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="94493-154">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="94493-155">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="94493-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="94493-156">Następnie otwórz folder pliku Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="94493-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="94493-157">Dodaj następujący kod do **Konfiguruj** metody:</span><span class="sxs-lookup"><span data-stu-id="94493-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="94493-158">Wdrażanie i uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="94493-158">Deploy and Run the Application</span></span>

<span data-ttu-id="94493-159">Przygotuj swoich wystąpień systemu Windows Server, aby wdrożyć aplikację SignalR.</span><span class="sxs-lookup"><span data-stu-id="94493-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="94493-160">Dodaj rolę usług IIS.</span><span class="sxs-lookup"><span data-stu-id="94493-160">Add the IIS role.</span></span> <span data-ttu-id="94493-161">Obejmują funkcje "Programowanie aplikacji", takie jak protokół WebSocket.</span><span class="sxs-lookup"><span data-stu-id="94493-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="94493-162">Obejmuje to też usługi zarządzania (wymienione w obszarze "Narzędzia do zarządzania").</span><span class="sxs-lookup"><span data-stu-id="94493-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="94493-163">**Zainstaluj narzędzie Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="94493-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="94493-164">Uruchom Menedżera usług IIS, zostanie wyświetlony monit do zainstalowania platformy Microsoft Web lub można [Pobierz intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="94493-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="94493-165">W Instalatorze platformy wyszukiwanie narzędzia Web Deploy i instalowanie usługi sieci Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="94493-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="94493-166">Sprawdź, czy jest uruchomiona usługa zarządzania siecią Web.</span><span class="sxs-lookup"><span data-stu-id="94493-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="94493-167">Jeśli nie, należy uruchomić usługę.</span><span class="sxs-lookup"><span data-stu-id="94493-167">If not, start the service.</span></span> <span data-ttu-id="94493-168">(Usługa zarządzania siecią Web nie jest widoczny na liście usług systemu Windows, aby się, że po dodaniu roli usług IIS są zainstalowane usługi zarządzania.)</span><span class="sxs-lookup"><span data-stu-id="94493-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="94493-169">Na koniec Otwórz port 8172 dla protokołu TCP.</span><span class="sxs-lookup"><span data-stu-id="94493-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="94493-170">Jest to port, który używa narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="94493-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="94493-171">Teraz można przystąpić do wdrażania projektu programu Visual Studio z komputerze deweloperskim z serwerem.</span><span class="sxs-lookup"><span data-stu-id="94493-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="94493-172">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="94493-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="94493-173">Aby uzyskać bardziej szczegółowe dokumentacji dotyczące wdrożenia sieci web, zobacz [Mapa zawartości wdrożenia sieci Web dla platformy ASP.NET i Visual Studio](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="94493-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="94493-174">W przypadku wdrożenia aplikacji na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobacz, każdy z nich odbierać wiadomości SignalR z innych.</span><span class="sxs-lookup"><span data-stu-id="94493-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="94493-175">(Oczywiście w środowisku produkcyjnym, dwa serwery będą sit za modułem równoważenia obciążenia.)</span><span class="sxs-lookup"><span data-stu-id="94493-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="94493-176">Po uruchomieniu aplikacji widać, że SignalR automatycznie utworzył tabele w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="94493-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="94493-177">SignalR zarządza tabel.</span><span class="sxs-lookup"><span data-stu-id="94493-177">SignalR manages the tables.</span></span> <span data-ttu-id="94493-178">Tak długo, jak aplikacja jest wdrażana, nie usuwać wiersze, zmodyfikuj tabelę i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="94493-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
