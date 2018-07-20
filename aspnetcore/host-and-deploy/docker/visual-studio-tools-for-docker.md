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
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>Visual Studio Tools for Docker z platformą ASP.NET Core

[Program Visual Studio 2017](https://www.visualstudio.com/) obsługuje kompilowania, debugowania i uruchamiania konteneryzowanych platformy ASP.NET Core z aplikacji przeznaczonych dla platformy .NET Core. Są obsługiwane kontenerów systemów Windows i Linux.

## <a name="prerequisites"></a>Wymagania wstępne

* [Program Visual Studio 2017](https://www.visualstudio.com/) z **programowanie dla wielu platform .NET Core** obciążenia
* [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Instalacja i Konfiguracja

Instalacja platformy Docker, zapoznaj się z informacjami o [Docker for Windows: co należy wiedzieć przed zainstalowaniem](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) i zainstaluj [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).

**[Udostępnione dyski](https://docs.docker.com/docker-for-windows/#shared-drives)**  w Docker for Windows musi być skonfigurowana do obsługi mapowania woluminu i debugowania. Kliknij prawym przyciskiem myszy ikonę platformy Docker w zasobniku systemowym, wybierz **ustawień...** i wybierz **udostępnione dyski**. Wybierz dysk, na którym Docker przechowuje pliki. Wybierz **zastosować**.

![Dyski udostępnione](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Visual Studio 2017 w wersji 15.6 lub nowszej monitowanie, gdy **udostępnione dyski** nie są skonfigurowane.

## <a name="add-docker-support-to-an-app"></a>Dodaj obsługę platformy Docker do aplikacji

Aby dodać obsługę platformy Docker do projektu programu ASP.NET Core, projekt musi przeznaczony dla platformy .NET Core. Są obsługiwane kontenerów systemów Linux i Windows.

Podczas dodawania obsługi programu Docker do projektu, wybierz kontener systemu Linux lub Windows. Hosta platformy Docker musi działać ten sam typ kontenera. Aby zmienić typ kontenera, na uruchomione wystąpienie platformy Docker, kliknij prawym przyciskiem myszy ikonę platformy Docker w zasobniku systemowym, a następnie wybierz **przełączyć się do kontenerów Windows...**  lub **przełączyć się do kontenerów systemu Linux...** .

### <a name="new-app"></a>Nowa aplikacja

Podczas tworzenia nowej aplikacji za pomocą **aplikacji sieci Web programu ASP.NET Core** szablonów projektu wybierz **włączyć obsługę platformy Docker** pole wyboru:

![Zaznacz pole wyboru obsługę platformy Docker](visual-studio-tools-for-docker/_static/enable-docker-support-check box.png)

W przypadku platformy docelowej platformy .NET Core **OS** listy rozwijanej umożliwia wybór typu kontenera.

### <a name="existing-app"></a>Istniejąca aplikacja

Visual Studio Tools for Docker nie jest obsługiwane dodawanie platformy Docker do istniejącego projektu platformy ASP.NET Core przeznaczone dla .NET Framework. Dla projektów ASP.NET Core przeznaczone dla platformy .NET Core istnieją dwa sposoby dodawania obsługę platformy Docker za pomocą narzędzi. Otwórz projekt w programie Visual Studio i wybierz jedną z następujących opcji:

* Wybierz **obsługę platformy Docker** z **projektu** menu.
* Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Dodaj** > **obsługę platformy Docker**.

## <a name="docker-assets-overview"></a>Przegląd zasobów platformy docker

Dodaj program Visual Studio Tools for Docker *docker-compose* projekt do rozwiązania zawierającego następujące pliki:

* *.dockerignore*: zawiera listę wzorców plików i katalogów, które mają zostać wykluczone podczas generowania kontekstu kompilacji.
* *docker-compose.yml*: Podstawa [narzędzia Docker Compose](https://docs.docker.com/compose/overview/) umożliwiają zdefiniowanie kolekcji obrazów można skompilować i uruchomić przy użyciu pliku `docker-compose build` i `docker-compose run`odpowiednio.
* *docker-compose.override.yml*: opcjonalny plik odczytany przez narzędzia Docker Compose, zawierający konfigurację wartości zastąpień dla usług. Visual Studio wykonuje `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` do scalenia tych plików.

A *pliku Dockerfile*, przepisu do utworzenia końcowej obrazu platformy Docker, zostanie dodany do katalogu głównego projektu. Zapoznaj się [odwołanie do pliku Dockerfile](https://docs.docker.com/engine/reference/builder/) dla zrozumienia poleceń w nim. Tej konkretnej *pliku Dockerfile* używa [kompilacji wieloetapowych](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) zawierający cztery różne, o nazwie etapy kompilacji generują:

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

*Pliku Dockerfile* opiera się na [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) obrazu. Tego obrazu podstawowego obejmuje pakiety platformy ASP.NET Core NuGet, które zostały wstępnie trybie JIT na zwiększeniu wydajności uruchamiania.

*Docker-compose.yml* plik zawiera nazwę obrazu, który jest tworzony po uruchomieniu projektu:

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

W powyższym przykładzie `image: hellodockertools` generuje obraz `hellodockertools:dev` uruchamiania aplikacji **debugowania** trybu. `hellodockertools:latest` Obraz jest generowany, gdy aplikacja jest uruchamiana **wersji** trybu.

Nazwa obrazu przy użyciu prefiksu [usługi Docker Hub](https://hub.docker.com/) nazwy użytkownika (na przykład `dockerhubusername/hellodockertools`) Jeśli obraz, który zostanie przekazany do rejestru. Alternatywnie Zmień nazwę obrazu, aby zawierały adres URL rejestru prywatnego (na przykład `privateregistry.domain.com/hellodockertools`) w zależności od konfiguracji.

## <a name="debug"></a>Debugowanie

Wybierz **Docker** z poziomu pozycji Debuguj listy rozwijanej w pasku narzędzi, a następnie uruchamiania, debugowania aplikacji. **Docker** widoku **dane wyjściowe** oknie wyświetlane są następujące akcje miejsce:

* *Microsoft/aspnetcore* obraz środowiska uruchomieniowego jest uzyskiwany (Jeśli nie są już w pamięci podręcznej).
* *Microsoft/aspnetcore-build* kompilacji/publikowania obrazu jest uzyskiwany (Jeśli nie są już w pamięci podręcznej).
* *ASPNETCORE_ENVIRONMENT* ustawiono zmiennej środowiskowej `Development` w kontenerze.
* Port 80 jest udostępniane i mapowane na port dynamicznie przypisywanej nazwy localhost. Numer portu jest określany przez hosta platformy Docker i może być odpytywana za pomocą `docker ps` polecenia.
* Aplikacja jest kopiowana do kontenera.
* W debugerze do kontenera przy użyciu portu przypisywany dynamicznie, uruchamiana jest domyślna przeglądarka.

Wynikowy obraz platformy Docker jest *dev* obrazu aplikacji za pomocą *microsoft/aspnetcore* obrazów jako obrazu podstawowego. Uruchom `docker images` polecenia w pliku **Konsola Menedżera pakietów** okna (PMC). Wyświetlane są obrazy na komputerze:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> Obraz dev brakuje zawartość aplikacji jako **debugowania** konfiguracje używają instalowania woluminów oferują środowisko o iteracyjne. Aby wypchnąć obraz, należy użyć **wersji** konfiguracji.

Uruchom `docker ps` polecenia w konsoli zarządzania Pakietami. Zwróć uwagę, że aplikacja jest uruchomiona przy użyciu kontenera:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Edytuj i Kontynuuj

Zmiany plików statycznych i widokami Razor są automatycznie aktualizowane bez konieczności kroku kompilacji. Wprowadzić zmiany, Zapisz i Odśwież przeglądarkę, aby wyświetlić tę aktualizację.

Modyfikacje plików kodu wymagają kompilacja i ponowne uruchomienie Kestrel w kontenerze. Po wprowadzeniu zmian, należy użyć `CTRL+F5` na wykonanie procesu i uruchomić aplikację w kontenerze. Kontener platformy Docker nie jest ponownie skompilowany lub zatrzymana. Uruchom `docker ps` polecenia w konsoli zarządzania Pakietami. Zwróć uwagę, oryginalnym kontenerze jest nadal uruchomiona od 10 minut temu:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Publikowanie obrazów platformy Docker

Po zakończeniu cyklu programowanie i debugowanie aplikacji Visual Studio Tools for Docker pomagać w tworzeniu obraz produkcyjnych aplikacji. Zmień konfigurację menu rozwijane **wersji** i kompilowania aplikacji. Narzędzi tworzy obraz z *najnowsze* znacznik, który może zostać przeniesiony do prywatnego rejestru lub usługi Docker Hub.

Uruchom `docker images` polecenia w konsoli zarządzania Pakietami, aby wyświetlić listę obrazów:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> `docker images` Polecenie zwraca pośrednie obrazów za pomocą nazwy repozytorium i tagi zidentyfikowane jako  *\<Brak >* (niewymienione na liście powyżej). Te obrazy nienazwane są produkowane przez [kompilacji wieloetapowych](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *pliku Dockerfile*. Poprawiają wydajność tworzenia finalnego obrazu&mdash;tylko niezbędne warstwy są ponownie skompilowany, gdy wystąpią zmiany. Gdy pośrednie obrazy nie są już potrzebne, usuń je przy użyciu [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) polecenia.

Może to być oczekiwanie produkcji lub wersji obrazu na mniejszy rozmiar w porównaniu do *dev* obrazu. Ze względu na mapowanie woluminu, debuger i aplikacji były uruchamiane z komputera lokalnego, a nie w kontenerze. *Najnowsze* obraz ma spakowane kodu aplikacji niezbędnych do uruchomienia aplikacji na komputerze hosta. Dlatego delta jest rozmiar kodu aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Rozwiązywanie problemów z programowania Visual Studio 2017 przy użyciu rozwiązania Docker](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [Visual Studio Tools dla repozytorium GitHub platformy Docker](https://github.com/Microsoft/DockerTools)
