---
title: Obrazy platformy Docker dla ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać opublikowanych obrazów .NET Core Docker z rejestru platformy Docker. Ściągaj obrazy i Kompiluj własne obrazy.
ms.author: riande
ms.custom: mvc
ms.date: 06/18/2019
uid: host-and-deploy/docker/building-net-docker-images
ms.openlocfilehash: 12665fb2e7a9c75747f5c83129a617aea6adfbbf
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317692"
---
# <a name="docker-images-for-aspnet-core"></a><span data-ttu-id="b5f71-104">Obrazy platformy Docker dla ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5f71-104">Docker images for ASP.NET Core</span></span>

<span data-ttu-id="b5f71-105">W tym samouczku pokazano, jak uruchomić aplikację ASP.NET Core w kontenerach platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="b5f71-105">This tutorial shows how to run an ASP.NET Core app in Docker containers.</span></span>

<span data-ttu-id="b5f71-106">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="b5f71-106">In this tutorial, you:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="b5f71-107">Dowiedz się więcej na temat Microsoft .NET podstawowych obrazów platformy Docker</span><span class="sxs-lookup"><span data-stu-id="b5f71-107">Learn about Microsoft .NET Core Docker images</span></span> 
> * <span data-ttu-id="b5f71-108">Pobieranie przykładowej aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5f71-108">Download an ASP.NET Core sample app</span></span>
> * <span data-ttu-id="b5f71-109">Uruchamianie przykładowej aplikacji lokalnie</span><span class="sxs-lookup"><span data-stu-id="b5f71-109">Run the sample app locally</span></span>
> * <span data-ttu-id="b5f71-110">Uruchamianie przykładowej aplikacji w kontenerach systemu Linux</span><span class="sxs-lookup"><span data-stu-id="b5f71-110">Run the sample app in Linux containers</span></span>
> * <span data-ttu-id="b5f71-111">Uruchamianie przykładowej aplikacji w kontenerach systemu Windows</span><span class="sxs-lookup"><span data-stu-id="b5f71-111">Run the sample app in Windows containers</span></span>
> * <span data-ttu-id="b5f71-112">Kompiluj i wdrażaj ręcznie</span><span class="sxs-lookup"><span data-stu-id="b5f71-112">Build and deploy manually</span></span>

## <a name="aspnet-core-docker-images"></a><span data-ttu-id="b5f71-113">ASP.NET Core obrazów platformy Docker</span><span class="sxs-lookup"><span data-stu-id="b5f71-113">ASP.NET Core Docker images</span></span>

<span data-ttu-id="b5f71-114">Na potrzeby tego samouczka pobierzesz przykładową aplikację ASP.NET Core i uruchomisz ją w kontenerach platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="b5f71-114">For this tutorial, you download an ASP.NET Core sample app and run it in Docker containers.</span></span> <span data-ttu-id="b5f71-115">Przykład działa w przypadku kontenerów systemów Linux i Windows.</span><span class="sxs-lookup"><span data-stu-id="b5f71-115">The sample works with both Linux and Windows containers.</span></span>

<span data-ttu-id="b5f71-116">Przykładowy pliku dockerfile używa [funkcji budowania wielu etapów platformy Docker](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) do kompilowania i uruchamiania w różnych kontenerach.</span><span class="sxs-lookup"><span data-stu-id="b5f71-116">The sample Dockerfile uses the [Docker multi-stage build feature](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) to build and run in different containers.</span></span> <span data-ttu-id="b5f71-117">Kontenery kompilacji i uruchamiania są tworzone na podstawie obrazów udostępnianych w usłudze Docker Hub przez firmę Microsoft:</span><span class="sxs-lookup"><span data-stu-id="b5f71-117">The build and run containers are created from images that are provided in Docker Hub by Microsoft:</span></span>

* `dotnet/core/sdk`

  <span data-ttu-id="b5f71-118">Przykład używa tego obrazu do kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b5f71-118">The sample uses this image for building the app.</span></span> <span data-ttu-id="b5f71-119">Obraz zawiera zestaw .NET Core SDK, który zawiera narzędzia wiersza polecenia (CLI).</span><span class="sxs-lookup"><span data-stu-id="b5f71-119">The image contains the .NET Core SDK, which includes the Command Line Tools (CLI).</span></span> <span data-ttu-id="b5f71-120">Obraz jest zoptymalizowany pod kątem lokalnego projektowania, debugowania i testowania jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="b5f71-120">The image is optimized for local development, debugging, and unit testing.</span></span> <span data-ttu-id="b5f71-121">Narzędzia zainstalowane na potrzeby programowania i kompilowania sprawiają, że jest to stosunkowo duży obraz.</span><span class="sxs-lookup"><span data-stu-id="b5f71-121">The tools installed for development and compilation make this a relatively large image.</span></span> 

* `dotnet/core/aspnet` 

   <span data-ttu-id="b5f71-122">Przykład używa tego obrazu do uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b5f71-122">The sample uses this image for running the app.</span></span> <span data-ttu-id="b5f71-123">Obraz zawiera ASP.NET Core środowiska uruchomieniowego i bibliotek, które są zoptymalizowane pod kątem uruchamiania aplikacji w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="b5f71-123">The image contains the ASP.NET Core runtime and libraries and is optimized for running apps in production.</span></span> <span data-ttu-id="b5f71-124">Obraz jest stosunkowo mały, zaprojektowany z myślą o szybkości wdrażania i uruchamiania aplikacji, dlatego Optymalizacja wydajności sieci z poziomu rejestru platformy Docker do hosta platformy Docker jest zoptymalizowana.</span><span class="sxs-lookup"><span data-stu-id="b5f71-124">Designed for speed of deployment and app startup, the image is relatively small, so network performance from Docker Registry to Docker host is optimized.</span></span> <span data-ttu-id="b5f71-125">Tylko pliki binarne i zawartość, które są konieczne do uruchomienia aplikacji, są kopiowane do kontenera.</span><span class="sxs-lookup"><span data-stu-id="b5f71-125">Only the binaries and content needed to run an app are copied to the container.</span></span> <span data-ttu-id="b5f71-126">Zawartość jest gotowa do uruchomienia, co pozwoli na najszybszy czas od `Docker run` do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b5f71-126">The contents are ready to run, enabling the fastest time from `Docker run` to app startup.</span></span> <span data-ttu-id="b5f71-127">W modelu platformy Docker nie jest wymagana kompilacja kodu dynamicznego.</span><span class="sxs-lookup"><span data-stu-id="b5f71-127">Dynamic code compilation isn't needed in the Docker model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5f71-128">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b5f71-128">Prerequisites</span></span>

* [<span data-ttu-id="b5f71-129">.NET Core SDK 3.0</span><span class="sxs-lookup"><span data-stu-id="b5f71-129">.NET Core SDK 3.0</span></span>](https://dotnet.microsoft.com/download)

* <span data-ttu-id="b5f71-130">Klient platformy Docker 18,03 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="b5f71-130">Docker client 18.03 or later</span></span>

  * <span data-ttu-id="b5f71-131">Dystrybucje systemu Linux</span><span class="sxs-lookup"><span data-stu-id="b5f71-131">Linux distributions</span></span>
    * [<span data-ttu-id="b5f71-132">CentOS</span><span class="sxs-lookup"><span data-stu-id="b5f71-132">CentOS</span></span>](https://docs.docker.com/install/linux/docker-ce/centos/)
    * [<span data-ttu-id="b5f71-133">Debian</span><span class="sxs-lookup"><span data-stu-id="b5f71-133">Debian</span></span>](https://docs.docker.com/install/linux/docker-ce/debian/)
    * [<span data-ttu-id="b5f71-134">Fedora</span><span class="sxs-lookup"><span data-stu-id="b5f71-134">Fedora</span></span>](https://docs.docker.com/install/linux/docker-ce/fedora/)
    * [<span data-ttu-id="b5f71-135">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="b5f71-135">Ubuntu</span></span>](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
  * [<span data-ttu-id="b5f71-136">macOS</span><span class="sxs-lookup"><span data-stu-id="b5f71-136">macOS</span></span>](https://docs.docker.com/docker-for-mac/install/)
  * [<span data-ttu-id="b5f71-137">Windows</span><span class="sxs-lookup"><span data-stu-id="b5f71-137">Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

* [<span data-ttu-id="b5f71-138">Usługa Git</span><span class="sxs-lookup"><span data-stu-id="b5f71-138">Git</span></span>](https://git-scm.com/download)

## <a name="download-the-sample-app"></a><span data-ttu-id="b5f71-139">Pobieranie przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="b5f71-139">Download the sample app</span></span>

* <span data-ttu-id="b5f71-140">Pobierz próbkę, klonowanie [repozytorium .NET Core Docker](https://github.com/dotnet/dotnet-docker):</span><span class="sxs-lookup"><span data-stu-id="b5f71-140">Download the sample by cloning the [.NET Core Docker repository](https://github.com/dotnet/dotnet-docker):</span></span> 

  ```console
  git clone https://github.com/dotnet/dotnet-docker
  ```

## <a name="run-the-app-locally"></a><span data-ttu-id="b5f71-141">Uruchamianie aplikacji lokalnie</span><span class="sxs-lookup"><span data-stu-id="b5f71-141">Run the app locally</span></span>

* <span data-ttu-id="b5f71-142">Przejdź do folderu projektu w programie *dotnet-Docker/Samples/aspnetapp/aspnetapp*.</span><span class="sxs-lookup"><span data-stu-id="b5f71-142">Navigate to the project folder at *dotnet-docker/samples/aspnetapp/aspnetapp*.</span></span>

* <span data-ttu-id="b5f71-143">Uruchom następujące polecenie, aby skompilować i uruchomić aplikację lokalnie:</span><span class="sxs-lookup"><span data-stu-id="b5f71-143">Run the following command to build and run the app locally:</span></span>

  ```dotnetcli
  dotnet run
  ```

* <span data-ttu-id="b5f71-144">Przejdź do `http://localhost:5000` programu w przeglądarce, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="b5f71-144">Go to `http://localhost:5000` in a browser to test the app.</span></span>

* <span data-ttu-id="b5f71-145">Naciśnij klawisze CTRL + C w wierszu polecenia, aby zatrzymać aplikację.</span><span class="sxs-lookup"><span data-stu-id="b5f71-145">Press Ctrl+C at the command prompt to stop the app.</span></span>

## <a name="run-in-a-linux-container"></a><span data-ttu-id="b5f71-146">Uruchamianie w kontenerze systemu Linux</span><span class="sxs-lookup"><span data-stu-id="b5f71-146">Run in a Linux container</span></span>

* <span data-ttu-id="b5f71-147">W kliencie platformy Docker przejdź do obszaru kontenery systemu Linux.</span><span class="sxs-lookup"><span data-stu-id="b5f71-147">In the Docker client, switch to Linux containers.</span></span>

* <span data-ttu-id="b5f71-148">Przejdź do folderu pliku dockerfile w programie *dotnet-Docker/Samples/aspnetapp*.</span><span class="sxs-lookup"><span data-stu-id="b5f71-148">Navigate to the Dockerfile folder at *dotnet-docker/samples/aspnetapp*.</span></span>

* <span data-ttu-id="b5f71-149">Uruchom następujące polecenia, aby skompilować i uruchomić przykład w Docker:</span><span class="sxs-lookup"><span data-stu-id="b5f71-149">Run the following commands to build and run the sample in Docker:</span></span>

  ```console
  docker build -t aspnetapp .
  docker run -it --rm -p 5000:80 --name aspnetcore_sample aspnetapp
  ```

  <span data-ttu-id="b5f71-150">Argumenty `build` polecenia:</span><span class="sxs-lookup"><span data-stu-id="b5f71-150">The `build` command arguments:</span></span>
  * <span data-ttu-id="b5f71-151">Nazwij obraz aspnetapp.</span><span class="sxs-lookup"><span data-stu-id="b5f71-151">Name the image aspnetapp.</span></span>
  * <span data-ttu-id="b5f71-152">Wyszukaj pliku dockerfile w bieżącym folderze (okres na końcu).</span><span class="sxs-lookup"><span data-stu-id="b5f71-152">Look for the Dockerfile in the current folder (the period at the end).</span></span>

  <span data-ttu-id="b5f71-153">Argumenty polecenia Run:</span><span class="sxs-lookup"><span data-stu-id="b5f71-153">The run command arguments:</span></span>
  * <span data-ttu-id="b5f71-154">Przydziel pseudo-TTY i pozostaw go otwarty nawet wtedy, gdy nie jest dołączony.</span><span class="sxs-lookup"><span data-stu-id="b5f71-154">Allocate a pseudo-TTY and keep it open even if not attached.</span></span> <span data-ttu-id="b5f71-155">(Ten sam efekt `--interactive --tty`jako)</span><span class="sxs-lookup"><span data-stu-id="b5f71-155">(Same effect as `--interactive --tty`.)</span></span>
  * <span data-ttu-id="b5f71-156">Automatycznie Usuń kontener, gdy zostanie on zakończony.</span><span class="sxs-lookup"><span data-stu-id="b5f71-156">Automatically remove the container when it exits.</span></span>
  * <span data-ttu-id="b5f71-157">Mapuj port 5000 na komputerze lokalnym na port 80 w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="b5f71-157">Map port 5000 on the local machine to port 80 in the container.</span></span>
  * <span data-ttu-id="b5f71-158">Nazwij kontener aspnetcore_sample.</span><span class="sxs-lookup"><span data-stu-id="b5f71-158">Name the container aspnetcore_sample.</span></span>
  * <span data-ttu-id="b5f71-159">Określ obraz aspnetapp.</span><span class="sxs-lookup"><span data-stu-id="b5f71-159">Specify the aspnetapp image.</span></span>

* <span data-ttu-id="b5f71-160">Przejdź do `http://localhost:5000` programu w przeglądarce, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="b5f71-160">Go to `http://localhost:5000` in a browser to test the app.</span></span>

## <a name="run-in-a-windows-container"></a><span data-ttu-id="b5f71-161">Uruchamianie w kontenerze systemu Windows</span><span class="sxs-lookup"><span data-stu-id="b5f71-161">Run in a Windows container</span></span>

* <span data-ttu-id="b5f71-162">W kliencie platformy Docker przejdź do kontenerów systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="b5f71-162">In the Docker client, switch to Windows containers.</span></span>

<span data-ttu-id="b5f71-163">Przejdź do folderu pliku platformy Docker pod `dotnet-docker/samples/aspnetapp`adresem.</span><span class="sxs-lookup"><span data-stu-id="b5f71-163">Navigate to the docker file folder at `dotnet-docker/samples/aspnetapp`.</span></span>

* <span data-ttu-id="b5f71-164">Uruchom następujące polecenia, aby skompilować i uruchomić przykład w Docker:</span><span class="sxs-lookup"><span data-stu-id="b5f71-164">Run the following commands to build and run the sample in Docker:</span></span>

  ```console
  docker build -t aspnetapp .
  docker run -it --rm --name aspnetcore_sample aspnetapp
  ```

* <span data-ttu-id="b5f71-165">W przypadku kontenerów systemu Windows wymagany jest adres IP kontenera (przeglądanie w celu `http://localhost:5000` uniemożliwienia pracy):</span><span class="sxs-lookup"><span data-stu-id="b5f71-165">For Windows containers, you need the IP address of the container (browsing to `http://localhost:5000` won't work):</span></span>
  * <span data-ttu-id="b5f71-166">Otwórz inny wiersz polecenia.</span><span class="sxs-lookup"><span data-stu-id="b5f71-166">Open up another command prompt.</span></span>
  * <span data-ttu-id="b5f71-167">Uruchom `docker ps` , aby wyświetlić uruchomione kontenery.</span><span class="sxs-lookup"><span data-stu-id="b5f71-167">Run `docker ps` to see the running containers.</span></span> <span data-ttu-id="b5f71-168">Upewnij się, że kontener "aspnetcore_sample" znajduje się w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="b5f71-168">Verify that the "aspnetcore_sample" container is there.</span></span>
  * <span data-ttu-id="b5f71-169">Uruchom `docker exec aspnetcore_sample ipconfig` , aby wyświetlić adres IP kontenera.</span><span class="sxs-lookup"><span data-stu-id="b5f71-169">Run `docker exec aspnetcore_sample ipconfig` to display the IP address of the container.</span></span> <span data-ttu-id="b5f71-170">Dane wyjściowe polecenia wyglądają jak w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="b5f71-170">The output from the command looks like this example:</span></span>

    ```console
    Ethernet adapter Ethernet:

       Connection-specific DNS Suffix  . : contoso.com
       Link-local IPv6 Address . . . . . : fe80::1967:6598:124:cfa3%4
       IPv4 Address. . . . . . . . . . . : 172.29.245.43
       Subnet Mask . . . . . . . . . . . : 255.255.240.0
       Default Gateway . . . . . . . . . : 172.29.240.1
    ```

* <span data-ttu-id="b5f71-171">Skopiuj adres IPv4 kontenera (na przykład 172.29.245.43) i wklej go na pasku adresu przeglądarki, aby przetestować aplikację.</span><span class="sxs-lookup"><span data-stu-id="b5f71-171">Copy the container IPv4 address (for example, 172.29.245.43) and paste into the browser address bar to test the app.</span></span>

## <a name="build-and-deploy-manually"></a><span data-ttu-id="b5f71-172">Kompiluj i wdrażaj ręcznie</span><span class="sxs-lookup"><span data-stu-id="b5f71-172">Build and deploy manually</span></span>

<span data-ttu-id="b5f71-173">W niektórych scenariuszach może zajść potrzeba wdrożenia aplikacji w kontenerze przez skopiowanie do niego plików aplikacji, które są potrzebne w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="b5f71-173">In some scenarios, you might want to deploy an app to a container by copying to it the application files that are needed at run time.</span></span> <span data-ttu-id="b5f71-174">W tej sekcji pokazano, jak wdrożyć ręcznie.</span><span class="sxs-lookup"><span data-stu-id="b5f71-174">This section shows how to deploy manually.</span></span>

* <span data-ttu-id="b5f71-175">Przejdź do folderu projektu w programie *dotnet-Docker/Samples/aspnetapp/aspnetapp*.</span><span class="sxs-lookup"><span data-stu-id="b5f71-175">Navigate to the project folder at *dotnet-docker/samples/aspnetapp/aspnetapp*.</span></span>

* <span data-ttu-id="b5f71-176">Uruchom [dotnet Publish](/dotnet/core/tools/dotnet-publish) polecenie:</span><span class="sxs-lookup"><span data-stu-id="b5f71-176">Run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

  ```dotnetcli
  dotnet publish -c Release -o published
  ```

  <span data-ttu-id="b5f71-177">Argumenty polecenia:</span><span class="sxs-lookup"><span data-stu-id="b5f71-177">The command arguments:</span></span>
  * <span data-ttu-id="b5f71-178">Kompiluj aplikację w trybie wydania (domyślnie jest to tryb debugowania).</span><span class="sxs-lookup"><span data-stu-id="b5f71-178">Build the application in release mode (the default is debug mode).</span></span>
  * <span data-ttu-id="b5f71-179">Utwórz pliki w folderze *opublikowanym* .</span><span class="sxs-lookup"><span data-stu-id="b5f71-179">Create the files in the *published* folder.</span></span>

* <span data-ttu-id="b5f71-180">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="b5f71-180">Run the application.</span></span>

  * <span data-ttu-id="b5f71-181">W systemie Windows:</span><span class="sxs-lookup"><span data-stu-id="b5f71-181">Windows:</span></span>

    ```dotnetcli
    dotnet published\aspnetapp.dll
    ```

  * <span data-ttu-id="b5f71-182">W systemie Linux:</span><span class="sxs-lookup"><span data-stu-id="b5f71-182">Linux:</span></span>

    ```dotnetcli
    dotnet published/aspnetapp.dll
    ```

* <span data-ttu-id="b5f71-183">Przejdź do `http://localhost:5000` strony głównej.</span><span class="sxs-lookup"><span data-stu-id="b5f71-183">Browse to `http://localhost:5000` to see the home page.</span></span>

<span data-ttu-id="b5f71-184">Aby użyć ręcznie opublikowanej aplikacji w kontenerze platformy Docker, Utwórz nowy pliku dockerfile i użyj polecenia `docker build .` , aby skompilować kontener.</span><span class="sxs-lookup"><span data-stu-id="b5f71-184">To use the manually published application within a Docker container, create a new Dockerfile and use the `docker build .` command to build the container.</span></span>

```console
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS runtime
WORKDIR /app
COPY published/aspnetapp.dll ./
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```

### <a name="the-dockerfile"></a><span data-ttu-id="b5f71-185">Pliku dockerfile</span><span class="sxs-lookup"><span data-stu-id="b5f71-185">The Dockerfile</span></span>

<span data-ttu-id="b5f71-186">Oto *pliku dockerfile* używany przez `docker build` wykonane wcześniej polecenie.</span><span class="sxs-lookup"><span data-stu-id="b5f71-186">Here's the *Dockerfile* used by the `docker build` command you ran earlier.</span></span>  <span data-ttu-id="b5f71-187">Używa `dotnet publish` tego samego sposobu tworzenia i wdrażania w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="b5f71-187">It uses `dotnet publish` the same way you did in this section to build and deploy.</span></span>  

```dockerfile
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

## <a name="additional-resources"></a><span data-ttu-id="b5f71-188">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b5f71-188">Additional resources</span></span>

* [<span data-ttu-id="b5f71-189">Docker Build — polecenie</span><span class="sxs-lookup"><span data-stu-id="b5f71-189">Docker build command</span></span>](https://docs.docker.com/engine/reference/commandline/build)
* [<span data-ttu-id="b5f71-190">Polecenie Docker Run</span><span class="sxs-lookup"><span data-stu-id="b5f71-190">Docker run command</span></span>](https://docs.docker.com/engine/reference/commandline/run)
* <span data-ttu-id="b5f71-191">[Przykład ASP.NET Core Docker](https://github.com/dotnet/dotnet-docker) (Używany w tym samouczku).</span><span class="sxs-lookup"><span data-stu-id="b5f71-191">[ASP.NET Core Docker sample](https://github.com/dotnet/dotnet-docker) (The one used in this tutorial.)</span></span>
* [<span data-ttu-id="b5f71-192">Konfigurowanie ASP.NET Core do pracy z serwerami proxy i usługami równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="b5f71-192">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](/aspnet/core/host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="b5f71-193">Praca z narzędziami platformy Docker programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5f71-193">Working with Visual Studio Docker Tools</span></span>](https://docs.microsoft.com/aspnet/core/publishing/visual-studio-tools-for-docker)
* [<span data-ttu-id="b5f71-194">Debugowanie za pomocą programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b5f71-194">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/nodejs/debugging-recipes#_debug-nodejs-in-docker-containers) 

## <a name="next-steps"></a><span data-ttu-id="b5f71-195">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b5f71-195">Next steps</span></span>

<span data-ttu-id="b5f71-196">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="b5f71-196">In this tutorial, you:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="b5f71-197">Informacje o Microsoft .NET podstawowych obrazów platformy Docker</span><span class="sxs-lookup"><span data-stu-id="b5f71-197">Learned about Microsoft .NET Core Docker images</span></span> 
> * <span data-ttu-id="b5f71-198">Pobieranie przykładowej aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5f71-198">Downloaded an ASP.NET Core sample app</span></span>
> * <span data-ttu-id="b5f71-199">Uruchamianie przykładowej aplikacji lokalnie</span><span class="sxs-lookup"><span data-stu-id="b5f71-199">Run the sample app locally</span></span>
> * <span data-ttu-id="b5f71-200">Uruchamianie przykładowej aplikacji w kontenerach systemu Linux</span><span class="sxs-lookup"><span data-stu-id="b5f71-200">Run the sample app in Linux containers</span></span>
> * <span data-ttu-id="b5f71-201">Uruchamianie przykładu w kontenerach systemu Windows</span><span class="sxs-lookup"><span data-stu-id="b5f71-201">Run the sample with in Windows containers</span></span>
> * <span data-ttu-id="b5f71-202">Skompilowane i wdrożone ręcznie</span><span class="sxs-lookup"><span data-stu-id="b5f71-202">Built and deployed manually</span></span>

<span data-ttu-id="b5f71-203">Repozytorium git zawierające przykładową aplikację zawiera również dokumentację.</span><span class="sxs-lookup"><span data-stu-id="b5f71-203">The Git repository that contains the sample app also includes documentation.</span></span> <span data-ttu-id="b5f71-204">Aby zapoznać się z omówieniem zasobów dostępnych w repozytorium, zobacz [plik Readme](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/README.md).</span><span class="sxs-lookup"><span data-stu-id="b5f71-204">For an overview of the resources available in the repository, see [the README file](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/README.md).</span></span> <span data-ttu-id="b5f71-205">W szczególności Dowiedz się, jak zaimplementować protokół HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b5f71-205">In particular, learn how to implement HTTPS:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b5f71-206">Opracowywanie aplikacji ASP.NET Core przy użyciu platformy Docker za pośrednictwem protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="b5f71-206">Developing ASP.NET Core Applications with Docker over HTTPS</span></span>](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md)
