---
title: Pakiet meta Microsoft.aspnetcore.all dla programu ASP.NET Core 2.0
author: Rick-Anderson
description: Pakiet meta Microsoft.aspnetcore.all nie jest zalecane dla platformy ASP.NET Core 2.1 lub nowszych.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: 5d49213e6d694f121d8301c94ba71782b2dc45cf
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086937"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="52ab2-103">Pakiet meta Microsoft.aspnetcore.all dla programu ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="52ab2-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="52ab2-104">Firma Microsoft zaleca, aby aplikacje przeznaczone na platformy ASP.NET Core 2.1 i później użyć [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) zamiast tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="52ab2-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="52ab2-105">Zobacz [migrowany pakiet do Microsoft.AspNetCore.App](#migrate) w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="52ab2-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="52ab2-106">Ta funkcja wymaga platformy ASP.NET Core 2.x określania wartości docelowej .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="52ab2-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="52ab2-107">[Pakiet](https://www.nuget.org/packages/Microsoft.AspNetCore.All) jest meta Microsoft.aspnetcore.all, która odwołuje się do udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="52ab2-107">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) is a metapackage that refers to a shared framework.</span></span> <span data-ttu-id="52ab2-108">A *udostępnionej platformy* to zbiór zestawów (*.dll* pliki) nie znajdują się w folderach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="52ab2-108">A *shared framework* is a set of assemblies (*.dll* files) that are not in the app's folders.</span></span> <span data-ttu-id="52ab2-109">Udostępnionej platformy musi być zainstalowany na komputerze, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="52ab2-109">The shared framework must be installed on the machine to run the app.</span></span> <span data-ttu-id="52ab2-110">Aby uzyskać więcej informacji, zobacz [udostępnionej platformy](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="52ab2-110">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="52ab2-111">Udostępnionej platformy, `Microsoft.AspNetCore.All` odwołuje się do obejmuje:</span><span class="sxs-lookup"><span data-stu-id="52ab2-111">The shared framework that `Microsoft.AspNetCore.All` refers to includes:</span></span>

* <span data-ttu-id="52ab2-112">Wszystkie obsługiwane pakiety przez zespół programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="52ab2-112">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="52ab2-113">Wszystkie obsługiwane pakiety przez Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="52ab2-113">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="52ab2-114">Zależności wewnętrzne i firm 3 używane przez program ASP.NET Core i Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="52ab2-114">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="52ab2-115">Wszystkie funkcje platformy ASP.NET Core 2.x i Entity Framework Core 2.x znajdują się w `Microsoft.AspNetCore.All` pakietu.</span><span class="sxs-lookup"><span data-stu-id="52ab2-115">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="52ab2-116">Domyślne szablony projektów przeznaczonych dla platformy ASP.NET Core 2.0, użyj tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="52ab2-116">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="52ab2-117">Numer wersji `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all reprezentuje minimalnej wersji platformy ASP.NET Core i Entity Framework Core wersji.</span><span class="sxs-lookup"><span data-stu-id="52ab2-117">The version number of the `Microsoft.AspNetCore.All` metapackage represents the minimum ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="52ab2-118">Następujące *.csproj* pliku odwołania `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all dla platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="52ab2-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a><span data-ttu-id="52ab2-119">Niejawne przechowywania wersji</span><span class="sxs-lookup"><span data-stu-id="52ab2-119">Implicit versioning</span></span>

<span data-ttu-id="52ab2-120">W programie ASP.NET Core 2.1 lub nowszej, możesz określić `Microsoft.AspNetCore.All` odwołania, które bez wersji pakietu.</span><span class="sxs-lookup"><span data-stu-id="52ab2-120">In ASP.NET Core 2.1 or later, you can specify the `Microsoft.AspNetCore.All` package reference without a version.</span></span> <span data-ttu-id="52ab2-121">Jeśli wersja nie jest określona, niejawne wersja jest określona przez zestaw SDK (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="52ab2-121">When the version isn't specified, an implicit version is specified by the SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="52ab2-122">Firma Microsoft zaleca, opierając się na niejawne wersji określony przez zestaw SDK i nie zostały jawnie ustawienie numeru wersji na odwołanie do pakietu.</span><span class="sxs-lookup"><span data-stu-id="52ab2-122">We recommend relying on the implicit version specified by the SDK and not explicitly setting the version number on the package reference.</span></span> <span data-ttu-id="52ab2-123">Jeśli masz pytania na temat tego podejścia, pozostaw komentarz GitHub na [dyskusję odnośnie do wersji niejawne Microsoft.AspNetCore.App](https://github.com/aspnet/AspNetCore.Docs/issues/6430).</span><span class="sxs-lookup"><span data-stu-id="52ab2-123">If you have questions about this approach, leave a GitHub comment at the [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/AspNetCore.Docs/issues/6430).</span></span>

<span data-ttu-id="52ab2-124">Jest ustawiona wersja niejawne `major.minor.0` przenośne aplikacji.</span><span class="sxs-lookup"><span data-stu-id="52ab2-124">The implicit version is set to `major.minor.0` for portable apps.</span></span> <span data-ttu-id="52ab2-125">Mechanizm przodu udostępnionej platformy uruchamia aplikację na najnowszej zgodnej wersji spośród zainstalowanych platform udostępnionych.</span><span class="sxs-lookup"><span data-stu-id="52ab2-125">The shared framework roll-forward mechanism runs the app on the latest compatible version among the installed shared frameworks.</span></span> <span data-ttu-id="52ab2-126">Aby zagwarantować, że ta sama wersja jest używana podczas tworzenia, testowania i produkcji, upewnij się, że tę samą wersję udostępnionego framework jest zainstalowana we wszystkich środowiskach.</span><span class="sxs-lookup"><span data-stu-id="52ab2-126">To guarantee the same version is used in development, test, and production, ensure the same version of the shared framework is installed in all environments.</span></span> <span data-ttu-id="52ab2-127">Dla aplikacji niezależna numer wersji niejawne został ustawiony na `major.minor.patch` udostępnionego Framework powiązane zainstalowanego zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="52ab2-127">For self-contained apps, the implicit version number is set to the `major.minor.patch` of the shared framework bundled in the installed SDK.</span></span>

<span data-ttu-id="52ab2-128">Określenie numeru wersji na `Microsoft.AspNetCore.All` jest odwołanie do pakietu **nie** gwarantuje daną wersję udostępnionego framework jest wybierany.</span><span class="sxs-lookup"><span data-stu-id="52ab2-128">Specifying a version number on the `Microsoft.AspNetCore.All` package reference does **not** guarantee that version of the shared framework is chosen.</span></span> <span data-ttu-id="52ab2-129">Na przykład załóżmy, że wersja "2.1.1" jest określona, ale zainstalowano "2.1.3".</span><span class="sxs-lookup"><span data-stu-id="52ab2-129">For example, suppose version "2.1.1" is specified, but "2.1.3" is installed.</span></span> <span data-ttu-id="52ab2-130">W takim przypadku aplikacja będzie używać "2.1.3".</span><span class="sxs-lookup"><span data-stu-id="52ab2-130">In that case, the app will use "2.1.3".</span></span> <span data-ttu-id="52ab2-131">Chociaż nie jest to zalecane, możesz wyłączyć przenoszenia do przodu (poprawki i/lub pomocnicze).</span><span class="sxs-lookup"><span data-stu-id="52ab2-131">Although not recommended, you can disable roll forward (patch and/or minor).</span></span> <span data-ttu-id="52ab2-132">Aby uzyskać więcej informacji na temat hosta dotnet przodu i konfigurowania jej zachowanie zobacz [dotnet hosta przenoszenia do przodu](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span><span class="sxs-lookup"><span data-stu-id="52ab2-132">For more information regarding dotnet host roll-forward and how to configure its behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

<span data-ttu-id="52ab2-133">Zestaw SDK projektu musi być równa `Microsoft.NET.Sdk.Web` w pliku projektu użyć niejawnego wersji `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="52ab2-133">The project's SDK must be set to `Microsoft.NET.Sdk.Web` in the project file to use the implicit version of `Microsoft.AspNetCore.All`.</span></span> <span data-ttu-id="52ab2-134">Gdy `Microsoft.NET.Sdk` zestawu SDK jest określony (`<Project Sdk="Microsoft.NET.Sdk">` u góry pliku projektu), jest generowane ostrzeżenie następujące:</span><span class="sxs-lookup"><span data-stu-id="52ab2-134">When the `Microsoft.NET.Sdk` SDK is specified (`<Project Sdk="Microsoft.NET.Sdk">` at the top of the project file), the following warning is generated:</span></span>

<span data-ttu-id="52ab2-135">*Ostrzeżenie NU1604: Zależności projektu pakiet nie zawiera włącznie dolną granicę. Uwzględnij dolną granicę w wersji zależności, aby zapewnić przywracania na poziomie wyniki.*</span><span class="sxs-lookup"><span data-stu-id="52ab2-135">*Warning NU1604: Project dependency Microsoft.AspNetCore.All does not contain an inclusive lower bound. Include a lower bound in the dependency version to ensure consistent restore results.*</span></span>

<span data-ttu-id="52ab2-136">Jest to znany problem z zestawu SDK programu .NET Core 2.1 i zostanie rozwiązany w .NET Core 2.2 SDK.</span><span class="sxs-lookup"><span data-stu-id="52ab2-136">This is a known issue with the .NET Core 2.1 SDK and will be fixed in the .NET Core 2.2 SDK.</span></span>

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="52ab2-137">Migrowanie z pakiet do Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="52ab2-137">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="52ab2-138">Następujące pakiety są objęte `Microsoft.AspNetCore.All` , ale nie `Microsoft.AspNetCore.App` pakietu.</span><span class="sxs-lookup"><span data-stu-id="52ab2-138">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span>

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

<span data-ttu-id="52ab2-139">Aby przenieść z `Microsoft.AspNetCore.All` do `Microsoft.AspNetCore.App`, jeśli aplikacja korzysta z żadnych interfejsów API z powyższych pakietów lub pakietów plików odwoływanych przez te pakiety, należy dodać odwołania do tych pakietów w projekcie.</span><span class="sxs-lookup"><span data-stu-id="52ab2-139">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="52ab2-140">Wszelkie zależności poprzedniego pakiety, które w przeciwnym razie nie ma zależności `Microsoft.AspNetCore.App` nie są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="52ab2-140">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="52ab2-141">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="52ab2-141">For example:</span></span>

* <span data-ttu-id="52ab2-142">`StackExchange.Redis` jako zależą od elementu `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="52ab2-142">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="52ab2-143">`Microsoft.ApplicationInsights` jako zależą od elementu `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="52ab2-143">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>

## <a name="update-aspnet-core-21"></a><span data-ttu-id="52ab2-144">Aktualizacja platformy ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="52ab2-144">Update ASP.NET Core 2.1</span></span>

<span data-ttu-id="52ab2-145">Zalecamy przeprowadzić migrację do `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all 2.1 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="52ab2-145">We recommend migrating to the `Microsoft.AspNetCore.App` metapackage for 2.1 and later.</span></span> <span data-ttu-id="52ab2-146">Aby nadal korzystać z `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all i upewnij się, jest wdrażana w najnowszej wersji poprawki:</span><span class="sxs-lookup"><span data-stu-id="52ab2-146">To keep using the `Microsoft.AspNetCore.All` metapackage and ensure the latest patch version is deployed:</span></span>

* <span data-ttu-id="52ab2-147">Na komputerach deweloperskich i serwerach kompilacji: Zainstaluj najnowszą wersję [zestawu .NET Core SDK](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="52ab2-147">On development machines and build servers: Install the latest [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="52ab2-148">Na serwerach wdrożenia: Zainstaluj najnowszą wersję [środowisko uruchomieniowe programu .NET Core](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="52ab2-148">On deployment servers: Install the latest [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>
 <span data-ttu-id="52ab2-149">Twojej aplikacji będą uaktualniane do najnowszej zainstalowanej wersji na ponowne uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="52ab2-149">Your app will roll forward to the latest installed version on an application restart.</span></span>
