---
title: dotnet ASPNET-CodeGenerator polecenie
author: rick-anderson
description: ASP.NET Core projekty są szkieletami poleceń dotnet ASPNET-CodeGenerator.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/04/2019
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: c2c815735ad1b4dcec761b26ea3992a4effebe62
ms.sourcegitcommit: 979dbfc5e9ce09b9470789989cddfcfb57079d94
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682690"
---
# <a name="dotnet-aspnet-codegenerator"></a><span data-ttu-id="0635f-103">dotnet ASPNET-CodeGenerator</span><span class="sxs-lookup"><span data-stu-id="0635f-103">dotnet aspnet-codegenerator</span></span>

<span data-ttu-id="0635f-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0635f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0635f-105">`dotnet aspnet-codegenerator`-Uruchamia aparat tworzenia szkieletów ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0635f-105">`dotnet aspnet-codegenerator` - Runs the ASP.NET Core scaffolding engine.</span></span> <span data-ttu-id="0635f-106">`dotnet aspnet-codegenerator`jest wymagany tylko dla szkieletu z wiersza polecenia, nie jest potrzebny do korzystania z szkieletów w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0635f-106">`dotnet aspnet-codegenerator` is only required to scaffold from the command line, it's not needed to use scaffolding with Visual Studio.</span></span>

<span data-ttu-id="0635f-107">Ten artykuł dotyczy [zestawu .NET Core 2,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1) i nowszych wersji.</span><span class="sxs-lookup"><span data-stu-id="0635f-107">This article applies to [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1) and later.</span></span>

## <a name="installing-aspnet-codegenerator"></a><span data-ttu-id="0635f-108">Instalowanie ASPNET-CodeGenerator</span><span class="sxs-lookup"><span data-stu-id="0635f-108">Installing aspnet-codegenerator</span></span>

<span data-ttu-id="0635f-109">`dotnet-aspnet-codegenerator`jest [globalnym narzędziem](/dotnet/core/tools/global-tools) , które musi być zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="0635f-109">`dotnet-aspnet-codegenerator` is a [global tool](/dotnet/core/tools/global-tools) that must be installed.</span></span> <span data-ttu-id="0635f-110">Następujące polecenie instaluje najnowszą stabilną wersję `dotnet-aspnet-codegenerator` narzędzia:</span><span class="sxs-lookup"><span data-stu-id="0635f-110">The following command installs the latest stable version of the `dotnet-aspnet-codegenerator` tool:</span></span>

```console
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="0635f-111">Następujące polecenie aktualizuje `dotnet-aspnet-codegenerator` najnowszą stabilną wersję dostępną z zainstalowanych zestawów SDK platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="0635f-111">The following command updates `dotnet-aspnet-codegenerator` to the latest stable version available from the installed .NET Core SDKs:</span></span>

```console
dotnet tool update -g dotnet-aspnet-codegenerator
```

## <a name="synopsis"></a><span data-ttu-id="0635f-112">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="0635f-112">Synopsis</span></span>

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a><span data-ttu-id="0635f-113">Opis</span><span class="sxs-lookup"><span data-stu-id="0635f-113">Description</span></span>

<span data-ttu-id="0635f-114">Polecenie `dotnet aspnet-codegenerator` globalne uruchamia ASP.NET Core generatora kodu i aparacie tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="0635f-114">The `dotnet aspnet-codegenerator` global command runs the ASP.NET Core code generator and scaffolding engine.</span></span>

## <a name="arguments"></a><span data-ttu-id="0635f-115">Argumenty</span><span class="sxs-lookup"><span data-stu-id="0635f-115">Arguments</span></span>

`generator`

<span data-ttu-id="0635f-116">Generator kodu do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="0635f-116">The code generator to run.</span></span> <span data-ttu-id="0635f-117">Dostępne są następujące generatory:</span><span class="sxs-lookup"><span data-stu-id="0635f-117">The following generators are available:</span></span>

| <span data-ttu-id="0635f-118">Generator</span><span class="sxs-lookup"><span data-stu-id="0635f-118">Generator</span></span> | <span data-ttu-id="0635f-119">Operacja</span><span class="sxs-lookup"><span data-stu-id="0635f-119">Operation</span></span> |
| ----------------- | ------------ | 
| <span data-ttu-id="0635f-120">Obszar</span><span class="sxs-lookup"><span data-stu-id="0635f-120">area</span></span>      | [<span data-ttu-id="0635f-121">Szkieletuje obszar</span><span class="sxs-lookup"><span data-stu-id="0635f-121">Scaffolds an Area</span></span>](/aspnet/core/mvc/controllers/areas) |
  <span data-ttu-id="0635f-122">kontroler</span><span class="sxs-lookup"><span data-stu-id="0635f-122">controller</span></span>| [<span data-ttu-id="0635f-123">Tworzy szkielety kontrolera</span><span class="sxs-lookup"><span data-stu-id="0635f-123">Scaffolds a controller</span></span>](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  <span data-ttu-id="0635f-124">tożsamość</span><span class="sxs-lookup"><span data-stu-id="0635f-124">identity</span></span>  | [<span data-ttu-id="0635f-125">Tożsamość szkieletów</span><span class="sxs-lookup"><span data-stu-id="0635f-125">Scaffolds Identity</span></span>](/aspnet/core/security/authentication/scaffold-identity) |
  <span data-ttu-id="0635f-126">razorpage</span><span class="sxs-lookup"><span data-stu-id="0635f-126">razorpage</span></span> | [<span data-ttu-id="0635f-127">Razor Pages szkieletów</span><span class="sxs-lookup"><span data-stu-id="0635f-127">Scaffolds Razor Pages</span></span>](/aspnet/core/tutorials/razor-pages/model) |
  <span data-ttu-id="0635f-128">widokiem</span><span class="sxs-lookup"><span data-stu-id="0635f-128">view</span></span>      | [<span data-ttu-id="0635f-129">Tworzy szkielety widoku</span><span class="sxs-lookup"><span data-stu-id="0635f-129">Scaffolds a view</span></span>](/aspnet/core/mvc/views/overview) |

## <a name="options"></a><span data-ttu-id="0635f-130">Opcje</span><span class="sxs-lookup"><span data-stu-id="0635f-130">Options</span></span>

`-n|--nuget-package-dir`

<span data-ttu-id="0635f-131">Określa katalog pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="0635f-131">Specifies the NuGet package directory.</span></span>

`-c|--configuration {Debug|Release}`

<span data-ttu-id="0635f-132">Definiuje konfigurację kompilacji.</span><span class="sxs-lookup"><span data-stu-id="0635f-132">Defines the build configuration.</span></span> <span data-ttu-id="0635f-133">Wartość domyślna to `Debug`.</span><span class="sxs-lookup"><span data-stu-id="0635f-133">The default value is `Debug`.</span></span>

`-tfm|--target-framework`

<span data-ttu-id="0635f-134">[Platforma](/dotnet/standard/frameworks) docelowa do użycia.</span><span class="sxs-lookup"><span data-stu-id="0635f-134">Target [Framework](/dotnet/standard/frameworks) to use.</span></span> <span data-ttu-id="0635f-135">Na przykład `net46`.</span><span class="sxs-lookup"><span data-stu-id="0635f-135">For example, `net46`.</span></span>

`-b|--build-base-path`

<span data-ttu-id="0635f-136">Ścieżka bazowa kompilacji.</span><span class="sxs-lookup"><span data-stu-id="0635f-136">The build base path.</span></span>

`-h|--help`

<span data-ttu-id="0635f-137">Drukuje krótką pomoc dla polecenia.</span><span class="sxs-lookup"><span data-stu-id="0635f-137">Prints out a short help for the command.</span></span>

`--no-build`

<span data-ttu-id="0635f-138">Nie kompiluje projektu przed uruchomieniem.</span><span class="sxs-lookup"><span data-stu-id="0635f-138">Doesn't build the project before running.</span></span> <span data-ttu-id="0635f-139">Również niejawnie ustawia `--no-restore` flagę.</span><span class="sxs-lookup"><span data-stu-id="0635f-139">It also implicitly sets the `--no-restore` flag.</span></span>

`-p|--project <PATH>`

<span data-ttu-id="0635f-140">Określa ścieżkę do pliku projektu do uruchomienia (nazwa folderu lub pełna ścieżka).</span><span class="sxs-lookup"><span data-stu-id="0635f-140">Specifies the path of the project file to run (folder name or full path).</span></span> <span data-ttu-id="0635f-141">Jeśli nie zostanie określony, domyślnie jest bieżącym katalogiem.</span><span class="sxs-lookup"><span data-stu-id="0635f-141">If not specified, it defaults to the current directory.</span></span>

## <a name="generator-options"></a><span data-ttu-id="0635f-142">Opcje generatora</span><span class="sxs-lookup"><span data-stu-id="0635f-142">Generator options</span></span>

<span data-ttu-id="0635f-143">Poniższe sekcje zawierają szczegółowe informacje o opcjach dostępnych dla obsługiwanych generatorów:</span><span class="sxs-lookup"><span data-stu-id="0635f-143">The following sections detail the options available for the supported generators:</span></span>

* <span data-ttu-id="0635f-144">Obszar</span><span class="sxs-lookup"><span data-stu-id="0635f-144">Area</span></span>
* <span data-ttu-id="0635f-145">Kontroler</span><span class="sxs-lookup"><span data-stu-id="0635f-145">Controller</span></span>
* <span data-ttu-id="0635f-146">Tożsamość</span><span class="sxs-lookup"><span data-stu-id="0635f-146">Identity</span></span>  
* <span data-ttu-id="0635f-147">Razorpage</span><span class="sxs-lookup"><span data-stu-id="0635f-147">Razorpage</span></span>
* <span data-ttu-id="0635f-148">Widok</span><span class="sxs-lookup"><span data-stu-id="0635f-148">View</span></span>

<a name="area"></a>

### <a name="area-options"></a><span data-ttu-id="0635f-149">Opcje obszaru</span><span class="sxs-lookup"><span data-stu-id="0635f-149">Area options</span></span>

<span data-ttu-id="0635f-150">To narzędzie jest przeznaczone do ASP.NET Core projektów sieci Web z kontrolerami i widokami.</span><span class="sxs-lookup"><span data-stu-id="0635f-150">This tool is intended for ASP.NET Core web projects with controllers and views.</span></span> <span data-ttu-id="0635f-151">Nie jest ona przeznaczona do Razor Pages aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0635f-151">It's not intended for Razor Pages apps.</span></span>

<span data-ttu-id="0635f-152">Użycie: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span><span class="sxs-lookup"><span data-stu-id="0635f-152">Usage: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span></span>

<span data-ttu-id="0635f-153">Poprzednie polecenie generuje następujące foldery:</span><span class="sxs-lookup"><span data-stu-id="0635f-153">The preceding command generates the following folders:</span></span>

* <span data-ttu-id="0635f-154">*Obszary*</span><span class="sxs-lookup"><span data-stu-id="0635f-154">*Areas*</span></span>
  * <span data-ttu-id="0635f-155">*AreaNameToGenerate*</span><span class="sxs-lookup"><span data-stu-id="0635f-155">*AreaNameToGenerate*</span></span>
    * <span data-ttu-id="0635f-156">*Kontrolery*</span><span class="sxs-lookup"><span data-stu-id="0635f-156">*Controllers*</span></span>
    * <span data-ttu-id="0635f-157">*Dane*</span><span class="sxs-lookup"><span data-stu-id="0635f-157">*Data*</span></span>
    * <span data-ttu-id="0635f-158">*Przykładów*</span><span class="sxs-lookup"><span data-stu-id="0635f-158">*Models*</span></span>
    * <span data-ttu-id="0635f-159">*Widoki*</span><span class="sxs-lookup"><span data-stu-id="0635f-159">*Views*</span></span>

<a name="ctl"></a>

### <a name="controller-options"></a><span data-ttu-id="0635f-160">Opcje kontrolera</span><span class="sxs-lookup"><span data-stu-id="0635f-160">Controller options</span></span>

<span data-ttu-id="0635f-161">W poniższej tabeli wymieniono opcje `aspnet-codegenerator` `controller` i `razorpage`:</span><span class="sxs-lookup"><span data-stu-id="0635f-161">The following table lists options for  `aspnet-codegenerator` `controller` and `razorpage`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="0635f-162">W poniższej tabeli wymieniono opcje unikatowe `aspnet-codegenerator controller`dla:</span><span class="sxs-lookup"><span data-stu-id="0635f-162">The following table lists options unique to  `aspnet-codegenerator controller`:</span></span>

| <span data-ttu-id="0635f-163">Opcja</span><span class="sxs-lookup"><span data-stu-id="0635f-163">Option</span></span>               | <span data-ttu-id="0635f-164">Opis</span><span class="sxs-lookup"><span data-stu-id="0635f-164">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="0635f-165">--ControllerName lub-Name</span><span class="sxs-lookup"><span data-stu-id="0635f-165">--controllerName or -name</span></span> | <span data-ttu-id="0635f-166">Nazwa kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0635f-166">Name of the controller.</span></span> |
| <span data-ttu-id="0635f-167">--useAsyncActions lub-async</span><span class="sxs-lookup"><span data-stu-id="0635f-167">--useAsyncActions or -async</span></span> | <span data-ttu-id="0635f-168">Generuj akcje kontrolerów asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="0635f-168">Generate async controller actions.</span></span> |
| <span data-ttu-id="0635f-169">--noviews lub NV</span><span class="sxs-lookup"><span data-stu-id="0635f-169">--noViews or -nv</span></span> | <span data-ttu-id="0635f-170">**Nie Generuj żadnych** widoków.</span><span class="sxs-lookup"><span data-stu-id="0635f-170">Generate **no** views.</span></span> |
| <span data-ttu-id="0635f-171">--restWithNoViews lub-API</span><span class="sxs-lookup"><span data-stu-id="0635f-171">--restWithNoViews or -api</span></span>  | <span data-ttu-id="0635f-172">Wygeneruj kontroler z interfejsem API REST.</span><span class="sxs-lookup"><span data-stu-id="0635f-172">Generate a Controller with REST style API.</span></span> <span data-ttu-id="0635f-173">`noViews`Założono, że wszystkie opcje powiązane z widokiem są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="0635f-173">`noViews` is assumed and any view related options are ignored.</span></span> |
| <span data-ttu-id="0635f-174">--readWriteActions lub-akcje</span><span class="sxs-lookup"><span data-stu-id="0635f-174">--readWriteActions or -actions</span></span> | <span data-ttu-id="0635f-175">Generuj kontroler z akcjami odczytu/zapisu bez modelu.</span><span class="sxs-lookup"><span data-stu-id="0635f-175">Generate controller with read/write actions without a model.</span></span> |

<span data-ttu-id="0635f-176">Użyj przełącznika `-h` , aby uzyskać pomoc `aspnet-codegenerator controller` dotyczącą polecenia:</span><span class="sxs-lookup"><span data-stu-id="0635f-176">Use the `-h` switch for help on the `aspnet-codegenerator controller` command:</span></span>

```console
dotnet aspnet-codegenerator controller -h
```

<span data-ttu-id="0635f-177">Aby zapoznać się z `dotnet aspnet-codegenerator controller`przykładem, zobacz Tworzenie szkieletu [modelu filmu](/aspnet/core/tutorials/razor-pages/model) .</span><span class="sxs-lookup"><span data-stu-id="0635f-177">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator controller`.</span></span>

### <a name="razorpage"></a><span data-ttu-id="0635f-178">Razorpage</span><span class="sxs-lookup"><span data-stu-id="0635f-178">Razorpage</span></span>

<a name="rp"></a>

<span data-ttu-id="0635f-179">Razor Pages mogą być tworzone indywidualnie przez określenie nazwy nowej strony i szablonu do użycia.</span><span class="sxs-lookup"><span data-stu-id="0635f-179">Razor Pages can be individually scaffolded by specifying the name of the new page and the template to use.</span></span> <span data-ttu-id="0635f-180">Obsługiwane są następujące szablony:</span><span class="sxs-lookup"><span data-stu-id="0635f-180">The supported templates are:</span></span>

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="0635f-181">Na przykład następujące polecenie używa polecenia Edytuj szablon do wygenerowania elementu *MyEdit.cshtml.cs* *. cshtml* i:</span><span class="sxs-lookup"><span data-stu-id="0635f-181">For example, the following command uses the Edit template to generate *MyEdit.cshtml* and *MyEdit.cshtml.cs*:</span></span>

```console
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

<span data-ttu-id="0635f-182">Zazwyczaj nazwa szablonu i wygenerowanego pliku nie jest określona i tworzone są następujące szablony:</span><span class="sxs-lookup"><span data-stu-id="0635f-182">Typically, the template and generated file name is not specified, and the following templates are created:</span></span>

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="0635f-183">W poniższej tabeli wymieniono opcje `aspnet-codegenerator` `razorpage` i `controller`:</span><span class="sxs-lookup"><span data-stu-id="0635f-183">The following table lists options for  `aspnet-codegenerator` `razorpage` and `controller`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="0635f-184">W poniższej tabeli wymieniono opcje unikatowe `aspnet-codegenerator razorpage`dla:</span><span class="sxs-lookup"><span data-stu-id="0635f-184">The following table lists options unique to  `aspnet-codegenerator razorpage`:</span></span>

| <span data-ttu-id="0635f-185">Opcja</span><span class="sxs-lookup"><span data-stu-id="0635f-185">Option</span></span>               | <span data-ttu-id="0635f-186">Opis</span><span class="sxs-lookup"><span data-stu-id="0635f-186">Description</span></span>|
| ----------------- | ------------ |
|   <span data-ttu-id="0635f-187">--NamespaceName lub-Namespace</span><span class="sxs-lookup"><span data-stu-id="0635f-187">--namespaceName or -namespace</span></span> | <span data-ttu-id="0635f-188">Nazwa przestrzeni nazw do użycia dla wygenerowanego PageModel</span><span class="sxs-lookup"><span data-stu-id="0635f-188">The name of the namespace to use for the generated PageModel</span></span> |
| <span data-ttu-id="0635f-189">--partialView lub-częściowe</span><span class="sxs-lookup"><span data-stu-id="0635f-189">--partialView or -partial</span></span> | <span data-ttu-id="0635f-190">Generuj widok częściowy.</span><span class="sxs-lookup"><span data-stu-id="0635f-190">Generate a partial view.</span></span> <span data-ttu-id="0635f-191">Opcje układu-l i-UDL są ignorowane, jeśli jest określony.</span><span class="sxs-lookup"><span data-stu-id="0635f-191">Layout options -l and -udl are ignored if this is specified.</span></span> |
| <span data-ttu-id="0635f-192">--noPageModel lub-npm</span><span class="sxs-lookup"><span data-stu-id="0635f-192">--noPageModel or -npm</span></span> | <span data-ttu-id="0635f-193">Przełącz, aby nie generować klasy PageModel dla pustego szablonu</span><span class="sxs-lookup"><span data-stu-id="0635f-193">Switch to not generate a PageModel class for Empty template</span></span> |

<span data-ttu-id="0635f-194">Użyj przełącznika `-h` , aby uzyskać pomoc `aspnet-codegenerator razorpage` dotyczącą polecenia:</span><span class="sxs-lookup"><span data-stu-id="0635f-194">Use the `-h` switch for help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```

<span data-ttu-id="0635f-195">Aby zapoznać się z `dotnet aspnet-codegenerator razorpage`przykładem, zobacz Tworzenie szkieletu [modelu filmu](/aspnet/core/tutorials/razor-pages/model) .</span><span class="sxs-lookup"><span data-stu-id="0635f-195">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator razorpage`.</span></span>

### <a name="identity"></a><span data-ttu-id="0635f-196">Tożsamość</span><span class="sxs-lookup"><span data-stu-id="0635f-196">Identity</span></span>

<span data-ttu-id="0635f-197">Zobacz [tożsamość szkieletu](/aspnet/core/security/authentication/scaffold-identity)</span><span class="sxs-lookup"><span data-stu-id="0635f-197">See [Scaffold Identity](/aspnet/core/security/authentication/scaffold-identity)</span></span>
