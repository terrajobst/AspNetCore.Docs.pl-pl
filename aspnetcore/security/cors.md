---
title: Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak CORS jako standardowe rozwiązanie dla zezwolenie lub odrzucenie żądań cross-origin w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/27/2019
uid: security/cors
ms.openlocfilehash: 2cad26d0f61519f63888a2bc399bb7e8a0f1ee04
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210135"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym artykule pokazano, jak włączanie mechanizmu CORS w aplikacji ASP.NET Core.

Poziom zabezpieczeń przeglądarki zapobiega wysyłania żądań do innej domeny niż ta, która obsłużyła stronę sieci web strony sieci web. To ograniczenie jest nazywany *zasadami tego samego źródła*. Zasada tego samego źródła uniemożliwia złośliwych witryn odczytywanie poufnych danych z innej lokacji. Czasami możesz chcieć zezwala na innych witryn wprowadzać żądań cross-origin Twojej aplikacji. Aby uzyskać więcej informacji, zobacz [artykułu Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (między źródłami CORS):

* Jest W3C w warstwie standardowa, dzięki któremu serwer może Poluzować zasady tego samego źródła.
* Jest **nie** to funkcja zabezpieczeń CORS dotyczących zabezpieczeń. Interfejs API nie jest bezpieczniejsze, umożliwiając mechanizmu CORS. Aby uzyskać więcej informacji, zobacz [działa jak CORS](#how-cors).
* Zezwala serwerowi jawnie zezwolić na niektóre żądania między źródłami, jednocześnie odrzucając inne.
* Jest bezpieczniejsze i bardziej elastyczne niż wcześniej technik, takich jak [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

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

Program Internet Explorer nie bierze pod uwagę portu podczas porównywania źródeł.

## <a name="cors-with-named-policy-and-middleware"></a>Mechanizm CORS za pomocą nazwanych zasad i oprogramowania pośredniczącego

Oprogramowanie pośredniczące CORS obsługuje żądań cross-origin. Poniższy kod obsługuje mechanizm CORS dla całej aplikacji z określonej lokalizacji:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Powyższy kod:

* Ustawia nazwę zasady "\_myAllowSpecificOrigins". Nazwa zasad jest dowolnego.
* Wywołania <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> metodę rozszerzenia, która umożliwia rdzeni.
* Wywołania <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> z [wyrażenia lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Wyrażenie lambda ma <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> obiektu. [Opcje konfiguracji](#cors-policy-options), takich jak `WithOrigins`, są opisane w dalszej części tego artykułu.

<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> Wywołania metody dodaje mechanizmu CORS usługi do aplikacji kontenera usługi:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Aby uzyskać więcej informacji, zobacz [opcje zasad CORS](#cpo) w tym dokumencie.

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Metoda można połączyć w łańcuch metod, jak pokazano w poniższym kodzie:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Następujący wyróżniony kod stosuje zasady CORS do wszystkich aplikacji punktów końcowych, które przez oprogramowanie pośredniczące CORS:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors();

    app.UseHttpsRedirection();
    app.UseMvc();
}
```

Zobacz [Włączanie mechanizmu CORS w stron Razor, kontrolerów i metod akcji](#ecors) zastosować zasady CORS na poziomie akcji/kontrolera/strony.

Uwaga:

* `UseCors` musi zostać wywołana przed `UseMvc`.
* Adres URL powinien **nie** zawierać ukośnika (`/`). Jeśli adres URL kończy się za pomocą `/`, zwraca wynik porównania `false` i zostanie zwrócony bez nagłówka.

Zobacz [CORS testu](#test) instrukcje dotyczące badania w poprzednim kodzie.

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a>Włączanie mechanizmu CORS za pomocą atrybutów

[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atrybut stanowi alternatywę dla globalnie stosowania mechanizmu CORS. `[EnableCors]` Atrybutu obsługuje mechanizm CORS dla wybranych punktów końcowych, a nie dla wszystkich punktów końcowych.

Użyj `[EnableCors]` określić domyślne zasady i `[EnableCors("{Policy String}")]` można określić zasady.

`[EnableCors]` Atrybut można stosować do:

* Strona razor `PageModel`
* Kontroler
* Metoda akcji kontrolera

Różne zasady można stosować do kontrolera/strony-/ Akcja modelu przy użyciu `[EnableCors]` atrybutu. Gdy `[EnableCors]` atrybut jest stosowany do metody kontrolerów/strony-/ Akcja modelu i mechanizmu CORS jest włączona w oprogramowaniu pośredniczącym, obie zasady są stosowane. Odradza się łączenie zasad. Użyj `[EnableCors]` atrybutu lub oprogramowanie pośredniczące nie oba w tej samej aplikacji.

Poniższy kod mają zastosowanie inne zasady do każdej metody:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Poniższy kod tworzy domyślne zasady mechanizmu CORS i zasad o nazwie `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Wyłączenia CORS

[ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) atrybut wyłącza mechanizm CORS dla/strony — model/akcji kontrolera.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Opcje zasad CORS

W tej sekcji opisano różne opcje, które można ustawić zasady CORS:

* [Ustaw dozwolone źródła](#set-the-allowed-origins)
* [Ustaw dozwolone metody HTTP](#set-the-allowed-http-methods)
* [Ustawia nagłówki żądania dozwolone](#set-the-allowed-request-headers)
* [Ustawianie nagłówków odpowiedzi narażone](#set-the-exposed-response-headers)
* [Poświadczenia w żądań cross-origin](#credentials-in-cross-origin-requests)
* [Ustaw czas wygaśnięcia wstępnego](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> jest wywoływana w `Startup.ConfigureServices`. Niektóre opcje mogą być pomocne dla odczytu [działa jak CORS](#how-cors) najpierw sekcji.

## <a name="set-the-allowed-origins"></a>Ustaw dozwolone źródła

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Umożliwia żądań CORPS ze wszystkich źródeł przy użyciu dowolnego schematu (`http` lub `https`). `AllowAnyOrigin` jest niebezpieczne ponieważ *dowolnej witrynie sieci Web* mogą wysyłać żądań cross-origin do aplikacji.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> Określanie `AllowAnyOrigin` i `AllowCredentials` jest Konfiguracja niebezpieczne i może spowodować fałszerstwo żądania międzywitrynowego. Usługa CORS zwraca nieprawidłową odpowiedź CORS, gdy aplikacja jest skonfigurowana za pomocą obu metod.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> Określanie `AllowAnyOrigin` i `AllowCredentials` jest Konfiguracja niebezpieczne i może spowodować fałszerstwo żądania międzywitrynowego. Dla bezpiecznych aplikacji należy określić dokładnej listy źródeł, jeśli należy autoryzować klienta w samej dostęp do zasobów serwera.

::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**.
-->

`AllowAnyOrigin` wpływa na stanu wstępnego żądania i `Access-Control-Allow-Origin` nagłówka. Aby uzyskać więcej informacji, zobacz [stanu wstępnego żądania](#preflight-requests) sekcji.

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Zestawy <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> właściwości zasad jako funkcja, która umożliwia źródeł dopasować domeny z symbolami wieloznacznymi skonfigurowane podczas obliczania, jeśli źródło jest dozwolone.

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>Ustaw dozwolone metody HTTP

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Umożliwia dowolną metodę HTTP:
* Wpływa na stanu wstępnego żądania i `Access-Control-Allow-Methods` nagłówka. Aby uzyskać więcej informacji, zobacz [stanu wstępnego żądania](#preflight-requests) sekcji.

### <a name="set-the-allowed-request-headers"></a>Ustawia nagłówki żądania dozwolone

Aby zezwolić na określone nagłówki do wysłania żądania CORS, o nazwie *tworzyć nagłówki żądania*, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> i określ dozwolone nagłówki:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Aby umożliwić Twórz wszystkie nagłówki żądania wywołania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

To ustawienie dotyczy żądań wstępnych i `Access-Control-Request-Headers` nagłówka. Aby uzyskać więcej informacji, zobacz [stanu wstępnego żądania](#preflight-requests) sekcji.

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

Domyślnie przeglądarka nie uwidacznia wszystkie nagłówki odpowiedzi do aplikacji. Aby uzyskać więcej informacji, zobacz [W3C Cross-Origin Resource Sharing (terminologii): Nagłówek odpowiedzi proste](https://www.w3.org/TR/cors/#simple-response-header).

Nagłówki odpowiedzi, które są domyślnie dostępne są następujące:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

Specyfikacja CORS wywołuje te nagłówki *nagłówki odpowiedzi proste*. Aby udostępnić innych nagłówków do aplikacji, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Poświadczenia w żądań cross-origin

Poświadczenia wymagają specjalnej obsługi żądania CORS. Domyślnie przeglądarka nie wysyła poświadczenia z żądaniem cross-origin. Poświadczenia obejmują pliki cookie i schematów uwierzytelniania HTTP. Do wysyłania poświadczeń z żądaniem cross-origin, klient musi być ustawiona `XMLHttpRequest.withCredentials` do `true`.

Za pomocą `XMLHttpRequest` bezpośrednio:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

Przy użyciu jQuery:

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

Za pomocą [interfejsu API Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

Serwer musi zezwalać na poświadczenia. Zezwalaj na współużytkowanie poświadczeń, należy wywołać <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

Odpowiedź HTTP zawiera `Access-Control-Allow-Credentials` nagłówka, który informuje przeglądarkę o tym, że serwer pozwala poświadczenia dla żądań cross-origin.

Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowej `Access-Control-Allow-Credentials` nagłówka, przeglądarka nie ujawnia odpowiedzi do aplikacji, a liczba nieudanych żądań cross-origin.

Dzięki czemu poświadczenia cross-origin to zagrożenie bezpieczeństwa. Witryny sieci Web w innej domenie może wysyłać poświadczeń zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

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

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Aby umożliwić Twórz wszystkie nagłówki żądania wywołania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

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

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>Jak działa mechanizmu CORS

W tej sekcji opisano, co dzieje się w [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) żądania na poziomie wiadomości HTTP.

* CORS to **nie** to funkcja zabezpieczeń. CORS to W3C standard, dzięki któremu serwer może Poluzować zasady tego samego źródła.
  * Na przykład użyć złośliwego aktora [zapobiec Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) względem lokacji, a następnie wykonaj fałszerstwo żądania do swojej lokacji włączono mechanizm CORS do wykradania informacji.
* Interfejs API nie jest bezpieczniejsze, umożliwiając mechanizmu CORS.
  * Wszystko zależy klienta (przeglądarki) wymuszenie mechanizmu CORS. Serwer ten wykonuje żądanie i zwraca odpowiedź okazał się klient, który zwraca błąd i bloków odpowiedź. Na przykład dowolne z następujących narzędzi spowoduje wyświetlenie odpowiedź serwera:
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [.NET HttpClient](/dotnet/csharp/tutorials/console-webapiclient)
    * Przeglądarki sieci web przy użyciu adresu URL w pasku adresu.
* Jest to sposób wykonania cross-origin dla serwera umożliwić przeglądarki [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) lub [pobrania interfejsu API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) żądania, w przeciwnym razie mogłyby być zabroniony.
  * Przeglądarek (CORS) nie można wykonać żądań cross-origin. Przed mechanizmu CORS [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) został użyty na obejście tego ograniczenia. Jest używany inny JSONP XHR, używa ona `<script>` tag do odbierania odpowiedzi. Skrypty mogą zostać załadowane cross-origin.

[Specyfikacji CORS](https://www.w3.org/TR/cors/) wprowadzono kilka nowych nagłówki HTTP, które Włączanie żądań cross-origin. Jeśli przeglądarka obsługuje mechanizm CORS, ustawia nagłówki te automatycznie dla żądań cross-origin. Niestandardowy kod JavaScript nie jest wymagane, aby włączyć mechanizm CORS.

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

<a name="test"></a>

## <a name="test-cors"></a>Test CORS

Aby przetestować CORS:

1. [Utwórz projekt interfejsu API](xref:tutorials/first-web-api). Możesz też [pobrać przykład](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Włączanie mechanizmu CORS za pomocą jednej z metod, w tym dokumencie. Na przykład:

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` powinna służyć wyłącznie do testowania podobne do przykładowej aplikacji [Pobierz przykładowy kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).

1. Tworzenie projektu aplikacji sieci web (stron Razor lub MVC). W przykładzie użyto stron Razor. W tym samym rozwiązaniu co projekt interfejsu API, można utworzyć aplikację internetową.
1. Dodaj następujący wyróżniony kod do *Index.cshtml* pliku:

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. W poprzednim kodzie Zastąp `url: 'https://<web app>.azurewebsites.net/api/values/1',` z adresem URL wdrożonej aplikacji.
1. Wdróż projekt interfejsu API. Na przykład [wdrażanie na platformie Azure](xref:host-and-deploy/azure-apps/index).
1. Uruchamianie aplikacji stron Razor lub MVC z pulpitu, a następnie kliknij przycisk na **testu** przycisku. Użyj narzędzi F12, aby przejrzeć komunikaty o błędach.
1. Usuń źródła localhost z `WithOrigins` i wdróż aplikację. Możesz też uruchomić aplikacji klienckiej, używając innego portu. Na przykład uruchomić z programu Visual Studio.
1. Testowanie za pomocą aplikacji klienckiej. Błędy CORS zwróci błąd, ale komunikat o błędzie nie jest dostępna w kodzie JavaScript. Karta konsoli w F12 tools do wyświetlony błąd. W zależności od przeglądarki wystąpi błąd (w konsoli narzędzia F12) podobny do następującego:

   * Korzystanie z przeglądarki Microsoft Edge:

     **SEC7120: [CORS] źródło `https://localhost:44375` nie znalazł `https://localhost:44375` w nagłówku odpowiedzi Access-Control-Allow-Origin zasobu cross-origin `https://webapi.azurewebsites.net/api/values/1`**

   * Za pomocą przeglądarki Chrome:

     **Dostęp do XMLHttpRequest na `https://webapi.azurewebsites.net/api/values/1` pochodzenia `https://localhost:44375` została zablokowana przez zasady CORS: Brak nagłówka "Access-Control-Allow-Origin" jest obecny dla żądanego zasobu.**

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
