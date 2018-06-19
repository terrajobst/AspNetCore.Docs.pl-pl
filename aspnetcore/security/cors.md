---
title: Włącz żądania między źródłami (CORS) w platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak CORS jako standard zezwalających lub odrzucanie żądań cross-origin w aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cors
ms.openlocfilehash: 3c5d0840426c7ed52353a7832a1a1959027121de
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077551"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Włącz żądania między źródłami (CORS) w platformy ASP.NET Core

Przez [Wasson Jan](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), i [Dykstra niestandardowy](https://github.com/tdykstra)

Poziom zabezpieczeń przeglądarki uniemożliwia wprowadzanie żądania AJAX do innej domeny przez stronę sieci web. To ograniczenie jest nazywany *zasad samego pochodzenia*i zapobiega złośliwa witryna odczytywanie danych poufnych z innej lokacji. Jednak czasami może mają mieć możliwość innych witryn, wprowadzić żądań cross-origin w interfejsie API sieci web.

[Krzyżowe udostępniania zasobów pochodzenia](http://www.w3.org/TR/cors/) (CORS) jest standardem W3C, dzięki której serwer złagodzenie zasad tego samego źródła. Przy użyciu mechanizmu CORS, serwer można jawnie zezwolić na niektórych żądań cross-origin podczas odrzucenia innych użytkowników. CORS jest bezpieczniejsze i bardziej elastyczne niż wcześniejszych metod takich jak [JSONP](https://wikipedia.org/wiki/JSONP). W tym temacie pokazano, jak włączyć mechanizm CORS w aplikacji platformy ASP.NET Core.

## <a name="what-is-same-origin"></a>Co to jest "tego samego źródła"?

Dwa adresy URL mają tego samego źródła, gdy mają identyczne schematy, hostów i portów. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Te dwa adresy URL są tego samego źródła:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Tych adresów URL mają różne źródła niż poprzedniej dwóch:

* `http://example.net` -Innej domeny

* `http://www.example.com/foo.html` -Różnych poddomeny

* `https://example.com/foo.html` -Inny schemat

* `http://example.com:9000/foo.html` -Różnych portów:

> [!NOTE]
> Program Internet Explorer nie należy wziąć pod uwagę portu podczas porównywania źródeł.

## <a name="setting-up-cors"></a>Konfigurowanie mechanizmu CORS

Aby skonfigurować dodać CORS dla aplikacji `Microsoft.AspNetCore.Cors` pakietu do projektu.

Dodaj usługi CORS w pliku Startup.cs:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>Włączanie mechanizmu CORS z oprogramowania pośredniczącego

Aby włączyć mechanizm CORS dla całej aplikacji Dodaj oprogramowanie pośredniczące CORS do Twojego żądania potoku przy użyciu `UseCors` — metoda rozszerzenia. Należy pamiętać, że oprogramowanie pośredniczące CORS musi poprzedzać żadnych zdefiniowanych punktów końcowych w aplikacji do obsługi żądań cross-origin (np. przed wywołaniem dowolnej `UseMvc`).

Podczas dodawania przy użyciu oprogramowanie pośredniczące CORS, można określić zasady cross-origin `CorsPolicyBuilder` klasy. Istnieją dwa sposoby, w tym celu. Pierwsza to wywołanie UseCors z wyrażenia lambda:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Uwaga:** musi być określony adres URL bez ukośnika (`/`). Jeśli adres URL kończy z `/`, zwróci porównanie `false` i zostanie zwrócony bez nagłówka.

Wyrażenie lambda ma `CorsPolicyBuilder` obiektu. Listę można znaleźć [opcje konfiguracji](#cors-policy-options) dalszej części tego tematu. W tym przykładzie zasady umożliwia żądań cross-origin z `http://example.com` i innych źródeł.

Należy pamiętać, że CorsPolicyBuilder ma fluent API, więc tworzenia łańcucha wywołań metody:

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

Drugi podejściem jest zdefiniować co najmniej jedne zasady CORS nazwanego, a następnie wybierz zasady według nazwy w czasie wykonywania.

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

W tym przykładzie dodaje zasady CORS o nazwie "AllowSpecificOrigin". Aby wybrać zasady, przekaż nazwę `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Włączanie mechanizmu CORS na platformie MVC

MVC służy również do zastosowania określonego CORS każdej akcji, każdy kontroler lub globalnie do wszystkich kontrolerów. W przypadku używania MVC do włączenia CORS są używane te same usługi CORS, ale nie jest oprogramowanie pośredniczące CORS.

### <a name="per-action"></a>Każdej akcji

Aby określić Dodaj zasad CORS dla danego działania `[EnableCors]` atrybutu w celu wykonania akcji. Określ nazwę zasady.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Każdy kontroler

Aby określić zasady CORS dla określonego kontrolera Dodaj `[EnableCors]` atrybutu do klasy kontrolera. Określ nazwę zasady.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Globalny

Można włączyć mechanizm CORS globalnie do wszystkich kontrolerów przez dodanie `CorsAuthorizationFilterFactory` filtr do kolekcji filtrów globalnych:

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

Kolejność jest: działania, kontrolera, globalnych. Zasad na poziomie akcji mają pierwszeństwo przed zasad na poziomie kontrolera, a zasad na poziomie kontrolera mają pierwszeństwo przed zasadami globalnego.

### <a name="disable-cors"></a>Wyłączenia CORS

Aby wyłączyć CORS dla kontrolera lub akcji, należy użyć `[DisableCors]` atrybutu.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>Opcje zasad CORS

W tej sekcji opisano różne opcje, które można ustawić zasady CORS.

* [Ustaw dozwolonych źródeł](#set-the-allowed-origins)

* [Ustaw dopuszczalnych metod HTTP](#set-the-allowed-http-methods)

* [Ustawia nagłówki żądania dozwolone](#set-the-allowed-request-headers)

* [Ustawianie nagłówków odpowiedzi narażonych](#set-the-exposed-response-headers)

* [Poświadczenia w żądań cross-origin](#credentials-in-cross-origin-requests)

* [Ustawianie czasu wygaśnięcia wstępnego](#set-the-preflight-expiration-time)

Dla niektórych opcji może być przydatne do odczytu [działa jak CORS](#how-cors-works) pierwszy.

### <a name="set-the-allowed-origins"></a>Ustaw dozwolonych źródeł

Aby zezwolić na co najmniej jeden z określonych źródeł:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Aby zezwolić na wszystkie pochodzenia:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Starannie rozważ, przed zezwoleniem na żądania z dowolnego źródła. Oznacza to, że dosłownie w dowolnej witrynie sieci Web można wykonywać wywołania AJAX do interfejsu API.

### <a name="set-the-allowed-http-methods"></a>Ustaw dopuszczalnych metod HTTP

Aby zezwolić na wszystkie metody HTTP:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Ma to wpływ na żądania wstępnego transmitowane i nagłówek dostępu-formant-Allow-Methods.

### <a name="set-the-allowed-request-headers"></a>Ustawia nagłówki żądania dozwolone

Żądania wstępnego CORS może obejmować nagłówka Access-Control-Request-Headers, lista nagłówków HTTP, ustawiane przez aplikację (tak zwane "Autor nagłówki żądania").

Do listy dozwolonych adresów IP określonych nagłówków:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Aby umożliwić tworzenie wszystkie nagłówki żądania:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Przeglądarki nie są całkowicie zgodne, w konfiguracji do programu Access-Control-Request-Headers. Po ustawieniu nagłówki na niczego innych niż "*", użytkownik powinien zawierać co najmniej "Zaakceptuj", "content-type", "origin", a także niestandardowe nagłówki, które mają być obsługiwane.

### <a name="set-the-exposed-response-headers"></a>Ustawianie nagłówków odpowiedzi narażonych

Domyślnie przeglądarka nie pokazuje wszystkie nagłówki odpowiedzi do aplikacji. (Zobacz [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Nagłówki odpowiedzi, które są domyślnie dostępne są następujące:

* Cache-Control

* Język zawartości

* Typ zawartości

* Wygasa

* Last-Modified

* Wartość dyrektywy pragma

Specyfikacja CORS wywołuje te *nagłówki odpowiedzi proste*. Aby udostępnić nagłówki innych aplikacji:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Poświadczenia w żądań cross-origin

Poświadczenia wymagają specjalnej obsługi żądania CORS. Domyślnie przeglądarka nie wysyła żadnych poświadczeń z żądaniem cross-origin. Poświadczenia obejmują pliki cookie, a także schematy uwierzytelniania HTTP. Aby wysłać poświadczeń z żądaniem cross-origin, klient musi Ustaw XMLHttpRequest.withCredentials wartość true.

Bezpośrednio za pomocą obiektu XMLHttpRequest:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

W jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Ponadto poświadczenia muszą zezwalać na serwerze. Aby umożliwić cross-origin poświadczeń:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Teraz odpowiedzi HTTP będzie zawierać nagłówka dostępu-formant-Allow-Credentials, który informuje przeglądarkę, że serwer umożliwia poświadczenia dla żądań cross-origin.

Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka dostępu-formant-Allow-Credentials, przeglądarka nie uwidacznia odpowiedzi do aplikacji i żądanie AJAX nie powiedzie się.

Należy zachować ostrożność w przypadku zezwalania poświadczenia cross-origin. Witryny sieci Web w innej domenie można wysłać poświadczeń zalogowanego użytkownika do aplikacji w imieniu użytkownika bez wiedzy użytkownika. Specyfikacja CORS stany również ustawienie źródeł do "*" (wszystkie pochodzenia) jest nieprawidłowa Jeśli `Access-Control-Allow-Credentials` nagłówka jest obecny.

### <a name="set-the-preflight-expiration-time"></a>Ustawianie czasu wygaśnięcia wstępnego

Nagłówka Access-formant-Max-Age Określa, jak długo mogą być buforowane odpowiedzi na żądania wstępnego. Aby ustawić ten nagłówek:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Jak działa CORS

W tej sekcji opisano, co się stanie w żądaniu CORS na poziomie wiadomości HTTP. Należy zrozumieć działanie CORS, dzięki czemu można poprawnie skonfigurować zasady CORS i troubleshooted, gdy występują nieoczekiwane wyniki.

Specyfikacja CORS wprowadzono kilka nowych nagłówków HTTP, które Włączanie żądań cross-origin. Jeśli przeglądarka obsługuje mechanizm CORS, ustawia automatycznie tych nagłówków żądań cross-origin. Niestandardowy kod JavaScript nie jest wymagane do włączenia CORS.

Oto przykład żądań cross-origin. `Origin` Domeny witryny, do której wysłano żądanie zawiera nagłówek:

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

Jeśli serwer zezwala na żądanie, ustawia nagłówka Access-Control-Allow-Origin w odpowiedzi. Wartość tego nagłówka dopasowuje początek nagłówek z żądania albo ma wartość symbol wieloznaczny "*", co oznacza, że każde źródło jest dozwolone:

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

Jeżeli odpowiedź nie zawiera nagłówka Access-Control-Allow-Origin, żądanie AJAX nie powiedzie się. W szczególności przeglądarki nie zezwala na żądanie. Nawet wtedy, gdy serwer zwraca odpowiedź oznaczająca Powodzenie, przeglądarka nie udostępnia odpowiedzi aplikacji klienckiej.

### <a name="preflight-requests"></a>Żądania wstępnego

Dla niektórych żądań CORS przeglądarce wysyła żądanie dodatkowych, o nazwie "żądania wstępnego", przed wysłaniem rzeczywistego żądania dla zasobu. Przeglądarka może pominąć żądania wstępnego, jeśli są spełnione następujące warunki:

* Metoda żądania jest GET, HEAD lub POST, a

* Aplikacja nie zmienia żadnych nagłówków żądania innego niż Akceptuj, Accept-Language, Content-Language, Content-Type lub Last-zdarzenia-ID, a

* Nagłówek Content-Type (Jeśli ustawiona) jest jednym z następujących czynności:

  * application/x-www-form-urlencoded

  * dane multipart/formularza

  * zwykły tekst

Reguła o nagłówków żądań ma zastosowanie do nagłówki, które ustawia aplikacji przez wywołanie metody setRequestHeader obiektu XMLHttpRequest. (Specyfikacja CORS wywołuje te "Autor nagłówki żądania"). Reguła nie ma zastosowania do nagłówków, które można ustawić przeglądarki, takich jak Agent użytkownika, hosta lub Content-Length.

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

Żądania wstępnego transmitowane używa metody OPTIONS protokołu HTTP. Zawiera dwa nagłówki specjalne:

* Access-Control-Request-Method: Metoda HTTP, która będzie służyć do rzeczywistego żądania.

* Access-Control-Request-Headers: Lista nagłówków żądań, których aplikacja jest ustawiona na rzeczywistego żądania. (Ponownie, to nie obejmuje nagłówki, które ustawia przeglądarki).

Oto przykład odpowiedzi, przy założeniu, że serwer zezwala na żądanie:

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

Odpowiedź zawiera nagłówek dostępu-formant-Allow-Methods, który wyświetla listę dozwolonych metod i opcjonalnie nagłówka Access-Control-Zezwalaj-Headers, który zawiera listę dozwolone nagłówki. Jeśli żądania wstępnego zakończy się powodzeniem, przeglądarka wysyła rzeczywistego żądania, zgodnie z wcześniejszym opisem.
