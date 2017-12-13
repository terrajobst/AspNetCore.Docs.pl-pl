---
uid: signalr/overview/older-versions/scaleout-with-redis
title: "Skalowania SignalR z pamięci podręcznej Redis (SignalR 1.x) | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: c0d6fd421dad02298326d1975ae68d1e7cc78d8c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="ddcc9-102">Skalowania SignalR z pamięci podręcznej Redis (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="ddcc9-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>
====================
<span data-ttu-id="ddcc9-103">przez [Wasson Jan](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="ddcc9-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="ddcc9-104">W tym samouczku użyjesz [Redis](http://redis.io/) przy dystrybucji wiadomości w aplikacji SignalR, który jest wdrożony na dwa różne wystąpienia usług IIS.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="ddcc9-105">Redis jest magazyn kluczy i wartości w pamięci.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="ddcc9-106">Obsługuje ona również system obsługi wiadomości z modelem publikowania/subskrypcji.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="ddcc9-107">Płyty montażowej SignalR Redis korzysta z funkcji pub/sub do przekazywania wiadomości na inne serwery.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="ddcc9-108">W tym samouczku użyjesz trzech serwerów:</span><span class="sxs-lookup"><span data-stu-id="ddcc9-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="ddcc9-109">Dwa serwery z systemem Windows, który będzie używany do wdrażania aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="ddcc9-110">Jeden serwer z systemem Linux, który będzie używany do uruchamiania Redis.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="ddcc9-111">Zrzuty ekranu w tym samouczku użyte I Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="ddcc9-112">Jeśli nie masz trzech serwerów fizycznych do użycia, można utworzyć maszyny wirtualne funkcji Hyper-v.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="ddcc9-113">Innym rozwiązaniem jest tworzenie maszyn wirtualnych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="ddcc9-114">Chociaż w tym samouczku korzysta z oficjalnego implementacji Redis, dostępna jest również [systemu Windows port Redis](https://github.com/MSOpenTech/redis) z MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="ddcc9-115">Instalacja i konfiguracja są różne, a w przeciwnym razie kroki są takie same.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ddcc9-116">Skalowania SignalR z pamięci podręcznej Redis nie obsługuje klastry Redis.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="ddcc9-117">Omówienie</span><span class="sxs-lookup"><span data-stu-id="ddcc9-117">Overview</span></span>

<span data-ttu-id="ddcc9-118">Przed uzyskujemy do szczegółowy samouczek, w tym miejscu jest to szybki przegląd będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="ddcc9-119">Zainstaluj usługę Redis i uruchomić serwera Redis.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="ddcc9-120">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ddcc9-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="ddcc9-121">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="ddcc9-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="ddcc9-122">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="ddcc9-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="ddcc9-123">Tworzenie aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="ddcc9-124">Dodaj następujący kod do pliku Global.asax do skonfigurowania systemu backplane:</span><span class="sxs-lookup"><span data-stu-id="ddcc9-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="ddcc9-125">Ubuntu w funkcji Hyper-V</span><span class="sxs-lookup"><span data-stu-id="ddcc9-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="ddcc9-126">Za pomocą funkcji Hyper-V systemu Windows, można łatwo utworzyć maszyny Wirtualnej systemu Ubuntu w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="ddcc9-127">Pobierz plik ISO Ubuntu od [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="ddcc9-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="ddcc9-128">W funkcji Hyper-V należy dodać nowej maszyny Wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="ddcc9-129">W **Podłączanie wirtualnego dysku twardego** krok, wybierz opcję **Utwórz wirtualny dysk twardy**.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="ddcc9-130">W **opcje instalacji** krok, wybierz opcję **plik obrazu (.iso)**, kliknij przycisk **Przeglądaj**i przejdź do instalacji Ubuntu ISO.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="ddcc9-131">Zainstaluj usługę Redis</span><span class="sxs-lookup"><span data-stu-id="ddcc9-131">Install Redis</span></span>

<span data-ttu-id="ddcc9-132">Wykonaj kroki opisane w temacie [http://redis.io/download](http://redis.io/download) do pobrania i Tworzenie pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="ddcc9-133">To z kolei pliki binarne Redis `src` katalogu.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="ddcc9-134">Domyślnie Redis nie wymaga hasła.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="ddcc9-135">Aby ustawić hasło, należy edytować `redis.conf` pliku, który znajduje się w katalogu głównym kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="ddcc9-136">(Utwórz kopię zapasową pliku, przed przystąpieniem do edytowania!) Dodaj następujące dyrektywy `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="ddcc9-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="ddcc9-137">Teraz można uruchomić serwera Redis:</span><span class="sxs-lookup"><span data-stu-id="ddcc9-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="ddcc9-138">Otwórz port 6379, który jest domyślnym portem, który Redis nasłuchuje.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="ddcc9-139">(Można zmienić numer portu w pliku konfiguracji.)</span><span class="sxs-lookup"><span data-stu-id="ddcc9-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="ddcc9-140">Tworzenie aplikacji SignalR</span><span class="sxs-lookup"><span data-stu-id="ddcc9-140">Create the SignalR Application</span></span>

<span data-ttu-id="ddcc9-141">Utwórz aplikację SignalR, wykonując jedną z tych samouczki:</span><span class="sxs-lookup"><span data-stu-id="ddcc9-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="ddcc9-142">Wprowadzenie do korzystania z biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="ddcc9-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="ddcc9-143">Wprowadzenie do korzystania z SignalR i MVC 4</span><span class="sxs-lookup"><span data-stu-id="ddcc9-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="ddcc9-144">Firma Microsoft będzie następnie zmodyfikować rozmów aplikację do obsługi skalowania w poziomie z pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="ddcc9-145">Najpierw Dodaj pakiet SignalR.Redis NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="ddcc9-146">W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-146">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="ddcc9-147">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ddcc9-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="ddcc9-148">Następnie otwórz plik Global.asax.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="ddcc9-149">Dodaj następujący kod do **aplikacji\_Start** metody:</span><span class="sxs-lookup"><span data-stu-id="ddcc9-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="ddcc9-150">"server" jest nazwą serwera, który działa Redis.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="ddcc9-151">*port* numer portu to</span><span class="sxs-lookup"><span data-stu-id="ddcc9-151">*port* is the port number</span></span>
- <span data-ttu-id="ddcc9-152">"password" oznacza hasło, które są zdefiniowane w pliku redis.conf.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="ddcc9-153">"AppName" jest dowolny ciąg.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-153">"AppName" is any string.</span></span> <span data-ttu-id="ddcc9-154">SignalR tworzy kanał Redis pub/sub o tej nazwie.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="ddcc9-155">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ddcc9-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="ddcc9-156">Wdrażanie i uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ddcc9-156">Deploy and Run the Application</span></span>

<span data-ttu-id="ddcc9-157">Przygotuj swoich wystąpień systemu Windows Server, aby wdrożyć aplikację SignalR.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="ddcc9-158">Dodaj rolę usług IIS.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-158">Add the IIS role.</span></span> <span data-ttu-id="ddcc9-159">Obejmują funkcje "Programowanie aplikacji", takie jak protokół WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="ddcc9-160">Obejmuje to też usługi zarządzania (wymienione w obszarze "Narzędzia do zarządzania").</span><span class="sxs-lookup"><span data-stu-id="ddcc9-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="ddcc9-161">**Zainstaluj narzędzie Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="ddcc9-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="ddcc9-162">Uruchom Menedżera usług IIS, zostanie wyświetlony monit do zainstalowania platformy Microsoft Web lub można [Pobierz intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="ddcc9-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="ddcc9-163">W Instalatorze platformy wyszukiwanie narzędzia Web Deploy i instalowanie usługi sieci Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="ddcc9-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="ddcc9-164">Sprawdź, czy jest uruchomiona usługa zarządzania siecią Web.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="ddcc9-165">Jeśli nie, należy uruchomić usługę.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-165">If not, start the service.</span></span> <span data-ttu-id="ddcc9-166">(Usługa zarządzania siecią Web nie jest widoczny na liście usług systemu Windows, aby się, że po dodaniu roli usług IIS są zainstalowane usługi zarządzania.)</span><span class="sxs-lookup"><span data-stu-id="ddcc9-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="ddcc9-167">Domyślnie usługa zarządzania siecią Web nasłuchuje na porcie TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="ddcc9-168">W Zaporze systemu Windows utwórz nową regułę ruchu przychodzącego zezwalająca na ruch TCP na porcie 8172.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="ddcc9-169">Aby uzyskać więcej informacji, zobacz [Konfigurowanie reguł zapory](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="ddcc9-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="ddcc9-170">(Jeśli są hostingu maszyn wirtualnych na platformie Azure, można w tym bezpośrednio w portalu Azure.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="ddcc9-171">Zobacz [jak skonfigurować punkty końcowe z maszyną wirtualną](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="ddcc9-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="ddcc9-172">Teraz można przystąpić do wdrażania projektu programu Visual Studio z komputerze deweloperskim z serwerem.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="ddcc9-173">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="ddcc9-174">Aby uzyskać bardziej szczegółowe dokumentacji dotyczące wdrożenia sieci web, zobacz [Mapa zawartości wdrożenia sieci Web dla platformy ASP.NET i Visual Studio](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="ddcc9-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="ddcc9-175">W przypadku wdrożenia aplikacji na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobacz, każdy z nich odbierać wiadomości SignalR z innych.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="ddcc9-176">(Oczywiście w środowisku produkcyjnym, dwa serwery będą sit za modułem równoważenia obciążenia.)</span><span class="sxs-lookup"><span data-stu-id="ddcc9-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="ddcc9-177">Jeśli zastanawiasz się wyświetlanie komunikatów, które są wysyłane do serwera Redis, można użyć **redis-cli** klienta, który instaluje z pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="ddcc9-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
