---
title: Host platformy ASP.NET Core w kontenerach Docker
author: rick-anderson
description: Wykryj linki do zasobów dla poznanie Hostuj aplikacje platformy ASP.NET Core w kontenerach Docker.
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
uid: host-and-deploy/docker/index
ms.openlocfilehash: 272bd0a0dad2fb62c33dcedd1ce8430eefb2c238
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276091"
---
# <a name="host-aspnet-core-in-docker-containers"></a>Host platformy ASP.NET Core w kontenerach Docker

Zapoznawanie hosting aplikacji platformy ASP.NET Core w Docker dostępne są następujące artykuły:

[Wprowadzenie do kontenerów i platformy Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
Dowiedz się jak przechowywanie w kontenerach podejście do rozwoju oprogramowania, w którym aplikacja lub usługa, zależnościami i jego konfiguracja są spakowane razem jako obraz kontenera. Obrazu można przetestować, a następnie wdrożona na hoście.

[Co to jest Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
Dowiedzieć się, jak Docker projekt open source do automatyzacji wdrażania aplikacji jako przenośny, samowystarczalne kontenerów, które można uruchomić w chmurze lub lokalnie.

[Terminologia docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
Dowiedz się terminy i definicje technologii Docker.

[Kontenery, obrazy i rejestry platformy Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
Dowiedz się, jak obrazy kontenera Docker są przechowywane w rejestrze obrazu wdrożenia spójne w środowiskach.

[Tworzenie aplikacji programu .NET Core obrazy usługi Docker](/dotnet/articles/core/docker/building-net-docker-images)  
Informacje o sposobie tworzenia i dockerize aplikacji platformy ASP.NET Core. Eksploruj obrazy usługi Docker obsługiwanego przez firmę Microsoft i przejrzyj przypadki użycia.

[Visual Studio Tools for Docker](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
Odkryj, jak Visual Studio 2017 obsługuje kompilowania, debugowania i uruchamianie platformy ASP.NET Core aplikacji przeznaczonych dla platformy .NET Framework lub .NET Core na Docker dla systemu Windows. Kontenery w systemach Windows i Linux są obsługiwane.

[Publikowanie w obrazie platformy Docker](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
Dowiedz się, jak używać programu Visual Studio Tools dla rozszerzenia Docker do wdrażania aplikacji platformy ASP.NET Core z hostem platformy Docker na platformie Azure przy użyciu programu PowerShell.

[Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer)  
Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych serwerów proxy i moduły równoważenia obciążenia. Przekazywanie żądań za pośrednictwem serwera proxy często ukrywa informacje o żądaniu oryginalnej, takie jak adres IP schemat i klienta. Konieczne może być przesyłane dalej niektóre informacje o żądaniu ręcznie do aplikacji.
