---
title: Rozproszonej pamięci podręcznej pomocnika tagów w platformy ASP.NET Core
author: pkellner
description: Pokazuje, jak pracować z pamięci podręcznej pomocnika tagów
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 9c1d91fc185a0afecf59af8927ddf6f25eff29ab
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962327"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="22384-103">Rozproszonej pamięci podręcznej pomocnika tagów w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22384-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="22384-104">Przez [Kellner Peterowi](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="22384-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="22384-105">Rozproszonej pamięci podręcznej pomocnika Tag pozwala na znacznie zwiększyć wydajność aplikacji platformy ASP.NET Core buforując zawartość ze źródłem rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="22384-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="22384-106">Dziedziczy pomocnika Tag rozproszonej pamięci podręcznej z tej samej klasy podstawowej jako pomocnika tagów pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="22384-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="22384-107">Wszystkie atrybuty skojarzone z pamięci podręcznej pomocnika tagów również będą działać na pomocnika tagów rozproszonych.</span><span class="sxs-lookup"><span data-stu-id="22384-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>

<span data-ttu-id="22384-108">Następuje pomocnika Tag rozproszonej pamięci podręcznej **jawne zależności zasady** znany jako **iniekcji konstruktora**.</span><span class="sxs-lookup"><span data-stu-id="22384-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span> <span data-ttu-id="22384-109">W szczególności `IDistributedCache` kontenera interfejs zostanie przekazany do konstruktora rozproszonej pamięci podręcznej Tag Pomocnik firmy.</span><span class="sxs-lookup"><span data-stu-id="22384-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="22384-110">Jeśli nie określonych konkretną implementację `IDistributedCache` został utworzony w `ConfigureServices`, zwykle znajdują się w pliku startup.cs, a następnie pomocnika Tag rozproszonej pamięci podręcznej będzie używać tego samego dostawcy w pamięci do przechowywania danych z pamięci podręcznej jako podstawowa pomocnika tagów pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="22384-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="22384-111">Rozproszonej pamięci podręcznej Tag pomocnika atrybutów</span><span class="sxs-lookup"><span data-stu-id="22384-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="22384-112">włączone wygasa na wygasa po wygaśnięciu przedłużanie różnią się w nagłówku różnią się przez zapytanie różnią się przez trasy różnią się przez cookie różnią się przez użytkownika różnią się według priorytetu</span><span class="sxs-lookup"><span data-stu-id="22384-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="22384-113">Zobacz pomocnika tagów pamięci podręcznej dla definicji.</span><span class="sxs-lookup"><span data-stu-id="22384-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="22384-114">Rozproszonej pamięci podręcznej Tag pomocnika dziedziczy z tej samej klasy jako pomocnika tagów pamięci podręcznej, więc te atrybuty są często używane z pamięci podręcznej pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="22384-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="22384-115">Nazwa (wymagane)</span><span class="sxs-lookup"><span data-stu-id="22384-115">name (required)</span></span>

| <span data-ttu-id="22384-116">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="22384-116">Attribute Type</span></span>    | <span data-ttu-id="22384-117">Przykładowa wartość</span><span class="sxs-lookup"><span data-stu-id="22384-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="22384-118">string</span><span class="sxs-lookup"><span data-stu-id="22384-118">string</span></span>    | <span data-ttu-id="22384-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="22384-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="22384-120">Wymagane `name` atrybut jest używany jako klucz tej pamięci podręcznej przechowywanych dla każdego wystąpienia pomocnika Tag rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="22384-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span> <span data-ttu-id="22384-121">W przeciwieństwie do podstawowych pomocnika tagów pamięci podręcznej przypisującej klucza do każdego wystąpienia pomocnika tagów pamięci podręcznej na podstawie Razor strony nazwy i lokalizacji pomocnika tagów razor strony pomocnika Tag rozproszonej pamięci podręcznej tylko określa jego klucza na na podstawie atrybutu `name`</span><span class="sxs-lookup"><span data-stu-id="22384-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the Tag Helper in the razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`</span></span>

<span data-ttu-id="22384-122">Przykład użycia:</span><span class="sxs-lookup"><span data-stu-id="22384-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="22384-123">Rozproszone implementacje IDistributedCache pomocnika tagów pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="22384-123">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="22384-124">Istnieją dwa implementacje `IDistributedCache` wbudowanych w platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="22384-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span> <span data-ttu-id="22384-125">Jeden opiera się na serwerze SQL i innych jest oparty na pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="22384-125">One is based on SQL Server and the other is based on Redis.</span></span> <span data-ttu-id="22384-126">Szczegółowe informacje o tych implementacji można znaleźć w folderze <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="22384-126">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="22384-127">Zarówno implementacje obejmują ustawianie wystąpienia `IDistributedCache` w ASP.NET Core *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="22384-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's *Startup.cs*.</span></span>

<span data-ttu-id="22384-128">Istnieją żadne atrybuty tagu w szczególności związanych z użyciem określonych implementacji `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="22384-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="22384-129">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="22384-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
