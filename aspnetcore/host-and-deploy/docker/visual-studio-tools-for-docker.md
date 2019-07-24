---
title: Narzędzia kontenera programu Visual Studio z ASP.NET Core
author: spboyer
description: Dowiedz się, jak używać narzędzi programu Visual Studio i Docker for Windows, aby konteneryzowanie aplikację ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/12/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: 5faf0be19448d8272901bf018357da63bbe22d4b
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308071"
---
# <a name="visual-studio-container-tools-with-aspnet-core"></a><span data-ttu-id="1b827-103">Narzędzia kontenera programu Visual Studio z ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1b827-103">Visual Studio Container Tools with ASP.NET Core</span></span>

<span data-ttu-id="1b827-104">Program Visual Studio 2017 i nowsze wersje obsługują kompilowanie, debugowanie i uruchamianie kontenerów ASP.NET Core aplikacji przeznaczonych dla platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1b827-104">Visual Studio 2017 and later versions support building, debugging, and running containerized ASP.NET Core apps targeting .NET Core.</span></span> <span data-ttu-id="1b827-105">Obsługiwane są kontenery systemów Windows i Linux.</span><span class="sxs-lookup"><span data-stu-id="1b827-105">Both Windows and Linux containers are supported.</span></span>

<span data-ttu-id="1b827-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/docker/visual-studio-tools-for-docker/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1b827-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/docker/visual-studio-tools-for-docker/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1b827-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1b827-107">Prerequisites</span></span>

* [<span data-ttu-id="1b827-108">Docker for Windows</span><span class="sxs-lookup"><span data-stu-id="1b827-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)
* <span data-ttu-id="1b827-109">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z obciążeniem programistycznym dla **wielu platform .NET Core**</span><span class="sxs-lookup"><span data-stu-id="1b827-109">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **.NET Core cross-platform development** workload</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="1b827-110">Instalacja i konfiguracja</span><span class="sxs-lookup"><span data-stu-id="1b827-110">Installation and setup</span></span>

<span data-ttu-id="1b827-111">Aby zainstalować platformę Docker, najpierw zapoznaj [się z informacjami w Docker for Windows: Co należy wiedzieć przed zainstalowaniem](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install)programu.</span><span class="sxs-lookup"><span data-stu-id="1b827-111">For Docker installation, first review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install).</span></span> <span data-ttu-id="1b827-112">Następnie zainstaluj [platformę Docker dla systemu Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="1b827-112">Next, install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="1b827-113">**[Udostępnione dyski](https://docs.docker.com/docker-for-windows/#shared-drives)** w Docker for Windows muszą być skonfigurowane do obsługi mapowania woluminów i debugowania.</span><span class="sxs-lookup"><span data-stu-id="1b827-113">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="1b827-114">Kliknij prawym przyciskiem myszy ikonę platformy Docker na pasku zadań, wybierz pozycję **Ustawienia**, a następnie wybierz pozycję **dyski udostępnione**.</span><span class="sxs-lookup"><span data-stu-id="1b827-114">Right-click the System Tray's Docker icon, select **Settings**, and select **Shared Drives**.</span></span> <span data-ttu-id="1b827-115">Wybierz dysk, na którym system Docker przechowuje pliki.</span><span class="sxs-lookup"><span data-stu-id="1b827-115">Select the drive where Docker stores files.</span></span> <span data-ttu-id="1b827-116">Kliknij przycisk **zastosować**.</span><span class="sxs-lookup"><span data-stu-id="1b827-116">Click **Apply**.</span></span>

![Okno dialogowe umożliwiające wybranie lokalnego udostępniania dysku C dla kontenerów](visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="1b827-118">Program Visual Studio 2017 w wersji 15,6 lub nowszej wyświetla monit, gdy **dyski udostępnione** nie są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="1b827-118">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-a-project-to-a-docker-container"></a><span data-ttu-id="1b827-119">Dodawanie projektu do kontenera platformy Docker</span><span class="sxs-lookup"><span data-stu-id="1b827-119">Add a project to a Docker container</span></span>

<span data-ttu-id="1b827-120">Aby konteneryzowanie projekt ASP.NET Core, projekt musi być przeznaczony dla platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1b827-120">To containerize an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="1b827-121">Obsługiwane są kontenery systemu Linux i Windows.</span><span class="sxs-lookup"><span data-stu-id="1b827-121">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="1b827-122">Podczas dodawania obsługi platformy Docker do projektu wybierz kontener systemu Windows lub Linux.</span><span class="sxs-lookup"><span data-stu-id="1b827-122">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="1b827-123">Na hoście platformy Docker musi być uruchomiony ten sam typ kontenera.</span><span class="sxs-lookup"><span data-stu-id="1b827-123">The Docker host must be running the same container type.</span></span> <span data-ttu-id="1b827-124">Aby zmienić typ kontenera w uruchomionym wystąpieniu platformy Docker, kliknij prawym przyciskiem myszy ikonę Docker na pasku zadań i wybierz polecenie **Przełącz do kontenerów systemu Windows...** lub **Przejdź do kontenerów z systemem Linux..** ..</span><span class="sxs-lookup"><span data-stu-id="1b827-124">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="1b827-125">Nowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="1b827-125">New app</span></span>

<span data-ttu-id="1b827-126">Podczas tworzenia nowej aplikacji z szablonami projektu **aplikacji sieci Web ASP.NET Core** zaznacz pole wyboru **Włącz obsługę platformy Docker** :</span><span class="sxs-lookup"><span data-stu-id="1b827-126">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** check box:</span></span>

![Włącz obsługę platformy Docker — pole wyboru](visual-studio-tools-for-docker/_static/enable-docker-support-check-box.png)

<span data-ttu-id="1b827-128">Jeśli platformą docelową jest .NET Core, lista rozwijana **systemu operacyjnego** umożliwia wybór typu kontenera.</span><span class="sxs-lookup"><span data-stu-id="1b827-128">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="1b827-129">Istniejąca aplikacja</span><span class="sxs-lookup"><span data-stu-id="1b827-129">Existing app</span></span>

<span data-ttu-id="1b827-130">W przypadku projektów ASP.NET Core przeznaczonych dla platformy .NET Core dostępne są dwie opcje dodawania obsługi platformy Docker za pomocą narzędzi.</span><span class="sxs-lookup"><span data-stu-id="1b827-130">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="1b827-131">Otwórz projekt w programie Visual Studio, a następnie wybierz jedną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="1b827-131">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="1b827-132">Wybierz opcję **Obsługa platformy Docker** z menu **projekt** .</span><span class="sxs-lookup"><span data-stu-id="1b827-132">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="1b827-133">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Dodaj** > **obsługę platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="1b827-133">Right-click the project in **Solution Explorer** and select **Add** > **Docker Support**.</span></span>

<span data-ttu-id="1b827-134">Narzędzia kontenerów programu Visual Studio nie obsługują dodawania platformy Docker do istniejącego .NET Framework ASP.NET Core projektu.</span><span class="sxs-lookup"><span data-stu-id="1b827-134">The Visual Studio Container Tools don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span>

## <a name="dockerfile-overview"></a><span data-ttu-id="1b827-135">Pliku dockerfile — Omówienie</span><span class="sxs-lookup"><span data-stu-id="1b827-135">Dockerfile overview</span></span>

<span data-ttu-id="1b827-136">*Pliku dockerfile*, przepis dotyczący tworzenia końcowego obrazu platformy Docker, jest dodawany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="1b827-136">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="1b827-137">Zapoznaj się z dokumentacją [pliku dockerfile](https://docs.docker.com/engine/reference/builder/) , aby zrozumieć polecenia w nim.</span><span class="sxs-lookup"><span data-stu-id="1b827-137">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="1b827-138">W tym konkretnym *pliku dockerfile* jest stosowana wieloetapowa [kompilacja](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) z czterema różnymi etapami kompilacji o nazwie:</span><span class="sxs-lookup"><span data-stu-id="1b827-138">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) with four distinct, named build stages:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile.original?highlight=1,6,14,17)]

<span data-ttu-id="1b827-139">Poprzedni *pliku dockerfile* opiera się na obrazie [Microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/) .</span><span class="sxs-lookup"><span data-stu-id="1b827-139">The preceding *Dockerfile* is based on the [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/) image.</span></span> <span data-ttu-id="1b827-140">Ten obraz podstawowy obejmuje środowisko uruchomieniowe ASP.NET Core i pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="1b827-140">This base image includes the ASP.NET Core runtime and NuGet packages.</span></span> <span data-ttu-id="1b827-141">Pakiety są skompilowane just-in-Time (JIT), aby zwiększyć wydajność uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1b827-141">The packages are just-in-time (JIT) compiled to improve startup performance.</span></span>

<span data-ttu-id="1b827-142">Gdy pole wyboru Konfiguruj nowy projekt **dla protokołu HTTPS** jest zaznaczone, *pliku dockerfile* uwidacznia dwa porty.</span><span class="sxs-lookup"><span data-stu-id="1b827-142">When the new project dialog's **Configure for HTTPS** check box is checked, the *Dockerfile* exposes two ports.</span></span> <span data-ttu-id="1b827-143">Jeden port jest używany na potrzeby ruchu HTTP; drugi port jest używany w przypadku protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1b827-143">One port is used for HTTP traffic; the other port is used for HTTPS.</span></span> <span data-ttu-id="1b827-144">Jeśli pole wyboru nie jest zaznaczone, pojedynczy port (80) zostanie uwidoczniony dla ruchu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1b827-144">If the check box isn't checked, a single port (80) is exposed for HTTP traffic.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.0/HelloDockerTools/Dockerfile?highlight=1,5,13,16)]

<span data-ttu-id="1b827-145">Poprzedni *pliku dockerfile* opiera się na obrazie [Microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore/) .</span><span class="sxs-lookup"><span data-stu-id="1b827-145">The preceding *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore/) image.</span></span> <span data-ttu-id="1b827-146">Ten obraz podstawowy zawiera ASP.NET Core pakiety NuGet, które są skompilowane just-in-Time (JIT), aby zwiększyć wydajność uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1b827-146">This base image includes the ASP.NET Core NuGet packages, which are just-in-time (JIT) compiled to improve startup performance.</span></span>

::: moniker-end

## <a name="add-container-orchestrator-support-to-an-app"></a><span data-ttu-id="1b827-147">Dodawanie obsługi programu Orchestrator kontenera do aplikacji</span><span class="sxs-lookup"><span data-stu-id="1b827-147">Add container orchestrator support to an app</span></span>

<span data-ttu-id="1b827-148">Program Visual Studio 2017 w wersji 15,7 lub starszej [Docker Compose](https://docs.docker.com/compose/overview/) jako jedyne rozwiązanie aranżacji kontenerów.</span><span class="sxs-lookup"><span data-stu-id="1b827-148">Visual Studio 2017 versions 15.7 or earlier support [Docker Compose](https://docs.docker.com/compose/overview/) as the sole container orchestration solution.</span></span> <span data-ttu-id="1b827-149">Docker Compose artefakty są dodawane za pośrednictwem **dodawania** > **obsługi platformy Docker**.</span><span class="sxs-lookup"><span data-stu-id="1b827-149">The Docker Compose artifacts are added via **Add** > **Docker Support**.</span></span>

<span data-ttu-id="1b827-150">W programie Visual Studio 2017 w wersji 15,8 lub nowszej rozwiązanie aranżacji jest dodawane tylko wtedy, gdy jest to nakazujene.</span><span class="sxs-lookup"><span data-stu-id="1b827-150">Visual Studio 2017 versions 15.8 or later add an orchestration solution only when instructed.</span></span> <span data-ttu-id="1b827-151">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Dodaj** > **obsługę koordynatora kontenerów**.</span><span class="sxs-lookup"><span data-stu-id="1b827-151">Right-click the project in **Solution Explorer** and select **Add** > **Container Orchestrator Support**.</span></span> <span data-ttu-id="1b827-152">Oferowane są dwa różne opcje: [Docker Compose](#docker-compose) i [Service Fabric](#service-fabric).</span><span class="sxs-lookup"><span data-stu-id="1b827-152">Two different choices are offered: [Docker Compose](#docker-compose) and [Service Fabric](#service-fabric).</span></span>

### <a name="docker-compose"></a><span data-ttu-id="1b827-153">Docker Compose</span><span class="sxs-lookup"><span data-stu-id="1b827-153">Docker Compose</span></span>

<span data-ttu-id="1b827-154">Narzędzia kontenera programu Visual Studio dodają projekt *Docker-Zredaguj* do rozwiązania z następującymi plikami:</span><span class="sxs-lookup"><span data-stu-id="1b827-154">The Visual Studio Container Tools add a *docker-compose* project to the solution with the following files:</span></span>

* <span data-ttu-id="1b827-155">*Docker-Zredaguj. dcproj* &ndash; plik reprezentujący projekt.</span><span class="sxs-lookup"><span data-stu-id="1b827-155">*docker-compose.dcproj* &ndash; The file representing the project.</span></span> <span data-ttu-id="1b827-156">`<DockerTargetOS>` Zawiera element określający system operacyjny, który ma być używany.</span><span class="sxs-lookup"><span data-stu-id="1b827-156">Includes a `<DockerTargetOS>` element specifying the OS to be used.</span></span>
* <span data-ttu-id="1b827-157">*. dockerignore* &ndash; Wyświetla listę wzorców plików i katalogów, które mają zostać wykluczone podczas generowania kontekstu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b827-157">*.dockerignore* &ndash; Lists the file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="1b827-158">*Docker-Compose. yml* &ndash; podstawowy plik [Docker Compose](https://docs.docker.com/compose/overview/) używany do definiowania kolekcji obrazów skompilowanych i uruchamianych z `docker-compose build` i `docker-compose run`, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="1b827-158">*docker-compose.yml* &ndash; The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="1b827-159">*Docker-Compose. override. yml* &ndash; opcjonalny plik, Odczytaj według Docker Compose, z zastąpieniami konfiguracji dla usług.</span><span class="sxs-lookup"><span data-stu-id="1b827-159">*docker-compose.override.yml* &ndash; An optional file, read by Docker Compose, with configuration overrides for services.</span></span> <span data-ttu-id="1b827-160">Program Visual Studio `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` wykonuje scalanie tych plików.</span><span class="sxs-lookup"><span data-stu-id="1b827-160">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="1b827-161">Plik *Docker-Compose. yml* odwołuje się do nazwy obrazu, który jest tworzony podczas działania projektu:</span><span class="sxs-lookup"><span data-stu-id="1b827-161">The *docker-compose.yml* file references the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/2.0/docker-compose.yml?highlight=5)]

<span data-ttu-id="1b827-162">W poprzednim przykładzie program generuje `image: hellodockertools` obraz `hellodockertools:dev` , gdy aplikacja jest uruchamiana w trybie **debugowania** .</span><span class="sxs-lookup"><span data-stu-id="1b827-162">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="1b827-163">Obraz `hellodockertools:latest` jest generowany, gdy aplikacja jest uruchamiana w trybie **wydania** .</span><span class="sxs-lookup"><span data-stu-id="1b827-163">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="1b827-164">Przedrostek nazwy obrazu za pomocą nazwy użytkownika usługi [Docker Hub](https://hub.docker.com/) ( `dockerhubusername/hellodockertools`na przykład), jeśli obraz jest wypychany do rejestru.</span><span class="sxs-lookup"><span data-stu-id="1b827-164">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image is pushed to the registry.</span></span> <span data-ttu-id="1b827-165">Alternatywnie można zmienić nazwę obrazu tak, aby zawierała adres URL rejestru prywatnego (na `privateregistry.domain.com/hellodockertools`przykład), w zależności od konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1b827-165">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

<span data-ttu-id="1b827-166">Jeśli chcesz mieć inne zachowanie na podstawie konfiguracji kompilacji (na przykład debugowanie lub wydanie), Dodaj określone pliki *platformy Docker* .</span><span class="sxs-lookup"><span data-stu-id="1b827-166">If you want different behavior based on the build configuration (for example, Debug or Release), add configuration-specific *docker-compose* files.</span></span> <span data-ttu-id="1b827-167">Pliki powinny mieć nazwę zgodną z konfiguracją kompilacji (na przykład *Docker-Compose. vs. Debug. yml* i *Docker-Compose. vs. release. yml*) i umieszczaną w tej samej lokalizacji co plik *Docker-Compose-override. yml* .</span><span class="sxs-lookup"><span data-stu-id="1b827-167">The files should be named according to the build configuration (for example, *docker-compose.vs.debug.yml* and *docker-compose.vs.release.yml*) and placed in the same location as the *docker-compose-override.yml* file.</span></span> 

<span data-ttu-id="1b827-168">Przy użyciu plików przesłonięć specyficznych dla konfiguracji można określić różne ustawienia konfiguracji (takie jak zmienne środowiskowe lub punkty wejścia) dla konfiguracji debugowania i wydania kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b827-168">Using the configuration-specific override files, you can specify different configuration settings (such as environment variables or entry points) for Debug and Release build configurations.</span></span>

### <a name="service-fabric"></a><span data-ttu-id="1b827-169">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1b827-169">Service Fabric</span></span>

<span data-ttu-id="1b827-170">Oprócz podstawowych [wymagań wstępnych](#prerequisites), rozwiązanie [Service Fabric](/azure/service-fabric/) aranżacji wymaga następujących wymagań wstępnych:</span><span class="sxs-lookup"><span data-stu-id="1b827-170">In addition to the base [Prerequisites](#prerequisites), the [Service Fabric](/azure/service-fabric/) orchestration solution demands the following prerequisites:</span></span>

* <span data-ttu-id="1b827-171">[Zestaw SDK Microsoft Azure Service Fabric](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK) w wersji 2,6 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="1b827-171">[Microsoft Azure Service Fabric SDK](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK) version 2.6 or later</span></span>
* <span data-ttu-id="1b827-172">Obciążenie programistyczne **platformy Azure** dla programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b827-172">Visual Studio's **Azure Development** workload</span></span>

<span data-ttu-id="1b827-173">Service Fabric nie obsługuje uruchamiania kontenerów systemu Linux w lokalnym klastrze projektowym w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="1b827-173">Service Fabric doesn't support running Linux containers in the local development cluster on Windows.</span></span> <span data-ttu-id="1b827-174">Jeśli projekt używa już kontenera systemu Linux, program Visual Studio wyświetli komunikat z prośbą o przełączenie do kontenerów Windows.</span><span class="sxs-lookup"><span data-stu-id="1b827-174">If the project is already using a Linux container, Visual Studio prompts to switch to Windows containers.</span></span>

<span data-ttu-id="1b827-175">Narzędzia kontenera programu Visual Studio wykonują następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="1b827-175">The Visual Studio Container Tools do the following tasks:</span></span>

* <span data-ttu-id="1b827-176">Dodaje *aplikację&gt;Project_NameServiceFabricprojektu aplikacji do rozwiązania. &lt;*</span><span class="sxs-lookup"><span data-stu-id="1b827-176">Adds a *&lt;project_name&gt;Application* **Service Fabric Application** project to the solution.</span></span>
* <span data-ttu-id="1b827-177">Dodaje plik *pliku dockerfile* i *. dockerignore* do projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1b827-177">Adds a *Dockerfile* and a *.dockerignore* file to the ASP.NET Core project.</span></span> <span data-ttu-id="1b827-178">Jeśli *pliku dockerfile* już istnieje w projekcie ASP.NET Core, jego nazwa zostanie zmieniona na *pliku dockerfile. Original*.</span><span class="sxs-lookup"><span data-stu-id="1b827-178">If a *Dockerfile* already exists in the ASP.NET Core project, it's renamed to *Dockerfile.original*.</span></span> <span data-ttu-id="1b827-179">Tworzony jest nowy *pliku dockerfile*, podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="1b827-179">A new *Dockerfile*, similar to the following, is created:</span></span>

    [!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile)]

* <span data-ttu-id="1b827-180">Dodaje element do pliku csproj projektu ASP.NET Core:  `<IsServiceFabricServiceProject>`</span><span class="sxs-lookup"><span data-stu-id="1b827-180">Adds an `<IsServiceFabricServiceProject>` element to the ASP.NET Core project's *.csproj* file:</span></span>

    [!code-xml[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/HelloDockerTools.csproj?name=snippet_IsServiceFabricServiceProject)]

* <span data-ttu-id="1b827-181">Dodaje folder *PackageRoot* do projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1b827-181">Adds a *PackageRoot* folder to the ASP.NET Core project.</span></span> <span data-ttu-id="1b827-182">Folder zawiera manifest usługi i ustawienia dla nowej usługi.</span><span class="sxs-lookup"><span data-stu-id="1b827-182">The folder includes the service manifest and settings for the new service.</span></span>

<span data-ttu-id="1b827-183">Aby uzyskać więcej informacji, zobacz [wdrażanie aplikacji .NET w kontenerze systemu Windows na platformie Azure Service Fabric](/azure/service-fabric/service-fabric-host-app-in-a-container).</span><span class="sxs-lookup"><span data-stu-id="1b827-183">For more information, see [Deploy a .NET app in a Windows container to Azure Service Fabric](/azure/service-fabric/service-fabric-host-app-in-a-container).</span></span>

## <a name="debug"></a><span data-ttu-id="1b827-184">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="1b827-184">Debug</span></span>

<span data-ttu-id="1b827-185">Wybierz  pozycję Docker z listy rozwijanej Debuguj na pasku narzędzi i Rozpocznij debugowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1b827-185">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="1b827-186">Widok **platformy Docker** okna **dane wyjściowe** zawiera następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="1b827-186">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="1b827-187">Zostanie pobrany tag *2,1-aspnetcore-Runtime* obrazu środowiska uruchomieniowego *Microsoft/dotnet* (jeśli jeszcze nie znajduje się w pamięci podręcznej).</span><span class="sxs-lookup"><span data-stu-id="1b827-187">The *2.1-aspnetcore-runtime* tag of the *microsoft/dotnet* runtime image is acquired (if not already in the cache).</span></span> <span data-ttu-id="1b827-188">Obraz instaluje ASP.NET Core i środowiska uruchomieniowe platformy .NET Core oraz powiązane biblioteki.</span><span class="sxs-lookup"><span data-stu-id="1b827-188">The image installs the ASP.NET Core and .NET Core runtimes and associated libraries.</span></span> <span data-ttu-id="1b827-189">Jest zoptymalizowany pod kątem uruchamiania aplikacji ASP.NET Core w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="1b827-189">It's optimized for running ASP.NET Core apps in production.</span></span>
* <span data-ttu-id="1b827-190">Zmienna środowiskowa jest ustawiona na `Development` wewnątrz kontenera. `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="1b827-190">The `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="1b827-191">Dostępne są dwa porty przypisane dynamicznie: jeden dla protokołu HTTP i jeden dla protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1b827-191">Two dynamically assigned ports are exposed: one for HTTP and one for HTTPS.</span></span> <span data-ttu-id="1b827-192">Do portu przypisanego do hosta lokalnego można wykonać zapytanie za `docker ps` pomocą polecenia.</span><span class="sxs-lookup"><span data-stu-id="1b827-192">The port assigned to localhost can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="1b827-193">Aplikacja jest kopiowana do kontenera.</span><span class="sxs-lookup"><span data-stu-id="1b827-193">The app is copied to the container.</span></span>
* <span data-ttu-id="1b827-194">Domyślna przeglądarka jest uruchamiana z debugerem dołączonym do kontenera przy użyciu dynamicznie przypisanego portu.</span><span class="sxs-lookup"><span data-stu-id="1b827-194">The default browser is launched with the debugger attached to the container using the dynamically assigned port.</span></span>

<span data-ttu-id="1b827-195">Utworzony obraz platformy Docker aplikacji jest otagowany jako *dev*.</span><span class="sxs-lookup"><span data-stu-id="1b827-195">The resulting Docker image of the app is tagged as *dev*.</span></span> <span data-ttu-id="1b827-196">Obraz jest oparty na tagu *2,1-aspnetcore-Runtime* obrazu podstawowego *Microsoft/dotnet* .</span><span class="sxs-lookup"><span data-stu-id="1b827-196">The image is based on the *2.1-aspnetcore-runtime* tag of the *microsoft/dotnet* base image.</span></span> <span data-ttu-id="1b827-197">Uruchom polecenie w oknie **konsola Menedżera pakietów** (PMC). `docker images`</span><span class="sxs-lookup"><span data-stu-id="1b827-197">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="1b827-198">Wyświetlane są obrazy na komputerze:</span><span class="sxs-lookup"><span data-stu-id="1b827-198">The images on the machine are displayed:</span></span>

```console
REPOSITORY        TAG                     IMAGE ID      CREATED         SIZE
hellodockertools  dev                     d72ce0f1dfe7  30 seconds ago  255MB
microsoft/dotnet  2.1-aspnetcore-runtime  fcc3887985bb  6 days ago      255MB
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <span data-ttu-id="1b827-199">Obraz środowiska uruchomieniowego *Microsoft/aspnetcore* został pobrany (jeśli nie jest jeszcze w pamięci podręcznej).</span><span class="sxs-lookup"><span data-stu-id="1b827-199">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="1b827-200">Zmienna środowiskowa jest ustawiona na `Development` wewnątrz kontenera. `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="1b827-200">The `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="1b827-201">Port 80 jest uwidoczniony i mapowany na dynamicznie przypisany port dla hosta lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1b827-201">Port 80 is exposed and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="1b827-202">Port jest określany przez hosta platformy Docker i można wykonać zapytania przy użyciu `docker ps` polecenia.</span><span class="sxs-lookup"><span data-stu-id="1b827-202">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="1b827-203">Aplikacja jest kopiowana do kontenera.</span><span class="sxs-lookup"><span data-stu-id="1b827-203">The app is copied to the container.</span></span>
* <span data-ttu-id="1b827-204">Domyślna przeglądarka jest uruchamiana z debugerem dołączonym do kontenera przy użyciu dynamicznie przypisanego portu.</span><span class="sxs-lookup"><span data-stu-id="1b827-204">The default browser is launched with the debugger attached to the container using the dynamically assigned port.</span></span>

<span data-ttu-id="1b827-205">Utworzony obraz platformy Docker aplikacji jest otagowany jako *dev*.</span><span class="sxs-lookup"><span data-stu-id="1b827-205">The resulting Docker image of the app is tagged as *dev*.</span></span> <span data-ttu-id="1b827-206">Obraz jest oparty na obrazie bazowym *Microsoft/aspnetcore* .</span><span class="sxs-lookup"><span data-stu-id="1b827-206">The image is based on the *microsoft/aspnetcore* base image.</span></span> <span data-ttu-id="1b827-207">Uruchom polecenie w oknie **konsola Menedżera pakietów** (PMC). `docker images`</span><span class="sxs-lookup"><span data-stu-id="1b827-207">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="1b827-208">Wyświetlane są obrazy na komputerze:</span><span class="sxs-lookup"><span data-stu-id="1b827-208">The images on the machine are displayed:</span></span>

```console
REPOSITORY            TAG  IMAGE ID      CREATED        SIZE
hellodockertools      dev  5fafe5d1ad5b  4 minutes ago  347MB
microsoft/aspnetcore  2.0  c69d39472da9  13 days ago    347MB
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="1b827-209">Obraz *deweloperski* nie zawiera zawartości aplikacji, ponieważ konfiguracje **debugowania** używają instalacji woluminu do zapewnienia iteracyjnego środowiska.</span><span class="sxs-lookup"><span data-stu-id="1b827-209">The *dev* image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="1b827-210">Aby wypchnąć obraz, użyj konfiguracji **wydania** .</span><span class="sxs-lookup"><span data-stu-id="1b827-210">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="1b827-211">`docker ps` Uruchom polecenie w PMC.</span><span class="sxs-lookup"><span data-stu-id="1b827-211">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="1b827-212">Zauważ, że aplikacja jest uruchomiona przy użyciu kontenera:</span><span class="sxs-lookup"><span data-stu-id="1b827-212">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="1b827-213">Edytuj i Kontynuuj</span><span class="sxs-lookup"><span data-stu-id="1b827-213">Edit and continue</span></span>

<span data-ttu-id="1b827-214">Zmiany w plikach statycznych i widokach Razor są automatycznie aktualizowane bez konieczności wykonywania kroku kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b827-214">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="1b827-215">Wprowadź zmiany, Zapisz i Odśwież przeglądarkę, aby wyświetlić aktualizację.</span><span class="sxs-lookup"><span data-stu-id="1b827-215">Make the change, save, and refresh the browser to view the update.</span></span>

<span data-ttu-id="1b827-216">Modyfikacje pliku kodu wymagają kompilacji i ponownego uruchomienia Kestrel w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="1b827-216">Code file modifications require compilation and a restart of Kestrel within the container.</span></span> <span data-ttu-id="1b827-217">Po wprowadzeniu zmiany Użyj `CTRL+F5` do wykonania procesu i uruchomienia aplikacji w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="1b827-217">After making the change, use `CTRL+F5` to perform the process and start the app within the container.</span></span> <span data-ttu-id="1b827-218">Kontener platformy Docker nie został odbudowany lub zatrzymany.</span><span class="sxs-lookup"><span data-stu-id="1b827-218">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="1b827-219">`docker ps` Uruchom polecenie w PMC.</span><span class="sxs-lookup"><span data-stu-id="1b827-219">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="1b827-220">Zwróć uwagę, że oryginalny kontener nadal działa po 10 minutach temu:</span><span class="sxs-lookup"><span data-stu-id="1b827-220">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="1b827-221">Publikowanie obrazów platformy Docker</span><span class="sxs-lookup"><span data-stu-id="1b827-221">Publish Docker images</span></span>

<span data-ttu-id="1b827-222">Po zakończeniu cyklu opracowywania i debugowania aplikacji narzędzia kontenerów programu Visual Studio ułatwiają tworzenie obrazu produkcyjnego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1b827-222">Once the develop and debug cycle of the app is completed, the Visual Studio Container Tools assist in creating the production image of the app.</span></span> <span data-ttu-id="1b827-223">Zmień listę rozwijaną konfiguracji, aby **zwolnić** i skompilować aplikację.</span><span class="sxs-lookup"><span data-stu-id="1b827-223">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="1b827-224">Narzędzie uzyskuje obraz kompilowania/publikowania z usługi Docker Hub (jeśli jeszcze nie znajduje się w pamięci podręcznej).</span><span class="sxs-lookup"><span data-stu-id="1b827-224">The tooling acquires the compile/publish image from Docker Hub (if not already in the cache).</span></span> <span data-ttu-id="1b827-225">Obraz jest tworzony przy użyciu *najnowszego* tagu, który można wypchnąć do rejestru prywatnego lub z koncentratora platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="1b827-225">An image is produced with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span>

<span data-ttu-id="1b827-226">`docker images` Uruchom polecenie w kryterium PMC, aby wyświetlić listę obrazów.</span><span class="sxs-lookup"><span data-stu-id="1b827-226">Run the `docker images` command in PMC to see the list of images.</span></span> <span data-ttu-id="1b827-227">Wyświetlane są dane wyjściowe podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="1b827-227">Output similar to the following is displayed:</span></span>

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

<span data-ttu-id="1b827-228">Obrazy `microsoft/aspnetcore-build` i `microsoft/aspnetcore` wymienione w`microsoft/dotnet` poprzednich danych wyjściowych są zastępowane obrazami programu .NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="1b827-228">The `microsoft/aspnetcore-build` and `microsoft/aspnetcore` images listed in the preceding output are replaced with `microsoft/dotnet` images as of .NET Core 2.1.</span></span> <span data-ttu-id="1b827-229">Aby uzyskać więcej informacji, zobacz [anons migracji repozytoriów platformy Docker](https://github.com/aspnet/Announcements/issues/298).</span><span class="sxs-lookup"><span data-stu-id="1b827-229">For more information, see [the Docker repositories migration announcement](https://github.com/aspnet/Announcements/issues/298).</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="1b827-230">Polecenie zwraca obrazy pośredniczące z nazwami repozytoriów i tagami identyfikowanych jako  *\<brak >* (niewymienione powyżej). `docker images`</span><span class="sxs-lookup"><span data-stu-id="1b827-230">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="1b827-231">Te obrazy bez nazwy są tworzone przez Wieloetapową [kompilację](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *pliku dockerfile*.</span><span class="sxs-lookup"><span data-stu-id="1b827-231">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="1b827-232">Zwiększają one wydajność tworzenia obrazu&mdash;końcowego tylko wtedy, gdy nastąpi zmiana.</span><span class="sxs-lookup"><span data-stu-id="1b827-232">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="1b827-233">Gdy obrazy pośredniczące nie są już potrzebne, usuń je za pomocą polecenia [Docker RMI](https://docs.docker.com/engine/reference/commandline/rmi/) .</span><span class="sxs-lookup"><span data-stu-id="1b827-233">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="1b827-234">Może się okazać, że oczekuje się, że rozmiar obrazu produkcyjnego lub wydania będzie mniejszy w porównaniu do obrazu *deweloperskiego* .</span><span class="sxs-lookup"><span data-stu-id="1b827-234">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="1b827-235">Ze względu na mapowanie woluminu debuger i aplikacja działały z komputera lokalnego, a nie w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="1b827-235">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="1b827-236">*Najnowszy* obraz zawiera spakowany kod aplikacji, aby uruchomić aplikację na komputerze hosta.</span><span class="sxs-lookup"><span data-stu-id="1b827-236">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="1b827-237">W związku z tym Delta jest rozmiarem kodu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1b827-237">Therefore, the delta is the size of the app code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1b827-238">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1b827-238">Additional resources</span></span>

* [<span data-ttu-id="1b827-239">Programowanie kontenerów za pomocą programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b827-239">Container development with Visual Studio</span></span>](/visualstudio/containers)
* [<span data-ttu-id="1b827-240">Service Fabric platformy Azure: Przygotuj środowisko programistyczne</span><span class="sxs-lookup"><span data-stu-id="1b827-240">Azure Service Fabric: Prepare your development environment</span></span>](/azure/service-fabric/service-fabric-get-started)
* [<span data-ttu-id="1b827-241">Wdrażanie aplikacji .NET w kontenerze systemu Windows na platformie Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1b827-241">Deploy a .NET app in a Windows container to Azure Service Fabric</span></span>](/azure/service-fabric/service-fabric-host-app-in-a-container)
* [<span data-ttu-id="1b827-242">Rozwiązywanie problemów z programowaniem programu Visual Studio przy użyciu platformy Docker</span><span class="sxs-lookup"><span data-stu-id="1b827-242">Troubleshoot Visual Studio development with Docker</span></span>](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [<span data-ttu-id="1b827-243">Repozytorium usługi GitHub dla narzędzi kontenerów programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b827-243">Visual Studio Container Tools GitHub repository</span></span>](https://github.com/Microsoft/DockerTools)
