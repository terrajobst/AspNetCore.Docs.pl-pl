---
title: Visual Studio Tools for Docker z platformą ASP.NET Core
author: spboyer
description: Dowiedz się, jak za pomocą narzędzi Visual Studio 2017 i platformy Docker for Windows konteneryzowanie aplikacji ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/18/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: afa7b05820ba021c50d9c23804095f7edd8b71f1
ms.sourcegitcommit: ee2b26c7d08b38c908c668522554b52ab8efa221
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146887"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="e1892-103">Visual Studio Tools for Docker z platformą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1892-103">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="e1892-104">[Program Visual Studio 2017](https://www.visualstudio.com/) obsługuje kompilowania, debugowania i uruchamiania konteneryzowanych platformy ASP.NET Core z aplikacji przeznaczonych dla platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e1892-104">[Visual Studio 2017](https://www.visualstudio.com/) supports building, debugging, and running containerized ASP.NET Core apps targeting .NET Core.</span></span> <span data-ttu-id="e1892-105">Są obsługiwane kontenerów systemów Windows i Linux.</span><span class="sxs-lookup"><span data-stu-id="e1892-105">Both Windows and Linux containers are supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1892-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e1892-106">Prerequisites</span></span>

* <span data-ttu-id="e1892-107">[Program Visual Studio 2017](https://www.visualstudio.com/) z **programowanie dla wielu platform .NET Core** obciążenia</span><span class="sxs-lookup"><span data-stu-id="e1892-107">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>
* [<span data-ttu-id="e1892-108">Docker for Windows</span><span class="sxs-lookup"><span data-stu-id="e1892-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="e1892-109">Instalacja i Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="e1892-109">Installation and setup</span></span>

<span data-ttu-id="e1892-110">Instalacja platformy Docker, zapoznaj się z informacjami o [Docker for Windows: co należy wiedzieć przed zainstalowaniem](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) i zainstaluj [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="e1892-110">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="e1892-111">**[Udostępnione dyski](https://docs.docker.com/docker-for-windows/#shared-drives)**  w Docker for Windows musi być skonfigurowana do obsługi mapowania woluminu i debugowania.</span><span class="sxs-lookup"><span data-stu-id="e1892-111">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="e1892-112">Kliknij prawym przyciskiem myszy ikonę platformy Docker w zasobniku systemowym, wybierz **ustawień...** i wybierz **udostępnione dyski**.</span><span class="sxs-lookup"><span data-stu-id="e1892-112">Right-click the System Tray's Docker icon, select **Settings...**, and select **Shared Drives**.</span></span> <span data-ttu-id="e1892-113">Wybierz dysk, na którym Docker przechowuje pliki.</span><span class="sxs-lookup"><span data-stu-id="e1892-113">Select the drive where Docker stores files.</span></span> <span data-ttu-id="e1892-114">Wybierz **zastosować**.</span><span class="sxs-lookup"><span data-stu-id="e1892-114">Select **Apply**.</span></span>

![Dyski udostępnione](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="e1892-116">Visual Studio 2017 w wersji 15.6 lub nowszej monitowanie, gdy **udostępnione dyski** nie są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="e1892-116">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-docker-support-to-an-app"></a><span data-ttu-id="e1892-117">Dodaj obsługę platformy Docker do aplikacji</span><span class="sxs-lookup"><span data-stu-id="e1892-117">Add Docker support to an app</span></span>

<span data-ttu-id="e1892-118">Aby dodać obsługę platformy Docker do projektu programu ASP.NET Core, projekt musi przeznaczony dla platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e1892-118">To add Docker support to an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="e1892-119">Są obsługiwane kontenerów systemów Linux i Windows.</span><span class="sxs-lookup"><span data-stu-id="e1892-119">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="e1892-120">Podczas dodawania obsługi programu Docker do projektu, wybierz kontener systemu Linux lub Windows.</span><span class="sxs-lookup"><span data-stu-id="e1892-120">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="e1892-121">Hosta platformy Docker musi działać ten sam typ kontenera.</span><span class="sxs-lookup"><span data-stu-id="e1892-121">The Docker host must be running the same container type.</span></span> <span data-ttu-id="e1892-122">Aby zmienić typ kontenera, na uruchomione wystąpienie platformy Docker, kliknij prawym przyciskiem myszy ikonę platformy Docker w zasobniku systemowym, a następnie wybierz **przełączyć się do kontenerów Windows...**  lub **przełączyć się do kontenerów systemu Linux...** .</span><span class="sxs-lookup"><span data-stu-id="e1892-122">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="e1892-123">Nowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="e1892-123">New app</span></span>

<span data-ttu-id="e1892-124">Podczas tworzenia nowej aplikacji za pomocą **aplikacji sieci Web programu ASP.NET Core** szablonów projektu wybierz **włączyć obsługę platformy Docker** pole wyboru:</span><span class="sxs-lookup"><span data-stu-id="e1892-124">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** check box:</span></span>

![Zaznacz pole wyboru obsługę platformy Docker](visual-studio-tools-for-docker/_static/enable-docker-support-check box.png)

<span data-ttu-id="e1892-126">W przypadku platformy docelowej platformy .NET Core **OS** listy rozwijanej umożliwia wybór typu kontenera.</span><span class="sxs-lookup"><span data-stu-id="e1892-126">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="e1892-127">Istniejąca aplikacja</span><span class="sxs-lookup"><span data-stu-id="e1892-127">Existing app</span></span>

<span data-ttu-id="e1892-128">Visual Studio Tools for Docker nie jest obsługiwane dodawanie platformy Docker do istniejącego projektu platformy ASP.NET Core przeznaczone dla .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e1892-128">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span> <span data-ttu-id="e1892-129">Dla projektów ASP.NET Core przeznaczone dla platformy .NET Core istnieją dwa sposoby dodawania obsługę platformy Docker za pomocą narzędzi.</span><span class="sxs-lookup"><span data-stu-id="e1892-129">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="e1892-130">Otwórz projekt w programie Visual Studio i wybierz jedną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="e1892-130">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="e1892-131">Wybierz **obsługę platformy Docker** z **projektu** menu.</span><span class="sxs-lookup"><span data-stu-id="e1892-131">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="e1892-132">Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Dodaj** > **obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="e1892-132">Right-click the project in Solution Explorer and select **Add** > **Docker Support**.</span></span>

## <a name="docker-assets-overview"></a><span data-ttu-id="e1892-133">Przegląd zasobów platformy docker</span><span class="sxs-lookup"><span data-stu-id="e1892-133">Docker assets overview</span></span>

<span data-ttu-id="e1892-134">Dodaj program Visual Studio Tools for Docker *docker-compose* projekt do rozwiązania zawierającego następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="e1892-134">The Visual Studio Tools for Docker add a *docker-compose* project to the solution containing the following files:</span></span>

* <span data-ttu-id="e1892-135">*.dockerignore*: zawiera listę wzorców plików i katalogów, które mają zostać wykluczone podczas generowania kontekstu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e1892-135">*.dockerignore*: Contains a list of file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="e1892-136">*docker-compose.yml*: Podstawa [narzędzia Docker Compose](https://docs.docker.com/compose/overview/) umożliwiają zdefiniowanie kolekcji obrazów można skompilować i uruchomić przy użyciu pliku `docker-compose build` i `docker-compose run`odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="e1892-136">*docker-compose.yml*: The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images to be built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="e1892-137">*docker-compose.override.yml*: opcjonalny plik odczytany przez narzędzia Docker Compose, zawierający konfigurację wartości zastąpień dla usług.</span><span class="sxs-lookup"><span data-stu-id="e1892-137">*docker-compose.override.yml*: An optional file, read by Docker Compose, containing configuration overrides for services.</span></span> <span data-ttu-id="e1892-138">Visual Studio wykonuje `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` do scalenia tych plików.</span><span class="sxs-lookup"><span data-stu-id="e1892-138">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="e1892-139">A *pliku Dockerfile*, przepisu do utworzenia końcowej obrazu platformy Docker, zostanie dodany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="e1892-139">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="e1892-140">Zapoznaj się [odwołanie do pliku Dockerfile](https://docs.docker.com/engine/reference/builder/) dla zrozumienia poleceń w nim.</span><span class="sxs-lookup"><span data-stu-id="e1892-140">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="e1892-141">Tej konkretnej *pliku Dockerfile* używa [kompilacji wieloetapowych](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) zawierający cztery różne, o nazwie etapy kompilacji generują:</span><span class="sxs-lookup"><span data-stu-id="e1892-141">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) containing four distinct, named build stages:</span></span>

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

<span data-ttu-id="e1892-142">*Pliku Dockerfile* opiera się na [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) obrazu.</span><span class="sxs-lookup"><span data-stu-id="e1892-142">The *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="e1892-143">Tego obrazu podstawowego obejmuje pakiety platformy ASP.NET Core NuGet, które zostały wstępnie trybie JIT na zwiększeniu wydajności uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="e1892-143">This base image includes the ASP.NET Core NuGet packages, which have been pre-jitted to improve startup performance.</span></span>

<span data-ttu-id="e1892-144">*Docker-compose.yml* plik zawiera nazwę obrazu, który jest tworzony po uruchomieniu projektu:</span><span class="sxs-lookup"><span data-stu-id="e1892-144">The *docker-compose.yml* file contains the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

<span data-ttu-id="e1892-145">W powyższym przykładzie `image: hellodockertools` generuje obraz `hellodockertools:dev` uruchamiania aplikacji **debugowania** trybu.</span><span class="sxs-lookup"><span data-stu-id="e1892-145">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="e1892-146">`hellodockertools:latest` Obraz jest generowany, gdy aplikacja jest uruchamiana **wersji** trybu.</span><span class="sxs-lookup"><span data-stu-id="e1892-146">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="e1892-147">Nazwa obrazu przy użyciu prefiksu [usługi Docker Hub](https://hub.docker.com/) nazwy użytkownika (na przykład `dockerhubusername/hellodockertools`) Jeśli obraz, który zostanie przekazany do rejestru.</span><span class="sxs-lookup"><span data-stu-id="e1892-147">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image will be pushed to the registry.</span></span> <span data-ttu-id="e1892-148">Alternatywnie Zmień nazwę obrazu, aby zawierały adres URL rejestru prywatnego (na przykład `privateregistry.domain.com/hellodockertools`) w zależności od konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e1892-148">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

## <a name="debug"></a><span data-ttu-id="e1892-149">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="e1892-149">Debug</span></span>

<span data-ttu-id="e1892-150">Wybierz **Docker** z poziomu pozycji Debuguj listy rozwijanej w pasku narzędzi, a następnie uruchamiania, debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e1892-150">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="e1892-151">**Docker** widoku **dane wyjściowe** oknie wyświetlane są następujące akcje miejsce:</span><span class="sxs-lookup"><span data-stu-id="e1892-151">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

* <span data-ttu-id="e1892-152">*Microsoft/aspnetcore* obraz środowiska uruchomieniowego jest uzyskiwany (Jeśli nie są już w pamięci podręcznej).</span><span class="sxs-lookup"><span data-stu-id="e1892-152">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="e1892-153">*Microsoft/aspnetcore-build* kompilacji/publikowania obrazu jest uzyskiwany (Jeśli nie są już w pamięci podręcznej).</span><span class="sxs-lookup"><span data-stu-id="e1892-153">The *microsoft/aspnetcore-build* compile/publish image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="e1892-154">*ASPNETCORE_ENVIRONMENT* ustawiono zmiennej środowiskowej `Development` w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="e1892-154">The *ASPNETCORE_ENVIRONMENT* environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="e1892-155">Port 80 jest udostępniane i mapowane na port dynamicznie przypisywanej nazwy localhost.</span><span class="sxs-lookup"><span data-stu-id="e1892-155">Port 80 is exposed and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="e1892-156">Numer portu jest określany przez hosta platformy Docker i może być odpytywana za pomocą `docker ps` polecenia.</span><span class="sxs-lookup"><span data-stu-id="e1892-156">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="e1892-157">Aplikacja jest kopiowana do kontenera.</span><span class="sxs-lookup"><span data-stu-id="e1892-157">The app is copied to the container.</span></span>
* <span data-ttu-id="e1892-158">W debugerze do kontenera przy użyciu portu przypisywany dynamicznie, uruchamiana jest domyślna przeglądarka.</span><span class="sxs-lookup"><span data-stu-id="e1892-158">The default browser is launched with the debugger attached to the container using the dynamically assigned port.</span></span>

<span data-ttu-id="e1892-159">Wynikowy obraz platformy Docker jest *dev* obrazu aplikacji za pomocą *microsoft/aspnetcore* obrazów jako obrazu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="e1892-159">The resulting Docker image is the *dev* image of the app with the *microsoft/aspnetcore* images as the base image.</span></span> <span data-ttu-id="e1892-160">Uruchom `docker images` polecenia w pliku **Konsola Menedżera pakietów** okna (PMC).</span><span class="sxs-lookup"><span data-stu-id="e1892-160">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="e1892-161">Wyświetlane są obrazy na komputerze:</span><span class="sxs-lookup"><span data-stu-id="e1892-161">The images on the machine are displayed:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="e1892-162">Obraz dev brakuje zawartość aplikacji jako **debugowania** konfiguracje używają instalowania woluminów oferują środowisko o iteracyjne.</span><span class="sxs-lookup"><span data-stu-id="e1892-162">The dev image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="e1892-163">Aby wypchnąć obraz, należy użyć **wersji** konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e1892-163">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="e1892-164">Uruchom `docker ps` polecenia w konsoli zarządzania Pakietami.</span><span class="sxs-lookup"><span data-stu-id="e1892-164">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="e1892-165">Zwróć uwagę, że aplikacja jest uruchomiona przy użyciu kontenera:</span><span class="sxs-lookup"><span data-stu-id="e1892-165">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="e1892-166">Edytuj i Kontynuuj</span><span class="sxs-lookup"><span data-stu-id="e1892-166">Edit and continue</span></span>

<span data-ttu-id="e1892-167">Zmiany plików statycznych i widokami Razor są automatycznie aktualizowane bez konieczności kroku kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e1892-167">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="e1892-168">Wprowadzić zmiany, Zapisz i Odśwież przeglądarkę, aby wyświetlić tę aktualizację.</span><span class="sxs-lookup"><span data-stu-id="e1892-168">Make the change, save, and refresh the browser to view the update.</span></span>

<span data-ttu-id="e1892-169">Modyfikacje plików kodu wymagają kompilacja i ponowne uruchomienie Kestrel w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="e1892-169">Code file modifications require compilation and a restart of Kestrel within the container.</span></span> <span data-ttu-id="e1892-170">Po wprowadzeniu zmian, należy użyć `CTRL+F5` na wykonanie procesu i uruchomić aplikację w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="e1892-170">After making the change, use `CTRL+F5` to perform the process and start the app within the container.</span></span> <span data-ttu-id="e1892-171">Kontener platformy Docker nie jest ponownie skompilowany lub zatrzymana.</span><span class="sxs-lookup"><span data-stu-id="e1892-171">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="e1892-172">Uruchom `docker ps` polecenia w konsoli zarządzania Pakietami.</span><span class="sxs-lookup"><span data-stu-id="e1892-172">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="e1892-173">Zwróć uwagę, oryginalnym kontenerze jest nadal uruchomiona od 10 minut temu:</span><span class="sxs-lookup"><span data-stu-id="e1892-173">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="e1892-174">Publikowanie obrazów platformy Docker</span><span class="sxs-lookup"><span data-stu-id="e1892-174">Publish Docker images</span></span>

<span data-ttu-id="e1892-175">Po zakończeniu cyklu programowanie i debugowanie aplikacji Visual Studio Tools for Docker pomagać w tworzeniu obraz produkcyjnych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e1892-175">Once the develop and debug cycle of the app is completed, the Visual Studio Tools for Docker assist in creating the production image of the app.</span></span> <span data-ttu-id="e1892-176">Zmień konfigurację menu rozwijane **wersji** i kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e1892-176">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="e1892-177">Narzędzi tworzy obraz z *najnowsze* znacznik, który może zostać przeniesiony do prywatnego rejestru lub usługi Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="e1892-177">The tooling produces the image with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span>

<span data-ttu-id="e1892-178">Uruchom `docker images` polecenia w konsoli zarządzania Pakietami, aby wyświetlić listę obrazów:</span><span class="sxs-lookup"><span data-stu-id="e1892-178">Run the `docker images` command in PMC to see the list of images:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="e1892-179">`docker images` Polecenie zwraca pośrednie obrazów za pomocą nazwy repozytorium i tagi zidentyfikowane jako  *\<Brak >* (niewymienione na liście powyżej).</span><span class="sxs-lookup"><span data-stu-id="e1892-179">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="e1892-180">Te obrazy nienazwane są produkowane przez [kompilacji wieloetapowych](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *pliku Dockerfile*.</span><span class="sxs-lookup"><span data-stu-id="e1892-180">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="e1892-181">Poprawiają wydajność tworzenia finalnego obrazu&mdash;tylko niezbędne warstwy są ponownie skompilowany, gdy wystąpią zmiany.</span><span class="sxs-lookup"><span data-stu-id="e1892-181">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="e1892-182">Gdy pośrednie obrazy nie są już potrzebne, usuń je przy użyciu [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) polecenia.</span><span class="sxs-lookup"><span data-stu-id="e1892-182">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="e1892-183">Może to być oczekiwanie produkcji lub wersji obrazu na mniejszy rozmiar w porównaniu do *dev* obrazu.</span><span class="sxs-lookup"><span data-stu-id="e1892-183">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="e1892-184">Ze względu na mapowanie woluminu, debuger i aplikacji były uruchamiane z komputera lokalnego, a nie w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="e1892-184">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="e1892-185">*Najnowsze* obraz ma spakowane kodu aplikacji niezbędnych do uruchomienia aplikacji na komputerze hosta.</span><span class="sxs-lookup"><span data-stu-id="e1892-185">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="e1892-186">Dlatego delta jest rozmiar kodu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e1892-186">Therefore, the delta is the size of the app code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1892-187">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e1892-187">Additional resources</span></span>

* [<span data-ttu-id="e1892-188">Rozwiązywanie problemów z programowania Visual Studio 2017 przy użyciu rozwiązania Docker</span><span class="sxs-lookup"><span data-stu-id="e1892-188">Troubleshoot Visual Studio 2017 development with Docker</span></span>](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [<span data-ttu-id="e1892-189">Visual Studio Tools dla repozytorium GitHub platformy Docker</span><span class="sxs-lookup"><span data-stu-id="e1892-189">Visual Studio Tools for Docker GitHub repository</span></span>](https://github.com/Microsoft/DockerTools)
