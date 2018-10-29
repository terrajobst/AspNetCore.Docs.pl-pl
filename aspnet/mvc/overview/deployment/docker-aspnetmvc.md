---
uid: mvc/overview/deployment/docker
title: Migrowanie aplikacji ASP.NET MVC do kontenerów systemu Windows
description: Dowiedz się, jak wykonać istniejącą aplikację ASP.NET MVC i uruchomienia jej w kontenerze platformy Docker Windows
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 7b34187747d3081998b8b60a72adae78cafe2c3e
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207969"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a><span data-ttu-id="4ae2d-104">Migrowanie aplikacji ASP.NET MVC do kontenerów systemu Windows</span><span class="sxs-lookup"><span data-stu-id="4ae2d-104">Migrating ASP.NET MVC Applications to Windows Containers</span></span>

<span data-ttu-id="4ae2d-105">Uruchomienie istniejącej aplikacji opartych na programie .NET Framework w kontenerze Windows nie wymaga żadnych zmian w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-105">Running an existing .NET Framework-based application in a Windows container doesn't require any changes to your app.</span></span> <span data-ttu-id="4ae2d-106">Aby uruchomić aplikację w kontenerze Windows tworzenia obrazu Docker zawierającego aplikację i uruchamianie kontenera.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-106">To run your app in a Windows container you create a Docker image containing your app and start the container.</span></span> <span data-ttu-id="4ae2d-107">W tym temacie wyjaśniono, jak wykonać istniejące [aplikacji platformy ASP.NET MVC](http://www.asp.net/mvc) oraz wdrożyć ją w kontenerze Windows.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-107">This topic explains how to take an existing [ASP.NET MVC application](http://www.asp.net/mvc) and deploy it in a Windows container.</span></span>

<span data-ttu-id="4ae2d-108">Możesz zaczynać istniejącą aplikację ASP.NET MVC, a następnie tworzyć zasoby opublikowanych przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-108">You start with an existing ASP.NET MVC app, then build the published assets using Visual Studio.</span></span> <span data-ttu-id="4ae2d-109">Możesz używać platformy Docker do tworzenia obrazu, który zawiera i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-109">You use Docker to create the image that contains and runs your app.</span></span> <span data-ttu-id="4ae2d-110">Będziesz przejdź do witryny, uruchomione w kontenerze Windows i sprawdź, czy aplikacja działa.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-110">You'll browse to the site running in a Windows container and verify the app is working.</span></span>

<span data-ttu-id="4ae2d-111">W tym artykule założono podstawową wiedzę na temat platformy docker.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-111">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="4ae2d-112">Informacje na temat platformy Docker, czytając [Docker — omówienie](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="4ae2d-112">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

<span data-ttu-id="4ae2d-113">Aplikacja, które będą uruchamiane w kontenerze jest prostą witrynę sieci Web, który losowo odpowiedzi na pytania.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-113">The app you'll run in a container is a simple website that answers questions randomly.</span></span> <span data-ttu-id="4ae2d-114">Ta aplikacja to podstawowa aplikacja MVC bez uwierzytelniania lub Magazyn bazy danych; Dzięki temu można skoncentrować się na przejście z warstwy internetowej do kontenera.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-114">This app is a basic MVC application with no authentication or database storage; it lets you focus on moving the web tier to a container.</span></span> <span data-ttu-id="4ae2d-115">Tematy w przyszłości pokazują, jak przenosić i zarządzanie nimi w konteneryzowanych aplikacji z magazynu trwałego.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-115">Future topics will show how to move and manage persistent storage in containerized applications.</span></span>

<span data-ttu-id="4ae2d-116">Przenoszenie aplikacji obejmuje następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="4ae2d-116">Moving your application involves these steps:</span></span>

1. [<span data-ttu-id="4ae2d-117">Tworzenie zadania Publikuj do tworzenia zasobów dla obrazu.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-117">Creating a publish task to build the assets for an image.</span></span>](#publish-script)
1. [<span data-ttu-id="4ae2d-118">Tworzenie obrazu platformy Docker, który uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-118">Building a Docker image that will run your application.</span></span>](#build-the-image)
1. [<span data-ttu-id="4ae2d-119">Uruchamianie kontenera platformy Docker działającą obrazu.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-119">Starting a Docker container that runs your image.</span></span>](#start-a-container)
1. [<span data-ttu-id="4ae2d-120">Sprawdzanie aplikacji za pomocą przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-120">Verifying the application using your browser.</span></span>](#verify-in-the-browser)

<span data-ttu-id="4ae2d-121">[Aplikacja](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) znajduje się w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-121">The [finished application](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) is on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ae2d-122">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="4ae2d-122">Prerequisites</span></span>

<span data-ttu-id="4ae2d-123">Na komputerze deweloperskim musi mieć następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="4ae2d-123">The development machine must have the following software:</span></span>

- <span data-ttu-id="4ae2d-124">[Rocznicowa aktualizacja systemu Windows 10](https://www.microsoft.com/software-download/windows10/) (lub nowszego) lub [systemu Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="4ae2d-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (or higher) or [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (or higher)</span></span>
- <span data-ttu-id="4ae2d-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) -Beta 26 wersja stabilna 1.13.0 lub 1.12 (lub nowsze wersje)</span><span class="sxs-lookup"><span data-stu-id="4ae2d-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - version Stable 1.13.0 or 1.12 Beta 26 (or newer versions)</span></span>
- [<span data-ttu-id="4ae2d-126">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="4ae2d-126">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> <span data-ttu-id="4ae2d-127">Jeśli używasz systemu Windows Server 2016, postępuj zgodnie z instrukcjami dotyczącymi [wdrażania hosta kontenera — system Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span><span class="sxs-lookup"><span data-stu-id="4ae2d-127">If you are using Windows Server 2016, follow the instructions for [Container Host Deployment - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span></span>

<span data-ttu-id="4ae2d-128">Po zainstalowaniu i uruchomieniu programu Docker, kliknij prawym przyciskiem myszy ikonę na pasku zadań i wybierz pozycję **przełączyć się do kontenerów Windows**.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-128">After installing and starting Docker, right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="4ae2d-129">Jest to wymagane do uruchomienia obrazów Docker opartych na Windows.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-129">This is required to run Docker images based on Windows.</span></span> <span data-ttu-id="4ae2d-130">To polecenie zajmuje kilka sekund, aby wykonać:</span><span class="sxs-lookup"><span data-stu-id="4ae2d-130">This command takes a few seconds to execute:</span></span>

<span data-ttu-id="4ae2d-131">![Windows Container][windows-container]</span><span class="sxs-lookup"><span data-stu-id="4ae2d-131">![Windows Container][windows-container]</span></span>

## <a name="publish-script"></a><span data-ttu-id="4ae2d-132">Publikowanie skryptu</span><span class="sxs-lookup"><span data-stu-id="4ae2d-132">Publish script</span></span>

<span data-ttu-id="4ae2d-133">Zbierz wszystkie zasoby, które są potrzebne do załadowania do obrazu platformy Docker w jednym miejscu.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-133">Collect all the assets that you need to load into a Docker image in one place.</span></span> <span data-ttu-id="4ae2d-134">Możesz użyć programu Visual Studio **Publikuj** polecenie, aby utworzyć profil publikowania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-134">You can use the Visual Studio **Publish** command to create a publish profile for your app.</span></span> <span data-ttu-id="4ae2d-135">Ten profil zostanie umieścić wszystkie zasoby w jednym drzewie katalogu, które można skopiować do obrazu docelowego w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-135">This profile will put all the assets in one directory tree that you copy to your target image later in this tutorial.</span></span>

<span data-ttu-id="4ae2d-136">**Kroki publikowania**</span><span class="sxs-lookup"><span data-stu-id="4ae2d-136">**Publish Steps**</span></span>

1. <span data-ttu-id="4ae2d-137">Kliknij prawym przyciskiem myszy projekt sieci web w programie Visual Studio, a następnie wybierz pozycję **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-137">Right click on the web project in Visual Studio, and select **Publish**.</span></span>
1. <span data-ttu-id="4ae2d-138">Kliknij przycisk **przycisk profil niestandardowy**, a następnie wybierz pozycję **System plików** jako metody.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-138">Click the **Custom profile button**, and then select **File System** as the method.</span></span>
1. <span data-ttu-id="4ae2d-139">Wybierz katalog.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-139">Choose the directory.</span></span> <span data-ttu-id="4ae2d-140">Zgodnie z Konwencją pobrane próbki `bin\Release\PublishOutput`.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-140">By convention, the downloaded sample uses `bin\Release\PublishOutput`.</span></span>

<span data-ttu-id="4ae2d-141">![Publikowanie połączenia][publish-connection]</span><span class="sxs-lookup"><span data-stu-id="4ae2d-141">![Publish Connection][publish-connection]</span></span>

<span data-ttu-id="4ae2d-142">Otwórz **opcji publikowania pliku** części **ustawienia** kartę. Wybierz **Precompile podczas publikowania**.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-142">Open the **File Publish Options** section of the **Settings** tab. Select **Precompile during publishing**.</span></span> <span data-ttu-id="4ae2d-143">Tego rodzaju optymalizacji oznacza, że będziesz kompilowanie widoków w kontenerze platformy Docker, kopiowane są wstępnie skompilowane widoków.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-143">This optimization means that you'll be compiling views in the Docker container, you are copying the precompiled views.</span></span>

<span data-ttu-id="4ae2d-144">![Ustawienia publikowania][publish-settings]</span><span class="sxs-lookup"><span data-stu-id="4ae2d-144">![Publish Settings][publish-settings]</span></span>

<span data-ttu-id="4ae2d-145">Kliknij przycisk **Publikuj**, i Visual Studio skopiuje wszystkie zasoby potrzebne do folderu docelowego.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-145">Click **Publish**, and Visual Studio will copy all the needed assets to the destination folder.</span></span>

## <a name="build-the-image"></a><span data-ttu-id="4ae2d-146">Tworzenie obrazu</span><span class="sxs-lookup"><span data-stu-id="4ae2d-146">Build the image</span></span>

<span data-ttu-id="4ae2d-147">Zdefiniuj obraz Docker w pliku Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-147">Define your Docker image in a Dockerfile.</span></span> <span data-ttu-id="4ae2d-148">Plik Dockerfile zawiera instrukcje dotyczące obraz podstawowy, dodatkowe składniki, aplikację, którą chcesz uruchomić i inne obrazy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-148">The Dockerfile contains instructions for the base image, additional components, the app you want to run, and other configuration images.</span></span>  <span data-ttu-id="4ae2d-149">Plik Dockerfile stanowi dane wejściowe `docker build` polecenia, co powoduje utworzenie obrazu.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-149">The Dockerfile is the input to the `docker build` command, which creates the image.</span></span>

<span data-ttu-id="4ae2d-150">Utworzysz obraz na podstawie `microsoft/aspnet` obrazów znajdujących się na [usługi Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span><span class="sxs-lookup"><span data-stu-id="4ae2d-150">You will build an image based on the `microsoft/aspnet` image located on [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span></span>
<span data-ttu-id="4ae2d-151">Obraz podstawowy `microsoft/aspnet`, jest obrazem systemu Windows Server.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-151">The base image, `microsoft/aspnet`, is a Windows Server image.</span></span> <span data-ttu-id="4ae2d-152">Zawiera on Windows Server Core, IIS i platformy ASP.NET 4.7.2.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-152">It contains Windows Server Core, IIS, and ASP.NET 4.7.2.</span></span> <span data-ttu-id="4ae2d-153">Po uruchomieniu tego obrazu w kontenerze, nastąpi automatyczne uruchomienie usług IIS i zainstalowanych witryn sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-153">When you run this image in your container, it will automatically start IIS and installed websites.</span></span>

<span data-ttu-id="4ae2d-154">Plik Dockerfile, który tworzy obraz wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="4ae2d-154">The Dockerfile that creates your image looks like this:</span></span>

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

<span data-ttu-id="4ae2d-155">Istnieje nie `ENTRYPOINT` polecenia w tym pliku Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-155">There is no `ENTRYPOINT` command in this Dockerfile.</span></span> <span data-ttu-id="4ae2d-156">Nie potrzebujesz.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-156">You don't need one.</span></span> <span data-ttu-id="4ae2d-157">Podczas uruchamiania systemu Windows Server z usługami IIS, proces IIS jest punkt wejścia, który jest skonfigurowany do uruchamiania w obrazie podstawowym aspnet.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-157">When running Windows Server with IIS, the IIS process is the entrypoint, which is configured to start in the aspnet base image.</span></span>

<span data-ttu-id="4ae2d-158">Uruchom polecenie kompilacji platformy Docker, aby utworzyć obraz uruchamiający aplikację platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-158">Run the Docker build command to create the image that runs your ASP.NET app.</span></span> <span data-ttu-id="4ae2d-159">Aby to zrobić, Otwórz okno programu PowerShell w katalogu projektu i wpisz następujące polecenie w katalogu rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="4ae2d-159">To do this, open a PowerShell window in the directory of your project and type the following command in the solution directory:</span></span>

```console
docker build -t mvcrandomanswers .
```

<span data-ttu-id="4ae2d-160">To polecenie utworzy nowy obraz zgodnie z instrukcjami z pliku Dockerfile nazewnictwa (-t znakowanie) obraz jako mvcrandomanswers.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-160">This command will build the new image using the instructions in your Dockerfile, naming (-t tagging) the image as mvcrandomanswers.</span></span> <span data-ttu-id="4ae2d-161">Może to obejmować ściąganie obrazu podstawowego z [usługi Docker Hub](http://hub.docker.com), a następnie dodanie aplikacji do tego obrazu.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-161">This may include pulling the base image from [Docker Hub](http://hub.docker.com), and then adding your app to that image.</span></span>

<span data-ttu-id="4ae2d-162">Po wykonaniu tego polecenia można uruchomić `docker images` polecenie, aby wyświetlić informacje o nowym obrazie:</span><span class="sxs-lookup"><span data-stu-id="4ae2d-162">Once that command completes, you can run the `docker images` command to see information on the new image:</span></span>

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

<span data-ttu-id="4ae2d-163">Identyfikator obrazu będzie różna na swojej maszynie.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-163">The IMAGE ID will be different on your machine.</span></span> <span data-ttu-id="4ae2d-164">Teraz uruchommy aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-164">Now, let's run the app.</span></span>

## <a name="start-a-container"></a><span data-ttu-id="4ae2d-165">Uruchamianie kontenera</span><span class="sxs-lookup"><span data-stu-id="4ae2d-165">Start a container</span></span>

<span data-ttu-id="4ae2d-166">Uruchamianie kontenera, wykonując następujące `docker run` polecenia:</span><span class="sxs-lookup"><span data-stu-id="4ae2d-166">Start a container by executing the following `docker run` command:</span></span>

```console
docker run -d --name randomanswers mvcrandomanswers
```

<span data-ttu-id="4ae2d-167">`-d` Argument nakazuje platformy Docker można uruchomić obrazu w trybie odłączonym.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-167">The `-d` argument tells Docker to start the image in detached mode.</span></span> <span data-ttu-id="4ae2d-168">Oznacza to, że obraz platformy Docker działa bez połączenia z bieżącej powłoce.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-168">That means the Docker image runs disconnected from the current shell.</span></span>

<span data-ttu-id="4ae2d-169">W wielu przykładach platformy docker może zostać wyświetlony -p do mapowania portów kontenera i hosta.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-169">In many docker examples, you may see -p to map the container and host ports.</span></span> <span data-ttu-id="4ae2d-170">Domyślny obraz aspnet skonfigurował już kontener do nasłuchiwania na porcie 80 i udostępnić ją.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-170">The default aspnet image has already configured the container to listen on port 80 and expose it.</span></span>

<span data-ttu-id="4ae2d-171">`--name randomanswers` Umożliwia nadanie nazwy działającemu kontenerowi.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-171">The `--name randomanswers` gives a name to the running container.</span></span> <span data-ttu-id="4ae2d-172">Można używać tej nazwy zamiast Identyfikatora kontenera, w większości poleceń.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-172">You can use this name instead of the container ID in most commands.</span></span>

<span data-ttu-id="4ae2d-173">`mvcrandomanswers` Nazywa się obraz do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-173">The `mvcrandomanswers` is the name of the image to start.</span></span>

## <a name="verify-in-the-browser"></a><span data-ttu-id="4ae2d-174">Sprawdź, czy w przeglądarce</span><span class="sxs-lookup"><span data-stu-id="4ae2d-174">Verify in the browser</span></span>

<span data-ttu-id="4ae2d-175">Po uruchomieniu kontenera, połączyć się z działającego kontenera `http://localhost` w przykładzie przedstawionym.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-175">Once the container starts, connect to the running container using `http://localhost` in the example shown.</span></span> <span data-ttu-id="4ae2d-176">Typ tego adresu URL w przeglądarce i powinien zostać wyświetlony działającej witryny.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-176">Type that URL into your browser, and you should see the running site.</span></span>

> [!NOTE]
> <span data-ttu-id="4ae2d-177">Niektóre programy sieci VPN lub serwer proxy może uniemożliwić przejście do witryny.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-177">Some VPN or proxy software may prevent you from navigating to your site.</span></span>
> <span data-ttu-id="4ae2d-178">Można tymczasowo wyłączyć, aby upewnić się, że kontener działa.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-178">You can temporarily disable it to make sure your container is working.</span></span>

<span data-ttu-id="4ae2d-179">Zawiera katalog przykładu w usłudze GitHub [skrypt programu PowerShell](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) , który jest wykonywany tych poleceń dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-179">The sample directory on GitHub contains a [PowerShell script](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) that executes these commands for you.</span></span> <span data-ttu-id="4ae2d-180">Otwórz okno programu PowerShell, zmień katalog na katalog rozwiązania i wpisz:</span><span class="sxs-lookup"><span data-stu-id="4ae2d-180">Open a PowerShell window, change directory to your solution directory, and type:</span></span>

```console
./run.ps1
```

<span data-ttu-id="4ae2d-181">Powyższe polecenie tworzy obraz, wyświetla listę obrazów na Twojej maszynie i uruchamia kontener.</span><span class="sxs-lookup"><span data-stu-id="4ae2d-181">The command above builds the image, displays the list of images on your machine, and starts a container.</span></span>

<span data-ttu-id="4ae2d-182">Aby zatrzymać kontener, należy wydać `docker stop` polecenia:</span><span class="sxs-lookup"><span data-stu-id="4ae2d-182">To stop your container, issue a `docker stop` command:</span></span>

```console
docker stop randomanswers
```

<span data-ttu-id="4ae2d-183">Aby usunąć kontener, należy wydać `docker rm` polecenia:</span><span class="sxs-lookup"><span data-stu-id="4ae2d-183">To remove the container, issue a `docker rm` command:</span></span>

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Przełącz się do kontenerów Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Publikowanie do systemu plików"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Ustawienia publikowania"
