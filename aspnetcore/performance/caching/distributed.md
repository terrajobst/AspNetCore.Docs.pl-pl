---
title: Rozproszone buforowanie w ASP.NET Core
author: guardrex
description: Dowiedz się, jak za pomocą rozproszonej pamięci podręcznej ASP.NET Core poprawić wydajność i skalowalność aplikacji, szczególnie w środowisku chmury lub farmy serwerów.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: performance/caching/distributed
ms.openlocfilehash: 8417463038bcdc0f77852bec3c3bb8a618153009
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773853"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="82da6-103">Rozproszone buforowanie w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="82da6-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="82da6-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="82da6-104">By [Luke Latham](https://github.com/guardrex) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="82da6-105">Rozproszonej pamięci podręcznej to pamięć podręczna współdzielona przez wiele serwerów aplikacji, zazwyczaj obsługiwana jako usługa zewnętrzna do serwerów aplikacji, które mają do niego dostęp.</span><span class="sxs-lookup"><span data-stu-id="82da6-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="82da6-106">Rozproszona pamięć podręczna może zwiększyć wydajność i skalowalność aplikacji ASP.NET Core, szczególnie gdy aplikacja jest hostowana przez usługę w chmurze lub farmę serwerów.</span><span class="sxs-lookup"><span data-stu-id="82da6-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="82da6-107">Rozproszonej pamięci podręcznej ma kilka zalet w porównaniu z innymi scenariuszami buforowania, w których buforowane dane są przechowywane na poszczególnych serwerach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82da6-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="82da6-108">Dane przechowywane w pamięci podręcznej są dystrybuowane:</span><span class="sxs-lookup"><span data-stu-id="82da6-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="82da6-109">Jest *spójna* (spójna) między żądaniami do wielu serwerów.</span><span class="sxs-lookup"><span data-stu-id="82da6-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="82da6-110">Przeżyje ponowne uruchomienia serwera i wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82da6-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="82da6-111">Nie używa pamięci lokalnej.</span><span class="sxs-lookup"><span data-stu-id="82da6-111">Doesn't use local memory.</span></span>

<span data-ttu-id="82da6-112">Konfiguracja rozproszonej pamięci podręcznej jest specyficzna dla implementacji.</span><span class="sxs-lookup"><span data-stu-id="82da6-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="82da6-113">W tym artykule opisano sposób konfigurowania SQL Server i rozproszonej pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="82da6-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="82da6-114">Dostępne są również implementacje innych firm, takie jak [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache w witrynie GitHub](https://github.com/Alachisoft/NCache)).</span><span class="sxs-lookup"><span data-stu-id="82da6-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="82da6-115">Niezależnie od tego, która implementacja została wybrana, aplikacja współdziała z pamięcią podręczną przy <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> użyciu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="82da6-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="82da6-116">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="82da6-116">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82da6-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="82da6-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="82da6-118">Aby użyć rozproszonej pamięci podręcznej SQL Server, Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. cache. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="82da6-118">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="82da6-119">Aby użyć rozproszonej pamięci podręcznej Redis, Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. cache. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="82da6-119">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="82da6-120">Aby użyć rozproszonej pamięci podręcznej SQL Server, należy odwołać się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub dodać odwołanie do pakietu do pakietu [Microsoft. Extensions. cache. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="82da6-120">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="82da6-121">Aby użyć rozproszonej pamięci podręcznej Redis, odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) , a następnie Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. cache. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="82da6-121">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="82da6-122">Pakiet Redis nie znajduje się w `Microsoft.AspNetCore.App` pakiecie, dlatego należy odwołać się do pakietu Redis oddzielnie w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="82da6-122">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="82da6-123">Aby użyć rozproszonej pamięci podręcznej SQL Server, należy odwołać się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub dodać odwołanie do pakietu do pakietu [Microsoft. Extensions. cache. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="82da6-123">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="82da6-124">Aby użyć rozproszonej pamięci podręcznej Redis, odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) , a następnie Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. cache. Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) .</span><span class="sxs-lookup"><span data-stu-id="82da6-124">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="82da6-125">Pakiet Redis nie znajduje się w `Microsoft.AspNetCore.App` pakiecie, dlatego należy odwołać się do pakietu Redis oddzielnie w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="82da6-125">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="82da6-126">IDistributedCache, interfejs</span><span class="sxs-lookup"><span data-stu-id="82da6-126">IDistributedCache interface</span></span>

<span data-ttu-id="82da6-127"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> Interfejs udostępnia następujące metody do manipulowania elementami w implementacji rozproszonej pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="82da6-127">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="82da6-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>`byte[]` , <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> Akceptujekluczciąguipobierabuforowanyelementjakotablicę,jeśliznajdujesięwpamięci&ndash; podręcznej.</span><span class="sxs-lookup"><span data-stu-id="82da6-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="82da6-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>`byte[]` , <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> Dodaje&ndash; element (jako tablicowy) do pamięci podręcznej przy użyciu klucza ciągu.</span><span class="sxs-lookup"><span data-stu-id="82da6-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="82da6-130"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> ,&ndash; Odświeża element w pamięci podręcznej na podstawie jego klucza, resetowanie przekroczenia limitu czasu wygaśnięcia (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="82da6-130"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="82da6-131"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> ,&ndash; Usuwa element pamięci podręcznej na podstawie jego klucza ciągu.</span><span class="sxs-lookup"><span data-stu-id="82da6-131"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="82da6-132">Ustanów usługi rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="82da6-132">Establish distributed caching services</span></span>

<span data-ttu-id="82da6-133">Zarejestruj implementację <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> programu w `Startup.ConfigureServices`programie.</span><span class="sxs-lookup"><span data-stu-id="82da6-133">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="82da6-134">Implementacje wdrożone w ramach platformy opisane w tym temacie:</span><span class="sxs-lookup"><span data-stu-id="82da6-134">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="82da6-135">Pamięć podręczna pamięci rozproszonej</span><span class="sxs-lookup"><span data-stu-id="82da6-135">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="82da6-136">Pamięć podręczna rozproszonej SQL Server</span><span class="sxs-lookup"><span data-stu-id="82da6-136">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="82da6-137">Pamięć podręczna Redis rozproszonej</span><span class="sxs-lookup"><span data-stu-id="82da6-137">Distributed Redis cache</span></span>](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="82da6-138">Pamięć podręczna pamięci rozproszonej</span><span class="sxs-lookup"><span data-stu-id="82da6-138">Distributed Memory Cache</span></span>

<span data-ttu-id="82da6-139">Pamięć podręczna pamięci podręcznej (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) to <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> implementacja udostępniona przez platformę, która przechowuje elementy w pamięci.</span><span class="sxs-lookup"><span data-stu-id="82da6-139">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="82da6-140">Pamięć podręczna pamięci rozproszonej nie jest rzeczywistą rozproszoną pamięcią podręczną.</span><span class="sxs-lookup"><span data-stu-id="82da6-140">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="82da6-141">Elementy buforowane są przechowywane przez wystąpienie aplikacji na serwerze, na którym działa aplikacja.</span><span class="sxs-lookup"><span data-stu-id="82da6-141">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="82da6-142">Pamięć podręczna pamięci rozproszonej jest przydatną implementacją:</span><span class="sxs-lookup"><span data-stu-id="82da6-142">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="82da6-143">W scenariuszach projektowania i testowania.</span><span class="sxs-lookup"><span data-stu-id="82da6-143">In development and testing scenarios.</span></span>
* <span data-ttu-id="82da6-144">Gdy jest używany pojedynczy serwer w środowisku produkcyjnym i użycie pamięci nie jest problemem.</span><span class="sxs-lookup"><span data-stu-id="82da6-144">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="82da6-145">Implementacja rozproszonej pamięci podręcznej pamięć podręczna magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="82da6-145">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="82da6-146">Pozwala to na wdrożenie prawdziwego rozwiązania do buforowania rozproszonego w przyszłości, jeśli będzie konieczne przeprowadzenie wielu węzłów lub odporności na uszkodzenia.</span><span class="sxs-lookup"><span data-stu-id="82da6-146">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="82da6-147">Przykładowa aplikacja korzysta z pamięci podręcznej pamięci rozproszonej, gdy aplikacja jest uruchamiana w środowisku `Startup.ConfigureServices`programistycznym w programie:</span><span class="sxs-lookup"><span data-stu-id="82da6-147">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="82da6-148">Pamięć podręczna rozproszonej SQL Server</span><span class="sxs-lookup"><span data-stu-id="82da6-148">Distributed SQL Server Cache</span></span>

<span data-ttu-id="82da6-149">Implementacja rozproszonej pamięci podręcznej SQL Server (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) umożliwia rozproszonej pamięci podręcznej użycie bazy danych SQL Server jako magazynu zapasowego.</span><span class="sxs-lookup"><span data-stu-id="82da6-149">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="82da6-150">Aby utworzyć SQL Server tabelę elementów w pamięci podręcznej w wystąpieniu SQL Server, można użyć `sql-cache` narzędzia.</span><span class="sxs-lookup"><span data-stu-id="82da6-150">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="82da6-151">Narzędzie tworzy tabelę z określoną nazwą i schematem.</span><span class="sxs-lookup"><span data-stu-id="82da6-151">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="82da6-152">Utwórz tabelę w SQL Server, uruchamiając `sql-cache create` polecenie.</span><span class="sxs-lookup"><span data-stu-id="82da6-152">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="82da6-153">Podaj`Data Source`wystąpienie SQL Server (), bazę danych (`Initial Catalog`), `dbo`schemat (na przykład) i nazwę tabeli (na przykład `TestCache`):</span><span class="sxs-lookup"><span data-stu-id="82da6-153">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="82da6-154">Komunikat jest rejestrowany w celu wskazania, że narzędzie zakończyło się pomyślnie:</span><span class="sxs-lookup"><span data-stu-id="82da6-154">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="82da6-155">Tabela utworzona przez `sql-cache` narzędzie ma następujący schemat:</span><span class="sxs-lookup"><span data-stu-id="82da6-155">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer Cache Table](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="82da6-157">Aplikacja powinna manipulować wartościami pamięci podręcznej przy użyciu <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>wystąpienia, a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>nie.</span><span class="sxs-lookup"><span data-stu-id="82da6-157">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="82da6-158">Przykładowa aplikacja jest <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> wdrażana w środowisku innym niż programowanie `Startup.ConfigureServices`w programie:</span><span class="sxs-lookup"><span data-stu-id="82da6-158">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="82da6-159">/ [](xref:security/app-secrets) (I <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> Opcjonalnie<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) są zwykle przechowywane poza kontrolą źródła (na przykład przechowywane przez Menedżera wpisów tajnych lub w pliku appSettings. JSON. { <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*>  *ENVIRONMENT}. JSON* — pliki).</span><span class="sxs-lookup"><span data-stu-id="82da6-159">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="82da6-160">Parametry połączenia mogą zawierać poświadczenia, które powinny być przechowywane w systemach kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="82da6-160">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="82da6-161">Redis Cache dystrybuowane</span><span class="sxs-lookup"><span data-stu-id="82da6-161">Distributed Redis Cache</span></span>

<span data-ttu-id="82da6-162">[Redis](https://redis.io/) to magazyn danych typu "open source", który jest często używany jako rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="82da6-162">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="82da6-163">Można używać Redis lokalnie i można skonfigurować [Azure Redis Cache](https://azure.microsoft.com/services/cache/) dla aplikacji ASP.NET Core hostowanej na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="82da6-163">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="82da6-164">Aplikacja konfiguruje implementację pamięci podręcznej przy<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>użyciu <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> wystąpienia () w środowisku innym niż `Startup.ConfigureServices`programowanie w programie:</span><span class="sxs-lookup"><span data-stu-id="82da6-164">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="82da6-165">Aplikacja konfiguruje implementację pamięci podręcznej przy<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>użyciu <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> wystąpienia () w środowisku innym niż `Startup.ConfigureServices`programowanie w programie:</span><span class="sxs-lookup"><span data-stu-id="82da6-165">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="82da6-166">Aplikacja konfiguruje implementację pamięci podręcznej przy<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>użyciu <xref:Microsoft.Extensions.Caching.Redis.RedisCache> wystąpienia ():</span><span class="sxs-lookup"><span data-stu-id="82da6-166">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

<span data-ttu-id="82da6-167">Aby zainstalować Redis na komputerze lokalnym:</span><span class="sxs-lookup"><span data-stu-id="82da6-167">To install Redis on your local machine:</span></span>

* <span data-ttu-id="82da6-168">Zainstaluj [pakiet Redis czekolady](https://chocolatey.org/packages/redis-64/).</span><span class="sxs-lookup"><span data-stu-id="82da6-168">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
* <span data-ttu-id="82da6-169">Uruchom `redis-server` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="82da6-169">Run `redis-server` from a command prompt.</span></span>

## <a name="use-the-distributed-cache"></a><span data-ttu-id="82da6-170">Korzystanie z rozproszonej pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="82da6-170">Use the distributed cache</span></span>

<span data-ttu-id="82da6-171">Aby użyć <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interfejsu, zażądaj <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> wystąpienia z dowolnego konstruktora w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82da6-171">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="82da6-172">Wystąpienie jest dostarczane przez [wstrzyknięcie zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="82da6-172">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="82da6-173">Po rozpoczęciu <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> przykładowej aplikacji jest wstrzykiwana `Startup.Configure`do.</span><span class="sxs-lookup"><span data-stu-id="82da6-173">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="82da6-174">Bieżący czas jest buforowany przy użyciu <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (Aby uzyskać więcej informacji, [Zobacz Host ogólny: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span><span class="sxs-lookup"><span data-stu-id="82da6-174">The current time is cached using <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (for more information, see [Generic Host: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="82da6-175">Po rozpoczęciu <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> przykładowej aplikacji jest wstrzykiwana `Startup.Configure`do.</span><span class="sxs-lookup"><span data-stu-id="82da6-175">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="82da6-176">Bieżący czas jest buforowany przy użyciu <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (Aby uzyskać więcej informacji, [Zobacz host sieci Web: Interfejs](xref:fundamentals/host/web-host#iapplicationlifetime-interface)IApplicationLifetime):</span><span class="sxs-lookup"><span data-stu-id="82da6-176">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

<span data-ttu-id="82da6-177">Przykładowa aplikacja wstrzykiwa <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `IndexModel` do użycia na stronie indeksu.</span><span class="sxs-lookup"><span data-stu-id="82da6-177">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="82da6-178">Za każdym razem, gdy strona indeksu zostanie załadowana, pamięć podręczna jest sprawdzana `OnGetAsync`pod kątem pamięci podręcznej w programie.</span><span class="sxs-lookup"><span data-stu-id="82da6-178">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="82da6-179">Jeśli czas w pamięci podręcznej nie upłynął, zostanie wyświetlony czas.</span><span class="sxs-lookup"><span data-stu-id="82da6-179">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="82da6-180">Jeśli od czasu ostatniego dostępu do pamięci podręcznej upłynie 20 sekund (podczas ostatniego ładowania strony), na stronie zostanie wyświetlona *godzina wygaśnięcia pamięci podręcznej*.</span><span class="sxs-lookup"><span data-stu-id="82da6-180">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="82da6-181">Natychmiast Aktualizuj buforowany czas do bieżącego czasu, wybierając przycisk **Resetuj buforowany czas** .</span><span class="sxs-lookup"><span data-stu-id="82da6-181">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="82da6-182">Przycisk wyzwala `OnPostResetCachedTime` metodę procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="82da6-182">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="82da6-183">Nie ma potrzeby używania pojedynczego lub zakresu okresu istnienia dla <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> wystąpień (co najmniej dla wbudowanych implementacji).</span><span class="sxs-lookup"><span data-stu-id="82da6-183">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="82da6-184">Możesz również utworzyć <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> wystąpienie wszędzie tam, gdzie może być potrzebne, zamiast używać di, ale utworzenie wystąpienia w kodzie może sprawić, że kod będzie trudniejszy do przetestowania i narusza [zasadę jawnych zależności](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span><span class="sxs-lookup"><span data-stu-id="82da6-184">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="82da6-185">Zalecenia</span><span class="sxs-lookup"><span data-stu-id="82da6-185">Recommendations</span></span>

<span data-ttu-id="82da6-186">Podczas decydowania, która implementacja <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> jest Najlepsza dla aplikacji, należy wziąć pod uwagę następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="82da6-186">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="82da6-187">Istniejąca infrastruktura</span><span class="sxs-lookup"><span data-stu-id="82da6-187">Existing infrastructure</span></span>
* <span data-ttu-id="82da6-188">Wymagania dotyczące wydajności</span><span class="sxs-lookup"><span data-stu-id="82da6-188">Performance requirements</span></span>
* <span data-ttu-id="82da6-189">Koszt</span><span class="sxs-lookup"><span data-stu-id="82da6-189">Cost</span></span>
* <span data-ttu-id="82da6-190">Środowisko pracy zespołu</span><span class="sxs-lookup"><span data-stu-id="82da6-190">Team experience</span></span>

<span data-ttu-id="82da6-191">Rozwiązania pamięci podręcznej zwykle polegają na magazynie w pamięci, aby zapewnić szybkie pobieranie danych z pamięci podręcznej, ale pamięć jest ograniczonym zasobem i kosztowna.</span><span class="sxs-lookup"><span data-stu-id="82da6-191">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="82da6-192">Przechowuj tylko często używane dane w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="82da6-192">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="82da6-193">Ogólnie pamięć podręczna Redis zapewnia wyższą przepływność i mniejsze opóźnienia niż w przypadku pamięci podręcznej SQL Server.</span><span class="sxs-lookup"><span data-stu-id="82da6-193">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="82da6-194">Jednak testy porównawcze są zwykle wymagane do określenia charakterystyki wydajności strategii buforowania.</span><span class="sxs-lookup"><span data-stu-id="82da6-194">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="82da6-195">Gdy SQL Server jest używany jako magazyn zapasowy rozproszonej pamięci podręcznej, korzystanie z tej samej bazy danych dla pamięci podręcznej i zwykłego magazynu danych aplikacji może mieć negatywny wpływ na wydajność obu tych elementów.</span><span class="sxs-lookup"><span data-stu-id="82da6-195">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="82da6-196">Zalecamy używanie dedykowanego wystąpienia SQL Server dla magazynu zapasowego rozproszonej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="82da6-196">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="82da6-197">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="82da6-197">Additional resources</span></span>

* [<span data-ttu-id="82da6-198">Redis Cache na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="82da6-198">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="82da6-199">SQL Database na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="82da6-199">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="82da6-200">[ASP.NET Core dostawcę IDistributedCache dla NCache w farmach sieci Web](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache w serwisie GitHub](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="82da6-200">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
