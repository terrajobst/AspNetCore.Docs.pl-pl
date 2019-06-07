---
title: Platforma ASP.NET Core Razor SDK
author: Rick-Anderson
description: Dowiedz się, jak stron Razor w programie ASP.NET Core umożliwia kodowania scenariuszy skoncentrowane na stronie łatwiejsze i bardziej wydajne niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/05/2019
uid: razor-pages/sdk
ms.openlocfilehash: 8c4e882af93b043afaa0bcf86fd1583405f84be9
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750177"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="3e71a-103">Platforma ASP.NET Core Razor SDK</span><span class="sxs-lookup"><span data-stu-id="3e71a-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="3e71a-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3e71a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3e71a-105">[!INCLUDE[](~/includes/2.1-SDK.md)] Obejmuje `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span><span class="sxs-lookup"><span data-stu-id="3e71a-105">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="3e71a-106">Zestaw Razor SDK:</span><span class="sxs-lookup"><span data-stu-id="3e71a-106">The Razor SDK:</span></span>

* <span data-ttu-id="3e71a-107">Standaryzuje środowisko wokół tworzenia, pakowania i publikowania projektów zawierających [Razor](xref:mvc/views/razor) plików dla projektów ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="3e71a-107">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="3e71a-108">Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementy, które umożliwiają dostosowanie kompilacji w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-108">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e71a-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3e71a-109">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a><span data-ttu-id="3e71a-110">Przy użyciu zestawu SDK elementu Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-110">Using the Razor SDK</span></span>

<span data-ttu-id="3e71a-111">Większość aplikacji sieci web nie są wymagane, aby jawnie odwołać się do zestawu SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-111">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

<span data-ttu-id="3e71a-112">Aby użyć zestaw Razor SDK do tworzenia biblioteki klas zawierający widokami Razor lub stron Razor:</span><span class="sxs-lookup"><span data-stu-id="3e71a-112">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="3e71a-113">Użyj `Microsoft.NET.Sdk.Razor` zamiast `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="3e71a-113">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    ...
  </Project>
  ```

* <span data-ttu-id="3e71a-114">Zazwyczaj odwołania do pakietu do `Microsoft.AspNetCore.Mvc` jest wymagana, aby otrzymać dodatkowe zależności, które są wymagane do kompilowania i kompilowania stron Razor i widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-114">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="3e71a-115">Co najmniej projektu należy dodać odwołania do pakietu do:</span><span class="sxs-lookup"><span data-stu-id="3e71a-115">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="3e71a-116">`Microsoft.AspNetCore.Razor.Design` Pakiet zawiera cele i zadania kompilacji Razor do projektu.</span><span class="sxs-lookup"><span data-stu-id="3e71a-116">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="3e71a-117">Poprzedni pakiety znajdują się w `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="3e71a-117">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="3e71a-118">Następujący kod przedstawia plik projektu, który używa zestaw Razor SDK do tworzenia plików Razor aplikacji ASP.NET Core Razor strony:</span><span class="sxs-lookup"><span data-stu-id="3e71a-118">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="3e71a-119">`Microsoft.AspNetCore.Razor.Design` i `Microsoft.AspNetCore.Mvc.Razor.Extensions` pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="3e71a-119">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="3e71a-120">Jednak bez wersji `Microsoft.AspNetCore.App` odwołania do pakietu zawiera meta Microsoft.aspnetcore.all, aplikacja która nie zawiera najnowszą wersję `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="3e71a-120">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="3e71a-121">Projekty muszą odwoływać się do wersję spójną `Microsoft.AspNetCore.Razor.Design` (lub `Microsoft.AspNetCore.Mvc`) tak, że uwzględniono najnowszych poprawek czas kompilacji dla elementu Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-121">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="3e71a-122">Aby uzyskać więcej informacji, zobacz [problem w usłudze GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="3e71a-122">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="3e71a-123">Właściwości</span><span class="sxs-lookup"><span data-stu-id="3e71a-123">Properties</span></span>

<span data-ttu-id="3e71a-124">Następujące właściwości kontrolować zachowanie zestawu SDK Razor jako część kompilacji projektu:</span><span class="sxs-lookup"><span data-stu-id="3e71a-124">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="3e71a-125">`RazorCompileOnBuild` &ndash; Gdy `true`, kompiluje i emituje zestaw Razor, w ramach tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="3e71a-125">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="3e71a-126">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="3e71a-126">Defaults to `true`.</span></span>
* <span data-ttu-id="3e71a-127">`RazorCompileOnPublish` &ndash; Gdy `true`, kompiluje i emituje zestaw Razor, w ramach publikowania projektu.</span><span class="sxs-lookup"><span data-stu-id="3e71a-127">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="3e71a-128">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="3e71a-128">Defaults to `true`.</span></span>

<span data-ttu-id="3e71a-129">Właściwości i elementy w poniższej tabeli są używane do konfigurowania danych wejściowych i wyjściowych zestawu SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-129">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

| <span data-ttu-id="3e71a-130">Elementy</span><span class="sxs-lookup"><span data-stu-id="3e71a-130">Items</span></span> | <span data-ttu-id="3e71a-131">Opis</span><span class="sxs-lookup"><span data-stu-id="3e71a-131">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="3e71a-132">Element elementów ( *.cshtml* plików), które są dane wejściowe do celów generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="3e71a-132">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| `RazorCompile` | <span data-ttu-id="3e71a-133">Element elementów ( *.cs* plików), które są dane wejściowe do celów kompilacji Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-133">Item elements (*.cs* files) that are inputs to  Razor compilation targets.</span></span> <span data-ttu-id="3e71a-134">Użyj tego ItemGroup, aby określić dodatkowe pliki do skompilowany w zestawie Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-134">Use this ItemGroup to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="3e71a-135">Element służącego do kodu generowania atrybuty dla zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-135">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="3e71a-136">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3e71a-136">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="3e71a-137">Elementy dodane do wygenerowanego zestawu Razor jako zasoby osadzone.</span><span class="sxs-lookup"><span data-stu-id="3e71a-137">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="3e71a-138">Właściwość</span><span class="sxs-lookup"><span data-stu-id="3e71a-138">Property</span></span> | <span data-ttu-id="3e71a-139">Opis</span><span class="sxs-lookup"><span data-stu-id="3e71a-139">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="3e71a-140">Nazwa pliku (bez rozszerzenia) zestaw utworzony przez Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-140">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="3e71a-141">Katalog wyjściowy Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-141">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="3e71a-142">Używany do określenia zestawu narzędzi, używany do tworzenia zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-142">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="3e71a-143">Prawidłowe wartości to `Implicit`, `RazorSDK`, i `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="3e71a-143">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="3e71a-144">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="3e71a-144">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="3e71a-145">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="3e71a-145">Default is `true`.</span></span> <span data-ttu-id="3e71a-146">Gdy `true`, obejmuje *web.config*, *.json*, i *.cshtml* pliki zawartości w projekcie.</span><span class="sxs-lookup"><span data-stu-id="3e71a-146">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="3e71a-147">Gdy przywoływana za pomocą `Microsoft.NET.Sdk.Web`, plików w obszarze *wwwroot* i dołączane są także pliki konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3e71a-147">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="3e71a-148">Gdy `true`, obejmuje *.cshtml* plików ze `Content` elementy w `RazorGenerate` elementów.</span><span class="sxs-lookup"><span data-stu-id="3e71a-148">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="3e71a-149">Gdy `true`, generuje *.cs* plik zawierający atrybuty określone przez `RazorAssemblyAttribute` włącznie z plikiem w danych wyjściowych kompilacji.</span><span class="sxs-lookup"><span data-stu-id="3e71a-149">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="3e71a-150">Gdy `true`, dodaje domyślny zestaw atrybutów zestawu do `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="3e71a-150">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="3e71a-151">Gdy `true`, kopie `RazorGenerate` elementów ( *.cshtml*) pliki do katalogu publikowania.</span><span class="sxs-lookup"><span data-stu-id="3e71a-151">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="3e71a-152">Zazwyczaj pliki Razor nie są wymagane dla opublikowanej aplikacji, jeśli uczestniczą w kompilacji w czasie kompilacji lub czasu publikowania.</span><span class="sxs-lookup"><span data-stu-id="3e71a-152">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="3e71a-153">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="3e71a-153">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="3e71a-154">Gdy `true`, skopiuj elementy zestawu odwołania do katalogu publikowania.</span><span class="sxs-lookup"><span data-stu-id="3e71a-154">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="3e71a-155">Zazwyczaj zestawy odwołań nie są wymagane dla opublikowanej aplikacji, jeśli kompilacja Razor występuje w czasie kompilacji lub czasu publikowania.</span><span class="sxs-lookup"><span data-stu-id="3e71a-155">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="3e71a-156">Ustaw `true` Jeśli Twojej opublikowanej aplikacji wymaga kompilacja środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="3e71a-156">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="3e71a-157">Na przykład, ustaw wartość `true` Jeśli aplikacja modyfikuje *.cshtml* plików w czasie wykonywania lub korzysta z osadzonych widoków.</span><span class="sxs-lookup"><span data-stu-id="3e71a-157">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="3e71a-158">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="3e71a-158">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="3e71a-159">Gdy `true`, wszystkie elementy zawartości Razor ( *.cshtml* pliki) są oznaczone do włączenia do wygenerowanego pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="3e71a-159">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="3e71a-160">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="3e71a-160">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="3e71a-161">Gdy `true`, dodaje RazorGenerate ( *.cshtml*) elementów jako osadzone pliki do wygenerowanego zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-161">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="3e71a-162">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="3e71a-162">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="3e71a-163">Gdy `true`, używa procesu serwera kompilacji trwałego odciążania roboczego generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="3e71a-163">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="3e71a-164">Wartością domyślną jest wartość `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="3e71a-164">Defaults to the value of `UseSharedCompilation`.</span></span> |

<span data-ttu-id="3e71a-165">Aby uzyskać więcej informacji na temat właściwości, zobacz [właściwości programu MSBuild](/visualstudio/msbuild/msbuild-properties).</span><span class="sxs-lookup"><span data-stu-id="3e71a-165">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="3e71a-166">Obiekty docelowe</span><span class="sxs-lookup"><span data-stu-id="3e71a-166">Targets</span></span>

<span data-ttu-id="3e71a-167">Zestaw Razor SDK definiuje dwa podstawowe cele:</span><span class="sxs-lookup"><span data-stu-id="3e71a-167">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="3e71a-168">`RazorGenerate` &ndash; Generuje kod *.cs* plików ze `RazorGenerate` elementu elementów.</span><span class="sxs-lookup"><span data-stu-id="3e71a-168">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="3e71a-169">Użyj `RazorGenerateDependsOn` właściwości w celu określenia dodatkowych jest przeznaczony dla, uruchomić przed lub po ten element docelowy.</span><span class="sxs-lookup"><span data-stu-id="3e71a-169">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="3e71a-170">`RazorCompile` &ndash; Kompiluje generowane *.cs* pliki do zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="3e71a-170">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="3e71a-171">Użyj `RazorCompileDependsOn` Aby określić dodatkowe obiekty docelowe, które można uruchomić przed lub po ten element docelowy.</span><span class="sxs-lookup"><span data-stu-id="3e71a-171">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="3e71a-172">Kompilacja środowiska uruchomieniowego z widokami Razor</span><span class="sxs-lookup"><span data-stu-id="3e71a-172">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="3e71a-173">Domyślnie zestaw Razor SDK nie publikować zestawy odwołań, które są wymagane do przeprowadzenia kompilacja środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="3e71a-173">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="3e71a-174">Skutkuje to błędy kompilacji podczas model aplikacji zależy od kompilacja środowiska uruchomieniowego&mdash;na przykład aplikacja używa osadzonych widoków widoków lub zmiany po opublikowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3e71a-174">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="3e71a-175">Ustaw `CopyRefAssembliesToPublishDirectory` do `true` aby kontynuować publikowanie zestawów odwołań.</span><span class="sxs-lookup"><span data-stu-id="3e71a-175">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="3e71a-176">Dla aplikacji sieci web, upewnij się, jest przeznaczona aplikacja `Microsoft.NET.Sdk.Web` zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="3e71a-176">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="3e71a-177">Wersja języka razor</span><span class="sxs-lookup"><span data-stu-id="3e71a-177">Razor language version</span></span>

<span data-ttu-id="3e71a-178">Podczas określania wartości `Microsoft.NET.Sdk.Web` zestawu SDK w wersji językowej Razor wynika z wersji platformy docelowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3e71a-178">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="3e71a-179">Dla projektów przeznaczonych dla `Microsoft.NET.Sdk.Razor` zestawu SDK lub w rzadkich przypadkach, że aplikacja wymaga innej wersji języka Razor niż wartość wywnioskowane wersji mogą być konfigurowane przez ustawienie `<RazorLangVersion>` właściwość w pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3e71a-179">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```
