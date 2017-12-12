---
title: Kestrel implementacja serwera sieci web platformy ASP.NET Core
author: tdykstra
description: Wprowadza Kestrel, serwer sieci web i platform dla platformy ASP.NET Core oparte na libuv.
keywords: Platformy ASP.NET Core Kestrel, libuv, prefiksy url
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 560bd66f-7dd0-4e68-b5fb-f31477e4b785
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 451c36fc9095b6e076e5287c992b6163205c523b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Wprowadzenie do Kestrel implementacja serwera sieci web platformy ASP.NET Core

Przez [Dykstra Tomasz](https://github.com/tdykstra), [Roaming Krzysztof](https://github.com/Tratcher), i [Stephen Halter](https://twitter.com/halter73)

Kestrel się między platformami [serwera sieci web dla platformy ASP.NET Core](index.md) na podstawie [libuv](https://github.com/libuv/libuv), biblioteki i platform asynchroniczne We/Wy. Kestrel to serwer sieci web, który jest domyślnie włączone w szablony projektów platformy ASP.NET Core. 

Kestrel obsługuje następujące funkcje:

  * HTTPS
  * Uaktualnienie nieprzezroczyste używane do włączania [WebSockets](https://github.com/aspnet/websockets)
  * Gniazda UNIX o wysokiej wydajności za Nginx 

Kestrel jest obsługiwana na wszystkich platformach i wersje, które obsługuje .NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

[Wyświetlić lub pobrać przykładowy kod 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

[Wyświetlić lub pobrać przykładowy kod 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Kiedy należy używać Kestrel z zwrotny serwer proxy

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Samodzielnie lub z użyciem Kestrel *zwrotnego serwera proxy*, takie jak usługi IIS, Nginx lub Apache. Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi.

![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

Albo konfiguracji &mdash; z lub bez zwrotnego serwera proxy &mdash; można również, czy Kestrel jest udostępniany tylko z siecią wewnętrzną.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

Jeśli aplikacja akceptuje żądania tylko z siecią wewnętrzną, można użyć Kestrel przez samego siebie.

![Kestrel komunikuje się bezpośrednio z sieci wewnętrznej](kestrel/_static/kestrel-to-internal.png)

Jeśli udostępnianie aplikacji z Internetem, należy użyć usług IIS, Nginx lub Apache jako *zwrotnego serwera proxy*. Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi.

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

Zwrotny serwer proxy jest wymagany w przypadku wdrożeń krawędzi (ujawniony na ruch z Internetu) ze względów bezpieczeństwa. Wersje 1.x Kestrel nie mają pełny zestaw zabezpieczenia przed atakami. Obejmuje, ale nie jest ograniczony do odpowiednich limitów czasu, limity rozmiaru i limity liczby jednoczesnych połączeń.

---

Scenariusz, który wymaga zwrotny serwer proxy jest w przypadku wielu zastosowań, które korzysta z tego samego adresu IP i port uruchomione na jednym serwerze. To nie zadziała z Kestrel bezpośrednio ponieważ Kestrel nie obsługuje udostępniania tego samego adresu IP i portu między wiele procesów. Po skonfigurowaniu Kestrel do nasłuchiwania na porcie obsługuje cały ruch do tego portu, niezależnie od tego nagłówka hosta. Zwrotny serwer proxy, który można udostępniać porty następnie musi przekazywać Kestrel na unikatowy adresu IP i portu.

Nawet jeśli zwrotnego serwera proxy nie jest wymagane, przy użyciu jednej może być dobrym rozwiązaniem z innych powodów:

* Można go ograniczyć dostępnego obszaru powierzchni.
* Zapewnia dodatkową warstwę opcjonalne konfiguracji i obrony.
* Może ją zintegrować lepiej z istniejącej infrastruktury.
* Takie rozwiązanie upraszcza równoważenia obciążenia i Konfiguracja protokołu SSL. Tylko zwrotnego serwera proxy wymaga certyfikatu SSL i że serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Jak używać Kestrel w aplikacji platformy ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pakietu znajduje się w [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

Szablony projektów platformy ASP.NET Core domyślnie używają Kestrel. W *Program.cs*, wywołań kodu szablonów `CreateDefaultBuilder`, które wywołuje [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) w tle.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Aby skonfigurować opcje Kestrel należy wywołać `UseKestrel` w *Program.cs* jak pokazano w poniższym przykładzie:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

Zainstaluj [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pakietu NuGet.

Wywołanie [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) — metoda rozszerzenia na `WebHostBuilder` w Twojej `Main` metoda określania żadnego [opcje Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) potrzebne, jak pokazano w następnej sekcji.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Opcje kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Serwer sieci web Kestrel ma ograniczenie opcje konfiguracji, które są szczególnie przydatne w przypadku wdrożeń skierowane do Internetu. Oto niektóre ograniczenia, które można ustawić:

- Maksymalna liczba połączeń klientów
- Żądanie maksymalny rozmiar treści
- Szybkość danych treści żądania minimalna

Ustaw tych warunków ograniczających i innych reguł w `Limits` właściwość [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) klasy. `Limits` Właściwość przechowuje wystąpienia [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) klasy. 

**Maksymalna liczba połączeń klientów**

Można ustawić maksymalną liczbę równoczesnych otwartych połączeń TCP dla całej aplikacji z następującym kodem:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

Brak oddzielnych limit połączeń, które zostały uaktualnione do inny protokół (na przykład na żądanie Websocket) z HTTP lub HTTPS. Po uaktualnieniu połączenie nie jest uwzględniane `MaxConcurrentConnections` limit. 

Maksymalna liczba połączeń jest nieograniczony (null) domyślnie.

**Żądanie maksymalny rozmiar treści**

Domyślny rozmiar treści żądania maksymalna jest 30,000,000 bajtów, czyli około 28.6 MB. 

Zalecanym sposobem zastąpienie limit w aplikacji platformy ASP.NET Core MVC jest użycie [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atrybutu metody akcji:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Oto przykład, który pokazuje, jak skonfigurować ograniczenia dla całej aplikacji, każde żądanie:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

Można zastąpić ustawienie dla określonego żądania w oprogramowaniu pośredniczącym:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
Jeśli próbujesz skonfigurować limit na żądanie, po uruchomieniu aplikacji odczytu żądania, jest zgłaszany wyjątek. Brak `IsReadOnly` właściwość, która informuje, jeśli `MaxRequestBodySize` właściwość jest w stanie tylko do odczytu, co oznacza jest za późno skonfigurować limit.

**Szybkość danych treści żądania minimalna**

Kestrel sprawdza co sekundę, gdy dane pochodzi szybkością określony w bajtach na sekundę. Jeżeli stawki spadnie poniżej wartości minimalnej, jest upłynął limit czasu połączenia. Okres prolongaty wynosi ilość czasu, że Kestrel daje klientowi zwiększyć jego częstotliwość wysyłania do minimum; w tym czasie nie zaznaczono stawki. Okres prolongaty pomaga uniknąć porzucenie połączeń, które początkowo wysyłania danych prędkością wolne ze względu na wolne TCP-start.

Minimalna szybkość domyślna jest 240 bajtów na sekundę, z okresu prolongaty 5-sekundowego.

Minimalna częstotliwość ma również zastosowanie do odpowiedzi. Kod, aby ustawić limit żądań i odpowiedzi limit jest taki sam, z wyjątkiem o `RequestBody` lub `Response` w nazwach właściwości i interfejsu. 

Oto przykład pokazujący sposób konfigurowania stawki minimalną ilość danych w *Program.cs*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

Stawki na żądanie można skonfigurować w oprogramowaniu pośredniczącym:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Aby uzyskać informacje o innych opcjach Kestrel zobacz następujące klasy:

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

Aby uzyskać informacje o opcjach Kestrel, zobacz [KestrelServerOptions klasy](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).

---

### <a name="endpoint-configuration"></a>Konfiguracja punktu końcowego

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Domyślnie wiąże platformy ASP.NET Core `http://localhost:5000`. Skonfiguruj prefiksy URL i portów dla Kestrel do nasłuchiwania przez wywołanie metody `Listen` lub `ListenUnixSocket` metody `KestrelServerOptions`. (`UseUrls`, `urls` argumentu wiersza polecenia i zmienną środowiskową ASPNETCORE_URLS również służbowych, ale ma ograniczenia zauważyć [dalszej części tego artykułu](#useurls-limitations).)

**Powiązać gniazda TCP**

`Listen` Metody wiąże gniazda TCP i lambda opcje można skonfigurować certyfikat SSL:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Zwróć uwagę, jak ten przykład konfiguruje SSL dla danego punktu końcowego za pomocą [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs). Skonfigurować inne ustawienia Kestrel dla konkretnego punktów końcowych, można użyć tego samego interfejsu API.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Powiązany z gniazdem systemu Unix**

Jak pokazano w tym przykładzie można nasłuchiwać gniazda Unix zwiększonej wydajności z Nginx:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Port 0**

Jeśli określisz numeru portu 0 Kestrel wiąże dynamicznie dostępnego portu. Poniższy przykład przedstawia sposób określić port, który faktycznie powiązany Kestrel w czasie wykonywania:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**Ograniczenia UseUrls**

Punkty końcowe można skonfigurować, wywołując `UseUrls` metody lub używając `urls` argumentu wiersza polecenia lub zmiennej środowiskowej ASPNETCORE_URLS. Te metody są przydatne w przypadku kodu do pracy z serwerami innych niż Kestrel. Jednak należy pamiętać o tych ograniczeniach:

* Nie można użyć protokołu SSL przy użyciu tych metod.
* Jeśli używasz zarówno `Listen` — metoda i `UseUrls`, `Listen` zastąpienie punktów końcowych `UseUrls` punktów końcowych.

**Konfiguracja punktu końcowego dla usług IIS**

Jeśli używasz usług IIS, powiązania adres URL dla usług IIS zastąpienie powiązań ustawionych przez wywołanie albo `Listen` lub `UseUrls`. Aby uzyskać więcej informacji, zobacz [wprowadzenie do platformy ASP.NET Core modułu](aspnet-core-module.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

Domyślnie wiąże platformy ASP.NET Core `http://localhost:5000`. Istnieje możliwość skonfigurowania prefiksy URL i portów dla Kestrel nasłuchiwać przy użyciu `UseUrls` — metoda rozszerzenia, `urls` argumentu wiersza polecenia lub systemu konfiguracji platformy ASP.NET Core. Aby uzyskać więcej informacji na temat tych metod, zobacz [hostingu](../../fundamentals/hosting.md). Aby uzyskać informacji na temat działania powiązanie adresu URL podczas używania serwera IIS jako zwrotny serwer proxy, zobacz [platformy ASP.NET Core modułu](aspnet-core-module.md). 

---

### <a name="url-prefixes"></a>Prefiksy adresów URL

Jeśli należy wywołać `UseUrls` lub użyj `urls` argumentu wiersza polecenia lub zmiennej środowiskowej ASPNETCORE_URLS prefiksy URL może być w dowolnym z następujących formatów. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Tylko prefiksy HTTP URL są ważne. Kestrel nie obsługuje protokołu SSL podczas konfigurowania powiązania adresu URL za pomocą `UseUrls`.

* Adres IPv4 z numerem portu

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 jest szczególnych przypadkach, która jest powiązana z wszystkich adresów IPv4.


* Adres IPv6 z numerem portu

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [:] jest odpowiednikiem IPv6 IPv4 0.0.0.0.


* Nazwa hosta z numerem portu

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Host nazwy, *, a + nie są specjalne. Wszystko, co nie jest rozpoznany adres IP lub "localhost" będzie powiązać wszystkich protokołów IPv4 i IPv6, adresy IP. Aby powiązać różne nazwy hostów z różnych aplikacji platformy ASP.NET Core w tym samym porcie, należy użyć [HTTP.sys](httpsys.md) lub zwrotnego serwera proxy usług IIS, Nginx lub Apache.

* Nazwa "Localhost" port numer lub sprzężenia zwrotnego protokołu IP z numerem portu

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Gdy `localhost` określono Kestrel próbuje powiązać do interfejsu sprzężenia zwrotnego protokołów IPv4 i IPv6. Jeśli żądany port jest używany przez inną usługę albo interfejsu sprzężenia zwrotnego, Kestrel nie powiedzie się. Jeśli z jakiegokolwiek powodu albo interfejsu sprzężenia zwrotnego jest niedostępny (większość często, ponieważ protokół IPv6 nie jest obsługiwany.), Kestrel dzienniki ostrzeżenie. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

* Adres IPv4 z numerem portu

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 jest szczególnych przypadkach, która jest powiązana z wszystkich adresów IPv4.


* Adres IPv6 z numerem portu

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [:] jest odpowiednikiem IPv6 IPv4 0.0.0.0.


* Nazwa hosta z numerem portu

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Nazwy, hostów \*, i + nie są specjalne. Wszystko, co nie jest rozpoznany adres IP lub "localhost" wiąże się do wszystkich protokołów IPv4 i IPv6, adresy IP. Aby powiązać różne nazwy hostów z różnych aplikacji platformy ASP.NET Core w tym samym porcie, należy użyć [WebListener](weblistener.md) lub zwrotnego serwera proxy usług IIS, Nginx lub Apache.

* Nazwa "Localhost" port numer lub sprzężenia zwrotnego protokołu IP z numerem portu

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Gdy `localhost` określono Kestrel próbuje powiązać do interfejsu sprzężenia zwrotnego protokołów IPv4 i IPv6. Jeśli żądany port jest używany przez inną usługę albo interfejsu sprzężenia zwrotnego, Kestrel nie powiedzie się. Jeśli z jakiegokolwiek powodu albo interfejsu sprzężenia zwrotnego jest niedostępny (większość często, ponieważ protokół IPv6 nie jest obsługiwany.), Kestrel dzienniki ostrzeżenie. 

* Gniazda systemu UNIX

  ```
  http://unix:/run/dan-live.sock  
  ```

**Port 0**

Jeśli określisz numeru portu 0 Kestrel wiąże dynamicznie dostępnego portu. Powiązanie z portu 0 jest dozwolony dla dowolnej nazwy hosta lub adres IP, z wyjątkiem `localhost` nazwy.

Poniższy przykład przedstawia sposób określić port, który faktycznie powiązany Kestrel w czasie wykonywania:

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**Prefiksy adresów URL dla protokołu SSL**

Należy uwzględnić prefiksy URL z `https:` połączeń `UseHttps` — metoda rozszerzenia, jak pokazano poniżej.

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> HTTPS i HTTP nie może być umieszczona w tym samym porcie.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Przykładowa aplikacja dla 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Kod źródłowy kestrel](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Przykładowa aplikacja dla 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Kod źródłowy kestrel](https://github.com/aspnet/KestrelHttpServer)

---
