---
title: Buforowanie w pamięci w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak buforować dane w pamięci w ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 8/22/2019
uid: performance/caching/memory
ms.openlocfilehash: d6b2aa363c552fdbda7f6e9ec5d476768c17d8a5
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779192"
---
# <a name="cache-in-memory-in-aspnet-core"></a>Buforowanie w pamięci w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT), [Jan Luo](https://github.com/JunTaoLuo)i [Steve Smith](https://ardalis.com/)

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/3.0sample) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Podstawowe informacje o buforowaniu

Buforowanie może znacząco poprawić wydajność i skalowalność aplikacji przez zmniejszenie ilości pracy wymaganej do wygenerowania zawartości. Buforowanie działa najlepiej w **przypadku rzadko używanych** danych. Buforowanie tworzy kopię danych, która może być zwracana znacznie szybciej niż ze źródła. Aplikacje powinny być zapisane i przetestowane w taki sposób, aby **nigdy nie** zależały od danych buforowanych.

ASP.NET Core obsługuje kilka różnych pamięci podręcznych. Najprostsza pamięć podręczna jest oparta na [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). `IMemoryCache` reprezentuje pamięć podręczną przechowywaną w pamięci serwera sieci Web. Aplikacje działające w farmie serwerów (wiele serwerów) powinny zapewnić, że sesje są w trakcie korzystania z pamięci podręcznej w pamięci. Sesje usługi Sticky Notes zapewniają, że kolejne żądania od klienta będą kierowane do tego samego serwera. Na przykład usługa Azure Web Apps używa [routingu żądań aplikacji](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) do kierowania wszystkich kolejnych żądań do tego samego serwera.

Sesje inne niż nietrwałe w kolektywie serwerów sieci Web wymagają [rozproszonej pamięci podręcznej](distributed.md) , aby uniknąć problemów ze spójnością pamięci W przypadku niektórych aplikacji rozproszonej pamięci podręcznej może obsługiwać większą skalowalność niż pamięć podręczna w pamięci. Użycie rozproszonej pamięci podręcznej powoduje odciążenie pamięci podręcznej do procesu zewnętrznego.

Pamięć podręczna w pamięci może przechowywać dowolny obiekt. Interfejs rozproszonej pamięci podręcznej jest ograniczony do `byte[]`. Magazyn w pamięci i rozproszonej pamięci podręcznej przechowuje elementy pamięci podręcznej jako pary klucz-wartość.

## <a name="systemruntimecachingmemorycache"></a>System. Runtime. buforowanie/elemencie MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([pakiet NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) może być używany z:

* .NET Standard 2,0 lub nowszy.
* Dowolna [implementacja platformy .NET](/dotnet/standard/net-standard#net-implementation-support) , która jest przeznaczona dla .NET Standard 2,0 lub nowszych. Na przykład ASP.NET Core 2,0 lub nowszy.
* .NET Framework 4,5 lub nowszy.

[Firma Microsoft. Extensions. buforowanie. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (opisany w tym artykule) jest zalecana w porównaniu `System.Runtime.Caching`/`MemoryCache`, ponieważ jest lepiej zintegrowana z ASP.NET Core. Na przykład `IMemoryCache` działa natywnie z [iniekcją ASP.NET Core zależności](xref:fundamentals/dependency-injection).

Użyj `System.Runtime.Caching`/`MemoryCache` jako mostka zgodności podczas przenoszenia kodu z ASP.NET 4. x do ASP.NET Core.

## <a name="cache-guidelines"></a>Wskazówki dotyczące pamięci podręcznej

* Kod powinien zawsze mieć opcję rezerwową, aby pobrać dane i **nie** zależał od dostępnej wartości w pamięci podręcznej.
* Pamięć podręczna używa nieodpowiedniego zasobu, pamięci. Ogranicz wzrost rozmiaru pamięci podręcznej:
  * **Nie** należy używać zewnętrznych danych wejściowych jako kluczy pamięci podręcznej.
  * Użyj wygaśnięć, aby ograniczyć wzrost rozmiaru pamięci podręcznej.
  * [Użyj SetSize, size i SizeLimit, aby ograniczyć rozmiar pamięci podręcznej](#use-setsize-size-and-sizelimit-to-limit-cache-size). Środowisko uruchomieniowe ASP.NET Core **nie ogranicza rozmiaru** pamięci podręcznej na podstawie nacisku pamięci. Aby ograniczyć rozmiar pamięci podręcznej, należy do dewelopera.

## <a name="use-imemorycache"></a>Użyj IMemoryCache

> [!WARNING]
> Użycie pamięci podręcznej pamięci *współużytkowanej* przed [iniekcją zależności](xref:fundamentals/dependency-injection) i wywołaniem `SetSize`, `Size` lub `SizeLimit` w celu ograniczenia rozmiaru pamięci podręcznej może spowodować niepowodzenie aplikacji. Po ustawieniu limitu rozmiaru w pamięci podręcznej, wszystkie wpisy muszą określać rozmiar podczas dodawania. Może to prowadzić do problemów, ponieważ deweloperzy mogą nie mieć pełnej kontroli nad używaniem udostępnionej pamięci podręcznej. Na przykład Entity Framework Core używa udostępnionej pamięci podręcznej i nie określa rozmiaru. Jeśli aplikacja ustawi limit rozmiaru pamięci podręcznej i używa EF Core, aplikacja zgłasza `InvalidOperationException`.
> W przypadku używania `SetSize`, `Size` lub `SizeLimit` do ograniczania pamięci podręcznej należy utworzyć pojedynczą pamięć podręczną dla buforowania. Aby uzyskać więcej informacji i zapoznać się z przykładem, zobacz [Używanie SetSize, size i SizeLimit w celu ograniczenia rozmiaru pamięci podręcznej](#use-setsize-size-and-sizelimit-to-limit-cache-size).
> Udostępniona pamięć podręczna jest współdzielona przez inne struktury lub biblioteki. Na przykład EF Core używa udostępnionej pamięci podręcznej i nie określa rozmiaru. 

Buforowanie w pamięci to *Usługa* , do której odwołuje się aplikacja przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection). Zażądaj wystąpienia `IMemoryCache` w konstruktorze:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ctor)]

Poniższy kod używa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) aby sprawdzić, czy czas znajduje się w pamięci podręcznej. Jeśli czas nie jest buforowany, nowy wpis zostanie utworzony i dodany do pamięci podręcznej z [zestawem](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_). Klasa `CacheKeys` jest częścią przykładu pobierania.

[!code-csharp[](memory/3.0sample/WebCacheSample/CacheKeys.cs)]

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet1)]

Bieżąca godzina i w pamięci podręcznej są wyświetlane:

[!code-cshtml[](memory/3.0sample/WebCacheSample/Views/Home/Cache.cshtml)]

Wartość buforowanego `DateTime` pozostaje w pamięci podręcznej, gdy istnieją żądania w określonym limicie czasu.

Poniższy kod używa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) i [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) do buforowania danych.

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Następujący kod wywołuje [pobieranie](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) , aby pobrać buforowany czas:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_gct)]

Poniższy kod pobiera lub tworzy buforowany element z bezwzględnym wygaśnięciem:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet99)]

W pamięci podręcznej zestaw elementów z przewijanym okresem ważności jest zagrożony przestarzałą. Jeśli dostęp do niego jest możliwy częściej niż przedział czasu wygaśnięcia, element nigdy nie wygaśnie. Połącz okresowe wygaśnięcie i bezwzględne wygaśnięcie w celu zagwarantowania, że element wygaśnie po upływie bezwzględnego czasu wygaśnięcia. Bezwzględne wygaśnięcie Ustawia górną granicę, jak długo element może być buforowany, przy jednoczesnym dopuszczeniu do wcześniejszego wygaśnięcia elementu, jeśli nie zostanie on żądany w przedziale czasu wygaśnięcia. Gdy są określone okresy ważności bezwzględne i przesuwania, wygasające są logiczne logicznie. Jeśli przedział czasu wygaśnięcia *lub* bezwzględny czas wygaśnięcia, element zostanie wykluczony z pamięci podręcznej.

Poniższy kod pobiera lub tworzy buforowany element z przewinięciem *i* bezwzględnym okresem ważności:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet9)]

Poprzedni kod gwarantuje, że dane nie będą przechowywane w pamięci podręcznej dłużej niż czas bezwzględny.

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*> i <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.Get*> to metody rozszerzające w klasie <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions>. Te metody zwiększają możliwości <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

Poniższy przykład:

* Ustawia czas wygaśnięcia. Żądania, które uzyskują dostęp do tego elementu w pamięci podręcznej, spowodują zresetowanie zegara zakończenia przewijania.
* Ustawia priorytet pamięci podręcznej na [CacheItemPriority. NeverRemove](xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove).
* Ustawia [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , który zostanie wywołany po wykluczeniu wpisu z pamięci podręcznej. Wywołanie zwrotne jest uruchamiane w innym wątku niż kod, który usuwa element z pamięci podręcznej.

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Użyj SetSize, size i SizeLimit, aby ograniczyć rozmiar pamięci podręcznej

Wystąpienie `MemoryCache` może opcjonalnie określić i wymusić limit rozmiaru. Limit rozmiaru pamięci podręcznej nie ma zdefiniowanej jednostki miary, ponieważ pamięć podręczna nie ma mechanizmu mierzenia rozmiaru wpisów. Jeśli ustawiono limit rozmiaru pamięci podręcznej, wszystkie wpisy muszą określać rozmiar. Środowisko uruchomieniowe ASP.NET Core nie ogranicza rozmiaru pamięci podręcznej na podstawie nacisku pamięci. Aby ograniczyć rozmiar pamięci podręcznej, należy do dewelopera. Określony rozmiar jest w jednostkach wybranych przez dewelopera.

Na przykład:

* Jeśli aplikacja sieci Web była przede wszystkim buforowania ciągów, każdy rozmiar wpisu pamięci podręcznej może być długością ciągu.
* Aplikacja może określić rozmiar wszystkich wpisów jako 1, a limit rozmiaru to liczba wpisów.

Jeśli nie ustawiono <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit>, pamięć podręczna rośnie bez powiązanych. Środowisko uruchomieniowe ASP.NET Core nie przycina pamięci podręcznej, gdy ilość pamięci systemowej jest niska. Aplikacje są w znacznym stopniu zaprojektowane pod kątem:

* Ogranicz wzrost rozmiaru pamięci podręcznej.
* Wywołaj <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> lub <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*>, jeśli ilość dostępnej pamięci jest ograniczona:

Poniższy kod tworzy bezjednostkowy rozmiar stały <xref:Microsoft.Extensions.Caching.Memory.MemoryCache> dostępne przez [iniekcję zależności](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

`SizeLimit` nie ma jednostek. Wpisy w pamięci podręcznej muszą określać rozmiar w jednostkach, które są uważane za najbardziej odpowiednie, jeśli ustawiono limit rozmiaru pamięci podręcznej. Wszyscy użytkownicy wystąpienia pamięci podręcznej powinni używać tego samego systemu jednostek. Wpis nie zostanie zapisany w pamięci podręcznej, jeśli suma rozmiarów buforowanych wpisów przekroczy wartość określoną przez `SizeLimit`. Jeśli limit rozmiaru pamięci podręcznej nie zostanie ustawiony, rozmiar pamięci podręcznej ustawiony na wpis zostanie zignorowany.

Poniższy kod rejestruje `MyMemoryCache` z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) .

[!code-csharp[](memory/3.0sample/RPcache/Startup.cs?name=snippet)]

`MyMemoryCache` jest tworzony jako pamięć podręczna niezależna pamięci dla składników, które są świadome pamięci podręcznej ograniczonej rozmiaru i wiedzą, jak ustawić odpowiednio rozmiar wpisu pamięci podręcznej.

Poniższy kod używa `MyMemoryCache`:

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet)]

Rozmiar wpisu pamięci podręcznej można ustawić przez <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.Size> lub metody rozszerzenia <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.SetSize*>:

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a>Elemencie MemoryCache. Compact

`MemoryCache.Compact` próbuje usunąć określony procent pamięci podręcznej w następującej kolejności:

* Wszystkie elementy wygasłe.
* Elementy według priorytetu. Elementy o najniższym priorytecie są usuwane jako pierwsze.
* Ostatnio używane obiekty.
* Elementy z najwcześniejszym bezwzględnym okresem ważności.
* Elementy z najwcześniejszym okresem ważności.

Elementy przypięte o priorytecie <xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove> nigdy nie są usuwane. Poniższy kod usuwa element pamięci podręcznej i wywołuje `Compact`:

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

Aby uzyskać więcej informacji, zobacz artykuł [Compact Source w witrynie GitHub](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393) .

## <a name="cache-dependencies"></a>Zależności pamięci podręcznej

Poniższy przykład pokazuje, jak wygasa wpis pamięci podręcznej, Jeśli wpis zależny wygaśnie. `CancellationChangeToken` zostanie dodany do elementu w pamięci podręcznej. Gdy `Cancel` jest wywoływana dla `CancellationTokenSource`, oba wpisy pamięci podręcznej są wykluczone.

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ed)]

Użycie `CancellationTokenSource` umożliwia wykluczenie wielu wpisów pamięci podręcznej jako grupy. Ze wzorcem `using` w powyższym kodzie, wpisy pamięci podręcznej utworzone w bloku `using` będą dziedziczyć wyzwalacze i ustawienia wygasania.

## <a name="additional-notes"></a>Dodatkowe uwagi

* Wygaśnięcie nie odbywa się w tle. Nie ma czasomierza, który aktywnie skanuje pamięć podręczną pod kątem wygasłych elementów. Wszystkie działania w pamięci podręcznej (`Get`, `Set`, `Remove`) mogą wyzwalać skanowanie w tle dla elementów, które utraciły ważność. Czasomierz na `CancellationTokenSource` (`CancelAfter`) również usunie wpis i wyzwolenie skanowania dla wygasłych elementów. Na przykład zamiast używać `SetAbsoluteExpiration(TimeSpan.FromHours(1))`, użyj `CancellationTokenSource.CancelAfter(TimeSpan.FromHours(1))` dla zarejestrowanego tokenu. Gdy ten token wyzwala, usuwa wpis natychmiast i wyzwala wywołania zwrotne wykluczenia. Aby uzyskać więcej informacji, zobacz [ten problem](https://github.com/aspnet/Caching/issues/248)w serwisie GitHub.
* W przypadku użycia wywołania zwrotnego do ponownego wypełnienia elementu pamięci podręcznej:

  * Wiele żądań może znaleźć pustą wartość klucza w pamięci podręcznej, ponieważ wywołanie zwrotne nie zostało zakończone.
  * Może to spowodować, że kilka wątków będzie przepełniać element w pamięci podręcznej.

* Gdy jeden wpis pamięci podręcznej jest używany do utworzenia innego, element podrzędny kopiuje tokeny wygaśnięcia i czas wygaśnięcia na podstawie czasu. Element podrzędny nie wygasł przez ręczne usunięcie lub aktualizację wpisu nadrzędnego.

* Użyj <xref:Microsoft.Extensions.Caching.Memory.ICacheEntry.PostEvictionCallbacks>, aby ustawić wywołania zwrotne, które będą wyzwalane po wykluczeniu wpisu pamięci podręcznej z tej pamięci.
* W przypadku większości aplikacji jest włączone `IMemoryCache`. Na przykład wywoływanie `AddMvc`, `AddControllersWithViews`, `AddRazorPages`, `AddMvcCore().AddRazorViewEngine` i wielu innych metod `Add{Service}` w `ConfigureServices` zezwala na `IMemoryCache`. W przypadku aplikacji, które nie wywołujący jednej z powyższych metod `Add{Service}`, może być konieczne wywołanie <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddMemoryCache*> w `ConfigureServices`.

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

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Podstawowe informacje o buforowaniu

Buforowanie może znacząco poprawić wydajność i skalowalność aplikacji przez zmniejszenie ilości pracy wymaganej do wygenerowania zawartości. Buforowanie działa najlepiej w przypadku rzadko używanych danych. Buforowanie tworzy kopię danych, która może być zwracana znacznie szybciej niż z oryginalnego źródła. Kod powinien być zapisany i przetestowany w taki sposób, aby **nigdy nie** zależał od danych buforowanych.

ASP.NET Core obsługuje kilka różnych pamięci podręcznych. Najprostsza pamięć podręczna jest oparta na [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), która reprezentuje pamięć podręczną przechowywaną w pamięci serwera sieci Web. Aplikacje działające w farmie serwerów (wiele serwerów) powinny mieć pewność, że sesje są w trakcie korzystania z pamięci podręcznej w pamięci. Sesje usługi Sticky Notes zapewniają, że późniejsze żądania od klienta będą kierowane do tego samego serwera. Na przykład usługa Azure Web Apps używa [routingu żądań aplikacji](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) do kierowania wszystkich żądań od agenta użytkownika na ten sam serwer.

Sesje inne niż nietrwałe w kolektywie serwerów sieci Web wymagają [rozproszonej pamięci podręcznej](distributed.md) , aby uniknąć problemów ze spójnością pamięci W przypadku niektórych aplikacji rozproszonej pamięci podręcznej może obsługiwać większą skalowalność niż pamięć podręczna w pamięci. Użycie rozproszonej pamięci podręcznej powoduje odciążenie pamięci podręcznej do procesu zewnętrznego.

Pamięć podręczna w pamięci może przechowywać dowolny obiekt. Interfejs rozproszonej pamięci podręcznej jest ograniczony do `byte[]`. Magazyn w pamięci i rozproszonej pamięci podręcznej przechowuje elementy pamięci podręcznej jako pary klucz-wartość.

## <a name="systemruntimecachingmemorycache"></a>System. Runtime. buforowanie/elemencie MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([pakiet NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) może być używany z:

* .NET Standard 2,0 lub nowszy.
* Dowolna [implementacja platformy .NET](/dotnet/standard/net-standard#net-implementation-support) , która jest przeznaczona dla .NET Standard 2,0 lub nowszych. Na przykład ASP.NET Core 2,0 lub nowszy.
* .NET Framework 4,5 lub nowszy.

[Firma Microsoft. Extensions. buforowanie. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (opisany w tym artykule) jest zalecana w porównaniu `System.Runtime.Caching`/`MemoryCache`, ponieważ jest lepiej zintegrowana z ASP.NET Core. Na przykład `IMemoryCache` działa natywnie z [iniekcją ASP.NET Core zależności](xref:fundamentals/dependency-injection).

Użyj `System.Runtime.Caching`/`MemoryCache` jako mostka zgodności podczas przenoszenia kodu z ASP.NET 4. x do ASP.NET Core.

## <a name="cache-guidelines"></a>Wskazówki dotyczące pamięci podręcznej

* Kod powinien zawsze mieć opcję rezerwową, aby pobrać dane i **nie** zależał od dostępnej wartości w pamięci podręcznej.
* Pamięć podręczna używa nieodpowiedniego zasobu, pamięci. Ogranicz wzrost rozmiaru pamięci podręcznej:
  * **Nie** należy używać zewnętrznych danych wejściowych jako kluczy pamięci podręcznej.
  * Użyj wygaśnięć, aby ograniczyć wzrost rozmiaru pamięci podręcznej.
  * [Użyj SetSize, size i SizeLimit, aby ograniczyć rozmiar pamięci podręcznej](#use-setsize-size-and-sizelimit-to-limit-cache-size). Środowisko uruchomieniowe ASP.NET Core nie ogranicza rozmiaru pamięci podręcznej na podstawie nacisku pamięci. Aby ograniczyć rozmiar pamięci podręcznej, należy do dewelopera.

## <a name="using-imemorycache"></a>Korzystanie z IMemoryCache

> [!WARNING]
> Użycie pamięci podręcznej pamięci *współużytkowanej* przed [iniekcją zależności](xref:fundamentals/dependency-injection) i wywołaniem `SetSize`, `Size` lub `SizeLimit` w celu ograniczenia rozmiaru pamięci podręcznej może spowodować niepowodzenie aplikacji. Po ustawieniu limitu rozmiaru w pamięci podręcznej, wszystkie wpisy muszą określać rozmiar podczas dodawania. Może to prowadzić do problemów, ponieważ deweloperzy mogą nie mieć pełnej kontroli nad używaniem udostępnionej pamięci podręcznej. Na przykład Entity Framework Core używa udostępnionej pamięci podręcznej i nie określa rozmiaru. Jeśli aplikacja ustawi limit rozmiaru pamięci podręcznej i używa EF Core, aplikacja zgłasza `InvalidOperationException`.
> W przypadku używania `SetSize`, `Size` lub `SizeLimit` do ograniczania pamięci podręcznej należy utworzyć pojedynczą pamięć podręczną dla buforowania. Aby uzyskać więcej informacji i zapoznać się z przykładem, zobacz [Używanie SetSize, size i SizeLimit w celu ograniczenia rozmiaru pamięci podręcznej](#use-setsize-size-and-sizelimit-to-limit-cache-size).

Buforowanie w pamięci to *Usługa* , do której odwołuje się aplikacja przy użyciu [iniekcji zależności](../../fundamentals/dependency-injection.md). Wywołanie `AddMemoryCache` w `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Zażądaj wystąpienia `IMemoryCache` w konstruktorze:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

`IMemoryCache` wymaga pakietu NuGet [Microsoft. Extensions. buforowanie. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), który jest dostępny w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

Poniższy kod używa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) aby sprawdzić, czy czas znajduje się w pamięci podręcznej. Jeśli czas nie jest buforowany, nowy wpis zostanie utworzony i dodany do pamięci podręcznej z [zestawem](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp[](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Bieżąca godzina i w pamięci podręcznej są wyświetlane:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Wartość buforowanego `DateTime` pozostaje w pamięci podręcznej, gdy istnieją żądania w określonym limicie czasu. Na poniższej ilustracji przedstawiono bieżący czas i wcześniejszy czas pobrany z pamięci podręcznej:

![Widok indeksu z dwoma różnymi godzinami wyświetlania](memory/_static/time.png)

Poniższy kod używa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) i [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) do buforowania danych.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Następujący kod wywołuje [pobieranie](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) , aby pobrać buforowany czas:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*> i [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) są metodami rozszerzenia części klasy [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) , która rozszerza możliwości <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>. Zobacz [metody IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) i [CacheExtensions metody](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) opisujące inne metody pamięci podręcznej.

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

Poniższy przykład:

* Ustawia czas wygaśnięcia. Żądania, które uzyskują dostęp do tego elementu w pamięci podręcznej, spowodują zresetowanie zegara zakończenia przewijania.
* Ustawia priorytet pamięci podręcznej na `CacheItemPriority.NeverRemove`.
* Ustawia [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , który zostanie wywołany po wykluczeniu wpisu z pamięci podręcznej. Wywołanie zwrotne jest uruchamiane w innym wątku niż kod, który usuwa element z pamięci podręcznej.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Użyj SetSize, size i SizeLimit, aby ograniczyć rozmiar pamięci podręcznej

Wystąpienie `MemoryCache` może opcjonalnie określić i wymusić limit rozmiaru. Limit rozmiaru pamięci podręcznej nie ma zdefiniowanej jednostki miary, ponieważ pamięć podręczna nie ma mechanizmu mierzenia rozmiaru wpisów. Jeśli ustawiono limit rozmiaru pamięci podręcznej, wszystkie wpisy muszą określać rozmiar. Środowisko uruchomieniowe ASP.NET Core nie ogranicza rozmiaru pamięci podręcznej na podstawie nacisku pamięci. Aby ograniczyć rozmiar pamięci podręcznej, należy do dewelopera. Określony rozmiar jest w jednostkach wybranych przez dewelopera.

Na przykład:

* Jeśli aplikacja sieci Web była przede wszystkim buforowania ciągów, każdy rozmiar wpisu pamięci podręcznej może być długością ciągu.
* Aplikacja może określić rozmiar wszystkich wpisów jako 1, a limit rozmiaru to liczba wpisów.

Jeśli nie ustawiono <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit>, pamięć podręczna rośnie bez powiązanych. Środowisko uruchomieniowe ASP.NET Core nie przycina pamięci podręcznej, gdy ilość pamięci systemowej jest niska. Aplikacje są w znacznym stopniu zaprojektowane pod kątem:

* Ogranicz wzrost rozmiaru pamięci podręcznej.
* Wywołaj <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> lub <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*>, jeśli ilość dostępnej pamięci jest ograniczona:

Poniższy kod tworzy bezjednostkowy rozmiar stały <xref:Microsoft.Extensions.Caching.Memory.MemoryCache> dostępne przez [iniekcję zależności](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

`SizeLimit` nie ma jednostek. Wpisy w pamięci podręcznej muszą określać rozmiar w jednostkach, które są uważane za najbardziej odpowiednie, jeśli ustawiono limit rozmiaru pamięci podręcznej. Wszyscy użytkownicy wystąpienia pamięci podręcznej powinni używać tego samego systemu jednostek. Wpis nie zostanie zapisany w pamięci podręcznej, jeśli suma rozmiarów buforowanych wpisów przekroczy wartość określoną przez `SizeLimit`. Jeśli limit rozmiaru pamięci podręcznej nie zostanie ustawiony, rozmiar pamięci podręcznej ustawiony na wpis zostanie zignorowany.

Poniższy kod rejestruje `MyMemoryCache` z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) .

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache` jest tworzony jako pamięć podręczna niezależna pamięci dla składników, które są świadome pamięci podręcznej ograniczonej rozmiaru i wiedzą, jak ustawić odpowiednio rozmiar wpisu pamięci podręcznej.

Poniższy kod używa `MyMemoryCache`:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

Rozmiar wpisu pamięci podręcznej można ustawić przez [rozmiar](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) lub metodę rozszerzenia [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) :

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a>Elemencie MemoryCache. Compact

`MemoryCache.Compact` próbuje usunąć określony procent pamięci podręcznej w następującej kolejności:

* Wszystkie elementy wygasłe.
* Elementy według priorytetu. Elementy o najniższym priorytecie są usuwane jako pierwsze.
* Ostatnio używane obiekty.
* Elementy z najwcześniejszym bezwzględnym okresem ważności.
* Elementy z najwcześniejszym okresem ważności.

Elementy przypięte o priorytecie <xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove> nigdy nie są usuwane.

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

Aby uzyskać więcej informacji, zobacz artykuł [Compact Source w witrynie GitHub](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393) .

## <a name="cache-dependencies"></a>Zależności pamięci podręcznej

Poniższy przykład pokazuje, jak wygasa wpis pamięci podręcznej, Jeśli wpis zależny wygaśnie. `CancellationChangeToken` zostanie dodany do elementu w pamięci podręcznej. Gdy `Cancel` jest wywoływana dla `CancellationTokenSource`, oba wpisy pamięci podręcznej są wykluczone.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Użycie `CancellationTokenSource` umożliwia wykluczenie wielu wpisów pamięci podręcznej jako grupy. Ze wzorcem `using` w powyższym kodzie, wpisy pamięci podręcznej utworzone w bloku `using` będą dziedziczyć wyzwalacze i ustawienia wygasania.

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
