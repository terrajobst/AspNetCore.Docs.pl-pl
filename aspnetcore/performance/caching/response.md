---
title: Buforowanie odpowiedzi w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać odpowiedzi z pamięci podręcznej w celu niższymi wymaganiami co do przepustowości i zwiększenie wydajności aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: bbf5b649bac9d31aa6d0ecdc3828648677b05716
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090696"
---
# <a name="response-caching-in-aspnet-core"></a>Buforowanie odpowiedzi w programie ASP.NET Core

Przez [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), i [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> Buforowanie odpowiedzi w stron Razor jest dostępna w programie ASP.NET Core 2.1 lub nowszej.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Buforowanie odpowiedzi zmniejsza liczbę żądań, które sprawia, że klient lub serwer proxy na serwerze sieci web. Buforowanie odpowiedzi zmniejsza ilość pracy serwera sieci web wykonuje do generowania odpowiedzi. Buforowanie odpowiedzi jest kontrolowana przez nagłówki, które określają, jak chcesz klienta, serwera proxy i oprogramowaniu pośredniczącym, aby buforowanie odpowiedzi.

Serwer sieci web może buforować odpowiedzi, po dodaniu [oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware).

## <a name="http-based-response-caching"></a>Buforowanie odpowiedzi oparte na protokole HTTP

[Specyfikacji protokołu HTTP 1.1 buforowania](https://tools.ietf.org/html/rfc7234) opisuje zachowanie pamięci podręcznych Internet. Jest podstawowym nagłówek HTTP używana do buforowania [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), który jest używany do określenia pamięci podręcznej *dyrektywy*. Dyrektywy Sterowanie zachowaniem buforowania żądań przedostają się od klientów do serwerów i jako odpowiedź przedostają się z serwerów z powrotem do klientów. Żądania i odpowiedzi, przenoszenie za pośrednictwem serwerów proxy i serwery proxy również musi być zgodna ze specyfikacją protokołu HTTP 1.1 buforowania.

Typowe `Cache-Control` dyrektywy są wyświetlane w poniższej tabeli.

| — Dyrektywa                                                       | Akcja |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Pamięć podręczna może przechowywać odpowiedzi. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | Odpowiedź nie mogą być przechowywane w udostępnionej pamięci podręcznej. Prywatnej pamięci podręcznej może przechowywać i ponowne użycie odpowiedzi. |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | Klient nie będzie akceptować odpowiedzi, którego okres ważności jest większa niż określoną liczbę sekund. Przykłady: `max-age=60` (60 sekund), `max-age=2592000` (1 miesiąc) |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **W odpowiedzi na żądania**: pamięć podręczna nie mogą używać odpowiedzi przechowywane do spełnienia żądania. Uwaga: Serwer pochodzenia ponownie generuje odpowiedzi klienta, a także oprogramowanie pośredniczące aktualizuje odpowiedzi przechowywane w pamięci podręcznej.<br><br>**W odpowiedzi**: odpowiedź nie może być używany dla kolejnych żądań bez sprawdzania poprawności na serwerze źródłowym. |
| [nie-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **W odpowiedzi na żądania**: pamięć podręczna nie musi przechowywać żądania.<br><br>**W odpowiedzi**: pamięć podręczna nie musi przechowywać dowolną część odpowiedzi. |

W poniższej tabeli przedstawiono innych nagłówków pamięci podręcznej, które mają znaczenie w pamięci podręcznej.

| nagłówek                                                     | Funkcja |
| ---------------------------------------------------------- | -------- |
| [Wiek](https://tools.ietf.org/html/rfc7234#section-5.1)     | Szacowana ilość czasu w sekundach, ponieważ odpowiedzi został wygenerowany pomyślnie zweryfikowane na serwerze źródłowym. |
| [Wygasa](https://tools.ietf.org/html/rfc7234#section-5.3) | Data/Godzina, po czym odpowiedź jest uznawana za starych. |
| [Dyrektywy pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Istnieje wstecznej zgodności przy użyciu protokołu HTTP/1.0 buforuje ustawienia `no-cache` zachowanie. Jeśli `Cache-Control` nagłówek jest obecny, `Pragma` nagłówka zostanie zignorowany. |
| [różnią się](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Określa, czy odpowiedzi z pamięci podręcznej musi nie zostać wysłane dopóki wszystkie z `Vary` pola nagłówka są takie same zarówno buforowaną odpowiedź oryginalne żądanie, jak i nowe żądanie. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>Oparte na protokole HTTP względem buforowania żądań dyrektyw sterujących pamięcią podręczną

[Specyfikacji protokołu HTTP 1.1 buforowania dla nagłówka Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) wymaga pamięci podręcznej respektować prawidłową `Cache-Control` nagłówka wysłane przez klienta. Klientowi mogą wysyłać żądania z `no-cache` wartość nagłówka i Wymuś serwera, aby wygenerować nową odpowiedź dla każdego żądania.

Zawsze zapewniane klienta `Cache-Control` nagłówków żądań ma sens należy wziąć pod uwagę w celu buforowania HTTP. W ramach specyfikacji oficjalne buforowania mają na celu obniżenie kosztów opóźnienia oraz sieci spełnienia żądania za pośrednictwem sieci klientów, serwery proxy i serwerów. Nie jest koniecznie sposób kontroluje obciążenie serwera pochodzenia.

Istnieje nie bieżącej deweloperowi kontroli nad tym zachowanie buforowania, gdy za pomocą [oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) ponieważ oprogramowanie pośredniczące działa zgodnie z oficjalną buforowania specyfikacji. [Przyszłe rozszerzenia będą miały do oprogramowania pośredniczącego](https://github.com/aspnet/ResponseCaching/issues/96) pozwoli na konfigurowanie oprogramowania pośredniczącego, aby zignorować żądania `Cache-Control` nagłówka podczas podejmowania decyzji o do obsługi buforowanych odpowiedzi. To zaoferuje Ci możliwość lepszej kontroli obciążenia na serwerze podczas korzystania z oprogramowania pośredniczącego.

## <a name="other-caching-technology-in-aspnet-core"></a>Innych technologii buforowania w programie ASP.NET Core

### <a name="in-memory-caching"></a>Buforowanie w pamięci

Wewnątrzpamięciowa pamięć podręczna używa pamięci serwera do przechowywania danych w pamięci podręcznej. Ten rodzaj buforowania nadaje się do pojedynczego serwera lub kilku serwerach wykorzystujących *trwałych sesji*. Trwałych sesji oznacza, że żądania klienta są zawsze kierowane do tego samego serwera w celu przetworzenia.

Aby uzyskać więcej informacji, zobacz [pamięci podręcznej w pamięci](xref:performance/caching/memory).

### <a name="distributed-cache"></a>Rozproszonej pamięci podręcznej

Umożliwia przechowywanie danych w pamięci, gdy aplikacja jest hostowana w chmurze i na serwerze farmie rozproszonej pamięci podręcznej. Pamięć podręczna jest współużytkowany przez serwery, które przetwarzają żądania. Klienci mogą przesyłać żądania, które jest obsługiwane przez dowolnego serwera w grupie, jeśli dane w pamięci podręcznej klienta jest dostępny. ASP.NET Core oferuje program SQL Server i pamięci podręczne Redis rozproszonych.

Aby uzyskać więcej informacji, zobacz <xref:performance/caching/distributed>.

### <a name="cache-tag-helper"></a>Pomocnik tagu pamięci podręcznej

Za pomocą Pomocnik tagu pamięci podręcznej, można buforować zawartość widoku składnika MVC lub strona Razor. Pomocnik tagu pamięci podręcznej używa buforowanie w pamięci do przechowywania danych.

Aby uzyskać więcej informacji, zobacz [Pomocnik tagu pamięci podręcznej na platformie ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="distributed-cache-tag-helper"></a>Pomocnik tagu rozproszonej pamięci podręcznej

Za pomocą rozproszonej Pomocnik tagu pamięci podręcznej, można buforować zawartość widoku składnika MVC lub strona Razor w rozproszonych w chmurze lub w scenariuszach z farmami internetowymi. Pomocnik tagu rozproszonej pamięci podręcznej używa programu SQL Server lub usługi Redis do przechowywania danych.

Aby uzyskać więcej informacji, zobacz [rozproszonych Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).

## <a name="responsecache-attribute"></a>Atrybut ResponseCache

[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) określa parametry, które są niezbędne do ustawienie odpowiednich nagłówków w buforowanie odpowiedzi.

> [!WARNING]
> Wyłącz buforowanie zawartości, która zawiera informacje dotyczące uwierzytelnionych klientów. Pamięć podręczna powinna być włączona tylko dla zawartości, która nie zmienia się na podstawie tożsamości użytkownika lub tego, czy użytkownik jest zalogowany.

[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) odpowiedzi przechowywane jest zależna od wartości podanej listy kluczy zapytania. Gdy do pojedynczej wartości `*` zostanie podana, oprogramowanie pośredniczące różni się w odpowiedzi dla wszystkich żądań parametrów ciągu zapytania. `VaryByQueryKeys` wymaga platformy ASP.NET Core 1.1 lub nowszej.

Oprogramowanie pośredniczące buforowania odpowiedzi musi być włączona, aby ustawić `VaryByQueryKeys` właściwości; w przeciwnym razie jest zgłaszany wyjątek czasu wykonywania. Nie ma odpowiedniego nagłówka HTTP dla `VaryByQueryKeys` właściwości. Właściwość jest funkcja protokołu HTTP obsługiwane przez oprogramowanie pośredniczące buforowania odpowiedzi. Oprogramowaniu pośredniczącym, aby obsługiwać odpowiedzi z pamięci podręcznej ciąg zapytania i wartość ciągu zapytania musi odpowiadać poprzedniego żądania. Na przykład należy wziąć pod uwagę sekwencji żądań i wyniki wyświetlane w poniższej tabeli.

| Żądanie                          | Wynik                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | Zwrócone z serwera     |
| `http://example.com?key1=value1` | Zwrócone przez oprogramowanie pośredniczące |
| `http://example.com?key1=value2` | Zwrócone z serwera     |

Pierwsze żądanie został zwrócony przez serwer, a następnie pamięci podręcznej w oprogramowaniu pośredniczącym. Drugie żądanie jest zwracany przez oprogramowanie pośredniczące, ponieważ poprzednie żądanie pasuje do ciągu zapytania. Trzecie żądanie nie jest w pamięci podręcznej oprogramowania pośredniczącego, ponieważ wartość ciągu zapytania nie pasuje do poprzedniego żądania. 

`ResponseCacheAttribute` Służy do konfigurowania i utworzyć (za pośrednictwem `IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter). `ResponseCacheFilter` Wykonuje pracę aktualizowania tych funkcji, odpowiedzi i odpowiednie nagłówki HTTP. Filtr:

* Usuwa wszystkie istniejące nagłówki `Vary`, `Cache-Control`, i `Pragma`. 
* Zapisuje się odpowiednie nagłówki, na podstawie właściwości ustawione w `ResponseCacheAttribute`. 
* Aktualizuje odpowiedzi buforowania funkcji protokołu HTTP, jeśli `VaryByQueryKeys` jest ustawiona.

### <a name="vary"></a>różnią się

Tego pliku nagłówkowego są zapisywane tylko wtedy, kiedy `VaryByHeader` właściwość jest ustawiona. Jest równa `Vary` wartości właściwości. Następujące przykładowe używa `VaryByHeader` właściwości:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

Można wyświetlić nagłówki odpowiedzi za pomocą narzędzi sieci w przeglądarce. Na poniższej ilustracji przedstawiono F12 krawędzi, dane wyjściowe na **sieci** kartę, kiedy `About2` metody akcji jest odświeżany:

![Krawędzi F12 w danych wyjściowych karty sieciowej, gdy wywoływana jest metoda akcji About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore i Location.None

`NoStore` zastępuje większości innych właściwości. Jeśli ta właściwość jest równa `true`, `Cache-Control` nagłówka ustawiono `no-store`. Jeśli `Location` ustawiono `None`:

* `Cache-Control` ustawiono `no-store,no-cache`.
* `Pragma` ustawiono `no-cache`.

Jeśli `NoStore` jest `false` i `Location` jest `None`, `Cache-Control` i `Pragma` są ustawione na `no-cache`.

Zwykle ustawiana `NoStore` do `true` na stron błędów. Na przykład:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

Skutkuje to następujące nagłówki:

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Lokalizacja i czas trwania

Aby włączyć buforowanie, `Duration` musi być równa wartości dodatniej i `Location` musi być albo `Any` (ustawienie domyślne) lub `Client`. W tym przypadku `Cache-Control` nagłówka ustawiono wartość lokalizacji, a następnie `max-age` odpowiedzi.

> [!NOTE]
> `Location`w opcji `Any` i `Client` przekłada się na `Cache-Control` wartości nagłówka `public` i `private`, odpowiednio. Jak wspomniano wcześniej, ustawienie `Location` do `None` ustawia zarówno `Cache-Control` i `Pragma` nagłówki, aby `no-cache`.

Poniżej przedstawiono przykładowy nagłówki określonemu przez ustawienie `Duration` i pozostawiając domyślną `Location` wartość:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

To generuje następujący nagłówek:

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>Profile pamięci podręcznej

Zamiast duplikowania `ResponseCache` ustawienia na wiele atrybutów akcji kontrolera, profile pamięci podręcznej można skonfigurować jako opcje podczas definiowania MVC w `ConfigureServices` method in Class metoda `Startup`. Wartości znajdujące się w profilu pamięci podręcznej odwołania są używane jako domyślne za `ResponseCache` atrybutu i są zastępowane przez dowolne właściwości określone w atrybucie.

Konfigurowanie profilu pamięci podręcznej:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

Odwoływanie się do profilu pamięci podręcznej:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

`ResponseCache` Atrybut można stosować zarówno do działania (metod) i kontrolerów (grupy). Atrybuty na poziomie zastępują ustawienia określone atrybuty na poziomie klasy.

W powyższym przykładzie atrybut poziomu klasa określa czas trwania wynoszący 30 sekund, gdy atrybut poziom metody odwołuje się do profilu pamięci podręcznej o czasie trwania wartość 60 sekund.

Nagłówek wynikowy:

```
Cache-Control: public,max-age=60
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
