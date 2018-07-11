---
title: Kompilowanie pliku razor i wstępnej kompilacji w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat zalet wstępnej kompilacji w plikach Razor i sposób wykonania wstępnej kompilacji pliku Razor, w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: 9355d467ca819ea8c6292963b31367ad5ca36d55
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938540"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="6bfce-103">Kompilacja pliku razor w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6bfce-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="6bfce-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6bfce-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="6bfce-105">Pliku Razor jest kompilowana w czasie wykonywania, gdy jest wywoływany skojarzonego widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="6bfce-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="6bfce-106">Publikowanie pliku Razor w czasie kompilacji nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="6bfce-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="6bfce-107">Opcjonalnie można kompilować plikach razor na czas publikacji i wdrożonych przy użyciu aplikacji&mdash;przy użyciu narzędzia wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6bfce-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="6bfce-108">Pliku Razor jest kompilowana w czasie wykonywania, po wywołaniu skojarzonego widoku Razor strony lub MVC.</span><span class="sxs-lookup"><span data-stu-id="6bfce-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="6bfce-109">Publikowanie pliku Razor w czasie kompilacji nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="6bfce-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="6bfce-110">Opcjonalnie można kompilować plikach razor na czas publikacji i wdrożonych przy użyciu aplikacji&mdash;przy użyciu narzędzia wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6bfce-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="6bfce-111">Pliku Razor jest kompilowana w czasie wykonywania, po wywołaniu skojarzonego widoku Razor strony lub MVC.</span><span class="sxs-lookup"><span data-stu-id="6bfce-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="6bfce-112">Pliki razor są kompilowane w obu kompilacji i opublikować przy użyciu [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="6bfce-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>
::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="6bfce-113">Zagadnienia dotyczące wstępnej kompilacji</span><span class="sxs-lookup"><span data-stu-id="6bfce-113">Precompilation considerations</span></span>

<span data-ttu-id="6bfce-114">Prekompilowanie plikach Razor efekty uboczne są następujące:</span><span class="sxs-lookup"><span data-stu-id="6bfce-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="6bfce-115">Mniejsze opublikowany pakiet</span><span class="sxs-lookup"><span data-stu-id="6bfce-115">A smaller published bundle</span></span>
* <span data-ttu-id="6bfce-116">Krótszy czas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="6bfce-116">A faster startup time</span></span>
* <span data-ttu-id="6bfce-117">Nie można edytować pliki Razor&mdash;Brak powiązanej zawartości z opublikowanych pakietu.</span><span class="sxs-lookup"><span data-stu-id="6bfce-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="6bfce-118">Wdrażanie wstępnie skompilowanych plików</span><span class="sxs-lookup"><span data-stu-id="6bfce-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="6bfce-119">Kompilacja i publikowania w czasie kompilacji plików Razor jest domyślnie włączona, przez zestaw SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="6bfce-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="6bfce-120">Po te są aktualizowane w plikach Razor do edycji jest obsługiwana w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6bfce-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="6bfce-121">Domyślnie tylko skompilowanych *Views.dll* i nie *.cshtml* pliki zostały wdrożone za pomocą aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6bfce-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6bfce-122">Zestaw Razor SDK jest efektywne tylko wtedy, gdy brak właściwości specyficzne dla wstępnej kompilacji są ustawione w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="6bfce-122">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="6bfce-123">Na przykład ustawienie *.csproj* pliku `MvcRazorCompileOnPublish` właściwość `true` wyłącza zestaw Razor SDK.</span><span class="sxs-lookup"><span data-stu-id="6bfce-123">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="6bfce-124">Jeśli projekt jest przeznaczony dla .NET Framework, należy zainstalować [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) pakietu NuGet:</span><span class="sxs-lookup"><span data-stu-id="6bfce-124">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="6bfce-125">Jeśli projekt jest przeznaczony dla platformy .NET Core, żadne zmiany nie są niezbędne.</span><span class="sxs-lookup"><span data-stu-id="6bfce-125">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="6bfce-126">Szablony projektów programu ASP.NET Core 2.x niejawnie ustaw `MvcRazorCompileOnPublish` właściwość `true` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="6bfce-126">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="6bfce-127">W związku z tym, ten element można bezpiecznie usunąć z *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="6bfce-127">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6bfce-128">Wstępnej kompilacji pliku razor jest niedostępna podczas wykonywania [niezależna wdrożenia (— SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) programu ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="6bfce-128">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="6bfce-129">Ustaw `MvcRazorCompileOnPublish` właściwości `true`i zainstaluj [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="6bfce-129">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="6bfce-130">Następujące *.csproj* przykładzie wyróżniono tych ustawień:</span><span class="sxs-lookup"><span data-stu-id="6bfce-130">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="6bfce-131">Przygotowywanie aplikacji dla [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd) z [polecenia publikowania interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="6bfce-131">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="6bfce-132">Na przykład wykonaj następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="6bfce-132">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="6bfce-133">A *< project_name >. PrecompiledViews.dll* zawierający skompilowane pliki Razor jest generowany po pomyślnym zakończeniu wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6bfce-133">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="6bfce-134">Na przykład, poniższy zrzut ekranu przedstawia zawartość *Index.cshtml* w ramach *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="6bfce-134">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Widokami razor wewnątrz biblioteki DLL](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6bfce-136">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6bfce-136">Additional resources</span></span>

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
