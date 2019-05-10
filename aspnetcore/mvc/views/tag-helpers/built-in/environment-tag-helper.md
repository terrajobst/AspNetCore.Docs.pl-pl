---
title: Pomocnik tagu środowiska w programie ASP.NET Core
author: pkellner
description: Pomocnik tagu środowiska ASP.NET Core zdefiniowane w tym wszystkie właściwości
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 379f58ed37329f047d53adf1dcfdfd2ad6a6ca4e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901003"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="f4cb0-103">Pomocnik tagu środowiska w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4cb0-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="f4cb0-104">Przez [Peter Kellner](http://peterkellner.net), [Ateya Hisham Bin](https://twitter.com/hishambinateya), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f4cb0-104">By [Peter Kellner](http://peterkellner.net), [Hisham Bin Ateya](https://twitter.com/hishambinateya), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f4cb0-105">Pomocnik tagu środowiska warunkowo renderuje zawartość ujęty na podstawie bieżącego [Środowisko hostingu](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="f4cb0-105">The Environment Tag Helper conditionally renders its enclosed content based on the current [hosting environment](xref:fundamentals/environments).</span></span> <span data-ttu-id="f4cb0-106">Pomocnik tagu środowiska jeden atrybut `names`, znajduje się lista rozdzielonych przecinkami nazw środowiska.</span><span class="sxs-lookup"><span data-stu-id="f4cb0-106">The Environment Tag Helper's single attribute, `names`, is a comma-separated list of environment names.</span></span> <span data-ttu-id="f4cb0-107">Jeśli żadnej z nazw dostarczonego środowiska zgodne bieżącego środowiska, ujęty zawartość jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="f4cb0-107">If any of the provided environment names match the current environment, the enclosed content is rendered.</span></span>

<span data-ttu-id="f4cb0-108">Aby zapoznać się z omówieniem pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="f4cb0-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="f4cb0-109">Atrybuty Pomocnik tagu środowiska</span><span class="sxs-lookup"><span data-stu-id="f4cb0-109">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="f4cb0-110">nazwy</span><span class="sxs-lookup"><span data-stu-id="f4cb0-110">names</span></span>

<span data-ttu-id="f4cb0-111">`names` Akceptuje pojedynczą nazwą środowiska hostingu lub rozdzielaną przecinkami listę hostingu nazwy środowiska, które mogą powodować renderowania ujęty zawartości.</span><span class="sxs-lookup"><span data-stu-id="f4cb0-111">`names` accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="f4cb0-112">Wartości środowiskowe są porównywane z bieżącej wartości zwracanej przez [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="f4cb0-112">Environment values are compared to the current value returned by [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="f4cb0-113">Porównanie, ignoruje wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="f4cb0-113">The comparison ignores case.</span></span>

<span data-ttu-id="f4cb0-114">W poniższym przykładzie użyto Pomocnik tagu środowiska.</span><span class="sxs-lookup"><span data-stu-id="f4cb0-114">The following example uses an Environment Tag Helper.</span></span> <span data-ttu-id="f4cb0-115">Zawartość jest wyświetlana, jeśli środowisko hostingu jest przejściowych lub produkcyjnych:</span><span class="sxs-lookup"><span data-stu-id="f4cb0-115">The content is rendered if the hosting environment is Staging or Production:</span></span>

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="f4cb0-116">Dołączanie i wykluczanie atrybutów</span><span class="sxs-lookup"><span data-stu-id="f4cb0-116">include and exclude attributes</span></span>

<span data-ttu-id="f4cb0-117">`include` & `exclude` atrybuty kontrolują renderowania zawartości ujęty na podstawie dołączone lub wykluczone hostingu środowiska nazw.</span><span class="sxs-lookup"><span data-stu-id="f4cb0-117">`include` & `exclude` attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include"></a><span data-ttu-id="f4cb0-118">include</span><span class="sxs-lookup"><span data-stu-id="f4cb0-118">include</span></span>

<span data-ttu-id="f4cb0-119">`include` Właściwość wykazuje zachowanie podobne do `names` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f4cb0-119">The `include` property exhibits similar behavior to the `names` attribute.</span></span> <span data-ttu-id="f4cb0-120">Środowisko na liście `include` wartość atrybutu muszą być zgodne Środowisko hostingu aplikacji ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) do renderowania zawartości `<environment>` tagu.</span><span class="sxs-lookup"><span data-stu-id="f4cb0-120">An environment listed in the `include` attribute value must match the app's hosting environment ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) to render the content of the `<environment>` tag.</span></span>

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a><span data-ttu-id="f4cb0-121">wykluczanie</span><span class="sxs-lookup"><span data-stu-id="f4cb0-121">exclude</span></span>

<span data-ttu-id="f4cb0-122">W przeciwieństwie do `include` atrybutu zawartość `<environment>` renderowania tagu, gdy środowisko hostingu nie są zgodne środowisko na liście `exclude` wartość atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f4cb0-122">In contrast to the `include` attribute, the content of the `<environment>` tag is rendered when the hosting environment doesn't match an environment listed in the `exclude` attribute value.</span></span>

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="f4cb0-123">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f4cb0-123">Additional resources</span></span>

* <xref:fundamentals/environments>
