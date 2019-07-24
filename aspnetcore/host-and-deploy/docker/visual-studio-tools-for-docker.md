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
# <a name="visual-studio-container-tools-with-aspnet-core"></a>Narzędzia kontenera programu Visual Studio z ASP.NET Core

Program Visual Studio 2017 i nowsze wersje obsługują kompilowanie, debugowanie i uruchamianie kontenerów ASP.NET Core aplikacji przeznaczonych dla platformy .NET Core. Obsługiwane są kontenery systemów Windows i Linux.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/docker/visual-studio-tools-for-docker/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

* [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z obciążeniem programistycznym dla **wielu platform .NET Core**

## <a name="installation-and-setup"></a>Instalacja i konfiguracja

Aby zainstalować platformę Docker, najpierw zapoznaj [się z informacjami w Docker for Windows: Co należy wiedzieć przed zainstalowaniem](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install)programu. Następnie zainstaluj [platformę Docker dla systemu Windows](https://docs.docker.com/docker-for-windows/install/).

**[Udostępnione dyski](https://docs.docker.com/docker-for-windows/#shared-drives)** w Docker for Windows muszą być skonfigurowane do obsługi mapowania woluminów i debugowania. Kliknij prawym przyciskiem myszy ikonę platformy Docker na pasku zadań, wybierz pozycję **Ustawienia**, a następnie wybierz pozycję **dyski udostępnione**. Wybierz dysk, na którym system Docker przechowuje pliki. Kliknij przycisk **zastosować**.

![Okno dialogowe umożliwiające wybranie lokalnego udostępniania dysku C dla kontenerów](visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Program Visual Studio 2017 w wersji 15,6 lub nowszej wyświetla monit, gdy **dyski udostępnione** nie są skonfigurowane.

## <a name="add-a-project-to-a-docker-container"></a>Dodawanie projektu do kontenera platformy Docker

Aby konteneryzowanie projekt ASP.NET Core, projekt musi być przeznaczony dla platformy .NET Core. Obsługiwane są kontenery systemu Linux i Windows.

Podczas dodawania obsługi platformy Docker do projektu wybierz kontener systemu Windows lub Linux. Na hoście platformy Docker musi być uruchomiony ten sam typ kontenera. Aby zmienić typ kontenera w uruchomionym wystąpieniu platformy Docker, kliknij prawym przyciskiem myszy ikonę Docker na pasku zadań i wybierz polecenie **Przełącz do kontenerów systemu Windows...** lub **Przejdź do kontenerów z systemem Linux..** ..

### <a name="new-app"></a>Nowa aplikacja

Podczas tworzenia nowej aplikacji z szablonami projektu **aplikacji sieci Web ASP.NET Core** zaznacz pole wyboru **Włącz obsługę platformy Docker** :

![Włącz obsługę platformy Docker — pole wyboru](visual-studio-tools-for-docker/_static/enable-docker-support-check-box.png)

Jeśli platformą docelową jest .NET Core, lista rozwijana **systemu operacyjnego** umożliwia wybór typu kontenera.

### <a name="existing-app"></a>Istniejąca aplikacja

W przypadku projektów ASP.NET Core przeznaczonych dla platformy .NET Core dostępne są dwie opcje dodawania obsługi platformy Docker za pomocą narzędzi. Otwórz projekt w programie Visual Studio, a następnie wybierz jedną z następujących opcji:

* Wybierz opcję **Obsługa platformy Docker** z menu **projekt** .
* Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Dodaj** > **obsługę platformy Docker**.

Narzędzia kontenerów programu Visual Studio nie obsługują dodawania platformy Docker do istniejącego .NET Framework ASP.NET Core projektu.

## <a name="dockerfile-overview"></a>Pliku dockerfile — Omówienie

*Pliku dockerfile*, przepis dotyczący tworzenia końcowego obrazu platformy Docker, jest dodawany do katalogu głównego projektu. Zapoznaj się z dokumentacją [pliku dockerfile](https://docs.docker.com/engine/reference/builder/) , aby zrozumieć polecenia w nim. W tym konkretnym *pliku dockerfile* jest stosowana wieloetapowa [kompilacja](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) z czterema różnymi etapami kompilacji o nazwie:

::: moniker range=">= aspnetcore-2.1"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile.original?highlight=1,6,14,17)]

Poprzedni *pliku dockerfile* opiera się na obrazie [Microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/) . Ten obraz podstawowy obejmuje środowisko uruchomieniowe ASP.NET Core i pakiety NuGet. Pakiety są skompilowane just-in-Time (JIT), aby zwiększyć wydajność uruchamiania.

Gdy pole wyboru Konfiguruj nowy projekt **dla protokołu HTTPS** jest zaznaczone, *pliku dockerfile* uwidacznia dwa porty. Jeden port jest używany na potrzeby ruchu HTTP; drugi port jest używany w przypadku protokołu HTTPS. Jeśli pole wyboru nie jest zaznaczone, pojedynczy port (80) zostanie uwidoczniony dla ruchu HTTP.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.0/HelloDockerTools/Dockerfile?highlight=1,5,13,16)]

Poprzedni *pliku dockerfile* opiera się na obrazie [Microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore/) . Ten obraz podstawowy zawiera ASP.NET Core pakiety NuGet, które są skompilowane just-in-Time (JIT), aby zwiększyć wydajność uruchamiania.

::: moniker-end

## <a name="add-container-orchestrator-support-to-an-app"></a>Dodawanie obsługi programu Orchestrator kontenera do aplikacji

Program Visual Studio 2017 w wersji 15,7 lub starszej [Docker Compose](https://docs.docker.com/compose/overview/) jako jedyne rozwiązanie aranżacji kontenerów. Docker Compose artefakty są dodawane za pośrednictwem **dodawania** > **obsługi platformy Docker**.

W programie Visual Studio 2017 w wersji 15,8 lub nowszej rozwiązanie aranżacji jest dodawane tylko wtedy, gdy jest to nakazujene. Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Dodaj** > **obsługę koordynatora kontenerów**. Oferowane są dwa różne opcje: [Docker Compose](#docker-compose) i [Service Fabric](#service-fabric).

### <a name="docker-compose"></a>Docker Compose

Narzędzia kontenera programu Visual Studio dodają projekt *Docker-Zredaguj* do rozwiązania z następującymi plikami:

* *Docker-Zredaguj. dcproj* &ndash; plik reprezentujący projekt. `<DockerTargetOS>` Zawiera element określający system operacyjny, który ma być używany.
* *. dockerignore* &ndash; Wyświetla listę wzorców plików i katalogów, które mają zostać wykluczone podczas generowania kontekstu kompilacji.
* *Docker-Compose. yml* &ndash; podstawowy plik [Docker Compose](https://docs.docker.com/compose/overview/) używany do definiowania kolekcji obrazów skompilowanych i uruchamianych z `docker-compose build` i `docker-compose run`, odpowiednio.
* *Docker-Compose. override. yml* &ndash; opcjonalny plik, Odczytaj według Docker Compose, z zastąpieniami konfiguracji dla usług. Program Visual Studio `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` wykonuje scalanie tych plików.

Plik *Docker-Compose. yml* odwołuje się do nazwy obrazu, który jest tworzony podczas działania projektu:

[!code-yaml[](visual-studio-tools-for-docker/samples/2.0/docker-compose.yml?highlight=5)]

W poprzednim przykładzie program generuje `image: hellodockertools` obraz `hellodockertools:dev` , gdy aplikacja jest uruchamiana w trybie **debugowania** . Obraz `hellodockertools:latest` jest generowany, gdy aplikacja jest uruchamiana w trybie **wydania** .

Przedrostek nazwy obrazu za pomocą nazwy użytkownika usługi [Docker Hub](https://hub.docker.com/) ( `dockerhubusername/hellodockertools`na przykład), jeśli obraz jest wypychany do rejestru. Alternatywnie można zmienić nazwę obrazu tak, aby zawierała adres URL rejestru prywatnego (na `privateregistry.domain.com/hellodockertools`przykład), w zależności od konfiguracji.

Jeśli chcesz mieć inne zachowanie na podstawie konfiguracji kompilacji (na przykład debugowanie lub wydanie), Dodaj określone pliki *platformy Docker* . Pliki powinny mieć nazwę zgodną z konfiguracją kompilacji (na przykład *Docker-Compose. vs. Debug. yml* i *Docker-Compose. vs. release. yml*) i umieszczaną w tej samej lokalizacji co plik *Docker-Compose-override. yml* . 

Przy użyciu plików przesłonięć specyficznych dla konfiguracji można określić różne ustawienia konfiguracji (takie jak zmienne środowiskowe lub punkty wejścia) dla konfiguracji debugowania i wydania kompilacji.

### <a name="service-fabric"></a>Service Fabric

Oprócz podstawowych [wymagań wstępnych](#prerequisites), rozwiązanie [Service Fabric](/azure/service-fabric/) aranżacji wymaga następujących wymagań wstępnych:

* [Zestaw SDK Microsoft Azure Service Fabric](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK) w wersji 2,6 lub nowszej
* Obciążenie programistyczne **platformy Azure** dla programu Visual Studio

Service Fabric nie obsługuje uruchamiania kontenerów systemu Linux w lokalnym klastrze projektowym w systemie Windows. Jeśli projekt używa już kontenera systemu Linux, program Visual Studio wyświetli komunikat z prośbą o przełączenie do kontenerów Windows.

Narzędzia kontenera programu Visual Studio wykonują następujące zadania:

* Dodaje *aplikację&gt;Project_NameServiceFabricprojektu aplikacji do rozwiązania. &lt;*
* Dodaje plik *pliku dockerfile* i *. dockerignore* do projektu ASP.NET Core. Jeśli *pliku dockerfile* już istnieje w projekcie ASP.NET Core, jego nazwa zostanie zmieniona na *pliku dockerfile. Original*. Tworzony jest nowy *pliku dockerfile*, podobny do następującego:

    [!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile)]

* Dodaje element do pliku csproj projektu ASP.NET Core:  `<IsServiceFabricServiceProject>`

    [!code-xml[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/HelloDockerTools.csproj?name=snippet_IsServiceFabricServiceProject)]

* Dodaje folder *PackageRoot* do projektu ASP.NET Core. Folder zawiera manifest usługi i ustawienia dla nowej usługi.

Aby uzyskać więcej informacji, zobacz [wdrażanie aplikacji .NET w kontenerze systemu Windows na platformie Azure Service Fabric](/azure/service-fabric/service-fabric-host-app-in-a-container).

## <a name="debug"></a>Debugowanie

Wybierz  pozycję Docker z listy rozwijanej Debuguj na pasku narzędzi i Rozpocznij debugowanie aplikacji. Widok **platformy Docker** okna **dane wyjściowe** zawiera następujące akcje:

::: moniker range=">= aspnetcore-2.1"

* Zostanie pobrany tag *2,1-aspnetcore-Runtime* obrazu środowiska uruchomieniowego *Microsoft/dotnet* (jeśli jeszcze nie znajduje się w pamięci podręcznej). Obraz instaluje ASP.NET Core i środowiska uruchomieniowe platformy .NET Core oraz powiązane biblioteki. Jest zoptymalizowany pod kątem uruchamiania aplikacji ASP.NET Core w środowisku produkcyjnym.
* Zmienna środowiskowa jest ustawiona na `Development` wewnątrz kontenera. `ASPNETCORE_ENVIRONMENT`
* Dostępne są dwa porty przypisane dynamicznie: jeden dla protokołu HTTP i jeden dla protokołu HTTPS. Do portu przypisanego do hosta lokalnego można wykonać zapytanie za `docker ps` pomocą polecenia.
* Aplikacja jest kopiowana do kontenera.
* Domyślna przeglądarka jest uruchamiana z debugerem dołączonym do kontenera przy użyciu dynamicznie przypisanego portu.

Utworzony obraz platformy Docker aplikacji jest otagowany jako *dev*. Obraz jest oparty na tagu *2,1-aspnetcore-Runtime* obrazu podstawowego *Microsoft/dotnet* . Uruchom polecenie w oknie **konsola Menedżera pakietów** (PMC). `docker images` Wyświetlane są obrazy na komputerze:

```console
REPOSITORY        TAG                     IMAGE ID      CREATED         SIZE
hellodockertools  dev                     d72ce0f1dfe7  30 seconds ago  255MB
microsoft/dotnet  2.1-aspnetcore-runtime  fcc3887985bb  6 days ago      255MB
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* Obraz środowiska uruchomieniowego *Microsoft/aspnetcore* został pobrany (jeśli nie jest jeszcze w pamięci podręcznej).
* Zmienna środowiskowa jest ustawiona na `Development` wewnątrz kontenera. `ASPNETCORE_ENVIRONMENT`
* Port 80 jest uwidoczniony i mapowany na dynamicznie przypisany port dla hosta lokalnego. Port jest określany przez hosta platformy Docker i można wykonać zapytania przy użyciu `docker ps` polecenia.
* Aplikacja jest kopiowana do kontenera.
* Domyślna przeglądarka jest uruchamiana z debugerem dołączonym do kontenera przy użyciu dynamicznie przypisanego portu.

Utworzony obraz platformy Docker aplikacji jest otagowany jako *dev*. Obraz jest oparty na obrazie bazowym *Microsoft/aspnetcore* . Uruchom polecenie w oknie **konsola Menedżera pakietów** (PMC). `docker images` Wyświetlane są obrazy na komputerze:

```console
REPOSITORY            TAG  IMAGE ID      CREATED        SIZE
hellodockertools      dev  5fafe5d1ad5b  4 minutes ago  347MB
microsoft/aspnetcore  2.0  c69d39472da9  13 days ago    347MB
```

::: moniker-end

> [!NOTE]
> Obraz *deweloperski* nie zawiera zawartości aplikacji, ponieważ konfiguracje **debugowania** używają instalacji woluminu do zapewnienia iteracyjnego środowiska. Aby wypchnąć obraz, użyj konfiguracji **wydania** .

`docker ps` Uruchom polecenie w PMC. Zauważ, że aplikacja jest uruchomiona przy użyciu kontenera:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Edytuj i Kontynuuj

Zmiany w plikach statycznych i widokach Razor są automatycznie aktualizowane bez konieczności wykonywania kroku kompilacji. Wprowadź zmiany, Zapisz i Odśwież przeglądarkę, aby wyświetlić aktualizację.

Modyfikacje pliku kodu wymagają kompilacji i ponownego uruchomienia Kestrel w kontenerze. Po wprowadzeniu zmiany Użyj `CTRL+F5` do wykonania procesu i uruchomienia aplikacji w kontenerze. Kontener platformy Docker nie został odbudowany lub zatrzymany. `docker ps` Uruchom polecenie w PMC. Zwróć uwagę, że oryginalny kontener nadal działa po 10 minutach temu:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Publikowanie obrazów platformy Docker

Po zakończeniu cyklu opracowywania i debugowania aplikacji narzędzia kontenerów programu Visual Studio ułatwiają tworzenie obrazu produkcyjnego aplikacji. Zmień listę rozwijaną konfiguracji, aby **zwolnić** i skompilować aplikację. Narzędzie uzyskuje obraz kompilowania/publikowania z usługi Docker Hub (jeśli jeszcze nie znajduje się w pamięci podręcznej). Obraz jest tworzony przy użyciu *najnowszego* tagu, który można wypchnąć do rejestru prywatnego lub z koncentratora platformy Docker.

`docker images` Uruchom polecenie w kryterium PMC, aby wyświetlić listę obrazów. Wyświetlane są dane wyjściowe podobne do następujących:

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

Obrazy `microsoft/aspnetcore-build` i `microsoft/aspnetcore` wymienione w`microsoft/dotnet` poprzednich danych wyjściowych są zastępowane obrazami programu .NET Core 2,1. Aby uzyskać więcej informacji, zobacz [anons migracji repozytoriów platformy Docker](https://github.com/aspnet/Announcements/issues/298).

::: moniker-end

> [!NOTE]
> Polecenie zwraca obrazy pośredniczące z nazwami repozytoriów i tagami identyfikowanych jako  *\<brak >* (niewymienione powyżej). `docker images` Te obrazy bez nazwy są tworzone przez Wieloetapową [kompilację](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *pliku dockerfile*. Zwiększają one wydajność tworzenia obrazu&mdash;końcowego tylko wtedy, gdy nastąpi zmiana. Gdy obrazy pośredniczące nie są już potrzebne, usuń je za pomocą polecenia [Docker RMI](https://docs.docker.com/engine/reference/commandline/rmi/) .

Może się okazać, że oczekuje się, że rozmiar obrazu produkcyjnego lub wydania będzie mniejszy w porównaniu do obrazu *deweloperskiego* . Ze względu na mapowanie woluminu debuger i aplikacja działały z komputera lokalnego, a nie w kontenerze. *Najnowszy* obraz zawiera spakowany kod aplikacji, aby uruchomić aplikację na komputerze hosta. W związku z tym Delta jest rozmiarem kodu aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Programowanie kontenerów za pomocą programu Visual Studio](/visualstudio/containers)
* [Service Fabric platformy Azure: Przygotuj środowisko programistyczne](/azure/service-fabric/service-fabric-get-started)
* [Wdrażanie aplikacji .NET w kontenerze systemu Windows na platformie Azure Service Fabric](/azure/service-fabric/service-fabric-host-app-in-a-container)
* [Rozwiązywanie problemów z programowaniem programu Visual Studio przy użyciu platformy Docker](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [Repozytorium usługi GitHub dla narzędzi kontenerów programu Visual Studio](https://github.com/Microsoft/DockerTools)
