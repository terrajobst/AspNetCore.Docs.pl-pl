---
uid: mvc/overview/deployment/docker
title: Migrowanie aplikacji ASP.NET MVC do kontenerów systemu Windows
description: Dowiedz się, jak wykonać istniejącej aplikacji ASP.NET MVC i uruchom go w kontenerze Docker systemu Windows
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.topic: article
ms.prod: .net-framework
ms.technology: dotnet-mvc
ms.devlang: dotnet
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 7a580c6c6236b375ea54ef4e9978fff6993d885a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/11/2018
ms.locfileid: "29143192"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a>Migrowanie aplikacji ASP.NET MVC do kontenerów systemu Windows

Uruchamianie istniejących aplikacji opartych na programie .NET Framework w kontenerze systemu Windows nie wymaga żadnych zmian do aplikacji. Aby uruchomić aplikację w kontenerze systemu Windows utworzyć obraz Docker zawierający aplikację i uruchomić kontenera. W tym temacie wyjaśniono, jak wykonać istniejące [aplikacji ASP.NET MVC](http://www.asp.net/mvc) i wdrożyć ją w kontenerze systemu Windows.

Można rozpoczynać się od istniejącej aplikacji ASP.NET MVC, a następnie utworzyć zasoby opublikowanych przy użyciu programu Visual Studio. Docker służy do tworzenia obrazu, który zawiera i uruchamia aplikację. Będzie przejdź do witryny uruchomione w kontenerze systemu Windows i sprawdź, czy aplikacja działa.

W tym artykule przyjęto założenie, podstawową wiedzę na temat programu Docker. Informacje na temat rozwiązania Docker, odczytując [omówienie Docker](https://docs.docker.com/engine/understanding-docker/).

Aplikacja zostanie uruchomione w kontenerze jest prosty witryny sieci Web, który losowo odpowiedzi na pytania. Ta aplikacja jest podstawowej aplikacji MVC bez uwierzytelniania lub magazynu bazy danych; Dzięki temu można skupić się na przenoszenie warstwa sieci web do kontenera. Tematy przyszłych pokazują, jak przenieść i zarządzanie nimi magazynu trwałego w aplikacjach konteneryzowanych.

Przenoszenie aplikacji obejmuje następujące kroki:

1. [Tworzenie zadania publikowania do tworzenia zasobów dla obrazu.](#publish-script)
1. [Budowanie Docker obrazu, który uruchomi aplikację.](#build-the-image)
1. [Uruchamianie kontener Docker, który uruchamia obrazu.](#start-a-container)
1. [Weryfikowanie aplikacji za pomocą przeglądarki.](#verify-in-the-browser)

[Zakończono aplikacji](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) znajduje się w witrynie GitHub.

## <a name="prerequisites"></a>Wymagania wstępne

Musi być uruchomiona na komputerze deweloperskim

- [Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (lub nowszej) lub [systemu Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (lub nowszej).
- [Docker dla systemu Windows](https://docs.docker.com/docker-for-windows/) -26 w wersji Beta w wersji stabilnej 1.13.0 lub 1.12 (lub nowsze wersje)
- [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx).

> [!IMPORTANT]
> Jeśli używasz systemu Windows Server 2016, postępuj zgodnie z instrukcjami dotyczącymi [wdrażania hosta kontenera - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).

Po instalacji i uruchamiania Docker, kliknij prawym przyciskiem myszy ikonę na pasku zadań i wybierz **przełączyć się do systemu Windows kontenery**. Jest to wymagane do uruchomienia obrazy usługi Docker opartych na systemie Windows. To polecenie zajmuje kilka sekund do wykonania:

![Windows Container][windows-container]

## <a name="publish-script"></a>Publikowanie skryptu

Zbieraj wszystkie zasoby, które należy załadować do obrazu platformy Docker w jednym miejscu. Można użyć programu Visual Studio **publikowania** polecenie, aby utworzyć profil publikowania dla aplikacji. Ten profil zostanie Umieść wszystkie zasoby w jednym drzewie katalogu skopiowane na obrazie docelowego w dalszej części tego samouczka.

**Kroki publikowania**

1. Kliknij prawym przyciskiem myszy projekt sieci web w programie Visual Studio i wybierz **publikowania**.
1. Kliknij przycisk **przycisk profil niestandardowy**, a następnie wybierz **systemu plików** jako metodę.
1. Wybierz katalog. Konwencja, używa pobrane próbki `bin\Release\PublishOutput`.

![Publikowanie połączenia][publish-connection]

Otwórz **opcji publikowania pliku** sekcji **ustawienia** kartę. Wybierz **Precompile podczas publikowania**. Tego rodzaju optymalizacji oznacza, że będzie kompilowanie widoków w kontenerze Docker, kopiowane są wstępnie skompilowanym widoków.

![Ustawienia publikowania][publish-settings]

Kliknij przycisk **publikowania**, i Visual Studio kopiuje wymagane zasoby do folderu docelowego.

## <a name="build-the-image"></a>Tworzenie obrazu

Plik Dockerfile do definiowania obrazu Docker. Plik Dockerfile zawiera instrukcje dotyczące obrazu podstawowego, dodatkowe składniki aplikacji, które mają zostać uruchomione i inne obrazy konfiguracji.  W danych wejściowych jest plik Dockerfile `docker build` polecenia, które tworzy obraz.

Zostanie utworzona na podstawie obrazu `microsoft/aspnet` obrazu, który znajduje się na [Centrum Docker](https://hub.docker.com/r/microsoft/aspnet/).
Podstawowy obraz `microsoft/aspnet`, jest obrazem systemu Windows Server. Zawiera on Windows Server Core, IIS i platformy ASP.NET 4.6.2. Po uruchomieniu tego obrazu w kontenerze użytkownika zostaną automatycznie uruchomione usługi IIS i zainstalowane witryny sieci Web.

Plik Dockerfile, która tworzy obraz wygląda następująco:

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

Brak nie `ENTRYPOINT` w tym plik Dockerfile polecenie. Nie można je utworzyć. Podczas uruchamiania systemu Windows Server z usługami IIS, proces programu IIS jest punktu wejścia, który jest skonfigurowany do uruchamiania w aspnet obrazu podstawowego.

Uruchom polecenie kompilacji Docker do utworzenia obrazu, który uruchamia aplikację ASP.NET. Aby to zrobić, Otwórz okno programu PowerShell w katalogu projektu i wpisz następujące polecenie w katalogu rozwiązania:

```console
docker build -t mvcrandomanswers .
```

To polecenie utworzy nowy obraz, korzystając z instrukcji w Twojej plik Dockerfile, nazewnictwa (-t znakowanie) jako mvcrandomanswers obrazu. Może to obejmować ściąganie podstawowy obraz z [Centrum Docker](http://hub.docker.com), a następnie dodanie aplikacji do tego obrazu.

Po zakończeniu wykonywania tego polecenia, można uruchomić `docker images` polecenie, aby wyświetlić informacje na nowy obraz:

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

Identyfikator obrazu będzie różna na tym komputerze. Teraz umożliwia uruchamianie aplikacji.

## <a name="start-a-container"></a>Uruchom kontenera

Uruchom kontener, wykonując następujące `docker run` polecenia:

```console
docker run -d --name randomanswers mvcrandomanswers
```

`-d` Argument nakazuje Docker można uruchomić obrazu w trybie odłączony. Oznacza to, że obraz Docker działa bez połączenia z bieżącym powłoki.

W wielu przykłady docker może wystąpić -p mapowania portów kontenera i hosta. Domyślny obraz aspnet skonfigurował już kontenera do nasłuchiwania na porcie 80 i uwidacznia go. 

`--name randomanswers` Nadaje nazwę do uruchomionej kontenera. Zamiast Identyfikatora kontenera w większości poleceń można używać tej nazwy.

`mvcrandomanswers` Jest nazwą obraz do uruchomienia.

## <a name="verify-in-the-browser"></a>Sprawdź, czy w przeglądarce

> [!NOTE]
> W bieżącej wersji kontenera systemu Windows nie może przejść do `http://localhost`.
> Jest to znane zachowanie w funkcję WinNAT i zostanie on rozwiązany w przyszłości. Do czasu, który zostanie rozwiązana, należy użyć adresu IP kontenera.

Po uruchomieniu kontenera, Znajdź swój adres IP, dzięki czemu możesz nawiązać połączenie z kontenera uruchomiona w przeglądarce:

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

Połączyć się z kontenerem uruchomiona, przy użyciu adresu IPv4 `http://172.31.194.61` w przykładzie. Typ tego adresu URL do przeglądarki, a powinien zostać wyświetlony uruchomionej witryny.

> [!NOTE]
> Niektóre programy sieci VPN lub serwer proxy może uniemożliwić przejście do witryny.
> Można tymczasowo wyłączyć, aby upewnić się, że działa z kontenera.

Zawiera katalog przykładu w serwisie GitHub [skrypt programu PowerShell](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) , który jest wykonywany tych poleceń dla Ciebie. Otwórz okno programu PowerShell, zmień katalog na katalog rozwiązania i wpisz:

```console
./run.ps1
```

Powyższe polecenie tworzy obraz, umożliwia wyświetlenie listy obrazów na komputerze, rozpoczyna się kontener i wyświetla adres IP dla tego kontenera.

Aby zatrzymać z kontenera, należy wydać `docker
stop` polecenia:

```console
docker stop randomanswers
```

Aby usunąć kontenera, należy wydać `docker rm` polecenia:

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Przełącz się do kontenera systemu Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Publikowanie do systemu plików"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Ustawienia publikowania"
