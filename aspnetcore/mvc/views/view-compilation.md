---
title: Kompilacja widoku razor i wstępnej kompilacji w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak włączyć kompilacji widoku MVC Razor i wstępnej kompilacji w aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 013ca0d149c6415b5e6825aa5a48e93ae48f6728
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a><span data-ttu-id="47189-103">Razor (cshtml) pliku kompilacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47189-103">Razor file (.cshtml) compilation in ASP.NET Core</span></span>

<span data-ttu-id="47189-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="47189-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="47189-105">Widokami razor są kompilowane w czasie wykonywania, gdy widok jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="47189-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="47189-106">Platformy ASP.NET Core 2.1.0 lub nowszym widoki kompilacji w kompilacji i opublikować za pomocą czasu [Razor Sdk](/aspnetcore/mvc/razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="47189-106">ASP.NET Core 2.1.0 or later compile views at build and publish time using [Razor Sdk](/aspnetcore/mvc/razor-pages/sdk).</span></span> <span data-ttu-id="47189-107">W ASP.NET Core 1.1 i ASP.NET Core 2.0, widoki Opcjonalnie można kompilowana w publikacji i wdrażać z aplikacją &mdash; narzędzie wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="47189-107">In ASP.NET Core 1.1, and ASP.NET Core 2.0, views can optionally be compiled at publish and deployed with the app &mdash; using the precompilation tool.</span></span> 



<span data-ttu-id="47189-108">Kwestie do rozważenia wstępnej kompilacji:</span><span class="sxs-lookup"><span data-stu-id="47189-108">Precompilation considerations:</span></span>

* <span data-ttu-id="47189-109">Prekompilowanie widoków powoduje mniejsze opublikowanego pakietu i skrócić czas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="47189-109">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="47189-110">Nie można edytować pliki Razor prekompilowanie widoków.</span><span class="sxs-lookup"><span data-stu-id="47189-110">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="47189-111">Edytowany widoków nie będzie znajdować się w opublikowanego pakietu.</span><span class="sxs-lookup"><span data-stu-id="47189-111">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="47189-112">Aby wdrożyć prekompilowany widoków:</span><span class="sxs-lookup"><span data-stu-id="47189-112">To deploy precompiled views:</span></span>

# <a name="aspnet-core-21tabaspnetcore21"></a>[<span data-ttu-id="47189-113">Platformy ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="47189-113">ASP.NET Core 2.1</span></span>](#tab/aspnetcore21/)
<span data-ttu-id="47189-114">Tworzenie i publikowanie czas kompilacji Razor plików jest włączona domyślnie przez zestaw Sdk Razor.</span><span class="sxs-lookup"><span data-stu-id="47189-114">Build and publish time compilation of Razor files is enabled by default by the Razor Sdk.</span></span> <span data-ttu-id="47189-115">Edytowanie plików Razor, po ich aktualizacji jest obsługiwana w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="47189-115">Editing Razor files after they are updated is supported at build time.</span></span> <span data-ttu-id="47189-116">Domyślnie tylko skompilowanych *Views.dll* i cshtml żadne pliki nie zostały wdrożone za pomocą aplikacji.</span><span class="sxs-lookup"><span data-stu-id="47189-116">By default, only the compiled *Views.dll* and no cshtml files are deployed with your application.</span></span> 
    
> [!IMPORTANT]
> <span data-ttu-id="47189-117">Zestaw Sdk Razor jest efektywne tylko wtedy, gdy nie właściwości specyficzne dla wstępnej kompilacji są ustawione w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="47189-117">The Razor Sdk is only effective when no precompilation specific properties are set in your project file.</span></span> <span data-ttu-id="47189-118">Na przykład ustawienie `MvcRazorCompileOnPublish` w Twojej *.csproj* pliku spowoduje wyłączenie Razor Sdk.</span><span class="sxs-lookup"><span data-stu-id="47189-118">For instance, setting `MvcRazorCompileOnPublish` in your *.csproj* file will disable the Razor Sdk.</span></span>

# <a name="aspnet-core-20tabaspnetcore20"></a>[<span data-ttu-id="47189-119">Platformy ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="47189-119">ASP.NET Core 2.0</span></span>](#tab/aspnetcore20/)

<span data-ttu-id="47189-120">Jeśli projekt jest przeznaczony dla środowiska .NET Framework, zawierać odwołanie do pakietu [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="47189-120">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="47189-121">Jeśli projekt jest przeznaczony dla platformy .NET Core, żadne zmiany nie są niezbędne.</span><span class="sxs-lookup"><span data-stu-id="47189-121">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="47189-122">Szablony projektów platformy ASP.NET Core 2.x niejawnie ustawia `MvcRazorCompileOnPublish` do `true` domyślnie oznacza tego węzła można bezpiecznie usunąć z *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="47189-122">The ASP.NET Core 2.x project templates implicitly sets `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span>
    
> [!IMPORTANT]
> <span data-ttu-id="47189-123">Wstępnej kompilacji widoku razor, gdy nie jest dostępny wykonywania [niezależne wdrożenia (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="47189-123">Razor view precompilation in not available when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> 

<span data-ttu-id="47189-124">Przygotowanie aplikacji dla [wdrożenia zależne od framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) z [.NET Core interfejsu wiersza polecenia Opublikuj polecenia](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="47189-124">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="47189-125">Na przykład uruchom następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="47189-125">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="47189-126">A *< project_name >. PrecompiledViews.dll* zawierający skompilowanych widokami Razor, jest generowany po wstępnej kompilacji zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="47189-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="47189-127">Na przykład poniższy zrzut ekranu przedstawia zawartość *Index.cshtml* wewnątrz *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="47189-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Widokami razor wewnątrz biblioteki DLL](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="47189-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="47189-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="47189-130">Ustaw `MvcRazorCompileOnPublish` do `true` i zawiera odwołanie do pakietu `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="47189-130">Set `MvcRazorCompileOnPublish` to `true` and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="47189-131">Następujące *.csproj* przykładzie wyróżniono te ustawienia:</span><span class="sxs-lookup"><span data-stu-id="47189-131">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

