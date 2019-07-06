---
title: Oprogramowanie pośredniczące w programie ASP.NET Core buforowania odpowiedzi
author: guardrex
description: Informacje o sposobie konfigurowania i używania oprogramowanie pośredniczące buforowania odpowiedzi w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/05/2019
uid: performance/caching/middleware
ms.openlocfilehash: d6756ce16396133da643cc08ca0f48369479ad3a
ms.sourcegitcommit: b9e914ef274b5ec359582f299724af6234dce135
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67596151"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Oprogramowanie pośredniczące w programie ASP.NET Core buforowania odpowiedzi

Przez [Luke Latham](https://github.com/guardrex) i [Luo Jan](https://github.com/JunTaoLuo)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

W tym artykule wyjaśniono, jak skonfigurować oprogramowanie pośredniczące buforowania odpowiedzi w aplikacji ASP.NET Core. Oprogramowanie pośredniczące Określa, kiedy podlega buforowaniu, na są odpowiedzi, magazyny odpowiedzi i służy odpowiedzi z pamięci podręcznej. Wprowadzenie do buforowania HTTP i [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) atrybutów, zobacz [buforowanie odpowiedzi](xref:performance/caching/response).

## <a name="configuration"></a>Konfiguracja

Użyj [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) pakietu.

W `Startup.ConfigureServices`, dodać do kolekcji usługi, oprogramowanie pośredniczące buforowania odpowiedzi:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

Konfiguruje aplikację do korzystania z oprogramowania pośredniczącego z <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> metodę rozszerzenia, która dodaje oprogramowanie pośredniczące do potoku przetwarzania żądań w `Startup.Configure`:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

Przykładowa aplikacja dodaje nagłówki, aby kontrolować, buforowanie na kolejne żądania:

* [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; buforuje podlega buforowaniu, odpowiedzi na maksymalnie 10 sekund.
* [Różnią się](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; Konfiguruje oprogramowanie pośredniczące do obsługi odpowiedzi z pamięci podręcznej tylko wtedy, gdy [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) nagłówka kolejne żądania jest zgodna z wersją z oryginalnego żądania.

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

Oprogramowanie pośredniczące buforowania odpowiedzi buforuje tylko odpowiedzi serwera, których wynikiem kod stanu 200 (OK). Inne odpowiedzi, w tym [stron błędów, które](xref:fundamentals/error-handling), są ignorowane przez oprogramowanie pośredniczące.

> [!WARNING]
> Odpowiedziami zawierającymi zawartości dla klientów uwierzytelnionego musi być oznaczona jako nie podlega buforowaniu, aby zapobiec oprogramowania pośredniczącego z przechowywania i obsługujących te odpowiedzi. Zobacz [warunków do buforowania](#conditions-for-caching) szczegółowe informacje na temat sposobu oprogramowania pośredniczącego Określa, czy odpowiedź jest podlega buforowaniu.

## <a name="options"></a>Opcje

W poniższej tabeli przedstawiono opcje buforowania odpowiedzi.

| Opcja | Opis |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | Największy rozmiar podlega buforowaniu, na treść odpowiedzi w bajtach. Wartość domyślna to `64 * 1024 * 1024` (64 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | Limit rozmiaru pamięci podręcznej odpowiedzi oprogramowania pośredniczącego w bajtach. Wartość domyślna to `100 * 1024 * 1024` (100 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | Określa, jeśli odpowiedzi są buforowane w ścieżkach uwzględniana wielkość liter. Wartość domyślna to `false`. |

Poniższy przykład umożliwia skonfigurowanie oprogramowaniu pośredniczącym, aby:

* Buforowanie odpowiedzi o rozmiarze treści mniejsze niż lub równa 1024 bajty.
* Store odpowiedzi przez ścieżki uwzględniana wielkość liter. Na przykład `/page1` i `/Page1` są przechowywane w innej lokalizacji.

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

Korzystając z MVC / sieci web interfejsu API kontrolerach ani w modelach strony stron Razor, [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) atrybut określa parametry, które są niezbędne do ustawiania odpowiednie nagłówki do buforowania odpowiedzi. Tylko parametr `[ResponseCache]` atrybut, który ściśle wymaga oprogramowania pośredniczącego jest <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, nie odpowiadać rzeczywistej nagłówka HTTP. Aby uzyskać więcej informacji, zobacz <xref:performance/caching/response#responsecache-attribute>.

Bez korzystania z `[ResponseCache]` atrybutu, buforowanie odpowiedzi może być zróżnicowane z `VaryByQueryKeys`. Użyj <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> bezpośrednio z [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

Za pomocą pojedynczej wartości równej `*` w `VaryByQueryKeys` różni się w pamięci podręcznej przez wszystkie parametry zapytania żądania.

## <a name="http-headers-used-by-response-caching-middleware"></a>Nagłówki HTTP używana przez oprogramowanie pośredniczące buforowania odpowiedzi

Poniższa tabela zawiera informacje dotyczące nagłówków HTTP, które mają wpływ na buforowanie odpowiedzi.

| nagłówek | Szczegóły |
| ------ | ------- |
| `Authorization` | Jeśli istnieje nagłówek odpowiedzi nie są buforowane. |
| `Cache-Control` | Tylko uwzględnia oprogramowanie pośredniczące buforowania odpowiedzi oznaczone `public` dyrektywy pamięci podręcznej. Kontrolowanie buforowania z następującymi parametrami:<ul><li>max-age</li><li>max-stale&#8224;</li><li>świeży min</li><li>must-revalidate</li><li>no-cache</li><li>nie-store</li><li>tylko jeśli-zapisanych w pamięci podręcznej</li><li>private</li><li>public</li><li>s maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224;Jeśli określono bez ograniczeń `max-stale`, oprogramowanie pośredniczące nie podejmuje żadnych działań.<br>&#8225;`proxy-revalidate`ma taki sam skutek jak `must-revalidate`.<br><br>Aby uzyskać więcej informacji, zobacz [RFC 7231: Żądanie dyrektyw sterujących pamięcią podręczną](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| `Pragma` | A `Pragma: no-cache` nagłówka w żądaniu daje taki sam skutek jak `Cache-Control: no-cache`. Ten nagłówek zostanie zastąpiona przez odpowiednie dyrektywy w `Cache-Control` nagłówka, jeśli jest obecny. Traktowane jako zgodności z poprzednimi wersjami przy użyciu protokołu HTTP/1.0. |
| `Set-Cookie` | Jeśli istnieje nagłówek odpowiedzi nie są buforowane. Wszelkie oprogramowanie pośredniczące w potoku przetwarzania żądań, który ustawia pliki cookie z co najmniej jeden zapobiega buforowanie odpowiedzi przez oprogramowanie pośredniczące buforowania odpowiedzi (na przykład [na podstawie plików cookie dostawcy TempData](xref:fundamentals/app-state#tempdata)).  |
| `Vary` | `Vary` Nagłówka służy różnicującej buforowane odpowiedzi innym nagłówkiem. Na przykład, buforują odpowiedzi przez kodowanie umieszczając `Vary: Accept-Encoding` nagłówka, który buforuje odpowiedzi dla żądań z nagłówkami `Accept-Encoding: gzip` i `Accept-Encoding: text/plain` oddzielnie. Odpowiedź o wartości nagłówka `*` nigdy nie jest przechowywany. |
| `Expires` | Uznane za przestarzałe przez ten nagłówek odpowiedzi nie jest przechowywany lub pobrać, jeśli zastąpiona przez inne `Cache-Control` nagłówków. |
| `If-None-Match` | Pełna odpowiedź jest obsługiwany z pamięci podręcznej, jeśli wartość nie jest `*` i `ETag` odpowiedzi nie pasuje do żadnej z podanych wartości. W przeciwnym razie odpowiedź 304 (nie zmodyfikowano) jest obsługiwany. |
| `If-Modified-Since` | Jeśli `If-None-Match` nagłówka nie jest obecny, pełnej odpowiedzi są dostarczane z pamięci podręcznej, jeśli data odpowiedzi z pamięci podręcznej jest nowsza niż podana wartość. W przeciwnym razie *304 - niezmodyfikowane* odpowiedzi jest obsługiwany. |
| `Date` | Przy wysyłaniu z pamięci podręcznej, `Date` nagłówek jest ustawiana przez oprogramowanie pośredniczące, jeśli nie podano w oryginalnej odpowiedzi. |
| `Content-Length` | Przy wysyłaniu z pamięci podręcznej, `Content-Length` nagłówek jest ustawiana przez oprogramowanie pośredniczące, jeśli nie podano w oryginalnej odpowiedzi. |
| `Age` | `Age` Wysyłane w oryginalnej odpowiedzi nagłówka zostanie zignorowany. Oprogramowanie pośredniczące oblicza nową wartość przy wysyłaniu odpowiedzi z pamięci podręcznej. |

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

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Warunki dotyczące buforowania

* Żądanie musi spowodować odpowiedź z serwera z kodem stanu 200 (OK).
* Metoda żądania musi być GET lub HEAD.
* W `Startup.Configure`, oprogramowanie pośredniczące buforowania odpowiedzi musi być umieszczony przed oprogramowania pośredniczącego, które wymagają buforowania. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.
* `Authorization` Nagłówka nie może być obecny.
* `Cache-Control` Parametry nagłówka musi być prawidłowy, a odpowiedzi muszą być oznaczone jako `public` , nie jest oznaczona `private`.
* `Pragma: no-cache` Nagłówka nie może być obecny Jeśli `Cache-Control` nagłówka nie jest obecny, jako `Cache-Control` zastępuje nagłówek `Pragma` nagłówka, jeśli jest obecny.
* `Set-Cookie` Nagłówka nie może być obecny.
* `Vary` Parametry nagłówka musi być prawidłowe i nie jest równa `*`.
* `Content-Length` Wartość nagłówka (jeśli ustawić) musi być zgodny z rozmiarem treść odpowiedzi.
* <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> Nie jest używany.
* Odpowiedź nie mogą być nieaktualne określony przez `Expires` nagłówka i `max-age` i `s-maxage` dyrektywy w pamięci podręcznej.
* Buforowanie odpowiedzi musi zakończyć się powodzeniem. Rozmiar odpowiedzi musi być mniejszy niż skonfigurowany lub domyślne <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>. Rozmiar treści odpowiedzi musi być mniejszy niż skonfigurowany lub domyślne <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.
* Odpowiedzi muszą być podlega buforowaniu, na podstawie położenia [RFC 7234](https://tools.ietf.org/html/rfc7234) specyfikacji. Na przykład `no-store` dyrektywy nie może istnieć w pola nagłówka żądania lub odpowiedzi. Zobacz *sekcja 3: Zapisywanie odpowiedzi w pamięci podręcznych* z [RFC 7234](https://tools.ietf.org/html/rfc7234) Aby uzyskać szczegółowe informacje.

> [!NOTE]
> Antiforgery systemu pod kątem generowania tokenów bezpieczne, aby zapobiec fałszerstwo żądania Międzywitrynowego Międzywitrynowych ataki zestawy `Cache-Control` i `Pragma` nagłówki, aby `no-cache` tak, aby odpowiedzi nie są buforowane. Aby uzyskać informacji na temat sposobu wyłączania antiforgery tokenów dla elementów formularza HTML, zobacz <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
