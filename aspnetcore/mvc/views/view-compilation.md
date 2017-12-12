---
title: "Kompilacja widoku razor i wstępnej kompilacji"
author: rick-anderson
description: "Dokument odwołanie wyjaśniający, jak włączyć kompilacji widoku MVC Razor i wstępnej kompilacji w aplikacji platformy ASP.NET Core."
keywords: "Platformy ASP.NET Core kompilacji widoku Razor, Razor pre kompilacji, Razor wstępnej kompilacji"
ms.author: riande
manager: wpickett
ms.date: 12/05/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 873f6203f9e7b5bb14968dcec3f8d8e5548bd834
ms.sourcegitcommit: 282f69e8dd63c39bde97a6d72783af2970d92040
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="7ead4-104">Kompilacja widoku razor i wstępnej kompilacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ead4-104">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="7ead4-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7ead4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7ead4-106">Widokami razor są kompilowane w czasie wykonywania, gdy widok jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="7ead4-106">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="7ead4-107">ASP.NET podstawowe 1.1.0 i wyższe można opcjonalnie widokami Razor skompilować i wdrożyć je z aplikacją&mdash;proces znany jako wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="7ead4-107">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="7ead4-108">Szablony projektów platformy ASP.NET Core 2.x domyślnie włączone wstępnej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="7ead4-108">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!NOTE]
> <span data-ttu-id="7ead4-109">Wstępnej kompilacji widoku razor jest obecnie niedostępna podczas wykonywania [niezależne wdrożenia (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="7ead4-109">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="7ead4-110">Funkcja będzie dostępna dla SCDs, gdy zwalnia 2.1.</span><span class="sxs-lookup"><span data-stu-id="7ead4-110">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="7ead4-111">Aby uzyskać więcej informacji, zobacz [widoku kompilacja zakończy się niepowodzeniem, podczas kompilowania między dla systemu Linux w systemie Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="7ead4-111">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="7ead4-112">Kwestie do rozważenia wstępnej kompilacji:</span><span class="sxs-lookup"><span data-stu-id="7ead4-112">Precompilation considerations:</span></span>

* <span data-ttu-id="7ead4-113">Prekompilowanie widoków powoduje mniejsze opublikowanego pakietu i skrócić czas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="7ead4-113">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="7ead4-114">Nie można edytować pliki Razor prekompilowanie widoków.</span><span class="sxs-lookup"><span data-stu-id="7ead4-114">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="7ead4-115">Edytowany widoków nie będzie znajdować się w opublikowanego pakietu.</span><span class="sxs-lookup"><span data-stu-id="7ead4-115">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="7ead4-116">Aby wdrożyć prekompilowany widoków:</span><span class="sxs-lookup"><span data-stu-id="7ead4-116">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7ead4-117">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7ead4-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7ead4-118">Jeśli projekt jest przeznaczony dla środowiska .NET Framework, zawierać odwołanie do pakietu [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="7ead4-118">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="7ead4-119">Jeśli projekt jest przeznaczony dla platformy .NET Core, żadne zmiany nie są niezbędne.</span><span class="sxs-lookup"><span data-stu-id="7ead4-119">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="7ead4-120">Szablony projektów platformy ASP.NET Core 2.x ustawiane niejawnie `MvcRazorCompileOnPublish` do `true` domyślnie oznacza tego węzła można bezpiecznie usunąć z *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="7ead4-120">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="7ead4-121">Jeśli wolisz można jawne, nie powoduje żadnych problemów w ustawieniu `MvcRazorCompileOnPublish` właściwości `true`.</span><span class="sxs-lookup"><span data-stu-id="7ead4-121">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="7ead4-122">Następujące *.csproj* przykładzie wyróżniono tego ustawienia:</span><span class="sxs-lookup"><span data-stu-id="7ead4-122">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7ead4-123">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7ead4-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7ead4-124">Ustaw `MvcRazorCompileOnPublish` do `true`i zawiera odwołanie do pakietu `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="7ead4-124">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="7ead4-125">Następujące *.csproj* przykładzie wyróżniono te ustawienia:</span><span class="sxs-lookup"><span data-stu-id="7ead4-125">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="7ead4-126">A *< project_name >. PrecompiledViews.dll* zawierający skompilowanych widokami Razor, jest generowany po wstępnej kompilacji zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="7ead4-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="7ead4-127">Na przykład poniższy zrzut ekranu przedstawia zawartość *Index.cshtml* wewnątrz *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="7ead4-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Widokami razor wewnątrz biblioteki DLL](view-compilation/_static/razor-views-in-dll.png)
