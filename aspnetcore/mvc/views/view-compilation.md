---
title: Kompilacja pliku razor i wstępnej kompilacji w ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej o zaletach prekompilowanie Razor plików i sposobu wykonywania wstępnej kompilacji pliku Razor w aplikacji platformy ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 03b11116a15c291452acd878e32cd015dc553dcc
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/20/2018
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="2bb0d-103">Kompilacja pliku razor w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2bb0d-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="2bb0d-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2bb0d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="2bb0d-105">Po wywołaniu skojarzonego widoku MVC pliku Razor jest kompilowana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="2bb0d-106">Publikowanie pliku Razor czas kompilacji nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="2bb0d-107">Opcjonalnie można kompilować pliki razor w czasie publikacji i wdrożone z aplikacją&mdash;narzędzie wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="2bb0d-108">Plik Razor jest kompilowana w czasie wykonywania, po wywołaniu skojarzonego widoku Razor strony lub MVC.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="2bb0d-109">Publikowanie pliku Razor czas kompilacji nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="2bb0d-110">Opcjonalnie można kompilować pliki razor w czasie publikacji i wdrożone z aplikacją&mdash;narzędzie wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="2bb0d-111">Plik Razor jest kompilowana w czasie wykonywania, po wywołaniu skojarzonego widoku Razor strony lub MVC.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="2bb0d-112">Razor pliki kompilowane w obu kompilacji i opublikować za pomocą czasu [Razor SDK](xref:mvc/razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="2bb0d-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:mvc/razor-pages/sdk).</span></span>
::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="2bb0d-113">Zagadnienia dotyczące wstępnej kompilacji</span><span class="sxs-lookup"><span data-stu-id="2bb0d-113">Precompilation considerations</span></span>

<span data-ttu-id="2bb0d-114">Efekty uboczne prekompilowanie Razor plików są następujące:</span><span class="sxs-lookup"><span data-stu-id="2bb0d-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="2bb0d-115">Mniejsze opublikowanego pakietu</span><span class="sxs-lookup"><span data-stu-id="2bb0d-115">A smaller published bundle</span></span>
* <span data-ttu-id="2bb0d-116">Skrócić czas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="2bb0d-116">A faster startup time</span></span>
* <span data-ttu-id="2bb0d-117">Nie można edytować pliki Razor&mdash;nie ma skojarzonej zawartości z opublikowanego pakietu.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="2bb0d-118">Wdrażanie wstępnie skompilowanych plików</span><span class="sxs-lookup"><span data-stu-id="2bb0d-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="2bb0d-119">Kompilacja kompilacji i opublikować godzinę Razor plików jest włączona domyślnie przez zestaw SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="2bb0d-120">Edytowanie plików Razor po te są aktualizowane jest obsługiwana w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="2bb0d-121">Domyślnie tylko skompilowanych *Views.dll* i nie *.cshtml* pliki zostały wdrożone za pomocą aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2bb0d-122">Zestaw SDK Razor jest efektywne tylko wtedy, gdy żadne właściwości specyficzne dla wstępnej kompilacji są ustawione w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-122">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="2bb0d-123">Na przykład ustawienie *.csproj* pliku `MvcRazorCompileOnPublish` właściwości `true` wyłącza Razor SDK.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-123">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="2bb0d-124">Jeśli projekt jest przeznaczony dla środowiska .NET Framework, zainstaluj [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) pakietu NuGet:</span><span class="sxs-lookup"><span data-stu-id="2bb0d-124">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

<span data-ttu-id="2bb0d-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span><span class="sxs-lookup"><span data-stu-id="2bb0d-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span></span>

<span data-ttu-id="2bb0d-126">Jeśli projekt jest przeznaczony dla platformy .NET Core, żadne zmiany nie są niezbędne.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="2bb0d-127">Szablony projektów platformy ASP.NET Core 2.x ustawiane niejawnie `MvcRazorCompileOnPublish` właściwości `true` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="2bb0d-128">W rezultacie tego elementu można bezpiecznie usunąć z *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2bb0d-129">Wstępnej kompilacji pliku razor jest niedostępna podczas wykonywania [niezależne wdrożenia (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-129">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="2bb0d-130">Ustaw `MvcRazorCompileOnPublish` właściwości `true`i zainstaluj [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-130">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="2bb0d-131">Następujące *.csproj* przykładzie wyróżniono te ustawienia:</span><span class="sxs-lookup"><span data-stu-id="2bb0d-131">The following *.csproj* sample highlights these settings:</span></span>

<span data-ttu-id="2bb0d-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span><span class="sxs-lookup"><span data-stu-id="2bb0d-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span></span>
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="2bb0d-133">Przygotowanie aplikacji dla [wdrożenia zależne od framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) z [.NET Core interfejsu wiersza polecenia Opublikuj polecenia](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="2bb0d-133">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="2bb0d-134">Na przykład uruchom następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="2bb0d-134">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="2bb0d-135">A *< project_name >. PrecompiledViews.dll* zawierający skompilowane pliki Razor, jest generowany po wstępnej kompilacji zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="2bb0d-135">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="2bb0d-136">Na przykład poniższy zrzut ekranu przedstawia zawartość *Index.cshtml* w *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="2bb0d-136">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Widokami razor wewnątrz biblioteki DLL](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="2bb0d-138">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2bb0d-138">Additional resources</span></span>

::: moniker range="= aspnetcore-1.1"
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
* <xref:mvc/razor-pages/index>
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
* <xref:mvc/razor-pages/index>
* <xref:mvc/views/overview>
* <xref:mvc/razor-pages/sdk>
::: moniker-end