---
title: Host platformy ASP.NET Core w kontenerach platformy Docker
author: rick-anderson
description: Dowiedz się, linki do zasobów do nauki, jak hostować aplikacje platformy ASP.NET Core w kontenerach platformy Docker.
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
uid: host-and-deploy/docker/index
ms.openlocfilehash: e56f90ec7272ce0411651ee6f8e7c754ae44b78d
ms.sourcegitcommit: 9b7fcb4ce00a3a32e153a080ebfaae4ef417aafa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/12/2019
ms.locfileid: "59516264"
---
# <a name="host-aspnet-core-in-docker-containers"></a><span data-ttu-id="36850-103">Host platformy ASP.NET Core w kontenerach platformy Docker</span><span class="sxs-lookup"><span data-stu-id="36850-103">Host ASP.NET Core in Docker containers</span></span>

<span data-ttu-id="36850-104">Dla szkoleniowe dotyczące hostowania aplikacji platformy ASP.NET Core na platformie Docker dostępne są następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="36850-104">The following articles are available for learning about hosting ASP.NET Core apps in Docker:</span></span>

[<span data-ttu-id="36850-105">Wprowadzenie do kontenerów i platformy Docker</span><span class="sxs-lookup"><span data-stu-id="36850-105">Introduction to Containers and Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
<span data-ttu-id="36850-106">Zobacz, jak konteneryzacji to podejście do tworzenia oprogramowania, w którym aplikacji lub usługi, jego zależności i jego konfigurację jednym pakiecie w postaci obrazu kontenera.</span><span class="sxs-lookup"><span data-stu-id="36850-106">See how containerization is an approach to software development in which an application or service, its dependencies, and its configuration are packaged together as a container image.</span></span> <span data-ttu-id="36850-107">Obraz, który można przetestować i następnie wdrożona na hoście.</span><span class="sxs-lookup"><span data-stu-id="36850-107">The image can be tested and then deployed to a host.</span></span>

[<span data-ttu-id="36850-108">Co to jest Docker</span><span class="sxs-lookup"><span data-stu-id="36850-108">What is Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
<span data-ttu-id="36850-109">Dowiedz się, jak Docker to projekt typu open source umożliwiająca automatyzację wdrażania aplikacji jako przenośny, samowystarczalne kontenery, które można uruchomić w chmurze lub lokalnie.</span><span class="sxs-lookup"><span data-stu-id="36850-109">Discover how Docker is an open-source project for automating the deployment of apps as portable, self-sufficient containers that can run on the cloud or on-premises.</span></span>

[<span data-ttu-id="36850-110">Terminologia platformy docker</span><span class="sxs-lookup"><span data-stu-id="36850-110">Docker Terminology</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
<span data-ttu-id="36850-111">Dowiedz się, terminy i definicje dla technologii platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="36850-111">Learn terms and definitions for Docker technology.</span></span>

[<span data-ttu-id="36850-112">Kontenery, obrazy i rejestry platformy Docker</span><span class="sxs-lookup"><span data-stu-id="36850-112">Docker containers, images, and registries</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
<span data-ttu-id="36850-113">Dowiedz się, jak obrazów kontenerów platformy Docker są przechowywane w rejestrze obraz spójne wdrażanie w środowiskach.</span><span class="sxs-lookup"><span data-stu-id="36850-113">Find out how Docker container images are stored in an image registry for consistent deployment across environments.</span></span>

[<span data-ttu-id="36850-114">Visual Studio Tools for Docker</span><span class="sxs-lookup"><span data-stu-id="36850-114">Visual Studio Tools for Docker</span></span>](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
<span data-ttu-id="36850-115">Dowiedz się, jak program Visual Studio 2017 obsługuje kompilowania, debugowania i uruchamiania programu ASP.NET Core z aplikacji przeznaczonych dla środowiska .NET Framework lub .NET Core w Docker for Windows.</span><span class="sxs-lookup"><span data-stu-id="36850-115">Discover how Visual Studio 2017 supports building, debugging, and running ASP.NET Core apps targeting either .NET Framework or .NET Core on Docker for Windows.</span></span> <span data-ttu-id="36850-116">Są obsługiwane kontenerów systemów Windows i Linux.</span><span class="sxs-lookup"><span data-stu-id="36850-116">Both Windows and Linux containers are supported.</span></span>

[<span data-ttu-id="36850-117">Publikowanie w obrazie platformy Docker</span><span class="sxs-lookup"><span data-stu-id="36850-117">Publish to a Docker Image</span></span>](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
<span data-ttu-id="36850-118">Dowiedz się, jak wdrożyć aplikację ASP.NET Core na hosta platformy Docker na platformie Azure przy użyciu programu PowerShell za pomocą Visual Studio Tools dla rozszerzenia platformy Docker.</span><span class="sxs-lookup"><span data-stu-id="36850-118">Find out how to use the Visual Studio Tools for Docker extension to deploy an ASP.NET Core app to a Docker host on Azure using PowerShell.</span></span>

[<span data-ttu-id="36850-119">Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="36850-119">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)  
<span data-ttu-id="36850-120">Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy i moduły równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="36850-120">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="36850-121">Przekazywanie żądań za pośrednictwem serwera proxy, często ukrycie informacji na temat oryginalne żądanie, takie jak adres IP schemat i klienta.</span><span class="sxs-lookup"><span data-stu-id="36850-121">Passing requests through a proxy often obscures information about the original request, such as the scheme and client IP.</span></span> <span data-ttu-id="36850-122">Konieczne może być przesyłane dalej niektóre informacje o żądaniu ręcznie do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="36850-122">It might be necessary to forwarded some information about the request manually to the app.</span></span>
