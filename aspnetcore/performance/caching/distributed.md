---
title: Rozproszone buforowanie w ASP.NET Core
author: guardrex
description: Dowiedz się, jak za pomocą rozproszonej pamięci podręcznej ASP.NET Core poprawić wydajność i skalowalność aplikacji, szczególnie w środowisku chmury lub farmy serwerów.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
uid: performance/caching/distributed
ms.openlocfilehash: 3e039a26505aed1bcc0299880039760fef19fd67
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727243"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="f3462-103">Rozproszone buforowanie w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f3462-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="f3462-104">Autorzy [Luke Latham](https://github.com/guardrex), [Mohsin Nasir](https://github.com/mohsinnasir)i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f3462-104">By [Luke Latham](https://github.com/guardrex), [Mohsin Nasir](https://github.com/mohsinnasir), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f3462-105">Rozproszonej pamięci podręcznej to pamięć podręczna współdzielona przez wiele serwerów aplikacji, zazwyczaj obsługiwana jako usługa zewnętrzna do serwerów aplikacji, które mają do niego dostęp.</span><span class="sxs-lookup"><span data-stu-id="f3462-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="f3462-106">Rozproszona pamięć podręczna może zwiększyć wydajność i skalowalność aplikacji ASP.NET Core, szczególnie gdy aplikacja jest hostowana przez usługę w chmurze lub farmę serwerów.</span><span class="sxs-lookup"><span data-stu-id="f3462-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="f3462-107">Rozproszonej pamięci podręcznej ma kilka zalet w porównaniu z innymi scenariuszami buforowania, w których buforowane dane są przechowywane na poszczególnych serwerach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f3462-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="f3462-108">Dane przechowywane w pamięci podręcznej są dystrybuowane:</span><span class="sxs-lookup"><span data-stu-id="f3462-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="f3462-109">Jest *spójna* (spójna) między żądaniami do wielu serwerów.</span><span class="sxs-lookup"><span data-stu-id="f3462-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="f3462-110">Przeżyje ponowne uruchomienia serwera i wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f3462-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="f3462-111">Nie używa pamięci lokalnej.</span><span class="sxs-lookup"><span data-stu-id="f3462-111">Doesn't use local memory.</span></span>

<span data-ttu-id="f3462-112">Konfiguracja rozproszonej pamięci podręcznej jest specyficzna dla implementacji.</span><span class="sxs-lookup"><span data-stu-id="f3462-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="f3462-113">W tym artykule opisano sposób konfigurowania SQL Server i rozproszonej pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="f3462-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="f3462-114">Dostępne są również implementacje innych firm, takie jak [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache w witrynie GitHub](https://github.com/Alachisoft/NCache)).</span><span class="sxs-lookup"><span data-stu-id="f3462-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="f3462-115">Niezależnie od tego, która implementacja została wybrana, aplikacja współdziała z pamięcią podręczną przy użyciu interfejsu <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>.</span><span class="sxs-lookup"><span data-stu-id="f3462-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="f3462-116">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f3462-116">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f3462-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f3462-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f3462-118">Aby użyć rozproszonej pamięci podręcznej SQL Server, Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. cache. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="f3462-118">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="f3462-119">Aby użyć rozproszonej pamięci podręcznej Redis, Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. cache. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="f3462-119">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span>

<span data-ttu-id="f3462-120">Aby użyć rozproszonej pamięci podręcznej NCache, Dodaj odwołanie do pakietu do pakietu [NCache. Microsoft. Extensions. cache. opensource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="f3462-120">To use NCache distributed cache, add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="f3462-121">Aby użyć rozproszonej pamięci podręcznej SQL Server, należy odwołać się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub dodać odwołanie do pakietu do pakietu [Microsoft. Extensions. cache. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="f3462-121">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="f3462-122">Aby użyć rozproszonej pamięci podręcznej Redis, odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) , a następnie Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. cache. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="f3462-122">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="f3462-123">Pakiet Redis nie jest uwzględniony w pakiecie `Microsoft.AspNetCore.App`, więc należy odwołać się do pakietu Redis oddzielnie w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="f3462-123">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="f3462-124">Aby użyć rozproszonej pamięci podręcznej NCache, odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) , a następnie Dodaj odwołanie do pakietu do pakietu [NCache. Microsoft. Extensions. cache. opensource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="f3462-124">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="f3462-125">Pakiet NCache nie jest uwzględniony w pakiecie `Microsoft.AspNetCore.App`, więc należy odwołać się do pakietu NCache oddzielnie w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="f3462-125">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="f3462-126">Aby użyć rozproszonej pamięci podręcznej SQL Server, należy odwołać się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub dodać odwołanie do pakietu do pakietu [Microsoft. Extensions. cache. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="f3462-126">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="f3462-127">Aby użyć rozproszonej pamięci podręcznej Redis, odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) , a następnie Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. cache. Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) .</span><span class="sxs-lookup"><span data-stu-id="f3462-127">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="f3462-128">Pakiet Redis nie jest uwzględniony w pakiecie `Microsoft.AspNetCore.App`, więc należy odwołać się do pakietu Redis oddzielnie w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="f3462-128">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="f3462-129">Aby użyć rozproszonej pamięci podręcznej NCache, odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) , a następnie Dodaj odwołanie do pakietu do pakietu [NCache. Microsoft. Extensions. cache. opensource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="f3462-129">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="f3462-130">Pakiet NCache nie jest uwzględniony w pakiecie `Microsoft.AspNetCore.App`, więc należy odwołać się do pakietu NCache oddzielnie w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="f3462-130">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="f3462-131">IDistributedCache, interfejs</span><span class="sxs-lookup"><span data-stu-id="f3462-131">IDistributedCache interface</span></span>

<span data-ttu-id="f3462-132">Interfejs <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> udostępnia następujące metody manipulowania elementami w implementacji rozproszonej pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="f3462-132">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="f3462-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; akceptuje klucz ciągu i pobiera buforowany element jako tablicę `byte[]`, jeśli znajduje się w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f3462-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="f3462-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; dodaje element (jako tablicę `byte[]`) do pamięci podręcznej przy użyciu klucza ciągu.</span><span class="sxs-lookup"><span data-stu-id="f3462-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="f3462-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; odświeża element w pamięci podręcznej na podstawie jego klucza, resetowanie przekroczenia limitu czasu wygaśnięcia (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="f3462-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="f3462-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; usuwa element pamięci podręcznej na podstawie jego klucza ciągu.</span><span class="sxs-lookup"><span data-stu-id="f3462-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="f3462-137">Ustanów usługi rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="f3462-137">Establish distributed caching services</span></span>

<span data-ttu-id="f3462-138">Zarejestruj implementację <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f3462-138">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f3462-139">Implementacje wdrożone w ramach platformy opisane w tym temacie:</span><span class="sxs-lookup"><span data-stu-id="f3462-139">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="f3462-140">Pamięć podręczna pamięci rozproszonej</span><span class="sxs-lookup"><span data-stu-id="f3462-140">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="f3462-141">Pamięć podręczna rozproszonej SQL Server</span><span class="sxs-lookup"><span data-stu-id="f3462-141">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="f3462-142">Pamięć podręczna Redis rozproszonej</span><span class="sxs-lookup"><span data-stu-id="f3462-142">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="f3462-143">Pamięć podręczna NCache rozproszonej</span><span class="sxs-lookup"><span data-stu-id="f3462-143">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="f3462-144">Pamięć podręczna pamięci rozproszonej</span><span class="sxs-lookup"><span data-stu-id="f3462-144">Distributed Memory Cache</span></span>

<span data-ttu-id="f3462-145">Pamięć podręczna pamięci rozproszonej (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) to implementacja platformy <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, która przechowuje elementy w pamięci.</span><span class="sxs-lookup"><span data-stu-id="f3462-145">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="f3462-146">Pamięć podręczna pamięci rozproszonej nie jest rzeczywistą rozproszoną pamięcią podręczną.</span><span class="sxs-lookup"><span data-stu-id="f3462-146">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="f3462-147">Elementy buforowane są przechowywane przez wystąpienie aplikacji na serwerze, na którym działa aplikacja.</span><span class="sxs-lookup"><span data-stu-id="f3462-147">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="f3462-148">Pamięć podręczna pamięci rozproszonej jest przydatną implementacją:</span><span class="sxs-lookup"><span data-stu-id="f3462-148">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="f3462-149">W scenariuszach projektowania i testowania.</span><span class="sxs-lookup"><span data-stu-id="f3462-149">In development and testing scenarios.</span></span>
* <span data-ttu-id="f3462-150">Gdy jest używany pojedynczy serwer w środowisku produkcyjnym i użycie pamięci nie jest problemem.</span><span class="sxs-lookup"><span data-stu-id="f3462-150">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="f3462-151">Implementacja rozproszonej pamięci podręcznej pamięć podręczna magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="f3462-151">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="f3462-152">Pozwala to na wdrożenie prawdziwego rozwiązania do buforowania rozproszonego w przyszłości, jeśli będzie konieczne przeprowadzenie wielu węzłów lub odporności na uszkodzenia.</span><span class="sxs-lookup"><span data-stu-id="f3462-152">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="f3462-153">Przykładowa aplikacja korzysta z pamięci podręcznej pamięci rozproszonej, gdy aplikacja jest uruchamiana w środowisku deweloperskim w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f3462-153">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="f3462-154">Pamięć podręczna rozproszonej SQL Server</span><span class="sxs-lookup"><span data-stu-id="f3462-154">Distributed SQL Server Cache</span></span>

<span data-ttu-id="f3462-155">Implementacja rozproszonej pamięci podręcznej SQL Server (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) umożliwia rozproszonej pamięci podręcznej użycie bazy danych SQL Server jako magazynu zapasowego.</span><span class="sxs-lookup"><span data-stu-id="f3462-155">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="f3462-156">Aby utworzyć SQL Server tabelę elementów w pamięci podręcznej w wystąpieniu SQL Server, można użyć narzędzia `sql-cache`.</span><span class="sxs-lookup"><span data-stu-id="f3462-156">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="f3462-157">Narzędzie tworzy tabelę z określoną nazwą i schematem.</span><span class="sxs-lookup"><span data-stu-id="f3462-157">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="f3462-158">Utwórz tabelę w SQL Server, uruchamiając polecenie `sql-cache create`.</span><span class="sxs-lookup"><span data-stu-id="f3462-158">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="f3462-159">Podaj wystąpienie SQL Server (`Data Source`), bazę danych (`Initial Catalog`), schemat (na przykład `dbo`) i nazwę tabeli (na przykład `TestCache`):</span><span class="sxs-lookup"><span data-stu-id="f3462-159">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="f3462-160">Komunikat jest rejestrowany w celu wskazania, że narzędzie zakończyło się pomyślnie:</span><span class="sxs-lookup"><span data-stu-id="f3462-160">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="f3462-161">Tabela utworzona za pomocą narzędzia `sql-cache` ma następujący schemat:</span><span class="sxs-lookup"><span data-stu-id="f3462-161">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer Cache Table](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="f3462-163">Aplikacja powinna manipulować wartościami pamięci podręcznej przy użyciu wystąpienia <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, a nie <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span><span class="sxs-lookup"><span data-stu-id="f3462-163">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="f3462-164">Przykładowa aplikacja implementuje <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> w środowisku innym niż programowanie w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f3462-164">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="f3462-165"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (i opcjonalnie <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> i <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) są zwykle przechowywane poza kontrolą źródła (na przykład przechowywane przez [Menedżera wpisów tajnych](xref:security/app-secrets) lub w pliku *appsettings. JSON*/*appSettings. { ENVIRONMENT}. JSON* — pliki).</span><span class="sxs-lookup"><span data-stu-id="f3462-165">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="f3462-166">Parametry połączenia mogą zawierać poświadczenia, które powinny być przechowywane w systemach kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="f3462-166">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="f3462-167">Redis Cache dystrybuowane</span><span class="sxs-lookup"><span data-stu-id="f3462-167">Distributed Redis Cache</span></span>

<span data-ttu-id="f3462-168">[Redis](https://redis.io/) to magazyn danych typu "open source", który jest często używany jako rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f3462-168">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="f3462-169">Można używać Redis lokalnie i można skonfigurować [Azure Redis Cache](https://azure.microsoft.com/services/cache/) dla aplikacji ASP.NET Core hostowanej na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="f3462-169">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f3462-170">Aplikacja konfiguruje implementację pamięci podręcznej przy użyciu wystąpienia <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) w środowisku innym niż programowanie w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f3462-170">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="f3462-171">Aplikacja konfiguruje implementację pamięci podręcznej przy użyciu wystąpienia <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) w środowisku innym niż programowanie w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f3462-171">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="f3462-172">Aplikacja konfiguruje implementację pamięci podręcznej przy użyciu wystąpienia <xref:Microsoft.Extensions.Caching.Redis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span><span class="sxs-lookup"><span data-stu-id="f3462-172">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

<span data-ttu-id="f3462-173">Aby zainstalować Redis na komputerze lokalnym:</span><span class="sxs-lookup"><span data-stu-id="f3462-173">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="f3462-174">Zainstaluj [pakiet Redis czekolady](https://chocolatey.org/packages/redis-64/).</span><span class="sxs-lookup"><span data-stu-id="f3462-174">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="f3462-175">Uruchom `redis-server` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="f3462-175">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="f3462-176">Pamięć podręczna NCache rozproszonej</span><span class="sxs-lookup"><span data-stu-id="f3462-176">Distributed NCache Cache</span></span>

<span data-ttu-id="f3462-177">[NCache](https://github.com/Alachisoft/NCache) to rozproszona pamięć podręczna rozproszonej w pamięci, opracowana natywnie w .NET i .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3462-177">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="f3462-178">NCache działa lokalnie i skonfigurowano jako klaster rozproszonej pamięci podręcznej dla aplikacji ASP.NET Core działającej na platformie Azure lub na innych platformach hostingowych.</span><span class="sxs-lookup"><span data-stu-id="f3462-178">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="f3462-179">Aby zainstalować i skonfigurować NCache na komputerze lokalnym, zobacz [Przewodnik po NCache wprowadzenie dla systemu Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span><span class="sxs-lookup"><span data-stu-id="f3462-179">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="f3462-180">Aby skonfigurować NCache:</span><span class="sxs-lookup"><span data-stu-id="f3462-180">To configure NCache:</span></span>

1. <span data-ttu-id="f3462-181">Zainstaluj pakiet [NuGet NCache Open Source](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span><span class="sxs-lookup"><span data-stu-id="f3462-181">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="f3462-182">Skonfiguruj klaster pamięci podręcznej w programie [Client. ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span><span class="sxs-lookup"><span data-stu-id="f3462-182">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="f3462-183">Dodaj następujący kod do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f3462-183">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="f3462-184">Korzystanie z rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="f3462-184">Use the distributed cache</span></span>

<span data-ttu-id="f3462-185">Aby użyć interfejsu <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, zażądaj wystąpienia <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> z dowolnego konstruktora w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f3462-185">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="f3462-186">Wystąpienie jest dostarczane przez [wstrzyknięcie zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f3462-186">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f3462-187">Po rozpoczęciu przykładowej aplikacji <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> jest wstrzykiwana do `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="f3462-187">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="f3462-188">Bieżący czas jest buforowany przy użyciu <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (Aby uzyskać więcej informacji, zobacz [host ogólny: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span><span class="sxs-lookup"><span data-stu-id="f3462-188">The current time is cached using <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (for more information, see [Generic Host: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f3462-189">Po rozpoczęciu przykładowej aplikacji <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> jest wstrzykiwana do `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="f3462-189">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="f3462-190">Bieżący czas jest buforowany przy użyciu <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (Aby uzyskać więcej informacji, zobacz [host sieci Web: interfejs IApplicationLifetime](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span><span class="sxs-lookup"><span data-stu-id="f3462-190">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

<span data-ttu-id="f3462-191">Aplikacja Przykładowa wprowadza <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> do `IndexModel` do użycia na stronie indeksu.</span><span class="sxs-lookup"><span data-stu-id="f3462-191">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="f3462-192">Za każdym razem, gdy strona indeksu zostanie załadowana, pamięć podręczna jest sprawdzana pod kątem pamięci podręcznej w `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="f3462-192">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="f3462-193">Jeśli czas w pamięci podręcznej nie upłynął, zostanie wyświetlony czas.</span><span class="sxs-lookup"><span data-stu-id="f3462-193">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="f3462-194">Jeśli od czasu ostatniego dostępu do pamięci podręcznej upłynie 20 sekund (podczas ostatniego ładowania strony), na stronie zostanie wyświetlona *godzina wygaśnięcia pamięci podręcznej*.</span><span class="sxs-lookup"><span data-stu-id="f3462-194">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="f3462-195">Natychmiast Aktualizuj buforowany czas do bieżącego czasu, wybierając przycisk **Resetuj buforowany czas** .</span><span class="sxs-lookup"><span data-stu-id="f3462-195">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="f3462-196">Przycisk wyzwala metodę procedury obsługi `OnPostResetCachedTime`.</span><span class="sxs-lookup"><span data-stu-id="f3462-196">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="f3462-197">Nie ma potrzeby używania pojedynczego lub zakresu okresu istnienia dla wystąpień <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> (co najmniej dla wbudowanych implementacji).</span><span class="sxs-lookup"><span data-stu-id="f3462-197">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="f3462-198">Możesz również utworzyć wystąpienie <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> wszędzie tam, gdzie może być potrzebne, zamiast używać DI, ale utworzenie wystąpienia w kodzie może sprawić, że kod będzie trudniejszy do przetestowania i narusza [zasadę jawnych zależności](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span><span class="sxs-lookup"><span data-stu-id="f3462-198">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="f3462-199">Zalecenia</span><span class="sxs-lookup"><span data-stu-id="f3462-199">Recommendations</span></span>

<span data-ttu-id="f3462-200">Podczas decydowania, która implementacja <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> jest Najlepsza dla aplikacji, weź pod uwagę następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="f3462-200">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="f3462-201">Istniejąca infrastruktura</span><span class="sxs-lookup"><span data-stu-id="f3462-201">Existing infrastructure</span></span>
* <span data-ttu-id="f3462-202">Wymagania dotyczące wydajności</span><span class="sxs-lookup"><span data-stu-id="f3462-202">Performance requirements</span></span>
* <span data-ttu-id="f3462-203">Koszt</span><span class="sxs-lookup"><span data-stu-id="f3462-203">Cost</span></span>
* <span data-ttu-id="f3462-204">Środowisko pracy zespołu</span><span class="sxs-lookup"><span data-stu-id="f3462-204">Team experience</span></span>

<span data-ttu-id="f3462-205">Rozwiązania pamięci podręcznej zwykle polegają na magazynie w pamięci, aby zapewnić szybkie pobieranie danych z pamięci podręcznej, ale pamięć jest ograniczonym zasobem i kosztowna.</span><span class="sxs-lookup"><span data-stu-id="f3462-205">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="f3462-206">Przechowuj tylko często używane dane w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f3462-206">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="f3462-207">Ogólnie pamięć podręczna Redis zapewnia wyższą przepływność i mniejsze opóźnienia niż w przypadku pamięci podręcznej SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f3462-207">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="f3462-208">Jednak testy porównawcze są zwykle wymagane do określenia charakterystyki wydajności strategii buforowania.</span><span class="sxs-lookup"><span data-stu-id="f3462-208">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="f3462-209">Gdy SQL Server jest używany jako magazyn zapasowy rozproszonej pamięci podręcznej, korzystanie z tej samej bazy danych dla pamięci podręcznej i zwykłego magazynu danych aplikacji może mieć negatywny wpływ na wydajność obu tych elementów.</span><span class="sxs-lookup"><span data-stu-id="f3462-209">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="f3462-210">Zalecamy używanie dedykowanego wystąpienia SQL Server dla magazynu zapasowego rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="f3462-210">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f3462-211">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f3462-211">Additional resources</span></span>

* [<span data-ttu-id="f3462-212">Redis Cache na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="f3462-212">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="f3462-213">SQL Database na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="f3462-213">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="f3462-214">[ASP.NET Core dostawcę IDistributedCache dla NCache w farmach serwerów sieci Web](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache w serwisie GitHub](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="f3462-214">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
