---
title: Kompilacja pliku razor w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak kompilacji plikach Razor odbywa się w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 0b3aea584de63cb8032e4ca112d2441349bdfbb3
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345502"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="e3b02-103">Kompilacja pliku razor w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3b02-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="e3b02-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e3b02-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="e3b02-105">Pliku Razor jest kompilowana w czasie wykonywania, gdy jest wywoływany skojarzonego widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="e3b02-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="e3b02-106">Publikowanie pliku Razor w czasie kompilacji nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="e3b02-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="e3b02-107">Opcjonalnie można kompilować plikach razor na czas publikacji i wdrożonych przy użyciu aplikacji&mdash;przy użyciu narzędzia wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e3b02-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e3b02-108">Pliku Razor jest kompilowana w czasie wykonywania, po wywołaniu skojarzonego widoku Razor strony lub MVC.</span><span class="sxs-lookup"><span data-stu-id="e3b02-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="e3b02-109">Publikowanie pliku Razor w czasie kompilacji nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="e3b02-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="e3b02-110">Opcjonalnie można kompilować plikach razor na czas publikacji i wdrożonych przy użyciu aplikacji&mdash;przy użyciu narzędzia wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e3b02-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="e3b02-111">Pliku Razor jest kompilowana w czasie wykonywania, po wywołaniu skojarzonego widoku Razor strony lub MVC.</span><span class="sxs-lookup"><span data-stu-id="e3b02-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="e3b02-112">Pliki razor są kompilowane w obu kompilacji i opublikować przy użyciu [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="e3b02-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e3b02-113">Pliki razor są kompilowane w obu kompilacji i opublikować przy użyciu [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="e3b02-113">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="e3b02-114">Kompilacja środowiska uruchomieniowego może być opcjonalnie włączona konfigurując aplikację.</span><span class="sxs-lookup"><span data-stu-id="e3b02-114">Runtime compilation may be optionally enabled by configuring your application.</span></span>

::: moniker-end

## <a name="razor-compilation"></a><span data-ttu-id="e3b02-115">Kompilacja razor</span><span class="sxs-lookup"><span data-stu-id="e3b02-115">Razor compilation</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="e3b02-116">Kompilacja i publikowania w czasie kompilacji plików Razor jest domyślnie włączona, przez zestaw SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="e3b02-116">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="e3b02-117">Po włączeniu kompilacja środowiska uruchomieniowego uzupełnienie utworzy czasu kompilacji, dzięki czemu plikach Razor do aktualizacji, jeśli są one editied.</span><span class="sxs-lookup"><span data-stu-id="e3b02-117">When enabled, runtime compilation, will complement build time compilation allowing Razor files to be updated if they are editied.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="e3b02-118">Kompilacja i publikowania w czasie kompilacji plików Razor jest domyślnie włączona, przez zestaw SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="e3b02-118">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="e3b02-119">Po te są aktualizowane w plikach Razor do edycji jest obsługiwana w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e3b02-119">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="e3b02-120">Domyślnie tylko skompilowanych *Views.dll* i nie *.cshtml* plików lub odwołania do zestawów wymagane do kompilowania plików Razor są wdrażane przy użyciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e3b02-120">By default, only the compiled *Views.dll* and no *.cshtml* files or references assemblies required to compile Razor files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e3b02-121">Narzędzie wstępnej kompilacji jest przestarzała i zostanie usunięta w programie ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="e3b02-121">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="e3b02-122">Zalecamy przeprowadzić migrację do [Razor Sdk](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="e3b02-122">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="e3b02-123">Zestaw Razor SDK jest efektywne tylko wtedy, gdy brak właściwości specyficzne dla wstępnej kompilacji są ustawione w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="e3b02-123">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="e3b02-124">Na przykład ustawienie *.csproj* pliku `MvcRazorCompileOnPublish` właściwość `true` wyłącza zestaw Razor SDK.</span><span class="sxs-lookup"><span data-stu-id="e3b02-124">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e3b02-125">Jeśli projekt jest przeznaczony dla .NET Framework, należy zainstalować [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) pakietu NuGet:</span><span class="sxs-lookup"><span data-stu-id="e3b02-125">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="e3b02-126">Jeśli projekt jest przeznaczony dla platformy .NET Core, żadne zmiany nie są niezbędne.</span><span class="sxs-lookup"><span data-stu-id="e3b02-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="e3b02-127">Szablony projektów programu ASP.NET Core 2.x niejawnie ustaw `MvcRazorCompileOnPublish` właściwość `true` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="e3b02-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="e3b02-128">W związku z tym, ten element można bezpiecznie usunąć z *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="e3b02-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e3b02-129">Narzędzie wstępnej kompilacji jest przestarzała i zostanie usunięta w programie ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="e3b02-129">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="e3b02-130">Zalecamy przeprowadzić migrację do [Razor Sdk](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="e3b02-130">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="e3b02-131">Wstępnej kompilacji pliku razor jest niedostępna podczas wykonywania [niezależna wdrożenia (— SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) programu ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e3b02-131">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="e3b02-132">Ustaw `MvcRazorCompileOnPublish` właściwości `true`i zainstaluj [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="e3b02-132">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="e3b02-133">Następujące *.csproj* przykładzie wyróżniono tych ustawień:</span><span class="sxs-lookup"><span data-stu-id="e3b02-133">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="e3b02-134">Przygotowywanie aplikacji dla [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd) z [polecenia publikowania interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="e3b02-134">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="e3b02-135">Na przykład wykonaj następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="e3b02-135">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="e3b02-136">A  *\<project_name >. PrecompiledViews.dll* zawierający skompilowane pliki Razor jest generowany po pomyślnym zakończeniu wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e3b02-136">A *\<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="e3b02-137">Na przykład, poniższy zrzut ekranu przedstawia zawartość *Index.cshtml* w ramach *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="e3b02-137">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Widokami razor wewnątrz biblioteki DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a><span data-ttu-id="e3b02-139">Kompilacja środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="e3b02-139">Runtime compilation</span></span>

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="e3b02-140">Czas kompilacji, kompilacja jest uzupełniana przez kompilacja środowiska uruchomieniowego plików Razor.</span><span class="sxs-lookup"><span data-stu-id="e3b02-140">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="e3b02-141">Platforma ASP.NET Core MVC będzie ponownie kompilować Razor pliki, gdy zawartość *.cshtml* zmianę pliku.</span><span class="sxs-lookup"><span data-stu-id="e3b02-141">ASP.NET Core MVC will recompile Razor files when the contents of a *.cshtml* file change.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="e3b02-142">Czas kompilacji, kompilacja jest uzupełniana przez kompilacja środowiska uruchomieniowego plików Razor.</span><span class="sxs-lookup"><span data-stu-id="e3b02-142">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="e3b02-143"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> Pobiera lub ustawia wartość określającą, jeśli pliki Razor (widokami Razor i stron Razor) są ponownie kompilowane i zaktualizowany w przypadku plików zmienią się na dysku.</span><span class="sxs-lookup"><span data-stu-id="e3b02-143">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="e3b02-144">Wartość domyślna to `true` dla:</span><span class="sxs-lookup"><span data-stu-id="e3b02-144">The default value is `true` for:</span></span>

* <span data-ttu-id="e3b02-145">Jeśli wersja zgodności aplikacji jest równa <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> lub starszy</span><span class="sxs-lookup"><span data-stu-id="e3b02-145">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier</span></span>
* <span data-ttu-id="e3b02-146">Wersja zgodności aplikacji czy zestaw do zestawu do <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> lub nowszej, a aplikacja jest w środowisku programistycznym <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span><span class="sxs-lookup"><span data-stu-id="e3b02-146">If the app's compatibility version is set to set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> or later and the app is in the Development environment <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span></span> <span data-ttu-id="e3b02-147">Innymi słowy, plikach Razor będzie nie należy ponownie skompilować w środowisku programistycznym bez chyba że <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> jest jawnie ustawione.</span><span class="sxs-lookup"><span data-stu-id="e3b02-147">In other words, Razor files would not recompile in non-Development environment unless <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is explicitly set.</span></span>

<span data-ttu-id="e3b02-148">Wskazówki i przykłady ustawienie wersji zgodności aplikacji, zobacz <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="e3b02-148">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e3b02-149">Kompilacja środowiska uruchomieniowego jest włączane przy użyciu `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` pakietu.</span><span class="sxs-lookup"><span data-stu-id="e3b02-149">Runtime compilation is enabled using the `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` package.</span></span> <span data-ttu-id="e3b02-150">Aby włączyć kompilacja środowiska uruchomieniowego, aplikacje muszą:</span><span class="sxs-lookup"><span data-stu-id="e3b02-150">To enable runtime compilation, apps must:</span></span>

* <span data-ttu-id="e3b02-151">Zainstaluj [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="e3b02-151">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>
* <span data-ttu-id="e3b02-152">Aktualizuj aplikację `ConfigureServices` obejmujący wywołanie `AddMvcRazorRuntimeCompilation`:</span><span class="sxs-lookup"><span data-stu-id="e3b02-152">Update the application's `ConfigureServices` to include a call to `AddMvcRazorRuntimeCompilation`:</span></span>

  ```csharp
  services
      .AddMvc()
      .AddMvcRazorRuntimeCompilation()
  ```

<span data-ttu-id="e3b02-153">Kompilacja środowiska uruchomieniowego do pracy po wdrożeniu, aplikacje Ponadto należy zmodyfikować swoje pliki projektu, aby ustawić `PreserveCompilationReferences` do `true`.</span><span class="sxs-lookup"><span data-stu-id="e3b02-153">For runtime compilation to work when deployed, apps must additionally modify their project files to set the `PreserveCompilationReferences` to `true`.</span></span>

[!code-xml[](view-compilation/sample/RuntimeCompilation.csproj?highlight=3)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="e3b02-154">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e3b02-154">Additional resources</span></span>

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
