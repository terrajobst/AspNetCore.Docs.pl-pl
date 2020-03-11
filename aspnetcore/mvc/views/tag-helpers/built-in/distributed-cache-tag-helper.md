---
title: Pomocnik tagów rozproszonej pamięci podręcznej w ASP.NET Core
author: pkellner
description: Dowiedz się, jak korzystać z pomocnika tagów rozproszonej pamięci podręcznej.
ms.author: riande
ms.custom: mvc
ms.date: 01/24/2020
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: f5957adf3cef8966812a1bf0cbc6b2627d19d026
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664018"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="bcffc-103">Pomocnik tagów rozproszonej pamięci podręcznej w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bcffc-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="bcffc-104">Według [Peterowi Kellner](https://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="bcffc-104">By [Peter Kellner](https://peterkellner.net)</span></span>

<span data-ttu-id="bcffc-105">Pomocnik tagów rozproszonej pamięci podręcznej umożliwia znaczne zwiększenie wydajności aplikacji ASP.NET Core przez buforowanie jej zawartości w rozproszonym źródle pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="bcffc-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="bcffc-106">Aby zapoznać się z omówieniem pomocników tagów, zobacz <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="bcffc-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="bcffc-107">Pomocnik tagów rozproszonej pamięci podręcznej dziedziczy z tej samej klasy bazowej co pomocnik tagu pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="bcffc-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="bcffc-108">Wszystkie atrybuty [pomocnika tagów pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) są dostępne dla pomocnika tagów rozproszonych.</span><span class="sxs-lookup"><span data-stu-id="bcffc-108">All of the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) attributes are available to the Distributed Tag Helper.</span></span>

<span data-ttu-id="bcffc-109">Pomocnik tagów rozproszonej pamięci podręcznej używa [iniekcji konstruktora](xref:fundamentals/dependency-injection#constructor-injection-behavior).</span><span class="sxs-lookup"><span data-stu-id="bcffc-109">The Distributed Cache Tag Helper uses [constructor injection](xref:fundamentals/dependency-injection#constructor-injection-behavior).</span></span> <span data-ttu-id="bcffc-110">Interfejs <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> jest przekazywany do konstruktora pomocnika tagów rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="bcffc-110">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="bcffc-111">Jeśli żadna konkretna implementacja `IDistributedCache` nie zostanie utworzona w `Startup.ConfigureServices` (*Startup.cs*), w ramach rozproszonej pamięci podręcznej, pomocnik będzie używać tego samego dostawcy w pamięci do przechowywania danych buforowanych jako [pomocnika tagów pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="bcffc-111">If no concrete implementation of `IDistributedCache` is created in `Startup.ConfigureServices` (*Startup.cs*), the Distributed Cache Tag Helper uses the same in-memory provider for storing cached data as the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="bcffc-112">Atrybuty pomocnika tagów rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="bcffc-112">Distributed Cache Tag Helper Attributes</span></span>

### <a name="attributes-shared-with-the-cache-tag-helper"></a><span data-ttu-id="bcffc-113">Atrybuty udostępnione za pomocą pomocnika tagów pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="bcffc-113">Attributes shared with the Cache Tag Helper</span></span>

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

<span data-ttu-id="bcffc-114">Pomocnik tagów rozproszonej pamięci podręcznej dziedziczy z tej samej klasy jako pomocnika tagów pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="bcffc-114">The Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper.</span></span> <span data-ttu-id="bcffc-115">Opisy tych atrybutów znajdują się w temacie [pomocnik tagów pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="bcffc-115">For descriptions of these attributes, see the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="name"></a><span data-ttu-id="bcffc-116">name</span><span class="sxs-lookup"><span data-stu-id="bcffc-116">name</span></span>

| <span data-ttu-id="bcffc-117">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="bcffc-117">Attribute Type</span></span> | <span data-ttu-id="bcffc-118">Przykład</span><span class="sxs-lookup"><span data-stu-id="bcffc-118">Example</span></span>                               |
| -------------- | ------------------------------------- |
| <span data-ttu-id="bcffc-119">Ciąg</span><span class="sxs-lookup"><span data-stu-id="bcffc-119">String</span></span>         | `my-distributed-cache-unique-key-101` |

<span data-ttu-id="bcffc-120">`name` jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="bcffc-120">`name` is required.</span></span> <span data-ttu-id="bcffc-121">Atrybut `name` jest używany jako klucz dla każdego przechowywanego wystąpienia pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="bcffc-121">The `name` attribute is used as a key for each stored cache instance.</span></span> <span data-ttu-id="bcffc-122">W przeciwieństwie do pomocnika tagu pamięci podręcznej, który przypisuje klucz pamięci podręcznej do każdego wystąpienia na podstawie nazwy i lokalizacji strony Razor na stronie Razor, pomocnik tagów rozproszonej pamięci podręcznej w atrybucie `name`.</span><span class="sxs-lookup"><span data-stu-id="bcffc-122">Unlike the Cache Tag Helper that assigns a cache key to each instance based on the Razor page name and location in the Razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`.</span></span>

<span data-ttu-id="bcffc-123">Przykład:</span><span class="sxs-lookup"><span data-stu-id="bcffc-123">Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="bcffc-124">IDistributedCache implementacje pomocnika tagów rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="bcffc-124">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="bcffc-125">Istnieją dwie implementacje <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> wbudowane w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bcffc-125">There are two implementations of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> built in to ASP.NET Core.</span></span> <span data-ttu-id="bcffc-126">Jest on oparty na SQL Server, a drugi jest oparty na Redis.</span><span class="sxs-lookup"><span data-stu-id="bcffc-126">One is based on SQL Server, and the other is based on Redis.</span></span> <span data-ttu-id="bcffc-127">Dostępne są również implementacje innych firm, takie jak [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html).</span><span class="sxs-lookup"><span data-stu-id="bcffc-127">Third-party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html).</span></span> <span data-ttu-id="bcffc-128">Szczegóły tych implementacji można znaleźć pod adresem <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="bcffc-128">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="bcffc-129">Obie implementacje wymagają ustawienia wystąpienia `IDistributedCache` w `Startup`.</span><span class="sxs-lookup"><span data-stu-id="bcffc-129">Both implementations involve setting an instance of `IDistributedCache` in `Startup`.</span></span>

<span data-ttu-id="bcffc-130">Nie istnieją żadne atrybuty tagów, które są skojarzone z użyciem żadnej konkretnej implementacji `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="bcffc-130">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bcffc-131">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bcffc-131">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
