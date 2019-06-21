---
title: Implementacja serwera sieci web kestrel w programie ASP.NET Core
author: guardrex
description: Więcej informacji na temat Kestrel, serwer sieci web dla wielu platform dla platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/18/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: fe74583cdc1c8fa40ab12b4b31dc47f5e0296a6c
ms.sourcegitcommit: 3eedd6180fbbdcb81a8e1ebdbeb035bf4f2feb92
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284528"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>Implementacja serwera sieci web kestrel w programie ASP.NET Core

Przez [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), i [Halter Autor: Stephen](https://twitter.com/halter73)

Kestrel jest dla wielu platform [serwera sieci web dla platformy ASP.NET Core](xref:fundamentals/servers/index). Kestrel jest serwer sieci web, który znajduje się domyślnie w szablonach projektu ASP.NET Core.

Kestrel obsługuje następujące scenariusze:

::: moniker range=">= aspnetcore-2.2"

* HTTPS
* Nieprzezroczysty uaktualnienia, które umożliwiają [WebSockets](https://github.com/aspnet/websockets)
* Gniazda systemu UNIX, dla wysoko wydajnych za serwera Nginx
* Protokołu HTTP/2 (z wyjątkiem w systemie macOS&dagger;)

&dagger;Protokołu HTTP/2 będą obsługiwane w systemie macOS w przyszłej wersji.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* HTTPS
* Nieprzezroczysty uaktualnienia, które umożliwiają [WebSockets](https://github.com/aspnet/websockets)
* Gniazda systemu UNIX, dla wysoko wydajnych za serwera Nginx

::: moniker-end

Kestrel jest obsługiwane na wszystkich platformach i wersjach, które obsługuje platformy .NET Core.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a>Obsługa protokołu HTTP/2

[Protokołu HTTP/2](https://httpwg.org/specs/rfc7540.html) jest dostępna dla aplikacji platformy ASP.NET Core, jeśli następujące podstawowa wymagania zostały spełnione:

* System operacyjny&dagger;
  * Windows Server 2016 i Windows 10 lub nowszym&Dagger;
  * Linux z protokołem OpenSSL 1.0.2 lub nowszej (na przykład Ubuntu 16.04 lub nowszy)
* Platforma docelowa: .NET Core 2,2 lub nowszy
* [Negocjowania protokołu warstwy aplikacji (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) połączenia
* Protokół TLS 1.2 lub nowszej połączenia

&dagger;Protokołu HTTP/2 będą obsługiwane w systemie macOS w przyszłej wersji.
&Dagger;Kestrel ma ograniczoną obsługę protokołu HTTP/2, Windows Server 2012 R2 i Windows 8.1. Obsługa jest ograniczona, ponieważ lista obsługiwanych mechanizmów szyfrowania TLS dostępnych w tych systemach operacyjnych jest ograniczona. Certyfikat wygenerowany za pomocą Elliptic krzywej cyfrowego Signature Algorithm (ECDSA) może być konieczne bezpiecznych połączeń TLS.

Jeśli zostanie nawiązane połączenie HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/2`.

Protokołu HTTP/2 jest domyślnie wyłączona. Aby uzyskać więcej informacji na temat konfigurowania, zobacz [opcje Kestrel](#kestrel-options) i [ListenOptions.Protocols](#listenoptionsprotocols) sekcje.

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy

Kestrel może służyć przez siebie lub za pomocą *zwrotnego serwera proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), lub [Apache](https://httpd.apache.org/). Serwer proxy odwrotnej odbiera żądania HTTP z sieci i przekazuje je do Kestrel.

Kestrel używany jako serwer sieci web do krawędzi (dostępnym z Internetu):

![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

Kestrel używane w konfiguracji zwrotny serwer proxy:

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotny serwer proxy, na przykład serwer Nginx, Apache lub IIS](kestrel/_static/kestrel-to-internet.png)

Każda konfiguracja&mdash;z lub bez serwera proxy odwrotnej&mdash;jest obsługiwana konfiguracja hostingu platformy ASP.NET Core 2.1 lub nowszej aplikacje, które odbierać żądania z Internetu.

Kestrel używany jako serwer graniczny bez serwera proxy odwrotnej nie obsługuje udostępniania tych samych adresów IP i port między różne procesy. Gdy Kestrel jest skonfigurowana do nasłuchiwania na porcie, Kestrel obsługuje cały ruch do tego portu, niezależnie od tego, żądania `Host` nagłówków. Zwrotny serwer proxy, który można udostępniać porty zdolność do przekazywania żądań do Kestrel na unikatowych adresów IP i porcie.

Nawet jeśli zwrotnego serwera proxy nie jest wymagane, przy użyciu zwrotnego serwera proxy może być dobrym wyborem.

Zwrotny serwer proxy:

* Można ograniczyć narażonych publiczny obszar powierzchni aplikacji, które zawiera.
* Zapewniają dodatkową warstwę konfiguracji i obrony.
* Może lepiej zintegrować z istniejącą infrastrukturą.
* Uprość Równoważenie obciążenia i konfiguracji bezpiecznej komunikacji (HTTPS). Tylko zwrotnego serwera proxy wymaga certyfikatu X.509, a ten serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej za pomocą zwykłego protokołu HTTP.

> [!WARNING]
> Hosting w konfiguracji zwrotny serwer proxy wymaga [filtrowania host](#host-filtering).

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Jak używać Kestrel w aplikacji platformy ASP.NET Core

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pakietu znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej).

Szablony projektów programu ASP.NET Core używa Kestrel domyślnie. W *Program.cs*, kod wywołuje szablon <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, która wywołuje metodę <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> w tle.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker range=">= aspnetcore-2.2"

Zapewnienie dodatkowej konfiguracji po wywołaniu `CreateDefaultBuilder`, użyj `ConfigureKestrel`:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

Jeśli aplikacja nie wywołać `CreateDefaultBuilder` Aby skonfigurować hosta, należy wywołać <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **przed** wywoływania `ConfigureKestrel`:

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        })
        .Build();

    host.Run();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Zapewnienie dodatkowej konfiguracji po wywołaniu `CreateDefaultBuilder`, wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

## <a name="kestrel-options"></a>Opcje kestrel

Serwer sieci web Kestrel ma ograniczenie opcje konfiguracji, które są szczególnie przydatne w przypadku wdrożeń z Internetu.

Ustawianie ograniczeń na <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> właściwość <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> klasy. `Limits` Właściwość posiada wystąpienie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> klasy.

W poniższych przykładach używane <xref:Microsoft.AspNetCore.Server.Kestrel.Core> przestrzeni nazw:

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a>Limit czasu utrzymywania aktywności

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

Pobiera lub ustawia [limitu czasu podtrzymania](https://tools.ietf.org/html/rfc7230#section-6.5). Wartość domyślna to 2 minuty.

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

::: moniker-end

### <a name="maximum-client-connections"></a>Maksymalna liczba połączeń klientów

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

Można ustawić maksymalną liczbę równoczesnych otwarte połączenia TCP dla całej aplikacji, używając następującego kodu:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

Ma osobnych limitu połączeń, które zostały uaktualnione do innego protokołu (na przykład na żądanie funkcji WebSockets) z protokołu HTTP lub HTTPS. Po uaktualnieniu połączenie nie jest uwzględniane `MaxConcurrentConnections` limit.

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

Maksymalna liczba połączeń jest nieograniczona (null) domyślnie.

### <a name="maximum-request-body-size"></a>Rozmiar treści żądania maksymalna

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

Domyślny rozmiar treści żądania maksymalna jest 30,000,000 bajtów, czyli około 28.6 MB.

Zalecane podejście do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> atrybutu metody akcji:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Oto przykład pokazujący sposób konfigurowania ograniczeń aplikacji na każde żądanie:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

Można zastąpić ustawienia dla konkretnego żądania w oprogramowaniu pośredniczącym:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

Wyjątek jest generowany, Jeśli spróbujesz skonfigurować limit na żądanie, po uruchomieniu aplikacji do odczytania żądania. Brak `IsReadOnly` właściwość, która wskazuje, czy `MaxRequestBodySize` właściwość jest w stanie tylko do odczytu, co oznacza, jest za późno, aby skonfigurować limit.

Gdy aplikacja jest uruchamiana [spoza procesu](xref:host-and-deploy/iis/index#out-of-process-hosting-model) za [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru w treści żądania Kestrel firmy jest wyłączony, ponieważ usługi IIS ustawiają już limit.

### <a name="minimum-request-body-data-rate"></a>Szybkość danych treści żądania minimalne

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

Kestrel sprawdza co sekundę, jeśli danych jest odebranych zgodnie ze stawką określoną w bajtach na sekundę. Jeśli szybkość spadnie poniżej minimum, jest upłynął limit czasu połączenia. Okres prolongaty to ilość czasu, że Kestrel daje klienta, aby zwiększyć jej szybkość wysyłania do minimum; wskaźnik nie jest sprawdzana w tym czasie. Okres prolongaty pomaga uniknąć porzucenie połączeń, które początkowo wysyłania danych stawki wolno z powodu start wolne TCP.

Minimalna szybkość domyślna jest 240 bajtów na sekundę z 5-sekundowego okresu prolongaty.

Minimalna szybkość dotyczy również odpowiedzi. Kod, aby ustawić limit żądań i limit odpowiedzi jest taki sam, z wyjątkiem o `RequestBody` lub `Response` w nazwach właściwości i interfejsu.

Oto przykład pokazujący sposób konfigurowania kursów minimalną ilość danych w *Program.cs*:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

::: moniker-end

Można zastąpić minimalne limitom na żądanie w oprogramowaniu pośredniczącym:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

::: moniker range=">= aspnetcore-2.2"

Ani funkcji szybkości, do którego odwołuje się poprzedniego przykładu znajdują się w `HttpContext.Features` dla żądania HTTP/2, ponieważ modyfikowanie limitów szybkości na podstawie danego żądania nie jest obsługiwane ze względu na obsługę protokołu Multipleksowanie żądania protokołu HTTP/2. Limity szybkości całego serwera, konfigurować za pośrednictwem `KestrelServerOptions.Limits` nadal mają zastosowanie do połączeń zarówno HTTP/1.x, jak i protokołu HTTP/2.

::: moniker-end

### <a name="request-headers-timeout"></a>Limit czasu nagłówki żądania

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

Pobiera lub ustawia maksymalną ilość czasu serwer zużywa odbieranie nagłówki żądania. Wartość domyślna to 30 sekund.

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a>Maksymalna strumienie na połączenie

`Http2.MaxStreamsPerConnection` ogranicza liczbę jednoczesnych żądań strumieni na połączenie HTTP/2. Nadmiarowe strumieni zostaną odrzucone.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

Wartość domyślna to 100.

### <a name="header-table-size"></a>Rozmiar tabeli nagłówka

Dekoder HPACK dekompresuje nagłówki HTTP dla połączeń HTTP/2. `Http2.HeaderTableSize` ogranicza rozmiar tabeli kompresji nagłówek, który używa dekodera HPACK. Wartość znajduje się w oktetach i musi być większa niż zero (0).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

Wartość domyślna to 4096.

### <a name="maximum-frame-size"></a>Maksymalny rozmiar ramki

`Http2.MaxFrameSize` Określa maksymalny rozmiar ładunku ramki połączenia protokołu HTTP/2 do odbierania. Wartość znajduje się w oktetach i musi mieć długość od 2 ^ 14 (16 384) i 2 ^ 24-1 (16 777 215).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

Wartość domyślna to 2 ^ 14 (16 384).

### <a name="maximum-request-header-size"></a>Rozmiar nagłówka żądania maksymalna

`Http2.MaxRequestHeaderFieldSize` Określa maksymalny dopuszczalny rozmiar w oktetach wartości nagłówka żądania. Ten limit dotyczy zarówno nazwę, jak i wartości ze sobą w ich skompresowanym i nieskompresowanym oświadczenia. Wartość musi być większa niż zero (0).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

Wartość domyślna to 8192.

### <a name="initial-connection-window-size"></a>Rozmiar okna początkowego połączenia

`Http2.InitialConnectionWindowSize` Wskazuje danych treści żądania maksymalny w bajtach bufory serwera w tym samym czasie agregowana dla wszystkich żądań (strumienie) na połączenie. Żądania są również ograniczyć, używając `Http2.InitialStreamWindowSize`. Wartość musi być mniejsza niż 65 535 i mniej niż 2 ^ 31 (2 147 483 648).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

Wartość domyślna to 128 KB (131072).

### <a name="initial-stream-window-size"></a>Rozmiar okna początkowej strumienia

`Http2.InitialStreamWindowSize` Wskazuje danych treści żądania maksymalny w bajtach bufory serwera, w tym samym czasie na żądanie (strumień). Żądania są również ograniczyć, używając `Http2.InitialStreamWindowSize`. Wartość musi być mniejsza niż 65 535 i mniej niż 2 ^ 31 (2 147 483 648).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

Wartość domyślna to (98304) o rozmiarze 96 KB.

::: moniker-end

Aby uzyskać informacje o innych opcjach Kestrel i limity zobacz:

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a>Konfiguracja punktu końcowego

Domyślnie platforma ASP.NET Core wiąże:

* `http://localhost:5000`
* `https://localhost:5001` (gdy certyfikat rozwoju lokalnego znajduje się)

Określ adresy URL przy użyciu:

* `ASPNETCORE_URLS` zmiennej środowiskowej.
* `--urls` argument wiersza polecenia.
* `urls` Klucz konfiguracji hosta.
* `UseUrls` metody rozszerzenia.

Podana wartość przy użyciu tych metod może być co najmniej jeden protokół HTTP i HTTPS punktów końcowych (HTTPS Jeśli dostępny jest certyfikatu z domyślną). Konfiguracja uwzględnia wartość będącą rozdzielonej średnikami liście (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).

Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwerów](xref:fundamentals/host/web-host#server-urls) i [zastępczą konfigurację](xref:fundamentals/host/web-host#override-configuration).

Certyfikat programistyczny jest tworzony:

* Gdy [zestawu .NET Core SDK](/dotnet/core/sdk) jest zainstalowany.
* [Narzędzia dev-certs](xref:aspnetcore-2.1#https) służy do tworzenia certyfikatu.

Niektóre przeglądarki wymagają przyznanie jawnych uprawnień w przeglądarce ufać certyfikatowi rozwoju lokalnego.

Nowsze szablony projektów i ASP.NET Core 2.1 Konfigurowanie aplikacji są domyślnie uruchamiane przy użyciu protokołu HTTPS i dołączenie [przekierowania protokołu HTTPS i HSTS obsługują](xref:security/enforcing-ssl).

Wywołaj <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> metod <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> skonfigurować przedrostki adresów URL i portów dla Kestrel.

`UseUrls`, `--urls` argument wiersza polecenia `urls` klucz konfiguracji hosta, a `ASPNETCORE_URLS` zmiennej środowiskowej również działają, ale ma ograniczenia, zauważyć później, w tej sekcji (domyślny certyfikat musi być dostępny dla punktu końcowego protokołu HTTPS Konfiguracja).

Platforma ASP.NET Core 2.1 lub nowszej `KestrelServerOptions` konfiguracji:

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a>ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)

Określa konfigurację `Action` do uruchamiania dla każdego określonego punktu końcowego. Wywoływanie `ConfigureEndpointDefaults` wielokrotnie zastępuje starszy `Action`s w ostatnich `Action` określony.

::: moniker range=">= aspnetcore-3.0"

```csharp
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.ConfigureKestrel(serverOptions =>
        {
            serverOptions.ConfigureEndpointDefaults(configureOptions =>
            {
                configureOptions.NoDelay = true;
            });
        });
        webBuilder.UseStartup<Startup>();
    });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ConfigureEndpointDefaults(configureOptions =>
            {
                configureOptions.NoDelay = true;
            });
        });
```

::: moniker-end

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a>ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)

Określa konfigurację `Action` do uruchamiania dla każdego punktu końcowego protokołu HTTPS. Wywoływanie `ConfigureHttpsDefaults` wielokrotnie zastępuje starszy `Action`s w ostatnich `Action` określony.

::: moniker range=">= aspnetcore-3.0"

```csharp
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.ConfigureKestrel(serverOptions =>
        {
            serverOptions.ConfigureHttpsDefaults(options =>
            {
                // certificate is an X509Certificate2
                options.ServerCertificate = certificate;
            });
        });
        webBuilder.UseStartup<Startup>();
    });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ConfigureHttpsDefaults(httpsOptions =>
            {
                // certificate is an X509Certificate2
                httpsOptions.ServerCertificate = certificate;
            });
        });
```

::: moniker-end

### <a name="configureiconfiguration"></a>Configure(IConfiguration)

Tworzy moduł ładujący konfiguracji dotyczące konfigurowania Kestrel, która przyjmuje <xref:Microsoft.Extensions.Configuration.IConfiguration> jako dane wejściowe. Konfiguracja musi należeć do sekcji konfiguracji zakresu dla Kestrel.

### <a name="listenoptionsusehttps"></a>ListenOptions.UseHttps

Konfigurowanie usługi Kestrel do używania protokołu HTTPS.

`ListenOptions.UseHttps` rozszerzenia:

* `UseHttps` &ndash; Konfigurowanie usługi Kestrel do używania protokołu HTTPS przy użyciu domyślnego certyfikatu. Zgłasza wyjątek, jeśli nie domyślnego certyfikatu jest skonfigurowany.
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

`ListenOptions.UseHttps` Parametry:

* `filename` jest ścieżka i nazwa pliku certyfikatu, względem katalogu zawierającego pliki zawartości aplikacji.
* `password` to hasło wymagane do uzyskania dostępu do danych certyfikatu X.509.
* `configureOptions` jest `Action` skonfigurować `HttpsConnectionAdapterOptions`. Zwraca `ListenOptions`.
* `storeName` jest magazyn certyfikatów, z którego można załadować certyfikatu.
* `subject` jest to nazwa podmiotu certyfikatu.
* `allowInvalid` Wskazuje, jeżeli nieprawidłowe certyfikaty należy rozważyć, takich jak certyfikaty z podpisem własnym.
* `location` jest to lokalizacja magazynu można załadować certyfikatu z.
* `serverCertificate` jest to certyfikat X.509.

W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS. Jako minimum należy podać certyfikat domyślny.

Obsługiwane konfiguracje opisane w dalszej części:

* Brak konfiguracji
* Zamień domyślny certyfikat z konfiguracji
* Zmiana wartości domyślnych w kodzie

*Brak konfiguracji*

Nasłuchuje kestrel `http://localhost:5000` i `https://localhost:5001` (jeśli jest to domyślny certyfikat jest dostępny).

<a name="configuration"></a>

*Zamień domyślny certyfikat z konfiguracji*

<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie można załadować konfiguracji Kestrel. Schemat konfiguracji ustawień aplikacji domyślnej HTTPS jest dostępna dla Kestrel. Skonfigurować wiele punktów końcowych, w tym adresy URL i certyfikaty, aby korzystać z pliku na dysku lub z magazynu certyfikatów.

W następującym *appsettings.json* przykładu:

* Ustaw **AllowInvalid** do `true` Aby zezwolić na korzystanie z certyfikatów nieprawidłowa (na przykład certyfikatów z podpisem własnym).
* Punkt końcowy HTTPS, który nie określa certyfikat (**HttpsDefaultCert** w poniższym przykładzie) powraca do certyfikatu zdefiniowane w obszarze **certyfikaty** > **domyślne** lub certyfikatu deweloperskiego.

```json
{
"Kestrel": {
  "Endpoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; required>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

Alternatywa dla użycia **ścieżki** i **hasło** dla każdego certyfikatu węzeł jest aby określić certyfikat przy użyciu pól magazynu certyfikatów. Na przykład **certyfikaty** > **domyślne** certyfikat może być określony jako:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Informacje o schemacie:

* Nazwy punktów końcowych jest rozróżniana wielkość liter. Na przykład `HTTPS` i `Https` są prawidłowe.
* `Url` Parametr jest wymagany dla każdego punktu końcowego. Format dla tego parametru jest taka sama jak najwyższego poziomu `Urls` parametru konfiguracji, ale jego ograniczone do pojedynczej wartości.
* Zastąp te punkty końcowe, identyczne ze zdefiniowanymi w najwyższego poziomu `Urls` konfiguracji zamiast dodawać do nich. Punkty końcowe zdefiniowane w kodzie za pomocą `Listen` kumulują się z punktami końcowymi, zdefiniowane w sekcji konfiguracji.
* `Certificate` Sekcja jest opcjonalna. Jeśli `Certificate` sekcji nie jest określona, używane są zdefiniowane w scenariuszach wcześniej wartości domyślne. Jeśli dostępnych żadnych ustawień domyślnych, serwer zgłasza wyjątek i nie można uruchomić.
* `Certificate` Sekcji obsługuje zarówno **ścieżki**&ndash;**hasło** i **podmiotu**&ndash;**Store** certyfikaty.
* Tak długo, jak one nie powodują konfliktów portów można zdefiniować dowolną liczbę punktów końcowych w ten sposób.
* `options.Configure(context.Configuration.GetSection("Kestrel"))` Zwraca `KestrelConfigurationLoader` z `.Endpoint(string name, options => { })` metodę, która może służyć jako uzupełnienie ustawienia skonfigurowanego punktu końcowego:

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  Można również uzyskać dostęp do `KestrelServerOptions.ConfigurationLoader` można zachować iteracja na istniejące modułu ładującego, takiego jak dostarczone przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.

* Sekcja konfiguracji dla każdego punktu końcowego jest dostępna na temat opcji w `Endpoint` metody, aby ustawienia niestandardowe, które mogą być odczytywane.
* Wiele konfiguracji mogą być ładowane przez wywołanie metody `options.Configure(context.Configuration.GetSection("Kestrel"))` ponownie, używając innej sekcji. Ostatnia konfiguracja jest używany, chyba że `Load` zostanie jawnie wywołany w poprzednich wystąpień. Nie wywołuje meta Microsoft.aspnetcore.all `Load` tak, aby jego sekcji konfiguracji domyślnej może zostać zastąpiony.
* `KestrelConfigurationLoader` wstecznych `Listen` rodziny interfejsów API z `KestrelServerOptions` jako `Endpoint` przeciążeń, dzięki czemu kod i konfiguracji punktów końcowych może zostać skonfigurowana w tym samym miejscu. Te przeciążenia, nie używaj nazw i tylko używać domyślnych ustawień konfiguracji.

*Zmiana wartości domyślnych w kodzie*

`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` może służyć do zmiany ustawień domyślnych dla `ListenOptions` i `HttpsConnectionAdapterOptions`, tym przesłanianie domyślnego certyfikatu określone w poprzednich scenariusza. `ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` powinna być wywoływana przed wszystkie punkty końcowe zostały skonfigurowane.

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

*Obsługa kestrel SNI*

[Oznaczaniem nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) mogą być używane do obsługi wielu domen na tym samym adresie IP i port. Dla SNI do funkcji Klient wysyła nazwę hosta dla bezpiecznej sesji na serwerze podczas uzgadniania TLS tak, aby serwer może zapewnić poprawny certyfikat. Klient korzysta z certyfikatu złożonych szyfrowaną komunikację z serwerem podczas nawiązywania bezpiecznej sesji, który następuje po TLS handshake.

Kestrel obsługuje SNI za pośrednictwem `ServerCertificateSelector` wywołania zwrotnego. Wywołanie zwrotne jest wywoływana jeden raz na połączenie, aby umożliwić aplikacji do sprawdzania nazwy hosta i wybierz odpowiedni certyfikat.

Obsługa SNI wymaga:

* Uruchamianie na platformie docelowej `netcoreapp2.1`. Na `netcoreapp2.0` i `net461`, wywołanie zwrotne jest wywoływane, ale `name` jest zawsze `null`. `name` Jest również `null` Jeśli klient nie udostępnia hosta, nazwy parametru w uzgadniania TLS.
* Wszystkie witryny sieci Web uruchamiane na tym samym wystąpieniu Kestrel. Kestrel nie obsługuje udostępniania adresu IP i portów w wielu wystąpieniach bez zwrotnego serwera proxy.

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        });
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        })
        .Build();
```

::: moniker-end

### <a name="bind-to-a-tcp-socket"></a>Powiąż z gniazda TCP

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> Metoda wiąże gniazda TCP i lambda opcje zezwala na konfigurację certyfikatu X.509:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

Przykład konfiguruje HTTPS dla punktu końcowego za pomocą <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>. Skonfiguruj inne ustawienia Kestrel do określonych punktów końcowych za pomocą tego samego interfejsu API.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a>Powiązany z gniazdem systemu Unix

Nasłuchiwanie na gniazdo systemu Unix za pomocą <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> w celu zwiększenia wydajności przy użyciu serwera Nginx, jak pokazano w poniższym przykładzie:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

### <a name="port-0"></a>Port 0

Gdy numer portu `0` określono Kestrel dynamicznie wiąże dostępny port. Poniższy przykład pokazuje, jak określić port, który Kestrel faktycznie powiązany w czasie wykonywania:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

Gdy aplikacja jest uruchamiana, dane wyjściowe z okna konsoli wskazuje port dynamiczny, gdzie można połączyć aplikacji:

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a>Ograniczenia

Konfigurowanie punktów końcowych przy użyciu następujących metod:

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* `--urls` argument wiersza polecenia
* `urls` Klucz konfiguracji hosta
* `ASPNETCORE_URLS` Zmienna środowiskowa

Te metody są przydatne do tworzenia kodu, pracy z serwerami innych niż Kestrel. Należy jednak pamiętać o następujących ograniczeniach:

* Nie można użyć protokołu HTTPS przy użyciu tych metod, jeśli domyślny certyfikat znajduje się w konfiguracji punktu końcowego protokołu HTTPS (na przykład za pomocą `KestrelServerOptions` konfiguracji lub plik konfiguracji, jak pokazano we wcześniejszej części tego tematu).
* Gdy zarówno `Listen` i `UseUrls` podejścia są używane równocześnie, `Listen` zastąpienia punktów końcowych `UseUrls` punktów końcowych.

### <a name="iis-endpoint-configuration"></a>Konfiguracja punktu końcowego usług IIS

Korzystając z usług IIS, powiązania adres URL dla zastąpienia IIS powiązania są ustawiane przez `Listen` lub `UseUrls`. Aby uzyskać więcej informacji, zobacz [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) tematu.

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a>ListenOptions.Protocols

`Protocols` Właściwość ustala protokołów HTTP (`HttpProtocols`) włączone w punkcie końcowym połączenia dla serwera. Przypisanie wartości do `Protocols` właściwość `HttpProtocols` wyliczenia.

| `HttpProtocols` Wartość wyliczenia | Protokół połączenia dozwolone |
| -------------------------- | ----------------------------- |
| `Http1`                    | HTTP/1.1 tylko. Można używać z lub bez protokołu TLS. |
| `Http2`                    | Protokołu HTTP/2 tylko. Głównie używane z protokołem TLS. Można używać bez protokołu TLS tylko wtedy, gdy klient obsługuje [tryb uprzednia](https://tools.ietf.org/html/rfc7540#section-3.4). |
| `Http1AndHttp2`            | HTTP/1.1 i protokołu HTTP/2. Wymaga szyfrowania TLS i [negocjowania protokołu warstwy aplikacji (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) połączenia w celu negocjowania protokołu HTTP/2; w przeciwnym razie Domyślnie połączenie HTTP/1.1. |

Protokół domyślny to HTTP/1.1.

Ograniczenia protokołu TLS dla protokołu HTTP/2:

* Protokół TLS w wersji 1.2 lub nowszej
* Ponowne negocjowanie wyłączone
* Kompresja wyłączona
* Rozmiary minimalne efemeryczne wymiany kluczy:
  * Krzywej eliptycznej Diffie'ego-Hellmana (ECDHE) &lbrack; [RFC4492](https://www.ietf.org/rfc/rfc4492.txt) &rbrack; &ndash; 224 usługi bits, minimalne
  * Pole skończoną Diffie'ego-Hellmana (DHE) &lbrack; `TLS12` &rbrack; &ndash; 2048 bitów minimalne
* Mechanizmy szyfrowania nie na czarnej liście

`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; z krzywej eliptycznej p-256 &lbrack; `FIPS186` &rbrack; jest domyślnie obsługiwany.

Poniższy przykład pozwala protokołu HTTP/1.1 i połączeń protokołu HTTP/2 na porcie 8000. Połączenia są zabezpieczone przez protokół TLS z dostarczony certyfikat:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

Opcjonalnie można utworzyć `IConnectionAdapter` implementacji, aby filtrować uzgodnienia protokołu TLS na podstawie poszczególnych połączeń dla określonych mechanizmów szyfrowania:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
                listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
            });
        });
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that you don't
        // wish to support. Alternatively, define and compare
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null
        // indicates that no cipher algorithm supported by Kestrel matches the
        // requested algorithm(s).
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        return Task.FromResult<IAdaptedConnection>(new AdaptedConnection(context.ConnectionStream));
    }

    private class AdaptedConnection : IAdaptedConnection
    {
        public AdaptedConnection(Stream adaptedStream)
        {
            ConnectionStream = adaptedStream;
        }

        public Stream ConnectionStream { get; }

        public void Dispose()
        {
        }
    }
}
```

*Ustaw protokół z konfiguracji*

<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie można załadować konfiguracji Kestrel.

W następującym *appsettings.json* przykład domyślnym protokołem połączenia (HTTP/1.1 i protokołu HTTP/2) zostanie nawiązane dla wszystkich punktów końcowych Kestrel firmy:

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

W poniższym przykładzie plik konfiguracji nawiązuje połączenie protokół dla określonego punktu końcowego:

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

Protokoły określić w kodzie zastępują wartości ustawione przez konfigurację.

::: moniker-end

## <a name="transport-configuration"></a>Konfiguracja transportu

W wersji platformy ASP.NET Core 2.1 transport domyślny serwera Kestrel nie jest już oparty na bibliotece libuv, ale na zarządzanych gniazdach. Jest to istotną zmianę dla aplikacji ASP.NET Core 2.0, uaktualnienie do wersji 2.1, który <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> i są zależne od jednej z następujących pakietów:

* [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (bezpośrednie odwołanie do pakietu)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

Dla platformy ASP.NET Core 2.1 lub nowszej projektów używających [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) i wymagają użycia Libuv:

* Dodawanie zależności dla [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) pakietu do pliku projektu aplikacji:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                      Version="<LATEST_VERSION>" />
    ```

* Wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

### <a name="url-prefixes"></a>Przedrostki adresów URL

Korzystając z `UseUrls`, `--urls` argument wiersza polecenia `urls` klucz konfiguracji hosta, lub `ASPNETCORE_URLS` zmiennej środowiskowej przedrostki adresów URL może być w dowolnym z następujących formatów.

Prawidłowe są tylko prefiksy URL protokołu HTTP. Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązania adresu URL używającego `UseUrls`.

* Adres IPv4 z numerem portu

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0` jest szczególnym przypadkiem, która jest powiązywana z wszystkich adresów IPv4.

* Numer portu w adresie IPv6

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]` jest to równoważne IPv6 IPv4 `0.0.0.0`.

* Nazwa hosta z numerem portu

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Nazwy, hostów `*`, i `+`, nie są specjalne. Niczego nie został rozpoznany jako prawidłowy adres IP lub `localhost` wiąże wszystkich protokołów IPv4 i IPv6, adresy IP. Aby powiązać różne nazwy hostów do różnych aplikacji platformy ASP.NET Core przy użyciu tego samego portu, należy użyć [HTTP.sys](xref:fundamentals/servers/httpsys) lub serwer zwrotny serwer proxy, na przykład serwer Nginx, Apache lub IIS.

  > [!WARNING]
  > Hosting w konfiguracji zwrotny serwer proxy wymaga [filtrowania host](#host-filtering).

* Host `localhost` nazwa portu numer lub sprzężenia zwrotnego protokołu IP z numerem portu

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Gdy `localhost` określono Kestrel próbuje powiązać interfejsów sprzężenia zwrotnego IPv4 i IPv6. Jeśli żądany port jest używany przez inną usługę na albo interfejsu sprzężenia zwrotnego, Kestrel uruchomienie nie powiedzie się. Jeśli albo interfejsu sprzężenia zwrotnego jest niedostępny z innego powodu (większość często, ponieważ nie jest obsługiwany protokół IPv6), Kestrel dzienniki ostrzeżenie.

## <a name="host-filtering"></a>Filtrowanie hosta

Natomiast Kestrel obsługuje konfigurację oparte na prefiksy takich jak `http://example.com:5000`, Kestrel ignoruje głównie nazwy hosta. Host `localhost` stanowią specjalny przypadek, używane do powiązania z adresami sprzężenia zwrotnego. Hostowanie dowolnego, inne niż jawnych adresów IP, który tworzy powiązanie z wszystkich publicznych adresów IP. `Host` nagłówki nie są weryfikowane.

Obejść ten problem użyj filtrowania hosta w oprogramowaniu pośredniczącym. Oprogramowanie pośredniczące filtrowania w hoście są dostarczane przez [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej). Oprogramowanie pośredniczące jest dodawany przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, która wywołuje metodę <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

Oprogramowanie pośredniczące filtrowania w hosta jest domyślnie wyłączona. Aby włączyć oprogramowanie pośredniczące, zdefiniuj `AllowedHosts` w *appsettings.json*/*appsettings.\< EnvironmentName > .json*. Wartość jest rozdzieloną średnikami listę nazw hostów bez numerów portów:

*appsettings.json*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> [Oprogramowanie pośredniczące nagłówki przekazywane](xref:host-and-deploy/proxy-load-balancer) ma również <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> opcji. Przekazane oprogramowanie pośredniczące nagłówków i hosta filtrowanie oprogramowanie pośredniczące mają podobną funkcjonalność dla różnych scenariuszy. Ustawienie `AllowedHosts` jest odpowiednie, gdy za pomocą przekazywanych oprogramowania pośredniczącego nagłówki `Host` nagłówka nie są zachowywane podczas przekazywania żądań przy użyciu zwrotnego serwera proxy lub moduł równoważenia obciążenia. Ustawienie `AllowedHosts` z hosta filtrowania w oprogramowaniu pośredniczącym przydaje się, gdy Kestrel jest używany jako serwer graniczny publicznego lub `Host` nagłówek bezpośrednio jest przekazywany.
>
> Aby uzyskać więcej informacji na temat przekazywane oprogramowania pośredniczącego nagłówków, zobacz <xref:host-and-deploy/proxy-load-balancer>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [Kod źródłowy kestrel](https://github.com/aspnet/AspNetCore/tree/master/src/Servers/Kestrel)
* [RFC 7230: Składnia i Routing komunikatów (ppkt 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)
