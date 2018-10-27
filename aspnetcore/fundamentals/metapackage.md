---
title: Pakiet meta Microsoft.aspnetcore.all dla programu ASP.NET Core 2.0
author: Rick-Anderson
description: Pakiet meta Microsoft.aspnetcore.all nie jest zalecane dla platformy ASP.NET Core 2.1 lub nowszych.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: d95bafd412969bb8db38499bd2ff01af510d872c
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148853"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="51e4a-103">Pakiet meta Microsoft.aspnetcore.all dla programu ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="51e4a-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="51e4a-104">Firma Microsoft zaleca, aby aplikacje przeznaczone na platformy ASP.NET Core 2.1 i później użyć [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) zamiast tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="51e4a-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="51e4a-105">Zobacz [migrowany pakiet do Microsoft.AspNetCore.App](#migrate) w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="51e4a-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="51e4a-106">Ta funkcja wymaga platformy ASP.NET Core 2.x określania wartości docelowej .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="51e4a-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="51e4a-107">[Pakiet](https://www.nuget.org/packages/Microsoft.AspNetCore.All) meta Microsoft.aspnetcore.all dla platformy ASP.NET Core obejmuje:</span><span class="sxs-lookup"><span data-stu-id="51e4a-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="51e4a-108">Wszystkie obsługiwane pakiety przez zespół programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="51e4a-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="51e4a-109">Wszystkie obsługiwane pakiety przez Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="51e4a-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="51e4a-110">Zależności wewnętrzne i firm 3 używane przez program ASP.NET Core i Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="51e4a-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="51e4a-111">Wszystkie funkcje platformy ASP.NET Core 2.x i Entity Framework Core 2.x znajdują się w `Microsoft.AspNetCore.All` pakietu.</span><span class="sxs-lookup"><span data-stu-id="51e4a-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="51e4a-112">Domyślne szablony projektów przeznaczonych dla platformy ASP.NET Core 2.0, użyj tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="51e4a-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="51e4a-113">Numer wersji `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all reprezentuje wersję platformy ASP.NET Core i Entity Framework Core wersji.</span><span class="sxs-lookup"><span data-stu-id="51e4a-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="51e4a-114">Aplikacje, które używają `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all automatycznie korzystać z zalet [Store środowiska uruchomieniowego programu .NET Core](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="51e4a-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="51e4a-115">Store środowiska uruchomieniowego zawiera wszystkie zasoby środowiska uruchomieniowego potrzebnych do uruchomienia programu ASP.NET Core 2.x aplikacji.</span><span class="sxs-lookup"><span data-stu-id="51e4a-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="51e4a-116">Kiedy używasz `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all, **nie** zasoby z pakietów platformy ASP.NET Core NuGet, do którego istnieje odwołanie, są wdrażane przy użyciu aplikacji &mdash; Store środowiska uruchomieniowego programu .NET Core zawiera te zasoby.</span><span class="sxs-lookup"><span data-stu-id="51e4a-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="51e4a-117">Zasoby w Store środowiska uruchomieniowego są wstępnie skompilowane, aby poprawić czas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="51e4a-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="51e4a-118">Aby usunąć pakiety, które nie są używane, można użyć procesu przycinania pakietu.</span><span class="sxs-lookup"><span data-stu-id="51e4a-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="51e4a-119">Przycięty pakiety są wyłączone w danych wyjściowych w opublikowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="51e4a-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="51e4a-120">Następujące *.csproj* pliku odwołania `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all dla platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="51e4a-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a><span data-ttu-id="51e4a-121">Niejawne przechowywania wersji</span><span class="sxs-lookup"><span data-stu-id="51e4a-121">Implicit versioning</span></span>

<span data-ttu-id="51e4a-122">W programie ASP.NET Core 2.1 lub nowszej, możesz określić `Microsoft.AspNetCore.All` odwołania, które bez wersji pakietu.</span><span class="sxs-lookup"><span data-stu-id="51e4a-122">In ASP.NET Core 2.1 or later, you can specify the `Microsoft.AspNetCore.All` package reference without a version.</span></span> <span data-ttu-id="51e4a-123">Jeśli wersja nie jest określona, niejawne wersja jest określona przez zestaw SDK (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="51e4a-123">When the version isn't specified, an implicit version is specified by the SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="51e4a-124">Firma Microsoft zaleca, opierając się na niejawne wersji określony przez zestaw SDK i nie zostały jawnie ustawienie numeru wersji na odwołanie do pakietu.</span><span class="sxs-lookup"><span data-stu-id="51e4a-124">We recommend relying on the implicit version specified by the SDK and not explicitly setting the version number on the package reference.</span></span> <span data-ttu-id="51e4a-125">Jeśli masz pytania na temat tego podejścia, pozostaw komentarz GitHub na [dyskusję odnośnie do wersji niejawne Microsoft.AspNetCore.App](https://github.com/aspnet/Docs/issues/6430).</span><span class="sxs-lookup"><span data-stu-id="51e4a-125">If you have questions about this approach, leave a GitHub comment at the [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430).</span></span>

<span data-ttu-id="51e4a-126">Jest ustawiona wersja niejawne `major.minor.0` przenośne aplikacji.</span><span class="sxs-lookup"><span data-stu-id="51e4a-126">The implicit version is set to `major.minor.0` for portable apps.</span></span> <span data-ttu-id="51e4a-127">Mechanizm przodu udostępnionej platformy uruchamia aplikację na najnowszej zgodnej wersji spośród zainstalowanych platform udostępnionych.</span><span class="sxs-lookup"><span data-stu-id="51e4a-127">The shared framework roll-forward mechanism runs the app on the latest compatible version among the installed shared frameworks.</span></span> <span data-ttu-id="51e4a-128">Aby zagwarantować, że ta sama wersja jest używana podczas tworzenia, testowania i produkcji, upewnij się, że tę samą wersję udostępnionego framework jest zainstalowana we wszystkich środowiskach.</span><span class="sxs-lookup"><span data-stu-id="51e4a-128">To guarantee the same version is used in development, test, and production, ensure the same version of the shared framework is installed in all environments.</span></span> <span data-ttu-id="51e4a-129">Dla aplikacji niezależna numer wersji niejawne został ustawiony na `major.minor.patch` udostępnionego Framework powiązane zainstalowanego zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="51e4a-129">For self-contained apps, the implicit version number is set to the `major.minor.patch` of the shared framework bundled in the installed SDK.</span></span>

<span data-ttu-id="51e4a-130">Określenie numeru wersji na `Microsoft.AspNetCore.All` jest odwołanie do pakietu **nie** gwarantuje daną wersję udostępnionego framework jest wybierany.</span><span class="sxs-lookup"><span data-stu-id="51e4a-130">Specifying a version number on the `Microsoft.AspNetCore.All` package reference does **not** guarantee that version of the shared framework is chosen.</span></span> <span data-ttu-id="51e4a-131">Na przykład załóżmy, że wersja "2.1.1" jest określona, ale zainstalowano "2.1.3".</span><span class="sxs-lookup"><span data-stu-id="51e4a-131">For example, suppose version "2.1.1" is specified, but "2.1.3" is installed.</span></span> <span data-ttu-id="51e4a-132">W takim przypadku aplikacja będzie używać "2.1.3".</span><span class="sxs-lookup"><span data-stu-id="51e4a-132">In that case, the app will use "2.1.3".</span></span> <span data-ttu-id="51e4a-133">Chociaż nie jest to zalecane, możesz wyłączyć przenoszenia do przodu (poprawki i/lub pomocnicze).</span><span class="sxs-lookup"><span data-stu-id="51e4a-133">Although not recommended, you can disable roll forward (patch and/or minor).</span></span> <span data-ttu-id="51e4a-134">Aby uzyskać więcej informacji na temat hosta dotnet przodu i konfigurowania jej zachowanie zobacz [dotnet hosta przenoszenia do przodu](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span><span class="sxs-lookup"><span data-stu-id="51e4a-134">For more information regarding dotnet host roll-forward and how to configure its behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

<span data-ttu-id="51e4a-135">Zestaw SDK projektu musi być równa `Microsoft.NET.Sdk.Web` w pliku projektu użyć niejawnego wersji `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="51e4a-135">The project's SDK must be set to `Microsoft.NET.Sdk.Web` in the project file to use the implicit version of `Microsoft.AspNetCore.All`.</span></span> <span data-ttu-id="51e4a-136">Gdy `Microsoft.NET.Sdk` zestawu SDK jest określony (`<Project Sdk="Microsoft.NET.Sdk">` u góry pliku projektu), jest generowane ostrzeżenie następujące:</span><span class="sxs-lookup"><span data-stu-id="51e4a-136">When the `Microsoft.NET.Sdk` SDK is specified (`<Project Sdk="Microsoft.NET.Sdk">` at the top of the project file), the following warning is generated:</span></span>

<span data-ttu-id="51e4a-137">*Ostrzeżenie NU1604: Zależność projektu pakiet nie zawiera włącznie dolną granicę. Uwzględnij dolną granicę w wersji zależności, aby zapewnić przywracania na poziomie wyniki.*</span><span class="sxs-lookup"><span data-stu-id="51e4a-137">*Warning NU1604: Project dependency Microsoft.AspNetCore.All does not contain an inclusive lower bound. Include a lower bound in the dependency version to ensure consistent restore results.*</span></span>

<span data-ttu-id="51e4a-138">Jest to znany problem z zestawu SDK programu .NET Core 2.1 i zostanie rozwiązany w .NET Core 2.2 SDK.</span><span class="sxs-lookup"><span data-stu-id="51e4a-138">This is a known issue with the .NET Core 2.1 SDK and will be fixed in the .NET Core 2.2 SDK.</span></span>

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="51e4a-139">Migrowanie z pakiet do Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="51e4a-139">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="51e4a-140">Następujące pakiety są objęte `Microsoft.AspNetCore.All` , ale nie `Microsoft.AspNetCore.App` pakietu.</span><span class="sxs-lookup"><span data-stu-id="51e4a-140">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span>

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

<span data-ttu-id="51e4a-141">Aby przenieść z `Microsoft.AspNetCore.All` do `Microsoft.AspNetCore.App`, jeśli aplikacja korzysta z żadnych interfejsów API z powyższych pakietów lub pakietów plików odwoływanych przez te pakiety, należy dodać odwołania do tych pakietów w projekcie.</span><span class="sxs-lookup"><span data-stu-id="51e4a-141">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="51e4a-142">Wszelkie zależności poprzedniego pakiety, które w przeciwnym razie nie ma zależności `Microsoft.AspNetCore.App` nie są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="51e4a-142">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="51e4a-143">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="51e4a-143">For example:</span></span>

* <span data-ttu-id="51e4a-144">`StackExchange.Redis` jako zależą od elementu `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="51e4a-144">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="51e4a-145">`Microsoft.ApplicationInsights` jako zależą od elementu `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="51e4a-145">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>

## <a name="update-aspnet-core-21"></a><span data-ttu-id="51e4a-146">Aktualizacja platformy ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="51e4a-146">Update ASP.NET Core 2.1</span></span>

<span data-ttu-id="51e4a-147">Zalecamy przeprowadzić migrację do `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all 2.1 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="51e4a-147">We recommend migrating to the `Microsoft.AspNetCore.App` metapackage for 2.1 and later.</span></span> <span data-ttu-id="51e4a-148">Aby nadal korzystać z `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all i upewnij się, jest wdrażana w najnowszej wersji poprawki:</span><span class="sxs-lookup"><span data-stu-id="51e4a-148">To keep using the `Microsoft.AspNetCore.All` metapackage and ensure the latest patch version is deployed:</span></span>

* <span data-ttu-id="51e4a-149">Na komputerach deweloperskich i serwery kompilacji: Zainstaluj najnowszą wersję [zestawu .NET Core SDK](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="51e4a-149">On development machines and build servers: Install the latest [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="51e4a-150">Na serwerach wdrożenia: Zainstaluj najnowszą wersję [środowisko uruchomieniowe programu .NET Core](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="51e4a-150">On deployment servers: Install the latest [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>
 <span data-ttu-id="51e4a-151">Twojej aplikacji będą uaktualniane do najnowszej zainstalowanej wersji na ponowne uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="51e4a-151">Your app will roll forward to the latest installed version on an application restart.</span></span>
