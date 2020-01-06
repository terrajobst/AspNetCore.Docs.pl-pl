---
title: Używanie ASP.NET Core interfejsów API w bibliotece klas
author: scottaddie
description: Dowiedz się, jak używać interfejsów API ASP.NET Core w bibliotece klas.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
no-loc:
- Blazor
uid: fundamentals/target-aspnetcore
ms.openlocfilehash: 72096fc2f03033dfe8325b5129e074913a2fbd1f
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75463889"
---
# <a name="use-aspnet-core-apis-in-a-class-library"></a><span data-ttu-id="5e006-103">Używanie ASP.NET Core interfejsów API w bibliotece klas</span><span class="sxs-lookup"><span data-stu-id="5e006-103">Use ASP.NET Core APIs in a class library</span></span>

<span data-ttu-id="5e006-104">Przez [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="5e006-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="5e006-105">Ten dokument zawiera wskazówki dotyczące używania ASP.NET Core interfejsów API w bibliotece klas.</span><span class="sxs-lookup"><span data-stu-id="5e006-105">This document provides guidance for using ASP.NET Core APIs in a class library.</span></span> <span data-ttu-id="5e006-106">Wszystkie inne wskazówki dotyczące biblioteki można znaleźć w temacie [wskazówki dotyczące biblioteki Open Source](/dotnet/standard/library-guidance/).</span><span class="sxs-lookup"><span data-stu-id="5e006-106">For all other library guidance, see [Open-source library guidance](/dotnet/standard/library-guidance/).</span></span>

## <a name="determine-which-aspnet-core-versions-to-support"></a><span data-ttu-id="5e006-107">Określ, które wersje ASP.NET Core mają być obsługiwane</span><span class="sxs-lookup"><span data-stu-id="5e006-107">Determine which ASP.NET Core versions to support</span></span>

<span data-ttu-id="5e006-108">ASP.NET Core jest zgodna z [zasadami obsługi .NET Core](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span><span class="sxs-lookup"><span data-stu-id="5e006-108">ASP.NET Core adheres to the [.NET Core support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span> <span data-ttu-id="5e006-109">Podczas określania, które wersje ASP.NET Core mają być obsługiwane w bibliotece, należy zapoznać się z zasadami pomocy technicznej.</span><span class="sxs-lookup"><span data-stu-id="5e006-109">Consult the support policy when determining which ASP.NET Core versions to support in a library.</span></span> <span data-ttu-id="5e006-110">Biblioteka powinna:</span><span class="sxs-lookup"><span data-stu-id="5e006-110">A library should:</span></span>

* <span data-ttu-id="5e006-111">Zwiększ nakład pracy, aby obsłużyć wszystkie ASP.NET Core wersje sklasyfikowane jako *długoterminowe wsparcie* (LTS).</span><span class="sxs-lookup"><span data-stu-id="5e006-111">Make an effort to support all ASP.NET Core versions classified as *Long-Term Support* (LTS).</span></span>
* <span data-ttu-id="5e006-112">Nie jest zobowiązana do obsługi wersji ASP.NET Core sklasyfikowanych jako *koniec cyklu życia* (EOL).</span><span class="sxs-lookup"><span data-stu-id="5e006-112">Not feel obligated to support ASP.NET Core versions classified as *End of Life* (EOL).</span></span>

<span data-ttu-id="5e006-113">W przypadku udostępnienia wersji zapoznawczej ASP.NET Core zostaną ogłoszone istotne zmiany w repozytorium GitHub [/anonsów](https://github.com/aspnet/Announcements/issues) w serwisie.</span><span class="sxs-lookup"><span data-stu-id="5e006-113">As preview releases of ASP.NET Core are made available, breaking changes are posted in the [aspnet/Announcements](https://github.com/aspnet/Announcements/issues) GitHub repository.</span></span> <span data-ttu-id="5e006-114">Testowanie zgodności bibliotek można przeprowadzić w miarę opracowania funkcji platformy.</span><span class="sxs-lookup"><span data-stu-id="5e006-114">Compatibility testing of libraries can be conducted as framework features are being developed.</span></span>

## <a name="use-the-aspnet-core-shared-framework"></a><span data-ttu-id="5e006-115">Użyj ASP.NET Core udostępnionej platformy</span><span class="sxs-lookup"><span data-stu-id="5e006-115">Use the ASP.NET Core shared framework</span></span>

<span data-ttu-id="5e006-116">W wersji programu .NET Core 3,0 wiele zestawów ASP.NET Core nie jest już publikowanych w pakiecie NuGet jako pakiety.</span><span class="sxs-lookup"><span data-stu-id="5e006-116">With the release of .NET Core 3.0, many ASP.NET Core assemblies are no longer published to NuGet as packages.</span></span> <span data-ttu-id="5e006-117">Zamiast tego zestawy są zawarte w `Microsoft.AspNetCore.App` udostępnionej platformie, która jest instalowana z instalatorami zestaw .NET Core SDK i uruchomieniowymi.</span><span class="sxs-lookup"><span data-stu-id="5e006-117">Instead, the assemblies are included in the `Microsoft.AspNetCore.App` shared framework, which is installed with the .NET Core SDK and runtime installers.</span></span> <span data-ttu-id="5e006-118">Aby zapoznać się z listą pakietów, które nie są już publikowane, zobacz [usuwanie przestarzałych odwołań do pakietów](xref:migration/22-to-30#remove-obsolete-package-references).</span><span class="sxs-lookup"><span data-stu-id="5e006-118">For a list of packages no longer being published, see [Remove obsolete package references](xref:migration/22-to-30#remove-obsolete-package-references).</span></span>

<span data-ttu-id="5e006-119">Począwszy od programu .NET Core 3,0, projekty używające zestawu SDK programu `Microsoft.NET.Sdk.Web` MSBuild niejawnie odwołują się do udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="5e006-119">As of .NET Core 3.0, projects using the `Microsoft.NET.Sdk.Web` MSBuild SDK implicitly reference the shared framework.</span></span> <span data-ttu-id="5e006-120">Projekty używające zestawu SDK `Microsoft.NET.Sdk` lub `Microsoft.NET.Sdk.Razor` muszą odwoływać się do ASP.NET Core, aby używać ASP.NET Core interfejsów API w środowisku współdzielonym.</span><span class="sxs-lookup"><span data-stu-id="5e006-120">Projects using the `Microsoft.NET.Sdk` or `Microsoft.NET.Sdk.Razor` SDK must reference ASP.NET Core to use ASP.NET Core APIs in the shared framework.</span></span>

<span data-ttu-id="5e006-121">Aby odwołać się do ASP.NET Core, Dodaj następujący element `<FrameworkReference>` do pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="5e006-121">To reference ASP.NET Core, add the following `<FrameworkReference>` element to your project file:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj?highlight=8)]

<span data-ttu-id="5e006-122">Odwoływanie się ASP.NET Core w ten sposób jest obsługiwane tylko dla projektów przeznaczonych dla platformy .NET Core 3. x.</span><span class="sxs-lookup"><span data-stu-id="5e006-122">Referencing ASP.NET Core in this manner is only supported for projects targeting .NET Core 3.x.</span></span>

## <a name="include-opno-locblazor-extensibility"></a><span data-ttu-id="5e006-123">Uwzględnij rozszerzalność Blazor</span><span class="sxs-lookup"><span data-stu-id="5e006-123">Include Blazor extensibility</span></span>

Blazor<span data-ttu-id="5e006-124"> obsługuje modele webassembly (WASM) i serwery [hostingowe](xref:blazor/hosting-models).</span><span class="sxs-lookup"><span data-stu-id="5e006-124"> supports WebAssembly (WASM) and Server [hosting models](xref:blazor/hosting-models).</span></span> <span data-ttu-id="5e006-125">O ile nie istnieje szczególny powód, który nie należy do, biblioteka [składników Razor](xref:blazor/components) powinna obsługiwać oba modele hostingu.</span><span class="sxs-lookup"><span data-stu-id="5e006-125">Unless there's a specific reason not to, a [Razor components](xref:blazor/components) library should support both hosting models.</span></span> <span data-ttu-id="5e006-126">Biblioteka składników Razor musi używać zestawu SDK [Microsoft. NET. Sdk. Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="5e006-126">A Razor components library must use the [Microsoft.NET.Sdk.Razor SDK](xref:razor-pages/sdk).</span></span>

### <a name="support-both-hosting-models"></a><span data-ttu-id="5e006-127">Obsługa obu modeli hostingu</span><span class="sxs-lookup"><span data-stu-id="5e006-127">Support both hosting models</span></span>

<span data-ttu-id="5e006-128">Aby zapewnić obsługę użycia składnika Razor zarówno z [Blazor serwera](xref:blazor/hosting-models#blazor-server) , jak i [Blazor projektów WASM](xref:blazor/hosting-models#blazor-webassembly) , należy wykonać następujące instrukcje dla edytora.</span><span class="sxs-lookup"><span data-stu-id="5e006-128">To support Razor component consumption from both [Blazor Server](xref:blazor/hosting-models#blazor-server) and [Blazor WASM](xref:blazor/hosting-models#blazor-webassembly) projects, use the following instructions for your editor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e006-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e006-129">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5e006-130">Użyj szablonu projektu **biblioteki klas Razor** .</span><span class="sxs-lookup"><span data-stu-id="5e006-130">Use the **Razor Class Library** project template.</span></span> <span data-ttu-id="5e006-131">Pole wyboru **strony obsługi i widoki** szablonu powinno zostać odwybrane.</span><span class="sxs-lookup"><span data-stu-id="5e006-131">The template's **Support pages and views** checkbox should be deselected.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e006-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e006-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5e006-133">Uruchom następujące polecenie w zintegrowanym terminalu:</span><span class="sxs-lookup"><span data-stu-id="5e006-133">Run the following command in the integrated terminal:</span></span>

```dotnetcli
dotnet new razorclasslib
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5e006-134">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5e006-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5e006-135">Użyj szablonu projektu **biblioteki klas Razor** .</span><span class="sxs-lookup"><span data-stu-id="5e006-135">Use the **Razor Class Library** project template.</span></span>

---

<span data-ttu-id="5e006-136">Projekt wygenerowany na podstawie szablonu wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="5e006-136">The project generated from the template does the following things:</span></span>

* <span data-ttu-id="5e006-137">Elementy docelowe .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="5e006-137">Targets .NET Standard 2.0.</span></span>
* <span data-ttu-id="5e006-138">Ustawia właściwość `RazorLangVersion` na `3.0`.</span><span class="sxs-lookup"><span data-stu-id="5e006-138">Sets the `RazorLangVersion` property to `3.0`.</span></span> <span data-ttu-id="5e006-139">`3.0` jest wartością domyślną dla programu .NET Core 3. x.</span><span class="sxs-lookup"><span data-stu-id="5e006-139">`3.0` is the default value for .NET Core 3.x.</span></span>
* <span data-ttu-id="5e006-140">Dodaje następujące odwołania do pakietów:</span><span class="sxs-lookup"><span data-stu-id="5e006-140">Adds the following package references:</span></span>
  * [<span data-ttu-id="5e006-141">Microsoft. AspNetCore. Components</span><span class="sxs-lookup"><span data-stu-id="5e006-141">Microsoft.AspNetCore.Components</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Components)
  * [<span data-ttu-id="5e006-142">Microsoft. AspNetCore. Components. Web</span><span class="sxs-lookup"><span data-stu-id="5e006-142">Microsoft.AspNetCore.Components.Web</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Web)

<span data-ttu-id="5e006-143">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5e006-143">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-razor-components-library.csproj)]

### <a name="support-a-specific-hosting-model"></a><span data-ttu-id="5e006-144">Obsługa określonego modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="5e006-144">Support a specific hosting model</span></span>

<span data-ttu-id="5e006-145">Jest to bardzo mniej powszechne do obsługi jednego Blazor modelu hostingu.</span><span class="sxs-lookup"><span data-stu-id="5e006-145">It's far less common to support a single Blazor hosting model.</span></span> <span data-ttu-id="5e006-146">Przykładowo, aby obsługiwać użycie składnika Razor tylko z projektów [serweraBlazor](xref:blazor/hosting-models#blazor-server) :</span><span class="sxs-lookup"><span data-stu-id="5e006-146">As an example, to support Razor component consumption from [Blazor Server](xref:blazor/hosting-models#blazor-server) projects only:</span></span>

* <span data-ttu-id="5e006-147">Docelowy .NET Core 3. x.</span><span class="sxs-lookup"><span data-stu-id="5e006-147">Target .NET Core 3.x.</span></span>
* <span data-ttu-id="5e006-148">Dodaj element `<FrameworkReference>` dla struktury udostępnionej.</span><span class="sxs-lookup"><span data-stu-id="5e006-148">Add a `<FrameworkReference>` element for the shared framework.</span></span>

<span data-ttu-id="5e006-149">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5e006-149">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-razor-components-library.csproj)]

<span data-ttu-id="5e006-150">Aby uzyskać więcej informacji na temat bibliotek zawierających składniki Razor, zobacz [biblioteki klas składników razor ASP.NET Core](xref:blazor/class-libraries).</span><span class="sxs-lookup"><span data-stu-id="5e006-150">For more information on libraries containing Razor components, see [ASP.NET Core Razor components class libraries](xref:blazor/class-libraries).</span></span>

## <a name="include-mvc-extensibility"></a><span data-ttu-id="5e006-151">Uwzględnij rozszerzalność MVC</span><span class="sxs-lookup"><span data-stu-id="5e006-151">Include MVC extensibility</span></span>

<span data-ttu-id="5e006-152">W tej części przedstawiono zalecenia dotyczące bibliotek, które obejmują:</span><span class="sxs-lookup"><span data-stu-id="5e006-152">This section outlines recommendations for libraries that include:</span></span>

* [<span data-ttu-id="5e006-153">Widoki Razor lub Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5e006-153">Razor views or Razor Pages</span></span>](#razor-views-or-razor-pages)
* [<span data-ttu-id="5e006-154">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="5e006-154">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="5e006-155">Składniki widoków</span><span class="sxs-lookup"><span data-stu-id="5e006-155">View components</span></span>](#view-components)

<span data-ttu-id="5e006-156">Ta sekcja nie omawia wielu elementów docelowych w celu obsługi wielu wersji MVC.</span><span class="sxs-lookup"><span data-stu-id="5e006-156">This section doesn't discuss multi-targeting to support multiple versions of MVC.</span></span> <span data-ttu-id="5e006-157">Aby uzyskać wskazówki dotyczące obsługi wielu wersji ASP.NET Core, zobacz [Obsługa wielu ASP.NET Core wersji](#support-multiple-aspnet-core-versions).</span><span class="sxs-lookup"><span data-stu-id="5e006-157">For guidance on supporting multiple ASP.NET Core versions, see [Support multiple ASP.NET Core versions](#support-multiple-aspnet-core-versions).</span></span>

### <a name="razor-views-or-razor-pages"></a><span data-ttu-id="5e006-158">Widoki Razor lub Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5e006-158">Razor views or Razor Pages</span></span>

<span data-ttu-id="5e006-159">Projekt, który zawiera [widoki Razor](xref:mvc/views/overview) lub [Razor Pages](xref:razor-pages/index) musi korzystać z [zestawu SDK Microsoft. NET. Sdk. Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="5e006-159">A project that includes [Razor views](xref:mvc/views/overview) or [Razor Pages](xref:razor-pages/index) must use the [Microsoft.NET.Sdk.Razor SDK](xref:razor-pages/sdk).</span></span>

<span data-ttu-id="5e006-160">Jeśli projekt jest przeznaczony dla platformy .NET Core 3. x, wymaga:</span><span class="sxs-lookup"><span data-stu-id="5e006-160">If the project targets .NET Core 3.x, it requires:</span></span>

* <span data-ttu-id="5e006-161">Właściwość programu MSBuild `AddRazorSupportForMvc` ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="5e006-161">An `AddRazorSupportForMvc` MSBuild property set to `true`.</span></span>
* <span data-ttu-id="5e006-162">Element `<FrameworkReference>` dla struktury udostępnionej.</span><span class="sxs-lookup"><span data-stu-id="5e006-162">A `<FrameworkReference>` element for the shared framework.</span></span>

<span data-ttu-id="5e006-163">Szablon projektu **biblioteki klas Razor** spełnia powyższe wymagania dotyczące projektów przeznaczonych dla platformy .NET Core 3. x.</span><span class="sxs-lookup"><span data-stu-id="5e006-163">The **Razor Class Library** project template satisfies the preceding requirements for projects targeting .NET Core 3.x.</span></span> <span data-ttu-id="5e006-164">Wykonaj następujące instrukcje dla edytora.</span><span class="sxs-lookup"><span data-stu-id="5e006-164">Use the following instructions for your editor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e006-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e006-165">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5e006-166">Użyj szablonu projektu **biblioteki klas Razor** .</span><span class="sxs-lookup"><span data-stu-id="5e006-166">Use the **Razor Class Library** project template.</span></span> <span data-ttu-id="5e006-167">Należy zaznaczyć pole wyboru **strony i widoki pomocy technicznej** szablonu.</span><span class="sxs-lookup"><span data-stu-id="5e006-167">The template's **Support pages and views** checkbox should be selected.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e006-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e006-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5e006-169">Uruchom następujące polecenie w zintegrowanym terminalu:</span><span class="sxs-lookup"><span data-stu-id="5e006-169">Run the following command in the integrated terminal:</span></span>

```dotnetcli
dotnet new razorclasslib -s
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5e006-170">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="5e006-170">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5e006-171">W tej chwili nie ma obsługi szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="5e006-171">No project template support at this time.</span></span>

---

<span data-ttu-id="5e006-172">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5e006-172">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-razor-views-pages-library.csproj)]

<span data-ttu-id="5e006-173">Jeśli obiekt docelowy projektu .NET Standard zamiast tego wymagane jest odwołanie do pakietu [Microsoft. AspNetCore. MVC](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc) .</span><span class="sxs-lookup"><span data-stu-id="5e006-173">If the project targets .NET Standard instead, a [Microsoft.AspNetCore.Mvc](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc) package reference is required.</span></span> <span data-ttu-id="5e006-174">Pakiet `Microsoft.AspNetCore.Mvc` został przeniesiony do udostępnionej struktury w ASP.NET Core 3,0 i dlatego nie jest już opublikowany.</span><span class="sxs-lookup"><span data-stu-id="5e006-174">The `Microsoft.AspNetCore.Mvc` package moved into the shared framework in ASP.NET Core 3.0 and is therefore no longer published.</span></span> <span data-ttu-id="5e006-175">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5e006-175">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-razor-views-pages-library.csproj?highlight=8)]

### <a name="tag-helpers"></a><span data-ttu-id="5e006-176">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="5e006-176">Tag Helpers</span></span>

<span data-ttu-id="5e006-177">Projekt zawierający [pomocników tagów](xref:mvc/views/tag-helpers/intro) powinien korzystać z zestawu SDK `Microsoft.NET.Sdk`.</span><span class="sxs-lookup"><span data-stu-id="5e006-177">A project that includes [Tag Helpers](xref:mvc/views/tag-helpers/intro) should use the `Microsoft.NET.Sdk` SDK.</span></span> <span data-ttu-id="5e006-178">Jeśli celem jest .NET Core 3. x, Dodaj element `<FrameworkReference>` dla struktury udostępnionej.</span><span class="sxs-lookup"><span data-stu-id="5e006-178">If targeting .NET Core 3.x, add a `<FrameworkReference>` element for the shared framework.</span></span> <span data-ttu-id="5e006-179">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5e006-179">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj)]

<span data-ttu-id="5e006-180">Jeśli .NET Standard określania wartości docelowej (aby obsługiwać wersje starsze niż ASP.NET Core 3. x), Dodaj odwołanie do pakietu do [Microsoft. AspNetCore. MVC. Razor](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor).</span><span class="sxs-lookup"><span data-stu-id="5e006-180">If targeting .NET Standard (to support versions earlier than ASP.NET Core 3.x), add a package reference to [Microsoft.AspNetCore.Mvc.Razor](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor).</span></span> <span data-ttu-id="5e006-181">Pakiet `Microsoft.AspNetCore.Mvc.Razor` został przeniesiony do udostępnionej struktury i dlatego nie jest już opublikowany.</span><span class="sxs-lookup"><span data-stu-id="5e006-181">The `Microsoft.AspNetCore.Mvc.Razor` package moved into the shared framework and is therefore no longer published.</span></span> <span data-ttu-id="5e006-182">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5e006-182">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-tag-helpers-library.csproj)]

### <a name="view-components"></a><span data-ttu-id="5e006-183">Składniki widoku</span><span class="sxs-lookup"><span data-stu-id="5e006-183">View components</span></span>

<span data-ttu-id="5e006-184">Projekt, który zawiera [składniki widoku](xref:mvc/views/view-components) , powinien korzystać z zestawu SDK `Microsoft.NET.Sdk`.</span><span class="sxs-lookup"><span data-stu-id="5e006-184">A project that includes [View components](xref:mvc/views/view-components) should use the `Microsoft.NET.Sdk` SDK.</span></span> <span data-ttu-id="5e006-185">Jeśli celem jest .NET Core 3. x, Dodaj element `<FrameworkReference>` dla struktury udostępnionej.</span><span class="sxs-lookup"><span data-stu-id="5e006-185">If targeting .NET Core 3.x, add a `<FrameworkReference>` element for the shared framework.</span></span> <span data-ttu-id="5e006-186">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5e006-186">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj)]

<span data-ttu-id="5e006-187">Jeśli .NET Standard określania wartości docelowej (aby obsługiwać wersje starsze niż ASP.NET Core 3. x), Dodaj odwołanie do pakietu do [Microsoft. AspNetCore. MVC. ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures).</span><span class="sxs-lookup"><span data-stu-id="5e006-187">If targeting .NET Standard (to support versions earlier than ASP.NET Core 3.x), add a package reference to [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures).</span></span> <span data-ttu-id="5e006-188">Pakiet `Microsoft.AspNetCore.Mvc.ViewFeatures` został przeniesiony do udostępnionej struktury i dlatego nie jest już opublikowany.</span><span class="sxs-lookup"><span data-stu-id="5e006-188">The `Microsoft.AspNetCore.Mvc.ViewFeatures` package moved into the shared framework and is therefore no longer published.</span></span> <span data-ttu-id="5e006-189">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5e006-189">For example:</span></span>

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-view-components-library.csproj)]

## <a name="support-multiple-aspnet-core-versions"></a><span data-ttu-id="5e006-190">Obsługa wielu wersji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e006-190">Support multiple ASP.NET Core versions</span></span>

<span data-ttu-id="5e006-191">Do utworzenia biblioteki, która obsługuje wiele wariantów ASP.NET Core, wymagany jest wiele elementów docelowych.</span><span class="sxs-lookup"><span data-stu-id="5e006-191">Multi-targeting is required to author a library that supports multiple variants of ASP.NET Core.</span></span> <span data-ttu-id="5e006-192">Rozważmy scenariusz, w którym Biblioteka pomocników tagów musi obsługiwać następujące ASP.NET Core wariantów:</span><span class="sxs-lookup"><span data-stu-id="5e006-192">Consider a scenario in which a Tag Helpers library must support the following ASP.NET Core variants:</span></span>

* <span data-ttu-id="5e006-193">ASP.NET Core 2,1 .NET Framework 4.6.1</span><span class="sxs-lookup"><span data-stu-id="5e006-193">ASP.NET Core 2.1 targeting .NET Framework 4.6.1</span></span>
* <span data-ttu-id="5e006-194">ASP.NET Core 2. x przeznaczony dla platformy .NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="5e006-194">ASP.NET Core 2.x targeting .NET Core 2.x</span></span>
* <span data-ttu-id="5e006-195">ASP.NET Core 3. x przeznaczony dla platformy .NET Core 3. x</span><span class="sxs-lookup"><span data-stu-id="5e006-195">ASP.NET Core 3.x targeting .NET Core 3.x</span></span>

<span data-ttu-id="5e006-196">Następujący plik projektu obsługuje te warianty za pośrednictwem właściwości `TargetFrameworks`:</span><span class="sxs-lookup"><span data-stu-id="5e006-196">The following project file supports these variants via the `TargetFrameworks` property:</span></span>

[!code-xml[](target-aspnetcore/samples/multi-tfm/recommended-tag-helpers-library.csproj)]

<span data-ttu-id="5e006-197">Z poprzednim plikiem projektu:</span><span class="sxs-lookup"><span data-stu-id="5e006-197">With the preceding project file:</span></span>

* <span data-ttu-id="5e006-198">Pakiet `Markdig` zostanie dodany dla wszystkich odbiorców.</span><span class="sxs-lookup"><span data-stu-id="5e006-198">The `Markdig` package is added for all consumers.</span></span>
* <span data-ttu-id="5e006-199">Odwołanie do [Microsoft. AspNetCore. MVC. Razor](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor) jest dodawane dla klientów docelowych .NET Framework 4.6.1 lub nowszych lub .NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="5e006-199">A reference to [Microsoft.AspNetCore.Mvc.Razor](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor) is added for consumers targeting .NET Framework 4.6.1 or later or .NET Core 2.x.</span></span> <span data-ttu-id="5e006-200">Wersja 2.1.0 pakietu współpracuje z ASP.NET Core 2,2 ze względu na zgodność z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="5e006-200">Version 2.1.0 of the package works with ASP.NET Core 2.2 because of backwards compatibility.</span></span>
* <span data-ttu-id="5e006-201">Udostępnione środowisko jest przywoływane dla odbiorców przeznaczonych dla platformy .NET Core 3. x.</span><span class="sxs-lookup"><span data-stu-id="5e006-201">The shared framework is referenced for consumers targeting .NET Core 3.x.</span></span> <span data-ttu-id="5e006-202">Pakiet `Microsoft.AspNetCore.Mvc.Razor` jest dołączony do udostępnionej struktury.</span><span class="sxs-lookup"><span data-stu-id="5e006-202">The `Microsoft.AspNetCore.Mvc.Razor` package is included in the shared framework.</span></span>

<span data-ttu-id="5e006-203">Alternatywnie, .NET Standard 2,0 może być ukierunkowana zamiast określania zarówno platformy .NET Core 2,1, jak i .NET Framework 4.6.1:</span><span class="sxs-lookup"><span data-stu-id="5e006-203">Alternatively, .NET Standard 2.0 could be targeted instead of targeting both .NET Core 2.1 and .NET Framework 4.6.1:</span></span>

[!code-xml[](target-aspnetcore/samples/multi-tfm/alternative-tag-helpers-library.csproj?highlight=4)]

<span data-ttu-id="5e006-204">W poprzednim pliku projektu istnieją następujące zastrzeżenia:</span><span class="sxs-lookup"><span data-stu-id="5e006-204">With the preceding project file, the following caveats exist:</span></span>

* <span data-ttu-id="5e006-205">Ponieważ biblioteka zawiera tylko pomocników tagów, jest bardziej prosta dla konkretnych platform, na których ASP.NET Core są uruchamiane: .NET Core i .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5e006-205">Since the library only contains Tag Helpers, it's more straightforward to target the specific platforms on which ASP.NET Core runs: .NET Core and .NET Framework.</span></span> <span data-ttu-id="5e006-206">Pomocnicy tagów nie mogą być używani przez inne platformy docelowe zgodne z .NET Standard 2,0, takie jak Unity, platformy UWP i Xamarin.</span><span class="sxs-lookup"><span data-stu-id="5e006-206">Tag Helpers can't be used by other .NET Standard 2.0-compliant target frameworks such as Unity, UWP, and Xamarin.</span></span>
* <span data-ttu-id="5e006-207">Korzystanie z .NET Standard 2,0 z .NET Framework ma problemy, które zostały rozwiązane w .NET Framework 4.7.2.</span><span class="sxs-lookup"><span data-stu-id="5e006-207">Using .NET Standard 2.0 from .NET Framework has some issues that were addressed in .NET Framework 4.7.2.</span></span> <span data-ttu-id="5e006-208">Możesz ulepszyć środowisko dla klientów, korzystając .NET Framework 4.6.1 przez 4.7.1 przez kierowanie .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="5e006-208">You can improve the experience for consumers using .NET Framework 4.6.1 through 4.7.1 by targeting .NET Framework 4.6.1.</span></span>

<span data-ttu-id="5e006-209">Jeśli biblioteka wymaga wywołania interfejsów API specyficznych dla platformy, użyj konkretnych implementacji platformy .NET, a nie .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="5e006-209">If your library needs to call platform-specific APIs, target specific .NET implementations instead of .NET Standard.</span></span> <span data-ttu-id="5e006-210">Aby uzyskać więcej informacji, zobacz temat [wiele elementów docelowych](/dotnet/standard/library-guidance/cross-platform-targeting#multi-targeting).</span><span class="sxs-lookup"><span data-stu-id="5e006-210">For more information, see [Multi-targeting](/dotnet/standard/library-guidance/cross-platform-targeting#multi-targeting).</span></span>

## <a name="use-an-api-that-hasnt-changed"></a><span data-ttu-id="5e006-211">Użyj interfejsu API, który nie został zmieniony</span><span class="sxs-lookup"><span data-stu-id="5e006-211">Use an API that hasn't changed</span></span>

<span data-ttu-id="5e006-212">Wyobraź sobie scenariusz, w którym uaktualniasz bibliotekę oprogramowania pośredniczącego z programu .NET Core 2,2 do 3,0.</span><span class="sxs-lookup"><span data-stu-id="5e006-212">Imagine a scenario in which you're upgrading a middleware library from .NET Core 2.2 to 3.0.</span></span> <span data-ttu-id="5e006-213">ASP.NET Core interfejsy API oprogramowania pośredniczącego używane w bibliotece nie uległy zmianie między ASP.NET Core 2,2 i 3,0.</span><span class="sxs-lookup"><span data-stu-id="5e006-213">The ASP.NET Core middleware APIs being used in the library haven't changed between ASP.NET Core 2.2 and 3.0.</span></span> <span data-ttu-id="5e006-214">Aby nadal obsługiwać bibliotekę oprogramowania pośredniczącego w programie .NET Core 3,0, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="5e006-214">To continue supporting the middleware library in .NET Core 3.0, take the following steps:</span></span>

* <span data-ttu-id="5e006-215">Postępuj zgodnie ze [wskazówkami dotyczącymi biblioteki standardowej](/dotnet/standard/library-guidance/).</span><span class="sxs-lookup"><span data-stu-id="5e006-215">Follow the [standard library guidance](/dotnet/standard/library-guidance/).</span></span>
* <span data-ttu-id="5e006-216">Dodaj odwołanie do pakietu dla każdego pakietu NuGet interfejsu API, jeśli odpowiedni zestaw nie istnieje w udostępnionej platformie.</span><span class="sxs-lookup"><span data-stu-id="5e006-216">Add a package reference for each API's NuGet package if the corresponding assembly doesn't exist in the shared framework.</span></span>

## <a name="use-an-api-that-changed"></a><span data-ttu-id="5e006-217">Użyj interfejsu API, który został zmieniony</span><span class="sxs-lookup"><span data-stu-id="5e006-217">Use an API that changed</span></span>

<span data-ttu-id="5e006-218">Wyobraź sobie scenariusz, w którym biblioteka jest uaktualniana z platformy .NET Core 2,2 do programu .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="5e006-218">Imagine a scenario in which you're upgrading a library from .NET Core 2.2 to .NET Core 3.0.</span></span> <span data-ttu-id="5e006-219">Interfejs API ASP.NET Core używany w bibliotece ma [nieprzerwaną zmianę](/dotnet/core/compatibility/breaking-changes) w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="5e006-219">An ASP.NET Core API being used in the library has a [breaking change](/dotnet/core/compatibility/breaking-changes) in ASP.NET Core 3.0.</span></span> <span data-ttu-id="5e006-220">Rozważ, czy biblioteka może być ponownie zapisywana, aby nie używać uszkodzonego interfejsu API we wszystkich wersjach.</span><span class="sxs-lookup"><span data-stu-id="5e006-220">Consider whether the library can be rewritten to not use the broken API in all versions.</span></span>

<span data-ttu-id="5e006-221">Jeśli możesz ponownie zapisać bibliotekę, zrób to i przejdź do lokalizacji docelowej starszej platformy docelowej (na przykład .NET Standard 2,0 lub .NET Framework 4.6.1) z odwołaniami do pakietu.</span><span class="sxs-lookup"><span data-stu-id="5e006-221">If you can rewrite the library, do so and continue to target an earlier target framework (for example, .NET Standard 2.0 or .NET Framework 4.6.1) with package references.</span></span>

<span data-ttu-id="5e006-222">Jeśli nie możesz ponownie zapisać biblioteki, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="5e006-222">If you can't rewrite the library, take the following steps:</span></span>

* <span data-ttu-id="5e006-223">Dodaj element docelowy dla platformy .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="5e006-223">Add a target for .NET Core 3.0.</span></span>
* <span data-ttu-id="5e006-224">Dodaj element `<FrameworkReference>` dla struktury udostępnionej.</span><span class="sxs-lookup"><span data-stu-id="5e006-224">Add a `<FrameworkReference>` element for the shared framework.</span></span>
* <span data-ttu-id="5e006-225">Użyj [dyrektywy preprocesora #if](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if) z odpowiednim symbolem platformy docelowej do warunkowego kompilowania kodu.</span><span class="sxs-lookup"><span data-stu-id="5e006-225">Use the [#if preprocessor directive](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if) with the appropriate target framework symbol to conditionally compile code.</span></span>

<span data-ttu-id="5e006-226">Na przykład synchroniczne operacje odczytu i zapisu w strumieniach żądań HTTP i odpowiedzi są domyślnie wyłączone w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="5e006-226">For example, synchronous reads and writes on HTTP request and response streams are disabled by default as of ASP.NET Core 3.0.</span></span> <span data-ttu-id="5e006-227">ASP.NET Core 2,2 domyślnie obsługuje zachowanie synchroniczne.</span><span class="sxs-lookup"><span data-stu-id="5e006-227">ASP.NET Core 2.2 supports the synchronous behavior by default.</span></span> <span data-ttu-id="5e006-228">Rozważmy bibliotekę oprogramowania pośredniczącego, w której operacje odczytu i zapisu synchronicznego powinny być włączone w przypadku wystąpienia operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="5e006-228">Consider a middleware library in which synchronous reads and writes should be enabled where IO is occurring.</span></span> <span data-ttu-id="5e006-229">Biblioteka powinna zawierać kod w celu włączenia funkcji synchronicznych w odpowiedniej dyrektywie preprocesora.</span><span class="sxs-lookup"><span data-stu-id="5e006-229">The library should enclose the code to enable synchronous features in the appropriate preprocessor directive.</span></span> <span data-ttu-id="5e006-230">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5e006-230">For example:</span></span>

[!code-csharp[](target-aspnetcore/samples/middleware.cs?highlight=9-24)]

## <a name="use-an-api-introduced-in-30"></a><span data-ttu-id="5e006-231">Korzystanie z interfejsu API wprowadzonego w 3,0</span><span class="sxs-lookup"><span data-stu-id="5e006-231">Use an API introduced in 3.0</span></span>

<span data-ttu-id="5e006-232">Załóżmy, że chcesz użyć interfejsu API ASP.NET Core, który został wprowadzony w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="5e006-232">Imagine that you want to use an ASP.NET Core API that was introduced in ASP.NET Core 3.0.</span></span> <span data-ttu-id="5e006-233">Zastanów się nad następującymi pytaniami:</span><span class="sxs-lookup"><span data-stu-id="5e006-233">Consider the following questions:</span></span>

1. <span data-ttu-id="5e006-234">Czy biblioteka wymaga, aby nowy interfejs API był funkcjonalny?</span><span class="sxs-lookup"><span data-stu-id="5e006-234">Does the library functionally require the new API?</span></span>
1. <span data-ttu-id="5e006-235">Czy biblioteka może zaimplementować tę funkcję w inny sposób?</span><span class="sxs-lookup"><span data-stu-id="5e006-235">Can the library implement this feature in a different way?</span></span>

<span data-ttu-id="5e006-236">Jeśli biblioteka funkcjonalnie wymaga interfejsu API i nie ma możliwości wdrożenia go na poziomie:</span><span class="sxs-lookup"><span data-stu-id="5e006-236">If the library functionally requires the API and there's no way to implement it down-level:</span></span>

* <span data-ttu-id="5e006-237">Tylko docelowy .NET Core 3. x.</span><span class="sxs-lookup"><span data-stu-id="5e006-237">Target .NET Core 3.x only.</span></span>
* <span data-ttu-id="5e006-238">Dodaj element `<FrameworkReference>` dla struktury udostępnionej.</span><span class="sxs-lookup"><span data-stu-id="5e006-238">Add a `<FrameworkReference>` element for the shared framework.</span></span>

<span data-ttu-id="5e006-239">Jeśli biblioteka może zaimplementować funkcję w inny sposób:</span><span class="sxs-lookup"><span data-stu-id="5e006-239">If the library can implement the feature in a different way:</span></span>

* <span data-ttu-id="5e006-240">Dodaj platformę .NET Core 3. x jako platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="5e006-240">Add .NET Core 3.x as a target framework.</span></span>
* <span data-ttu-id="5e006-241">Dodaj element `<FrameworkReference>` dla struktury udostępnionej.</span><span class="sxs-lookup"><span data-stu-id="5e006-241">Add a `<FrameworkReference>` element for the shared framework.</span></span>
* <span data-ttu-id="5e006-242">Użyj [dyrektywy preprocesora #if](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if) z odpowiednim symbolem platformy docelowej do warunkowego kompilowania kodu.</span><span class="sxs-lookup"><span data-stu-id="5e006-242">Use the [#if preprocessor directive](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if) with the appropriate target framework symbol to conditionally compile code.</span></span>

<span data-ttu-id="5e006-243">Na przykład poniższy pomocnik tagów używa interfejsu <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> wprowadzonego w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="5e006-243">For example, the following Tag Helper uses the <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> interface introduced in ASP.NET Core 3.0.</span></span> <span data-ttu-id="5e006-244">Odbiorcy korzystający z platformy .NET Core 3,0 wykonują ścieżkę kodu zdefiniowaną przez symbol `NETCOREAPP3_0` docelowej platformy.</span><span class="sxs-lookup"><span data-stu-id="5e006-244">Consumers targeting .NET Core 3.0 execute the code path defined by the `NETCOREAPP3_0` target framework symbol.</span></span> <span data-ttu-id="5e006-245">Typ parametru konstruktora pomocnika tagów zmieni się na <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> dla klientów .NET Core 2,1 i .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="5e006-245">The Tag Helper's constructor parameter type changes to <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> for .NET Core 2.1 and .NET Framework 4.6.1 consumers.</span></span> <span data-ttu-id="5e006-246">Ta zmiana była niezbędna, ponieważ ASP.NET Core 3,0 `IHostingEnvironment` oznaczona jako przestarzała i zalecana `IWebHostEnvironment` jako zamiennik.</span><span class="sxs-lookup"><span data-stu-id="5e006-246">This change was necessary because ASP.NET Core 3.0 marked `IHostingEnvironment` as obsolete and recommended `IWebHostEnvironment` as the replacement.</span></span>

```csharp
[HtmlTargetElement("script", Attributes = "asp-inline")]
public class ScriptInliningTagHelper : TagHelper
{
    private readonly IFileProvider _wwwroot;

#if NETCOREAPP3_0
    public ScriptInliningTagHelper(IWebHostEnvironment env)
#else
    public ScriptInliningTagHelper(IHostingEnvironment env)
#endif
    {
        _wwwroot = env.WebRootFileProvider;
    }

    // code omitted for brevity
}
```

<span data-ttu-id="5e006-247">Następujący plik projektu wielowymiarowego obsługuje ten scenariusz pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="5e006-247">The following multi-targeted project file supports this Tag Helper scenario:</span></span>

[!code-xml[](target-aspnetcore/samples/multi-tfm/recommended-tag-helpers-library.csproj)]

## <a name="use-an-api-removed-from-the-shared-framework"></a><span data-ttu-id="5e006-248">Użyj interfejsu API usuniętego z udostępnionej platformy</span><span class="sxs-lookup"><span data-stu-id="5e006-248">Use an API removed from the shared framework</span></span>

<span data-ttu-id="5e006-249">Aby użyć zestawu ASP.NET Core, który został usunięty z platformy udostępnionej, Dodaj odpowiednie odwołanie do pakietu.</span><span class="sxs-lookup"><span data-stu-id="5e006-249">To use an ASP.NET Core assembly that was removed from the shared framework, add the appropriate package reference.</span></span> <span data-ttu-id="5e006-250">Aby uzyskać listę pakietów usuniętych z udostępnionej platformy w ASP.NET Core 3,0, zobacz [usuwanie przestarzałych odwołań do pakietów](xref:migration/22-to-30#remove-obsolete-package-references).</span><span class="sxs-lookup"><span data-stu-id="5e006-250">For a list of packages removed from the shared framework in ASP.NET Core 3.0, see [Remove obsolete package references](xref:migration/22-to-30#remove-obsolete-package-references).</span></span>

<span data-ttu-id="5e006-251">Na przykład, aby dodać klienta internetowego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="5e006-251">For example, to add the web API client:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp3.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <FrameworkReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNet.WebApi.Client" Version="5.2.7" />
  </ItemGroup>

</Project>
```

## <a name="additional-resources"></a><span data-ttu-id="5e006-252">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5e006-252">Additional resources</span></span>

* <xref:razor-pages/ui-class>
* <xref:blazor/class-libraries>
* [<span data-ttu-id="5e006-253">Obsługa implementacji platformy .NET</span><span class="sxs-lookup"><span data-stu-id="5e006-253">.NET implementation support</span></span>](/dotnet/standard/net-standard#net-implementation-support)
* [<span data-ttu-id="5e006-254">Zasady pomocy technicznej platformy .NET</span><span class="sxs-lookup"><span data-stu-id="5e006-254">.NET support policies</span></span>](https://dotnet.microsoft.com/platform/support/policy)
