---
title: "Oprogramowanie pośredniczące w platformy ASP.NET Core buforowania odpowiedzi"
author: guardrex
description: "Dowiedz się, jak konfigurowanie i używanie oprogramowania pośredniczącego odpowiedzi buforowanie w ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/26/2017
ms.prod: asp.net-core
ms.topic: article
uid: performance/caching/middleware
ms.openlocfilehash: 78fa8fbe70eb7d6461b6e7340c6d57e330157911
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Oprogramowanie pośredniczące w platformy ASP.NET Core buforowania odpowiedzi

Przez [Luke Latham](https://github.com/guardrex) i [Luo Jan](https://github.com/JunTaoLuo)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

W tym artykule opisano sposób konfigurowania oprogramowania pośredniczącego odpowiedzi buforowanie w aplikacji platformy ASP.NET Core. Oprogramowanie pośredniczące Określa, kiedy buforowalnej są odpowiedzi, magazyny odpowiedzi i służy odpowiedzi z pamięci podręcznej. Aby obejrzeć wprowadzenie do buforowania HTTP i `ResponseCache` atrybutów, zobacz [buforowanie odpowiedzi](xref:performance/caching/response).

## <a name="package"></a>Package

Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) pakietu lub użyj [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) pakietu (platformy ASP.NET Core 2.0 lub nowszej, gdy przeznaczonych dla platformy .NET Core).

## <a name="configuration"></a>Konfiguracja

W `ConfigureServices`, Dodaj oprogramowanie pośredniczące do kolekcji usługi.

[!code-csharp[Main](middleware/sample/Startup.cs?name=snippet1&highlight=3)]

Skonfiguruj aplikację do używania oprogramowania pośredniczącego z `UseResponseCaching` metodę rozszerzenia, która dodaje oprogramowanie pośredniczące w potoku przetwarzania żądań. Przykładowa aplikacja dodaje [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) nagłówka odpowiedzi, który buforuje buforowalnej odpowiedzi do 10 sekund. Wysyła próbki [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) nagłówka do konfiguracji oprogramowania pośredniczącego do obsługi tylko wtedy, gdy odpowiedź buforowana [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) nagłówka kolejne żądania jest zgodna z wersją z oryginalnego żądania.

[!code-csharp[Main](middleware/sample/Startup.cs?name=snippet2&highlight=3,7-12)]

Oprogramowanie pośredniczące odpowiedzi buforowanie tylko buforuje odpowiedzi serwera, które powodują powstanie kod stanu 200 (OK). Inne odpowiedzi, w tym [stron błędów](xref:fundamentals/error-handling), są ignorowane przez oprogramowanie pośredniczące.

> [!WARNING]
> Odpowiedzi z zawartością, uwierzytelnionym klientów musi być oznaczona jako nie buforowalnej, aby zapobiec oprogramowania pośredniczącego z przechowywania i obsługująca tych odpowiedzi. Zobacz [warunki buforowanie](#conditions-for-caching) szczegółowe informacje na temat sposobu oprogramowania pośredniczącego Określa, czy odpowiedź jest buforowalnej.

## <a name="options"></a>Opcje

Oprogramowanie pośredniczące udostępnia trzy opcje do kontrolowania buforowania odpowiedzi.

| Opcja                | Wartość domyślna |
| --------------------- | ------------- |
| UseCaseSensitivePaths | Określa, czy odpowiedzi są buforowane w ścieżkach, z uwzględnieniem wielkości liter.</p><p>Wartość domyślna to `false`. |
| MaximumBodySize       | Największy rozmiar buforowalnej treść odpowiedzi w bajtach.</p>Wartość domyślna to `64 * 1024 * 1024` (64 MB). |
| SizeLimit             | Limit rozmiaru pamięci podręcznej odpowiedzi oprogramowania pośredniczącego w bajtach. Wartość domyślna to `100 * 1024 * 1024` (100 MB). |

Poniższy przykład konfiguruje oprogramowaniu pośredniczącym, aby:

* Odpowiedzi pamięci podręcznej jest mniejszy niż lub równy 1024 bajty.
* Przechowywanie odpowiedzi przez ścieżki z uwzględnieniem wielkości liter (na przykład `/page1` i `/Page1` są przechowywane w innej lokalizacji).

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

Podczas korzystania z kontrolerów interfejsu API MVC/sieci Web lub stron Razor strony modeli, `ResponseCache` atrybut określa parametry niezbędne do ustawiania odpowiednich nagłówki buforowanie odpowiedzi. Tylko parametr `ResponseCache` jest atrybut, który wymaga ściśle oprogramowanie pośredniczące `VaryByQueryKeys`, które nie odpowiadają rzeczywiste nagłówka HTTP. Aby uzyskać więcej informacji, zobacz [ResponseCache atrybutu](xref:performance/caching/response#responsecache-attribute).

Korzystając z nie `ResponseCache` atrybutu, buforowanie odpowiedzi można zmieniać z `VaryByQueryKeys` funkcji. Użyj `ResponseCachingFeature` bezpośrednio z `IFeatureCollection` z `HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

## <a name="http-headers-used-by-response-caching-middleware"></a>Nagłówki HTTP używana przez oprogramowanie pośredniczące buforowanie w odpowiedzi

Buforowanie odpowiedzi przez oprogramowanie pośredniczące jest konfigurowana przy użyciu nagłówków HTTP.

| nagłówek | Szczegóły |
| ------ | ------- |
| Autoryzacja | Jeśli istnieje nagłówek odpowiedzi nie są buforowane. |
| Cache-Control | Oprogramowanie pośredniczące uwzględnia tylko buforowanie odpowiedzi oznaczonych `public` dyrektywy pamięci podręcznej. Sterowanie buforowaniem z następującymi parametrami:<ul><li>Maksymalny wiek</li><li>max-stale&#8224;</li><li>świeża min</li><li>must-revalidate</li><li>no-cache</li><li>nie magazynu</li><li>tylko w przypadku-zapisanych w pamięci podręcznej</li><li>private</li><li>public</li><li>s-maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224; Jeśli nie limitu `max-stale`, oprogramowanie pośredniczące nie podejmuje żadnych akcji.<br>&#8225; `proxy-revalidate` ma ten sam efekt co `must-revalidate`.<br><br>Aby uzyskać więcej informacji, zobacz [RFC 7231: żądanie dyrektywy Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| Wartość dyrektywy pragma | A `Pragma: no-cache` nagłówka w żądaniu tworzy ten sam efekt co `Cache-Control: no-cache`. Ten nagłówek zostanie zastąpiona przez odpowiednie dyrektywy w `Cache-Control` nagłówka, jeśli jest obecny. Rozważyć zgodności z poprzednimi wersjami z protokołu HTTP/1.0. |
| Set-Cookie | Jeśli istnieje nagłówek odpowiedzi nie są buforowane. |
| różnią się | `Vary` Nagłówka jest używana będzie różnicować buforowane odpowiedzi przez inny nagłówek. Na przykład buforuje odpowiedzi przez kodowania, w tym `Vary: Accept-Encoding` nagłówek, który buforuje odpowiedzi dla żądań z nagłówkami `Accept-Encoding: gzip` i `Accept-Encoding: text/plain` osobno. Odpowiedzi o wartości nagłówka `*` nigdy nie są przechowywane. |
| Wygasa | Uznane za przestarzałe przez ten nagłówek odpowiedzi nie jest przechowywany lub pobrać, chyba że zastąpione przez inne `Cache-Control` nagłówków. |
| If-None-Match | Całą odpowiedź jest udostępniany z pamięci podręcznej, jeśli wartość nie jest `*` i `ETag` odpowiedzi nie pasuje do żadnego z podanych wartości. W przeciwnym razie odpowiedź 304 (nie jest modyfikowany) jest obsługiwana. |
| If-Modified-Since | Jeśli `If-None-Match` nagłówka nie jest obecny, całą odpowiedź jest udostępniany z pamięci podręcznej, jeśli data buforowanej odpowiedzi jest nowsza niż podana wartość. W przeciwnym razie odpowiedź 304 (nie jest modyfikowany) jest obsługiwana. |
| Data | Przy wysyłaniu z pamięci podręcznej, `Date` nagłówka jest ustawiony przez oprogramowanie pośredniczące, jeśli nie podano w oryginalnym odpowiedzi. |
| Długość zawartości | Przy wysyłaniu z pamięci podręcznej, `Content-Length` nagłówka jest ustawiony przez oprogramowanie pośredniczące, jeśli nie podano w oryginalnym odpowiedzi. |
| okres ważności | `Age` Wysyłany w odpowiedzi oryginalnego nagłówka zostanie zignorowany. Oprogramowanie pośredniczące oblicza nową wartość przy wysyłaniu buforowanej odpowiedzi. |

## <a name="caching-respects-request-cache-control-directives"></a>Buforowanie szanuje dyrektywy Cache-Control żądania

Oprogramowanie pośredniczące przestrzega zasad [specyfikacji protokołu HTTP 1.1 buforowanie](https://tools.ietf.org/html/rfc7234#section-5.2). Zasady wymagają pamięci podręcznej uwzględnić prawidłową `Cache-Control` nagłówka wysłane przez klienta. W obszarze specyfikację klienta mogą wysyłać żądania z `no-cache` wartość nagłówka i Wymuś serwera, aby wygenerować nową odpowiedź na każde żądanie. Obecnie nie są developer nie kontroluje zachowanie buforowania w tym gdy za pomocą oprogramowania pośredniczącego, ponieważ oprogramowanie pośredniczące działa zgodnie z oficjalnego specyfikacji buforowania.

Aby uzyskać większą kontrolę nad zachowanie buforowania eksplorowania innych funkcji buforowania platformy ASP.NET Core. Zobacz następujące tematy:

* [Buforowanie w pamięci](xref:performance/caching/memory)
* [Praca z rozproszonej pamięci podręcznej](xref:performance/caching/distributed)
* [Buforuj pomocnika tagów w podstawowej platformy ASP.NET MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Pomocnik tagu rozproszonej pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Zachowanie buforowania nie jest zgodnie z oczekiwaniami, upewnij się, że odpowiedzi są buforowalnej i stanie obsługiwanej z pamięci podręcznej. Sprawdź, czy nagłówki przychodzącego żądania i odpowiedzi nagłówków wychodzących. Włącz [rejestrowanie](xref:fundamentals/logging/index) ułatwiające debugowania.

Podczas testowania i rozwiązywania problemów zachowanie buforowania, przeglądarki mogą ustawiać nagłówków żądań, wpływających na buforowanie niepożądanych sposoby. Na przykład może ustawić przeglądarką `Cache-Control` nagłówka do `no-cache` lub `max-age=0` podczas odświeżania strony. Następujące narzędzia można jawnie ustawia nagłówki żądania i jest preferowane w przypadku testowania buforowania:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Warunki do buforowania

* Żądanie może skutkować odpowiedź serwera z kodem stanu 200 (OK).
* Metoda żądania musi być GET lub HEAD.
* Oprogramowanie pośredniczące terminali, takich jak [oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files), nie może przetwarzać odpowiedzi przed oprogramowanie pośredniczące buforowanie odpowiedzi.
* `Authorization` Nagłówka nie może być obecny.
* `Cache-Control`Parametry nagłówka musi być prawidłowy, a odpowiedzi muszą być oznaczone jako `public` i nie oznaczone `private`.
* `Pragma: no-cache` Nagłówek nie musi występować Jeśli `Cache-Control` nagłówka nie jest obecny jako `Cache-Control` zastępuje nagłówka `Pragma` nagłówka, jeśli jest obecny.
* `Set-Cookie` Nagłówka nie może być obecny.
* `Vary`Nagłówek parametry muszą być prawidłowe i nie jest równa `*`.
* `Content-Length` Wartość nagłówka (Jeśli ustawiona) muszą być zgodne rozmiar treść odpowiedzi.
* [IHttpSendFileFeature](/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) nie jest używany.
* Odpowiedź nie może być starych określony przez `Expires` nagłówka i `max-age` i `s-maxage` pamięci podręcznej dyrektywy.
* Buforowanie odpowiedzi musi zakończyć się powodzeniem, a rozmiar odpowiedzi musi być mniejszy niż skonfigurowany lub domyślna `SizeLimit`.
* Odpowiedź musi być buforowalnej zgodnie z [RFC 7234](https://tools.ietf.org/html/rfc7234) specyfikacji. Na przykład `no-store` dyrektywy nie może istnieć w pola nagłówka żądania lub odpowiedzi. Zobacz *sekcja 3: odpowiedzi na przechowywanie w pamięci podręcznej* z [RFC 7234](https://tools.ietf.org/html/rfc7234) szczegółowe informacje.

> [!NOTE]
> Antiforgery systemu do generowania tokenów bezpiecznego w celu zapobiegania fałszowaniu żądań Cross-Site (CSRF) przed atakami opartymi na zestawy `Cache-Control` i `Pragma` nagłówków `no-cache` tak, aby odpowiedzi nie są buforowane.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Uruchamianie aplikacji](xref:fundamentals/startup)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Buforowanie w pamięci](xref:performance/caching/memory)
* [Praca z rozproszonej pamięci podręcznej](xref:performance/caching/distributed)
* [Wykrywanie zmian z tokenami zmiany](xref:fundamentals/primitives/change-tokens)
* [Buforowanie odpowiedzi](xref:performance/caching/response)
* [Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Pomocnik tagu rozproszonej pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
