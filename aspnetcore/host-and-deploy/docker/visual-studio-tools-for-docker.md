---
title: Visual Studio Tools for Docker z platformą ASP.NET Core
author: spboyer
description: Dowiedz się, jak za pomocą narzędzi Visual Studio 2017 i platformy Docker for Windows konteneryzowanie aplikacji ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/12/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: 4bb28e7644997c50c14046bc0c89338fa35a5f14
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538482"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="1c2d2-103">Visual Studio Tools for Docker z platformą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c2d2-103">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="1c2d2-104">Program Visual Studio 2017 obsługuje kompilowania, debugowania i uruchamiania konteneryzowanych platformy ASP.NET Core z aplikacji przeznaczonych dla platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-104">Visual Studio 2017 supports building, debugging, and running containerized ASP.NET Core apps targeting .NET Core.</span></span> <span data-ttu-id="1c2d2-105">Są obsługiwane kontenerów systemów Windows i Linux.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-105">Both Windows and Linux containers are supported.</span></span>

<span data-ttu-id="1c2d2-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/docker/visual-studio-tools-for-docker/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1c2d2-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/docker/visual-studio-tools-for-docker/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c2d2-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1c2d2-107">Prerequisites</span></span>

* [<span data-ttu-id="1c2d2-108">Docker for Windows</span><span class="sxs-lookup"><span data-stu-id="1c2d2-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)
* <span data-ttu-id="1c2d2-109">[Program Visual Studio 2017](https://www.visualstudio.com/) z **programowanie dla wielu platform .NET Core** obciążenia</span><span class="sxs-lookup"><span data-stu-id="1c2d2-109">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="1c2d2-110">Instalacja i Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="1c2d2-110">Installation and setup</span></span>

<span data-ttu-id="1c2d2-111">Instalacja platformy Docker, najpierw zapoznaj się z informacjami o [Docker for Windows: co należy wiedzieć przed zainstalowaniem](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install).</span><span class="sxs-lookup"><span data-stu-id="1c2d2-111">For Docker installation, first review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install).</span></span> <span data-ttu-id="1c2d2-112">Następnie zainstaluj [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="1c2d2-112">Next, install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="1c2d2-113">**[Udostępnione dyski](https://docs.docker.com/docker-for-windows/#shared-drives)**  w Docker for Windows musi być skonfigurowana do obsługi mapowania woluminu i debugowania.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-113">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="1c2d2-114">Kliknij prawym przyciskiem myszy ikonę platformy Docker w zasobniku systemowym, wybierz **ustawienia**i wybierz **udostępnione dyski**.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-114">Right-click the System Tray's Docker icon, select **Settings**, and select **Shared Drives**.</span></span> <span data-ttu-id="1c2d2-115">Wybierz dysk, na którym Docker przechowuje pliki.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-115">Select the drive where Docker stores files.</span></span> <span data-ttu-id="1c2d2-116">Kliknij przycisk **zastosować**.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-116">Click **Apply**.</span></span>

![Okno dialogowe, aby wybrać lokalny dysk C do udostępniania dla kontenerów](visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="1c2d2-118">Visual Studio 2017 w wersji 15.6 lub nowszej monitowanie, gdy **udostępnione dyski** nie są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-118">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-a-project-to-a-docker-container"></a><span data-ttu-id="1c2d2-119">Dodaj projekt do kontenera platformy Docker</span><span class="sxs-lookup"><span data-stu-id="1c2d2-119">Add a project to a Docker container</span></span>

<span data-ttu-id="1c2d2-120">Aby konteneryzowanie projektu ASP.NET Core, projekt musi przeznaczony dla platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-120">To containerize an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="1c2d2-121">Są obsługiwane kontenerów systemów Linux i Windows.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-121">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="1c2d2-122">Podczas dodawania obsługi programu Docker do projektu, wybierz kontener systemu Linux lub Windows.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-122">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="1c2d2-123">Hosta platformy Docker musi działać ten sam typ kontenera.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-123">The Docker host must be running the same container type.</span></span> <span data-ttu-id="1c2d2-124">Aby zmienić typ kontenera, na uruchomione wystąpienie platformy Docker, kliknij prawym przyciskiem myszy ikonę platformy Docker w zasobniku systemowym, a następnie wybierz **przełączyć się do kontenerów Windows...**  lub **przełączyć się do kontenerów systemu Linux...** .</span><span class="sxs-lookup"><span data-stu-id="1c2d2-124">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="1c2d2-125">Nowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="1c2d2-125">New app</span></span>

<span data-ttu-id="1c2d2-126">Podczas tworzenia nowej aplikacji za pomocą **aplikacji sieci Web programu ASP.NET Core** szablonów projektu wybierz **włączyć obsługę platformy Docker** pole wyboru:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-126">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** check box:</span></span>

![Zaznacz pole wyboru obsługę platformy Docker](visual-studio-tools-for-docker/_static/enable-docker-support-check-box.png)

<span data-ttu-id="1c2d2-128">W przypadku platformy docelowej platformy .NET Core **OS** listy rozwijanej umożliwia wybór typu kontenera.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-128">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="1c2d2-129">Istniejąca aplikacja</span><span class="sxs-lookup"><span data-stu-id="1c2d2-129">Existing app</span></span>

<span data-ttu-id="1c2d2-130">Dla projektów ASP.NET Core przeznaczone dla platformy .NET Core istnieją dwa sposoby dodawania obsługę platformy Docker za pomocą narzędzi.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-130">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="1c2d2-131">Otwórz projekt w programie Visual Studio i wybierz jedną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-131">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="1c2d2-132">Wybierz **obsługę platformy Docker** z **projektu** menu.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-132">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="1c2d2-133">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Dodaj** > **obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-133">Right-click the project in **Solution Explorer** and select **Add** > **Docker Support**.</span></span>

<span data-ttu-id="1c2d2-134">Visual Studio Tools for Docker nie jest obsługiwane dodawanie platformy Docker do istniejącego projektu platformy ASP.NET Core przeznaczone dla .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-134">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span>

## <a name="dockerfile-overview"></a><span data-ttu-id="1c2d2-135">Plik Dockerfile — omówienie</span><span class="sxs-lookup"><span data-stu-id="1c2d2-135">Dockerfile overview</span></span>

<span data-ttu-id="1c2d2-136">A *pliku Dockerfile*, przepisu do utworzenia końcowej obrazu platformy Docker, zostanie dodany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-136">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="1c2d2-137">Zapoznaj się [odwołanie do pliku Dockerfile](https://docs.docker.com/engine/reference/builder/) dla zrozumienia poleceń w nim.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-137">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="1c2d2-138">Tej konkretnej *pliku Dockerfile* używa [kompilacji wieloetapowych](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) z czterech różnych, o nazwie etapy kompilacji generują:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-138">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) with four distinct, named build stages:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile.original?highlight=1,6,14,17)]

<span data-ttu-id="1c2d2-139">Poprzedni *pliku Dockerfile* opiera się na [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/) obrazu.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-139">The preceding *Dockerfile* is based on the [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/) image.</span></span> <span data-ttu-id="1c2d2-140">Ten podstawowy obraz zawiera środowisko uruchomieniowe programu ASP.NET Core i pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-140">This base image includes the ASP.NET Core runtime and NuGet packages.</span></span> <span data-ttu-id="1c2d2-141">Pakiety są just-in-time (JIT) skompilowany w celu zwiększenia wydajności uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-141">The packages are just-in-time (JIT) compiled to improve startup performance.</span></span>

<span data-ttu-id="1c2d2-142">Gdy okno dialogowe nowego projektu w **Konfigurowanie protokołu HTTPS** pole wyboru jest zaznaczone, *pliku Dockerfile* udostępnia dwa porty.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-142">When the new project dialog's **Configure for HTTPS** check box is checked, the *Dockerfile* exposes two ports.</span></span> <span data-ttu-id="1c2d2-143">Jeden port jest używany do ruchu HTTP; inne port jest używany do obsługi protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-143">One port is used for HTTP traffic; the other port is used for HTTPS.</span></span> <span data-ttu-id="1c2d2-144">Jeśli nie jest zaznaczone pole wyboru, jednego portu (80) jest uwidaczniany dla ruchu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-144">If the check box isn't checked, a single port (80) is exposed for HTTP traffic.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.0/HelloDockerTools/Dockerfile?highlight=1,5,13,16)]

<span data-ttu-id="1c2d2-145">Poprzedni *pliku Dockerfile* opiera się na [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore/) obrazu.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-145">The preceding *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore/) image.</span></span> <span data-ttu-id="1c2d2-146">Tego obrazu podstawowego zawiera pakiety platformy ASP.NET Core NuGet, które są just-in-time (JIT) skompilowany w celu zwiększenia wydajności uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-146">This base image includes the ASP.NET Core NuGet packages, which are just-in-time (JIT) compiled to improve startup performance.</span></span>

::: moniker-end

## <a name="add-container-orchestrator-support-to-an-app"></a><span data-ttu-id="1c2d2-147">Dodawanie obsługi orkiestratora kontenerów do aplikacji</span><span class="sxs-lookup"><span data-stu-id="1c2d2-147">Add container orchestrator support to an app</span></span>

<span data-ttu-id="1c2d2-148">Obsługa Visual Studio 2017 w wersji 15.7 lub wcześniejszych [narzędzia Docker Compose](https://docs.docker.com/compose/overview/) jako rozwiązania do organizowania kontenerów wyłącznie.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-148">Visual Studio 2017 versions 15.7 or earlier support [Docker Compose](https://docs.docker.com/compose/overview/) as the sole container orchestration solution.</span></span> <span data-ttu-id="1c2d2-149">Artefakty narzędzia Docker Compose są dodane za pośrednictwem **Dodaj** > **obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-149">The Docker Compose artifacts are added via **Add** > **Docker Support**.</span></span>

<span data-ttu-id="1c2d2-150">Visual Studio 2017 w wersji, należy zachować 15,8 lub nowszej, Dodaj rozwiązania do organizowania tylko wtedy, gdy zgodnie z instrukcjami otrzymywanymi.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-150">Visual Studio 2017 versions 15.8 or later add an orchestration solution only when instructed.</span></span> <span data-ttu-id="1c2d2-151">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Dodaj** > **obsługi Orkiestratora kontenerów**.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-151">Right-click the project in **Solution Explorer** and select **Add** > **Container Orchestrator Support**.</span></span> <span data-ttu-id="1c2d2-152">Są dostępne dwie różne opcje: [narzędzia Docker Compose](#docker-compose) i [usługi Service Fabric](#service-fabric).</span><span class="sxs-lookup"><span data-stu-id="1c2d2-152">Two different choices are offered: [Docker Compose](#docker-compose) and [Service Fabric](#service-fabric).</span></span>

### <a name="docker-compose"></a><span data-ttu-id="1c2d2-153">Narzędzia docker Compose</span><span class="sxs-lookup"><span data-stu-id="1c2d2-153">Docker Compose</span></span>

<span data-ttu-id="1c2d2-154">Dodaj program Visual Studio Tools for Docker *docker-compose* projekt do rozwiązania przy użyciu następujących plików:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-154">The Visual Studio Tools for Docker add a *docker-compose* project to the solution with the following files:</span></span>

* <span data-ttu-id="1c2d2-155">*docker compose.dcproj* &ndash; pliku reprezentujący projektu.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-155">*docker-compose.dcproj* &ndash; The file representing the project.</span></span> <span data-ttu-id="1c2d2-156">Obejmuje `<DockerTargetOS>` elementu, określając system operacyjny, który ma być używany.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-156">Includes a `<DockerTargetOS>` element specifying the OS to be used.</span></span>
* <span data-ttu-id="1c2d2-157">*.dockerignore* &ndash; zawiera listę wzorców plików i katalogów, które mają zostać wykluczone podczas generowania kontekstu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-157">*.dockerignore* &ndash; Lists the file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="1c2d2-158">*docker-compose.yml* &ndash; base [narzędzia Docker Compose](https://docs.docker.com/compose/overview/) plik wykorzystywany do definiowania kolekcję obrazów, skompilować i uruchomić z `docker-compose build` i `docker-compose run`, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-158">*docker-compose.yml* &ndash; The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="1c2d2-159">*docker-compose.override.yml* &ndash; opcjonalny plik odczytywane przez narzędzia Docker Compose za pomocą konfiguracji zastąpień dla usług.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-159">*docker-compose.override.yml* &ndash; An optional file, read by Docker Compose, with configuration overrides for services.</span></span> <span data-ttu-id="1c2d2-160">Visual Studio wykonuje `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` do scalenia tych plików.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-160">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="1c2d2-161">*Docker-compose.yml* plik odwołuje się nazwa obrazu, który jest tworzony po uruchomieniu projektu:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-161">The *docker-compose.yml* file references the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/2.0/docker-compose.yml?highlight=5)]

<span data-ttu-id="1c2d2-162">W powyższym przykładzie `image: hellodockertools` generuje obraz `hellodockertools:dev` uruchamiania aplikacji **debugowania** trybu.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-162">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="1c2d2-163">`hellodockertools:latest` Obraz jest generowany, gdy aplikacja jest uruchamiana **wersji** trybu.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-163">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="1c2d2-164">Nazwa obrazu przy użyciu prefiksu [usługi Docker Hub](https://hub.docker.com/) nazwy użytkownika (na przykład `dockerhubusername/hellodockertools`) Jeśli obraz jest wypchnięte do rejestru.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-164">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image is pushed to the registry.</span></span> <span data-ttu-id="1c2d2-165">Alternatywnie Zmień nazwę obrazu, aby zawierały adres URL rejestru prywatnego (na przykład `privateregistry.domain.com/hellodockertools`) w zależności od konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-165">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

<span data-ttu-id="1c2d2-166">Różne zachowania na podstawie konfiguracji kompilacji (na przykład Debug i Release), należy dodać specyficznych dla konfiguracji *narzędzia docker compose* plików.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-166">If you want different behavior based on the build configuration (for example, Debug or Release), add configuration-specific *docker-compose* files.</span></span> <span data-ttu-id="1c2d2-167">Pliki powinno się nazywać zgodnie z konfiguracją kompilacji (na przykład *docker compose.vs.debug.yml* i *docker compose.vs.release.yml*) i jest umieszczany w tej samej lokalizacji co *docker-compose-override.yml* pliku.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-167">The files should be named according to the build configuration (for example, *docker-compose.vs.debug.yml* and *docker-compose.vs.release.yml*) and placed in the same location as the *docker-compose-override.yml* file.</span></span> 

<span data-ttu-id="1c2d2-168">Za pomocą plików zastąpienie specyficznych dla konfiguracji, można określić różnych ustawień konfiguracji (np. zmienne środowiskowe lub punkty wejścia) dla konfiguracji kompilacji debugowania, jak i wydania.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-168">Using the configuration-specific override files, you can specify different configuration settings (such as environment variables or entry points) for Debug and Release build configurations.</span></span>

### <a name="service-fabric"></a><span data-ttu-id="1c2d2-169">Usługa Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1c2d2-169">Service Fabric</span></span>

<span data-ttu-id="1c2d2-170">Oprócz base [wymagania wstępne](#prerequisites), [usługi Service Fabric](/azure/service-fabric/) rozwiązania do organizowania zapotrzebowania na następujące wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-170">In addition to the base [Prerequisites](#prerequisites), the [Service Fabric](/azure/service-fabric/) orchestration solution demands the following prerequisites:</span></span>

* <span data-ttu-id="1c2d2-171">[Zestaw SDK Microsoft Azure Service Fabric](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK) wersji 2.6 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="1c2d2-171">[Microsoft Azure Service Fabric SDK](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK) version 2.6 or later</span></span>
* <span data-ttu-id="1c2d2-172">W programie Visual Studio 2017 **programowanie na platformie Azure** obciążenia</span><span class="sxs-lookup"><span data-stu-id="1c2d2-172">Visual Studio 2017's **Azure Development** workload</span></span>

<span data-ttu-id="1c2d2-173">Usługa Service Fabric nie obsługuje uruchamianie kontenerów systemu Linux w lokalnym klastrze projektowym na Windows.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-173">Service Fabric doesn't support running Linux containers in the local development cluster on Windows.</span></span> <span data-ttu-id="1c2d2-174">Jeśli projekt jest już używany w kontenerze systemu Linux, aby przełączyć się do kontenerów Windows wyświetla monit o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-174">If the project is already using a Linux container, Visual Studio prompts to switch to Windows containers.</span></span>

<span data-ttu-id="1c2d2-175">Visual Studio Tools for Docker, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-175">The Visual Studio Tools for Docker do the following tasks:</span></span>

* <span data-ttu-id="1c2d2-176">Dodaje  *&lt;project_name&gt;aplikacji* **aplikacji usługi Service Fabric** projektu do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-176">Adds a *&lt;project_name&gt;Application* **Service Fabric Application** project to the solution.</span></span>
* <span data-ttu-id="1c2d2-177">Dodaje *pliku Dockerfile* i *.dockerignore* pliku do projektu programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-177">Adds a *Dockerfile* and a *.dockerignore* file to the ASP.NET Core project.</span></span> <span data-ttu-id="1c2d2-178">Jeśli *pliku Dockerfile* już istnieje w projekcie platformy ASP.NET Core została zmieniona na *Dockerfile.original*.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-178">If a *Dockerfile* already exists in the ASP.NET Core project, it's renamed to *Dockerfile.original*.</span></span> <span data-ttu-id="1c2d2-179">Nowy *pliku Dockerfile*, podobny do poniższego, zostanie utworzony:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-179">A new *Dockerfile*, similar to the following, is created:</span></span>

    [!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile)]

* <span data-ttu-id="1c2d2-180">Dodaje `<IsServiceFabricServiceProject>` elementu do projektu programu ASP.NET Core *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-180">Adds an `<IsServiceFabricServiceProject>` element to the ASP.NET Core project's *.csproj* file:</span></span>

    [!code-xml[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/HelloDockerTools.csproj?name=snippet_IsServiceFabricServiceProject)]

* <span data-ttu-id="1c2d2-181">Dodaje *PackageRoot* folderu do projektu programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-181">Adds a *PackageRoot* folder to the ASP.NET Core project.</span></span> <span data-ttu-id="1c2d2-182">Folder zawiera manifestu usługi oraz ustawienia dla nowej usługi.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-182">The folder includes the service manifest and settings for the new service.</span></span>

<span data-ttu-id="1c2d2-183">Aby uzyskać więcej informacji, zobacz [wdrażania aplikacji .NET w kontenerze Windows w usłudze Azure Service Fabric](/azure/service-fabric/service-fabric-host-app-in-a-container).</span><span class="sxs-lookup"><span data-stu-id="1c2d2-183">For more information, see [Deploy a .NET app in a Windows container to Azure Service Fabric](/azure/service-fabric/service-fabric-host-app-in-a-container).</span></span>

## <a name="debug"></a><span data-ttu-id="1c2d2-184">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="1c2d2-184">Debug</span></span>

<span data-ttu-id="1c2d2-185">Wybierz **Docker** z poziomu pozycji Debuguj listy rozwijanej w pasku narzędzi, a następnie uruchamiania, debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-185">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="1c2d2-186">**Docker** widoku **dane wyjściowe** oknie wyświetlane są następujące akcje miejsce:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-186">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="1c2d2-187">*2.1 — aspnetcore-środowiska uruchomieniowego* tag *microsoft/dotnet* obraz środowiska uruchomieniowego jest uzyskiwany (Jeśli nie są już w pamięci podręcznej).</span><span class="sxs-lookup"><span data-stu-id="1c2d2-187">The *2.1-aspnetcore-runtime* tag of the *microsoft/dotnet* runtime image is acquired (if not already in the cache).</span></span> <span data-ttu-id="1c2d2-188">Obraz, który instaluje środowiska uruchomieniowe platformy ASP.NET Core i .NET Core i skojarzonymi z nimi bibliotekami.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-188">The image installs the ASP.NET Core and .NET Core runtimes and associated libraries.</span></span> <span data-ttu-id="1c2d2-189">Jest zoptymalizowany do uruchamiania aplikacji ASP.NET Core w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-189">It's optimized for running ASP.NET Core apps in production.</span></span>
* <span data-ttu-id="1c2d2-190">`ASPNETCORE_ENVIRONMENT` Ustawiono zmiennej środowiskowej `Development` w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-190">The `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="1c2d2-191">Dwa dynamicznie przydzielanego porty są dostępne: jeden dla protokołu HTTP i jeden do obsługi protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-191">Two dynamically assigned ports are exposed: one for HTTP and one for HTTPS.</span></span> <span data-ttu-id="1c2d2-192">Numer portu przypisany do hosta lokalnego może być odpytywany za pomocą `docker ps` polecenia.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-192">The port assigned to localhost can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="1c2d2-193">Aplikacja jest kopiowana do kontenera.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-193">The app is copied to the container.</span></span>
* <span data-ttu-id="1c2d2-194">W debugerze do kontenera przy użyciu portu przypisywany dynamicznie, uruchamiana jest domyślna przeglądarka.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-194">The default browser is launched with the debugger attached to the container using the dynamically assigned port.</span></span>

<span data-ttu-id="1c2d2-195">Wynikowy obraz platformy Docker w aplikacji zostanie oznaczony jako *dev*.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-195">The resulting Docker image of the app is tagged as *dev*.</span></span> <span data-ttu-id="1c2d2-196">Obraz, który jest oparty na *2.1 — aspnetcore-środowiska uruchomieniowego* tag *microsoft/dotnet* obrazu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-196">The image is based on the *2.1-aspnetcore-runtime* tag of the *microsoft/dotnet* base image.</span></span> <span data-ttu-id="1c2d2-197">Uruchom `docker images` polecenia w pliku **Konsola Menedżera pakietów** okna (PMC).</span><span class="sxs-lookup"><span data-stu-id="1c2d2-197">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="1c2d2-198">Wyświetlane są obrazy na komputerze:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-198">The images on the machine are displayed:</span></span>

```console
REPOSITORY        TAG                     IMAGE ID      CREATED         SIZE
hellodockertools  dev                     d72ce0f1dfe7  30 seconds ago  255MB
microsoft/dotnet  2.1-aspnetcore-runtime  fcc3887985bb  6 days ago      255MB
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <span data-ttu-id="1c2d2-199">*Microsoft/aspnetcore* obraz środowiska uruchomieniowego jest uzyskiwany (Jeśli nie są już w pamięci podręcznej).</span><span class="sxs-lookup"><span data-stu-id="1c2d2-199">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="1c2d2-200">`ASPNETCORE_ENVIRONMENT` Ustawiono zmiennej środowiskowej `Development` w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-200">The `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="1c2d2-201">Port 80 jest udostępniane i mapowane na port dynamicznie przypisywanej nazwy localhost.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-201">Port 80 is exposed and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="1c2d2-202">Numer portu jest określany przez hosta platformy Docker i może być odpytywana za pomocą `docker ps` polecenia.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-202">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="1c2d2-203">Aplikacja jest kopiowana do kontenera.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-203">The app is copied to the container.</span></span>
* <span data-ttu-id="1c2d2-204">W debugerze do kontenera przy użyciu portu przypisywany dynamicznie, uruchamiana jest domyślna przeglądarka.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-204">The default browser is launched with the debugger attached to the container using the dynamically assigned port.</span></span>

<span data-ttu-id="1c2d2-205">Wynikowy obraz platformy Docker w aplikacji zostanie oznaczony jako *dev*.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-205">The resulting Docker image of the app is tagged as *dev*.</span></span> <span data-ttu-id="1c2d2-206">Obraz, który jest oparty na *microsoft/aspnetcore* obrazu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-206">The image is based on the *microsoft/aspnetcore* base image.</span></span> <span data-ttu-id="1c2d2-207">Uruchom `docker images` polecenia w pliku **Konsola Menedżera pakietów** okna (PMC).</span><span class="sxs-lookup"><span data-stu-id="1c2d2-207">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="1c2d2-208">Wyświetlane są obrazy na komputerze:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-208">The images on the machine are displayed:</span></span>

```console
REPOSITORY            TAG  IMAGE ID      CREATED        SIZE
hellodockertools      dev  5fafe5d1ad5b  4 minutes ago  347MB
microsoft/aspnetcore  2.0  c69d39472da9  13 days ago    347MB
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="1c2d2-209">*Dev* obraz nie ma zawartość aplikacji jako **debugowania** konfiguracje używają instalowania woluminów oferują środowisko o iteracyjne.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-209">The *dev* image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="1c2d2-210">Aby wypchnąć obraz, należy użyć **wersji** konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-210">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="1c2d2-211">Uruchom `docker ps` polecenia w konsoli zarządzania Pakietami.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-211">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="1c2d2-212">Zwróć uwagę, że aplikacja jest uruchomiona przy użyciu kontenera:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-212">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="1c2d2-213">Edytuj i Kontynuuj</span><span class="sxs-lookup"><span data-stu-id="1c2d2-213">Edit and continue</span></span>

<span data-ttu-id="1c2d2-214">Zmiany plików statycznych i widokami Razor są automatycznie aktualizowane bez konieczności kroku kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-214">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="1c2d2-215">Wprowadzić zmiany, Zapisz i Odśwież przeglądarkę, aby wyświetlić tę aktualizację.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-215">Make the change, save, and refresh the browser to view the update.</span></span>

<span data-ttu-id="1c2d2-216">Modyfikacje plików kodu wymagają kompilacja i ponowne uruchomienie Kestrel w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-216">Code file modifications require compilation and a restart of Kestrel within the container.</span></span> <span data-ttu-id="1c2d2-217">Po wprowadzeniu zmian, należy użyć `CTRL+F5` na wykonanie procesu i uruchomić aplikację w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-217">After making the change, use `CTRL+F5` to perform the process and start the app within the container.</span></span> <span data-ttu-id="1c2d2-218">Kontener platformy Docker nie jest ponownie skompilowany lub zatrzymana.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-218">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="1c2d2-219">Uruchom `docker ps` polecenia w konsoli zarządzania Pakietami.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-219">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="1c2d2-220">Zwróć uwagę, oryginalnym kontenerze jest nadal uruchomiona od 10 minut temu:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-220">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="1c2d2-221">Publikowanie obrazów platformy Docker</span><span class="sxs-lookup"><span data-stu-id="1c2d2-221">Publish Docker images</span></span>

<span data-ttu-id="1c2d2-222">Po zakończeniu cyklu programowanie i debugowanie aplikacji Visual Studio Tools for Docker pomagać w tworzeniu obraz produkcyjnych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-222">Once the develop and debug cycle of the app is completed, the Visual Studio Tools for Docker assist in creating the production image of the app.</span></span> <span data-ttu-id="1c2d2-223">Zmień konfigurację menu rozwijane **wersji** i kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-223">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="1c2d2-224">Narzędzi uzyskuje kompilacji/publikowania obrazu z usługi Docker Hub (Jeśli nie jest już w pamięci podręcznej).</span><span class="sxs-lookup"><span data-stu-id="1c2d2-224">The tooling acquires the compile/publish image from Docker Hub (if not already in the cache).</span></span> <span data-ttu-id="1c2d2-225">Obraz jest generowany przy użyciu *najnowsze* znacznik, który może zostać przeniesiony do prywatnego rejestru lub usługi Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-225">An image is produced with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span>

<span data-ttu-id="1c2d2-226">Uruchom `docker images` polecenia w konsoli zarządzania Pakietami, aby wyświetlić listę obrazów.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-226">Run the `docker images` command in PMC to see the list of images.</span></span> <span data-ttu-id="1c2d2-227">Wyświetlone dane wyjściowe podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="1c2d2-227">Output similar to the following is displayed:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
REPOSITORY        TAG                     IMAGE ID      CREATED             SIZE
hellodockertools  latest                  e3984a64230c  About a minute ago  258MB
hellodockertools  dev                     d72ce0f1dfe7  4 minutes ago       255MB
microsoft/dotnet  2.1-sdk                 9e243db15f91  6 days ago          1.7GB
microsoft/dotnet  2.1-aspnetcore-runtime  fcc3887985bb  6 days ago          255MB
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```console
REPOSITORY                  TAG     IMAGE ID      CREATED         SIZE
hellodockertools            latest  cd28f0d4abbd  12 seconds ago  349MB
hellodockertools            dev     5fafe5d1ad5b  23 minutes ago  347MB
microsoft/aspnetcore-build  2.0     7fed40fbb647  13 days ago     2.02GB
microsoft/aspnetcore        2.0     c69d39472da9  13 days ago     347MB
```

<span data-ttu-id="1c2d2-228">`microsoft/aspnetcore-build` i `microsoft/aspnetcore` obrazów wymienione w powyższym danych wyjściowych są zastępowane `microsoft/dotnet` obrazów, począwszy od platformy .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-228">The `microsoft/aspnetcore-build` and `microsoft/aspnetcore` images listed in the preceding output are replaced with `microsoft/dotnet` images as of .NET Core 2.1.</span></span> <span data-ttu-id="1c2d2-229">Aby uzyskać więcej informacji, zobacz [ogłoszenie migracji repozytoriów platformy Docker](https://github.com/aspnet/Announcements/issues/298).</span><span class="sxs-lookup"><span data-stu-id="1c2d2-229">For more information, see [the Docker repositories migration announcement](https://github.com/aspnet/Announcements/issues/298).</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="1c2d2-230">`docker images` Polecenie zwraca pośrednie obrazów za pomocą nazwy repozytorium i tagi zidentyfikowane jako  *\<Brak >* (niewymienione na liście powyżej).</span><span class="sxs-lookup"><span data-stu-id="1c2d2-230">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="1c2d2-231">Te obrazy nienazwane są produkowane przez [kompilacji wieloetapowych](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *pliku Dockerfile*.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-231">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="1c2d2-232">Poprawiają wydajność tworzenia finalnego obrazu&mdash;tylko niezbędne warstwy są ponownie skompilowany, gdy wystąpią zmiany.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-232">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="1c2d2-233">Gdy pośrednie obrazy nie są już potrzebne, usuń je przy użyciu [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) polecenia.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-233">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="1c2d2-234">Może to być oczekiwanie produkcji lub wersji obrazu na mniejszy rozmiar w porównaniu do *dev* obrazu.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-234">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="1c2d2-235">Ze względu na mapowanie woluminu, debuger i aplikacji były uruchamiane z komputera lokalnego, a nie w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-235">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="1c2d2-236">*Najnowsze* obraz ma spakowane kodu aplikacji niezbędnych do uruchomienia aplikacji na komputerze hosta.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-236">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="1c2d2-237">Dlatego delta jest rozmiar kodu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c2d2-237">Therefore, the delta is the size of the app code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1c2d2-238">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1c2d2-238">Additional resources</span></span>

* [<span data-ttu-id="1c2d2-239">Tworzenie kontenerów za pomocą programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c2d2-239">Container development with Visual Studio</span></span>](/visualstudio/containers)
* [<span data-ttu-id="1c2d2-240">Usługa Azure Service Fabric: Przygotowywanie środowiska projektowego</span><span class="sxs-lookup"><span data-stu-id="1c2d2-240">Azure Service Fabric: Prepare your development environment</span></span>](/azure/service-fabric/service-fabric-get-started)
* [<span data-ttu-id="1c2d2-241">Wdrażanie aplikacji .NET w kontenerze Windows w usłudze Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1c2d2-241">Deploy a .NET app in a Windows container to Azure Service Fabric</span></span>](/azure/service-fabric/service-fabric-host-app-in-a-container)
* [<span data-ttu-id="1c2d2-242">Rozwiązywanie problemów z programowania Visual Studio 2017 przy użyciu rozwiązania Docker</span><span class="sxs-lookup"><span data-stu-id="1c2d2-242">Troubleshoot Visual Studio 2017 development with Docker</span></span>](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [<span data-ttu-id="1c2d2-243">Visual Studio Tools dla repozytorium GitHub platformy Docker</span><span class="sxs-lookup"><span data-stu-id="1c2d2-243">Visual Studio Tools for Docker GitHub repository</span></span>](https://github.com/Microsoft/DockerTools)
