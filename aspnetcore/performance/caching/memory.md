---
title: Buforowanie w pamięci w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak buforować dane w pamięci w ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 8/22/2019
uid: performance/caching/memory
ms.openlocfilehash: 3005adec9ffe41859d05a3f61c7c45b8e7bfeefc
ms.sourcegitcommit: bdaee0e8c657fe7546fd6b7990db9c03c2af04df
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2019
ms.locfileid: "69908374"
---
# <a name="cache-in-memory-in-aspnet-core"></a>Buforowanie w pamięci w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT), [Jan Luo](https://github.com/JunTaoLuo)i [Steve Smith](https://ardalis.com/)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/3.0sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Podstawowe informacje o buforowaniu

Buforowanie może znacząco poprawić wydajność i skalowalność aplikacji przez zmniejszenie ilości pracy wymaganej do wygenerowania zawartości. Buforowanie działa najlepiej w przypadku rzadko używanych danych. Buforowanie tworzy kopię danych, która może być zwracana znacznie szybciej niż ze źródła. Aplikacje powinny być zapisane i przetestowane w taki sposób, aby **nigdy nie** zależały od danych buforowanych.

ASP.NET Core obsługuje kilka różnych pamięci podręcznych. Najprostsza pamięć podręczna jest oparta na [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). `IMemoryCache`Reprezentuje pamięć podręczną przechowywaną w pamięci serwera sieci Web. Aplikacje działające w farmie serwerów (wiele serwerów) powinny zapewnić, że sesje są w trakcie korzystania z pamięci podręcznej w pamięci. Sesje usługi Sticky Notes zapewniają, że kolejne żądania od klienta będą kierowane do tego samego serwera. Na przykład usługa Azure Web Apps używa [routingu żądań aplikacji](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) do kierowania wszystkich kolejnych żądań do tego samego serwera.

Sesje inne niż nietrwałe w kolektywie serwerów sieci Web wymagają [rozproszonej pamięci](distributed.md) podręcznej, aby uniknąć problemów ze spójnością pamięci W przypadku niektórych aplikacji rozproszonej pamięci podręcznej może obsługiwać większą skalowalność niż pamięć podręczna w pamięci. Użycie rozproszonej pamięci podręcznej powoduje odciążenie pamięci podręcznej do procesu zewnętrznego.

Pamięć podręczna w pamięci może przechowywać dowolny obiekt. Interfejs rozproszonej pamięci podręcznej `byte[]`jest ograniczony do. Magazyn w pamięci i rozproszonej pamięci podręcznej przechowuje elementy pamięci podręcznej jako pary klucz-wartość.

## <a name="systemruntimecachingmemorycache"></a>System. Runtime. buforowanie/elemencie MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>([Pakiet NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) może być używany z:

* .NET Standard 2,0 lub nowszy.
* Dowolna [implementacja platformy .NET](/dotnet/standard/net-standard#net-implementation-support) , która jest przeznaczona dla .NET Standard 2,0 lub nowszych. Na przykład ASP.NET Core 2,0 lub nowszy.
* .NET Framework 4,5 lub nowszy.

[Firma Microsoft. Extensions. buforowanie. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (opisana w tym artykule) jest zalecana `System.Runtime.Caching` w porównaniu / `MemoryCache` ze względu na to, że jest lepiej zintegrowana z ASP.NET Core. Na przykład `IMemoryCache` działa natywnie z iniekcją ASP.NET Core [zależności](xref:fundamentals/dependency-injection).

Użyj `System.Runtime.Caching` jakomostka`MemoryCache` zgodności podczas przenoszenia kodu z ASP.NET 4. x do ASP.NET Core. /

## <a name="cache-guidelines"></a>Wskazówki dotyczące pamięci podręcznej

* Kod powinien zawsze mieć opcję rezerwową, aby pobrać dane i **nie** zależał od dostępnej wartości w pamięci podręcznej.
* Pamięć podręczna używa nieodpowiedniego zasobu, pamięci. Ogranicz wzrost rozmiaru pamięci podręcznej:
  * **Nie** należy używać zewnętrznych danych wejściowych jako kluczy pamięci podręcznej.
  * Użyj wygaśnięć, aby ograniczyć wzrost rozmiaru pamięci podręcznej.
  * [Użyj SetSize, size i SizeLimit, aby ograniczyć rozmiar pamięci](#use-setsize-size-and-sizelimit-to-limit-cache-size)podręcznej. Środowisko uruchomieniowe ASP.NET Core nie ogranicza rozmiaru pamięci podręcznej na podstawie nacisku pamięci. Aby ograniczyć rozmiar pamięci podręcznej, należy do dewelopera.

## <a name="use-imemorycache"></a>Użyj IMemoryCache

> [!WARNING]
> Użycie pamięci podręcznej pamięci współużytkowanej przed `SetSize` [iniekcją zależności](xref:fundamentals/dependency-injection) i wywołaniem, `Size`lub `SizeLimit` w celu ograniczenia rozmiaru pamięci podręcznej może spowodować niepowodzenie aplikacji. Po ustawieniu limitu rozmiaru w pamięci podręcznej, wszystkie wpisy muszą określać rozmiar podczas dodawania. Może to prowadzić do problemów, ponieważ deweloperzy mogą nie mieć pełnej kontroli nad używaniem udostępnionej pamięci podręcznej. Na przykład Entity Framework Core używa udostępnionej pamięci podręcznej i nie określa rozmiaru. Jeśli aplikacja ustawi limit rozmiaru pamięci podręcznej i użyje EF Core, aplikacja zgłosi `InvalidOperationException`.
> W przypadku `SetSize`korzystania `Size`z, `SizeLimit` , lub do ograniczania pamięci podręcznej, należy utworzyć pojedynczą pamięć podręczną dla buforowania. Aby uzyskać więcej informacji i zapoznać się z przykładem, zobacz [Używanie SetSize, size i SizeLimit w celu ograniczenia rozmiaru pamięci](#use-setsize-size-and-sizelimit-to-limit-cache-size)podręcznej.

Buforowanie w pamięci to *Usługa* , do której odwołuje się aplikacja przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection). Zażądaj `IMemoryCache` wystąpienia w konstruktorze:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ctor)]

Poniższy kod używa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) , aby sprawdzić, czy czas znajduje się w pamięci podręcznej. Jeśli czas nie jest buforowany, nowy wpis zostanie utworzony i dodany do pamięci podręcznej [](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)z zestawem. `CacheKeys` Klasa jest częścią przykładu pobierania.

[! code-CSharp [] (przykład pamięci/3.0/WebCacheSample/CacheKeys. cs) [](memory/3.0sample/WebCacheSample/CacheKeys.cs)]

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet1)]

Bieżąca godzina i w pamięci podręcznej są wyświetlane:

[!code-cshtml[](memory/3.0sample/WebCacheSample/Views/Home/Cache.cshtml)]

Buforowana `DateTime` wartość pozostaje w pamięci podręcznej, gdy istnieją żądania w określonym limicie czasu.

Poniższy kod używa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) i [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) do buforowania danych.

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Następujący kod wywołuje [pobieranie](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) , aby pobrać buforowany czas:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_gct)]

Poniższy kod pobiera lub tworzy buforowany element z bezwzględnym wygaśnięciem:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet99)]

Buforowany element zestawu elementów z przewijanym okresem ważności jest narażony na nieodświeżony, ponieważ nie ma żadnego powiązania z jego wygaśnięciem. Użyj bezwzględnego wygaśnięcia z ruchomym wygaśnięciem w celu zagwarantowania, że buforowany element nie stanie się bardziej nieodświeżony niż bezwzględne wygaśnięcie. Gdy bezwzględne wygaśnięcie jest połączone z przesuwaniem, bezwzględne wygaśnięcie Ustawia górną granicę, jak długo element może być buforowany. W przeciwieństwie do bezwzględnego czasu wygaśnięcia, jeśli element nie jest żądany z pamięci podręcznej w ramach interwału wygaśnięcia, element zostanie wykluczony z pamięci podręcznej. Po określeniu wygaśnięcia bezwzględnego i przesuwania, wygaśnięcia są logicznie logicznie.

Poniższy kod pobiera lub tworzy buforowany element z przesuwaniem i bezwzględnym okresem ważności:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet9)]

Poprzedni kod gwarantuje, że dane nie będą przechowywane w pamięci podręcznej dłużej niż czas bezwzględny.

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, i <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.Get*> są metodami <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions> rozszerzenia części klasy, która rozszerza możliwości programu <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>. Zobacz [metody IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) i [CacheExtensions metody](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) opisujące inne metody pamięci podręcznej.

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

Poniższy przykład:

* Ustawia czas wygaśnięcia. Żądania, które uzyskują dostęp do tego elementu w pamięci podręcznej, spowodują zresetowanie zegara zakończenia przewijania.
* Ustawia priorytet pamięci podręcznej na [CacheItemPriority. NeverRemove](xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove).
* Ustawia [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , który zostanie wywołany po wykluczeniu wpisu z pamięci podręcznej. Wywołanie zwrotne jest uruchamiane w innym wątku niż kod, który usuwa element z pamięci podręcznej.

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Użyj SetSize, size i SizeLimit, aby ograniczyć rozmiar pamięci podręcznej

`MemoryCache` Wystąpienie może opcjonalnie określić i wymusić limit rozmiaru. Limit rozmiaru pamięci nie ma zdefiniowanej jednostki miary, ponieważ pamięć podręczna nie ma mechanizmu mierzenia rozmiaru wpisów. Jeśli ustawiono limit rozmiaru pamięci podręcznej, wszystkie wpisy muszą określać rozmiar. Środowisko uruchomieniowe ASP.NET Core nie ogranicza rozmiaru pamięci podręcznej na podstawie nacisku pamięci. Aby ograniczyć rozmiar pamięci podręcznej, należy do dewelopera. Określony rozmiar jest w jednostkach wybranych przez dewelopera.

Na przykład:

* Jeśli aplikacja sieci Web była przede wszystkim buforowania ciągów, każdy rozmiar wpisu pamięci podręcznej może być długością ciągu.
* Aplikacja może określić rozmiar wszystkich wpisów jako 1, a limit rozmiaru to liczba wpisów.

Jeśli <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit> nie jest ustawiona, pamięć podręczna rośnie bez powiązania. Środowisko uruchomieniowe ASP.NET Core nie przycina pamięci podręcznej, gdy ilość pamięci systemowej jest niska. Aplikacje są w znacznym stopniu zaprojektowane pod kątem:

* Ogranicz wzrost rozmiaru pamięci podręcznej.
* Połączenie <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> lub<xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*> Jeśli dostępna pamięć jest ograniczona:

Poniższy kod tworzy bezjednostkowy rozmiar <xref:Microsoft.Extensions.Caching.Memory.MemoryCache> dostępny w ramach iniekcji [zależności](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

`SizeLimit`nie ma jednostek. Wpisy w pamięci podręcznej muszą określać rozmiar w jednostkach, które są uważane za najbardziej odpowiednie, jeśli ustawiono rozmiar pamięci podręcznej. Wszyscy użytkownicy wystąpienia pamięci podręcznej powinni używać tego samego systemu jednostek. Wpis nie zostanie zapisany w pamięci podręcznej, jeśli suma rozmiarów buforowanych wpisów przekroczy `SizeLimit`wartość określoną przez. Jeśli limit rozmiaru pamięci podręcznej nie zostanie ustawiony, rozmiar pamięci podręcznej ustawiony na wpis zostanie zignorowany.

Poniższy kod rejestruje `MyMemoryCache` z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) .

[!code-csharp[](memory/3.0sample/RPcache/Startup.cs?name=snippet)]

`MyMemoryCache`jest tworzony jako pamięć podręczna niezależna pamięci dla składników, które są świadome pamięci podręcznej ograniczonej rozmiaru i wiedzą, jak ustawić odpowiednio rozmiar wpisu pamięci podręcznej.

Następujący kod używa `MyMemoryCache`:

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet)]

Rozmiar wpisu pamięci podręcznej może być ustawiony przez <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.Size> <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.SetSize*> lub metody rozszerzenia:

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a>Elemencie MemoryCache. Compact

`MemoryCache.Compact`próbuje usunąć określony procent pamięci podręcznej w następującej kolejności:

* Wszystkie elementy wygasłe.
* Elementy według priorytetu. Elementy o najniższym priorytecie są usuwane jako pierwsze.
* Ostatnio używane obiekty.
* Elementy z najwcześniejszym bezwzględnym okresem ważności.
* Elementy z najwcześniejszym okresem ważności.

Przypięte elementy <xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove> z priorytetem nigdy nie są usuwane. Poniższy kod usuwa element pamięci podręcznej i `Compact`wywołuje:

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

Aby uzyskać więcej informacji, zobacz artykuł [Compact Source w witrynie GitHub](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393) .

## <a name="cache-dependencies"></a>Zależności pamięci podręcznej

Poniższy przykład pokazuje, jak wygasa wpis pamięci podręcznej, Jeśli wpis zależny wygaśnie. Element `CancellationChangeToken` jest dodawany do elementu w pamięci podręcznej. Gdy `Cancel` jest wywoływana `CancellationTokenSource`w, oba wpisy pamięci podręcznej są wykluczone.

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ed)]

Użycie a `CancellationTokenSource` umożliwia wykluczenie wielu wpisów pamięci podręcznej jako grupy. Ze wzorcem w powyższym kodzie wpisy pamięci podręcznej `using` utworzone wewnątrz bloku będą dziedziczyć wyzwalacze i ustawienia wygasania. `using`

## <a name="additional-notes"></a>Dodatkowe uwagi

* Wygaśnięcie nie odbywa się w tle. Nie ma czasomierza, który aktywnie skanuje pamięć podręczną pod kątem wygasłych elementów. Wszystkie działania w pamięci podręcznej `Set`( `Remove``Get`,,) mogą wyzwalać skanowanie w tle dla elementów, które utraciły ważność. Czasomierz w `CancellationTokenSource` (`CancelAfter`) spowoduje również usunięcie wpisu i wyzwolenie skanowania w poszukiwaniu wygasłych elementów. Na przykład zamiast `SetAbsoluteExpiration(TimeSpan.FromHours(1))`używać, użyj `CancellationTokenSource.CancelAfter(TimeSpan.FromHours(1))` dla zarejestrowanego tokenu. Gdy ten token wyzwala, usuwa wpis natychmiast i wyzwala wywołania zwrotne wykluczenia. Aby uzyskać więcej informacji, zobacz [problem w usłudze GitHub](https://github.com/aspnet/Caching/issues/248).
* W przypadku użycia wywołania zwrotnego do ponownego wypełnienia elementu pamięci podręcznej:

  * Wiele żądań może znaleźć pustą wartość klucza w pamięci podręcznej, ponieważ wywołanie zwrotne nie zostało zakończone.
  * Może to spowodować, że kilka wątków będzie przepełniać element w pamięci podręcznej.

* Gdy jeden wpis pamięci podręcznej jest używany do utworzenia innego, element podrzędny kopiuje tokeny wygaśnięcia i czas wygaśnięcia na podstawie czasu. Element podrzędny nie wygasł przez ręczne usunięcie lub aktualizację wpisu nadrzędnego.

* Użyj <xref:Microsoft.Extensions.Caching.Memory.ICacheEntry.PostEvictionCallbacks> , aby ustawić wywołania zwrotne, które będą wyzwalane po usunięciu wpisu pamięci podręcznej z tej pamięci.
* W przypadku większości aplikacji `IMemoryCache` jest włączona. Na przykład wywoływanie `AddMvc`, `AddControllersWithViews`, `AddRazorPages` `Add{Service}` `ConfigureServices`,, i wiele innych metod w, włącza `IMemoryCache`. `AddMvcCore().AddRazorViewEngine` W przypadku aplikacji, które nie wywołuje jednej z powyższych `Add{Service}` metod, może być konieczne `ConfigureServices`wywołanie metody <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddMemoryCache*> .

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<!-- This is the 2.1 version -->
Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT), [Jan Luo](https://github.com/JunTaoLuo)i [Steve Smith](https://ardalis.com/)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Podstawowe informacje o buforowaniu

Buforowanie może znacząco poprawić wydajność i skalowalność aplikacji przez zmniejszenie ilości pracy wymaganej do wygenerowania zawartości. Buforowanie działa najlepiej w przypadku rzadko używanych danych. Buforowanie tworzy kopię danych, która może być zwracana znacznie szybciej niż z oryginalnego źródła. Kod powinien być zapisany i przetestowany w taki sposób, aby **nigdy nie** zależał od danych buforowanych.

ASP.NET Core obsługuje kilka różnych pamięci podręcznych. Najprostsza pamięć podręczna jest oparta na [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), która reprezentuje pamięć podręczną przechowywaną w pamięci serwera sieci Web. Aplikacje działające w farmie serwerów (wiele serwerów) powinny mieć pewność, że sesje są w trakcie korzystania z pamięci podręcznej w pamięci. Sesje usługi Sticky Notes zapewniają, że późniejsze żądania od klienta będą kierowane do tego samego serwera. Na przykład usługa Azure Web Apps używa [routingu żądań aplikacji](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) do kierowania wszystkich żądań od agenta użytkownika na ten sam serwer.

Sesje inne niż nietrwałe w kolektywie serwerów sieci Web wymagają [rozproszonej pamięci](distributed.md) podręcznej, aby uniknąć problemów ze spójnością pamięci W przypadku niektórych aplikacji rozproszonej pamięci podręcznej może obsługiwać większą skalowalność niż pamięć podręczna w pamięci. Użycie rozproszonej pamięci podręcznej powoduje odciążenie pamięci podręcznej do procesu zewnętrznego.

Pamięć podręczna w pamięci może przechowywać dowolny obiekt. Interfejs rozproszonej pamięci podręcznej `byte[]`jest ograniczony do. Magazyn w pamięci i rozproszonej pamięci podręcznej przechowuje elementy pamięci podręcznej jako pary klucz-wartość.

## <a name="systemruntimecachingmemorycache"></a>System. Runtime. buforowanie/elemencie MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>([Pakiet NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) może być używany z:

* .NET Standard 2,0 lub nowszy.
* Dowolna [implementacja platformy .NET](/dotnet/standard/net-standard#net-implementation-support) , która jest przeznaczona dla .NET Standard 2,0 lub nowszych. Na przykład ASP.NET Core 2,0 lub nowszy.
* .NET Framework 4,5 lub nowszy.

[Firma Microsoft. Extensions. buforowanie. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (opisana w tym artykule) jest zalecana `System.Runtime.Caching` w porównaniu / `MemoryCache` ze względu na to, że jest lepiej zintegrowana z ASP.NET Core. Na przykład `IMemoryCache` działa natywnie z iniekcją ASP.NET Core [zależności](xref:fundamentals/dependency-injection).

Użyj `System.Runtime.Caching` jakomostka`MemoryCache` zgodności podczas przenoszenia kodu z ASP.NET 4. x do ASP.NET Core. /

## <a name="cache-guidelines"></a>Wskazówki dotyczące pamięci podręcznej

* Kod powinien zawsze mieć opcję rezerwową, aby pobrać dane i **nie** zależał od dostępnej wartości w pamięci podręcznej.
* Pamięć podręczna używa nieodpowiedniego zasobu, pamięci. Ogranicz wzrost rozmiaru pamięci podręcznej:
  * **Nie** należy używać zewnętrznych danych wejściowych jako kluczy pamięci podręcznej.
  * Użyj wygaśnięć, aby ograniczyć wzrost rozmiaru pamięci podręcznej.
  * [Użyj SetSize, size i SizeLimit, aby ograniczyć rozmiar pamięci](#use-setsize-size-and-sizelimit-to-limit-cache-size)podręcznej. Środowisko uruchomieniowe ASP.NET Core nie ogranicza rozmiaru pamięci podręcznej na podstawie nacisku pamięci. Aby ograniczyć rozmiar pamięci podręcznej, należy do dewelopera.

## <a name="using-imemorycache"></a>Korzystanie z IMemoryCache

> [!WARNING]
> Użycie pamięci podręcznej pamięci współużytkowanej przed `SetSize` [iniekcją zależności](xref:fundamentals/dependency-injection) i wywołaniem, `Size`lub `SizeLimit` w celu ograniczenia rozmiaru pamięci podręcznej może spowodować niepowodzenie aplikacji. Po ustawieniu limitu rozmiaru w pamięci podręcznej, wszystkie wpisy muszą określać rozmiar podczas dodawania. Może to prowadzić do problemów, ponieważ deweloperzy mogą nie mieć pełnej kontroli nad używaniem udostępnionej pamięci podręcznej. Na przykład Entity Framework Core używa udostępnionej pamięci podręcznej i nie określa rozmiaru. Jeśli aplikacja ustawi limit rozmiaru pamięci podręcznej i użyje EF Core, aplikacja zgłosi `InvalidOperationException`.
> W przypadku `SetSize`korzystania `Size`z, `SizeLimit` , lub do ograniczania pamięci podręcznej, należy utworzyć pojedynczą pamięć podręczną dla buforowania. Aby uzyskać więcej informacji i zapoznać się z przykładem, zobacz [Używanie SetSize, size i SizeLimit w celu ograniczenia rozmiaru pamięci](#use-setsize-size-and-sizelimit-to-limit-cache-size)podręcznej.

Buforowanie w pamięci to *Usługa* , do której odwołuje się aplikacja przy użyciu [iniekcji zależności](../../fundamentals/dependency-injection.md). Wywołanie `AddMemoryCache` w `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Zażądaj `IMemoryCache` wystąpienia w konstruktorze:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

`IMemoryCache`wymaga pakietu NuGet [Microsoft. Extensions. buforowanie. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), który jest dostępny w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

Poniższy kod używa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) , aby sprawdzić, czy czas znajduje się w pamięci podręcznej. Jeśli czas nie jest buforowany, nowy wpis zostanie utworzony i dodany do pamięci podręcznej [](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)z zestawem.

[! code-CSharp [] (pamięć/przykład/webcache/CacheKeys. cs) [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Bieżąca godzina i w pamięci podręcznej są wyświetlane:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Buforowana `DateTime` wartość pozostaje w pamięci podręcznej, gdy istnieją żądania w określonym limicie czasu. Na poniższej ilustracji przedstawiono bieżący czas i wcześniejszy czas pobrany z pamięci podręcznej:

![Widok indeksu z dwoma różnymi godzinami wyświetlania](memory/_static/time.png)

Poniższy kod używa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) i [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) do buforowania danych.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Następujący kod wywołuje [pobieranie](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) , aby pobrać buforowany czas:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, i [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) są metodami rozszerzenia części klasy [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) , która rozszerza możliwości programu <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>. Zobacz [metody IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) i [CacheExtensions metody](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) opisujące inne metody pamięci podręcznej.

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

Poniższy przykład:

* Ustawia czas wygaśnięcia. Żądania, które uzyskują dostęp do tego elementu w pamięci podręcznej, spowodują zresetowanie zegara zakończenia przewijania.
* Ustawia priorytet pamięci podręcznej na `CacheItemPriority.NeverRemove`.
* Ustawia [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , który zostanie wywołany po wykluczeniu wpisu z pamięci podręcznej. Wywołanie zwrotne jest uruchamiane w innym wątku niż kod, który usuwa element z pamięci podręcznej.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Użyj SetSize, size i SizeLimit, aby ograniczyć rozmiar pamięci podręcznej

`MemoryCache` Wystąpienie może opcjonalnie określić i wymusić limit rozmiaru. Limit rozmiaru pamięci nie ma zdefiniowanej jednostki miary, ponieważ pamięć podręczna nie ma mechanizmu mierzenia rozmiaru wpisów. Jeśli ustawiono limit rozmiaru pamięci podręcznej, wszystkie wpisy muszą określać rozmiar. Środowisko uruchomieniowe ASP.NET Core nie ogranicza rozmiaru pamięci podręcznej na podstawie nacisku pamięci. Aby ograniczyć rozmiar pamięci podręcznej, należy do dewelopera. Określony rozmiar jest w jednostkach wybranych przez dewelopera.

Przykład:

* Jeśli aplikacja sieci Web była przede wszystkim buforowania ciągów, każdy rozmiar wpisu pamięci podręcznej może być długością ciągu.
* Aplikacja może określić rozmiar wszystkich wpisów jako 1, a limit rozmiaru to liczba wpisów.

Jeśli <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit> nie jest ustawiona, pamięć podręczna rośnie bez powiązania. Środowisko uruchomieniowe ASP.NET Core nie przycina pamięci podręcznej, gdy ilość pamięci systemowej jest niska. Aplikacje są w znacznym stopniu zaprojektowane pod kątem:

* Ogranicz wzrost rozmiaru pamięci podręcznej.
* Połączenie <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> lub<xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*> Jeśli dostępna pamięć jest ograniczona:

Poniższy kod tworzy bezjednostkowy rozmiar <xref:Microsoft.Extensions.Caching.Memory.MemoryCache> dostępny w ramach iniekcji [zależności](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

`SizeLimit`nie ma jednostek. Wpisy w pamięci podręcznej muszą określać rozmiar w jednostkach, które są uważane za najbardziej odpowiednie, jeśli ustawiono rozmiar pamięci podręcznej. Wszyscy użytkownicy wystąpienia pamięci podręcznej powinni używać tego samego systemu jednostek. Wpis nie zostanie zapisany w pamięci podręcznej, jeśli suma rozmiarów buforowanych wpisów przekroczy `SizeLimit`wartość określoną przez. Jeśli limit rozmiaru pamięci podręcznej nie zostanie ustawiony, rozmiar pamięci podręcznej ustawiony na wpis zostanie zignorowany.

Poniższy kod rejestruje `MyMemoryCache` z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) .

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache`jest tworzony jako pamięć podręczna niezależna pamięci dla składników, które są świadome pamięci podręcznej ograniczonej rozmiaru i wiedzą, jak ustawić odpowiednio rozmiar wpisu pamięci podręcznej.

Następujący kod używa `MyMemoryCache`:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

Rozmiar wpisu pamięci podręcznej można ustawić przez [rozmiar](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) lub metodę rozszerzenia [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) :

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a>Elemencie MemoryCache. Compact

`MemoryCache.Compact`próbuje usunąć określony procent pamięci podręcznej w następującej kolejności:

* Wszystkie elementy wygasłe.
* Elementy według priorytetu. Elementy o najniższym priorytecie są usuwane jako pierwsze.
* Ostatnio używane obiekty.
* Elementy z najwcześniejszym bezwzględnym okresem ważności.
* Elementy z najwcześniejszym okresem ważności.

Przypięte elementy <xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove> z priorytetem nigdy nie są usuwane.

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

Aby uzyskać więcej informacji, zobacz artykuł [Compact Source w witrynie GitHub](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393) .

## <a name="cache-dependencies"></a>Zależności pamięci podręcznej

Poniższy przykład pokazuje, jak wygasa wpis pamięci podręcznej, Jeśli wpis zależny wygaśnie. Element `CancellationChangeToken` jest dodawany do elementu w pamięci podręcznej. Gdy `Cancel` jest wywoływana `CancellationTokenSource`w, oba wpisy pamięci podręcznej są wykluczone.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Użycie a `CancellationTokenSource` umożliwia wykluczenie wielu wpisów pamięci podręcznej jako grupy. Ze wzorcem w powyższym kodzie wpisy pamięci podręcznej `using` utworzone wewnątrz bloku będą dziedziczyć wyzwalacze i ustawienia wygasania. `using`

## <a name="additional-notes"></a>Dodatkowe uwagi

* W przypadku użycia wywołania zwrotnego do ponownego wypełnienia elementu pamięci podręcznej:

  * Wiele żądań może znaleźć pustą wartość klucza w pamięci podręcznej, ponieważ wywołanie zwrotne nie zostało zakończone.
  * Może to spowodować, że kilka wątków będzie przepełniać element w pamięci podręcznej.

* Gdy jeden wpis pamięci podręcznej jest używany do utworzenia innego, element podrzędny kopiuje tokeny wygaśnięcia i czas wygaśnięcia na podstawie czasu. Element podrzędny nie wygasł przez ręczne usunięcie lub aktualizację wpisu nadrzędnego.

* Użyj [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) , aby ustawić wywołania zwrotne, które zostaną wyzwolone po usunięciu wpisu pamięci podręcznej z pamięci.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end
