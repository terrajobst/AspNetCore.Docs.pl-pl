---
title: Metapackage Microsoft.AspNetCore.All dla platformy ASP.NET Core 2.0 lub nowszy
author: Rick-Anderson
description: Microsoft.AspNetCore.All metapackage obejmuje wszystkie obsługiwane pakiety Entity Framework Core i ASP.NET Core, wraz z ich zależności.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage
ms.openlocfilehash: 2fddc59d74dce4b114b5b4ed0646f773eb66ffb9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277825"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="8ae2a-103">Metapackage Microsoft.AspNetCore.All dla platformy ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="8ae2a-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="8ae2a-104">Firma Microsoft zaleca aplikacji przeznaczonych dla platformy ASP.NET Core 2.1 i później użyć [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) zamiast tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="8ae2a-105">Zobacz [migrowania Microsoft.AspNetCore.All do Microsoft.AspNetCore.App](#migrate) w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="8ae2a-106">Ta funkcja wymaga platformy ASP.NET Core 2.x docelowy .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="8ae2a-107">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage dla platformy ASP.NET Core obejmuje:</span><span class="sxs-lookup"><span data-stu-id="8ae2a-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="8ae2a-108">Wszystkie obsługiwane pakiety przez zespół platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="8ae2a-109">Wszystkie obsługiwane pakiety przez Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-109">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="8ae2a-110">Zależności wewnętrzne i firm 3rd używane przez Entity Framework Core i ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="8ae2a-111">Wszystkie funkcje platformy ASP.NET Core 2.x i Entity Framework Core 2.x znajdują się w `Microsoft.AspNetCore.All` pakietu.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="8ae2a-112">Domyślne szablony projektów przeznaczonych dla platformy ASP.NET Core 2.0 Użyj tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="8ae2a-113">Numer wersji `Microsoft.AspNetCore.All` metapackage reprezentuje wersji platformy ASP.NET Core i wersji programu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="8ae2a-114">Aplikacje używające `Microsoft.AspNetCore.All` metapackage automatycznie korzystać z [.NET Core środowiska uruchomieniowego magazynu](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="8ae2a-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="8ae2a-115">Magazyn środowiska uruchomieniowego zawiera wszystkie zasoby środowiska uruchomieniowego potrzebne do uruchomienia aplikacji 2.x platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="8ae2a-116">Jeśli używasz `Microsoft.AspNetCore.All` metapackage, **nie** zasobów, z którym związane są odwołania pakietów platformy ASP.NET Core NuGet zostały wdrożone za pomocą aplikacji &mdash; magazynie środowiska uruchomieniowego .NET Core zawiera te zasoby.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="8ae2a-117">Zasoby w magazynie środowiska uruchomieniowego są wstępnie skompilowana zwiększające czasu uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="8ae2a-118">Proces przycinanie pakietu służy do usuwania pakietów, które nie są używane.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="8ae2a-119">Przycięte pakiety są wyłączone w danych wyjściowych opublikowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="8ae2a-120">Następujące *.csproj* pliku odwołań `Microsoft.AspNetCore.All` metapackage dla platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="8ae2a-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="8ae2a-121">Migrowanie z Microsoft.AspNetCore.All do Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="8ae2a-121">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="8ae2a-122">Następujące pakiety są uwzględnione w `Microsoft.AspNetCore.All` , ale nie `Microsoft.AspNetCore.App` pakietu.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-122">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span> 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

<span data-ttu-id="8ae2a-123">Aby przenieść z `Microsoft.AspNetCore.All` do `Microsoft.AspNetCore.App`, jeśli aplikacja korzysta z żadnych interfejsów API z powyższych pakietów lub pakietów, sprowadzonych przez te pakiety, dodaj odwołania do tych pakietów do projektu.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-123">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="8ae2a-124">Wszelkie zależności powyższych pakietów, które w przeciwnym razie nie ma zależności `Microsoft.AspNetCore.App` nie są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="8ae2a-124">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="8ae2a-125">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8ae2a-125">For example:</span></span>

* <span data-ttu-id="8ae2a-126">`StackExchange.Redis` jako zależność z `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="8ae2a-126">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="8ae2a-127">`Microsoft.ApplicationInsights` jako zależność z `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="8ae2a-127">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>
