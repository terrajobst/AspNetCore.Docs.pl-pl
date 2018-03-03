---
title: Migrowanie z platformy ASP.NET Core 1.x 2.0
author: scottaddie
description: "W tym artykule opisano wymagania wstępne i najbardziej typowe kroki dotyczące migrowania projekt platformy ASP.NET Core 1.x ASP.NET Core 2.0."
manager: wpickett
ms.author: scaddie
ms.date: 10/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/index
ms.openlocfilehash: 42906e95d17f76f69dddc40f351b41e6cbdd087c
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a><span data-ttu-id="298ff-103">Migrowanie z platformy ASP.NET Core 1.x do platformy ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="298ff-103">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>

<span data-ttu-id="298ff-104">Przez [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="298ff-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="298ff-105">W tym artykule pomożemy Ci przez aktualizowanie istniejącego projektu platformy ASP.NET Core 1.x do składnika ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="298ff-105">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="298ff-106">Migrowanie aplikacji platformy ASP.NET Core 2.0 umożliwia korzystanie z [wiele nowe funkcje i ulepszenia wydajności](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="298ff-106">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span></span> 

<span data-ttu-id="298ff-107">Istniejące aplikacje 1.x platformy ASP.NET Core są oparte na znajdujące się na nich szablony przeznaczone do wersji projektu.</span><span class="sxs-lookup"><span data-stu-id="298ff-107">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="298ff-108">W miarę rozwoju platformy ASP.NET Core framework środowisko tak zrobić szablonów projektu i kod startowy zawarte w nich.</span><span class="sxs-lookup"><span data-stu-id="298ff-108">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="298ff-109">Oprócz aktualizacji w ramach platformy ASP.NET Core, musisz zaktualizować kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="298ff-109">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="298ff-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="298ff-110">Prerequisites</span></span>
<span data-ttu-id="298ff-111">Zobacz [wprowadzenie do platformy ASP.NET Core](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="298ff-111">Please see [Getting Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="298ff-112">Zaktualizuj Moniker platformy docelowej (TFM)</span><span class="sxs-lookup"><span data-stu-id="298ff-112">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="298ff-113">Należy używać w projektach przeznaczonych dla platformy .NET Core [TFM](/dotnet/standard/frameworks#referring-to-frameworks) wersji większa lub równa .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="298ff-113">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="298ff-114">Wyszukaj `<TargetFramework>` w węźle *.csproj* plików i zastąp jego tekst wewnętrzny z `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="298ff-114">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="298ff-115">Projektów przeznaczanie platformy .NET należy używać TFM wersji większa lub równa .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="298ff-115">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="298ff-116">Wyszukaj `<TargetFramework>` w węźle *.csproj* plików i zastąp jego tekst wewnętrzny z `net461`:</span><span class="sxs-lookup"><span data-stu-id="298ff-116">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="298ff-117">.NET core 2.0 oferuje znacznie większą powierzchni niż .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="298ff-117">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="298ff-118">Jeśli aplikacja jest przeznaczona dla .NET Framework wyłącznie z powodu brakującego interfejs API programu .NET Core 1.x przeznaczonych dla platformy .NET Core 2.0 jest zwykle działa.</span><span class="sxs-lookup"><span data-stu-id="298ff-118">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="298ff-119">Zaktualizuj wersję zestawu SDK programu .NET Core w global.json</span><span class="sxs-lookup"><span data-stu-id="298ff-119">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="298ff-120">Jeśli rozwiązanie opiera się na [ *global.json* ](https://docs.microsoft.com/dotnet/core/tools/global-json) plik, aby zastosować określoną wersję zestawu SDK programu .NET Core, zaktualizuj jego `version` właściwości korzysta z wersji 2.0 na komputerze jest zainstalowany:</span><span class="sxs-lookup"><span data-stu-id="298ff-120">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="298ff-121">Odwołania do pakietu aktualizacji</span><span class="sxs-lookup"><span data-stu-id="298ff-121">Update package references</span></span>
<span data-ttu-id="298ff-122">*.Csproj* każdy pakiet NuGet używane przez projekt zawiera listę plików w projekcie 1.x.</span><span class="sxs-lookup"><span data-stu-id="298ff-122">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="298ff-123">W projekcie platformy ASP.NET Core 2.0 przeznaczonych dla platformy .NET Core 2.0, jeden [metapackage](xref:fundamentals/metapackage) odwołania w *.csproj* plik zastępuje kolekcję pakietów:</span><span class="sxs-lookup"><span data-stu-id="298ff-123">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="298ff-124">Wszystkie funkcje platformy ASP.NET Core 2.0 oraz Entity Framework Core 2.0 znajdują się w metapackage.</span><span class="sxs-lookup"><span data-stu-id="298ff-124">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="298ff-125">Platformy ASP.NET Core 2.0 projektów przeznaczanie platformy .NET ma przejść do odwołania się do poszczególnych pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="298ff-125">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="298ff-126">Aktualizacja `Version` atrybut każdego `<PackageReference />` węzeł 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="298ff-126">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="298ff-127">Na przykład poniżej przedstawiono listę `<PackageReference />` węzłów używany w projekcie platformy ASP.NET Core 2.0 typowe przeznaczonych dla platformy .NET Framework:</span><span class="sxs-lookup"><span data-stu-id="298ff-127">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="298ff-128">Aktualizacja narzędzi interfejsu wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="298ff-128">Update .NET Core CLI tools</span></span>
<span data-ttu-id="298ff-129">W *.csproj* plików, zaktualizuj `Version` atrybut każdego `<DotNetCliToolReference />` węzeł 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="298ff-129">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="298ff-130">Na przykład poniżej przedstawiono listę narzędzi interfejsu wiersza polecenia używane w typowych projektu programu ASP.NET 2.0 Core przeznaczonych dla platformy .NET Core 2.0:</span><span class="sxs-lookup"><span data-stu-id="298ff-130">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="298ff-131">Zmień nazwę właściwości pakietu docelowy powrotu</span><span class="sxs-lookup"><span data-stu-id="298ff-131">Rename Package Target Fallback property</span></span>
<span data-ttu-id="298ff-132">*.Csproj* pliku projektu 1.x używane `PackageTargetFallback` węzeł i zmienną:</span><span class="sxs-lookup"><span data-stu-id="298ff-132">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="298ff-133">Zmień nazwę węzeł i zmienna `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="298ff-133">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="298ff-134">Metoda Main w pliku Program.cs aktualizacji</span><span class="sxs-lookup"><span data-stu-id="298ff-134">Update Main method in Program.cs</span></span>
<span data-ttu-id="298ff-135">W projektach 1.x `Main` metody *Program.cs* po zapoznaniu się następująco:</span><span class="sxs-lookup"><span data-stu-id="298ff-135">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="298ff-136">W projektach 2.0 `Main` metody *Program.cs* uproszczono:</span><span class="sxs-lookup"><span data-stu-id="298ff-136">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="298ff-137">Zastosowanie tego wzorca nowych 2.0 zdecydowanie zaleca się i jest wymagane dla produktu funkcji, takich jak [migracje Core Entity Framework (EF)](xref:data/ef-mvc/migrations) do pracy.</span><span class="sxs-lookup"><span data-stu-id="298ff-137">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="298ff-138">Na przykład uruchomiona `Update-Database` z okna konsoli Menedżera pakietów lub `dotnet ef database update` polecenia wiersza (w przypadku projektów przekonwertować ASP.NET Core 2.0) generuje następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="298ff-138">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a><span data-ttu-id="298ff-139">Dodawanie dostawcy konfiguracji</span><span class="sxs-lookup"><span data-stu-id="298ff-139">Add configuration providers</span></span>
<span data-ttu-id="298ff-140">W projektach 1.x wielkoskalowych został Dodawanie dostawcy konfiguracji do uruchamianych aplikacji `Startup` konstruktora.</span><span class="sxs-lookup"><span data-stu-id="298ff-140">In 1.x projects, adding configuration providers to an app was accomplished via the `Startup` constructor.</span></span> <span data-ttu-id="298ff-141">Kroki związane z tworzenia wystąpienia `ConfigurationBuilder`, ładowanie odpowiednich dostawców (zmienne środowiskowe, ustawienia aplikacji, itp.) i Inicjowanie członkiem `IConfigurationRoot`.</span><span class="sxs-lookup"><span data-stu-id="298ff-141">The steps involved creating an instance of `ConfigurationBuilder`, loading applicable providers (environment variables, app settings, etc.), and initializing a member of `IConfigurationRoot`.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

<span data-ttu-id="298ff-142">Ładuje w poprzednim przykładzie `Configuration` elementu członkowskiego przy użyciu ustawień konfiguracyjnych z *appsettings.json* oraz wszelkie *appsettings.\< EnvironmentName\>JSON* Dopasowywanie plików `IHostingEnvironment.EnvironmentName` właściwości.</span><span class="sxs-lookup"><span data-stu-id="298ff-142">The preceding example loads the `Configuration` member with configuration settings from *appsettings.json* as well as any *appsettings.\<EnvironmentName\>.json* file matching the `IHostingEnvironment.EnvironmentName` property.</span></span> <span data-ttu-id="298ff-143">Lokalizacja tych plików jest w tej samej ścieżce jako *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="298ff-143">The location of these files is at the same path as *Startup.cs*.</span></span>

<span data-ttu-id="298ff-144">W projektach 2.0 schematyczny kod konfiguracji związane z projektami 1.x uruchamia wewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="298ff-144">In 2.0 projects, the boilerplate configuration code inherent to 1.x projects runs behind-the-scenes.</span></span> <span data-ttu-id="298ff-145">Na przykład zmienne środowiskowe i ustawienia aplikacji są ładowane podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="298ff-145">For example, environment variables and app settings are loaded at startup.</span></span> <span data-ttu-id="298ff-146">Odpowiednik *Startup.cs* kodu zostanie zmniejszona do `IConfiguration` inicjowania z wprowadzonym wystąpieniem:</span><span class="sxs-lookup"><span data-stu-id="298ff-146">The equivalent *Startup.cs* code is reduced to `IConfiguration` initialization with the injected instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

<span data-ttu-id="298ff-147">Aby usunąć domyślnych dodane przez dostawców `WebHostBuilder.CreateDefaultBuilder`, wywołania `Clear` metoda `IConfigurationBuilder.Sources` właściwości wewnątrz `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="298ff-147">To remove the default providers added by `WebHostBuilder.CreateDefaultBuilder`, invoke the `Clear` method on the `IConfigurationBuilder.Sources` property inside of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="298ff-148">Dodaj ponownie dostawców, należy skorzystać `ConfigureAppConfiguration` metody w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="298ff-148">To add providers back, utilize the `ConfigureAppConfiguration` method in *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

<span data-ttu-id="298ff-149">Konfiguracja używana przez `CreateDefaultBuilder` są widoczne w poprzednim fragment kodu metody [tutaj](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="298ff-149">The configuration used by the `CreateDefaultBuilder` method in the preceding code snippet can be seen [here](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span></span>

<span data-ttu-id="298ff-150">Aby uzyskać więcej informacji, zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="298ff-150">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="298ff-151">Przenieś kod inicjujący bazy danych</span><span class="sxs-lookup"><span data-stu-id="298ff-151">Move database initialization code</span></span>
<span data-ttu-id="298ff-152">W projektach 1.x przy użyciu EF Core 1.x, polecenia, takich jak `dotnet ef migrations add` wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="298ff-152">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>
1. <span data-ttu-id="298ff-153">Tworzy wystąpienie `Startup` wystąpienia</span><span class="sxs-lookup"><span data-stu-id="298ff-153">Instantiates a `Startup` instance</span></span>
2. <span data-ttu-id="298ff-154">Wywołuje `ConfigureServices` metody do rejestrowania wszystkich usług z iniekcji zależności (w tym `DbContext` typów)</span><span class="sxs-lookup"><span data-stu-id="298ff-154">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
3. <span data-ttu-id="298ff-155">Wykonuje jego wymagane zadania</span><span class="sxs-lookup"><span data-stu-id="298ff-155">Performs its requisite tasks</span></span>

<span data-ttu-id="298ff-156">W projektach 2.0 za pomocą EF Core 2.0 `Program.BuildWebHost` jest wywoływane w celu uzyskania usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="298ff-156">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="298ff-157">W odróżnieniu od 1.x, to ma dodatkowe ubocznym wywoływania `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="298ff-157">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="298ff-158">Jeśli aplikacja 1.x wywoływany kod inicjujący bazy danych w jego `Configure` metody, mogą wystąpić nieoczekiwane problemy.</span><span class="sxs-lookup"><span data-stu-id="298ff-158">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="298ff-159">Na przykład jeśli jeszcze nie istnieje bazy danych, rozmieszczania kod jest uruchamiany przed wykonaniem polecenia EF Core migracji.</span><span class="sxs-lookup"><span data-stu-id="298ff-159">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="298ff-160">Ten problem powoduje, że `dotnet ef migrations list` polecenie, aby zakończyć się niepowodzeniem, jeśli jeszcze nie istnieje bazy danych.</span><span class="sxs-lookup"><span data-stu-id="298ff-160">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="298ff-161">Należy wziąć pod uwagę następujące kod inicjujący inicjatora 1.x w `Configure` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="298ff-161">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="298ff-162">W projektach 2.0, należy przenieść `SeedData.Initialize` wywołanie `Main` metody *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="298ff-162">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="298ff-163">Począwszy od 2.0, wykonywać żadnych czynności zły rozwiązaniem jest `BuildWebHost` z wyjątkiem kompilacji i skonfigurować hosta sieci web.</span><span class="sxs-lookup"><span data-stu-id="298ff-163">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="298ff-164">Wszystko, co jest o uruchamianiu aplikacji powinno zostać obsłużone poza `BuildWebHost` &mdash; zwykle w `Main` metody *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="298ff-164">Anything that's about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a><span data-ttu-id="298ff-165">Przejrzyj ustawienia kompilacji widoku Razor</span><span class="sxs-lookup"><span data-stu-id="298ff-165">Review Razor view compilation setting</span></span>
<span data-ttu-id="298ff-166">Skrócić czas uruchamiania aplikacji i mniejszych opublikowane pakiety są największe znaczenie dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="298ff-166">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="298ff-167">Z tego względu [kompilacji widoku Razor](xref:mvc/views/view-compilation) jest domyślnie włączone w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="298ff-167">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="298ff-168">Ustawienie `MvcRazorCompileOnPublish` właściwości na wartość true nie jest już wymagane.</span><span class="sxs-lookup"><span data-stu-id="298ff-168">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="298ff-169">O ile nie jest wyłączenie widoku kompilacji, właściwość może zostać usunięte z *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="298ff-169">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="298ff-170">Jeśli celem .NET Framework, nadal należy jawnie odwoływać się do [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) pakietu NuGet w Twojej *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="298ff-170">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="298ff-171">Funkcje "w górę jasny" usługi Application Insights</span><span class="sxs-lookup"><span data-stu-id="298ff-171">Rely on Application Insights "light-up" features</span></span>
<span data-ttu-id="298ff-172">Ważne jest łatwe ustawień Instrumentacji wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="298ff-172">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="298ff-173">Teraz polega na nowym [usługi Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "światła w górę" Funkcje dostępne w narzędzi Visual Studio 2017 r.</span><span class="sxs-lookup"><span data-stu-id="298ff-173">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="298ff-174">Domyślnie projektów platformy ASP.NET Core 1.1 utworzonych w programie Visual Studio 2017 dodany usługi Application Insights.</span><span class="sxs-lookup"><span data-stu-id="298ff-174">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="298ff-175">Jeśli nie używasz zestawu SDK usługi Application Insights bezpośrednio, poza *Program.cs* i *Startup.cs*, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="298ff-175">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="298ff-176">Jeśli przeznaczonych dla platformy .NET Core, Usuń następujące `<PackageReference />` węzła z *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="298ff-176">If targeting .NET Core, remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="298ff-177">Jeśli przeznaczonych dla platformy .NET Core, Usuń `UseApplicationInsights` wywołanie metody rozszerzenia z *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="298ff-177">If targeting .NET Core, remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="298ff-178">Usuń wywołanie interfejsu API klienta usługi Application Insights z *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="298ff-178">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="298ff-179">Obejmuje on następujące dwa wiersze kodu:</span><span class="sxs-lookup"><span data-stu-id="298ff-179">It comprises the following two lines of code:</span></span>

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

<span data-ttu-id="298ff-180">Jeśli korzystasz z zestawu SDK usługi Application Insights bezpośrednio, nadal można to zrobić.</span><span class="sxs-lookup"><span data-stu-id="298ff-180">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="298ff-181">2.0 [metapackage](xref:fundamentals/metapackage) zawiera najnowszą wersję usługi Application Insights, więc błąd starszą wersję pakietu jest dostępna w przypadku odwołujesz starszej wersji.</span><span class="sxs-lookup"><span data-stu-id="298ff-181">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a><span data-ttu-id="298ff-182">Przyjąć tożsamość/uwierzytelniania ulepszenia</span><span class="sxs-lookup"><span data-stu-id="298ff-182">Adopt authentication/Identity improvements</span></span>
<span data-ttu-id="298ff-183">Platformy ASP.NET Core 2.0 ma nowy model uwierzytelniania i Liczba znaczących zmian dotyczących tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="298ff-183">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="298ff-184">Jeśli utworzony projekt z włączoną indywidualnych kont użytkowników, lub jeśli ręcznie dodano uwierzytelniania lub tożsamości, zobacz [Migrowanie uwierzytelnianie i tożsamość platformy ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span><span class="sxs-lookup"><span data-stu-id="298ff-184">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="298ff-185">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="298ff-185">Additional resources</span></span>

* [<span data-ttu-id="298ff-186">Fundamentalne zmiany w podstawowej platformy ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="298ff-186">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
