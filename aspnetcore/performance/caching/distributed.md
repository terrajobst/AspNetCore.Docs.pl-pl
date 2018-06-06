---
title: Praca z rozproszonej pamięci podręcznej w ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać platformy ASP.NET Core rozproszonych buforowanie, aby zwiększyć wydajność aplikacji i skalowalność, szczególnie w środowisku farmy chmury lub serwera.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: 6c595572641604d241c0c8f702d4f392afe34f71
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734461"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="77886-103">Praca z rozproszonej pamięci podręcznej w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77886-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="77886-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="77886-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="77886-105">Rozproszonej pamięci podręcznej może zwiększyć wydajność i skalowalność aplikacji platformy ASP.NET Core, szczególnie w przypadku hostowanych w środowisku farmy chmury lub serwera.</span><span class="sxs-lookup"><span data-stu-id="77886-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="77886-106">W tym artykule opisano sposób pracy z implementacji i abstrakcje wbudowanych rozproszonej pamięci podręcznej platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77886-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

<span data-ttu-id="77886-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="77886-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="77886-108">Co to jest rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="77886-108">What is a distributed cache</span></span>

<span data-ttu-id="77886-109">Rozproszonej pamięci podręcznej jest współużytkowany przez wiele serwerów aplikacji (zobacz [podstawowe informacje o pamięci podręcznej](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="77886-109">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="77886-110">Informacje w pamięci podręcznej nie jest przechowywany w pamięci poszczególnych sieci web, serwerów i buforowane dane są dostępne dla wszystkich serwerów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="77886-110">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="77886-111">To zapewnia kilka korzyści:</span><span class="sxs-lookup"><span data-stu-id="77886-111">This provides several advantages:</span></span>

1. <span data-ttu-id="77886-112">Buforowane dane są spójne na wszystkich serwerach sieci web.</span><span class="sxs-lookup"><span data-stu-id="77886-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="77886-113">Użytkownicy nie będą widzieli różne wyniki, w zależności od tego, które sieci web serwer obsługuje żądania</span><span class="sxs-lookup"><span data-stu-id="77886-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="77886-114">Buforowane dane przeżyje ponownego uruchomienia serwera sieci web i wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="77886-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="77886-115">Serwery sieci web poszczególnych można usunięte lub dodane bez wpływu na pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="77886-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="77886-116">Źródłowy magazyn danych ma mniejszą liczbę żądań wysyłanych do niego (nie z wielu buforów w pamięci lub nie pamięci podręcznej w ogóle).</span><span class="sxs-lookup"><span data-stu-id="77886-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="77886-117">Jeśli używasz programu SQL Server rozproszonej pamięci podręcznej, niektóre z tych zalet są tylko wartość true, jeśli wystąpienie osobna baza danych jest używana dla pamięci podręcznej niż aplikacji źródła danych.</span><span class="sxs-lookup"><span data-stu-id="77886-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="77886-118">Podobnie jak wszelkie pamięci podręcznej rozproszonej pamięci podręcznej mogą znacznie zwiększyć czas odpowiedzi aplikacji, ponieważ zwykle można pobrać danych z pamięci podręcznej znacznie szybciej niż z relacyjnej bazy danych (lub usługa sieci web).</span><span class="sxs-lookup"><span data-stu-id="77886-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="77886-119">Konfigurację pamięci podręcznej zależy od implementacji.</span><span class="sxs-lookup"><span data-stu-id="77886-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="77886-120">W tym artykule opisano, jak skonfigurować zarówno Redis i buforuje rozproszonych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="77886-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="77886-121">Niezależnie od tego, którego implementacja jest zaznaczone, aplikacja współdziała z pamięci podręcznej, za pomocą wspólnego `IDistributedCache` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="77886-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="77886-122">Interfejs IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="77886-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="77886-123">`IDistributedCache` Interfejs zawiera metody synchroniczne i asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="77886-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="77886-124">Interfejs umożliwia elementów do dodania, pobrać i usunięcia z implementacja rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="77886-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="77886-125">`IDistributedCache` Interfejs zawiera następujące metody:</span><span class="sxs-lookup"><span data-stu-id="77886-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="77886-126">**GET, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="77886-126">**Get, GetAsync**</span></span>

<span data-ttu-id="77886-127">Pobiera klucz ciągu i pobiera buforowany element jako `byte[]` Jeśli znaleziono w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="77886-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="77886-128">**Zestaw, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="77886-128">**Set, SetAsync**</span></span>

<span data-ttu-id="77886-129">Dodaje element (jako `byte[]`) do pamięci podręcznej przy użyciu klucza ciągu.</span><span class="sxs-lookup"><span data-stu-id="77886-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="77886-130">**Odświeżanie, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="77886-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="77886-131">Odświeża element w pamięci podręcznej na podstawie jego klucza zresetowanie jego przesuwanego limit czasu wygaśnięcia (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="77886-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="77886-132">**Usuń funkcja removeasync do usuwania**</span><span class="sxs-lookup"><span data-stu-id="77886-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="77886-133">Usuwa wpis pamięci podręcznej na podstawie jego klucza.</span><span class="sxs-lookup"><span data-stu-id="77886-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="77886-134">Aby użyć `IDistributedCache` interfejsu:</span><span class="sxs-lookup"><span data-stu-id="77886-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="77886-135">Dodaj wymagane pakiety NuGet do pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="77886-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="77886-136">Skonfiguruj implementacja `IDistributedCache` w Twojej `Startup` klasy `ConfigureServices` metody i dodaj go do kontenera.</span><span class="sxs-lookup"><span data-stu-id="77886-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="77886-137">Z poziomu aplikacji [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) lub klasy kontrolera MVC, żądania wystąpienia `IDistributedCache` z konstruktora.</span><span class="sxs-lookup"><span data-stu-id="77886-137">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="77886-138">Wystąpienie, które będą udostępniane przez [iniekcji zależności](../../fundamentals/dependency-injection.md) (Podpisane).</span><span class="sxs-lookup"><span data-stu-id="77886-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="77886-139">Nie musi być pojedyncza lub polu Okres istnienia `IDistributedCache` wystąpień (co najmniej wbudowanych implementacji).</span><span class="sxs-lookup"><span data-stu-id="77886-139">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="77886-140">Można również utworzyć wystąpienie wszędzie tam, gdzie może być konieczne jedną (zamiast [iniekcji zależności](../../fundamentals/dependency-injection.md)), ale może być trudniejsze do testowania, kodu i narusza [jawne zależności zasady](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="77886-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="77886-141">Poniższy przykład przedstawia użycie wystąpienia `IDistributedCache` w składniku proste oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="77886-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="77886-142">W powyższym kodzie wartość w pamięci podręcznej jest do odczytu, ale nigdy nie są zapisywane.</span><span class="sxs-lookup"><span data-stu-id="77886-142">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="77886-143">W tym przykładzie wartość jest ustawiona tylko, gdy serwer jest uruchamiany i nie ulega zmianie.</span><span class="sxs-lookup"><span data-stu-id="77886-143">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="77886-144">W scenariuszu z wieloma serwerami najnowszych serwera w celu uruchomienia zostaną zastąpione poprzednie wartości, które zostały określone przez inne serwery.</span><span class="sxs-lookup"><span data-stu-id="77886-144">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="77886-145">`Get` i `Set` metody `byte[]` typu.</span><span class="sxs-lookup"><span data-stu-id="77886-145">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="77886-146">W związku z tym wartość ciągu musi zostać przekonwertowany za pomocą `Encoding.UTF8.GetString` (dla `Get`) i `Encoding.UTF8.GetBytes` (dla `Set`).</span><span class="sxs-lookup"><span data-stu-id="77886-146">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="77886-147">Poniższy kod z *Startup.cs* pokazuje ustawienie wartości:</span><span class="sxs-lookup"><span data-stu-id="77886-147">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="77886-148">Ponieważ `IDistributedCache` jest skonfigurowany w `ConfigureServices` metody, jest ona dostępna do `Configure` metody jako parametr.</span><span class="sxs-lookup"><span data-stu-id="77886-148">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="77886-149">Dodanie go jako parametru umożliwi skonfigurowanego wystąpienia być dostarczane za pośrednictwem Podpisane.</span><span class="sxs-lookup"><span data-stu-id="77886-149">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="77886-150">Przy użyciu pamięci podręcznej Redis rozproszonych</span><span class="sxs-lookup"><span data-stu-id="77886-150">Using a Redis distributed cache</span></span>

<span data-ttu-id="77886-151">[Redis](https://redis.io/) magazynu danych w pamięci typu open source, która jest często używana jako rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="77886-151">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="77886-152">Możesz użyć jej lokalnie i można skonfigurować [pamięć podręczna Redis Azure](https://azure.microsoft.com/services/cache/) dla aplikacji hostowanej Azure platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77886-152">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="77886-153">Konfiguruje aplikację ASP.NET Core za pomocą implementacji pamięci podręcznej `RedisDistributedCache` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="77886-153">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="77886-154">Konfigurowanie wdrożenia Redis w `ConfigureServices` i dostęp do niego w kodzie aplikacji przez zażądanie wystąpienia `IDistributedCache` (zobacz powyżej kodu).</span><span class="sxs-lookup"><span data-stu-id="77886-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="77886-155">W przykładowym kodzie `RedisCache` implementacji jest używany, gdy serwer jest skonfigurowany dla `Staging` środowiska.</span><span class="sxs-lookup"><span data-stu-id="77886-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="77886-156">W związku z tym `ConfigureStagingServices` konfiguruje metody `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="77886-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="77886-157">Aby zainstalować Redis na komputerze lokalnym, należy zainstalować pakiet chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) i uruchom `redis-server` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="77886-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="77886-158">Używanie programu SQL Server rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="77886-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="77886-159">Implementacja SqlServerCache umożliwia rozproszonej pamięci podręcznej do użycia jako magazynu zapasowego, jego bazy danych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="77886-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="77886-160">Aby utworzyć program SQL Server tabeli można użyć narzędzia pamięci podręcznej sql, narzędzie tworzy tabelę z nazwą i schematu, które określisz.</span><span class="sxs-lookup"><span data-stu-id="77886-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="77886-161">Dodaj `SqlConfig.Tools` do `<ItemGroup>` element pliku projektu i uruchomienia `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="77886-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="77886-162">Przetestuj SqlConfig.Tools, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="77886-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="77886-163">SqlConfig.Tools Wyświetla użycia i opcje Pomoc dla polecenia.</span><span class="sxs-lookup"><span data-stu-id="77886-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="77886-164">Utwórz tabelę w programie SQL Server, uruchamiając `sql-cache create` polecenia:</span><span class="sxs-lookup"><span data-stu-id="77886-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="77886-165">Utworzona tabela ma następującego schematu:</span><span class="sxs-lookup"><span data-stu-id="77886-165">The created table has the following schema:</span></span>

![Tabela SqlServer pamięci podręcznej](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="77886-167">Podobnie jak wszystkie implementacje pamięci podręcznej, aplikacja powinna pobieranie i ustawianie wartości pamięci podręcznej za pomocą wystąpienia `IDistributedCache`, a nie `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="77886-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="77886-168">Implementuje próbki `SqlServerCache` w środowisku produkcyjnym (tak jest skonfigurowany w `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="77886-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="77886-169">`ConnectionString` (I opcjonalnie `SchemaName` i `TableName`) zwykle powinny być przechowywane poza kontroli źródła (na przykład UserSecrets), ponieważ mogą one zawierać poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="77886-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="77886-170">Zalecenia</span><span class="sxs-lookup"><span data-stu-id="77886-170">Recommendations</span></span>

<span data-ttu-id="77886-171">Podczas podejmowania decyzji o życie `IDistributedCache` jest prawa dla aplikacji, wybierz między Redis i SQL Server na podstawie istniejącej infrastruktury i środowiska, wymagań dotyczących wydajności i środowisko zespołu.</span><span class="sxs-lookup"><span data-stu-id="77886-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="77886-172">Jeśli zespół jest wygodniejsze Praca z pamięci podręcznej Redis, jest doskonałym wyborem.</span><span class="sxs-lookup"><span data-stu-id="77886-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="77886-173">Jeśli zespół preferuje programu SQL Server, można mieć pewność, że wykonanie również.</span><span class="sxs-lookup"><span data-stu-id="77886-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="77886-174">Należy pamiętać, że tradycyjnych rozwiązań buforowania przechowuje dane w pamięci, która pozwala na szybkie odzyskiwanie danych.</span><span class="sxs-lookup"><span data-stu-id="77886-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="77886-175">Należy przechowywania często używanych danych w pamięci podręcznej i Zapisz dane całej w magazynu trwałego wewnętrznej bazy danych, takich jak SQL Server lub usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="77886-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="77886-176">Pamięć podręczna redis to rozwiązanie buforowania, co pozwala uzyskać wysoką przepustowość i małe opóźnienia w porównaniu do pamięci podręcznej SQL.</span><span class="sxs-lookup"><span data-stu-id="77886-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="77886-177">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="77886-177">Additional resources</span></span>

* [<span data-ttu-id="77886-178">Pamięć podręczna Azure redis</span><span class="sxs-lookup"><span data-stu-id="77886-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="77886-179">Baza danych SQL Azure</span><span class="sxs-lookup"><span data-stu-id="77886-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* [<span data-ttu-id="77886-180">Buforowanie w pamięci</span><span class="sxs-lookup"><span data-stu-id="77886-180">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="77886-181">Wykrywanie zmian z tokenami zmiany</span><span class="sxs-lookup"><span data-stu-id="77886-181">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="77886-182">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="77886-182">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="77886-183">Oprogramowanie pośredniczące buforowania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="77886-183">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="77886-184">Pomocnik tagu pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="77886-184">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="77886-185">Pomocnik tagu rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="77886-185">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
