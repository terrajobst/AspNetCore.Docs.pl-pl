---
title: Hostowanie i wdrażanie składników Razor i Blazor
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikacje składniki Razor i Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/index
ms.openlocfilehash: eeaa27bef584523b77d8b47000b310fe0cac7341
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59069778"
---
# <a name="host-and-deploy-razor-components-and-blazor"></a><span data-ttu-id="38197-103">Hostowanie i wdrażanie składników Razor i Blazor</span><span class="sxs-lookup"><span data-stu-id="38197-103">Host and deploy Razor Components and Blazor</span></span>

<span data-ttu-id="38197-104">Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="38197-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="38197-105">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="38197-105">Publish the app</span></span>

<span data-ttu-id="38197-106">Aplikacje są publikowane do wdrożenia w konfiguracji wydania przy użyciu [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenia.</span><span class="sxs-lookup"><span data-stu-id="38197-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="38197-107">Zintegrowanym środowisku programistycznym (IDE) może obsługiwać wykonywanie `dotnet publish` polecenia automatycznie przy użyciu jego wbudowanych funkcji publikowania, dzięki czemu może nie być konieczne ręczne wykonaj polecenie w wierszu polecenia, w zależności od rozwoju narzędzia w użyciu.</span><span class="sxs-lookup"><span data-stu-id="38197-107">An integrated development environment (IDE) may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

`dotnet publish` <span data-ttu-id="38197-108">Wyzwalacze [przywrócić](/dotnet/core/tools/dotnet-restore) zależności projektu i [kompilacje](/dotnet/core/tools/dotnet-build) projekcie przed utworzeniem zasobów dla wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="38197-108">triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="38197-109">Jako część procesu kompilacji Aby zmniejszyć rozmiar pobierania aplikacji i czasów ładowania są usuwane nieużywane metod i zestawów.</span><span class="sxs-lookup"><span data-stu-id="38197-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="38197-110">Wdrożenie jest tworzony w */bin/wersji / {struktury docelowej} / publish* folderu.</span><span class="sxs-lookup"><span data-stu-id="38197-110">The deployment is created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="38197-111">Zasoby w *publikowania* folderu wdrożonych na serwerze sieci web.</span><span class="sxs-lookup"><span data-stu-id="38197-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="38197-112">Wdrożenie może być ręcznego lub zautomatyzowanego procesu w zależności od poziomu narzędzi deweloperskich w użyciu.</span><span class="sxs-lookup"><span data-stu-id="38197-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="38197-113">wdrażania</span><span class="sxs-lookup"><span data-stu-id="38197-113">Deployment</span></span>

<span data-ttu-id="38197-114">Aby uzyskać wskazówki dotyczące wdrażania zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="38197-114">For deployment guidance, see the following topics:</span></span>

* [<span data-ttu-id="38197-115">Blazor po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="38197-115">Client-side Blazor</span></span>](xref:host-and-deploy/razor-components-blazor/blazor)
* [<span data-ttu-id="38197-116">Server-side ASP.NET Core Razor Components</span><span class="sxs-lookup"><span data-stu-id="38197-116">Server-side ASP.NET Core Razor Components</span></span>](xref:host-and-deploy/razor-components-blazor/razor-components)
