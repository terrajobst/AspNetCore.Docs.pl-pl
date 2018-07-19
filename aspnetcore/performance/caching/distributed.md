---
title: Praca z rozproszoną pamięcią podręczną w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać platformy ASP.NET Core rozproszonej pamięci podręcznej w celu poprawy wydajności aplikacji i skalowalności, szczególnie w środowisku farmy, chmurze i na serwerze.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 9c41a6e008045231bd2e1c1f53a9161e11daafa9
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123843"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>Praca z rozproszoną pamięcią podręczną w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Rozproszonej pamięci podręcznej może zwiększyć wydajność i skalowalność aplikacji platformy ASP.NET Core, szczególnie w przypadku hostowanych w chmurze lub w farmie serwerów.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>Co to jest rozproszonej pamięci podręcznej

Rozproszonej pamięci podręcznej jest współużytkowany przez wiele serwerów aplikacji (zobacz [podstawowe informacje o pamięci podręcznej](memory.md#caching-basics)). Informacje w pamięci podręcznej nie jest przechowywany w pamięci web poszczególnych serwerów, a dane w pamięci podręcznej jest dostępny dla wszystkich serwerów aplikacji. To zapewnia kilka korzyści:

1. Dane w pamięci podręcznej jest spójny na wszystkich serwerach sieci web. Użytkownicy nie widzą różne wyniki, w zależności od tego, w której sieci web serwer obsługuje żądania

2. Dane w pamięci podręcznej przeżyje ponowne uruchomienia serwera sieci web i wdrożenia. Serwery sieci web poszczególnych można usunięte lub dodane bez wywierania wpływu na pamięci podręcznej.

3. Źródłowy magazyn danych ma mniejszą liczbę żądań do niego (nie za pomocą wielu buforów w pamięci lub nie w pamięci podręcznej w ogóle).

> [!NOTE]
> Jeśli używasz programu SQL Server rozproszonej pamięci podręcznej, niektóre z tych korzyści są tylko wartość true, jeśli wystąpienie oddzielna baza danych jest używana dla pamięci podręcznej niż w przypadku aplikacji źródła danych.

Podobnie jak wszelkie pamięci rozproszonej pamięci podręcznej może znacznie zwiększyć czas reakcji aplikacji, ponieważ zazwyczaj można pobrać danych z pamięci podręcznej znacznie szybsze niż z relacyjnej bazy danych (lub usługi sieci web).

Konfiguracja pamięci podręcznej jest zależne od implementacji. W tym artykule opisano, jak skonfigurować zarówno Redis i buforuje rozproszone programu SQL Server. Niezależnie od tego, która implementacja jest zaznaczone, aplikacja korzysta z pamięci podręcznej, korzystając ze wspólnego `IDistributedCache` interfejsu.

## <a name="the-idistributedcache-interface"></a>Interfejs IDistributedCache

`IDistributedCache` Interfejs zawiera synchroniczne i asynchroniczne metody. Interfejs umożliwia elementy dodane, pobrane i usunięta z implementacja rozproszonej pamięci podręcznej. `IDistributedCache` Interfejs zawiera następujące metody:

**Get-GetAsync**

Pobiera klucz ciągu i pobiera element pamięci podręcznej jako `byte[]` Jeśli znaleziono w pamięci podręcznej.

**Zestaw, SetAsync**

Dodaje element (jako `byte[]`) do pamięci podręcznej przy użyciu klucza typu ciąg.

**Odświeżanie, RefreshAsync**

Odświeża element w pamięci podręcznej na podstawie jego klucza zresetowanie jej przewijania limit czasu wygaśnięcia (jeśli istnieje).

**Usuń RemoveAsync**

Usuwa wpis pamięci podręcznej na podstawie jego klucza.

Aby użyć `IDistributedCache` interfejsu:

   1. Dodaj wymagane pakiety NuGet do pliku projektu.

   2. Konfigurowanie określonej implementacji `IDistributedCache` w Twojej `Startup` klasy `ConfigureServices` metody i dodaj go do kontenera, istnieje.

   3. Z poziomu aplikacji [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) projektu i zlecania klasy kontrolera MVC, wystąpienie `IDistributedCache` z konstruktora. Wystąpienie będzie świadczona przez [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) (DI).

> [!NOTE]
> Nie ma potrzeby używania pojedynczego wystąpienia lub polu Okres istnienia `IDistributedCache` wystąpienia (co najmniej wbudowanych implementacji). Można również utworzyć wystąpienie wszędzie tam, gdzie możesz potrzebować jeden (zamiast [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md)), ale to może utrudnić kodu testu i narusza [jawne zależności zasady](http://deviq.com/explicit-dependencies-principle/).

Poniższy przykład pokazuje, jak używać wystąpienia `IDistributedCache` w składniku proste oprogramowania pośredniczącego:

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

W powyższym kodzie wartość w pamięci podręcznej jest odczytu, ale nigdy nie są zapisywane. W tym przykładzie ma tylko wartość serwer jest uruchamiany, gdy nie ulega zmianie. W przypadku wielu serwerów najnowszych serwera w celu uruchomienia spowoduje zastąpienie poprzedniej wartości, które zostały określone przez inne serwery. `Get` i `Set` metody za pomocą `byte[]` typu. W związku z tym, wartość ciągu musi być konwertowane przy użyciu `Encoding.UTF8.GetString` (dla `Get`) i `Encoding.UTF8.GetBytes` (dla `Set`).

Poniższy kod z *Startup.cs* pokazuje wartość:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

Ponieważ `IDistributedCache` jest skonfigurowana w `ConfigureServices` metody, jest ona dostępna do `Configure` metoda jako parametr. Umożliwi skonfigurowanego wystąpienia mają być dostarczane za pośrednictwem DI dodawania go jako parametr.

## <a name="using-a-redis-distributed-cache"></a>Za pomocą rozproszonej pamięci podręcznej Redis cache

[Redis](https://redis.io/) to magazyn danych w pamięci "open source", która jest często używana jako rozproszonej pamięci podręcznej. Można używać go lokalnie, a także można skonfigurować [usługi Azure Redis Cache](https://azure.microsoft.com/services/cache/) dla aplikacji hostowanych na platformie Azure platformy ASP.NET Core. Konfiguruje aplikacji platformy ASP.NET Core, przy użyciu implementacji pamięci podręcznej `RedisDistributedCache` wystąpienia.

Pamięć podręczna Redis wymaga [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)

Konfigurowanie implementacja pamięci podręcznej Redis w `ConfigureServices` i uzyskać do niego dostęp w kodzie aplikacji, wysyłając żądanie wystąpienie `IDistributedCache` (zobacz powyższy kod).

W przykładowym kodzie `RedisCache` implementacja jest używana, gdy serwer jest skonfigurowany dla `Staging` środowiska. Ten sposób `ConfigureStagingServices` konfiguruje metoda `RedisCache`:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

Aby zainstalować usługi Redis na maszynie lokalnej, należy zainstalować pakiet chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) i uruchom `redis-server` z poziomu wiersza polecenia.

## <a name="using-a-sql-server-distributed-cache"></a>Używanie programu SQL Server rozproszonej pamięci podręcznej

Implementacja SqlServerCache umożliwia rozproszonej pamięci podręcznej do użycia bazę danych programu SQL Server jako jego magazyn zapasowy. Do utworzenia programu SQL Server tabeli można użyć narzędzia pamięci podręcznej sql, to narzędzie tworzy tabelę o nazwie i schematu, które określisz.

::: moniker range="< aspnetcore-2.1"

Dodaj `SqlConfig.Tools` do `<ItemGroup>` elementu w pliku projektu i uruchom `dotnet restore`.

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

Przetestuj SqlConfig.Tools, uruchamiając następujące polecenie:

```console
dotnet sql-cache create --help
```

SqlConfig.Tools przedstawia sposób użycia i opcje pomocy dla poleceń.

Utwórz tabelę w programie SQL Server, uruchamiając `sql-cache create` polecenia:

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

Utworzonej tabeli ma zgodny z następującym schematem:

![Tabeli pamięci podręcznej SqlServer](distributed/_static/SqlServerCacheTable.png)

Podobnie jak wszystkie implementacjach pamięci podręcznej, aplikacji powinien pobieranie i ustawianie wartości pamięci podręcznej przy użyciu wystąpienia `IDistributedCache`, a nie `SqlServerCache`. Implementuje próbki `SqlServerCache` w środowisku produkcyjnym (dzięki czemu jest skonfigurowana w `ConfigureProductionServices`).

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> `ConnectionString` (I ewentualnie `SchemaName` i `TableName`) zwykle powinny być przechowywane poza kontrolą źródła (takich jak UserSecrets), ponieważ mogą one zawierać poświadczenia.

## <a name="recommendations"></a>Zalecenia

Podczas podejmowania decyzji o życie `IDistributedCache` jest odpowiednią dla aplikacji, wybierz między Redis i programu SQL Server na podstawie istniejącej infrastruktury i środowiska, wymagań dotyczących wydajności i doświadczeń zespołu. Jeśli Twój zespół jest idealnie pracy z pamięcią podręczną Redis, jest doskonałym wyborem. Jeśli preferuje zespół programu SQL Server, można mieć pewność, że także tę implementację. Należy pamiętać, że tradycyjne rozwiązanie pamięci podręcznej przechowuje dane w pamięci, która umożliwia do szybkiego pobierania danych. Należy przechowywać często używane dane w pamięci podręcznej i przechowywanie danych całej w magazynie trwałym wewnętrznej bazy danych, takich jak SQL Server lub usługi Azure Storage. Pamięć podręczna redis Cache jest rozwiązanie pamięci podręcznej, co daje wysokiej przepływności i małego opóźnienia w porównaniu do pamięci podręcznej SQL.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Usługa redis Cache na platformie Azure](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Bazy danych SQL na platformie Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
