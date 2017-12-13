---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Skalowania SignalR z programem SQL Server (SignalR 1.x) | Dokumentacja firmy Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7a589f5c6a5196444c3c616b39f501f3a7512471
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="bf118-102">Skalowania SignalR z programem SQL Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="bf118-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="bf118-103">przez [Wasson Jan](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="bf118-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="bf118-104">W tym samouczku użyjesz programu SQL Server do dystrybucji wiadomości w aplikacji SignalR, które zostało wdrożone w dwóch osobnych wystąpień usług IIS.</span><span class="sxs-lookup"><span data-stu-id="bf118-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="bf118-105">W tym samouczku można również uruchomić na maszynie pojedynczy test, ale uzyskanie pełnego efektu, należy wdrożyć co najmniej dwa serwery aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="bf118-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="bf118-106">SQL Server należy również zainstalować na jednym serwerze lub na osobnym serwerze dedykowanym.</span><span class="sxs-lookup"><span data-stu-id="bf118-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="bf118-107">Innym rozwiązaniem jest Uruchamianie samouczka przy użyciu maszyn wirtualnych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="bf118-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="bf118-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="bf118-108">Prerequisites</span></span>

<span data-ttu-id="bf118-109">Microsoft SQL Server 2005 lub nowszego.</span><span class="sxs-lookup"><span data-stu-id="bf118-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="bf118-110">Systemu backplane obsługuje zarówno stacjonarnych i serwerów wersje programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bf118-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="bf118-111">Nie obsługuje programu SQL Server Compact Edition lub baza danych SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="bf118-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="bf118-112">(Jeśli aplikacja jest hostowana na platformie Azure, należy wziąć pod uwagę płyty montażowej usługi Service Bus zamiast.)</span><span class="sxs-lookup"><span data-stu-id="bf118-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="bf118-113">Omówienie</span><span class="sxs-lookup"><span data-stu-id="bf118-113">Overview</span></span>

<span data-ttu-id="bf118-114">Przed uzyskujemy do szczegółowy samouczek, w tym miejscu jest to szybki przegląd będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="bf118-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="bf118-115">Tworzenie nowej pustej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bf118-115">Create a new empty database.</span></span> <span data-ttu-id="bf118-116">Systemu backplane utworzy potrzebne tabele w tej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="bf118-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="bf118-117">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="bf118-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="bf118-118">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="bf118-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="bf118-119">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="bf118-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="bf118-120">Tworzenie aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="bf118-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="bf118-121">Dodaj następujący kod do pliku Global.asax do skonfigurowania systemu backplane:</span><span class="sxs-lookup"><span data-stu-id="bf118-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="bf118-122">Skonfiguruj bazę danych</span><span class="sxs-lookup"><span data-stu-id="bf118-122">Configure the Database</span></span>

<span data-ttu-id="bf118-123">Zdecyduj, czy aplikacja będzie używać uwierzytelniania systemu Windows lub uwierzytelniania programu SQL Server dostęp do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bf118-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="bf118-124">Niezależnie od tego upewnij się, że użytkownik bazy danych ma uprawnienia do logowania, Utwórz schematy i tworzenie tabel.</span><span class="sxs-lookup"><span data-stu-id="bf118-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="bf118-125">Utwórz nową bazę danych do płyty montażowej do użycia.</span><span class="sxs-lookup"><span data-stu-id="bf118-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="bf118-126">Bazy danych można nadać dowolną nazwę.</span><span class="sxs-lookup"><span data-stu-id="bf118-126">You can give the database any name.</span></span> <span data-ttu-id="bf118-127">Nie trzeba tworzyć żadnych tabel w bazie danych. systemu backplane utworzy potrzebne tabele.</span><span class="sxs-lookup"><span data-stu-id="bf118-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="bf118-128">Włącz brokera usług</span><span class="sxs-lookup"><span data-stu-id="bf118-128">Enable Service Broker</span></span>

<span data-ttu-id="bf118-129">Zalecane jest, aby włączyć brokera usługi dla bazy danych systemu backplane.</span><span class="sxs-lookup"><span data-stu-id="bf118-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="bf118-130">Usługa Service Broker zapewnia macierzystą obsługę dla obsługi wiadomości i kolejkowania w programie SQL Server, co pozwala montażowa wydajniej otrzymywać aktualizacje.</span><span class="sxs-lookup"><span data-stu-id="bf118-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="bf118-131">(Jednak systemu backplane również działa bez programu Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="bf118-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="bf118-132">Aby sprawdzić, czy Usługa Service Broker jest włączona, zapytanie **jest\_brokera\_włączone** kolumny w **sys.databases** widoku katalogu.</span><span class="sxs-lookup"><span data-stu-id="bf118-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="bf118-133">Aby włączyć brokera usługi, użyj następującej kwerendy SQL:</span><span class="sxs-lookup"><span data-stu-id="bf118-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="bf118-134">Jeśli to zapytanie wydaje się do zakleszczenia, upewnij się, że nie ma żadnych aplikacji, połączony z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="bf118-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="bf118-135">Jeśli śledzenie jest włączone, dane śledzenia zostaną wyświetlone również czy Service Broker jest włączona.</span><span class="sxs-lookup"><span data-stu-id="bf118-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="bf118-136">Tworzenie aplikacji SignalR</span><span class="sxs-lookup"><span data-stu-id="bf118-136">Create a SignalR Application</span></span>

<span data-ttu-id="bf118-137">Utwórz aplikację SignalR, wykonując jedną z tych samouczki:</span><span class="sxs-lookup"><span data-stu-id="bf118-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="bf118-138">Wprowadzenie do korzystania z biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="bf118-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="bf118-139">Wprowadzenie do korzystania z SignalR i MVC 4</span><span class="sxs-lookup"><span data-stu-id="bf118-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="bf118-140">Firma Microsoft będzie następnie zmodyfikować rozmów aplikację do obsługi skalowania w poziomie z programem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bf118-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="bf118-141">Najpierw Dodaj pakiet SignalR.SqlServer NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="bf118-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="bf118-142">W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="bf118-142">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="bf118-143">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bf118-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="bf118-144">Następnie otwórz plik Global.asax.</span><span class="sxs-lookup"><span data-stu-id="bf118-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="bf118-145">Dodaj następujący kod do **aplikacji\_Start** metody:</span><span class="sxs-lookup"><span data-stu-id="bf118-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="bf118-146">Wdrażanie i uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="bf118-146">Deploy and Run the Application</span></span>

<span data-ttu-id="bf118-147">Przygotuj swoich wystąpień systemu Windows Server, aby wdrożyć aplikację SignalR.</span><span class="sxs-lookup"><span data-stu-id="bf118-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="bf118-148">Dodaj rolę usług IIS.</span><span class="sxs-lookup"><span data-stu-id="bf118-148">Add the IIS role.</span></span> <span data-ttu-id="bf118-149">Obejmują funkcje "Programowanie aplikacji", takie jak protokół WebSocket.</span><span class="sxs-lookup"><span data-stu-id="bf118-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="bf118-150">Obejmuje to też usługi zarządzania (wymienione w obszarze "Narzędzia do zarządzania").</span><span class="sxs-lookup"><span data-stu-id="bf118-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="bf118-151">**Zainstaluj narzędzie Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="bf118-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="bf118-152">Uruchom Menedżera usług IIS, zostanie wyświetlony monit do zainstalowania platformy Microsoft Web lub można [Pobierz intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="bf118-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="bf118-153">W Instalatorze platformy wyszukiwanie narzędzia Web Deploy i instalowanie usługi sieci Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="bf118-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="bf118-154">Sprawdź, czy jest uruchomiona usługa zarządzania siecią Web.</span><span class="sxs-lookup"><span data-stu-id="bf118-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="bf118-155">Jeśli nie, należy uruchomić usługę.</span><span class="sxs-lookup"><span data-stu-id="bf118-155">If not, start the service.</span></span> <span data-ttu-id="bf118-156">(Usługa zarządzania siecią Web nie jest widoczny na liście usług systemu Windows, aby się, że po dodaniu roli usług IIS są zainstalowane usługi zarządzania.)</span><span class="sxs-lookup"><span data-stu-id="bf118-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="bf118-157">Na koniec Otwórz port 8172 dla protokołu TCP.</span><span class="sxs-lookup"><span data-stu-id="bf118-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="bf118-158">Jest to port, który używa narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="bf118-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="bf118-159">Teraz można przystąpić do wdrażania projektu programu Visual Studio z komputerze deweloperskim z serwerem.</span><span class="sxs-lookup"><span data-stu-id="bf118-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="bf118-160">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="bf118-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="bf118-161">Aby uzyskać bardziej szczegółowe dokumentacji dotyczące wdrożenia sieci web, zobacz [Mapa zawartości wdrożenia sieci Web dla platformy ASP.NET i Visual Studio](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="bf118-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="bf118-162">W przypadku wdrożenia aplikacji na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobacz, każdy z nich odbierać wiadomości SignalR z innych.</span><span class="sxs-lookup"><span data-stu-id="bf118-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="bf118-163">(Oczywiście w środowisku produkcyjnym, dwa serwery będą sit za modułem równoważenia obciążenia.)</span><span class="sxs-lookup"><span data-stu-id="bf118-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="bf118-164">Po uruchomieniu aplikacji widać, że SignalR automatycznie utworzył tabele w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="bf118-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="bf118-165">SignalR zarządza tabel.</span><span class="sxs-lookup"><span data-stu-id="bf118-165">SignalR manages the tables.</span></span> <span data-ttu-id="bf118-166">Tak długo, jak aplikacja jest wdrażana, nie usuwać wiersze, zmodyfikuj tabelę i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="bf118-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
