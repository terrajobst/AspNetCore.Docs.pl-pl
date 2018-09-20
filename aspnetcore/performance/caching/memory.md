---
title: Buforowanie w pamięci w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dane w pamięci w programie ASP.NET Core z pamięci podręcznej.
ms.author: riande
ms.custom: mvc
ms.date: 09/15/2018
uid: performance/caching/memory
ms.openlocfilehash: 2570ad7d939d67530b3de8cd0147815c2e25ecc8
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482986"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="b0c56-103">Buforowanie w pamięci w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b0c56-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="b0c56-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b0c56-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b0c56-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b0c56-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="b0c56-106">Podstawy pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="b0c56-106">Caching basics</span></span>

<span data-ttu-id="b0c56-107">Buforowanie może znacznie poprawić wydajność i skalowalność aplikacji, zmniejszając pracy wymaganej do generowania zawartości.</span><span class="sxs-lookup"><span data-stu-id="b0c56-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="b0c56-108">Buforowanie działa najlepiej z danymi, które zmieniają się rzadko.</span><span class="sxs-lookup"><span data-stu-id="b0c56-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="b0c56-109">Buforowanie sprawia, że kopii danych, które mogą być zwrócone znacznie szybciej niż z oryginalnego źródła.</span><span class="sxs-lookup"><span data-stu-id="b0c56-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="b0c56-110">Napisz, a następnie przetestować aplikację, aby nigdy nie są zależne od danych w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="b0c56-111">Platforma ASP.NET Core obsługuje kilka różnych pamięci podręcznych.</span><span class="sxs-lookup"><span data-stu-id="b0c56-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="b0c56-112">Najprostsza cache jest oparta na [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), która reprezentuje pamięć podręczną przechowywane w pamięci serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="b0c56-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="b0c56-113">Aplikacje działające w farmie serwerów wielu serwerów należy upewnić się, sesje są umocowany, korzystając z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="b0c56-114">Trwałych sesji upewnij się, że kolejne żądania z klienta wszystkich przejdź do tego samego serwera.</span><span class="sxs-lookup"><span data-stu-id="b0c56-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="b0c56-115">Na przykład użycie aplikacji sieci Web platformy Azure [Routing żądań aplikacji](https://www.iis.net/learn/extensions/planning-for-arr) (ARR), aby przekierować wszystkie kolejne żądania do tego samego serwera.</span><span class="sxs-lookup"><span data-stu-id="b0c56-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="b0c56-116">Non trwałych sesji w ramach farmy sieci web wymagają [rozproszonej pamięci podręcznej](distributed.md) Aby uniknąć problemów spójności pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="b0c56-117">W przypadku niektórych aplikacji rozproszonej pamięci podręcznej może obsługiwać wyższe skalowalnego w poziomie niż w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-117">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="b0c56-118">Za pomocą rozproszonej pamięci podręcznej mniejsze pamięci podręcznej procesu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="b0c56-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="b0c56-119">`IMemoryCache` Pamięci podręcznej Wyklucz wpisy w pamięci podręcznej duże wykorzystanie pamięci, chyba że [priorytet w pamięci podręcznej](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) ustawiono `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="b0c56-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="b0c56-120">Możesz ustawić `CacheItemPriority` aby dopasować priorytet za pomocą której pamięci podręcznej wyklucza mogą elementów duże wykorzystanie pamięci.</span><span class="sxs-lookup"><span data-stu-id="b0c56-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="b0c56-121">Wewnątrzpamięciowa pamięć podręczna może przechowywać dowolny obiekt; Interfejs rozproszonej pamięci podręcznej jest ograniczona do `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="b0c56-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="systemruntimecachingmemorycache"></a><span data-ttu-id="b0c56-122">System.Runtime.Caching/MemoryCache</span><span class="sxs-lookup"><span data-stu-id="b0c56-122">System.Runtime.Caching/MemoryCache</span></span>

<span data-ttu-id="b0c56-123"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([Pakietu NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) mogą być używane z:</span><span class="sxs-lookup"><span data-stu-id="b0c56-123"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet package](https://www.nuget.org/packages/System.Runtime.Caching/)) can be used with:</span></span>

* <span data-ttu-id="b0c56-124">.NET standard 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-124">.NET Standard 2.0 or later.</span></span>
* <span data-ttu-id="b0c56-125">Wszelkie [implementacji .NET](/dotnet/standard/net-standard#net-implementation-support) który jest przeznaczony dla .NET Standard 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-125">Any [.NET implementation](/dotnet/standard/net-standard#net-implementation-support) that targets .NET Standard 2.0 or later.</span></span> <span data-ttu-id="b0c56-126">Na przykład platforma ASP.NET Core 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-126">For example, ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="b0c56-127">.NET framework 4.5 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-127">.NET Framework 4.5 or later.</span></span>

<span data-ttu-id="b0c56-128">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (opisanych w tym temacie) są zalecane przez `System.Runtime.Caching` / `MemoryCache` ponieważ lepiej jest zintegrowana platforma ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b0c56-128">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (described in this topic) is recommended over `System.Runtime.Caching`/`MemoryCache` because it's better integrated into ASP.NET Core.</span></span> <span data-ttu-id="b0c56-129">Na przykład `IMemoryCache` działa natywnie przy użyciu platformy ASP.NET Core [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b0c56-129">For example, `IMemoryCache` works natively with ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="b0c56-130">Użyj `System.Runtime.Caching` / `MemoryCache` jako Most zgodności podczas przenoszenia kodu z ASP.NET 4.x, ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b0c56-130">Use `System.Runtime.Caching`/`MemoryCache` as a compatibility bridge when porting code from ASP.NET 4.x to ASP.NET Core.</span></span>

## <a name="cache-guidelines"></a><span data-ttu-id="b0c56-131">Wskazówki dotyczące pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="b0c56-131">Cache guidelines</span></span>

* <span data-ttu-id="b0c56-132">Kod powinien zawsze mieć opcja przełączania awaryjnego można pobrać danych i **nie** zależą od wartość w pamięci podręcznej jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="b0c56-132">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="b0c56-133">Pamięć podręczna używa ograniczonych zasobów pamięci.</span><span class="sxs-lookup"><span data-stu-id="b0c56-133">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="b0c56-134">Limit pamięci podręcznej wzrost:</span><span class="sxs-lookup"><span data-stu-id="b0c56-134">Limit cache growth:</span></span>
  * <span data-ttu-id="b0c56-135">Czy **nie** zewnętrzne dane wejściowe są używane jako klucze pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-135">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="b0c56-136">Wygasanie ważności poświadczeń można użyć, aby ograniczyć wzrostu pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-136">Use expirations to limit cache growth.</span></span>
  * [<span data-ttu-id="b0c56-137">SetSize użycie, rozmiar i SizeLimit limit rozmiaru pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="b0c56-137">Use SetSize, Size, and SizeLimit to limit cache size</span></span>](#use-setsize-size-and-sizelimit-to-limit-cache-size)

## <a name="using-imemorycache"></a><span data-ttu-id="b0c56-138">Za pomocą IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="b0c56-138">Using IMemoryCache</span></span>

<span data-ttu-id="b0c56-139">Buforowanie w pamięci jest *usługi* , o której mowa w aplikacji przy użyciu [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="b0c56-139">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="b0c56-140">Wywołaj `AddMemoryCache` w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b0c56-140">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="b0c56-141">Żądanie `IMemoryCache` wystąpienia w Konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="b0c56-141">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b0c56-142">`IMemoryCache` wymaga pakietu NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="b0c56-142">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b0c56-143">`IMemoryCache` wymaga pakietu NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), który jest dostępny w [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="b0c56-143">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="b0c56-144">`IMemoryCache` wymaga pakietu NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), który jest dostępny w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b0c56-144">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="b0c56-145">Poniższy kod używa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) do sprawdzenia, czy w pamięci podręcznej przez czas.</span><span class="sxs-lookup"><span data-stu-id="b0c56-145">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="b0c56-146">Jeśli nie jest buforowany przez czas, nowy wpis jest utworzony i dodany do pamięci podręcznej przy użyciu [ustaw](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="b0c56-146">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="b0c56-147">Bieżący czas i czas pamięci podręcznej są wyświetlane:</span><span class="sxs-lookup"><span data-stu-id="b0c56-147">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="b0c56-148">Buforowane `DateTime` wartość pozostaje w pamięci podręcznej, gdy istnieją żądań w określonym przedziale czasu (i nie wykluczania z powodu braku pamięci).</span><span class="sxs-lookup"><span data-stu-id="b0c56-148">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="b0c56-149">Na poniższej ilustracji przedstawiono bieżący czas i czas starsze, pobierane z pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="b0c56-149">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Widok indeksu z dwóch różnych godzinach wyświetlane](memory/_static/time.png)

<span data-ttu-id="b0c56-151">Poniższy kod używa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) i [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) do buforowania danych.</span><span class="sxs-lookup"><span data-stu-id="b0c56-151">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="b0c56-152">Poniższy kod wywoła [uzyskać](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) można pobrać czasu pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="b0c56-152">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="b0c56-153">Zobacz [metody IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) i [metody CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) opis metod pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-153">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="b0c56-154">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="b0c56-154">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="b0c56-155">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="b0c56-155">The following sample:</span></span>

- <span data-ttu-id="b0c56-156">Ustawia czas wygaśnięcia bezwzględne.</span><span class="sxs-lookup"><span data-stu-id="b0c56-156">Sets the absolute expiration time.</span></span> <span data-ttu-id="b0c56-157">To jest maksymalny czas, które mogą być buforowane wpis i uniemożliwia element staje się zbyt stare przy wygaśniecie stale zostanie odnowiony.</span><span class="sxs-lookup"><span data-stu-id="b0c56-157">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="b0c56-158">Ustawia przewijania czas wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="b0c56-158">Sets a sliding expiration time.</span></span> <span data-ttu-id="b0c56-159">Żądania, uzyskujących dostęp do tego elementu pamięci podręcznej spowoduje zresetowanie przewijania zegara wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="b0c56-159">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="b0c56-160">Ustawia priorytet pamięci podręcznej na `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="b0c56-160">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="b0c56-161">Zestawy [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , zostanie wywołana po wpis zostanie usunięty z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-161">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="b0c56-162">Wywołanie zwrotne jest uruchamiane w innym wątku z kodu, który usuwa element z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-162">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="b0c56-163">SetSize użycie, rozmiar i SizeLimit limit rozmiaru pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="b0c56-163">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="b0c56-164">A `MemoryCache` wystąpienia, mogą opcjonalnie ustalenie i wymuszenie limit rozmiaru.</span><span class="sxs-lookup"><span data-stu-id="b0c56-164">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="b0c56-165">Limit rozmiaru pamięci nie ma zdefiniowanych jednostka miary, ponieważ żaden mechanizm do mierzenia rozmiar wpisów pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-165">The memory size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="b0c56-166">Jeśli ustawiono limit rozmiaru pamięci podręcznej, wszystkie wpisy, należy określić rozmiar.</span><span class="sxs-lookup"><span data-stu-id="b0c56-166">If the cache memory size limit is set, all entries must specify size.</span></span> <span data-ttu-id="b0c56-167">Określony rozmiar jest wyrażona w jednostkach, który wybiera dewelopera.</span><span class="sxs-lookup"><span data-stu-id="b0c56-167">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="b0c56-168">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b0c56-168">For example:</span></span>

* <span data-ttu-id="b0c56-169">Jeśli aplikacja sieci web został przede wszystkim buforowania ciągów, rozmiar każdego wpisu pamięci podręcznej może być długość ciągu.</span><span class="sxs-lookup"><span data-stu-id="b0c56-169">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="b0c56-170">Aplikacja może określić rozmiar wszystkich wpisów jako 1, a limit rozmiaru wynosi liczba wpisów.</span><span class="sxs-lookup"><span data-stu-id="b0c56-170">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="b0c56-171">Poniższy kod tworzy unitless ustalony rozmiar [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) dostępny za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="b0c56-171">The following code creates a unitless fixed size [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="b0c56-172">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) nie mieć jednostki.</span><span class="sxs-lookup"><span data-stu-id="b0c56-172">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) does not have units.</span></span> <span data-ttu-id="b0c56-173">Wpisy pamięci podręcznej, należy określić rozmiar w dowolnych jednostkach uznają za najbardziej odpowiednie, jeśli rozmiar pamięci podręcznej został ustawiony.</span><span class="sxs-lookup"><span data-stu-id="b0c56-173">Cached entries must specify size in whatever units they deem most appropriate if the cache memory size has been set.</span></span> <span data-ttu-id="b0c56-174">Wszyscy użytkownicy wystąpienia pamięci podręcznej należy używać tego samego systemu jednostki.</span><span class="sxs-lookup"><span data-stu-id="b0c56-174">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="b0c56-175">Wpis nie będzie zapisywane, jeśli suma rozmiarów wpisu pamięci podręcznej przekracza wartość określoną przez `SizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="b0c56-175">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="b0c56-176">Jeśli nie mają limitu rozmiaru pamięci podręcznej jest ustawiona, rozmiar pamięci podręcznej wpisu zostaną zignorowane.</span><span class="sxs-lookup"><span data-stu-id="b0c56-176">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="b0c56-177">Poniższy kod rejestruje `MyMemoryCache` z [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="b0c56-177">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="b0c56-178">`MyMemoryCache` zostanie utworzona jako niezależne pamięci podręcznej dla składników, które zdawali sobie sprawę z tego ograniczony rozmiar pamięci podręcznej i dowiedzieć się, jak odpowiednie ustawienie rozmiaru wpisu pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-178">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="b0c56-179">Poniższy kod używa `MyMemoryCache`:</span><span class="sxs-lookup"><span data-stu-id="b0c56-179">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

<span data-ttu-id="b0c56-180">Można ustawić rozmiaru wpisu pamięci podręcznej [rozmiar](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) lub [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="b0c56-180">The size of the cache entry can be set by [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) or the [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) extension method:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a><span data-ttu-id="b0c56-181">Zależności pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="b0c56-181">Cache dependencies</span></span>

<span data-ttu-id="b0c56-182">Poniższy przykład pokazuje, jak wygaśnie wpisu pamięci podręcznej, jeśli wygaśnięcia wpisu zależnych.</span><span class="sxs-lookup"><span data-stu-id="b0c56-182">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="b0c56-183">Element `CancellationChangeToken` jest dodawana do pamięci podręcznej elementu.</span><span class="sxs-lookup"><span data-stu-id="b0c56-183">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="b0c56-184">Gdy `Cancel` jest wywoływana w `CancellationTokenSource`, obrazuje zarówno wpisy w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-184">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="b0c56-185">Za pomocą `CancellationTokenSource` zezwala na wiele wpisów pamięci podręcznej wykluczenie jako grupa.</span><span class="sxs-lookup"><span data-stu-id="b0c56-185">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="b0c56-186">Za pomocą `using` wzorca w powyższym kodzie, wpisy w pamięci podręcznej utworzony wewnątrz `using` bloku będzie dziedziczyć wyzwalaczy i ustawienia wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="b0c56-186">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="b0c56-187">Dodatkowe uwagi</span><span class="sxs-lookup"><span data-stu-id="b0c56-187">Additional notes</span></span>

- <span data-ttu-id="b0c56-188">Korzystając z wywołania zwrotnego do wypełnienia elementu pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="b0c56-188">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="b0c56-189">Wiele żądań można znaleźć wartość klucza w pamięci podręcznej pusty, ponieważ wywołanie zwrotne nie zostało ukończone.</span><span class="sxs-lookup"><span data-stu-id="b0c56-189">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="b0c56-190">Może to spowodować, że wiele wątków uważnie element pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-190">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="b0c56-191">Gdy jeden wpis pamięci podręcznej jest używany do tworzenia kolejnych, elementu podrzędnego kopiuje wpis nadrzędny wygaśnięcia tokenów i ustawienia na podstawie czasu wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="b0c56-191">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="b0c56-192">Element podrzędny nie jest wygasły przez ręczne usunięcie lub aktualizowania wpisu nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="b0c56-192">The child isn't expired by manual removal or updating of the parent entry.</span></span>

- <span data-ttu-id="b0c56-193">Użyj [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) można ustawić wywołania zwrotne, które będą uruchamiane po wpisu pamięci podręcznej zostanie usunięty z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="b0c56-193">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0c56-194">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b0c56-194">Additional resources</span></span>

* [<span data-ttu-id="b0c56-195">Praca z rozproszoną pamięcią podręczną</span><span class="sxs-lookup"><span data-stu-id="b0c56-195">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="b0c56-196">Wykrywanie zmian za pomocą tokenów zmiany</span><span class="sxs-lookup"><span data-stu-id="b0c56-196">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="b0c56-197">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="b0c56-197">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="b0c56-198">Oprogramowanie pośredniczące buforowania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="b0c56-198">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="b0c56-199">Pomocnik tagu pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="b0c56-199">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="b0c56-200">Pomocnik tagu rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="b0c56-200">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
