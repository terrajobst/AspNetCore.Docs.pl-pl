---
uid: mvc/overview/deployment/docker
title: "Migrowanie aplikacji ASP.NET MVC do kontenerów systemu Windows"
description: "Dowiedz się, jak wykonać istniejącej aplikacji ASP.NET MVC i uruchom go w kontenerze Docker systemu Windows"
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.topic: article
ms.prod: .net-framework
ms.technology: dotnet-mvc
ms.devlang: dotnet
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 7a580c6c6236b375ea54ef4e9978fff6993d885a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/11/2018
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a><span data-ttu-id="fcc8a-104">Migrowanie aplikacji ASP.NET MVC do kontenerów systemu Windows</span><span class="sxs-lookup"><span data-stu-id="fcc8a-104">Migrating ASP.NET MVC Applications to Windows Containers</span></span>

<span data-ttu-id="fcc8a-105">Uruchamianie istniejących aplikacji opartych na programie .NET Framework w kontenerze systemu Windows nie wymaga żadnych zmian do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-105">Running an existing .NET Framework-based application in a Windows container doesn't require any changes to your app.</span></span> <span data-ttu-id="fcc8a-106">Aby uruchomić aplikację w kontenerze systemu Windows utworzyć obraz Docker zawierający aplikację i uruchomić kontenera.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-106">To run your app in a Windows container you create a Docker image containing your app and start the container.</span></span> <span data-ttu-id="fcc8a-107">W tym temacie wyjaśniono, jak wykonać istniejące [aplikacji ASP.NET MVC](http://www.asp.net/mvc) i wdrożyć ją w kontenerze systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-107">This topic explains how to take an existing [ASP.NET MVC application](http://www.asp.net/mvc) and deploy it in a Windows container.</span></span>

<span data-ttu-id="fcc8a-108">Można rozpoczynać się od istniejącej aplikacji ASP.NET MVC, a następnie utworzyć zasoby opublikowanych przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-108">You start with an existing ASP.NET MVC app, then build the published assets using Visual Studio.</span></span> <span data-ttu-id="fcc8a-109">Docker służy do tworzenia obrazu, który zawiera i uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-109">You use Docker to create the image that contains and runs your app.</span></span> <span data-ttu-id="fcc8a-110">Będzie przejdź do witryny uruchomione w kontenerze systemu Windows i sprawdź, czy aplikacja działa.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-110">You'll browse to the site running in a Windows container and verify the app is working.</span></span>

<span data-ttu-id="fcc8a-111">W tym artykule przyjęto założenie, podstawową wiedzę na temat programu Docker.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-111">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="fcc8a-112">Informacje na temat rozwiązania Docker, odczytując [omówienie Docker](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="fcc8a-112">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

<span data-ttu-id="fcc8a-113">Aplikacja zostanie uruchomione w kontenerze jest prosty witryny sieci Web, który losowo odpowiedzi na pytania.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-113">The app you'll run in a container is a simple website that answers questions randomly.</span></span> <span data-ttu-id="fcc8a-114">Ta aplikacja jest podstawowej aplikacji MVC bez uwierzytelniania lub magazynu bazy danych; Dzięki temu można skupić się na przenoszenie warstwa sieci web do kontenera.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-114">This app is a basic MVC application with no authentication or database storage; it lets you focus on moving the web tier to a container.</span></span> <span data-ttu-id="fcc8a-115">Tematy przyszłych pokazują, jak przenieść i zarządzanie nimi magazynu trwałego w aplikacjach konteneryzowanych.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-115">Future topics will show how to move and manage persistent storage in containerized applications.</span></span>

<span data-ttu-id="fcc8a-116">Przenoszenie aplikacji obejmuje następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="fcc8a-116">Moving your application involves these steps:</span></span>

1. [<span data-ttu-id="fcc8a-117">Tworzenie zadania publikowania do tworzenia zasobów dla obrazu.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-117">Creating a publish task to build the assets for an image.</span></span>](#publish-script)
1. [<span data-ttu-id="fcc8a-118">Budowanie Docker obrazu, który uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-118">Building a Docker image that will run your application.</span></span>](#build-the-image)
1. [<span data-ttu-id="fcc8a-119">Uruchamianie kontener Docker, który uruchamia obrazu.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-119">Starting a Docker container that runs your image.</span></span>](#start-a-container)
1. [<span data-ttu-id="fcc8a-120">Weryfikowanie aplikacji za pomocą przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-120">Verifying the application using your browser.</span></span>](#verify-in-the-browser)

<span data-ttu-id="fcc8a-121">[Zakończono aplikacji](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) znajduje się w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-121">The [finished application](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) is on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fcc8a-122">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="fcc8a-122">Prerequisites</span></span>

<span data-ttu-id="fcc8a-123">Musi być uruchomiona na komputerze deweloperskim</span><span class="sxs-lookup"><span data-stu-id="fcc8a-123">The development machine must be running</span></span>

- <span data-ttu-id="fcc8a-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (lub nowszej) lub [systemu Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="fcc8a-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (or higher) or [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (or higher).</span></span>
- <span data-ttu-id="fcc8a-125">[Docker dla systemu Windows](https://docs.docker.com/docker-for-windows/) -26 w wersji Beta w wersji stabilnej 1.13.0 lub 1.12 (lub nowsze wersje)</span><span class="sxs-lookup"><span data-stu-id="fcc8a-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - version Stable 1.13.0 or 1.12 Beta 26 (or newer versions)</span></span>
- <span data-ttu-id="fcc8a-126">[Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="fcc8a-126">[Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fcc8a-127">Jeśli używasz systemu Windows Server 2016, postępuj zgodnie z instrukcjami dotyczącymi [wdrażania hosta kontenera - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span><span class="sxs-lookup"><span data-stu-id="fcc8a-127">If you are using Windows Server 2016, follow the instructions for [Container Host Deployment - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span></span>

<span data-ttu-id="fcc8a-128">Po instalacji i uruchamiania Docker, kliknij prawym przyciskiem myszy ikonę na pasku zadań i wybierz **przełączyć się do systemu Windows kontenery**.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-128">After installing and starting Docker,  right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="fcc8a-129">Jest to wymagane do uruchomienia obrazy usługi Docker opartych na systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-129">This is required to run Docker images based on Windows.</span></span> <span data-ttu-id="fcc8a-130">To polecenie zajmuje kilka sekund do wykonania:</span><span class="sxs-lookup"><span data-stu-id="fcc8a-130">This command takes a few seconds to execute:</span></span>

<span data-ttu-id="fcc8a-131">![Windows Container][windows-container]</span><span class="sxs-lookup"><span data-stu-id="fcc8a-131">![Windows Container][windows-container]</span></span>

## <a name="publish-script"></a><span data-ttu-id="fcc8a-132">Publikowanie skryptu</span><span class="sxs-lookup"><span data-stu-id="fcc8a-132">Publish script</span></span>

<span data-ttu-id="fcc8a-133">Zbieraj wszystkie zasoby, które należy załadować do obrazu platformy Docker w jednym miejscu.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-133">Collect all the assets that you need to load into a Docker image in one place.</span></span> <span data-ttu-id="fcc8a-134">Można użyć programu Visual Studio **publikowania** polecenie, aby utworzyć profil publikowania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-134">You can use the Visual Studio **Publish** command to create a publish profile for your app.</span></span> <span data-ttu-id="fcc8a-135">Ten profil zostanie Umieść wszystkie zasoby w jednym drzewie katalogu skopiowane na obrazie docelowego w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-135">This profile will put all the assets in one directory tree that you copy to your target image later in this tutorial.</span></span>

<span data-ttu-id="fcc8a-136">**Kroki publikowania**</span><span class="sxs-lookup"><span data-stu-id="fcc8a-136">**Publish Steps**</span></span>

1. <span data-ttu-id="fcc8a-137">Kliknij prawym przyciskiem myszy projekt sieci web w programie Visual Studio i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-137">Right click on the web project in Visual Studio, and select **Publish**.</span></span>
1. <span data-ttu-id="fcc8a-138">Kliknij przycisk **przycisk profil niestandardowy**, a następnie wybierz **systemu plików** jako metodę.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-138">Click the **Custom profile button**, and then select **File System** as the method.</span></span>
1. <span data-ttu-id="fcc8a-139">Wybierz katalog.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-139">Choose the directory.</span></span> <span data-ttu-id="fcc8a-140">Konwencja, używa pobrane próbki `bin\Release\PublishOutput`.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-140">By convention, the downloaded sample uses `bin\Release\PublishOutput`.</span></span>

<span data-ttu-id="fcc8a-141">![Publikowanie połączenia][publish-connection]</span><span class="sxs-lookup"><span data-stu-id="fcc8a-141">![Publish Connection][publish-connection]</span></span>

<span data-ttu-id="fcc8a-142">Otwórz **opcji publikowania pliku** sekcji **ustawienia** kartę. Wybierz **Precompile podczas publikowania**.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-142">Open the **File Publish Options** section of the **Settings** tab. Select **Precompile during publishing**.</span></span> <span data-ttu-id="fcc8a-143">Tego rodzaju optymalizacji oznacza, że będzie kompilowanie widoków w kontenerze Docker, kopiowane są wstępnie skompilowanym widoków.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-143">This optimization means that you'll be compiling views in the Docker container, you are copying the precompiled views.</span></span>

<span data-ttu-id="fcc8a-144">![Ustawienia publikowania][publish-settings]</span><span class="sxs-lookup"><span data-stu-id="fcc8a-144">![Publish Settings][publish-settings]</span></span>

<span data-ttu-id="fcc8a-145">Kliknij przycisk **publikowania**, i Visual Studio kopiuje wymagane zasoby do folderu docelowego.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-145">Click **Publish**, and Visual Studio will copy all the needed assets to the destination folder.</span></span>

## <a name="build-the-image"></a><span data-ttu-id="fcc8a-146">Tworzenie obrazu</span><span class="sxs-lookup"><span data-stu-id="fcc8a-146">Build the image</span></span>

<span data-ttu-id="fcc8a-147">Plik Dockerfile do definiowania obrazu Docker.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-147">Define your Docker image in a Dockerfile.</span></span> <span data-ttu-id="fcc8a-148">Plik Dockerfile zawiera instrukcje dotyczące obrazu podstawowego, dodatkowe składniki aplikacji, które mają zostać uruchomione i inne obrazy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-148">The Dockerfile contains instructions for the base image, additional components, the app you want to run, and other configuration images.</span></span>  <span data-ttu-id="fcc8a-149">W danych wejściowych jest plik Dockerfile `docker build` polecenia, które tworzy obraz.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-149">The Dockerfile is the input to the `docker build` command, which creates the image.</span></span>

<span data-ttu-id="fcc8a-150">Zostanie utworzona na podstawie obrazu `microsoft/aspnet` obrazu, który znajduje się na [Centrum Docker](https://hub.docker.com/r/microsoft/aspnet/).</span><span class="sxs-lookup"><span data-stu-id="fcc8a-150">You will build an image based on the `microsoft/aspnet` image located on [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span></span>
<span data-ttu-id="fcc8a-151">Podstawowy obraz `microsoft/aspnet`, jest obrazem systemu Windows Server.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-151">The base image, `microsoft/aspnet`, is a Windows Server image.</span></span> <span data-ttu-id="fcc8a-152">Zawiera on Windows Server Core, IIS i platformy ASP.NET 4.6.2.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-152">It contains Windows Server Core, IIS and ASP.NET 4.6.2.</span></span> <span data-ttu-id="fcc8a-153">Po uruchomieniu tego obrazu w kontenerze użytkownika zostaną automatycznie uruchomione usługi IIS i zainstalowane witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-153">When you run this image in your container, it will automatically start IIS and installed websites.</span></span>

<span data-ttu-id="fcc8a-154">Plik Dockerfile, która tworzy obraz wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="fcc8a-154">The Dockerfile that creates your image looks like this:</span></span>

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

<span data-ttu-id="fcc8a-155">Brak nie `ENTRYPOINT` w tym plik Dockerfile polecenie.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-155">There is no `ENTRYPOINT` command in this Dockerfile.</span></span> <span data-ttu-id="fcc8a-156">Nie można je utworzyć.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-156">You don't need one.</span></span> <span data-ttu-id="fcc8a-157">Podczas uruchamiania systemu Windows Server z usługami IIS, proces programu IIS jest punktu wejścia, który jest skonfigurowany do uruchamiania w aspnet obrazu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-157">When running Windows Server with IIS, the IIS process is the entrypoint, which is configured to start in the aspnet base image.</span></span>

<span data-ttu-id="fcc8a-158">Uruchom polecenie kompilacji Docker do utworzenia obrazu, który uruchamia aplikację ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-158">Run the Docker build command to create the image that runs your ASP.NET app.</span></span> <span data-ttu-id="fcc8a-159">Aby to zrobić, Otwórz okno programu PowerShell w katalogu projektu i wpisz następujące polecenie w katalogu rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="fcc8a-159">To do this, open a PowerShell window in the directory of your project and type the following command in the solution directory:</span></span>

```console
docker build -t mvcrandomanswers .
```

<span data-ttu-id="fcc8a-160">To polecenie utworzy nowy obraz, korzystając z instrukcji w Twojej plik Dockerfile, nazewnictwa (-t znakowanie) jako mvcrandomanswers obrazu.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-160">This command will build the new image using the instructions in your Dockerfile, naming (-t tagging) the image as mvcrandomanswers.</span></span> <span data-ttu-id="fcc8a-161">Może to obejmować ściąganie podstawowy obraz z [Centrum Docker](http://hub.docker.com), a następnie dodanie aplikacji do tego obrazu.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-161">This may include pulling the base image from [Docker Hub](http://hub.docker.com), and then adding your app to that image.</span></span>

<span data-ttu-id="fcc8a-162">Po zakończeniu wykonywania tego polecenia, można uruchomić `docker images` polecenie, aby wyświetlić informacje na nowy obraz:</span><span class="sxs-lookup"><span data-stu-id="fcc8a-162">Once that command completes, you can run the `docker images` command to see information on the new image:</span></span>

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

<span data-ttu-id="fcc8a-163">Identyfikator obrazu będzie różna na tym komputerze.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-163">The IMAGE ID will be different on your machine.</span></span> <span data-ttu-id="fcc8a-164">Teraz umożliwia uruchamianie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-164">Now, let's run the app.</span></span>

## <a name="start-a-container"></a><span data-ttu-id="fcc8a-165">Uruchom kontenera</span><span class="sxs-lookup"><span data-stu-id="fcc8a-165">Start a container</span></span>

<span data-ttu-id="fcc8a-166">Uruchom kontener, wykonując następujące `docker run` polecenia:</span><span class="sxs-lookup"><span data-stu-id="fcc8a-166">Start a container by executing the following `docker run` command:</span></span>

```console
docker run -d --name randomanswers mvcrandomanswers
```

<span data-ttu-id="fcc8a-167">`-d` Argument nakazuje Docker można uruchomić obrazu w trybie odłączony.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-167">The `-d` argument tells Docker to start the image in detached mode.</span></span> <span data-ttu-id="fcc8a-168">Oznacza to, że obraz Docker działa bez połączenia z bieżącym powłoki.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-168">That means the Docker image runs disconnected from the current shell.</span></span>

<span data-ttu-id="fcc8a-169">W wielu przykłady docker może wystąpić -p mapowania portów kontenera i hosta.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-169">In many docker examples, you may see -p to map the container and host ports.</span></span> <span data-ttu-id="fcc8a-170">Domyślny obraz aspnet skonfigurował już kontenera do nasłuchiwania na porcie 80 i uwidacznia go.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-170">The default aspnet image has already configured the container to listen on port 80 and expose it.</span></span> 

<span data-ttu-id="fcc8a-171">`--name randomanswers` Nadaje nazwę do uruchomionej kontenera.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-171">The `--name randomanswers` gives a name to the running container.</span></span> <span data-ttu-id="fcc8a-172">Zamiast Identyfikatora kontenera w większości poleceń można używać tej nazwy.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-172">You can use this name instead of the container ID in most commands.</span></span>

<span data-ttu-id="fcc8a-173">`mvcrandomanswers` Jest nazwą obraz do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-173">The `mvcrandomanswers` is the name of the image to start.</span></span>

## <a name="verify-in-the-browser"></a><span data-ttu-id="fcc8a-174">Sprawdź, czy w przeglądarce</span><span class="sxs-lookup"><span data-stu-id="fcc8a-174">Verify in the browser</span></span>

> [!NOTE]
> <span data-ttu-id="fcc8a-175">W bieżącej wersji kontenera systemu Windows nie może przejść do `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-175">With the current Windows Container release, you can't browse to `http://localhost`.</span></span>
> <span data-ttu-id="fcc8a-176">Jest to znane zachowanie w funkcję WinNAT i zostanie on rozwiązany w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-176">This is a known behavior in WinNAT, and it will be resolved in the future.</span></span> <span data-ttu-id="fcc8a-177">Do czasu, który zostanie rozwiązana, należy użyć adresu IP kontenera.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-177">Until that is addressed, you need to use the IP address of the container.</span></span>

<span data-ttu-id="fcc8a-178">Po uruchomieniu kontenera, Znajdź swój adres IP, dzięki czemu możesz nawiązać połączenie z kontenera uruchomiona w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="fcc8a-178">Once the container starts, find its IP address so that you can connect to your running container from a browser:</span></span>

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

<span data-ttu-id="fcc8a-179">Połączyć się z kontenerem uruchomiona, przy użyciu adresu IPv4 `http://172.31.194.61` w przykładzie.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-179">Connect to the running container using the IPv4 address, `http://172.31.194.61` in the example shown.</span></span> <span data-ttu-id="fcc8a-180">Typ tego adresu URL do przeglądarki, a powinien zostać wyświetlony uruchomionej witryny.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-180">Type that URL into your browser, and you should see the running site.</span></span>

> [!NOTE]
> <span data-ttu-id="fcc8a-181">Niektóre programy sieci VPN lub serwer proxy może uniemożliwić przejście do witryny.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-181">Some VPN or proxy software may prevent you from navigating to your site.</span></span>
> <span data-ttu-id="fcc8a-182">Można tymczasowo wyłączyć, aby upewnić się, że działa z kontenera.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-182">You can temporarily disable it to make sure your container is working.</span></span>

<span data-ttu-id="fcc8a-183">Zawiera katalog przykładu w serwisie GitHub [skrypt programu PowerShell](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) , który jest wykonywany tych poleceń dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-183">The sample directory on GitHub contains a [PowerShell script](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) that executes these commands for you.</span></span> <span data-ttu-id="fcc8a-184">Otwórz okno programu PowerShell, zmień katalog na katalog rozwiązania i wpisz:</span><span class="sxs-lookup"><span data-stu-id="fcc8a-184">Open a PowerShell window, change directory to your solution directory, and type:</span></span>

```console
./run.ps1
```

<span data-ttu-id="fcc8a-185">Powyższe polecenie tworzy obraz, umożliwia wyświetlenie listy obrazów na komputerze, rozpoczyna się kontener i wyświetla adres IP dla tego kontenera.</span><span class="sxs-lookup"><span data-stu-id="fcc8a-185">The command above builds the image, displays the list of images on your machine, starts a container, and displays the IP address for that container.</span></span>

<span data-ttu-id="fcc8a-186">Aby zatrzymać z kontenera, należy wydać `docker
stop` polecenia:</span><span class="sxs-lookup"><span data-stu-id="fcc8a-186">To stop your container, issue a `docker
stop` command:</span></span>

```console
docker stop randomanswers
```

<span data-ttu-id="fcc8a-187">Aby usunąć kontenera, należy wydać `docker rm` polecenia:</span><span class="sxs-lookup"><span data-stu-id="fcc8a-187">To remove the container, issue a `docker rm` command:</span></span>

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Przełącz się do kontenera systemu Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Publikowanie do systemu plików"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Ustawienia publikowania"
