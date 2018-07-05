---
uid: mvc/overview/deployment/docker
title: Migrowanie aplikacji ASP.NET MVC do kontenerów systemu Windows
description: Dowiedz się, jak wykonać istniejącą aplikację ASP.NET MVC i uruchomienia jej w kontenerze platformy Docker Windows
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.topic: article
ms.technology: dotnet-mvc
ms.devlang: dotnet
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: fa010e795878b26c79dbe04ef0017373283c4269
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403022"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a>Migrowanie aplikacji ASP.NET MVC do kontenerów systemu Windows

Uruchomienie istniejącej aplikacji opartych na programie .NET Framework w kontenerze Windows nie wymaga żadnych zmian w aplikacji. Aby uruchomić aplikację w kontenerze Windows tworzenia obrazu Docker zawierającego aplikację i uruchamianie kontenera. W tym temacie wyjaśniono, jak wykonać istniejące [aplikacji platformy ASP.NET MVC](http://www.asp.net/mvc) oraz wdrożyć ją w kontenerze Windows.

Możesz zaczynać istniejącą aplikację ASP.NET MVC, a następnie tworzyć zasoby opublikowanych przy użyciu programu Visual Studio. Możesz używać platformy Docker do tworzenia obrazu, który zawiera i uruchamia aplikację. Będziesz przejdź do witryny, uruchomione w kontenerze Windows i sprawdź, czy aplikacja działa.

W tym artykule założono podstawową wiedzę na temat platformy docker. Informacje na temat platformy Docker, czytając [Docker — omówienie](https://docs.docker.com/engine/understanding-docker/).

Aplikacja, które będą uruchamiane w kontenerze jest prostą witrynę sieci Web, który losowo odpowiedzi na pytania. Ta aplikacja to podstawowa aplikacja MVC bez uwierzytelniania lub Magazyn bazy danych; Dzięki temu można skoncentrować się na przejście z warstwy internetowej do kontenera. Tematy w przyszłości pokazują, jak przenosić i zarządzanie nimi w konteneryzowanych aplikacji z magazynu trwałego.

Przenoszenie aplikacji obejmuje następujące kroki:

1. [Tworzenie zadania Publikuj do tworzenia zasobów dla obrazu.](#publish-script)
1. [Tworzenie obrazu platformy Docker, który uruchomi aplikację.](#build-the-image)
1. [Uruchamianie kontenera platformy Docker działającą obrazu.](#start-a-container)
1. [Sprawdzanie aplikacji za pomocą przeglądarki.](#verify-in-the-browser)

[Aplikacja](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) znajduje się w witrynie GitHub.

## <a name="prerequisites"></a>Wymagania wstępne

Musi być uruchomiona na komputerze deweloperskim

- [Rocznicowa aktualizacja systemu Windows 10](https://www.microsoft.com/software-download/windows10/) (lub nowszego) lub [systemu Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (lub nowszej).
- [Docker for Windows](https://docs.docker.com/docker-for-windows/) -Beta 26 wersja stabilna 1.13.0 lub 1.12 (lub nowsze wersje)
- [Program Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx).

> [!IMPORTANT]
> Jeśli używasz systemu Windows Server 2016, postępuj zgodnie z instrukcjami dotyczącymi [wdrażania hosta kontenera — system Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).

Po zainstalowaniu i uruchomieniu programu Docker, kliknij prawym przyciskiem myszy ikonę na pasku zadań i wybierz pozycję **przełączyć się do kontenerów Windows**. Jest to wymagane do uruchomienia obrazów Docker opartych na Windows. To polecenie zajmuje kilka sekund, aby wykonać:

![Windows Container][windows-container]

## <a name="publish-script"></a>Publikowanie skryptu

Zbierz wszystkie zasoby, które są potrzebne do załadowania do obrazu platformy Docker w jednym miejscu. Możesz użyć programu Visual Studio **Publikuj** polecenie, aby utworzyć profil publikowania dla aplikacji. Ten profil zostanie umieścić wszystkie zasoby w jednym drzewie katalogu, które można skopiować do obrazu docelowego w dalszej części tego samouczka.

**Kroki publikowania**

1. Kliknij prawym przyciskiem myszy projekt sieci web w programie Visual Studio, a następnie wybierz pozycję **Publikuj**.
1. Kliknij przycisk **przycisk profil niestandardowy**, a następnie wybierz pozycję **System plików** jako metody.
1. Wybierz katalog. Zgodnie z Konwencją pobrane próbki `bin\Release\PublishOutput`.

![Publikowanie połączenia][publish-connection]

Otwórz **opcji publikowania pliku** części **ustawienia** kartę. Wybierz **Precompile podczas publikowania**. Tego rodzaju optymalizacji oznacza, że będziesz kompilowanie widoków w kontenerze platformy Docker, kopiowane są wstępnie skompilowane widoków.

![Ustawienia publikowania][publish-settings]

Kliknij przycisk **Publikuj**, i Visual Studio skopiuje wszystkie zasoby potrzebne do folderu docelowego.

## <a name="build-the-image"></a>Tworzenie obrazu

Zdefiniuj obraz Docker w pliku Dockerfile. Plik Dockerfile zawiera instrukcje dotyczące obraz podstawowy, dodatkowe składniki, aplikację, którą chcesz uruchomić i inne obrazy konfiguracji.  Plik Dockerfile stanowi dane wejściowe `docker build` polecenia, co powoduje utworzenie obrazu.

Utworzysz obraz na podstawie `microsoft/aspnet` obrazów znajdujących się na [usługi Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).
Obraz podstawowy `microsoft/aspnet`, jest obrazem systemu Windows Server. Zawiera on Windows Server Core, IIS i programu ASP.NET 4.6.2. Po uruchomieniu tego obrazu w kontenerze, nastąpi automatyczne uruchomienie usług IIS i zainstalowanych witryn sieci Web.

Plik Dockerfile, który tworzy obraz wygląda następująco:

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

Istnieje nie `ENTRYPOINT` polecenia w tym pliku Dockerfile. Nie potrzebujesz. Podczas uruchamiania systemu Windows Server z usługami IIS, proces IIS jest punkt wejścia, który jest skonfigurowany do uruchamiania w obrazie podstawowym aspnet.

Uruchom polecenie kompilacji platformy Docker, aby utworzyć obraz uruchamiający aplikację platformy ASP.NET. Aby to zrobić, Otwórz okno programu PowerShell w katalogu projektu i wpisz następujące polecenie w katalogu rozwiązania:

```console
docker build -t mvcrandomanswers .
```

To polecenie utworzy nowy obraz zgodnie z instrukcjami z pliku Dockerfile nazewnictwa (-t znakowanie) obraz jako mvcrandomanswers. Może to obejmować ściąganie obrazu podstawowego z [usługi Docker Hub](http://hub.docker.com), a następnie dodanie aplikacji do tego obrazu.

Po wykonaniu tego polecenia można uruchomić `docker images` polecenie, aby wyświetlić informacje o nowym obrazie:

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

Identyfikator obrazu będzie różna na swojej maszynie. Teraz uruchommy aplikację.

## <a name="start-a-container"></a>Uruchamianie kontenera

Uruchamianie kontenera, wykonując następujące `docker run` polecenia:

```console
docker run -d --name randomanswers mvcrandomanswers
```

`-d` Argument nakazuje platformy Docker można uruchomić obrazu w trybie odłączonym. Oznacza to, że obraz platformy Docker działa bez połączenia z bieżącej powłoce.

W wielu przykładach platformy docker może zostać wyświetlony -p do mapowania portów kontenera i hosta. Domyślny obraz aspnet skonfigurował już kontener do nasłuchiwania na porcie 80 i udostępnić ją. 

`--name randomanswers` Umożliwia nadanie nazwy działającemu kontenerowi. Można używać tej nazwy zamiast Identyfikatora kontenera, w większości poleceń.

`mvcrandomanswers` Nazywa się obraz do uruchomienia.

## <a name="verify-in-the-browser"></a>Sprawdź, czy w przeglądarce

> [!NOTE]
> W bieżącej wersji kontenerów Windows nie może przejść do `http://localhost`.
> Jest to znane zachowanie w WinNAT i rozwiąże go w przyszłości. Do momentu, która jest skierowana, musisz użyć adresu IP kontenera.

Po uruchomieniu kontenera Znajdź jego adres IP, czemu możesz nawiązywać połączenia z działającym kontenerem w przeglądarce:

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

Łączenie z działającym kontenerem przy użyciu adresu IPv4 `http://172.31.194.61` w przykładzie przedstawionym. Typ tego adresu URL w przeglądarce i powinien zostać wyświetlony działającej witryny.

> [!NOTE]
> Niektóre programy sieci VPN lub serwer proxy może uniemożliwić przejście do witryny.
> Można tymczasowo wyłączyć, aby upewnić się, że kontener działa.

Zawiera katalog przykładu w usłudze GitHub [skrypt programu PowerShell](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) , który jest wykonywany tych poleceń dla Ciebie. Otwórz okno programu PowerShell, zmień katalog na katalog rozwiązania i wpisz:

```console
./run.ps1
```

Powyższe polecenie tworzy obraz, wyświetla listę obrazów na swojej maszynie, uruchamia kontener i wyświetla adres IP dla tego kontenera.

Aby zatrzymać kontener, należy wydać `docker
stop` polecenia:

```console
docker stop randomanswers
```

Aby usunąć kontener, należy wydać `docker rm` polecenia:

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Przełącz się do kontenerów Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Publikowanie do systemu plików"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Ustawienia publikowania"
