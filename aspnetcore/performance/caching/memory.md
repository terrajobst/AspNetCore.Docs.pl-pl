---
title: "Buforowanie w pamięci w ASP.NET Core"
author: rick-anderson
description: "Dowiedz się, jak dane w pamięci w ASP.NET Core z pamięci podręcznej."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: 4219cae4e3d3f9d15afe6725b21cc8966979d95c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="in-memory-caching-in-aspnet-core"></a>Buforowanie w pamięci w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Luo Jan](https://github.com/JunTaoLuo), i [Steve Smith](https://ardalis.com/)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>Buforowanie podstawy

Buforowanie może znacznie poprawić wydajność i skalowalność aplikacji dzięki zmniejszeniu pracy wymaganej do generowania zawartości. Buforowanie działa najlepiej z danymi, które zmieniają się rzadko. Buforowanie tworzy kopię danych, które mogą być zwrócone znacznie szybciej niż z oryginalnego źródła. Należy zapisać i przetestować aplikację, aby nigdy nie są zależne od danych z pamięci podręcznej.

Platformy ASP.NET Core obsługuje kilka różnych pamięci podręcznych. Najprostsza pamięci podręcznej jest oparta na [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache), który reprezentuje pamięć podręczną przechowywane w pamięci serwera sieci web. Aplikacje działające w farmie serwerów wielu serwerów należy upewnij się, sesje są trwałe, korzystając z pamięci podręcznej. Trwałe sesje upewnij się, że kolejne żądania w kliencie wszystkich przejść do tego samego serwera. Na przykład użycia aplikacji sieci Web Azure [Routing żądań aplikacji](https://www.iis.net/learn/extensions/planning-for-arr) (ARR), aby przekierować wszystkie kolejne żądania do tego samego serwera.

Inne niż trwałe sesje w kolektywie serwerów sieci web wymagają [rozproszonej pamięci podręcznej](distributed.md) Aby uniknąć problemów spójności pamięci podręcznej. W przypadku niektórych aplikacji rozproszonej pamięci podręcznej może obsługiwać wyższe skalowania w poziomie niż w pamięci podręcznej. Przy użyciu rozproszonej pamięci podręcznej odciąża pamięci podręcznej procesu zewnętrznego. 

`IMemoryCache` Pamięci podręcznej Wyklucz wpisy w pamięci podręcznej wykorzystanie pamięci, chyba że [pamięci podręcznej priorytet](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority) ma ustawioną wartość `CacheItemPriority.NeverRemove`. Można ustawić `CacheItemPriority` aby dopasować priorytet pamięci podręcznej wyklucza mogą elementów wykorzystanie pamięci.

Dowolny obiekt; mogą być przechowywane w pamięci podręcznej Interfejs rozproszonej pamięci podręcznej jest ograniczona do `byte[]`.

## <a name="using-imemorycache"></a>Przy użyciu IMemoryCache

Buforowanie w pamięci jest *usługi* który jest wywoływany przez przy użyciu aplikacji [iniekcji zależności](../../fundamentals/dependency-injection.md). Wywołanie `AddMemoryCache` w `ConfigureServices`:

[!code-csharp[Main](memory/sample/WebCache/Startup.cs?highlight=8)] 

Żądanie `IMemoryCache` wystąpienia w Konstruktorze:

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-)] 

`IMemoryCache`wymaga pakietu NuGet "Microsoft.Extensions.Caching.Memory".

Poniższy kod używa [TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) do Sprawdź, czy bieżący czas jest w pamięci podręcznej. Jeśli element nie są buforowane, nowy wpis jest tworzony i dodawany do pamięci podręcznej z [ustawić](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_).

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Wyświetlany jest bieżący czas i czas pamięci podręcznej:

[!code-html[Main](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Zapisane w pamięci podręcznej `DateTime` wartość zostanie umieszczony w pamięci podręcznej podczas żądania znajduje się w przedziale czasu (i nie wykluczenia z powodu braku pamięci). Na poniższym obrazie przedstawiono bieżący czas i czas starsze pobrać z pamięci podręcznej:

![Widok indeksu z dwóch różnych porach wyświetlane](memory/_static/time.png)

Poniższy kod używa [GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) i [GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) do pamięci podręcznej danych. 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Poniższy kod wywołania [uzyskać](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) można pobrać czasu pamięci podręcznej:

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

Zobacz [metody IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache) i [metody CacheExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions) opis metody pamięci podręcznej.

## <a name="using-memorycacheentryoptions"></a>Using MemoryCacheEntryOptions

Poniższy przykład:

- Ustawia czas wygaśnięcia bezwzględne. To jest maksymalny czas, które mogą być buforowane wpis i uniemożliwia stają się zbyt stare po odnowieniu stale wygaśniecie elementu.
- Ustawia zmienną czas wygaśnięcia. Żądania, które uzyskują dostęp do tego elementu pamięci podręcznej spowoduje zresetowanie przesuwanego zegara wygaśnięcia.
- Ustawia priorytet pamięci podręcznej na `CacheItemPriority.NeverRemove`. 
- Ustawia [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) która będzie wywoływana po wykonaniu wpis zostanie usunięty z pamięci podręcznej. Wywołanie zwrotne jest uruchamiane w innym wątku z kodu, który usuwa element z pamięci podręcznej.

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a>Zależności pamięci podręcznej

Poniższy przykład przedstawia sposób wygaśnięcia wpisu pamięci podręcznej po przekroczeniu pozycji zależności. A `CancellationChangeToken` zostanie dodany do elementu pamięci podręcznej. Gdy `Cancel` jest wywoływana na `CancellationTokenSource`, zarówno wpisy w pamięci podręcznej jest wykluczony. 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Przy użyciu `CancellationTokenSource` zezwala na wiele wpisów pamięci podręcznej wykluczenie jako grupa. Z `using` wzorca w powyższym kodzie, tworzyć wpisy w pamięci podręcznej `using` bloku odziedziczy ustawienia wygaśnięcia i wyzwalaczy.

## <a name="additional-notes"></a>Dodatkowe uwagi

- Korzystając z wywołania zwrotnego do wypełnienia elementu pamięci podręcznej:

  - Wiele żądań można znaleźć wartość klucza w pamięci podręcznej pusty ponieważ wywołania zwrotnego nie zostało ukończone. 
  - Może to spowodować kilka wątków ponownego uzupełnienia pamięci podręcznej elementu.

- Jeden wpis pamięci podręcznej jest używany do tworzenia drugiego, podrzędne kopiuje wpis nadrzędny tokeny wygaśnięcia i ustawienia na podstawie czasu wygaśnięcia. Obiekt podrzędny nie jest wygasłe przez ręczne usunięcie lub aktualizowania wprowadzania nadrzędnej.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Praca z rozproszonej pamięci podręcznej](xref:performance/caching/distributed)
* [Wykrywanie zmian z tokenami zmiany](xref:fundamentals/primitives/change-tokens)
* [Buforowanie odpowiedzi](xref:performance/caching/response)
* [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware)
* [Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Pomocnik tagu rozproszonej pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
