---
title: Buforowanie w pamięci w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dane w pamięci w programie ASP.NET Core z pamięci podręcznej.
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: performance/caching/memory
ms.openlocfilehash: 9a7727ad41a05f39d74877af3c8f2e3f7a620c7d
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56103075"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="9aa00-103">Buforowanie w pamięci w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9aa00-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="9aa00-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9aa00-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9aa00-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9aa00-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="9aa00-106">Podstawy pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="9aa00-106">Caching basics</span></span>

<span data-ttu-id="9aa00-107">Buforowanie może znacznie poprawić wydajność i skalowalność aplikacji, zmniejszając pracy wymaganej do generowania zawartości.</span><span class="sxs-lookup"><span data-stu-id="9aa00-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="9aa00-108">Buforowanie działa najlepiej z danymi, które zmieniają się rzadko.</span><span class="sxs-lookup"><span data-stu-id="9aa00-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="9aa00-109">Buforowanie sprawia, że kopii danych, które mogą być zwrócone znacznie szybciej niż z oryginalnego źródła.</span><span class="sxs-lookup"><span data-stu-id="9aa00-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="9aa00-110">Napisz, a następnie przetestować aplikację, aby nigdy nie są zależne od danych w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="9aa00-111">Platforma ASP.NET Core obsługuje kilka różnych pamięci podręcznych.</span><span class="sxs-lookup"><span data-stu-id="9aa00-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="9aa00-112">Najprostsza cache jest oparta na [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), która reprezentuje pamięć podręczną przechowywane w pamięci serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="9aa00-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="9aa00-113">Aplikacje działające w farmie serwerów wielu serwerów należy upewnić się, sesje są umocowany, korzystając z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="9aa00-114">Trwałych sesji upewnij się, że kolejne żądania z klienta wszystkich przejdź do tego samego serwera.</span><span class="sxs-lookup"><span data-stu-id="9aa00-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="9aa00-115">Na przykład użycie aplikacji sieci Web platformy Azure [Routing żądań aplikacji](https://www.iis.net/learn/extensions/planning-for-arr) (ARR), aby przekierować wszystkie kolejne żądania do tego samego serwera.</span><span class="sxs-lookup"><span data-stu-id="9aa00-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="9aa00-116">Non trwałych sesji w ramach farmy sieci web wymagają [rozproszonej pamięci podręcznej](distributed.md) Aby uniknąć problemów spójności pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="9aa00-117">W przypadku niektórych aplikacji rozproszonej pamięci podręcznej może obsługiwać wyższe skalowalnego w poziomie niż w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-117">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="9aa00-118">Za pomocą rozproszonej pamięci podręcznej mniejsze pamięci podręcznej procesu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="9aa00-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9aa00-119">`IMemoryCache` Pamięci podręcznej Wyklucz wpisy w pamięci podręcznej duże wykorzystanie pamięci, chyba że [priorytet w pamięci podręcznej](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) ustawiono `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="9aa00-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="9aa00-120">Możesz ustawić `CacheItemPriority` aby dopasować priorytet za pomocą której pamięci podręcznej wyklucza mogą elementów duże wykorzystanie pamięci.</span><span class="sxs-lookup"><span data-stu-id="9aa00-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

::: moniker-end

<span data-ttu-id="9aa00-121">Wewnątrzpamięciowa pamięć podręczna może przechowywać dowolny obiekt; Interfejs rozproszonej pamięci podręcznej jest ograniczona do `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="9aa00-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span> <span data-ttu-id="9aa00-122">Elementy w pamięci i rozproszonej pamięci podręcznej magazynu pamięci podręcznej jako pary klucz wartość.</span><span class="sxs-lookup"><span data-stu-id="9aa00-122">The in-memory and distributed cache store cache items as key-value pairs.</span></span>

## <a name="systemruntimecachingmemorycache"></a><span data-ttu-id="9aa00-123">System.Runtime.Caching/MemoryCache</span><span class="sxs-lookup"><span data-stu-id="9aa00-123">System.Runtime.Caching/MemoryCache</span></span>

<span data-ttu-id="9aa00-124"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([Pakietu NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) mogą być używane z:</span><span class="sxs-lookup"><span data-stu-id="9aa00-124"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet package](https://www.nuget.org/packages/System.Runtime.Caching/)) can be used with:</span></span>

* <span data-ttu-id="9aa00-125">.NET standard 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-125">.NET Standard 2.0 or later.</span></span>
* <span data-ttu-id="9aa00-126">Wszelkie [implementacji .NET](/dotnet/standard/net-standard#net-implementation-support) który jest przeznaczony dla .NET Standard 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-126">Any [.NET implementation](/dotnet/standard/net-standard#net-implementation-support) that targets .NET Standard 2.0 or later.</span></span> <span data-ttu-id="9aa00-127">Na przykład platforma ASP.NET Core 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-127">For example, ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="9aa00-128">.NET framework 4.5 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-128">.NET Framework 4.5 or later.</span></span>

<span data-ttu-id="9aa00-129">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (opisanych w tym temacie) są zalecane przez `System.Runtime.Caching` / `MemoryCache` ponieważ lepiej jest zintegrowana platforma ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9aa00-129">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (described in this topic) is recommended over `System.Runtime.Caching`/`MemoryCache` because it's better integrated into ASP.NET Core.</span></span> <span data-ttu-id="9aa00-130">Na przykład `IMemoryCache` działa natywnie przy użyciu platformy ASP.NET Core [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9aa00-130">For example, `IMemoryCache` works natively with ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="9aa00-131">Użyj `System.Runtime.Caching` / `MemoryCache` jako Most zgodności podczas przenoszenia kodu z ASP.NET 4.x, ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9aa00-131">Use `System.Runtime.Caching`/`MemoryCache` as a compatibility bridge when porting code from ASP.NET 4.x to ASP.NET Core.</span></span>

## <a name="cache-guidelines"></a><span data-ttu-id="9aa00-132">Wskazówki dotyczące pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="9aa00-132">Cache guidelines</span></span>

* <span data-ttu-id="9aa00-133">Kod powinien zawsze mieć opcja przełączania awaryjnego można pobrać danych i **nie** zależą od wartość w pamięci podręcznej jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="9aa00-133">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="9aa00-134">Pamięć podręczna używa ograniczonych zasobów pamięci.</span><span class="sxs-lookup"><span data-stu-id="9aa00-134">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="9aa00-135">Limit pamięci podręcznej wzrost:</span><span class="sxs-lookup"><span data-stu-id="9aa00-135">Limit cache growth:</span></span>
  * <span data-ttu-id="9aa00-136">Czy **nie** zewnętrzne dane wejściowe są używane jako klucze pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-136">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="9aa00-137">Wygasanie ważności poświadczeń można użyć, aby ograniczyć wzrostu pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-137">Use expirations to limit cache growth.</span></span>
  * [<span data-ttu-id="9aa00-138">SetSize użycie, rozmiar i SizeLimit limit rozmiaru pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="9aa00-138">Use SetSize, Size, and SizeLimit to limit cache size</span></span>](#use-setsize-size-and-sizelimit-to-limit-cache-size)

## <a name="using-imemorycache"></a><span data-ttu-id="9aa00-139">Za pomocą IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="9aa00-139">Using IMemoryCache</span></span>

<span data-ttu-id="9aa00-140">Buforowanie w pamięci jest *usługi* , o której mowa w aplikacji przy użyciu [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="9aa00-140">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="9aa00-141">Wywołaj `AddMemoryCache` w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9aa00-141">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="9aa00-142">Żądanie `IMemoryCache` wystąpienia w Konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="9aa00-142">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9aa00-143">`IMemoryCache` wymaga pakietu NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="9aa00-143">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9aa00-144">`IMemoryCache` wymaga pakietu NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), który jest dostępny w [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="9aa00-144">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="9aa00-145">`IMemoryCache` wymaga pakietu NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), który jest dostępny w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="9aa00-145">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="9aa00-146">Poniższy kod używa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) do sprawdzenia, czy w pamięci podręcznej przez czas.</span><span class="sxs-lookup"><span data-stu-id="9aa00-146">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="9aa00-147">Jeśli nie jest buforowany przez czas, nowy wpis jest utworzony i dodany do pamięci podręcznej przy użyciu [ustaw](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="9aa00-147">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="9aa00-148">Bieżący czas i czas pamięci podręcznej są wyświetlane:</span><span class="sxs-lookup"><span data-stu-id="9aa00-148">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="9aa00-149">Buforowane `DateTime` wartość pozostaje w pamięci podręcznej, gdy istnieją żądań w określonym przedziale czasu (i nie wykluczania z powodu braku pamięci).</span><span class="sxs-lookup"><span data-stu-id="9aa00-149">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="9aa00-150">Na poniższej ilustracji przedstawiono bieżący czas i czas starsze, pobierane z pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="9aa00-150">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Widok indeksu z dwóch różnych godzinach wyświetlane](memory/_static/time.png)

<span data-ttu-id="9aa00-152">Poniższy kod używa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) i [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) do buforowania danych.</span><span class="sxs-lookup"><span data-stu-id="9aa00-152">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="9aa00-153">Poniższy kod wywoła [uzyskać](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) można pobrać czasu pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="9aa00-153">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="9aa00-154"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*> , <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, i [uzyskać](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) wchodzą w skład metod rozszerzenia [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) klasy, która rozszerza możliwości <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="9aa00-154"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*> , <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, and [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) are extension methods part of the [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) class that extends the capability of <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="9aa00-155">Zobacz [metody IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) i [metody CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) opis innych metod pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-155">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of other cache methods.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="9aa00-156">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="9aa00-156">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="9aa00-157">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="9aa00-157">The following sample:</span></span>

- <span data-ttu-id="9aa00-158">Ustawia czas wygaśnięcia bezwzględne.</span><span class="sxs-lookup"><span data-stu-id="9aa00-158">Sets the absolute expiration time.</span></span> <span data-ttu-id="9aa00-159">To jest maksymalny czas, które mogą być buforowane wpis i uniemożliwia element staje się zbyt stare przy wygaśniecie stale zostanie odnowiony.</span><span class="sxs-lookup"><span data-stu-id="9aa00-159">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="9aa00-160">Ustawia przewijania czas wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="9aa00-160">Sets a sliding expiration time.</span></span> <span data-ttu-id="9aa00-161">Żądania, uzyskujących dostęp do tego elementu pamięci podręcznej spowoduje zresetowanie przewijania zegara wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="9aa00-161">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="9aa00-162">Ustawia priorytet pamięci podręcznej na `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="9aa00-162">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="9aa00-163">Zestawy [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , zostanie wywołana po wpis zostanie usunięty z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-163">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="9aa00-164">Wywołanie zwrotne jest uruchamiane w innym wątku z kodu, który usuwa element z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-164">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="9aa00-165">SetSize użycie, rozmiar i SizeLimit limit rozmiaru pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="9aa00-165">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="9aa00-166">A `MemoryCache` wystąpienia, mogą opcjonalnie ustalenie i wymuszenie limit rozmiaru.</span><span class="sxs-lookup"><span data-stu-id="9aa00-166">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="9aa00-167">Limit rozmiaru pamięci nie ma zdefiniowanych jednostka miary, ponieważ żaden mechanizm do mierzenia rozmiar wpisów pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-167">The memory size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="9aa00-168">Jeśli ustawiono limit rozmiaru pamięci podręcznej, wszystkie wpisy, należy określić rozmiar.</span><span class="sxs-lookup"><span data-stu-id="9aa00-168">If the cache memory size limit is set, all entries must specify size.</span></span> <span data-ttu-id="9aa00-169">Określony rozmiar jest wyrażona w jednostkach, który wybiera dewelopera.</span><span class="sxs-lookup"><span data-stu-id="9aa00-169">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="9aa00-170">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="9aa00-170">For example:</span></span>

* <span data-ttu-id="9aa00-171">Jeśli aplikacja sieci web został przede wszystkim buforowania ciągów, rozmiar każdego wpisu pamięci podręcznej może być długość ciągu.</span><span class="sxs-lookup"><span data-stu-id="9aa00-171">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="9aa00-172">Aplikacja może określić rozmiar wszystkich wpisów jako 1, a limit rozmiaru wynosi liczba wpisów.</span><span class="sxs-lookup"><span data-stu-id="9aa00-172">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="9aa00-173">Poniższy kod tworzy unitless ustalony rozmiar [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) dostępny za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="9aa00-173">The following code creates a unitless fixed size [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="9aa00-174">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) nie mieć jednostki.</span><span class="sxs-lookup"><span data-stu-id="9aa00-174">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) does not have units.</span></span> <span data-ttu-id="9aa00-175">Wpisy pamięci podręcznej, należy określić rozmiar w dowolnych jednostkach uznają za najbardziej odpowiednie, jeśli rozmiar pamięci podręcznej został ustawiony.</span><span class="sxs-lookup"><span data-stu-id="9aa00-175">Cached entries must specify size in whatever units they deem most appropriate if the cache memory size has been set.</span></span> <span data-ttu-id="9aa00-176">Wszyscy użytkownicy wystąpienia pamięci podręcznej należy używać tego samego systemu jednostki.</span><span class="sxs-lookup"><span data-stu-id="9aa00-176">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="9aa00-177">Wpis nie będzie zapisywane, jeśli suma rozmiarów wpisu pamięci podręcznej przekracza wartość określoną przez `SizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="9aa00-177">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="9aa00-178">Jeśli nie mają limitu rozmiaru pamięci podręcznej jest ustawiona, rozmiar pamięci podręcznej wpisu zostaną zignorowane.</span><span class="sxs-lookup"><span data-stu-id="9aa00-178">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="9aa00-179">Poniższy kod rejestruje `MyMemoryCache` z [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="9aa00-179">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="9aa00-180">`MyMemoryCache` zostanie utworzona jako niezależne pamięci podręcznej dla składników, które zdawali sobie sprawę z tego ograniczony rozmiar pamięci podręcznej i dowiedzieć się, jak odpowiednie ustawienie rozmiaru wpisu pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-180">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="9aa00-181">Poniższy kod używa `MyMemoryCache`:</span><span class="sxs-lookup"><span data-stu-id="9aa00-181">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

<span data-ttu-id="9aa00-182">Można ustawić rozmiaru wpisu pamięci podręcznej [rozmiar](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) lub [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="9aa00-182">The size of the cache entry can be set by [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) or the [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) extension method:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a><span data-ttu-id="9aa00-183">Zależności pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="9aa00-183">Cache dependencies</span></span>

<span data-ttu-id="9aa00-184">Poniższy przykład pokazuje, jak wygaśnie wpisu pamięci podręcznej, jeśli wygaśnięcia wpisu zależnych.</span><span class="sxs-lookup"><span data-stu-id="9aa00-184">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="9aa00-185">Element `CancellationChangeToken` jest dodawana do pamięci podręcznej elementu.</span><span class="sxs-lookup"><span data-stu-id="9aa00-185">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="9aa00-186">Gdy `Cancel` jest wywoływana w `CancellationTokenSource`, obrazuje zarówno wpisy w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-186">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="9aa00-187">Za pomocą `CancellationTokenSource` zezwala na wiele wpisów pamięci podręcznej wykluczenie jako grupa.</span><span class="sxs-lookup"><span data-stu-id="9aa00-187">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="9aa00-188">Za pomocą `using` wzorca w powyższym kodzie, wpisy w pamięci podręcznej utworzony wewnątrz `using` bloku będzie dziedziczyć wyzwalaczy i ustawienia wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="9aa00-188">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="9aa00-189">Dodatkowe uwagi</span><span class="sxs-lookup"><span data-stu-id="9aa00-189">Additional notes</span></span>

- <span data-ttu-id="9aa00-190">Korzystając z wywołania zwrotnego do wypełnienia elementu pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="9aa00-190">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="9aa00-191">Wiele żądań można znaleźć wartość klucza w pamięci podręcznej pusty, ponieważ wywołanie zwrotne nie zostało ukończone.</span><span class="sxs-lookup"><span data-stu-id="9aa00-191">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="9aa00-192">Może to spowodować, że wiele wątków uważnie element pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-192">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="9aa00-193">Gdy jeden wpis pamięci podręcznej jest używany do tworzenia kolejnych, elementu podrzędnego kopiuje wpis nadrzędny wygaśnięcia tokenów i ustawienia na podstawie czasu wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="9aa00-193">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="9aa00-194">Element podrzędny nie jest wygasły przez ręczne usunięcie lub aktualizowania wpisu nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="9aa00-194">The child isn't expired by manual removal or updating of the parent entry.</span></span>

- <span data-ttu-id="9aa00-195">Użyj [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) można ustawić wywołania zwrotne, które będą uruchamiane po wpisu pamięci podręcznej zostanie usunięty z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="9aa00-195">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9aa00-196">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9aa00-196">Additional resources</span></span>

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
