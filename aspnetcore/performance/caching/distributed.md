---
title: Rozproszonej pamięci podręcznej w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak poprawić wydajność aplikacji i skalowalności, szczególnie w środowisku farmy, chmurze i na serwerze za pomocą platformy ASP.NET Core rozproszonej pamięci podręcznej.
ms.author: riande
ms.custom: mvc
ms.date: 10/19/2018
uid: performance/caching/distributed
ms.openlocfilehash: d80cde372535aa04604ce0cd5a731a1448515093
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50253011"
---
# <a name="distributed-caching-in-aspnet-core"></a>Rozproszonej pamięci podręcznej w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/) i [Luke Latham](https://github.com/guardrex)

Rozproszonej pamięci podręcznej jest pamięć podręczna współużytkowane przez wiele serwerów aplikacji, zazwyczaj przechowywane jako zewnętrznej usługi na serwerach aplikacji, mających do niej dostęp. Rozproszonej pamięci podręcznej może zwiększyć wydajność i skalowalność aplikacji ASP.NET Core, szczególnie w przypadku, gdy aplikacja jest hostowana w usłudze usługi w chmurze lub w farmie serwerów.

Rozproszonej pamięci podręcznej ma kilka zalet w stosunku do innych scenariuszy buforowania, gdzie buforowane dane są przechowywane na serwerach poszczególnych aplikacji.

Gdy dane w pamięci podręcznej jest rozpowszechniana, dane:

* Jest *spójnego* (spójność) w ramach żądania na wielu serwerach.
* Przeżyje serwer zostanie ponownie uruchomiony i wdrożenia aplikacji.
* Nie korzysta z lokalnej pamięci.

Konfiguracja rozproszonej pamięci podręcznej jest zależne od implementacji. W tym artykule opisano sposób konfigurowania programu SQL Server i rozproszonej pamięci podręczne redis Cache. Implementacje firm również są dostępne, takich jak [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache w serwisie GitHub](https://github.com/Alachisoft/NCache)). Niezależnie od tego, która implementacja jest zaznaczone, aplikacja korzysta z pamięci podręcznej przy użyciu <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interfejsu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

::: moniker range=">= aspnetcore-2.1"

Używanie programu SQL Server rozproszonej pamięci podręcznej, odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) pakietu.

Do użycia pamięci podręcznej Redis rozproszonej pamięci podręcznej, odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) i Dodaj odwołanie do pakietu [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) pakietu. Pakiet Redis nie jest zawarty w `Microsoft.AspNetCore.App` pakietu, więc użytkownik musi odwoływać się pakietu Redis oddzielnie w pliku projektu.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Używanie programu SQL Server rozproszonej pamięci podręcznej, odwołanie [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) lub Dodaj odwołanie do pakietu [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) pakietu.

Do użycia pamięci podręcznej Redis rozproszonej pamięci podręcznej, odwołanie [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) lub Dodaj odwołanie do pakietu [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) pakietu. Pakiet Redis znajduje się w `Microsoft.AspNetCore.All` pakietu, więc nie ma potrzeby odwołują się do pakietu Redis oddzielnie w pliku projektu.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Używanie programu SQL Server rozproszonej pamięci podręcznej, należy dodać odwołanie do pakietu [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) pakietu.

Aby używanie pamięci podręcznej Redis rozproszonej pamięci podręcznej, należy dodać odwołania do pakietu do [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) pakietu.

::: moniker-end

## <a name="idistributedcache-interface"></a>Interfejs IDistributedCache

<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> Interfejsu udostępnia następujące metody do manipulowania elementów w implementacja rozproszonej pamięci podręcznej:

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Akceptuje klucza typu ciąg i pobiera element pamięci podręcznej jako `byte[]` tablicy, jeśli znaleziono w pamięci podręcznej.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Dodaje element (jako `byte[]` tablicy) do pamięci podręcznej przy użyciu klucza typu ciąg.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Odświeża element w pamięci podręcznej na podstawie jego klucza zresetowanie jej przewijania limit czasu wygaśnięcia (jeśli istnieje).
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Usuwa element pamięci podręcznej na podstawie jego ciągu klucza.

## <a name="establish-distributed-caching-services"></a>Ustanowić rozproszonego usługi pamięci podręcznej

Zarejestruj implementację <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> w `Startup.ConfigureServices`. Implementacje dostarczone przez Framework opisane w tym temacie obejmują:

* [Rozproszonej pamięci podręcznej](#distributed-memory-cache)
* [Rozproszonej pamięci podręcznej programu SQL Server](#distributed-sql-server-cache)
* [Rozproszonej pamięci podręcznej redis Cache](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a>Rozproszonej pamięci podręcznej

Rozproszonej pamięci podręcznej (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) to implementacja dostarczone przez framework `IDistributedCache` , elementy są przechowywane w pamięci. Rozproszonej pamięci podręcznej nie jest rzeczywisty rozproszonej pamięci podręcznej. Elementy pamięci podręcznej są przechowywane przez wystąpienie aplikacji na serwerze, na którym działa aplikacja.

Rozproszonej pamięci podręcznej jest przydatne w implementacji:

* W zakresie projektowania i testowania scenariuszy.
* Gdy jeden serwer jest używany w produkcji i pamięci, użycie nie będzie to problemem. Implementowanie streszczenia rozproszonej pamięci podręcznej pamięci podręcznej magazynu danych. Umożliwia ona wdrażanie true rozproszonych rozwiązanie pamięci podręcznej w przyszłości w przypadku wielu węzłów lub odporności na uszkodzenia stają się niezbędne.

Przykładowa aplikacja korzysta z rozproszonej pamięci podręcznej, gdy aplikacja jest uruchamiana w środowisku deweloperskim:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

### <a name="distributed-sql-server-cache"></a>Rozproszone programu SQL Server w pamięci podręcznej

Implementacja rozproszonej pamięci podręcznej serwera SQL (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) umożliwia rozproszonej pamięci podręcznej do użycia bazę danych programu SQL Server jako jego magazyn zapasowy. Aby utworzyć tabelę element pamięci podręcznej programu SQL Server w wystąpieniu programu SQL Server, można użyć `sql-cache` narzędzia. Narzędzie tworzy tabelę o nazwie i schemat, który określisz.

::: moniker range="< aspnetcore-2.1"

Dodaj `SqlConfig.Tools` do `<ItemGroup>` elementu w pliku projektu i uruchom `dotnet restore`.

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools"
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

Utwórz tabelę w programie SQL Server, uruchamiając `sql-cache create` polecenia. Podaj wystąpienie programu SQL Server (`Data Source`), bazy danych (`Initial Catalog`), schematu (na przykład `dbo`) i nazwę tabeli (na przykład `TestCache`):

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

Aby wskazać, że narzędzie zakończyła się pomyślnie, zostanie zarejestrowany komunikat:

```console
Table and index were created successfully.
```

Tabelę utworzoną przez `sql-cache` narzędzie posiada zgodny z następującym schematem:

![Tabeli pamięci podręcznej SqlServer](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> Aplikacja powinna manipulowania wartości pamięci podręcznej przy użyciu wystąpienia <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, a nie <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.

Implementuje aplikacji przykładowej <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> w środowisku deweloperskim niż:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=9-15)]

> [!NOTE]
> A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (i, opcjonalnie, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> i <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) są zazwyczaj przechowywane poza kontrolą źródła (na przykład przechowywane przez [Menedżera klucz tajny](xref:security/app-secrets) lub *appsettings.json* / *appsettings. {Środowiska} .json* plików). Parametry połączenia mogą zawierać poświadczenia, które powinny być przechowywane poza systemów kontroli źródła.

### <a name="distributed-redis-cache"></a>Rozproszonej pamięci podręcznej Redis Cache

[Redis](https://redis.io/) to magazyn danych w pamięci "open source", która jest często używana jako rozproszonej pamięci podręcznej. Magazynu Redis można używać lokalnie i można skonfigurować [usługi Azure Redis Cache](https://azure.microsoft.com/services/cache/) dla aplikacji hostowanych na platformie Azure ASP.NET Core. Konfiguruje aplikację, za pomocą implementacji pamięci podręcznej <xref:Microsoft.Extensions.Caching.Redis.RedisCache> wystąpienia (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

Aby zainstalować usługi Redis na maszynie lokalnej:

* Zainstaluj [pakietu Chocolatey Redis](https://chocolatey.org/packages/redis-64/).
* Uruchom `redis-server` z poziomu wiersza polecenia.

## <a name="use-the-distributed-cache"></a>Przy użyciu rozproszonej pamięci podręcznej

Do użycia <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interfejsu, żądań wystąpienie `IDistributedCache` z dowolnym konstruktora w aplikacji. Wystąpienie jest dostarczany przez [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).

Po uruchomieniu aplikacji, `IDistributedCache` są wstrzykiwane do `Startup.Configure`. Bieżący czas jest buforowana przy użyciu <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (Aby uzyskać więcej informacji, zobacz [hosta sieci Web: interfejs IApplicationLifetime](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

Przykładowa aplikacja wprowadza `IDistributedCache` do `IndexModel` do użytku przez stronę indeksu.

Każdym załadowaniu strony indeksu pamięci podręcznej są sprawdzane pod kątem pamięci podręcznej czas w `OnGetAsync`. Jeśli nie upłynął czas pamięci podręcznej, jest wyświetlany czas. Po upływie 20 sekund od czasu ostatniego czasu pamięci podręcznej uzyskano dostęp (ostatni czas, ta strona została załadowana), zostanie wyświetlona strona *upłynął limit czasu pamięci podręcznej*.

Natychmiast zaktualizować pamięci podręcznej czas bieżący czas, wybierając **zresetować czasu pamięci podręcznej** przycisku. Wyzwalacze przycisk `OnPostResetCachedTime` metodę procedury obsługi.

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> Nie ma potrzeby używania pojedynczego wystąpienia lub polu Okres istnienia `IDistributedCache` wystąpienia (co najmniej wbudowanych implementacji).
>
> Można również utworzyć `IDistributedCache` wystąpienia wszędzie tam, gdzie może być konieczne zamiast DI, ale Tworzenie wystąpienia usługi w kodzie może być trudniejsze do testowania kodu i narusza [jawne zależności zasady](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).

## <a name="recommendations"></a>Zalecenia

Podczas podejmowania decyzji o życie `IDistributedCache` sprawdza się najlepiej w swojej aplikacji, należy wziąć pod uwagę następujące czynności:

* Istniejącą infrastrukturę
* Wymagania dotyczące wydajności
* Koszt
* Doświadczenia zespołu użytkownika

Rozwiązań buforowania na ogół magazynu w pamięci w celu zapewnienia szybkiego pobierania danych z pamięci podręcznej pamięci jest jednak ograniczona zasobów i kosztowna rozwinąć. Tylko magazynu często używane dane w pamięci podręcznej.

Ogólnie rzecz biorąc usługi Redis cache zapewnia wyższą przepływność i mniejsze opóźnienia niż pamięć podręczna programu SQL Server. Jednak testów porównawczych jest zazwyczaj wymagane, aby określić charakterystyki wydajności strategii buforowania.

Gdy program SQL Server jest używana jako magazyn zapasowy rozproszonej pamięci podręcznej, użycie tej samej bazy danych dla pamięci podręcznej i przechowywanie danych zwykłych aplikacji i pobierania może niekorzystnie wpłynąć na wydajność zarówno. Zalecamy używanie dedykowanego wystąpienia programu SQL Server dla rozproszonej pamięci podręcznej, magazyn zapasowy.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Usługa redis Cache na platformie Azure](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Bazy danych SQL na platformie Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* [ASP.NET Core IDistributedCache dostarczyciela NCache w formularzach sieci Web](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache w serwisie GitHub](https://github.com/Alachisoft/NCache))
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
