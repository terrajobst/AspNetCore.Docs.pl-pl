---
title: Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak CORS jako standardowe rozwiązanie dla zezwolenie lub odrzucenie żądań cross-origin w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2018
ms.locfileid: "41756339"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core

Przez [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), i [Tom Dykstra](https://github.com/tdykstra)

Poziom zabezpieczeń przeglądarki uniemożliwia strony sieci web wprowadzanie wysyłanie żądań AJAX do innej domeny. To ograniczenie jest nazywany *zasadami tego samego źródła*i zapobiega złośliwych witryn odczytywanie poufnych danych z innej lokacji. Jednak czasami możesz chcieć umożliwić innych witryn, wprowadzać żądań cross-origin w interfejsie API sieci web.

[Krzyżowe współużytkowanie zasobów](http://www.w3.org/TR/cors/) (między źródłami CORS) jest to standard W3C, dzięki któremu serwer może Poluzować zasady tego samego źródła. Przy użyciu mechanizmu CORS, serwer można jawnie zezwolić na niektórych żądań cross-origin jednocześnie odrzucając inne. CORS to bezpieczniejsze i bardziej elastyczne niż wcześniej techniki takie jak [JSONP](https://wikipedia.org/wiki/JSONP). W tym temacie pokazano, jak włączanie mechanizmu CORS w aplikacji ASP.NET Core.

## <a name="what-is-same-origin"></a>Co to jest "tego samego origin"?

Dwa adresy URL mają tego samego źródła, jeśli mają identyczne schematy, hosty i portów. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Te dwa adresy URL są tego samego źródła:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Te adresy URL są źródła innego niż poprzednie dwa:

* `http://example.net` -Innej domeny

* `http://www.example.com/foo.html` — Różne domeny podrzędnej

* `https://example.com/foo.html` -Innego schematu

* `http://example.com:9000/foo.html` -Innego portu

> [!NOTE]
> Program Internet Explorer nie bierze pod uwagę portu podczas porównywania źródeł.

## <a name="enable-cors"></a>Włączanie mechanizmu CORS

::: moniker range="<= aspnetcore-1.1"

Aby skonfigurować mechanizmu CORS w aplikacji Dodaj `Microsoft.AspNetCore.Cors` pakietu do projektu.

::: moniker-end

Wywołaj [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) w `Startup.ConfigureServices`:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>Włączanie funkcji CORS, za pomocą oprogramowania pośredniczącego

Aby włączyć mechanizm CORS, Dodaj oprogramowanie pośredniczące CORS do potoku żądania przy użyciu `UseCors` — metoda rozszerzenia. Oprogramowanie pośredniczące CORS musi poprzedzać żadnych zdefiniowanych punktów końcowych w aplikacji, której chcesz obsługiwać żądań cross-origin (na przykład, zanim każde wywołanie `UseMvc`).

Podczas dodawania przy użyciu oprogramowanie pośredniczące CORS można określić zasady cross-origin [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) klasy. Istnieją dwa sposoby, aby to zrobić. Pierwszy jest wywołanie `UseCors` za pomocą wyrażenia lambda:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Uwaga:** musi być określony adres URL bez znaku ukośnika na końcu (`/`). Jeśli adres URL kończy się za pomocą `/`, zwróci wynik porównania `false` i zostanie zwrócony bez nagłówka.

Wyrażenie lambda ma `CorsPolicyBuilder` obiektu. Znajdziesz listę [opcje konfiguracji](#cors-policy-options) w dalszej części tego tematu. W tym przykładzie zasady umożliwiają żądań cross-origin z `http://example.com` i innych źródeł.

CorsPolicyBuilder ma interfejs fluent API, dzięki czemu można połączyć w łańcuch wywołań metod:

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

Druga metoda jest zdefiniować co najmniej jedne zasady o nazwie CORS, a następnie wybierz pozycję zasady według nazwy w czasie wykonywania.

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

Ten przykład dodaje zasady CORS, o nazwie "AllowSpecificOrigin". Aby wybrać zasady, należy przekazać nazwę aby `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Włączanie funkcji CORS na platformie MVC

Można też użyć MVC do zastosowania określonego CORS każdej akcji, każdy kontroler lub globalnie dla wszystkich kontrolerów. Gdy za pomocą MVC w celu włączenia CORS tych samych usług mechanizmu CORS są używane, ale nie jest oprogramowanie pośredniczące CORS.

### <a name="per-action"></a>Za akcję

Aby określić zasad CORS dla danego działania Dodaj `[EnableCors]` atrybutu akcji. Podaj nazwę zasad.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Każdy kontroler

Aby określić zasady CORS dla określonego kontrolera Dodaj `[EnableCors]` atrybutów do klasy kontrolera. Podaj nazwę zasad.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Globalnie

Można włączyć mechanizm CORS globalnie dla wszystkich kontrolerów przez dodanie `CorsAuthorizationFilterFactory` filtr do kolekcji filtrów globalnych:

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

Kolejność pierwszeństwa jest: działania, kontrolera, globalne. Zasady na poziomie akcji mają pierwszeństwo przed zasad na poziomie kontrolera, a zasady na poziomie kontrolera mają pierwszeństwo przed zasad globalnych.

### <a name="disable-cors"></a>Wyłączenia CORS

Aby wyłączyć CORS dla kontrolera lub akcji, należy użyć `[DisableCors]` atrybutu.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>Opcje zasad CORS

W tej sekcji opisano różne opcje, które można ustawić zasady CORS.

* [Ustaw dozwolone źródła](#set-the-allowed-origins)

* [Ustaw dozwolone metody HTTP](#set-the-allowed-http-methods)

* [Ustawia nagłówki żądania dozwolone](#set-the-allowed-request-headers)

* [Ustawianie nagłówków odpowiedzi narażone](#set-the-exposed-response-headers)

* [Poświadczenia w żądań cross-origin](#credentials-in-cross-origin-requests)

* [Ustaw czas wygaśnięcia wstępnego](#set-the-preflight-expiration-time)

Niektóre opcje mogą być pomocne dla odczytu [działa jak CORS](#how-cors-works) pierwszy.

### <a name="set-the-allowed-origins"></a>Ustaw dozwolone źródła

Aby zezwolić na co najmniej jeden z określonych źródeł:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Aby zezwolić na wszystkie pochodzenia:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Starannie rozważ, przed zezwoleniem na żądania z dowolnego źródła. Oznacza to, że dosłownie w dowolnej witrynie sieci Web mogą przesłać wywołania AJAX do interfejsu API.

### <a name="set-the-allowed-http-methods"></a>Ustaw dozwolone metody HTTP

Aby zezwolić na wszystkie metody HTTP:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Dotyczy to żądań krótkiej i nagłówka Access-kontroli-Allow-Methods.

### <a name="set-the-allowed-request-headers"></a>Ustawia nagłówki żądania dozwolone

Żądania wstępnego CORS może obejmować nagłówka Access-Control-Request-Headers, lista nagłówków HTTP, ustawiane przez aplikację (tak zwane "Autor nagłówki żądania").

Do listy dozwolonych nagłówków określonych:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Aby umożliwić Twórz wszystkie nagłówki żądania:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Przeglądarki nie są całkowicie zgodne, w jaki ustawiają Access-Control-Request-Headers. Jeśli ustawisz nagłówki do żadnego elementu innego niż "*", użytkownik powinien zawierać co najmniej "Zaakceptuj", "content-type" i "origin", a także niestandardowe nagłówki, które mają być obsługiwane.

### <a name="set-the-exposed-response-headers"></a>Ustawianie nagłówków odpowiedzi narażone

Domyślnie przeglądarka nie uwidacznia wszystkie nagłówki odpowiedzi do aplikacji. (Zobacz [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Nagłówki odpowiedzi, które są domyślnie dostępne są następujące:

* Cache-Control

* Język zawartości

* Typ zawartości

* Wygasa

* Last-Modified

* Dyrektywy pragma

Specyfikacja CORS wywołuje te *nagłówki odpowiedzi proste*. Aby udostępnić innych nagłówków do aplikacji:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Poświadczenia w żądań cross-origin

Poświadczenia wymagają specjalnej obsługi żądania CORS. Domyślnie przeglądarka nie wysyła żadnych poświadczeń z żądaniem cross-origin. Poświadczenia obejmują pliki cookie, a także schematów uwierzytelniania HTTP. Do wysyłania poświadczeń przy użyciu żądania między źródłami, klient musi ustawić XMLHttpRequest.withCredentials na wartość true.

Bezpośrednio za pomocą obiektu XMLHttpRequest:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

W technologii jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Ponadto serwer musi zezwalać na poświadczenia. Aby zezwolić na współużytkowanie poświadczeń:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Teraz odpowiedzi HTTP będzie zawierać nagłówka Access-kontroli-Allow-Credentials, który informuje przeglądarkę o tym, że serwer pozwala poświadczenia dla żądań cross-origin.

Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka Access-kontroli-Allow-Credentials, przeglądarka nie będzie ujawniać odpowiedź do aplikacji, a żądanie AJAX nie powiodło się.

Należy zachować ostrożność, dzięki czemu poświadczenia cross-origin. Witryny sieci Web w innej domenie może wysyłać poświadczeń zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika. Specyfikacja CORS również stany to ustawienie źródeł do `"*"` (wszystkie źródła) jest nieprawidłowa Jeśli `Access-Control-Allow-Credentials` nagłówka znajduje się.

### <a name="set-the-preflight-expiration-time"></a>Ustaw czas wygaśnięcia wstępnego

Nagłówek dostępu — kontrola-Max-Age Określa, jak długo mogą być buforowane odpowiedzi na żądania wstępnego. Aby ustawić tego nagłówka:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Jak działa mechanizmu CORS

W tej sekcji opisano, co się stanie, w ramach żądania CORS na poziomie wiadomości HTTP. Należy zrozumieć, jak CORS działa tak, aby zasady CORS może być prawidłowo skonfigurowane i debugowania, gdy wystąpią nieoczekiwane wyniki.

Specyfikacja CORS wprowadza kilka nowych nagłówki HTTP, które Włączanie żądań cross-origin. Jeśli przeglądarka obsługuje mechanizm CORS, ustawia nagłówki te automatycznie dla żądań cross-origin. Niestandardowy kod JavaScript nie jest wymagane, aby włączyć mechanizm CORS.

Oto przykład żądania między źródłami. `Origin` Nagłówek zapewnia domeny obiektu, z której wysłano żądanie:

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Jeśli serwer zezwala na żądanie, ustawia nagłówka Access-Control-Allow-Origin w odpowiedzi. Wartość tego nagłówka stanowi odpowiada nagłówek źródła z żądania albo jest wartością symbol wieloznaczny "*", co oznacza, że każde źródło jest dozwolony:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Odpowiedź nie zawiera nagłówka Access-Control-Allow-Origin, żądanie AJAX nie powiodło się. W szczególności przeglądarki nie zezwalają na żądanie. Nawet jeśli serwer zwraca odpowiedź oznaczająca Powodzenie, przeglądarka nie udostępnia odpowiedzi do aplikacji klienckiej.

### <a name="preflight-requests"></a>Żądania wstępnego

W przypadku niektórych żądań CORPS przeglądarki przesyła wysłanie dodatkowego żądania o nazwie "żądania wstępnego", przed wysłaniem rzeczywistego żądania dla zasobu. Przeglądarka może pominąć żądania wstępnego, jeśli są spełnione następujące warunki:

* Metoda żądania jest GET, HEAD lub WPIS, a

* Aplikacja nie zmienia żadnych nagłówków żądania innego niż zawartości Accept, Accept-Language-Language, Content-Type lub ostatni-Event-ID, a

* Nagłówek Content-Type (jeśli ustawić) jest jednym z następujących czynności:

  * application/x-www-form-urlencoded

  * multipart/formularza data

  * zwykły tekst

Reguła o nagłówki żądania dotyczy nagłówki, które aplikacja ustawia przez wywołanie metody setRequestHeader obiektu XMLHttpRequest. (Specyfikacji CORS wywołuje te "Autor nagłówki żądania"). Reguła nie ma zastosowania do nagłówków, które można ustawić przeglądarki, takich jak Agent użytkownika, Host lub Content-Length.

Oto przykład żądania wstępnego:

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

Żądanie krótkiej wykorzystuje metodę OPTIONS protokołu HTTP. Zawiera ona dwie specjalnych nagłówków:

* Access-Control-Request-Method: Metoda HTTP, która będzie służyć do rzeczywistego żądania.

* Access-Control-Request-Headers: Lista nagłówków żądań, które aplikacja jest ustawiona na rzeczywistego żądania. (Ponownie, to nie obejmuje nagłówki, które ustawia w przeglądarce).

Poniżej przedstawiono przykładową odpowiedź, przy założeniu, że serwer zezwala na żądanie:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

Odpowiedź zawiera nagłówek dostępu — kontrola-Allow-Methods, zawierającego dozwolone metody i, opcjonalnie nagłówka Access-Control-Zezwalaj-Headers, który zawiera listę dozwolonych nagłówków. Jeśli żądania wstępnego zakończy się powodzeniem, przeglądarka wysyła rzeczywistego żądania, zgodnie z wcześniejszym opisem.
