---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: SignalR — skalowanie w poziomie z programem SQL Server (SignalR 1.x) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: d68196aa8e4d0c60570a0e36ccc75a2a080d57bc
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912751"
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="98114-102">SignalR — skalowanie w poziomie z programem SQL Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="98114-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="98114-103">przez [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="98114-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="98114-104">W tym samouczku użyjesz programu SQL Server, aby dystrybuować komunikaty do aplikacji SignalR, które zostało wdrożone w dwóch osobnych wystąpień usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="98114-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="98114-105">W tym samouczku można również uruchomić na maszynie jeden test, ale aby uzyskać pełny wpływ, należy wdrożyć aplikacji SignalR do co najmniej dwóch serwerów.</span><span class="sxs-lookup"><span data-stu-id="98114-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="98114-106">SQL Server należy również zainstalować na jednym serwerze lub na oddzielnym serwerze dedykowanym.</span><span class="sxs-lookup"><span data-stu-id="98114-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="98114-107">Innym rozwiązaniem jest uruchamianie samouczek przy użyciu maszyn wirtualnych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="98114-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="98114-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="98114-108">Prerequisites</span></span>

<span data-ttu-id="98114-109">Microsoft SQL Server 2005 lub nowszego.</span><span class="sxs-lookup"><span data-stu-id="98114-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="98114-110">Systemu backplane obsługuje wersje komputerach stacjonarnych, jak i serwera programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="98114-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="98114-111">Nie obsługuje programu SQL Server Compact Edition lub Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="98114-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="98114-112">(Jeśli aplikacja jest hostowana na platformie Azure, należy wziąć pod uwagę płyty montażowej usługi Service Bus zamiast.)</span><span class="sxs-lookup"><span data-stu-id="98114-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="98114-113">Omówienie</span><span class="sxs-lookup"><span data-stu-id="98114-113">Overview</span></span>

<span data-ttu-id="98114-114">Przed przejściem do szczegółowe podręcznika, poniżej przedstawiono krótkie podsumowanie będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="98114-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="98114-115">Tworzenie nowej pustej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="98114-115">Create a new empty database.</span></span> <span data-ttu-id="98114-116">Systemu backplane utworzy niezbędne tabele w tej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="98114-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="98114-117">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="98114-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="98114-118">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="98114-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="98114-119">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="98114-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="98114-120">Tworzenie aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="98114-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="98114-121">Dodaj następujący kod do pliku Global.asax w celu skonfigurowania systemu backplane:</span><span class="sxs-lookup"><span data-stu-id="98114-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="98114-122">Skonfiguruj bazę danych</span><span class="sxs-lookup"><span data-stu-id="98114-122">Configure the Database</span></span>

<span data-ttu-id="98114-123">Zdecyduj, czy aplikacja użyje uwierzytelniania Windows lub uwierzytelniania programu SQL Server dostęp do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="98114-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="98114-124">Niezależnie od tego upewnij się, że użytkownik bazy danych ma uprawnienia do zalogować, Utwórz schematy i tworzenie tabel.</span><span class="sxs-lookup"><span data-stu-id="98114-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="98114-125">Utwórz nową bazę danych do płyty montażowej do użycia.</span><span class="sxs-lookup"><span data-stu-id="98114-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="98114-126">Bazy danych można nadać dowolną nazwę.</span><span class="sxs-lookup"><span data-stu-id="98114-126">You can give the database any name.</span></span> <span data-ttu-id="98114-127">Nie trzeba tworzyć żadnych tabel w bazie danych. Spowoduje to utworzenie niezbędne tabele systemu backplane.</span><span class="sxs-lookup"><span data-stu-id="98114-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="98114-128">Włącz brokera usług</span><span class="sxs-lookup"><span data-stu-id="98114-128">Enable Service Broker</span></span>

<span data-ttu-id="98114-129">Zalecane jest, aby włączyć brokera usługi dla bazy danych systemu backplane.</span><span class="sxs-lookup"><span data-stu-id="98114-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="98114-130">Usługa Service Broker zapewnia macierzystą obsługę komunikatów i kolejkowania w SQL Server, która umożliwia bardziej wydajnie otrzymywać aktualizacje systemu backplane.</span><span class="sxs-lookup"><span data-stu-id="98114-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="98114-131">(Jednak systemu backplane również działa bez programu Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="98114-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="98114-132">Aby sprawdzić, czy programu Service Broker jest włączona, należy zbadać **jest\_brokera\_włączone** kolumny w **sys.databases** widok katalogu.</span><span class="sxs-lookup"><span data-stu-id="98114-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="98114-133">Aby włączyć programu Service Broker, użyj następującego zapytania SQL:</span><span class="sxs-lookup"><span data-stu-id="98114-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="98114-134">Jeśli wydaje się zakleszczenie, upewnij się, to zapytanie nie istnieją żadne aplikacje połączone z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="98114-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="98114-135">Po włączeniu śledzenia śledzenia również pokaże czy programu Service Broker jest włączona.</span><span class="sxs-lookup"><span data-stu-id="98114-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="98114-136">Tworzenie aplikacji SignalR</span><span class="sxs-lookup"><span data-stu-id="98114-136">Create a SignalR Application</span></span>

<span data-ttu-id="98114-137">Tworzenie aplikacji SignalR, wykonując dowolną z następujących samouczków:</span><span class="sxs-lookup"><span data-stu-id="98114-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="98114-138">Wprowadzenie do SignalR</span><span class="sxs-lookup"><span data-stu-id="98114-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="98114-139">Wprowadzenie do SignalR i MVC 4</span><span class="sxs-lookup"><span data-stu-id="98114-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="98114-140">Następnie zmodyfikujemy aplikacji rozmów w celu obsługi skalowania w poziomie z programem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="98114-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="98114-141">Najpierw Dodaj pakiet SignalR.SqlServer NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="98114-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="98114-142">W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="98114-142">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="98114-143">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="98114-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="98114-144">Następnie otwórz plik Global.asax.</span><span class="sxs-lookup"><span data-stu-id="98114-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="98114-145">Dodaj następujący kod do **aplikacji\_Start** metody:</span><span class="sxs-lookup"><span data-stu-id="98114-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="98114-146">Wdrażanie i uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="98114-146">Deploy and Run the Application</span></span>

<span data-ttu-id="98114-147">Przygotowanie do wdrożenia aplikacji SignalR wystąpień systemu Windows Server.</span><span class="sxs-lookup"><span data-stu-id="98114-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="98114-148">Dodaj rolę usług IIS.</span><span class="sxs-lookup"><span data-stu-id="98114-148">Add the IIS role.</span></span> <span data-ttu-id="98114-149">Obejmują funkcje "Opracowywanie zawartości dla aplikacji", w tym protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="98114-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="98114-150">Również obejmować usługę zarządzania (wymienione w obszarze "Narzędzia do zarządzania").</span><span class="sxs-lookup"><span data-stu-id="98114-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="98114-151">**Zainstaluj narzędzie Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="98114-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="98114-152">Podczas uruchamiania Menedżera usług IIS, zostanie wyświetlony monit o zainstalowanie platformy sieci Web firmy Microsoft lub możesz [Pobierz intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="98114-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="98114-153">W Instalatorze platformy wyszukiwania dla narzędzia Web Deploy i zainstalować Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="98114-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="98114-154">Sprawdź, czy usługa zarządzania sieci Web jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="98114-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="98114-155">Jeśli nie, uruchom usługę.</span><span class="sxs-lookup"><span data-stu-id="98114-155">If not, start the service.</span></span> <span data-ttu-id="98114-156">(Jeśli nie widzisz usługi zarządzania siecią Web, na liście usług Windows, upewnij się, że po dodaniu roli usług IIS są zainstalowane usługi zarządzania.)</span><span class="sxs-lookup"><span data-stu-id="98114-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="98114-157">Wreszcie Otwórz port 8172 dla protokołu TCP.</span><span class="sxs-lookup"><span data-stu-id="98114-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="98114-158">Jest to port, który korzysta z narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="98114-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="98114-159">Teraz wszystko jest gotowe do wdrożenia projektu programu Visual Studio z komputera deweloperskiego z serwerem.</span><span class="sxs-lookup"><span data-stu-id="98114-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="98114-160">W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="98114-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="98114-161">Aby uzyskać bardziej szczegółową dokumentację na temat wdrażania w Internecie, zobacz [zawartości mapy wdrażania w sieci Web dla programu Visual Studio i platformy ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="98114-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="98114-162">W przypadku wdrożenia aplikacji na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobacz ich odbierania komunikatów SignalR w innej.</span><span class="sxs-lookup"><span data-stu-id="98114-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="98114-163">(Oczywiście w środowisku produkcyjnym, dwa serwery będą znajdują się za modułem równoważenia obciążenia.)</span><span class="sxs-lookup"><span data-stu-id="98114-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="98114-164">Po uruchomieniu aplikacji, możesz zobaczyć, że SignalR automatycznie utworzył tabele w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="98114-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="98114-165">SignalR zarządza tabelami.</span><span class="sxs-lookup"><span data-stu-id="98114-165">SignalR manages the tables.</span></span> <span data-ttu-id="98114-166">Tak długo, jak Twoja aplikacja zostanie wdrożona, nie usunąć wiersze, zmodyfikować tabeli i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="98114-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
