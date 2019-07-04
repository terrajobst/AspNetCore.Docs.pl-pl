---
title: polecenie elementu codegenerator aspnet DotNet
author: rick-anderson
ms.author: riande
description: Polecenia dotnet elementu codegenerator aspnet szkielety mechanizmów projektów ASP.NET Core
ms.date: 07/04/2019
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: 55b592d9d203970777c84438e210519957abb35d
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561740"
---
# <a name="dotnet-aspnet-codegenerator"></a><span data-ttu-id="eb408-103">dotnet aspnet-codegenerator</span><span class="sxs-lookup"><span data-stu-id="eb408-103">dotnet aspnet-codegenerator</span></span>

<span data-ttu-id="eb408-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eb408-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eb408-105">`dotnet aspnet-codegenerator` — Działa aparat tworzenia szkieletu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eb408-105">`dotnet aspnet-codegenerator` - Runs the ASP.NET Core scaffolding engine.</span></span> <span data-ttu-id="eb408-106">`dotnet aspnet-codegenerator` jest tylko wymagane do tworzenia szkieletu z wiersza polecenia, nie jest konieczność tworzenia szkieletu za pomocą programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb408-106">`dotnet aspnet-codegenerator` is only required to scaffold from the command line, it's not need to use scaffolding with Visual Studio.</span></span>

<span data-ttu-id="eb408-107">Ten artykuł dotyczy [zestawu .NET Core 2.2 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.2) i nowszych.</span><span class="sxs-lookup"><span data-stu-id="eb408-107">This article applies to [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.2) and later.</span></span>

## <a name="installing-aspnet-codegenerator"></a><span data-ttu-id="eb408-108">Instalowanie elementu codegenerator aspnet</span><span class="sxs-lookup"><span data-stu-id="eb408-108">Installing aspnet-codegenerator</span></span>

<span data-ttu-id="eb408-109">`aspnet-codegenerator` jest [narzędzie globalne](/dotnet/core/tools/global-tools) musi być zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="eb408-109">`aspnet-codegenerator` is a [global tool](/dotnet/core/tools/global-tools) that must be installed.</span></span> <span data-ttu-id="eb408-110">Poniższe polecenie instaluje najnowszą stabilną wersję `aspnet-codegenerator` narzędzie:</span><span class="sxs-lookup"><span data-stu-id="eb408-110">The following command installs the latest stable version of the `aspnet-codegenerator` tool:</span></span>

```console
dotnet tool install -g aspnet-codegenerator
```

<span data-ttu-id="eb408-111">Następujące polecenie aktualizacji `aspnet-codegenerator` do najnowszej stabilnej wersji dostępne z zainstalowanych zestawów .NET Core SDK:</span><span class="sxs-lookup"><span data-stu-id="eb408-111">The following command updates `aspnet-codegenerator` to the latest stable version available from the installed .NET Core SDKs:</span></span>

```console
dotnet tool update -g aspnet-codegenerator
```

## <a name="synopsis"></a><span data-ttu-id="eb408-112">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="eb408-112">Synopsis</span></span>

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a><span data-ttu-id="eb408-113">Opis</span><span class="sxs-lookup"><span data-stu-id="eb408-113">Description</span></span>

<span data-ttu-id="eb408-114">`dotnet aspnet-codegenerator ` Globalnego polecenie uruchamia platformy ASP.NET Core, generator kodu i aparatu do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="eb408-114">The `dotnet aspnet-codegenerator ` global command runs the ASP.NET Core code generator and scaffolding engine.</span></span>

## <a name="arguments"></a><span data-ttu-id="eb408-115">Argumenty</span><span class="sxs-lookup"><span data-stu-id="eb408-115">Arguments</span></span>

`generator`

<span data-ttu-id="eb408-116">Generator kodu do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="eb408-116">The code generator to run.</span></span> <span data-ttu-id="eb408-117">Dostępne są następujące generatory:</span><span class="sxs-lookup"><span data-stu-id="eb408-117">The following generators are available:</span></span>

| <span data-ttu-id="eb408-118">Generator</span><span class="sxs-lookup"><span data-stu-id="eb408-118">Generator</span></span> | <span data-ttu-id="eb408-119">Operacja</span><span class="sxs-lookup"><span data-stu-id="eb408-119">Operation</span></span> |
| ----------------- | ------------ | 
| <span data-ttu-id="eb408-120">Obszar</span><span class="sxs-lookup"><span data-stu-id="eb408-120">area</span></span>      | [<span data-ttu-id="eb408-121">Szkielety mechanizmów obszar</span><span class="sxs-lookup"><span data-stu-id="eb408-121">Scaffolds an Area</span></span>](/aspnet/core/mvc/controllers/areas) |
  <span data-ttu-id="eb408-122">kontroler</span><span class="sxs-lookup"><span data-stu-id="eb408-122">controller</span></span>| [<span data-ttu-id="eb408-123">Scaffolds kontrolera</span><span class="sxs-lookup"><span data-stu-id="eb408-123">Scaffolds a controller</span></span>](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  <span data-ttu-id="eb408-124">tożsamość</span><span class="sxs-lookup"><span data-stu-id="eb408-124">identity</span></span>  | [<span data-ttu-id="eb408-125">Szkielety mechanizmów tożsamości</span><span class="sxs-lookup"><span data-stu-id="eb408-125">Scaffolds Identity</span></span>](/aspnet/core/security/authentication/scaffold-identity) |
  <span data-ttu-id="eb408-126">razorpage</span><span class="sxs-lookup"><span data-stu-id="eb408-126">razorpage</span></span> | [<span data-ttu-id="eb408-127">Szkielety mechanizmów stron Razor</span><span class="sxs-lookup"><span data-stu-id="eb408-127">Scaffolds Razor Pages</span></span>](/aspnet/core/tutorials/razor-pages/model) |
  <span data-ttu-id="eb408-128">Widok</span><span class="sxs-lookup"><span data-stu-id="eb408-128">view</span></span>      | [<span data-ttu-id="eb408-129">Scaffolds widoku</span><span class="sxs-lookup"><span data-stu-id="eb408-129">Scaffolds a view</span></span>](/aspnet/core/mvc/views/overview) |

## <a name="options"></a><span data-ttu-id="eb408-130">Opcje</span><span class="sxs-lookup"><span data-stu-id="eb408-130">Options</span></span>

`-n|--nuget-package-dir`

<span data-ttu-id="eb408-131">Określa katalog pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="eb408-131">Specifies the NuGet package directory.</span></span>

`-c|--configuration {Debug|Release}`

<span data-ttu-id="eb408-132">Definiuje konfigurację kompilacji.</span><span class="sxs-lookup"><span data-stu-id="eb408-132">Defines the build configuration.</span></span> <span data-ttu-id="eb408-133">Wartość domyślna to `Debug`.</span><span class="sxs-lookup"><span data-stu-id="eb408-133">The default value is `Debug`.</span></span>

`-tfm|--target-framework`

<span data-ttu-id="eb408-134">Docelowy [Framework](/dotnet/standard/frameworks) do użycia.</span><span class="sxs-lookup"><span data-stu-id="eb408-134">Target [Framework](/dotnet/standard/frameworks) to use.</span></span> <span data-ttu-id="eb408-135">Na przykład `net46`.</span><span class="sxs-lookup"><span data-stu-id="eb408-135">For example, `net46`.</span></span>

`-b|--build-base-path`

<span data-ttu-id="eb408-136">Ścieżka podstawowa kompilacji.</span><span class="sxs-lookup"><span data-stu-id="eb408-136">The build base path.</span></span>

`-h|--help`

<span data-ttu-id="eb408-137">Drukuje krótki pomoc dotyczącą polecenia.</span><span class="sxs-lookup"><span data-stu-id="eb408-137">Prints out a short help for the command.</span></span>

`--no-build`

<span data-ttu-id="eb408-138">Nie da się skompilować projekt przed uruchomieniem.</span><span class="sxs-lookup"><span data-stu-id="eb408-138">Doesn't build the project before running.</span></span> <span data-ttu-id="eb408-139">Ustawia również niejawnie `--no-restore` flagi.</span><span class="sxs-lookup"><span data-stu-id="eb408-139">It also implicitly sets the `--no-restore` flag.</span></span>

`-p|--project <PATH>`

<span data-ttu-id="eb408-140">Określa ścieżkę do pliku projektu do uruchomienia (nazwa folderu lub pełną ścieżkę).</span><span class="sxs-lookup"><span data-stu-id="eb408-140">Specifies the path of the project file to run (folder name or full path).</span></span> <span data-ttu-id="eb408-141">Jeśli nie zostanie określony, jego wartość domyślna w bieżącym katalogu.</span><span class="sxs-lookup"><span data-stu-id="eb408-141">If not specified, it defaults to the current directory.</span></span>

## <a name="generator-options"></a><span data-ttu-id="eb408-142">Opcje Generator</span><span class="sxs-lookup"><span data-stu-id="eb408-142">Generator options</span></span>

<span data-ttu-id="eb408-143">Opcje dostępne dla obsługiwanych generatorów można znaleźć w poniższych sekcjach:</span><span class="sxs-lookup"><span data-stu-id="eb408-143">The following sections detail the options available for the supported generators:</span></span>

* <span data-ttu-id="eb408-144">Obszar</span><span class="sxs-lookup"><span data-stu-id="eb408-144">Area</span></span>
* <span data-ttu-id="eb408-145">Kontroler</span><span class="sxs-lookup"><span data-stu-id="eb408-145">Controller</span></span>
* <span data-ttu-id="eb408-146">Tożsamość</span><span class="sxs-lookup"><span data-stu-id="eb408-146">Identity</span></span>  
* <span data-ttu-id="eb408-147">Razorpage</span><span class="sxs-lookup"><span data-stu-id="eb408-147">Razorpage</span></span>
* <span data-ttu-id="eb408-148">Widok</span><span class="sxs-lookup"><span data-stu-id="eb408-148">View</span></span>

<a name="area"></a>

### <a name="area-options"></a><span data-ttu-id="eb408-149">Opcji obszaru</span><span class="sxs-lookup"><span data-stu-id="eb408-149">Area options</span></span>

<span data-ttu-id="eb408-150">To narzędzie jest przeznaczony dla projektów sieci web platformy ASP.NET Core za pomocą kontrolerów i widoków.</span><span class="sxs-lookup"><span data-stu-id="eb408-150">This tool is intended for ASP.NET Core web projects with controllers and views.</span></span> <span data-ttu-id="eb408-151">Nie jest przeznaczona dla aplikacji stron Razor.</span><span class="sxs-lookup"><span data-stu-id="eb408-151">It's not intended for Razor Pages apps.</span></span>

<span data-ttu-id="eb408-152">Użycie: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span><span class="sxs-lookup"><span data-stu-id="eb408-152">Usage: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span></span>

<span data-ttu-id="eb408-153">Poprzednie polecenie generuje następujące foldery:</span><span class="sxs-lookup"><span data-stu-id="eb408-153">The preceding command generates the following folders:</span></span>

* <span data-ttu-id="eb408-154">*Obszary*</span><span class="sxs-lookup"><span data-stu-id="eb408-154">*Areas*</span></span>
  * <span data-ttu-id="eb408-155">*AreaNameToGenerate*</span><span class="sxs-lookup"><span data-stu-id="eb408-155">*AreaNameToGenerate*</span></span>
    * <span data-ttu-id="eb408-156">*Kontrolery*</span><span class="sxs-lookup"><span data-stu-id="eb408-156">*Controllers*</span></span>
    * <span data-ttu-id="eb408-157">*Dane*</span><span class="sxs-lookup"><span data-stu-id="eb408-157">*Data*</span></span>
    * <span data-ttu-id="eb408-158">*Modele*</span><span class="sxs-lookup"><span data-stu-id="eb408-158">*Models*</span></span>
    * <span data-ttu-id="eb408-159">*Widoki*</span><span class="sxs-lookup"><span data-stu-id="eb408-159">*Views*</span></span>

<a name="ctl"></a>

### <a name="controller-options"></a><span data-ttu-id="eb408-160">Opcje kontrolera</span><span class="sxs-lookup"><span data-stu-id="eb408-160">Controller options</span></span>

<span data-ttu-id="eb408-161">W poniższej tabeli wymieniono opcje `aspnet-codegenerator` `controller` i `razorpage`:</span><span class="sxs-lookup"><span data-stu-id="eb408-161">The following table lists options for  `aspnet-codegenerator` `controller` and `razorpage`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="eb408-162">W poniższej tabeli wymieniono opcje unikatowe dla `aspnet-codegenerator controller`:</span><span class="sxs-lookup"><span data-stu-id="eb408-162">The following table lists options unique to  `aspnet-codegenerator controller`:</span></span>

| <span data-ttu-id="eb408-163">Opcja</span><span class="sxs-lookup"><span data-stu-id="eb408-163">Option</span></span>               | <span data-ttu-id="eb408-164">Opis</span><span class="sxs-lookup"><span data-stu-id="eb408-164">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="eb408-165">Element controllerName — lub — nazwa</span><span class="sxs-lookup"><span data-stu-id="eb408-165">--controllerName or -name</span></span> | <span data-ttu-id="eb408-166">Nazwa kontrolera.</span><span class="sxs-lookup"><span data-stu-id="eb408-166">Name of the controller.</span></span> |
| <span data-ttu-id="eb408-167">--useAsyncActions lub - async</span><span class="sxs-lookup"><span data-stu-id="eb408-167">--useAsyncActions or -async</span></span> | <span data-ttu-id="eb408-168">Generowanie asynchronicznych akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="eb408-168">Generate async controller actions.</span></span> |
| <span data-ttu-id="eb408-169">--noViews lub -nv</span><span class="sxs-lookup"><span data-stu-id="eb408-169">--noViews or -nv</span></span> | <span data-ttu-id="eb408-170">Generowanie **nie** widoków.</span><span class="sxs-lookup"><span data-stu-id="eb408-170">Generate **no** views.</span></span> |
| <span data-ttu-id="eb408-171">--restWithNoViews lub - interfejsu api</span><span class="sxs-lookup"><span data-stu-id="eb408-171">--restWithNoViews or -api</span></span>  | <span data-ttu-id="eb408-172">Generowanie kontrolera ze stylem REST API.</span><span class="sxs-lookup"><span data-stu-id="eb408-172">Generate a Controller with REST style API.</span></span> <span data-ttu-id="eb408-173">`noViews` przyjęto, że i dowolne Wyświetl powiązane opcje są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="eb408-173">`noViews` is assumed and any view related options are ignored.</span></span> |
| <span data-ttu-id="eb408-174">--readWriteActions lub - akcje</span><span class="sxs-lookup"><span data-stu-id="eb408-174">--readWriteActions or -actions</span></span> | <span data-ttu-id="eb408-175">Generowanie kontroler z akcjami odczytu/zapisu bez modelu.</span><span class="sxs-lookup"><span data-stu-id="eb408-175">Generate controller with read/write actions without a model.</span></span> |

<span data-ttu-id="eb408-176">Użyj `-h` przełączać w celu uzyskania pomocy na `aspnet-codegenerator controller` polecenia:</span><span class="sxs-lookup"><span data-stu-id="eb408-176">Use the `-h` switch for help on the `aspnet-codegenerator controller` command:</span></span>

```console
dotnet aspnet-codegenerator controller -h
```

<span data-ttu-id="eb408-177">Zobacz [tworzenia szkieletu modelu movie](/aspnet/core/tutorials/razor-pages/model) przykład `dotnet aspnet-codegenerator controller`.</span><span class="sxs-lookup"><span data-stu-id="eb408-177">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator controller`.</span></span>

### <a name="razorpage"></a><span data-ttu-id="eb408-178">Razorpage</span><span class="sxs-lookup"><span data-stu-id="eb408-178">Razorpage</span></span>

<a name="rp"></a>

<span data-ttu-id="eb408-179">Strony razor można indywidualnie szkieletu, określając nazwę nowej strony i szablon do użycia.</span><span class="sxs-lookup"><span data-stu-id="eb408-179">Razor Pages can be individually scaffolded by specifying the name of the new page and the template to use.</span></span> <span data-ttu-id="eb408-180">Są obsługiwane szablony:</span><span class="sxs-lookup"><span data-stu-id="eb408-180">The supported templates are:</span></span>

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="eb408-181">Na przykład następujące polecenie używa Edytuj szablon do generowania *MyEdit.cshtml* i *MyEdit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="eb408-181">For example, the following command uses the Edit template to generate *MyEdit.cshtml* and *MyEdit.cshtml.cs*:</span></span>

```console
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

<span data-ttu-id="eb408-182">Typowo, szablon i wygenerowanej nazwy pliku nie zostanie określony, i są tworzone następujące szablony:</span><span class="sxs-lookup"><span data-stu-id="eb408-182">Typically, the template and generated file name is not specified, and the following templates are created:</span></span>

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="eb408-183">W poniższej tabeli wymieniono opcje `aspnet-codegenerator` `razorpage` i `controller`:</span><span class="sxs-lookup"><span data-stu-id="eb408-183">The following table lists options for  `aspnet-codegenerator` `razorpage` and `controller`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="eb408-184">W poniższej tabeli wymieniono opcje unikatowe dla `aspnet-codegenerator razorpage`:</span><span class="sxs-lookup"><span data-stu-id="eb408-184">The following table lists options unique to  `aspnet-codegenerator razorpage`:</span></span>

| <span data-ttu-id="eb408-185">Opcja</span><span class="sxs-lookup"><span data-stu-id="eb408-185">Option</span></span>               | <span data-ttu-id="eb408-186">Opis</span><span class="sxs-lookup"><span data-stu-id="eb408-186">Description</span></span>|
| ----------------- | ------------ |
|   <span data-ttu-id="eb408-187">--namespaceName lub — przestrzeń nazw</span><span class="sxs-lookup"><span data-stu-id="eb408-187">--namespaceName or -namespace</span></span> | <span data-ttu-id="eb408-188">Nazwa przestrzeni nazw do użycia dla wygenerowanej klasy PageModel</span><span class="sxs-lookup"><span data-stu-id="eb408-188">The name of the namespace to use for the generated PageModel</span></span> |
| <span data-ttu-id="eb408-189">--partialView ani - częściowy</span><span class="sxs-lookup"><span data-stu-id="eb408-189">--partialView or -partial</span></span> | <span data-ttu-id="eb408-190">Generowanie widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="eb408-190">Generate a partial view.</span></span> <span data-ttu-id="eb408-191">Układ opcji -l i - udl są ignorowane, jeśli jest określony.</span><span class="sxs-lookup"><span data-stu-id="eb408-191">Layout options -l and -udl are ignored if this is specified.</span></span> |
| <span data-ttu-id="eb408-192">--noPageModel lub - npm</span><span class="sxs-lookup"><span data-stu-id="eb408-192">--noPageModel or -npm</span></span> | <span data-ttu-id="eb408-193">Przełącz na nie Generuj klasę PageModel w przypadku pustego szablonu</span><span class="sxs-lookup"><span data-stu-id="eb408-193">Switch to not generate a PageModel class for Empty template</span></span> |

<span data-ttu-id="eb408-194">Użyj `-h` przełączać w celu uzyskania pomocy na `aspnet-codegenerator razorpage` polecenia:</span><span class="sxs-lookup"><span data-stu-id="eb408-194">Use the `-h` switch for help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```

<span data-ttu-id="eb408-195">Zobacz [tworzenia szkieletu modelu movie](/aspnet/core/tutorials/razor-pages/model) przykład `dotnet aspnet-codegenerator razorpage`.</span><span class="sxs-lookup"><span data-stu-id="eb408-195">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator razorpage`.</span></span>

### <a name="identity"></a><span data-ttu-id="eb408-196">Tożsamość</span><span class="sxs-lookup"><span data-stu-id="eb408-196">Identity</span></span>

<span data-ttu-id="eb408-197">Zobacz [tworzenia szkieletu tożsamości](/aspnet/core/security/authentication/scaffold-identity)</span><span class="sxs-lookup"><span data-stu-id="eb408-197">See [Scaffold Identity](/aspnet/core/security/authentication/scaffold-identity)</span></span>