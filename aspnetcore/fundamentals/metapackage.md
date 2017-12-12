---
title: Metapackage Microsoft.AspNetCore.All dla platformy ASP.NET Core 2.x i nowsze
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage obejmuje wszystkie obsługiwane pakiety Entity Framework Core i ASP.NET Core, wraz z ich zależności."
keywords: ASP.NET Core,NuGet,package,Microsoft.AspNetCore.All,metapackage
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: ff25d80be907994f7ac3d64a8ffa39ae53278ba6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="d2f65-104">Metapackage Microsoft.AspNetCore.All dla platformy ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2f65-104">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="d2f65-105">Ta funkcja wymaga platformy ASP.NET Core 2.x docelowy .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="d2f65-105">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="d2f65-106">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage dla platformy ASP.NET Core obejmuje:</span><span class="sxs-lookup"><span data-stu-id="d2f65-106">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="d2f65-107">Wszystkie obsługiwane pakiety przez zespół platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2f65-107">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="d2f65-108">Wszystkie obsługiwane pakiety przez Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d2f65-108">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="d2f65-109">Zależności wewnętrzne i firm 3rd używane przez Entity Framework Core i ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2f65-109">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="d2f65-110">Wszystkie funkcje platformy ASP.NET Core 2.x i Entity Framework Core 2.x znajdują się w `Microsoft.AspNetCore.All` pakietu.</span><span class="sxs-lookup"><span data-stu-id="d2f65-110">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="d2f65-111">Tego pakietu używać domyślnych szablonów projektu.</span><span class="sxs-lookup"><span data-stu-id="d2f65-111">The default project templates use this package.</span></span>

<span data-ttu-id="d2f65-112">Numer wersji `Microsoft.AspNetCore.All` metapackage reprezentuje wersji platformy ASP.NET Core i wersji programu Entity Framework Core (zgodne z wersją .NET Core).</span><span class="sxs-lookup"><span data-stu-id="d2f65-112">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="d2f65-113">Aplikacje używające `Microsoft.AspNetCore.All` metapackage automatycznie korzystać z [.NET Core środowiska uruchomieniowego magazynu](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="d2f65-113">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="d2f65-114">Magazyn środowiska uruchomieniowego zawiera wszystkie zasoby środowiska uruchomieniowego potrzebne do uruchomienia aplikacji 2.x platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2f65-114">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="d2f65-115">Jeśli używasz `Microsoft.AspNetCore.All` metapackage, **nie** zasobów, z którym związane są odwołania pakietów platformy ASP.NET Core NuGet zostały wdrożone za pomocą aplikacji &mdash; magazynie środowiska uruchomieniowego .NET Core zawiera te zasoby.</span><span class="sxs-lookup"><span data-stu-id="d2f65-115">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="d2f65-116">Zasoby w magazynie środowiska uruchomieniowego są wstępnie skompilowana zwiększające czasu uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d2f65-116">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="d2f65-117">Proces przycinanie pakietu służy do usuwania pakietów, które nie są używane.</span><span class="sxs-lookup"><span data-stu-id="d2f65-117">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="d2f65-118">Przycięte pakiety są wyłączone w danych wyjściowych opublikowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d2f65-118">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="d2f65-119">Następujące *.csproj* pliku odwołań `Microsoft.AspNetCore.All` metapackage dla platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="d2f65-119">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
