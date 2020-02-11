---
title: Buforowanie oprogramowania pośredniczącego w ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować i używać oprogramowania pośredniczącego buforowania odpowiedzi w ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: performance/caching/middleware
ms.openlocfilehash: 61fa42161560ce2b512a73f1d7e32d11cd9bcb2c
ms.sourcegitcommit: 235623b6e5a5d1841139c82a11ac2b4b3f31a7a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/10/2020
ms.locfileid: "77114799"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Buforowanie oprogramowania pośredniczącego w ASP.NET Core

Autorzy [Luke Latham](https://github.com/guardrex) i [Jan Luo](https://github.com/JunTaoLuo)

::: moniker range=">= aspnetcore-3.0"

W tym artykule opisano sposób konfigurowania oprogramowania pośredniczącego buforowania odpowiedzi w aplikacji ASP.NET Core. Oprogramowanie pośredniczące określa, kiedy odpowiedzi są buforowane, przechowuje odpowiedzi i obsługuje odpowiedzi z pamięci podręcznej. Aby uzyskać wprowadzenie do buforowania HTTP i atrybutu [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) , zobacz [buforowanie odpowiedzi](xref:performance/caching/response).

[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="configuration"></a>Konfiguracja

Oprogramowanie pośredniczące buforowania odpowiedzi jest niejawnie dostępne dla aplikacji ASP.NET Core za pośrednictwem udostępnionej platformy.

W `Startup.ConfigureServices`Dodaj oprogramowanie pośredniczące buforowania odpowiedzi do kolekcji usług:

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

Skonfiguruj aplikację do korzystania z oprogramowania pośredniczącego z metodą rozszerzenia <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*>, która dodaje oprogramowanie pośredniczące do potoku przetwarzania żądań w `Startup.Configure`:

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=16)]

Przykładowa aplikacja dodaje nagłówki, aby kontrolować buforowanie w kolejnych żądaniach:

* &ndash; [kontroli pamięci](https://tools.ietf.org/html/rfc7234#section-5.2) podręcznej buforuje odpowiedzi w pamięci podręcznej przez maksymalnie 10 sekund.
* [Różne](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; konfiguruje oprogramowanie pośredniczące, aby obsługiwało buforowaną odpowiedź tylko wtedy, gdy nagłówek [Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4) kolejnych żądań jest zgodny z oryginalnym żądaniem.

[!code-csharp[](middleware/samples_snippets/3.x/AddHeaders.cs)]

Buforowanie odpowiedzi w pamięci podręcznej tylko buforuje odpowiedzi serwera, które powodują, że jest to kod stanu 200 (OK). Wszystkie inne odpowiedzi, w tym [strony błędów](xref:fundamentals/error-handling), są ignorowane przez oprogramowanie pośredniczące.

> [!WARNING]
> Odpowiedzi zawierające zawartość uwierzytelnionych klientów muszą być oznaczone jako nieobsługujące pamięci podręcznej, aby uniemożliwić przechowywanie i obsługę tych odpowiedzi przez oprogramowanie pośredniczące. Zobacz [warunki buforowania](#conditions-for-caching) , aby uzyskać szczegółowe informacje na temat sposobu, w jaki oprogramowanie pośredniczące określa, czy odpowiedź jest buforowana.

## <a name="options"></a>Opcje

Opcje buforowania odpowiedzi przedstawiono w poniższej tabeli.

| Opcja | Opis |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | Największy rozmiar pamięci podręcznej dla treści odpowiedzi (w bajtach). Wartość domyślna to `64 * 1024 * 1024` (64 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | Limit rozmiaru dla oprogramowania pośredniczącego pamięci podręcznej odpowiedzi w bajtach. Wartość domyślna to `100 * 1024 * 1024` (100 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | Określa, czy odpowiedzi są buforowane w przypadku ścieżek z uwzględnieniem wielkości liter. Wartość domyślna to `false`. |

Poniższy przykład konfiguruje oprogramowanie pośredniczące w następujący sposób:

* Buforuj odpowiedzi o rozmiarze treści mniejszym lub równym 1 024 bajtów.
* Przechowaj odpowiedzi w ścieżce z uwzględnieniem wielkości liter. Na przykład `/page1` i `/Page1` są przechowywane oddzielnie.

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

W przypadku używania kontrolerów MVC/Web API lub Razor Pages modeli strony atrybut [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) określa parametry niezbędne do ustawiania odpowiednich nagłówków dla buforowania odpowiedzi. Jedyny parametr `[ResponseCache]` atrybutu, który ściśle wymaga oprogramowania pośredniczącego, jest <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, który nie odpowiada rzeczywistemu nagłówkowi HTTP. Aby uzyskać więcej informacji, zobacz <xref:performance/caching/response#responsecache-attribute>.

Gdy nie korzystasz z atrybutu `[ResponseCache]`, buforowanie odpowiedzi może być zróżnicowane z `VaryByQueryKeys`. Użyj <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> bezpośrednio z obiektu [HttpContext. Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

Użycie pojedynczej wartości równej `*` w `VaryByQueryKeys` różnicuje pamięć podręczną przez wszystkie parametry zapytania żądania.

## <a name="http-headers-used-by-response-caching-middleware"></a>Nagłówki HTTP używane przez oprogramowanie pośredniczące buforowania odpowiedzi

Poniższa tabela zawiera informacje dotyczące nagłówków HTTP, które mają wpływ na buforowanie odpowiedzi.

| nagłówek | Szczegóły |
| ------ | ------- |
| `Authorization` | Odpowiedź nie jest buforowana, jeśli nagłówek istnieje. |
| `Cache-Control` | Oprogramowanie pośredniczące traktuje tylko odpowiedzi w pamięci podręcznej oznaczone za pomocą dyrektywy Cache `public`. Sterowanie buforowaniem przy użyciu następujących parametrów:<ul><li>maks. wiek</li><li>max-stale&#8224;</li><li>min — nowy</li><li>must-revalidate</li><li>no-cache</li><li>bez sklepu</li><li>tylko-w pamięci podręcznej</li><li>private</li><li>public</li><li>s-maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224;Jeśli żaden limit nie zostanie określony do `max-stale`, oprogramowanie pośredniczące nie podejmuje żadnych działań.<br>&#8225;`proxy-revalidate` ma ten sam skutek co `must-revalidate`.<br><br>Aby uzyskać więcej informacji, zobacz [RFC 7231: dyrektywy kontroli pamięci podręcznej żądania](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| `Pragma` | Nagłówek `Pragma: no-cache` w żądaniu daje ten sam skutek co `Cache-Control: no-cache`. Ten nagłówek jest zastępowany przez odpowiednie dyrektywy w nagłówku `Cache-Control`, jeśli jest obecny. Uwzględnianie zgodności z poprzednimi wersjami przy użyciu protokołu HTTP/1.0. |
| `Set-Cookie` | Odpowiedź nie jest buforowana, jeśli nagłówek istnieje. Wszystkie oprogramowanie pośredniczące w potoku przetwarzania żądań, które ustawia jeden lub więcej plików cookie, uniemożliwia buforowanie odpowiedzi z buforowania odpowiedzi (na przykład [dostawcy TempData opartego na plikach cookie](xref:fundamentals/app-state#tempdata)).  |
| `Vary` | Nagłówek `Vary` jest używany do różnicowania buforowanej odpowiedzi przez inny nagłówek. Na przykład w pamięci podręcznej odpowiedzi są kodowane przez uwzględnienie nagłówka `Vary: Accept-Encoding`, który buforuje odpowiedzi na żądania z nagłówkami `Accept-Encoding: gzip` i `Accept-Encoding: text/plain` oddzielnie. Odpowiedź z wartością nagłówka `*` nigdy nie jest przechowywana. |
| `Expires` | Odpowiedź uznana za nieodświeżoną przez ten nagłówek nie jest zapisywana ani pobierana, chyba że zostanie zastąpiona przez inne nagłówki `Cache-Control`. |
| `If-None-Match` | Pełna odpowiedź jest obsługiwana z pamięci podręcznej, jeśli wartość nie jest `*` i `ETag` odpowiedzi nie pasuje do żadnej z podanych wartości. W przeciwnym razie jest obsługiwana odpowiedź 304 (nie modyfikowane). |
| `If-Modified-Since` | Jeśli nagłówek `If-None-Match` nie istnieje, jest obsługiwana Pełna odpowiedź z pamięci podręcznej, jeśli data odpowiedzi w pamięci podręcznej jest nowsza niż podana wartość. W przeciwnym razie jest obsługiwana odpowiedź *304 — nie zmodyfikowano* . |
| `Date` | W przypadku obsługi z pamięci podręcznej nagłówek `Date` jest ustawiany przez oprogramowanie pośredniczące, jeśli nie został podany w oryginalnej odpowiedzi. |
| `Content-Length` | W przypadku obsługi z pamięci podręcznej nagłówek `Content-Length` jest ustawiany przez oprogramowanie pośredniczące, jeśli nie został podany w oryginalnej odpowiedzi. |
| `Age` | Nagłówek `Age` wysłany w oryginalnej odpowiedzi jest ignorowany. Oprogramowanie pośredniczące oblicza nową wartość w przypadku obsługi odpowiedzi w pamięci podręcznej. |

## <a name="caching-respects-request-cache-control-directives"></a>Buforowanie — dyrektywy dotyczące kontroli pamięci podręcznej żądań

Oprogramowanie pośredniczące uwzględnia reguły [buforowania HTTP 1,1](https://tools.ietf.org/html/rfc7234#section-5.2). Reguły wymagają pamięci podręcznej, aby móc przestrzegać prawidłowego nagłówka `Cache-Control` wysyłanego przez klienta. W obszarze Specyfikacja klient może wykonywać żądania o wartości nagłówka `no-cache` i wymusić, aby serwer generował nową odpowiedź dla każdego żądania. Obecnie nie ma kontroli nad tym zachowaniem buforowania podczas korzystania z oprogramowania pośredniczącego, ponieważ oprogramowanie pośredniczące jest zgodne z oficjalną specyfikacją buforowania.

Aby uzyskać większą kontrolę nad zachowaniem buforowania, zapoznaj się z innymi funkcjami buforowania ASP.NET Core. Zobacz następujące tematy:

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli zachowanie buforowania nie jest zgodne z oczekiwaniami, upewnij się, że odpowiedzi są buforowane i mogą być obsługiwane z pamięci podręcznej. Sprawdź przychodzące nagłówki żądania i wychodzące nagłówki odpowiedzi. Włącz [Rejestrowanie](xref:fundamentals/logging/index) , aby pomóc w debugowaniu.

Podczas testowania i rozwiązywania problemów z pamięcią podręczną przeglądarka może ustawić nagłówki żądań, które mają wpływ na buforowanie na różne sposoby. Na przykład przeglądarka może ustawić `Cache-Control` nagłówek do `no-cache` lub `max-age=0` podczas odświeżania strony. Poniższe narzędzia mogą jawnie ustawić nagłówki żądań i są preferowane w celu przeprowadzenia testu w pamięci podręcznej:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Warunki buforowania

* Żądanie musi spowodować odpowiedź serwera z kodem stanu 200 (OK).
* Metoda żądania musi mieć wartość GET lub $.
* W `Startup.Configure`należy umieścić oprogramowanie pośredniczące buforowania odpowiedzi przed oprogramowanie pośredniczące wymagające buforowania. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.
* Nagłówek `Authorization` nie może być obecny.
* parametry nagłówka `Cache-Control` muszą być prawidłowe, a odpowiedź musi być oznaczona jako `public`, a nie oznaczona jako `private`.
* Nagłówek `Pragma: no-cache` nie może być obecny, jeśli nagłówek `Cache-Control` nie istnieje, ponieważ nagłówek `Cache-Control` zastępuje nagłówek `Pragma`, jeśli jest obecny.
* Nagłówek `Set-Cookie` nie może być obecny.
* parametry nagłówka `Vary` muszą być prawidłowe i nie są równe `*`.
* Wartość nagłówka `Content-Length` (jeśli jest ustawiona) musi być zgodna z rozmiarem treści odpowiedzi.
* <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> nie jest używana.
* Odpowiedź nie może być przestarzała zgodnie z definicją `Expires` nagłówka i `max-age` i `s-maxage` pamięci podręcznej.
* Buforowanie odpowiedzi musi się powieść. Rozmiar odpowiedzi musi być mniejszy niż skonfigurowany lub domyślny <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>. Rozmiar treści odpowiedzi musi być mniejszy niż skonfigurowany lub domyślny <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.
* Odpowiedź musi być buforowana zgodnie ze specyfikacją [RFC 7234](https://tools.ietf.org/html/rfc7234) . Na przykład dyrektywa `no-store` nie może istnieć w polach żądania ani nagłówka odpowiedzi. Zobacz *sekcję 3: przechowywanie odpowiedzi w pamięci podręcznej* [RFC 7234](https://tools.ietf.org/html/rfc7234) , aby uzyskać szczegółowe informacje.

> [!NOTE]
> System antysfałszowany do generowania zabezpieczonych tokenów, aby zapobiec atakom CSRFm między lokacjami, ustawia nagłówki `Cache-Control` i `Pragma` do `no-cache`, tak aby odpowiedzi nie były buforowane. Aby uzyskać informacje na temat sposobu wyłączania tokenów antysfałszowanych dla elementów formularza HTML, zobacz <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

W tym artykule opisano sposób konfigurowania oprogramowania pośredniczącego buforowania odpowiedzi w aplikacji ASP.NET Core. Oprogramowanie pośredniczące określa, kiedy odpowiedzi są buforowane, przechowuje odpowiedzi i obsługuje odpowiedzi z pamięci podręcznej. Aby uzyskać wprowadzenie do buforowania HTTP i atrybutu [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) , zobacz [buforowanie odpowiedzi](xref:performance/caching/response).

[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="configuration"></a>Konfiguracja

Użyj pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. AspNetCore. ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) .

W `Startup.ConfigureServices`Dodaj oprogramowanie pośredniczące buforowania odpowiedzi do kolekcji usług:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

Skonfiguruj aplikację do korzystania z oprogramowania pośredniczącego z metodą rozszerzenia <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*>, która dodaje oprogramowanie pośredniczące do potoku przetwarzania żądań w `Startup.Configure`:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

Przykładowa aplikacja dodaje nagłówki, aby kontrolować buforowanie w kolejnych żądaniach:

* &ndash; [kontroli pamięci](https://tools.ietf.org/html/rfc7234#section-5.2) podręcznej buforuje odpowiedzi w pamięci podręcznej przez maksymalnie 10 sekund.
* [Różne](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; konfiguruje oprogramowanie pośredniczące, aby obsługiwało buforowaną odpowiedź tylko wtedy, gdy nagłówek [Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4) kolejnych żądań jest zgodny z oryginalnym żądaniem.

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

Buforowanie odpowiedzi w pamięci podręcznej tylko buforuje odpowiedzi serwera, które powodują, że jest to kod stanu 200 (OK). Wszystkie inne odpowiedzi, w tym [strony błędów](xref:fundamentals/error-handling), są ignorowane przez oprogramowanie pośredniczące.

> [!WARNING]
> Odpowiedzi zawierające zawartość uwierzytelnionych klientów muszą być oznaczone jako nieobsługujące pamięci podręcznej, aby uniemożliwić przechowywanie i obsługę tych odpowiedzi przez oprogramowanie pośredniczące. Zobacz [warunki buforowania](#conditions-for-caching) , aby uzyskać szczegółowe informacje na temat sposobu, w jaki oprogramowanie pośredniczące określa, czy odpowiedź jest buforowana.

## <a name="options"></a>Opcje

Opcje buforowania odpowiedzi przedstawiono w poniższej tabeli.

| Opcja | Opis |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | Największy rozmiar pamięci podręcznej dla treści odpowiedzi (w bajtach). Wartość domyślna to `64 * 1024 * 1024` (64 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | Limit rozmiaru dla oprogramowania pośredniczącego pamięci podręcznej odpowiedzi w bajtach. Wartość domyślna to `100 * 1024 * 1024` (100 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | Określa, czy odpowiedzi są buforowane w przypadku ścieżek z uwzględnieniem wielkości liter. Wartość domyślna to `false`. |

Poniższy przykład konfiguruje oprogramowanie pośredniczące w następujący sposób:

* Buforuj odpowiedzi o rozmiarze treści mniejszym lub równym 1 024 bajtów.
* Przechowaj odpowiedzi w ścieżce z uwzględnieniem wielkości liter. Na przykład `/page1` i `/Page1` są przechowywane oddzielnie.

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

W przypadku używania kontrolerów MVC/Web API lub Razor Pages modeli strony atrybut [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) określa parametry niezbędne do ustawiania odpowiednich nagłówków dla buforowania odpowiedzi. Jedyny parametr `[ResponseCache]` atrybutu, który ściśle wymaga oprogramowania pośredniczącego, jest <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, który nie odpowiada rzeczywistemu nagłówkowi HTTP. Aby uzyskać więcej informacji, zobacz <xref:performance/caching/response#responsecache-attribute>.

Gdy nie korzystasz z atrybutu `[ResponseCache]`, buforowanie odpowiedzi może być zróżnicowane z `VaryByQueryKeys`. Użyj <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> bezpośrednio z obiektu [HttpContext. Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

Użycie pojedynczej wartości równej `*` w `VaryByQueryKeys` różnicuje pamięć podręczną przez wszystkie parametry zapytania żądania.

## <a name="http-headers-used-by-response-caching-middleware"></a>Nagłówki HTTP używane przez oprogramowanie pośredniczące buforowania odpowiedzi

Poniższa tabela zawiera informacje dotyczące nagłówków HTTP, które mają wpływ na buforowanie odpowiedzi.

| nagłówek | Szczegóły |
| ------ | ------- |
| `Authorization` | Odpowiedź nie jest buforowana, jeśli nagłówek istnieje. |
| `Cache-Control` | Oprogramowanie pośredniczące traktuje tylko odpowiedzi w pamięci podręcznej oznaczone za pomocą dyrektywy Cache `public`. Sterowanie buforowaniem przy użyciu następujących parametrów:<ul><li>maks. wiek</li><li>max-stale&#8224;</li><li>min — nowy</li><li>must-revalidate</li><li>no-cache</li><li>bez sklepu</li><li>tylko-w pamięci podręcznej</li><li>private</li><li>public</li><li>s-maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224;Jeśli żaden limit nie zostanie określony do `max-stale`, oprogramowanie pośredniczące nie podejmuje żadnych działań.<br>&#8225;`proxy-revalidate` ma ten sam skutek co `must-revalidate`.<br><br>Aby uzyskać więcej informacji, zobacz [RFC 7231: dyrektywy kontroli pamięci podręcznej żądania](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| `Pragma` | Nagłówek `Pragma: no-cache` w żądaniu daje ten sam skutek co `Cache-Control: no-cache`. Ten nagłówek jest zastępowany przez odpowiednie dyrektywy w nagłówku `Cache-Control`, jeśli jest obecny. Uwzględnianie zgodności z poprzednimi wersjami przy użyciu protokołu HTTP/1.0. |
| `Set-Cookie` | Odpowiedź nie jest buforowana, jeśli nagłówek istnieje. Wszystkie oprogramowanie pośredniczące w potoku przetwarzania żądań, które ustawia jeden lub więcej plików cookie, uniemożliwia buforowanie odpowiedzi z buforowania odpowiedzi (na przykład [dostawcy TempData opartego na plikach cookie](xref:fundamentals/app-state#tempdata)).  |
| `Vary` | Nagłówek `Vary` jest używany do różnicowania buforowanej odpowiedzi przez inny nagłówek. Na przykład w pamięci podręcznej odpowiedzi są kodowane przez uwzględnienie nagłówka `Vary: Accept-Encoding`, który buforuje odpowiedzi na żądania z nagłówkami `Accept-Encoding: gzip` i `Accept-Encoding: text/plain` oddzielnie. Odpowiedź z wartością nagłówka `*` nigdy nie jest przechowywana. |
| `Expires` | Odpowiedź uznana za nieodświeżoną przez ten nagłówek nie jest zapisywana ani pobierana, chyba że zostanie zastąpiona przez inne nagłówki `Cache-Control`. |
| `If-None-Match` | Pełna odpowiedź jest obsługiwana z pamięci podręcznej, jeśli wartość nie jest `*` i `ETag` odpowiedzi nie pasuje do żadnej z podanych wartości. W przeciwnym razie jest obsługiwana odpowiedź 304 (nie modyfikowane). |
| `If-Modified-Since` | Jeśli nagłówek `If-None-Match` nie istnieje, jest obsługiwana Pełna odpowiedź z pamięci podręcznej, jeśli data odpowiedzi w pamięci podręcznej jest nowsza niż podana wartość. W przeciwnym razie jest obsługiwana odpowiedź *304 — nie zmodyfikowano* . |
| `Date` | W przypadku obsługi z pamięci podręcznej nagłówek `Date` jest ustawiany przez oprogramowanie pośredniczące, jeśli nie został podany w oryginalnej odpowiedzi. |
| `Content-Length` | W przypadku obsługi z pamięci podręcznej nagłówek `Content-Length` jest ustawiany przez oprogramowanie pośredniczące, jeśli nie został podany w oryginalnej odpowiedzi. |
| `Age` | Nagłówek `Age` wysłany w oryginalnej odpowiedzi jest ignorowany. Oprogramowanie pośredniczące oblicza nową wartość w przypadku obsługi odpowiedzi w pamięci podręcznej. |

## <a name="caching-respects-request-cache-control-directives"></a>Buforowanie — dyrektywy dotyczące kontroli pamięci podręcznej żądań

Oprogramowanie pośredniczące uwzględnia reguły [buforowania HTTP 1,1](https://tools.ietf.org/html/rfc7234#section-5.2). Reguły wymagają pamięci podręcznej, aby móc przestrzegać prawidłowego nagłówka `Cache-Control` wysyłanego przez klienta. W obszarze Specyfikacja klient może wykonywać żądania o wartości nagłówka `no-cache` i wymusić, aby serwer generował nową odpowiedź dla każdego żądania. Obecnie nie ma kontroli nad tym zachowaniem buforowania podczas korzystania z oprogramowania pośredniczącego, ponieważ oprogramowanie pośredniczące jest zgodne z oficjalną specyfikacją buforowania.

Aby uzyskać większą kontrolę nad zachowaniem buforowania, zapoznaj się z innymi funkcjami buforowania ASP.NET Core. Zobacz następujące tematy:

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli zachowanie buforowania nie jest zgodne z oczekiwaniami, upewnij się, że odpowiedzi są buforowane i mogą być obsługiwane z pamięci podręcznej. Sprawdź przychodzące nagłówki żądania i wychodzące nagłówki odpowiedzi. Włącz [Rejestrowanie](xref:fundamentals/logging/index) , aby pomóc w debugowaniu.

Podczas testowania i rozwiązywania problemów z pamięcią podręczną przeglądarka może ustawić nagłówki żądań, które mają wpływ na buforowanie na różne sposoby. Na przykład przeglądarka może ustawić `Cache-Control` nagłówek do `no-cache` lub `max-age=0` podczas odświeżania strony. Poniższe narzędzia mogą jawnie ustawić nagłówki żądań i są preferowane w celu przeprowadzenia testu w pamięci podręcznej:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Warunki buforowania

* Żądanie musi spowodować odpowiedź serwera z kodem stanu 200 (OK).
* Metoda żądania musi mieć wartość GET lub $.
* W `Startup.Configure`należy umieścić oprogramowanie pośredniczące buforowania odpowiedzi przed oprogramowanie pośredniczące wymagające buforowania. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.
* Nagłówek `Authorization` nie może być obecny.
* parametry nagłówka `Cache-Control` muszą być prawidłowe, a odpowiedź musi być oznaczona jako `public`, a nie oznaczona jako `private`.
* Nagłówek `Pragma: no-cache` nie może być obecny, jeśli nagłówek `Cache-Control` nie istnieje, ponieważ nagłówek `Cache-Control` zastępuje nagłówek `Pragma`, jeśli jest obecny.
* Nagłówek `Set-Cookie` nie może być obecny.
* parametry nagłówka `Vary` muszą być prawidłowe i nie są równe `*`.
* Wartość nagłówka `Content-Length` (jeśli jest ustawiona) musi być zgodna z rozmiarem treści odpowiedzi.
* <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> nie jest używana.
* Odpowiedź nie może być przestarzała zgodnie z definicją `Expires` nagłówka i `max-age` i `s-maxage` pamięci podręcznej.
* Buforowanie odpowiedzi musi się powieść. Rozmiar odpowiedzi musi być mniejszy niż skonfigurowany lub domyślny <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>. Rozmiar treści odpowiedzi musi być mniejszy niż skonfigurowany lub domyślny <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.
* Odpowiedź musi być buforowana zgodnie ze specyfikacją [RFC 7234](https://tools.ietf.org/html/rfc7234) . Na przykład dyrektywa `no-store` nie może istnieć w polach żądania ani nagłówka odpowiedzi. Zobacz *sekcję 3: przechowywanie odpowiedzi w pamięci podręcznej* [RFC 7234](https://tools.ietf.org/html/rfc7234) , aby uzyskać szczegółowe informacje.

> [!NOTE]
> System antysfałszowany do generowania zabezpieczonych tokenów, aby zapobiec atakom CSRFm między lokacjami, ustawia nagłówki `Cache-Control` i `Pragma` do `no-cache`, tak aby odpowiedzi nie były buforowane. Aby uzyskać informacje na temat sposobu wyłączania tokenów antysfałszowanych dla elementów formularza HTML, zobacz <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end
