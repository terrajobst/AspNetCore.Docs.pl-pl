---
title: Platforma ASP.NET Core Razor SDK
author: Rick-Anderson
description: Dowiedz się, jak stron Razor w programie ASP.NET Core umożliwia kodowania scenariuszy skoncentrowane na stronie łatwiejsze i bardziej wydajne niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/18/2019
uid: razor-pages/sdk
ms.openlocfilehash: fa69e4840377e0c1c8291c7ba9305a27bd3e6b82
ms.sourcegitcommit: 516f166c5f7cec54edf3d9c71e6e2ba53fb3b0e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196360"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="df49a-103">Platforma ASP.NET Core Razor SDK</span><span class="sxs-lookup"><span data-stu-id="df49a-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="df49a-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="df49a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="df49a-105">Omówienie</span><span class="sxs-lookup"><span data-stu-id="df49a-105">Overview</span></span>

<span data-ttu-id="df49a-106">[!INCLUDE[](~/includes/2.1-SDK.md)] Obejmuje `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span><span class="sxs-lookup"><span data-stu-id="df49a-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="df49a-107">Zestaw Razor SDK:</span><span class="sxs-lookup"><span data-stu-id="df49a-107">The Razor SDK:</span></span>

* <span data-ttu-id="df49a-108">Standaryzuje środowisko wokół tworzenia, pakowania i publikowania projektów zawierających [Razor](xref:mvc/views/razor) plików dla projektów ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="df49a-108">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="df49a-109">Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementy, które umożliwiają dostosowanie kompilacji w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="df49a-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="df49a-110">Zestaw Razor SDK zawiera `<Content>` element z `Include` ustawioną wartość atrybutu `**\*.cshtml` wzoru symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="df49a-110">The Razor SDK includes a `<Content>` element with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="df49a-111">Odpowiednie pliki są publikowane.</span><span class="sxs-lookup"><span data-stu-id="df49a-111">Matching files are published.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="df49a-112">Zestaw Razor SDK zawiera `<Content>` elementów za pomocą `Include` Ustaw atrybuty `**\*.cshtml` i `**\*.razor` wzorców obsługi symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="df49a-112">The Razor SDK includes `<Content>` elements with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="df49a-113">Odpowiednie pliki są publikowane.</span><span class="sxs-lookup"><span data-stu-id="df49a-113">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="df49a-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="df49a-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="df49a-115">Użyj elementu Razor SDK</span><span class="sxs-lookup"><span data-stu-id="df49a-115">Use the Razor SDK</span></span>

<span data-ttu-id="df49a-116">Większość aplikacji sieci web nie są wymagane, aby jawnie odwołać się do zestawu SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="df49a-116">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

<span data-ttu-id="df49a-117">Aby użyć zestaw Razor SDK do tworzenia biblioteki klas zawierający widokami Razor lub stron Razor:</span><span class="sxs-lookup"><span data-stu-id="df49a-117">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="df49a-118">Użyj `Microsoft.NET.Sdk.Razor` zamiast `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="df49a-118">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="df49a-119">Zazwyczaj odwołania do pakietu do `Microsoft.AspNetCore.Mvc` jest wymagana, aby otrzymać dodatkowe zależności, które są wymagane do kompilowania i kompilowania stron Razor i widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="df49a-119">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="df49a-120">Co najmniej projektu należy dodać odwołania do pakietu do:</span><span class="sxs-lookup"><span data-stu-id="df49a-120">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="df49a-121">`Microsoft.AspNetCore.Razor.Design` Pakiet zawiera cele i zadania kompilacji Razor do projektu.</span><span class="sxs-lookup"><span data-stu-id="df49a-121">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="df49a-122">Poprzedni pakiety znajdują się w `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="df49a-122">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="df49a-123">Następujący kod przedstawia plik projektu, który używa zestaw Razor SDK do tworzenia plików Razor aplikacji ASP.NET Core Razor strony:</span><span class="sxs-lookup"><span data-stu-id="df49a-123">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="df49a-124">`Microsoft.AspNetCore.Razor.Design` i `Microsoft.AspNetCore.Mvc.Razor.Extensions` pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="df49a-124">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="df49a-125">Jednak bez wersji `Microsoft.AspNetCore.App` odwołania do pakietu zawiera meta Microsoft.aspnetcore.all, aplikacja która nie zawiera najnowszą wersję `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="df49a-125">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="df49a-126">Projekty muszą odwoływać się do wersję spójną `Microsoft.AspNetCore.Razor.Design` (lub `Microsoft.AspNetCore.Mvc`) tak, że uwzględniono najnowszych poprawek czas kompilacji dla elementu Razor.</span><span class="sxs-lookup"><span data-stu-id="df49a-126">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="df49a-127">Aby uzyskać więcej informacji, zobacz [problem w usłudze GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="df49a-127">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="df49a-128">Właściwości</span><span class="sxs-lookup"><span data-stu-id="df49a-128">Properties</span></span>

<span data-ttu-id="df49a-129">Następujące właściwości kontrolować zachowanie zestawu SDK Razor jako część kompilacji projektu:</span><span class="sxs-lookup"><span data-stu-id="df49a-129">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="df49a-130">`RazorCompileOnBuild` &ndash; Gdy `true`, kompiluje i emituje zestaw Razor, w ramach tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="df49a-130">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="df49a-131">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="df49a-131">Defaults to `true`.</span></span>
* <span data-ttu-id="df49a-132">`RazorCompileOnPublish` &ndash; Gdy `true`, kompiluje i emituje zestaw Razor, w ramach publikowania projektu.</span><span class="sxs-lookup"><span data-stu-id="df49a-132">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="df49a-133">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="df49a-133">Defaults to `true`.</span></span>

<span data-ttu-id="df49a-134">Właściwości i elementy w poniższej tabeli są używane do konfigurowania danych wejściowych i wyjściowych zestawu SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="df49a-134">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

| <span data-ttu-id="df49a-135">Elementy</span><span class="sxs-lookup"><span data-stu-id="df49a-135">Items</span></span> | <span data-ttu-id="df49a-136">Opis</span><span class="sxs-lookup"><span data-stu-id="df49a-136">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="df49a-137">Element elementów ( *.cshtml* plików), które są dane wejściowe do celów generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="df49a-137">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| `RazorCompile` | <span data-ttu-id="df49a-138">Element elementów ( *.cs* plików), które są dane wejściowe do celów kompilacji Razor.</span><span class="sxs-lookup"><span data-stu-id="df49a-138">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="df49a-139">Użyj tego `ItemGroup` Aby określić dodatkowe pliki do skompilowany w zestawie Razor.</span><span class="sxs-lookup"><span data-stu-id="df49a-139">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="df49a-140">Element służącego do kodu generowania atrybuty dla zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="df49a-140">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="df49a-141">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="df49a-141">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="df49a-142">Elementy dodane do wygenerowanego zestawu Razor jako zasoby osadzone.</span><span class="sxs-lookup"><span data-stu-id="df49a-142">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="df49a-143">Właściwość</span><span class="sxs-lookup"><span data-stu-id="df49a-143">Property</span></span> | <span data-ttu-id="df49a-144">Opis</span><span class="sxs-lookup"><span data-stu-id="df49a-144">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="df49a-145">Nazwa pliku (bez rozszerzenia) zestaw utworzony przez Razor.</span><span class="sxs-lookup"><span data-stu-id="df49a-145">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="df49a-146">Katalog wyjściowy Razor.</span><span class="sxs-lookup"><span data-stu-id="df49a-146">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="df49a-147">Używany do określenia zestawu narzędzi, używany do tworzenia zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="df49a-147">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="df49a-148">Prawidłowe wartości to `Implicit`, `RazorSDK`, i `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="df49a-148">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="df49a-149">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="df49a-149">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="df49a-150">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="df49a-150">Default is `true`.</span></span> <span data-ttu-id="df49a-151">Gdy `true`, obejmuje *web.config*, *.json*, i *.cshtml* pliki zawartości w projekcie.</span><span class="sxs-lookup"><span data-stu-id="df49a-151">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="df49a-152">Gdy przywoływana za pomocą `Microsoft.NET.Sdk.Web`, plików w obszarze *wwwroot* i dołączane są także pliki konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="df49a-152">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="df49a-153">Gdy `true`, obejmuje *.cshtml* plików ze `Content` elementy w `RazorGenerate` elementów.</span><span class="sxs-lookup"><span data-stu-id="df49a-153">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="df49a-154">Gdy `true`, generuje *.cs* plik zawierający atrybuty określone przez `RazorAssemblyAttribute` włącznie z plikiem w danych wyjściowych kompilacji.</span><span class="sxs-lookup"><span data-stu-id="df49a-154">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="df49a-155">Gdy `true`, dodaje domyślny zestaw atrybutów zestawu do `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="df49a-155">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="df49a-156">Gdy `true`, kopie `RazorGenerate` elementów ( *.cshtml*) pliki do katalogu publikowania.</span><span class="sxs-lookup"><span data-stu-id="df49a-156">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="df49a-157">Zazwyczaj pliki Razor nie są wymagane dla opublikowanej aplikacji, jeśli uczestniczą w kompilacji w czasie kompilacji lub czasu publikowania.</span><span class="sxs-lookup"><span data-stu-id="df49a-157">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="df49a-158">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="df49a-158">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="df49a-159">Gdy `true`, skopiuj elementy zestawu odwołania do katalogu publikowania.</span><span class="sxs-lookup"><span data-stu-id="df49a-159">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="df49a-160">Zazwyczaj zestawy odwołań nie są wymagane dla opublikowanej aplikacji, jeśli kompilacja Razor występuje w czasie kompilacji lub czasu publikowania.</span><span class="sxs-lookup"><span data-stu-id="df49a-160">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="df49a-161">Ustaw `true` Jeśli Twojej opublikowanej aplikacji wymaga kompilacja środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="df49a-161">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="df49a-162">Na przykład, ustaw wartość `true` Jeśli aplikacja modyfikuje *.cshtml* plików w czasie wykonywania lub korzysta z osadzonych widoków.</span><span class="sxs-lookup"><span data-stu-id="df49a-162">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="df49a-163">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="df49a-163">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="df49a-164">Gdy `true`, wszystkie elementy zawartości Razor ( *.cshtml* pliki) są oznaczone do włączenia do wygenerowanego pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="df49a-164">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="df49a-165">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="df49a-165">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="df49a-166">Gdy `true`, dodaje RazorGenerate ( *.cshtml*) elementów jako osadzone pliki do wygenerowanego zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="df49a-166">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="df49a-167">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="df49a-167">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="df49a-168">Gdy `true`, używa procesu serwera kompilacji trwałego odciążania roboczego generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="df49a-168">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="df49a-169">Wartością domyślną jest wartość `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="df49a-169">Defaults to the value of `UseSharedCompilation`.</span></span> |

<span data-ttu-id="df49a-170">Aby uzyskać więcej informacji na temat właściwości, zobacz [właściwości programu MSBuild](/visualstudio/msbuild/msbuild-properties).</span><span class="sxs-lookup"><span data-stu-id="df49a-170">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="df49a-171">Obiekty docelowe</span><span class="sxs-lookup"><span data-stu-id="df49a-171">Targets</span></span>

<span data-ttu-id="df49a-172">Zestaw Razor SDK definiuje dwa podstawowe cele:</span><span class="sxs-lookup"><span data-stu-id="df49a-172">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="df49a-173">`RazorGenerate` &ndash; Generuje kod *.cs* plików ze `RazorGenerate` elementu elementów.</span><span class="sxs-lookup"><span data-stu-id="df49a-173">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="df49a-174">Użyj `RazorGenerateDependsOn` właściwości w celu określenia dodatkowych jest przeznaczony dla, uruchomić przed lub po ten element docelowy.</span><span class="sxs-lookup"><span data-stu-id="df49a-174">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="df49a-175">`RazorCompile` &ndash; Kompiluje generowane *.cs* pliki do zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="df49a-175">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="df49a-176">Użyj `RazorCompileDependsOn` Aby określić dodatkowe obiekty docelowe, które można uruchomić przed lub po ten element docelowy.</span><span class="sxs-lookup"><span data-stu-id="df49a-176">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="df49a-177">Kompilacja środowiska uruchomieniowego z widokami Razor</span><span class="sxs-lookup"><span data-stu-id="df49a-177">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="df49a-178">Domyślnie zestaw Razor SDK nie publikować zestawy odwołań, które są wymagane do przeprowadzenia kompilacja środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="df49a-178">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="df49a-179">Skutkuje to błędy kompilacji podczas model aplikacji zależy od kompilacja środowiska uruchomieniowego&mdash;na przykład aplikacja używa osadzonych widoków widoków lub zmiany po opublikowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="df49a-179">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="df49a-180">Ustaw `CopyRefAssembliesToPublishDirectory` do `true` aby kontynuować publikowanie zestawów odwołań.</span><span class="sxs-lookup"><span data-stu-id="df49a-180">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="df49a-181">Dla aplikacji sieci web, upewnij się, jest przeznaczona aplikacja `Microsoft.NET.Sdk.Web` zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="df49a-181">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="df49a-182">Wersja języka razor</span><span class="sxs-lookup"><span data-stu-id="df49a-182">Razor language version</span></span>

<span data-ttu-id="df49a-183">Podczas określania wartości `Microsoft.NET.Sdk.Web` zestawu SDK w wersji językowej Razor wynika z wersji platformy docelowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="df49a-183">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="df49a-184">Dla projektów przeznaczonych dla `Microsoft.NET.Sdk.Razor` zestawu SDK lub w rzadkich przypadkach, że aplikacja wymaga innej wersji języka Razor niż wartość wywnioskowane wersji mogą być konfigurowane przez ustawienie `<RazorLangVersion>` właściwość w pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="df49a-184">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

## <a name="additional-resources"></a><span data-ttu-id="df49a-185">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="df49a-185">Additional resources</span></span>

* [<span data-ttu-id="df49a-186">Dodatki do formatu csproj dla platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="df49a-186">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="df49a-187">Wspólne elementy projektów MSBuild</span><span class="sxs-lookup"><span data-stu-id="df49a-187">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
