---
title: Kompilacja pliku Razor w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak kompilacja plików Razor występuje w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 95fa0d72ed9c088945707ac6b79c3fbde35a5a30
ms.sourcegitcommit: eb2fe5ad2e82fab86ca952463af8d017ba659b25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2019
ms.locfileid: "73416148"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="61f61-103">Kompilacja pliku Razor w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61f61-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="61f61-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="61f61-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="61f61-105">Plik Razor jest kompilowany w czasie wykonywania, gdy jest wywoływany skojarzony widok MVC.</span><span class="sxs-lookup"><span data-stu-id="61f61-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="61f61-106">Publikowanie plików Razor w czasie kompilacji nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="61f61-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="61f61-107">Pliki Razor mogą być opcjonalnie kompilowane w czasie publikowania i wdrażane za pomocą aplikacji&mdash;przy użyciu narzędzia prekompilacji.</span><span class="sxs-lookup"><span data-stu-id="61f61-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="61f61-108">Plik Razor jest kompilowany w czasie wykonywania, gdy zostanie wywołana skojarzona Strona Razor lub widok MVC.</span><span class="sxs-lookup"><span data-stu-id="61f61-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="61f61-109">Publikowanie plików Razor w czasie kompilacji nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="61f61-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="61f61-110">Pliki Razor mogą być opcjonalnie kompilowane w czasie publikowania i wdrażane za pomocą aplikacji&mdash;przy użyciu narzędzia prekompilacji.</span><span class="sxs-lookup"><span data-stu-id="61f61-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="61f61-111">Plik Razor jest kompilowany w czasie wykonywania, gdy zostanie wywołana skojarzona Strona Razor lub widok MVC.</span><span class="sxs-lookup"><span data-stu-id="61f61-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="61f61-112">Pliki Razor są kompilowane w czasie kompilacji i publikowania przy użyciu [zestawu Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="61f61-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="61f61-113">Pliki Razor z rozszerzeniem *. cshtml* są kompilowane w czasie kompilacji i publikowania przy użyciu [zestawu Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="61f61-113">Razor files with a *.cshtml* extension are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="61f61-114">Kompilacja środowiska uruchomieniowego może być opcjonalnie włączona przez skonfigurowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="61f61-114">Runtime compilation may be optionally enabled by configuring your application.</span></span>

::: moniker-end

## <a name="razor-compilation"></a><span data-ttu-id="61f61-115">Kompilacja Razor</span><span class="sxs-lookup"><span data-stu-id="61f61-115">Razor compilation</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="61f61-116">Kompilacja i czas publikowania pliki Razor są domyślnie włączone w zestawie SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="61f61-116">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="61f61-117">Gdy ta funkcja jest włączona, Kompilacja środowiska uruchomieniowego uzupełnia kompilację w czasie kompilacji, umożliwiając aktualizowanie plików Razor, jeśli są edytowane.</span><span class="sxs-lookup"><span data-stu-id="61f61-117">When enabled, runtime compilation complements build-time compilation, allowing Razor files to be updated if they are edited.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="61f61-118">Kompilacja i czas publikowania pliki Razor są domyślnie włączone w zestawie SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="61f61-118">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="61f61-119">Edytowanie plików Razor po ich aktualizacji jest obsługiwane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="61f61-119">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="61f61-120">Domyślnie tylko skompilowane *widoki. dll* i brak plików *. cshtml* lub zestawy odwołań wymagane do kompilowania plików Razor są wdrażane przy użyciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="61f61-120">By default, only the compiled *Views.dll* and no *.cshtml* files or references assemblies required to compile Razor files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61f61-121">Narzędzie prekompilacji jest przestarzałe i zostanie usunięte w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="61f61-121">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="61f61-122">Zalecamy Migrowanie do [zestawu Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="61f61-122">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="61f61-123">Zestaw SDK Razor działa tylko wtedy, gdy w pliku projektu nie są ustawione właściwości specyficzne dla wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="61f61-123">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="61f61-124">Na przykład ustawienie właściwości `MvcRazorCompileOnPublish` pliku *. csproj* na `true` powoduje wyłączenie zestawu Razor SDK.</span><span class="sxs-lookup"><span data-stu-id="61f61-124">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="61f61-125">Jeśli projekt jest przeznaczony .NET Framework, zainstaluj pakiet NuGet [Microsoft. AspNetCore. MVC. Razor. ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :</span><span class="sxs-lookup"><span data-stu-id="61f61-125">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="61f61-126">Jeśli projekt jest przeznaczony dla platformy .NET Core, nie są wymagane żadne zmiany.</span><span class="sxs-lookup"><span data-stu-id="61f61-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="61f61-127">Szablony projektu ASP.NET Core 2. x jawnie domyślnie ustawiają Właściwość `MvcRazorCompileOnPublish` na `true`.</span><span class="sxs-lookup"><span data-stu-id="61f61-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="61f61-128">W związku z tym ten element może zostać bezpiecznie usunięty z pliku *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="61f61-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61f61-129">Narzędzie prekompilacji jest przestarzałe i zostanie usunięte w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="61f61-129">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="61f61-130">Zalecamy Migrowanie do [zestawu Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="61f61-130">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="61f61-131">Prekompilacja pliku Razor jest niedostępna podczas wykonywania niezależnego [wdrożenia (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) w ASP.NET Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="61f61-131">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="61f61-132">Ustaw właściwość `MvcRazorCompileOnPublish` na `true`i zainstaluj pakiet NuGet [Microsoft. AspNetCore. MVC. Razor. ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) .</span><span class="sxs-lookup"><span data-stu-id="61f61-132">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="61f61-133">Poniższy przykład *. csproj* przedstawia następujące ustawienia:</span><span class="sxs-lookup"><span data-stu-id="61f61-133">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="61f61-134">Przygotuj aplikację dla [wdrożenia zależnego od platformy](/dotnet/core/deploying/#framework-dependent-deployments-fdd) przy użyciu [polecenia interfejs wiersza polecenia platformy .NET Core Publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="61f61-134">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="61f61-135">Na przykład wykonaj następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="61f61-135">For example, execute the following command at the project root:</span></span>

```dotnetcli
dotnet publish -c Release
```

<span data-ttu-id="61f61-136">*\<Project_Name. Plik PrecompiledViews. dll* zawierający skompilowane pliki Razor jest tworzony, gdy kompilacja zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="61f61-136">A *\<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="61f61-137">Na przykład poniższy zrzut ekranu przedstawia zawartość *index. cshtml* w *WebApplication1. PrecompiledViews. dll*:</span><span class="sxs-lookup"><span data-stu-id="61f61-137">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Widoki Razor wewnątrz biblioteki DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a><span data-ttu-id="61f61-139">Kompilacja środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="61f61-139">Runtime compilation</span></span>

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="61f61-140">Kompilacja w czasie kompilacji jest uzupełniana przez kompilację plików Razor w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="61f61-140">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="61f61-141">ASP.NET Core MVC będzie ponownie kompilować pliki Razor, gdy zostanie zmieniona zawartość pliku *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="61f61-141">ASP.NET Core MVC will recompile Razor files when the contents of a *.cshtml* file change.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="61f61-142">Kompilacja w czasie kompilacji jest uzupełniana przez kompilację plików Razor w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="61f61-142">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="61f61-143"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> Pobiera lub ustawia wartość określającą, czy pliki Razor (widoki Razor i Razor Pages) zostaną ponownie skompilowane i zaktualizowane w przypadku zmiany plików na dysku.</span><span class="sxs-lookup"><span data-stu-id="61f61-143">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="61f61-144">Wartość domyślna to `true` dla:</span><span class="sxs-lookup"><span data-stu-id="61f61-144">The default value is `true` for:</span></span>

* <span data-ttu-id="61f61-145">Jeśli wersja zgodności aplikacji jest ustawiona na <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> lub wcześniejszą</span><span class="sxs-lookup"><span data-stu-id="61f61-145">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier</span></span>
* <span data-ttu-id="61f61-146">Jeśli wersja zgodności aplikacji jest ustawiona na <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> lub nowsza, a aplikacja znajduje się w <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="61f61-146">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> or later and the app is in the Development environment <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span></span> <span data-ttu-id="61f61-147">Inaczej mówiąc, pliki Razor nie będą ponownie kompilowane w środowisku innym niż programowanie, chyba że <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> jest jawnie ustawiona.</span><span class="sxs-lookup"><span data-stu-id="61f61-147">In other words, Razor files would not recompile in non-Development environment unless <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is explicitly set.</span></span>

<span data-ttu-id="61f61-148">Aby uzyskać wskazówki i przykłady dotyczące ustawiania wersji zgodności aplikacji, zobacz <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="61f61-148">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="61f61-149">Kompilacja środowiska uruchomieniowego jest włączona przy użyciu pakietu `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation`.</span><span class="sxs-lookup"><span data-stu-id="61f61-149">Runtime compilation is enabled using the `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` package.</span></span> <span data-ttu-id="61f61-150">Aby włączyć kompilację środowiska uruchomieniowego, aplikacje muszą:</span><span class="sxs-lookup"><span data-stu-id="61f61-150">To enable runtime compilation, apps must:</span></span>

* <span data-ttu-id="61f61-151">Zainstaluj pakiet NuGet [Microsoft. AspNetCore. MVC. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) .</span><span class="sxs-lookup"><span data-stu-id="61f61-151">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>
* <span data-ttu-id="61f61-152">Zaktualizuj metodę `Startup.ConfigureServices` projektu w celu uwzględnienia wywołania `AddRazorRuntimeCompilation`:</span><span class="sxs-lookup"><span data-stu-id="61f61-152">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`:</span></span>

  ```csharp
  services
      .AddControllersWithViews()
      .AddRazorRuntimeCompilation();
  ```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="61f61-153">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="61f61-153">Additional resources</span></span>

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
