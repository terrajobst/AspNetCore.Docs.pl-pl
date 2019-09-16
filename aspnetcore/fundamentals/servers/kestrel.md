---
title: Implementacja serwera sieci Web Kestrel w ASP.NET Core
author: guardrex
description: Dowiedz się więcej na temat Kestrel, międzyplatformowego serwera sieci Web dla ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: b1c28f084e67d9cce74258433aa0c884878c2520
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011165"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="61976-103">Implementacja serwera sieci Web Kestrel w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61976-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="61976-104">Autorzy [Dykstra](https://github.com/tdykstra), [Krzysztof Ross](https://github.com/Tratcher), [Stephen](https://twitter.com/halter73), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="61976-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="61976-105">Kestrel to Międzyplatformowy [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="61976-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="61976-106">Kestrel to serwer sieci Web, który jest domyślnie uwzględniony w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61976-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="61976-107">Kestrel obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="61976-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="61976-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="61976-108">HTTPS</span></span>
* <span data-ttu-id="61976-109">Nieprzezroczyste uaktualnienie używane do włączania obsługi obiektów [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="61976-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="61976-110">Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx</span><span class="sxs-lookup"><span data-stu-id="61976-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="61976-111">HTTP/2 (z wyjątkiem macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="61976-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="61976-112">&dagger;Protokół HTTP/2 będzie obsługiwany w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="61976-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="61976-113">Kestrel jest obsługiwana na wszystkich platformach i wersjach obsługiwanych przez platformę .NET Core.</span><span class="sxs-lookup"><span data-stu-id="61976-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="61976-114">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="61976-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="61976-115">Obsługa protokołu HTTP/2</span><span class="sxs-lookup"><span data-stu-id="61976-115">HTTP/2 support</span></span>

<span data-ttu-id="61976-116">[Protokół HTTP/2](https://httpwg.org/specs/rfc7540.html) jest dostępny dla aplikacji ASP.NET Core, jeśli spełnione są następujące wymagania podstawowe:</span><span class="sxs-lookup"><span data-stu-id="61976-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="61976-117">System operacyjny&dagger;</span><span class="sxs-lookup"><span data-stu-id="61976-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="61976-118">Windows Server 2016/Windows 10 lub nowszy&Dagger;</span><span class="sxs-lookup"><span data-stu-id="61976-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="61976-119">Linux z OpenSSL 1.0.2 lub nowszym (na przykład Ubuntu 16,04 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="61976-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="61976-120">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="61976-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="61976-121">Połączenie [negocjowania protokołu warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="61976-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="61976-122">Protokół TLS 1.2 lub nowszej połączenia</span><span class="sxs-lookup"><span data-stu-id="61976-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="61976-123">&dagger;Protokół HTTP/2 będzie obsługiwany w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="61976-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="61976-124">&Dagger;Kestrel ma ograniczoną obsługę protokołu HTTP/2 w systemie Windows Server 2012 R2 i Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="61976-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="61976-125">Obsługa jest ograniczona, ponieważ lista obsługiwanych mechanizmów szyfrowania TLS dostępnych w tych systemach operacyjnych jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="61976-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="61976-126">Do zabezpieczenia połączeń TLS może być wymagany certyfikat wygenerowany przy użyciu algorytmu podpisu cyfrowego (ECDSA) krzywej eliptycznej.</span><span class="sxs-lookup"><span data-stu-id="61976-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="61976-127">Jeśli zostanie nawiązane połączenie HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="61976-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="61976-128">Protokół HTTP/2 jest domyślnie wyłączony.</span><span class="sxs-lookup"><span data-stu-id="61976-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="61976-129">Więcej informacji na temat konfiguracji można znaleźć w sekcjach [Kestrel Options](#kestrel-options) and [ListenOptions. Protocols](#listenoptionsprotocols) .</span><span class="sxs-lookup"><span data-stu-id="61976-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="61976-130">Kiedy używać Kestrel z zwrotnym serwerem proxy</span><span class="sxs-lookup"><span data-stu-id="61976-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="61976-131">Kestrel może być używana przez siebie lub z *odwrotnym serwerem proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org)lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="61976-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="61976-132">Odwrotny serwer proxy odbiera żądania HTTP z sieci i przekazuje je do usługi Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="61976-133">Kestrel używany jako serwer sieci Web z krawędzią (dostępną z Internetu):</span><span class="sxs-lookup"><span data-stu-id="61976-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem bez serwera proxy zwrotnego](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="61976-135">Kestrel używany w konfiguracji zwrotnego serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="61976-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel komunikuje się pośrednio z Internetem za pomocą odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="61976-137">Konfiguracja, z serwerem proxy odwrotnych lub bez niego, jest obsługiwaną konfiguracją hostingu.</span><span class="sxs-lookup"><span data-stu-id="61976-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="61976-138">Kestrel używany jako serwer graniczny bez serwera proxy odwrotnego nie obsługuje udostępniania tego samego adresu IP i portu między wieloma procesami.</span><span class="sxs-lookup"><span data-stu-id="61976-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="61976-139">Gdy Kestrel jest skonfigurowany do nasłuchiwania na porcie, Kestrel obsługuje cały ruch dla tego portu bez względu na `Host` nagłówki żądań.</span><span class="sxs-lookup"><span data-stu-id="61976-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="61976-140">Zwrotny serwer proxy, który może udostępniać porty, ma możliwość przekazywania żądań do Kestrel na unikatowym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="61976-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="61976-141">Nawet jeśli odwrotny serwer proxy nie jest wymagany, może to być dobry wybór przy użyciu odwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="61976-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="61976-142">Zwrotny serwer proxy:</span><span class="sxs-lookup"><span data-stu-id="61976-142">A reverse proxy:</span></span>

* <span data-ttu-id="61976-143">Może ograniczać uwidoczniony obszar publicznej powierzchni aplikacji, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="61976-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="61976-144">Zapewnienie dodatkowej warstwy konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="61976-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="61976-145">Integracja z istniejącą infrastrukturą może być lepsza.</span><span class="sxs-lookup"><span data-stu-id="61976-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="61976-146">Uprość Równoważenie obciążenia i konfigurację komunikacji zabezpieczonej (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="61976-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="61976-147">Tylko serwer zwrotny proxy wymaga certyfikatu X. 509, a serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="61976-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="61976-148">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="61976-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="61976-149">Jak używać Kestrel w aplikacjach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61976-149">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="61976-150">Szablony projektów ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="61976-151">W *program.cs*, kod szablonu wywołuje <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, który wywołuje <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> się w tle.</span><span class="sxs-lookup"><span data-stu-id="61976-151">In *Program.cs*, the template code calls <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="61976-152">Aby zapewnić dodatkową konfigurację po wywołaniu `CreateDefaultBuilder` i `ConfigureWebHostDefaults`, użyj `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="61976-152">To provide additional configuration after calling `CreateDefaultBuilder` and `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseStartup<Startup>();
        });
```

<span data-ttu-id="61976-153">Jeśli aplikacja nie wywoła połączenia `CreateDefaultBuilder` w celu skonfigurowania hosta, wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **przed** wywołaniem `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="61976-153">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new HostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseIISIntegration()
            .UseStartup<Startup>();
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="61976-154">Opcje Kestrel</span><span class="sxs-lookup"><span data-stu-id="61976-154">Kestrel options</span></span>

<span data-ttu-id="61976-155">Serwer sieci Web Kestrel ma opcje konfiguracji ograniczeń, które są szczególnie przydatne w przypadku wdrożeń mających dostęp do Internetu.</span><span class="sxs-lookup"><span data-stu-id="61976-155">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="61976-156">Ustaw ograniczenia <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> właściwości <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> klasy.</span><span class="sxs-lookup"><span data-stu-id="61976-156">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="61976-157">Właściwość przechowuje wystąpienie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>klasy. `Limits`</span><span class="sxs-lookup"><span data-stu-id="61976-157">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="61976-158">W poniższych przykładach użyto <xref:Microsoft.AspNetCore.Server.Kestrel.Core> przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="61976-158">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="61976-159">Limit czasu utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="61976-159">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="61976-160">Pobiera lub ustawia [limit czasu utrzymywania aktywności](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="61976-160">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="61976-161">Wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="61976-161">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="61976-162">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="61976-162">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="61976-163">Maksymalna liczba współbieżnych otwartych połączeń TCP można ustawić dla całej aplikacji przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="61976-163">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="61976-164">Istnieje oddzielny limit połączeń, które zostały uaktualnione z protokołu HTTP lub HTTPS do innego protokołu (na przykład w żądaniu usługi WebSockets).</span><span class="sxs-lookup"><span data-stu-id="61976-164">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="61976-165">Po uaktualnieniu połączenia nie jest ono wliczane do `MaxConcurrentConnections` limitu.</span><span class="sxs-lookup"><span data-stu-id="61976-165">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="61976-166">Maksymalna liczba połączeń jest domyślnie nieograniczona (null).</span><span class="sxs-lookup"><span data-stu-id="61976-166">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="61976-167">Maksymalny rozmiar treści żądania</span><span class="sxs-lookup"><span data-stu-id="61976-167">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="61976-168">Domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="61976-168">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="61976-169">Zalecanym podejściem do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> atrybutu dla metody akcji:</span><span class="sxs-lookup"><span data-stu-id="61976-169">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="61976-170">Oto przykład, który pokazuje, jak skonfigurować ograniczenie dla aplikacji w każdym żądaniu:</span><span class="sxs-lookup"><span data-stu-id="61976-170">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="61976-171">Zastąp ustawienie określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="61976-171">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="61976-172">Wyjątek jest generowany, jeśli aplikacja skonfiguruje limit żądania po rozpoczęciu uruchamiania aplikacji w celu odczytania żądania.</span><span class="sxs-lookup"><span data-stu-id="61976-172">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="61976-173">Istnieje właściwość, która wskazuje, `MaxRequestBodySize` czy właściwość jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit. `IsReadOnly`</span><span class="sxs-lookup"><span data-stu-id="61976-173">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="61976-174">Gdy aplikacja jest uruchamiana [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model) [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru treści żądania Kestrel jest wyłączony, ponieważ program IIS już ustawia limit.</span><span class="sxs-lookup"><span data-stu-id="61976-174">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="61976-175">Minimalna stawka danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="61976-175">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="61976-176">Kestrel sprawdza co sekundę, jeśli dane są odbierane z określoną szybkością w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="61976-176">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="61976-177">Jeśli szybkość spadnie poniżej wartości minimalnej, upłynął limit czasu połączenia. Okres prolongaty to czas, przez który Kestrel nadaje klientowi zwiększenie jego szybkości wysyłania do minimum; Ta częstotliwość nie jest sprawdzana w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="61976-177">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="61976-178">Okres prolongaty pozwala uniknąć porzucania połączeń, które początkowo wysyłają dane z niską szybkością.</span><span class="sxs-lookup"><span data-stu-id="61976-178">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="61976-179">Domyślna stawka minimalna to 240 bajtów na sekundę z 5-sekundowym okresem prolongaty.</span><span class="sxs-lookup"><span data-stu-id="61976-179">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="61976-180">Minimalna stawka dotyczy także odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="61976-180">A minimum rate also applies to the response.</span></span> <span data-ttu-id="61976-181">Kod określający limit żądań i limit odpowiedzi jest taki sam, z wyjątkiem `RequestBody` `Response` właściwości i nazwy interfejsów.</span><span class="sxs-lookup"><span data-stu-id="61976-181">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="61976-182">Oto przykład pokazujący sposób konfigurowania minimalnych stawek danych w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="61976-182">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="61976-183">Przesłoń minimalne limity szybkości dla żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="61976-183">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="61976-184">Odwołania <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> w poprzednim przykładzie nie występują w `HttpContext.Features` żądaniach HTTP/2, ponieważ Modyfikowanie limitów szybkości dla poszczególnych żądań jest ogólnie nieobsługiwane w przypadku protokołu HTTP/2 z powodu obsługi żądania multipleksowania żądań.</span><span class="sxs-lookup"><span data-stu-id="61976-184">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="61976-185">`HttpContext.Features` `null` Jednak nadal występuje dla żądań HTTP/2, ponieważ limit liczby odczytów nadal można wyłączyć całkowicie dla każdego żądania przez ustawienie `IHttpMinRequestBodyDataRateFeature.MinDataRate` nawet dla żądania HTTP/2. <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature></span><span class="sxs-lookup"><span data-stu-id="61976-185">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="61976-186">Próba odczytania `IHttpMinRequestBodyDataRateFeature.MinDataRate` lub próby ustawienia jej na wartość inną niż `null` spowoduje `NotSupportedException` zgłoszenie żądania HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="61976-186">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="61976-187">Limity szybkości dla całego serwera skonfigurowane za `KestrelServerOptions.Limits` pośrednictwem nadal mają zastosowanie do połączeń HTTP/1. x i http/2.</span><span class="sxs-lookup"><span data-stu-id="61976-187">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="61976-188">Limit czasu nagłówków żądań</span><span class="sxs-lookup"><span data-stu-id="61976-188">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="61976-189">Pobiera lub ustawia maksymalny czas, przez jaki serwer spędza nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="61976-189">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="61976-190">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="61976-190">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="61976-191">Maksymalna liczba strumieni na połączenie</span><span class="sxs-lookup"><span data-stu-id="61976-191">Maximum streams per connection</span></span>

<span data-ttu-id="61976-192">`Http2.MaxStreamsPerConnection`ogranicza liczbę współbieżnych strumieni żądań na połączenie HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="61976-192">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="61976-193">Odrzucone strumienie są nadmiarowe.</span><span class="sxs-lookup"><span data-stu-id="61976-193">Excess streams are refused.</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="61976-194">Wartość domyślna to 100.</span><span class="sxs-lookup"><span data-stu-id="61976-194">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="61976-195">Rozmiar tabeli nagłówka</span><span class="sxs-lookup"><span data-stu-id="61976-195">Header table size</span></span>

<span data-ttu-id="61976-196">Dekoder HPACK dekompresuje nagłówki HTTP dla połączeń HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="61976-196">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="61976-197">`Http2.HeaderTableSize`ogranicza rozmiar tabeli kompresji nagłówka używanej przez dekoder HPACK.</span><span class="sxs-lookup"><span data-stu-id="61976-197">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="61976-198">Wartość jest podawana w oktetach i musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="61976-198">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="61976-199">Wartość domyślna to 4096.</span><span class="sxs-lookup"><span data-stu-id="61976-199">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="61976-200">Maksymalny rozmiar ramki</span><span class="sxs-lookup"><span data-stu-id="61976-200">Maximum frame size</span></span>

<span data-ttu-id="61976-201">`Http2.MaxFrameSize`wskazuje maksymalny dozwolony rozmiar ładunku ramki połączenia HTTP/2 odebranego lub wysłanego przez serwer.</span><span class="sxs-lookup"><span data-stu-id="61976-201">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="61976-202">Wartość jest podawana w oktetach i musi należeć do przedziału od 2 ^ 14 (16 384) do 2 ^ 24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="61976-202">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="61976-203">Wartość domyślna to 2 ^ 14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="61976-203">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="61976-204">Maksymalny rozmiar nagłówka żądania</span><span class="sxs-lookup"><span data-stu-id="61976-204">Maximum request header size</span></span>

<span data-ttu-id="61976-205">`Http2.MaxRequestHeaderFieldSize`wskazuje maksymalny dozwolony rozmiar w oktetach wartości nagłówka żądania.</span><span class="sxs-lookup"><span data-stu-id="61976-205">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="61976-206">Ten limit dotyczy zarówno nazwy, jak i wartości w skompresowanych i nieskompresowanych reprezentacji.</span><span class="sxs-lookup"><span data-stu-id="61976-206">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="61976-207">Wartość musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="61976-207">The value must be greater than zero (0).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="61976-208">Wartość domyślna to 8 192.</span><span class="sxs-lookup"><span data-stu-id="61976-208">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="61976-209">Początkowy rozmiar okna połączenia</span><span class="sxs-lookup"><span data-stu-id="61976-209">Initial connection window size</span></span>

<span data-ttu-id="61976-210">`Http2.InitialConnectionWindowSize`wskazuje maksymalne dane dotyczące treści żądania w bajtach, które są agregowane w buforach serwera w ramach wszystkich żądań (strumieni) na połączenie.</span><span class="sxs-lookup"><span data-stu-id="61976-210">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="61976-211">Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="61976-211">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="61976-212">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="61976-212">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="61976-213">Wartość domyślna to 128 KB (131 072).</span><span class="sxs-lookup"><span data-stu-id="61976-213">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="61976-214">Rozmiar początkowego okna strumienia</span><span class="sxs-lookup"><span data-stu-id="61976-214">Initial stream window size</span></span>

<span data-ttu-id="61976-215">`Http2.InitialStreamWindowSize`wskazuje maksymalne dane dotyczące treści żądania w bajtach bufory serwera w ramach żądania (Stream).</span><span class="sxs-lookup"><span data-stu-id="61976-215">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="61976-216">Żądania są również ograniczone przez `Http2.InitialConnectionWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="61976-216">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="61976-217">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="61976-217">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="61976-218">Wartość domyślna to 96 KB (98 304).</span><span class="sxs-lookup"><span data-stu-id="61976-218">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="61976-219">Synchroniczne we/wy</span><span class="sxs-lookup"><span data-stu-id="61976-219">Synchronous IO</span></span>

<span data-ttu-id="61976-220"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>Określa, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="61976-220"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="61976-221">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="61976-221">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="61976-222">Duża liczba blokowania synchronicznych operacji we/wy może prowadzić do zablokowania puli wątków, co sprawia, że aplikacja nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="61976-222">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="61976-223">Włączaj `AllowSynchronousIO` tylko w przypadku korzystania z biblioteki, która nie obsługuje asynchronicznej operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="61976-223">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="61976-224">Poniższy przykład włącza synchroniczną operację we/wy:</span><span class="sxs-lookup"><span data-stu-id="61976-224">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="61976-225">Aby uzyskać informacje o innych opcjach i ograniczeniach Kestrel, zobacz:</span><span class="sxs-lookup"><span data-stu-id="61976-225">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="61976-226">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="61976-226">Endpoint configuration</span></span>

<span data-ttu-id="61976-227">Domyślnie ASP.NET Core wiąże się z:</span><span class="sxs-lookup"><span data-stu-id="61976-227">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="61976-228">`https://localhost:5001`(gdy obecny jest lokalny certyfikat programistyczny)</span><span class="sxs-lookup"><span data-stu-id="61976-228">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="61976-229">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="61976-229">Specify URLs using the:</span></span>

* <span data-ttu-id="61976-230">`ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="61976-230">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="61976-231">`--urls`argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="61976-231">`--urls` command-line argument.</span></span>
* <span data-ttu-id="61976-232">`urls`klucz konfiguracji hosta.</span><span class="sxs-lookup"><span data-stu-id="61976-232">`urls` host configuration key.</span></span>
* <span data-ttu-id="61976-233">`UseUrls`Metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="61976-233">`UseUrls` extension method.</span></span>

<span data-ttu-id="61976-234">Wartość podana przy użyciu tych metod może być jednym lub większą liczbą punktów końcowych HTTP i HTTPS (HTTPS, jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="61976-234">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="61976-235">Skonfiguruj wartość jako listę rozdzieloną średnikami (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="61976-235">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="61976-236">Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwera](xref:fundamentals/host/web-host#server-urls) i [Zastąp konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="61976-236">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="61976-237">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="61976-237">A development certificate is created:</span></span>

* <span data-ttu-id="61976-238">Po zainstalowaniu [zestaw .NET Core SDK](/dotnet/core/sdk) .</span><span class="sxs-lookup"><span data-stu-id="61976-238">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="61976-239">Do utworzenia certyfikatu służy [Narzędzie dev-certs](xref:aspnetcore-2.1#https) .</span><span class="sxs-lookup"><span data-stu-id="61976-239">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="61976-240">Niektóre przeglądarki wymagają przyznania jawnego uprawnienia do zaufania do lokalnego certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="61976-240">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="61976-241">Szablony projektu konfigurują aplikacje do uruchamiania domyślnie przy użyciu protokołu HTTPS i obejmują [przekierowania https i obsługę HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="61976-241">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="61976-242">Wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> metody w<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> celu skonfigurowania prefiksów i portów adresów URL dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-242">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="61976-243">`UseUrls`, argument wiersza `urls` `ASPNETCORE_URLS` polecenia, klucz konfiguracji hosta i zmienna środowiskowa również działają, ale mają ograniczenia wymienione w dalszej części tej sekcji (certyfikat domyślny musi być dostępny dla punktu końcowego HTTPS `--urls` Konfiguracja).</span><span class="sxs-lookup"><span data-stu-id="61976-243">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="61976-244">`KestrelServerOptions`skonfigurować</span><span class="sxs-lookup"><span data-stu-id="61976-244">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="61976-245">ConfigureEndpointDefaults (Akcja\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="61976-245">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="61976-246">Określa konfigurację `Action` do uruchomienia dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="61976-246">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="61976-247">Wywołanie `ConfigureEndpointDefaults` wielokrotne zastępuje przed `Action`ostatnimi `Action` parametrami.</span><span class="sxs-lookup"><span data-stu-id="61976-247">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureEndpointDefaults(listenOptions =>
                {
                    // Configure endpoint defaults
                });
            })
            .UseStartup<Startup>();
        });
}
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="61976-248">ConfigureHttpsDefaults (Akcja\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="61976-248">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="61976-249">Określa konfigurację `Action` do uruchomienia dla każdego punktu końcowego HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61976-249">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="61976-250">Wywołanie `ConfigureHttpsDefaults` wielokrotne zastępuje przed `Action`ostatnimi `Action` parametrami.</span><span class="sxs-lookup"><span data-stu-id="61976-250">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureHttpsDefaults(listenOptions =>
                {
                    // certificate is an X509Certificate2
                    listenOptions.ServerCertificate = certificate;
                });
            })
            .UseStartup<Startup>();
        });
}
```

### <a name="configureiconfiguration"></a><span data-ttu-id="61976-251">Konfiguruj (IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="61976-251">Configure(IConfiguration)</span></span>

<span data-ttu-id="61976-252">Tworzy moduł ładujący konfigurację na potrzeby konfigurowania Kestrel, które <xref:Microsoft.Extensions.Configuration.IConfiguration> pobiera jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="61976-252">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="61976-253">Konfiguracja musi być objęta zakresem sekcji konfiguracji dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-253">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="61976-254">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="61976-254">ListenOptions.UseHttps</span></span>

<span data-ttu-id="61976-255">Skonfiguruj Kestrel do korzystania z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61976-255">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="61976-256">`ListenOptions.UseHttps`rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="61976-256">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="61976-257">`UseHttps`&ndash; Skonfiguruj Kestrel do używania protokołu HTTPS z domyślnym certyfikatem.</span><span class="sxs-lookup"><span data-stu-id="61976-257">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="61976-258">Zgłasza wyjątek, jeśli nie został skonfigurowany żaden certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="61976-258">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="61976-259">`ListenOptions.UseHttps`wejściowe</span><span class="sxs-lookup"><span data-stu-id="61976-259">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="61976-260">`filename`jest ścieżką i nazwą pliku certyfikatu względem katalogu zawierającego pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="61976-260">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="61976-261">`password`to hasło wymagane do uzyskania dostępu do danych certyfikatu X. 509.</span><span class="sxs-lookup"><span data-stu-id="61976-261">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="61976-262">`configureOptions``Action` jest do skonfigurowania `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="61976-262">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="61976-263">Zwraca wartość `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="61976-263">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="61976-264">`storeName`to magazyn certyfikatów, z którego ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="61976-264">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="61976-265">`subject`to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="61976-265">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="61976-266">`allowInvalid`wskazuje, czy należy wziąć pod uwagę nieprawidłowe certyfikaty, takie jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="61976-266">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="61976-267">`location`jest lokalizacją magazynu, z której ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="61976-267">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="61976-268">`serverCertificate`jest certyfikatem X. 509.</span><span class="sxs-lookup"><span data-stu-id="61976-268">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="61976-269">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61976-269">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="61976-270">Należy podać co najmniej certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="61976-270">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="61976-271">Obsługiwane konfiguracje opisane dalej:</span><span class="sxs-lookup"><span data-stu-id="61976-271">Supported configurations described next:</span></span>

* <span data-ttu-id="61976-272">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="61976-272">No configuration</span></span>
* <span data-ttu-id="61976-273">Zastąp domyślny certyfikat z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="61976-273">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="61976-274">Zmień wartości domyślne w kodzie</span><span class="sxs-lookup"><span data-stu-id="61976-274">Change the defaults in code</span></span>

<span data-ttu-id="61976-275">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="61976-275">*No configuration*</span></span>

<span data-ttu-id="61976-276">Kestrel nasłuchuje `http://localhost:5000` w `https://localhost:5001` systemie i (Jeśli domyślny certyfikat jest dostępny).</span><span class="sxs-lookup"><span data-stu-id="61976-276">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="61976-277">*Zastąp domyślny certyfikat z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="61976-277">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="61976-278">`CreateDefaultBuilder`wywołania `Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-278">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="61976-279">Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępny dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-279">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="61976-280">Konfigurowanie wielu punktów końcowych, w tym adresów URL i certyfikatów do użycia, z pliku znajdującego się na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="61976-280">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="61976-281">W poniższym przykładzie pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="61976-281">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="61976-282">Ustaw **AllowInvalid** na `true` , aby zezwolić na korzystanie z nieprawidłowych certyfikatów (na przykład certyfikatów z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="61976-282">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="61976-283">Wszystkie punkty końcowe HTTPS, które nie określają certyfikatu (**HttpsDefaultCert** w poniższym przykładzie) powracają do certyfikatu zdefiniowanego w obszarze **Certyfikaty** > **domyślne** lub certyfikat programistyczny.</span><span class="sxs-lookup"><span data-stu-id="61976-283">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="61976-284">Alternatywą dla korzystania z **ścieżki** i **hasła** dla dowolnego węzła certyfikatu jest określenie certyfikatu przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="61976-284">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="61976-285">Certyfikat**domyślny** można na > przykład określić jako:</span><span class="sxs-lookup"><span data-stu-id="61976-285">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="61976-286">Uwagi dotyczące schematu:</span><span class="sxs-lookup"><span data-stu-id="61976-286">Schema notes:</span></span>

* <span data-ttu-id="61976-287">W nazwach punktów końcowych nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="61976-287">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="61976-288">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="61976-288">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="61976-289">`Url` Parametr jest wymagany dla każdego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="61976-289">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="61976-290">Format tego parametru jest taki sam jak parametr konfiguracji najwyższego poziomu `Urls` , z tą różnicą, że jest ograniczony do pojedynczej wartości.</span><span class="sxs-lookup"><span data-stu-id="61976-290">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="61976-291">Te punkty końcowe zastępują te zdefiniowane w konfiguracji najwyższego poziomu `Urls` zamiast dodawać je do nich.</span><span class="sxs-lookup"><span data-stu-id="61976-291">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="61976-292">Punkty końcowe zdefiniowane w kodzie `Listen` za pośrednictwem łączą się z punktami końcowymi zdefiniowanymi w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="61976-292">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="61976-293">`Certificate` Sekcja jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="61976-293">The `Certificate` section is optional.</span></span> <span data-ttu-id="61976-294">`Certificate` Jeśli sekcja nie jest określona, używane są wartości domyślne zdefiniowane we wcześniejszych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="61976-294">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="61976-295">Jeśli żadne wartości domyślne nie są dostępne, serwer zgłasza wyjątek i nie może się uruchomić.</span><span class="sxs-lookup"><span data-stu-id="61976-295">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="61976-296">&ndash;&ndash;Sekcja obsługuje zarówno hasło ścieżki, jak i certyfikaty magazynu podmiotu. `Certificate`</span><span class="sxs-lookup"><span data-stu-id="61976-296">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="61976-297">W ten sposób można zdefiniować dowolną liczbę punktów końcowych, dopóki nie spowoduje to konfliktów portów.</span><span class="sxs-lookup"><span data-stu-id="61976-297">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="61976-298">`options.Configure(context.Configuration.GetSection("{SECTION}"))``KestrelConfigurationLoader` zwracametodę,któramożesłużyćdouzupełnianiaskonfigurowanych`.Endpoint(string name, listenOptions => { })` ustawień punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="61976-298">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseKestrel((context, serverOptions) =>
            {
                serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                    .Endpoint("HTTPS", listenOptions =>
                    {
                        listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                    });
            });
        });
```

<span data-ttu-id="61976-299">`KestrelServerOptions.ConfigurationLoader`można uzyskać bezpośredni dostęp do kontynuowania iteracji w istniejącym module ładującym, takim jak dostarczony <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>przez.</span><span class="sxs-lookup"><span data-stu-id="61976-299">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="61976-300">Sekcja konfiguracji dla każdego punktu końcowego jest dostępna w opcjach w `Endpoint` metodzie, aby można było odczytać ustawienia niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="61976-300">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="61976-301">Można załadować wiele konfiguracji, wywołując `options.Configure(context.Configuration.GetSection("{SECTION}"))` ponownie z inną sekcją.</span><span class="sxs-lookup"><span data-stu-id="61976-301">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="61976-302">Używana jest tylko Ostatnia konfiguracja, chyba że `Load` jest jawnie wywoływana w poprzednich wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="61976-302">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="61976-303">Pakiet nie wywołuje metody `Load` , aby można było zastąpić sekcję konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="61976-303">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="61976-304">`KestrelConfigurationLoader`odzwierciedla rodzinę interfejsów API z `KestrelServerOptions` programu `Endpoint` jako przeciążenia, dlatego punkty końcowe kodu i konfiguracji można skonfigurować w tym samym miejscu. `Listen`</span><span class="sxs-lookup"><span data-stu-id="61976-304">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="61976-305">Te przeciążenia nie używają nazw i używają tylko ustawień domyślnych z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="61976-305">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="61976-306">*Zmień wartości domyślne w kodzie*</span><span class="sxs-lookup"><span data-stu-id="61976-306">*Change the defaults in code*</span></span>

<span data-ttu-id="61976-307">`ConfigureEndpointDefaults`i `ConfigureHttpsDefaults` może służyć do zmiany ustawień domyślnych dla `ListenOptions` i `HttpsConnectionAdapterOptions`, w tym przesłanianie domyślnego certyfikatu określonego w poprzednim scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="61976-307">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="61976-308">`ConfigureEndpointDefaults`i `ConfigureHttpsDefaults` powinny być wywoływane przed skonfigurowaniem punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="61976-308">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureEndpointDefaults(listenOptions =>
                {
                    // Configure endpoint defaults
                });

                serverOptions.ConfigureHttpsDefaults(listenOptions =>
                {
                    listenOptions.SslProtocols = SslProtocols.Tls12;
                });
            });
        });
```

<span data-ttu-id="61976-309">*Obsługa usługi Kestrel dla SNI*</span><span class="sxs-lookup"><span data-stu-id="61976-309">*Kestrel support for SNI*</span></span>

<span data-ttu-id="61976-310">[Oznaczanie nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do hostowania wielu domen na tym samym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="61976-310">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="61976-311">Aby SNI działał, klient wysyła nazwę hosta dla bezpiecznej sesji do serwera podczas uzgadniania TLS, aby serwer mógł zapewnić prawidłowy certyfikat.</span><span class="sxs-lookup"><span data-stu-id="61976-311">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="61976-312">Klient korzysta z dostarczonego certyfikatu w celu zaszyfrowania komunikacji z serwerem podczas bezpiecznej sesji, która następuje po uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="61976-312">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="61976-313">Kestrel obsługuje SNI za pośrednictwem `ServerCertificateSelector` wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="61976-313">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="61976-314">Wywołanie zwrotne jest wywoływane jednokrotnie dla każdego połączenia, aby umożliwić aplikacji inspekcję nazwy hosta i wybieranie odpowiedniego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="61976-314">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="61976-315">Obsługa SNI wymaga:</span><span class="sxs-lookup"><span data-stu-id="61976-315">SNI support requires:</span></span>

* <span data-ttu-id="61976-316">Uruchomiona w środowisku `netcoreapp2.1` docelowym lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="61976-316">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="61976-317">W `net461` dniu lub później wywołanie zwrotne jest wywoływane, `name` ale jest `null`zawsze.</span><span class="sxs-lookup"><span data-stu-id="61976-317">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="61976-318">Jest `name` również`null` , jeśli klient nie poda parametru nazwy hosta w uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="61976-318">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="61976-319">Wszystkie witryny sieci Web działają na tym samym wystąpieniu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-319">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="61976-320">Kestrel nie obsługuje udostępniania adresu IP i portu w wielu wystąpieniach bez zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="61976-320">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ListenAnyIP(5005, listenOptions =>
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
            .UseStartup<Startup>();
        });
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="61976-321">Powiąż z gniazdem TCP</span><span class="sxs-lookup"><span data-stu-id="61976-321">Bind to a TCP socket</span></span>

<span data-ttu-id="61976-322"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> Metoda wiąże się z gniazdem TCP, a opcja lambda zezwala na konfigurację certyfikatu X. 509:</span><span class="sxs-lookup"><span data-stu-id="61976-322">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="61976-323">Przykład konfiguruje HTTPS dla punktu końcowego za <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>pomocą.</span><span class="sxs-lookup"><span data-stu-id="61976-323">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="61976-324">Użyj tego samego interfejsu API, aby skonfigurować inne ustawienia Kestrel dla określonych punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="61976-324">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="61976-325">Powiąż z gniazdem systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="61976-325">Bind to a Unix socket</span></span>

<span data-ttu-id="61976-326">Nasłuchiwanie w gnieździe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> systemu UNIX za pomocą programu w celu zwiększenia wydajności za pomocą usługi Nginx, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="61976-326">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="61976-327">Port 0</span><span class="sxs-lookup"><span data-stu-id="61976-327">Port 0</span></span>

<span data-ttu-id="61976-328">Gdy numer `0` portu jest określony, Kestrel dynamicznie wiąże się z dostępnym portem.</span><span class="sxs-lookup"><span data-stu-id="61976-328">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="61976-329">W poniższym przykładzie pokazano, jak określić, który port Kestrel faktycznie powiązany w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="61976-329">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="61976-330">Po uruchomieniu aplikacji dane wyjściowe okna konsoli wskazują port dynamiczny, w którym można uzyskać dostęp do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="61976-330">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="61976-331">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="61976-331">Limitations</span></span>

<span data-ttu-id="61976-332">Skonfiguruj punkty końcowe przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="61976-332">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="61976-333">`--urls`argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="61976-333">`--urls` command-line argument</span></span>
* <span data-ttu-id="61976-334">`urls`klucz konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="61976-334">`urls` host configuration key</span></span>
* <span data-ttu-id="61976-335">`ASPNETCORE_URLS`Zmienna środowiskowa</span><span class="sxs-lookup"><span data-stu-id="61976-335">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="61976-336">Te metody są przydatne do tworzenia kodu w pracy z serwerami innymi niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-336">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="61976-337">Należy jednak pamiętać o następujących ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="61976-337">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="61976-338">Nie można użyć protokołu HTTPS z tymi metodami, chyba że w konfiguracji punktu końcowego HTTPS jest podany certyfikat domyślny (na `KestrelServerOptions` przykład przy użyciu konfiguracji lub pliku konfiguracji, jak pokazano wcześniej w tym temacie).</span><span class="sxs-lookup"><span data-stu-id="61976-338">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="61976-339">Gdy oba `Listen` podejścia i `UseUrls` `UseUrls` są używane jednocześnie, punktykońcowezastępująpunktykońcowe.`Listen`</span><span class="sxs-lookup"><span data-stu-id="61976-339">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="61976-340">Konfiguracja punktu końcowego usług IIS</span><span class="sxs-lookup"><span data-stu-id="61976-340">IIS endpoint configuration</span></span>

<span data-ttu-id="61976-341">W przypadku korzystania z usług IIS powiązania URL dla powiązań przesłonięć usług IIS są ustawiane `UseUrls`przez `Listen` lub.</span><span class="sxs-lookup"><span data-stu-id="61976-341">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="61976-342">Aby uzyskać więcej informacji, zobacz temat [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="61976-342">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="61976-343">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="61976-343">ListenOptions.Protocols</span></span>

<span data-ttu-id="61976-344">Właściwość ustanawia protokoły HTTP (`HttpProtocols`) włączone w punkcie końcowym połączenia lub dla serwera. `Protocols`</span><span class="sxs-lookup"><span data-stu-id="61976-344">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="61976-345">Przypisz wartość do `Protocols` właściwości `HttpProtocols` z wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="61976-345">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="61976-346">`HttpProtocols`wartość wyliczenia</span><span class="sxs-lookup"><span data-stu-id="61976-346">`HttpProtocols` enum value</span></span> | <span data-ttu-id="61976-347">Dozwolony protokół połączenia</span><span class="sxs-lookup"><span data-stu-id="61976-347">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="61976-348">Tylko HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="61976-348">HTTP/1.1 only.</span></span> <span data-ttu-id="61976-349">Może być używany z protokołem TLS lub bez niego.</span><span class="sxs-lookup"><span data-stu-id="61976-349">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="61976-350">Tylko HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="61976-350">HTTP/2 only.</span></span> <span data-ttu-id="61976-351">Mogą być używane bez protokołu TLS tylko wtedy, gdy klient obsługuje [poprzedni tryb wiedzy](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="61976-351">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="61976-352">HTTP/1.1 i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="61976-352">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="61976-353">Protokół HTTP/2 wymaga od klienta wyboru protokołu HTTP/2 w uzgadnianiu [negocjowania protokołu TLS aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3) . w przeciwnym razie wartość domyślna połączenia to HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="61976-353">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="61976-354">Wartość domyślna `ListenOptions.Protocols` dla każdego punktu końcowego to `HttpProtocols.Http1AndHttp2`.</span><span class="sxs-lookup"><span data-stu-id="61976-354">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="61976-355">Ograniczenia protokołu TLS dla protokołu HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="61976-355">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="61976-356">TLS w wersji 1,2 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="61976-356">TLS version 1.2 or later</span></span>
* <span data-ttu-id="61976-357">Ponowne negocjowanie wyłączone</span><span class="sxs-lookup"><span data-stu-id="61976-357">Renegotiation disabled</span></span>
* <span data-ttu-id="61976-358">Kompresja wyłączona</span><span class="sxs-lookup"><span data-stu-id="61976-358">Compression disabled</span></span>
* <span data-ttu-id="61976-359">Minimalne rozmiary tymczasowych kluczy wymiany:</span><span class="sxs-lookup"><span data-stu-id="61976-359">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="61976-360">Krzywa eliptyczna Diffie-Hellmana (ECDHE &lbrack;) [RFC4492](https://www.ietf.org/rfc/rfc4492.txt) &rbrack; &ndash; 224 (minimum)</span><span class="sxs-lookup"><span data-stu-id="61976-360">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="61976-361">Ograniczone pole Diffie-Hellmana (DHE) &lbrack; `TLS12` &rbrack; &ndash; 2048 bity minimum</span><span class="sxs-lookup"><span data-stu-id="61976-361">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="61976-362">Mechanizm szyfrowania nie został zabroniony</span><span class="sxs-lookup"><span data-stu-id="61976-362">Cipher suite not blacklisted</span></span>

<span data-ttu-id="61976-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`&lbrack;zkrzywą eliptycznaP&lbrack; -256 jest domyślnieobsługiwana.`FIPS186` `TLS-ECDHE` &rbrack; &rbrack;</span><span class="sxs-lookup"><span data-stu-id="61976-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="61976-364">Poniższy przykład umożliwia nawiązywanie połączeń HTTP/1.1 i HTTP/2 na porcie 8000.</span><span class="sxs-lookup"><span data-stu-id="61976-364">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="61976-365">Połączenia są zabezpieczone przez protokół TLS przy użyciu podanego certyfikatu:</span><span class="sxs-lookup"><span data-stu-id="61976-365">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="61976-366">Opcjonalnie Używaj oprogramowania pośredniczącego do filtrowania uzgadniania protokołu TLS dla poszczególnych połączeń dla określonych szyfrów:</span><span class="sxs-lookup"><span data-stu-id="61976-366">Optionally use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseConnectionHandler<TlsFilterConnectionHandler>();
    });
});
```

```csharp
using System;
using System.Buffers;
using System.Security.Authentication;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Connections;
using Microsoft.AspNetCore.Connections.Features;

public class TlsFilterConnectionHandler : ConnectionHandler
{
    public override async Task OnConnectedAsync(ConnectionContext connection)
    {
        var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
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

        while (true)
        {
            var result = await connection.Transport.Input.ReadAsync();
            var buffer = result.Buffer;

            if (!buffer.IsEmpty)
            {
                await connection.Transport.Output.WriteAsync(buffer.ToArray());
            }
            else if (result.IsCompleted)
            {
                break;
            }

            connection.Transport.Input.AdvanceTo(buffer.End);
        }
    }
}
```

<span data-ttu-id="61976-367">*Ustawianie protokołu z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="61976-367">*Set the protocol from configuration*</span></span>

<span data-ttu-id="61976-368">`CreateDefaultBuilder`wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-368">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="61976-369">W poniższym przykładzie pliku *appSettings. JSON* jako domyślny protokół połączenia dla wszystkich punktów końcowych:</span><span class="sxs-lookup"><span data-stu-id="61976-369">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="61976-370">Poniższy przykład *appSettings. JSON* ustanawia protokół połączeń HTTP/1.1 dla określonego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="61976-370">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1"
      }
    }
  }
}
```

<span data-ttu-id="61976-371">Protokoły określone w wartościach zastąpienia kodu ustawione przez konfigurację.</span><span class="sxs-lookup"><span data-stu-id="61976-371">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="61976-372">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="61976-372">Transport configuration</span></span>

<span data-ttu-id="61976-373">Dla projektów, które wymagają użycia Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span><span class="sxs-lookup"><span data-stu-id="61976-373">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="61976-374">Dodaj zależność dla pakietu [Microsoft. AspNetCore. Server. Kestrel. transport. Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="61976-374">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="61976-375">Wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>: `IWebHostBuilder`</span><span class="sxs-lookup"><span data-stu-id="61976-375">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

   ```csharp
   public class Program
   {
       public static void Main(string[] args)
       {
           CreateHostBuilder(args).Build().Run();
       }

       public static IHostBuilder CreateHostBuilder(string[] args) =>
           Host.CreateDefaultBuilder(args)
               .ConfigureWebHostDefaults(webBuilder =>
               {
                   webBuilder.UseLibuv();
                   webBuilder.UseStartup<Startup>();
               });
   }
   ```

### <a name="url-prefixes"></a><span data-ttu-id="61976-376">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="61976-376">URL prefixes</span></span>

<span data-ttu-id="61976-377">W przypadku `UseUrls`użycia `--urls` , argumentu wiersza polecenia, `urls` klucza konfiguracji hosta lub `ASPNETCORE_URLS` zmiennej środowiskowej prefiksy adresów URL mogą znajdować się w jednym z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="61976-377">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="61976-378">Tylko prefiksy adresów URL HTTP są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="61976-378">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="61976-379">Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązań adresów `UseUrls`URL przy użyciu programu.</span><span class="sxs-lookup"><span data-stu-id="61976-379">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="61976-380">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="61976-380">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="61976-381">`0.0.0.0`jest szczególnym przypadkiem, który wiąże się ze wszystkimi adresami IPv4.</span><span class="sxs-lookup"><span data-stu-id="61976-381">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="61976-382">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="61976-382">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="61976-383">`[::]`jest odpowiednikiem IPv6 dla protokołu `0.0.0.0`IPv4.</span><span class="sxs-lookup"><span data-stu-id="61976-383">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="61976-384">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="61976-384">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="61976-385">Nazwy `*`hostów, i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="61976-385">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="61976-386">Wszystkie nie są rozpoznawane jako prawidłowy adres IP `localhost` lub są powiązane ze wszystkimi IP IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="61976-386">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="61976-387">Aby powiązać różne nazwy hostów z różnymi ASP.NET Core aplikacjami na tym samym porcie, użyj [protokołu HTTP. sys](xref:fundamentals/servers/httpsys) lub odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="61976-387">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="61976-388">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="61976-388">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="61976-389">Nazwa `localhost` hosta z numerem portu lub adresem IP sprzężenia zwrotnego z numerem portu</span><span class="sxs-lookup"><span data-stu-id="61976-389">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="61976-390">Gdy `localhost` jest określony, Kestrel próbuje powiązać z interfejsami sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="61976-390">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="61976-391">Jeśli żądany port jest używany przez inną usługę w dowolnym interfejsie sprzężenia zwrotnego, uruchomienie Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="61976-391">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="61976-392">Jeśli którykolwiek z innych przyczyn interfejsu sprzężenia zwrotnego jest niedostępny (najczęściej jest to spowodowane tym, że protokół IPv6 nie jest obsługiwany), Kestrel rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="61976-392">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="61976-393">Filtrowanie hostów</span><span class="sxs-lookup"><span data-stu-id="61976-393">Host filtering</span></span>

<span data-ttu-id="61976-394">Program Kestrel obsługuje konfigurację na podstawie prefiksów, takich `http://example.com:5000`jak, Kestrel w znacznym stopniu ignoruje nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="61976-394">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="61976-395">Host `localhost` jest specjalnym przypadkiem używanym do tworzenia powiązań z adresami sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="61976-395">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="61976-396">Każdy host poza jawnym adresem IP tworzy powiązanie ze wszystkimi publicznymi adresami IP.</span><span class="sxs-lookup"><span data-stu-id="61976-396">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="61976-397">`Host`nagłówki nie są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="61976-397">`Host` headers aren't validated.</span></span>

<span data-ttu-id="61976-398">Aby obejść ten sposób, użyj oprogramowania pośredniczącego filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="61976-398">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="61976-399">Oprogramowanie pośredniczące do filtrowania hosta jest dostarczane przez pakiet [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , który jest niejawnie dostarczany dla ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="61976-399">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="61976-400">Oprogramowanie pośredniczące jest dodawane przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>program, który <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>wywołuje następujące wywołania:</span><span class="sxs-lookup"><span data-stu-id="61976-400">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="61976-401">Programowe filtrowanie hosta jest domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="61976-401">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="61976-402">Aby włączyć oprogramowanie pośredniczące, zdefiniuj klucz `AllowedHosts` w pliku *appSettings. JSON*/ *.\< EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="61976-402">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="61976-403">Wartość to rozdzielana średnikami lista nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="61976-403">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="61976-404">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="61976-404">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="61976-405">[Przekierowane nagłówki oprogramowania](xref:host-and-deploy/proxy-load-balancer) również mają <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> opcję.</span><span class="sxs-lookup"><span data-stu-id="61976-405">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="61976-406">Przekazane nagłówki — oprogramowanie pośredniczące i filtrowanie hostów oprogramowanie pośredniczące ma podobną funkcjonalność dla różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="61976-406">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="61976-407">Ustawienie `AllowedHosts` z przekierowanymi nagłówkami — oprogramowanie pośredniczące jest odpowiednie, `Host` gdy nagłówek nie jest zachowywany podczas przekazywania żądań z odwrotnym serwerem proxy lub modułem równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="61976-407">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="61976-408">Ustawienie `AllowedHosts` przy użyciu oprogramowania pośredniczącego filtrowania hosta jest odpowiednie, gdy Kestrel jest używany jako publiczny serwer graniczny lub `Host` gdy nagłówek jest bezpośrednio przekazywany.</span><span class="sxs-lookup"><span data-stu-id="61976-408">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="61976-409">Aby uzyskać więcej informacji na temat przekierowanych nagłówków <xref:host-and-deploy/proxy-load-balancer>, należy zapoznać się z tematem.</span><span class="sxs-lookup"><span data-stu-id="61976-409">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="61976-410">Kestrel to Międzyplatformowy [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="61976-410">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="61976-411">Kestrel to serwer sieci Web, który jest domyślnie uwzględniony w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61976-411">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="61976-412">Kestrel obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="61976-412">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="61976-413">HTTPS</span><span class="sxs-lookup"><span data-stu-id="61976-413">HTTPS</span></span>
* <span data-ttu-id="61976-414">Nieprzezroczyste uaktualnienie używane do włączania obsługi obiektów [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="61976-414">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="61976-415">Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx</span><span class="sxs-lookup"><span data-stu-id="61976-415">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="61976-416">HTTP/2 (z wyjątkiem macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="61976-416">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="61976-417">&dagger;Protokół HTTP/2 będzie obsługiwany w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="61976-417">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="61976-418">Kestrel jest obsługiwana na wszystkich platformach i wersjach obsługiwanych przez platformę .NET Core.</span><span class="sxs-lookup"><span data-stu-id="61976-418">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="61976-419">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="61976-419">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="61976-420">Obsługa protokołu HTTP/2</span><span class="sxs-lookup"><span data-stu-id="61976-420">HTTP/2 support</span></span>

<span data-ttu-id="61976-421">[Protokół HTTP/2](https://httpwg.org/specs/rfc7540.html) jest dostępny dla aplikacji ASP.NET Core, jeśli spełnione są następujące wymagania podstawowe:</span><span class="sxs-lookup"><span data-stu-id="61976-421">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="61976-422">System operacyjny&dagger;</span><span class="sxs-lookup"><span data-stu-id="61976-422">Operating system&dagger;</span></span>
  * <span data-ttu-id="61976-423">Windows Server 2016/Windows 10 lub nowszy&Dagger;</span><span class="sxs-lookup"><span data-stu-id="61976-423">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="61976-424">Linux z OpenSSL 1.0.2 lub nowszym (na przykład Ubuntu 16,04 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="61976-424">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="61976-425">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="61976-425">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="61976-426">Połączenie [negocjowania protokołu warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="61976-426">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="61976-427">Protokół TLS 1.2 lub nowszej połączenia</span><span class="sxs-lookup"><span data-stu-id="61976-427">TLS 1.2 or later connection</span></span>

<span data-ttu-id="61976-428">&dagger;Protokół HTTP/2 będzie obsługiwany w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="61976-428">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="61976-429">&Dagger;Kestrel ma ograniczoną obsługę protokołu HTTP/2 w systemie Windows Server 2012 R2 i Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="61976-429">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="61976-430">Obsługa jest ograniczona, ponieważ lista obsługiwanych mechanizmów szyfrowania TLS dostępnych w tych systemach operacyjnych jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="61976-430">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="61976-431">Do zabezpieczenia połączeń TLS może być wymagany certyfikat wygenerowany przy użyciu algorytmu podpisu cyfrowego (ECDSA) krzywej eliptycznej.</span><span class="sxs-lookup"><span data-stu-id="61976-431">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="61976-432">Jeśli zostanie nawiązane połączenie HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="61976-432">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="61976-433">Protokół HTTP/2 jest domyślnie wyłączony.</span><span class="sxs-lookup"><span data-stu-id="61976-433">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="61976-434">Więcej informacji na temat konfiguracji można znaleźć w sekcjach [Kestrel Options](#kestrel-options) and [ListenOptions. Protocols](#listenoptionsprotocols) .</span><span class="sxs-lookup"><span data-stu-id="61976-434">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="61976-435">Kiedy używać Kestrel z zwrotnym serwerem proxy</span><span class="sxs-lookup"><span data-stu-id="61976-435">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="61976-436">Kestrel może być używana przez siebie lub z *odwrotnym serwerem proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org)lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="61976-436">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="61976-437">Odwrotny serwer proxy odbiera żądania HTTP z sieci i przekazuje je do usługi Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-437">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="61976-438">Kestrel używany jako serwer sieci Web z krawędzią (dostępną z Internetu):</span><span class="sxs-lookup"><span data-stu-id="61976-438">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem bez serwera proxy zwrotnego](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="61976-440">Kestrel używany w konfiguracji zwrotnego serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="61976-440">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel komunikuje się pośrednio z Internetem za pomocą odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="61976-442">Konfiguracja, z serwerem proxy odwrotnych lub bez niego, jest obsługiwaną konfiguracją hostingu.</span><span class="sxs-lookup"><span data-stu-id="61976-442">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="61976-443">Kestrel używany jako serwer graniczny bez serwera proxy odwrotnego nie obsługuje udostępniania tego samego adresu IP i portu między wieloma procesami.</span><span class="sxs-lookup"><span data-stu-id="61976-443">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="61976-444">Gdy Kestrel jest skonfigurowany do nasłuchiwania na porcie, Kestrel obsługuje cały ruch dla tego portu bez względu na `Host` nagłówki żądań.</span><span class="sxs-lookup"><span data-stu-id="61976-444">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="61976-445">Zwrotny serwer proxy, który może udostępniać porty, ma możliwość przekazywania żądań do Kestrel na unikatowym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="61976-445">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="61976-446">Nawet jeśli odwrotny serwer proxy nie jest wymagany, może to być dobry wybór przy użyciu odwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="61976-446">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="61976-447">Zwrotny serwer proxy:</span><span class="sxs-lookup"><span data-stu-id="61976-447">A reverse proxy:</span></span>

* <span data-ttu-id="61976-448">Może ograniczać uwidoczniony obszar publicznej powierzchni aplikacji, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="61976-448">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="61976-449">Zapewnienie dodatkowej warstwy konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="61976-449">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="61976-450">Integracja z istniejącą infrastrukturą może być lepsza.</span><span class="sxs-lookup"><span data-stu-id="61976-450">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="61976-451">Uprość Równoważenie obciążenia i konfigurację komunikacji zabezpieczonej (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="61976-451">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="61976-452">Tylko serwer zwrotny proxy wymaga certyfikatu X. 509, a serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="61976-452">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="61976-453">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="61976-453">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="61976-454">Jak używać Kestrel w aplikacjach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61976-454">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="61976-455">Pakiet [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="61976-455">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="61976-456">Szablony projektów ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-456">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="61976-457">W *program.cs*, kod szablonu wywołuje <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, który wywołuje <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> się w tle.</span><span class="sxs-lookup"><span data-stu-id="61976-457">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="61976-458">Aby zapewnić dodatkową konfigurację po wywołaniu `CreateDefaultBuilder`, użyj `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="61976-458">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="61976-459">Jeśli aplikacja nie wywoła połączenia `CreateDefaultBuilder` w celu skonfigurowania hosta, wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **przed** wywołaniem `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="61976-459">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="61976-460">Opcje Kestrel</span><span class="sxs-lookup"><span data-stu-id="61976-460">Kestrel options</span></span>

<span data-ttu-id="61976-461">Serwer sieci Web Kestrel ma opcje konfiguracji ograniczeń, które są szczególnie przydatne w przypadku wdrożeń mających dostęp do Internetu.</span><span class="sxs-lookup"><span data-stu-id="61976-461">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="61976-462">Ustaw ograniczenia <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> właściwości <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> klasy.</span><span class="sxs-lookup"><span data-stu-id="61976-462">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="61976-463">Właściwość przechowuje wystąpienie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>klasy. `Limits`</span><span class="sxs-lookup"><span data-stu-id="61976-463">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="61976-464">W poniższych przykładach użyto <xref:Microsoft.AspNetCore.Server.Kestrel.Core> przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="61976-464">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="61976-465">Limit czasu utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="61976-465">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="61976-466">Pobiera lub ustawia [limit czasu utrzymywania aktywności](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="61976-466">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="61976-467">Wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="61976-467">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="61976-468">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="61976-468">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="61976-469">Maksymalna liczba współbieżnych otwartych połączeń TCP można ustawić dla całej aplikacji przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="61976-469">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="61976-470">Istnieje oddzielny limit połączeń, które zostały uaktualnione z protokołu HTTP lub HTTPS do innego protokołu (na przykład w żądaniu usługi WebSockets).</span><span class="sxs-lookup"><span data-stu-id="61976-470">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="61976-471">Po uaktualnieniu połączenia nie jest ono wliczane do `MaxConcurrentConnections` limitu.</span><span class="sxs-lookup"><span data-stu-id="61976-471">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="61976-472">Maksymalna liczba połączeń jest domyślnie nieograniczona (null).</span><span class="sxs-lookup"><span data-stu-id="61976-472">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="61976-473">Maksymalny rozmiar treści żądania</span><span class="sxs-lookup"><span data-stu-id="61976-473">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="61976-474">Domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="61976-474">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="61976-475">Zalecanym podejściem do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> atrybutu dla metody akcji:</span><span class="sxs-lookup"><span data-stu-id="61976-475">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="61976-476">Oto przykład, który pokazuje, jak skonfigurować ograniczenie dla aplikacji w każdym żądaniu:</span><span class="sxs-lookup"><span data-stu-id="61976-476">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="61976-477">Zastąp ustawienie określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="61976-477">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="61976-478">Wyjątek jest generowany, jeśli aplikacja skonfiguruje limit żądania po rozpoczęciu uruchamiania aplikacji w celu odczytania żądania.</span><span class="sxs-lookup"><span data-stu-id="61976-478">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="61976-479">Istnieje właściwość, która wskazuje, `MaxRequestBodySize` czy właściwość jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit. `IsReadOnly`</span><span class="sxs-lookup"><span data-stu-id="61976-479">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="61976-480">Gdy aplikacja jest uruchamiana [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model) [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru treści żądania Kestrel jest wyłączony, ponieważ program IIS już ustawia limit.</span><span class="sxs-lookup"><span data-stu-id="61976-480">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="61976-481">Minimalna stawka danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="61976-481">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="61976-482">Kestrel sprawdza co sekundę, jeśli dane są odbierane z określoną szybkością w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="61976-482">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="61976-483">Jeśli szybkość spadnie poniżej wartości minimalnej, upłynął limit czasu połączenia. Okres prolongaty to czas, przez który Kestrel nadaje klientowi zwiększenie jego szybkości wysyłania do minimum; Ta częstotliwość nie jest sprawdzana w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="61976-483">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="61976-484">Okres prolongaty pozwala uniknąć porzucania połączeń, które początkowo wysyłają dane z niską szybkością.</span><span class="sxs-lookup"><span data-stu-id="61976-484">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="61976-485">Domyślna stawka minimalna to 240 bajtów na sekundę z 5-sekundowym okresem prolongaty.</span><span class="sxs-lookup"><span data-stu-id="61976-485">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="61976-486">Minimalna stawka dotyczy także odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="61976-486">A minimum rate also applies to the response.</span></span> <span data-ttu-id="61976-487">Kod określający limit żądań i limit odpowiedzi jest taki sam, z wyjątkiem `RequestBody` `Response` właściwości i nazwy interfejsów.</span><span class="sxs-lookup"><span data-stu-id="61976-487">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="61976-488">Oto przykład pokazujący sposób konfigurowania minimalnych stawek danych w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="61976-488">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="61976-489">Przesłoń minimalne limity szybkości dla żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="61976-489">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="61976-490">Funkcja rate, do której odwołuje się Poprzednia `HttpContext.Features` próba, nie jest dostępna w przypadku żądań HTTP/2, ponieważ Modyfikowanie limitów szybkości dla poszczególnych żądań nie jest obsługiwane w przypadku protokołu HTTP/2 ze względu na obsługę multipleksera żądania.</span><span class="sxs-lookup"><span data-stu-id="61976-490">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="61976-491">Limity szybkości dla całego serwera skonfigurowane za `KestrelServerOptions.Limits` pośrednictwem nadal mają zastosowanie do połączeń HTTP/1. x i http/2.</span><span class="sxs-lookup"><span data-stu-id="61976-491">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="61976-492">Limit czasu nagłówków żądań</span><span class="sxs-lookup"><span data-stu-id="61976-492">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="61976-493">Pobiera lub ustawia maksymalny czas, przez jaki serwer spędza nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="61976-493">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="61976-494">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="61976-494">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="61976-495">Maksymalna liczba strumieni na połączenie</span><span class="sxs-lookup"><span data-stu-id="61976-495">Maximum streams per connection</span></span>

<span data-ttu-id="61976-496">`Http2.MaxStreamsPerConnection`ogranicza liczbę współbieżnych strumieni żądań na połączenie HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="61976-496">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="61976-497">Odrzucone strumienie są nadmiarowe.</span><span class="sxs-lookup"><span data-stu-id="61976-497">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="61976-498">Wartość domyślna to 100.</span><span class="sxs-lookup"><span data-stu-id="61976-498">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="61976-499">Rozmiar tabeli nagłówka</span><span class="sxs-lookup"><span data-stu-id="61976-499">Header table size</span></span>

<span data-ttu-id="61976-500">Dekoder HPACK dekompresuje nagłówki HTTP dla połączeń HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="61976-500">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="61976-501">`Http2.HeaderTableSize`ogranicza rozmiar tabeli kompresji nagłówka używanej przez dekoder HPACK.</span><span class="sxs-lookup"><span data-stu-id="61976-501">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="61976-502">Wartość jest podawana w oktetach i musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="61976-502">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="61976-503">Wartość domyślna to 4096.</span><span class="sxs-lookup"><span data-stu-id="61976-503">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="61976-504">Maksymalny rozmiar ramki</span><span class="sxs-lookup"><span data-stu-id="61976-504">Maximum frame size</span></span>

<span data-ttu-id="61976-505">`Http2.MaxFrameSize`wskazuje maksymalny rozmiar ładunku ramki połączenia HTTP/2 do odebrania.</span><span class="sxs-lookup"><span data-stu-id="61976-505">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="61976-506">Wartość jest podawana w oktetach i musi należeć do przedziału od 2 ^ 14 (16 384) do 2 ^ 24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="61976-506">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="61976-507">Wartość domyślna to 2 ^ 14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="61976-507">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="61976-508">Maksymalny rozmiar nagłówka żądania</span><span class="sxs-lookup"><span data-stu-id="61976-508">Maximum request header size</span></span>

<span data-ttu-id="61976-509">`Http2.MaxRequestHeaderFieldSize`wskazuje maksymalny dozwolony rozmiar w oktetach wartości nagłówka żądania.</span><span class="sxs-lookup"><span data-stu-id="61976-509">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="61976-510">Ten limit dotyczy zarówno nazwy, jak i wartości razem w skompresowanych i nieskompresowanych reprezentacji.</span><span class="sxs-lookup"><span data-stu-id="61976-510">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="61976-511">Wartość musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="61976-511">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="61976-512">Wartość domyślna to 8 192.</span><span class="sxs-lookup"><span data-stu-id="61976-512">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="61976-513">Początkowy rozmiar okna połączenia</span><span class="sxs-lookup"><span data-stu-id="61976-513">Initial connection window size</span></span>

<span data-ttu-id="61976-514">`Http2.InitialConnectionWindowSize`wskazuje maksymalne dane dotyczące treści żądania w bajtach, które są agregowane w buforach serwera w ramach wszystkich żądań (strumieni) na połączenie.</span><span class="sxs-lookup"><span data-stu-id="61976-514">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="61976-515">Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="61976-515">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="61976-516">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="61976-516">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="61976-517">Wartość domyślna to 128 KB (131 072).</span><span class="sxs-lookup"><span data-stu-id="61976-517">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="61976-518">Rozmiar początkowego okna strumienia</span><span class="sxs-lookup"><span data-stu-id="61976-518">Initial stream window size</span></span>

<span data-ttu-id="61976-519">`Http2.InitialStreamWindowSize`wskazuje maksymalne dane dotyczące treści żądania w bajtach bufory serwera w ramach żądania (Stream).</span><span class="sxs-lookup"><span data-stu-id="61976-519">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="61976-520">Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="61976-520">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="61976-521">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="61976-521">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="61976-522">Wartość domyślna to 96 KB (98 304).</span><span class="sxs-lookup"><span data-stu-id="61976-522">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="61976-523">Synchroniczne we/wy</span><span class="sxs-lookup"><span data-stu-id="61976-523">Synchronous IO</span></span>

<span data-ttu-id="61976-524"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>Określa, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="61976-524"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="61976-525">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="61976-525">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="61976-526">Duża liczba blokowania synchronicznych operacji we/wy może prowadzić do zablokowania puli wątków, co sprawia, że aplikacja nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="61976-526">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="61976-527">Włączaj `AllowSynchronousIO` tylko w przypadku korzystania z biblioteki, która nie obsługuje asynchronicznej operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="61976-527">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="61976-528">Poniższy przykład włącza synchroniczną operację we/wy:</span><span class="sxs-lookup"><span data-stu-id="61976-528">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="61976-529">Aby uzyskać informacje o innych opcjach i ograniczeniach Kestrel, zobacz:</span><span class="sxs-lookup"><span data-stu-id="61976-529">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="61976-530">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="61976-530">Endpoint configuration</span></span>

<span data-ttu-id="61976-531">Domyślnie ASP.NET Core wiąże się z:</span><span class="sxs-lookup"><span data-stu-id="61976-531">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="61976-532">`https://localhost:5001`(gdy obecny jest lokalny certyfikat programistyczny)</span><span class="sxs-lookup"><span data-stu-id="61976-532">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="61976-533">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="61976-533">Specify URLs using the:</span></span>

* <span data-ttu-id="61976-534">`ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="61976-534">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="61976-535">`--urls`argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="61976-535">`--urls` command-line argument.</span></span>
* <span data-ttu-id="61976-536">`urls`klucz konfiguracji hosta.</span><span class="sxs-lookup"><span data-stu-id="61976-536">`urls` host configuration key.</span></span>
* <span data-ttu-id="61976-537">`UseUrls`Metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="61976-537">`UseUrls` extension method.</span></span>

<span data-ttu-id="61976-538">Wartość podana przy użyciu tych metod może być jednym lub większą liczbą punktów końcowych HTTP i HTTPS (HTTPS, jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="61976-538">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="61976-539">Skonfiguruj wartość jako listę rozdzieloną średnikami (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="61976-539">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="61976-540">Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwera](xref:fundamentals/host/web-host#server-urls) i [Zastąp konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="61976-540">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="61976-541">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="61976-541">A development certificate is created:</span></span>

* <span data-ttu-id="61976-542">Po zainstalowaniu [zestaw .NET Core SDK](/dotnet/core/sdk) .</span><span class="sxs-lookup"><span data-stu-id="61976-542">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="61976-543">Do utworzenia certyfikatu służy [Narzędzie dev-certs](xref:aspnetcore-2.1#https) .</span><span class="sxs-lookup"><span data-stu-id="61976-543">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="61976-544">Niektóre przeglądarki wymagają przyznania jawnego uprawnienia do zaufania do lokalnego certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="61976-544">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="61976-545">Szablony projektu konfigurują aplikacje do uruchamiania domyślnie przy użyciu protokołu HTTPS i obejmują [przekierowania https i obsługę HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="61976-545">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="61976-546">Wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> metody w<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> celu skonfigurowania prefiksów i portów adresów URL dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-546">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="61976-547">`UseUrls`, argument wiersza `urls` `ASPNETCORE_URLS` polecenia, klucz konfiguracji hosta i zmienna środowiskowa również działają, ale mają ograniczenia wymienione w dalszej części tej sekcji (certyfikat domyślny musi być dostępny dla punktu końcowego HTTPS `--urls` Konfiguracja).</span><span class="sxs-lookup"><span data-stu-id="61976-547">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="61976-548">`KestrelServerOptions`skonfigurować</span><span class="sxs-lookup"><span data-stu-id="61976-548">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="61976-549">ConfigureEndpointDefaults (Akcja\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="61976-549">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="61976-550">Określa konfigurację `Action` do uruchomienia dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="61976-550">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="61976-551">Wywołanie `ConfigureEndpointDefaults` wielokrotne zastępuje przed `Action`ostatnimi `Action` parametrami.</span><span class="sxs-lookup"><span data-stu-id="61976-551">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="61976-552">ConfigureHttpsDefaults (Akcja\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="61976-552">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="61976-553">Określa konfigurację `Action` do uruchomienia dla każdego punktu końcowego HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61976-553">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="61976-554">Wywołanie `ConfigureHttpsDefaults` wielokrotne zastępuje przed `Action`ostatnimi `Action` parametrami.</span><span class="sxs-lookup"><span data-stu-id="61976-554">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="61976-555">Konfiguruj (IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="61976-555">Configure(IConfiguration)</span></span>

<span data-ttu-id="61976-556">Tworzy moduł ładujący konfigurację na potrzeby konfigurowania Kestrel, które <xref:Microsoft.Extensions.Configuration.IConfiguration> pobiera jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="61976-556">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="61976-557">Konfiguracja musi być objęta zakresem sekcji konfiguracji dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-557">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="61976-558">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="61976-558">ListenOptions.UseHttps</span></span>

<span data-ttu-id="61976-559">Skonfiguruj Kestrel do korzystania z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61976-559">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="61976-560">`ListenOptions.UseHttps`rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="61976-560">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="61976-561">`UseHttps`&ndash; Skonfiguruj Kestrel do używania protokołu HTTPS z domyślnym certyfikatem.</span><span class="sxs-lookup"><span data-stu-id="61976-561">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="61976-562">Zgłasza wyjątek, jeśli nie został skonfigurowany żaden certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="61976-562">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="61976-563">`ListenOptions.UseHttps`wejściowe</span><span class="sxs-lookup"><span data-stu-id="61976-563">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="61976-564">`filename`jest ścieżką i nazwą pliku certyfikatu względem katalogu zawierającego pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="61976-564">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="61976-565">`password`to hasło wymagane do uzyskania dostępu do danych certyfikatu X. 509.</span><span class="sxs-lookup"><span data-stu-id="61976-565">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="61976-566">`configureOptions``Action` jest do skonfigurowania `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="61976-566">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="61976-567">Zwraca wartość `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="61976-567">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="61976-568">`storeName`to magazyn certyfikatów, z którego ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="61976-568">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="61976-569">`subject`to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="61976-569">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="61976-570">`allowInvalid`wskazuje, czy należy wziąć pod uwagę nieprawidłowe certyfikaty, takie jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="61976-570">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="61976-571">`location`jest lokalizacją magazynu, z której ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="61976-571">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="61976-572">`serverCertificate`jest certyfikatem X. 509.</span><span class="sxs-lookup"><span data-stu-id="61976-572">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="61976-573">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61976-573">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="61976-574">Należy podać co najmniej certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="61976-574">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="61976-575">Obsługiwane konfiguracje opisane dalej:</span><span class="sxs-lookup"><span data-stu-id="61976-575">Supported configurations described next:</span></span>

* <span data-ttu-id="61976-576">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="61976-576">No configuration</span></span>
* <span data-ttu-id="61976-577">Zastąp domyślny certyfikat z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="61976-577">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="61976-578">Zmień wartości domyślne w kodzie</span><span class="sxs-lookup"><span data-stu-id="61976-578">Change the defaults in code</span></span>

<span data-ttu-id="61976-579">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="61976-579">*No configuration*</span></span>

<span data-ttu-id="61976-580">Kestrel nasłuchuje `http://localhost:5000` w `https://localhost:5001` systemie i (Jeśli domyślny certyfikat jest dostępny).</span><span class="sxs-lookup"><span data-stu-id="61976-580">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="61976-581">*Zastąp domyślny certyfikat z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="61976-581">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="61976-582">`CreateDefaultBuilder`wywołania `Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-582">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="61976-583">Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępny dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-583">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="61976-584">Konfigurowanie wielu punktów końcowych, w tym adresów URL i certyfikatów do użycia, z pliku znajdującego się na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="61976-584">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="61976-585">W poniższym przykładzie pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="61976-585">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="61976-586">Ustaw **AllowInvalid** na `true` , aby zezwolić na korzystanie z nieprawidłowych certyfikatów (na przykład certyfikatów z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="61976-586">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="61976-587">Wszystkie punkty końcowe HTTPS, które nie określają certyfikatu (**HttpsDefaultCert** w poniższym przykładzie) powracają do certyfikatu zdefiniowanego w obszarze **Certyfikaty** > **domyślne** lub certyfikat programistyczny.</span><span class="sxs-lookup"><span data-stu-id="61976-587">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="61976-588">Alternatywą dla korzystania z **ścieżki** i **hasła** dla dowolnego węzła certyfikatu jest określenie certyfikatu przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="61976-588">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="61976-589">Certyfikat**domyślny** można na > przykład określić jako:</span><span class="sxs-lookup"><span data-stu-id="61976-589">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="61976-590">Uwagi dotyczące schematu:</span><span class="sxs-lookup"><span data-stu-id="61976-590">Schema notes:</span></span>

* <span data-ttu-id="61976-591">W nazwach punktów końcowych nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="61976-591">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="61976-592">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="61976-592">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="61976-593">`Url` Parametr jest wymagany dla każdego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="61976-593">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="61976-594">Format tego parametru jest taki sam jak parametr konfiguracji najwyższego poziomu `Urls` , z tą różnicą, że jest ograniczony do pojedynczej wartości.</span><span class="sxs-lookup"><span data-stu-id="61976-594">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="61976-595">Te punkty końcowe zastępują te zdefiniowane w konfiguracji najwyższego poziomu `Urls` zamiast dodawać je do nich.</span><span class="sxs-lookup"><span data-stu-id="61976-595">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="61976-596">Punkty końcowe zdefiniowane w kodzie `Listen` za pośrednictwem łączą się z punktami końcowymi zdefiniowanymi w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="61976-596">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="61976-597">`Certificate` Sekcja jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="61976-597">The `Certificate` section is optional.</span></span> <span data-ttu-id="61976-598">`Certificate` Jeśli sekcja nie jest określona, używane są wartości domyślne zdefiniowane we wcześniejszych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="61976-598">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="61976-599">Jeśli żadne wartości domyślne nie są dostępne, serwer zgłasza wyjątek i nie może się uruchomić.</span><span class="sxs-lookup"><span data-stu-id="61976-599">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="61976-600">&ndash;&ndash;Sekcja obsługuje zarówno hasło ścieżki, jak i certyfikaty magazynu podmiotu. `Certificate`</span><span class="sxs-lookup"><span data-stu-id="61976-600">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="61976-601">W ten sposób można zdefiniować dowolną liczbę punktów końcowych, dopóki nie spowoduje to konfliktów portów.</span><span class="sxs-lookup"><span data-stu-id="61976-601">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="61976-602">`options.Configure(context.Configuration.GetSection("{SECTION}"))``KestrelConfigurationLoader` zwracametodę,któramożesłużyćdouzupełnianiaskonfigurowanych`.Endpoint(string name, listenOptions => { })` ustawień punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="61976-602">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="61976-603">`KestrelServerOptions.ConfigurationLoader`można uzyskać bezpośredni dostęp do kontynuowania iteracji w istniejącym module ładującym, takim jak dostarczony <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>przez.</span><span class="sxs-lookup"><span data-stu-id="61976-603">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="61976-604">Sekcja konfiguracji dla każdego punktu końcowego jest dostępna w opcjach w `Endpoint` metodzie, aby można było odczytać ustawienia niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="61976-604">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="61976-605">Można załadować wiele konfiguracji, wywołując `options.Configure(context.Configuration.GetSection("{SECTION}"))` ponownie z inną sekcją.</span><span class="sxs-lookup"><span data-stu-id="61976-605">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="61976-606">Używana jest tylko Ostatnia konfiguracja, chyba że `Load` jest jawnie wywoływana w poprzednich wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="61976-606">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="61976-607">Pakiet nie wywołuje metody `Load` , aby można było zastąpić sekcję konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="61976-607">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="61976-608">`KestrelConfigurationLoader`odzwierciedla rodzinę interfejsów API z `KestrelServerOptions` programu `Endpoint` jako przeciążenia, dlatego punkty końcowe kodu i konfiguracji można skonfigurować w tym samym miejscu. `Listen`</span><span class="sxs-lookup"><span data-stu-id="61976-608">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="61976-609">Te przeciążenia nie używają nazw i używają tylko ustawień domyślnych z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="61976-609">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="61976-610">*Zmień wartości domyślne w kodzie*</span><span class="sxs-lookup"><span data-stu-id="61976-610">*Change the defaults in code*</span></span>

<span data-ttu-id="61976-611">`ConfigureEndpointDefaults`i `ConfigureHttpsDefaults` może służyć do zmiany ustawień domyślnych dla `ListenOptions` i `HttpsConnectionAdapterOptions`, w tym przesłanianie domyślnego certyfikatu określonego w poprzednim scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="61976-611">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="61976-612">`ConfigureEndpointDefaults`i `ConfigureHttpsDefaults` powinny być wywoływane przed skonfigurowaniem punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="61976-612">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="61976-613">*Obsługa usługi Kestrel dla SNI*</span><span class="sxs-lookup"><span data-stu-id="61976-613">*Kestrel support for SNI*</span></span>

<span data-ttu-id="61976-614">[Oznaczanie nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do hostowania wielu domen na tym samym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="61976-614">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="61976-615">Aby SNI działał, klient wysyła nazwę hosta dla bezpiecznej sesji do serwera podczas uzgadniania TLS, aby serwer mógł zapewnić prawidłowy certyfikat.</span><span class="sxs-lookup"><span data-stu-id="61976-615">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="61976-616">Klient korzysta z dostarczonego certyfikatu w celu zaszyfrowania komunikacji z serwerem podczas bezpiecznej sesji, która następuje po uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="61976-616">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="61976-617">Kestrel obsługuje SNI za pośrednictwem `ServerCertificateSelector` wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="61976-617">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="61976-618">Wywołanie zwrotne jest wywoływane jednokrotnie dla każdego połączenia, aby umożliwić aplikacji inspekcję nazwy hosta i wybieranie odpowiedniego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="61976-618">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="61976-619">Obsługa SNI wymaga:</span><span class="sxs-lookup"><span data-stu-id="61976-619">SNI support requires:</span></span>

* <span data-ttu-id="61976-620">Uruchomiona w środowisku `netcoreapp2.1` docelowym lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="61976-620">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="61976-621">W `net461` dniu lub później wywołanie zwrotne jest wywoływane, `name` ale jest `null`zawsze.</span><span class="sxs-lookup"><span data-stu-id="61976-621">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="61976-622">Jest `name` również`null` , jeśli klient nie poda parametru nazwy hosta w uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="61976-622">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="61976-623">Wszystkie witryny sieci Web działają na tym samym wystąpieniu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-623">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="61976-624">Kestrel nie obsługuje udostępniania adresu IP i portu w wielu wystąpieniach bez zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="61976-624">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
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

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="61976-625">Powiąż z gniazdem TCP</span><span class="sxs-lookup"><span data-stu-id="61976-625">Bind to a TCP socket</span></span>

<span data-ttu-id="61976-626"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> Metoda wiąże się z gniazdem TCP, a opcja lambda zezwala na konfigurację certyfikatu X. 509:</span><span class="sxs-lookup"><span data-stu-id="61976-626">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="61976-627">Przykład konfiguruje HTTPS dla punktu końcowego za <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>pomocą.</span><span class="sxs-lookup"><span data-stu-id="61976-627">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="61976-628">Użyj tego samego interfejsu API, aby skonfigurować inne ustawienia Kestrel dla określonych punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="61976-628">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="61976-629">Powiąż z gniazdem systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="61976-629">Bind to a Unix socket</span></span>

<span data-ttu-id="61976-630">Nasłuchiwanie w gnieździe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> systemu UNIX za pomocą programu w celu zwiększenia wydajności za pomocą usługi Nginx, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="61976-630">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="61976-631">Port 0</span><span class="sxs-lookup"><span data-stu-id="61976-631">Port 0</span></span>

<span data-ttu-id="61976-632">Gdy numer `0` portu jest określony, Kestrel dynamicznie wiąże się z dostępnym portem.</span><span class="sxs-lookup"><span data-stu-id="61976-632">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="61976-633">W poniższym przykładzie pokazano, jak określić, który port Kestrel faktycznie powiązany w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="61976-633">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="61976-634">Po uruchomieniu aplikacji dane wyjściowe okna konsoli wskazują port dynamiczny, w którym można uzyskać dostęp do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="61976-634">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="61976-635">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="61976-635">Limitations</span></span>

<span data-ttu-id="61976-636">Skonfiguruj punkty końcowe przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="61976-636">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="61976-637">`--urls`argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="61976-637">`--urls` command-line argument</span></span>
* <span data-ttu-id="61976-638">`urls`klucz konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="61976-638">`urls` host configuration key</span></span>
* <span data-ttu-id="61976-639">`ASPNETCORE_URLS`Zmienna środowiskowa</span><span class="sxs-lookup"><span data-stu-id="61976-639">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="61976-640">Te metody są przydatne do tworzenia kodu w pracy z serwerami innymi niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-640">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="61976-641">Należy jednak pamiętać o następujących ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="61976-641">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="61976-642">Nie można użyć protokołu HTTPS z tymi metodami, chyba że w konfiguracji punktu końcowego HTTPS jest podany certyfikat domyślny (na `KestrelServerOptions` przykład przy użyciu konfiguracji lub pliku konfiguracji, jak pokazano wcześniej w tym temacie).</span><span class="sxs-lookup"><span data-stu-id="61976-642">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="61976-643">Gdy oba `Listen` podejścia i `UseUrls` `UseUrls` są używane jednocześnie, punktykońcowezastępująpunktykońcowe.`Listen`</span><span class="sxs-lookup"><span data-stu-id="61976-643">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="61976-644">Konfiguracja punktu końcowego usług IIS</span><span class="sxs-lookup"><span data-stu-id="61976-644">IIS endpoint configuration</span></span>

<span data-ttu-id="61976-645">W przypadku korzystania z usług IIS powiązania URL dla powiązań przesłonięć usług IIS są ustawiane `UseUrls`przez `Listen` lub.</span><span class="sxs-lookup"><span data-stu-id="61976-645">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="61976-646">Aby uzyskać więcej informacji, zobacz temat [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="61976-646">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="61976-647">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="61976-647">ListenOptions.Protocols</span></span>

<span data-ttu-id="61976-648">Właściwość ustanawia protokoły HTTP (`HttpProtocols`) włączone w punkcie końcowym połączenia lub dla serwera. `Protocols`</span><span class="sxs-lookup"><span data-stu-id="61976-648">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="61976-649">Przypisz wartość do `Protocols` właściwości `HttpProtocols` z wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="61976-649">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="61976-650">`HttpProtocols`wartość wyliczenia</span><span class="sxs-lookup"><span data-stu-id="61976-650">`HttpProtocols` enum value</span></span> | <span data-ttu-id="61976-651">Dozwolony protokół połączenia</span><span class="sxs-lookup"><span data-stu-id="61976-651">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="61976-652">Tylko HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="61976-652">HTTP/1.1 only.</span></span> <span data-ttu-id="61976-653">Może być używany z protokołem TLS lub bez niego.</span><span class="sxs-lookup"><span data-stu-id="61976-653">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="61976-654">Tylko HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="61976-654">HTTP/2 only.</span></span> <span data-ttu-id="61976-655">Mogą być używane bez protokołu TLS tylko wtedy, gdy klient obsługuje [poprzedni tryb wiedzy](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="61976-655">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="61976-656">HTTP/1.1 i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="61976-656">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="61976-657">Protokół HTTP/2 wymaga połączenia protokołów TLS i [warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3) . w przeciwnym razie wartość domyślna połączenia to HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="61976-657">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="61976-658">Domyślnym protokołem jest HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="61976-658">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="61976-659">Ograniczenia protokołu TLS dla protokołu HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="61976-659">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="61976-660">TLS w wersji 1,2 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="61976-660">TLS version 1.2 or later</span></span>
* <span data-ttu-id="61976-661">Ponowne negocjowanie wyłączone</span><span class="sxs-lookup"><span data-stu-id="61976-661">Renegotiation disabled</span></span>
* <span data-ttu-id="61976-662">Kompresja wyłączona</span><span class="sxs-lookup"><span data-stu-id="61976-662">Compression disabled</span></span>
* <span data-ttu-id="61976-663">Minimalne rozmiary tymczasowych kluczy wymiany:</span><span class="sxs-lookup"><span data-stu-id="61976-663">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="61976-664">Krzywa eliptyczna Diffie-Hellmana (ECDHE &lbrack;) [RFC4492](https://www.ietf.org/rfc/rfc4492.txt) &rbrack; &ndash; 224 (minimum)</span><span class="sxs-lookup"><span data-stu-id="61976-664">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="61976-665">Ograniczone pole Diffie-Hellmana (DHE) &lbrack; `TLS12` &rbrack; &ndash; 2048 bity minimum</span><span class="sxs-lookup"><span data-stu-id="61976-665">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="61976-666">Mechanizm szyfrowania nie został zabroniony</span><span class="sxs-lookup"><span data-stu-id="61976-666">Cipher suite not blacklisted</span></span>

<span data-ttu-id="61976-667">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`&lbrack;zkrzywą eliptycznaP&lbrack; -256 jest domyślnieobsługiwana.`FIPS186` `TLS-ECDHE` &rbrack; &rbrack;</span><span class="sxs-lookup"><span data-stu-id="61976-667">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="61976-668">Poniższy przykład umożliwia nawiązywanie połączeń HTTP/1.1 i HTTP/2 na porcie 8000.</span><span class="sxs-lookup"><span data-stu-id="61976-668">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="61976-669">Połączenia są zabezpieczone przez protokół TLS przy użyciu podanego certyfikatu:</span><span class="sxs-lookup"><span data-stu-id="61976-669">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="61976-670">Opcjonalnie można utworzyć `IConnectionAdapter` implementację do filtrowania uzgadniania protokołu TLS dla poszczególnych połączeń dla określonych szyfrów:</span><span class="sxs-lookup"><span data-stu-id="61976-670">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
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

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
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

<span data-ttu-id="61976-671">*Ustawianie protokołu z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="61976-671">*Set the protocol from configuration*</span></span>

<span data-ttu-id="61976-672"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-672"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="61976-673">W poniższym przykładzie pliku *appSettings. JSON* jest ustanawiany domyślny protokół połączeń (http/1.1 i http/2) dla wszystkich punktów końcowych Kestrel:</span><span class="sxs-lookup"><span data-stu-id="61976-673">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="61976-674">Poniższy przykład pliku konfiguracji ustanawia protokół połączenia dla określonego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="61976-674">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="61976-675">Protokoły określone w wartościach zastąpienia kodu ustawione przez konfigurację.</span><span class="sxs-lookup"><span data-stu-id="61976-675">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="61976-676">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="61976-676">Transport configuration</span></span>

<span data-ttu-id="61976-677">W wersji platformy ASP.NET Core 2.1 transport domyślny serwera Kestrel nie jest już oparty na bibliotece libuv, ale na zarządzanych gniazdach.</span><span class="sxs-lookup"><span data-stu-id="61976-677">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="61976-678">Jest to istotna zmiana dla aplikacji ASP.NET Core 2,0 uaktualniana do 2,1, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> które wywołują i zależą od jednego z następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="61976-678">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="61976-679">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (bezpośrednie odwołanie do pakietu)</span><span class="sxs-lookup"><span data-stu-id="61976-679">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="61976-680">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="61976-680">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="61976-681">Dla projektów, które wymagają użycia Libuv:</span><span class="sxs-lookup"><span data-stu-id="61976-681">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="61976-682">Dodaj zależność dla pakietu [Microsoft. AspNetCore. Server. Kestrel. transport. Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="61976-682">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="61976-683">Wywołanie <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="61976-683">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="61976-684">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="61976-684">URL prefixes</span></span>

<span data-ttu-id="61976-685">W przypadku `UseUrls`użycia `--urls` , argumentu wiersza polecenia, `urls` klucza konfiguracji hosta lub `ASPNETCORE_URLS` zmiennej środowiskowej prefiksy adresów URL mogą znajdować się w jednym z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="61976-685">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="61976-686">Tylko prefiksy adresów URL HTTP są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="61976-686">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="61976-687">Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązań adresów `UseUrls`URL przy użyciu programu.</span><span class="sxs-lookup"><span data-stu-id="61976-687">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="61976-688">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="61976-688">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="61976-689">`0.0.0.0`jest szczególnym przypadkiem, który wiąże się ze wszystkimi adresami IPv4.</span><span class="sxs-lookup"><span data-stu-id="61976-689">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="61976-690">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="61976-690">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="61976-691">`[::]`jest odpowiednikiem IPv6 dla protokołu `0.0.0.0`IPv4.</span><span class="sxs-lookup"><span data-stu-id="61976-691">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="61976-692">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="61976-692">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="61976-693">Nazwy `*`hostów, i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="61976-693">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="61976-694">Wszystkie nie są rozpoznawane jako prawidłowy adres IP `localhost` lub są powiązane ze wszystkimi IP IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="61976-694">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="61976-695">Aby powiązać różne nazwy hostów z różnymi ASP.NET Core aplikacjami na tym samym porcie, użyj [protokołu HTTP. sys](xref:fundamentals/servers/httpsys) lub odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="61976-695">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="61976-696">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="61976-696">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="61976-697">Nazwa `localhost` hosta z numerem portu lub adresem IP sprzężenia zwrotnego z numerem portu</span><span class="sxs-lookup"><span data-stu-id="61976-697">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="61976-698">Gdy `localhost` jest określony, Kestrel próbuje powiązać z interfejsami sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="61976-698">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="61976-699">Jeśli żądany port jest używany przez inną usługę w dowolnym interfejsie sprzężenia zwrotnego, uruchomienie Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="61976-699">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="61976-700">Jeśli którykolwiek z innych przyczyn interfejsu sprzężenia zwrotnego jest niedostępny (najczęściej jest to spowodowane tym, że protokół IPv6 nie jest obsługiwany), Kestrel rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="61976-700">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="61976-701">Filtrowanie hostów</span><span class="sxs-lookup"><span data-stu-id="61976-701">Host filtering</span></span>

<span data-ttu-id="61976-702">Program Kestrel obsługuje konfigurację na podstawie prefiksów, takich `http://example.com:5000`jak, Kestrel w znacznym stopniu ignoruje nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="61976-702">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="61976-703">Host `localhost` jest specjalnym przypadkiem używanym do tworzenia powiązań z adresami sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="61976-703">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="61976-704">Każdy host poza jawnym adresem IP tworzy powiązanie ze wszystkimi publicznymi adresami IP.</span><span class="sxs-lookup"><span data-stu-id="61976-704">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="61976-705">`Host`nagłówki nie są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="61976-705">`Host` headers aren't validated.</span></span>

<span data-ttu-id="61976-706">Aby obejść ten sposób, użyj oprogramowania pośredniczącego filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="61976-706">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="61976-707">Oprogramowanie pośredniczące do filtrowania hosta jest dostarczane przez pakiet [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , który jest zawarty w pakiecie [Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub 2,2).</span><span class="sxs-lookup"><span data-stu-id="61976-707">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="61976-708">Oprogramowanie pośredniczące jest dodawane przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>program, który <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>wywołuje następujące wywołania:</span><span class="sxs-lookup"><span data-stu-id="61976-708">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="61976-709">Programowe filtrowanie hosta jest domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="61976-709">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="61976-710">Aby włączyć oprogramowanie pośredniczące, zdefiniuj klucz `AllowedHosts` w pliku *appSettings. JSON*/ *.\< EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="61976-710">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="61976-711">Wartość to rozdzielana średnikami lista nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="61976-711">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="61976-712">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="61976-712">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="61976-713">[Przekierowane nagłówki oprogramowania](xref:host-and-deploy/proxy-load-balancer) również mają <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> opcję.</span><span class="sxs-lookup"><span data-stu-id="61976-713">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="61976-714">Przekazane nagłówki — oprogramowanie pośredniczące i filtrowanie hostów oprogramowanie pośredniczące ma podobną funkcjonalność dla różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="61976-714">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="61976-715">Ustawienie `AllowedHosts` z przekierowanymi nagłówkami — oprogramowanie pośredniczące jest odpowiednie, `Host` gdy nagłówek nie jest zachowywany podczas przekazywania żądań z odwrotnym serwerem proxy lub modułem równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="61976-715">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="61976-716">Ustawienie `AllowedHosts` przy użyciu oprogramowania pośredniczącego filtrowania hosta jest odpowiednie, gdy Kestrel jest używany jako publiczny serwer graniczny lub `Host` gdy nagłówek jest bezpośrednio przekazywany.</span><span class="sxs-lookup"><span data-stu-id="61976-716">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="61976-717">Aby uzyskać więcej informacji na temat przekierowanych nagłówków <xref:host-and-deploy/proxy-load-balancer>, należy zapoznać się z tematem.</span><span class="sxs-lookup"><span data-stu-id="61976-717">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="61976-718">Kestrel to Międzyplatformowy [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="61976-718">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="61976-719">Kestrel to serwer sieci Web, który jest domyślnie uwzględniony w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61976-719">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="61976-720">Kestrel obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="61976-720">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="61976-721">HTTPS</span><span class="sxs-lookup"><span data-stu-id="61976-721">HTTPS</span></span>
* <span data-ttu-id="61976-722">Nieprzezroczyste uaktualnienie używane do włączania obsługi obiektów [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="61976-722">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="61976-723">Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx</span><span class="sxs-lookup"><span data-stu-id="61976-723">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="61976-724">Kestrel jest obsługiwana na wszystkich platformach i wersjach obsługiwanych przez platformę .NET Core.</span><span class="sxs-lookup"><span data-stu-id="61976-724">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="61976-725">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="61976-725">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="61976-726">Kiedy używać Kestrel z zwrotnym serwerem proxy</span><span class="sxs-lookup"><span data-stu-id="61976-726">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="61976-727">Kestrel może być używana przez siebie lub z *odwrotnym serwerem proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org)lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="61976-727">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="61976-728">Odwrotny serwer proxy odbiera żądania HTTP z sieci i przekazuje je do usługi Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-728">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="61976-729">Kestrel używany jako serwer sieci Web z krawędzią (dostępną z Internetu):</span><span class="sxs-lookup"><span data-stu-id="61976-729">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem bez serwera proxy zwrotnego](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="61976-731">Kestrel używany w konfiguracji zwrotnego serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="61976-731">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel komunikuje się pośrednio z Internetem za pomocą odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="61976-733">Konfiguracja, z serwerem proxy odwrotnych lub bez niego, jest obsługiwaną konfiguracją hostingu.</span><span class="sxs-lookup"><span data-stu-id="61976-733">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="61976-734">Kestrel używany jako serwer graniczny bez serwera proxy odwrotnego nie obsługuje udostępniania tego samego adresu IP i portu między wieloma procesami.</span><span class="sxs-lookup"><span data-stu-id="61976-734">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="61976-735">Gdy Kestrel jest skonfigurowany do nasłuchiwania na porcie, Kestrel obsługuje cały ruch dla tego portu bez względu na `Host` nagłówki żądań.</span><span class="sxs-lookup"><span data-stu-id="61976-735">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="61976-736">Zwrotny serwer proxy, który może udostępniać porty, ma możliwość przekazywania żądań do Kestrel na unikatowym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="61976-736">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="61976-737">Nawet jeśli odwrotny serwer proxy nie jest wymagany, może to być dobry wybór przy użyciu odwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="61976-737">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="61976-738">Zwrotny serwer proxy:</span><span class="sxs-lookup"><span data-stu-id="61976-738">A reverse proxy:</span></span>

* <span data-ttu-id="61976-739">Może ograniczać uwidoczniony obszar publicznej powierzchni aplikacji, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="61976-739">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="61976-740">Zapewnienie dodatkowej warstwy konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="61976-740">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="61976-741">Integracja z istniejącą infrastrukturą może być lepsza.</span><span class="sxs-lookup"><span data-stu-id="61976-741">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="61976-742">Uprość Równoważenie obciążenia i konfigurację komunikacji zabezpieczonej (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="61976-742">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="61976-743">Tylko serwer zwrotny proxy wymaga certyfikatu X. 509, a serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="61976-743">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="61976-744">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="61976-744">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="61976-745">Jak używać Kestrel w aplikacjach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61976-745">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="61976-746">Pakiet [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="61976-746">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="61976-747">Szablony projektów ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-747">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="61976-748">W *program.cs*, kod szablonu wywołuje <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, który wywołuje <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> się w tle.</span><span class="sxs-lookup"><span data-stu-id="61976-748">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="61976-749">Aby zapewnić dodatkową konfigurację po wywołaniu `CreateDefaultBuilder`, wywołaj: <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*></span><span class="sxs-lookup"><span data-stu-id="61976-749">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

## <a name="kestrel-options"></a><span data-ttu-id="61976-750">Opcje Kestrel</span><span class="sxs-lookup"><span data-stu-id="61976-750">Kestrel options</span></span>

<span data-ttu-id="61976-751">Serwer sieci Web Kestrel ma opcje konfiguracji ograniczeń, które są szczególnie przydatne w przypadku wdrożeń mających dostęp do Internetu.</span><span class="sxs-lookup"><span data-stu-id="61976-751">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="61976-752">Ustaw ograniczenia <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> właściwości <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> klasy.</span><span class="sxs-lookup"><span data-stu-id="61976-752">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="61976-753">Właściwość przechowuje wystąpienie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>klasy. `Limits`</span><span class="sxs-lookup"><span data-stu-id="61976-753">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="61976-754">W poniższych przykładach użyto <xref:Microsoft.AspNetCore.Server.Kestrel.Core> przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="61976-754">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="61976-755">Limit czasu utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="61976-755">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="61976-756">Pobiera lub ustawia [limit czasu utrzymywania aktywności](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="61976-756">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="61976-757">Wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="61976-757">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="61976-758">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="61976-758">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="61976-759">Maksymalna liczba współbieżnych otwartych połączeń TCP można ustawić dla całej aplikacji przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="61976-759">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="61976-760">Istnieje oddzielny limit połączeń, które zostały uaktualnione z protokołu HTTP lub HTTPS do innego protokołu (na przykład w żądaniu usługi WebSockets).</span><span class="sxs-lookup"><span data-stu-id="61976-760">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="61976-761">Po uaktualnieniu połączenia nie jest ono wliczane do `MaxConcurrentConnections` limitu.</span><span class="sxs-lookup"><span data-stu-id="61976-761">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="61976-762">Maksymalna liczba połączeń jest domyślnie nieograniczona (null).</span><span class="sxs-lookup"><span data-stu-id="61976-762">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="61976-763">Maksymalny rozmiar treści żądania</span><span class="sxs-lookup"><span data-stu-id="61976-763">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="61976-764">Domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="61976-764">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="61976-765">Zalecanym podejściem do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> atrybutu dla metody akcji:</span><span class="sxs-lookup"><span data-stu-id="61976-765">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="61976-766">Oto przykład, który pokazuje, jak skonfigurować ograniczenie dla aplikacji w każdym żądaniu:</span><span class="sxs-lookup"><span data-stu-id="61976-766">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="61976-767">Zastąp ustawienie określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="61976-767">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="61976-768">Wyjątek jest generowany, jeśli aplikacja skonfiguruje limit żądania po rozpoczęciu uruchamiania aplikacji w celu odczytania żądania.</span><span class="sxs-lookup"><span data-stu-id="61976-768">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="61976-769">Istnieje właściwość, która wskazuje, `MaxRequestBodySize` czy właściwość jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit. `IsReadOnly`</span><span class="sxs-lookup"><span data-stu-id="61976-769">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="61976-770">Gdy aplikacja jest uruchamiana [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model) [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru treści żądania Kestrel jest wyłączony, ponieważ program IIS już ustawia limit.</span><span class="sxs-lookup"><span data-stu-id="61976-770">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="61976-771">Minimalna stawka danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="61976-771">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="61976-772">Kestrel sprawdza co sekundę, jeśli dane są odbierane z określoną szybkością w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="61976-772">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="61976-773">Jeśli szybkość spadnie poniżej wartości minimalnej, upłynął limit czasu połączenia. Okres prolongaty to czas, przez który Kestrel nadaje klientowi zwiększenie jego szybkości wysyłania do minimum; Ta częstotliwość nie jest sprawdzana w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="61976-773">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="61976-774">Okres prolongaty pozwala uniknąć porzucania połączeń, które początkowo wysyłają dane z niską szybkością.</span><span class="sxs-lookup"><span data-stu-id="61976-774">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="61976-775">Domyślna stawka minimalna to 240 bajtów na sekundę z 5-sekundowym okresem prolongaty.</span><span class="sxs-lookup"><span data-stu-id="61976-775">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="61976-776">Minimalna stawka dotyczy także odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="61976-776">A minimum rate also applies to the response.</span></span> <span data-ttu-id="61976-777">Kod określający limit żądań i limit odpowiedzi jest taki sam, z wyjątkiem `RequestBody` `Response` właściwości i nazwy interfejsów.</span><span class="sxs-lookup"><span data-stu-id="61976-777">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="61976-778">Oto przykład pokazujący sposób konfigurowania minimalnych stawek danych w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="61976-778">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            serverOptions.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

### <a name="request-headers-timeout"></a><span data-ttu-id="61976-779">Limit czasu nagłówków żądań</span><span class="sxs-lookup"><span data-stu-id="61976-779">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="61976-780">Pobiera lub ustawia maksymalny czas, przez jaki serwer spędza nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="61976-780">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="61976-781">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="61976-781">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="61976-782">Synchroniczne we/wy</span><span class="sxs-lookup"><span data-stu-id="61976-782">Synchronous IO</span></span>

<span data-ttu-id="61976-783"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>Określa, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="61976-783"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="61976-784">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="61976-784">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="61976-785">Duża liczba blokowania synchronicznych operacji we/wy może prowadzić do zablokowania puli wątków, co sprawia, że aplikacja nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="61976-785">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="61976-786">Włączaj `AllowSynchronousIO` tylko w przypadku korzystania z biblioteki, która nie obsługuje asynchronicznej operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="61976-786">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="61976-787">Poniższy przykład wyłącza synchroniczną operację we/wy:</span><span class="sxs-lookup"><span data-stu-id="61976-787">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="61976-788">Aby uzyskać informacje o innych opcjach i ograniczeniach Kestrel, zobacz:</span><span class="sxs-lookup"><span data-stu-id="61976-788">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="61976-789">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="61976-789">Endpoint configuration</span></span>

<span data-ttu-id="61976-790">Domyślnie ASP.NET Core wiąże się z:</span><span class="sxs-lookup"><span data-stu-id="61976-790">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="61976-791">`https://localhost:5001`(gdy obecny jest lokalny certyfikat programistyczny)</span><span class="sxs-lookup"><span data-stu-id="61976-791">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="61976-792">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="61976-792">Specify URLs using the:</span></span>

* <span data-ttu-id="61976-793">`ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="61976-793">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="61976-794">`--urls`argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="61976-794">`--urls` command-line argument.</span></span>
* <span data-ttu-id="61976-795">`urls`klucz konfiguracji hosta.</span><span class="sxs-lookup"><span data-stu-id="61976-795">`urls` host configuration key.</span></span>
* <span data-ttu-id="61976-796">`UseUrls`Metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="61976-796">`UseUrls` extension method.</span></span>

<span data-ttu-id="61976-797">Wartość podana przy użyciu tych metod może być jednym lub większą liczbą punktów końcowych HTTP i HTTPS (HTTPS, jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="61976-797">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="61976-798">Skonfiguruj wartość jako listę rozdzieloną średnikami (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="61976-798">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="61976-799">Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwera](xref:fundamentals/host/web-host#server-urls) i [Zastąp konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="61976-799">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="61976-800">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="61976-800">A development certificate is created:</span></span>

* <span data-ttu-id="61976-801">Po zainstalowaniu [zestaw .NET Core SDK](/dotnet/core/sdk) .</span><span class="sxs-lookup"><span data-stu-id="61976-801">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="61976-802">Do utworzenia certyfikatu służy [Narzędzie dev-certs](xref:aspnetcore-2.1#https) .</span><span class="sxs-lookup"><span data-stu-id="61976-802">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="61976-803">Niektóre przeglądarki wymagają przyznania jawnego uprawnienia do zaufania do lokalnego certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="61976-803">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="61976-804">Szablony projektu konfigurują aplikacje do uruchamiania domyślnie przy użyciu protokołu HTTPS i obejmują [przekierowania https i obsługę HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="61976-804">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="61976-805">Wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> metody w<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> celu skonfigurowania prefiksów i portów adresów URL dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-805">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="61976-806">`UseUrls`, argument wiersza `urls` `ASPNETCORE_URLS` polecenia, klucz konfiguracji hosta i zmienna środowiskowa również działają, ale mają ograniczenia wymienione w dalszej części tej sekcji (certyfikat domyślny musi być dostępny dla punktu końcowego HTTPS `--urls` Konfiguracja).</span><span class="sxs-lookup"><span data-stu-id="61976-806">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="61976-807">`KestrelServerOptions`skonfigurować</span><span class="sxs-lookup"><span data-stu-id="61976-807">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="61976-808">ConfigureEndpointDefaults (Akcja\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="61976-808">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="61976-809">Określa konfigurację `Action` do uruchomienia dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="61976-809">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="61976-810">Wywołanie `ConfigureEndpointDefaults` wielokrotne zastępuje przed `Action`ostatnimi `Action` parametrami.</span><span class="sxs-lookup"><span data-stu-id="61976-810">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="61976-811">ConfigureHttpsDefaults (Akcja\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="61976-811">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="61976-812">Określa konfigurację `Action` do uruchomienia dla każdego punktu końcowego HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61976-812">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="61976-813">Wywołanie `ConfigureHttpsDefaults` wielokrotne zastępuje przed `Action`ostatnimi `Action` parametrami.</span><span class="sxs-lookup"><span data-stu-id="61976-813">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="61976-814">Konfiguruj (IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="61976-814">Configure(IConfiguration)</span></span>

<span data-ttu-id="61976-815">Tworzy moduł ładujący konfigurację na potrzeby konfigurowania Kestrel, które <xref:Microsoft.Extensions.Configuration.IConfiguration> pobiera jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="61976-815">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="61976-816">Konfiguracja musi być objęta zakresem sekcji konfiguracji dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-816">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="61976-817">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="61976-817">ListenOptions.UseHttps</span></span>

<span data-ttu-id="61976-818">Skonfiguruj Kestrel do korzystania z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61976-818">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="61976-819">`ListenOptions.UseHttps`rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="61976-819">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="61976-820">`UseHttps`&ndash; Skonfiguruj Kestrel do używania protokołu HTTPS z domyślnym certyfikatem.</span><span class="sxs-lookup"><span data-stu-id="61976-820">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="61976-821">Zgłasza wyjątek, jeśli nie został skonfigurowany żaden certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="61976-821">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="61976-822">`ListenOptions.UseHttps`wejściowe</span><span class="sxs-lookup"><span data-stu-id="61976-822">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="61976-823">`filename`jest ścieżką i nazwą pliku certyfikatu względem katalogu zawierającego pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="61976-823">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="61976-824">`password`to hasło wymagane do uzyskania dostępu do danych certyfikatu X. 509.</span><span class="sxs-lookup"><span data-stu-id="61976-824">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="61976-825">`configureOptions``Action` jest do skonfigurowania `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="61976-825">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="61976-826">Zwraca wartość `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="61976-826">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="61976-827">`storeName`to magazyn certyfikatów, z którego ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="61976-827">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="61976-828">`subject`to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="61976-828">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="61976-829">`allowInvalid`wskazuje, czy należy wziąć pod uwagę nieprawidłowe certyfikaty, takie jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="61976-829">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="61976-830">`location`jest lokalizacją magazynu, z której ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="61976-830">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="61976-831">`serverCertificate`jest certyfikatem X. 509.</span><span class="sxs-lookup"><span data-stu-id="61976-831">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="61976-832">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61976-832">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="61976-833">Należy podać co najmniej certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="61976-833">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="61976-834">Obsługiwane konfiguracje opisane dalej:</span><span class="sxs-lookup"><span data-stu-id="61976-834">Supported configurations described next:</span></span>

* <span data-ttu-id="61976-835">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="61976-835">No configuration</span></span>
* <span data-ttu-id="61976-836">Zastąp domyślny certyfikat z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="61976-836">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="61976-837">Zmień wartości domyślne w kodzie</span><span class="sxs-lookup"><span data-stu-id="61976-837">Change the defaults in code</span></span>

<span data-ttu-id="61976-838">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="61976-838">*No configuration*</span></span>

<span data-ttu-id="61976-839">Kestrel nasłuchuje `http://localhost:5000` w `https://localhost:5001` systemie i (Jeśli domyślny certyfikat jest dostępny).</span><span class="sxs-lookup"><span data-stu-id="61976-839">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="61976-840">*Zastąp domyślny certyfikat z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="61976-840">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="61976-841">`CreateDefaultBuilder`wywołania `Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-841">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="61976-842">Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępny dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-842">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="61976-843">Konfigurowanie wielu punktów końcowych, w tym adresów URL i certyfikatów do użycia, z pliku znajdującego się na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="61976-843">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="61976-844">W poniższym przykładzie pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="61976-844">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="61976-845">Ustaw **AllowInvalid** na `true` , aby zezwolić na korzystanie z nieprawidłowych certyfikatów (na przykład certyfikatów z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="61976-845">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="61976-846">Wszystkie punkty końcowe HTTPS, które nie określają certyfikatu (**HttpsDefaultCert** w poniższym przykładzie) powracają do certyfikatu zdefiniowanego w obszarze **Certyfikaty** > **domyślne** lub certyfikat programistyczny.</span><span class="sxs-lookup"><span data-stu-id="61976-846">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="61976-847">Alternatywą dla korzystania z **ścieżki** i **hasła** dla dowolnego węzła certyfikatu jest określenie certyfikatu przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="61976-847">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="61976-848">Certyfikat**domyślny** można na > przykład określić jako:</span><span class="sxs-lookup"><span data-stu-id="61976-848">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="61976-849">Uwagi dotyczące schematu:</span><span class="sxs-lookup"><span data-stu-id="61976-849">Schema notes:</span></span>

* <span data-ttu-id="61976-850">W nazwach punktów końcowych nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="61976-850">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="61976-851">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="61976-851">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="61976-852">`Url` Parametr jest wymagany dla każdego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="61976-852">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="61976-853">Format tego parametru jest taki sam jak parametr konfiguracji najwyższego poziomu `Urls` , z tą różnicą, że jest ograniczony do pojedynczej wartości.</span><span class="sxs-lookup"><span data-stu-id="61976-853">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="61976-854">Te punkty końcowe zastępują te zdefiniowane w konfiguracji najwyższego poziomu `Urls` zamiast dodawać je do nich.</span><span class="sxs-lookup"><span data-stu-id="61976-854">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="61976-855">Punkty końcowe zdefiniowane w kodzie `Listen` za pośrednictwem łączą się z punktami końcowymi zdefiniowanymi w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="61976-855">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="61976-856">`Certificate` Sekcja jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="61976-856">The `Certificate` section is optional.</span></span> <span data-ttu-id="61976-857">`Certificate` Jeśli sekcja nie jest określona, używane są wartości domyślne zdefiniowane we wcześniejszych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="61976-857">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="61976-858">Jeśli żadne wartości domyślne nie są dostępne, serwer zgłasza wyjątek i nie może się uruchomić.</span><span class="sxs-lookup"><span data-stu-id="61976-858">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="61976-859">&ndash;&ndash;Sekcja obsługuje zarówno hasło ścieżki, jak i certyfikaty magazynu podmiotu. `Certificate`</span><span class="sxs-lookup"><span data-stu-id="61976-859">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="61976-860">W ten sposób można zdefiniować dowolną liczbę punktów końcowych, dopóki nie spowoduje to konfliktów portów.</span><span class="sxs-lookup"><span data-stu-id="61976-860">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="61976-861">`options.Configure(context.Configuration.GetSection("{SECTION}"))``KestrelConfigurationLoader` zwracametodę,któramożesłużyćdouzupełnianiaskonfigurowanych`.Endpoint(string name, listenOptions => { })` ustawień punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="61976-861">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="61976-862">`KestrelServerOptions.ConfigurationLoader`można uzyskać bezpośredni dostęp do kontynuowania iteracji w istniejącym module ładującym, takim jak dostarczony <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>przez.</span><span class="sxs-lookup"><span data-stu-id="61976-862">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="61976-863">Sekcja konfiguracji dla każdego punktu końcowego jest dostępna w opcjach w `Endpoint` metodzie, aby można było odczytać ustawienia niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="61976-863">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="61976-864">Można załadować wiele konfiguracji, wywołując `options.Configure(context.Configuration.GetSection("{SECTION}"))` ponownie z inną sekcją.</span><span class="sxs-lookup"><span data-stu-id="61976-864">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="61976-865">Używana jest tylko Ostatnia konfiguracja, chyba że `Load` jest jawnie wywoływana w poprzednich wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="61976-865">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="61976-866">Pakiet nie wywołuje metody `Load` , aby można było zastąpić sekcję konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="61976-866">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="61976-867">`KestrelConfigurationLoader`odzwierciedla rodzinę interfejsów API z `KestrelServerOptions` programu `Endpoint` jako przeciążenia, dlatego punkty końcowe kodu i konfiguracji można skonfigurować w tym samym miejscu. `Listen`</span><span class="sxs-lookup"><span data-stu-id="61976-867">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="61976-868">Te przeciążenia nie używają nazw i używają tylko ustawień domyślnych z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="61976-868">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="61976-869">*Zmień wartości domyślne w kodzie*</span><span class="sxs-lookup"><span data-stu-id="61976-869">*Change the defaults in code*</span></span>

<span data-ttu-id="61976-870">`ConfigureEndpointDefaults`i `ConfigureHttpsDefaults` może służyć do zmiany ustawień domyślnych dla `ListenOptions` i `HttpsConnectionAdapterOptions`, w tym przesłanianie domyślnego certyfikatu określonego w poprzednim scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="61976-870">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="61976-871">`ConfigureEndpointDefaults`i `ConfigureHttpsDefaults` powinny być wywoływane przed skonfigurowaniem punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="61976-871">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="61976-872">*Obsługa usługi Kestrel dla SNI*</span><span class="sxs-lookup"><span data-stu-id="61976-872">*Kestrel support for SNI*</span></span>

<span data-ttu-id="61976-873">[Oznaczanie nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do hostowania wielu domen na tym samym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="61976-873">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="61976-874">Aby SNI działał, klient wysyła nazwę hosta dla bezpiecznej sesji do serwera podczas uzgadniania TLS, aby serwer mógł zapewnić prawidłowy certyfikat.</span><span class="sxs-lookup"><span data-stu-id="61976-874">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="61976-875">Klient korzysta z dostarczonego certyfikatu w celu zaszyfrowania komunikacji z serwerem podczas bezpiecznej sesji, która następuje po uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="61976-875">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="61976-876">Kestrel obsługuje SNI za pośrednictwem `ServerCertificateSelector` wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="61976-876">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="61976-877">Wywołanie zwrotne jest wywoływane jednokrotnie dla każdego połączenia, aby umożliwić aplikacji inspekcję nazwy hosta i wybieranie odpowiedniego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="61976-877">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="61976-878">Obsługa SNI wymaga:</span><span class="sxs-lookup"><span data-stu-id="61976-878">SNI support requires:</span></span>

* <span data-ttu-id="61976-879">Uruchomiona w środowisku `netcoreapp2.1` docelowym lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="61976-879">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="61976-880">W `net461` dniu lub później wywołanie zwrotne jest wywoływane, `name` ale jest `null`zawsze.</span><span class="sxs-lookup"><span data-stu-id="61976-880">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="61976-881">Jest `name` również`null` , jeśli klient nie poda parametru nazwy hosta w uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="61976-881">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="61976-882">Wszystkie witryny sieci Web działają na tym samym wystąpieniu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-882">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="61976-883">Kestrel nie obsługuje udostępniania adresu IP i portu w wielu wystąpieniach bez zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="61976-883">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
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

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="61976-884">Powiąż z gniazdem TCP</span><span class="sxs-lookup"><span data-stu-id="61976-884">Bind to a TCP socket</span></span>

<span data-ttu-id="61976-885"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> Metoda wiąże się z gniazdem TCP, a opcja lambda zezwala na konfigurację certyfikatu X. 509:</span><span class="sxs-lookup"><span data-stu-id="61976-885">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
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
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

<span data-ttu-id="61976-886">Przykład konfiguruje HTTPS dla punktu końcowego za <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>pomocą.</span><span class="sxs-lookup"><span data-stu-id="61976-886">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="61976-887">Użyj tego samego interfejsu API, aby skonfigurować inne ustawienia Kestrel dla określonych punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="61976-887">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="61976-888">Powiąż z gniazdem systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="61976-888">Bind to a Unix socket</span></span>

<span data-ttu-id="61976-889">Nasłuchiwanie w gnieździe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> systemu UNIX za pomocą programu w celu zwiększenia wydajności za pomocą usługi Nginx, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="61976-889">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock");
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

### <a name="port-0"></a><span data-ttu-id="61976-890">Port 0</span><span class="sxs-lookup"><span data-stu-id="61976-890">Port 0</span></span>

<span data-ttu-id="61976-891">Gdy numer `0` portu jest określony, Kestrel dynamicznie wiąże się z dostępnym portem.</span><span class="sxs-lookup"><span data-stu-id="61976-891">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="61976-892">W poniższym przykładzie pokazano, jak określić, który port Kestrel faktycznie powiązany w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="61976-892">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="61976-893">Po uruchomieniu aplikacji dane wyjściowe okna konsoli wskazują port dynamiczny, w którym można uzyskać dostęp do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="61976-893">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="61976-894">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="61976-894">Limitations</span></span>

<span data-ttu-id="61976-895">Skonfiguruj punkty końcowe przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="61976-895">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="61976-896">`--urls`argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="61976-896">`--urls` command-line argument</span></span>
* <span data-ttu-id="61976-897">`urls`klucz konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="61976-897">`urls` host configuration key</span></span>
* <span data-ttu-id="61976-898">`ASPNETCORE_URLS`Zmienna środowiskowa</span><span class="sxs-lookup"><span data-stu-id="61976-898">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="61976-899">Te metody są przydatne do tworzenia kodu w pracy z serwerami innymi niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61976-899">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="61976-900">Należy jednak pamiętać o następujących ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="61976-900">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="61976-901">Nie można użyć protokołu HTTPS z tymi metodami, chyba że w konfiguracji punktu końcowego HTTPS jest podany certyfikat domyślny (na `KestrelServerOptions` przykład przy użyciu konfiguracji lub pliku konfiguracji, jak pokazano wcześniej w tym temacie).</span><span class="sxs-lookup"><span data-stu-id="61976-901">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="61976-902">Gdy oba `Listen` podejścia i `UseUrls` `UseUrls` są używane jednocześnie, punktykońcowezastępująpunktykońcowe.`Listen`</span><span class="sxs-lookup"><span data-stu-id="61976-902">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="61976-903">Konfiguracja punktu końcowego usług IIS</span><span class="sxs-lookup"><span data-stu-id="61976-903">IIS endpoint configuration</span></span>

<span data-ttu-id="61976-904">W przypadku korzystania z usług IIS powiązania URL dla powiązań przesłonięć usług IIS są ustawiane `UseUrls`przez `Listen` lub.</span><span class="sxs-lookup"><span data-stu-id="61976-904">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="61976-905">Aby uzyskać więcej informacji, zobacz temat [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="61976-905">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="61976-906">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="61976-906">Transport configuration</span></span>

<span data-ttu-id="61976-907">W wersji platformy ASP.NET Core 2.1 transport domyślny serwera Kestrel nie jest już oparty na bibliotece libuv, ale na zarządzanych gniazdach.</span><span class="sxs-lookup"><span data-stu-id="61976-907">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="61976-908">Jest to istotna zmiana dla aplikacji ASP.NET Core 2,0 uaktualniana do 2,1, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> które wywołują i zależą od jednego z następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="61976-908">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="61976-909">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (bezpośrednie odwołanie do pakietu)</span><span class="sxs-lookup"><span data-stu-id="61976-909">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="61976-910">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="61976-910">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="61976-911">Dla projektów, które wymagają użycia Libuv:</span><span class="sxs-lookup"><span data-stu-id="61976-911">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="61976-912">Dodaj zależność dla pakietu [Microsoft. AspNetCore. Server. Kestrel. transport. Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="61976-912">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="61976-913">Wywołanie <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="61976-913">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="61976-914">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="61976-914">URL prefixes</span></span>

<span data-ttu-id="61976-915">W przypadku `UseUrls`użycia `--urls` , argumentu wiersza polecenia, `urls` klucza konfiguracji hosta lub `ASPNETCORE_URLS` zmiennej środowiskowej prefiksy adresów URL mogą znajdować się w jednym z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="61976-915">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="61976-916">Tylko prefiksy adresów URL HTTP są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="61976-916">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="61976-917">Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązań adresów `UseUrls`URL przy użyciu programu.</span><span class="sxs-lookup"><span data-stu-id="61976-917">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="61976-918">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="61976-918">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="61976-919">`0.0.0.0`jest szczególnym przypadkiem, który wiąże się ze wszystkimi adresami IPv4.</span><span class="sxs-lookup"><span data-stu-id="61976-919">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="61976-920">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="61976-920">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="61976-921">`[::]`jest odpowiednikiem IPv6 dla protokołu `0.0.0.0`IPv4.</span><span class="sxs-lookup"><span data-stu-id="61976-921">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="61976-922">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="61976-922">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="61976-923">Nazwy `*`hostów, i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="61976-923">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="61976-924">Wszystkie nie są rozpoznawane jako prawidłowy adres IP `localhost` lub są powiązane ze wszystkimi IP IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="61976-924">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="61976-925">Aby powiązać różne nazwy hostów z różnymi ASP.NET Core aplikacjami na tym samym porcie, użyj [protokołu HTTP. sys](xref:fundamentals/servers/httpsys) lub odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="61976-925">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="61976-926">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="61976-926">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="61976-927">Nazwa `localhost` hosta z numerem portu lub adresem IP sprzężenia zwrotnego z numerem portu</span><span class="sxs-lookup"><span data-stu-id="61976-927">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="61976-928">Gdy `localhost` jest określony, Kestrel próbuje powiązać z interfejsami sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="61976-928">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="61976-929">Jeśli żądany port jest używany przez inną usługę w dowolnym interfejsie sprzężenia zwrotnego, uruchomienie Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="61976-929">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="61976-930">Jeśli którykolwiek z innych przyczyn interfejsu sprzężenia zwrotnego jest niedostępny (najczęściej jest to spowodowane tym, że protokół IPv6 nie jest obsługiwany), Kestrel rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="61976-930">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="61976-931">Filtrowanie hostów</span><span class="sxs-lookup"><span data-stu-id="61976-931">Host filtering</span></span>

<span data-ttu-id="61976-932">Program Kestrel obsługuje konfigurację na podstawie prefiksów, takich `http://example.com:5000`jak, Kestrel w znacznym stopniu ignoruje nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="61976-932">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="61976-933">Host `localhost` jest specjalnym przypadkiem używanym do tworzenia powiązań z adresami sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="61976-933">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="61976-934">Każdy host poza jawnym adresem IP tworzy powiązanie ze wszystkimi publicznymi adresami IP.</span><span class="sxs-lookup"><span data-stu-id="61976-934">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="61976-935">`Host`nagłówki nie są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="61976-935">`Host` headers aren't validated.</span></span>

<span data-ttu-id="61976-936">Aby obejść ten sposób, użyj oprogramowania pośredniczącego filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="61976-936">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="61976-937">Oprogramowanie pośredniczące do filtrowania hosta jest dostarczane przez pakiet [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , który jest zawarty w pakiecie [Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub 2,2).</span><span class="sxs-lookup"><span data-stu-id="61976-937">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="61976-938">Oprogramowanie pośredniczące jest dodawane przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>program, który <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>wywołuje następujące wywołania:</span><span class="sxs-lookup"><span data-stu-id="61976-938">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="61976-939">Programowe filtrowanie hosta jest domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="61976-939">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="61976-940">Aby włączyć oprogramowanie pośredniczące, zdefiniuj klucz `AllowedHosts` w pliku *appSettings. JSON*/ *.\< EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="61976-940">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="61976-941">Wartość to rozdzielana średnikami lista nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="61976-941">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="61976-942">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="61976-942">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="61976-943">[Przekierowane nagłówki oprogramowania](xref:host-and-deploy/proxy-load-balancer) również mają <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> opcję.</span><span class="sxs-lookup"><span data-stu-id="61976-943">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="61976-944">Przekazane nagłówki — oprogramowanie pośredniczące i filtrowanie hostów oprogramowanie pośredniczące ma podobną funkcjonalność dla różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="61976-944">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="61976-945">Ustawienie `AllowedHosts` z przekierowanymi nagłówkami — oprogramowanie pośredniczące jest odpowiednie, `Host` gdy nagłówek nie jest zachowywany podczas przekazywania żądań z odwrotnym serwerem proxy lub modułem równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="61976-945">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="61976-946">Ustawienie `AllowedHosts` przy użyciu oprogramowania pośredniczącego filtrowania hosta jest odpowiednie, gdy Kestrel jest używany jako publiczny serwer graniczny lub `Host` gdy nagłówek jest bezpośrednio przekazywany.</span><span class="sxs-lookup"><span data-stu-id="61976-946">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="61976-947">Aby uzyskać więcej informacji na temat przekierowanych nagłówków <xref:host-and-deploy/proxy-load-balancer>, należy zapoznać się z tematem.</span><span class="sxs-lookup"><span data-stu-id="61976-947">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="61976-948">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="61976-948">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="61976-949">RFC 7230: Składnia i routing wiadomości (sekcja 5,4: Host)</span><span class="sxs-lookup"><span data-stu-id="61976-949">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
