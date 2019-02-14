---
title: Kompilowanie pliku razor i wstępnej kompilacji w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat zalet wstępnej kompilacji w plikach Razor i sposób wykonania wstępnej kompilacji pliku Razor, w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: c4e8f722fdf3d3f64807cc35ff9f349af7f32abd
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248189"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="6e936-103">Kompilacja pliku razor w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e936-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="6e936-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6e936-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="6e936-105">Pliku Razor jest kompilowana w czasie wykonywania, gdy jest wywoływany skojarzonego widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="6e936-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="6e936-106">Publikowanie pliku Razor w czasie kompilacji nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="6e936-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="6e936-107">Opcjonalnie można kompilować plikach razor na czas publikacji i wdrożonych przy użyciu aplikacji&mdash;przy użyciu narzędzia wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6e936-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6e936-108">Pliku Razor jest kompilowana w czasie wykonywania, po wywołaniu skojarzonego widoku Razor strony lub MVC.</span><span class="sxs-lookup"><span data-stu-id="6e936-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="6e936-109">Publikowanie pliku Razor w czasie kompilacji nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="6e936-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="6e936-110">Opcjonalnie można kompilować plikach razor na czas publikacji i wdrożonych przy użyciu aplikacji&mdash;przy użyciu narzędzia wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6e936-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6e936-111">Pliku Razor jest kompilowana w czasie wykonywania, po wywołaniu skojarzonego widoku Razor strony lub MVC.</span><span class="sxs-lookup"><span data-stu-id="6e936-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="6e936-112">Pliki razor są kompilowane w obu kompilacji i opublikować przy użyciu [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="6e936-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="6e936-113">Zagadnienia dotyczące wstępnej kompilacji</span><span class="sxs-lookup"><span data-stu-id="6e936-113">Precompilation considerations</span></span>

<span data-ttu-id="6e936-114">Prekompilowanie plikach Razor efekty uboczne są następujące:</span><span class="sxs-lookup"><span data-stu-id="6e936-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="6e936-115">Mniejsze opublikowany pakiet</span><span class="sxs-lookup"><span data-stu-id="6e936-115">A smaller published bundle</span></span>
* <span data-ttu-id="6e936-116">Krótszy czas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="6e936-116">A faster startup time</span></span>
* <span data-ttu-id="6e936-117">Nie można edytować pliki Razor&mdash;Brak powiązanej zawartości z opublikowanych pakietu.</span><span class="sxs-lookup"><span data-stu-id="6e936-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="6e936-118">Wdrażanie wstępnie skompilowanych plików</span><span class="sxs-lookup"><span data-stu-id="6e936-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6e936-119">Kompilacja i publikowania w czasie kompilacji plików Razor jest domyślnie włączona, przez zestaw SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="6e936-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="6e936-120">Po te są aktualizowane w plikach Razor do edycji jest obsługiwana w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6e936-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="6e936-121">Domyślnie tylko skompilowanych *Views.dll* i nie *.cshtml* pliki zostały wdrożone za pomocą aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e936-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6e936-122">Narzędzie wstępnej kompilacji zostaną usunięte w programie ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="6e936-122">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="6e936-123">Zalecamy przeprowadzić migrację do [Razor Sdk](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="6e936-123">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="6e936-124">Zestaw Razor SDK jest efektywne tylko wtedy, gdy brak właściwości specyficzne dla wstępnej kompilacji są ustawione w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="6e936-124">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="6e936-125">Na przykład ustawienie *.csproj* pliku `MvcRazorCompileOnPublish` właściwość `true` wyłącza zestaw Razor SDK.</span><span class="sxs-lookup"><span data-stu-id="6e936-125">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6e936-126">Jeśli projekt jest przeznaczony dla .NET Framework, należy zainstalować [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) pakietu NuGet:</span><span class="sxs-lookup"><span data-stu-id="6e936-126">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="6e936-127">Jeśli projekt jest przeznaczony dla platformy .NET Core, żadne zmiany nie są niezbędne.</span><span class="sxs-lookup"><span data-stu-id="6e936-127">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="6e936-128">Szablony projektów programu ASP.NET Core 2.x niejawnie ustaw `MvcRazorCompileOnPublish` właściwość `true` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="6e936-128">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="6e936-129">W związku z tym, ten element można bezpiecznie usunąć z *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="6e936-129">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6e936-130">Narzędzie wstępnej kompilacji zostaną usunięte w programie ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="6e936-130">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="6e936-131">Zalecamy przeprowadzić migrację do [Razor Sdk](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="6e936-131">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="6e936-132">Wstępnej kompilacji pliku razor jest niedostępna podczas wykonywania [niezależna wdrożenia (— SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) programu ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="6e936-132">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="6e936-133">Ustaw `MvcRazorCompileOnPublish` właściwości `true`i zainstaluj [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="6e936-133">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="6e936-134">Następujące *.csproj* przykładzie wyróżniono tych ustawień:</span><span class="sxs-lookup"><span data-stu-id="6e936-134">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="6e936-135">Przygotowywanie aplikacji dla [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd) z [polecenia publikowania interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="6e936-135">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="6e936-136">Na przykład wykonaj następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="6e936-136">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="6e936-137">A *< project_name >. PrecompiledViews.dll* zawierający skompilowane pliki Razor jest generowany po pomyślnym zakończeniu wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6e936-137">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="6e936-138">Na przykład, poniższy zrzut ekranu przedstawia zawartość *Index.cshtml* w ramach *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="6e936-138">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Widokami razor wewnątrz biblioteki DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="recompile-razor-files-on-change"></a><span data-ttu-id="6e936-140">Skompiluj ponownie plikach Razor przy zmianie</span><span class="sxs-lookup"><span data-stu-id="6e936-140">Recompile Razor files on change</span></span>

<span data-ttu-id="6e936-141"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> Pobiera lub ustawia wartość określającą, jeśli pliki Razor (widokami Razor i stron Razor) są ponownie kompilowane i zaktualizowany w przypadku plików zmienią się na dysku.</span><span class="sxs-lookup"><span data-stu-id="6e936-141">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="6e936-142">Po ustawieniu `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) skonfigurowane zegarki zmian w plikach Razor w <xref:Microsoft.Extensions.FileProviders.IFileProvider> wystąpień.</span><span class="sxs-lookup"><span data-stu-id="6e936-142">When set to `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) watches for changes to Razor files in configured <xref:Microsoft.Extensions.FileProviders.IFileProvider> instances.</span></span>

<span data-ttu-id="6e936-143">Wartość domyślna to `true` dla:</span><span class="sxs-lookup"><span data-stu-id="6e936-143">The default value is `true` for:</span></span>

* <span data-ttu-id="6e936-144">Platforma ASP.NET Core 2.1 lub starszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e936-144">ASP.NET Core 2.1 or earlier apps.</span></span>
* <span data-ttu-id="6e936-145">Aplikacje platformy ASP.NET Core 2,2 lub nowszym w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="6e936-145">ASP.NET Core 2.2 or later apps in the Development environment.</span></span>

<span data-ttu-id="6e936-146"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> jest skojarzony z przełącznikiem zgodności i zapewniają różne zachowanie w zależności od skonfigurowanych zgodność wersji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e936-146"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is associated with a compatibility switch and can provide different behavior depending on the configured compatibility version for the app.</span></span> <span data-ttu-id="6e936-147">Konfigurowanie aplikacji przez ustawienie <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> ma pierwszeństwo przed wartość też dorozumianych przez wersja zgodności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e936-147">Configuring the app by setting <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> takes precedence over the value implied by the app's compatibility version.</span></span>

<span data-ttu-id="6e936-148">Jeśli wersja zgodności aplikacji jest równa <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> lub wcześniej, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> ustawiono `true` , chyba że jawnie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="6e936-148">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is set to `true` unless explicitly configured.</span></span>

<span data-ttu-id="6e936-149">Jeśli wersja zgodności aplikacji jest równa <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> lub nowszym, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> ustawiono `false` chyba, że środowisko jest rozwoju lub wartość jest jawnie skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="6e936-149">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> or later, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is set to `false` unless the environment is Development or the value is explicitly configured.</span></span>

<span data-ttu-id="6e936-150">Wskazówki i przykłady ustawienie wersji zgodności aplikacji, zobacz <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="6e936-150">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6e936-151">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6e936-151">Additional resources</span></span>

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
