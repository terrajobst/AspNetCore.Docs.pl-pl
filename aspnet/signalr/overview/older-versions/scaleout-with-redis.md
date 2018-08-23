---
uid: signalr/overview/older-versions/scaleout-with-redis
title: SignalR — skalowanie w poziomie z pamięcią podręczną Redis (SignalR 1.x) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 4f587b129a1a22e64625d2ab0fc7655984262ebe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755554"
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="ac92d-102">SignalR — skalowanie w poziomie z pamięcią podręczną Redis (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="ac92d-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>
====================
<span data-ttu-id="ac92d-103">przez [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="ac92d-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="ac92d-104">W tym samouczku użyjesz [Redis](http://redis.io/) aby dystrybuować komunikaty aplikacji SignalR, która została wdrożona w dwóch osobnych wystąpień usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="ac92d-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="ac92d-105">Redis jest przechowywanie par klucz wartość w pamięci.</span><span class="sxs-lookup"><span data-stu-id="ac92d-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="ac92d-106">Obsługuje ona również system obsługi komunikatów za pomocą modelu publikowania/subskrybowania.</span><span class="sxs-lookup"><span data-stu-id="ac92d-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="ac92d-107">Płyty montażowej Redis w SignalR używa funkcji publikowania/subskrybowania do przekazywania wiadomości na inne serwery.</span><span class="sxs-lookup"><span data-stu-id="ac92d-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="ac92d-108">W tym samouczku użyjesz trzech serwerów:</span><span class="sxs-lookup"><span data-stu-id="ac92d-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="ac92d-109">Dwa serwery, systemem Windows, który będzie używany do wdrażania aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="ac92d-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="ac92d-110">Jeden serwer z systemem Linux, który będzie używany do uruchamiania usługi Redis.</span><span class="sxs-lookup"><span data-stu-id="ac92d-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="ac92d-111">Zrzuty ekranu w tym samouczku używany Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="ac92d-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="ac92d-112">Jeśli nie masz trzech serwerów fizycznych do użycia, można utworzyć maszyny wirtualne funkcji Hyper-v.</span><span class="sxs-lookup"><span data-stu-id="ac92d-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="ac92d-113">Innym rozwiązaniem jest tworzenie maszyn wirtualnych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="ac92d-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="ac92d-114">Chociaż w tym samouczku korzysta z oficjalnego implementacja pamięci podręcznej Redis, dostępna jest również [Windows port Redis](https://github.com/MSOpenTech/redis) z MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="ac92d-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="ac92d-115">Instalacja i konfiguracja są różne, ale w przeciwnym razie kroki są takie same.</span><span class="sxs-lookup"><span data-stu-id="ac92d-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ac92d-116">SignalR — skalowanie w poziomie przy użyciu usługi Redis nie obsługuje klastrów usługi Redis.</span><span class="sxs-lookup"><span data-stu-id="ac92d-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="ac92d-117">Omówienie</span><span class="sxs-lookup"><span data-stu-id="ac92d-117">Overview</span></span>

<span data-ttu-id="ac92d-118">Przed przejściem do szczegółowe podręcznika, poniżej przedstawiono krótkie podsumowanie będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="ac92d-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="ac92d-119">Zainstaluj usługę Redis i uruchomić serwera Redis.</span><span class="sxs-lookup"><span data-stu-id="ac92d-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="ac92d-120">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ac92d-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="ac92d-121">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="ac92d-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="ac92d-122">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="ac92d-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="ac92d-123">Tworzenie aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="ac92d-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="ac92d-124">Dodaj następujący kod do pliku Global.asax w celu skonfigurowania systemu backplane:</span><span class="sxs-lookup"><span data-stu-id="ac92d-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="ac92d-125">Ubuntu na funkcji Hyper-V</span><span class="sxs-lookup"><span data-stu-id="ac92d-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="ac92d-126">Za pomocą funkcji Windows Hyper-V, można łatwo utworzyć Maszynę wirtualną Ubuntu w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="ac92d-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="ac92d-127">Pobierz obraz ISO systemu Ubuntu z [ http://www.ubuntu.com ](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="ac92d-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="ac92d-128">W funkcji Hyper-V Dodaj nową maszynę Wirtualną.</span><span class="sxs-lookup"><span data-stu-id="ac92d-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="ac92d-129">W **Podłączanie wirtualnego dysku twardego** kroku, wybierz pozycję **Utwórz wirtualny dysk twardy**.</span><span class="sxs-lookup"><span data-stu-id="ac92d-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="ac92d-130">W **opcje instalacji** kroku, wybierz pozycję **pliku obrazu (.iso)**, kliknij przycisk **Przeglądaj**, a następnie przejdź do instalacji systemu Ubuntu ISO.</span><span class="sxs-lookup"><span data-stu-id="ac92d-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="ac92d-131">Zainstaluj usługę Redis</span><span class="sxs-lookup"><span data-stu-id="ac92d-131">Install Redis</span></span>

<span data-ttu-id="ac92d-132">Wykonaj kroki opisane w temacie [ http://redis.io/download ](http://redis.io/download) do pobrania i Tworzenie pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="ac92d-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="ac92d-133">To tworzy pliki binarne pamięci podręcznej Redis `src` katalogu.</span><span class="sxs-lookup"><span data-stu-id="ac92d-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="ac92d-134">Domyślnie usługa Redis nie wymaga hasła.</span><span class="sxs-lookup"><span data-stu-id="ac92d-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="ac92d-135">Aby ustawić hasło, należy edytować `redis.conf` pliku, który znajduje się w katalogu głównym kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="ac92d-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="ac92d-136">(Utwórz kopię zapasową pliku, przed przystąpieniem do edytowania)! Dodaj następujące dyrektywy `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="ac92d-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="ac92d-137">Teraz uruchom serwer Redis:</span><span class="sxs-lookup"><span data-stu-id="ac92d-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="ac92d-138">Otwarty port 6379, który jest domyślnym portem, który Redis nasłuchuje.</span><span class="sxs-lookup"><span data-stu-id="ac92d-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="ac92d-139">(Można zmienić numer portu w pliku konfiguracji.)</span><span class="sxs-lookup"><span data-stu-id="ac92d-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="ac92d-140">Tworzenie aplikacji SignalR</span><span class="sxs-lookup"><span data-stu-id="ac92d-140">Create the SignalR Application</span></span>

<span data-ttu-id="ac92d-141">Tworzenie aplikacji SignalR, wykonując dowolną z następujących samouczków:</span><span class="sxs-lookup"><span data-stu-id="ac92d-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="ac92d-142">Wprowadzenie do SignalR</span><span class="sxs-lookup"><span data-stu-id="ac92d-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="ac92d-143">Wprowadzenie do SignalR i MVC 4</span><span class="sxs-lookup"><span data-stu-id="ac92d-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="ac92d-144">Następnie zmodyfikujemy aplikacji rozmów w celu obsługi skalowania w poziomie przy użyciu usługi Redis.</span><span class="sxs-lookup"><span data-stu-id="ac92d-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="ac92d-145">Najpierw Dodaj pakiet SignalR.Redis NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="ac92d-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="ac92d-146">W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="ac92d-146">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="ac92d-147">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ac92d-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="ac92d-148">Następnie otwórz plik Global.asax.</span><span class="sxs-lookup"><span data-stu-id="ac92d-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="ac92d-149">Dodaj następujący kod do **aplikacji\_Start** metody:</span><span class="sxs-lookup"><span data-stu-id="ac92d-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="ac92d-150">"server" jest nazwą serwera, na którym jest uruchomiona usługa Redis.</span><span class="sxs-lookup"><span data-stu-id="ac92d-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="ac92d-151">*port* jest numerem portu</span><span class="sxs-lookup"><span data-stu-id="ac92d-151">*port* is the port number</span></span>
- <span data-ttu-id="ac92d-152">"password" jest hasło, które są zdefiniowane w pliku redis.conf.</span><span class="sxs-lookup"><span data-stu-id="ac92d-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="ac92d-153">"AppName" jest dowolny ciąg.</span><span class="sxs-lookup"><span data-stu-id="ac92d-153">"AppName" is any string.</span></span> <span data-ttu-id="ac92d-154">SignalR tworzy kanał Redis pub/sub o tej nazwie.</span><span class="sxs-lookup"><span data-stu-id="ac92d-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="ac92d-155">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ac92d-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="ac92d-156">Wdrażanie i uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ac92d-156">Deploy and Run the Application</span></span>

<span data-ttu-id="ac92d-157">Przygotowanie do wdrożenia aplikacji SignalR wystąpień systemu Windows Server.</span><span class="sxs-lookup"><span data-stu-id="ac92d-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="ac92d-158">Dodaj rolę usług IIS.</span><span class="sxs-lookup"><span data-stu-id="ac92d-158">Add the IIS role.</span></span> <span data-ttu-id="ac92d-159">Obejmują funkcje "Opracowywanie zawartości dla aplikacji", w tym protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ac92d-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="ac92d-160">Również obejmować usługę zarządzania (wymienione w obszarze "Narzędzia do zarządzania").</span><span class="sxs-lookup"><span data-stu-id="ac92d-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="ac92d-161">**Zainstaluj narzędzie Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="ac92d-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="ac92d-162">Podczas uruchamiania Menedżera usług IIS, zostanie wyświetlony monit o zainstalowanie platformy sieci Web firmy Microsoft lub możesz [Pobierz intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="ac92d-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="ac92d-163">W Instalatorze platformy wyszukiwania dla narzędzia Web Deploy i zainstalować Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="ac92d-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="ac92d-164">Sprawdź, czy usługa zarządzania sieci Web jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="ac92d-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="ac92d-165">Jeśli nie, uruchom usługę.</span><span class="sxs-lookup"><span data-stu-id="ac92d-165">If not, start the service.</span></span> <span data-ttu-id="ac92d-166">(Jeśli nie widzisz usługi zarządzania siecią Web, na liście usług Windows, upewnij się, że po dodaniu roli usług IIS są zainstalowane usługi zarządzania.)</span><span class="sxs-lookup"><span data-stu-id="ac92d-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="ac92d-167">Domyślnie usługi zarządzania siecią Web nasłuchuje na porcie TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="ac92d-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="ac92d-168">W Zaporze Windows utwórz nową regułę dla ruchu przychodzącego, aby umożliwić ruch TCP na porcie 8172.</span><span class="sxs-lookup"><span data-stu-id="ac92d-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="ac92d-169">Aby uzyskać więcej informacji, zobacz [Konfigurowanie reguł zapory](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="ac92d-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="ac92d-170">(Jeśli są hostingu maszyn wirtualnych na platformie Azure, możesz to zrobić bezpośrednio w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="ac92d-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="ac92d-171">Zobacz [jak skonfigurować punkty końcowe do maszyny wirtualnej](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="ac92d-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="ac92d-172">Teraz wszystko jest gotowe do wdrożenia projektu programu Visual Studio z komputera deweloperskiego z serwerem.</span><span class="sxs-lookup"><span data-stu-id="ac92d-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="ac92d-173">W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="ac92d-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="ac92d-174">Aby uzyskać bardziej szczegółową dokumentację na temat wdrażania w Internecie, zobacz [zawartości mapy wdrażania w sieci Web dla programu Visual Studio i platformy ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="ac92d-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="ac92d-175">W przypadku wdrożenia aplikacji na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobacz ich odbierania komunikatów SignalR w innej.</span><span class="sxs-lookup"><span data-stu-id="ac92d-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="ac92d-176">(Oczywiście w środowisku produkcyjnym, dwa serwery będą znajdują się za modułem równoważenia obciążenia.)</span><span class="sxs-lookup"><span data-stu-id="ac92d-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="ac92d-177">Jeśli zastanawiasz się wyświetlić komunikaty, które są wysyłane do pamięci Redis, można użyć **redis-cli** klienta, który instaluje przy użyciu usługi Redis.</span><span class="sxs-lookup"><span data-stu-id="ac92d-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
