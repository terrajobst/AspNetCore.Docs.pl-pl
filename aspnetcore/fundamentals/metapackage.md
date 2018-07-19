---
title: Pakiet meta Microsoft.aspnetcore.all dla programu ASP.NET Core 2.0
author: Rick-Anderson
description: Pakiet meta Microsoft.aspnetcore.all obejmuje wszystkie obsługiwane pakiety platformy ASP.NET Core i Entity Framework Core, wraz z ich zależnościami.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage
ms.openlocfilehash: fbc0f5465dc37a612b81c293f1a58b53ea4b2238
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123830"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="5140a-103">Pakiet meta Microsoft.aspnetcore.all dla programu ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="5140a-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="5140a-104">Firma Microsoft zaleca, aby aplikacje przeznaczone na platformy ASP.NET Core 2.1 i później użyć [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) zamiast tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="5140a-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="5140a-105">Zobacz [migrowany pakiet do Microsoft.AspNetCore.App](#migrate) w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="5140a-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="5140a-106">Ta funkcja wymaga platformy ASP.NET Core 2.x określania wartości docelowej .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="5140a-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="5140a-107">[Pakiet](https://www.nuget.org/packages/Microsoft.AspNetCore.All) meta Microsoft.aspnetcore.all dla platformy ASP.NET Core obejmuje:</span><span class="sxs-lookup"><span data-stu-id="5140a-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="5140a-108">Wszystkie obsługiwane pakiety przez zespół programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5140a-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="5140a-109">Wszystkie obsługiwane pakiety przez Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="5140a-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="5140a-110">Zależności wewnętrzne i firm 3 używane przez program ASP.NET Core i Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="5140a-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="5140a-111">Wszystkie funkcje platformy ASP.NET Core 2.x i Entity Framework Core 2.x znajdują się w `Microsoft.AspNetCore.All` pakietu.</span><span class="sxs-lookup"><span data-stu-id="5140a-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="5140a-112">Domyślne szablony projektów przeznaczonych dla platformy ASP.NET Core 2.0, użyj tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="5140a-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="5140a-113">Numer wersji `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all reprezentuje wersję platformy ASP.NET Core i Entity Framework Core wersji.</span><span class="sxs-lookup"><span data-stu-id="5140a-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="5140a-114">Aplikacje, które używają `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all automatycznie korzystać z zalet [Store środowiska uruchomieniowego programu .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="5140a-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="5140a-115">Store środowiska uruchomieniowego zawiera wszystkie zasoby środowiska uruchomieniowego potrzebnych do uruchomienia programu ASP.NET Core 2.x aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5140a-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="5140a-116">Kiedy używasz `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all, **nie** zasoby z pakietów platformy ASP.NET Core NuGet, do którego istnieje odwołanie, są wdrażane przy użyciu aplikacji &mdash; Store środowiska uruchomieniowego programu .NET Core zawiera te zasoby.</span><span class="sxs-lookup"><span data-stu-id="5140a-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="5140a-117">Zasoby w Store środowiska uruchomieniowego są wstępnie skompilowane, aby poprawić czas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5140a-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="5140a-118">Aby usunąć pakiety, które nie są używane, można użyć procesu przycinania pakietu.</span><span class="sxs-lookup"><span data-stu-id="5140a-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="5140a-119">Przycięty pakiety są wyłączone w danych wyjściowych w opublikowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5140a-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="5140a-120">Następujące *.csproj* pliku odwołania `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all dla platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="5140a-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="5140a-121">Migrowanie z pakiet do Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="5140a-121">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="5140a-122">Następujące pakiety są objęte `Microsoft.AspNetCore.All` , ale nie `Microsoft.AspNetCore.App` pakietu.</span><span class="sxs-lookup"><span data-stu-id="5140a-122">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span> 

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

<span data-ttu-id="5140a-123">Aby przenieść z `Microsoft.AspNetCore.All` do `Microsoft.AspNetCore.App`, jeśli aplikacja korzysta z żadnych interfejsów API z powyższych pakietów lub pakietów plików odwoływanych przez te pakiety, należy dodać odwołania do tych pakietów w projekcie.</span><span class="sxs-lookup"><span data-stu-id="5140a-123">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="5140a-124">Wszelkie zależności poprzedniego pakiety, które w przeciwnym razie nie ma zależności `Microsoft.AspNetCore.App` nie są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="5140a-124">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="5140a-125">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5140a-125">For example:</span></span>

* <span data-ttu-id="5140a-126">`StackExchange.Redis` jako zależą od elementu `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="5140a-126">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="5140a-127">`Microsoft.ApplicationInsights` jako zależą od elementu `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="5140a-127">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>
