---
title: Praca z rozproszoną pamięcią podręczną w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać platformy ASP.NET Core rozproszonej pamięci podręcznej w celu poprawy wydajności aplikacji i skalowalności, szczególnie w środowisku farmy, chmurze i na serwerze.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 85da734f3ae7bcf0936888edfb6ac91d4362eef2
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477479"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="ab6da-103">Praca z rozproszoną pamięcią podręczną w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab6da-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="ab6da-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ab6da-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ab6da-105">Rozproszonej pamięci podręcznej może zwiększyć wydajność i skalowalność aplikacji platformy ASP.NET Core, szczególnie w przypadku hostowanych w chmurze lub w farmie serwerów.</span><span class="sxs-lookup"><span data-stu-id="ab6da-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in the cloud or a server farm.</span></span>

<span data-ttu-id="ab6da-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ab6da-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="ab6da-107">Co to jest rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="ab6da-107">What is a distributed cache</span></span>

<span data-ttu-id="ab6da-108">Rozproszonej pamięci podręcznej jest współużytkowany przez wiele serwerów aplikacji (zobacz [podstawowe informacje o pamięci podręcznej](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="ab6da-108">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="ab6da-109">Informacje w pamięci podręcznej nie jest przechowywany w pamięci web poszczególnych serwerów, a dane w pamięci podręcznej jest dostępny dla wszystkich serwerów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ab6da-109">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="ab6da-110">To zapewnia kilka korzyści:</span><span class="sxs-lookup"><span data-stu-id="ab6da-110">This provides several advantages:</span></span>

1. <span data-ttu-id="ab6da-111">Dane w pamięci podręcznej jest spójny na wszystkich serwerach sieci web.</span><span class="sxs-lookup"><span data-stu-id="ab6da-111">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="ab6da-112">Użytkownicy nie widzą różne wyniki, w zależności od tego, w której sieci web serwer obsługuje żądania</span><span class="sxs-lookup"><span data-stu-id="ab6da-112">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="ab6da-113">Dane w pamięci podręcznej przeżyje ponowne uruchomienia serwera sieci web i wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="ab6da-113">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="ab6da-114">Serwery sieci web poszczególnych można usunięte lub dodane bez wywierania wpływu na pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="ab6da-114">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="ab6da-115">Źródłowy magazyn danych ma mniejszą liczbę żądań do niego (nie za pomocą wielu buforów w pamięci lub nie w pamięci podręcznej w ogóle).</span><span class="sxs-lookup"><span data-stu-id="ab6da-115">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="ab6da-116">Jeśli używasz programu SQL Server rozproszonej pamięci podręcznej, niektóre z tych korzyści są tylko wartość true, jeśli wystąpienie oddzielna baza danych jest używana dla pamięci podręcznej niż w przypadku aplikacji źródła danych.</span><span class="sxs-lookup"><span data-stu-id="ab6da-116">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="ab6da-117">Podobnie jak wszelkie pamięci rozproszonej pamięci podręcznej może znacznie zwiększyć czas reakcji aplikacji, ponieważ zazwyczaj można pobrać danych z pamięci podręcznej znacznie szybsze niż z relacyjnej bazy danych (lub usługi sieci web).</span><span class="sxs-lookup"><span data-stu-id="ab6da-117">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="ab6da-118">Konfiguracja pamięci podręcznej jest zależne od implementacji.</span><span class="sxs-lookup"><span data-stu-id="ab6da-118">Cache configuration is implementation specific.</span></span> <span data-ttu-id="ab6da-119">W tym artykule opisano, jak skonfigurować zarówno Redis i buforuje rozproszone programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ab6da-119">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="ab6da-120">Niezależnie od tego, która implementacja jest zaznaczone, aplikacja korzysta z pamięci podręcznej, korzystając ze wspólnego `IDistributedCache` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ab6da-120">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="ab6da-121">Interfejs IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="ab6da-121">The IDistributedCache Interface</span></span>

<span data-ttu-id="ab6da-122">`IDistributedCache` Interfejs zawiera synchroniczne i asynchroniczne metody.</span><span class="sxs-lookup"><span data-stu-id="ab6da-122">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="ab6da-123">Interfejs umożliwia elementy dodane, pobrane i usunięta z implementacja rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="ab6da-123">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="ab6da-124">`IDistributedCache` Interfejs zawiera następujące metody:</span><span class="sxs-lookup"><span data-stu-id="ab6da-124">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="ab6da-125">**Get-GetAsync**</span><span class="sxs-lookup"><span data-stu-id="ab6da-125">**Get, GetAsync**</span></span>

<span data-ttu-id="ab6da-126">Pobiera klucz ciągu i pobiera element pamięci podręcznej jako `byte[]` Jeśli znaleziono w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="ab6da-126">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="ab6da-127">**Zestaw, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="ab6da-127">**Set, SetAsync**</span></span>

<span data-ttu-id="ab6da-128">Dodaje element (jako `byte[]`) do pamięci podręcznej przy użyciu klucza typu ciąg.</span><span class="sxs-lookup"><span data-stu-id="ab6da-128">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="ab6da-129">**Odświeżanie, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="ab6da-129">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="ab6da-130">Odświeża element w pamięci podręcznej na podstawie jego klucza zresetowanie jej przewijania limit czasu wygaśnięcia (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="ab6da-130">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="ab6da-131">**Usuń RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="ab6da-131">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="ab6da-132">Usuwa wpis pamięci podręcznej na podstawie jego klucza.</span><span class="sxs-lookup"><span data-stu-id="ab6da-132">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="ab6da-133">Aby użyć `IDistributedCache` interfejsu:</span><span class="sxs-lookup"><span data-stu-id="ab6da-133">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="ab6da-134">Dodaj wymagane pakiety NuGet do pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="ab6da-134">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="ab6da-135">Konfigurowanie określonej implementacji `IDistributedCache` w Twojej `Startup` klasy `ConfigureServices` metody i dodaj go do kontenera, istnieje.</span><span class="sxs-lookup"><span data-stu-id="ab6da-135">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="ab6da-136">Z poziomu aplikacji [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) projektu i zlecania klasy kontrolera MVC, wystąpienie `IDistributedCache` z konstruktora.</span><span class="sxs-lookup"><span data-stu-id="ab6da-136">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="ab6da-137">Wystąpienie będzie świadczona przez [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="ab6da-137">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="ab6da-138">Nie ma potrzeby używania pojedynczego wystąpienia lub polu Okres istnienia `IDistributedCache` wystąpienia (co najmniej wbudowanych implementacji).</span><span class="sxs-lookup"><span data-stu-id="ab6da-138">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="ab6da-139">Można również utworzyć wystąpienie wszędzie tam, gdzie możesz potrzebować jeden (zamiast [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md)), ale to może utrudnić kodu testu i narusza [jawne zależności zasady](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="ab6da-139">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="ab6da-140">Poniższy przykład pokazuje, jak używać wystąpienia `IDistributedCache` w składniku proste oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="ab6da-140">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="ab6da-141">W powyższym kodzie wartość w pamięci podręcznej jest odczytu, ale nigdy nie są zapisywane.</span><span class="sxs-lookup"><span data-stu-id="ab6da-141">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="ab6da-142">W tym przykładzie ma tylko wartość serwer jest uruchamiany, gdy nie ulega zmianie.</span><span class="sxs-lookup"><span data-stu-id="ab6da-142">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="ab6da-143">W przypadku wielu serwerów najnowszych serwera w celu uruchomienia spowoduje zastąpienie poprzedniej wartości, które zostały określone przez inne serwery.</span><span class="sxs-lookup"><span data-stu-id="ab6da-143">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="ab6da-144">`Get` i `Set` metody za pomocą `byte[]` typu.</span><span class="sxs-lookup"><span data-stu-id="ab6da-144">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="ab6da-145">W związku z tym, wartość ciągu musi być konwertowane przy użyciu `Encoding.UTF8.GetString` (dla `Get`) i `Encoding.UTF8.GetBytes` (dla `Set`).</span><span class="sxs-lookup"><span data-stu-id="ab6da-145">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="ab6da-146">Poniższy kod z *Startup.cs* pokazuje wartość:</span><span class="sxs-lookup"><span data-stu-id="ab6da-146">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

<span data-ttu-id="ab6da-147">Ponieważ `IDistributedCache` jest skonfigurowana w `ConfigureServices` metody, jest ona dostępna do `Configure` metoda jako parametr.</span><span class="sxs-lookup"><span data-stu-id="ab6da-147">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="ab6da-148">Umożliwi skonfigurowanego wystąpienia mają być dostarczane za pośrednictwem DI dodawania go jako parametr.</span><span class="sxs-lookup"><span data-stu-id="ab6da-148">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="ab6da-149">Za pomocą rozproszonej pamięci podręcznej Redis cache</span><span class="sxs-lookup"><span data-stu-id="ab6da-149">Using a Redis distributed cache</span></span>

<span data-ttu-id="ab6da-150">[Redis](https://redis.io/) to magazyn danych w pamięci "open source", która jest często używana jako rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="ab6da-150">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="ab6da-151">Można używać go lokalnie, a także można skonfigurować [usługi Azure Redis Cache](https://azure.microsoft.com/services/cache/) dla aplikacji hostowanych na platformie Azure platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ab6da-151">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="ab6da-152">Konfiguruje aplikacji platformy ASP.NET Core, przy użyciu implementacji pamięci podręcznej `RedisDistributedCache` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ab6da-152">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="ab6da-153">Pamięć podręczna Redis wymaga [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span><span class="sxs-lookup"><span data-stu-id="ab6da-153">The Redis cache requires [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span></span>

<span data-ttu-id="ab6da-154">Konfigurowanie implementacja pamięci podręcznej Redis w `ConfigureServices` i uzyskać do niego dostęp w kodzie aplikacji, wysyłając żądanie wystąpienie `IDistributedCache` (zobacz powyższy kod).</span><span class="sxs-lookup"><span data-stu-id="ab6da-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="ab6da-155">W przykładowym kodzie `RedisCache` implementacja jest używana, gdy serwer jest skonfigurowany dla `Staging` środowiska.</span><span class="sxs-lookup"><span data-stu-id="ab6da-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="ab6da-156">Ten sposób `ConfigureStagingServices` konfiguruje metoda `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="ab6da-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

<span data-ttu-id="ab6da-157">Aby zainstalować usługi Redis na maszynie lokalnej, należy zainstalować pakiet chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) i uruchom `redis-server` z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="ab6da-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="ab6da-158">Używanie programu SQL Server rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="ab6da-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="ab6da-159">Implementacja SqlServerCache umożliwia rozproszonej pamięci podręcznej do użycia bazę danych programu SQL Server jako jego magazyn zapasowy.</span><span class="sxs-lookup"><span data-stu-id="ab6da-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="ab6da-160">Do utworzenia programu SQL Server tabeli można użyć narzędzia pamięci podręcznej sql, to narzędzie tworzy tabelę o nazwie i schematu, które określisz.</span><span class="sxs-lookup"><span data-stu-id="ab6da-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="ab6da-161">Dodaj `SqlConfig.Tools` do `<ItemGroup>` elementu w pliku projektu i uruchom `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="ab6da-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="ab6da-162">Przetestuj SqlConfig.Tools, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ab6da-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="ab6da-163">SqlConfig.Tools przedstawia sposób użycia i opcje pomocy dla poleceń.</span><span class="sxs-lookup"><span data-stu-id="ab6da-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="ab6da-164">Utwórz tabelę w programie SQL Server, uruchamiając `sql-cache create` polecenia:</span><span class="sxs-lookup"><span data-stu-id="ab6da-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="ab6da-165">Utworzonej tabeli ma zgodny z następującym schematem:</span><span class="sxs-lookup"><span data-stu-id="ab6da-165">The created table has the following schema:</span></span>

![Tabeli pamięci podręcznej SqlServer](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="ab6da-167">Podobnie jak wszystkie implementacjach pamięci podręcznej, aplikacji powinien pobieranie i ustawianie wartości pamięci podręcznej przy użyciu wystąpienia `IDistributedCache`, a nie `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="ab6da-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="ab6da-168">Implementuje próbki `SqlServerCache` w środowisku produkcyjnym (dzięki czemu jest skonfigurowana w `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="ab6da-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="ab6da-169">`ConnectionString` (I ewentualnie `SchemaName` i `TableName`) zwykle powinny być przechowywane poza kontrolą źródła (takich jak UserSecrets), ponieważ mogą one zawierać poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="ab6da-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="ab6da-170">Zalecenia</span><span class="sxs-lookup"><span data-stu-id="ab6da-170">Recommendations</span></span>

<span data-ttu-id="ab6da-171">Podczas podejmowania decyzji o życie `IDistributedCache` jest odpowiednią dla aplikacji, wybierz między Redis i programu SQL Server na podstawie istniejącej infrastruktury i środowiska, wymagań dotyczących wydajności i doświadczeń zespołu.</span><span class="sxs-lookup"><span data-stu-id="ab6da-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="ab6da-172">Jeśli Twój zespół jest idealnie pracy z pamięcią podręczną Redis, jest doskonałym wyborem.</span><span class="sxs-lookup"><span data-stu-id="ab6da-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="ab6da-173">Jeśli preferuje zespół programu SQL Server, można mieć pewność, że także tę implementację.</span><span class="sxs-lookup"><span data-stu-id="ab6da-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="ab6da-174">Należy pamiętać, że tradycyjne rozwiązanie pamięci podręcznej przechowuje dane w pamięci, która umożliwia do szybkiego pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="ab6da-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="ab6da-175">Należy przechowywać często używane dane w pamięci podręcznej i przechowywanie danych całej w magazynie trwałym wewnętrznej bazy danych, takich jak SQL Server lub usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="ab6da-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="ab6da-176">Pamięć podręczna redis Cache jest rozwiązanie pamięci podręcznej, co daje wysokiej przepływności i małego opóźnienia w porównaniu do pamięci podręcznej SQL.</span><span class="sxs-lookup"><span data-stu-id="ab6da-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ab6da-177">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ab6da-177">Additional resources</span></span>

* [<span data-ttu-id="ab6da-178">Usługa redis Cache na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="ab6da-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="ab6da-179">Bazy danych SQL na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="ab6da-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
