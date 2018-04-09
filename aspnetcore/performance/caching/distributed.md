---
title: Praca z rozproszonej pamięci podręcznej w ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać platformy ASP.NET Core rozproszonych buforowanie, aby zwiększyć wydajność aplikacji i skalowalność, szczególnie w środowisku farmy chmury lub serwera.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: d9c7c1c3b2c052ba11f9ea5eaaa424d69bc43eb2
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>Praca z rozproszonej pamięci podręcznej w ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Rozproszonej pamięci podręcznej może zwiększyć wydajność i skalowalność aplikacji platformy ASP.NET Core, szczególnie w przypadku hostowanych w środowisku farmy chmury lub serwera. W tym artykule opisano sposób pracy z implementacji i abstrakcje wbudowanych rozproszonej pamięci podręcznej platformy ASP.NET Core.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>Co to jest rozproszonej pamięci podręcznej

Rozproszonej pamięci podręcznej jest współużytkowany przez wiele serwerów aplikacji (zobacz [podstawowe informacje o pamięci podręcznej](memory.md#caching-basics)). Informacje w pamięci podręcznej nie jest przechowywany w pamięci poszczególnych sieci web, serwerów i buforowane dane są dostępne dla wszystkich serwerów aplikacji. To zapewnia kilka korzyści:

1. Buforowane dane są spójne na wszystkich serwerach sieci web. Użytkownicy nie będą widzieli różne wyniki, w zależności od tego, które sieci web serwer obsługuje żądania

2. Buforowane dane przeżyje ponownego uruchomienia serwera sieci web i wdrożenia. Serwery sieci web poszczególnych można usunięte lub dodane bez wpływu na pamięci podręcznej.

3. Źródłowy magazyn danych ma mniejszą liczbę żądań wysyłanych do niego (nie z wielu buforów w pamięci lub nie pamięci podręcznej w ogóle).

> [!NOTE]
> Jeśli używasz programu SQL Server rozproszonej pamięci podręcznej, niektóre z tych zalet są tylko wartość true, jeśli wystąpienie osobna baza danych jest używana dla pamięci podręcznej niż aplikacji źródła danych.

Podobnie jak wszelkie pamięci podręcznej rozproszonej pamięci podręcznej mogą znacznie zwiększyć czas odpowiedzi aplikacji, ponieważ zwykle można pobrać danych z pamięci podręcznej znacznie szybciej niż z relacyjnej bazy danych (lub usługa sieci web).

Konfigurację pamięci podręcznej zależy od implementacji. W tym artykule opisano, jak skonfigurować zarówno Redis i buforuje rozproszonych programu SQL Server. Niezależnie od tego, którego implementacja jest zaznaczone, aplikacja współdziała z pamięci podręcznej, za pomocą wspólnego `IDistributedCache` interfejsu.

## <a name="the-idistributedcache-interface"></a>Interfejs IDistributedCache

`IDistributedCache` Interfejs zawiera metody synchroniczne i asynchroniczne. Interfejs umożliwia elementów do dodania, pobrać i usunięcia z implementacja rozproszonej pamięci podręcznej. `IDistributedCache` Interfejs zawiera następujące metody:

**Get, GetAsync**

Pobiera klucz ciągu i pobiera buforowany element jako `byte[]` Jeśli znaleziono w pamięci podręcznej.

**Zestaw, SetAsync**

Dodaje element (jako `byte[]`) do pamięci podręcznej przy użyciu klucza ciągu.

**Odświeżanie, RefreshAsync**

Odświeża element w pamięci podręcznej na podstawie jego klucza zresetowanie jego przesuwanego limit czasu wygaśnięcia (jeśli istnieje).

**Usuń funkcja removeasync do usuwania**

Usuwa wpis pamięci podręcznej na podstawie jego klucza.

Aby użyć `IDistributedCache` interfejsu:

   1. Dodaj wymagane pakiety NuGet do pliku projektu.

   2. Skonfiguruj implementacja `IDistributedCache` w Twojej `Startup` klasy `ConfigureServices` metody i dodaj go do kontenera.

   3. Z poziomu aplikacji [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) lub klasy kontrolera MVC, żądania wystąpienia `IDistributedCache` z konstruktora. Wystąpienie, które będą udostępniane przez [iniekcji zależności](../../fundamentals/dependency-injection.md) (Podpisane).

> [!NOTE]
> Nie musi być pojedyncza lub polu Okres istnienia `IDistributedCache` wystąpień (co najmniej wbudowanych implementacji). Można również utworzyć wystąpienie wszędzie tam, gdzie może być konieczne jedną (zamiast [iniekcji zależności](../../fundamentals/dependency-injection.md)), ale może być trudniejsze do testowania, kodu i narusza [jawne zależności zasady](http://deviq.com/explicit-dependencies-principle/).

Poniższy przykład przedstawia użycie wystąpienia `IDistributedCache` w składniku proste oprogramowania pośredniczącego:

[!code-csharp[](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]

W powyższym kodzie wartość w pamięci podręcznej jest do odczytu, ale nigdy nie są zapisywane. W tym przykładzie wartość jest ustawiona tylko, gdy serwer jest uruchamiany i nie ulega zmianie. W scenariuszu z wieloma serwerami najnowszych serwera w celu uruchomienia zostaną zastąpione poprzednie wartości, które zostały określone przez inne serwery. `Get` i `Set` metody `byte[]` typu. W związku z tym wartość ciągu musi zostać przekonwertowany za pomocą `Encoding.UTF8.GetString` (dla `Get`) i `Encoding.UTF8.GetBytes` (dla `Set`).

Poniższy kod z *Startup.cs* pokazuje ustawienie wartości:

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]

> [!NOTE]
> Ponieważ `IDistributedCache` jest skonfigurowany w `ConfigureServices` metody, jest ona dostępna do `Configure` metody jako parametr. Dodanie go jako parametru umożliwi skonfigurowanego wystąpienia być dostarczane za pośrednictwem Podpisane.

## <a name="using-a-redis-distributed-cache"></a>Przy użyciu pamięci podręcznej Redis rozproszonych

[Redis](https://redis.io/) magazynu danych w pamięci typu open source, która jest często używana jako rozproszonej pamięci podręcznej. Możesz użyć jej lokalnie i można skonfigurować [pamięć podręczna Redis Azure](https://azure.microsoft.com/services/cache/) dla aplikacji hostowanej Azure platformy ASP.NET Core. Konfiguruje aplikację ASP.NET Core za pomocą implementacji pamięci podręcznej `RedisDistributedCache` wystąpienia.

Konfigurowanie wdrożenia Redis w `ConfigureServices` i dostęp do niego w kodzie aplikacji przez zażądanie wystąpienia `IDistributedCache` (zobacz powyżej kodu).

W przykładowym kodzie `RedisCache` implementacji jest używany, gdy serwer jest skonfigurowany dla `Staging` środowiska. W związku z tym `ConfigureStagingServices` konfiguruje metody `RedisCache`:

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]

> [!NOTE]
> Aby zainstalować Redis na komputerze lokalnym, należy zainstalować pakiet chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) i uruchom `redis-server` z wiersza polecenia.

## <a name="using-a-sql-server-distributed-cache"></a>Używanie programu SQL Server rozproszonej pamięci podręcznej

Implementacja SqlServerCache umożliwia rozproszonej pamięci podręcznej do użycia jako magazynu zapasowego, jego bazy danych programu SQL Server. Aby utworzyć program SQL Server tabeli można użyć narzędzia pamięci podręcznej sql, narzędzie tworzy tabelę z nazwą i schematu, które określisz.

Aby korzystać z narzędzia pamięci podręcznej sql, należy dodać `SqlConfig.Tools` do `<ItemGroup>` elementu *.csproj* pliku i uruchom dotnet przywracania.

[!code-xml[](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]

Przetestuj SqlConfig.Tools, uruchamiając następujące polecenie

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

Narzędzie pamięci podręcznej SQL do wyświetlenia pomocy użycia, opcje i poleceń, teraz możesz utworzyć tabel do programu sql server, uruchamiając polecenie "Utwórz pamięci podręcznej sql":

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

Utworzona tabela ma następującego schematu:

![Tabela SqlServer pamięci podręcznej](distributed/_static/SqlServerCacheTable.png)

Podobnie jak wszystkie implementacje pamięci podręcznej, aplikacja powinna pobieranie i ustawianie wartości pamięci podręcznej za pomocą wystąpienia `IDistributedCache`, a nie `SqlServerCache`. Implementuje próbki `SqlServerCache` w `Production` środowiska (tak jest skonfigurowany w `ConfigureProductionServices`).

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]

> [!NOTE]
> `ConnectionString` (I opcjonalnie `SchemaName` i `TableName`) zwykle powinny być przechowywane poza kontroli źródła (na przykład UserSecrets), ponieważ mogą one zawierać poświadczeń.

## <a name="recommendations"></a>Zalecenia

Podczas podejmowania decyzji o życie `IDistributedCache` jest prawa dla aplikacji, wybierz między Redis i SQL Server na podstawie istniejącej infrastruktury i środowiska, wymagań dotyczących wydajności i środowisko zespołu. Jeśli zespół jest wygodniejsze Praca z pamięci podręcznej Redis, jest doskonałym wyborem. Jeśli zespół preferuje programu SQL Server, można mieć pewność, że wykonanie również. Należy pamiętać, że tradycyjnych rozwiązań buforowania przechowuje dane w pamięci, która pozwala na szybkie odzyskiwanie danych. Należy przechowywania często używanych danych w pamięci podręcznej i Zapisz dane całej w magazynu trwałego wewnętrznej bazy danych, takich jak SQL Server lub usługi Azure Storage. Pamięć podręczna redis to rozwiązanie buforowania, co pozwala uzyskać wysoką przepustowość i małe opóźnienia w porównaniu do pamięci podręcznej SQL.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Pamięć podręczna Azure redis](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Baza danych SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* [Pamięci podręcznej w pamięci](xref:performance/caching/memory)
* [Wykrywanie zmian z tokenami zmiany](xref:fundamentals/primitives/change-tokens)
* [Buforowanie odpowiedzi](xref:performance/caching/response)
* [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware)
* [Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Pomocnik tagu rozproszonej pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
