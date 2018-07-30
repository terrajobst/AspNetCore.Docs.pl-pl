---
title: Pomocnik tagu pamięci podręcznej w programie ASP.NET Core rozproszonych
author: pkellner
description: Pokazuje, jak pracować z Pomocnik tagu pamięci podręcznej
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a35a5795c086273e773c613c483fc6343c694bf2
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342175"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="abe56-103">Pomocnik tagu pamięci podręcznej w programie ASP.NET Core rozproszonych</span><span class="sxs-lookup"><span data-stu-id="abe56-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="abe56-104">Przez [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="abe56-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="abe56-105">Pomocnik tagu usługi rozproszonej pamięci podręcznej umożliwia znacznie zwiększyć wydajność aplikacji platformy ASP.NET Core, buforując zawartość ze źródłem rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="abe56-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="abe56-106">Pomocnik tagu rozproszonej pamięci podręcznej dziedziczy z klasy bazowej tego samego jako Pomocnik tagu pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="abe56-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="abe56-107">Wszystkie atrybuty skojarzone z Pomocnik tagu pamięci podręcznej będą również działać w Pomocnik tagu rozproszonej.</span><span class="sxs-lookup"><span data-stu-id="abe56-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>

<span data-ttu-id="abe56-108">Pomocnik tagu rozproszonej pamięci podręcznej następuje **jawne zależności zasady** znane jako **iniekcji konstruktora**.</span><span class="sxs-lookup"><span data-stu-id="abe56-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span> <span data-ttu-id="abe56-109">W szczególności `IDistributedCache` interfejs kontenera jest przekazywany do konstruktora rozproszonych Pomocnik tagu pamięci podręcznej w.</span><span class="sxs-lookup"><span data-stu-id="abe56-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="abe56-110">Jeśli nie określonej implementacji konkretnego `IDistributedCache` został utworzony w `ConfigureServices`, zwykle znajduje się w pliku startup.cs, a następnie Pomocnik tagu rozproszonej pamięci podręcznej będzie używać tego samego dostawcy w pamięci do przechowywania danych w pamięci podręcznej jako podstawowego Pomocnik tagu pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="abe56-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="abe56-111">Rozproszone atrybutów Pomocnik tagu pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="abe56-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="abe56-112">włączone, wygasa w wygasa po wygaśnięciu przedłużanie różnią się w nagłówku różnią się przez zapytanie różnią się przez trasy różnią się przez cookie różnią się przez użytkownika różnią się według priorytetu</span><span class="sxs-lookup"><span data-stu-id="abe56-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="abe56-113">Pomocnik tagu pamięci podręcznej znajdują się definicje.</span><span class="sxs-lookup"><span data-stu-id="abe56-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="abe56-114">Pomocnik tagu usługi rozproszonej pamięci podręcznej dziedziczy taka sama klasa co Pomocnik tagu pamięci podręcznej, dlatego te atrybuty są wspólne w Pomocnik tagu pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="abe56-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="abe56-115">(wymagane)</span><span class="sxs-lookup"><span data-stu-id="abe56-115">name (required)</span></span>

| <span data-ttu-id="abe56-116">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="abe56-116">Attribute Type</span></span>    | <span data-ttu-id="abe56-117">Przykładowa wartość</span><span class="sxs-lookup"><span data-stu-id="abe56-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="abe56-118">string</span><span class="sxs-lookup"><span data-stu-id="abe56-118">string</span></span>    | <span data-ttu-id="abe56-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="abe56-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="abe56-120">Wymagane `name` atrybut jest używany jako klucz tej pamięci podręcznej przechowywanych dla każdego wystąpienia Pomocnik tagu rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="abe56-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span> <span data-ttu-id="abe56-121">W odróżnieniu od podstawowych Pomocnik tagu pamięci podręcznej przypisującej klucza do każdego wystąpienia Pomocnik tagu pamięci podręcznej na podstawie nazwy strony Razor i lokalizacja Pomocnik tagu na stronie razor Pomocnik tagu rozproszonej pamięci podręcznej tylko określa jego klucza na na podstawie atrybutu `name`</span><span class="sxs-lookup"><span data-stu-id="abe56-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the Tag Helper in the razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`</span></span>

<span data-ttu-id="abe56-122">Przykład użycia:</span><span class="sxs-lookup"><span data-stu-id="abe56-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="abe56-123">Rozproszone implementacje IDistributedCache Pomocnik tagu pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="abe56-123">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="abe56-124">Istnieją dwie implementacje `IDistributedCache` wbudowanych w platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="abe56-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span> <span data-ttu-id="abe56-125">Jeden opiera się na serwerze SQL Server, a drugi jest oparta na pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="abe56-125">One is based on SQL Server and the other is based on Redis.</span></span> <span data-ttu-id="abe56-126">Szczegóły tych implementacji znajduje się w temacie <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="abe56-126">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="abe56-127">Obu implementacjach obejmują ustawienia wystąpienie `IDistributedCache` w programie ASP.NET Core *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="abe56-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's *Startup.cs*.</span></span>

<span data-ttu-id="abe56-128">Istnieją żadne atrybuty znacznika, w szczególności związanych z użyciem określonych implementacji `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="abe56-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="abe56-129">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="abe56-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
