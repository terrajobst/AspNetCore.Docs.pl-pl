---
title: Implementacja serwera sieci Web Kestrel w ASP.NET Core
author: guardrex
description: Dowiedz się więcej na temat Kestrel, międzyplatformowego serwera sieci Web dla ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/24/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 763f65ea26367e56c2ff1392eea51e62fc663ee6
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975526"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>Implementacja serwera sieci Web Kestrel w ASP.NET Core

Autorzy [Dykstra](https://github.com/tdykstra), [Krzysztof Ross](https://github.com/Tratcher)i [Stephen](https://twitter.com/halter73)

Kestrel to Międzyplatformowy [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index). Kestrel to serwer sieci Web, który jest domyślnie uwzględniony w szablonach projektu ASP.NET Core.

Kestrel obsługuje następujące scenariusze:

::: moniker range=">= aspnetcore-2.2"

* HTTPS
* Nieprzezroczyste uaktualnienie używane [](https://github.com/aspnet/websockets) do włączania obsługi obiektów WebSockets
* Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx
* HTTP/2 (z wyjątkiem macOS&dagger;)

&dagger;Protokół HTTP/2 będzie obsługiwany w przypadku macOS w przyszłej wersji.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* HTTPS
* Nieprzezroczyste uaktualnienie używane [](https://github.com/aspnet/websockets) do włączania obsługi obiektów WebSockets
* Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx

::: moniker-end

Kestrel jest obsługiwana na wszystkich platformach i wersjach obsługiwanych przez platformę .NET Core.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a>Obsługa protokołu HTTP/2

[Protokół HTTP/2](https://httpwg.org/specs/rfc7540.html) jest dostępny dla aplikacji ASP.NET Core, jeśli spełnione są następujące wymagania podstawowe:

* System operacyjny&dagger;
  * Windows Server 2016/Windows 10 lub nowszy&Dagger;
  * Linux z OpenSSL 1.0.2 lub nowszym (na przykład Ubuntu 16,04 lub nowszy)
* Platforma docelowa: .NET Core 2,2 lub nowszy
* Połączenie [negocjowania protokołu warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3)
* Protokół TLS 1.2 lub nowszej połączenia

&dagger;Protokół HTTP/2 będzie obsługiwany w przypadku macOS w przyszłej wersji.
&Dagger;Kestrel ma ograniczoną obsługę protokołu HTTP/2 w systemie Windows Server 2012 R2 i Windows 8.1. Obsługa jest ograniczona, ponieważ lista obsługiwanych mechanizmów szyfrowania TLS dostępnych w tych systemach operacyjnych jest ograniczona. Do zabezpieczenia połączeń TLS może być wymagany certyfikat wygenerowany przy użyciu algorytmu podpisu cyfrowego (ECDSA) krzywej eliptycznej.

Jeśli zostanie nawiązane połączenie HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/2`.

Protokół HTTP/2 jest domyślnie wyłączony. Więcej informacji na temat konfiguracji można znaleźć w sekcjach [Kestrel Options](#kestrel-options) and [ListenOptions. Protocols](#listenoptionsprotocols) .

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Kiedy używać Kestrel z zwrotnym serwerem proxy

Kestrel może być używana przez siebie lub z *odwrotnym serwerem proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org)lub [Apache](https://httpd.apache.org/). Odwrotny serwer proxy odbiera żądania HTTP z sieci i przekazuje je do usługi Kestrel.

Kestrel używany jako serwer sieci Web z krawędzią (dostępną z Internetu):

![Kestrel komunikuje się bezpośrednio z Internetem bez serwera proxy zwrotnego](kestrel/_static/kestrel-to-internet2.png)

Kestrel używany w konfiguracji zwrotnego serwera proxy:

![Kestrel komunikuje się pośrednio z Internetem za pomocą odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache](kestrel/_static/kestrel-to-internet.png)

Konfiguracja&mdash;z odwrotnym serwerem&mdash;proxy lub bez niego jest obsługiwaną konfiguracją hostingu dla aplikacji ASP.NET Core 2,1 lub nowszych, które odbierają żądania z Internetu.

Kestrel używany jako serwer graniczny bez serwera proxy odwrotnego nie obsługuje udostępniania tego samego adresu IP i portu między wieloma procesami. Gdy Kestrel jest skonfigurowany do nasłuchiwania na porcie, Kestrel obsługuje cały ruch dla tego portu bez względu na `Host` nagłówki żądań. Zwrotny serwer proxy, który może udostępniać porty, ma możliwość przekazywania żądań do Kestrel na unikatowym adresie IP i porcie.

Nawet jeśli odwrotny serwer proxy nie jest wymagany, może to być dobry wybór przy użyciu odwrotnego serwera proxy.

Zwrotny serwer proxy:

* Może ograniczać uwidoczniony obszar publicznej powierzchni aplikacji, które obsługuje.
* Zapewnienie dodatkowej warstwy konfiguracji i obrony.
* Integracja z istniejącą infrastrukturą może być lepsza.
* Uprość Równoważenie obciążenia i konfigurację komunikacji zabezpieczonej (HTTPS). Tylko serwer zwrotny proxy wymaga certyfikatu X. 509, a serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.

> [!WARNING]
> Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Jak używać Kestrel w aplikacjach ASP.NET Core

Pakiet [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) jest zawarty w [pakiecie Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub nowszym).

Szablony projektów ASP.NET Core domyślnie używają Kestrel. W *program.cs*, kod szablonu wywołuje <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, który wywołuje <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> się w tle.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker range=">= aspnetcore-2.2"

Aby zapewnić dodatkową konfigurację po wywołaniu `CreateDefaultBuilder`, użyj `ConfigureKestrel`:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

Jeśli aplikacja nie wywoła połączenia `CreateDefaultBuilder` w celu skonfigurowania hosta, wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **przed** wywołaniem `ConfigureKestrel`:

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

Aby zapewnić dodatkową konfigurację po wywołaniu `CreateDefaultBuilder`, wywołaj: <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>

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

## <a name="kestrel-options"></a>Opcje Kestrel

Serwer sieci Web Kestrel ma opcje konfiguracji ograniczeń, które są szczególnie przydatne w przypadku wdrożeń mających dostęp do Internetu.

Ustaw ograniczenia <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> właściwości <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> klasy. Właściwość przechowuje wystąpienie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>klasy. `Limits`

W poniższych przykładach użyto <xref:Microsoft.AspNetCore.Server.Kestrel.Core> przestrzeni nazw:

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a>Limit czasu utrzymywania aktywności

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

Pobiera lub ustawia [limit czasu utrzymywania aktywności](https://tools.ietf.org/html/rfc7230#section-6.5). Wartość domyślna to 2 minuty.

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

### <a name="maximum-client-connections"></a>Maksymalna liczba połączeń klienta

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

Maksymalna liczba współbieżnych otwartych połączeń TCP można ustawić dla całej aplikacji przy użyciu następującego kodu:

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

Istnieje oddzielny limit połączeń, które zostały uaktualnione z protokołu HTTP lub HTTPS do innego protokołu (na przykład w żądaniu usługi WebSockets). Po uaktualnieniu połączenia nie jest ono wliczane do `MaxConcurrentConnections` limitu.

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

Maksymalna liczba połączeń jest domyślnie nieograniczona (null).

### <a name="maximum-request-body-size"></a>Maksymalny rozmiar treści żądania

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

Domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.

Zalecanym podejściem do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> atrybutu dla metody akcji:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Oto przykład, który pokazuje, jak skonfigurować ograniczenie dla aplikacji w każdym żądaniu:

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

To ustawienie można zastąpić określonym żądaniem w oprogramowaniu pośredniczącym:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

Wyjątek jest zgłaszany w przypadku próby skonfigurowania limitu dla żądania po rozpoczęciu pracy przez aplikację w celu odczytania żądania. Istnieje właściwość, która wskazuje, `MaxRequestBodySize` czy właściwość jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit. `IsReadOnly`

Gdy aplikacja jest uruchamiana [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model) [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru treści żądania Kestrel jest wyłączony, ponieważ program IIS już ustawia limit.

### <a name="minimum-request-body-data-rate"></a>Minimalna stawka danych treści żądania

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

Kestrel sprawdza co sekundę, jeśli dane są odbierane z określoną szybkością w bajtach na sekundę. Jeśli szybkość spadnie poniżej wartości minimalnej, upłynął limit czasu połączenia. Okres prolongaty to czas, przez który Kestrel nadaje klientowi zwiększenie jego szybkości wysyłania do minimum; Ta częstotliwość nie jest sprawdzana w tym czasie. Okres prolongaty pozwala uniknąć porzucania połączeń, które początkowo wysyłają dane z niską szybkością.

Domyślna stawka minimalna to 240 bajtów na sekundę z 5-sekundowym okresem prolongaty.

Minimalna stawka dotyczy także odpowiedzi. Kod określający limit żądań i limit odpowiedzi jest taki sam, z wyjątkiem `RequestBody` `Response` właściwości i nazwy interfejsów.

Oto przykład pokazujący sposób konfigurowania minimalnych stawek danych w *program.cs*:

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

Limity szybkości minimalnej dla żądania można zastąpić w oprogramowaniu pośredniczącym:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

::: moniker range=">= aspnetcore-3.0"

Odwołania <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> w poprzednim przykładzie nie występują w `HttpContext.Features` żądaniach HTTP/2, ponieważ Modyfikowanie limitów szybkości dla poszczególnych żądań jest ogólnie nieobsługiwane w przypadku protokołu HTTP/2 z powodu obsługi żądania multipleksowania żądań. `HttpContext.Features` `null` Jednak nadal występuje dla żądań HTTP/2, ponieważ limit liczby odczytów nadal można wyłączyć całkowicie dla każdego żądania przez ustawienie `IHttpMinRequestBodyDataRateFeature.MinDataRate` nawet dla żądania HTTP/2. <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> Próba odczytania `IHttpMinRequestBodyDataRateFeature.MinDataRate` lub próby ustawienia jej na wartość inną niż `null` spowoduje `NotSupportedException` zgłoszenie żądania HTTP/2.

Limity szybkości dla całego serwera skonfigurowane za `KestrelServerOptions.Limits` pośrednictwem nadal mają zastosowanie do połączeń HTTP/1. x i http/2.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Funkcja rate, do której odwołuje się Poprzednia `HttpContext.Features` próba, nie jest dostępna w przypadku żądań HTTP/2, ponieważ Modyfikowanie limitów szybkości dla poszczególnych żądań nie jest obsługiwane w przypadku protokołu HTTP/2 ze względu na obsługę multipleksera żądania. Limity szybkości dla całego serwera skonfigurowane za `KestrelServerOptions.Limits` pośrednictwem nadal mają zastosowanie do połączeń HTTP/1. x i http/2.

::: moniker-end

### <a name="request-headers-timeout"></a>Limit czasu nagłówków żądań

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

Pobiera lub ustawia maksymalny czas, przez jaki serwer spędza nagłówki żądania. Wartość domyślna to 30 sekund.

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

### <a name="maximum-streams-per-connection"></a>Maksymalna liczba strumieni na połączenie

`Http2.MaxStreamsPerConnection`ogranicza liczbę współbieżnych strumieni żądań na połączenie HTTP/2. Odrzucone strumienie są nadmiarowe.

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

Dekoder HPACK dekompresuje nagłówki HTTP dla połączeń HTTP/2. `Http2.HeaderTableSize`ogranicza rozmiar tabeli kompresji nagłówka używanej przez dekoder HPACK. Wartość jest podawana w oktetach i musi być większa od zera (0).

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

`Http2.MaxFrameSize`wskazuje maksymalny rozmiar ładunku ramki połączenia HTTP/2 do odebrania. Wartość jest podawana w oktetach i musi należeć do przedziału od 2 ^ 14 (16 384) do 2 ^ 24-1 (16 777 215).

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

### <a name="maximum-request-header-size"></a>Maksymalny rozmiar nagłówka żądania

`Http2.MaxRequestHeaderFieldSize`wskazuje maksymalny dozwolony rozmiar w oktetach wartości nagłówka żądania. Ten limit dotyczy zarówno nazwy, jak i wartości razem w skompresowanych i nieskompresowanych reprezentacji. Wartość musi być większa od zera (0).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

Wartość domyślna to 8 192.

### <a name="initial-connection-window-size"></a>Początkowy rozmiar okna połączenia

`Http2.InitialConnectionWindowSize`wskazuje maksymalne dane dotyczące treści żądania w bajtach, które są agregowane w buforach serwera w ramach wszystkich żądań (strumieni) na połączenie. Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`. Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

Wartość domyślna to 128 KB (131 072).

### <a name="initial-stream-window-size"></a>Rozmiar początkowego okna strumienia

`Http2.InitialStreamWindowSize`wskazuje maksymalne dane dotyczące treści żądania w bajtach bufory serwera w ramach żądania (Stream). Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`. Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

Wartość domyślna to 96 KB (98 304).

::: moniker-end

### <a name="synchronous-io"></a>Synchroniczne we/wy

::: moniker range=">= aspnetcore-3.0"

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>Określa, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi. Wartość domyślna to `false`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>Określa, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi. Wartość domyślna to `true`.

::: moniker-end

> [!WARNING]
> Duża liczba blokowania synchronicznych operacji we/wy może prowadzić do zablokowania puli wątków, co sprawia, że aplikacja nie odpowiada. Włączaj `AllowSynchronousIO` tylko w przypadku korzystania z biblioteki, która nie obsługuje asynchronicznej operacji we/wy.

::: moniker range=">= aspnetcore-2.2"

Poniższy przykład włącza synchroniczną operację we/wy:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Poniższy przykład wyłącza synchroniczną operację we/wy:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.AllowSynchronousIO = false;
        });
```

::: moniker-end

Aby uzyskać informacje o innych opcjach i ograniczeniach Kestrel, zobacz:

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a>Konfiguracja punktu końcowego

Domyślnie ASP.NET Core wiąże się z:

* `http://localhost:5000`
* `https://localhost:5001`(gdy obecny jest lokalny certyfikat programistyczny)

Określ adresy URL przy użyciu:

* `ASPNETCORE_URLS` zmiennej środowiskowej.
* `--urls`argument wiersza polecenia.
* `urls`klucz konfiguracji hosta.
* `UseUrls`Metoda rozszerzenia.

Wartość podana przy użyciu tych metod może być jednym lub większą liczbą punktów końcowych HTTP i HTTPS (HTTPS, jeśli jest dostępny domyślny certyfikat). Skonfiguruj wartość jako listę rozdzieloną średnikami (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).

Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwera](xref:fundamentals/host/web-host#server-urls) i Zastąp [konfigurację](xref:fundamentals/host/web-host#override-configuration).

Certyfikat programistyczny jest tworzony:

* Po zainstalowaniu [zestaw .NET Core SDK](/dotnet/core/sdk) .
* Do utworzenia certyfikatu służy [Narzędzie dev-certs](xref:aspnetcore-2.1#https) .

Niektóre przeglądarki wymagają przyznania jawnego uprawnienia do przeglądarki w celu zaufać lokalnemu certyfikatowi programistycznemu.

ASP.NET Core 2,1 i nowsze szablony projektów konfigurują aplikacje do uruchamiania domyślnie przy użyciu protokołu HTTPS i obejmują [przekierowania https i obsługę HSTS](xref:security/enforcing-ssl).

Wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> metody w<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> celu skonfigurowania prefiksów i portów adresów URL dla Kestrel.

`UseUrls`, argument wiersza `urls` `ASPNETCORE_URLS` polecenia, klucz konfiguracji hosta i zmienna środowiskowa również działają, ale mają ograniczenia wymienione w dalszej części tej sekcji (certyfikat domyślny musi być dostępny dla punktu końcowego HTTPS `--urls` Konfiguracja).

ASP.NET Core 2,1 lub nowsza `KestrelServerOptions` :

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a>ConfigureEndpointDefaults (Akcja&lt;ListenOptions&gt;)

Określa konfigurację `Action` do uruchomienia dla każdego określonego punktu końcowego. Wywołanie `ConfigureEndpointDefaults` wielokrotne zastępuje przed `Action`ostatnimi `Action` parametrami.

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

Określa konfigurację `Action` do uruchomienia dla każdego punktu końcowego HTTPS. Wywołanie `ConfigureHttpsDefaults` wielokrotne zastępuje przed `Action`ostatnimi `Action` parametrami.

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

### <a name="configureiconfiguration"></a>Konfiguruj (IConfiguration)

Tworzy moduł ładujący konfigurację na potrzeby konfigurowania Kestrel, które <xref:Microsoft.Extensions.Configuration.IConfiguration> pobiera jako dane wejściowe. Konfiguracja musi być objęta zakresem sekcji konfiguracji dla Kestrel.

### <a name="listenoptionsusehttps"></a>ListenOptions.UseHttps

Skonfiguruj Kestrel do korzystania z protokołu HTTPS.

`ListenOptions.UseHttps`rozszerzenia

* `UseHttps`&ndash; Skonfiguruj Kestrel do używania protokołu HTTPS z domyślnym certyfikatem. Zgłasza wyjątek, jeśli nie został skonfigurowany żaden certyfikat domyślny.
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

`ListenOptions.UseHttps`wejściowe

* `filename`jest ścieżką i nazwą pliku certyfikatu względem katalogu zawierającego pliki zawartości aplikacji.
* `password`to hasło wymagane do uzyskania dostępu do danych certyfikatu X. 509.
* `configureOptions``Action` jest do skonfigurowania `HttpsConnectionAdapterOptions`. Zwraca wartość `ListenOptions`.
* `storeName`to magazyn certyfikatów, z którego ma zostać załadowany certyfikat.
* `subject`to nazwa podmiotu certyfikatu.
* `allowInvalid`wskazuje, czy należy wziąć pod uwagę nieprawidłowe certyfikaty, takie jak certyfikaty z podpisem własnym.
* `location`jest lokalizacją magazynu, z której ma zostać załadowany certyfikat.
* `serverCertificate`jest certyfikatem X. 509.

W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS. Należy podać co najmniej certyfikat domyślny.

Obsługiwane konfiguracje opisane dalej:

* Brak konfiguracji
* Zastąp domyślny certyfikat z konfiguracji
* Zmień wartości domyślne w kodzie

*Brak konfiguracji*

Kestrel nasłuchuje `http://localhost:5000` w `https://localhost:5001` systemie i (Jeśli domyślny certyfikat jest dostępny).

<a name="configuration"></a>

*Zastąp domyślny certyfikat z konfiguracji*

<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel. Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępny dla Kestrel. Konfigurowanie wielu punktów końcowych, w tym adresów URL i certyfikatów do użycia, z pliku znajdującego się na dysku lub z magazynu certyfikatów.

W poniższym przykładzie pliku *appSettings. JSON* :

* Ustaw **AllowInvalid** na `true` , aby zezwolić na korzystanie z nieprawidłowych certyfikatów (na przykład certyfikatów z podpisem własnym).
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

Alternatywą dla korzystania z **ścieżki** i **hasła** dla dowolnego węzła certyfikatu jest określenie certyfikatu przy użyciu pól magazynu certyfikatów. Certyfikat**domyślny** można na > przykład określić jako:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Uwagi dotyczące schematu:

* W nazwach punktów końcowych nie jest rozróżniana wielkość liter. Na przykład `HTTPS` i `Https` są prawidłowe.
* `Url` Parametr jest wymagany dla każdego punktu końcowego. Format tego parametru jest taki sam jak parametr konfiguracji najwyższego poziomu `Urls` , z tą różnicą, że jest ograniczony do pojedynczej wartości.
* Te punkty końcowe zastępują te zdefiniowane w konfiguracji najwyższego poziomu `Urls` zamiast dodawać je do nich. Punkty końcowe zdefiniowane w kodzie `Listen` za pośrednictwem łączą się z punktami końcowymi zdefiniowanymi w sekcji konfiguracji.
* `Certificate` Sekcja jest opcjonalna. `Certificate` Jeśli sekcja nie jest określona, używane są wartości domyślne zdefiniowane we wcześniejszych scenariuszach. Jeśli żadne wartości domyślne nie są dostępne, serwer zgłasza wyjątek i nie może się uruchomić.
* &ndash;&ndash; Sekcja obsługuje zarówno hasło ścieżki, jak i certyfikaty magazynu podmiotu. `Certificate`
* W ten sposób można zdefiniować dowolną liczbę punktów końcowych, dopóki nie spowoduje to konfliktów portów.
* `options.Configure(context.Configuration.GetSection("Kestrel"))``KestrelConfigurationLoader` zwracametodę,któramożesłużyćdouzupełnianiaskonfigurowanych`.Endpoint(string name, options => { })` ustawień punktu końcowego:

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  Możesz również uzyskać bezpośredni dostęp `KestrelServerOptions.ConfigurationLoader` do zachowania iteracji na istniejącym module ładującym, takim jak udostępniony przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.

* Sekcja konfiguracji dla każdego punktu końcowego jest dostępna w opcjach w `Endpoint` metodzie, aby można było odczytać ustawienia niestandardowe.
* Można załadować wiele konfiguracji, wywołując `options.Configure(context.Configuration.GetSection("Kestrel"))` ponownie z inną sekcją. Używana jest tylko Ostatnia konfiguracja, chyba że `Load` jest jawnie wywoływana w poprzednich wystąpieniach. Pakiet nie wywołuje metody `Load` , aby można było zastąpić sekcję konfiguracji domyślnej.
* `KestrelConfigurationLoader`odzwierciedla rodzinę interfejsów API z `KestrelServerOptions` programu `Endpoint` jako przeciążenia, dlatego punkty końcowe kodu i konfiguracji można skonfigurować w tym samym miejscu. `Listen` Te przeciążenia nie używają nazw i używają tylko ustawień domyślnych z konfiguracji.

*Zmień wartości domyślne w kodzie*

`ConfigureEndpointDefaults`i `ConfigureHttpsDefaults` może służyć do zmiany ustawień domyślnych dla `ListenOptions` i `HttpsConnectionAdapterOptions`, w tym przesłanianie domyślnego certyfikatu określonego w poprzednim scenariuszu. `ConfigureEndpointDefaults`i `ConfigureHttpsDefaults` powinny być wywoływane przed skonfigurowaniem punktów końcowych.

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

*Obsługa usługi Kestrel dla SNI*

[Oznaczanie nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do hostowania wielu domen na tym samym adresie IP i porcie. Aby SNI działał, klient wysyła nazwę hosta dla bezpiecznej sesji do serwera podczas uzgadniania TLS, aby serwer mógł zapewnić prawidłowy certyfikat. Klient korzysta z dostarczonego certyfikatu w celu zaszyfrowania komunikacji z serwerem podczas bezpiecznej sesji, która następuje po uzgadnianiu protokołu TLS.

Kestrel obsługuje SNI za pośrednictwem `ServerCertificateSelector` wywołania zwrotnego. Wywołanie zwrotne jest wywoływane jednokrotnie dla każdego połączenia, aby umożliwić aplikacji inspekcję nazwy hosta i wybieranie odpowiedniego certyfikatu.

Obsługa SNI wymaga:

* Uruchamianie w środowisku `netcoreapp2.1`docelowym. W `netcoreapp2.0` systemach `net461`i wywołaniezwrotne`null`jest wywoływane, ale jestzawsze.`name` Jest `name` również`null` , jeśli klient nie poda parametru nazwy hosta w uzgadnianiu protokołu TLS.
* Wszystkie witryny sieci Web działają na tym samym wystąpieniu Kestrel. Kestrel nie obsługuje udostępniania adresu IP i portu w wielu wystąpieniach bez zwrotnego serwera proxy.

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

### <a name="bind-to-a-tcp-socket"></a>Powiąż z gniazdem TCP

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> Metoda wiąże się z gniazdem TCP, a opcja lambda zezwala na konfigurację certyfikatu X. 509:

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

Przykład konfiguruje HTTPS dla punktu końcowego za <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>pomocą. Użyj tego samego interfejsu API, aby skonfigurować inne ustawienia Kestrel dla określonych punktów końcowych.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a>Powiąż z gniazdem systemu UNIX

Nasłuchiwanie w gnieździe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> systemu UNIX za pomocą programu w celu zwiększenia wydajności za pomocą usługi Nginx, jak pokazano w tym przykładzie:

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

Gdy numer `0` portu jest określony, Kestrel dynamicznie wiąże się z dostępnym portem. W poniższym przykładzie pokazano, jak określić, który port Kestrel faktycznie powiązany w czasie wykonywania:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

Po uruchomieniu aplikacji dane wyjściowe okna konsoli wskazują port dynamiczny, w którym można uzyskać dostęp do aplikacji:

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a>Ograniczenia

Skonfiguruj punkty końcowe przy użyciu następujących metod:

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* `--urls`argument wiersza polecenia
* `urls`klucz konfiguracji hosta
* `ASPNETCORE_URLS`Zmienna środowiskowa

Te metody są przydatne do tworzenia kodu w pracy z serwerami innymi niż Kestrel. Należy jednak pamiętać o następujących ograniczeniach:

* Nie można użyć protokołu HTTPS z tymi metodami, chyba że w konfiguracji punktu końcowego HTTPS jest podany certyfikat domyślny (na `KestrelServerOptions` przykład przy użyciu konfiguracji lub pliku konfiguracji, jak pokazano wcześniej w tym temacie).
* Gdy oba `Listen` podejścia i `UseUrls` `UseUrls` są używane jednocześnie, punktykońcowezastępująpunktykońcowe.`Listen`

### <a name="iis-endpoint-configuration"></a>Konfiguracja punktu końcowego usług IIS

W przypadku korzystania z usług IIS powiązania URL dla powiązań przesłonięć usług IIS są ustawiane `UseUrls`przez `Listen` lub. Aby uzyskać więcej informacji, zobacz temat [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) .

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a>ListenOptions. Protocols

Właściwość ustanawia protokoły HTTP (`HttpProtocols`) włączone w punkcie końcowym połączenia lub dla serwera. `Protocols` Przypisz wartość do `Protocols` właściwości `HttpProtocols` z wyliczenia.

| `HttpProtocols`wartość wyliczenia | Dozwolony protokół połączenia |
| -------------------------- | ----------------------------- |
| `Http1`                    | Tylko HTTP/1.1. Może być używany z protokołem TLS lub bez niego. |
| `Http2`                    | Tylko HTTP/2. Używane głównie z protokołem TLS. Mogą być używane bez protokołu TLS tylko wtedy, gdy klient obsługuje [poprzedni tryb wiedzy](https://tools.ietf.org/html/rfc7540#section-3.4). |
| `Http1AndHttp2`            | HTTP/1.1 i HTTP/2. Do negocjowania protokołu HTTP/2 wymagane jest połączenie protokołów TLS i [Layer-warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3) . w przeciwnym razie wartość domyślna połączenia to HTTP/1.1. |

Domyślnym protokołem jest HTTP/1.1.

Ograniczenia protokołu TLS dla protokołu HTTP/2:

* TLS w wersji 1,2 lub nowszej
* Ponowne negocjowanie wyłączone
* Kompresja wyłączona
* Minimalne rozmiary tymczasowych kluczy wymiany:
  * Krzywa eliptyczna Diffie-Hellmana (ECDHE &lbrack;) [RFC4492](https://www.ietf.org/rfc/rfc4492.txt) &rbrack; &ndash; 224 (minimum)
  * Ograniczone pole Diffie-Hellmana (DHE) &lbrack; `TLS12` &rbrack; &ndash; 2048 bity minimum
* Mechanizm szyfrowania nie został zabroniony

`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`&lbrack;zkrzywą eliptycznaP&lbrack; -256 jest domyślnieobsługiwana.`FIPS186` `TLS-ECDHE` &rbrack; &rbrack;

Poniższy przykład umożliwia nawiązywanie połączeń HTTP/1.1 i HTTP/2 na porcie 8000. Połączenia są zabezpieczone przez protokół TLS przy użyciu podanego certyfikatu:

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

Opcjonalnie można utworzyć `IConnectionAdapter` implementację do filtrowania uzgadniania protokołu TLS dla poszczególnych połączeń dla określonych szyfrów:

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

*Ustawianie protokołu z konfiguracji*

<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.

W poniższym przykładzie pliku *appSettings. JSON* jest ustanawiany domyślny protokół połączeń (http/1.1 i http/2) dla wszystkich punktów końcowych Kestrel:

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

Poniższy przykład pliku konfiguracji ustanawia protokół połączenia dla określonego punktu końcowego:

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

Protokoły określone w wartościach zastąpienia kodu ustawione przez konfigurację.

::: moniker-end

## <a name="transport-configuration"></a>Konfiguracja transportu

W wersji platformy ASP.NET Core 2.1 transport domyślny serwera Kestrel nie jest już oparty na bibliotece libuv, ale na zarządzanych gniazdach. Jest to istotna zmiana dla aplikacji ASP.NET Core 2,0 uaktualniana do 2,1, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> które wywołują i zależą od jednego z następujących pakietów:

* [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (bezpośrednie odwołanie do pakietu)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

W przypadku ASP.NET Core 2,1 lub nowszych projektów, które używają [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) , i wymagają użycia Libuv:

* Dodaj zależność dla pakietu [Microsoft. AspNetCore. Server. Kestrel. transport. Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) do pliku projektu aplikacji:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                      Version="<LATEST_VERSION>" />
    ```

* Wywołanie <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:

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

### <a name="url-prefixes"></a>Prefiksy adresów URL

W przypadku `UseUrls`użycia `--urls` , argumentu wiersza polecenia, `urls` klucza konfiguracji hosta lub `ASPNETCORE_URLS` zmiennej środowiskowej prefiksy adresów URL mogą znajdować się w jednym z następujących formatów.

Tylko prefiksy adresów URL HTTP są prawidłowe. Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązań adresów `UseUrls`URL przy użyciu programu.

* Adres IPv4 z numerem portu

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0`jest szczególnym przypadkiem, który wiąże się ze wszystkimi adresami IPv4.

* Adres IPv6 z numerem portu

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]`jest odpowiednikiem IPv6 dla protokołu `0.0.0.0`IPv4.

* Nazwa hosta z numerem portu

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Nazwy `*`hostów, i `+`, nie są specjalne. Wszystkie nie są rozpoznawane jako prawidłowy adres IP `localhost` lub są powiązane ze wszystkimi IP IPv4 i IPv6. Aby powiązać różne nazwy hostów z różnymi ASP.NET Core aplikacjami na tym samym porcie, użyj [protokołu HTTP. sys](xref:fundamentals/servers/httpsys) lub odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache.

  > [!WARNING]
  > Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).

* Nazwa `localhost` hosta z numerem portu lub adresem IP sprzężenia zwrotnego z numerem portu

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Gdy `localhost` jest określony, Kestrel próbuje powiązać z interfejsami sprzężenia zwrotnego IPv4 i IPv6. Jeśli żądany port jest używany przez inną usługę w dowolnym interfejsie sprzężenia zwrotnego, uruchomienie Kestrel nie powiedzie się. Jeśli którykolwiek z innych przyczyn interfejsu sprzężenia zwrotnego jest niedostępny (najczęściej jest to spowodowane tym, że protokół IPv6 nie jest obsługiwany), Kestrel rejestruje ostrzeżenie.

## <a name="host-filtering"></a>Filtrowanie hostów

Program Kestrel obsługuje konfigurację na podstawie prefiksów, takich `http://example.com:5000`jak, Kestrel w znacznym stopniu ignoruje nazwę hosta. Host `localhost` jest specjalnym przypadkiem używanym do tworzenia powiązań z adresami sprzężenia zwrotnego. Każdy host poza jawnym adresem IP tworzy powiązanie ze wszystkimi publicznymi adresami IP. `Host`nagłówki nie są sprawdzane.

Aby obejść ten sposób, użyj oprogramowania pośredniczącego filtrowania hosta. Oprogramowanie pośredniczące do filtrowania hosta jest dostarczane przez pakiet [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , który jest zawarty w pakiecie [Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub nowszym). Oprogramowanie pośredniczące jest dodawane przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>program, który <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>wywołuje następujące wywołania:

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

Programowe filtrowanie hosta jest domyślnie wyłączone. Aby włączyć oprogramowanie pośredniczące, zdefiniuj klucz `AllowedHosts` w pliku *appSettings. JSON*/ *.\< EnvironmentName >. JSON*. Wartość to rozdzielana średnikami lista nazw hostów bez numerów portów:

*appsettings.json*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> [Przekierowane nagłówki oprogramowania](xref:host-and-deploy/proxy-load-balancer) również mają <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> opcję. Przekazane nagłówki — oprogramowanie pośredniczące i filtrowanie hostów oprogramowanie pośredniczące ma podobną funkcjonalność dla różnych scenariuszy. Ustawienie `AllowedHosts` z przekierowanymi nagłówkami — oprogramowanie pośredniczące jest odpowiednie, `Host` gdy nagłówek nie jest zachowywany podczas przekazywania żądań z odwrotnym serwerem proxy lub modułem równoważenia obciążenia. Ustawienie `AllowedHosts` przy użyciu oprogramowania pośredniczącego filtrowania hosta jest odpowiednie, gdy Kestrel jest używany jako publiczny serwer graniczny lub `Host` gdy nagłówek jest bezpośrednio przekazywany.
>
> Aby uzyskać więcej informacji na temat przekierowanych nagłówków <xref:host-and-deploy/proxy-load-balancer>, należy zapoznać się z tematem.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [Kod źródłowy Kestrel](https://github.com/aspnet/AspNetCore/tree/master/src/Servers/Kestrel)
* [RFC 7230: Składnia i routing wiadomości (sekcja 5,4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)
