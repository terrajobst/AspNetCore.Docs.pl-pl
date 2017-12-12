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
# <a name="visual-studio-tools-for-docker"></a>Visual Studio Tools for Docker

[Microsoft Visual Studio 2017](https://www.visualstudio.com/) z [Docker dla systemu Windows](https://docs.docker.com/docker-for-windows/install/) obsługuje kompilowania, debugowania i uruchamiania .NET Framework i .NET Core aplikacji sieci web i konsoli przy użyciu kontenerów systemu Windows i Linux.

## <a name="prerequisites"></a>Wymagania wstępne

- [Microsoft Visual Studio 2017](https://www.visualstudio.com/) z obciążenia .NET Core
- [Docker dla systemu Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Instalacja i Konfiguracja

Zainstaluj [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) z obciążeniem, .NET Core. Jeśli masz już zainstalowanego programu Visual Studio, możesz [Zmodyfikuj instalację programu Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) można dodać obciążenia .NET Core.

Docker instalacji, przejrzyj informacje w [Docker dla systemu Windows: co należy wiedzieć przed zainstalowaniem](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) i zainstaluj [Docker dla systemu Windows](https://docs.docker.com/docker-for-windows/install/).

Instalator jest wymagana konfiguracja  **[udostępnione dyski](https://docs.docker.com/docker-for-windows/#shared-drives)**  Docker dla systemu Windows. Ustawienie jest wymagane dla woluminu mapowania i debugowanie pomocy technicznej.

Kliknij prawym przyciskiem myszy ikonę Docker na pasku zadań, kliknij przycisk **ustawienia**i wybierz **udostępnione dyski**. Wybierz dysk, gdzie Docker będzie przechowywać pliki i zastosować zmiany.

![Dyski udostępnione](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

## <a name="create-an-aspnet-web-application-and-add-docker-support"></a>Tworzenie aplikacji sieci Web ASP.NET i dodanie obsługi Docker

Za pomocą programu Visual Studio, Utwórz nową aplikację sieci Web platformy ASP.NET Core. Podczas ładowania aplikacji, albo wybrać **dodać obsługę Docker** z **Menu Projekt** lub kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz polecenie **Dodaj**  >  **Obsługa docker**.

*Menu Projekt*

![Projekt dodać obsługę Docker](./visual-studio-tools-for-docker/_static/project-add-docker-support.png)

*Menu kontekstowe projektu*

![Kliknij prawym przyciskiem myszy dodać obsługę Docker](./visual-studio-tools-for-docker/_static/right-click-add-docker-support.png)

Po dodaniu obsługi Docker do projektu można kontenery systemu Windows lub Linux. (Hostów Docker musi być uruchomiona ten sam typ kontenera. Jeśli należy zmienić typ kontenera w uruchomione wystąpienie Docker, kliknij prawym przyciskiem myszy **Docker** ikonę na pasku zadań i wybierz polecenie **przełączyć się do systemu Windows kontenery** lub **przełączyć się do systemu Linux kontenery**.) 

Następujące pliki zostały dodane do projektu:

- **Plik Dockerfile**: plik Docker dla aplikacji platformy ASP.NET Core opiera się na [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) obrazu. Ten obraz zawiera pakietów platformy ASP.NET Core NuGet, które zostały wstępnie przy użyciu kompilatora JIT poprawę wydajności uruchamiania. Podczas tworzenia aplikacji konsoli .NET Core, plik Dockerfile z będzie odwoływać się do najnowszej [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) obrazu.   
- **docker-compose.yml**: podstawowy plik rozwiązania Docker Compose używane do definiowania kolekcji obrazów być skompilowany i uruchom z docker-tworzą kompilacji/Uruchom.   
- **docker compose.dev.debug.yml**: dodatkowe rozwiązania docker compose plik o zmiany iteracyjne ustawianą konfiguracji debugowania. Visual Studio będzie wywoływać docker-compose.yml - f -f docker-compose.dev.debug.yml scalenie tych razem. Ten plik Redaguj jest używany przez narzędzia deweloperskie programu Visual Studio.   
- **docker compose.dev.release.yml**: dodatkowego pliku rozwiązania Docker Compose do debugowania z definicji wersji. Go będzie instalacji woluminu debugera, nie powoduje zmiany zawartości obrazu produkcji.  

*Docker-compose.yml* plik zawiera nazwę obrazu, który jest tworzony w momencie uruchomienia projektu. 

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

W tym przykładzie `image: user/hellodockertools${TAG}` generuje obraz `user/hellodockertools:dev` po uruchomieniu aplikacji **debugowania** tryb i `user/hellodockertools:latest` w **wersji** tryb odpowiednio. 

Można zmienić `user` do Twojej [Centrum Docker](https://hub.docker.com/) nazwy użytkownika, jeśli planujesz push obrazu w rejestrze. Na przykład `spboyer/hellodockertools`, lub zmień adres URL prywatnych rejestru `privateregistry.domain.com/` w zależności od konfiguracji.

### <a name="debugging"></a>Debugowanie

Wybierz **Docker** z listy rozwijanej w pasku narzędzi i Użyj F5, aby rozpocząć debugowanie aplikacji debugowania. 

- *Microsoft/aspnetcore* obrazu są uzyskiwane (Jeśli nie jest jeszcze w pamięci podręcznej)
- *ASPNETCORE_ENVIRONMENT* ma ustawioną wartość Programowanie w kontenerze
- PORT 80 jest UWIDACZNIANY i mapować do portu przypisywany dynamicznie hosta lokalnego. Port jest określana przez hosta docker i mogą być przeszukiwane przy docker ps. 
- Aplikacja jest kopiowany do kontenera
- Domyślna przeglądarka jest uruchamiana w debugerze w kontenerze, za pomocą portu przypisywany dynamicznie. 

Wynikowy obraz Docker wbudowane jest *deweloperów* obrazu aplikacji za pomocą *microsoft/aspnetcore* obrazów jako obrazu podstawowego.

**Uwaga:** obraz deweloperów jest pusty zawartości Twojej aplikacji zgodnie z konfiguracji debugowania Użyj zamontowania woluminu, aby zapewnić środowisko iteracji. Aby wypchnąć obrazu, Użyj konfiguracji wydanie.

```console
REPOSITORY                  TAG         IMAGE ID            CREATED         SIZE
spboyer/hellodockertools    dev         0b6e2e44b3df        4 minutes ago   268.9 MB
microsoft/aspnetcore        1.0.1       189ad4312ce7        5 days ago      268.9 MB
```

Aplikacja została uruchomiona z użyciem kontenera, w którym można zobaczyć, uruchamiając `docker ps` polecenia.

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   4 minutes ago       Up 4 minutes        0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="edit-and-continue"></a>Edytuj i kontynuuj

Zmienia się na pliki statyczne i/lub pliki szablonów razor (*.cshtml*) są automatycznie aktualizowane bez konieczności kroku kompilacji. Wprowadzić zmiany, Zapisz, a następnie naciśnij przycisk Odśwież w przeglądarce, aby wyświetlić tę aktualizację.  

Modyfikacje plików kodu wymagają kompilacja i ponowne uruchomienie Kestrel w kontenerze. Po wprowadzeniu zmian, użyj CTRL + F5, aby wykonać proces i uruchomić aplikację w kontenerze. Kontener Docker nie jest ponownie skompilowany lub zatrzymana; przy użyciu `docker ps` w wierszu polecenia widoczny oryginalnego kontenera nadal działa na 10 minut temu. 

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   10 minutes ago      Up 10 minutes       0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="publishing-docker-images"></a>Obrazy usługi Docker publikowania

Po zakończeniu cyklu opracowanie i debugowania aplikacji Visual Studio Tools for Docker pomoże utworzyć obraz produkcyjnej aplikacji. Zmiana listy rozwijanej debugowania, aby **wersji** i kompilowania aplikacji. Narzędzia utworzy obraz z `:latest` tag, którego możesz wypchnąć do prywatnego rejestru lub Centrum Docker. 

Przy użyciu `docker images` polecenia, można zapoznać się z listą obrazów.

```console
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
spboyer/hellodockertools   latest              8184ae38ba91        5 seconds ago       278.4 MB
spboyer/hellodockertools   dev                 0b6e2e44b3df        About an hour ago   268.9 MB
microsoft/aspnetcore       1.0.1               189ad4312ce7        5 days ago          268.9 MB
```

Może być oczekiwanie środowisko produkcyjne lub wersji obrazu na mniejszy rozmiar w porównaniu do **deweloperów** obraz; jednak przy użyciu mapowania woluminu debugera i aplikacji zostały faktycznie uruchamiana z sieci lokalnej komputera, a nie w kontenerze. **Najnowsze** obraz ma spakowane kodu całej aplikacji potrzebne do uruchomienia aplikacji na maszynie hosta, w związku z tym różnicowej jest rozmiar kodu aplikacji.
