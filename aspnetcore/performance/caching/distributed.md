---
title: "Praca z rozproszonej pamięci podręcznej w ASP.NET Core"
author: ardalis
description: "Dowiedz się, jak za pomocą systemy buforowania rozproszonego zwiększyć wydajność i skalowalność aplikacji platformy ASP.NET Core, szczególnie w przypadku hostowanych w środowisku farmy chmury lub serwera."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/distributed
ms.openlocfilehash: a0af4887143f6ed37a1af982ec21a2ad5eae9515
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="f2903-103">Praca z rozproszonej pamięci podręcznej w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f2903-103">Working with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="f2903-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f2903-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f2903-105">Rozproszonej pamięci podręcznej może zwiększyć wydajność i skalowalność aplikacji platformy ASP.NET Core, szczególnie w przypadku hostowanych w środowisku farmy chmury lub serwera.</span><span class="sxs-lookup"><span data-stu-id="f2903-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="f2903-106">W tym artykule opisano sposób pracy z implementacji i abstrakcje wbudowanych rozproszonej pamięci podręcznej platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2903-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

<span data-ttu-id="f2903-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f2903-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="f2903-108">Co to jest rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="f2903-108">What is a distributed cache</span></span>

<span data-ttu-id="f2903-109">Rozproszonej pamięci podręcznej jest współużytkowany przez wiele serwerów aplikacji (zobacz [buforowanie podstawy](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="f2903-109">A distributed cache is shared by multiple app servers (see [Caching Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="f2903-110">Informacje w pamięci podręcznej nie jest przechowywany w pamięci poszczególnych sieci web, serwerów i buforowane dane są dostępne dla wszystkich serwerów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f2903-110">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="f2903-111">To zapewnia kilka korzyści:</span><span class="sxs-lookup"><span data-stu-id="f2903-111">This provides several advantages:</span></span>

1. <span data-ttu-id="f2903-112">Buforowane dane są spójne na wszystkich serwerach sieci web.</span><span class="sxs-lookup"><span data-stu-id="f2903-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="f2903-113">Użytkownicy nie będą widzieli różne wyniki, w zależności od tego, które sieci web serwer obsługuje żądania</span><span class="sxs-lookup"><span data-stu-id="f2903-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="f2903-114">Buforowane dane przeżyje ponownego uruchomienia serwera sieci web i wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="f2903-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="f2903-115">Serwery sieci web poszczególnych można usunięte lub dodane bez wpływu na pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f2903-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="f2903-116">Źródłowy magazyn danych ma mniejszą liczbę żądań wysyłanych do niego (nie z wielu buforów w pamięci lub nie pamięci podręcznej w ogóle).</span><span class="sxs-lookup"><span data-stu-id="f2903-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="f2903-117">Jeśli używasz programu SQL Server rozproszonej pamięci podręcznej, niektóre z tych zalet są tylko wartość true, jeśli wystąpienie osobna baza danych jest używana dla pamięci podręcznej niż aplikacji źródła danych.</span><span class="sxs-lookup"><span data-stu-id="f2903-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="f2903-118">Podobnie jak wszelkie pamięci podręcznej rozproszonej pamięci podręcznej mogą znacznie zwiększyć czas odpowiedzi aplikacji, ponieważ zwykle można pobrać danych z pamięci podręcznej znacznie szybciej niż z relacyjnej bazy danych (lub usługa sieci web).</span><span class="sxs-lookup"><span data-stu-id="f2903-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="f2903-119">Konfigurację pamięci podręcznej zależy od implementacji.</span><span class="sxs-lookup"><span data-stu-id="f2903-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="f2903-120">W tym artykule opisano, jak skonfigurować zarówno Redis i buforuje rozproszonych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f2903-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="f2903-121">Niezależnie od tego, którego implementacja jest zaznaczone, aplikacja współdziała z pamięci podręcznej, za pomocą wspólnego `IDistributedCache` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="f2903-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="f2903-122">Interfejs IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="f2903-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="f2903-123">`IDistributedCache` Interfejs zawiera metody synchroniczne i asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="f2903-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="f2903-124">Interfejs umożliwia elementów do dodania, pobrać i usunięcia z implementacja rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f2903-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="f2903-125">`IDistributedCache` Interfejs zawiera następujące metody:</span><span class="sxs-lookup"><span data-stu-id="f2903-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="f2903-126">**Get, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="f2903-126">**Get, GetAsync**</span></span>

<span data-ttu-id="f2903-127">Pobiera klucz ciągu i pobiera buforowany element jako `byte[]` Jeśli znaleziono w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f2903-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="f2903-128">**Zestaw, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="f2903-128">**Set, SetAsync**</span></span>

<span data-ttu-id="f2903-129">Dodaje element (jako `byte[]`) do pamięci podręcznej przy użyciu klucza ciągu.</span><span class="sxs-lookup"><span data-stu-id="f2903-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="f2903-130">**Odświeżanie, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="f2903-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="f2903-131">Odświeża element w pamięci podręcznej na podstawie jego klucza zresetowanie jego przesuwanego limit czasu wygaśnięcia (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="f2903-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="f2903-132">**Usuń funkcja removeasync do usuwania**</span><span class="sxs-lookup"><span data-stu-id="f2903-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="f2903-133">Usuwa wpis pamięci podręcznej na podstawie jego klucza.</span><span class="sxs-lookup"><span data-stu-id="f2903-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="f2903-134">Aby użyć `IDistributedCache` interfejsu:</span><span class="sxs-lookup"><span data-stu-id="f2903-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="f2903-135">Dodaj wymagane pakiety NuGet do pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="f2903-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="f2903-136">Skonfiguruj implementacja `IDistributedCache` w Twojej `Startup` klasy `ConfigureServices` metody i dodaj go do kontenera.</span><span class="sxs-lookup"><span data-stu-id="f2903-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="f2903-137">Z poziomu aplikacji [oprogramowanie pośredniczące](../../fundamentals/middleware.md) lub klasy kontrolera MVC, żądania wystąpienia `IDistributedCache` z konstruktora.</span><span class="sxs-lookup"><span data-stu-id="f2903-137">From the app's [Middleware](../../fundamentals/middleware.md) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="f2903-138">Wystąpienie, które będą udostępniane przez [iniekcji zależności](../../fundamentals/dependency-injection.md) (Podpisane).</span><span class="sxs-lookup"><span data-stu-id="f2903-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="f2903-139">Nie musi być pojedyncza lub polu Okres istnienia `IDistributedCache` wystąpień (co najmniej wbudowanych implementacji).</span><span class="sxs-lookup"><span data-stu-id="f2903-139">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="f2903-140">Można również utworzyć wystąpienie wszędzie tam, gdzie może być konieczne jedną (zamiast [iniekcji zależności](../../fundamentals/dependency-injection.md)), ale może być trudniejsze do testowania, kodu i narusza [jawne zależności zasady](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="f2903-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="f2903-141">Poniższy przykład przedstawia użycie wystąpienia `IDistributedCache` w składniku proste oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="f2903-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]

<span data-ttu-id="f2903-142">W powyższym kodzie wartość w pamięci podręcznej jest do odczytu, ale nigdy nie są zapisywane.</span><span class="sxs-lookup"><span data-stu-id="f2903-142">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="f2903-143">W tym przykładzie wartość jest ustawiona tylko, gdy serwer jest uruchamiany i nie ulega zmianie.</span><span class="sxs-lookup"><span data-stu-id="f2903-143">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="f2903-144">W scenariuszu z wieloma serwerami najnowszych serwera w celu uruchomienia zostaną zastąpione poprzednie wartości, które zostały określone przez inne serwery.</span><span class="sxs-lookup"><span data-stu-id="f2903-144">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="f2903-145">`Get` i `Set` metody `byte[]` typu.</span><span class="sxs-lookup"><span data-stu-id="f2903-145">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="f2903-146">W związku z tym wartość ciągu musi zostać przekonwertowany za pomocą `Encoding.UTF8.GetString` (dla `Get`) i `Encoding.UTF8.GetBytes` (dla `Set`).</span><span class="sxs-lookup"><span data-stu-id="f2903-146">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="f2903-147">Poniższy kod z *Startup.cs* pokazuje ustawienie wartości:</span><span class="sxs-lookup"><span data-stu-id="f2903-147">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]

> [!NOTE]
> <span data-ttu-id="f2903-148">Ponieważ `IDistributedCache` jest skonfigurowany w `ConfigureServices` metody, jest ona dostępna do `Configure` metody jako parametr.</span><span class="sxs-lookup"><span data-stu-id="f2903-148">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="f2903-149">Dodanie go jako parametru umożliwi skonfigurowanego wystąpienia być dostarczane za pośrednictwem Podpisane.</span><span class="sxs-lookup"><span data-stu-id="f2903-149">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="f2903-150">Przy użyciu pamięci podręcznej Redis rozproszonych</span><span class="sxs-lookup"><span data-stu-id="f2903-150">Using a Redis distributed cache</span></span>

<span data-ttu-id="f2903-151">[Redis](https://redis.io/) magazynu danych w pamięci typu open source, która jest często używana jako rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f2903-151">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="f2903-152">Możesz użyć jej lokalnie i można skonfigurować [pamięć podręczna Redis Azure](https://azure.microsoft.com/services/cache/) dla aplikacji hostowanej Azure platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2903-152">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="f2903-153">Konfiguruje aplikację ASP.NET Core za pomocą implementacji pamięci podręcznej `RedisDistributedCache` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="f2903-153">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="f2903-154">Konfigurowanie wdrożenia Redis w `ConfigureServices` i dostęp do niego w kodzie aplikacji przez zażądanie wystąpienia `IDistributedCache` (zobacz powyżej kodu).</span><span class="sxs-lookup"><span data-stu-id="f2903-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="f2903-155">W przykładowym kodzie `RedisCache` implementacji jest używany, gdy serwer jest skonfigurowany dla `Staging` środowiska.</span><span class="sxs-lookup"><span data-stu-id="f2903-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="f2903-156">W związku z tym `ConfigureStagingServices` konfiguruje metody `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="f2903-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]

> [!NOTE]
> <span data-ttu-id="f2903-157">Aby zainstalować Redis na komputerze lokalnym, należy zainstalować pakiet chocolatey [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) i uruchom `redis-server` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="f2903-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="f2903-158">Używanie programu SQL Server rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="f2903-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="f2903-159">Implementacja SqlServerCache umożliwia rozproszonej pamięci podręcznej do użycia jako magazynu zapasowego, jego bazy danych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f2903-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="f2903-160">Aby utworzyć program SQL Server tabeli można użyć narzędzia pamięci podręcznej sql, narzędzie tworzy tabelę z nazwą i schematu, które określisz.</span><span class="sxs-lookup"><span data-stu-id="f2903-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

<span data-ttu-id="f2903-161">Aby korzystać z narzędzia pamięci podręcznej sql, należy dodać `SqlConfig.Tools` do `<ItemGroup>` elementu *.csproj* pliku i uruchom dotnet przywracania.</span><span class="sxs-lookup"><span data-stu-id="f2903-161">To use the sql-cache tool, add `SqlConfig.Tools` to the `<ItemGroup>` element of the *.csproj* file and run dotnet restore.</span></span>

[!code-xml[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]

<span data-ttu-id="f2903-162">Przetestuj SqlConfig.Tools, uruchamiając następujące polecenie</span><span class="sxs-lookup"><span data-stu-id="f2903-162">Test SqlConfig.Tools by running the following command</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

<span data-ttu-id="f2903-163">Narzędzie pamięci podręcznej SQL do wyświetlenia pomocy użycia, opcje i poleceń, teraz możesz utworzyć tabel do programu sql server, uruchamiając polecenie "Utwórz pamięci podręcznej sql":</span><span class="sxs-lookup"><span data-stu-id="f2903-163">sql-cache tool  will display usage, options and command help, now you can create tables into sql server, running "sql-cache create" command :</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

<span data-ttu-id="f2903-164">Utworzona tabela ma następującego schematu:</span><span class="sxs-lookup"><span data-stu-id="f2903-164">The created table has the following schema:</span></span>

![Tabela SqlServer pamięci podręcznej](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="f2903-166">Podobnie jak wszystkie implementacje pamięci podręcznej, aplikacja powinna pobieranie i ustawianie wartości pamięci podręcznej za pomocą wystąpienia `IDistributedCache`, a nie `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="f2903-166">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="f2903-167">Implementuje próbki `SqlServerCache` w `Production` środowiska (tak jest skonfigurowany w `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="f2903-167">The sample implements `SqlServerCache` in the `Production` environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]

> [!NOTE]
> <span data-ttu-id="f2903-168">`ConnectionString` (I opcjonalnie `SchemaName` i `TableName`) zwykle powinny być przechowywane poza kontroli źródła (na przykład UserSecrets), ponieważ mogą one zawierać poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="f2903-168">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="f2903-169">Zalecenia</span><span class="sxs-lookup"><span data-stu-id="f2903-169">Recommendations</span></span>

<span data-ttu-id="f2903-170">Podczas podejmowania decyzji o życie `IDistributedCache` jest prawa dla aplikacji, wybierz między Redis i SQL Server na podstawie istniejącej infrastruktury i środowiska, wymagań dotyczących wydajności i środowisko zespołu.</span><span class="sxs-lookup"><span data-stu-id="f2903-170">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="f2903-171">Jeśli zespół jest wygodniejsze Praca z pamięci podręcznej Redis, jest doskonałym wyborem.</span><span class="sxs-lookup"><span data-stu-id="f2903-171">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="f2903-172">Jeśli zespół preferuje programu SQL Server, można mieć pewność, że wykonanie również.</span><span class="sxs-lookup"><span data-stu-id="f2903-172">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="f2903-173">Należy pamiętać, że tradycyjnych rozwiązań buforowania przechowuje dane w pamięci, która pozwala na szybkie odzyskiwanie danych.</span><span class="sxs-lookup"><span data-stu-id="f2903-173">Note that A traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="f2903-174">Należy przechowywania często używanych danych w pamięci podręcznej i Zapisz dane całej w magazynu trwałego wewnętrznej bazy danych, takich jak SQL Server lub usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f2903-174">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="f2903-175">Pamięć podręczna redis to rozwiązanie buforowania, co pozwala uzyskać wysoką przepustowość i małe opóźnienia w porównaniu do pamięci podręcznej SQL.</span><span class="sxs-lookup"><span data-stu-id="f2903-175">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f2903-176">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f2903-176">Additional resources</span></span>

* [<span data-ttu-id="f2903-177">Pamięć podręczna Azure redis</span><span class="sxs-lookup"><span data-stu-id="f2903-177">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="f2903-178">Baza danych SQL Azure</span><span class="sxs-lookup"><span data-stu-id="f2903-178">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* [<span data-ttu-id="f2903-179">Buforowanie w pamięci</span><span class="sxs-lookup"><span data-stu-id="f2903-179">In-memory caching</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="f2903-180">Wykrywanie zmian z tokenami zmiany</span><span class="sxs-lookup"><span data-stu-id="f2903-180">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="f2903-181">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="f2903-181">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="f2903-182">Oprogramowanie pośredniczące buforowania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="f2903-182">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="f2903-183">Pomocnik tagu pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="f2903-183">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="f2903-184">Pomocnik tagu rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="f2903-184">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
