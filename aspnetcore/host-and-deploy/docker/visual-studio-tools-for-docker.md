---
title: Visual Studio Tools for Docker z platformą ASP.NET Core
author: spboyer
description: Dowiedz się, jak za pomocą narzędzi Visual Studio 2017 i platformy Docker for Windows konteneryzowanie aplikacji ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/26/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: 962c35cb1487dacd93fd78d09e2417ef77387e42
ms.sourcegitcommit: 75bf5fdbfdcb6a7cfe8fe207b9ff37655ccbacd4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39275866"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>Visual Studio Tools for Docker z platformą ASP.NET Core

Program Visual Studio 2017 obsługuje kompilowania, debugowania i uruchamiania konteneryzowanych platformy ASP.NET Core z aplikacji przeznaczonych dla platformy .NET Core. Są obsługiwane kontenerów systemów Windows i Linux.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/docker/visual-studio-tools-for-docker/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

* [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)
* [Program Visual Studio 2017](https://www.visualstudio.com/) z **programowanie dla wielu platform .NET Core** obciążenia

## <a name="installation-and-setup"></a>Instalacja i Konfiguracja

Instalacja platformy Docker, najpierw zapoznaj się z informacjami o [Docker for Windows: co należy wiedzieć przed zainstalowaniem](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install). Następnie zainstaluj [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).

**[Udostępnione dyski](https://docs.docker.com/docker-for-windows/#shared-drives)**  w Docker for Windows musi być skonfigurowana do obsługi mapowania woluminu i debugowania. Kliknij prawym przyciskiem myszy ikonę platformy Docker w zasobniku systemowym, wybierz **ustawienia**i wybierz **udostępnione dyski**. Wybierz dysk, na którym Docker przechowuje pliki. Kliknij przycisk **zastosować**.

![Okno dialogowe, aby wybrać lokalny dysk C do udostępniania dla kontenerów](visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Visual Studio 2017 w wersji 15.6 lub nowszej monitowanie, gdy **udostępnione dyski** nie są skonfigurowane.

## <a name="add-a-project-to-a-docker-container"></a>Dodaj projekt do kontenera platformy Docker

Aby konteneryzowanie projektu ASP.NET Core, projekt musi przeznaczony dla platformy .NET Core. Są obsługiwane kontenerów systemów Linux i Windows.

Podczas dodawania obsługi programu Docker do projektu, wybierz kontener systemu Linux lub Windows. Hosta platformy Docker musi działać ten sam typ kontenera. Aby zmienić typ kontenera, na uruchomione wystąpienie platformy Docker, kliknij prawym przyciskiem myszy ikonę platformy Docker w zasobniku systemowym, a następnie wybierz **przełączyć się do kontenerów Windows...**  lub **przełączyć się do kontenerów systemu Linux...** .

### <a name="new-app"></a>Nowa aplikacja

Podczas tworzenia nowej aplikacji za pomocą **aplikacji sieci Web programu ASP.NET Core** szablonów projektu wybierz **włączyć obsługę platformy Docker** pole wyboru:

![Zaznacz pole wyboru obsługę platformy Docker](visual-studio-tools-for-docker/_static/enable-docker-support-check-box.png)

W przypadku platformy docelowej platformy .NET Core **OS** listy rozwijanej umożliwia wybór typu kontenera.

### <a name="existing-app"></a>Istniejąca aplikacja

Dla projektów ASP.NET Core przeznaczone dla platformy .NET Core istnieją dwa sposoby dodawania obsługę platformy Docker za pomocą narzędzi. Otwórz projekt w programie Visual Studio i wybierz jedną z następujących opcji:

* Wybierz **obsługę platformy Docker** z **projektu** menu.
* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Dodaj** > **obsługę platformy Docker**.

Visual Studio Tools for Docker nie jest obsługiwane dodawanie platformy Docker do istniejącego projektu platformy ASP.NET Core przeznaczone dla .NET Framework.

## <a name="dockerfile-overview"></a>Plik Dockerfile — omówienie

A *pliku Dockerfile*, przepisu do utworzenia końcowej obrazu platformy Docker, zostanie dodany do katalogu głównego projektu. Zapoznaj się [odwołanie do pliku Dockerfile](https://docs.docker.com/engine/reference/builder/) dla zrozumienia poleceń w nim. Tej konkretnej *pliku Dockerfile* używa [kompilacji wieloetapowych](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) z czterech różnych, o nazwie etapy kompilacji generują:

::: moniker range=">= aspnetcore-2.1"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile.original?highlight=1,6,14,17)]

Poprzedni *pliku Dockerfile* opiera się na [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/) obrazu. Ten podstawowy obraz zawiera środowisko uruchomieniowe programu ASP.NET Core i pakiety NuGet. Pakiety są just-in-time (JIT) skompilowany w celu zwiększenia wydajności uruchamiania.

Gdy okno dialogowe nowego projektu w **Konfigurowanie protokołu HTTPS** pole wyboru jest zaznaczone, *pliku Dockerfile* udostępnia dwa porty. Jeden port jest używany do ruchu HTTP; inne port jest używany do obsługi protokołu HTTPS. Jeśli nie jest zaznaczone pole wyboru, jednego portu (80) jest uwidaczniany dla ruchu HTTP.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.0/HelloDockerTools/Dockerfile?highlight=1,5,13,16)]

Poprzedni *pliku Dockerfile* opiera się na [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore/) obrazu. Tego obrazu podstawowego zawiera pakiety platformy ASP.NET Core NuGet, które są just-in-time (JIT) skompilowany w celu zwiększenia wydajności uruchamiania.

::: moniker-end

## <a name="add-container-orchestrator-support-to-an-app"></a>Dodawanie obsługi orkiestratora kontenerów do aplikacji

Obsługa Visual Studio 2017 w wersji 15.7 lub wcześniejszych [narzędzia Docker Compose](https://docs.docker.com/compose/overview/) jako rozwiązania do organizowania kontenerów wyłącznie. Artefakty narzędzia Docker Compose są dodane za pośrednictwem **Dodaj** > **obsługę platformy Docker**.

Visual Studio 2017 w wersji, należy zachować 15,8 lub nowszej, Dodaj rozwiązania do organizowania tylko wtedy, gdy zgodnie z instrukcjami otrzymywanymi. Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Dodaj** > **obsługi Orkiestratora kontenerów**. Są dostępne dwie różne opcje: [narzędzia Docker Compose](#docker-compose) i [usługi Service Fabric](#service-fabric).

### <a name="docker-compose"></a>Narzędzia docker Compose

Dodaj program Visual Studio Tools for Docker *docker-compose* projekt do rozwiązania przy użyciu następujących plików:

* *docker compose.dcproj* &ndash; pliku reprezentujący projektu. Obejmuje `<DockerTargetOS>` elementu, określając system operacyjny, który ma być używany.
* *.dockerignore* &ndash; zawiera listę wzorców plików i katalogów, które mają zostać wykluczone podczas generowania kontekstu kompilacji.
* *docker-compose.yml* &ndash; base [narzędzia Docker Compose](https://docs.docker.com/compose/overview/) plik wykorzystywany do definiowania kolekcję obrazów, skompilować i uruchomić z `docker-compose build` i `docker-compose run`, odpowiednio.
* *docker-compose.override.yml* &ndash; opcjonalny plik odczytywane przez narzędzia Docker Compose za pomocą konfiguracji zastąpień dla usług. Visual Studio wykonuje `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` do scalenia tych plików.

*Docker-compose.yml* plik odwołuje się nazwa obrazu, który jest tworzony po uruchomieniu projektu:

[!code-yaml[](visual-studio-tools-for-docker/samples/2.0/docker-compose.yml?highlight=5)]

W powyższym przykładzie `image: hellodockertools` generuje obraz `hellodockertools:dev` uruchamiania aplikacji **debugowania** trybu. `hellodockertools:latest` Obraz jest generowany, gdy aplikacja jest uruchamiana **wersji** trybu.

Nazwa obrazu przy użyciu prefiksu [usługi Docker Hub](https://hub.docker.com/) nazwy użytkownika (na przykład `dockerhubusername/hellodockertools`) Jeśli obraz jest wypchnięte do rejestru. Alternatywnie Zmień nazwę obrazu, aby zawierały adres URL rejestru prywatnego (na przykład `privateregistry.domain.com/hellodockertools`) w zależności od konfiguracji.

### <a name="service-fabric"></a>Usługa Service Fabric

Oprócz base [wymagania wstępne](#prerequisites), [usługi Service Fabric](/azure/service-fabric/) rozwiązania do organizowania zapotrzebowania na następujące wymagania wstępne:

* [Zestaw SDK Microsoft Azure Service Fabric](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK) wersji 2.6 lub nowszej
* W programie Visual Studio 2017 **programowanie na platformie Azure** obciążenia

Usługa Service Fabric nie obsługuje uruchamianie kontenerów systemu Linux w lokalnym klastrze projektowym na Windows. Jeśli projekt jest już używany w kontenerze systemu Linux, aby przełączyć się do kontenerów Windows wyświetla monit o Visual Studio.

Visual Studio Tools for Docker, wykonaj następujące czynności:

* Dodaje  *&lt;project_name&gt;aplikacji* **aplikacji usługi Service Fabric** projektu do rozwiązania.
* Dodaje *pliku Dockerfile* i *.dockerignore* pliku do projektu programu ASP.NET Core. Jeśli *pliku Dockerfile* już istnieje w projekcie platformy ASP.NET Core została zmieniona na *Dockerfile.original*. Nowy *pliku Dockerfile*, podobny do poniższego, zostanie utworzony:

    [!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile)]

* Dodaje `<IsServiceFabricServiceProject>` elementu do projektu programu ASP.NET Core *.csproj* pliku:

    [!code-xml[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/HelloDockerTools.csproj?name=snippet_IsServiceFabricServiceProject)]

* Dodaje *PackageRoot* folderu do projektu programu ASP.NET Core. Folder zawiera manifestu usługi oraz ustawienia dla nowej usługi.

Aby uzyskać więcej informacji, zobacz [wdrażania aplikacji .NET w kontenerze Windows w usłudze Azure Service Fabric](/azure/service-fabric/service-fabric-host-app-in-a-container).

## <a name="debug"></a>Debugowanie

Wybierz **Docker** z poziomu pozycji Debuguj listy rozwijanej w pasku narzędzi, a następnie uruchamiania, debugowania aplikacji. **Docker** widoku **dane wyjściowe** oknie wyświetlane są następujące akcje miejsce:

::: moniker range=">= aspnetcore-2.1"

* *2.1 — aspnetcore-środowiska uruchomieniowego* tag *microsoft/dotnet* obraz środowiska uruchomieniowego jest uzyskiwany (Jeśli nie są już w pamięci podręcznej). Obraz, który instaluje środowiska uruchomieniowe platformy ASP.NET Core i .NET Core i skojarzonymi z nimi bibliotekami. Jest zoptymalizowany do uruchamiania aplikacji ASP.NET Core w środowisku produkcyjnym.
* `ASPNETCORE_ENVIRONMENT` Ustawiono zmiennej środowiskowej `Development` w kontenerze.
* Dwa dynamicznie przydzielanego porty są dostępne: jeden dla protokołu HTTP i jeden do obsługi protokołu HTTPS. Numer portu przypisany do hosta lokalnego może być odpytywany za pomocą `docker ps` polecenia.
* Aplikacja jest kopiowana do kontenera.
* W debugerze do kontenera przy użyciu portu przypisywany dynamicznie, uruchamiana jest domyślna przeglądarka.

Wynikowy obraz platformy Docker w aplikacji zostanie oznaczony jako *dev*. Obraz, który jest oparty na *2.1 — aspnetcore-środowiska uruchomieniowego* tag *microsoft/dotnet* obrazu podstawowego. Uruchom `docker images` polecenia w pliku **Konsola Menedżera pakietów** okna (PMC). Wyświetlane są obrazy na komputerze:

```console
REPOSITORY        TAG                     IMAGE ID      CREATED         SIZE
hellodockertools  dev                     d72ce0f1dfe7  30 seconds ago  255MB
microsoft/dotnet  2.1-aspnetcore-runtime  fcc3887985bb  6 days ago      255MB
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* *Microsoft/aspnetcore* obraz środowiska uruchomieniowego jest uzyskiwany (Jeśli nie są już w pamięci podręcznej).
* `ASPNETCORE_ENVIRONMENT` Ustawiono zmiennej środowiskowej `Development` w kontenerze.
* Port 80 jest udostępniane i mapowane na port dynamicznie przypisywanej nazwy localhost. Numer portu jest określany przez hosta platformy Docker i może być odpytywana za pomocą `docker ps` polecenia.
* Aplikacja jest kopiowana do kontenera.
* W debugerze do kontenera przy użyciu portu przypisywany dynamicznie, uruchamiana jest domyślna przeglądarka.

Wynikowy obraz platformy Docker w aplikacji zostanie oznaczony jako *dev*. Obraz, który jest oparty na *microsoft/aspnetcore* obrazu podstawowego. Uruchom `docker images` polecenia w pliku **Konsola Menedżera pakietów** okna (PMC). Wyświetlane są obrazy na komputerze:

```console
REPOSITORY            TAG  IMAGE ID      CREATED        SIZE
hellodockertools      dev  5fafe5d1ad5b  4 minutes ago  347MB
microsoft/aspnetcore  2.0  c69d39472da9  13 days ago    347MB
```

::: moniker-end

> [!NOTE]
> *Dev* obraz nie ma zawartość aplikacji jako **debugowania** konfiguracje używają instalowania woluminów oferują środowisko o iteracyjne. Aby wypchnąć obraz, należy użyć **wersji** konfiguracji.

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

Po zakończeniu cyklu programowanie i debugowanie aplikacji Visual Studio Tools for Docker pomagać w tworzeniu obraz produkcyjnych aplikacji. Zmień konfigurację menu rozwijane **wersji** i kompilowania aplikacji. Narzędzi uzyskuje kompilacji/publikowania obrazu z usługi Docker Hub (Jeśli nie jest już w pamięci podręcznej). Obraz jest generowany przy użyciu *najnowsze* znacznik, który może zostać przeniesiony do prywatnego rejestru lub usługi Docker Hub.

Uruchom `docker images` polecenia w konsoli zarządzania Pakietami, aby wyświetlić listę obrazów. Wyświetlone dane wyjściowe podobne do następujących:

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

`microsoft/aspnetcore-build` i `microsoft/aspnetcore` obrazów wymienione w powyższym danych wyjściowych są zastępowane `microsoft/dotnet` obrazów, począwszy od platformy .NET Core 2.1. Aby uzyskać więcej informacji, zobacz [ogłoszenie migracji repozytoriów platformy Docker](https://github.com/aspnet/Announcements/issues/298).

::: moniker-end

> [!NOTE]
> `docker images` Polecenie zwraca pośrednie obrazów za pomocą nazwy repozytorium i tagi zidentyfikowane jako  *\<Brak >* (niewymienione na liście powyżej). Te obrazy nienazwane są produkowane przez [kompilacji wieloetapowych](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *pliku Dockerfile*. Poprawiają wydajność tworzenia finalnego obrazu&mdash;tylko niezbędne warstwy są ponownie skompilowany, gdy wystąpią zmiany. Gdy pośrednie obrazy nie są już potrzebne, usuń je przy użyciu [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) polecenia.

Może to być oczekiwanie produkcji lub wersji obrazu na mniejszy rozmiar w porównaniu do *dev* obrazu. Ze względu na mapowanie woluminu, debuger i aplikacji były uruchamiane z komputera lokalnego, a nie w kontenerze. *Najnowsze* obraz ma spakowane kodu aplikacji niezbędnych do uruchomienia aplikacji na komputerze hosta. Dlatego delta jest rozmiar kodu aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Usługa Azure Service Fabric: Przygotowywanie środowiska projektowego](/azure/service-fabric/service-fabric-get-started)
* [Wdrażanie aplikacji .NET w kontenerze Windows w usłudze Azure Service Fabric](/azure/service-fabric/service-fabric-host-app-in-a-container)
* [Rozwiązywanie problemów z programowania Visual Studio 2017 przy użyciu rozwiązania Docker](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [Visual Studio Tools dla repozytorium GitHub platformy Docker](https://github.com/Microsoft/DockerTools)
