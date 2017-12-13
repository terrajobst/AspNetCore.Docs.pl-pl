---
title: "Pomocnik Tag środowiska w platformy ASP.NET Core"
author: pkellner
description: "Pomocnika Tag środowiska ASP.NET Core zdefiniowane w tym wszystkie właściwości"
keywords: "Platformy ASP.NET Core pomocnika tagów"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 2639e4d7494e752462a1a2cb0648042a2d2d06ec
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="7bb36-104">Pomocnik Tag środowiska w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7bb36-104">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="7bb36-105">Przez [Kellner Peterowi](http://peterkellner.net) i [Ateya Hisham Bin](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="7bb36-105">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="7bb36-106">Pomocnik Tag środowiska warunkowo renderuje zawartość objętego na podstawie bieżącego środowiska hostingu.</span><span class="sxs-lookup"><span data-stu-id="7bb36-106">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="7bb36-107">Jego pojedynczy atrybut `names` jest rozdzielana przecinkami lista środowiska nazw, które, gdy dowolne pasuje do bieżącego środowiska wyzwoli objętego zawartości do renderowania.</span><span class="sxs-lookup"><span data-stu-id="7bb36-107">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="7bb36-108">Atrybuty pomocnika Tag środowiska</span><span class="sxs-lookup"><span data-stu-id="7bb36-108">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="7bb36-109">nazwy</span><span class="sxs-lookup"><span data-stu-id="7bb36-109">names</span></span>

<span data-ttu-id="7bb36-110">Akceptuje pojedynczą nazwą Środowisko hostingu lub rozdzielaną przecinkami listę hosting nazwy środowiska, wyzwalających renderowania zawartości zamknięte.</span><span class="sxs-lookup"><span data-stu-id="7bb36-110">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="7bb36-111">Te wartości są porównywane z bieżącą wartość zwracana z właściwości statycznej platformy ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="7bb36-111">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="7bb36-112">Ta wartość jest jedną z następujących: **przemieszczania**; **Programowanie** lub **produkcji**.</span><span class="sxs-lookup"><span data-stu-id="7bb36-112">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="7bb36-113">Porównanie ignoruje wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="7bb36-113">The comparison ignores case.</span></span>

<span data-ttu-id="7bb36-114">Przykład prawidłowego `environment` pomocnika tagów jest:</span><span class="sxs-lookup"><span data-stu-id="7bb36-114">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="7bb36-115">Dołącz i Wyklucz atrybutów</span><span class="sxs-lookup"><span data-stu-id="7bb36-115">include and exclude attributes</span></span>

<span data-ttu-id="7bb36-116">Platformy ASP.NET Core dodaje 2.x `include`  &  `exclude` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="7bb36-116">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="7bb36-117">Te atrybuty kontrolują renderowania objętego zawartości na podstawie dołączone lub wykluczone hostingu środowiska nazw.</span><span class="sxs-lookup"><span data-stu-id="7bb36-117">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="7bb36-118">obejmują platformy ASP.NET Core 2.0 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="7bb36-118">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="7bb36-119">`include` Właściwość ma podobne zachowania `names` atrybutu w ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="7bb36-119">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="7bb36-120">Wyklucz platformy ASP.NET Core 2.0 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="7bb36-120">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="7bb36-121">Z kolei `exclude` umożliwia właściwości `EnvironmentTagHelper` renderowanie objętego zawartości dla wszystkich nazw Środowisko hostingu, z wyjątkiem wskaż, określony.</span><span class="sxs-lookup"><span data-stu-id="7bb36-121">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="7bb36-122">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7bb36-122">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
