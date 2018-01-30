---
title: Buforowanie odpowiedzi w ASP.NET Core
author: rick-anderson
description: "Dowiedz się, jak używać odpowiedzi buforowanie, aby niższe wymagania dotyczące przepustowości i zwiększyć wydajność aplikacji platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.topic: article
uid: performance/caching/response
ms.openlocfilehash: c38f9b9a1bf1c523951e2cf1f3070858fe5daf04
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="response-caching-in-aspnet-core"></a>Buforowanie odpowiedzi w ASP.NET Core

Przez [Luo Jan](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), i [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> Buforowanie odpowiedzi [nie jest obsługiwane na stronach Razor, ASP.NET 2.0 Core](https://github.com/aspnet/Mvc/issues/6437). Ta funkcja będzie obsługiwany od [wersji platformy ASP.NET Core 2.1](https://github.com/aspnet/Home/wiki/Roadmap).
  
[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Buforowanie odpowiedzi zmniejsza liczbę żądań, które powoduje, że klient lub serwer proxy na serwerze sieci web. Buforowanie odpowiedzi zmniejsza ilość pracy serwera sieci web wykonuje do generowania odpowiedzi. Buforowanie odpowiedzi jest kontrolowana przez nagłówki, które określają sposób klienta, serwera proxy i oprogramowanie pośredniczące do odpowiedzi z pamięci podręcznej.

Serwer sieci web może buforować odpowiedzi, po dodaniu [buforowanie odpowiedzi w oprogramowaniu pośredniczącym](xref:performance/caching/middleware).

## <a name="http-based-response-caching"></a>Buforowanie odpowiedzi oparte na protokole HTTP

[Specyfikacji protokołu HTTP 1.1 buforowanie](https://tools.ietf.org/html/rfc7234) opisano zachowanie pamięci podręcznych Internet. Jest podstawowy nagłówka HTTP używana do buforowania [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), który służy do określania pamięci podręcznej *dyrektywy*. Dyrektywy kontroli zachowania buforowania żądań umożliwiającej im od klientów na serwerach i ponieważ odpowiedź sposób ich z serwerów do klientów. Żądania i odpowiedzi Przenieś za pośrednictwem serwerów proxy, i serwery proxy również musi być zgodna z Specyfikacja protokołu HTTP 1.1 buforowania.

Typowe `Cache-Control` dyrektywy przedstawiono w poniższej tabeli.

| Dyrektywy                                                       | Akcja |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Odpowiedzi mogą być przechowywane w pamięci podręcznej. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | Odpowiedź nie mogą być przechowywane przez współużytkowanej pamięci podręcznej. Prywatne pamięć podręczna może przechowywać i ponowne użycie odpowiedzi. |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | Klient nie będzie akceptować odpowiedzi, którego okres ważności jest większa niż określoną liczbę sekund. Przykłady: `max-age=60` (60 sekund), `max-age=2592000` (miesiąc) |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **W odpowiedzi na żądania**: pamięci podręcznej nie może używać odpowiedzi przechowywane do spełnienia żądania. Uwaga: Serwer pochodzenia ponownie generuje odpowiedzi do klienta, a oprogramowanie pośredniczące aktualizuje odpowiedzi przechowywane w pamięci podręcznej.<br><br>**W odpowiedzi**: odpowiedź nie mogą być używane dla kolejnych żądań bez sprawdzania poprawności na serwerze źródłowym. |
| [nie magazynu](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **W odpowiedzi na żądania**: pamięć podręczna nie mogą przechowywać żądania.<br><br>**W odpowiedzi**: pamięć podręczna nie mogą przechowywać dowolną część odpowiedzi. |

W poniższej tabeli przedstawiono innych nagłówków pamięci podręcznej, które uczestniczy w pamięci podręcznej.

| nagłówek                                                     | Funkcja |
| ---------------------------------------------------------- | -------- |
| [Okres ważności](https://tools.ietf.org/html/rfc7234#section-5.1)     | Szacowana ilość czasu w sekundach od czasu odpowiedzi został wygenerowany lub pomyślnie zweryfikowane na serwerze źródłowym. |
| [Wygasa](https://tools.ietf.org/html/rfc7234#section-5.3) | Data/Godzina po upływie którego odpowiedź jest uznawane za przestarzałe. |
| [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Istnieje dla zapewnienia zgodności z protokołu HTTP/1.0 buforuje ustawienia `no-cache` zachowanie. Jeśli `Cache-Control` występuje nagłówek `Pragma` nagłówka zostanie zignorowany. |
| [Różnią się](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Określa, że buforowanej odpowiedzi może nie zostać wysłane dopóki wszystkie z `Vary` pola nagłówka są takie same w nowe żądanie i odpowiedź buforowana oryginalne żądanie. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>Oparte na protokole HTTP względem buforowania żądań dyrektywy Cache-Control

[HTTP 1.1 buforowanie specyfikacji nagłówek Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) wymaga pamięci podręcznej uwzględnić prawidłową `Cache-Control` nagłówka wysłane przez klienta. Klientowi mogą wysyłać żądania z `no-cache` wartość nagłówka i Wymuś serwera, aby wygenerować nową odpowiedź na każde żądanie.

Zawsze zachowaniu klienta `Cache-Control` nagłówków żądań ma sens, jeśli należy wziąć pod uwagę w celu buforowania HTTP. W obszarze specyfikację oficjalnego buforowanie ma zmniejszyć koszty opóźnienia i sieci spełnienia żądania w sieci klientów, serwery proxy i serwerów. Nie jest zawsze pozwala sterować obciążenia na serwerze źródłowym.

Istnieje nie bieżącego deweloperowi kontroli nad to zachowanie buforowania, gdy przy użyciu [buforowanie odpowiedzi w oprogramowaniu pośredniczącym](xref:performance/caching/middleware) ponieważ oprogramowanie pośredniczące odpowiada oficjalne buforowanie specyfikacji. [Przyszłe ulepszenia oprogramowanie pośredniczące](https://github.com/aspnet/ResponseCaching/issues/96) pozwoli na konfigurowanie oprogramowania pośredniczącego, aby zignorować żądania `Cache-Control` nagłówka podczas podejmowania decyzji o do obsługi buforowanej odpowiedzi. To zaoferuje Ci możliwość lepszą kontrolę obciążenia na serwerze używania oprogramowania pośredniczącego.

## <a name="other-caching-technology-in-aspnet-core"></a>Inne technologie buforowania w ASP.NET Core

### <a name="in-memory-caching"></a>Buforowanie w pamięci

Buforowanie w pamięci używa pamięci serwera do przechowywania danych z pamięci podręcznej. Tego rodzaju buforowanie jest odpowiedni dla pojedynczy serwer lub wiele serwerów przy użyciu *trwałe sesje*. Trwałe sesje oznacza, że żądania od klienta zawsze są kierowane do tego samego serwera przetwarzania.

Aby uzyskać więcej informacji, zobacz [wprowadzenie do buforowania w pamięci w ASP.NET Core](xref:performance/caching/memory).

### <a name="distributed-cache"></a>Rozproszonej pamięci podręcznej

Rozproszonej pamięci podręcznej umożliwia przechowywanie danych w pamięci, gdy aplikacja jest hostowana na farmie sieci chmury lub serwera. Pamięć podręczna jest współużytkowana przez serwery, które przetwarzają żądania. Klient może przesłać żądania, które zostały obsłużone przez dowolny serwer w grupie dane z pamięci podręcznej klienta jest dostępny. Platformy ASP.NET Core oferuje programu SQL Server i pamięci podręczne Redis rozproszonych.

Aby uzyskać więcej informacji, zobacz [Praca z rozproszonej pamięci podręcznej](xref:performance/caching/distributed).

### <a name="cache-tag-helper"></a>Pamięć podręczna pomocnika tagów

Może buforować zawartość z widoku MVC lub Razor strony z pamięci podręcznej pomocnika tagów. Pamięć podręczna pomocnika tagów używa do przechowywania danych buforowanie w pamięci.

Aby uzyskać więcej informacji, zobacz [pomocnika tagów pamięci podręcznej w programie ASP.NET MVC Core](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="distributed-cache-tag-helper"></a>Pomocnik Tag rozproszonej pamięci podręcznej

Zawartość z widoku MVC lub Razor strony w chmurze rozproszonej lub scenariusze farmy sieci web może buforować z Pomocnika Tag rozproszonej pamięci podręcznej. Pomocnika Tag rozproszonej pamięci podręcznej używa programu SQL Server lub Redis do przechowywania danych.

Aby uzyskać więcej informacji, zobacz [rozproszonej pamięci podręcznej pomocnika tagów](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).

## <a name="responsecache-attribute"></a>ResponseCache attribute

`ResponseCacheAttribute` Określa parametry, które są niezbędne do ustawiania odpowiednich nagłówków w ramach buforowania odpowiedzi. Zobacz [ResponseCacheAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) opis parametrów.

> [!WARNING]
> Wyłącz buforowanie zawartości, który zawiera informacje dotyczące klientów, uwierzytelnionym. Buforowanie powinna być włączona tylko dla zawartości, która nie zmienia się na podstawie tożsamości użytkownika lub określa, czy użytkownik jest zalogowany.

`VaryByQueryKeys string[]`(wymaga platformy ASP.NET Core 1.1 i nowsze): gdy są ustawione, oprogramowanie pośredniczące buforowanie odpowiedzi zależy od odpowiedzi przechowywane wartości danej listy kluczy zapytania. Oprogramowanie pośredniczące buforowanie odpowiedzi musi być włączona, aby ustawić `VaryByQueryKeys` właściwość; w przeciwnym razie jest zgłaszany wyjątek czasu wykonywania. Nagłówek HTTP nie odpowiednie dla występuje `VaryByQueryKeys` właściwości. Ta właściwość jest funkcją HTTP obsługiwane przez oprogramowanie pośredniczące buforowanie odpowiedzi. Oprogramowanie pośredniczące do obsługi buforowaną odpowiedź ciąg zapytania i wartość ciągu zapytania musi odpowiadać poprzedniego żądania. Rozważmy na przykład sekwencji żądań i wyniki będą wyświetlane w poniższej tabeli.

| Żądanie                          | Wynik                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | Zwrócony z serwera     |
| `http://example.com?key1=value1` | Zwrócone przez oprogramowanie pośredniczące |
| `http://example.com?key1=value2` | Zwrócony z serwera     |

Pierwsze żądanie jest zwrócony przez serwer i w pamięci podręcznej w oprogramowaniu pośredniczącym. Drugie żądanie jest zwracana przez oprogramowanie pośredniczące, ponieważ ciąg zapytania jest zgodna z poprzedniego żądania. W pamięci podręcznej oprogramowania pośredniczącego nie trzeci żądania, ponieważ wartość ciągu kwerendy nie pasuje do poprzedniego żądania. 

`ResponseCacheAttribute` Służy do konfigurowania i utworzyć (za pośrednictwem `IFilterFactory`) `ResponseCacheFilter`. `ResponseCacheFilter` Wykonuje pracę aktualizacji odpowiednich nagłówków HTTP i funkcje odpowiedzi. Filtr:

* Usuwa wszystkie istniejące nagłówki dla `Vary`, `Cache-Control`, i `Pragma`. 
* Zapisuje się odpowiednie nagłówki na podstawie właściwości w `ResponseCacheAttribute`. 
* Aktualizuje odpowiedzi HTTP funkcji buforowania, jeśli `VaryByQueryKeys` jest ustawiona.

### <a name="vary"></a>różnią się

Ten nagłówek są zapisywane tylko, gdy `VaryByHeader` właściwość jest ustawiona. Ma ustawioną wartość `Vary` wartość właściwości. Następujące przykładowe używa `VaryByHeader` właściwości:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

Można wyświetlić nagłówki odpowiedzi z narzędziami sieci w przeglądarce. Na poniższej ilustracji przedstawiono krawędzi naciśnięcia klawisza F12, dane wyjściowe na **sieci** karcie kiedy `About2` odświeżeniu metody akcji:

![Krawędzi F12 dane wyjściowe na karcie sieciowej, gdy wywoływana jest metoda akcji About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore i Location.None

`NoStore`zastępuje większości innych właściwości. Jeśli ta właściwość jest skonfigurowana `true`, `Cache-Control` ma ustawioną wartość nagłówka `no-store`. Jeśli `Location` ma ustawioną wartość `None`:

* `Cache-Control`ustawiono `no-store,no-cache`.
* `Pragma`ustawiono `no-cache`.

Jeśli `NoStore` jest `false` i `Location` jest `None`, `Cache-Control` i `Pragma` są ustawione na `no-cache`.

Zwykle ustawiana `NoStore` do `true` na stron błędów. Na przykład:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

Powoduje to następujące nagłówki:

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Miejsce i czas trwania

Aby włączyć buforowanie, `Duration` musi mieć ustawioną wartość dodatnią i `Location` musi być równa albo `Any` (ustawienie domyślne) lub `Client`. W takim przypadku `Cache-Control` nagłówka ma ustawioną wartość lokalizacji, a następnie `max-age` odpowiedzi.

> [!NOTE]
> `Location`w opcji `Any` i `Client` umożliwiło `Cache-Control` wartości nagłówka `public` i `private`odpowiednio. Jak wspomniano wcześniej, ustawienie `Location` do `None` ustawia zarówno `Cache-Control` i `Pragma` nagłówków `no-cache`.

Poniżej przedstawiono przykładowy nagłówki powstaje przez ustawienie `Duration` i pozostawienie domyślnej `Location` wartość:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

Spowoduje to utworzenie następujący nagłówek:

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>Profile pamięci podręcznej

Zamiast duplikowania `ResponseCache` ustawienia na wiele atrybutów akcji kontrolera, profile pamięci podręcznej można skonfigurować jako opcje podczas konfigurowania MVC w `ConfigureServices` metoda `Startup`. Wartości znajdujące się w profilu pamięci podręcznej przywoływanego są używane jako domyślne przez `ResponseCache` atrybutu i zostały zastąpione przez dowolne właściwości określony w atrybucie.

Konfigurowanie profilu pamięci podręcznej:

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

Odwołanie do profilu pamięci podręcznej:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

`ResponseCache` Atrybut może odnosić się zarówno do działania (metody) i kontrolery (klasy). Atrybuty na poziomie metody zastępują ustawienia określone w poziomie klasy atrybutów.

W powyższym przykładzie atrybut poziomie klasy określa czas trwania wynoszący 30 sekund, podczas gdy atrybut poziomie metody odwołuje się do profilu pamięci podręcznej o czasie trwania ustawioną 60 sekund.

Wynikowa nagłówka:

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Buforowanie w protokole HTTP z specyfikację](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Buforowanie w pamięci](xref:performance/caching/memory)
* [Praca z rozproszonej pamięci podręcznej](xref:performance/caching/distributed)
* [Wykrywanie zmian z tokenami zmiany](xref:fundamentals/primitives/change-tokens)
* [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware)
* [Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Pomocnik tagu rozproszonej pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
