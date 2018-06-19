---
uid: signalr/overview/performance/scaleout-with-redis
title: Skalowania SignalR z pamięci podręcznej Redis | Dokumentacja firmy Microsoft
author: MikeWasson
description: Wersje oprogramowania używane w tym temacie Visual Studio 2013 .NET 4.5 SignalR w wersji 2 poprzednie wersje tego tematu informacji o wcześniejszych wersji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 2ef161f35e69ef4a754d2740199166ee48c3fbab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042693"
---
<a name="signalr-scaleout-with-redis"></a><span data-ttu-id="30787-103">Skalowania SignalR z pamięci podręcznej Redis</span><span class="sxs-lookup"><span data-stu-id="30787-103">SignalR Scaleout with Redis</span></span>
====================
<span data-ttu-id="30787-104">przez [Wasson Jan](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="30787-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="30787-105">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="30787-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="30787-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="30787-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="30787-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="30787-107">.NET 4.5</span></span>
> - <span data-ttu-id="30787-108">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="30787-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="30787-109">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="30787-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="30787-110">Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="30787-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="30787-111">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="30787-111">Questions and comments</span></span>
> 
> <span data-ttu-id="30787-112">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="30787-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="30787-113">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="30787-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="30787-114">W tym samouczku użyjesz [Redis](http://redis.io/) przy dystrybucji wiadomości w aplikacji SignalR, który jest wdrożony na dwa różne wystąpienia usług IIS.</span><span class="sxs-lookup"><span data-stu-id="30787-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="30787-115">Redis jest magazyn kluczy i wartości w pamięci.</span><span class="sxs-lookup"><span data-stu-id="30787-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="30787-116">Obsługuje ona również system obsługi wiadomości z modelem publikowania/subskrypcji.</span><span class="sxs-lookup"><span data-stu-id="30787-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="30787-117">Płyty montażowej SignalR Redis korzysta z funkcji pub/sub do przekazywania wiadomości na inne serwery.</span><span class="sxs-lookup"><span data-stu-id="30787-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="30787-118">W tym samouczku użyjesz trzech serwerów:</span><span class="sxs-lookup"><span data-stu-id="30787-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="30787-119">Dwa serwery z systemem Windows, który będzie używany do wdrażania aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="30787-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="30787-120">Jeden serwer z systemem Linux, który będzie używany do uruchamiania Redis.</span><span class="sxs-lookup"><span data-stu-id="30787-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="30787-121">Zrzuty ekranu w tym samouczku użyte I Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="30787-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="30787-122">Jeśli nie masz trzech serwerów fizycznych do użycia, można utworzyć maszyny wirtualne funkcji Hyper-v.</span><span class="sxs-lookup"><span data-stu-id="30787-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="30787-123">Innym rozwiązaniem jest tworzenie maszyn wirtualnych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="30787-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="30787-124">Chociaż w tym samouczku korzysta z oficjalnego implementacji Redis, dostępna jest również [systemu Windows port Redis](https://github.com/MSOpenTech/redis) z MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="30787-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="30787-125">Instalacja i konfiguracja są różne, a w przeciwnym razie kroki są takie same.</span><span class="sxs-lookup"><span data-stu-id="30787-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="30787-126">Skalowania SignalR z pamięci podręcznej Redis nie obsługuje klastry Redis.</span><span class="sxs-lookup"><span data-stu-id="30787-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="30787-127">Omówienie</span><span class="sxs-lookup"><span data-stu-id="30787-127">Overview</span></span>

<span data-ttu-id="30787-128">Przed uzyskujemy do szczegółowy samouczek, w tym miejscu jest to szybki przegląd będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="30787-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="30787-129">Zainstaluj usługę Redis i uruchomić serwera Redis.</span><span class="sxs-lookup"><span data-stu-id="30787-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="30787-130">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="30787-130">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="30787-131">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="30787-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="30787-132">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="30787-132">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="30787-133">Tworzenie aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="30787-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="30787-134">Dodaj następujący kod do pliku Startup.cs do skonfigurowania systemu backplane:</span><span class="sxs-lookup"><span data-stu-id="30787-134">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="30787-135">Ubuntu w funkcji Hyper-V</span><span class="sxs-lookup"><span data-stu-id="30787-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="30787-136">Za pomocą funkcji Hyper-V systemu Windows, można łatwo utworzyć maszyny Wirtualnej systemu Ubuntu w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="30787-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="30787-137">Pobierz plik ISO Ubuntu od [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="30787-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="30787-138">W funkcji Hyper-V należy dodać nowej maszyny Wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="30787-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="30787-139">W **Podłączanie wirtualnego dysku twardego** krok, wybierz opcję **Utwórz wirtualny dysk twardy**.</span><span class="sxs-lookup"><span data-stu-id="30787-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="30787-140">W **opcje instalacji** krok, wybierz opcję **plik obrazu (.iso)**, kliknij przycisk **Przeglądaj**i przejdź do instalacji Ubuntu ISO.</span><span class="sxs-lookup"><span data-stu-id="30787-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="30787-141">Zainstaluj usługę Redis</span><span class="sxs-lookup"><span data-stu-id="30787-141">Install Redis</span></span>

<span data-ttu-id="30787-142">Wykonaj kroki opisane w temacie [http://redis.io/download](http://redis.io/download) do pobrania i Tworzenie pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="30787-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="30787-143">To z kolei pliki binarne Redis `src` katalogu.</span><span class="sxs-lookup"><span data-stu-id="30787-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="30787-144">Domyślnie Redis nie wymaga hasła.</span><span class="sxs-lookup"><span data-stu-id="30787-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="30787-145">Aby ustawić hasło, należy edytować `redis.conf` pliku, który znajduje się w katalogu głównym kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="30787-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="30787-146">(Utwórz kopię zapasową pliku, przed przystąpieniem do edytowania!) Dodaj następujące dyrektywy `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="30787-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="30787-147">Teraz można uruchomić serwera Redis:</span><span class="sxs-lookup"><span data-stu-id="30787-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="30787-148">Otwórz port 6379, który jest domyślnym portem, który Redis nasłuchuje.</span><span class="sxs-lookup"><span data-stu-id="30787-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="30787-149">(Można zmienić numer portu w pliku konfiguracji.)</span><span class="sxs-lookup"><span data-stu-id="30787-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="30787-150">Tworzenie aplikacji SignalR</span><span class="sxs-lookup"><span data-stu-id="30787-150">Create the SignalR Application</span></span>

<span data-ttu-id="30787-151">Utwórz aplikację SignalR, wykonując jedną z tych samouczki:</span><span class="sxs-lookup"><span data-stu-id="30787-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="30787-152">Wprowadzenie do korzystania z SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="30787-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="30787-153">Wprowadzenie do korzystania z SignalR 2.0 i MVC 5</span><span class="sxs-lookup"><span data-stu-id="30787-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="30787-154">Firma Microsoft będzie następnie zmodyfikować rozmów aplikację do obsługi skalowania w poziomie z pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="30787-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="30787-155">Najpierw Dodaj pakiet SignalR.Redis NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="30787-155">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="30787-156">W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="30787-156">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="30787-157">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="30787-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="30787-158">Następnie otwórz folder pliku Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="30787-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="30787-159">Dodaj następujący kod do **konfiguracji** metody:</span><span class="sxs-lookup"><span data-stu-id="30787-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="30787-160">"server" jest nazwą serwera, który działa Redis.</span><span class="sxs-lookup"><span data-stu-id="30787-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="30787-161">*port* numer portu to</span><span class="sxs-lookup"><span data-stu-id="30787-161">*port* is the port number</span></span>
- <span data-ttu-id="30787-162">"password" oznacza hasło, które są zdefiniowane w pliku redis.conf.</span><span class="sxs-lookup"><span data-stu-id="30787-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="30787-163">"AppName" jest dowolny ciąg.</span><span class="sxs-lookup"><span data-stu-id="30787-163">"AppName" is any string.</span></span> <span data-ttu-id="30787-164">SignalR tworzy kanał Redis pub/sub o tej nazwie.</span><span class="sxs-lookup"><span data-stu-id="30787-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="30787-165">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="30787-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="30787-166">Wdrażanie i uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="30787-166">Deploy and Run the Application</span></span>

<span data-ttu-id="30787-167">Przygotuj swoich wystąpień systemu Windows Server, aby wdrożyć aplikację SignalR.</span><span class="sxs-lookup"><span data-stu-id="30787-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="30787-168">Dodaj rolę usług IIS.</span><span class="sxs-lookup"><span data-stu-id="30787-168">Add the IIS role.</span></span> <span data-ttu-id="30787-169">Obejmują funkcje "Programowanie aplikacji", takie jak protokół WebSocket.</span><span class="sxs-lookup"><span data-stu-id="30787-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="30787-170">Obejmuje to też usługi zarządzania (wymienione w obszarze "Narzędzia do zarządzania").</span><span class="sxs-lookup"><span data-stu-id="30787-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="30787-171">**Zainstaluj narzędzie Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="30787-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="30787-172">Uruchom Menedżera usług IIS, zostanie wyświetlony monit do zainstalowania platformy Microsoft Web lub można [Pobierz intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="30787-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="30787-173">W Instalatorze platformy wyszukiwanie narzędzia Web Deploy i instalowanie usługi sieci Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="30787-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="30787-174">Sprawdź, czy jest uruchomiona usługa zarządzania siecią Web.</span><span class="sxs-lookup"><span data-stu-id="30787-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="30787-175">Jeśli nie, należy uruchomić usługę.</span><span class="sxs-lookup"><span data-stu-id="30787-175">If not, start the service.</span></span> <span data-ttu-id="30787-176">(Usługa zarządzania siecią Web nie jest widoczny na liście usług systemu Windows, aby się, że po dodaniu roli usług IIS są zainstalowane usługi zarządzania.)</span><span class="sxs-lookup"><span data-stu-id="30787-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="30787-177">Domyślnie usługa zarządzania siecią Web nasłuchuje na porcie TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="30787-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="30787-178">W Zaporze systemu Windows utwórz nową regułę ruchu przychodzącego zezwalająca na ruch TCP na porcie 8172.</span><span class="sxs-lookup"><span data-stu-id="30787-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="30787-179">Aby uzyskać więcej informacji, zobacz [Konfigurowanie reguł zapory](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="30787-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="30787-180">(Jeśli są hostingu maszyn wirtualnych na platformie Azure, można w tym bezpośrednio w portalu Azure.</span><span class="sxs-lookup"><span data-stu-id="30787-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="30787-181">Zobacz [jak skonfigurować punkty końcowe z maszyną wirtualną](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="30787-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="30787-182">Teraz można przystąpić do wdrażania projektu programu Visual Studio z komputerze deweloperskim z serwerem.</span><span class="sxs-lookup"><span data-stu-id="30787-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="30787-183">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="30787-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="30787-184">Aby uzyskać bardziej szczegółowe dokumentacji dotyczące wdrożenia sieci web, zobacz [Mapa zawartości wdrożenia sieci Web dla platformy ASP.NET i Visual Studio](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="30787-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="30787-185">W przypadku wdrożenia aplikacji na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobacz, każdy z nich odbierać wiadomości SignalR z innych.</span><span class="sxs-lookup"><span data-stu-id="30787-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="30787-186">(Oczywiście w środowisku produkcyjnym, dwa serwery będą sit za modułem równoważenia obciążenia.)</span><span class="sxs-lookup"><span data-stu-id="30787-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="30787-187">Jeśli zastanawiasz się wyświetlanie komunikatów, które są wysyłane do serwera Redis, można użyć **redis-cli** klienta, który instaluje z pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="30787-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
