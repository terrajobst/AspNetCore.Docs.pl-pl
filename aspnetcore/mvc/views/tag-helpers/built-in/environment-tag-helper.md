---
title: Pomocnik Tag środowiska w platformy ASP.NET Core
author: pkellner
description: Pomocnika Tag środowiska ASP.NET Core zdefiniowane w tym wszystkie właściwości
manager: wpickett
ms.author: riande
ms.date: 07/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 7a99ee0e59c7f49a3208d2c86c11cabce4294889
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
ms.locfileid: "28887810"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="a808a-103">Pomocnik Tag środowiska w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a808a-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="a808a-104">Przez [Kellner Peterowi](http://peterkellner.net) i [Ateya Hisham Bin](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="a808a-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="a808a-105">Pomocnik Tag środowiska warunkowo renderuje zawartość objętego na podstawie bieżącego środowiska hostingu.</span><span class="sxs-lookup"><span data-stu-id="a808a-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="a808a-106">Jego pojedynczy atrybut `names` jest rozdzielana przecinkami lista środowiska nazw, które, gdy dowolne pasuje do bieżącego środowiska wyzwoli objętego zawartości do renderowania.</span><span class="sxs-lookup"><span data-stu-id="a808a-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="a808a-107">Atrybuty pomocnika Tag środowiska</span><span class="sxs-lookup"><span data-stu-id="a808a-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="a808a-108">nazwy</span><span class="sxs-lookup"><span data-stu-id="a808a-108">names</span></span>

<span data-ttu-id="a808a-109">Akceptuje pojedynczą nazwą Środowisko hostingu lub rozdzielaną przecinkami listę hosting nazwy środowiska, wyzwalających renderowania zawartości zamknięte.</span><span class="sxs-lookup"><span data-stu-id="a808a-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="a808a-110">Te wartości są porównywane z bieżącą wartość zwracana z właściwości statycznej platformy ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="a808a-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="a808a-111">Ta wartość jest jedną z następujących: **przemieszczania**; **Programowanie** lub **produkcji**.</span><span class="sxs-lookup"><span data-stu-id="a808a-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="a808a-112">Porównanie ignoruje wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="a808a-112">The comparison ignores case.</span></span>

<span data-ttu-id="a808a-113">Przykład prawidłowego `environment` pomocnika tagów jest:</span><span class="sxs-lookup"><span data-stu-id="a808a-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="a808a-114">Dołącz i Wyklucz atrybutów</span><span class="sxs-lookup"><span data-stu-id="a808a-114">include and exclude attributes</span></span>

<span data-ttu-id="a808a-115">Platformy ASP.NET Core dodaje 2.x `include`  &  `exclude` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a808a-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="a808a-116">Te atrybuty kontrolują renderowania objętego zawartości na podstawie dołączone lub wykluczone hostingu środowiska nazw.</span><span class="sxs-lookup"><span data-stu-id="a808a-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="a808a-117">obejmują platformy ASP.NET Core 2.0 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a808a-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="a808a-118">`include` Właściwość ma podobne zachowania `names` atrybutu w ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="a808a-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="a808a-119">Wyklucz platformy ASP.NET Core 2.0 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a808a-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="a808a-120">Z kolei `exclude` umożliwia właściwości `EnvironmentTagHelper` renderowanie objętego zawartości dla wszystkich nazw Środowisko hostingu, z wyjątkiem wskaż, określony.</span><span class="sxs-lookup"><span data-stu-id="a808a-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="a808a-121">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a808a-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
