---
title: Obrazy platformy docker dla platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak używać opublikowanych obrazów Docker w programie .NET Core z rejestru platformy Docker. Ściąganie obrazów i tworzenie własnych obrazów.
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/09/2019
uid: host-and-deploy/docker/building-net-docker-images
ms.openlocfilehash: d4031ee94194800176b90676b74afa42e33e5f5b
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59984521"
---
# <a name="docker-images-for-aspnet-core"></a>Obrazy platformy docker dla platformy ASP.NET Core

W tym samouczku pokazano, jak uruchomić aplikację ASP.NET Core w kontenerach platformy Docker.

W ramach tego samouczka możesz:
> [!div class="checklist"]
> * Więcej informacji na temat obrazów platformy Docker programu Microsoft .NET Core 
> * Pobierz przykładową aplikację platformy ASP.NET Core
> * Uruchamianie przykładowej aplikacji lokalnie
> * Uruchamianie przykładowej aplikacji w kontenerach systemu Linux
> * Uruchamianie przykładowej aplikacji w kontenerach Windows
> * Tworzenie i wdrażanie ręcznie

## <a name="aspnet-core-docker-images"></a>Obrazy platformy Docker Core ASP.NET

Na potrzeby tego samouczka możesz pobrać przykładową aplikację platformy ASP.NET Core i uruchom go w kontenerach platformy Docker. Przykład współdziała z kontenerów systemów Linux i Windows.

Przykładowy plik Dockerfile używa [wieloetapowych platformy Docker, tworzenie funkcji](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) Aby skompilować i uruchomić w różnych kontenerach. Kompilowanie i uruchom kontenery są tworzone na podstawie obrazów, które znajdują się w usłudze Docker Hub przez firmę Microsoft:

* `dotnet/core/sdk`

  W przykładzie użyto tego obrazu w celu skompilowania aplikacji. Obraz zawiera zestaw .NET Core SDK, który zawiera narzędzia wiersza polecenia (CLI). Obraz, który jest zoptymalizowany pod kątem rozwoju lokalnego, debugowania i testowania jednostek. Narzędzi zainstalowanych na potrzeby programowania i kompilacji Ustaw stosunkowo duży obraz. 

* `dotnet/core/aspnet` 

   W przykładzie użyto tego obrazu do uruchamiania aplikacji. Obraz zawiera środowisko uruchomieniowe programu ASP.NET Core oraz biblioteki i jest zoptymalizowany pod kątem uruchamiania aplikacji w środowisku produkcyjnym. Zaprojektowana pod kątem szybkiego wdrażania i uruchamiania aplikacji, są stosunkowo mały, więc zoptymalizowane pod kątem wydajności sieci hosta platformy Docker z rejestru platformy Docker. Tylko pliki binarne i zawartość wymaganą do uruchomienia aplikacji są kopiowane do kontenera. Zawartość jest gotowa do uruchomienia, umożliwiając najkrótszy czas z `Docker run` do uruchamiania aplikacji. Kompilacja kodu dynamicznej nie jest potrzebna w modelu platformy Docker.

## <a name="prerequisites"></a>Wymagania wstępne

* [.NET core SDK 2,2](https://www.microsoft.com/net/core)

* Klient platformy docker 18.03 lub nowszy

  * Dystrybucje systemu Linux
     * [CentOS](https://docs.docker.com/install/linux/docker-ce/centos/)
     * [Debian](https://docs.docker.com/install/linux/docker-ce/debian/)
     * [Fedora](https://docs.docker.com/install/linux/docker-ce/fedora/)
     * [Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
  * [macOS](https://docs.docker.com/docker-for-mac/install/)
  * [Windows](https://docs.docker.com/docker-for-windows/install/)

* [git](https://git-scm.com/download)

## <a name="download-the-sample-app"></a>Pobierz przykładową aplikację

* Pobierz próbkę, klonowanie [repozytorium Docker w programie .NET Core](https://github.com/dotnet/dotnet-docker): 

  ```console
  git clone https://github.com/dotnet/dotnet-docker
  ```

## <a name="run-the-app-locally"></a>Uruchamianie aplikacji lokalnie

* Przejdź do folderu projektu w *dotnet-docker/samples/aspnetapp/aspnetapp*.

* Uruchom następujące polecenie, aby skompilować i uruchomić aplikację lokalnie:

  ```console
  dotnet run
  ```

* Przejdź do `http://localhost:5000` w przeglądarce, aby przetestować aplikację.

* Naciśnij klawisze Ctrl + C, aby zatrzymać aplikację, w wierszu polecenia.

## <a name="run-in-a-linux-container"></a>Uruchom w kontenerze systemu Linux

* W kliencie platformy Docker Przełącz się do kontenerów systemu Linux.

* Przejdź do folderu pliku Dockerfile w *dotnet-docker/samples/aspnetapp*.

* Uruchom następujące polecenia, aby skompilować i uruchomić przykład na platformie Docker:

  ```console
  docker build -t aspnetapp .
  docker run -it --rm -p 5000:80 --name aspnetcore_sample aspnetapp
  ```

  `build` Argumenty polecenia:
  * Nazwa aspnetapp obrazu.
  * Wyszukaj plik Dockerfile w bieżącym folderze (kropki na końcu).

  Argumenty polecenia uruchomienia:
  * Przydziel pseudo-TTY i zostawić je otwarte, nawet jeśli nie jest dołączony. (Sama efektu jako `--interactive --tty`.)
  * Kontenera są automatycznie usuwane po wydaniu.
  * Mapowania portu 5000 na komputerze lokalnym porcie 80 w kontenerze.
  * Nazwa aspnetcore_sample kontenera.
  * Określ obraz aspnetapp.

* Przejdź do `http://localhost:5000` w przeglądarce, aby przetestować aplikację.

## <a name="run-in-a-windows-container"></a>Uruchamianie w kontenerze Windows

* W kliencie platformy Docker Przełącz się do kontenerów Windows.

Przejdź do folderu pliku platformy docker w `dotnet-docker/samples/aspnetapp`.

* Uruchom następujące polecenia, aby skompilować i uruchomić przykład na platformie Docker:

  ```console
  docker build -t aspnetapp .
  docker run -it --rm --name aspnetcore_sample aspnetapp
  ```

* Dla kontenerów Windows jest wymagany adres IP kontenera (przejście do `http://localhost:5000` nie będzie działać):
  * Otwórz innego wiersza polecenia.
  * Uruchom `docker ps` Aby wyświetlić uruchomione kontenery. Sprawdź kontener "aspnetcore_sample" istnieje.
  * Uruchom `docker exec aspnetcore_sample ipconfig` Aby wyświetlić adres IP kontenera. Dane wyjściowe polecenia będzie wyglądać następująco:

    ```console
    Ethernet adapter Ethernet:

       Connection-specific DNS Suffix  . : contoso.com
       Link-local IPv6 Address . . . . . : fe80::1967:6598:124:cfa3%4
       IPv4 Address. . . . . . . . . . . : 172.29.245.43
       Subnet Mask . . . . . . . . . . . : 255.255.240.0
       Default Gateway . . . . . . . . . : 172.29.240.1
    ```

* Skopiuj kontenera adres IPv4 (na przykład 172.29.245.43) i Wklej w pasku adresu przeglądarki, aby przetestować aplikację.

## <a name="build-and-deploy-manually"></a>Tworzenie i wdrażanie ręcznie

W niektórych przypadkach warto wdrożyć aplikację do kontenera przez skopiowanie plików aplikacji, które są wymagane w czasie wykonywania. W tej sekcji pokazano, jak wdrożyć ręcznie.

* Przejdź do folderu projektu w *dotnet-docker/samples/aspnetapp/aspnetapp*.

* Uruchom [publikowania dotnet](https://docs.microsoft.com/dotnet/core/tools/dotnet-publish.md) polecenia:

  ```console
  dotnet publish -c Release -o published
  ```

  Argumenty wiersza polecenia:
  * Utwórz aplikację w trybie wydania (wartość domyślna to tryb debugowania).
  * Tworzenie plików w *opublikowane* folderu.

* Uruchom aplikację.

  * W systemie Windows:

    ```console
    dotnet published\aspnetapp.dll
    ```

  * W systemie Linux:

    ```bash
    dotnet published/aspnetapp.dll
    ```

* Przejdź do `http://localhost:5000` można znaleźć na stronie głównej.

### <a name="the-dockerfile"></a>The Dockerfile

W tym pliku Dockerfile używana przez `docker build` polecenia został przeprowadzony wcześniej.  Używa ona `dotnet publish` taki sam sposób jak w tej sekcji, aby skompilować i wdrożyć.  

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

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Polecenie kompilacji platformy docker](https://docs.docker.com/engine/reference/commandline/build)
* [Docker, uruchom polecenie](https://docs.docker.com/engine/reference/commandline/run)
* [ASP.NET Core, Docker próbki](https://github.com/dotnet/dotnet-docker) (używaną w ramach tego samouczka.)
* [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](/aspnet/core/host-and-deploy/proxy-load-balancer)
* [Praca z narzędzia Docker programu Visual Studio](https://docs.microsoft.com/aspnet/core/publishing/visual-studio-tools-for-docker)
* [Debugowanie za pomocą programu Visual Studio Code](https://code.visualstudio.com/docs/nodejs/debugging-recipes#_debug-nodejs-in-docker-containers) 

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:
> [!div class="checklist"]
> * Przedstawia informacje na temat obrazów platformy Docker programu Microsoft .NET Core 
> * Pobrać przykładową aplikację platformy ASP.NET Core
> * Uruchamianie przykładowej aplikacji lokalnie
> * Uruchamianie przykładowej aplikacji w kontenerach systemu Linux
> * Uruchom aplikację przykładową, za pomocą w kontenerach Windows
> * Utworzeniu i wdrożeniu ręcznie

Następnie Dowiedz się więcej o narzędziach, których program Visual Studio zawiera do pracy z platformą Docker.

> [!div class="nextstepaction"]
> <xref:host-and-deploy/docker/visual-studio-tools-for-docker>
