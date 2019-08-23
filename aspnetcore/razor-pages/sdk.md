---
title: Platforma ASP.NET Core Razor SDK
author: Rick-Anderson
description: Dowiedz się, jak stron Razor w programie ASP.NET Core umożliwia kodowania scenariuszy skoncentrowane na stronie łatwiejsze i bardziej wydajne niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/18/2019
uid: razor-pages/sdk
ms.openlocfilehash: 1dc001c7c5fe320629835e06fe6db7fadabff94d
ms.sourcegitcommit: 6189b0ced9c115248c6ede02efcd0b29d31f2115
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/23/2019
ms.locfileid: "69985396"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="c9d25-103">Platforma ASP.NET Core Razor SDK</span><span class="sxs-lookup"><span data-stu-id="c9d25-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="c9d25-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c9d25-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="c9d25-105">Omówienie</span><span class="sxs-lookup"><span data-stu-id="c9d25-105">Overview</span></span>

<span data-ttu-id="c9d25-106">[!INCLUDE[](~/includes/2.1-SDK.md)] Obejmuje `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span><span class="sxs-lookup"><span data-stu-id="c9d25-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="c9d25-107">Zestaw Razor SDK:</span><span class="sxs-lookup"><span data-stu-id="c9d25-107">The Razor SDK:</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"
* <span data-ttu-id="c9d25-108">Standaryzuje środowisko wokół tworzenia, pakowania i publikowania projektów zawierających [Razor](xref:mvc/views/razor) plików dla projektów ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c9d25-108">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="c9d25-109">Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementy, które umożliwiają dostosowanie kompilacji w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-3.0"
* <span data-ttu-id="c9d25-110">jest wymagana do kompilowania, pakowania i publikowania projektów zawierających pliki [Razor](xref:mvc/views/razor) dla ASP.NET Core projektów opartych na MVC lub projektów [Blazor](xref:blazor/index)</span><span class="sxs-lookup"><span data-stu-id="c9d25-110">is required to build, package, and publish projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects, or [Blazor](xref:blazor/index) projects</span></span>
* <span data-ttu-id="c9d25-111">Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementów, które umożliwiają dostosowanie kompilacji plików Razor (. cshtml lub. Razor).</span><span class="sxs-lookup"><span data-stu-id="c9d25-111">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor (.cshtml or .razor) files.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="c9d25-112">Zestaw SDK Razor zawiera `<Content>` element `Include` z atrybutem ustawionym na `**\*.cshtml` wzorzec obsługi symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="c9d25-112">The Razor SDK includes a `<Content>` element with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="c9d25-113">Zgodne pliki są publikowane.</span><span class="sxs-lookup"><span data-stu-id="c9d25-113">Matching files are published.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c9d25-114">Zestaw SDK Razor zawiera `<Content>` elementy z `Include` atrybutami ustawionymi `**\*.cshtml` na `**\*.razor` wzorce i obsługi symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="c9d25-114">The Razor SDK includes `<Content>` elements with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="c9d25-115">Zgodne pliki są publikowane.</span><span class="sxs-lookup"><span data-stu-id="c9d25-115">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="c9d25-116">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="c9d25-116">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="c9d25-117">Korzystanie z zestawu SDK Razor</span><span class="sxs-lookup"><span data-stu-id="c9d25-117">Use the Razor SDK</span></span>

<span data-ttu-id="c9d25-118">Większość aplikacji sieci web nie są wymagane, aby jawnie odwołać się do zestawu SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-118">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"
<span data-ttu-id="c9d25-119">Aby użyć zestaw Razor SDK do tworzenia biblioteki klas zawierający widokami Razor lub stron Razor:</span><span class="sxs-lookup"><span data-stu-id="c9d25-119">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="c9d25-120">Użyj `Microsoft.NET.Sdk.Razor` zamiast `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="c9d25-120">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="c9d25-121">Zazwyczaj odwołania do pakietu do `Microsoft.AspNetCore.Mvc` jest wymagana, aby otrzymać dodatkowe zależności, które są wymagane do kompilowania i kompilowania stron Razor i widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-121">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="c9d25-122">Co najmniej projektu należy dodać odwołania do pakietu do:</span><span class="sxs-lookup"><span data-stu-id="c9d25-122">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="c9d25-123">`Microsoft.AspNetCore.Razor.Design` Pakiet zawiera cele i zadania kompilacji Razor do projektu.</span><span class="sxs-lookup"><span data-stu-id="c9d25-123">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="c9d25-124">Poprzedni pakiety znajdują się w `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="c9d25-124">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="c9d25-125">Następujący kod przedstawia plik projektu, który używa zestaw Razor SDK do tworzenia plików Razor aplikacji ASP.NET Core Razor strony:</span><span class="sxs-lookup"><span data-stu-id="c9d25-125">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="c9d25-126">Aby użyć zestawu Razor SDK do kompilowania bibliotek klas zawierających widoki Razor lub Razor Pages zalecamy rozpoczęcie od szablonu projektu biblioteki klas Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-126">To use the Razor SDK to build class libraries containing Razor views or Razor Pages we recommend starting with the Razor Class Library project template.</span></span> <span data-ttu-id="c9d25-127">Biblioteka klas Razor, która jest używana do kompilowania plików Blazor (. Razor), będzie wymagała co najmniej odwołania `Microsoft.AspNetCore.Components` do pakietu.</span><span class="sxs-lookup"><span data-stu-id="c9d25-127">A Razor class library that is used to build Blazor (.razor) files will at minimum require a reference to the `Microsoft.AspNetCore.Components` package.</span></span> <span data-ttu-id="c9d25-128">Biblioteka klas Razor, która jest używana do kompilowania widoków i stron Razor (pliki. cshtml), będzie wymagała określania wartości docelowej `netcoreapp3.0` lub nowszej oraz może `FrameworkReference` zawierać do `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="c9d25-128">A Razor class library that is used to build Razor views or pages (.cshtml files), will require targeting `netcoreapp3.0` or newer and have a `FrameworkReference` to `Microsoft.AspNetCore.App`.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="c9d25-129">`Microsoft.AspNetCore.Razor.Design` i `Microsoft.AspNetCore.Mvc.Razor.Extensions` pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c9d25-129">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="c9d25-130">Jednak bez wersji `Microsoft.AspNetCore.App` odwołania do pakietu zawiera meta Microsoft.aspnetcore.all, aplikacja która nie zawiera najnowszą wersję `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="c9d25-130">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="c9d25-131">Projekty muszą odwoływać się do wersję spójną `Microsoft.AspNetCore.Razor.Design` (lub `Microsoft.AspNetCore.Mvc`) tak, że uwzględniono najnowszych poprawek czas kompilacji dla elementu Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-131">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="c9d25-132">Aby uzyskać więcej informacji, zobacz [problem w usłudze GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="c9d25-132">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="c9d25-133">Właściwości</span><span class="sxs-lookup"><span data-stu-id="c9d25-133">Properties</span></span>

<span data-ttu-id="c9d25-134">Następujące właściwości kontrolować zachowanie zestawu SDK Razor jako część kompilacji projektu:</span><span class="sxs-lookup"><span data-stu-id="c9d25-134">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="c9d25-135">`RazorCompileOnBuild` &ndash; Gdy `true`, kompiluje i emituje zestaw Razor, w ramach tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="c9d25-135">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="c9d25-136">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="c9d25-136">Defaults to `true`.</span></span>
* <span data-ttu-id="c9d25-137">`RazorCompileOnPublish` &ndash; Gdy `true`, kompiluje i emituje zestaw Razor, w ramach publikowania projektu.</span><span class="sxs-lookup"><span data-stu-id="c9d25-137">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="c9d25-138">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="c9d25-138">Defaults to `true`.</span></span>

<span data-ttu-id="c9d25-139">Właściwości i elementy w poniższej tabeli są używane do konfigurowania danych wejściowych i wyjściowych zestawu SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-139">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"
> [!WARNING]
<span data-ttu-id="c9d25-140">Począwszy od ASP.NET Core 3,0, widoki MVC lub Razor Pages nie będą obsługiwane domyślnie, jeśli `RazorCompileOnBuild` lub `RazorCompileOnPublish` jest wyłączone.</span><span class="sxs-lookup"><span data-stu-id="c9d25-140">Starting with ASP.NET Core 3.0, MVC Views or Razor Pages will not be served by default if `RazorCompileOnBuild` or `RazorCompileOnPublish` is disabled.</span></span> <span data-ttu-id="c9d25-141">Aplikacje muszą dodać jawne odwołanie do `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` pakietu, aby dodać obsługę kompilacji w czasie wykonywania, jeśli są one zależne od kompilacji środowiska uruchomieniowego w celu przetworzenia plików cshtml.</span><span class="sxs-lookup"><span data-stu-id="c9d25-141">Applications must add an explicit reference to `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` package to add support for runtime compilation if they rely on runtime compilation to process cshtml files.</span></span>
::: moniker-end


| <span data-ttu-id="c9d25-142">Elementy</span><span class="sxs-lookup"><span data-stu-id="c9d25-142">Items</span></span> | <span data-ttu-id="c9d25-143">Opis</span><span class="sxs-lookup"><span data-stu-id="c9d25-143">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="c9d25-144">Elementy elementu (pliki *. cshtml* ), które są danymi wejściowymi do generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="c9d25-144">Item elements (*.cshtml* files) that are inputs to code generation.</span></span> |
| `RazorComponent` | <span data-ttu-id="c9d25-145">Elementy elementu (pliki*Razor* ), które są danymi wejściowymi generowania kodu składnika.</span><span class="sxs-lookup"><span data-stu-id="c9d25-145">Item elements (*.razor* files) that are inputs to Component code generation.</span></span>
| `RazorCompile` | <span data-ttu-id="c9d25-146">Elementy elementów (pliki*CS* ), które są danymi wejściowymi do elementów docelowych kompilacji Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-146">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="c9d25-147">Użyj tego `ItemGroup` , aby określić dodatkowe pliki do skompilowania do zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-147">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="c9d25-148">Element służącego do kodu generowania atrybuty dla zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-148">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="c9d25-149">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c9d25-149">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="c9d25-150">Elementy dodane do wygenerowanego zestawu Razor jako zasoby osadzone.</span><span class="sxs-lookup"><span data-stu-id="c9d25-150">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="c9d25-151">Właściwość</span><span class="sxs-lookup"><span data-stu-id="c9d25-151">Property</span></span> | <span data-ttu-id="c9d25-152">Opis</span><span class="sxs-lookup"><span data-stu-id="c9d25-152">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="c9d25-153">Nazwa pliku (bez rozszerzenia) zestaw utworzony przez Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-153">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="c9d25-154">Katalog wyjściowy Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-154">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="c9d25-155">Używany do określenia zestawu narzędzi, używany do tworzenia zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-155">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="c9d25-156">Prawidłowe wartości to `Implicit`, `RazorSDK`, i `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="c9d25-156">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="c9d25-157">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="c9d25-157">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="c9d25-158">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="c9d25-158">Default is `true`.</span></span> <span data-ttu-id="c9d25-159">Gdy `true`, zawiera pliki *Web. config*, *. JSON*i *. cshtml* jako zawartość projektu.</span><span class="sxs-lookup"><span data-stu-id="c9d25-159">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="c9d25-160">Gdy przywoływana za pomocą `Microsoft.NET.Sdk.Web`, plików w obszarze *wwwroot* i dołączane są także pliki konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c9d25-160">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="c9d25-161">Gdy `true`, obejmuje *.cshtml* plików ze `Content` elementy w `RazorGenerate` elementów.</span><span class="sxs-lookup"><span data-stu-id="c9d25-161">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="c9d25-162">Gdy `true`, generuje *.cs* plik zawierający atrybuty określone przez `RazorAssemblyAttribute` włącznie z plikiem w danych wyjściowych kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c9d25-162">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="c9d25-163">Gdy `true`, dodaje domyślny zestaw atrybutów zestawu do `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="c9d25-163">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="c9d25-164">Gdy `true`, kopie `RazorGenerate` elementów ( *.cshtml*) pliki do katalogu publikowania.</span><span class="sxs-lookup"><span data-stu-id="c9d25-164">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="c9d25-165">Zazwyczaj pliki Razor nie są wymagane dla opublikowanej aplikacji, jeśli uczestniczą w kompilacji w czasie kompilacji lub czasu publikowania.</span><span class="sxs-lookup"><span data-stu-id="c9d25-165">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="c9d25-166">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="c9d25-166">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="c9d25-167">Gdy `true`, skopiuj elementy zestawu odwołania do katalogu publikowania.</span><span class="sxs-lookup"><span data-stu-id="c9d25-167">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="c9d25-168">Zazwyczaj zestawy odwołań nie są wymagane dla opublikowanej aplikacji, jeśli kompilacja Razor występuje w czasie kompilacji lub czasu publikowania.</span><span class="sxs-lookup"><span data-stu-id="c9d25-168">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="c9d25-169">Ustaw `true` Jeśli Twojej opublikowanej aplikacji wymaga kompilacja środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="c9d25-169">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="c9d25-170">Na przykład, ustaw wartość `true` Jeśli aplikacja modyfikuje *.cshtml* plików w czasie wykonywania lub korzysta z osadzonych widoków.</span><span class="sxs-lookup"><span data-stu-id="c9d25-170">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="c9d25-171">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="c9d25-171">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="c9d25-172">Gdy `true`, wszystkie elementy zawartości Razor ( *.cshtml* pliki) są oznaczone do włączenia do wygenerowanego pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="c9d25-172">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="c9d25-173">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="c9d25-173">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="c9d25-174">Gdy `true`, dodaje RazorGenerate ( *.cshtml*) elementów jako osadzone pliki do wygenerowanego zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-174">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="c9d25-175">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="c9d25-175">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="c9d25-176">Gdy `true`, używa procesu serwera kompilacji trwałego odciążania roboczego generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="c9d25-176">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="c9d25-177">Wartością domyślną jest wartość `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="c9d25-177">Defaults to the value of `UseSharedCompilation`.</span></span> |
| `GenerateMvcApplicationPartsAssemblyAttributes` | <span data-ttu-id="c9d25-178">Gdy `true`zestaw SDK generuje dodatkowe atrybuty używane przez MVC w czasie wykonywania w celu przeprowadzenia odnajdywania części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c9d25-178">When `true`, the SDK generates additional attributes used by MVC at runtime to perform application part discovery.</span></span> |

<span data-ttu-id="c9d25-179">Aby uzyskać więcej informacji na temat właściwości, zobacz [Właściwości programu MSBuild](/visualstudio/msbuild/msbuild-properties).</span><span class="sxs-lookup"><span data-stu-id="c9d25-179">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="c9d25-180">Obiekty docelowe</span><span class="sxs-lookup"><span data-stu-id="c9d25-180">Targets</span></span>

<span data-ttu-id="c9d25-181">Zestaw Razor SDK definiuje dwa podstawowe cele:</span><span class="sxs-lookup"><span data-stu-id="c9d25-181">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="c9d25-182">`RazorGenerate` &ndash; Generuje kod *.cs* plików ze `RazorGenerate` elementu elementów.</span><span class="sxs-lookup"><span data-stu-id="c9d25-182">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="c9d25-183">Użyj `RazorGenerateDependsOn` właściwości w celu określenia dodatkowych jest przeznaczony dla, uruchomić przed lub po ten element docelowy.</span><span class="sxs-lookup"><span data-stu-id="c9d25-183">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="c9d25-184">`RazorCompile` &ndash; Kompiluje generowane *.cs* pliki do zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="c9d25-184">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="c9d25-185">Użyj `RazorCompileDependsOn` Aby określić dodatkowe obiekty docelowe, które można uruchomić przed lub po ten element docelowy.</span><span class="sxs-lookup"><span data-stu-id="c9d25-185">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="c9d25-186">`RazorComponentGenerate`Kod generuje pliki *CS* dla `RazorComponent` elementów Item. &ndash;</span><span class="sxs-lookup"><span data-stu-id="c9d25-186">`RazorComponentGenerate` &ndash; Code generates *.cs* files for `RazorComponent` item elements.</span></span> <span data-ttu-id="c9d25-187">Użyj `RazorComponentGenerateDependsOn` właściwości w celu określenia dodatkowych jest przeznaczony dla, uruchomić przed lub po ten element docelowy.</span><span class="sxs-lookup"><span data-stu-id="c9d25-187">Use `RazorComponentGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="c9d25-188">Kompilacja środowiska uruchomieniowego z widokami Razor</span><span class="sxs-lookup"><span data-stu-id="c9d25-188">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="c9d25-189">Domyślnie zestaw Razor SDK nie publikować zestawy odwołań, które są wymagane do przeprowadzenia kompilacja środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="c9d25-189">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="c9d25-190">Skutkuje to błędy kompilacji podczas model aplikacji zależy od kompilacja środowiska uruchomieniowego&mdash;na przykład aplikacja używa osadzonych widoków widoków lub zmiany po opublikowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c9d25-190">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="c9d25-191">Ustaw `CopyRefAssembliesToPublishDirectory` do `true` aby kontynuować publikowanie zestawów odwołań.</span><span class="sxs-lookup"><span data-stu-id="c9d25-191">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="c9d25-192">Dla aplikacji sieci web, upewnij się, jest przeznaczona aplikacja `Microsoft.NET.Sdk.Web` zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="c9d25-192">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="c9d25-193">Wersja języka Razor</span><span class="sxs-lookup"><span data-stu-id="c9d25-193">Razor language version</span></span>

<span data-ttu-id="c9d25-194">W przypadku określania `Microsoft.NET.Sdk.Web` zestawu SDK wersja języka Razor jest wywnioskowana z wersji platformy docelowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c9d25-194">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="c9d25-195">W przypadku projektów `Microsoft.NET.Sdk.Razor` przeznaczonych dla zestawu SDK lub w rzadkich przypadkach, gdy aplikacja wymaga innej wersji języka Razor niż wartość wywnioskowana, można skonfigurować wersję, `<RazorLangVersion>` ustawiając właściwość w pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c9d25-195">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

<span data-ttu-id="c9d25-196">Wersja języka Razor jest ściśle zintegrowana z wersją środowiska uruchomieniowego, dla którego została skompilowana.</span><span class="sxs-lookup"><span data-stu-id="c9d25-196">Razor's language version is tightly integrated with the version of the runtime that it was built for.</span></span> <span data-ttu-id="c9d25-197">Określanie wersji językowej, która nie jest przeznaczona dla środowiska uruchomieniowego, nie jest obsługiwane i prawdopodobnie spowoduje błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c9d25-197">Targeting a language version that is not designed for the runtime is unsupported, and will likely produce build errors.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c9d25-198">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c9d25-198">Additional resources</span></span>

* [<span data-ttu-id="c9d25-199">Dodatki do formatu csproj dla platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="c9d25-199">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="c9d25-200">Wspólne elementy projektu MSBuild</span><span class="sxs-lookup"><span data-stu-id="c9d25-200">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
