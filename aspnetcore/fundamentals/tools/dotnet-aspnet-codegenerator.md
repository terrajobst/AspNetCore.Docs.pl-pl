---
title: polecenie elementu codegenerator aspnet DotNet
author: rick-anderson
description: Polecenia dotnet elementu codegenerator aspnet szkielety mechanizmów projektów ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/04/2019
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: c96362f320efd84c35dc07294a2968a2c687ee94
ms.sourcegitcommit: b9e914ef274b5ec359582f299724af6234dce135
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67596140"
---
# <a name="dotnet-aspnet-codegenerator"></a><span data-ttu-id="26baa-103">dotnet aspnet-codegenerator</span><span class="sxs-lookup"><span data-stu-id="26baa-103">dotnet aspnet-codegenerator</span></span>

<span data-ttu-id="26baa-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="26baa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="26baa-105">`dotnet aspnet-codegenerator` — Działa aparat tworzenia szkieletu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="26baa-105">`dotnet aspnet-codegenerator` - Runs the ASP.NET Core scaffolding engine.</span></span> <span data-ttu-id="26baa-106">`dotnet aspnet-codegenerator` jest tylko wymagane do tworzenia szkieletu z wiersza polecenia, nie jest konieczność tworzenia szkieletu za pomocą programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26baa-106">`dotnet aspnet-codegenerator` is only required to scaffold from the command line, it's not need to use scaffolding with Visual Studio.</span></span>

<span data-ttu-id="26baa-107">Ten artykuł dotyczy [zestawu SDK programu .NET Core 2.1](https://dotnet.microsoft.com/download/dotnet-core/2.1) i nowszych.</span><span class="sxs-lookup"><span data-stu-id="26baa-107">This article applies to [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1) and later.</span></span>

## <a name="installing-aspnet-codegenerator"></a><span data-ttu-id="26baa-108">Instalowanie elementu codegenerator aspnet</span><span class="sxs-lookup"><span data-stu-id="26baa-108">Installing aspnet-codegenerator</span></span>

<span data-ttu-id="26baa-109">`dotnet-aspnet-codegenerator` jest [narzędzie globalne](/dotnet/core/tools/global-tools) musi być zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="26baa-109">`dotnet-aspnet-codegenerator` is a [global tool](/dotnet/core/tools/global-tools) that must be installed.</span></span> <span data-ttu-id="26baa-110">Poniższe polecenie instaluje najnowszą stabilną wersję `dotnet-aspnet-codegenerator` narzędzie:</span><span class="sxs-lookup"><span data-stu-id="26baa-110">The following command installs the latest stable version of the `dotnet-aspnet-codegenerator` tool:</span></span>

```console
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="26baa-111">Następujące polecenie aktualizacji `dotnet-aspnet-codegenerator` do najnowszej stabilnej wersji dostępne z zainstalowanych zestawów .NET Core SDK:</span><span class="sxs-lookup"><span data-stu-id="26baa-111">The following command updates `dotnet-aspnet-codegenerator` to the latest stable version available from the installed .NET Core SDKs:</span></span>

```console
dotnet tool update -g dotnet-aspnet-codegenerator
```

## <a name="synopsis"></a><span data-ttu-id="26baa-112">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="26baa-112">Synopsis</span></span>

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a><span data-ttu-id="26baa-113">Opis</span><span class="sxs-lookup"><span data-stu-id="26baa-113">Description</span></span>

<span data-ttu-id="26baa-114">`dotnet aspnet-codegenerator` Globalnego polecenie uruchamia platformy ASP.NET Core, generator kodu i aparatu do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="26baa-114">The `dotnet aspnet-codegenerator` global command runs the ASP.NET Core code generator and scaffolding engine.</span></span>

## <a name="arguments"></a><span data-ttu-id="26baa-115">Argumenty</span><span class="sxs-lookup"><span data-stu-id="26baa-115">Arguments</span></span>

`generator`

<span data-ttu-id="26baa-116">Generator kodu do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="26baa-116">The code generator to run.</span></span> <span data-ttu-id="26baa-117">Dostępne są następujące generatory:</span><span class="sxs-lookup"><span data-stu-id="26baa-117">The following generators are available:</span></span>

| <span data-ttu-id="26baa-118">Generator</span><span class="sxs-lookup"><span data-stu-id="26baa-118">Generator</span></span> | <span data-ttu-id="26baa-119">Operacja</span><span class="sxs-lookup"><span data-stu-id="26baa-119">Operation</span></span> |
| ----------------- | ------------ | 
| <span data-ttu-id="26baa-120">Obszar</span><span class="sxs-lookup"><span data-stu-id="26baa-120">area</span></span>      | [<span data-ttu-id="26baa-121">Szkielety mechanizmów obszar</span><span class="sxs-lookup"><span data-stu-id="26baa-121">Scaffolds an Area</span></span>](/aspnet/core/mvc/controllers/areas) |
  <span data-ttu-id="26baa-122">kontroler</span><span class="sxs-lookup"><span data-stu-id="26baa-122">controller</span></span>| [<span data-ttu-id="26baa-123">Scaffolds kontrolera</span><span class="sxs-lookup"><span data-stu-id="26baa-123">Scaffolds a controller</span></span>](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  <span data-ttu-id="26baa-124">tożsamość</span><span class="sxs-lookup"><span data-stu-id="26baa-124">identity</span></span>  | [<span data-ttu-id="26baa-125">Szkielety mechanizmów tożsamości</span><span class="sxs-lookup"><span data-stu-id="26baa-125">Scaffolds Identity</span></span>](/aspnet/core/security/authentication/scaffold-identity) |
  <span data-ttu-id="26baa-126">razorpage</span><span class="sxs-lookup"><span data-stu-id="26baa-126">razorpage</span></span> | [<span data-ttu-id="26baa-127">Szkielety mechanizmów stron Razor</span><span class="sxs-lookup"><span data-stu-id="26baa-127">Scaffolds Razor Pages</span></span>](/aspnet/core/tutorials/razor-pages/model) |
  <span data-ttu-id="26baa-128">Widok</span><span class="sxs-lookup"><span data-stu-id="26baa-128">view</span></span>      | [<span data-ttu-id="26baa-129">Scaffolds widoku</span><span class="sxs-lookup"><span data-stu-id="26baa-129">Scaffolds a view</span></span>](/aspnet/core/mvc/views/overview) |

## <a name="options"></a><span data-ttu-id="26baa-130">Opcje</span><span class="sxs-lookup"><span data-stu-id="26baa-130">Options</span></span>

`-n|--nuget-package-dir`

<span data-ttu-id="26baa-131">Określa katalog pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="26baa-131">Specifies the NuGet package directory.</span></span>

`-c|--configuration {Debug|Release}`

<span data-ttu-id="26baa-132">Definiuje konfigurację kompilacji.</span><span class="sxs-lookup"><span data-stu-id="26baa-132">Defines the build configuration.</span></span> <span data-ttu-id="26baa-133">Wartość domyślna to `Debug`.</span><span class="sxs-lookup"><span data-stu-id="26baa-133">The default value is `Debug`.</span></span>

`-tfm|--target-framework`

<span data-ttu-id="26baa-134">Docelowy [Framework](/dotnet/standard/frameworks) do użycia.</span><span class="sxs-lookup"><span data-stu-id="26baa-134">Target [Framework](/dotnet/standard/frameworks) to use.</span></span> <span data-ttu-id="26baa-135">Na przykład `net46`.</span><span class="sxs-lookup"><span data-stu-id="26baa-135">For example, `net46`.</span></span>

`-b|--build-base-path`

<span data-ttu-id="26baa-136">Ścieżka podstawowa kompilacji.</span><span class="sxs-lookup"><span data-stu-id="26baa-136">The build base path.</span></span>

`-h|--help`

<span data-ttu-id="26baa-137">Drukuje krótki pomoc dotyczącą polecenia.</span><span class="sxs-lookup"><span data-stu-id="26baa-137">Prints out a short help for the command.</span></span>

`--no-build`

<span data-ttu-id="26baa-138">Nie da się skompilować projekt przed uruchomieniem.</span><span class="sxs-lookup"><span data-stu-id="26baa-138">Doesn't build the project before running.</span></span> <span data-ttu-id="26baa-139">Ustawia również niejawnie `--no-restore` flagi.</span><span class="sxs-lookup"><span data-stu-id="26baa-139">It also implicitly sets the `--no-restore` flag.</span></span>

`-p|--project <PATH>`

<span data-ttu-id="26baa-140">Określa ścieżkę do pliku projektu do uruchomienia (nazwa folderu lub pełną ścieżkę).</span><span class="sxs-lookup"><span data-stu-id="26baa-140">Specifies the path of the project file to run (folder name or full path).</span></span> <span data-ttu-id="26baa-141">Jeśli nie zostanie określony, jego wartość domyślna w bieżącym katalogu.</span><span class="sxs-lookup"><span data-stu-id="26baa-141">If not specified, it defaults to the current directory.</span></span>

## <a name="generator-options"></a><span data-ttu-id="26baa-142">Opcje Generator</span><span class="sxs-lookup"><span data-stu-id="26baa-142">Generator options</span></span>

<span data-ttu-id="26baa-143">Opcje dostępne dla obsługiwanych generatorów można znaleźć w poniższych sekcjach:</span><span class="sxs-lookup"><span data-stu-id="26baa-143">The following sections detail the options available for the supported generators:</span></span>

* <span data-ttu-id="26baa-144">Obszar</span><span class="sxs-lookup"><span data-stu-id="26baa-144">Area</span></span>
* <span data-ttu-id="26baa-145">Kontroler</span><span class="sxs-lookup"><span data-stu-id="26baa-145">Controller</span></span>
* <span data-ttu-id="26baa-146">Tożsamość</span><span class="sxs-lookup"><span data-stu-id="26baa-146">Identity</span></span>  
* <span data-ttu-id="26baa-147">Razorpage</span><span class="sxs-lookup"><span data-stu-id="26baa-147">Razorpage</span></span>
* <span data-ttu-id="26baa-148">Widok</span><span class="sxs-lookup"><span data-stu-id="26baa-148">View</span></span>

<a name="area"></a>

### <a name="area-options"></a><span data-ttu-id="26baa-149">Opcji obszaru</span><span class="sxs-lookup"><span data-stu-id="26baa-149">Area options</span></span>

<span data-ttu-id="26baa-150">To narzędzie jest przeznaczony dla projektów sieci web platformy ASP.NET Core za pomocą kontrolerów i widoków.</span><span class="sxs-lookup"><span data-stu-id="26baa-150">This tool is intended for ASP.NET Core web projects with controllers and views.</span></span> <span data-ttu-id="26baa-151">Nie jest przeznaczona dla aplikacji stron Razor.</span><span class="sxs-lookup"><span data-stu-id="26baa-151">It's not intended for Razor Pages apps.</span></span>

<span data-ttu-id="26baa-152">Użycie: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span><span class="sxs-lookup"><span data-stu-id="26baa-152">Usage: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span></span>

<span data-ttu-id="26baa-153">Poprzednie polecenie generuje następujące foldery:</span><span class="sxs-lookup"><span data-stu-id="26baa-153">The preceding command generates the following folders:</span></span>

* <span data-ttu-id="26baa-154">*Obszary*</span><span class="sxs-lookup"><span data-stu-id="26baa-154">*Areas*</span></span>
  * <span data-ttu-id="26baa-155">*AreaNameToGenerate*</span><span class="sxs-lookup"><span data-stu-id="26baa-155">*AreaNameToGenerate*</span></span>
    * <span data-ttu-id="26baa-156">*Kontrolery*</span><span class="sxs-lookup"><span data-stu-id="26baa-156">*Controllers*</span></span>
    * <span data-ttu-id="26baa-157">*Dane*</span><span class="sxs-lookup"><span data-stu-id="26baa-157">*Data*</span></span>
    * <span data-ttu-id="26baa-158">*Modele*</span><span class="sxs-lookup"><span data-stu-id="26baa-158">*Models*</span></span>
    * <span data-ttu-id="26baa-159">*Widoki*</span><span class="sxs-lookup"><span data-stu-id="26baa-159">*Views*</span></span>

<a name="ctl"></a>

### <a name="controller-options"></a><span data-ttu-id="26baa-160">Opcje kontrolera</span><span class="sxs-lookup"><span data-stu-id="26baa-160">Controller options</span></span>

<span data-ttu-id="26baa-161">W poniższej tabeli wymieniono opcje `aspnet-codegenerator` `controller` i `razorpage`:</span><span class="sxs-lookup"><span data-stu-id="26baa-161">The following table lists options for  `aspnet-codegenerator` `controller` and `razorpage`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="26baa-162">W poniższej tabeli wymieniono opcje unikatowe dla `aspnet-codegenerator controller`:</span><span class="sxs-lookup"><span data-stu-id="26baa-162">The following table lists options unique to  `aspnet-codegenerator controller`:</span></span>

| <span data-ttu-id="26baa-163">Opcja</span><span class="sxs-lookup"><span data-stu-id="26baa-163">Option</span></span>               | <span data-ttu-id="26baa-164">Opis</span><span class="sxs-lookup"><span data-stu-id="26baa-164">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="26baa-165">Element controllerName — lub — nazwa</span><span class="sxs-lookup"><span data-stu-id="26baa-165">--controllerName or -name</span></span> | <span data-ttu-id="26baa-166">Nazwa kontrolera.</span><span class="sxs-lookup"><span data-stu-id="26baa-166">Name of the controller.</span></span> |
| <span data-ttu-id="26baa-167">--useAsyncActions lub - async</span><span class="sxs-lookup"><span data-stu-id="26baa-167">--useAsyncActions or -async</span></span> | <span data-ttu-id="26baa-168">Generowanie asynchronicznych akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="26baa-168">Generate async controller actions.</span></span> |
| <span data-ttu-id="26baa-169">--noViews lub -nv</span><span class="sxs-lookup"><span data-stu-id="26baa-169">--noViews or -nv</span></span> | <span data-ttu-id="26baa-170">Generowanie **nie** widoków.</span><span class="sxs-lookup"><span data-stu-id="26baa-170">Generate **no** views.</span></span> |
| <span data-ttu-id="26baa-171">--restWithNoViews lub - interfejsu api</span><span class="sxs-lookup"><span data-stu-id="26baa-171">--restWithNoViews or -api</span></span>  | <span data-ttu-id="26baa-172">Generowanie kontrolera ze stylem REST API.</span><span class="sxs-lookup"><span data-stu-id="26baa-172">Generate a Controller with REST style API.</span></span> <span data-ttu-id="26baa-173">`noViews` przyjęto, że i dowolne Wyświetl powiązane opcje są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="26baa-173">`noViews` is assumed and any view related options are ignored.</span></span> |
| <span data-ttu-id="26baa-174">--readWriteActions lub - akcje</span><span class="sxs-lookup"><span data-stu-id="26baa-174">--readWriteActions or -actions</span></span> | <span data-ttu-id="26baa-175">Generowanie kontroler z akcjami odczytu/zapisu bez modelu.</span><span class="sxs-lookup"><span data-stu-id="26baa-175">Generate controller with read/write actions without a model.</span></span> |

<span data-ttu-id="26baa-176">Użyj `-h` przełączać w celu uzyskania pomocy na `aspnet-codegenerator controller` polecenia:</span><span class="sxs-lookup"><span data-stu-id="26baa-176">Use the `-h` switch for help on the `aspnet-codegenerator controller` command:</span></span>

```console
dotnet aspnet-codegenerator controller -h
```

<span data-ttu-id="26baa-177">Zobacz [tworzenia szkieletu modelu movie](/aspnet/core/tutorials/razor-pages/model) przykład `dotnet aspnet-codegenerator controller`.</span><span class="sxs-lookup"><span data-stu-id="26baa-177">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator controller`.</span></span>

### <a name="razorpage"></a><span data-ttu-id="26baa-178">Razorpage</span><span class="sxs-lookup"><span data-stu-id="26baa-178">Razorpage</span></span>

<a name="rp"></a>

<span data-ttu-id="26baa-179">Strony razor można indywidualnie szkieletu, określając nazwę nowej strony i szablon do użycia.</span><span class="sxs-lookup"><span data-stu-id="26baa-179">Razor Pages can be individually scaffolded by specifying the name of the new page and the template to use.</span></span> <span data-ttu-id="26baa-180">Są obsługiwane szablony:</span><span class="sxs-lookup"><span data-stu-id="26baa-180">The supported templates are:</span></span>

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="26baa-181">Na przykład następujące polecenie używa Edytuj szablon do generowania *MyEdit.cshtml* i *MyEdit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="26baa-181">For example, the following command uses the Edit template to generate *MyEdit.cshtml* and *MyEdit.cshtml.cs*:</span></span>

```console
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

<span data-ttu-id="26baa-182">Typowo, szablon i wygenerowanej nazwy pliku nie zostanie określony, i są tworzone następujące szablony:</span><span class="sxs-lookup"><span data-stu-id="26baa-182">Typically, the template and generated file name is not specified, and the following templates are created:</span></span>

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="26baa-183">W poniższej tabeli wymieniono opcje `aspnet-codegenerator` `razorpage` i `controller`:</span><span class="sxs-lookup"><span data-stu-id="26baa-183">The following table lists options for  `aspnet-codegenerator` `razorpage` and `controller`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="26baa-184">W poniższej tabeli wymieniono opcje unikatowe dla `aspnet-codegenerator razorpage`:</span><span class="sxs-lookup"><span data-stu-id="26baa-184">The following table lists options unique to  `aspnet-codegenerator razorpage`:</span></span>

| <span data-ttu-id="26baa-185">Opcja</span><span class="sxs-lookup"><span data-stu-id="26baa-185">Option</span></span>               | <span data-ttu-id="26baa-186">Opis</span><span class="sxs-lookup"><span data-stu-id="26baa-186">Description</span></span>|
| ----------------- | ------------ |
|   <span data-ttu-id="26baa-187">--namespaceName lub — przestrzeń nazw</span><span class="sxs-lookup"><span data-stu-id="26baa-187">--namespaceName or -namespace</span></span> | <span data-ttu-id="26baa-188">Nazwa przestrzeni nazw do użycia dla wygenerowanej klasy PageModel</span><span class="sxs-lookup"><span data-stu-id="26baa-188">The name of the namespace to use for the generated PageModel</span></span> |
| <span data-ttu-id="26baa-189">--partialView ani - częściowy</span><span class="sxs-lookup"><span data-stu-id="26baa-189">--partialView or -partial</span></span> | <span data-ttu-id="26baa-190">Generowanie widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="26baa-190">Generate a partial view.</span></span> <span data-ttu-id="26baa-191">Układ opcji -l i - udl są ignorowane, jeśli jest określony.</span><span class="sxs-lookup"><span data-stu-id="26baa-191">Layout options -l and -udl are ignored if this is specified.</span></span> |
| <span data-ttu-id="26baa-192">--noPageModel lub - npm</span><span class="sxs-lookup"><span data-stu-id="26baa-192">--noPageModel or -npm</span></span> | <span data-ttu-id="26baa-193">Przełącz na nie Generuj klasę PageModel w przypadku pustego szablonu</span><span class="sxs-lookup"><span data-stu-id="26baa-193">Switch to not generate a PageModel class for Empty template</span></span> |

<span data-ttu-id="26baa-194">Użyj `-h` przełączać w celu uzyskania pomocy na `aspnet-codegenerator razorpage` polecenia:</span><span class="sxs-lookup"><span data-stu-id="26baa-194">Use the `-h` switch for help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```

<span data-ttu-id="26baa-195">Zobacz [tworzenia szkieletu modelu movie](/aspnet/core/tutorials/razor-pages/model) przykład `dotnet aspnet-codegenerator razorpage`.</span><span class="sxs-lookup"><span data-stu-id="26baa-195">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator razorpage`.</span></span>

### <a name="identity"></a><span data-ttu-id="26baa-196">Tożsamość</span><span class="sxs-lookup"><span data-stu-id="26baa-196">Identity</span></span>

<span data-ttu-id="26baa-197">Zobacz [tworzenia szkieletu tożsamości](/aspnet/core/security/authentication/scaffold-identity)</span><span class="sxs-lookup"><span data-stu-id="26baa-197">See [Scaffold Identity](/aspnet/core/security/authentication/scaffold-identity)</span></span>
