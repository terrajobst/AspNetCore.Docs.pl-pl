---
title: Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak CORS jako standardowe rozwiązanie dla zezwolenie lub odrzucenie żądań cross-origin w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2018
uid: security/cors
ms.openlocfilehash: cfbf24edb1dae76f676d51738b0d57266688d53e
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045591"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core

Przez [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), i [Tom Dykstra](https://github.com/tdykstra)

Poziom zabezpieczeń przeglądarki zapobiega wysyłania żądań do innej domeny niż ta, która obsłużyła stronę sieci web strony sieci web. To ograniczenie jest nazywany *zasadami tego samego źródła*. Zasada tego samego źródła uniemożliwia złośliwych witryn odczytywanie poufnych danych z innej lokacji. Czasami możesz chcieć zezwala na innych witryn wprowadzać żądań cross-origin Twojej aplikacji.

[Krzyżowe współużytkowanie zasobów](https://www.w3.org/TR/cors/) (między źródłami CORS) jest to standard W3C, dzięki któremu serwer może Poluzować zasady tego samego źródła. Przy użyciu mechanizmu CORS, serwer można jawnie zezwolić na niektórych żądań cross-origin jednocześnie odrzucając inne. CORS to bezpieczniejsze i bardziej elastyczne niż wcześniej technik, takich jak [JSONP](https://wikipedia.org/wiki/JSONP). W tym temacie pokazano, jak włączanie mechanizmu CORS w aplikacji ASP.NET Core.

## <a name="same-origin"></a>Tego samego źródła

Dwa adresy URL mają tego samego źródła, jeśli mają one hostów, porty i schematy identyczne ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Te dwa adresy URL są tego samego źródła:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Te adresy URL są różne źródła niż poprzednie dwa adresy URL:

* `https://example.net` &ndash; Inną domenę
* `https://www.example.com/foo.html` &ndash; Różne domeny podrzędnej
* `http://example.com/foo.html` &ndash; Inny schemat
* `https://example.com:9000/foo.html` &ndash; Inny port

> [!NOTE]
> Program Internet Explorer nie bierze pod uwagę portu podczas porównywania źródeł.

## <a name="register-cors-services"></a>Rejestrowanie usługi CORS

::: moniker range=">= aspnetcore-2.1"

Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pakietu.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Odwołanie [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) lub Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pakietu.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pakietu.

::: moniker-end

Wywołaj <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> w `Startup.ConfigureServices` nad dodaniem usług CORS do aplikacji kontenera usługi:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a>Włączanie mechanizmu CORS

Po zarejestrowaniu mechanizmu CORS usługi, użyj jednej z następujących metod Włączanie mechanizmu CORS w aplikacji ASP.NET Core:

* [Oprogramowanie pośredniczące CORS](#enable-cors-with-cors-middleware) &ndash; zasady CORS do zastosowania globalnie do aplikacji za pomocą oprogramowania pośredniczącego.
* [Mechanizm CORS w MVC](#enable-cors-in-mvc) &ndash; zasady CORS do zastosowania na akcji lub kontrolera. Oprogramowanie pośredniczące CORS nie jest używany.

### <a name="enable-cors-with-cors-middleware"></a>Włączanie mechanizmu CORS z oprogramowaniem pośredniczącym CORS

Oprogramowanie pośredniczące CORS obsługuje żądań cross-origin w aplikacji. Aby włączyć oprogramowanie pośredniczące CORS w potoku przetwarzania żądań, należy wywołać <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> metody rozszerzenia w `Startup.Configure`.

Oprogramowanie pośredniczące CORS musi poprzedzać żadnych zdefiniowanych punktów końcowych w aplikacji, której chcesz obsługiwać żądań cross-origin (na przykład przed wywołaniem do `UseMvc` dla oprogramowania pośredniczącego stron MVC i Razor).

A *zasad cross-origin* można określić podczas dodawania przy użyciu oprogramowanie pośredniczące CORS <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> klasy. Dostępne są dwie opcje do definiowania zasad CORS:

* Wywołaj `UseCors` za pomocą wyrażenia lambda:

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  Wyrażenie lambda ma <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> obiektu. [Opcje konfiguracji](#cors-policy-options), takich jak `WithOrigins`, są opisane w dalszej części tego tematu. W powyższym przykładzie zasady umożliwiają żądań cross-origin z `https://example.com` i innych źródeł.

  Należy określić adres URL bez znaku ukośnika na końcu (`/`). Jeśli adres URL kończy się za pomocą `/`, zwraca wynik porównania `false` i zostanie zwrócony bez nagłówka.

  `CorsPolicyBuilder` ma interfejs fluent API, dzięki czemu można połączyć w łańcuch wywołań metod:

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* Zdefiniuj co najmniej jedne zasady o nazwie CORS i wybierz zasady według nazwy w czasie wykonywania. W poniższym przykładzie dodano zasady CORS zdefiniowanych przez użytkownika o nazwie *AllowSpecificOrigin*. Aby wybrać zasady, należy przekazać nazwę aby `UseCors`:

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a>Włączanie mechanizmu CORS na platformie MVC

Można też użyć MVC zastosować określone zasady CORS na akcji lub kontrolera. Używanie wzorca MVC, aby włączyć mechanizm CORS, używane są zarejestrowane usługi CORS. Oprogramowanie pośredniczące CORS nie jest używany.

### <a name="per-action"></a>Za akcję

Aby określić zasad CORS dla danego działania, Dodaj [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atrybutu akcji. Podaj nazwę zasad.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a>Każdy kontroler

Aby określić zasady CORS dla określonego kontrolera, należy dodać [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atrybutów do klasy kontrolera. Podaj nazwę zasad.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

Kolejność pierwszeństwa jest:

1. Akcja
1. kontroler

### <a name="disable-cors"></a>Wyłączenia CORS

Aby wyłączyć CORS dla kontrolera lub akcji, należy użyć [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) atrybutu:

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a>Opcje zasad CORS

W tej sekcji opisano różne opcje, które można ustawić zasady CORS.

* [Ustaw dozwolone źródła](#set-the-allowed-origins)
* [Ustaw dozwolone metody HTTP](#set-the-allowed-http-methods)
* [Ustawia nagłówki żądania dozwolone](#set-the-allowed-request-headers)
* [Ustawianie nagłówków odpowiedzi narażone](#set-the-exposed-response-headers)
* [Poświadczenia w żądań cross-origin](#credentials-in-cross-origin-requests)
* [Ustaw czas wygaśnięcia wstępnego](#set-the-preflight-expiration-time)

Niektóre opcje mogą być pomocne dla odczytu [działa jak CORS](#how-cors-works) najpierw sekcji.

### <a name="set-the-allowed-origins"></a>Ustaw dozwolone źródła

Aby zezwolić na co najmniej jeden z określonych źródeł, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

Aby zezwolić na wszystkie pochodzenia, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

Starannie rozważ, przed zezwoleniem na żądania z dowolnego źródła. Zezwolenie na żądania z dowolnego źródła oznacza, że *dowolnej witrynie sieci Web* mogą wysyłać żądań cross-origin do swojej aplikacji.

To ustawienie dotyczy [stanu wstępnego żądania i nagłówka Access-Control-Allow-Origin](#preflight-requests) (opisanych w dalszej części tego tematu).

### <a name="set-the-allowed-http-methods"></a>Ustaw dozwolone metody HTTP

Aby zezwolić na wszystkie metody HTTP, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

To ustawienie dotyczy [stanu wstępnego żądania i nagłówka Access-kontroli-Allow-Methods](#preflight-requests) (opisanych w dalszej części tego tematu).

### <a name="set-the-allowed-request-headers"></a>Ustawia nagłówki żądania dozwolone

Aby zezwolić na określone nagłówki do wysłania żądania CORS, o nazwie *tworzyć nagłówki żądania*, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> i określ dozwolone nagłówki:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

Aby umożliwić Twórz wszystkie nagłówki żądania wywołania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

To ustawienie dotyczy [stanu wstępnego żądania i nagłówka Access-Control-Request-Headers](#preflight-requests) (opisanych w dalszej części tego tematu).

::: moniker range=">= aspnetcore-2.2"

Oprogramowanie pośredniczące CORS dopasowania zasad do określonych nagłówków, określony przez `WithHeaders` jest możliwe tylko wtedy, gdy nagłówki są wysyłane `Access-Control-Request-Headers` dokładnie pasują do nagłówków w `WithHeaders`.

Na przykład należy wziąć pod uwagę aplikacji skonfigurowanej w następujący sposób:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Oprogramowanie pośredniczące CORS odmawia żądania wstępnego za pomocą następujących nagłówek żądania, ponieważ `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) nie ma na liście w `WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

Zwraca aplikacji *200 OK* odpowiedzi, ale nie wysyła ponownie nagłówków CORS. W związku z tym przeglądarka nie podejmować żądań cross-origin.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Oprogramowanie pośredniczące CORS zawsze zezwala na cztery nagłówków w `Access-Control-Request-Headers` mają być wysyłane niezależnie od wartości skonfigurowanych w CorsPolicy.Headers. Ta lista nagłówków zawiera:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Na przykład należy wziąć pod uwagę aplikacji skonfigurowanej w następujący sposób:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Oprogramowanie pośredniczące CORS odpowiada pomyślnie żądania wstępnego za pomocą następujących nagłówek żądania ponieważ `Content-Language` zawsze jest umieszczona na białej liście:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Ustawianie nagłówków odpowiedzi narażone

Domyślnie przeglądarka nie uwidacznia wszystkie nagłówki odpowiedzi do aplikacji. Aby uzyskać więcej informacji, zobacz [W3C Cross-Origin Resource Sharing (terminologii): prosty nagłówek odpowiedzi](https://www.w3.org/TR/cors/#simple-response-header).

Nagłówki odpowiedzi, które są domyślnie dostępne są następujące:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

Specyfikacja CORS wywołuje te nagłówki *nagłówki odpowiedzi proste*. Aby udostępnić innych nagłówków do aplikacji, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Poświadczenia w żądań cross-origin

Poświadczenia wymagają specjalnej obsługi żądania CORS. Domyślnie przeglądarka nie wysyła poświadczenia z żądaniem cross-origin. Poświadczenia obejmują pliki cookie i schematów uwierzytelniania HTTP. Do wysyłania poświadczeń z żądaniem cross-origin, klient musi być ustawiona `XMLHttpRequest.withCredentials` do `true`.

Za pomocą `XMLHttpRequest` bezpośrednio:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

W technologii jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Ponadto serwer musi zezwalać na poświadczenia. Zezwalaj na współużytkowanie poświadczeń, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

Odpowiedź HTTP zawiera `Access-Control-Allow-Credentials` nagłówka, który informuje przeglądarkę o tym, że serwer pozwala poświadczenia dla żądań cross-origin.

Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowej `Access-Control-Allow-Credentials` nagłówka, przeglądarka nie ujawnia odpowiedzi do aplikacji, a liczba nieudanych żądań cross-origin.

Należy zachować ostrożność, dzięki czemu poświadczenia cross-origin. Witryny sieci Web w innej domenie może wysyłać poświadczeń zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika.

Specyfikacja CORS również stany to ustawienie źródeł do `"*"` (wszystkie źródła) jest nieprawidłowa Jeśli `Access-Control-Allow-Credentials` nagłówka znajduje się.

### <a name="preflight-requests"></a>Żądania wstępnego

Dla niektórych żądań CORPS przeglądarce wysyła żądanie dodatkowe przed wprowadzeniem rzeczywistego żądania. To żądanie jest nazywany *żądania wstępnego*. Przeglądarka może pominąć żądania wstępnego, jeśli są spełnione następujące warunki:

* Metoda żądania jest GET, HEAD lub POST.
* Aplikacja nie ustawia nagłówki żądania innego niż `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, lub `Last-Event-ID`.
* `Content-Type` Nagłówka, jeśli ustawiona, ma jedną z następujących wartości:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

Reguła w nagłówkach żądania skonfigurowana dla żądania klienta, który ma zastosowanie do nagłówków, które aplikacja ustawia przez wywołanie metody `setRequestHeader` na `XMLHttpRequest` obiektu. Specyfikacja CORS wywołuje te nagłówki *tworzyć nagłówki żądania*. Reguła nie ma zastosowania do przeglądarki można ustawić, takie jak nagłówki `User-Agent`, `Host`, lub `Content-Length`.

Oto przykład żądania wstępnego:

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

Żądanie krótkiej wykorzystuje metodę OPTIONS protokołu HTTP. Zawiera ona dwie specjalnych nagłówków:

* `Access-Control-Request-Method`: Metoda HTTP, która będzie służyć do rzeczywistego żądania.
* `Access-Control-Request-Headers`: Lista nagłówków żądań, które aplikacja ustawia rzeczywistego żądania. Jak wspomniano wcześniej, to nie zawiera nagłówki, które ustawia przeglądarki, takich jak `User-Agent`.

Może obejmować żądania wstępnego CORS `Access-Control-Request-Headers` nagłówka, który wskazuje na serwerze, nagłówki, które są wysyłane przy użyciu rzeczywistego żądania.

Aby zezwolić na określone nagłówki, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

Aby umożliwić Twórz wszystkie nagłówki żądania wywołania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

Przeglądarki nie są w pełni zgodne, w jaki sposób ustawić `Access-Control-Request-Headers`. Jeśli ustawisz nagłówki do żadnego elementu innego niż `"*"` (lub użyj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), powinien obejmować co najmniej `Accept`, `Content-Type`, i `Origin`, oraz niestandardowe nagłówki, które mają być obsługiwane.

Poniżej zamieszczono przykładową odpowiedź do żądania wstępnego (przy założeniu, że serwer zezwala na żądanie):

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

Odpowiedź zawiera `Access-Control-Allow-Methods` nagłówek, który zawiera listę dozwolonych metod i opcjonalnie `Access-Control-Allow-Headers` nagłówka, który zawiera listę dozwolonych nagłówków. Jeśli żądania wstępnego zakończy się powodzeniem, przeglądarka wysyła rzeczywistego żądania.

W przypadku odmowy żądania wstępnego aplikacja zwróci *200 OK* odpowiedzi, ale nie wysyła ponownie nagłówków CORS. W związku z tym przeglądarka nie podejmować żądań cross-origin.

### <a name="set-the-preflight-expiration-time"></a>Ustaw czas wygaśnięcia wstępnego

`Access-Control-Max-Age` Nagłówek Określa, jak długo mogą być buforowane odpowiedzi na żądania wstępnego. Aby ustawić tego pliku nagłówkowego, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a>Jak działa mechanizmu CORS

W tej sekcji opisano, co się stanie, w ramach żądania CORS na poziomie wiadomości HTTP. Należy zrozumieć, jak CORS działa tak, aby zasady CORS może być prawidłowo skonfigurowane i debugowania, gdy wystąpią nieoczekiwane wyniki.

Specyfikacja CORS wprowadza kilka nowych nagłówki HTTP, które Włączanie żądań cross-origin. Jeśli przeglądarka obsługuje mechanizm CORS, ustawia nagłówki te automatycznie dla żądań cross-origin. Niestandardowy kod JavaScript nie jest wymagane, aby włączyć mechanizm CORS.

Oto przykład żądania między źródłami. `Origin` Nagłówek zapewnia domeny obiektu, z której wysłano żądanie:

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Jeśli serwer zezwala na żądanie, ustawia `Access-Control-Allow-Origin` nagłówka w odpowiedzi. Wartość tego nagłówka stanowi albo odpowiada `Origin` nagłówek z żądania lub wartość symbolu wieloznacznego `"*"`, co oznacza, że każde źródło jest dozwolony:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Jeżeli odpowiedź nie zawiera `Access-Control-Allow-Origin` nagłówka żądania między źródłami zakończy się niepowodzeniem. W szczególności przeglądarki nie zezwalają na żądanie. Nawet jeśli serwer zwraca odpowiedź oznaczająca Powodzenie, przeglądarka nie udostępnia odpowiedzi do aplikacji klienckiej.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
