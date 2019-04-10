---
title: Buforowanie w pamięci w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dane w pamięci w programie ASP.NET Core z pamięci podręcznej.
ms.author: riande
ms.custom: mvc
ms.date: 04/11/2019
uid: performance/caching/memory
ms.openlocfilehash: 6433df36023b79bc679186bee8b0a92371661dbe
ms.sourcegitcommit: 258a97159da206f9009f23fdf6f8fa32f178e50b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59425052"
---
# <a name="cache-in-memory-in-aspnet-core"></a>Buforowanie w pamięci w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), i [Steve Smith](https://ardalis.com/)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Podstawy pamięci podręcznej

Buforowanie może znacznie poprawić wydajność i skalowalność aplikacji, zmniejszając pracy wymaganej do generowania zawartości. Buforowanie działa najlepiej z danymi, które zmieniają się rzadko. Buforowanie sprawia, że kopii danych, które mogą być zwrócone znacznie szybciej niż z oryginalnego źródła. Aplikacje powinny być napisana i przetestowany w celu **nigdy nie** są zależne od danych w pamięci podręcznej.

Platforma ASP.NET Core obsługuje kilka różnych pamięci podręcznych. Najprostsza cache jest oparta na [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), która reprezentuje pamięć podręczną przechowywane w pamięci serwera sieci web. Aplikacje, które działają w farmie serwerów, wiele serwerów należy upewnić się, sesje są umocowany, korzystając z pamięci podręcznej. Trwałych sesji upewnij się, że kolejne żądania z klienta wszystkich przejdź do tego samego serwera. Na przykład użycie aplikacji sieci Web platformy Azure [Routing żądań aplikacji](https://www.iis.net/learn/extensions/planning-for-arr) (ARR), aby przekierować wszystkie kolejne żądania do tego samego serwera.

Non trwałych sesji w ramach farmy sieci web wymagają [rozproszonej pamięci podręcznej](distributed.md) Aby uniknąć problemów spójności pamięci podręcznej. W przypadku niektórych aplikacji rozproszonej pamięci podręcznej może obsługiwać wyższe skalowalnego w poziomie niż w pamięci podręcznej. Za pomocą rozproszonej pamięci podręcznej mniejsze pamięci podręcznej procesu zewnętrznego.

::: moniker range="< aspnetcore-2.0"

`IMemoryCache` Pamięci podręcznej Wyklucz wpisy w pamięci podręcznej duże wykorzystanie pamięci, chyba że [priorytet w pamięci podręcznej](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) ustawiono `CacheItemPriority.NeverRemove`. Możesz ustawić `CacheItemPriority` aby dopasować priorytet za pomocą której pamięci podręcznej wyklucza mogą elementów duże wykorzystanie pamięci.

::: moniker-end

Wewnątrzpamięciowa pamięć podręczna może przechowywać dowolny obiekt; Interfejs rozproszonej pamięci podręcznej jest ograniczona do `byte[]`. Elementy w pamięci i rozproszonej pamięci podręcznej magazynu pamięci podręcznej jako pary klucz wartość.

## <a name="systemruntimecachingmemorycache"></a>System.Runtime.Caching/MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([Pakietu NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) mogą być używane z:

* .NET standard 2.0 lub nowszej.
* Wszelkie [implementacji .NET](/dotnet/standard/net-standard#net-implementation-support) który jest przeznaczony dla .NET Standard 2.0 lub nowszej. Na przykład platforma ASP.NET Core 2.0 lub nowszej.
* .NET framework 4.5 lub nowszej.

[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (opisanych w tym artykule) są zalecane przez `System.Runtime.Caching` / `MemoryCache` ponieważ lepiej jest zintegrowana platforma ASP.NET Core. Na przykład `IMemoryCache` działa natywnie przy użyciu platformy ASP.NET Core [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).

Użyj `System.Runtime.Caching` / `MemoryCache` jako Most zgodności podczas przenoszenia kodu z ASP.NET 4.x, ASP.NET Core.

## <a name="cache-guidelines"></a>Wskazówki dotyczące pamięci podręcznej

* Kod powinien zawsze mieć opcja przełączania awaryjnego można pobrać danych i **nie** zależą od wartość w pamięci podręcznej jest dostępna.
* Pamięć podręczna używa ograniczonych zasobów pamięci. Limit pamięci podręcznej wzrost:
  * Czy **nie** zewnętrzne dane wejściowe są używane jako klucze pamięci podręcznej.
  * Wygasanie ważności poświadczeń można użyć, aby ograniczyć wzrostu pamięci podręcznej.
  * [Umożliwia ograniczenie rozmiaru pamięci podręcznej SetSize, rozmiar i SizeLimit](#use-setsize-size-and-sizelimit-to-limit-cache-size). Środowisko uruchomieniowe programu ASP.NET Core nie ogranicza rozmiar pamięci podręcznej, w oparciu o braku pamięci. To Ty dla deweloperów, aby ograniczyć rozmiar pamięci podręcznej.

## <a name="using-imemorycache"></a>Za pomocą IMemoryCache

Buforowanie w pamięci jest *usługi* , o której mowa w aplikacji przy użyciu [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md). Wywołaj `AddMemoryCache` w `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Żądanie `IMemoryCache` wystąpienia w Konstruktorze:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache` wymaga pakietu NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache` wymaga pakietu NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), który jest dostępny w [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache` wymaga pakietu NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), który jest dostępny w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

Poniższy kod używa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) do sprawdzenia, czy w pamięci podręcznej przez czas. Jeśli nie jest buforowany przez czas, nowy wpis jest utworzony i dodany do pamięci podręcznej przy użyciu [ustaw](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Bieżący czas i czas pamięci podręcznej są wyświetlane:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Buforowane `DateTime` wartość pozostaje w pamięci podręcznej, gdy istnieją żądań przed upływem limitu czasu. Na poniższej ilustracji przedstawiono bieżący czas i czas starsze, pobierane z pamięci podręcznej:

![Widok indeksu z dwóch różnych godzinach wyświetlane](memory/_static/time.png)

Poniższy kod używa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) i [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) do buforowania danych.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Poniższy kod wywoła [uzyskać](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) można pobrać czasu pamięci podręcznej:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*> , <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, i [uzyskać](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) wchodzą w skład metod rozszerzenia [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) klasy, która rozszerza możliwości <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>. Zobacz [metody IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) i [metody CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) opis innych metod pamięci podręcznej.

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

Poniższy przykład:

* Ustawia czas wygaśnięcia bezwzględne. To jest maksymalny czas, które mogą być buforowane wpis i uniemożliwia element staje się zbyt stare przy wygaśniecie stale zostanie odnowiony.
* Ustawia przewijania czas wygaśnięcia. Żądania, uzyskujących dostęp do tego elementu pamięci podręcznej spowoduje zresetowanie przewijania zegara wygaśnięcia.
* Ustawia priorytet pamięci podręcznej na `CacheItemPriority.NeverRemove`.
* Zestawy [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , zostanie wywołana po wpis zostanie usunięty z pamięci podręcznej. Wywołanie zwrotne jest uruchamiane w innym wątku z kodu, który usuwa element z pamięci podręcznej.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>SetSize użycie, rozmiar i SizeLimit limit rozmiaru pamięci podręcznej

A `MemoryCache` wystąpienia, mogą opcjonalnie ustalenie i wymuszenie limit rozmiaru. Limit rozmiaru pamięci nie ma zdefiniowanych jednostka miary, ponieważ żaden mechanizm do mierzenia rozmiar wpisów pamięci podręcznej. Jeśli ustawiono limit rozmiaru pamięci podręcznej, wszystkie wpisy, należy określić rozmiar. Środowisko uruchomieniowe programu ASP.NET Core nie ogranicza rozmiar pamięci podręcznej, w oparciu o braku pamięci. To Ty dla deweloperów, aby ograniczyć rozmiar pamięci podręcznej. Określony rozmiar jest wyrażona w jednostkach, który wybiera dewelopera.

Na przykład:

* Jeśli aplikacja sieci web został przede wszystkim buforowania ciągów, rozmiar każdego wpisu pamięci podręcznej może być długość ciągu.
* Aplikacja może określić rozmiar wszystkich wpisów jako 1, a limit rozmiaru wynosi liczba wpisów.

Poniższy kod tworzy unitless ustalony rozmiar [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) dostępny za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) nie mieć jednostki. Wpisy pamięci podręcznej, należy określić rozmiar w dowolnych jednostkach uznają za najbardziej odpowiednie, jeśli rozmiar pamięci podręcznej został ustawiony. Wszyscy użytkownicy wystąpienia pamięci podręcznej należy używać tego samego systemu jednostki. Wpis nie będzie zapisywane, jeśli suma rozmiarów wpisu pamięci podręcznej przekracza wartość określoną przez `SizeLimit`. Jeśli nie mają limitu rozmiaru pamięci podręcznej jest ustawiona, rozmiar pamięci podręcznej wpisu zostaną zignorowane.

Poniższy kod rejestruje `MyMemoryCache` z [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera.

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache` zostanie utworzona jako niezależne pamięci podręcznej dla składników, które zdawali sobie sprawę z tego ograniczony rozmiar pamięci podręcznej i dowiedzieć się, jak odpowiednie ustawienie rozmiaru wpisu pamięci podręcznej.

Poniższy kod używa `MyMemoryCache`:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

Można ustawić rozmiaru wpisu pamięci podręcznej [rozmiar](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) lub [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) — metoda rozszerzenia:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a>Zależności pamięci podręcznej

Poniższy przykład pokazuje, jak wygaśnie wpisu pamięci podręcznej, jeśli wygaśnięcia wpisu zależnych. Element `CancellationChangeToken` jest dodawana do pamięci podręcznej elementu. Gdy `Cancel` jest wywoływana w `CancellationTokenSource`, obrazuje zarówno wpisy w pamięci podręcznej.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Za pomocą `CancellationTokenSource` zezwala na wiele wpisów pamięci podręcznej wykluczenie jako grupa. Za pomocą `using` wzorca w powyższym kodzie, wpisy w pamięci podręcznej utworzony wewnątrz `using` bloku będzie dziedziczyć wyzwalaczy i ustawienia wygaśnięcia.

## <a name="additional-notes"></a>Dodatkowe uwagi

* Korzystając z wywołania zwrotnego do wypełnienia elementu pamięci podręcznej:

  * Wiele żądań można znaleźć wartość klucza w pamięci podręcznej pusty, ponieważ wywołanie zwrotne nie zostało ukończone.
  * Może to spowodować, że wiele wątków uważnie element pamięci podręcznej.

* Gdy jeden wpis pamięci podręcznej jest używany do tworzenia kolejnych, elementu podrzędnego kopiuje wpis nadrzędny wygaśnięcia tokenów i ustawienia na podstawie czasu wygaśnięcia. Element podrzędny nie jest wygasły przez ręczne usunięcie lub aktualizowania wpisu nadrzędnego.

* Użyj [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) można ustawić wywołania zwrotne, które będą uruchamiane po wpisu pamięci podręcznej zostanie usunięty z pamięci podręcznej.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
