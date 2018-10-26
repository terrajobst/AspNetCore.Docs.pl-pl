---
title: Oprogramowanie pośredniczące w programie ASP.NET Core buforowania odpowiedzi
author: guardrex
description: Informacje o sposobie konfigurowania i używania oprogramowanie pośredniczące buforowania odpowiedzi w programie ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/26/2017
uid: performance/caching/middleware
ms.openlocfilehash: b5eef012356236d600d026e5161a2df5a7954e3b
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090488"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Oprogramowanie pośredniczące w programie ASP.NET Core buforowania odpowiedzi

Przez [Luke Latham](https://github.com/guardrex) i [Luo Jan](https://github.com/JunTaoLuo)

[Wyświetlanie lub pobieranie platformy ASP.NET Core 2.1 Przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

W tym artykule wyjaśniono, jak skonfigurować oprogramowanie pośredniczące buforowania odpowiedzi w aplikacji ASP.NET Core. Oprogramowanie pośredniczące Określa, kiedy podlega buforowaniu, na są odpowiedzi, magazyny odpowiedzi i służy odpowiedzi z pamięci podręcznej. Wprowadzenie do buforowania HTTP i `ResponseCache` atrybutów, zobacz [buforowanie odpowiedzi](xref:performance/caching/response).

## <a name="package"></a>Package

::: moniker range=">= aspnetcore-2.1"

Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) pakietu.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Odwołanie [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) lub Dodaj odwołanie do pakietu [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) pakietu.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Dodaj odwołanie do pakietu [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) pakietu.

::: moniker-end

## <a name="configuration"></a>Konfiguracja

W `Startup.ConfigureServices`, dodać do kolekcji usługi, oprogramowanie pośredniczące.

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=9)]

Konfiguruje aplikację do korzystania z oprogramowania pośredniczącego z `UseResponseCaching` metodę rozszerzenia, która dodaje oprogramowanie pośredniczące do potoku przetwarzania żądań. Przykładowa aplikacja dodaje [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) nagłówka odpowiedzi, który buforuje podlega buforowaniu, odpowiedzi na maksymalnie 10 sekund. Przykład wysyła [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) nagłówka do konfiguracji oprogramowania pośredniczącego, aby obsługiwać odpowiedzi z pamięci podręcznej tylko wtedy, gdy [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) nagłówka kolejne żądania jest zgodna z wersją z oryginalnego żądania. W poniższym przykładzie kodu [CacheControlHeaderValue](/dotnet/api/microsoft.net.http.headers.cachecontrolheadervalue) i [HeaderNames](/dotnet/api/microsoft.net.http.headers.headernames) wymagają `using` poufności informacji dotyczące [Microsoft.Net.Http.Headers](/dotnet/api/microsoft.net.http.headers) przestrzeń nazw.

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=17,22-29)]

Oprogramowanie pośredniczące buforowania odpowiedzi buforuje tylko odpowiedzi serwera, których wynikiem kod stanu 200 (OK). Inne odpowiedzi, w tym [stron błędów, które](xref:fundamentals/error-handling), są ignorowane przez oprogramowanie pośredniczące.

> [!WARNING]
> Odpowiedziami zawierającymi zawartości dla klientów uwierzytelnionego musi być oznaczona jako nie podlega buforowaniu, aby zapobiec oprogramowania pośredniczącego z przechowywania i obsługujących te odpowiedzi. Zobacz [warunków do buforowania](#conditions-for-caching) szczegółowe informacje na temat sposobu oprogramowania pośredniczącego Określa, czy odpowiedź jest podlega buforowaniu.

## <a name="options"></a>Opcje

Oprogramowanie pośredniczące udostępnia trzy opcje do kontrolowania buforowania odpowiedzi.

| Opcja                | Opis |
| --------------------- | ----------- |
| UseCaseSensitivePaths | Określa, jeśli odpowiedzi są buforowane w ścieżkach uwzględniana wielkość liter. Wartość domyślna to `false`. |
| MaximumBodySize       | Największy rozmiar podlega buforowaniu, na treść odpowiedzi w bajtach. Wartość domyślna to `64 * 1024 * 1024` (64 MB). |
| SizeLimit             | Limit rozmiaru pamięci podręcznej odpowiedzi oprogramowania pośredniczącego w bajtach. Wartość domyślna to `100 * 1024 * 1024` (100 MB). |

Poniższy przykład umożliwia skonfigurowanie oprogramowaniu pośredniczącym, aby:

* Buforowanie odpowiedzi mniejsza niż lub równa 1024 bajty.
* Store odpowiedzi przez ścieżki uwzględniana wielkość liter (na przykład `/page1` i `/Page1` są przechowywane w innej lokalizacji).

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

Podczas korzystania z kontrolerów interfejsu API sieci Web i MVC lub stron Razor strony modeli, `ResponseCache` atrybut określa parametry, które są niezbędne do ustawiania odpowiednie nagłówki do buforowania odpowiedzi. Tylko parametr `ResponseCache` atrybut, który ściśle wymaga oprogramowania pośredniczącego jest `VaryByQueryKeys`, nie odpowiadać rzeczywistej nagłówka HTTP. Aby uzyskać więcej informacji, zobacz [atrybut ResponseCache](xref:performance/caching/response#responsecache-attribute).

Bez korzystania z `ResponseCache` atrybutu, buforowanie odpowiedzi może być zróżnicowane z `VaryByQueryKeys` funkcji. Użyj `ResponseCachingFeature` bezpośrednio z `IFeatureCollection` z `HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

Za pomocą pojedynczej wartości równej `*` w `VaryByQueryKeys` różni się w pamięci podręcznej przez wszystkie parametry zapytania żądania.

## <a name="http-headers-used-by-response-caching-middleware"></a>Nagłówki HTTP używana przez oprogramowanie pośredniczące buforowania odpowiedzi

Buforowanie odpowiedzi przez oprogramowanie pośredniczące jest skonfigurowany przy użyciu nagłówków HTTP.

| nagłówek | Szczegóły |
| ------ | ------- |
| Autoryzacja | Jeśli istnieje nagłówek odpowiedzi nie są buforowane. |
| Cache-Control | Tylko uwzględnia oprogramowanie pośredniczące buforowania odpowiedzi oznaczone `public` dyrektywy pamięci podręcznej. Kontrolowanie buforowania z następującymi parametrami:<ul><li>Maksymalny wiek</li><li>max-stale&#8224;</li><li>świeży min</li><li>must-revalidate</li><li>no-cache</li><li>nie-store</li><li>tylko jeśli-zapisanych w pamięci podręcznej</li><li>private</li><li>public</li><li>s maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224;Jeśli określono bez ograniczeń `max-stale`, oprogramowanie pośredniczące nie podejmuje żadnych działań.<br>&#8225;`proxy-revalidate`ma taki sam skutek jak `must-revalidate`.<br><br>Aby uzyskać więcej informacji, zobacz [RFC 7231: żądanie dyrektyw sterujących pamięcią podręczną](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| Dyrektywy pragma | A `Pragma: no-cache` nagłówka w żądaniu daje taki sam skutek jak `Cache-Control: no-cache`. Ten nagłówek zostanie zastąpiona przez odpowiednie dyrektywy w `Cache-Control` nagłówka, jeśli jest obecny. Traktowane jako zgodności z poprzednimi wersjami przy użyciu protokołu HTTP/1.0. |
| Set-Cookie | Jeśli istnieje nagłówek odpowiedzi nie są buforowane. Wszelkie oprogramowanie pośredniczące w potoku przetwarzania żądań, który ustawia pliki cookie z co najmniej jeden zapobiega buforowanie odpowiedzi przez oprogramowanie pośredniczące buforowania odpowiedzi (na przykład [na podstawie plików cookie dostawcy TempData](xref:fundamentals/app-state#tempdata)).  |
| różnią się | `Vary` Nagłówka służy różnicującej buforowane odpowiedzi innym nagłówkiem. Na przykład, buforują odpowiedzi przez kodowanie umieszczając `Vary: Accept-Encoding` nagłówka, który buforuje odpowiedzi dla żądań z nagłówkami `Accept-Encoding: gzip` i `Accept-Encoding: text/plain` oddzielnie. Odpowiedź o wartości nagłówka `*` nigdy nie jest przechowywany. |
| Wygasa | Uznane za przestarzałe przez ten nagłówek odpowiedzi nie jest przechowywany lub pobrać, jeśli zastąpiona przez inne `Cache-Control` nagłówków. |
| If-None-Match | Pełna odpowiedź jest obsługiwany z pamięci podręcznej, jeśli wartość nie jest `*` i `ETag` odpowiedzi nie pasuje do żadnej z podanych wartości. W przeciwnym razie odpowiedź 304 (nie zmodyfikowano) jest obsługiwany. |
| If-Modified-Since | Jeśli `If-None-Match` nagłówka nie jest obecny, pełnej odpowiedzi są dostarczane z pamięci podręcznej, jeśli data odpowiedzi z pamięci podręcznej jest nowsza niż podana wartość. W przeciwnym razie odpowiedź 304 (nie zmodyfikowano) jest obsługiwany. |
| Data | Przy wysyłaniu z pamięci podręcznej, `Date` nagłówek jest ustawiana przez oprogramowanie pośredniczące, jeśli nie podano w oryginalnej odpowiedzi. |
| Długość zawartości | Przy wysyłaniu z pamięci podręcznej, `Content-Length` nagłówek jest ustawiana przez oprogramowanie pośredniczące, jeśli nie podano w oryginalnej odpowiedzi. |
| Wiek | `Age` Wysyłane w oryginalnej odpowiedzi nagłówka zostanie zignorowany. Oprogramowanie pośredniczące oblicza nową wartość przy wysyłaniu odpowiedzi z pamięci podręcznej. |

## <a name="caching-respects-request-cache-control-directives"></a>Buforowanie szanuje dyrektywy żądania Cache-Control

Oprogramowanie pośredniczące przestrzega zasad [specyfikacji protokołu HTTP 1.1 buforowania](https://tools.ietf.org/html/rfc7234#section-5.2). Zasady wymagają pamięci podręcznej respektować prawidłową `Cache-Control` nagłówka wysłane przez klienta. W ramach specyfikacji, klient może utworzyć żądania z `no-cache` wartość nagłówka i Wymuś serwera, aby wygenerować nową odpowiedź dla każdego żądania. Obecnie nie ma żadnych deweloperowi kontroli nad tym zachowanie buforowania po za pomocą oprogramowania pośredniczącego, ponieważ oprogramowanie pośredniczące działa zgodnie z oficjalnego specyfikacji buforowania.

Aby uzyskać większą kontrolę nad zachowanie buforowania zapoznaj się z innych funkcji buforowania platformy ASP.NET Core. Zobacz następujące tematy:

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli zachowanie buforowania jest zgodnie z oczekiwaniami, upewnij się, że odpowiedzi są podlega buforowaniu i nadaje się do obsługiwanych z pamięci podręcznej. Przeanalizuj żądania przychodzące nagłówki i nagłówki wychodzące odpowiedzi. Włącz [rejestrowania](xref:fundamentals/logging/index) aby pomóc w debugowaniu.

Podczas testowania i rozwiązywania problemów z zachowaniem buforowania, przeglądarki mogą być ustawione nagłówki żądania, które mają wpływ na buforowanie w sposób niepożądane. Na przykład może ustawić przeglądarkę `Cache-Control` nagłówka do `no-cache` lub `max-age=0` podczas odświeżania strony. Następujące narzędzia można jawnie ustawić nagłówki żądania i są preferowane dla testowania w pamięci podręcznej:

* [Narzędzie fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Warunki dotyczące buforowania

* Żądanie musi spowodować odpowiedź z serwera z kodem stanu 200 (OK).
* Metoda żądania musi być GET lub HEAD.
* Oprogramowanie pośredniczące w terminalu, takich jak [oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files), nie musi przetwarzać odpowiedzi przed oprogramowanie pośredniczące buforowania odpowiedzi.
* `Authorization` Nagłówka nie może być obecny.
* `Cache-Control` Parametry nagłówka musi być prawidłowy, a odpowiedzi muszą być oznaczone jako `public` , nie jest oznaczona `private`.
* `Pragma: no-cache` Nagłówka nie może być obecny Jeśli `Cache-Control` nagłówka nie jest obecny, jako `Cache-Control` zastępuje nagłówek `Pragma` nagłówka, jeśli jest obecny.
* `Set-Cookie` Nagłówka nie może być obecny.
* `Vary` Parametry nagłówka musi być prawidłowe i nie jest równa `*`.
* `Content-Length` Wartość nagłówka (jeśli ustawić) musi być zgodny z rozmiarem treść odpowiedzi.
* [IHttpSendFileFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) nie jest używany.
* Odpowiedź nie mogą być nieaktualne określony przez `Expires` nagłówka i `max-age` i `s-maxage` dyrektywy w pamięci podręcznej.
* Buforowanie odpowiedzi musi zakończyć się powodzeniem, a rozmiar odpowiedzi musi być mniejszy niż skonfigurowany lub domyślne `SizeLimit`.
* Odpowiedzi muszą być podlega buforowaniu, na podstawie położenia [RFC 7234](https://tools.ietf.org/html/rfc7234) specyfikacji. Na przykład `no-store` dyrektywy nie może istnieć w pola nagłówka żądania lub odpowiedzi. Zobacz *sekcja 3: przechowywanie odpowiedzi w pamięci podręcznych* z [RFC 7234](https://tools.ietf.org/html/rfc7234) Aby uzyskać szczegółowe informacje.

> [!NOTE]
> Antiforgery systemu pod kątem generowania tokenów bezpieczne, aby zapobiec fałszerstwo żądania Międzywitrynowego Międzywitrynowych ataki zestawy `Cache-Control` i `Pragma` nagłówki, aby `no-cache` tak, aby odpowiedzi nie są buforowane. Aby uzyskać informacji na temat sposobu wyłączania antiforgery tokenów dla elementów formularza HTML, zobacz [antiforgery konfiguracji platformy ASP.NET Core](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
