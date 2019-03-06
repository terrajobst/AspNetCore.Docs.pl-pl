---
title: Buforowanie odpowiedzi w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać odpowiedzi z pamięci podręcznej w celu niższymi wymaganiami co do przepustowości i zwiększenie wydajności aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/28/2019
uid: performance/caching/response
ms.openlocfilehash: efcf443b1487827fe6cf4d43b6dda69adf4d61fb
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345749"
---
# <a name="response-caching-in-aspnet-core"></a>Buforowanie odpowiedzi w programie ASP.NET Core

Przez [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), i [Luke Latham](https://github.com/guardrex)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Buforowanie odpowiedzi zmniejsza liczbę żądań, które sprawia, że klient lub serwer proxy na serwerze sieci web. Buforowanie odpowiedzi zmniejsza ilość pracy serwera sieci web wykonuje do generowania odpowiedzi. Buforowanie odpowiedzi jest kontrolowana przez nagłówki, które określają, jak chcesz klienta, serwera proxy i oprogramowaniu pośredniczącym, aby buforowanie odpowiedzi.

[Atrybut ResponseCache](#responsecache-attribute) uczestniczy podczas ustawiania nagłówków, których klienci mogą przestrzegać podczas buforowania odpowiedzi buforowanie odpowiedzi. [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) może służyć do odpowiedzi z pamięci podręcznej na serwerze. Można użyć oprogramowania pośredniczącego <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> właściwości w celu wywierania wpływu na zachowanie buforowania po stronie serwera.

## <a name="http-based-response-caching"></a>Buforowanie odpowiedzi oparte na protokole HTTP

[Specyfikacji protokołu HTTP 1.1 buforowania](https://tools.ietf.org/html/rfc7234) opisuje zachowanie pamięci podręcznych Internet. Jest podstawowym nagłówek HTTP używana do buforowania [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), który jest używany do określenia pamięci podręcznej *dyrektywy*. Dyrektywy Sterowanie zachowaniem buforowania żądań przedostają się od klientów do serwerów i jak odpowiedzi przedostają się z serwerów z powrotem do klientów. Żądania i odpowiedzi, przenoszenie za pośrednictwem serwerów proxy i serwery proxy również musi być zgodna ze specyfikacją protokołu HTTP 1.1 buforowania.

Typowe `Cache-Control` dyrektywy są wyświetlane w poniższej tabeli.

| — Dyrektywa                                                       | Akcja |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Pamięć podręczna może przechowywać odpowiedzi. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | Odpowiedź nie mogą być przechowywane w udostępnionej pamięci podręcznej. Prywatnej pamięci podręcznej może przechowywać i ponowne użycie odpowiedzi. |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | Klient nie akceptuje odpowiedzi, którego okres ważności jest większa niż określoną liczbę sekund. Przykłady: `max-age=60` (60 sekund), `max-age=2592000` (1 miesiąc) |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **W odpowiedzi na żądania**: Pamięć podręczna nie może używać odpowiedzi przechowywane do spełnienia żądania. Serwer pochodzenia generuje odpowiedzi do klienta, a oprogramowanie pośredniczące aktualizuje odpowiedzi przechowywane w pamięci podręcznej.<br><br>**W odpowiedzi**: Odpowiedź nie mogą być używane dla kolejnych żądań bez sprawdzania poprawności na serwerze źródłowym. |
| [no-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **W odpowiedzi na żądania**: Pamięć podręczna nie musi przechowywać żądania.<br><br>**W odpowiedzi**: Pamięć podręczna nie musi przechowywać dowolną część odpowiedzi. |

W poniższej tabeli przedstawiono innych nagłówków pamięci podręcznej, które mają znaczenie w pamięci podręcznej.

| nagłówek                                                     | Funkcja |
| ---------------------------------------------------------- | -------- |
| [Wiek](https://tools.ietf.org/html/rfc7234#section-5.1)     | Szacowana ilość czasu w sekundach, ponieważ odpowiedzi został wygenerowany pomyślnie zweryfikowane na serwerze źródłowym. |
| [Expires](https://tools.ietf.org/html/rfc7234#section-5.3) | Czas, po czym odpowiedź jest uznawany za przestarzałe. |
| [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Istnieje wstecznej zgodności przy użyciu protokołu HTTP/1.0 buforuje ustawienia `no-cache` zachowanie. Jeśli `Cache-Control` nagłówek jest obecny, `Pragma` nagłówka zostanie zignorowany. |
| [różnią się](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Określa, czy odpowiedzi z pamięci podręcznej musi nie zostać wysłane dopóki wszystkie z `Vary` pola nagłówka są takie same zarówno buforowaną odpowiedź oryginalne żądanie, jak i nowe żądanie. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>Oparte na protokole HTTP względem buforowania żądań dyrektyw sterujących pamięcią podręczną

[Specyfikacji protokołu HTTP 1.1 buforowania dla nagłówka Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) wymaga pamięci podręcznej respektować prawidłową `Cache-Control` nagłówka wysłane przez klienta. Klientowi mogą wysyłać żądania z `no-cache` wartość nagłówka i Wymuś serwera, aby wygenerować nową odpowiedź dla każdego żądania.

Zawsze zapewniane klienta `Cache-Control` nagłówków żądań ma sens należy wziąć pod uwagę w celu buforowania HTTP. W ramach specyfikacji oficjalne buforowania mają na celu obniżenie kosztów opóźnienia oraz sieci spełnienia żądania za pośrednictwem sieci klientów, serwery proxy i serwerów. Nie jest koniecznie sposób kontroluje obciążenie serwera pochodzenia.

Istnieje nie deweloperowi kontroli nad tym zachowaniem buforowania przy użyciu [oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) ponieważ oprogramowanie pośredniczące działa zgodnie z oficjalną buforowania specyfikacji. [Planowane ulepszenia oprogramowanie pośredniczące](https://github.com/aspnet/AspNetCore/issues/2612) jest możliwość konfiguracji oprogramowania pośredniczącego, aby zignorować żądania `Cache-Control` nagłówka podczas podejmowania decyzji o do obsługi buforowanych odpowiedzi. Ulepszenia planowane udostępniają możliwość lepszej obciążenie serwera kontroli.

## <a name="other-caching-technology-in-aspnet-core"></a>Innych technologii buforowania w programie ASP.NET Core

### <a name="in-memory-caching"></a>Buforowanie w pamięci

Wewnątrzpamięciowa pamięć podręczna używa pamięci serwera do przechowywania danych w pamięci podręcznej. Ten rodzaj buforowania nadaje się do pojedynczego serwera lub kilku serwerach wykorzystujących *trwałych sesji*. Trwałych sesji oznacza, że żądania klienta są zawsze kierowane do tego samego serwera w celu przetworzenia.

Aby uzyskać więcej informacji, zobacz <xref:performance/caching/memory>.

### <a name="distributed-cache"></a>Rozproszona pamięć podręczna

Umożliwia przechowywanie danych w pamięci, gdy aplikacja jest hostowana w chmurze i na serwerze farmie rozproszonej pamięci podręcznej. Pamięć podręczna jest współużytkowany przez serwery, które przetwarzają żądania. Klienci mogą przesyłać żądania, które jest obsługiwane przez dowolnego serwera w grupie, jeśli dane w pamięci podręcznej klienta jest dostępny. ASP.NET Core oferuje program SQL Server i pamięci podręczne Redis rozproszonych.

Aby uzyskać więcej informacji, zobacz <xref:performance/caching/distributed>.

### <a name="cache-tag-helper"></a>Pomocnik tagu pamięci podręcznej

Buforowanie zawartości z widoku składnika MVC lub strony Razor za pomocą Pomocnik tagu pamięci podręcznej. Pomocnik tagu pamięci podręcznej używa buforowanie w pamięci do przechowywania danych.

Aby uzyskać więcej informacji, zobacz <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>.

### <a name="distributed-cache-tag-helper"></a>Pomocnik tagu rozproszonej pamięci podręcznej

Za pomocą rozproszonej Pomocnik tagu pamięci podręcznej w pamięci podręcznej zawartości widoku składnika MVC lub strona Razor w rozproszonych scenariuszach kolektywu serwerów sieci web lub w chmurze. Pomocnik tagu rozproszonej pamięci podręcznej używa programu SQL Server lub usługi Redis do przechowywania danych.

Aby uzyskać więcej informacji, zobacz <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>.

## <a name="responsecache-attribute"></a>Atrybut ResponseCache

<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> Określa parametry, które są niezbędne do ustawienie odpowiednich nagłówków w buforowanie odpowiedzi.

> [!WARNING]
> Wyłącz buforowanie zawartości, która zawiera informacje dotyczące uwierzytelnionych klientów. Pamięć podręczna powinna być włączona tylko dla zawartości, która nie zmienia się na podstawie tożsamości użytkownika lub tego, czy użytkownik jest zalogowany.

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> Zmienia odpowiedzi przechowywane wartości podanej listy kluczy zapytania. Gdy do pojedynczej wartości `*` zostanie podana, oprogramowanie pośredniczące różni się w odpowiedzi dla wszystkich żądań parametrów ciągu zapytania.

[Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) musi być włączona, aby ustawić <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> właściwości. W przeciwnym razie jest zgłaszany wyjątek czasu wykonywania. Nie ma odpowiedniego nagłówka HTTP dla <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> właściwości. Właściwość jest funkcja protokołu HTTP obsługiwane przez oprogramowanie pośredniczące buforowania odpowiedzi. Oprogramowaniu pośredniczącym, aby obsługiwać odpowiedzi z pamięci podręcznej ciąg zapytania i wartość ciągu zapytania musi odpowiadać poprzedniego żądania. Na przykład należy wziąć pod uwagę sekwencji żądań i wyniki wyświetlane w poniższej tabeli.

| Żądanie                          | Wynik                    |
| -------------------------------- | ------------------------- |
| `http://example.com?key1=value1` | Zwrócona z serwera. |
| `http://example.com?key1=value1` | Zwrócony z oprogramowaniem pośredniczącym. |
| `http://example.com?key1=value2` | Zwrócona z serwera. |

Pierwsze żądanie został zwrócony przez serwer, a następnie pamięci podręcznej w oprogramowaniu pośredniczącym. Drugie żądanie jest zwracany przez oprogramowanie pośredniczące, ponieważ poprzednie żądanie pasuje do ciągu zapytania. Trzecie żądanie nie jest w pamięci podręcznej oprogramowania pośredniczącego, ponieważ wartość ciągu zapytania nie pasuje do poprzedniego żądania.

<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> Służy do konfigurowania i utworzyć (za pośrednictwem <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>) <xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter>. <xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter> Wykonuje pracę aktualizowania tych funkcji, odpowiedzi i odpowiednie nagłówki HTTP. Filtr:

* Usuwa wszystkie istniejące nagłówki `Vary`, `Cache-Control`, i `Pragma`.
* Zapisuje się odpowiednie nagłówki, na podstawie właściwości ustawione w <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>.
* Aktualizuje odpowiedzi buforowania funkcji protokołu HTTP, jeśli <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> jest ustawiona.

### <a name="vary"></a>różnią się

Tego pliku nagłówkowego są zapisywane tylko wtedy, kiedy <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> właściwość jest ustawiona. Właściwość ustawioną na `Vary` wartości właściwości. Następujące przykładowe używa <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> właściwości:

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache1.cshtml.cs?name=snippet)]

Za pomocą aplikacji przykładowej, Wyświetl nagłówki odpowiedzi za pomocą narzędzi sieci w przeglądarce. Następujące nagłówki odpowiedzi są wysyłane z Cache1 odpowiedzi strony:

```
Cache-Control: public,max-age=30
Vary: User-Agent
```

### <a name="nostore-and-locationnone"></a>NoStore i Location.None

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> zastępuje większości innych właściwości. Jeśli ta właściwość jest równa `true`, `Cache-Control` nagłówka ustawiono `no-store`. Jeśli <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> ustawiono `None`:

* `Cache-Control` ustawiono `no-store,no-cache`.
* `Pragma` ustawiono `no-cache`.

Jeśli <xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> jest `false` i <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> jest `None`, `Cache-Control`, i `Pragma` są ustawione na `no-cache`.

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> Zazwyczaj jest ustawiona na `true` dla stron błędów. Na stronie Cache2 Przykładowa aplikacja tworzy nagłówki odpowiedzi, które poinstruowanie klienta nie będą przechowywane w odpowiedzi.

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache2.cshtml.cs?name=snippet)]

Przykładowa aplikacja zwraca stronę Cache2 z następujące nagłówki:

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Lokalizacja i czas trwania

Aby włączyć buforowanie, <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> musi być równa wartości dodatniej i <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> musi być albo `Any` (ustawienie domyślne) lub `Client`. W tym przypadku `Cache-Control` nagłówka ustawiono wartość lokalizacji, a następnie `max-age` odpowiedzi.

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>w opcji `Any` i `Client` przekłada się na `Cache-Control` wartości nagłówka `public` i `private`, odpowiednio. Jak wspomniano wcześniej, ustawienie <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> do `None` ustawia zarówno `Cache-Control` i `Pragma` nagłówki, aby `no-cache`.

W poniższym przykładzie pokazano modelu strony Cache3 z przykładową aplikację i nagłówki produkowane przez ustawienie <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> i pozostawiając domyślną <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> wartość:

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache3.cshtml.cs?name=snippet)]

Przykładowa aplikacja zwraca następujący nagłówek strony z Cache3:

```
Cache-Control: public,max-age=10
```

### <a name="cache-profiles"></a>Profile pamięci podręcznej

Zamiast duplikowania ustawienia pamięci podręcznej odpowiedzi na wiele atrybutów akcji kontrolera, pamięci podręcznej można skonfigurować profile jako opcje podczas definiowania stron MVC i Razor w `Startup.ConfigureServices`. Wartości znajdujące się w profilu pamięci podręcznej odwołania są używane jako domyślne za <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> i są zastępowane przez dowolne właściwości określone w atrybucie.

Ustawić profil pamięci podręcznej. W poniższym przykładzie pokazano 30 drugi profil pamięci podręcznej w przykładowej aplikacji `Startup.ConfigureServices`:

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

Przykładowa aplikacja Cache4 strony modelu odniesienia `Default30` pamięci podręcznej profilu:

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache4.cshtml.cs?name=snippet)]

<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> Mogą być stosowane do:

* Programy obsługi strony razor (grupy) &ndash; nie można zastosować atrybutów do metody obsługi.
* Kontrolerów MVC (grupy).
* Akcje MVC (metod) &ndash; atrybuty na poziomie przesłonić ustawienia określone atrybuty na poziomie klasy.

Nagłówek wynikowy zastosowane do odpowiedzi strony Cache4 przez `Default30` pamięci podręcznej profilu:

```
Cache-Control: public,max-age=30
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zapisywanie odpowiedzi w pamięci podręcznej](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
