---
title: ASP.NET Core hosta w kontenerach platformy Docker
author: rick-anderson
description: Odkryj linki do zasobów, aby dowiedzieć się, jak hostować aplikacje ASP.NET Core w kontenerach platformy Docker.
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
uid: host-and-deploy/docker/index
ms.openlocfilehash: cb5f774db5fab46a57f8ca4bbbca148f20f371ba
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664081"
---
# <a name="host-aspnet-core-in-docker-containers"></a>ASP.NET Core hosta w kontenerach platformy Docker

Następujące artykuły są dostępne na potrzeby uczenia się, jak hostować ASP.NET Core aplikacje w programie Docker:

[Wprowadzenie do kontenerów i platformy Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
Zobacz, jak kontenerach jest podejściem do tworzenia oprogramowania, w którym aplikacja lub usługa, jej zależności i jej konfiguracja są pakowane razem jako obraz kontenera. Obraz może zostać przetestowany, a następnie wdrożony na hoście.

[Co to jest platforma Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
Odkryj, jak platforma Docker to projekt typu "open source" służący do automatyzowania wdrażania aplikacji jako przenośnych, samowystarczalnych kontenerów, które mogą być uruchamiane w chmurze lub lokalnie.

[Terminologia platformy Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
Poznaj warunki i definicje dotyczące technologii platformy Docker.

[Kontenery, obrazy i rejestry platformy Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
Dowiedz się, w jaki sposób obrazy kontenerów platformy Docker są przechowywane w rejestrze obrazów na potrzeby spójnego wdrażania w różnych środowiskach.

<xref:host-and-deploy/docker/building-net-docker-images> dowiedzieć się, jak skompilować i przekształcać aplikację ASP.NET Core. Eksploruj obrazy platformy Docker obsługiwane przez firmę Microsoft i badaj przypadki użycia.

[Narzędzia kontenera programu Visual Studio](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
Dowiedz się, w jaki sposób program Visual Studio obsługuje kompilowanie, debugowanie i uruchamianie aplikacji ASP.NET Core przeznaczonych dla .NET Framework lub .NET Core w Docker for Windows. Obsługiwane są kontenery zarówno systemu Windows, jak i Linux.

[Publikuj w Azure Container Registry](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
Dowiedz się, jak za pomocą rozszerzenia Visual Studio Container Tools wdrożyć aplikację ASP.NET Core na hoście Docker na platformie Azure przy użyciu programu PowerShell.

[Konfigurowanie ASP.NET Core do pracy z serwerami proxy i usługami równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer)  
Dodatkowa konfiguracja może być wymagana dla aplikacji hostowanych w ramach serwerów proxy i modułów równoważenia obciążenia. Przekazywanie żądań za pomocą serwera proxy często zasłania informacje o oryginalnym żądaniu, takie jak schemat i adres IP klienta. Może być konieczne ręczne przekazanie pewnych informacji dotyczących żądania do aplikacji.
