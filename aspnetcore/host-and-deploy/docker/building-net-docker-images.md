---
title: Obrazy platformy docker dla platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak używać opublikowanych obrazów Docker w programie .NET Core z rejestru platformy Docker. Ściąganie obrazów i tworzenie własnych obrazów.
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/09/2019
uid: host-and-deploy/docker/building-net-docker-images
ms.openlocfilehash: 48fc53a4c2139960c0f696af5732ff68fc6c4b8a
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65451011"
---
# <a name="docker-images-for-aspnet-core"></a><span data-ttu-id="7d5b2-104">Obrazy platformy docker dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d5b2-104">Docker images for ASP.NET Core</span></span>

<span data-ttu-id="7d5b2-105">W tym samouczku pokazano, jak uruchomić aplikację ASP.NET Core w kontenerach platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-105">This tutorial shows how to run an ASP.NET Core app in Docker containers.</span></span>

<span data-ttu-id="7d5b2-106">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-106">In this tutorial, you:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="7d5b2-107">Więcej informacji na temat obrazów platformy Docker programu Microsoft .NET Core</span><span class="sxs-lookup"><span data-stu-id="7d5b2-107">Learn about Microsoft .NET Core Docker images</span></span> 
> * <span data-ttu-id="7d5b2-108">Pobierz przykładową aplikację platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d5b2-108">Download an ASP.NET Core sample app</span></span>
> * <span data-ttu-id="7d5b2-109">Uruchamianie przykładowej aplikacji lokalnie</span><span class="sxs-lookup"><span data-stu-id="7d5b2-109">Run the sample app locally</span></span>
> * <span data-ttu-id="7d5b2-110">Uruchamianie przykładowej aplikacji w kontenerach systemu Linux</span><span class="sxs-lookup"><span data-stu-id="7d5b2-110">Run the sample app in Linux containers</span></span>
> * <span data-ttu-id="7d5b2-111">Uruchamianie przykładowej aplikacji w kontenerach Windows</span><span class="sxs-lookup"><span data-stu-id="7d5b2-111">Run the sample app in Windows containers</span></span>
> * <span data-ttu-id="7d5b2-112">Tworzenie i wdrażanie ręcznie</span><span class="sxs-lookup"><span data-stu-id="7d5b2-112">Build and deploy manually</span></span>

## <a name="aspnet-core-docker-images"></a><span data-ttu-id="7d5b2-113">Obrazy platformy Docker Core ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7d5b2-113">ASP.NET Core Docker images</span></span>

<span data-ttu-id="7d5b2-114">Na potrzeby tego samouczka możesz pobrać przykładową aplikację platformy ASP.NET Core i uruchom go w kontenerach platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-114">For this tutorial, you download an ASP.NET Core sample app and run it in Docker containers.</span></span> <span data-ttu-id="7d5b2-115">Przykład współdziała z kontenerów systemów Linux i Windows.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-115">The sample works with both Linux and Windows containers.</span></span>

<span data-ttu-id="7d5b2-116">Przykładowy plik Dockerfile używa [wieloetapowych platformy Docker, tworzenie funkcji](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) Aby skompilować i uruchomić w różnych kontenerach.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-116">The sample Dockerfile uses the [Docker multi-stage build feature](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) to build and run in different containers.</span></span> <span data-ttu-id="7d5b2-117">Kompilowanie i uruchom kontenery są tworzone na podstawie obrazów, które znajdują się w usłudze Docker Hub przez firmę Microsoft:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-117">The build and run containers are created from images that are provided in Docker Hub by Microsoft:</span></span>

* `dotnet/core/sdk`

  <span data-ttu-id="7d5b2-118">W przykładzie użyto tego obrazu w celu skompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-118">The sample uses this image for building the app.</span></span> <span data-ttu-id="7d5b2-119">Obraz zawiera zestaw .NET Core SDK, który zawiera narzędzia wiersza polecenia (CLI).</span><span class="sxs-lookup"><span data-stu-id="7d5b2-119">The image contains the .NET Core SDK, which includes the Command Line Tools (CLI).</span></span> <span data-ttu-id="7d5b2-120">Obraz, który jest zoptymalizowany pod kątem rozwoju lokalnego, debugowania i testowania jednostek.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-120">The image is optimized for local development, debugging, and unit testing.</span></span> <span data-ttu-id="7d5b2-121">Narzędzi zainstalowanych na potrzeby programowania i kompilacji Ustaw stosunkowo duży obraz.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-121">The tools installed for development and compilation make this a relatively large image.</span></span> 

* `dotnet/core/aspnet` 

   <span data-ttu-id="7d5b2-122">W przykładzie użyto tego obrazu do uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-122">The sample uses this image for running the app.</span></span> <span data-ttu-id="7d5b2-123">Obraz zawiera środowisko uruchomieniowe programu ASP.NET Core oraz biblioteki i jest zoptymalizowany pod kątem uruchamiania aplikacji w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-123">The image contains the ASP.NET Core runtime and libraries and is optimized for running apps in production.</span></span> <span data-ttu-id="7d5b2-124">Zaprojektowana pod kątem szybkiego wdrażania i uruchamiania aplikacji, są stosunkowo mały, więc zoptymalizowane pod kątem wydajności sieci hosta platformy Docker z rejestru platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-124">Designed for speed of deployment and app startup, the image is relatively small, so network performance from Docker Registry to Docker host is optimized.</span></span> <span data-ttu-id="7d5b2-125">Tylko pliki binarne i zawartość wymaganą do uruchomienia aplikacji są kopiowane do kontenera.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-125">Only the binaries and content needed to run an app are copied to the container.</span></span> <span data-ttu-id="7d5b2-126">Zawartość jest gotowa do uruchomienia, umożliwiając najkrótszy czas z `Docker run` do uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-126">The contents are ready to run, enabling the fastest time from `Docker run` to app startup.</span></span> <span data-ttu-id="7d5b2-127">Kompilacja kodu dynamicznej nie jest potrzebna w modelu platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-127">Dynamic code compilation isn't needed in the Docker model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d5b2-128">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7d5b2-128">Prerequisites</span></span>

* [<span data-ttu-id="7d5b2-129">.NET core SDK 2,2</span><span class="sxs-lookup"><span data-stu-id="7d5b2-129">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/core)

* <span data-ttu-id="7d5b2-130">Klient platformy docker 18.03 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="7d5b2-130">Docker client 18.03 or later</span></span>

  * <span data-ttu-id="7d5b2-131">Dystrybucje systemu Linux</span><span class="sxs-lookup"><span data-stu-id="7d5b2-131">Linux distributions</span></span>
    * [<span data-ttu-id="7d5b2-132">CentOS</span><span class="sxs-lookup"><span data-stu-id="7d5b2-132">CentOS</span></span>](https://docs.docker.com/install/linux/docker-ce/centos/)
    * [<span data-ttu-id="7d5b2-133">Debian</span><span class="sxs-lookup"><span data-stu-id="7d5b2-133">Debian</span></span>](https://docs.docker.com/install/linux/docker-ce/debian/)
    * [<span data-ttu-id="7d5b2-134">Fedora</span><span class="sxs-lookup"><span data-stu-id="7d5b2-134">Fedora</span></span>](https://docs.docker.com/install/linux/docker-ce/fedora/)
    * [<span data-ttu-id="7d5b2-135">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="7d5b2-135">Ubuntu</span></span>](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
  * [<span data-ttu-id="7d5b2-136">macOS</span><span class="sxs-lookup"><span data-stu-id="7d5b2-136">macOS</span></span>](https://docs.docker.com/docker-for-mac/install/)
  * [<span data-ttu-id="7d5b2-137">Windows</span><span class="sxs-lookup"><span data-stu-id="7d5b2-137">Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

* [<span data-ttu-id="7d5b2-138">Usługa Git</span><span class="sxs-lookup"><span data-stu-id="7d5b2-138">Git</span></span>](https://git-scm.com/download)

## <a name="download-the-sample-app"></a><span data-ttu-id="7d5b2-139">Pobierz przykładową aplikację</span><span class="sxs-lookup"><span data-stu-id="7d5b2-139">Download the sample app</span></span>

* <span data-ttu-id="7d5b2-140">Pobierz próbkę, klonowanie [repozytorium Docker w programie .NET Core](https://github.com/dotnet/dotnet-docker):</span><span class="sxs-lookup"><span data-stu-id="7d5b2-140">Download the sample by cloning the [.NET Core Docker repository](https://github.com/dotnet/dotnet-docker):</span></span> 

  ```console
  git clone https://github.com/dotnet/dotnet-docker
  ```

## <a name="run-the-app-locally"></a><span data-ttu-id="7d5b2-141">Uruchamianie aplikacji lokalnie</span><span class="sxs-lookup"><span data-stu-id="7d5b2-141">Run the app locally</span></span>

* <span data-ttu-id="7d5b2-142">Przejdź do folderu projektu w *dotnet-docker/samples/aspnetapp/aspnetapp*.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-142">Navigate to the project folder at *dotnet-docker/samples/aspnetapp/aspnetapp*.</span></span>

* <span data-ttu-id="7d5b2-143">Uruchom następujące polecenie, aby skompilować i uruchomić aplikację lokalnie:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-143">Run the following command to build and run the app locally:</span></span>

  ```console
  dotnet run
  ```

* <span data-ttu-id="7d5b2-144">Przejdź do `http://localhost:5000` w przeglądarce, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-144">Go to `http://localhost:5000` in a browser to test the app.</span></span>

* <span data-ttu-id="7d5b2-145">Naciśnij klawisze Ctrl + C, aby zatrzymać aplikację, w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-145">Press Ctrl+C at the command prompt to stop the app.</span></span>

## <a name="run-in-a-linux-container"></a><span data-ttu-id="7d5b2-146">Uruchom w kontenerze systemu Linux</span><span class="sxs-lookup"><span data-stu-id="7d5b2-146">Run in a Linux container</span></span>

* <span data-ttu-id="7d5b2-147">W kliencie platformy Docker Przełącz się do kontenerów systemu Linux.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-147">In the Docker client, switch to Linux containers.</span></span>

* <span data-ttu-id="7d5b2-148">Przejdź do folderu pliku Dockerfile w *dotnet-docker/samples/aspnetapp*.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-148">Navigate to the Dockerfile folder at *dotnet-docker/samples/aspnetapp*.</span></span>

* <span data-ttu-id="7d5b2-149">Uruchom następujące polecenia, aby skompilować i uruchomić przykład na platformie Docker:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-149">Run the following commands to build and run the sample in Docker:</span></span>

  ```console
  docker build -t aspnetapp .
  docker run -it --rm -p 5000:80 --name aspnetcore_sample aspnetapp
  ```

  <span data-ttu-id="7d5b2-150">`build` Argumenty polecenia:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-150">The `build` command arguments:</span></span>
  * <span data-ttu-id="7d5b2-151">Nazwa aspnetapp obrazu.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-151">Name the image aspnetapp.</span></span>
  * <span data-ttu-id="7d5b2-152">Wyszukaj plik Dockerfile w bieżącym folderze (kropki na końcu).</span><span class="sxs-lookup"><span data-stu-id="7d5b2-152">Look for the Dockerfile in the current folder (the period at the end).</span></span>

  <span data-ttu-id="7d5b2-153">Argumenty polecenia uruchomienia:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-153">The run command arguments:</span></span>
  * <span data-ttu-id="7d5b2-154">Przydziel pseudo-TTY i zostawić je otwarte, nawet jeśli nie jest dołączony.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-154">Allocate a pseudo-TTY and keep it open even if not attached.</span></span> <span data-ttu-id="7d5b2-155">(Sama efektu jako `--interactive --tty`.)</span><span class="sxs-lookup"><span data-stu-id="7d5b2-155">(Same effect as `--interactive --tty`.)</span></span>
  * <span data-ttu-id="7d5b2-156">Kontenera są automatycznie usuwane po wydaniu.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-156">Automatically remove the container when it exits.</span></span>
  * <span data-ttu-id="7d5b2-157">Mapowania portu 5000 na komputerze lokalnym porcie 80 w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-157">Map port 5000 on the local machine to port 80 in the container.</span></span>
  * <span data-ttu-id="7d5b2-158">Nazwa aspnetcore_sample kontenera.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-158">Name the container aspnetcore_sample.</span></span>
  * <span data-ttu-id="7d5b2-159">Określ obraz aspnetapp.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-159">Specify the aspnetapp image.</span></span>

* <span data-ttu-id="7d5b2-160">Przejdź do `http://localhost:5000` w przeglądarce, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-160">Go to `http://localhost:5000` in a browser to test the app.</span></span>

## <a name="run-in-a-windows-container"></a><span data-ttu-id="7d5b2-161">Uruchamianie w kontenerze Windows</span><span class="sxs-lookup"><span data-stu-id="7d5b2-161">Run in a Windows container</span></span>

* <span data-ttu-id="7d5b2-162">W kliencie platformy Docker Przełącz się do kontenerów Windows.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-162">In the Docker client, switch to Windows containers.</span></span>

<span data-ttu-id="7d5b2-163">Przejdź do folderu pliku platformy docker w `dotnet-docker/samples/aspnetapp`.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-163">Navigate to the docker file folder at `dotnet-docker/samples/aspnetapp`.</span></span>

* <span data-ttu-id="7d5b2-164">Uruchom następujące polecenia, aby skompilować i uruchomić przykład na platformie Docker:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-164">Run the following commands to build and run the sample in Docker:</span></span>

  ```console
  docker build -t aspnetapp .
  docker run -it --rm --name aspnetcore_sample aspnetapp
  ```

* <span data-ttu-id="7d5b2-165">Dla kontenerów Windows jest wymagany adres IP kontenera (przejście do `http://localhost:5000` nie będzie działać):</span><span class="sxs-lookup"><span data-stu-id="7d5b2-165">For Windows containers, you need the IP address of the container (browsing to `http://localhost:5000` won't work):</span></span>
  * <span data-ttu-id="7d5b2-166">Otwórz innego wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-166">Open up another command prompt.</span></span>
  * <span data-ttu-id="7d5b2-167">Uruchom `docker ps` Aby wyświetlić uruchomione kontenery.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-167">Run `docker ps` to see the running containers.</span></span> <span data-ttu-id="7d5b2-168">Sprawdź kontener "aspnetcore_sample" istnieje.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-168">Verify that the "aspnetcore_sample" container is there.</span></span>
  * <span data-ttu-id="7d5b2-169">Uruchom `docker exec aspnetcore_sample ipconfig` Aby wyświetlić adres IP kontenera.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-169">Run `docker exec aspnetcore_sample ipconfig` to display the IP address of the container.</span></span> <span data-ttu-id="7d5b2-170">Dane wyjściowe polecenia będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-170">The output from the command looks like this example:</span></span>

    ```console
    Ethernet adapter Ethernet:

       Connection-specific DNS Suffix  . : contoso.com
       Link-local IPv6 Address . . . . . : fe80::1967:6598:124:cfa3%4
       IPv4 Address. . . . . . . . . . . : 172.29.245.43
       Subnet Mask . . . . . . . . . . . : 255.255.240.0
       Default Gateway . . . . . . . . . : 172.29.240.1
    ```

* <span data-ttu-id="7d5b2-171">Skopiuj kontenera adres IPv4 (na przykład 172.29.245.43) i Wklej w pasku adresu przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-171">Copy the container IPv4 address (for example, 172.29.245.43) and paste into the browser address bar to test the app.</span></span>

## <a name="build-and-deploy-manually"></a><span data-ttu-id="7d5b2-172">Tworzenie i wdrażanie ręcznie</span><span class="sxs-lookup"><span data-stu-id="7d5b2-172">Build and deploy manually</span></span>

<span data-ttu-id="7d5b2-173">W niektórych przypadkach warto wdrożyć aplikację do kontenera przez skopiowanie plików aplikacji, które są wymagane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-173">In some scenarios, you might want to deploy an app to a container by copying to it the application files that are needed at run time.</span></span> <span data-ttu-id="7d5b2-174">W tej sekcji pokazano, jak wdrożyć ręcznie.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-174">This section shows how to deploy manually.</span></span>

* <span data-ttu-id="7d5b2-175">Przejdź do folderu projektu w *dotnet-docker/samples/aspnetapp/aspnetapp*.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-175">Navigate to the project folder at *dotnet-docker/samples/aspnetapp/aspnetapp*.</span></span>

* <span data-ttu-id="7d5b2-176">Uruchom [publikowania dotnet](https://docs.microsoft.com/dotnet/core/tools/dotnet-publish.md) polecenia:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-176">Run the [dotnet publish](https://docs.microsoft.com/dotnet/core/tools/dotnet-publish.md) command:</span></span>

  ```console
  dotnet publish -c Release -o published
  ```

  <span data-ttu-id="7d5b2-177">Argumenty wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-177">The command arguments:</span></span>
  * <span data-ttu-id="7d5b2-178">Utwórz aplikację w trybie wydania (wartość domyślna to tryb debugowania).</span><span class="sxs-lookup"><span data-stu-id="7d5b2-178">Build the application in release mode (the default is debug mode).</span></span>
  * <span data-ttu-id="7d5b2-179">Tworzenie plików w *opublikowane* folderu.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-179">Create the files in the *published* folder.</span></span>

* <span data-ttu-id="7d5b2-180">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-180">Run the application.</span></span>

  * <span data-ttu-id="7d5b2-181">W systemie Windows:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-181">Windows:</span></span>

    ```console
    dotnet published\aspnetapp.dll
    ```

  * <span data-ttu-id="7d5b2-182">W systemie Linux:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-182">Linux:</span></span>

    ```bash
    dotnet published/aspnetapp.dll
    ```

* <span data-ttu-id="7d5b2-183">Przejdź do `http://localhost:5000` można znaleźć na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-183">Browse to `http://localhost:5000` to see the home page.</span></span>

### <a name="the-dockerfile"></a><span data-ttu-id="7d5b2-184">The Dockerfile</span><span class="sxs-lookup"><span data-stu-id="7d5b2-184">The Dockerfile</span></span>

<span data-ttu-id="7d5b2-185">W tym pliku Dockerfile używana przez `docker build` polecenia został przeprowadzony wcześniej.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-185">Here's the Dockerfile used by the `docker build` command you ran earlier.</span></span>  <span data-ttu-id="7d5b2-186">Używa ona `dotnet publish` taki sam sposób jak w tej sekcji, aby skompilować i wdrożyć.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-186">It uses `dotnet publish` the same way you did in this section to build and deploy.</span></span>  

```console
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.sln .
COPY aspnetapp/*.csproj ./aspnetapp/
RUN dotnet restore

# copy everything else and build app
COPY aspnetapp/. ./aspnetapp/
WORKDIR /app/aspnetapp
RUN dotnet publish -c Release -o out


FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS runtime
WORKDIR /app
COPY --from=build /app/aspnetapp/out ./
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```

## <a name="additional-resources"></a><span data-ttu-id="7d5b2-187">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7d5b2-187">Additional resources</span></span>

* [<span data-ttu-id="7d5b2-188">Polecenie kompilacji platformy docker</span><span class="sxs-lookup"><span data-stu-id="7d5b2-188">Docker build command</span></span>](https://docs.docker.com/engine/reference/commandline/build)
* [<span data-ttu-id="7d5b2-189">Docker, uruchom polecenie</span><span class="sxs-lookup"><span data-stu-id="7d5b2-189">Docker run command</span></span>](https://docs.docker.com/engine/reference/commandline/run)
* <span data-ttu-id="7d5b2-190">[ASP.NET Core, Docker próbki](https://github.com/dotnet/dotnet-docker) (używaną w ramach tego samouczka.)</span><span class="sxs-lookup"><span data-stu-id="7d5b2-190">[ASP.NET Core Docker sample](https://github.com/dotnet/dotnet-docker) (The one used in this tutorial.)</span></span>
* [<span data-ttu-id="7d5b2-191">Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="7d5b2-191">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](/aspnet/core/host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="7d5b2-192">Praca z narzędzia Docker programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d5b2-192">Working with Visual Studio Docker Tools</span></span>](https://docs.microsoft.com/aspnet/core/publishing/visual-studio-tools-for-docker)
* [<span data-ttu-id="7d5b2-193">Debugowanie za pomocą programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7d5b2-193">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/nodejs/debugging-recipes#_debug-nodejs-in-docker-containers) 

## <a name="next-steps"></a><span data-ttu-id="7d5b2-194">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7d5b2-194">Next steps</span></span>

<span data-ttu-id="7d5b2-195">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-195">In this tutorial, you:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="7d5b2-196">Przedstawia informacje na temat obrazów platformy Docker programu Microsoft .NET Core</span><span class="sxs-lookup"><span data-stu-id="7d5b2-196">Learned about Microsoft .NET Core Docker images</span></span> 
> * <span data-ttu-id="7d5b2-197">Pobrać przykładową aplikację platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d5b2-197">Downloaded an ASP.NET Core sample app</span></span>
> * <span data-ttu-id="7d5b2-198">Uruchamianie przykładowej aplikacji lokalnie</span><span class="sxs-lookup"><span data-stu-id="7d5b2-198">Run the sample app locally</span></span>
> * <span data-ttu-id="7d5b2-199">Uruchamianie przykładowej aplikacji w kontenerach systemu Linux</span><span class="sxs-lookup"><span data-stu-id="7d5b2-199">Run the sample app in Linux containers</span></span>
> * <span data-ttu-id="7d5b2-200">Uruchom aplikację przykładową, za pomocą w kontenerach Windows</span><span class="sxs-lookup"><span data-stu-id="7d5b2-200">Run the sample with in Windows containers</span></span>
> * <span data-ttu-id="7d5b2-201">Utworzeniu i wdrożeniu ręcznie</span><span class="sxs-lookup"><span data-stu-id="7d5b2-201">Built and deployed manually</span></span>

<span data-ttu-id="7d5b2-202">Repozytorium Git, który zawiera Przykładowa aplikacja umożliwia także dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="7d5b2-202">The Git repository that contains the sample app also includes documentation.</span></span> <span data-ttu-id="7d5b2-203">Aby uzyskać omówienie dostępnych zasobów w repozytorium, zobacz [pliku README](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/README.md).</span><span class="sxs-lookup"><span data-stu-id="7d5b2-203">For an overview of the resources available in the repository, see [the README file](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/README.md).</span></span> <span data-ttu-id="7d5b2-204">W szczególności Dowiedz się, jak zaimplementować protokół HTTPS:</span><span class="sxs-lookup"><span data-stu-id="7d5b2-204">In particular, learn how to implement HTTPS:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7d5b2-205">Tworzenie aplikacji platformy ASP.NET Core za pomocą platformy Docker przy użyciu protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="7d5b2-205">Developing ASP.NET Core Applications with Docker over HTTPS</span></span>](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md)
