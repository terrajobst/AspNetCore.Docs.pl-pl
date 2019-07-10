---
title: Obiekt ponownemu użyciu starych z Element ObjectPool w programie ASP.NET Core
author: rick-anderson
description: Porady dotyczące zwiększania wydajności aplikacji platformy ASP.NET Core przy użyciu Element ObjectPool.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 4/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 92106d5add7dbda36e451614429baa0db420f0e8
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724857"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a><span data-ttu-id="c795f-103">Obiekt ponownemu użyciu starych z Element ObjectPool w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c795f-103">Object reuse with ObjectPool in ASP.NET Core</span></span>

<span data-ttu-id="c795f-104">Przez [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c795f-104">By [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c795f-105"><xref:Microsoft.Extensions.ObjectPool> jest częścią infrastruktury platformy ASP.NET Core, która obsługuje program utrzymywanie grupy obiektów w pamięci do ponownego użycia, a nie zezwolenie obiekty do pamięci zbierane.</span><span class="sxs-lookup"><span data-stu-id="c795f-105"><xref:Microsoft.Extensions.ObjectPool> is part of the ASP.NET Core infrastructure that supports keeping a group of objects in memory for reuse rather than allowing the objects to be garbage collected.</span></span>

<span data-ttu-id="c795f-106">Możesz chcieć użyć puli obiektów, jeśli obiekty, które są zarządzane są:</span><span class="sxs-lookup"><span data-stu-id="c795f-106">You might want to use the object pool if the objects that are being managed are:</span></span>

- <span data-ttu-id="c795f-107">Kosztowne przydzielić inicjowania.</span><span class="sxs-lookup"><span data-stu-id="c795f-107">Expensive to allocate/initialize.</span></span>
- <span data-ttu-id="c795f-108">Reprezentuje niektórych ograniczonych zasobów.</span><span class="sxs-lookup"><span data-stu-id="c795f-108">Represent some limited resource.</span></span>
- <span data-ttu-id="c795f-109">Używane przewidywalny i często.</span><span class="sxs-lookup"><span data-stu-id="c795f-109">Used predictably and frequently.</span></span>

<span data-ttu-id="c795f-110">Na przykład platformę ASP.NET Core używa puli obiektów w jednych miejscach do ponownego użycia <xref:System.Text.StringBuilder> wystąpień.</span><span class="sxs-lookup"><span data-stu-id="c795f-110">For example, the ASP.NET Core framework uses the object pool in some places to reuse <xref:System.Text.StringBuilder> instances.</span></span> <span data-ttu-id="c795f-111">`StringBuilder` przydziela i zarządza własną buforów do przechowywania danych znakowych.</span><span class="sxs-lookup"><span data-stu-id="c795f-111">`StringBuilder` allocates and manages its own buffers to hold character data.</span></span> <span data-ttu-id="c795f-112">Platforma ASP.NET Core regularnie używa `StringBuilder` do zaimplementowania funkcji i ponowne użycie ich zapewnia korzyści wydajności.</span><span class="sxs-lookup"><span data-stu-id="c795f-112">ASP.NET Core regularly uses `StringBuilder` to implement features, and reusing them provides a performance benefit.</span></span>

<span data-ttu-id="c795f-113">Buforowanie obiektów nie zawsze poprawi wydajność:</span><span class="sxs-lookup"><span data-stu-id="c795f-113">Object pooling doesn't always improve performance:</span></span>

- <span data-ttu-id="c795f-114">Chyba że koszt inicjowania obiektu jest wysoka, jest zwykle wolniejsze uzyskać obiekt z puli.</span><span class="sxs-lookup"><span data-stu-id="c795f-114">Unless the initialization cost of an object is high, it's usually slower to get the object from the pool.</span></span>
- <span data-ttu-id="c795f-115">Obiekty zarządzane przez pulę nie są dezalokowany momentu dezalokowany puli.</span><span class="sxs-lookup"><span data-stu-id="c795f-115">Objects managed by the pool aren't de-allocated until the pool is de-allocated.</span></span>

<span data-ttu-id="c795f-116">Za pomocą buforowanie obiektu dopiero po zbierania danych wydajności przy użyciu realistycznej scenariuszy dla swojej aplikacji lub biblioteki.</span><span class="sxs-lookup"><span data-stu-id="c795f-116">Use object pooling only after collecting performance data using realistic scenarios for your app or library.</span></span>

<span data-ttu-id="c795f-117">**OSTRZEŻENIE: `ObjectPool` Nie implementuje `IDisposable`. Nie zaleca się korzystania z typów, które muszą usuwania.**</span><span class="sxs-lookup"><span data-stu-id="c795f-117">**WARNING: The `ObjectPool` doesn't implement `IDisposable`. We don't recommend using it with types that need disposal.**</span></span>

<span data-ttu-id="c795f-118">**UWAGA: Element ObjectPool nie umieszcza limit liczby obiektów, które rozdzieli, umieszcza limit liczby obiektów, które go zostaną zachowane.**</span><span class="sxs-lookup"><span data-stu-id="c795f-118">**NOTE: The ObjectPool doesn't place a limit on the number of objects that it will allocate, it places a limit on the number of objects it will retain.**</span></span>

## <a name="concepts"></a><span data-ttu-id="c795f-119">Pojęcia</span><span class="sxs-lookup"><span data-stu-id="c795f-119">Concepts</span></span>

<span data-ttu-id="c795f-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -abstrakcji puli obiektu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="c795f-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> - the basic object pool abstraction.</span></span> <span data-ttu-id="c795f-121">Umożliwia pobieranie i zwracać obiekty.</span><span class="sxs-lookup"><span data-stu-id="c795f-121">Used to get and return objects.</span></span>

<span data-ttu-id="c795f-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> — Ten element, aby dostosować sposób tworzenia obiektu oraz określić sposób implementacji *resetowania* po zwróceniu do puli.</span><span class="sxs-lookup"><span data-stu-id="c795f-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> - implement this to customize how an object is created and how it is *reset* when returned to the pool.</span></span> <span data-ttu-id="c795f-123">To może być przekazywany do puli obiektów, które możesz utworzyć bezpośrednio... LUB</span><span class="sxs-lookup"><span data-stu-id="c795f-123">This can be passed into an object pool that you construct directly.... OR</span></span>

<span data-ttu-id="c795f-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> działa jako fabrykę do tworzenia pul obiektu.</span><span class="sxs-lookup"><span data-stu-id="c795f-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> acts as a factory for creating object pools.</span></span>
<!-- REview, there is no ObjectPoolProvider<T> -->

<span data-ttu-id="c795f-125">Element ObjectPool może być używany w aplikacji na wiele sposobów:</span><span class="sxs-lookup"><span data-stu-id="c795f-125">The ObjectPool can be used in an app in multiple ways:</span></span>

* <span data-ttu-id="c795f-126">Utworzenie wystąpienia puli.</span><span class="sxs-lookup"><span data-stu-id="c795f-126">Instantiating a pool.</span></span>
* <span data-ttu-id="c795f-127">Rejestrowanie puli [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI) jako wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="c795f-127">Registering a pool in [Dependency injection](xref:fundamentals/dependency-injection) (DI) as an instance.</span></span>
* <span data-ttu-id="c795f-128">Rejestrowanie `ObjectPoolProvider<>` DI i używać go jako fabrykę.</span><span class="sxs-lookup"><span data-stu-id="c795f-128">Registering the `ObjectPoolProvider<>` in DI and using it as a factory.</span></span>

## <a name="how-to-use-objectpool"></a><span data-ttu-id="c795f-129">Jak używać Element ObjectPool</span><span class="sxs-lookup"><span data-stu-id="c795f-129">How to use ObjectPool</span></span>

<span data-ttu-id="c795f-130">Wywołaj <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> pobierania obiektu i <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> zwracać obiektu.</span><span class="sxs-lookup"><span data-stu-id="c795f-130">Call <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> to get an object and <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> to return the object.</span></span>  <span data-ttu-id="c795f-131">Nie jest wymagane powrocie każdego obiektu.</span><span class="sxs-lookup"><span data-stu-id="c795f-131">There's no requirement that you return every object.</span></span> <span data-ttu-id="c795f-132">Jeśli nie zwraca obiektu, będzie bezużyteczne.</span><span class="sxs-lookup"><span data-stu-id="c795f-132">If you don't return an object, it will be garbage collected.</span></span>

## <a name="objectpool-sample"></a><span data-ttu-id="c795f-133">Przykładowy element ObjectPool</span><span class="sxs-lookup"><span data-stu-id="c795f-133">ObjectPool sample</span></span>

<span data-ttu-id="c795f-134">Poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="c795f-134">The following code:</span></span>

* <span data-ttu-id="c795f-135">Dodaje `ObjectPoolProvider` do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera (DI).</span><span class="sxs-lookup"><span data-stu-id="c795f-135">Adds `ObjectPoolProvider` to the [Dependency injection](xref:fundamentals/dependency-injection) (DI) container.</span></span>
* <span data-ttu-id="c795f-136">Dodaje i konfiguruje `ObjectPool<StringBuilder>` do kontenera DI.</span><span class="sxs-lookup"><span data-stu-id="c795f-136">Adds and configures `ObjectPool<StringBuilder>` to the DI container.</span></span>
* <span data-ttu-id="c795f-137">Dodaje `BirthdayMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="c795f-137">Adds the `BirthdayMiddleware`.</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

<span data-ttu-id="c795f-138">Poniższy kod implementuje `BirthdayMiddleware`</span><span class="sxs-lookup"><span data-stu-id="c795f-138">The following code implements `BirthdayMiddleware`</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
