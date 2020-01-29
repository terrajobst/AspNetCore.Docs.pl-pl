---
title: Platforma ASP.NET Core Razor SDK
author: Rick-Anderson
description: Dowiedz się, jak stron Razor w programie ASP.NET Core umożliwia kodowania scenariuszy skoncentrowane na stronie łatwiejsze i bardziej wydajne niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
no-loc:
- Blazor
uid: razor-pages/sdk
ms.openlocfilehash: 872d90662494735dc0e4caa01c46fcdcc2606bc6
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/28/2020
ms.locfileid: "76809136"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="fcbc9-103">Platforma ASP.NET Core Razor SDK</span><span class="sxs-lookup"><span data-stu-id="fcbc9-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="fcbc9-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fcbc9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="fcbc9-105">Omówienie</span><span class="sxs-lookup"><span data-stu-id="fcbc9-105">Overview</span></span>

<span data-ttu-id="fcbc9-106">[!INCLUDE[](~/includes/2.1-SDK.md)] Obejmuje `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span><span class="sxs-lookup"><span data-stu-id="fcbc9-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="fcbc9-107">Zestaw Razor SDK:</span><span class="sxs-lookup"><span data-stu-id="fcbc9-107">The Razor SDK:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="fcbc9-108">Jest wymagana do kompilowania, pakowania i publikowania projektów zawierających pliki [Razor](xref:mvc/views/razor) dla ASP.NET Core projektów opartych na MVC lub [Blazor](xref:blazor/index) .</span><span class="sxs-lookup"><span data-stu-id="fcbc9-108">Is required to build, package, and publish projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based or [Blazor](xref:blazor/index) projects.</span></span>
* <span data-ttu-id="fcbc9-109">Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementów, które umożliwiają dostosowanie kompilacji plików Razor ( *. cshtml* lub *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="fcbc9-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor (*.cshtml* or *.razor*) files.</span></span>

<span data-ttu-id="fcbc9-110">Zestaw SDK Razor zawiera elementy `Content` z atrybutami `Include` ustawionymi na `**\*.cshtml` i `**\*.razor` wzorców obsługi symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-110">The Razor SDK includes `Content` items with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="fcbc9-111">Zgodne pliki są publikowane.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-111">Matching files are published.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="fcbc9-112">Standaryzuje środowisko wokół tworzenia, pakowania i publikowania projektów zawierających [Razor](xref:mvc/views/razor) plików dla projektów ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-112">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="fcbc9-113">Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementy, które umożliwiają dostosowanie kompilacji w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-113">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

<span data-ttu-id="fcbc9-114">Zestaw SDK Razor zawiera element `Content` z atrybutem `Include` ustawionym na wzorzec `**\*.cshtml` obsługi symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-114">The Razor SDK includes a `Content` item with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="fcbc9-115">Zgodne pliki są publikowane.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-115">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="fcbc9-116">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="fcbc9-116">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="fcbc9-117">Korzystanie z zestawu SDK Razor</span><span class="sxs-lookup"><span data-stu-id="fcbc9-117">Use the Razor SDK</span></span>

<span data-ttu-id="fcbc9-118">Większość aplikacji sieci web nie są wymagane, aby jawnie odwołać się do zestawu SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-118">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fcbc9-119">Aby użyć zestawu Razor SDK do kompilowania bibliotek klas zawierających widoki Razor lub Razor Pages, zalecamy rozpoczęcie od szablonu projektu biblioteki klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="fcbc9-119">To use the Razor SDK to build class libraries containing Razor views or Razor Pages, we recommend starting with the Razor class library (RCL) project template.</span></span> <span data-ttu-id="fcbc9-120">RCL, który jest używany do kompilowania plików Blazor ( *. Razor*) w minimalnym stopniu wymaga odwołania do pakietu [Microsoft. AspNetCore. Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) .</span><span class="sxs-lookup"><span data-stu-id="fcbc9-120">An RCL that's used to build Blazor (*.razor*) files minimally requires a reference to the [Microsoft.AspNetCore.Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) package.</span></span> <span data-ttu-id="fcbc9-121">RCL, który jest używany do kompilowania widoków Razor lub stron (pliki *. cshtml* ), wymaga minimalnej `netcoreapp3.0` lub nowszej i ma `FrameworkReference` do [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-121">An RCL that's used to build Razor views or pages (*.cshtml* files) minimally requires targeting `netcoreapp3.0` or later and has a `FrameworkReference` to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in its project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fcbc9-122">Aby użyć zestaw Razor SDK do tworzenia biblioteki klas zawierający widokami Razor lub stron Razor:</span><span class="sxs-lookup"><span data-stu-id="fcbc9-122">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="fcbc9-123">Użyj `Microsoft.NET.Sdk.Razor` zamiast `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="fcbc9-123">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="fcbc9-124">Zazwyczaj odwołania do pakietu do `Microsoft.AspNetCore.Mvc` jest wymagana, aby otrzymać dodatkowe zależności, które są wymagane do kompilowania i kompilowania stron Razor i widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-124">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="fcbc9-125">Co najmniej projektu należy dodać odwołania do pakietu do:</span><span class="sxs-lookup"><span data-stu-id="fcbc9-125">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design`
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`

  <span data-ttu-id="fcbc9-126">`Microsoft.AspNetCore.Razor.Design` Pakiet zawiera cele i zadania kompilacji Razor do projektu.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-126">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="fcbc9-127">Poprzedni pakiety znajdują się w `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-127">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="fcbc9-128">Następujący kod przedstawia plik projektu, który używa zestaw Razor SDK do tworzenia plików Razor aplikacji ASP.NET Core Razor strony:</span><span class="sxs-lookup"><span data-stu-id="fcbc9-128">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>

  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="fcbc9-129">`Microsoft.AspNetCore.Razor.Design` i `Microsoft.AspNetCore.Mvc.Razor.Extensions` pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="fcbc9-129">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="fcbc9-130">Jednak bez wersji `Microsoft.AspNetCore.App` odwołania do pakietu zawiera meta Microsoft.aspnetcore.all, aplikacja która nie zawiera najnowszą wersję `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-130">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="fcbc9-131">Projekty muszą odwoływać się do wersję spójną `Microsoft.AspNetCore.Razor.Design` (lub `Microsoft.AspNetCore.Mvc`) tak, że uwzględniono najnowszych poprawek czas kompilacji dla elementu Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-131">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="fcbc9-132">Aby uzyskać więcej informacji, zobacz [problem w usłudze GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="fcbc9-132">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="fcbc9-133">Właściwości</span><span class="sxs-lookup"><span data-stu-id="fcbc9-133">Properties</span></span>

<span data-ttu-id="fcbc9-134">Następujące właściwości kontrolować zachowanie zestawu SDK Razor jako część kompilacji projektu:</span><span class="sxs-lookup"><span data-stu-id="fcbc9-134">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="fcbc9-135">`RazorCompileOnBuild` &ndash; Gdy `true`, kompiluje i emituje zestaw Razor, w ramach tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-135">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="fcbc9-136">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-136">Defaults to `true`.</span></span>
* <span data-ttu-id="fcbc9-137">`RazorCompileOnPublish` &ndash; Gdy `true`, kompiluje i emituje zestaw Razor, w ramach publikowania projektu.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-137">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="fcbc9-138">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-138">Defaults to `true`.</span></span>

<span data-ttu-id="fcbc9-139">Właściwości i elementy w poniższej tabeli są używane do konfigurowania danych wejściowych i wyjściowych zestawu SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-139">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="fcbc9-140">Począwszy od ASP.NET Core 3,0, widoki MVC lub Razor Pages nie są obsługiwane domyślnie, jeśli właściwości `RazorCompileOnBuild` lub `RazorCompileOnPublish` programu MSBuild w pliku projektu są wyłączone.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-140">Starting with ASP.NET Core 3.0, MVC Views or Razor Pages aren't served by default if the `RazorCompileOnBuild` or `RazorCompileOnPublish` MSBuild properties in the project file are disabled.</span></span> <span data-ttu-id="fcbc9-141">Aplikacje muszą dodać jawne odwołanie do pakietu [Microsoft. AspNetCore. MVC. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) , jeśli aplikacja korzysta z kompilacji środowiska uruchomieniowego w celu przetworzenia plików *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="fcbc9-141">Applications must add an explicit reference to the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) package if the app relies on runtime compilation to process *.cshtml* files.</span></span>

::: moniker-end

| <span data-ttu-id="fcbc9-142">Elementy</span><span class="sxs-lookup"><span data-stu-id="fcbc9-142">Items</span></span> | <span data-ttu-id="fcbc9-143">Opis</span><span class="sxs-lookup"><span data-stu-id="fcbc9-143">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="fcbc9-144">Elementy elementu (pliki *. cshtml* ), które są danymi wejściowymi do generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-144">Item elements (*.cshtml* files) that are inputs to code generation.</span></span> |
| `RazorComponent` | <span data-ttu-id="fcbc9-145">Elementy elementu (pliki*Razor* ), które są danymi wejściowymi do generowania kodu składnika Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-145">Item elements (*.razor* files) that are inputs to Razor component code generation.</span></span> |
| `RazorCompile` | <span data-ttu-id="fcbc9-146">Elementy elementów (pliki*CS* ), które są danymi wejściowymi do elementów docelowych kompilacji Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-146">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="fcbc9-147">Użyj tego `ItemGroup`, aby określić dodatkowe pliki do skompilowania do zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-147">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="fcbc9-148">Element służącego do kodu generowania atrybuty dla zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-148">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="fcbc9-149">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fcbc9-149">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="fcbc9-150">Elementy dodane do wygenerowanego zestawu Razor jako zasoby osadzone.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-150">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="fcbc9-151">Właściwość</span><span class="sxs-lookup"><span data-stu-id="fcbc9-151">Property</span></span> | <span data-ttu-id="fcbc9-152">Opis</span><span class="sxs-lookup"><span data-stu-id="fcbc9-152">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="fcbc9-153">Nazwa pliku (bez rozszerzenia) zestaw utworzony przez Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-153">File name (without extension) of the assembly produced by Razor.</span></span> |
| `RazorOutputPath` | <span data-ttu-id="fcbc9-154">Katalog wyjściowy Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-154">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="fcbc9-155">Używany do określenia zestawu narzędzi, używany do tworzenia zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-155">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="fcbc9-156">Prawidłowe wartości to `Implicit`, `RazorSDK`, i `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-156">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="fcbc9-157">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="fcbc9-157">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="fcbc9-158">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-158">Default is `true`.</span></span> <span data-ttu-id="fcbc9-159">Gdy `true`, zawiera pliki *Web. config*, *. JSON*i *. cshtml* jako zawartość projektu.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-159">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="fcbc9-160">Gdy przywoływana za pomocą `Microsoft.NET.Sdk.Web`, plików w obszarze *wwwroot* i dołączane są także pliki konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-160">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="fcbc9-161">Gdy `true`, obejmuje *.cshtml* plików ze `Content` elementy w `RazorGenerate` elementów.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-161">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="fcbc9-162">Gdy `true`, generuje *.cs* plik zawierający atrybuty określone przez `RazorAssemblyAttribute` włącznie z plikiem w danych wyjściowych kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-162">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="fcbc9-163">Gdy `true`, dodaje domyślny zestaw atrybutów zestawu do `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-163">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="fcbc9-164">Gdy `true`, kopie `RazorGenerate` elementów ( *.cshtml*) pliki do katalogu publikowania.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-164">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="fcbc9-165">Zazwyczaj pliki Razor nie są wymagane dla opublikowanej aplikacji, jeśli uczestniczą w kompilacji w czasie kompilacji lub czasu publikowania.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-165">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="fcbc9-166">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-166">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="fcbc9-167">Gdy `true`, skopiuj elementy zestawu odwołania do katalogu publikowania.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-167">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="fcbc9-168">Zazwyczaj zestawy odwołań nie są wymagane dla opublikowanej aplikacji, jeśli kompilacja Razor występuje w czasie kompilacji lub czasu publikowania.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-168">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="fcbc9-169">Ustaw `true` Jeśli Twojej opublikowanej aplikacji wymaga kompilacja środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-169">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="fcbc9-170">Na przykład, ustaw wartość `true` Jeśli aplikacja modyfikuje *.cshtml* plików w czasie wykonywania lub korzysta z osadzonych widoków.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-170">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="fcbc9-171">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-171">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="fcbc9-172">Gdy `true`, wszystkie elementy zawartości Razor ( *.cshtml* pliki) są oznaczone do włączenia do wygenerowanego pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-172">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="fcbc9-173">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-173">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="fcbc9-174">Gdy `true`, dodaje RazorGenerate ( *.cshtml*) elementów jako osadzone pliki do wygenerowanego zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-174">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="fcbc9-175">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-175">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="fcbc9-176">Gdy `true`, używa procesu serwera kompilacji trwałego odciążania roboczego generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-176">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="fcbc9-177">Wartością domyślną jest wartość `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-177">Defaults to the value of `UseSharedCompilation`.</span></span> |
| `GenerateMvcApplicationPartsAssemblyAttributes` | <span data-ttu-id="fcbc9-178">Gdy `true`, zestaw SDK generuje dodatkowe atrybuty używane przez MVC w czasie wykonywania w celu przeprowadzenia odnajdywania części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-178">When `true`, the SDK generates additional attributes used by MVC at runtime to perform application part discovery.</span></span> |
| `DefaultWebContentItemExcludes` | <span data-ttu-id="fcbc9-179">Wzorzec obsługi symboli wieloznacznych dla elementów elementów, które mają zostać wykluczone z grupy elementów `Content` w projektach przeznaczonych dla zestawu SDK sieci Web lub Razor</span><span class="sxs-lookup"><span data-stu-id="fcbc9-179">A globbing pattern for item elements that are to be excluded from the `Content` item group in projects targeting the Web or Razor SDK</span></span> |
| `ExcludeConfigFilesFromBuildOutput` | <span data-ttu-id="fcbc9-180">W przypadku `true`pliki *config* i *JSON* nie są kopiowane do katalogu wyjściowego kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-180">When `true`, *.config* and *.json* files do not get copied to the build output directory.</span></span> |
| `AddRazorSupportForMvc` | <span data-ttu-id="fcbc9-181">Gdy `true`, program skonfiguruje zestaw SDK Razor do dodawania obsługi konfiguracji MVC, która jest wymagana podczas kompilowania aplikacji zawierających widoki MVC lub Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-181">When `true`, configures the Razor SDK to add support for the MVC configuration that is required when building applications containing MVC views or Razor Pages.</span></span> <span data-ttu-id="fcbc9-182">Ta właściwość jest niejawnie ustawiona dla projektów .NET Core 3,0 lub nowszych przeznaczonych dla zestawu SDK sieci Web</span><span class="sxs-lookup"><span data-stu-id="fcbc9-182">This property is implicitly set for .NET Core 3.0 or later projects targeting the Web SDK</span></span> |
| `RazorLangVersion` | <span data-ttu-id="fcbc9-183">Wersja języka Razor do celów.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-183">The version of the Razor Language to target.</span></span> |

<span data-ttu-id="fcbc9-184">Aby uzyskać więcej informacji na temat właściwości, zobacz [Właściwości programu MSBuild](/visualstudio/msbuild/msbuild-properties).</span><span class="sxs-lookup"><span data-stu-id="fcbc9-184">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="fcbc9-185">Obiekty docelowe</span><span class="sxs-lookup"><span data-stu-id="fcbc9-185">Targets</span></span>

<span data-ttu-id="fcbc9-186">Zestaw Razor SDK definiuje dwa podstawowe cele:</span><span class="sxs-lookup"><span data-stu-id="fcbc9-186">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="fcbc9-187">`RazorGenerate` &ndash; Generuje kod *.cs* plików ze `RazorGenerate` elementu elementów.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-187">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="fcbc9-188">Użyj właściwości `RazorGenerateDependsOn`, aby określić dodatkowe elementy docelowe, które mogą być uruchamiane przed lub po tym elemencie docelowym.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-188">Use the `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="fcbc9-189">`RazorCompile` &ndash; Kompiluje generowane *.cs* pliki do zestawu Razor.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-189">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="fcbc9-190">Użyj `RazorCompileDependsOn`, aby określić dodatkowe elementy docelowe, które mogą być uruchamiane przed lub po tym elemencie docelowym.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-190">Use the `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="fcbc9-191">`RazorComponentGenerate` &ndash; kod generuje pliki *CS* dla elementów `RazorComponent` elementu.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-191">`RazorComponentGenerate` &ndash; Code generates *.cs* files for `RazorComponent` item elements.</span></span> <span data-ttu-id="fcbc9-192">Użyj właściwości `RazorComponentGenerateDependsOn`, aby określić dodatkowe elementy docelowe, które mogą być uruchamiane przed lub po tym elemencie docelowym.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-192">Use the `RazorComponentGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="fcbc9-193">Kompilacja środowiska uruchomieniowego z widokami Razor</span><span class="sxs-lookup"><span data-stu-id="fcbc9-193">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="fcbc9-194">Domyślnie zestaw Razor SDK nie publikować zestawy odwołań, które są wymagane do przeprowadzenia kompilacja środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-194">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="fcbc9-195">Skutkuje to błędy kompilacji podczas model aplikacji zależy od kompilacja środowiska uruchomieniowego&mdash;na przykład aplikacja używa osadzonych widoków widoków lub zmiany po opublikowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-195">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="fcbc9-196">Ustaw `CopyRefAssembliesToPublishDirectory` do `true` aby kontynuować publikowanie zestawów odwołań.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-196">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="fcbc9-197">Dla aplikacji sieci web, upewnij się, jest przeznaczona aplikacja `Microsoft.NET.Sdk.Web` zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-197">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="fcbc9-198">Wersja języka Razor</span><span class="sxs-lookup"><span data-stu-id="fcbc9-198">Razor language version</span></span>

<span data-ttu-id="fcbc9-199">W przypadku określania zestawu SDK `Microsoft.NET.Sdk.Web` wersja języka Razor jest wywnioskowana z wersji platformy docelowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-199">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the app's target framework version.</span></span> <span data-ttu-id="fcbc9-200">W przypadku projektów przeznaczonych dla zestawu SDK `Microsoft.NET.Sdk.Razor` lub w rzadkich przypadkach, gdy aplikacja wymaga innej wersji języka Razor niż wartość wywnioskowana, można skonfigurować wersję, ustawiając właściwość `<RazorLangVersion>` w pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="fcbc9-200">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

<span data-ttu-id="fcbc9-201">Wersja języka Razor jest ściśle zintegrowana z wersją środowiska uruchomieniowego, dla którego została skompilowana.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-201">Razor's language version is tightly integrated with the version of the runtime that it was built for.</span></span> <span data-ttu-id="fcbc9-202">Określanie wersji językowej, która nie jest przeznaczona dla środowiska uruchomieniowego, nie jest obsługiwane i prawdopodobnie powoduje błędy kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fcbc9-202">Targeting a language version that isn't designed for the runtime is unsupported and likely produces build errors.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fcbc9-203">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fcbc9-203">Additional resources</span></span>

* [<span data-ttu-id="fcbc9-204">Dodatki do formatu csproj dla platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="fcbc9-204">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="fcbc9-205">Wspólne elementy projektu MSBuild</span><span class="sxs-lookup"><span data-stu-id="fcbc9-205">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
