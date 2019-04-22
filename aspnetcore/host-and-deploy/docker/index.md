---
title: Host platformy ASP.NET Core w kontenerach platformy Docker
author: rick-anderson
description: Dowiedz się, linki do zasobów do nauki, jak hostować aplikacje platformy ASP.NET Core w kontenerach platformy Docker.
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
uid: host-and-deploy/docker/index
ms.openlocfilehash: e56f90ec7272ce0411651ee6f8e7c754ae44b78d
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59516264"
---
# <a name="host-aspnet-core-in-docker-containers"></a>Host platformy ASP.NET Core w kontenerach platformy Docker

Dla szkoleniowe dotyczące hostowania aplikacji platformy ASP.NET Core na platformie Docker dostępne są następujące artykuły:

[Wprowadzenie do kontenerów i platformy Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
Zobacz, jak konteneryzacji to podejście do tworzenia oprogramowania, w którym aplikacji lub usługi, jego zależności i jego konfigurację jednym pakiecie w postaci obrazu kontenera. Obraz, który można przetestować i następnie wdrożona na hoście.

[Co to jest Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
Dowiedz się, jak Docker to projekt typu open source umożliwiająca automatyzację wdrażania aplikacji jako przenośny, samowystarczalne kontenery, które można uruchomić w chmurze lub lokalnie.

[Terminologia platformy docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
Dowiedz się, terminy i definicje dla technologii platformy Docker.

[Kontenery, obrazy i rejestry platformy Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
Dowiedz się, jak obrazów kontenerów platformy Docker są przechowywane w rejestrze obraz spójne wdrażanie w środowiskach.

[Visual Studio Tools for Docker](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
Dowiedz się, jak program Visual Studio 2017 obsługuje kompilowania, debugowania i uruchamiania programu ASP.NET Core z aplikacji przeznaczonych dla środowiska .NET Framework lub .NET Core w Docker for Windows. Są obsługiwane kontenerów systemów Windows i Linux.

[Publikowanie w obrazie platformy Docker](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
Dowiedz się, jak wdrożyć aplikację ASP.NET Core na hosta platformy Docker na platformie Azure przy użyciu programu PowerShell za pomocą Visual Studio Tools dla rozszerzenia platformy Docker.

[Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer)  
Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy i moduły równoważenia obciążenia. Przekazywanie żądań za pośrednictwem serwera proxy, często ukrycie informacji na temat oryginalne żądanie, takie jak adres IP schemat i klienta. Konieczne może być przesyłane dalej niektóre informacje o żądaniu ręcznie do aplikacji.
