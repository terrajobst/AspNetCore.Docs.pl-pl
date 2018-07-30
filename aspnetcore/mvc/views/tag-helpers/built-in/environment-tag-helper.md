---
title: Pomocnik tagu środowiska w programie ASP.NET Core
author: pkellner
description: Pomocnik tagu środowiska ASP.NET Core zdefiniowane w tym wszystkie właściwości
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 4a283a3a03aa6cac228ec6effd02e3f1095be260
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342227"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="a1084-103">Pomocnik tagu środowiska w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a1084-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="a1084-104">Przez [Peter Kellner](http://peterkellner.net) i [Ateya Hisham pojemnika](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="a1084-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="a1084-105">Pomocnik tagu środowiska warunkowo renderuje zawartość ujęty na podstawie bieżącego środowiska hostingu.</span><span class="sxs-lookup"><span data-stu-id="a1084-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="a1084-106">Jego jeden atrybut `names` jest rozdzielana przecinkami lista środowiska nazwy, że jeśli dowolny pasuje do bieżącego środowiska, wywoła ujęty zawartości do renderowania.</span><span class="sxs-lookup"><span data-stu-id="a1084-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="a1084-107">Atrybuty Pomocnik tagu środowiska</span><span class="sxs-lookup"><span data-stu-id="a1084-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="a1084-108">nazwy</span><span class="sxs-lookup"><span data-stu-id="a1084-108">names</span></span>

<span data-ttu-id="a1084-109">Akceptuje pojedynczą nazwą środowiska hostingu lub rozdzielaną przecinkami listę hostingu nazwy środowiska, które mogą powodować renderowania ujęty zawartości.</span><span class="sxs-lookup"><span data-stu-id="a1084-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="a1084-110">Te wartości są porównywane z bieżącą wartość zwracana z właściwości statycznej platformy ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="a1084-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="a1084-111">Ta wartość jest jedną z następujących: **przemieszczania**; **Rozwoju** lub **produkcji**.</span><span class="sxs-lookup"><span data-stu-id="a1084-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="a1084-112">Porównanie, ignoruje wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="a1084-112">The comparison ignores case.</span></span>

<span data-ttu-id="a1084-113">Przykładem prawidłowego `environment` Pomocnik tagu:</span><span class="sxs-lookup"><span data-stu-id="a1084-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="a1084-114">Dołączanie i wykluczanie atrybutów</span><span class="sxs-lookup"><span data-stu-id="a1084-114">include and exclude attributes</span></span>

<span data-ttu-id="a1084-115">Platforma ASP.NET Core 2.x dodaje `include`  &  `exclude` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a1084-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="a1084-116">Te atrybuty kontrolują renderowania zawartości ujęty na podstawie dołączone lub wykluczone hostingu środowiska nazw.</span><span class="sxs-lookup"><span data-stu-id="a1084-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="a1084-117">obejmują platformy ASP.NET Core 2.0 i nowsze wersje</span><span class="sxs-lookup"><span data-stu-id="a1084-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="a1084-118">`include` Właściwość ma podobne zachowanie `names` atrybutu w programie ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="a1084-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="a1084-119">Wyklucz platformy ASP.NET Core 2.0 i nowsze wersje</span><span class="sxs-lookup"><span data-stu-id="a1084-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="a1084-120">Z kolei `exclude` właściwości `EnvironmentTagHelper` renderowania ujęty zawartości dla wszystkich nazw środowiska hostingu, z wyjątkiem wskaż, który określiłeś.</span><span class="sxs-lookup"><span data-stu-id="a1084-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="a1084-121">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a1084-121">Additional resources</span></span>

* <xref:fundamentals/environments>
