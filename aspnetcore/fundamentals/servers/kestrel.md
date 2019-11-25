---
title: Implementacja serwera sieci Web Kestrel w ASP.NET Core
author: guardrex
description: Dowiedz się więcej na temat Kestrel, międzyplatformowego serwera sieci Web dla ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 6fba6689f72f7a565e28d80f6770765ab097cf11
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289104"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="536eb-103">Implementacja serwera sieci Web Kestrel w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="536eb-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="536eb-104">Autorzy [Dykstra](https://github.com/tdykstra), [Krzysztof Ross](https://github.com/Tratcher), [Stephen](https://twitter.com/halter73), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="536eb-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="536eb-105">Kestrel to Międzyplatformowy [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="536eb-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="536eb-106">Kestrel to serwer sieci Web, który jest domyślnie uwzględniony w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="536eb-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="536eb-107">Kestrel obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="536eb-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="536eb-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="536eb-108">HTTPS</span></span>
* <span data-ttu-id="536eb-109">Nieprzezroczyste uaktualnienie używane do włączania obsługi obiektów [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="536eb-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="536eb-110">Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx</span><span class="sxs-lookup"><span data-stu-id="536eb-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="536eb-111">HTTP/2 (z wyjątkiem macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="536eb-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="536eb-112">&dagger;HTTP/2 będą obsługiwane w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="536eb-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="536eb-113">Kestrel jest obsługiwana na wszystkich platformach i wersjach obsługiwanych przez platformę .NET Core.</span><span class="sxs-lookup"><span data-stu-id="536eb-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="536eb-114">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="536eb-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="536eb-115">Obsługa protokołu HTTP/2</span><span class="sxs-lookup"><span data-stu-id="536eb-115">HTTP/2 support</span></span>

<span data-ttu-id="536eb-116">[Protokół HTTP/2](https://httpwg.org/specs/rfc7540.html) jest dostępny dla aplikacji ASP.NET Core, jeśli spełnione są następujące wymagania podstawowe:</span><span class="sxs-lookup"><span data-stu-id="536eb-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="536eb-117">&dagger; systemu operacyjnego</span><span class="sxs-lookup"><span data-stu-id="536eb-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="536eb-118">Windows Server 2016/Windows 10 lub nowszy&Dagger;</span><span class="sxs-lookup"><span data-stu-id="536eb-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="536eb-119">Linux z OpenSSL 1.0.2 lub nowszym (na przykład Ubuntu 16,04 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="536eb-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="536eb-120">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="536eb-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="536eb-121">Połączenie [negocjowania protokołu warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="536eb-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="536eb-122">Protokół TLS 1.2 lub nowszej połączenia</span><span class="sxs-lookup"><span data-stu-id="536eb-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="536eb-123">&dagger;HTTP/2 będą obsługiwane w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="536eb-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="536eb-124">&Dagger;Kestrel ma ograniczoną obsługę protokołu HTTP/2 w systemie Windows Server 2012 R2 i Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="536eb-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="536eb-125">Obsługa jest ograniczona, ponieważ lista obsługiwanych mechanizmów szyfrowania TLS dostępnych w tych systemach operacyjnych jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="536eb-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="536eb-126">Do zabezpieczenia połączeń TLS może być wymagany certyfikat wygenerowany przy użyciu algorytmu podpisu cyfrowego (ECDSA) krzywej eliptycznej.</span><span class="sxs-lookup"><span data-stu-id="536eb-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="536eb-127">Jeśli zostanie nawiązane połączenie HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="536eb-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="536eb-128">Protokół HTTP/2 jest domyślnie wyłączony.</span><span class="sxs-lookup"><span data-stu-id="536eb-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="536eb-129">Więcej informacji na temat konfiguracji można znaleźć w sekcjach [Kestrel Options](#kestrel-options) and [ListenOptions. Protocols](#listenoptionsprotocols) .</span><span class="sxs-lookup"><span data-stu-id="536eb-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="536eb-130">Kiedy używać Kestrel z zwrotnym serwerem proxy</span><span class="sxs-lookup"><span data-stu-id="536eb-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="536eb-131">Kestrel może być używana przez siebie lub z *odwrotnym serwerem proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org)lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="536eb-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="536eb-132">Odwrotny serwer proxy odbiera żądania HTTP z sieci i przekazuje je do usługi Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="536eb-133">Kestrel używany jako serwer sieci Web z krawędzią (dostępną z Internetu):</span><span class="sxs-lookup"><span data-stu-id="536eb-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem bez serwera proxy zwrotnego](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="536eb-135">Kestrel używany w konfiguracji zwrotnego serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="536eb-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel komunikuje się pośrednio z Internetem za pomocą odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="536eb-137">Konfiguracja, z serwerem proxy odwrotnych lub bez niego, jest obsługiwaną konfiguracją hostingu.</span><span class="sxs-lookup"><span data-stu-id="536eb-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="536eb-138">Kestrel używany jako serwer graniczny bez serwera proxy odwrotnego nie obsługuje udostępniania tego samego adresu IP i portu między wieloma procesami.</span><span class="sxs-lookup"><span data-stu-id="536eb-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="536eb-139">Gdy Kestrel jest skonfigurowany do nasłuchiwania na porcie, Kestrel obsługuje cały ruch dla tego portu bez względu na żądania `Host` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="536eb-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="536eb-140">Zwrotny serwer proxy, który może udostępniać porty, ma możliwość przekazywania żądań do Kestrel na unikatowym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="536eb-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="536eb-141">Nawet jeśli odwrotny serwer proxy nie jest wymagany, może to być dobry wybór przy użyciu odwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="536eb-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="536eb-142">Zwrotny serwer proxy:</span><span class="sxs-lookup"><span data-stu-id="536eb-142">A reverse proxy:</span></span>

* <span data-ttu-id="536eb-143">Może ograniczać uwidoczniony obszar publicznej powierzchni aplikacji, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="536eb-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="536eb-144">Zapewnienie dodatkowej warstwy konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="536eb-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="536eb-145">Integracja z istniejącą infrastrukturą może być lepsza.</span><span class="sxs-lookup"><span data-stu-id="536eb-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="536eb-146">Uprość Równoważenie obciążenia i konfigurację komunikacji zabezpieczonej (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="536eb-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="536eb-147">Tylko serwer zwrotny proxy wymaga certyfikatu X. 509, a serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="536eb-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="536eb-148">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="536eb-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="kestrel-in-aspnet-core-apps"></a><span data-ttu-id="536eb-149">Kestrel w aplikacjach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="536eb-149">Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="536eb-150">Szablony projektów ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="536eb-151">W *program.cs*Metoda <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> wywołuje <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span><span class="sxs-lookup"><span data-stu-id="536eb-151">In *Program.cs*, the <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> method calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=8)]

<span data-ttu-id="536eb-152">Aby uzyskać więcej informacji na temat tworzenia hosta, zobacz sekcję *Konfigurowanie ustawień hosta* i *domyślnego konstruktora* dla <xref:fundamentals/host/generic-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="536eb-152">For more information on building the host, see the *Set up a host* and *Default builder settings* sections of <xref:fundamentals/host/generic-host#set-up-a-host>.</span></span>

<span data-ttu-id="536eb-153">Aby zapewnić dodatkową konfigurację po wywołaniu `ConfigureWebHostDefaults`, użyj `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="536eb-153">To provide additional configuration after calling `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="536eb-154">Opcje Kestrel</span><span class="sxs-lookup"><span data-stu-id="536eb-154">Kestrel options</span></span>

<span data-ttu-id="536eb-155">Serwer sieci Web Kestrel ma opcje konfiguracji ograniczeń, które są szczególnie przydatne w przypadku wdrożeń mających dostęp do Internetu.</span><span class="sxs-lookup"><span data-stu-id="536eb-155">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="536eb-156">Ustaw ograniczenia we właściwości <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="536eb-156">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="536eb-157">Właściwość `Limits` przechowuje wystąpienie klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="536eb-157">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="536eb-158">W poniższych przykładach użyto przestrzeni nazw <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="536eb-158">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="536eb-159">Opcje Kestrel, które są konfigurowane w C# kodzie w poniższych przykładach, można również ustawić za pomocą [dostawcy konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="536eb-159">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="536eb-160">Na przykład dostawca konfiguracji plików może załadować konfigurację Kestrel z pliku *appSettings. JSON* lub *appSettings. { Environment} plik JSON* :</span><span class="sxs-lookup"><span data-stu-id="536eb-160">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    },
    "DisableStringReuse": true
  }
}
```

<span data-ttu-id="536eb-161">Skorzystaj z **jednej** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="536eb-161">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="536eb-162">Skonfiguruj Kestrel w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="536eb-162">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="536eb-163">Wstrzyknąć wystąpienie `IConfiguration` do klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="536eb-163">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="536eb-164">W poniższym przykładzie przyjęto założenie, że wprowadzona konfiguracja jest przypisana do właściwości `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="536eb-164">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="536eb-165">W `Startup.ConfigureServices`Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-165">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="536eb-166">Skonfiguruj Kestrel podczas kompilowania hosta:</span><span class="sxs-lookup"><span data-stu-id="536eb-166">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="536eb-167">W programie *program.cs*Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="536eb-167">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IHostBuilder CreateHostBuilder(string[] args) =>
      Host.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .ConfigureWebHostDefaults(webBuilder =>
          {
              webBuilder.UseStartup<Startup>();
          });
  ```

<span data-ttu-id="536eb-168">Obie powyższe podejścia współpracują z dowolnym [dostawcą konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="536eb-168">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="536eb-169">Limit czasu utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="536eb-169">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="536eb-170">Pobiera lub ustawia [limit czasu utrzymywania aktywności](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="536eb-170">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="536eb-171">Wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="536eb-171">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="536eb-172">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="536eb-172">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="536eb-173">Maksymalna liczba współbieżnych otwartych połączeń TCP można ustawić dla całej aplikacji przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="536eb-173">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="536eb-174">Istnieje oddzielny limit połączeń, które zostały uaktualnione z protokołu HTTP lub HTTPS do innego protokołu (na przykład w żądaniu usługi WebSockets).</span><span class="sxs-lookup"><span data-stu-id="536eb-174">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="536eb-175">Po uaktualnieniu połączenia nie jest ono wliczane do limitu `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="536eb-175">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="536eb-176">Maksymalna liczba połączeń jest domyślnie nieograniczona (null).</span><span class="sxs-lookup"><span data-stu-id="536eb-176">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="536eb-177">Maksymalny rozmiar treści żądania</span><span class="sxs-lookup"><span data-stu-id="536eb-177">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="536eb-178">Domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="536eb-178">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="536eb-179">Zalecanym podejściem do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie atrybutu <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> dla metody akcji:</span><span class="sxs-lookup"><span data-stu-id="536eb-179">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="536eb-180">Oto przykład, który pokazuje, jak skonfigurować ograniczenie dla aplikacji w każdym żądaniu:</span><span class="sxs-lookup"><span data-stu-id="536eb-180">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="536eb-181">Zastąp ustawienie określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="536eb-181">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="536eb-182">Wyjątek jest generowany, jeśli aplikacja skonfiguruje limit żądania po rozpoczęciu uruchamiania aplikacji w celu odczytania żądania.</span><span class="sxs-lookup"><span data-stu-id="536eb-182">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="536eb-183">Istnieje właściwość `IsReadOnly`, która wskazuje, czy właściwość `MaxRequestBodySize` jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="536eb-183">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="536eb-184">Gdy aplikacja jest uruchamiana [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model) [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru treści żądania Kestrel jest wyłączony, ponieważ program IIS już ustawia limit.</span><span class="sxs-lookup"><span data-stu-id="536eb-184">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="536eb-185">Minimalna stawka danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="536eb-185">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="536eb-186">Kestrel sprawdza co sekundę, jeśli dane są odbierane z określoną szybkością w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="536eb-186">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="536eb-187">Jeśli szybkość spadnie poniżej wartości minimalnej, upłynął limit czasu połączenia. Okres prolongaty to czas, przez który Kestrel nadaje klientowi zwiększenie jego szybkości wysyłania do minimum; Ta częstotliwość nie jest sprawdzana w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="536eb-187">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="536eb-188">Okres prolongaty pozwala uniknąć porzucania połączeń, które początkowo wysyłają dane z niską szybkością.</span><span class="sxs-lookup"><span data-stu-id="536eb-188">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="536eb-189">Domyślna stawka minimalna to 240 bajtów na sekundę z 5-sekundowym okresem prolongaty.</span><span class="sxs-lookup"><span data-stu-id="536eb-189">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="536eb-190">Minimalna stawka dotyczy także odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="536eb-190">A minimum rate also applies to the response.</span></span> <span data-ttu-id="536eb-191">Kod określający limit żądań i limit odpowiedzi jest taki sam, chyba że ma `RequestBody` lub `Response` w nazwach właściwości i interfejsów.</span><span class="sxs-lookup"><span data-stu-id="536eb-191">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="536eb-192">Oto przykład pokazujący sposób konfigurowania minimalnych stawek danych w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="536eb-192">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="536eb-193">Przesłoń minimalne limity szybkości dla żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="536eb-193">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="536eb-194"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature>, do których odwołuje się Poprzednia próbka nie występuje w `HttpContext.Features` dla żądań HTTP/2, ponieważ Modyfikowanie limitów szybkości dla poszczególnych żądań jest ogólnie nieobsługiwane w przypadku protokołu HTTP/2 z powodu obsługi żądania multipleksowania żądań.</span><span class="sxs-lookup"><span data-stu-id="536eb-194">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="536eb-195">Jednak <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> nadal obecne `HttpContext.Features` dla żądań HTTP/2, ponieważ limit liczby odczytów nadal można *wyłączyć całkowicie* dla poszczególnych żądań, ustawiając `IHttpMinRequestBodyDataRateFeature.MinDataRate` `null` nawet dla żądania HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="536eb-195">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="536eb-196">Próba odczytania `IHttpMinRequestBodyDataRateFeature.MinDataRate` lub próby ustawienia jej na wartość inną niż `null` spowoduje, że `NotSupportedException` zostanie zgłoszone żądanie HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="536eb-196">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="536eb-197">Limity szybkości dla całego serwera skonfigurowane za pomocą `KestrelServerOptions.Limits` nadal mają zastosowanie do połączeń HTTP/1. x i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="536eb-197">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="536eb-198">Limit czasu nagłówków żądań</span><span class="sxs-lookup"><span data-stu-id="536eb-198">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="536eb-199">Pobiera lub ustawia maksymalny czas, przez jaki serwer spędza nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="536eb-199">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="536eb-200">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="536eb-200">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="536eb-201">Maksymalna liczba strumieni na połączenie</span><span class="sxs-lookup"><span data-stu-id="536eb-201">Maximum streams per connection</span></span>

<span data-ttu-id="536eb-202">`Http2.MaxStreamsPerConnection` ogranicza liczbę współbieżnych strumieni żądań dla połączenia HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="536eb-202">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="536eb-203">Odrzucone strumienie są nadmiarowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-203">Excess streams are refused.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="536eb-204">Wartość domyślna to 100.</span><span class="sxs-lookup"><span data-stu-id="536eb-204">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="536eb-205">Rozmiar tabeli nagłówka</span><span class="sxs-lookup"><span data-stu-id="536eb-205">Header table size</span></span>

<span data-ttu-id="536eb-206">Dekoder HPACK dekompresuje nagłówki HTTP dla połączeń HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="536eb-206">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="536eb-207">`Http2.HeaderTableSize` ogranicza rozmiar tabeli kompresji nagłówka używanej przez dekoder HPACK.</span><span class="sxs-lookup"><span data-stu-id="536eb-207">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="536eb-208">Wartość jest podawana w oktetach i musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="536eb-208">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="536eb-209">Wartość domyślna to 4096.</span><span class="sxs-lookup"><span data-stu-id="536eb-209">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="536eb-210">Maksymalny rozmiar ramki</span><span class="sxs-lookup"><span data-stu-id="536eb-210">Maximum frame size</span></span>

<span data-ttu-id="536eb-211">`Http2.MaxFrameSize` wskazuje maksymalny dozwolony rozmiar ładunku ramki połączenia HTTP/2 otrzymanego lub wysyłanego przez serwer.</span><span class="sxs-lookup"><span data-stu-id="536eb-211">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="536eb-212">Wartość jest podawana w oktetach i musi należeć do przedziału od 2 ^ 14 (16 384) do 2 ^ 24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="536eb-212">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="536eb-213">Wartość domyślna to 2 ^ 14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="536eb-213">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="536eb-214">Maksymalny rozmiar nagłówka żądania</span><span class="sxs-lookup"><span data-stu-id="536eb-214">Maximum request header size</span></span>

<span data-ttu-id="536eb-215">`Http2.MaxRequestHeaderFieldSize` wskazuje maksymalny dozwolony rozmiar w oktetach wartości nagłówka żądania.</span><span class="sxs-lookup"><span data-stu-id="536eb-215">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="536eb-216">Ten limit dotyczy zarówno nazwy, jak i wartości w skompresowanych i nieskompresowanych reprezentacji.</span><span class="sxs-lookup"><span data-stu-id="536eb-216">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="536eb-217">Wartość musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="536eb-217">The value must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="536eb-218">Wartość domyślna to 8 192.</span><span class="sxs-lookup"><span data-stu-id="536eb-218">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="536eb-219">Początkowy rozmiar okna połączenia</span><span class="sxs-lookup"><span data-stu-id="536eb-219">Initial connection window size</span></span>

<span data-ttu-id="536eb-220">`Http2.InitialConnectionWindowSize` wskazuje maksymalne dane dotyczące treści żądania w bajtach, które bufory serwera w tym samym czasie są agregowane dla wszystkich żądań (strumieni) na połączenie.</span><span class="sxs-lookup"><span data-stu-id="536eb-220">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="536eb-221">Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="536eb-221">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="536eb-222">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="536eb-222">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="536eb-223">Wartość domyślna to 128 KB (131 072).</span><span class="sxs-lookup"><span data-stu-id="536eb-223">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="536eb-224">Rozmiar początkowego okna strumienia</span><span class="sxs-lookup"><span data-stu-id="536eb-224">Initial stream window size</span></span>

<span data-ttu-id="536eb-225">`Http2.InitialStreamWindowSize` wskazuje maksymalne dane dotyczące treści żądania w bajtach bufory serwerów w jednym momencie na żądanie (Stream).</span><span class="sxs-lookup"><span data-stu-id="536eb-225">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="536eb-226">Żądania są również ograniczone przez `Http2.InitialConnectionWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="536eb-226">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="536eb-227">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="536eb-227">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="536eb-228">Wartość domyślna to 96 KB (98 304).</span><span class="sxs-lookup"><span data-stu-id="536eb-228">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="536eb-229">Synchroniczne we/wy</span><span class="sxs-lookup"><span data-stu-id="536eb-229">Synchronous IO</span></span>

<span data-ttu-id="536eb-230"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> kontroluje, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="536eb-230"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="536eb-231">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="536eb-231">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="536eb-232">Duża liczba blokowania synchronicznych operacji we/wy może prowadzić do zablokowania puli wątków, co sprawia, że aplikacja nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="536eb-232">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="536eb-233">Włącz `AllowSynchronousIO` tylko w przypadku korzystania z biblioteki, która nie obsługuje asynchronicznych operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="536eb-233">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="536eb-234">Poniższy przykład włącza synchroniczną operację we/wy:</span><span class="sxs-lookup"><span data-stu-id="536eb-234">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="536eb-235">Aby uzyskać informacje o innych opcjach i ograniczeniach Kestrel, zobacz:</span><span class="sxs-lookup"><span data-stu-id="536eb-235">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="536eb-236">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="536eb-236">Endpoint configuration</span></span>

<span data-ttu-id="536eb-237">Domyślnie ASP.NET Core wiąże się z:</span><span class="sxs-lookup"><span data-stu-id="536eb-237">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="536eb-238">`https://localhost:5001` (gdy obecny jest lokalny certyfikat programistyczny)</span><span class="sxs-lookup"><span data-stu-id="536eb-238">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="536eb-239">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="536eb-239">Specify URLs using the:</span></span>

* <span data-ttu-id="536eb-240">`ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="536eb-240">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="536eb-241">`--urls` argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="536eb-241">`--urls` command-line argument.</span></span>
* <span data-ttu-id="536eb-242">klucz konfiguracji hosta `urls`.</span><span class="sxs-lookup"><span data-stu-id="536eb-242">`urls` host configuration key.</span></span>
* <span data-ttu-id="536eb-243">Metoda rozszerzenia `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="536eb-243">`UseUrls` extension method.</span></span>

<span data-ttu-id="536eb-244">Wartość podana przy użyciu tych metod może być jednym lub większą liczbą punktów końcowych HTTP i HTTPS (HTTPS, jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="536eb-244">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="536eb-245">Skonfiguruj wartość jako listę rozdzieloną średnikami (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="536eb-245">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="536eb-246">Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwera](xref:fundamentals/host/web-host#server-urls) i [Zastąp konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="536eb-246">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="536eb-247">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="536eb-247">A development certificate is created:</span></span>

* <span data-ttu-id="536eb-248">Po zainstalowaniu [zestaw .NET Core SDK](/dotnet/core/sdk) .</span><span class="sxs-lookup"><span data-stu-id="536eb-248">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="536eb-249">Do utworzenia certyfikatu służy [Narzędzie dev-certs](xref:aspnetcore-2.1#https) .</span><span class="sxs-lookup"><span data-stu-id="536eb-249">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="536eb-250">Niektóre przeglądarki wymagają przyznania jawnego uprawnienia do zaufania do lokalnego certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="536eb-250">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="536eb-251">Szablony projektu konfigurują aplikacje do uruchamiania domyślnie przy użyciu protokołu HTTPS i obejmują [przekierowania https i obsługę HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="536eb-251">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="536eb-252">Wywołaj metody <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> w <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, aby skonfigurować prefiksy i porty adresów URL dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-252">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="536eb-253">`UseUrls`, `--urls` argument wiersza polecenia, klucz konfiguracji hosta `urls` oraz zmienna środowiskowa `ASPNETCORE_URLS` również działają, ale mają ograniczenia wymienione w dalszej części tej sekcji (certyfikat domyślny musi być dostępny do konfiguracji punktu końcowego HTTPS).</span><span class="sxs-lookup"><span data-stu-id="536eb-253">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="536eb-254">Konfiguracja `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="536eb-254">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="536eb-255">ConfigureEndpointDefaults (Akcja\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="536eb-255">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="536eb-256">Określa `Action` konfiguracji do uruchomienia dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="536eb-256">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="536eb-257">Wywoływanie `ConfigureEndpointDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="536eb-257">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });
});
```

> [!NOTE]
> <span data-ttu-id="536eb-258">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="536eb-258">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="536eb-259">ConfigureHttpsDefaults (Akcja\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="536eb-259">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="536eb-260">Określa `Action` konfiguracji do uruchomienia dla każdego punktu końcowego HTTPS.</span><span class="sxs-lookup"><span data-stu-id="536eb-260">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="536eb-261">Wywoływanie `ConfigureHttpsDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="536eb-261">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        // certificate is an X509Certificate2
        listenOptions.ServerCertificate = certificate;
    });
});
```

> [!NOTE]
> <span data-ttu-id="536eb-262">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="536eb-262">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="536eb-263">Konfiguruj (IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="536eb-263">Configure(IConfiguration)</span></span>

<span data-ttu-id="536eb-264">Tworzy moduł ładujący konfigurację służący do konfigurowania Kestrel, który pobiera <xref:Microsoft.Extensions.Configuration.IConfiguration> jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-264">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="536eb-265">Konfiguracja musi być objęta zakresem sekcji konfiguracji dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-265">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="536eb-266">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="536eb-266">ListenOptions.UseHttps</span></span>

<span data-ttu-id="536eb-267">Skonfiguruj Kestrel do korzystania z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="536eb-267">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="536eb-268">rozszerzenia `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="536eb-268">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="536eb-269">`UseHttps` &ndash; skonfigurować Kestrel do używania protokołu HTTPS z certyfikatem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="536eb-269">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="536eb-270">Zgłasza wyjątek, jeśli nie został skonfigurowany żaden certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="536eb-270">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="536eb-271">`ListenOptions.UseHttps` parametry:</span><span class="sxs-lookup"><span data-stu-id="536eb-271">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="536eb-272">`filename` to ścieżka i nazwa pliku certyfikatu odnosząca się do katalogu zawierającego pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="536eb-272">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="536eb-273">`password` to hasło wymagane do uzyskania dostępu do danych certyfikatu X. 509.</span><span class="sxs-lookup"><span data-stu-id="536eb-273">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="536eb-274">`configureOptions` jest `Action`, aby skonfigurować `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="536eb-274">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="536eb-275">Zwraca `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="536eb-275">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="536eb-276">`storeName` to magazyn certyfikatów, z którego ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="536eb-276">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="536eb-277">`subject` to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="536eb-277">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="536eb-278">`allowInvalid` wskazuje, czy należy wziąć pod uwagę nieprawidłowe certyfikaty, takie jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="536eb-278">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="536eb-279">`location` to lokalizacja magazynu, z której ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="536eb-279">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="536eb-280">`serverCertificate` to certyfikat X. 509.</span><span class="sxs-lookup"><span data-stu-id="536eb-280">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="536eb-281">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="536eb-281">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="536eb-282">Należy podać co najmniej certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="536eb-282">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="536eb-283">Obsługiwane konfiguracje opisane dalej:</span><span class="sxs-lookup"><span data-stu-id="536eb-283">Supported configurations described next:</span></span>

* <span data-ttu-id="536eb-284">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="536eb-284">No configuration</span></span>
* <span data-ttu-id="536eb-285">Zastąp domyślny certyfikat z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="536eb-285">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="536eb-286">Zmień wartości domyślne w kodzie</span><span class="sxs-lookup"><span data-stu-id="536eb-286">Change the defaults in code</span></span>

<span data-ttu-id="536eb-287">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="536eb-287">*No configuration*</span></span>

<span data-ttu-id="536eb-288">Kestrel nasłuchuje na `http://localhost:5000` i `https://localhost:5001` (jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="536eb-288">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="536eb-289">*Zastąp domyślny certyfikat z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="536eb-289">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="536eb-290">`CreateDefaultBuilder` wywołania `Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-290">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="536eb-291">Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępny dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-291">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="536eb-292">Konfigurowanie wielu punktów końcowych, w tym adresów URL i certyfikatów do użycia, z pliku znajdującego się na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="536eb-292">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="536eb-293">W poniższym przykładzie pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="536eb-293">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="536eb-294">Ustaw **AllowInvalid** na `true`, aby zezwolić na korzystanie z nieprawidłowych certyfikatów (na przykład certyfikatów z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="536eb-294">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="536eb-295">Wszystkie punkty końcowe HTTPS, które nie określają certyfikatu (**HttpsDefaultCert** w poniższym przykładzie) powracają do certyfikatu zdefiniowanego w obszarze **Certyfikaty** > **domyślnym** lub certyfikatem deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="536eb-295">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="536eb-296">Alternatywą dla korzystania z **ścieżki** i **hasła** dla dowolnego węzła certyfikatu jest określenie certyfikatu przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="536eb-296">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="536eb-297">Na przykład **certyfikaty** > **domyślnego** certyfikatu można określić jako:</span><span class="sxs-lookup"><span data-stu-id="536eb-297">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="536eb-298">Uwagi dotyczące schematu:</span><span class="sxs-lookup"><span data-stu-id="536eb-298">Schema notes:</span></span>

* <span data-ttu-id="536eb-299">W nazwach punktów końcowych nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="536eb-299">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="536eb-300">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-300">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="536eb-301">Dla każdego punktu końcowego jest wymagany parametr `Url`.</span><span class="sxs-lookup"><span data-stu-id="536eb-301">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="536eb-302">Format tego parametru jest taki sam jak parametr konfiguracji `Urls` najwyższego poziomu, z tą różnicą, że jest ograniczony do pojedynczej wartości.</span><span class="sxs-lookup"><span data-stu-id="536eb-302">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="536eb-303">Te punkty końcowe zastępują te zdefiniowane w konfiguracji `Urls` najwyższego poziomu zamiast dodawać do nich.</span><span class="sxs-lookup"><span data-stu-id="536eb-303">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="536eb-304">Punkty końcowe zdefiniowane w kodzie za pośrednictwem `Listen` kumulują się z punktami końcowymi zdefiniowanymi w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="536eb-304">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="536eb-305">Sekcja `Certificate` jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="536eb-305">The `Certificate` section is optional.</span></span> <span data-ttu-id="536eb-306">Jeśli sekcja `Certificate` nie zostanie określona, są używane wartości domyślne zdefiniowane we wcześniejszych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="536eb-306">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="536eb-307">Jeśli żadne wartości domyślne nie są dostępne, serwer zgłasza wyjątek i nie może się uruchomić.</span><span class="sxs-lookup"><span data-stu-id="536eb-307">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="536eb-308">Sekcja `Certificate` obsługuje zarówno **ścieżkę**&ndash;**hasło** , jak i **temat**&ndash;certyfikatów **magazynu** .</span><span class="sxs-lookup"><span data-stu-id="536eb-308">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="536eb-309">W ten sposób można zdefiniować dowolną liczbę punktów końcowych, dopóki nie spowoduje to konfliktów portów.</span><span class="sxs-lookup"><span data-stu-id="536eb-309">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="536eb-310">`options.Configure(context.Configuration.GetSection("{SECTION}"))` zwraca `KestrelConfigurationLoader` z metodą `.Endpoint(string name, listenOptions => { })`, której można użyć do uzupełnienia ustawień skonfigurowanego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="536eb-310">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
webBuilder.UseKestrel((context, serverOptions) =>
{
    serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
        .Endpoint("HTTPS", listenOptions =>
        {
            listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
        });
});
```

<span data-ttu-id="536eb-311">dostęp do `KestrelServerOptions.ConfigurationLoader` można uzyskać, aby kontynuować iterację istniejącego modułu ładującego, takiego jak dostarczony przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="536eb-311">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="536eb-312">Sekcja konfiguracji dla każdego punktu końcowego jest dostępna w opcjach w `Endpoint` metodzie, aby można było odczytać ustawienia niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-312">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="536eb-313">Można załadować wiele konfiguracji, wywołując `options.Configure(context.Configuration.GetSection("{SECTION}"))` ponownie z inną sekcją.</span><span class="sxs-lookup"><span data-stu-id="536eb-313">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="536eb-314">Używana jest tylko Ostatnia konfiguracja, chyba że `Load` jest jawnie wywoływana w poprzednich wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="536eb-314">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="536eb-315">Pakiet nie wywołuje `Load`, tak aby jego sekcja konfiguracji domyślnej mogła zostać zamieniona.</span><span class="sxs-lookup"><span data-stu-id="536eb-315">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="536eb-316">`KestrelConfigurationLoader` odzwierciedla `Listen`ą rodzinę interfejsów API z `KestrelServerOptions` jako przeciążania `Endpoint`, dlatego punkty końcowe kodu i konfiguracji można skonfigurować w tym samym miejscu.</span><span class="sxs-lookup"><span data-stu-id="536eb-316">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="536eb-317">Te przeciążenia nie używają nazw i używają tylko ustawień domyślnych z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="536eb-317">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="536eb-318">*Zmień wartości domyślne w kodzie*</span><span class="sxs-lookup"><span data-stu-id="536eb-318">*Change the defaults in code*</span></span>

<span data-ttu-id="536eb-319">`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` mogą służyć do zmiany domyślnych ustawień dla `ListenOptions` i `HttpsConnectionAdapterOptions`, w tym przesłanianie domyślnego certyfikatu określonego w poprzednim scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="536eb-319">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="536eb-320">przed skonfigurowaniem punktów końcowych należy wywołać `ConfigureEndpointDefaults` i `ConfigureHttpsDefaults`.</span><span class="sxs-lookup"><span data-stu-id="536eb-320">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
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
```

<span data-ttu-id="536eb-321">*Obsługa usługi Kestrel dla SNI*</span><span class="sxs-lookup"><span data-stu-id="536eb-321">*Kestrel support for SNI*</span></span>

<span data-ttu-id="536eb-322">[Oznaczanie nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do hostowania wielu domen na tym samym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="536eb-322">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="536eb-323">Aby SNI działał, klient wysyła nazwę hosta dla bezpiecznej sesji do serwera podczas uzgadniania TLS, aby serwer mógł zapewnić prawidłowy certyfikat.</span><span class="sxs-lookup"><span data-stu-id="536eb-323">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="536eb-324">Klient korzysta z dostarczonego certyfikatu w celu zaszyfrowania komunikacji z serwerem podczas bezpiecznej sesji, która następuje po uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="536eb-324">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="536eb-325">Kestrel obsługuje SNI za pośrednictwem wywołania zwrotnego `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="536eb-325">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="536eb-326">Wywołanie zwrotne jest wywoływane jednokrotnie dla każdego połączenia, aby umożliwić aplikacji inspekcję nazwy hosta i wybieranie odpowiedniego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="536eb-326">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="536eb-327">Obsługa SNI wymaga:</span><span class="sxs-lookup"><span data-stu-id="536eb-327">SNI support requires:</span></span>

* <span data-ttu-id="536eb-328">Uruchomione na platformie docelowej `netcoreapp2.1` lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="536eb-328">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="536eb-329">W `net461` lub później wywołanie zwrotne jest wywoływane, ale `name` jest zawsze `null`.</span><span class="sxs-lookup"><span data-stu-id="536eb-329">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="536eb-330">`name` jest również `null`, jeśli klient nie poda parametru nazwy hosta w uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="536eb-330">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="536eb-331">Wszystkie witryny sieci Web działają na tym samym wystąpieniu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-331">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="536eb-332">Kestrel nie obsługuje udostępniania adresu IP i portu w wielu wystąpieniach bez zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="536eb-332">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
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
});
```

### <a name="connection-logging"></a><span data-ttu-id="536eb-333">Rejestrowanie połączeń</span><span class="sxs-lookup"><span data-stu-id="536eb-333">Connection logging</span></span>

<span data-ttu-id="536eb-334">Wywołaj <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>, aby emitować dzienniki poziomu debugowania dla komunikacji na poziomie bajtów w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="536eb-334">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="536eb-335">Rejestrowanie połączeń ułatwia rozwiązywanie problemów z komunikacją na niskim poziomie, na przykład podczas szyfrowania TLS i za serwerami proxy.</span><span class="sxs-lookup"><span data-stu-id="536eb-335">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="536eb-336">Jeśli `UseConnectionLogging` jest umieszczony przed `UseHttps`, szyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="536eb-336">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="536eb-337">Jeśli `UseConnectionLogging` jest umieszczony po `UseHttps`, odszyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="536eb-337">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="536eb-338">Powiąż z gniazdem TCP</span><span class="sxs-lookup"><span data-stu-id="536eb-338">Bind to a TCP socket</span></span>

<span data-ttu-id="536eb-339">Metoda <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> wiąże się z gniazdem TCP, a opcja lambda zezwala na konfigurację certyfikatu X. 509:</span><span class="sxs-lookup"><span data-stu-id="536eb-339">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="536eb-340">Przykład konfiguruje HTTPS dla punktu końcowego z <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="536eb-340">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="536eb-341">Użyj tego samego interfejsu API, aby skonfigurować inne ustawienia Kestrel dla określonych punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="536eb-341">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="536eb-342">Powiąż z gniazdem systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="536eb-342">Bind to a Unix socket</span></span>

<span data-ttu-id="536eb-343">Nasłuchiwanie w gnieździe systemu UNIX za pomocą <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, aby zwiększyć wydajność za pomocą Nginx, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="536eb-343">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="536eb-344">Port 0</span><span class="sxs-lookup"><span data-stu-id="536eb-344">Port 0</span></span>

<span data-ttu-id="536eb-345">Gdy `0` jest określony numer portu, Kestrel dynamicznie wiąże się z dostępnym portem.</span><span class="sxs-lookup"><span data-stu-id="536eb-345">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="536eb-346">W poniższym przykładzie pokazano, jak określić, który port Kestrel faktycznie powiązany w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="536eb-346">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="536eb-347">Po uruchomieniu aplikacji dane wyjściowe okna konsoli wskazują port dynamiczny, w którym można uzyskać dostęp do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="536eb-347">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="536eb-348">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="536eb-348">Limitations</span></span>

<span data-ttu-id="536eb-349">Skonfiguruj punkty końcowe przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="536eb-349">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="536eb-350">`--urls` argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="536eb-350">`--urls` command-line argument</span></span>
* <span data-ttu-id="536eb-351">klucz konfiguracji hosta `urls`</span><span class="sxs-lookup"><span data-stu-id="536eb-351">`urls` host configuration key</span></span>
* <span data-ttu-id="536eb-352">Zmienna środowiskowa `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="536eb-352">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="536eb-353">Te metody są przydatne do tworzenia kodu w pracy z serwerami innymi niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-353">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="536eb-354">Należy jednak pamiętać o następujących ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="536eb-354">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="536eb-355">Nie można użyć protokołu HTTPS z tymi metodami, chyba że w konfiguracji punktu końcowego HTTPS jest podany certyfikat domyślny (na przykład za pomocą konfiguracji `KestrelServerOptions` lub pliku konfiguracji, jak pokazano wcześniej w tym temacie).</span><span class="sxs-lookup"><span data-stu-id="536eb-355">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="536eb-356">Gdy oba podejścia `Listen` i `UseUrls` są używane jednocześnie, `Listen` punkty końcowe przesłaniają `UseUrls` punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-356">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="536eb-357">Konfiguracja punktu końcowego usług IIS</span><span class="sxs-lookup"><span data-stu-id="536eb-357">IIS endpoint configuration</span></span>

<span data-ttu-id="536eb-358">W przypadku korzystania z usług IIS powiązania URL dla powiązań przesłonięć usług IIS są ustawiane przez `Listen` lub `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="536eb-358">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="536eb-359">Aby uzyskać więcej informacji, zobacz temat [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="536eb-359">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="536eb-360">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="536eb-360">ListenOptions.Protocols</span></span>

<span data-ttu-id="536eb-361">Właściwość `Protocols` ustanawia protokoły HTTP (`HttpProtocols`) włączone w punkcie końcowym połączenia lub na serwerze.</span><span class="sxs-lookup"><span data-stu-id="536eb-361">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="536eb-362">Przypisz wartość do właściwości `Protocols` z wyliczenia `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="536eb-362">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="536eb-363">`HttpProtocols` wartość wyliczenia</span><span class="sxs-lookup"><span data-stu-id="536eb-363">`HttpProtocols` enum value</span></span> | <span data-ttu-id="536eb-364">Dozwolony protokół połączenia</span><span class="sxs-lookup"><span data-stu-id="536eb-364">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="536eb-365">Tylko HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="536eb-365">HTTP/1.1 only.</span></span> <span data-ttu-id="536eb-366">Może być używany z protokołem TLS lub bez niego.</span><span class="sxs-lookup"><span data-stu-id="536eb-366">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="536eb-367">Tylko HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="536eb-367">HTTP/2 only.</span></span> <span data-ttu-id="536eb-368">Mogą być używane bez protokołu TLS tylko wtedy, gdy klient obsługuje [poprzedni tryb wiedzy](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="536eb-368">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="536eb-369">HTTP/1.1 i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="536eb-369">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="536eb-370">Protokół HTTP/2 wymaga od klienta wyboru protokołu HTTP/2 w uzgadnianiu [negocjowania protokołu TLS aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3) . w przeciwnym razie wartość domyślna połączenia to HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="536eb-370">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="536eb-371">Domyślna wartość `ListenOptions.Protocols` dla dowolnego punktu końcowego jest `HttpProtocols.Http1AndHttp2`.</span><span class="sxs-lookup"><span data-stu-id="536eb-371">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="536eb-372">Ograniczenia protokołu TLS dla protokołu HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="536eb-372">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="536eb-373">TLS w wersji 1,2 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="536eb-373">TLS version 1.2 or later</span></span>
* <span data-ttu-id="536eb-374">Ponowne negocjowanie wyłączone</span><span class="sxs-lookup"><span data-stu-id="536eb-374">Renegotiation disabled</span></span>
* <span data-ttu-id="536eb-375">Kompresja wyłączona</span><span class="sxs-lookup"><span data-stu-id="536eb-375">Compression disabled</span></span>
* <span data-ttu-id="536eb-376">Minimalne rozmiary tymczasowych kluczy wymiany:</span><span class="sxs-lookup"><span data-stu-id="536eb-376">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="536eb-377">Krzywa eliptyczna Diffie-Hellmana (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 BITS</span><span class="sxs-lookup"><span data-stu-id="536eb-377">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="536eb-378">Ograniczone pole Diffie-Hellmana (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bitów</span><span class="sxs-lookup"><span data-stu-id="536eb-378">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="536eb-379">Mechanizm szyfrowania nie został zabroniony</span><span class="sxs-lookup"><span data-stu-id="536eb-379">Cipher suite not blacklisted</span></span>

<span data-ttu-id="536eb-380">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; z krzywą eliptyczna P-256 &lbrack;`FIPS186`&rbrack; jest domyślnie obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="536eb-380">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="536eb-381">Poniższy przykład umożliwia nawiązywanie połączeń HTTP/1.1 i HTTP/2 na porcie 8000.</span><span class="sxs-lookup"><span data-stu-id="536eb-381">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="536eb-382">Połączenia są zabezpieczone przez protokół TLS przy użyciu podanego certyfikatu:</span><span class="sxs-lookup"><span data-stu-id="536eb-382">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="536eb-383">Używaj oprogramowania pośredniczącego do filtrowania uzgadniania protokołu TLS dla poszczególnych połączeń dla określonych szyfrów, jeśli jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="536eb-383">Use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers if required.</span></span>

<span data-ttu-id="536eb-384">Poniższy przykład zgłasza <xref:System.NotSupportedException> dla dowolnego algorytmu szyfrowania, który nie jest obsługiwany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="536eb-384">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="536eb-385">Alternatywnie można zdefiniować i porównać [ITlsHandshakeFeature. CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) z listą akceptowalnych mechanizmów szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="536eb-385">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="536eb-386">Nie jest używane szyfrowanie z algorytmem szyfrowania [CipherAlgorithmType. null](xref:System.Security.Authentication.CipherAlgorithmType) .</span><span class="sxs-lookup"><span data-stu-id="536eb-386">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseTlsFilter();
    });
});
```

```csharp
using System;
using System.Security.Authentication;
using Microsoft.AspNetCore.Connections.Features;

namespace Microsoft.AspNetCore.Connections
{
    public static class TlsFilterConnectionMiddlewareExtensions
    {
        public static IConnectionBuilder UseTlsFilter(
            this IConnectionBuilder builder)
        {
            return builder.Use((connection, next) =>
            {
                var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

                if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
                {
                    throw new NotSupportedException("Prohibited cipher: " +
                        tlsFeature.CipherAlgorithm);
                }

                return next();
            });
        }
    }
}
```

<span data-ttu-id="536eb-387">Filtrowanie połączeń można również skonfigurować za pomocą <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span><span class="sxs-lookup"><span data-stu-id="536eb-387">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

```csharp
// using System;
// using System.Net;
// using System.Security.Authentication;
// using Microsoft.AspNetCore.Connections;
// using Microsoft.AspNetCore.Connections.Features;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.Use((context, next) =>
        {
            var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

            if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
            {
                throw new NotSupportedException(
                    $"Prohibited cipher: {tlsFeature.CipherAlgorithm}");
            }

            return next();
        });
    });
});
```

<span data-ttu-id="536eb-388">W systemie Linux <xref:System.Net.Security.CipherSuitesPolicy> może służyć do filtrowania uzgadniania protokołu TLS dla poszczególnych połączeń:</span><span class="sxs-lookup"><span data-stu-id="536eb-388">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

```csharp
// using System.Net.Security;
// using Microsoft.AspNetCore.Hosting;
// using Microsoft.AspNetCore.Server.Kestrel.Core;
// using Microsoft.Extensions.DependencyInjection;
// using Microsoft.Extensions.Hosting;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        listenOptions.OnAuthenticate = (context, sslOptions) =>
        {
            sslOptions.CipherSuitesPolicy = new CipherSuitesPolicy(
                new[]
                {
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,
                    // ...
                });
        };
    });
});
```

<span data-ttu-id="536eb-389">*Ustawianie protokołu z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="536eb-389">*Set the protocol from configuration*</span></span>

<span data-ttu-id="536eb-390">`CreateDefaultBuilder` wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-390">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="536eb-391">W poniższym przykładzie pliku *appSettings. JSON* jako domyślny protokół połączenia dla wszystkich punktów końcowych:</span><span class="sxs-lookup"><span data-stu-id="536eb-391">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="536eb-392">Poniższy przykład *appSettings. JSON* ustanawia protokół połączeń HTTP/1.1 dla określonego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="536eb-392">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="536eb-393">Protokoły określone w wartościach zastąpienia kodu ustawione przez konfigurację.</span><span class="sxs-lookup"><span data-stu-id="536eb-393">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="536eb-394">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="536eb-394">Transport configuration</span></span>

<span data-ttu-id="536eb-395">Dla projektów, które wymagają użycia Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span><span class="sxs-lookup"><span data-stu-id="536eb-395">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="536eb-396">Dodaj zależność dla pakietu [Microsoft. AspNetCore. Server. Kestrel. transport. Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="536eb-396">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="536eb-397">Wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> na `IWebHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="536eb-397">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="536eb-398">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="536eb-398">URL prefixes</span></span>

<span data-ttu-id="536eb-399">W przypadku korzystania z `UseUrls`, `--urls` argumentu wiersza polecenia, klucza konfiguracji hosta `urls` lub zmiennej środowiskowej `ASPNETCORE_URLS`, prefiksy adresów URL mogą mieć jeden z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="536eb-399">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="536eb-400">Tylko prefiksy adresów URL HTTP są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-400">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="536eb-401">Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązań adresów URL przy użyciu `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="536eb-401">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="536eb-402">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="536eb-402">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="536eb-403">`0.0.0.0` jest szczególnym przypadkiem, który wiąże się ze wszystkimi adresami IPv4.</span><span class="sxs-lookup"><span data-stu-id="536eb-403">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="536eb-404">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="536eb-404">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="536eb-405">`[::]` to odpowiednik IPv6 `0.0.0.0`IPv4.</span><span class="sxs-lookup"><span data-stu-id="536eb-405">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="536eb-406">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="536eb-406">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="536eb-407">Nazwy hostów, `*`i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="536eb-407">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="536eb-408">Wszystkie nie są rozpoznawane jako prawidłowy adres IP lub `localhost` są powiązane z protokołem IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="536eb-408">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="536eb-409">Aby powiązać różne nazwy hostów z różnymi ASP.NET Core aplikacjami na tym samym porcie, użyj [protokołu HTTP. sys](xref:fundamentals/servers/httpsys) lub odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="536eb-409">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="536eb-410">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="536eb-410">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="536eb-411">Nazwa hosta `localhost` z numerem portu lub adresem IP sprzężenia zwrotnego z numerem portu</span><span class="sxs-lookup"><span data-stu-id="536eb-411">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="536eb-412">Gdy `localhost` jest określony, Kestrel próbuje powiązać z interfejsami sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="536eb-412">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="536eb-413">Jeśli żądany port jest używany przez inną usługę w dowolnym interfejsie sprzężenia zwrotnego, uruchomienie Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="536eb-413">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="536eb-414">Jeśli którykolwiek z innych przyczyn interfejsu sprzężenia zwrotnego jest niedostępny (najczęściej jest to spowodowane tym, że protokół IPv6 nie jest obsługiwany), Kestrel rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="536eb-414">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="536eb-415">Filtrowanie hostów</span><span class="sxs-lookup"><span data-stu-id="536eb-415">Host filtering</span></span>

<span data-ttu-id="536eb-416">Chociaż usługa Kestrel obsługuje konfigurację na podstawie prefiksów, takich jak `http://example.com:5000`, Kestrel w znacznym stopniu ignoruje nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="536eb-416">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="536eb-417">`localhost` hosta jest specjalnym przypadkiem używanym do tworzenia powiązań z adresami sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="536eb-417">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="536eb-418">Każdy host poza jawnym adresem IP tworzy powiązanie ze wszystkimi publicznymi adresami IP.</span><span class="sxs-lookup"><span data-stu-id="536eb-418">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="536eb-419">nagłówki `Host` nie są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="536eb-419">`Host` headers aren't validated.</span></span>

<span data-ttu-id="536eb-420">Aby obejść ten sposób, użyj oprogramowania pośredniczącego filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="536eb-420">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="536eb-421">Oprogramowanie pośredniczące do filtrowania hosta jest dostarczane przez pakiet [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , który jest niejawnie dostarczany dla ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="536eb-421">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="536eb-422">Oprogramowanie pośredniczące jest dodawane przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="536eb-422">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="536eb-423">Programowe filtrowanie hosta jest domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="536eb-423">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="536eb-424">Aby włączyć oprogramowanie pośredniczące, zdefiniuj klucz `AllowedHosts` w pliku *appSettings. json*/*appSettings.\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="536eb-424">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="536eb-425">Wartość to rozdzielana średnikami lista nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="536eb-425">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="536eb-426">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="536eb-426">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="536eb-427">W przypadku [przekierowanych nagłówków oprogramowanie pośredniczące](xref:host-and-deploy/proxy-load-balancer) ma również opcję <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="536eb-427">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="536eb-428">Przekazane nagłówki — oprogramowanie pośredniczące i filtrowanie hostów oprogramowanie pośredniczące ma podobną funkcjonalność dla różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="536eb-428">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="536eb-429">Ustawienie `AllowedHosts` z przekierowanymi nagłówkami — oprogramowanie pośredniczące jest odpowiednie, gdy nagłówek `Host` nie jest zachowywany podczas przekazywania żądań z odwrotnym serwerem proxy lub modułem równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="536eb-429">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="536eb-430">Ustawienie `AllowedHosts` przy użyciu oprogramowania pośredniczącego filtrowania hosta jest odpowiednie, gdy Kestrel jest używany jako publiczny serwer graniczny lub gdy `Host` nagłówek jest bezpośrednio przekazywany.</span><span class="sxs-lookup"><span data-stu-id="536eb-430">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="536eb-431">Aby uzyskać więcej informacji na temat przekierowanych nagłówków, należy zapoznać się z tematem <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="536eb-431">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="536eb-432">Kestrel to Międzyplatformowy [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="536eb-432">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="536eb-433">Kestrel to serwer sieci Web, który jest domyślnie uwzględniony w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="536eb-433">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="536eb-434">Kestrel obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="536eb-434">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="536eb-435">HTTPS</span><span class="sxs-lookup"><span data-stu-id="536eb-435">HTTPS</span></span>
* <span data-ttu-id="536eb-436">Nieprzezroczyste uaktualnienie używane do włączania obsługi obiektów [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="536eb-436">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="536eb-437">Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx</span><span class="sxs-lookup"><span data-stu-id="536eb-437">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="536eb-438">HTTP/2 (z wyjątkiem macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="536eb-438">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="536eb-439">&dagger;HTTP/2 będą obsługiwane w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="536eb-439">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="536eb-440">Kestrel jest obsługiwana na wszystkich platformach i wersjach obsługiwanych przez platformę .NET Core.</span><span class="sxs-lookup"><span data-stu-id="536eb-440">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="536eb-441">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="536eb-441">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="536eb-442">Obsługa protokołu HTTP/2</span><span class="sxs-lookup"><span data-stu-id="536eb-442">HTTP/2 support</span></span>

<span data-ttu-id="536eb-443">[Protokół HTTP/2](https://httpwg.org/specs/rfc7540.html) jest dostępny dla aplikacji ASP.NET Core, jeśli spełnione są następujące wymagania podstawowe:</span><span class="sxs-lookup"><span data-stu-id="536eb-443">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="536eb-444">&dagger; systemu operacyjnego</span><span class="sxs-lookup"><span data-stu-id="536eb-444">Operating system&dagger;</span></span>
  * <span data-ttu-id="536eb-445">Windows Server 2016/Windows 10 lub nowszy&Dagger;</span><span class="sxs-lookup"><span data-stu-id="536eb-445">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="536eb-446">Linux z OpenSSL 1.0.2 lub nowszym (na przykład Ubuntu 16,04 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="536eb-446">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="536eb-447">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="536eb-447">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="536eb-448">Połączenie [negocjowania protokołu warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="536eb-448">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="536eb-449">Protokół TLS 1.2 lub nowszej połączenia</span><span class="sxs-lookup"><span data-stu-id="536eb-449">TLS 1.2 or later connection</span></span>

<span data-ttu-id="536eb-450">&dagger;HTTP/2 będą obsługiwane w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="536eb-450">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="536eb-451">&Dagger;Kestrel ma ograniczoną obsługę protokołu HTTP/2 w systemie Windows Server 2012 R2 i Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="536eb-451">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="536eb-452">Obsługa jest ograniczona, ponieważ lista obsługiwanych mechanizmów szyfrowania TLS dostępnych w tych systemach operacyjnych jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="536eb-452">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="536eb-453">Do zabezpieczenia połączeń TLS może być wymagany certyfikat wygenerowany przy użyciu algorytmu podpisu cyfrowego (ECDSA) krzywej eliptycznej.</span><span class="sxs-lookup"><span data-stu-id="536eb-453">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="536eb-454">Jeśli zostanie nawiązane połączenie HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="536eb-454">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="536eb-455">Protokół HTTP/2 jest domyślnie wyłączony.</span><span class="sxs-lookup"><span data-stu-id="536eb-455">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="536eb-456">Więcej informacji na temat konfiguracji można znaleźć w sekcjach [Kestrel Options](#kestrel-options) and [ListenOptions. Protocols](#listenoptionsprotocols) .</span><span class="sxs-lookup"><span data-stu-id="536eb-456">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="536eb-457">Kiedy używać Kestrel z zwrotnym serwerem proxy</span><span class="sxs-lookup"><span data-stu-id="536eb-457">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="536eb-458">Kestrel może być używana przez siebie lub z *odwrotnym serwerem proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org)lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="536eb-458">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="536eb-459">Odwrotny serwer proxy odbiera żądania HTTP z sieci i przekazuje je do usługi Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-459">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="536eb-460">Kestrel używany jako serwer sieci Web z krawędzią (dostępną z Internetu):</span><span class="sxs-lookup"><span data-stu-id="536eb-460">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem bez serwera proxy zwrotnego](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="536eb-462">Kestrel używany w konfiguracji zwrotnego serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="536eb-462">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel komunikuje się pośrednio z Internetem za pomocą odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="536eb-464">Konfiguracja, z serwerem proxy odwrotnych lub bez niego, jest obsługiwaną konfiguracją hostingu.</span><span class="sxs-lookup"><span data-stu-id="536eb-464">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="536eb-465">Kestrel używany jako serwer graniczny bez serwera proxy odwrotnego nie obsługuje udostępniania tego samego adresu IP i portu między wieloma procesami.</span><span class="sxs-lookup"><span data-stu-id="536eb-465">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="536eb-466">Gdy Kestrel jest skonfigurowany do nasłuchiwania na porcie, Kestrel obsługuje cały ruch dla tego portu bez względu na żądania `Host` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="536eb-466">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="536eb-467">Zwrotny serwer proxy, który może udostępniać porty, ma możliwość przekazywania żądań do Kestrel na unikatowym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="536eb-467">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="536eb-468">Nawet jeśli odwrotny serwer proxy nie jest wymagany, może to być dobry wybór przy użyciu odwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="536eb-468">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="536eb-469">Zwrotny serwer proxy:</span><span class="sxs-lookup"><span data-stu-id="536eb-469">A reverse proxy:</span></span>

* <span data-ttu-id="536eb-470">Może ograniczać uwidoczniony obszar publicznej powierzchni aplikacji, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="536eb-470">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="536eb-471">Zapewnienie dodatkowej warstwy konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="536eb-471">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="536eb-472">Integracja z istniejącą infrastrukturą może być lepsza.</span><span class="sxs-lookup"><span data-stu-id="536eb-472">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="536eb-473">Uprość Równoważenie obciążenia i konfigurację komunikacji zabezpieczonej (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="536eb-473">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="536eb-474">Tylko serwer zwrotny proxy wymaga certyfikatu X. 509, a serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="536eb-474">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="536eb-475">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="536eb-475">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="536eb-476">Jak używać Kestrel w aplikacjach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="536eb-476">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="536eb-477">Pakiet [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="536eb-477">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="536eb-478">Szablony projektów ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-478">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="536eb-479">W *program.cs*kod szablonu wywołuje <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> w tle.</span><span class="sxs-lookup"><span data-stu-id="536eb-479">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="536eb-480">Aby uzyskać więcej informacji na temat `CreateDefaultBuilder` i kompilowania hosta, zobacz sekcję *Konfigurowanie hosta* w programie <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="536eb-480">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

<span data-ttu-id="536eb-481">Aby zapewnić dodatkową konfigurację po wywołaniu `CreateDefaultBuilder`, użyj `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="536eb-481">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="536eb-482">Jeśli aplikacja nie wywoła `CreateDefaultBuilder`, aby skonfigurować hosta, wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **przed** wywołaniem `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="536eb-482">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="536eb-483">Opcje Kestrel</span><span class="sxs-lookup"><span data-stu-id="536eb-483">Kestrel options</span></span>

<span data-ttu-id="536eb-484">Serwer sieci Web Kestrel ma opcje konfiguracji ograniczeń, które są szczególnie przydatne w przypadku wdrożeń mających dostęp do Internetu.</span><span class="sxs-lookup"><span data-stu-id="536eb-484">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="536eb-485">Ustaw ograniczenia we właściwości <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="536eb-485">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="536eb-486">Właściwość `Limits` przechowuje wystąpienie klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="536eb-486">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="536eb-487">W poniższych przykładach użyto przestrzeni nazw <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="536eb-487">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="536eb-488">Opcje Kestrel, które są konfigurowane w C# kodzie w poniższych przykładach, można również ustawić za pomocą [dostawcy konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="536eb-488">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="536eb-489">Na przykład dostawca konfiguracji plików może załadować konfigurację Kestrel z pliku *appSettings. JSON* lub *appSettings. { Environment} plik JSON* :</span><span class="sxs-lookup"><span data-stu-id="536eb-489">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

<span data-ttu-id="536eb-490">Skorzystaj z **jednej** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="536eb-490">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="536eb-491">Skonfiguruj Kestrel w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="536eb-491">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="536eb-492">Wstrzyknąć wystąpienie `IConfiguration` do klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="536eb-492">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="536eb-493">W poniższym przykładzie przyjęto założenie, że wprowadzona konfiguracja jest przypisana do właściwości `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="536eb-493">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="536eb-494">W `Startup.ConfigureServices`Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-494">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="536eb-495">Skonfiguruj Kestrel podczas kompilowania hosta:</span><span class="sxs-lookup"><span data-stu-id="536eb-495">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="536eb-496">W programie *program.cs*Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="536eb-496">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

<span data-ttu-id="536eb-497">Obie powyższe podejścia współpracują z dowolnym [dostawcą konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="536eb-497">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="536eb-498">Limit czasu utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="536eb-498">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="536eb-499">Pobiera lub ustawia [limit czasu utrzymywania aktywności](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="536eb-499">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="536eb-500">Wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="536eb-500">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="536eb-501">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="536eb-501">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="536eb-502">Maksymalna liczba współbieżnych otwartych połączeń TCP można ustawić dla całej aplikacji przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="536eb-502">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="536eb-503">Istnieje oddzielny limit połączeń, które zostały uaktualnione z protokołu HTTP lub HTTPS do innego protokołu (na przykład w żądaniu usługi WebSockets).</span><span class="sxs-lookup"><span data-stu-id="536eb-503">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="536eb-504">Po uaktualnieniu połączenia nie jest ono wliczane do limitu `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="536eb-504">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="536eb-505">Maksymalna liczba połączeń jest domyślnie nieograniczona (null).</span><span class="sxs-lookup"><span data-stu-id="536eb-505">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="536eb-506">Maksymalny rozmiar treści żądania</span><span class="sxs-lookup"><span data-stu-id="536eb-506">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="536eb-507">Domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="536eb-507">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="536eb-508">Zalecanym podejściem do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie atrybutu <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> dla metody akcji:</span><span class="sxs-lookup"><span data-stu-id="536eb-508">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="536eb-509">Oto przykład, który pokazuje, jak skonfigurować ograniczenie dla aplikacji w każdym żądaniu:</span><span class="sxs-lookup"><span data-stu-id="536eb-509">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="536eb-510">Zastąp ustawienie określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="536eb-510">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="536eb-511">Wyjątek jest generowany, jeśli aplikacja skonfiguruje limit żądania po rozpoczęciu uruchamiania aplikacji w celu odczytania żądania.</span><span class="sxs-lookup"><span data-stu-id="536eb-511">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="536eb-512">Istnieje właściwość `IsReadOnly`, która wskazuje, czy właściwość `MaxRequestBodySize` jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="536eb-512">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="536eb-513">Gdy aplikacja jest uruchamiana [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model) [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru treści żądania Kestrel jest wyłączony, ponieważ program IIS już ustawia limit.</span><span class="sxs-lookup"><span data-stu-id="536eb-513">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="536eb-514">Minimalna stawka danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="536eb-514">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="536eb-515">Kestrel sprawdza co sekundę, jeśli dane są odbierane z określoną szybkością w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="536eb-515">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="536eb-516">Jeśli szybkość spadnie poniżej wartości minimalnej, upłynął limit czasu połączenia. Okres prolongaty to czas, przez który Kestrel nadaje klientowi zwiększenie jego szybkości wysyłania do minimum; Ta częstotliwość nie jest sprawdzana w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="536eb-516">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="536eb-517">Okres prolongaty pozwala uniknąć porzucania połączeń, które początkowo wysyłają dane z niską szybkością.</span><span class="sxs-lookup"><span data-stu-id="536eb-517">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="536eb-518">Domyślna stawka minimalna to 240 bajtów na sekundę z 5-sekundowym okresem prolongaty.</span><span class="sxs-lookup"><span data-stu-id="536eb-518">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="536eb-519">Minimalna stawka dotyczy także odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="536eb-519">A minimum rate also applies to the response.</span></span> <span data-ttu-id="536eb-520">Kod określający limit żądań i limit odpowiedzi jest taki sam, chyba że ma `RequestBody` lub `Response` w nazwach właściwości i interfejsów.</span><span class="sxs-lookup"><span data-stu-id="536eb-520">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="536eb-521">Oto przykład pokazujący sposób konfigurowania minimalnych stawek danych w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="536eb-521">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="536eb-522">Przesłoń minimalne limity szybkości dla żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="536eb-522">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="536eb-523">Żadna funkcja współczynnika, do której odwołuje się poprzednia próba nie jest obecna w `HttpContext.Features` dla żądań HTTP/2, ponieważ Modyfikowanie limitów szybkości dla każdego żądania nie jest obsługiwane w przypadku protokołu HTTP/2 ze względu na obsługę multipleksera żądania.</span><span class="sxs-lookup"><span data-stu-id="536eb-523">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="536eb-524">Limity szybkości dla całego serwera skonfigurowane za pomocą `KestrelServerOptions.Limits` nadal mają zastosowanie do połączeń HTTP/1. x i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="536eb-524">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="536eb-525">Limit czasu nagłówków żądań</span><span class="sxs-lookup"><span data-stu-id="536eb-525">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="536eb-526">Pobiera lub ustawia maksymalny czas, przez jaki serwer spędza nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="536eb-526">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="536eb-527">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="536eb-527">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="536eb-528">Maksymalna liczba strumieni na połączenie</span><span class="sxs-lookup"><span data-stu-id="536eb-528">Maximum streams per connection</span></span>

<span data-ttu-id="536eb-529">`Http2.MaxStreamsPerConnection` ogranicza liczbę współbieżnych strumieni żądań dla połączenia HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="536eb-529">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="536eb-530">Odrzucone strumienie są nadmiarowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-530">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="536eb-531">Wartość domyślna to 100.</span><span class="sxs-lookup"><span data-stu-id="536eb-531">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="536eb-532">Rozmiar tabeli nagłówka</span><span class="sxs-lookup"><span data-stu-id="536eb-532">Header table size</span></span>

<span data-ttu-id="536eb-533">Dekoder HPACK dekompresuje nagłówki HTTP dla połączeń HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="536eb-533">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="536eb-534">`Http2.HeaderTableSize` ogranicza rozmiar tabeli kompresji nagłówka używanej przez dekoder HPACK.</span><span class="sxs-lookup"><span data-stu-id="536eb-534">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="536eb-535">Wartość jest podawana w oktetach i musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="536eb-535">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="536eb-536">Wartość domyślna to 4096.</span><span class="sxs-lookup"><span data-stu-id="536eb-536">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="536eb-537">Maksymalny rozmiar ramki</span><span class="sxs-lookup"><span data-stu-id="536eb-537">Maximum frame size</span></span>

<span data-ttu-id="536eb-538">`Http2.MaxFrameSize` wskazuje maksymalny rozmiar ładunku ramki połączenia HTTP/2 do odebrania.</span><span class="sxs-lookup"><span data-stu-id="536eb-538">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="536eb-539">Wartość jest podawana w oktetach i musi należeć do przedziału od 2 ^ 14 (16 384) do 2 ^ 24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="536eb-539">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="536eb-540">Wartość domyślna to 2 ^ 14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="536eb-540">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="536eb-541">Maksymalny rozmiar nagłówka żądania</span><span class="sxs-lookup"><span data-stu-id="536eb-541">Maximum request header size</span></span>

<span data-ttu-id="536eb-542">`Http2.MaxRequestHeaderFieldSize` wskazuje maksymalny dozwolony rozmiar w oktetach wartości nagłówka żądania.</span><span class="sxs-lookup"><span data-stu-id="536eb-542">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="536eb-543">Ten limit dotyczy zarówno nazwy, jak i wartości razem w skompresowanych i nieskompresowanych reprezentacji.</span><span class="sxs-lookup"><span data-stu-id="536eb-543">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="536eb-544">Wartość musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="536eb-544">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="536eb-545">Wartość domyślna to 8 192.</span><span class="sxs-lookup"><span data-stu-id="536eb-545">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="536eb-546">Początkowy rozmiar okna połączenia</span><span class="sxs-lookup"><span data-stu-id="536eb-546">Initial connection window size</span></span>

<span data-ttu-id="536eb-547">`Http2.InitialConnectionWindowSize` wskazuje maksymalne dane dotyczące treści żądania w bajtach, które bufory serwera w tym samym czasie są agregowane dla wszystkich żądań (strumieni) na połączenie.</span><span class="sxs-lookup"><span data-stu-id="536eb-547">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="536eb-548">Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="536eb-548">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="536eb-549">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="536eb-549">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="536eb-550">Wartość domyślna to 128 KB (131 072).</span><span class="sxs-lookup"><span data-stu-id="536eb-550">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="536eb-551">Rozmiar początkowego okna strumienia</span><span class="sxs-lookup"><span data-stu-id="536eb-551">Initial stream window size</span></span>

<span data-ttu-id="536eb-552">`Http2.InitialStreamWindowSize` wskazuje maksymalne dane dotyczące treści żądania w bajtach bufory serwerów w jednym momencie na żądanie (Stream).</span><span class="sxs-lookup"><span data-stu-id="536eb-552">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="536eb-553">Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="536eb-553">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="536eb-554">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="536eb-554">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="536eb-555">Wartość domyślna to 96 KB (98 304).</span><span class="sxs-lookup"><span data-stu-id="536eb-555">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="536eb-556">Synchroniczne we/wy</span><span class="sxs-lookup"><span data-stu-id="536eb-556">Synchronous IO</span></span>

<span data-ttu-id="536eb-557"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> kontroluje, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="536eb-557"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="536eb-558">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="536eb-558">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="536eb-559">Duża liczba blokowania synchronicznych operacji we/wy może prowadzić do zablokowania puli wątków, co sprawia, że aplikacja nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="536eb-559">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="536eb-560">Włącz `AllowSynchronousIO` tylko w przypadku korzystania z biblioteki, która nie obsługuje asynchronicznych operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="536eb-560">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="536eb-561">Poniższy przykład włącza synchroniczną operację we/wy:</span><span class="sxs-lookup"><span data-stu-id="536eb-561">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="536eb-562">Aby uzyskać informacje o innych opcjach i ograniczeniach Kestrel, zobacz:</span><span class="sxs-lookup"><span data-stu-id="536eb-562">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="536eb-563">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="536eb-563">Endpoint configuration</span></span>

<span data-ttu-id="536eb-564">Domyślnie ASP.NET Core wiąże się z:</span><span class="sxs-lookup"><span data-stu-id="536eb-564">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="536eb-565">`https://localhost:5001` (gdy obecny jest lokalny certyfikat programistyczny)</span><span class="sxs-lookup"><span data-stu-id="536eb-565">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="536eb-566">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="536eb-566">Specify URLs using the:</span></span>

* <span data-ttu-id="536eb-567">`ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="536eb-567">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="536eb-568">`--urls` argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="536eb-568">`--urls` command-line argument.</span></span>
* <span data-ttu-id="536eb-569">klucz konfiguracji hosta `urls`.</span><span class="sxs-lookup"><span data-stu-id="536eb-569">`urls` host configuration key.</span></span>
* <span data-ttu-id="536eb-570">Metoda rozszerzenia `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="536eb-570">`UseUrls` extension method.</span></span>

<span data-ttu-id="536eb-571">Wartość podana przy użyciu tych metod może być jednym lub większą liczbą punktów końcowych HTTP i HTTPS (HTTPS, jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="536eb-571">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="536eb-572">Skonfiguruj wartość jako listę rozdzieloną średnikami (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="536eb-572">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="536eb-573">Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwera](xref:fundamentals/host/web-host#server-urls) i [Zastąp konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="536eb-573">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="536eb-574">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="536eb-574">A development certificate is created:</span></span>

* <span data-ttu-id="536eb-575">Po zainstalowaniu [zestaw .NET Core SDK](/dotnet/core/sdk) .</span><span class="sxs-lookup"><span data-stu-id="536eb-575">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="536eb-576">Do utworzenia certyfikatu służy [Narzędzie dev-certs](xref:aspnetcore-2.1#https) .</span><span class="sxs-lookup"><span data-stu-id="536eb-576">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="536eb-577">Niektóre przeglądarki wymagają przyznania jawnego uprawnienia do zaufania do lokalnego certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="536eb-577">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="536eb-578">Szablony projektu konfigurują aplikacje do uruchamiania domyślnie przy użyciu protokołu HTTPS i obejmują [przekierowania https i obsługę HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="536eb-578">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="536eb-579">Wywołaj metody <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> w <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, aby skonfigurować prefiksy i porty adresów URL dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-579">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="536eb-580">`UseUrls`, `--urls` argument wiersza polecenia, klucz konfiguracji hosta `urls` oraz zmienna środowiskowa `ASPNETCORE_URLS` również działają, ale mają ograniczenia wymienione w dalszej części tej sekcji (certyfikat domyślny musi być dostępny do konfiguracji punktu końcowego HTTPS).</span><span class="sxs-lookup"><span data-stu-id="536eb-580">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="536eb-581">Konfiguracja `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="536eb-581">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="536eb-582">ConfigureEndpointDefaults (Akcja\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="536eb-582">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="536eb-583">Określa `Action` konfiguracji do uruchomienia dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="536eb-583">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="536eb-584">Wywoływanie `ConfigureEndpointDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="536eb-584">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="536eb-585">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="536eb-585">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="536eb-586">ConfigureHttpsDefaults (Akcja\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="536eb-586">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="536eb-587">Określa `Action` konfiguracji do uruchomienia dla każdego punktu końcowego HTTPS.</span><span class="sxs-lookup"><span data-stu-id="536eb-587">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="536eb-588">Wywoływanie `ConfigureHttpsDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="536eb-588">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="536eb-589">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="536eb-589">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>


### <a name="configureiconfiguration"></a><span data-ttu-id="536eb-590">Konfiguruj (IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="536eb-590">Configure(IConfiguration)</span></span>

<span data-ttu-id="536eb-591">Tworzy moduł ładujący konfigurację służący do konfigurowania Kestrel, który pobiera <xref:Microsoft.Extensions.Configuration.IConfiguration> jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-591">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="536eb-592">Konfiguracja musi być objęta zakresem sekcji konfiguracji dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-592">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="536eb-593">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="536eb-593">ListenOptions.UseHttps</span></span>

<span data-ttu-id="536eb-594">Skonfiguruj Kestrel do korzystania z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="536eb-594">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="536eb-595">rozszerzenia `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="536eb-595">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="536eb-596">`UseHttps` &ndash; skonfigurować Kestrel do używania protokołu HTTPS z certyfikatem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="536eb-596">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="536eb-597">Zgłasza wyjątek, jeśli nie został skonfigurowany żaden certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="536eb-597">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="536eb-598">`ListenOptions.UseHttps` parametry:</span><span class="sxs-lookup"><span data-stu-id="536eb-598">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="536eb-599">`filename` to ścieżka i nazwa pliku certyfikatu odnosząca się do katalogu zawierającego pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="536eb-599">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="536eb-600">`password` to hasło wymagane do uzyskania dostępu do danych certyfikatu X. 509.</span><span class="sxs-lookup"><span data-stu-id="536eb-600">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="536eb-601">`configureOptions` jest `Action`, aby skonfigurować `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="536eb-601">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="536eb-602">Zwraca `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="536eb-602">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="536eb-603">`storeName` to magazyn certyfikatów, z którego ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="536eb-603">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="536eb-604">`subject` to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="536eb-604">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="536eb-605">`allowInvalid` wskazuje, czy należy wziąć pod uwagę nieprawidłowe certyfikaty, takie jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="536eb-605">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="536eb-606">`location` to lokalizacja magazynu, z której ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="536eb-606">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="536eb-607">`serverCertificate` to certyfikat X. 509.</span><span class="sxs-lookup"><span data-stu-id="536eb-607">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="536eb-608">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="536eb-608">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="536eb-609">Należy podać co najmniej certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="536eb-609">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="536eb-610">Obsługiwane konfiguracje opisane dalej:</span><span class="sxs-lookup"><span data-stu-id="536eb-610">Supported configurations described next:</span></span>

* <span data-ttu-id="536eb-611">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="536eb-611">No configuration</span></span>
* <span data-ttu-id="536eb-612">Zastąp domyślny certyfikat z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="536eb-612">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="536eb-613">Zmień wartości domyślne w kodzie</span><span class="sxs-lookup"><span data-stu-id="536eb-613">Change the defaults in code</span></span>

<span data-ttu-id="536eb-614">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="536eb-614">*No configuration*</span></span>

<span data-ttu-id="536eb-615">Kestrel nasłuchuje na `http://localhost:5000` i `https://localhost:5001` (jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="536eb-615">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="536eb-616">*Zastąp domyślny certyfikat z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="536eb-616">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="536eb-617">`CreateDefaultBuilder` wywołania `Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-617">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="536eb-618">Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępny dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-618">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="536eb-619">Konfigurowanie wielu punktów końcowych, w tym adresów URL i certyfikatów do użycia, z pliku znajdującego się na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="536eb-619">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="536eb-620">W poniższym przykładzie pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="536eb-620">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="536eb-621">Ustaw **AllowInvalid** na `true`, aby zezwolić na korzystanie z nieprawidłowych certyfikatów (na przykład certyfikatów z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="536eb-621">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="536eb-622">Wszystkie punkty końcowe HTTPS, które nie określają certyfikatu (**HttpsDefaultCert** w poniższym przykładzie) powracają do certyfikatu zdefiniowanego w obszarze **Certyfikaty** > **domyślnym** lub certyfikatem deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="536eb-622">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="536eb-623">Alternatywą dla korzystania z **ścieżki** i **hasła** dla dowolnego węzła certyfikatu jest określenie certyfikatu przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="536eb-623">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="536eb-624">Na przykład **certyfikaty** > **domyślnego** certyfikatu można określić jako:</span><span class="sxs-lookup"><span data-stu-id="536eb-624">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="536eb-625">Uwagi dotyczące schematu:</span><span class="sxs-lookup"><span data-stu-id="536eb-625">Schema notes:</span></span>

* <span data-ttu-id="536eb-626">W nazwach punktów końcowych nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="536eb-626">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="536eb-627">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-627">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="536eb-628">Dla każdego punktu końcowego jest wymagany parametr `Url`.</span><span class="sxs-lookup"><span data-stu-id="536eb-628">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="536eb-629">Format tego parametru jest taki sam jak parametr konfiguracji `Urls` najwyższego poziomu, z tą różnicą, że jest ograniczony do pojedynczej wartości.</span><span class="sxs-lookup"><span data-stu-id="536eb-629">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="536eb-630">Te punkty końcowe zastępują te zdefiniowane w konfiguracji `Urls` najwyższego poziomu zamiast dodawać do nich.</span><span class="sxs-lookup"><span data-stu-id="536eb-630">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="536eb-631">Punkty końcowe zdefiniowane w kodzie za pośrednictwem `Listen` kumulują się z punktami końcowymi zdefiniowanymi w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="536eb-631">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="536eb-632">Sekcja `Certificate` jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="536eb-632">The `Certificate` section is optional.</span></span> <span data-ttu-id="536eb-633">Jeśli sekcja `Certificate` nie zostanie określona, są używane wartości domyślne zdefiniowane we wcześniejszych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="536eb-633">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="536eb-634">Jeśli żadne wartości domyślne nie są dostępne, serwer zgłasza wyjątek i nie może się uruchomić.</span><span class="sxs-lookup"><span data-stu-id="536eb-634">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="536eb-635">Sekcja `Certificate` obsługuje zarówno **ścieżkę**&ndash;**hasło** , jak i **temat**&ndash;certyfikatów **magazynu** .</span><span class="sxs-lookup"><span data-stu-id="536eb-635">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="536eb-636">W ten sposób można zdefiniować dowolną liczbę punktów końcowych, dopóki nie spowoduje to konfliktów portów.</span><span class="sxs-lookup"><span data-stu-id="536eb-636">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="536eb-637">`options.Configure(context.Configuration.GetSection("{SECTION}"))` zwraca `KestrelConfigurationLoader` z metodą `.Endpoint(string name, listenOptions => { })`, której można użyć do uzupełnienia ustawień skonfigurowanego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="536eb-637">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="536eb-638">dostęp do `KestrelServerOptions.ConfigurationLoader` można uzyskać, aby kontynuować iterację istniejącego modułu ładującego, takiego jak dostarczony przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="536eb-638">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="536eb-639">Sekcja konfiguracji dla każdego punktu końcowego jest dostępna w opcjach w `Endpoint` metodzie, aby można było odczytać ustawienia niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-639">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="536eb-640">Można załadować wiele konfiguracji, wywołując `options.Configure(context.Configuration.GetSection("{SECTION}"))` ponownie z inną sekcją.</span><span class="sxs-lookup"><span data-stu-id="536eb-640">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="536eb-641">Używana jest tylko Ostatnia konfiguracja, chyba że `Load` jest jawnie wywoływana w poprzednich wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="536eb-641">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="536eb-642">Pakiet nie wywołuje `Load`, tak aby jego sekcja konfiguracji domyślnej mogła zostać zamieniona.</span><span class="sxs-lookup"><span data-stu-id="536eb-642">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="536eb-643">`KestrelConfigurationLoader` odzwierciedla `Listen`ą rodzinę interfejsów API z `KestrelServerOptions` jako przeciążania `Endpoint`, dlatego punkty końcowe kodu i konfiguracji można skonfigurować w tym samym miejscu.</span><span class="sxs-lookup"><span data-stu-id="536eb-643">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="536eb-644">Te przeciążenia nie używają nazw i używają tylko ustawień domyślnych z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="536eb-644">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="536eb-645">*Zmień wartości domyślne w kodzie*</span><span class="sxs-lookup"><span data-stu-id="536eb-645">*Change the defaults in code*</span></span>

<span data-ttu-id="536eb-646">`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` mogą służyć do zmiany domyślnych ustawień dla `ListenOptions` i `HttpsConnectionAdapterOptions`, w tym przesłanianie domyślnego certyfikatu określonego w poprzednim scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="536eb-646">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="536eb-647">przed skonfigurowaniem punktów końcowych należy wywołać `ConfigureEndpointDefaults` i `ConfigureHttpsDefaults`.</span><span class="sxs-lookup"><span data-stu-id="536eb-647">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="536eb-648">*Obsługa usługi Kestrel dla SNI*</span><span class="sxs-lookup"><span data-stu-id="536eb-648">*Kestrel support for SNI*</span></span>

<span data-ttu-id="536eb-649">[Oznaczanie nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do hostowania wielu domen na tym samym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="536eb-649">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="536eb-650">Aby SNI działał, klient wysyła nazwę hosta dla bezpiecznej sesji do serwera podczas uzgadniania TLS, aby serwer mógł zapewnić prawidłowy certyfikat.</span><span class="sxs-lookup"><span data-stu-id="536eb-650">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="536eb-651">Klient korzysta z dostarczonego certyfikatu w celu zaszyfrowania komunikacji z serwerem podczas bezpiecznej sesji, która następuje po uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="536eb-651">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="536eb-652">Kestrel obsługuje SNI za pośrednictwem wywołania zwrotnego `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="536eb-652">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="536eb-653">Wywołanie zwrotne jest wywoływane jednokrotnie dla każdego połączenia, aby umożliwić aplikacji inspekcję nazwy hosta i wybieranie odpowiedniego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="536eb-653">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="536eb-654">Obsługa SNI wymaga:</span><span class="sxs-lookup"><span data-stu-id="536eb-654">SNI support requires:</span></span>

* <span data-ttu-id="536eb-655">Uruchomione na platformie docelowej `netcoreapp2.1` lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="536eb-655">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="536eb-656">W `net461` lub później wywołanie zwrotne jest wywoływane, ale `name` jest zawsze `null`.</span><span class="sxs-lookup"><span data-stu-id="536eb-656">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="536eb-657">`name` jest również `null`, jeśli klient nie poda parametru nazwy hosta w uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="536eb-657">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="536eb-658">Wszystkie witryny sieci Web działają na tym samym wystąpieniu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-658">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="536eb-659">Kestrel nie obsługuje udostępniania adresu IP i portu w wielu wystąpieniach bez zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="536eb-659">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="536eb-660">Rejestrowanie połączeń</span><span class="sxs-lookup"><span data-stu-id="536eb-660">Connection logging</span></span>

<span data-ttu-id="536eb-661">Wywołaj <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>, aby emitować dzienniki poziomu debugowania dla komunikacji na poziomie bajtów w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="536eb-661">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="536eb-662">Rejestrowanie połączeń ułatwia rozwiązywanie problemów z komunikacją na niskim poziomie, na przykład podczas szyfrowania TLS i za serwerami proxy.</span><span class="sxs-lookup"><span data-stu-id="536eb-662">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="536eb-663">Jeśli `UseConnectionLogging` jest umieszczony przed `UseHttps`, szyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="536eb-663">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="536eb-664">Jeśli `UseConnectionLogging` jest umieszczony po `UseHttps`, odszyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="536eb-664">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="536eb-665">Powiąż z gniazdem TCP</span><span class="sxs-lookup"><span data-stu-id="536eb-665">Bind to a TCP socket</span></span>

<span data-ttu-id="536eb-666">Metoda <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> wiąże się z gniazdem TCP, a opcja lambda zezwala na konfigurację certyfikatu X. 509:</span><span class="sxs-lookup"><span data-stu-id="536eb-666">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="536eb-667">Przykład konfiguruje HTTPS dla punktu końcowego z <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="536eb-667">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="536eb-668">Użyj tego samego interfejsu API, aby skonfigurować inne ustawienia Kestrel dla określonych punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="536eb-668">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="536eb-669">Powiąż z gniazdem systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="536eb-669">Bind to a Unix socket</span></span>

<span data-ttu-id="536eb-670">Nasłuchiwanie w gnieździe systemu UNIX za pomocą <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, aby zwiększyć wydajność za pomocą Nginx, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="536eb-670">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="536eb-671">Port 0</span><span class="sxs-lookup"><span data-stu-id="536eb-671">Port 0</span></span>

<span data-ttu-id="536eb-672">Gdy `0` jest określony numer portu, Kestrel dynamicznie wiąże się z dostępnym portem.</span><span class="sxs-lookup"><span data-stu-id="536eb-672">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="536eb-673">W poniższym przykładzie pokazano, jak określić, który port Kestrel faktycznie powiązany w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="536eb-673">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="536eb-674">Po uruchomieniu aplikacji dane wyjściowe okna konsoli wskazują port dynamiczny, w którym można uzyskać dostęp do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="536eb-674">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="536eb-675">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="536eb-675">Limitations</span></span>

<span data-ttu-id="536eb-676">Skonfiguruj punkty końcowe przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="536eb-676">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="536eb-677">`--urls` argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="536eb-677">`--urls` command-line argument</span></span>
* <span data-ttu-id="536eb-678">klucz konfiguracji hosta `urls`</span><span class="sxs-lookup"><span data-stu-id="536eb-678">`urls` host configuration key</span></span>
* <span data-ttu-id="536eb-679">Zmienna środowiskowa `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="536eb-679">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="536eb-680">Te metody są przydatne do tworzenia kodu w pracy z serwerami innymi niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-680">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="536eb-681">Należy jednak pamiętać o następujących ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="536eb-681">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="536eb-682">Nie można użyć protokołu HTTPS z tymi metodami, chyba że w konfiguracji punktu końcowego HTTPS jest podany certyfikat domyślny (na przykład za pomocą konfiguracji `KestrelServerOptions` lub pliku konfiguracji, jak pokazano wcześniej w tym temacie).</span><span class="sxs-lookup"><span data-stu-id="536eb-682">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="536eb-683">Gdy oba podejścia `Listen` i `UseUrls` są używane jednocześnie, `Listen` punkty końcowe przesłaniają `UseUrls` punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-683">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="536eb-684">Konfiguracja punktu końcowego usług IIS</span><span class="sxs-lookup"><span data-stu-id="536eb-684">IIS endpoint configuration</span></span>

<span data-ttu-id="536eb-685">W przypadku korzystania z usług IIS powiązania URL dla powiązań przesłonięć usług IIS są ustawiane przez `Listen` lub `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="536eb-685">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="536eb-686">Aby uzyskać więcej informacji, zobacz temat [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="536eb-686">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="536eb-687">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="536eb-687">ListenOptions.Protocols</span></span>

<span data-ttu-id="536eb-688">Właściwość `Protocols` ustanawia protokoły HTTP (`HttpProtocols`) włączone w punkcie końcowym połączenia lub na serwerze.</span><span class="sxs-lookup"><span data-stu-id="536eb-688">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="536eb-689">Przypisz wartość do właściwości `Protocols` z wyliczenia `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="536eb-689">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="536eb-690">`HttpProtocols` wartość wyliczenia</span><span class="sxs-lookup"><span data-stu-id="536eb-690">`HttpProtocols` enum value</span></span> | <span data-ttu-id="536eb-691">Dozwolony protokół połączenia</span><span class="sxs-lookup"><span data-stu-id="536eb-691">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="536eb-692">Tylko HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="536eb-692">HTTP/1.1 only.</span></span> <span data-ttu-id="536eb-693">Może być używany z protokołem TLS lub bez niego.</span><span class="sxs-lookup"><span data-stu-id="536eb-693">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="536eb-694">Tylko HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="536eb-694">HTTP/2 only.</span></span> <span data-ttu-id="536eb-695">Mogą być używane bez protokołu TLS tylko wtedy, gdy klient obsługuje [poprzedni tryb wiedzy](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="536eb-695">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="536eb-696">HTTP/1.1 i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="536eb-696">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="536eb-697">Protokół HTTP/2 wymaga połączenia protokołów TLS i [warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3) . w przeciwnym razie wartość domyślna połączenia to HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="536eb-697">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="536eb-698">Domyślnym protokołem jest HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="536eb-698">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="536eb-699">Ograniczenia protokołu TLS dla protokołu HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="536eb-699">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="536eb-700">TLS w wersji 1,2 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="536eb-700">TLS version 1.2 or later</span></span>
* <span data-ttu-id="536eb-701">Ponowne negocjowanie wyłączone</span><span class="sxs-lookup"><span data-stu-id="536eb-701">Renegotiation disabled</span></span>
* <span data-ttu-id="536eb-702">Kompresja wyłączona</span><span class="sxs-lookup"><span data-stu-id="536eb-702">Compression disabled</span></span>
* <span data-ttu-id="536eb-703">Minimalne rozmiary tymczasowych kluczy wymiany:</span><span class="sxs-lookup"><span data-stu-id="536eb-703">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="536eb-704">Krzywa eliptyczna Diffie-Hellmana (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 BITS</span><span class="sxs-lookup"><span data-stu-id="536eb-704">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="536eb-705">Ograniczone pole Diffie-Hellmana (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bitów</span><span class="sxs-lookup"><span data-stu-id="536eb-705">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="536eb-706">Mechanizm szyfrowania nie został zabroniony</span><span class="sxs-lookup"><span data-stu-id="536eb-706">Cipher suite not blacklisted</span></span>

<span data-ttu-id="536eb-707">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; z krzywą eliptyczna P-256 &lbrack;`FIPS186`&rbrack; jest domyślnie obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="536eb-707">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="536eb-708">Poniższy przykład umożliwia nawiązywanie połączeń HTTP/1.1 i HTTP/2 na porcie 8000.</span><span class="sxs-lookup"><span data-stu-id="536eb-708">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="536eb-709">Połączenia są zabezpieczone przez protokół TLS przy użyciu podanego certyfikatu:</span><span class="sxs-lookup"><span data-stu-id="536eb-709">Connections are secured by TLS with a supplied certificate:</span></span>

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

<span data-ttu-id="536eb-710">Opcjonalnie można utworzyć implementację `IConnectionAdapter` do filtrowania uzgadniania protokołu TLS dla poszczególnych połączeń dla określonych szyfrów:</span><span class="sxs-lookup"><span data-stu-id="536eb-710">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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
        // No encryption is used with a CipherAlgorithmType.Null cipher algorithm.
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

<span data-ttu-id="536eb-711">*Ustawianie protokołu z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="536eb-711">*Set the protocol from configuration*</span></span>

<span data-ttu-id="536eb-712"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-712"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="536eb-713">W poniższym przykładzie pliku *appSettings. JSON* jest ustanawiany domyślny protokół połączeń (http/1.1 i http/2) dla wszystkich punktów końcowych Kestrel:</span><span class="sxs-lookup"><span data-stu-id="536eb-713">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="536eb-714">Poniższy przykład pliku konfiguracji ustanawia protokół połączenia dla określonego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="536eb-714">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="536eb-715">Protokoły określone w wartościach zastąpienia kodu ustawione przez konfigurację.</span><span class="sxs-lookup"><span data-stu-id="536eb-715">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="536eb-716">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="536eb-716">Transport configuration</span></span>

<span data-ttu-id="536eb-717">W wersji platformy ASP.NET Core 2.1 transport domyślny serwera Kestrel nie jest już oparty na bibliotece libuv, ale na zarządzanych gniazdach.</span><span class="sxs-lookup"><span data-stu-id="536eb-717">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="536eb-718">Jest to istotna zmiana dla aplikacji ASP.NET Core 2,0 uaktualniana do 2,1, które wywołują <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> i zależą od jednego z następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="536eb-718">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="536eb-719">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (bezpośrednie odwołanie do pakietu)</span><span class="sxs-lookup"><span data-stu-id="536eb-719">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="536eb-720">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="536eb-720">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="536eb-721">Dla projektów, które wymagają użycia Libuv:</span><span class="sxs-lookup"><span data-stu-id="536eb-721">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="536eb-722">Dodaj zależność dla pakietu [Microsoft. AspNetCore. Server. Kestrel. transport. Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="536eb-722">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="536eb-723"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>wywołania:</span><span class="sxs-lookup"><span data-stu-id="536eb-723">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="536eb-724">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="536eb-724">URL prefixes</span></span>

<span data-ttu-id="536eb-725">W przypadku korzystania z `UseUrls`, `--urls` argumentu wiersza polecenia, klucza konfiguracji hosta `urls` lub zmiennej środowiskowej `ASPNETCORE_URLS`, prefiksy adresów URL mogą mieć jeden z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="536eb-725">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="536eb-726">Tylko prefiksy adresów URL HTTP są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-726">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="536eb-727">Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązań adresów URL przy użyciu `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="536eb-727">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="536eb-728">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="536eb-728">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="536eb-729">`0.0.0.0` jest szczególnym przypadkiem, który wiąże się ze wszystkimi adresami IPv4.</span><span class="sxs-lookup"><span data-stu-id="536eb-729">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="536eb-730">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="536eb-730">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="536eb-731">`[::]` to odpowiednik IPv6 `0.0.0.0`IPv4.</span><span class="sxs-lookup"><span data-stu-id="536eb-731">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="536eb-732">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="536eb-732">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="536eb-733">Nazwy hostów, `*`i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="536eb-733">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="536eb-734">Wszystkie nie są rozpoznawane jako prawidłowy adres IP lub `localhost` są powiązane z protokołem IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="536eb-734">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="536eb-735">Aby powiązać różne nazwy hostów z różnymi ASP.NET Core aplikacjami na tym samym porcie, użyj [protokołu HTTP. sys](xref:fundamentals/servers/httpsys) lub odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="536eb-735">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="536eb-736">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="536eb-736">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="536eb-737">Nazwa hosta `localhost` z numerem portu lub adresem IP sprzężenia zwrotnego z numerem portu</span><span class="sxs-lookup"><span data-stu-id="536eb-737">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="536eb-738">Gdy `localhost` jest określony, Kestrel próbuje powiązać z interfejsami sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="536eb-738">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="536eb-739">Jeśli żądany port jest używany przez inną usługę w dowolnym interfejsie sprzężenia zwrotnego, uruchomienie Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="536eb-739">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="536eb-740">Jeśli którykolwiek z innych przyczyn interfejsu sprzężenia zwrotnego jest niedostępny (najczęściej jest to spowodowane tym, że protokół IPv6 nie jest obsługiwany), Kestrel rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="536eb-740">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="536eb-741">Filtrowanie hostów</span><span class="sxs-lookup"><span data-stu-id="536eb-741">Host filtering</span></span>

<span data-ttu-id="536eb-742">Chociaż usługa Kestrel obsługuje konfigurację na podstawie prefiksów, takich jak `http://example.com:5000`, Kestrel w znacznym stopniu ignoruje nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="536eb-742">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="536eb-743">`localhost` hosta jest specjalnym przypadkiem używanym do tworzenia powiązań z adresami sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="536eb-743">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="536eb-744">Każdy host poza jawnym adresem IP tworzy powiązanie ze wszystkimi publicznymi adresami IP.</span><span class="sxs-lookup"><span data-stu-id="536eb-744">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="536eb-745">nagłówki `Host` nie są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="536eb-745">`Host` headers aren't validated.</span></span>

<span data-ttu-id="536eb-746">Aby obejść ten sposób, użyj oprogramowania pośredniczącego filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="536eb-746">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="536eb-747">Oprogramowanie pośredniczące do filtrowania hosta jest dostarczane przez pakiet [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , który jest zawarty w pakiecie [Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub 2,2).</span><span class="sxs-lookup"><span data-stu-id="536eb-747">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="536eb-748">Oprogramowanie pośredniczące jest dodawane przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="536eb-748">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="536eb-749">Programowe filtrowanie hosta jest domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="536eb-749">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="536eb-750">Aby włączyć oprogramowanie pośredniczące, zdefiniuj klucz `AllowedHosts` w pliku *appSettings. json*/*appSettings.\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="536eb-750">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="536eb-751">Wartość to rozdzielana średnikami lista nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="536eb-751">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="536eb-752">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="536eb-752">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="536eb-753">W przypadku [przekierowanych nagłówków oprogramowanie pośredniczące](xref:host-and-deploy/proxy-load-balancer) ma również opcję <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="536eb-753">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="536eb-754">Przekazane nagłówki — oprogramowanie pośredniczące i filtrowanie hostów oprogramowanie pośredniczące ma podobną funkcjonalność dla różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="536eb-754">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="536eb-755">Ustawienie `AllowedHosts` z przekierowanymi nagłówkami — oprogramowanie pośredniczące jest odpowiednie, gdy nagłówek `Host` nie jest zachowywany podczas przekazywania żądań z odwrotnym serwerem proxy lub modułem równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="536eb-755">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="536eb-756">Ustawienie `AllowedHosts` przy użyciu oprogramowania pośredniczącego filtrowania hosta jest odpowiednie, gdy Kestrel jest używany jako publiczny serwer graniczny lub gdy `Host` nagłówek jest bezpośrednio przekazywany.</span><span class="sxs-lookup"><span data-stu-id="536eb-756">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="536eb-757">Aby uzyskać więcej informacji na temat przekierowanych nagłówków, należy zapoznać się z tematem <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="536eb-757">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="536eb-758">Kestrel to Międzyplatformowy [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="536eb-758">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="536eb-759">Kestrel to serwer sieci Web, który jest domyślnie uwzględniony w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="536eb-759">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="536eb-760">Kestrel obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="536eb-760">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="536eb-761">HTTPS</span><span class="sxs-lookup"><span data-stu-id="536eb-761">HTTPS</span></span>
* <span data-ttu-id="536eb-762">Nieprzezroczyste uaktualnienie używane do włączania obsługi obiektów [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="536eb-762">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="536eb-763">Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx</span><span class="sxs-lookup"><span data-stu-id="536eb-763">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="536eb-764">Kestrel jest obsługiwana na wszystkich platformach i wersjach obsługiwanych przez platformę .NET Core.</span><span class="sxs-lookup"><span data-stu-id="536eb-764">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="536eb-765">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="536eb-765">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="536eb-766">Kiedy używać Kestrel z zwrotnym serwerem proxy</span><span class="sxs-lookup"><span data-stu-id="536eb-766">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="536eb-767">Kestrel może być używana przez siebie lub z *odwrotnym serwerem proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org)lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="536eb-767">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="536eb-768">Odwrotny serwer proxy odbiera żądania HTTP z sieci i przekazuje je do usługi Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-768">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="536eb-769">Kestrel używany jako serwer sieci Web z krawędzią (dostępną z Internetu):</span><span class="sxs-lookup"><span data-stu-id="536eb-769">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem bez serwera proxy zwrotnego](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="536eb-771">Kestrel używany w konfiguracji zwrotnego serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="536eb-771">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel komunikuje się pośrednio z Internetem za pomocą odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="536eb-773">Konfiguracja, z serwerem proxy odwrotnych lub bez niego, jest obsługiwaną konfiguracją hostingu.</span><span class="sxs-lookup"><span data-stu-id="536eb-773">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="536eb-774">Kestrel używany jako serwer graniczny bez serwera proxy odwrotnego nie obsługuje udostępniania tego samego adresu IP i portu między wieloma procesami.</span><span class="sxs-lookup"><span data-stu-id="536eb-774">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="536eb-775">Gdy Kestrel jest skonfigurowany do nasłuchiwania na porcie, Kestrel obsługuje cały ruch dla tego portu bez względu na żądania `Host` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="536eb-775">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="536eb-776">Zwrotny serwer proxy, który może udostępniać porty, ma możliwość przekazywania żądań do Kestrel na unikatowym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="536eb-776">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="536eb-777">Nawet jeśli odwrotny serwer proxy nie jest wymagany, może to być dobry wybór przy użyciu odwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="536eb-777">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="536eb-778">Zwrotny serwer proxy:</span><span class="sxs-lookup"><span data-stu-id="536eb-778">A reverse proxy:</span></span>

* <span data-ttu-id="536eb-779">Może ograniczać uwidoczniony obszar publicznej powierzchni aplikacji, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="536eb-779">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="536eb-780">Zapewnienie dodatkowej warstwy konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="536eb-780">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="536eb-781">Integracja z istniejącą infrastrukturą może być lepsza.</span><span class="sxs-lookup"><span data-stu-id="536eb-781">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="536eb-782">Uprość Równoważenie obciążenia i konfigurację komunikacji zabezpieczonej (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="536eb-782">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="536eb-783">Tylko serwer zwrotny proxy wymaga certyfikatu X. 509, a serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="536eb-783">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="536eb-784">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="536eb-784">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="536eb-785">Jak używać Kestrel w aplikacjach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="536eb-785">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="536eb-786">Pakiet [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="536eb-786">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="536eb-787">Szablony projektów ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-787">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="536eb-788">W *program.cs*kod szablonu wywołuje <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> w tle.</span><span class="sxs-lookup"><span data-stu-id="536eb-788">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="536eb-789">Aby zapewnić dodatkową konfigurację po wywołaniu `CreateDefaultBuilder`, wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span><span class="sxs-lookup"><span data-stu-id="536eb-789">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="536eb-790">Aby uzyskać więcej informacji na temat `CreateDefaultBuilder` i kompilowania hosta, zobacz sekcję *Konfigurowanie hosta* w programie <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="536eb-790">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="kestrel-options"></a><span data-ttu-id="536eb-791">Opcje Kestrel</span><span class="sxs-lookup"><span data-stu-id="536eb-791">Kestrel options</span></span>

<span data-ttu-id="536eb-792">Serwer sieci Web Kestrel ma opcje konfiguracji ograniczeń, które są szczególnie przydatne w przypadku wdrożeń mających dostęp do Internetu.</span><span class="sxs-lookup"><span data-stu-id="536eb-792">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="536eb-793">Ustaw ograniczenia we właściwości <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="536eb-793">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="536eb-794">Właściwość `Limits` przechowuje wystąpienie klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="536eb-794">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="536eb-795">W poniższych przykładach użyto przestrzeni nazw <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="536eb-795">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="536eb-796">Opcje Kestrel, które są konfigurowane w C# kodzie w poniższych przykładach, można również ustawić za pomocą [dostawcy konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="536eb-796">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="536eb-797">Na przykład dostawca konfiguracji plików może załadować konfigurację Kestrel z pliku *appSettings. JSON* lub *appSettings. { Environment} plik JSON* :</span><span class="sxs-lookup"><span data-stu-id="536eb-797">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

<span data-ttu-id="536eb-798">Skorzystaj z **jednej** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="536eb-798">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="536eb-799">Skonfiguruj Kestrel w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="536eb-799">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="536eb-800">Wstrzyknąć wystąpienie `IConfiguration` do klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="536eb-800">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="536eb-801">W poniższym przykładzie przyjęto założenie, że wprowadzona konfiguracja jest przypisana do właściwości `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="536eb-801">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="536eb-802">W `Startup.ConfigureServices`Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-802">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="536eb-803">Skonfiguruj Kestrel podczas kompilowania hosta:</span><span class="sxs-lookup"><span data-stu-id="536eb-803">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="536eb-804">W programie *program.cs*Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="536eb-804">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

<span data-ttu-id="536eb-805">Obie powyższe podejścia współpracują z dowolnym [dostawcą konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="536eb-805">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="536eb-806">Limit czasu utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="536eb-806">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="536eb-807">Pobiera lub ustawia [limit czasu utrzymywania aktywności](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="536eb-807">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="536eb-808">Wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="536eb-808">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="536eb-809">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="536eb-809">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="536eb-810">Maksymalna liczba współbieżnych otwartych połączeń TCP można ustawić dla całej aplikacji przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="536eb-810">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="536eb-811">Istnieje oddzielny limit połączeń, które zostały uaktualnione z protokołu HTTP lub HTTPS do innego protokołu (na przykład w żądaniu usługi WebSockets).</span><span class="sxs-lookup"><span data-stu-id="536eb-811">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="536eb-812">Po uaktualnieniu połączenia nie jest ono wliczane do limitu `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="536eb-812">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="536eb-813">Maksymalna liczba połączeń jest domyślnie nieograniczona (null).</span><span class="sxs-lookup"><span data-stu-id="536eb-813">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="536eb-814">Maksymalny rozmiar treści żądania</span><span class="sxs-lookup"><span data-stu-id="536eb-814">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="536eb-815">Domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="536eb-815">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="536eb-816">Zalecanym podejściem do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie atrybutu <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> dla metody akcji:</span><span class="sxs-lookup"><span data-stu-id="536eb-816">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="536eb-817">Oto przykład, który pokazuje, jak skonfigurować ograniczenie dla aplikacji w każdym żądaniu:</span><span class="sxs-lookup"><span data-stu-id="536eb-817">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="536eb-818">Zastąp ustawienie określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="536eb-818">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="536eb-819">Wyjątek jest generowany, jeśli aplikacja skonfiguruje limit żądania po rozpoczęciu uruchamiania aplikacji w celu odczytania żądania.</span><span class="sxs-lookup"><span data-stu-id="536eb-819">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="536eb-820">Istnieje właściwość `IsReadOnly`, która wskazuje, czy właściwość `MaxRequestBodySize` jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="536eb-820">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="536eb-821">Gdy aplikacja jest uruchamiana [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model) [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru treści żądania Kestrel jest wyłączony, ponieważ program IIS już ustawia limit.</span><span class="sxs-lookup"><span data-stu-id="536eb-821">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="536eb-822">Minimalna stawka danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="536eb-822">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="536eb-823">Kestrel sprawdza co sekundę, jeśli dane są odbierane z określoną szybkością w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="536eb-823">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="536eb-824">Jeśli szybkość spadnie poniżej wartości minimalnej, upłynął limit czasu połączenia. Okres prolongaty to czas, przez który Kestrel nadaje klientowi zwiększenie jego szybkości wysyłania do minimum; Ta częstotliwość nie jest sprawdzana w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="536eb-824">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="536eb-825">Okres prolongaty pozwala uniknąć porzucania połączeń, które początkowo wysyłają dane z niską szybkością.</span><span class="sxs-lookup"><span data-stu-id="536eb-825">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="536eb-826">Domyślna stawka minimalna to 240 bajtów na sekundę z 5-sekundowym okresem prolongaty.</span><span class="sxs-lookup"><span data-stu-id="536eb-826">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="536eb-827">Minimalna stawka dotyczy także odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="536eb-827">A minimum rate also applies to the response.</span></span> <span data-ttu-id="536eb-828">Kod określający limit żądań i limit odpowiedzi jest taki sam, chyba że ma `RequestBody` lub `Response` w nazwach właściwości i interfejsów.</span><span class="sxs-lookup"><span data-stu-id="536eb-828">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="536eb-829">Oto przykład pokazujący sposób konfigurowania minimalnych stawek danych w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="536eb-829">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

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

### <a name="request-headers-timeout"></a><span data-ttu-id="536eb-830">Limit czasu nagłówków żądań</span><span class="sxs-lookup"><span data-stu-id="536eb-830">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="536eb-831">Pobiera lub ustawia maksymalny czas, przez jaki serwer spędza nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="536eb-831">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="536eb-832">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="536eb-832">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="536eb-833">Synchroniczne we/wy</span><span class="sxs-lookup"><span data-stu-id="536eb-833">Synchronous IO</span></span>

<span data-ttu-id="536eb-834"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> kontroluje, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="536eb-834"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="536eb-835">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="536eb-835">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="536eb-836">Duża liczba blokowania synchronicznych operacji we/wy może prowadzić do zablokowania puli wątków, co sprawia, że aplikacja nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="536eb-836">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="536eb-837">Włącz `AllowSynchronousIO` tylko w przypadku korzystania z biblioteki, która nie obsługuje asynchronicznych operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="536eb-837">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="536eb-838">Poniższy przykład wyłącza synchroniczną operację we/wy:</span><span class="sxs-lookup"><span data-stu-id="536eb-838">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="536eb-839">Aby uzyskać informacje o innych opcjach i ograniczeniach Kestrel, zobacz:</span><span class="sxs-lookup"><span data-stu-id="536eb-839">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="536eb-840">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="536eb-840">Endpoint configuration</span></span>

<span data-ttu-id="536eb-841">Domyślnie ASP.NET Core wiąże się z:</span><span class="sxs-lookup"><span data-stu-id="536eb-841">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="536eb-842">`https://localhost:5001` (gdy obecny jest lokalny certyfikat programistyczny)</span><span class="sxs-lookup"><span data-stu-id="536eb-842">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="536eb-843">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="536eb-843">Specify URLs using the:</span></span>

* <span data-ttu-id="536eb-844">`ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="536eb-844">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="536eb-845">`--urls` argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="536eb-845">`--urls` command-line argument.</span></span>
* <span data-ttu-id="536eb-846">klucz konfiguracji hosta `urls`.</span><span class="sxs-lookup"><span data-stu-id="536eb-846">`urls` host configuration key.</span></span>
* <span data-ttu-id="536eb-847">Metoda rozszerzenia `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="536eb-847">`UseUrls` extension method.</span></span>

<span data-ttu-id="536eb-848">Wartość podana przy użyciu tych metod może być jednym lub większą liczbą punktów końcowych HTTP i HTTPS (HTTPS, jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="536eb-848">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="536eb-849">Skonfiguruj wartość jako listę rozdzieloną średnikami (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="536eb-849">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="536eb-850">Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwera](xref:fundamentals/host/web-host#server-urls) i [Zastąp konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="536eb-850">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="536eb-851">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="536eb-851">A development certificate is created:</span></span>

* <span data-ttu-id="536eb-852">Po zainstalowaniu [zestaw .NET Core SDK](/dotnet/core/sdk) .</span><span class="sxs-lookup"><span data-stu-id="536eb-852">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="536eb-853">Do utworzenia certyfikatu służy [Narzędzie dev-certs](xref:aspnetcore-2.1#https) .</span><span class="sxs-lookup"><span data-stu-id="536eb-853">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="536eb-854">Niektóre przeglądarki wymagają przyznania jawnego uprawnienia do zaufania do lokalnego certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="536eb-854">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="536eb-855">Szablony projektu konfigurują aplikacje do uruchamiania domyślnie przy użyciu protokołu HTTPS i obejmują [przekierowania https i obsługę HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="536eb-855">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="536eb-856">Wywołaj metody <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> w <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, aby skonfigurować prefiksy i porty adresów URL dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-856">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="536eb-857">`UseUrls`, `--urls` argument wiersza polecenia, klucz konfiguracji hosta `urls` oraz zmienna środowiskowa `ASPNETCORE_URLS` również działają, ale mają ograniczenia wymienione w dalszej części tej sekcji (certyfikat domyślny musi być dostępny do konfiguracji punktu końcowego HTTPS).</span><span class="sxs-lookup"><span data-stu-id="536eb-857">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="536eb-858">Konfiguracja `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="536eb-858">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="536eb-859">ConfigureEndpointDefaults (Akcja\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="536eb-859">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="536eb-860">Określa `Action` konfiguracji do uruchomienia dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="536eb-860">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="536eb-861">Wywoływanie `ConfigureEndpointDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="536eb-861">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="536eb-862">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="536eb-862">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="536eb-863">ConfigureHttpsDefaults (Akcja\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="536eb-863">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="536eb-864">Określa `Action` konfiguracji do uruchomienia dla każdego punktu końcowego HTTPS.</span><span class="sxs-lookup"><span data-stu-id="536eb-864">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="536eb-865">Wywoływanie `ConfigureHttpsDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="536eb-865">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="536eb-866">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="536eb-866">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="536eb-867">Konfiguruj (IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="536eb-867">Configure(IConfiguration)</span></span>

<span data-ttu-id="536eb-868">Tworzy moduł ładujący konfigurację służący do konfigurowania Kestrel, który pobiera <xref:Microsoft.Extensions.Configuration.IConfiguration> jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-868">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="536eb-869">Konfiguracja musi być objęta zakresem sekcji konfiguracji dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-869">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="536eb-870">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="536eb-870">ListenOptions.UseHttps</span></span>

<span data-ttu-id="536eb-871">Skonfiguruj Kestrel do korzystania z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="536eb-871">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="536eb-872">rozszerzenia `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="536eb-872">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="536eb-873">`UseHttps` &ndash; skonfigurować Kestrel do używania protokołu HTTPS z certyfikatem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="536eb-873">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="536eb-874">Zgłasza wyjątek, jeśli nie został skonfigurowany żaden certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="536eb-874">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="536eb-875">`ListenOptions.UseHttps` parametry:</span><span class="sxs-lookup"><span data-stu-id="536eb-875">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="536eb-876">`filename` to ścieżka i nazwa pliku certyfikatu odnosząca się do katalogu zawierającego pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="536eb-876">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="536eb-877">`password` to hasło wymagane do uzyskania dostępu do danych certyfikatu X. 509.</span><span class="sxs-lookup"><span data-stu-id="536eb-877">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="536eb-878">`configureOptions` jest `Action`, aby skonfigurować `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="536eb-878">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="536eb-879">Zwraca `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="536eb-879">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="536eb-880">`storeName` to magazyn certyfikatów, z którego ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="536eb-880">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="536eb-881">`subject` to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="536eb-881">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="536eb-882">`allowInvalid` wskazuje, czy należy wziąć pod uwagę nieprawidłowe certyfikaty, takie jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="536eb-882">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="536eb-883">`location` to lokalizacja magazynu, z której ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="536eb-883">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="536eb-884">`serverCertificate` to certyfikat X. 509.</span><span class="sxs-lookup"><span data-stu-id="536eb-884">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="536eb-885">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="536eb-885">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="536eb-886">Należy podać co najmniej certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="536eb-886">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="536eb-887">Obsługiwane konfiguracje opisane dalej:</span><span class="sxs-lookup"><span data-stu-id="536eb-887">Supported configurations described next:</span></span>

* <span data-ttu-id="536eb-888">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="536eb-888">No configuration</span></span>
* <span data-ttu-id="536eb-889">Zastąp domyślny certyfikat z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="536eb-889">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="536eb-890">Zmień wartości domyślne w kodzie</span><span class="sxs-lookup"><span data-stu-id="536eb-890">Change the defaults in code</span></span>

<span data-ttu-id="536eb-891">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="536eb-891">*No configuration*</span></span>

<span data-ttu-id="536eb-892">Kestrel nasłuchuje na `http://localhost:5000` i `https://localhost:5001` (jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="536eb-892">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="536eb-893">*Zastąp domyślny certyfikat z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="536eb-893">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="536eb-894">`CreateDefaultBuilder` wywołania `Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-894">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="536eb-895">Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępny dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-895">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="536eb-896">Konfigurowanie wielu punktów końcowych, w tym adresów URL i certyfikatów do użycia, z pliku znajdującego się na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="536eb-896">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="536eb-897">W poniższym przykładzie pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="536eb-897">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="536eb-898">Ustaw **AllowInvalid** na `true`, aby zezwolić na korzystanie z nieprawidłowych certyfikatów (na przykład certyfikatów z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="536eb-898">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="536eb-899">Wszystkie punkty końcowe HTTPS, które nie określają certyfikatu (**HttpsDefaultCert** w poniższym przykładzie) powracają do certyfikatu zdefiniowanego w obszarze **Certyfikaty** > **domyślnym** lub certyfikatem deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="536eb-899">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="536eb-900">Alternatywą dla korzystania z **ścieżki** i **hasła** dla dowolnego węzła certyfikatu jest określenie certyfikatu przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="536eb-900">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="536eb-901">Na przykład **certyfikaty** > **domyślnego** certyfikatu można określić jako:</span><span class="sxs-lookup"><span data-stu-id="536eb-901">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="536eb-902">Uwagi dotyczące schematu:</span><span class="sxs-lookup"><span data-stu-id="536eb-902">Schema notes:</span></span>

* <span data-ttu-id="536eb-903">W nazwach punktów końcowych nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="536eb-903">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="536eb-904">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-904">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="536eb-905">Dla każdego punktu końcowego jest wymagany parametr `Url`.</span><span class="sxs-lookup"><span data-stu-id="536eb-905">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="536eb-906">Format tego parametru jest taki sam jak parametr konfiguracji `Urls` najwyższego poziomu, z tą różnicą, że jest ograniczony do pojedynczej wartości.</span><span class="sxs-lookup"><span data-stu-id="536eb-906">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="536eb-907">Te punkty końcowe zastępują te zdefiniowane w konfiguracji `Urls` najwyższego poziomu zamiast dodawać do nich.</span><span class="sxs-lookup"><span data-stu-id="536eb-907">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="536eb-908">Punkty końcowe zdefiniowane w kodzie za pośrednictwem `Listen` kumulują się z punktami końcowymi zdefiniowanymi w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="536eb-908">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="536eb-909">Sekcja `Certificate` jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="536eb-909">The `Certificate` section is optional.</span></span> <span data-ttu-id="536eb-910">Jeśli sekcja `Certificate` nie zostanie określona, są używane wartości domyślne zdefiniowane we wcześniejszych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="536eb-910">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="536eb-911">Jeśli żadne wartości domyślne nie są dostępne, serwer zgłasza wyjątek i nie może się uruchomić.</span><span class="sxs-lookup"><span data-stu-id="536eb-911">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="536eb-912">Sekcja `Certificate` obsługuje zarówno **ścieżkę**&ndash;**hasło** , jak i **temat**&ndash;certyfikatów **magazynu** .</span><span class="sxs-lookup"><span data-stu-id="536eb-912">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="536eb-913">W ten sposób można zdefiniować dowolną liczbę punktów końcowych, dopóki nie spowoduje to konfliktów portów.</span><span class="sxs-lookup"><span data-stu-id="536eb-913">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="536eb-914">`options.Configure(context.Configuration.GetSection("{SECTION}"))` zwraca `KestrelConfigurationLoader` z metodą `.Endpoint(string name, listenOptions => { })`, której można użyć do uzupełnienia ustawień skonfigurowanego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="536eb-914">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="536eb-915">dostęp do `KestrelServerOptions.ConfigurationLoader` można uzyskać, aby kontynuować iterację istniejącego modułu ładującego, takiego jak dostarczony przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="536eb-915">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="536eb-916">Sekcja konfiguracji dla każdego punktu końcowego jest dostępna w opcjach w `Endpoint` metodzie, aby można było odczytać ustawienia niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-916">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="536eb-917">Można załadować wiele konfiguracji, wywołując `options.Configure(context.Configuration.GetSection("{SECTION}"))` ponownie z inną sekcją.</span><span class="sxs-lookup"><span data-stu-id="536eb-917">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="536eb-918">Używana jest tylko Ostatnia konfiguracja, chyba że `Load` jest jawnie wywoływana w poprzednich wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="536eb-918">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="536eb-919">Pakiet nie wywołuje `Load`, tak aby jego sekcja konfiguracji domyślnej mogła zostać zamieniona.</span><span class="sxs-lookup"><span data-stu-id="536eb-919">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="536eb-920">`KestrelConfigurationLoader` odzwierciedla `Listen`ą rodzinę interfejsów API z `KestrelServerOptions` jako przeciążania `Endpoint`, dlatego punkty końcowe kodu i konfiguracji można skonfigurować w tym samym miejscu.</span><span class="sxs-lookup"><span data-stu-id="536eb-920">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="536eb-921">Te przeciążenia nie używają nazw i używają tylko ustawień domyślnych z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="536eb-921">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="536eb-922">*Zmień wartości domyślne w kodzie*</span><span class="sxs-lookup"><span data-stu-id="536eb-922">*Change the defaults in code*</span></span>

<span data-ttu-id="536eb-923">`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` mogą służyć do zmiany domyślnych ustawień dla `ListenOptions` i `HttpsConnectionAdapterOptions`, w tym przesłanianie domyślnego certyfikatu określonego w poprzednim scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="536eb-923">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="536eb-924">przed skonfigurowaniem punktów końcowych należy wywołać `ConfigureEndpointDefaults` i `ConfigureHttpsDefaults`.</span><span class="sxs-lookup"><span data-stu-id="536eb-924">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="536eb-925">*Obsługa usługi Kestrel dla SNI*</span><span class="sxs-lookup"><span data-stu-id="536eb-925">*Kestrel support for SNI*</span></span>

<span data-ttu-id="536eb-926">[Oznaczanie nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do hostowania wielu domen na tym samym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="536eb-926">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="536eb-927">Aby SNI działał, klient wysyła nazwę hosta dla bezpiecznej sesji do serwera podczas uzgadniania TLS, aby serwer mógł zapewnić prawidłowy certyfikat.</span><span class="sxs-lookup"><span data-stu-id="536eb-927">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="536eb-928">Klient korzysta z dostarczonego certyfikatu w celu zaszyfrowania komunikacji z serwerem podczas bezpiecznej sesji, która następuje po uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="536eb-928">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="536eb-929">Kestrel obsługuje SNI za pośrednictwem wywołania zwrotnego `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="536eb-929">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="536eb-930">Wywołanie zwrotne jest wywoływane jednokrotnie dla każdego połączenia, aby umożliwić aplikacji inspekcję nazwy hosta i wybieranie odpowiedniego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="536eb-930">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="536eb-931">Obsługa SNI wymaga:</span><span class="sxs-lookup"><span data-stu-id="536eb-931">SNI support requires:</span></span>

* <span data-ttu-id="536eb-932">Uruchomione na platformie docelowej `netcoreapp2.1` lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="536eb-932">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="536eb-933">W `net461` lub później wywołanie zwrotne jest wywoływane, ale `name` jest zawsze `null`.</span><span class="sxs-lookup"><span data-stu-id="536eb-933">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="536eb-934">`name` jest również `null`, jeśli klient nie poda parametru nazwy hosta w uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="536eb-934">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="536eb-935">Wszystkie witryny sieci Web działają na tym samym wystąpieniu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-935">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="536eb-936">Kestrel nie obsługuje udostępniania adresu IP i portu w wielu wystąpieniach bez zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="536eb-936">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="536eb-937">Rejestrowanie połączeń</span><span class="sxs-lookup"><span data-stu-id="536eb-937">Connection logging</span></span>

<span data-ttu-id="536eb-938">Wywołaj <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>, aby emitować dzienniki poziomu debugowania dla komunikacji na poziomie bajtów w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="536eb-938">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="536eb-939">Rejestrowanie połączeń ułatwia rozwiązywanie problemów z komunikacją na niskim poziomie, na przykład podczas szyfrowania TLS i za serwerami proxy.</span><span class="sxs-lookup"><span data-stu-id="536eb-939">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="536eb-940">Jeśli `UseConnectionLogging` jest umieszczony przed `UseHttps`, szyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="536eb-940">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="536eb-941">Jeśli `UseConnectionLogging` jest umieszczony po `UseHttps`, odszyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="536eb-941">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="536eb-942">Powiąż z gniazdem TCP</span><span class="sxs-lookup"><span data-stu-id="536eb-942">Bind to a TCP socket</span></span>

<span data-ttu-id="536eb-943">Metoda <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> wiąże się z gniazdem TCP, a opcja lambda zezwala na konfigurację certyfikatu X. 509:</span><span class="sxs-lookup"><span data-stu-id="536eb-943">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

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

<span data-ttu-id="536eb-944">Przykład konfiguruje HTTPS dla punktu końcowego z <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="536eb-944">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="536eb-945">Użyj tego samego interfejsu API, aby skonfigurować inne ustawienia Kestrel dla określonych punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="536eb-945">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="536eb-946">Powiąż z gniazdem systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="536eb-946">Bind to a Unix socket</span></span>

<span data-ttu-id="536eb-947">Nasłuchiwanie w gnieździe systemu UNIX za pomocą <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, aby zwiększyć wydajność za pomocą Nginx, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="536eb-947">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

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

### <a name="port-0"></a><span data-ttu-id="536eb-948">Port 0</span><span class="sxs-lookup"><span data-stu-id="536eb-948">Port 0</span></span>

<span data-ttu-id="536eb-949">Gdy `0` jest określony numer portu, Kestrel dynamicznie wiąże się z dostępnym portem.</span><span class="sxs-lookup"><span data-stu-id="536eb-949">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="536eb-950">W poniższym przykładzie pokazano, jak określić, który port Kestrel faktycznie powiązany w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="536eb-950">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="536eb-951">Po uruchomieniu aplikacji dane wyjściowe okna konsoli wskazują port dynamiczny, w którym można uzyskać dostęp do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="536eb-951">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="536eb-952">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="536eb-952">Limitations</span></span>

<span data-ttu-id="536eb-953">Skonfiguruj punkty końcowe przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="536eb-953">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="536eb-954">`--urls` argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="536eb-954">`--urls` command-line argument</span></span>
* <span data-ttu-id="536eb-955">klucz konfiguracji hosta `urls`</span><span class="sxs-lookup"><span data-stu-id="536eb-955">`urls` host configuration key</span></span>
* <span data-ttu-id="536eb-956">Zmienna środowiskowa `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="536eb-956">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="536eb-957">Te metody są przydatne do tworzenia kodu w pracy z serwerami innymi niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="536eb-957">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="536eb-958">Należy jednak pamiętać o następujących ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="536eb-958">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="536eb-959">Nie można użyć protokołu HTTPS z tymi metodami, chyba że w konfiguracji punktu końcowego HTTPS jest podany certyfikat domyślny (na przykład za pomocą konfiguracji `KestrelServerOptions` lub pliku konfiguracji, jak pokazano wcześniej w tym temacie).</span><span class="sxs-lookup"><span data-stu-id="536eb-959">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="536eb-960">Gdy oba podejścia `Listen` i `UseUrls` są używane jednocześnie, `Listen` punkty końcowe przesłaniają `UseUrls` punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-960">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="536eb-961">Konfiguracja punktu końcowego usług IIS</span><span class="sxs-lookup"><span data-stu-id="536eb-961">IIS endpoint configuration</span></span>

<span data-ttu-id="536eb-962">W przypadku korzystania z usług IIS powiązania URL dla powiązań przesłonięć usług IIS są ustawiane przez `Listen` lub `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="536eb-962">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="536eb-963">Aby uzyskać więcej informacji, zobacz temat [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="536eb-963">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="536eb-964">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="536eb-964">Transport configuration</span></span>

<span data-ttu-id="536eb-965">W wersji platformy ASP.NET Core 2.1 transport domyślny serwera Kestrel nie jest już oparty na bibliotece libuv, ale na zarządzanych gniazdach.</span><span class="sxs-lookup"><span data-stu-id="536eb-965">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="536eb-966">Jest to istotna zmiana dla aplikacji ASP.NET Core 2,0 uaktualniana do 2,1, które wywołują <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> i zależą od jednego z następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="536eb-966">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="536eb-967">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (bezpośrednie odwołanie do pakietu)</span><span class="sxs-lookup"><span data-stu-id="536eb-967">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="536eb-968">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="536eb-968">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="536eb-969">Dla projektów, które wymagają użycia Libuv:</span><span class="sxs-lookup"><span data-stu-id="536eb-969">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="536eb-970">Dodaj zależność dla pakietu [Microsoft. AspNetCore. Server. Kestrel. transport. Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="536eb-970">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="536eb-971"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>wywołania:</span><span class="sxs-lookup"><span data-stu-id="536eb-971">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="536eb-972">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="536eb-972">URL prefixes</span></span>

<span data-ttu-id="536eb-973">W przypadku korzystania z `UseUrls`, `--urls` argumentu wiersza polecenia, klucza konfiguracji hosta `urls` lub zmiennej środowiskowej `ASPNETCORE_URLS`, prefiksy adresów URL mogą mieć jeden z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="536eb-973">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="536eb-974">Tylko prefiksy adresów URL HTTP są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="536eb-974">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="536eb-975">Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązań adresów URL przy użyciu `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="536eb-975">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="536eb-976">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="536eb-976">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="536eb-977">`0.0.0.0` jest szczególnym przypadkiem, który wiąże się ze wszystkimi adresami IPv4.</span><span class="sxs-lookup"><span data-stu-id="536eb-977">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="536eb-978">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="536eb-978">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="536eb-979">`[::]` to odpowiednik IPv6 `0.0.0.0`IPv4.</span><span class="sxs-lookup"><span data-stu-id="536eb-979">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="536eb-980">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="536eb-980">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="536eb-981">Nazwy hostów, `*`i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="536eb-981">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="536eb-982">Wszystkie nie są rozpoznawane jako prawidłowy adres IP lub `localhost` są powiązane z protokołem IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="536eb-982">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="536eb-983">Aby powiązać różne nazwy hostów z różnymi ASP.NET Core aplikacjami na tym samym porcie, użyj [protokołu HTTP. sys](xref:fundamentals/servers/httpsys) lub odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="536eb-983">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="536eb-984">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="536eb-984">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="536eb-985">Nazwa hosta `localhost` z numerem portu lub adresem IP sprzężenia zwrotnego z numerem portu</span><span class="sxs-lookup"><span data-stu-id="536eb-985">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="536eb-986">Gdy `localhost` jest określony, Kestrel próbuje powiązać z interfejsami sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="536eb-986">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="536eb-987">Jeśli żądany port jest używany przez inną usługę w dowolnym interfejsie sprzężenia zwrotnego, uruchomienie Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="536eb-987">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="536eb-988">Jeśli którykolwiek z innych przyczyn interfejsu sprzężenia zwrotnego jest niedostępny (najczęściej jest to spowodowane tym, że protokół IPv6 nie jest obsługiwany), Kestrel rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="536eb-988">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="536eb-989">Filtrowanie hostów</span><span class="sxs-lookup"><span data-stu-id="536eb-989">Host filtering</span></span>

<span data-ttu-id="536eb-990">Chociaż usługa Kestrel obsługuje konfigurację na podstawie prefiksów, takich jak `http://example.com:5000`, Kestrel w znacznym stopniu ignoruje nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="536eb-990">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="536eb-991">`localhost` hosta jest specjalnym przypadkiem używanym do tworzenia powiązań z adresami sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="536eb-991">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="536eb-992">Każdy host poza jawnym adresem IP tworzy powiązanie ze wszystkimi publicznymi adresami IP.</span><span class="sxs-lookup"><span data-stu-id="536eb-992">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="536eb-993">nagłówki `Host` nie są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="536eb-993">`Host` headers aren't validated.</span></span>

<span data-ttu-id="536eb-994">Aby obejść ten sposób, użyj oprogramowania pośredniczącego filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="536eb-994">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="536eb-995">Oprogramowanie pośredniczące do filtrowania hosta jest dostarczane przez pakiet [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , który jest zawarty w pakiecie [Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub 2,2).</span><span class="sxs-lookup"><span data-stu-id="536eb-995">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="536eb-996">Oprogramowanie pośredniczące jest dodawane przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="536eb-996">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="536eb-997">Programowe filtrowanie hosta jest domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="536eb-997">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="536eb-998">Aby włączyć oprogramowanie pośredniczące, zdefiniuj klucz `AllowedHosts` w pliku *appSettings. json*/*appSettings.\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="536eb-998">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="536eb-999">Wartość to rozdzielana średnikami lista nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="536eb-999">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="536eb-1000">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="536eb-1000">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="536eb-1001">W przypadku [przekierowanych nagłówków oprogramowanie pośredniczące](xref:host-and-deploy/proxy-load-balancer) ma również opcję <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="536eb-1001">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="536eb-1002">Przekazane nagłówki — oprogramowanie pośredniczące i filtrowanie hostów oprogramowanie pośredniczące ma podobną funkcjonalność dla różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="536eb-1002">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="536eb-1003">Ustawienie `AllowedHosts` z przekierowanymi nagłówkami — oprogramowanie pośredniczące jest odpowiednie, gdy nagłówek `Host` nie jest zachowywany podczas przekazywania żądań z odwrotnym serwerem proxy lub modułem równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="536eb-1003">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="536eb-1004">Ustawienie `AllowedHosts` przy użyciu oprogramowania pośredniczącego filtrowania hosta jest odpowiednie, gdy Kestrel jest używany jako publiczny serwer graniczny lub gdy `Host` nagłówek jest bezpośrednio przekazywany.</span><span class="sxs-lookup"><span data-stu-id="536eb-1004">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="536eb-1005">Aby uzyskać więcej informacji na temat przekierowanych nagłówków, należy zapoznać się z tematem <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="536eb-1005">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="536eb-1006">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="536eb-1006">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="536eb-1007">RFC 7230: Składnia i Routing komunikatu (sekcja 5,4: Host)</span><span class="sxs-lookup"><span data-stu-id="536eb-1007">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
