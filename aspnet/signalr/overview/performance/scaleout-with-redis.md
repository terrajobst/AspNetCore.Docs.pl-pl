---
uid: signalr/overview/performance/scaleout-with-redis
title: SignalR — skalowanie w poziomie przy użyciu usługi Redis | Dokumentacja firmy Microsoft
author: bradygaster
description: Wersje oprogramowania, używaną w tym temacie dodatku .NET 4.5 SignalR dla programu Visual Studio 2013 w wersji 2, poprzednie wersje w tym temacie informacji o wcześniejszych wersjach...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 22b8379fd2d97aeb85137e1cc128fe5d053f44ed
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837029"
---
<a name="signalr-scaleout-with-redis"></a><span data-ttu-id="b1724-103">SignalR — skalowanie w poziomie z użyciem pamięci podręcznej Redis</span><span class="sxs-lookup"><span data-stu-id="b1724-103">SignalR Scaleout with Redis</span></span>
====================
<span data-ttu-id="b1724-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b1724-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="b1724-105">Wersje oprogramowania używaną w tym temacie</span><span class="sxs-lookup"><span data-stu-id="b1724-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="b1724-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b1724-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="b1724-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b1724-107">.NET 4.5</span></span>
> - <span data-ttu-id="b1724-108">SignalR wersji 2.4</span><span class="sxs-lookup"><span data-stu-id="b1724-108">SignalR version 2.4</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="b1724-109">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="b1724-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="b1724-110">Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="b1724-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="b1724-111">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="b1724-111">Questions and comments</span></span>
>
> <span data-ttu-id="b1724-112">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="b1724-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b1724-113">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b1724-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="b1724-114">W tym samouczku użyjesz [Redis](http://redis.io/) aby dystrybuować komunikaty aplikacji SignalR, która została wdrożona w dwóch osobnych wystąpień usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="b1724-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="b1724-115">Redis jest przechowywanie par klucz wartość w pamięci.</span><span class="sxs-lookup"><span data-stu-id="b1724-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="b1724-116">Obsługuje ona również system obsługi komunikatów za pomocą modelu publikowania/subskrybowania.</span><span class="sxs-lookup"><span data-stu-id="b1724-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="b1724-117">Płyty montażowej Redis w SignalR używa funkcji publikowania/subskrybowania do przekazywania wiadomości na inne serwery.</span><span class="sxs-lookup"><span data-stu-id="b1724-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="b1724-118">W tym samouczku użyjesz trzech serwerów:</span><span class="sxs-lookup"><span data-stu-id="b1724-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="b1724-119">Dwa serwery, systemem Windows, który będzie używany do wdrażania aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="b1724-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="b1724-120">Jeden serwer z systemem Linux, który będzie używany do uruchamiania usługi Redis.</span><span class="sxs-lookup"><span data-stu-id="b1724-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="b1724-121">Zrzuty ekranu w tym samouczku używany Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="b1724-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="b1724-122">Jeśli nie masz trzech serwerów fizycznych do użycia, można utworzyć maszyny wirtualne funkcji Hyper-v.</span><span class="sxs-lookup"><span data-stu-id="b1724-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="b1724-123">Innym rozwiązaniem jest tworzenie maszyn wirtualnych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="b1724-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="b1724-124">Chociaż w tym samouczku korzysta z oficjalnego implementacja pamięci podręcznej Redis, dostępna jest również [Windows port Redis](https://github.com/MSOpenTech/redis) z MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="b1724-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="b1724-125">Instalacja i konfiguracja są różne, ale w przeciwnym razie kroki są takie same.</span><span class="sxs-lookup"><span data-stu-id="b1724-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE]
>
> <span data-ttu-id="b1724-126">SignalR — skalowanie w poziomie przy użyciu usługi Redis nie obsługuje klastrów usługi Redis.</span><span class="sxs-lookup"><span data-stu-id="b1724-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="b1724-127">Omówienie</span><span class="sxs-lookup"><span data-stu-id="b1724-127">Overview</span></span>

<span data-ttu-id="b1724-128">Przed przejściem do szczegółowe podręcznika, poniżej przedstawiono krótkie podsumowanie będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="b1724-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="b1724-129">Zainstaluj usługę Redis i uruchomić serwera Redis.</span><span class="sxs-lookup"><span data-stu-id="b1724-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="b1724-130">Dodaj te pakiety NuGet do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="b1724-130">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="b1724-131">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="b1724-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="b1724-132">Microsoft.AspNet.SignalR.StackExchangeRedis</span><span class="sxs-lookup"><span data-stu-id="b1724-132">Microsoft.AspNet.SignalR.StackExchangeRedis</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. <span data-ttu-id="b1724-133">Tworzenie aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="b1724-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="b1724-134">Dodaj następujący kod do pliku Startup.cs w celu skonfigurowania systemu backplane:</span><span class="sxs-lookup"><span data-stu-id="b1724-134">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="b1724-135">Ubuntu na funkcji Hyper-V</span><span class="sxs-lookup"><span data-stu-id="b1724-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="b1724-136">Za pomocą funkcji Windows Hyper-V, można łatwo utworzyć Maszynę wirtualną Ubuntu w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="b1724-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="b1724-137">Pobierz obraz ISO systemu Ubuntu z [ http://www.ubuntu.com ](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="b1724-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="b1724-138">W funkcji Hyper-V Dodaj nową maszynę Wirtualną.</span><span class="sxs-lookup"><span data-stu-id="b1724-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="b1724-139">W **Podłączanie wirtualnego dysku twardego** kroku, wybierz pozycję **Utwórz wirtualny dysk twardy**.</span><span class="sxs-lookup"><span data-stu-id="b1724-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="b1724-140">W **opcje instalacji** kroku, wybierz pozycję **pliku obrazu (.iso)**, kliknij przycisk **Przeglądaj**, a następnie przejdź do instalacji systemu Ubuntu ISO.</span><span class="sxs-lookup"><span data-stu-id="b1724-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="b1724-141">Zainstaluj usługę Redis</span><span class="sxs-lookup"><span data-stu-id="b1724-141">Install Redis</span></span>

<span data-ttu-id="b1724-142">Wykonaj kroki opisane w temacie [ http://redis.io/download ](http://redis.io/download) do pobrania i Tworzenie pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="b1724-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="b1724-143">To tworzy pliki binarne pamięci podręcznej Redis `src` katalogu.</span><span class="sxs-lookup"><span data-stu-id="b1724-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="b1724-144">Domyślnie usługa Redis nie wymaga hasła.</span><span class="sxs-lookup"><span data-stu-id="b1724-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="b1724-145">Aby ustawić hasło, należy edytować `redis.conf` pliku, który znajduje się w katalogu głównym kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="b1724-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="b1724-146">(Utwórz kopię zapasową pliku, przed przystąpieniem do edytowania)! Dodaj następujące dyrektywy `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="b1724-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="b1724-147">Teraz uruchom serwer Redis:</span><span class="sxs-lookup"><span data-stu-id="b1724-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="b1724-148">Otwarty port 6379, który jest domyślnym portem, który Redis nasłuchuje.</span><span class="sxs-lookup"><span data-stu-id="b1724-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="b1724-149">(Można zmienić numer portu w pliku konfiguracji.)</span><span class="sxs-lookup"><span data-stu-id="b1724-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="b1724-150">Tworzenie aplikacji SignalR</span><span class="sxs-lookup"><span data-stu-id="b1724-150">Create the SignalR Application</span></span>

<span data-ttu-id="b1724-151">Tworzenie aplikacji SignalR, wykonując dowolną z następujących samouczków:</span><span class="sxs-lookup"><span data-stu-id="b1724-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="b1724-152">Wprowadzenie do SignalR w wersji 2.0</span><span class="sxs-lookup"><span data-stu-id="b1724-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="b1724-153">Wprowadzenie do SignalR w wersji 2.0 i MVC 5</span><span class="sxs-lookup"><span data-stu-id="b1724-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="b1724-154">Następnie zmodyfikujemy aplikacji rozmów w celu obsługi skalowania w poziomie przy użyciu usługi Redis.</span><span class="sxs-lookup"><span data-stu-id="b1724-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="b1724-155">Najpierw dodaj `Microsoft.AspNet.SignalR.StackExchangeRedis` pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="b1724-155">First, add the `Microsoft.AspNet.SignalR.StackExchangeRedis` NuGet package to your project.</span></span> <span data-ttu-id="b1724-156">W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="b1724-156">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b1724-157">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b1724-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="b1724-158">Następnie otwórz plik Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="b1724-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="b1724-159">Dodaj następujący kod do **konfiguracji** metody:</span><span class="sxs-lookup"><span data-stu-id="b1724-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="b1724-160">"server" jest nazwą serwera, na którym jest uruchomiona usługa Redis.</span><span class="sxs-lookup"><span data-stu-id="b1724-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="b1724-161">*port* jest numerem portu</span><span class="sxs-lookup"><span data-stu-id="b1724-161">*port* is the port number</span></span>
- <span data-ttu-id="b1724-162">"password" jest hasło, które są zdefiniowane w pliku redis.conf.</span><span class="sxs-lookup"><span data-stu-id="b1724-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="b1724-163">"AppName" jest dowolny ciąg.</span><span class="sxs-lookup"><span data-stu-id="b1724-163">"AppName" is any string.</span></span> <span data-ttu-id="b1724-164">SignalR tworzy kanał Redis pub/sub o tej nazwie.</span><span class="sxs-lookup"><span data-stu-id="b1724-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="b1724-165">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b1724-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="b1724-166">Wdrażanie i uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="b1724-166">Deploy and Run the Application</span></span>

<span data-ttu-id="b1724-167">Przygotowanie do wdrożenia aplikacji SignalR wystąpień systemu Windows Server.</span><span class="sxs-lookup"><span data-stu-id="b1724-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="b1724-168">Dodaj rolę usług IIS.</span><span class="sxs-lookup"><span data-stu-id="b1724-168">Add the IIS role.</span></span> <span data-ttu-id="b1724-169">Obejmują funkcje "Opracowywanie zawartości dla aplikacji", w tym protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b1724-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="b1724-170">Również obejmować usługę zarządzania (wymienione w obszarze "Narzędzia do zarządzania").</span><span class="sxs-lookup"><span data-stu-id="b1724-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="b1724-171">**Zainstaluj narzędzie Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="b1724-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="b1724-172">Podczas uruchamiania Menedżera usług IIS, zostanie wyświetlony monit o zainstalowanie platformy sieci Web firmy Microsoft lub możesz [Pobierz intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="b1724-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="b1724-173">W Instalatorze platformy wyszukiwania dla narzędzia Web Deploy i zainstalować Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="b1724-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="b1724-174">Sprawdź, czy usługa zarządzania sieci Web jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="b1724-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="b1724-175">Jeśli nie, uruchom usługę.</span><span class="sxs-lookup"><span data-stu-id="b1724-175">If not, start the service.</span></span> <span data-ttu-id="b1724-176">(Jeśli nie widzisz usługi zarządzania siecią Web, na liście usług Windows, upewnij się, że po dodaniu roli usług IIS są zainstalowane usługi zarządzania.)</span><span class="sxs-lookup"><span data-stu-id="b1724-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="b1724-177">Domyślnie usługi zarządzania siecią Web nasłuchuje na porcie TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="b1724-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="b1724-178">W Zaporze Windows utwórz nową regułę dla ruchu przychodzącego, aby umożliwić ruch TCP na porcie 8172.</span><span class="sxs-lookup"><span data-stu-id="b1724-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="b1724-179">Aby uzyskać więcej informacji, zobacz [Konfigurowanie reguł zapory](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="b1724-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="b1724-180">(Jeśli są hostingu maszyn wirtualnych na platformie Azure, możesz to zrobić bezpośrednio w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="b1724-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="b1724-181">Zobacz [jak skonfigurować punkty końcowe do maszyny wirtualnej](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="b1724-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="b1724-182">Teraz wszystko jest gotowe do wdrożenia projektu programu Visual Studio z komputera deweloperskiego z serwerem.</span><span class="sxs-lookup"><span data-stu-id="b1724-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="b1724-183">W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="b1724-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="b1724-184">Aby uzyskać bardziej szczegółową dokumentację na temat wdrażania w Internecie, zobacz [zawartości mapy wdrażania w sieci Web dla programu Visual Studio i platformy ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="b1724-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="b1724-185">W przypadku wdrożenia aplikacji na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobacz ich odbierania komunikatów SignalR w innej.</span><span class="sxs-lookup"><span data-stu-id="b1724-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="b1724-186">(Oczywiście w środowisku produkcyjnym, dwa serwery będą znajdują się za modułem równoważenia obciążenia.)</span><span class="sxs-lookup"><span data-stu-id="b1724-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="b1724-187">Jeśli zastanawiasz się wyświetlić komunikaty, które są wysyłane do pamięci Redis, można użyć **redis-cli** klienta, który instaluje przy użyciu usługi Redis.</span><span class="sxs-lookup"><span data-stu-id="b1724-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
