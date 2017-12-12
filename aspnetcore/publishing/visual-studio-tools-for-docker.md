---
title: Visual Studio Tools for Docker z platformy ASP.NET Core
description: "W tym artykule przedstawiono containerize aplikacji platformy ASP.NET Core za pomocą narzędzi Visual Studio 2017 i Docker dla systemu Windows."
keywords: Docker,ASP.NET kontenera Core, programu Visual Studio
author: spboyer
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.prod: asp.net-core
ms.technology: aspnet
ms.assetid: 1f3b9a68-4dea-4b60-8cb3-f46164eedbbf
uid: publishing/vs-tools-for-docker
ms.openlocfilehash: 2d8e337141ae4e0d0258f1d7546510b0ab077e39
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="visual-studio-tools-for-docker"></a><span data-ttu-id="984e4-104">Visual Studio Tools for Docker</span><span class="sxs-lookup"><span data-stu-id="984e4-104">Visual Studio Tools for Docker</span></span>

<span data-ttu-id="984e4-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) z [Docker dla systemu Windows](https://docs.docker.com/docker-for-windows/install/) obsługuje kompilowania, debugowania i uruchamiania .NET Framework i .NET Core aplikacji sieci web i konsoli przy użyciu kontenerów systemu Windows i Linux.</span><span class="sxs-lookup"><span data-stu-id="984e4-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with [Docker for Windows](https://docs.docker.com/docker-for-windows/install/) supports building, debugging, and running .NET Framework and .NET Core web and console applications using Windows and Linux containers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="984e4-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="984e4-106">Prerequisites</span></span>

- <span data-ttu-id="984e4-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) z obciążenia .NET Core</span><span class="sxs-lookup"><span data-stu-id="984e4-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with .NET Core workload</span></span>
- [<span data-ttu-id="984e4-108">Docker dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="984e4-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="984e4-109">Instalacja i Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="984e4-109">Installation and setup</span></span>

<span data-ttu-id="984e4-110">Zainstaluj [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) z obciążeniem, .NET Core.</span><span class="sxs-lookup"><span data-stu-id="984e4-110">Install [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) with the .NET Core workload.</span></span> <span data-ttu-id="984e4-111">Jeśli masz już zainstalowanego programu Visual Studio, możesz [Zmodyfikuj instalację programu Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) można dodać obciążenia .NET Core.</span><span class="sxs-lookup"><span data-stu-id="984e4-111">If you already have Visual Studio installed, you can [modify your Visual Studio installation](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) to add the .NET Core workload.</span></span>

<span data-ttu-id="984e4-112">Docker instalacji, przejrzyj informacje w [Docker dla systemu Windows: co należy wiedzieć przed zainstalowaniem](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) i zainstaluj [Docker dla systemu Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="984e4-112">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="984e4-113">Instalator jest wymagana konfiguracja  **[udostępnione dyski](https://docs.docker.com/docker-for-windows/#shared-drives)**  Docker dla systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="984e4-113">A required configuration is to setup **[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows.</span></span> <span data-ttu-id="984e4-114">Ustawienie jest wymagane dla woluminu mapowania i debugowanie pomocy technicznej.</span><span class="sxs-lookup"><span data-stu-id="984e4-114">The setting is required for the volume mapping and debugging support.</span></span>

<span data-ttu-id="984e4-115">Kliknij prawym przyciskiem myszy ikonę Docker na pasku zadań, kliknij przycisk **ustawienia**i wybierz **udostępnione dyski**.</span><span class="sxs-lookup"><span data-stu-id="984e4-115">Right-click the Docker icon in the System Tray, click **Settings**, and select **Shared Drives**.</span></span> <span data-ttu-id="984e4-116">Wybierz dysk, gdzie Docker będzie przechowywać pliki i zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="984e4-116">Select the drive where Docker will store your files and apply changes.</span></span>

![Dyski udostępnione](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

## <a name="create-an-aspnet-web-application-and-add-docker-support"></a><span data-ttu-id="984e4-118">Tworzenie aplikacji sieci Web ASP.NET i dodanie obsługi Docker</span><span class="sxs-lookup"><span data-stu-id="984e4-118">Create an ASP.NET Web Application and add Docker Support</span></span>

<span data-ttu-id="984e4-119">Za pomocą programu Visual Studio, Utwórz nową aplikację sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="984e4-119">Using Visual Studio, create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="984e4-120">Podczas ładowania aplikacji, albo wybrać **dodać obsługę Docker** z **Menu Projekt** lub kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz polecenie **Dodaj**  >  **Obsługa docker**.</span><span class="sxs-lookup"><span data-stu-id="984e4-120">When the application is loaded, either select **Add Docker Support** from the **Project Menu** or right-click the project from the Solution Explorer and select **Add** > **Docker Support**.</span></span>

<span data-ttu-id="984e4-121">*Menu Projekt*</span><span class="sxs-lookup"><span data-stu-id="984e4-121">*Project Menu*</span></span>

![Projekt dodać obsługę Docker](./visual-studio-tools-for-docker/_static/project-add-docker-support.png)

<span data-ttu-id="984e4-123">*Menu kontekstowe projektu*</span><span class="sxs-lookup"><span data-stu-id="984e4-123">*Project Context Menu*</span></span>

![Kliknij prawym przyciskiem myszy dodać obsługę Docker](./visual-studio-tools-for-docker/_static/right-click-add-docker-support.png)

<span data-ttu-id="984e4-125">Po dodaniu obsługi Docker do projektu można kontenery systemu Windows lub Linux.</span><span class="sxs-lookup"><span data-stu-id="984e4-125">When you add Docker support to your project, you can choose either Windows or Linux containers.</span></span> <span data-ttu-id="984e4-126">(Hostów Docker musi być uruchomiona ten sam typ kontenera.</span><span class="sxs-lookup"><span data-stu-id="984e4-126">(The Docker host must be running the same container type.</span></span> <span data-ttu-id="984e4-127">Jeśli należy zmienić typ kontenera w uruchomione wystąpienie Docker, kliknij prawym przyciskiem myszy **Docker** ikonę na pasku zadań i wybierz polecenie **przełączyć się do systemu Windows kontenery** lub **przełączyć się do systemu Linux kontenery**.)</span><span class="sxs-lookup"><span data-stu-id="984e4-127">If you need change the container type in the running Docker instance, right-click the **Docker** icon in the System Tray, and choose **Switch to Windows containers** or **Switch to Linux containers**.)</span></span> 

<span data-ttu-id="984e4-128">Następujące pliki zostały dodane do projektu:</span><span class="sxs-lookup"><span data-stu-id="984e4-128">The following files are added to the project:</span></span>

- <span data-ttu-id="984e4-129">**Plik Dockerfile**: plik Docker dla aplikacji platformy ASP.NET Core opiera się na [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) obrazu.</span><span class="sxs-lookup"><span data-stu-id="984e4-129">**Dockerfile**: the Docker file for ASP.NET Core applications is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="984e4-130">Ten obraz zawiera pakietów platformy ASP.NET Core NuGet, które zostały wstępnie przy użyciu kompilatora JIT poprawę wydajności uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="984e4-130">This image includes the ASP.NET Core NuGet packages, which have been pre-jitted improving startup performance.</span></span> <span data-ttu-id="984e4-131">Podczas tworzenia aplikacji konsoli .NET Core, plik Dockerfile z będzie odwoływać się do najnowszej [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) obrazu.</span><span class="sxs-lookup"><span data-stu-id="984e4-131">When building .NET Core Console Applications, the Dockerfile FROM will reference the most recent [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) image.</span></span>   
- <span data-ttu-id="984e4-132">**docker-compose.yml**: podstawowy plik rozwiązania Docker Compose używane do definiowania kolekcji obrazów być skompilowany i uruchom z docker-tworzą kompilacji/Uruchom.</span><span class="sxs-lookup"><span data-stu-id="984e4-132">**docker-compose.yml**: base Docker Compose file used to define the collection of images to be built and run with docker-compose build/run.</span></span>   
- <span data-ttu-id="984e4-133">**docker compose.dev.debug.yml**: dodatkowe rozwiązania docker compose plik o zmiany iteracyjne ustawianą konfiguracji debugowania.</span><span class="sxs-lookup"><span data-stu-id="984e4-133">**docker-compose.dev.debug.yml**: additional docker-compose file with for iterative changes when your configuration is set to debug.</span></span> <span data-ttu-id="984e4-134">Visual Studio będzie wywoływać docker-compose.yml - f -f docker-compose.dev.debug.yml scalenie tych razem.</span><span class="sxs-lookup"><span data-stu-id="984e4-134">Visual Studio will call -f docker-compose.yml -f docker-compose.dev.debug.yml to merge these together.</span></span> <span data-ttu-id="984e4-135">Ten plik Redaguj jest używany przez narzędzia deweloperskie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="984e4-135">This compose file is used by Visual Studio development tools.</span></span>   
- <span data-ttu-id="984e4-136">**docker compose.dev.release.yml**: dodatkowego pliku rozwiązania Docker Compose do debugowania z definicji wersji.</span><span class="sxs-lookup"><span data-stu-id="984e4-136">**docker-compose.dev.release.yml**: additional Docker Compose file to debug your release definition.</span></span> <span data-ttu-id="984e4-137">Go będzie instalacji woluminu debugera, nie powoduje zmiany zawartości obrazu produkcji.</span><span class="sxs-lookup"><span data-stu-id="984e4-137">It will volume mount the debugger so it doesn't change the contents of the production image.</span></span>  

<span data-ttu-id="984e4-138">*Docker-compose.yml* plik zawiera nazwę obrazu, który jest tworzony w momencie uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="984e4-138">The *docker-compose.yml* file contains the name of the image that is created when project is run.</span></span> 

```
version '2'

services:
  hellodockertools:
    image:  user/hellodockertools${TAG}
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80"
``` 

<span data-ttu-id="984e4-139">W tym przykładzie `image: user/hellodockertools${TAG}` generuje obraz `user/hellodockertools:dev` po uruchomieniu aplikacji **debugowania** tryb i `user/hellodockertools:latest` w **wersji** tryb odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="984e4-139">In this example, `image: user/hellodockertools${TAG}` generates the image `user/hellodockertools:dev` when the application is run in **Debug** mode and `user/hellodockertools:latest` in **Release** mode respectively.</span></span> 

<span data-ttu-id="984e4-140">Można zmienić `user` do Twojej [Centrum Docker](https://hub.docker.com/) nazwy użytkownika, jeśli planujesz push obrazu w rejestrze.</span><span class="sxs-lookup"><span data-stu-id="984e4-140">You will want to change the `user` to your [Docker Hub](https://hub.docker.com/) username if you plan to push the image to the registry.</span></span> <span data-ttu-id="984e4-141">Na przykład `spboyer/hellodockertools`, lub zmień adres URL prywatnych rejestru `privateregistry.domain.com/` w zależności od konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="984e4-141">For example, `spboyer/hellodockertools`, or change to your private registry URL `privateregistry.domain.com/` depending on your configuration.</span></span>

### <a name="debugging"></a><span data-ttu-id="984e4-142">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="984e4-142">Debugging</span></span>

<span data-ttu-id="984e4-143">Wybierz **Docker** z listy rozwijanej w pasku narzędzi i Użyj F5, aby rozpocząć debugowanie aplikacji debugowania.</span><span class="sxs-lookup"><span data-stu-id="984e4-143">Select **Docker** from the debug drop-down in the toolbar and use F5 to start debugging the application.</span></span> 

- <span data-ttu-id="984e4-144">*Microsoft/aspnetcore* obrazu są uzyskiwane (Jeśli nie jest jeszcze w pamięci podręcznej)</span><span class="sxs-lookup"><span data-stu-id="984e4-144">The *microsoft/aspnetcore* image is acquired (if not already in your cache)</span></span>
- <span data-ttu-id="984e4-145">*ASPNETCORE_ENVIRONMENT* ma ustawioną wartość Programowanie w kontenerze</span><span class="sxs-lookup"><span data-stu-id="984e4-145">*ASPNETCORE_ENVIRONMENT* is set to Development within the container</span></span>
- <span data-ttu-id="984e4-146">PORT 80 jest UWIDACZNIANY i mapować do portu przypisywany dynamicznie hosta lokalnego.</span><span class="sxs-lookup"><span data-stu-id="984e4-146">PORT 80 is EXPOSED and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="984e4-147">Port jest określana przez hosta docker i mogą być przeszukiwane przy docker ps.</span><span class="sxs-lookup"><span data-stu-id="984e4-147">The port is determined by the docker host and can be queried with docker ps.</span></span> 
- <span data-ttu-id="984e4-148">Aplikacja jest kopiowany do kontenera</span><span class="sxs-lookup"><span data-stu-id="984e4-148">Your application is copied to the container</span></span>
- <span data-ttu-id="984e4-149">Domyślna przeglądarka jest uruchamiana w debugerze w kontenerze, za pomocą portu przypisywany dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="984e4-149">Default browser is launched with the debugger attached to the container, using the dynamically assigned port.</span></span> 

<span data-ttu-id="984e4-150">Wynikowy obraz Docker wbudowane jest *deweloperów* obrazu aplikacji za pomocą *microsoft/aspnetcore* obrazów jako obrazu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="984e4-150">The resulting Docker image built is the *dev* image of your application with the *microsoft/aspnetcore* images as the base image.</span></span>

<span data-ttu-id="984e4-151">**Uwaga:** obraz deweloperów jest pusty zawartości Twojej aplikacji zgodnie z konfiguracji debugowania Użyj zamontowania woluminu, aby zapewnić środowisko iteracji.</span><span class="sxs-lookup"><span data-stu-id="984e4-151">**Note:** The dev image is empty of your app contents as Debug configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="984e4-152">Aby wypchnąć obrazu, Użyj konfiguracji wydanie.</span><span class="sxs-lookup"><span data-stu-id="984e4-152">To push an image, use the Release configuration.</span></span>

```console
REPOSITORY                  TAG         IMAGE ID            CREATED         SIZE
spboyer/hellodockertools    dev         0b6e2e44b3df        4 minutes ago   268.9 MB
microsoft/aspnetcore        1.0.1       189ad4312ce7        5 days ago      268.9 MB
```

<span data-ttu-id="984e4-153">Aplikacja została uruchomiona z użyciem kontenera, w którym można zobaczyć, uruchamiając `docker ps` polecenia.</span><span class="sxs-lookup"><span data-stu-id="984e4-153">The application is running using the container, which you can see by running the `docker ps` command.</span></span>

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   4 minutes ago       Up 4 minutes        0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="edit-and-continue"></a><span data-ttu-id="984e4-154">Edytuj i kontynuuj</span><span class="sxs-lookup"><span data-stu-id="984e4-154">Edit and Continue</span></span>

<span data-ttu-id="984e4-155">Zmienia się na pliki statyczne i/lub pliki szablonów razor (*.cshtml*) są automatycznie aktualizowane bez konieczności kroku kompilacji.</span><span class="sxs-lookup"><span data-stu-id="984e4-155">Changes to static files and/or razor template files (*.cshtml*) are automatically updated without the need of a compilation step.</span></span> <span data-ttu-id="984e4-156">Wprowadzić zmiany, Zapisz, a następnie naciśnij przycisk Odśwież w przeglądarce, aby wyświetlić tę aktualizację.</span><span class="sxs-lookup"><span data-stu-id="984e4-156">Make the change, save, and tap refresh in the browser to view the update.</span></span>  

<span data-ttu-id="984e4-157">Modyfikacje plików kodu wymagają kompilacja i ponowne uruchomienie Kestrel w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="984e4-157">Modifications to code files require compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="984e4-158">Po wprowadzeniu zmian, użyj CTRL + F5, aby wykonać proces i uruchomić aplikację w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="984e4-158">After making the change, use CTRL + F5 to perform the process and start the application within the container.</span></span> <span data-ttu-id="984e4-159">Kontener Docker nie jest ponownie skompilowany lub zatrzymana; przy użyciu `docker ps` w wierszu polecenia widoczny oryginalnego kontenera nadal działa na 10 minut temu.</span><span class="sxs-lookup"><span data-stu-id="984e4-159">The Docker container is not rebuilt or stopped; using `docker ps` in the command line, you can see that the original container is still running as of 10 minutes ago.</span></span> 

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   10 minutes ago      Up 10 minutes       0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="publishing-docker-images"></a><span data-ttu-id="984e4-160">Obrazy usługi Docker publikowania</span><span class="sxs-lookup"><span data-stu-id="984e4-160">Publishing Docker images</span></span>

<span data-ttu-id="984e4-161">Po zakończeniu cyklu opracowanie i debugowania aplikacji Visual Studio Tools for Docker pomoże utworzyć obraz produkcyjnej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="984e4-161">Once you have completed the develop and debug cycle of your application, the Visual Studio Tools for Docker will help you create the production image of your application.</span></span> <span data-ttu-id="984e4-162">Zmiana listy rozwijanej debugowania, aby **wersji** i kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="984e4-162">Change the debug dropdown to **Release** and build the application.</span></span> <span data-ttu-id="984e4-163">Narzędzia utworzy obraz z `:latest` tag, którego możesz wypchnąć do prywatnego rejestru lub Centrum Docker.</span><span class="sxs-lookup"><span data-stu-id="984e4-163">The tooling will produce the image with the `:latest` tag which you can push to your private registry or Docker Hub.</span></span> 

<span data-ttu-id="984e4-164">Przy użyciu `docker images` polecenia, można zapoznać się z listą obrazów.</span><span class="sxs-lookup"><span data-stu-id="984e4-164">Using the `docker images` command, you can see the list of images.</span></span>

```console
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
spboyer/hellodockertools   latest              8184ae38ba91        5 seconds ago       278.4 MB
spboyer/hellodockertools   dev                 0b6e2e44b3df        About an hour ago   268.9 MB
microsoft/aspnetcore       1.0.1               189ad4312ce7        5 days ago          268.9 MB
```

<span data-ttu-id="984e4-165">Może być oczekiwanie środowisko produkcyjne lub wersji obrazu na mniejszy rozmiar w porównaniu do **deweloperów** obraz; jednak przy użyciu mapowania woluminu debugera i aplikacji zostały faktycznie uruchamiana z sieci lokalnej komputera, a nie w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="984e4-165">There may be an expectation for the production or release image to be smaller in size by comparison to the **dev** image; however, through the use of the volume mapping, the debugger and application were actually being run from your local machine and not within the container.</span></span> <span data-ttu-id="984e4-166">**Najnowsze** obraz ma spakowane kodu całej aplikacji potrzebne do uruchomienia aplikacji na maszynie hosta, w związku z tym różnicowej jest rozmiar kodu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="984e4-166">The **latest** image has packaged the entire application code needed to run the application on a host machine, therefore the delta is the size of your application code.</span></span>
