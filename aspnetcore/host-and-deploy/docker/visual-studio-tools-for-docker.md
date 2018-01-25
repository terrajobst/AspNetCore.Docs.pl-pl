---
title: Visual Studio Tools for Docker z platformy ASP.NET Core
author: spboyer
description: "Dowiedz się, jak containerize aplikacji platformy ASP.NET Core za pomocą narzędzi Visual Studio 2017 i Docker dla systemu Windows."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: caf0e423d8e6f61fd2470d1f4ea2dd93909c3696
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>Visual Studio Tools for Docker z platformy ASP.NET Core

[Visual Studio 2017](https://www.visualstudio.com/) obsługuje kompilowania, debugowania i uruchamianie platformy ASP.NET Core aplikacji przeznaczonych dla platformy .NET Framework lub .NET Core. Kontenery w systemach Windows i Linux są obsługiwane.

## <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio 2017](https://www.visualstudio.com/) z **aplikacji dla wielu platform .NET Core** obciążenia
* [Docker dla systemu Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Instalacja i Konfiguracja

Docker instalacji, przejrzyj informacje w [Docker dla systemu Windows: co należy wiedzieć przed zainstalowaniem](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) i zainstaluj [Docker dla systemu Windows](https://docs.docker.com/docker-for-windows/install/).

**[Udostępnione dyski](https://docs.docker.com/docker-for-windows/#shared-drives)**  Docker dla systemu Windows musi być skonfigurowana do obsługi mapowania woluminu i debugowanie. Kliknij prawym przyciskiem myszy ikonę Docker pasku zadań, zaznacz **ustawień...** i wybierz **udostępnione dyski**. Wybierz dysk, gdzie są przechowywane pliki Docker. Wybierz **zastosować**.

![Dyski udostępnione](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Monitowanie w Visual Studio 2017 wersji 15.6 lub nowszej, gdy **udostępnione dyski** nie są skonfigurowane.

## <a name="add-docker-support-to-an-app"></a>Dodaj obsługę Docker do aplikacji

Platforma docelowa projektu platformy ASP.NET Core Określa typy obsługiwanych kontenera. Projektach przeznaczonych dla platformy .NET Core obsługuje kontenery zarówno systemu Windows, jak i Linux. Projektów przeznaczanie tylko platformy .NET obsługuje kontenery systemu Windows.

Podczas dodawania obsługi Docker do projektu, wybierz systemu Windows lub Linux kontenera. Na hoście Docker musi być uruchomiony ten sam typ kontenera. Aby zmienić typ kontenera w uruchomione wystąpienie Docker, kliknij prawym przyciskiem myszy ikonę Docker pasku zadań i wybierz polecenie **przełączyć się do kontenerów Windows...**  lub **przełączyć się do kontenerów Linux...** .

### <a name="new-app"></a>Nowa aplikacja

Podczas tworzenia nowej aplikacji z **aplikacji sieci Web platformy ASP.NET Core** szablony projektów, wybierz opcję **Włącz obsługę Docker** wyboru:

![Włącz obsługę Docker wyboru](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

W przypadku platformy docelowej .NET Core **OS** listy rozwijanej umożliwia wybór typu kontenera.

### <a name="existing-app"></a>Istniejącej aplikacji

Visual Studio Tools for Docker nie obsługuje dodawania Docker do istniejącego projektu platformy ASP.NET Core przeznaczonych dla platformy .NET Framework. Dla platformy ASP.NET Core projektów przeznaczonych dla platformy .NET Core istnieją dwie opcje do dodawania obsługi Docker za pomocą narzędzi. Otwórz projekt w programie Visual Studio i wybierz jedną z następujących opcji:

* Wybierz **Obsługa Docker** z **projektu** menu.
* Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Dodaj** > **Obsługa Docker**.

## <a name="docker-assets-overview"></a>Omówienie zasoby docker

Dodaj Visual Studio Tools for Docker *docker-tworzą* projektu do rozwiązania, które zawierają:

* *.dockerignore*: zawiera listę wzorców plików i katalogów, które mają zostać wykluczone podczas generowania kontekst kompilacji.
* *docker-compose.yml*: base [rozwiązania Docker Compose](https://docs.docker.com/compose/overview/) plik używany do definiowania kolekcją obrazów, wbudowane i uruchom z `docker-compose build` i `docker-compose run`odpowiednio.
* *docker compose.override.yml*: opcjonalny plik odczytywane przez rozwiązania Docker Compose, zawierający konfigurację zastąpień dla usług. Wykonuje program Visual Studio `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` scalenie tych plików.

A *plik Dockerfile*, przepisu tworzenia finalnego obrazu Docker, jest dodawany do katalogu głównym projektu. Zapoznaj się [odwołania plik Dockerfile](https://docs.docker.com/engine/reference/builder/) dla zrozumienia poleceń w niej. Tego określonego *plik Dockerfile* używa [kompilacji wieloetapowym](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) zawierający cztery różne, o nazwie etapy kompilacji:

[!code-text[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

*Plik Dockerfile* opiera się na [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) obrazu. Ten obraz podstawowy obejmuje pakiety platformy ASP.NET Core NuGet, które zostały wstępnie przy użyciu kompilatora JIT do zwiększenia wydajności uruchamiania.

*Docker-compose.yml* plik zawiera nazwę obrazu, który jest tworzona po uruchomieniu projektu:

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

W powyższym przykładzie `image: hellodockertools` generuje obraz `hellodockertools:dev` po uruchomieniu aplikacji **debugowania** tryb. `hellodockertools:latest` Obraz jest generowany, gdy aplikacja jest uruchamiana **wersji** tryb.

Nazwa obrazu z prefiksu [Centrum Docker](https://hub.docker.com/) nazwy użytkownika (na przykład `dockerhubusername/hellodockertools`) Jeśli obrazu zostanie przekazany do rejestru. Możesz też zmienić nazwę obrazu uwzględnienie rejestru prywatny adres URL (na przykład `privateregistry.domain.com/hellodockertools`) w zależności od konfiguracji.

## <a name="debug"></a>Debugowanie

Wybierz **Docker** z listy rozwijanej w pasku narzędzi i rozpoczęcia debugowania aplikacji debugowania. **Docker** widoku **dane wyjściowe** oknie wyświetlane są następujące akcje zachodzące:

* *Microsoft/aspnetcore* obraz środowiska uruchomieniowego są uzyskiwane (Jeśli nie jest jeszcze w pamięci podręcznej).
* *Aspnetcore-microsoft Zbuduj* kompilacji/publikowania obrazu są uzyskiwane (Jeśli nie jest jeszcze w pamięci podręcznej).
* *ASPNETCORE_ENVIRONMENT* ustawiono zmiennej środowiskowej `Development` w kontenerze.
* Port 80 jest widoczne i zamapowane do portu przypisywane dynamicznie hosta lokalnego. Port jest określana przez hosta Docker i mogą być przeszukiwane przy `docker ps` polecenia.
* Aplikacja jest kopiowany do kontenera.
* Domyślna przeglądarka jest uruchamiana w debugerze kontenera przy użyciu portu przypisywane dynamicznie. 

Wynikowy obraz Docker jest *deweloperów* obrazu w aplikacji przy użyciu *microsoft/aspnetcore* obrazów jako obrazu podstawowego. Uruchom `docker images` w **Konsola Menedżera pakietów** okna (PMC). Wyświetlane są obrazy na komputerze:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> Zawartość aplikacji nie ma obrazu deweloperów jako **debugowania** konfiguracje Użyj zamontowania woluminu, aby zapewnić środowisko iteracji. Aby wypchnąć obrazu, należy użyć **wersji** konfiguracji.

Uruchom `docker ps` w kryterium. Należy zauważyć, że aplikacja zostanie uruchomiona przy użyciu kontenera:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Edytuj i Kontynuuj

Zmiany pliki statyczne i widokami Razor są automatycznie aktualizowane bez konieczności kroku kompilacji. Wprowadzić zmiany, Zapisz i Odśwież przeglądarkę, aby wyświetlić tę aktualizację.  

Modyfikacje plików kodu wymaga, kompilowania i ponownego uruchomienia Kestrel w kontenerze. Po wprowadzeniu zmian, użyj CTRL + F5, aby wykonać proces i uruchomić aplikację w kontenerze. Kontener Docker nie jest ponownie skompilowany lub zatrzymana. Uruchom `docker ps` w kryterium. Począwszy od 10 minut temu nadal działa powiadomienia oryginalnego kontenera:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Publikowanie obrazy usługi Docker

Po zakończeniu cyklu opracowanie i debugowania aplikacji Visual Studio Tools for Docker pomagają w tworzenie obrazu produkcyjnej aplikacji. Zmień konfigurację listy rozwijanej **wersji** i kompilowania aplikacji. Narzędzia tworzy obraz z *najnowsze* tagu, który może zostać przeniesiony do rejestru prywatnych lub Centrum Docker. 

Uruchom `docker images` w kryterium, aby wyświetlić listę obrazów:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> `docker images` Polecenie zwraca pośredniczące obrazów za pomocą nazwy repozytorium i tagi zidentyfikowane jako  *\<Brak >* (nie są wymienione powyżej). Te bez nazwy obrazów są produkowane przez [kompilacji wieloetapowym](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *plik Dockerfile*. Poprawiają wydajność tworzenia obrazu końcowego&mdash;tylko niezbędne warstwy są również przebudowany w przypadku wystąpienia zmian. Gdy pośredniczące obrazy nie są już potrzebne, usuń je przy użyciu [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) polecenia.

Może być oczekiwanie środowisko produkcyjne lub wersji obrazu na mniejszy rozmiar w porównaniu do *deweloperów* obrazu. Z powodu mapowania woluminu debugera i aplikacji były uruchomione na komputerze lokalnym, a nie w kontenerze. *Najnowsze* obraz ma spakowane kodu aplikacji niezbędne do uruchomienia aplikacji na komputerze hosta. W związku z tym delta jest rozmiar kodu aplikacji.
