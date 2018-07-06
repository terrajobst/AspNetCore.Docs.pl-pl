---
uid: signalr/overview/performance/scaleout-with-sql-server
title: SignalR — skalowanie w poziomie z programem SQL Server | Dokumentacja firmy Microsoft
author: MikeWasson
description: Wersje oprogramowania, używaną w tym temacie dodatku .NET 4.5 SignalR dla programu Visual Studio 2013 w wersji 2, poprzednie wersje w tym temacie informacji o wcześniejszych wersjach...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: fb21bee1737c5783d47abd2642af2c613b5d087b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810214"
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="5e72c-103">SignalR — skalowanie w poziomie z programem SQL Server</span><span class="sxs-lookup"><span data-stu-id="5e72c-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="5e72c-104">przez [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5e72c-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5e72c-105">Wersje oprogramowania używaną w tym temacie</span><span class="sxs-lookup"><span data-stu-id="5e72c-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="5e72c-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5e72c-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="5e72c-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5e72c-107">.NET 4.5</span></span>
> - <span data-ttu-id="5e72c-108">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="5e72c-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5e72c-109">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="5e72c-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="5e72c-110">Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5e72c-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="5e72c-111">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="5e72c-111">Questions and comments</span></span>
> 
> <span data-ttu-id="5e72c-112">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="5e72c-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5e72c-113">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5e72c-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="5e72c-114">W tym samouczku użyjesz programu SQL Server, aby dystrybuować komunikaty do aplikacji SignalR, które zostało wdrożone w dwóch osobnych wystąpień usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="5e72c-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="5e72c-115">W tym samouczku można również uruchomić na maszynie jeden test, ale aby uzyskać pełny wpływ, należy wdrożyć aplikacji SignalR do co najmniej dwóch serwerów.</span><span class="sxs-lookup"><span data-stu-id="5e72c-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="5e72c-116">SQL Server należy również zainstalować na jednym serwerze lub na oddzielnym serwerze dedykowanym.</span><span class="sxs-lookup"><span data-stu-id="5e72c-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="5e72c-117">Innym rozwiązaniem jest uruchamianie samouczek przy użyciu maszyn wirtualnych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="5e72c-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="5e72c-118">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5e72c-118">Prerequisites</span></span>

<span data-ttu-id="5e72c-119">Microsoft SQL Server 2005 lub nowszego.</span><span class="sxs-lookup"><span data-stu-id="5e72c-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="5e72c-120">Systemu backplane obsługuje wersje komputerach stacjonarnych, jak i serwera programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5e72c-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="5e72c-121">Nie obsługuje programu SQL Server Compact Edition lub Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5e72c-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="5e72c-122">(Jeśli aplikacja jest hostowana na platformie Azure, należy wziąć pod uwagę płyty montażowej usługi Service Bus zamiast.)</span><span class="sxs-lookup"><span data-stu-id="5e72c-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="5e72c-123">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5e72c-123">Overview</span></span>

<span data-ttu-id="5e72c-124">Przed przejściem do szczegółowe podręcznika, poniżej przedstawiono krótkie podsumowanie będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="5e72c-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="5e72c-125">Tworzenie nowej pustej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5e72c-125">Create a new empty database.</span></span> <span data-ttu-id="5e72c-126">Systemu backplane utworzy niezbędne tabele w tej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5e72c-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="5e72c-127">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="5e72c-127">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="5e72c-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="5e72c-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="5e72c-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="5e72c-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="5e72c-130">Tworzenie aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="5e72c-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="5e72c-131">Dodaj następujący kod do pliku Startup.cs w celu skonfigurowania systemu backplane:</span><span class="sxs-lookup"><span data-stu-id="5e72c-131">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="5e72c-132">Ten kod konfiguruje systemu backplane z wartościami domyślnymi dla [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) i [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="5e72c-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="5e72c-133">Aby uzyskać informacje na temat zmiany tych wartości, zobacz [wydajność SignalR: metryki skalowania](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="5e72c-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span> 

## <a name="configure-the-database"></a><span data-ttu-id="5e72c-134">Skonfiguruj bazę danych</span><span class="sxs-lookup"><span data-stu-id="5e72c-134">Configure the Database</span></span>

<span data-ttu-id="5e72c-135">Zdecyduj, czy aplikacja użyje uwierzytelniania Windows lub uwierzytelniania programu SQL Server dostęp do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5e72c-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="5e72c-136">Niezależnie od tego upewnij się, że użytkownik bazy danych ma uprawnienia do zalogować, Utwórz schematy i tworzenie tabel.</span><span class="sxs-lookup"><span data-stu-id="5e72c-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="5e72c-137">Utwórz nową bazę danych do płyty montażowej do użycia.</span><span class="sxs-lookup"><span data-stu-id="5e72c-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="5e72c-138">Bazy danych można nadać dowolną nazwę.</span><span class="sxs-lookup"><span data-stu-id="5e72c-138">You can give the database any name.</span></span> <span data-ttu-id="5e72c-139">Nie trzeba tworzyć żadnych tabel w bazie danych. Spowoduje to utworzenie niezbędne tabele systemu backplane.</span><span class="sxs-lookup"><span data-stu-id="5e72c-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="5e72c-140">Włącz brokera usług</span><span class="sxs-lookup"><span data-stu-id="5e72c-140">Enable Service Broker</span></span>

<span data-ttu-id="5e72c-141">Zalecane jest, aby włączyć brokera usługi dla bazy danych systemu backplane.</span><span class="sxs-lookup"><span data-stu-id="5e72c-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="5e72c-142">Usługa Service Broker zapewnia macierzystą obsługę komunikatów i kolejkowania w SQL Server, która umożliwia bardziej wydajnie otrzymywać aktualizacje systemu backplane.</span><span class="sxs-lookup"><span data-stu-id="5e72c-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="5e72c-143">(Jednak systemu backplane również działa bez programu Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="5e72c-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="5e72c-144">Aby sprawdzić, czy programu Service Broker jest włączona, należy zbadać **jest\_brokera\_włączone** kolumny w **sys.databases** widok katalogu.</span><span class="sxs-lookup"><span data-stu-id="5e72c-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="5e72c-145">Aby włączyć programu Service Broker, użyj następującego zapytania SQL:</span><span class="sxs-lookup"><span data-stu-id="5e72c-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="5e72c-146">Jeśli wydaje się zakleszczenie, upewnij się, to zapytanie nie istnieją żadne aplikacje połączone z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="5e72c-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="5e72c-147">Po włączeniu śledzenia śledzenia również pokaże czy programu Service Broker jest włączona.</span><span class="sxs-lookup"><span data-stu-id="5e72c-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="5e72c-148">Tworzenie aplikacji SignalR</span><span class="sxs-lookup"><span data-stu-id="5e72c-148">Create a SignalR Application</span></span>

<span data-ttu-id="5e72c-149">Tworzenie aplikacji SignalR, wykonując dowolną z następujących samouczków:</span><span class="sxs-lookup"><span data-stu-id="5e72c-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="5e72c-150">Wprowadzenie do SignalR w wersji 2.0</span><span class="sxs-lookup"><span data-stu-id="5e72c-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="5e72c-151">Wprowadzenie do SignalR w wersji 2.0 i MVC 5</span><span class="sxs-lookup"><span data-stu-id="5e72c-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="5e72c-152">Następnie zmodyfikujemy aplikacji rozmów w celu obsługi skalowania w poziomie z programem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5e72c-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="5e72c-153">Najpierw Dodaj pakiet SignalR.SqlServer NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="5e72c-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="5e72c-154">W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="5e72c-154">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="5e72c-155">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5e72c-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="5e72c-156">Następnie otwórz plik Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="5e72c-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="5e72c-157">Dodaj następujący kod do **Konfiguruj** metody:</span><span class="sxs-lookup"><span data-stu-id="5e72c-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="5e72c-158">Wdrażanie i uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5e72c-158">Deploy and Run the Application</span></span>

<span data-ttu-id="5e72c-159">Przygotowanie do wdrożenia aplikacji SignalR wystąpień systemu Windows Server.</span><span class="sxs-lookup"><span data-stu-id="5e72c-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="5e72c-160">Dodaj rolę usług IIS.</span><span class="sxs-lookup"><span data-stu-id="5e72c-160">Add the IIS role.</span></span> <span data-ttu-id="5e72c-161">Obejmują funkcje "Opracowywanie zawartości dla aplikacji", w tym protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5e72c-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="5e72c-162">Również obejmować usługę zarządzania (wymienione w obszarze "Narzędzia do zarządzania").</span><span class="sxs-lookup"><span data-stu-id="5e72c-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="5e72c-163">**Zainstaluj narzędzie Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="5e72c-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="5e72c-164">Podczas uruchamiania Menedżera usług IIS, zostanie wyświetlony monit o zainstalowanie platformy sieci Web firmy Microsoft lub możesz [Pobierz intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="5e72c-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="5e72c-165">W Instalatorze platformy wyszukiwania dla narzędzia Web Deploy i zainstalować Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="5e72c-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="5e72c-166">Sprawdź, czy usługa zarządzania sieci Web jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="5e72c-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="5e72c-167">Jeśli nie, uruchom usługę.</span><span class="sxs-lookup"><span data-stu-id="5e72c-167">If not, start the service.</span></span> <span data-ttu-id="5e72c-168">(Jeśli nie widzisz usługi zarządzania siecią Web, na liście usług Windows, upewnij się, że po dodaniu roli usług IIS są zainstalowane usługi zarządzania.)</span><span class="sxs-lookup"><span data-stu-id="5e72c-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="5e72c-169">Wreszcie Otwórz port 8172 dla protokołu TCP.</span><span class="sxs-lookup"><span data-stu-id="5e72c-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="5e72c-170">Jest to port, który korzysta z narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="5e72c-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="5e72c-171">Teraz wszystko jest gotowe do wdrożenia projektu programu Visual Studio z komputera deweloperskiego z serwerem.</span><span class="sxs-lookup"><span data-stu-id="5e72c-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="5e72c-172">W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="5e72c-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="5e72c-173">Aby uzyskać bardziej szczegółową dokumentację na temat wdrażania w Internecie, zobacz [zawartości mapy wdrażania w sieci Web dla programu Visual Studio i platformy ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="5e72c-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="5e72c-174">W przypadku wdrożenia aplikacji na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobacz ich odbierania komunikatów SignalR w innej.</span><span class="sxs-lookup"><span data-stu-id="5e72c-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="5e72c-175">(Oczywiście w środowisku produkcyjnym, dwa serwery będą znajdują się za modułem równoważenia obciążenia.)</span><span class="sxs-lookup"><span data-stu-id="5e72c-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="5e72c-176">Po uruchomieniu aplikacji, możesz zobaczyć, że SignalR automatycznie utworzył tabele w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="5e72c-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="5e72c-177">SignalR zarządza tabelami.</span><span class="sxs-lookup"><span data-stu-id="5e72c-177">SignalR manages the tables.</span></span> <span data-ttu-id="5e72c-178">Tak długo, jak Twoja aplikacja zostanie wdrożona, nie usunąć wiersze, zmodyfikować tabeli i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="5e72c-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
