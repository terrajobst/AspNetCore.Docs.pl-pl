---
title: Włącz żądania między źródłami (CORS) w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób mechanizm CORS jest standardem umożliwiającym lub odrzucanie żądań między źródłami w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: 09cc296ebf605907371619124cac00883beb6abb
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511577"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Włącz żądania między źródłami (CORS) w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym artykule pokazano, jak włączyć funkcję CORS w aplikacji ASP.NET Core.

Zabezpieczenia przeglądarki uniemożliwiają stronom sieci Web wykonywanie żądań do innej domeny niż ta, która była obsługiwana przez stronę sieci Web. To ograniczenie jest nazywane *zasadami tego samego źródła*. Zasady tego samego źródła uniemożliwiają złośliwej lokacji odczytywanie poufnych danych z innej lokacji. Czasami możesz chcieć zezwolić innym lokacjom na wykonywanie żądań między źródłami do aplikacji. Aby uzyskać więcej informacji, zobacz [artykuł CORS firmy Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Współużytkowanie zasobów między źródłami](https://www.w3.org/TR/cors/) (CORS):

* Jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła.
* **Nie** jest funkcją zabezpieczeń, funkcja CORS osłabi zabezpieczenia. Interfejs API nie jest bezpieczniejszy przez umożliwienie mechanizmu CORS. Aby uzyskać więcej informacji, zobacz [jak działa mechanizm CORS](#how-cors).
* Zezwala serwerowi jawnie zezwolić na niektóre żądania między źródłami podczas odrzucania innych.
* Jest bezpieczniejsze i bardziej elastyczne niż wcześniejsze techniki, takie jak [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>To samo źródło

Dwa adresy URL mają te same źródła, jeśli mają identyczne schematy, hosty i porty ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Te dwa adresy URL mają te same źródła:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Te adresy URL mają różne źródła niż poprzednie dwa adresy URL:

* `https://example.net` &ndash; innej domeny
* `https://www.example.com/foo.html` &ndash; inną poddomeną
* `http://example.com/foo.html` &ndash; innego schematu
* `https://example.com:9000/foo.html` &ndash; inny port

Program Internet Explorer nie traktuje portu podczas porównywania źródeł.

## <a name="cors-with-named-policy-and-middleware"></a>CORS z nazwanymi zasadami i programem pośredniczącym

Oprogramowanie pośredniczące CORS obsługuje żądania między źródłami. Poniższy kod włącza mechanizm CORS dla całej aplikacji z określonym źródłem:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Powyższy kod:

* Ustawia nazwę zasad na "\_myAllowSpecificOrigins". Nazwa zasad jest dowolną.
* Wywołuje metodę rozszerzenia <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, która umożliwia mechanizm CORS.
* Wywołuje <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> z [wyrażeniem lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Wyrażenie lambda przyjmuje obiekt <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>. [Opcje konfiguracji](#cors-policy-options), takie jak `WithOrigins`, zostały opisane w dalszej części tego artykułu.

Wywołanie metody <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> dodaje usługi CORS do kontenera usługi aplikacji:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Aby uzyskać więcej informacji, zobacz [Opcje zasad CORS](#cpo) w tym dokumencie.

Metoda <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> może łączyć metody łańcucha, jak pokazano w poniższym kodzie:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Uwaga: adres URL **nie** może zawierać końcowego ukośnika (`/`). Jeśli adres URL kończy się `/`, porównanie zwraca `false` i żaden nagłówek nie zostanie zwrócony.

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a>Zastosuj zasady CORS do wszystkich punktów końcowych

Poniższy kod dotyczy zasad CORS dla wszystkich punktów końcowych aplikacji za pośrednictwem oprogramowania do obsługi mechanizmu CORS:
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code omitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code omitted.
}
```

> [!WARNING]
> W przypadku routingu punktu końcowego oprogramowanie do obsługi mechanizmu CORS musi być skonfigurowane do wykonywania między wywołaniami `UseRouting` i `UseEndpoints`. Niepoprawna konfiguracja spowoduje, że oprogramowanie pośredniczące przestanie działać poprawnie.

Zobacz [Włączanie mechanizmu CORS w Razor Pages, kontrolerach i metodach akcji](#ecors) , aby zastosować zasady CORS na poziomie strony/kontrolera/akcji.

Aby uzyskać instrukcje dotyczące testowania poprzedniego kodu, zobacz temat [CORS testów](#test) .

<a name="ecors"></a>

## <a name="enable-cors-with-endpoint-routing"></a>Włączanie mechanizmu CORS przy użyciu routingu punktu końcowego

Za pomocą routingu punktu końcowego można włączyć funkcję CORS dla poszczególnych punktów końcowych przy użyciu zestawu `RequireCors` metod rozszerzenia.

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

Podobnie można również włączyć funkcję CORS dla wszystkich kontrolerów:

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```

## <a name="enable-cors-with-attributes"></a>Włączanie mechanizmu CORS z atrybutami

Atrybut [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) stanowi alternatywę do stosowania mechanizmu CORS globalnie. Atrybut `[EnableCors]` włącza funkcję CORS dla wybranych punktów końcowych, a nie wszystkich punktów końcowych.

Użyj `[EnableCors]`, aby określić zasady domyślne i `[EnableCors("{Policy String}")]`, aby określić zasady.

Atrybut `[EnableCors]` można zastosować do:

* `PageModel` strony Razor
* Kontroler
* Metoda akcji kontrolera

Możesz zastosować różne zasady do kontrolera/strony-model/akcja z atrybutem `[EnableCors]`. Gdy atrybut `[EnableCors]` jest stosowany do metody controllers/model/akcja, a funkcja CORS jest włączona w oprogramowaniu pośredniczącym, obie zasady są stosowane. Zalecamy łączenie zasad. Użyj atrybutu `[EnableCors]` lub oprogramowania pośredniczącego, a nie obu w tej samej aplikacji.

Poniższy kod stosuje różne zasady do każdej metody:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Poniższy kod tworzy domyślne zasady CORS i zasady o nazwie `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Wyłącz funkcję CORS

Atrybut [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) wyłącza funkcję CORS dla kontrolera/strony-modelu/akcji.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Opcje zasad CORS

W tej sekcji opisano różne opcje, które można ustawić w zasadach CORS:

* [Ustaw dozwolone źródła](#set-the-allowed-origins)
* [Ustaw dozwolone metody HTTP](#set-the-allowed-http-methods)
* [Ustaw nagłówki dozwolonych żądań](#set-the-allowed-request-headers)
* [Ustawianie nagłówków uwidocznionych odpowiedzi](#set-the-exposed-response-headers)
* [Poświadczenia w żądaniach między źródłami](#credentials-in-cross-origin-requests)
* [Ustaw czas wygaśnięcia inspekcji wstępnej](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> jest wywoływana w `Startup.ConfigureServices`. W przypadku niektórych opcji warto przeczytać najpierw sekcję [jak działa mechanizm CORS](#how-cors) .

## <a name="set-the-allowed-origins"></a>Ustaw dozwolone źródła

&ndash; <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> zezwala na żądania CORS ze wszystkich źródeł z dowolnym schematem (`http` lub `https`). `AllowAnyOrigin` jest niezabezpieczona, ponieważ *Każda witryna sieci Web* może wprowadzać żądania między źródłami do aplikacji.

> [!NOTE]
> Określanie `AllowAnyOrigin` i `AllowCredentials` jest niebezpieczną konfiguracją i może skutkować fałszerstwem żądania między lokacjami. Usługa CORS zwraca nieprawidłową odpowiedź CORS, gdy aplikacja jest skonfigurowana przy użyciu obu metod.

`AllowAnyOrigin` ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Allow-Origin`. Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; ustawia właściwość <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> zasad jako funkcję, która umożliwia pochodzenie do dopasowania do skonfigurowanej domeny z symbolami wieloznacznymi podczas oceniania, czy pochodzenie jest dozwolone.

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a>Ustaw dozwolone metody HTTP

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Zezwala na dowolną metodę HTTP:
* Ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Allow-Methods`. Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .

### <a name="set-the-allowed-request-headers"></a>Ustaw nagłówki dozwolonych żądań

Aby zezwolić na wysyłanie określonych nagłówków w żądaniu CORS, nazywanymi *nagłówkami żądań autora*, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> i określ dozwolone nagłówki:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Aby zezwolić na wszystkie nagłówki żądań autora, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

To ustawienie ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Request-Headers`. Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .


Zasady oprogramowania CORS są zgodne z określonymi nagłówkami określonymi przez `WithHeaders` jest możliwe tylko wtedy, gdy nagłówki wysyłane w `Access-Control-Request-Headers` dokładnie pasują do nagłówków określonych w `WithHeaders`.

Na przykład rozważ zastosowanie skonfigurowanej aplikacji w następujący sposób:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Oprogramowanie pośredniczące CORS odrzuca żądanie wstępne z następującym nagłówkiem żądania, ponieważ `Content-Language` ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) nie znajduje się na liście `WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

Aplikacja zwraca odpowiedź *200 OK* , ale nie wysyła nagłówków CORS z powrotem. W związku z tym przeglądarka nie próbuje żądania między źródłami.

### <a name="set-the-exposed-response-headers"></a>Ustawianie nagłówków uwidocznionych odpowiedzi

Domyślnie przeglądarka nie ujawnia wszystkich nagłówków odpowiedzi dla aplikacji. Aby uzyskać więcej informacji, zobacz temat [udostępnianie zasobów między źródłami W3C (terminologia): prosty nagłówek odpowiedzi](https://www.w3.org/TR/cors/#simple-response-header).

Domyślnie dostępne są nagłówki odpowiedzi:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

Specyfikacja CORS wywołuje te nagłówki *proste odpowiedzi*. Aby udostępnić inne nagłówki dla aplikacji, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Poświadczenia w żądaniach między źródłami

Poświadczenia wymagają specjalnej obsługi w żądaniu CORS. Domyślnie przeglądarka nie wysyła poświadczeń z żądaniem między źródłami. Poświadczenia obejmują pliki cookie i schematy uwierzytelniania HTTP. Aby wysłać poświadczenia z żądaniem między źródłami, klient musi ustawić `XMLHttpRequest.withCredentials`, aby `true`.

Używanie `XMLHttpRequest` bezpośrednio:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

Za pomocą jQuery:

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

Za pomocą [interfejsu API pobierania](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

Serwer musi zezwalać na poświadczenia. Aby zezwolić na poświadczenia między źródłami, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

Odpowiedź HTTP zawiera nagłówek `Access-Control-Allow-Credentials`, który informuje przeglądarkę, że serwer zezwala na poświadczenia dla żądania między źródłami.

Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka `Access-Control-Allow-Credentials`, przeglądarka nie ujawnia odpowiedzi aplikacji, a żądanie między źródłami nie powiedzie się.

Zezwalanie na poświadczenia między źródłami stanowi zagrożenie bezpieczeństwa. Witryna sieci Web w innej domenie może wysyłać poświadczenia zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

Specyfikacja CORS określa również, że źródła ustawień dla `"*"` (wszystkie źródła) są nieprawidłowe, jeśli nagłówek `Access-Control-Allow-Credentials` jest obecny.

### <a name="preflight-requests"></a>Żądania wstępnego lotu

W przypadku niektórych żądań CORS przeglądarka wysyła dodatkowe żądanie przed wykonaniem rzeczywistego żądania. To żądanie jest nazywane *żądaniem wstępnym*. Jeśli spełnione są następujące warunki, przeglądarka może pominąć żądanie wstępne:

* Metoda żądania ma wartość GET, główna lub OPUBLIKOWANa.
* Aplikacja nie ustawia nagłówków żądań innych niż `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`lub `Last-Event-ID`.
* Nagłówek `Content-Type`, jeśli jest ustawiony, ma jedną z następujących wartości:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

Reguła dotycząca nagłówków żądań ustawiona dla żądania klienta dotyczy nagłówków, które są ustawiane przez aplikację przez wywołanie `setRequestHeader` na obiekcie `XMLHttpRequest`. Specyfikacja CORS wywołuje *nagłówki żądania autora*tych nagłówków. Reguła nie ma zastosowania do nagłówków, które można ustawić w przeglądarce, takich jak `User-Agent`, `Host`lub `Content-Length`.

Poniżej znajduje się przykład żądania wstępnego:

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

Żądanie przed inspekcją używa metody HTTP OPTIONS. Zawiera dwa specjalne nagłówki:

* `Access-Control-Request-Method`: metoda HTTP, która będzie używana dla rzeczywistego żądania.
* `Access-Control-Request-Headers`: lista nagłówków żądań, które aplikacja ustawia na rzeczywiste żądanie. Jak wspomniano wcześniej, nie obejmuje to nagłówków, które są ustawiane przez przeglądarkę, takich jak `User-Agent`.

Żądanie inspekcji wstępnej CORS może zawierać nagłówek `Access-Control-Request-Headers` wskazujący serwerowi nagłówki wysyłane z rzeczywistym żądaniem.

Aby zezwolić na określone nagłówki, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Aby zezwolić na wszystkie nagłówki żądań autora, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Przeglądarki nie są w pełni spójne w sposób, w jaki ustawili `Access-Control-Request-Headers`. Jeśli ustawisz nagłówki na inne niż `"*"` (lub użyj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), należy uwzględnić co najmniej `Accept`, `Content-Type`i `Origin`oraz wszystkie niestandardowe nagłówki, które mają być obsługiwane.

Poniżej znajduje się Przykładowa odpowiedź na żądanie inspekcji wstępnej (przy założeniu, że serwer zezwala na żądanie):

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

Odpowiedź zawiera nagłówek `Access-Control-Allow-Methods`, który zawiera listę dozwolonych metod i opcjonalnie nagłówek `Access-Control-Allow-Headers`, który zawiera listę dozwolonych nagłówków. W przypadku pomyślnego przeprowadzenia żądania wstępnego przeglądarka wyśle rzeczywiste żądanie.

Jeśli żądanie wstępne nie zostanie odrzucone, aplikacja zwróci odpowiedź *200 OK* , ale nie wyśle nagłówków CORS z powrotem. W związku z tym przeglądarka nie próbuje żądania między źródłami.

### <a name="set-the-preflight-expiration-time"></a>Ustaw czas wygaśnięcia inspekcji wstępnej

Nagłówek `Access-Control-Max-Age` określa, jak długo odpowiedź na żądanie inspekcji wstępnej może być buforowana. Aby ustawić ten nagłówek, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>Jak działa mechanizm CORS

W tej sekcji opisano, co się dzieje w żądaniu [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) na poziomie komunikatów http.

* Mechanizm CORS **nie** jest funkcją zabezpieczeń. CORS jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła.
  * Na przykład złośliwy aktor może użyć [zapobiegania skryptom między lokacjami (XSS)](xref:security/cross-site-scripting) względem witryny i wykonać żądanie między lokacjami w celu wykraść informacji.
* Interfejs API nie jest bezpieczniejszy przez umożliwienie mechanizmu CORS.
  * Aby wymusić mechanizm CORS, należy do klienta (przeglądarki). Serwer wykonuje żądanie i zwraca odpowiedź, jest to klient, który zwraca błąd i blokuje odpowiedź. Na przykład w dowolnym z poniższych narzędzi zostanie wyświetlona odpowiedź serwera:
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [HttpClient .NET](/dotnet/csharp/tutorials/console-webapiclient)
    * Przeglądarka sieci Web, wprowadzając adres URL na pasku adresu.
* Jest to sposób, aby serwer zezwalał przeglądarce na wykonywanie żądania [interfejsu API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) lub pobierania między źródłami, które w przeciwnym razie byłoby zabronione.
  * Przeglądarki (bez CORS) nie mogą wykonywać żądań między źródłami. Przed zastosowaniem mechanizmu CORS [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) został użyty do obejścia tego ograniczenia. JSONP nie używa XHR, aby otrzymać odpowiedź, używa znacznika `<script>`. Skrypty mogą być ładowane między różnymi źródłami.

[Specyfikacja CORS](https://www.w3.org/TR/cors/) wprowadziła kilka nowych nagłówków HTTP, które umożliwiają żądania między źródłami. Jeśli przeglądarka obsługuje mechanizm CORS, ustawia te nagłówki automatycznie dla żądań cross-Origin. Niestandardowy kod JavaScript nie jest wymagany do włączenia mechanizmu CORS.

Poniżej przedstawiono przykład żądania między źródłami danych. Nagłówek `Origin` zapewnia domenę lokacji, która żąda żądania. Nagłówek `Origin` jest wymagany i musi się różnić od hosta.

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

Jeśli serwer zezwala na żądanie, ustawia nagłówek `Access-Control-Allow-Origin` w odpowiedzi. Wartość tego nagłówka jest zgodna z nagłówkiem `Origin` z żądania lub jest wartością symbolu wieloznacznego `"*"`, co oznacza, że wszystkie źródła są dozwolone:

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

Jeśli odpowiedź nie zawiera nagłówka `Access-Control-Allow-Origin`, żądanie między źródłami nie powiedzie się. W programie przeglądarka nie zezwala na żądanie. Nawet jeśli serwer zwróci pomyślną odpowiedź, przeglądarka nie udostępni odpowiedzi dla aplikacji klienckiej.

<a name="test"></a>

## <a name="test-cors"></a>Testowanie CORS

Aby przetestować CORS:

1. [Utwórz projekt interfejsu API](xref:tutorials/first-web-api). Możesz również [pobrać przykład](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Włącz funkcję CORS przy użyciu jednego z metod w tym dokumencie. Na przykład:

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` należy używać tylko do testowania przykładowej aplikacji podobnej do [przykładowego kodu pobierania](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).

1. Utwórz projekt aplikacji sieci Web (Razor Pages lub MVC). Przykład używa Razor Pages. Aplikację sieci Web można utworzyć w tym samym rozwiązaniu co projekt interfejsu API.
1. Dodaj następujący wyróżniony kod do pliku *index. cshtml* :

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. W poprzednim kodzie Zastąp `url: 'https://<web app>.azurewebsites.net/api/values/1',` adresem URL wdrożonej aplikacji.
1. Wdróż projekt interfejsu API. Na przykład [Wdróż na platformie Azure](xref:host-and-deploy/azure-apps/index).
1. Uruchom aplikację Razor Pages lub MVC na pulpicie, a następnie kliknij przycisk **Testuj** . Użyj narzędzi F12, aby przejrzeć komunikaty o błędach.
1. Usuń pochodzenie hosta lokalnego z `WithOrigins` i Wdróż aplikację. Alternatywnie Uruchom aplikację kliencką z innym portem. Na przykład uruchom polecenie z programu Visual Studio.
1. Przetestuj za pomocą aplikacji klienckiej. Błędy funkcji CORS zwracają błąd, ale komunikat o błędzie nie jest dostępny dla języka JavaScript. Aby wyświetlić błąd, Użyj karty konsola w narzędziach F12. W zależności od przeglądarki pojawia się błąd (w konsoli narzędzia F12) podobny do poniższego:

   * Korzystanie z przeglądarki Microsoft Edge:

     **SEC7120: [CORS] Źródło `https://localhost:44375` nie znalazło `https://localhost:44375` w nagłówku odpowiedzi "Access-Control-Allow-Origin" dla zasobu Cross-Origin w `https://webapi.azurewebsites.net/api/values/1`**

   * Korzystanie z programu Chrome:

     **Dostęp do elementu XMLHttpRequest na `https://webapi.azurewebsites.net/api/values/1` ze źródła `https://localhost:44375` został zablokowany przez zasady CORS: brak nagłówka "Access-Control-Allow-Origin" w żądanym zasobie.**
     
Punkty końcowe z obsługą mechanizmu CORS można testować za pomocą narzędzia, takiego jak [programu Fiddler](https://www.telerik.com/fiddler) lub [Poster](https://www.getpostman.com/). W przypadku korzystania z narzędzia, Źródło żądania określone przez nagłówek `Origin` musi różnić się od hosta przyjmującego żądanie. Jeśli żądanie nie jest *źródłem krzyżowe* na podstawie wartości nagłówka `Origin`:

* Nie ma potrzeby przetwarzania żądania przez oprogramowanie pośredniczące CORS.
* Nagłówki CORS nie są zwracane w odpowiedzi.

## <a name="cors-in-iis"></a>Mechanizm CORS w usługach IIS

W przypadku wdrażania w programie IIS należy uruchomić funkcję CORS przed uwierzytelnianiem systemu Windows, jeśli serwer nie jest skonfigurowany do zezwalania na dostęp anonimowy. Aby zapewnić obsługę tego scenariusza, należy zainstalować i skonfigurować [moduł CORS usług IIS](https://www.iis.net/downloads/microsoft/iis-cors-module) dla aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Współużytkowanie zasobów między źródłami (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [Wprowadzenie do modułu CORS usług IIS](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym artykule pokazano, jak włączyć funkcję CORS w aplikacji ASP.NET Core.

Zabezpieczenia przeglądarki uniemożliwiają stronom sieci Web wykonywanie żądań do innej domeny niż ta, która była obsługiwana przez stronę sieci Web. To ograniczenie jest nazywane *zasadami tego samego źródła*. Zasady tego samego źródła uniemożliwiają złośliwej lokacji odczytywanie poufnych danych z innej lokacji. Czasami możesz chcieć zezwolić innym lokacjom na wykonywanie żądań między źródłami do aplikacji. Aby uzyskać więcej informacji, zobacz [artykuł CORS firmy Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Współużytkowanie zasobów między źródłami](https://www.w3.org/TR/cors/) (CORS):

* Jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła.
* **Nie** jest funkcją zabezpieczeń, funkcja CORS osłabi zabezpieczenia. Interfejs API nie jest bezpieczniejszy przez umożliwienie mechanizmu CORS. Aby uzyskać więcej informacji, zobacz [jak działa mechanizm CORS](#how-cors).
* Zezwala serwerowi jawnie zezwolić na niektóre żądania między źródłami podczas odrzucania innych.
* Jest bezpieczniejsze i bardziej elastyczne niż wcześniejsze techniki, takie jak [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>To samo źródło

Dwa adresy URL mają te same źródła, jeśli mają identyczne schematy, hosty i porty ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Te dwa adresy URL mają te same źródła:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Te adresy URL mają różne źródła niż poprzednie dwa adresy URL:

* `https://example.net` &ndash; innej domeny
* `https://www.example.com/foo.html` &ndash; inną poddomeną
* `http://example.com/foo.html` &ndash; innego schematu
* `https://example.com:9000/foo.html` &ndash; inny port

Program Internet Explorer nie traktuje portu podczas porównywania źródeł.

## <a name="cors-with-named-policy-and-middleware"></a>CORS z nazwanymi zasadami i programem pośredniczącym

Oprogramowanie pośredniczące CORS obsługuje żądania między źródłami. Poniższy kod włącza mechanizm CORS dla całej aplikacji z określonym źródłem:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Powyższy kod:

* Ustawia nazwę zasad na "\_myAllowSpecificOrigins". Nazwa zasad jest dowolną.
* Wywołuje metodę rozszerzenia <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, która umożliwia mechanizm CORS.
* Wywołuje <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> z [wyrażeniem lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Wyrażenie lambda przyjmuje obiekt <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>. [Opcje konfiguracji](#cors-policy-options), takie jak `WithOrigins`, zostały opisane w dalszej części tego artykułu.

Wywołanie metody <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> dodaje usługi CORS do kontenera usługi aplikacji:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Aby uzyskać więcej informacji, zobacz [Opcje zasad CORS](#cpo) w tym dokumencie.

Metoda <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> może łączyć metody łańcucha, jak pokazano w poniższym kodzie:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Uwaga: adres URL **nie** może zawierać końcowego ukośnika (`/`). Jeśli adres URL kończy się `/`, porównanie zwraca `false` i żaden nagłówek nie zostanie zwrócony.

Poniższy kod dotyczy zasad CORS dla wszystkich punktów końcowych aplikacji za pośrednictwem oprogramowania do obsługi mechanizmu CORS:
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
Uwaga: przed `UseMvc`należy wywołać `UseCors`.

Zobacz [Włączanie mechanizmu CORS w Razor Pages, kontrolerach i metodach akcji](#ecors) , aby zastosować zasady CORS na poziomie strony/kontrolera/akcji.

Aby uzyskać instrukcje dotyczące testowania poprzedniego kodu, zobacz temat [CORS testów](#test) .

## <a name="enable-cors-with-attributes"></a>Włączanie mechanizmu CORS z atrybutami

Atrybut [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) stanowi alternatywę do stosowania mechanizmu CORS globalnie. Atrybut `[EnableCors]` włącza funkcję CORS dla wybranych punktów końcowych, a nie wszystkich punktów końcowych.

Użyj `[EnableCors]`, aby określić zasady domyślne i `[EnableCors("{Policy String}")]`, aby określić zasady.

Atrybut `[EnableCors]` można zastosować do:

* `PageModel` strony Razor
* Kontroler
* Metoda akcji kontrolera

Możesz zastosować różne zasady do kontrolera/strony-model/akcja z atrybutem `[EnableCors]`. Gdy atrybut `[EnableCors]` jest stosowany do metody controllers/model/akcja, a funkcja CORS jest włączona w oprogramowaniu pośredniczącym, obie zasady są stosowane. Zalecamy łączenie zasad. Użyj atrybutu `[EnableCors]` lub oprogramowania pośredniczącego, a nie obu w tej samej aplikacji.

Poniższy kod stosuje różne zasady do każdej metody:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Poniższy kod tworzy domyślne zasady CORS i zasady o nazwie `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Wyłącz funkcję CORS

Atrybut [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) wyłącza funkcję CORS dla kontrolera/strony-modelu/akcji.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Opcje zasad CORS

W tej sekcji opisano różne opcje, które można ustawić w zasadach CORS:

* [Ustaw dozwolone źródła](#set-the-allowed-origins)
* [Ustaw dozwolone metody HTTP](#set-the-allowed-http-methods)
* [Ustaw nagłówki dozwolonych żądań](#set-the-allowed-request-headers)
* [Ustawianie nagłówków uwidocznionych odpowiedzi](#set-the-exposed-response-headers)
* [Poświadczenia w żądaniach między źródłami](#credentials-in-cross-origin-requests)
* [Ustaw czas wygaśnięcia inspekcji wstępnej](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> jest wywoływana w `Startup.ConfigureServices`. W przypadku niektórych opcji warto przeczytać najpierw sekcję [jak działa mechanizm CORS](#how-cors) .

## <a name="set-the-allowed-origins"></a>Ustaw dozwolone źródła

&ndash; <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> zezwala na żądania CORS ze wszystkich źródeł z dowolnym schematem (`http` lub `https`). `AllowAnyOrigin` jest niezabezpieczona, ponieważ *Każda witryna sieci Web* może wprowadzać żądania między źródłami do aplikacji.

> [!NOTE]
> Określanie `AllowAnyOrigin` i `AllowCredentials` jest niebezpieczną konfiguracją i może skutkować fałszerstwem żądania między lokacjami. W przypadku bezpiecznej aplikacji należy określić dokładną listę źródeł, jeśli klient musi autoryzować sam do uzyskiwania dostępu do zasobów serwera.

`AllowAnyOrigin` ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Allow-Origin`. Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; ustawia właściwość <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> zasad jako funkcję, która umożliwia pochodzenie do dopasowania do skonfigurowanej domeny z symbolami wieloznacznymi podczas oceniania, czy pochodzenie jest dozwolone.

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a>Ustaw dozwolone metody HTTP

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Zezwala na dowolną metodę HTTP:
* Ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Allow-Methods`. Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .

### <a name="set-the-allowed-request-headers"></a>Ustaw nagłówki dozwolonych żądań

Aby zezwolić na wysyłanie określonych nagłówków w żądaniu CORS, nazywanymi *nagłówkami żądań autora*, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> i określ dozwolone nagłówki:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Aby zezwolić na wszystkie nagłówki żądań autora, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

To ustawienie ma wpływ na żądania inspekcji wstępnej i nagłówek `Access-Control-Request-Headers`. Aby uzyskać więcej informacji, zobacz sekcję [żądania dotyczące inspekcji wstępnej](#preflight-requests) .

Oprogramowanie pośredniczące CORS zawsze umożliwia wysyłanie czterech nagłówków w `Access-Control-Request-Headers` niezależnie od wartości skonfigurowanych w CorsPolicy. Heads. Ta lista nagłówków obejmuje:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Na przykład rozważ zastosowanie skonfigurowanej aplikacji w następujący sposób:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Oprogramowanie pośredniczące CORS pomyślnie reaguje na żądanie inspekcji wstępnej z następującym nagłówkiem żądania, ponieważ `Content-Language` jest zawsze listy dozwolonych:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

### <a name="set-the-exposed-response-headers"></a>Ustawianie nagłówków uwidocznionych odpowiedzi

Domyślnie przeglądarka nie ujawnia wszystkich nagłówków odpowiedzi dla aplikacji. Aby uzyskać więcej informacji, zobacz temat [udostępnianie zasobów między źródłami W3C (terminologia): prosty nagłówek odpowiedzi](https://www.w3.org/TR/cors/#simple-response-header).

Domyślnie dostępne są nagłówki odpowiedzi:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

Specyfikacja CORS wywołuje te nagłówki *proste odpowiedzi*. Aby udostępnić inne nagłówki dla aplikacji, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Poświadczenia w żądaniach między źródłami

Poświadczenia wymagają specjalnej obsługi w żądaniu CORS. Domyślnie przeglądarka nie wysyła poświadczeń z żądaniem między źródłami. Poświadczenia obejmują pliki cookie i schematy uwierzytelniania HTTP. Aby wysłać poświadczenia z żądaniem między źródłami, klient musi ustawić `XMLHttpRequest.withCredentials`, aby `true`.

Używanie `XMLHttpRequest` bezpośrednio:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

Za pomocą jQuery:

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

Za pomocą [interfejsu API pobierania](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

Serwer musi zezwalać na poświadczenia. Aby zezwolić na poświadczenia między źródłami, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

Odpowiedź HTTP zawiera nagłówek `Access-Control-Allow-Credentials`, który informuje przeglądarkę, że serwer zezwala na poświadczenia dla żądania między źródłami.

Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka `Access-Control-Allow-Credentials`, przeglądarka nie ujawnia odpowiedzi aplikacji, a żądanie między źródłami nie powiedzie się.

Zezwalanie na poświadczenia między źródłami stanowi zagrożenie bezpieczeństwa. Witryna sieci Web w innej domenie może wysyłać poświadczenia zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

Specyfikacja CORS określa również, że źródła ustawień dla `"*"` (wszystkie źródła) są nieprawidłowe, jeśli nagłówek `Access-Control-Allow-Credentials` jest obecny.

### <a name="preflight-requests"></a>Żądania wstępnego lotu

W przypadku niektórych żądań CORS przeglądarka wysyła dodatkowe żądanie przed wykonaniem rzeczywistego żądania. To żądanie jest nazywane *żądaniem wstępnym*. Jeśli spełnione są następujące warunki, przeglądarka może pominąć żądanie wstępne:

* Metoda żądania ma wartość GET, główna lub OPUBLIKOWANa.
* Aplikacja nie ustawia nagłówków żądań innych niż `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`lub `Last-Event-ID`.
* Nagłówek `Content-Type`, jeśli jest ustawiony, ma jedną z następujących wartości:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

Reguła dotycząca nagłówków żądań ustawiona dla żądania klienta dotyczy nagłówków, które są ustawiane przez aplikację przez wywołanie `setRequestHeader` na obiekcie `XMLHttpRequest`. Specyfikacja CORS wywołuje *nagłówki żądania autora*tych nagłówków. Reguła nie ma zastosowania do nagłówków, które można ustawić w przeglądarce, takich jak `User-Agent`, `Host`lub `Content-Length`.

Poniżej znajduje się przykład żądania wstępnego:

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

Żądanie przed inspekcją używa metody HTTP OPTIONS. Zawiera dwa specjalne nagłówki:

* `Access-Control-Request-Method`: metoda HTTP, która będzie używana dla rzeczywistego żądania.
* `Access-Control-Request-Headers`: lista nagłówków żądań, które aplikacja ustawia na rzeczywiste żądanie. Jak wspomniano wcześniej, nie obejmuje to nagłówków, które są ustawiane przez przeglądarkę, takich jak `User-Agent`.

Żądanie inspekcji wstępnej CORS może zawierać nagłówek `Access-Control-Request-Headers` wskazujący serwerowi nagłówki wysyłane z rzeczywistym żądaniem.

Aby zezwolić na określone nagłówki, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Aby zezwolić na wszystkie nagłówki żądań autora, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Przeglądarki nie są w pełni spójne w sposób, w jaki ustawili `Access-Control-Request-Headers`. Jeśli ustawisz nagłówki na inne niż `"*"` (lub użyj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), należy uwzględnić co najmniej `Accept`, `Content-Type`i `Origin`oraz wszystkie niestandardowe nagłówki, które mają być obsługiwane.

Poniżej znajduje się Przykładowa odpowiedź na żądanie inspekcji wstępnej (przy założeniu, że serwer zezwala na żądanie):

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

Odpowiedź zawiera nagłówek `Access-Control-Allow-Methods`, który zawiera listę dozwolonych metod i opcjonalnie nagłówek `Access-Control-Allow-Headers`, który zawiera listę dozwolonych nagłówków. W przypadku pomyślnego przeprowadzenia żądania wstępnego przeglądarka wyśle rzeczywiste żądanie.

Jeśli żądanie wstępne nie zostanie odrzucone, aplikacja zwróci odpowiedź *200 OK* , ale nie wyśle nagłówków CORS z powrotem. W związku z tym przeglądarka nie próbuje żądania między źródłami.

### <a name="set-the-preflight-expiration-time"></a>Ustaw czas wygaśnięcia inspekcji wstępnej

Nagłówek `Access-Control-Max-Age` określa, jak długo odpowiedź na żądanie inspekcji wstępnej może być buforowana. Aby ustawić ten nagłówek, wywołaj <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>Jak działa mechanizm CORS

W tej sekcji opisano, co się dzieje w żądaniu [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) na poziomie komunikatów http.

* Mechanizm CORS **nie** jest funkcją zabezpieczeń. CORS jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła.
  * Na przykład złośliwy aktor może użyć [zapobiegania skryptom między lokacjami (XSS)](xref:security/cross-site-scripting) względem witryny i wykonać żądanie między lokacjami w celu wykraść informacji.
* Interfejs API nie jest bezpieczniejszy przez umożliwienie mechanizmu CORS.
  * Aby wymusić mechanizm CORS, należy do klienta (przeglądarki). Serwer wykonuje żądanie i zwraca odpowiedź, jest to klient, który zwraca błąd i blokuje odpowiedź. Na przykład w dowolnym z poniższych narzędzi zostanie wyświetlona odpowiedź serwera:
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [HttpClient .NET](/dotnet/csharp/tutorials/console-webapiclient)
    * Przeglądarka sieci Web, wprowadzając adres URL na pasku adresu.
* Jest to sposób, aby serwer zezwalał przeglądarce na wykonywanie żądania [interfejsu API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) lub pobierania między źródłami, które w przeciwnym razie byłoby zabronione.
  * Przeglądarki (bez CORS) nie mogą wykonywać żądań między źródłami. Przed zastosowaniem mechanizmu CORS [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) został użyty do obejścia tego ograniczenia. JSONP nie używa XHR, aby otrzymać odpowiedź, używa znacznika `<script>`. Skrypty mogą być ładowane między różnymi źródłami.

[Specyfikacja CORS](https://www.w3.org/TR/cors/) wprowadziła kilka nowych nagłówków HTTP, które umożliwiają żądania między źródłami. Jeśli przeglądarka obsługuje mechanizm CORS, ustawia te nagłówki automatycznie dla żądań cross-Origin. Niestandardowy kod JavaScript nie jest wymagany do włączenia mechanizmu CORS.

Poniżej przedstawiono przykład żądania między źródłami danych. Nagłówek `Origin` zapewnia domenę lokacji, która żąda żądania. Nagłówek `Origin` jest wymagany i musi się różnić od hosta.

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

Jeśli serwer zezwala na żądanie, ustawia nagłówek `Access-Control-Allow-Origin` w odpowiedzi. Wartość tego nagłówka jest zgodna z nagłówkiem `Origin` z żądania lub jest wartością symbolu wieloznacznego `"*"`, co oznacza, że wszystkie źródła są dozwolone:

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

Jeśli odpowiedź nie zawiera nagłówka `Access-Control-Allow-Origin`, żądanie między źródłami nie powiedzie się. W programie przeglądarka nie zezwala na żądanie. Nawet jeśli serwer zwróci pomyślną odpowiedź, przeglądarka nie udostępni odpowiedzi dla aplikacji klienckiej.

<a name="test"></a>

## <a name="test-cors"></a>Testowanie CORS

Aby przetestować CORS:

1. [Utwórz projekt interfejsu API](xref:tutorials/first-web-api). Możesz również [pobrać przykład](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Włącz funkcję CORS przy użyciu jednego z metod w tym dokumencie. Na przykład:

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` należy używać tylko do testowania przykładowej aplikacji podobnej do [przykładowego kodu pobierania](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).

1. Utwórz projekt aplikacji sieci Web (Razor Pages lub MVC). Przykład używa Razor Pages. Aplikację sieci Web można utworzyć w tym samym rozwiązaniu co projekt interfejsu API.
1. Dodaj następujący wyróżniony kod do pliku *index. cshtml* :

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. W poprzednim kodzie Zastąp `url: 'https://<web app>.azurewebsites.net/api/values/1',` adresem URL wdrożonej aplikacji.
1. Wdróż projekt interfejsu API. Na przykład [Wdróż na platformie Azure](xref:host-and-deploy/azure-apps/index).
1. Uruchom aplikację Razor Pages lub MVC na pulpicie, a następnie kliknij przycisk **Testuj** . Użyj narzędzi F12, aby przejrzeć komunikaty o błędach.
1. Usuń pochodzenie hosta lokalnego z `WithOrigins` i Wdróż aplikację. Alternatywnie Uruchom aplikację kliencką z innym portem. Na przykład uruchom polecenie z programu Visual Studio.
1. Przetestuj za pomocą aplikacji klienckiej. Błędy funkcji CORS zwracają błąd, ale komunikat o błędzie nie jest dostępny dla języka JavaScript. Aby wyświetlić błąd, Użyj karty konsola w narzędziach F12. W zależności od przeglądarki pojawia się błąd (w konsoli narzędzia F12) podobny do poniższego:

   * Korzystanie z przeglądarki Microsoft Edge:

     **SEC7120: [CORS] Źródło `https://localhost:44375` nie znalazło `https://localhost:44375` w nagłówku odpowiedzi "Access-Control-Allow-Origin" dla zasobu Cross-Origin w `https://webapi.azurewebsites.net/api/values/1`**

   * Korzystanie z programu Chrome:

     **Dostęp do elementu XMLHttpRequest na `https://webapi.azurewebsites.net/api/values/1` ze źródła `https://localhost:44375` został zablokowany przez zasady CORS: brak nagłówka "Access-Control-Allow-Origin" w żądanym zasobie.**
     
Punkty końcowe z obsługą mechanizmu CORS można testować za pomocą narzędzia, takiego jak [programu Fiddler](https://www.telerik.com/fiddler) lub [Poster](https://www.getpostman.com/). W przypadku korzystania z narzędzia, Źródło żądania określone przez nagłówek `Origin` musi różnić się od hosta przyjmującego żądanie. Jeśli żądanie nie jest *źródłem krzyżowe* na podstawie wartości nagłówka `Origin`:

* Nie ma potrzeby przetwarzania żądania przez oprogramowanie pośredniczące CORS.
* Nagłówki CORS nie są zwracane w odpowiedzi.

## <a name="cors-in-iis"></a>Mechanizm CORS w usługach IIS

W przypadku wdrażania w programie IIS należy uruchomić funkcję CORS przed uwierzytelnianiem systemu Windows, jeśli serwer nie jest skonfigurowany do zezwalania na dostęp anonimowy. Aby zapewnić obsługę tego scenariusza, należy zainstalować i skonfigurować [moduł CORS usług IIS](https://www.iis.net/downloads/microsoft/iis-cors-module) dla aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Współużytkowanie zasobów między źródłami (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [Wprowadzenie do modułu CORS usług IIS](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end