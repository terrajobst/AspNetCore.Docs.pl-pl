---
title: Implementacja serwera sieci Web Kestrel w ASP.NET Core
author: guardrex
description: Dowiedz się więcej na temat Kestrel, międzyplatformowego serwera sieci Web dla ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2020
uid: fundamentals/servers/kestrel
ms.openlocfilehash: d026e1b6fc1a9ecc66014eacb8eb0b46dd9353ec
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2020
ms.locfileid: "77171733"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="6596d-103">Implementacja serwera sieci Web Kestrel w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6596d-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="6596d-104">Autorzy [Dykstra](https://github.com/tdykstra), [Krzysztof Ross](https://github.com/Tratcher), [Stephen](https://twitter.com/halter73), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6596d-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6596d-105">Kestrel to Międzyplatformowy [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="6596d-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="6596d-106">Kestrel to serwer sieci Web, który jest domyślnie uwzględniony w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6596d-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="6596d-107">Kestrel obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="6596d-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="6596d-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6596d-108">HTTPS</span></span>
* <span data-ttu-id="6596d-109">Nieprzezroczyste uaktualnienie używane do włączania obsługi obiektów [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="6596d-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="6596d-110">Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx</span><span class="sxs-lookup"><span data-stu-id="6596d-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="6596d-111">HTTP/2 (z wyjątkiem macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="6596d-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="6596d-112">&dagger;HTTP/2 będą obsługiwane w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="6596d-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="6596d-113">Kestrel jest obsługiwana na wszystkich platformach i wersjach obsługiwanych przez platformę .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6596d-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="6596d-114">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6596d-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="6596d-115">Obsługa protokołu HTTP/2</span><span class="sxs-lookup"><span data-stu-id="6596d-115">HTTP/2 support</span></span>

<span data-ttu-id="6596d-116">[Protokół HTTP/2](https://httpwg.org/specs/rfc7540.html) jest dostępny dla aplikacji ASP.NET Core, jeśli spełnione są następujące wymagania podstawowe:</span><span class="sxs-lookup"><span data-stu-id="6596d-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="6596d-117">&dagger; systemu operacyjnego</span><span class="sxs-lookup"><span data-stu-id="6596d-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="6596d-118">Windows Server 2016/Windows 10 lub nowszy&Dagger;</span><span class="sxs-lookup"><span data-stu-id="6596d-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="6596d-119">Linux z OpenSSL 1.0.2 lub nowszym (na przykład Ubuntu 16,04 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="6596d-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="6596d-120">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="6596d-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="6596d-121">Połączenie [negocjowania protokołu warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="6596d-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="6596d-122">Protokół TLS 1.2 lub nowszej połączenia</span><span class="sxs-lookup"><span data-stu-id="6596d-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="6596d-123">&dagger;HTTP/2 będą obsługiwane w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="6596d-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="6596d-124">&Dagger;Kestrel ma ograniczoną obsługę protokołu HTTP/2 w systemie Windows Server 2012 R2 i Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="6596d-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="6596d-125">Obsługa jest ograniczona, ponieważ lista obsługiwanych mechanizmów szyfrowania TLS dostępnych w tych systemach operacyjnych jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="6596d-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="6596d-126">Do zabezpieczenia połączeń TLS może być wymagany certyfikat wygenerowany przy użyciu algorytmu podpisu cyfrowego (ECDSA) krzywej eliptycznej.</span><span class="sxs-lookup"><span data-stu-id="6596d-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="6596d-127">W przypadku ustanowienia połączenia HTTP/2 raporty [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="6596d-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="6596d-128">Protokół HTTP/2 jest domyślnie wyłączony.</span><span class="sxs-lookup"><span data-stu-id="6596d-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="6596d-129">Więcej informacji na temat konfiguracji można znaleźć w sekcjach [Kestrel Options](#kestrel-options) and [ListenOptions. Protocols](#listenoptionsprotocols) .</span><span class="sxs-lookup"><span data-stu-id="6596d-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="6596d-130">Kiedy używać Kestrel z zwrotnym serwerem proxy</span><span class="sxs-lookup"><span data-stu-id="6596d-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="6596d-131">Kestrel może być używana przez siebie lub z *odwrotnym serwerem proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org)lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="6596d-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="6596d-132">Odwrotny serwer proxy odbiera żądania HTTP z sieci i przekazuje je do usługi Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="6596d-133">Kestrel używany jako serwer sieci Web z krawędzią (dostępną z Internetu):</span><span class="sxs-lookup"><span data-stu-id="6596d-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem bez serwera proxy zwrotnego](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="6596d-135">Kestrel używany w konfiguracji zwrotnego serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="6596d-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel komunikuje się pośrednio z Internetem za pomocą odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="6596d-137">Konfiguracja, z serwerem proxy odwrotnych lub bez niego, jest obsługiwaną konfiguracją hostingu.</span><span class="sxs-lookup"><span data-stu-id="6596d-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="6596d-138">Kestrel używany jako serwer graniczny bez serwera proxy odwrotnego nie obsługuje udostępniania tego samego adresu IP i portu między wieloma procesami.</span><span class="sxs-lookup"><span data-stu-id="6596d-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="6596d-139">Gdy Kestrel jest skonfigurowany do nasłuchiwania na porcie, Kestrel obsługuje cały ruch dla tego portu bez względu na żądania `Host` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="6596d-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="6596d-140">Zwrotny serwer proxy, który może udostępniać porty, ma możliwość przekazywania żądań do Kestrel na unikatowym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="6596d-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="6596d-141">Nawet jeśli odwrotny serwer proxy nie jest wymagany, może to być dobry wybór przy użyciu odwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="6596d-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="6596d-142">Zwrotny serwer proxy:</span><span class="sxs-lookup"><span data-stu-id="6596d-142">A reverse proxy:</span></span>

* <span data-ttu-id="6596d-143">Może ograniczać uwidoczniony obszar publicznej powierzchni aplikacji, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="6596d-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="6596d-144">Zapewnienie dodatkowej warstwy konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="6596d-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="6596d-145">Integracja z istniejącą infrastrukturą może być lepsza.</span><span class="sxs-lookup"><span data-stu-id="6596d-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="6596d-146">Uprość Równoważenie obciążenia i konfigurację komunikacji zabezpieczonej (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6596d-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="6596d-147">Tylko serwer zwrotny proxy wymaga certyfikatu X. 509, a serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6596d-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="6596d-148">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="6596d-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="kestrel-in-aspnet-core-apps"></a><span data-ttu-id="6596d-149">Kestrel w aplikacjach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6596d-149">Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="6596d-150">Szablony projektów ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="6596d-151">W *program.cs*Metoda <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> wywołuje <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span><span class="sxs-lookup"><span data-stu-id="6596d-151">In *Program.cs*, the <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> method calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=8)]

<span data-ttu-id="6596d-152">Aby uzyskać więcej informacji na temat tworzenia hosta, zobacz sekcję *Konfigurowanie ustawień hosta* i *domyślnego konstruktora* dla <xref:fundamentals/host/generic-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="6596d-152">For more information on building the host, see the *Set up a host* and *Default builder settings* sections of <xref:fundamentals/host/generic-host#set-up-a-host>.</span></span>

<span data-ttu-id="6596d-153">Aby zapewnić dodatkową konfigurację po wywołaniu `ConfigureWebHostDefaults`, użyj `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="6596d-153">To provide additional configuration after calling `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="6596d-154">Opcje Kestrel</span><span class="sxs-lookup"><span data-stu-id="6596d-154">Kestrel options</span></span>

<span data-ttu-id="6596d-155">Serwer sieci Web Kestrel ma opcje konfiguracji ograniczeń, które są szczególnie przydatne w przypadku wdrożeń mających dostęp do Internetu.</span><span class="sxs-lookup"><span data-stu-id="6596d-155">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="6596d-156">Ustaw ograniczenia we właściwości <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="6596d-156">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="6596d-157">Właściwość `Limits` przechowuje wystąpienie klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="6596d-157">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="6596d-158">W poniższych przykładach użyto przestrzeni nazw <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="6596d-158">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="6596d-159">W przykładach przedstawionych w dalszej części tego artykułu opcje Kestrel C# są konfigurowane w kodzie.</span><span class="sxs-lookup"><span data-stu-id="6596d-159">In examples shown later in this article, Kestrel options are configured in C# code.</span></span> <span data-ttu-id="6596d-160">Opcje Kestrel można również ustawić za pomocą [dostawcy konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6596d-160">Kestrel options can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="6596d-161">Na przykład [dostawca konfiguracji plików](xref:fundamentals/configuration/index#file-configuration-provider) może załadować konfigurację Kestrel z pliku *appSettings. JSON* lub *appSettings. { Environment} plik JSON* :</span><span class="sxs-lookup"><span data-stu-id="6596d-161">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider) can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

> [!NOTE]
> <span data-ttu-id="6596d-162">Konfiguracja <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> i [punktu końcowego](#endpoint-configuration) można konfigurować z poziomu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="6596d-162"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> and [endpoint configuration](#endpoint-configuration) are configurable from configuration providers.</span></span> <span data-ttu-id="6596d-163">Pozostała konfiguracja Kestrel musi być C# skonfigurowana w kodzie.</span><span class="sxs-lookup"><span data-stu-id="6596d-163">Remaining Kestrel configuration must be configured in C# code.</span></span>

<span data-ttu-id="6596d-164">Skorzystaj z **jednej** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="6596d-164">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="6596d-165">Skonfiguruj Kestrel w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6596d-165">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="6596d-166">Wstrzyknąć wystąpienie `IConfiguration` do klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="6596d-166">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="6596d-167">W poniższym przykładzie przyjęto założenie, że wprowadzona konfiguracja jest przypisana do właściwości `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="6596d-167">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="6596d-168">W `Startup.ConfigureServices`Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="6596d-168">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

     ```csharp
     using Microsoft.Extensions.Configuration
     
     public class Startup
     {
         public Startup(IConfiguration configuration)
         {
             Configuration = configuration;
         }

         public IConfiguration Configuration { get; }

         public void ConfigureServices(IServiceCollection services)
         {
             services.Configure<KestrelServerOptions>(
                 Configuration.GetSection("Kestrel"));
         }

         public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
         {
             ...
         }
     }
     ```

* <span data-ttu-id="6596d-169">Skonfiguruj Kestrel podczas kompilowania hosta:</span><span class="sxs-lookup"><span data-stu-id="6596d-169">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="6596d-170">W programie *program.cs*Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="6596d-170">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="6596d-171">Obie powyższe podejścia współpracują z dowolnym [dostawcą konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6596d-171">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="6596d-172">Limit czasu utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="6596d-172">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="6596d-173">Pobiera lub ustawia [limit czasu utrzymywania aktywności](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="6596d-173">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="6596d-174">Wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="6596d-174">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="6596d-175">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="6596d-175">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="6596d-176">Maksymalna liczba współbieżnych otwartych połączeń TCP można ustawić dla całej aplikacji przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="6596d-176">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="6596d-177">Istnieje oddzielny limit połączeń, które zostały uaktualnione z protokołu HTTP lub HTTPS do innego protokołu (na przykład w żądaniu usługi WebSockets).</span><span class="sxs-lookup"><span data-stu-id="6596d-177">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="6596d-178">Po uaktualnieniu połączenia nie jest ono wliczane do limitu `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="6596d-178">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="6596d-179">Maksymalna liczba połączeń jest domyślnie nieograniczona (null).</span><span class="sxs-lookup"><span data-stu-id="6596d-179">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="6596d-180">Maksymalny rozmiar treści żądania</span><span class="sxs-lookup"><span data-stu-id="6596d-180">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="6596d-181">Domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="6596d-181">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="6596d-182">Zalecanym podejściem do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie atrybutu <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> dla metody akcji:</span><span class="sxs-lookup"><span data-stu-id="6596d-182">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="6596d-183">Oto przykład, który pokazuje, jak skonfigurować ograniczenie dla aplikacji w każdym żądaniu:</span><span class="sxs-lookup"><span data-stu-id="6596d-183">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="6596d-184">Zastąp ustawienie określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="6596d-184">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="6596d-185">Wyjątek jest generowany, jeśli aplikacja skonfiguruje limit żądania po rozpoczęciu uruchamiania aplikacji w celu odczytania żądania.</span><span class="sxs-lookup"><span data-stu-id="6596d-185">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="6596d-186">Istnieje właściwość `IsReadOnly`, która wskazuje, czy właściwość `MaxRequestBodySize` jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="6596d-186">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="6596d-187">Gdy aplikacja jest uruchamiana [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model) [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru treści żądania Kestrel jest wyłączony, ponieważ program IIS już ustawia limit.</span><span class="sxs-lookup"><span data-stu-id="6596d-187">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="6596d-188">Minimalna stawka danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="6596d-188">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="6596d-189">Kestrel sprawdza co sekundę, jeśli dane są odbierane z określoną szybkością w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="6596d-189">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="6596d-190">Jeśli szybkość spadnie poniżej wartości minimalnej, upłynął limit czasu połączenia. Okres prolongaty to czas, przez który Kestrel nadaje klientowi zwiększenie jego szybkości wysyłania do minimum; Ta częstotliwość nie jest sprawdzana w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="6596d-190">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="6596d-191">Okres prolongaty pozwala uniknąć porzucania połączeń, które początkowo wysyłają dane z niską szybkością.</span><span class="sxs-lookup"><span data-stu-id="6596d-191">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="6596d-192">Domyślna stawka minimalna to 240 bajtów na sekundę z 5-sekundowym okresem prolongaty.</span><span class="sxs-lookup"><span data-stu-id="6596d-192">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="6596d-193">Minimalna stawka dotyczy także odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6596d-193">A minimum rate also applies to the response.</span></span> <span data-ttu-id="6596d-194">Kod określający limit żądań i limit odpowiedzi jest taki sam, chyba że ma `RequestBody` lub `Response` w nazwach właściwości i interfejsów.</span><span class="sxs-lookup"><span data-stu-id="6596d-194">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="6596d-195">Oto przykład pokazujący sposób konfigurowania minimalnych stawek danych w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="6596d-195">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="6596d-196">Przesłoń minimalne limity szybkości dla żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="6596d-196">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="6596d-197"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature>, do których odwołuje się Poprzednia próbka nie występuje w `HttpContext.Features` dla żądań HTTP/2, ponieważ Modyfikowanie limitów szybkości dla poszczególnych żądań jest ogólnie nieobsługiwane w przypadku protokołu HTTP/2 z powodu obsługi żądania multipleksowania żądań.</span><span class="sxs-lookup"><span data-stu-id="6596d-197">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="6596d-198">Jednak <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> nadal obecne `HttpContext.Features` dla żądań HTTP/2, ponieważ limit liczby odczytów nadal można *wyłączyć całkowicie* dla poszczególnych żądań, ustawiając `IHttpMinRequestBodyDataRateFeature.MinDataRate` `null` nawet dla żądania HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6596d-198">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="6596d-199">Próba odczytania `IHttpMinRequestBodyDataRateFeature.MinDataRate` lub próby ustawienia jej na wartość inną niż `null` spowoduje, że `NotSupportedException` zostanie zgłoszone żądanie HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6596d-199">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="6596d-200">Limity szybkości dla całego serwera skonfigurowane za pomocą `KestrelServerOptions.Limits` nadal mają zastosowanie do połączeń HTTP/1. x i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6596d-200">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="6596d-201">Limit czasu nagłówków żądań</span><span class="sxs-lookup"><span data-stu-id="6596d-201">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="6596d-202">Pobiera lub ustawia maksymalny czas, przez jaki serwer spędza nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="6596d-202">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="6596d-203">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="6596d-203">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="6596d-204">Maksymalna liczba strumieni na połączenie</span><span class="sxs-lookup"><span data-stu-id="6596d-204">Maximum streams per connection</span></span>

<span data-ttu-id="6596d-205">`Http2.MaxStreamsPerConnection` ogranicza liczbę współbieżnych strumieni żądań dla połączenia HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6596d-205">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="6596d-206">Odrzucone strumienie są nadmiarowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-206">Excess streams are refused.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="6596d-207">Wartość domyślna to 100.</span><span class="sxs-lookup"><span data-stu-id="6596d-207">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="6596d-208">Rozmiar tabeli nagłówka</span><span class="sxs-lookup"><span data-stu-id="6596d-208">Header table size</span></span>

<span data-ttu-id="6596d-209">Dekoder HPACK dekompresuje nagłówki HTTP dla połączeń HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6596d-209">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="6596d-210">`Http2.HeaderTableSize` ogranicza rozmiar tabeli kompresji nagłówka używanej przez dekoder HPACK.</span><span class="sxs-lookup"><span data-stu-id="6596d-210">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="6596d-211">Wartość jest podawana w oktetach i musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="6596d-211">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="6596d-212">Wartość domyślna to 4096.</span><span class="sxs-lookup"><span data-stu-id="6596d-212">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="6596d-213">Maksymalny rozmiar ramki</span><span class="sxs-lookup"><span data-stu-id="6596d-213">Maximum frame size</span></span>

<span data-ttu-id="6596d-214">`Http2.MaxFrameSize` wskazuje maksymalny dozwolony rozmiar ładunku ramki połączenia HTTP/2 otrzymanego lub wysyłanego przez serwer.</span><span class="sxs-lookup"><span data-stu-id="6596d-214">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="6596d-215">Wartość jest podawana w oktetach i musi należeć do przedziału od 2 ^ 14 (16 384) do 2 ^ 24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="6596d-215">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="6596d-216">Wartość domyślna to 2 ^ 14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="6596d-216">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="6596d-217">Maksymalny rozmiar nagłówka żądania</span><span class="sxs-lookup"><span data-stu-id="6596d-217">Maximum request header size</span></span>

<span data-ttu-id="6596d-218">`Http2.MaxRequestHeaderFieldSize` wskazuje maksymalny dozwolony rozmiar w oktetach wartości nagłówka żądania.</span><span class="sxs-lookup"><span data-stu-id="6596d-218">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="6596d-219">Ten limit dotyczy zarówno nazwy, jak i wartości w skompresowanych i nieskompresowanych reprezentacji.</span><span class="sxs-lookup"><span data-stu-id="6596d-219">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="6596d-220">Wartość musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="6596d-220">The value must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="6596d-221">Wartość domyślna to 8 192.</span><span class="sxs-lookup"><span data-stu-id="6596d-221">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="6596d-222">Początkowy rozmiar okna połączenia</span><span class="sxs-lookup"><span data-stu-id="6596d-222">Initial connection window size</span></span>

<span data-ttu-id="6596d-223">`Http2.InitialConnectionWindowSize` wskazuje maksymalne dane dotyczące treści żądania w bajtach, które bufory serwera w tym samym czasie są agregowane dla wszystkich żądań (strumieni) na połączenie.</span><span class="sxs-lookup"><span data-stu-id="6596d-223">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="6596d-224">Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="6596d-224">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="6596d-225">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="6596d-225">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="6596d-226">Wartość domyślna to 128 KB (131 072).</span><span class="sxs-lookup"><span data-stu-id="6596d-226">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="6596d-227">Rozmiar początkowego okna strumienia</span><span class="sxs-lookup"><span data-stu-id="6596d-227">Initial stream window size</span></span>

<span data-ttu-id="6596d-228">`Http2.InitialStreamWindowSize` wskazuje maksymalne dane dotyczące treści żądania w bajtach bufory serwerów w jednym momencie na żądanie (Stream).</span><span class="sxs-lookup"><span data-stu-id="6596d-228">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="6596d-229">Żądania są również ograniczone przez `Http2.InitialConnectionWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="6596d-229">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="6596d-230">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="6596d-230">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="6596d-231">Wartość domyślna to 96 KB (98 304).</span><span class="sxs-lookup"><span data-stu-id="6596d-231">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="6596d-232">Synchroniczne we/wy</span><span class="sxs-lookup"><span data-stu-id="6596d-232">Synchronous IO</span></span>

<span data-ttu-id="6596d-233"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> kontroluje, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6596d-233"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="6596d-234">Wartością domyślną jest `false`.</span><span class="sxs-lookup"><span data-stu-id="6596d-234">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="6596d-235">Duża liczba blokowania synchronicznych operacji we/wy może prowadzić do zablokowania puli wątków, co sprawia, że aplikacja nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="6596d-235">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="6596d-236">Włącz `AllowSynchronousIO` tylko w przypadku korzystania z biblioteki, która nie obsługuje asynchronicznych operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="6596d-236">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="6596d-237">Poniższy przykład włącza synchroniczną operację we/wy:</span><span class="sxs-lookup"><span data-stu-id="6596d-237">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="6596d-238">Aby uzyskać informacje o innych opcjach i ograniczeniach Kestrel, zobacz:</span><span class="sxs-lookup"><span data-stu-id="6596d-238">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="6596d-239">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="6596d-239">Endpoint configuration</span></span>

<span data-ttu-id="6596d-240">Domyślnie ASP.NET Core wiąże się z:</span><span class="sxs-lookup"><span data-stu-id="6596d-240">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="6596d-241">`https://localhost:5001` (gdy obecny jest lokalny certyfikat programistyczny)</span><span class="sxs-lookup"><span data-stu-id="6596d-241">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="6596d-242">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="6596d-242">Specify URLs using the:</span></span>

* <span data-ttu-id="6596d-243">Zmienna środowiskowa `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="6596d-243">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="6596d-244">`--urls` argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="6596d-244">`--urls` command-line argument.</span></span>
* <span data-ttu-id="6596d-245">klucz konfiguracji hosta `urls`.</span><span class="sxs-lookup"><span data-stu-id="6596d-245">`urls` host configuration key.</span></span>
* <span data-ttu-id="6596d-246">Metoda rozszerzenia `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6596d-246">`UseUrls` extension method.</span></span>

<span data-ttu-id="6596d-247">Wartość podana przy użyciu tych metod może być jednym lub większą liczbą punktów końcowych HTTP i HTTPS (HTTPS, jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="6596d-247">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="6596d-248">Skonfiguruj wartość jako listę rozdzieloną średnikami (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="6596d-248">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="6596d-249">Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwera](xref:fundamentals/host/web-host#server-urls) i [Zastąp konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="6596d-249">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="6596d-250">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="6596d-250">A development certificate is created:</span></span>

* <span data-ttu-id="6596d-251">Po zainstalowaniu [zestaw .NET Core SDK](/dotnet/core/sdk) .</span><span class="sxs-lookup"><span data-stu-id="6596d-251">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="6596d-252">Do utworzenia certyfikatu służy [Narzędzie dev-certs](xref:aspnetcore-2.1#https) .</span><span class="sxs-lookup"><span data-stu-id="6596d-252">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="6596d-253">Niektóre przeglądarki wymagają przyznania jawnego uprawnienia do zaufania do lokalnego certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="6596d-253">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="6596d-254">Szablony projektu konfigurują aplikacje do uruchamiania domyślnie przy użyciu protokołu HTTPS i obejmują [przekierowania https i obsługę HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="6596d-254">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="6596d-255">Wywołaj metody <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> w <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, aby skonfigurować prefiksy i porty adresów URL dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-255">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="6596d-256">`UseUrls`, `--urls` argument wiersza polecenia, klucz konfiguracji hosta `urls` oraz zmienna środowiskowa `ASPNETCORE_URLS` również działają, ale mają ograniczenia wymienione w dalszej części tej sekcji (certyfikat domyślny musi być dostępny do konfiguracji punktu końcowego HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6596d-256">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="6596d-257">Konfiguracja `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="6596d-257">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="6596d-258">ConfigureEndpointDefaults (Akcja\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="6596d-258">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="6596d-259">Określa `Action` konfiguracji do uruchomienia dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="6596d-259">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="6596d-260">Wywoływanie `ConfigureEndpointDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="6596d-260">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="6596d-261">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="6596d-261">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="6596d-262">ConfigureHttpsDefaults (Akcja\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="6596d-262">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="6596d-263">Określa `Action` konfiguracji do uruchomienia dla każdego punktu końcowego HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6596d-263">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="6596d-264">Wywoływanie `ConfigureHttpsDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="6596d-264">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="6596d-265">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="6596d-265">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="6596d-266">Konfiguruj (IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="6596d-266">Configure(IConfiguration)</span></span>

<span data-ttu-id="6596d-267">Tworzy moduł ładujący konfigurację służący do konfigurowania Kestrel, który pobiera <xref:Microsoft.Extensions.Configuration.IConfiguration> jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-267">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="6596d-268">Konfiguracja musi być objęta zakresem sekcji konfiguracji dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-268">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="6596d-269">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="6596d-269">ListenOptions.UseHttps</span></span>

<span data-ttu-id="6596d-270">Skonfiguruj Kestrel do korzystania z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6596d-270">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="6596d-271">rozszerzenia `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="6596d-271">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="6596d-272">`UseHttps` &ndash; skonfigurować Kestrel do używania protokołu HTTPS z certyfikatem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="6596d-272">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="6596d-273">Zgłasza wyjątek, jeśli nie został skonfigurowany żaden certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="6596d-273">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="6596d-274">`ListenOptions.UseHttps` parametry:</span><span class="sxs-lookup"><span data-stu-id="6596d-274">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="6596d-275">`filename` to ścieżka i nazwa pliku certyfikatu odnosząca się do katalogu zawierającego pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6596d-275">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="6596d-276">`password` to hasło wymagane do uzyskania dostępu do danych certyfikatu X. 509.</span><span class="sxs-lookup"><span data-stu-id="6596d-276">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="6596d-277">`configureOptions` jest `Action`, aby skonfigurować `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="6596d-277">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="6596d-278">Zwraca `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="6596d-278">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="6596d-279">`storeName` to magazyn certyfikatów, z którego ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="6596d-279">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="6596d-280">`subject` to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="6596d-280">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="6596d-281">`allowInvalid` wskazuje, czy należy wziąć pod uwagę nieprawidłowe certyfikaty, takie jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="6596d-281">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="6596d-282">`location` to lokalizacja magazynu, z której ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="6596d-282">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="6596d-283">`serverCertificate` to certyfikat X. 509.</span><span class="sxs-lookup"><span data-stu-id="6596d-283">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="6596d-284">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6596d-284">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="6596d-285">Należy podać co najmniej certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="6596d-285">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="6596d-286">Obsługiwane konfiguracje opisane dalej:</span><span class="sxs-lookup"><span data-stu-id="6596d-286">Supported configurations described next:</span></span>

* <span data-ttu-id="6596d-287">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="6596d-287">No configuration</span></span>
* <span data-ttu-id="6596d-288">Zastąp domyślny certyfikat z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="6596d-288">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="6596d-289">Zmień wartości domyślne w kodzie</span><span class="sxs-lookup"><span data-stu-id="6596d-289">Change the defaults in code</span></span>

<span data-ttu-id="6596d-290">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="6596d-290">*No configuration*</span></span>

<span data-ttu-id="6596d-291">Kestrel nasłuchuje na `http://localhost:5000` i `https://localhost:5001` (jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="6596d-291">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="6596d-292">*Zastąp domyślny certyfikat z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="6596d-292">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="6596d-293">`CreateDefaultBuilder` wywołania `Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-293">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="6596d-294">Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępny dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-294">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="6596d-295">Konfigurowanie wielu punktów końcowych, w tym adresów URL i certyfikatów do użycia, z pliku znajdującego się na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="6596d-295">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="6596d-296">W poniższym przykładzie pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="6596d-296">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="6596d-297">Ustaw **AllowInvalid** na `true`, aby zezwolić na korzystanie z nieprawidłowych certyfikatów (na przykład certyfikatów z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="6596d-297">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="6596d-298">Wszystkie punkty końcowe HTTPS, które nie określają certyfikatu (**HttpsDefaultCert** w poniższym przykładzie) powracają do certyfikatu zdefiniowanego w obszarze **Certyfikaty** > **domyślnym** lub certyfikatem deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="6596d-298">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="6596d-299">Alternatywą dla korzystania z **ścieżki** i **hasła** dla dowolnego węzła certyfikatu jest określenie certyfikatu przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="6596d-299">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="6596d-300">Na przykład **certyfikaty** > **domyślnego** certyfikatu można określić jako:</span><span class="sxs-lookup"><span data-stu-id="6596d-300">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="6596d-301">Uwagi dotyczące schematu:</span><span class="sxs-lookup"><span data-stu-id="6596d-301">Schema notes:</span></span>

* <span data-ttu-id="6596d-302">W nazwach punktów końcowych nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="6596d-302">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="6596d-303">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-303">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="6596d-304">Dla każdego punktu końcowego jest wymagany parametr `Url`.</span><span class="sxs-lookup"><span data-stu-id="6596d-304">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="6596d-305">Format tego parametru jest taki sam jak parametr konfiguracji `Urls` najwyższego poziomu, z tą różnicą, że jest ograniczony do pojedynczej wartości.</span><span class="sxs-lookup"><span data-stu-id="6596d-305">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="6596d-306">Te punkty końcowe zastępują te zdefiniowane w konfiguracji `Urls` najwyższego poziomu zamiast dodawać do nich.</span><span class="sxs-lookup"><span data-stu-id="6596d-306">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="6596d-307">Punkty końcowe zdefiniowane w kodzie za pośrednictwem `Listen` kumulują się z punktami końcowymi zdefiniowanymi w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="6596d-307">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="6596d-308">Sekcja `Certificate` jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="6596d-308">The `Certificate` section is optional.</span></span> <span data-ttu-id="6596d-309">Jeśli sekcja `Certificate` nie zostanie określona, są używane wartości domyślne zdefiniowane we wcześniejszych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="6596d-309">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="6596d-310">Jeśli żadne wartości domyślne nie są dostępne, serwer zgłasza wyjątek i nie może się uruchomić.</span><span class="sxs-lookup"><span data-stu-id="6596d-310">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="6596d-311">Sekcja `Certificate` obsługuje zarówno **ścieżkę**&ndash;**hasło** , jak i **temat**&ndash;certyfikatów **magazynu** .</span><span class="sxs-lookup"><span data-stu-id="6596d-311">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="6596d-312">W ten sposób można zdefiniować dowolną liczbę punktów końcowych, dopóki nie spowoduje to konfliktów portów.</span><span class="sxs-lookup"><span data-stu-id="6596d-312">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="6596d-313">`options.Configure(context.Configuration.GetSection("{SECTION}"))` zwraca `KestrelConfigurationLoader` z metodą `.Endpoint(string name, listenOptions => { })`, której można użyć do uzupełnienia ustawień skonfigurowanego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="6596d-313">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="6596d-314">dostęp do `KestrelServerOptions.ConfigurationLoader` można uzyskać, aby kontynuować iterację istniejącego modułu ładującego, takiego jak dostarczony przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6596d-314">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="6596d-315">Sekcja konfiguracji dla każdego punktu końcowego jest dostępna w opcjach w `Endpoint` metodzie, aby można było odczytać ustawienia niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-315">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="6596d-316">Można załadować wiele konfiguracji, wywołując `options.Configure(context.Configuration.GetSection("{SECTION}"))` ponownie z inną sekcją.</span><span class="sxs-lookup"><span data-stu-id="6596d-316">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="6596d-317">Używana jest tylko Ostatnia konfiguracja, chyba że `Load` jest jawnie wywoływana w poprzednich wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="6596d-317">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="6596d-318">Pakiet nie wywołuje `Load`, tak aby jego sekcja konfiguracji domyślnej mogła zostać zamieniona.</span><span class="sxs-lookup"><span data-stu-id="6596d-318">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="6596d-319">`KestrelConfigurationLoader` odzwierciedla `Listen`ą rodzinę interfejsów API z `KestrelServerOptions` jako przeciążania `Endpoint`, dlatego punkty końcowe kodu i konfiguracji można skonfigurować w tym samym miejscu.</span><span class="sxs-lookup"><span data-stu-id="6596d-319">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="6596d-320">Te przeciążenia nie używają nazw i używają tylko ustawień domyślnych z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="6596d-320">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="6596d-321">*Zmień wartości domyślne w kodzie*</span><span class="sxs-lookup"><span data-stu-id="6596d-321">*Change the defaults in code*</span></span>

<span data-ttu-id="6596d-322">`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` mogą służyć do zmiany domyślnych ustawień dla `ListenOptions` i `HttpsConnectionAdapterOptions`, w tym przesłanianie domyślnego certyfikatu określonego w poprzednim scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="6596d-322">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="6596d-323">przed skonfigurowaniem punktów końcowych należy wywołać `ConfigureEndpointDefaults` i `ConfigureHttpsDefaults`.</span><span class="sxs-lookup"><span data-stu-id="6596d-323">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="6596d-324">*Obsługa usługi Kestrel dla SNI*</span><span class="sxs-lookup"><span data-stu-id="6596d-324">*Kestrel support for SNI*</span></span>

<span data-ttu-id="6596d-325">[Oznaczanie nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do hostowania wielu domen na tym samym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="6596d-325">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="6596d-326">Aby SNI działał, klient wysyła nazwę hosta dla bezpiecznej sesji do serwera podczas uzgadniania TLS, aby serwer mógł zapewnić prawidłowy certyfikat.</span><span class="sxs-lookup"><span data-stu-id="6596d-326">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="6596d-327">Klient korzysta z dostarczonego certyfikatu w celu zaszyfrowania komunikacji z serwerem podczas bezpiecznej sesji, która następuje po uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="6596d-327">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="6596d-328">Kestrel obsługuje SNI za pośrednictwem wywołania zwrotnego `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="6596d-328">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="6596d-329">Wywołanie zwrotne jest wywoływane jednokrotnie dla każdego połączenia, aby umożliwić aplikacji inspekcję nazwy hosta i wybieranie odpowiedniego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="6596d-329">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="6596d-330">Obsługa SNI wymaga:</span><span class="sxs-lookup"><span data-stu-id="6596d-330">SNI support requires:</span></span>

* <span data-ttu-id="6596d-331">Uruchomione na platformie docelowej `netcoreapp2.1` lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="6596d-331">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="6596d-332">W `net461` lub później wywołanie zwrotne jest wywoływane, ale `name` jest zawsze `null`.</span><span class="sxs-lookup"><span data-stu-id="6596d-332">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="6596d-333">`name` jest również `null`, jeśli klient nie poda parametru nazwy hosta w uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="6596d-333">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="6596d-334">Wszystkie witryny sieci Web działają na tym samym wystąpieniu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-334">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="6596d-335">Kestrel nie obsługuje udostępniania adresu IP i portu w wielu wystąpieniach bez zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="6596d-335">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="6596d-336">Rejestrowanie połączeń</span><span class="sxs-lookup"><span data-stu-id="6596d-336">Connection logging</span></span>

<span data-ttu-id="6596d-337">Wywołaj <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>, aby emitować dzienniki poziomu debugowania dla komunikacji na poziomie bajtów w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="6596d-337">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="6596d-338">Rejestrowanie połączeń ułatwia rozwiązywanie problemów z komunikacją na niskim poziomie, na przykład podczas szyfrowania TLS i za serwerami proxy.</span><span class="sxs-lookup"><span data-stu-id="6596d-338">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="6596d-339">Jeśli `UseConnectionLogging` jest umieszczony przed `UseHttps`, szyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="6596d-339">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="6596d-340">Jeśli `UseConnectionLogging` jest umieszczony po `UseHttps`, odszyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="6596d-340">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="6596d-341">Powiąż z gniazdem TCP</span><span class="sxs-lookup"><span data-stu-id="6596d-341">Bind to a TCP socket</span></span>

<span data-ttu-id="6596d-342">Metoda <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> wiąże się z gniazdem TCP, a opcja lambda zezwala na konfigurację certyfikatu X. 509:</span><span class="sxs-lookup"><span data-stu-id="6596d-342">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=12-18)]

<span data-ttu-id="6596d-343">Przykład konfiguruje HTTPS dla punktu końcowego z <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="6596d-343">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="6596d-344">Użyj tego samego interfejsu API, aby skonfigurować inne ustawienia Kestrel dla określonych punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="6596d-344">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="6596d-345">Powiąż z gniazdem systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="6596d-345">Bind to a Unix socket</span></span>

<span data-ttu-id="6596d-346">Nasłuchiwanie w gnieździe systemu UNIX za pomocą <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, aby zwiększyć wydajność za pomocą Nginx, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="6596d-346">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

* <span data-ttu-id="6596d-347">W pliku Nginx confiuguration Ustaw `proxy_pass` `server` > `location` > `http://unix:/tmp/{KESTREL SOCKET}:/;`.</span><span class="sxs-lookup"><span data-stu-id="6596d-347">In the Nginx confiuguration file, set the `server` > `location` > `proxy_pass` entry to `http://unix:/tmp/{KESTREL SOCKET}:/;`.</span></span> <span data-ttu-id="6596d-348">`{KESTREL SOCKET}` to nazwa gniazda dostarczanego do <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> (na przykład `kestrel-test.sock` w powyższym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="6596d-348">`{KESTREL SOCKET}` is the name of the socket provided to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> (for example, `kestrel-test.sock` in the preceding example).</span></span>
* <span data-ttu-id="6596d-349">Upewnij się, że gniazdo jest zapisywalne przez Nginx (na przykład `chmod go+w /tmp/kestrel-test.sock`).</span><span class="sxs-lookup"><span data-stu-id="6596d-349">Ensure that the socket is writeable by Nginx (for example, `chmod go+w /tmp/kestrel-test.sock`).</span></span>

### <a name="port-0"></a><span data-ttu-id="6596d-350">Port 0</span><span class="sxs-lookup"><span data-stu-id="6596d-350">Port 0</span></span>

<span data-ttu-id="6596d-351">Gdy `0` jest określony numer portu, Kestrel dynamicznie wiąże się z dostępnym portem.</span><span class="sxs-lookup"><span data-stu-id="6596d-351">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="6596d-352">W poniższym przykładzie pokazano, jak określić, który port Kestrel faktycznie powiązany w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="6596d-352">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="6596d-353">Po uruchomieniu aplikacji dane wyjściowe okna konsoli wskazują port dynamiczny, w którym można uzyskać dostęp do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="6596d-353">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="6596d-354">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="6596d-354">Limitations</span></span>

<span data-ttu-id="6596d-355">Skonfiguruj punkty końcowe przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="6596d-355">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="6596d-356">`--urls` argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="6596d-356">`--urls` command-line argument</span></span>
* <span data-ttu-id="6596d-357">klucz konfiguracji hosta `urls`</span><span class="sxs-lookup"><span data-stu-id="6596d-357">`urls` host configuration key</span></span>
* <span data-ttu-id="6596d-358">Zmienna środowiskowa `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="6596d-358">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="6596d-359">Te metody są przydatne do tworzenia kodu w pracy z serwerami innymi niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-359">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="6596d-360">Należy jednak pamiętać o następujących ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="6596d-360">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="6596d-361">Nie można użyć protokołu HTTPS z tymi metodami, chyba że w konfiguracji punktu końcowego HTTPS jest podany certyfikat domyślny (na przykład za pomocą konfiguracji `KestrelServerOptions` lub pliku konfiguracji, jak pokazano wcześniej w tym temacie).</span><span class="sxs-lookup"><span data-stu-id="6596d-361">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="6596d-362">Gdy oba podejścia `Listen` i `UseUrls` są używane jednocześnie, `Listen` punkty końcowe przesłaniają `UseUrls` punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-362">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="6596d-363">Konfiguracja punktu końcowego usług IIS</span><span class="sxs-lookup"><span data-stu-id="6596d-363">IIS endpoint configuration</span></span>

<span data-ttu-id="6596d-364">W przypadku korzystania z usług IIS powiązania URL dla powiązań przesłonięć usług IIS są ustawiane przez `Listen` lub `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6596d-364">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="6596d-365">Aby uzyskać więcej informacji, zobacz temat [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="6596d-365">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="6596d-366">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="6596d-366">ListenOptions.Protocols</span></span>

<span data-ttu-id="6596d-367">Właściwość `Protocols` ustanawia protokoły HTTP (`HttpProtocols`) włączone w punkcie końcowym połączenia lub na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6596d-367">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="6596d-368">Przypisz wartość do właściwości `Protocols` z wyliczenia `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="6596d-368">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="6596d-369">`HttpProtocols` wartość wyliczenia</span><span class="sxs-lookup"><span data-stu-id="6596d-369">`HttpProtocols` enum value</span></span> | <span data-ttu-id="6596d-370">Dozwolony protokół połączenia</span><span class="sxs-lookup"><span data-stu-id="6596d-370">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="6596d-371">Tylko HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="6596d-371">HTTP/1.1 only.</span></span> <span data-ttu-id="6596d-372">Może być używany z protokołem TLS lub bez niego.</span><span class="sxs-lookup"><span data-stu-id="6596d-372">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="6596d-373">Tylko HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6596d-373">HTTP/2 only.</span></span> <span data-ttu-id="6596d-374">Mogą być używane bez protokołu TLS tylko wtedy, gdy klient obsługuje [poprzedni tryb wiedzy](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="6596d-374">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="6596d-375">HTTP/1.1 i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6596d-375">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="6596d-376">Protokół HTTP/2 wymaga od klienta wyboru protokołu HTTP/2 w uzgadnianiu [negocjowania protokołu TLS aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3) . w przeciwnym razie wartość domyślna połączenia to HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="6596d-376">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="6596d-377">Domyślna wartość `ListenOptions.Protocols` dla dowolnego punktu końcowego jest `HttpProtocols.Http1AndHttp2`.</span><span class="sxs-lookup"><span data-stu-id="6596d-377">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="6596d-378">Ograniczenia protokołu TLS dla protokołu HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="6596d-378">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="6596d-379">TLS w wersji 1,2 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="6596d-379">TLS version 1.2 or later</span></span>
* <span data-ttu-id="6596d-380">Ponowne negocjowanie wyłączone</span><span class="sxs-lookup"><span data-stu-id="6596d-380">Renegotiation disabled</span></span>
* <span data-ttu-id="6596d-381">Kompresja wyłączona</span><span class="sxs-lookup"><span data-stu-id="6596d-381">Compression disabled</span></span>
* <span data-ttu-id="6596d-382">Minimalne rozmiary tymczasowych kluczy wymiany:</span><span class="sxs-lookup"><span data-stu-id="6596d-382">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="6596d-383">Krzywa eliptyczna Diffie-Hellmana (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 BITS</span><span class="sxs-lookup"><span data-stu-id="6596d-383">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="6596d-384">Ograniczone pole Diffie-Hellmana (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bitów</span><span class="sxs-lookup"><span data-stu-id="6596d-384">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="6596d-385">Mechanizm szyfrowania nie został zabroniony</span><span class="sxs-lookup"><span data-stu-id="6596d-385">Cipher suite not blacklisted</span></span>

<span data-ttu-id="6596d-386">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; z krzywą eliptyczna P-256 &lbrack;`FIPS186`&rbrack; jest domyślnie obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="6596d-386">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="6596d-387">Poniższy przykład umożliwia nawiązywanie połączeń HTTP/1.1 i HTTP/2 na porcie 8000.</span><span class="sxs-lookup"><span data-stu-id="6596d-387">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="6596d-388">Połączenia są zabezpieczone przez protokół TLS przy użyciu podanego certyfikatu:</span><span class="sxs-lookup"><span data-stu-id="6596d-388">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="6596d-389">Używaj oprogramowania pośredniczącego do filtrowania uzgadniania protokołu TLS dla poszczególnych połączeń dla określonych szyfrów, jeśli jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="6596d-389">Use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers if required.</span></span>

<span data-ttu-id="6596d-390">Poniższy przykład zgłasza <xref:System.NotSupportedException> dla dowolnego algorytmu szyfrowania, który nie jest obsługiwany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="6596d-390">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="6596d-391">Alternatywnie można zdefiniować i porównać [ITlsHandshakeFeature. CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) z listą akceptowalnych mechanizmów szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="6596d-391">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="6596d-392">Nie jest używane szyfrowanie z algorytmem szyfrowania [CipherAlgorithmType. null](xref:System.Security.Authentication.CipherAlgorithmType) .</span><span class="sxs-lookup"><span data-stu-id="6596d-392">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

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

<span data-ttu-id="6596d-393">Filtrowanie połączeń można również skonfigurować za pomocą <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span><span class="sxs-lookup"><span data-stu-id="6596d-393">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

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

<span data-ttu-id="6596d-394">W systemie Linux <xref:System.Net.Security.CipherSuitesPolicy> może służyć do filtrowania uzgadniania protokołu TLS dla poszczególnych połączeń:</span><span class="sxs-lookup"><span data-stu-id="6596d-394">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

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

<span data-ttu-id="6596d-395">*Ustawianie protokołu z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="6596d-395">*Set the protocol from configuration*</span></span>

<span data-ttu-id="6596d-396">`CreateDefaultBuilder` wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-396">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="6596d-397">W poniższym przykładzie pliku *appSettings. JSON* jako domyślny protokół połączenia dla wszystkich punktów końcowych:</span><span class="sxs-lookup"><span data-stu-id="6596d-397">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="6596d-398">Poniższy przykład *appSettings. JSON* ustanawia protokół połączeń HTTP/1.1 dla określonego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="6596d-398">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="6596d-399">Protokoły określone w wartościach zastąpienia kodu ustawione przez konfigurację.</span><span class="sxs-lookup"><span data-stu-id="6596d-399">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="6596d-400">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="6596d-400">Transport configuration</span></span>

<span data-ttu-id="6596d-401">Dla projektów, które wymagają użycia Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span><span class="sxs-lookup"><span data-stu-id="6596d-401">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="6596d-402">Dodaj zależność dla pakietu [Microsoft. AspNetCore. Server. Kestrel. transport. Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="6596d-402">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="6596d-403">Wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> na `IWebHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="6596d-403">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="6596d-404">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="6596d-404">URL prefixes</span></span>

<span data-ttu-id="6596d-405">W przypadku korzystania z `UseUrls`, `--urls` argumentu wiersza polecenia, klucza konfiguracji hosta `urls` lub zmiennej środowiskowej `ASPNETCORE_URLS`, prefiksy adresów URL mogą mieć jeden z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="6596d-405">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="6596d-406">Tylko prefiksy adresów URL HTTP są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-406">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="6596d-407">Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązań adresów URL przy użyciu `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6596d-407">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="6596d-408">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="6596d-408">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="6596d-409">`0.0.0.0` jest szczególnym przypadkiem, który wiąże się ze wszystkimi adresami IPv4.</span><span class="sxs-lookup"><span data-stu-id="6596d-409">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="6596d-410">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="6596d-410">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="6596d-411">`[::]` to odpowiednik IPv6 `0.0.0.0`IPv4.</span><span class="sxs-lookup"><span data-stu-id="6596d-411">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="6596d-412">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="6596d-412">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="6596d-413">Nazwy hostów, `*`i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="6596d-413">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="6596d-414">Wszystkie nie są rozpoznawane jako prawidłowy adres IP lub `localhost` są powiązane z protokołem IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="6596d-414">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="6596d-415">Aby powiązać różne nazwy hostów z różnymi ASP.NET Core aplikacjami na tym samym porcie, użyj [protokołu HTTP. sys](xref:fundamentals/servers/httpsys) lub odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="6596d-415">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="6596d-416">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="6596d-416">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="6596d-417">Nazwa hosta `localhost` z numerem portu lub adresem IP sprzężenia zwrotnego z numerem portu</span><span class="sxs-lookup"><span data-stu-id="6596d-417">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="6596d-418">Gdy `localhost` jest określony, Kestrel próbuje powiązać z interfejsami sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="6596d-418">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="6596d-419">Jeśli żądany port jest używany przez inną usługę w dowolnym interfejsie sprzężenia zwrotnego, uruchomienie Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="6596d-419">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="6596d-420">Jeśli którykolwiek z innych przyczyn interfejsu sprzężenia zwrotnego jest niedostępny (najczęściej jest to spowodowane tym, że protokół IPv6 nie jest obsługiwany), Kestrel rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="6596d-420">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="6596d-421">Filtrowanie hostów</span><span class="sxs-lookup"><span data-stu-id="6596d-421">Host filtering</span></span>

<span data-ttu-id="6596d-422">Chociaż usługa Kestrel obsługuje konfigurację na podstawie prefiksów, takich jak `http://example.com:5000`, Kestrel w znacznym stopniu ignoruje nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="6596d-422">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="6596d-423">`localhost` hosta jest specjalnym przypadkiem używanym do tworzenia powiązań z adresami sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="6596d-423">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="6596d-424">Każdy host poza jawnym adresem IP tworzy powiązanie ze wszystkimi publicznymi adresami IP.</span><span class="sxs-lookup"><span data-stu-id="6596d-424">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="6596d-425">nagłówki `Host` nie są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="6596d-425">`Host` headers aren't validated.</span></span>

<span data-ttu-id="6596d-426">Aby obejść ten sposób, użyj oprogramowania pośredniczącego filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="6596d-426">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="6596d-427">Oprogramowanie pośredniczące do filtrowania hosta jest dostarczane przez pakiet [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , który jest niejawnie dostarczany dla ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6596d-427">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="6596d-428">Oprogramowanie pośredniczące jest dodawane przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="6596d-428">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="6596d-429">Programowe filtrowanie hosta jest domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="6596d-429">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="6596d-430">Aby włączyć oprogramowanie pośredniczące, zdefiniuj klucz `AllowedHosts` w pliku *appSettings. json*/*appSettings.\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="6596d-430">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="6596d-431">Wartość to rozdzielana średnikami lista nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="6596d-431">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="6596d-432">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="6596d-432">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="6596d-433">W przypadku [przekierowanych nagłówków oprogramowanie pośredniczące](xref:host-and-deploy/proxy-load-balancer) ma również opcję <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="6596d-433">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="6596d-434">Przekazane nagłówki — oprogramowanie pośredniczące i filtrowanie hostów oprogramowanie pośredniczące ma podobną funkcjonalność dla różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="6596d-434">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="6596d-435">Ustawienie `AllowedHosts` z przekierowanymi nagłówkami — oprogramowanie pośredniczące jest odpowiednie, gdy nagłówek `Host` nie jest zachowywany podczas przekazywania żądań z odwrotnym serwerem proxy lub modułem równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="6596d-435">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="6596d-436">Ustawienie `AllowedHosts` przy użyciu oprogramowania pośredniczącego filtrowania hosta jest odpowiednie, gdy Kestrel jest używany jako publiczny serwer graniczny lub gdy `Host` nagłówek jest bezpośrednio przekazywany.</span><span class="sxs-lookup"><span data-stu-id="6596d-436">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="6596d-437">Aby uzyskać więcej informacji na temat przekierowanych nagłówków, należy zapoznać się z tematem <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="6596d-437">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="6596d-438">Kestrel to Międzyplatformowy [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="6596d-438">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="6596d-439">Kestrel to serwer sieci Web, który jest domyślnie uwzględniony w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6596d-439">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="6596d-440">Kestrel obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="6596d-440">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="6596d-441">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6596d-441">HTTPS</span></span>
* <span data-ttu-id="6596d-442">Nieprzezroczyste uaktualnienie używane do włączania obsługi obiektów [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="6596d-442">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="6596d-443">Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx</span><span class="sxs-lookup"><span data-stu-id="6596d-443">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="6596d-444">HTTP/2 (z wyjątkiem macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="6596d-444">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="6596d-445">&dagger;HTTP/2 będą obsługiwane w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="6596d-445">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="6596d-446">Kestrel jest obsługiwana na wszystkich platformach i wersjach obsługiwanych przez platformę .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6596d-446">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="6596d-447">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6596d-447">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="6596d-448">Obsługa protokołu HTTP/2</span><span class="sxs-lookup"><span data-stu-id="6596d-448">HTTP/2 support</span></span>

<span data-ttu-id="6596d-449">[Protokół HTTP/2](https://httpwg.org/specs/rfc7540.html) jest dostępny dla aplikacji ASP.NET Core, jeśli spełnione są następujące wymagania podstawowe:</span><span class="sxs-lookup"><span data-stu-id="6596d-449">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="6596d-450">&dagger; systemu operacyjnego</span><span class="sxs-lookup"><span data-stu-id="6596d-450">Operating system&dagger;</span></span>
  * <span data-ttu-id="6596d-451">Windows Server 2016/Windows 10 lub nowszy&Dagger;</span><span class="sxs-lookup"><span data-stu-id="6596d-451">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="6596d-452">Linux z OpenSSL 1.0.2 lub nowszym (na przykład Ubuntu 16,04 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="6596d-452">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="6596d-453">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="6596d-453">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="6596d-454">Połączenie [negocjowania protokołu warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="6596d-454">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="6596d-455">Protokół TLS 1.2 lub nowszej połączenia</span><span class="sxs-lookup"><span data-stu-id="6596d-455">TLS 1.2 or later connection</span></span>

<span data-ttu-id="6596d-456">&dagger;HTTP/2 będą obsługiwane w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="6596d-456">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="6596d-457">&Dagger;Kestrel ma ograniczoną obsługę protokołu HTTP/2 w systemie Windows Server 2012 R2 i Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="6596d-457">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="6596d-458">Obsługa jest ograniczona, ponieważ lista obsługiwanych mechanizmów szyfrowania TLS dostępnych w tych systemach operacyjnych jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="6596d-458">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="6596d-459">Do zabezpieczenia połączeń TLS może być wymagany certyfikat wygenerowany przy użyciu algorytmu podpisu cyfrowego (ECDSA) krzywej eliptycznej.</span><span class="sxs-lookup"><span data-stu-id="6596d-459">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="6596d-460">W przypadku ustanowienia połączenia HTTP/2 raporty [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="6596d-460">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="6596d-461">Protokół HTTP/2 jest domyślnie wyłączony.</span><span class="sxs-lookup"><span data-stu-id="6596d-461">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="6596d-462">Więcej informacji na temat konfiguracji można znaleźć w sekcjach [Kestrel Options](#kestrel-options) and [ListenOptions. Protocols](#listenoptionsprotocols) .</span><span class="sxs-lookup"><span data-stu-id="6596d-462">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="6596d-463">Kiedy używać Kestrel z zwrotnym serwerem proxy</span><span class="sxs-lookup"><span data-stu-id="6596d-463">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="6596d-464">Kestrel może być używana przez siebie lub z *odwrotnym serwerem proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org)lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="6596d-464">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="6596d-465">Odwrotny serwer proxy odbiera żądania HTTP z sieci i przekazuje je do usługi Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-465">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="6596d-466">Kestrel używany jako serwer sieci Web z krawędzią (dostępną z Internetu):</span><span class="sxs-lookup"><span data-stu-id="6596d-466">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem bez serwera proxy zwrotnego](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="6596d-468">Kestrel używany w konfiguracji zwrotnego serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="6596d-468">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel komunikuje się pośrednio z Internetem za pomocą odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="6596d-470">Konfiguracja, z serwerem proxy odwrotnych lub bez niego, jest obsługiwaną konfiguracją hostingu.</span><span class="sxs-lookup"><span data-stu-id="6596d-470">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="6596d-471">Kestrel używany jako serwer graniczny bez serwera proxy odwrotnego nie obsługuje udostępniania tego samego adresu IP i portu między wieloma procesami.</span><span class="sxs-lookup"><span data-stu-id="6596d-471">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="6596d-472">Gdy Kestrel jest skonfigurowany do nasłuchiwania na porcie, Kestrel obsługuje cały ruch dla tego portu bez względu na żądania `Host` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="6596d-472">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="6596d-473">Zwrotny serwer proxy, który może udostępniać porty, ma możliwość przekazywania żądań do Kestrel na unikatowym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="6596d-473">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="6596d-474">Nawet jeśli odwrotny serwer proxy nie jest wymagany, może to być dobry wybór przy użyciu odwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="6596d-474">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="6596d-475">Zwrotny serwer proxy:</span><span class="sxs-lookup"><span data-stu-id="6596d-475">A reverse proxy:</span></span>

* <span data-ttu-id="6596d-476">Może ograniczać uwidoczniony obszar publicznej powierzchni aplikacji, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="6596d-476">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="6596d-477">Zapewnienie dodatkowej warstwy konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="6596d-477">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="6596d-478">Integracja z istniejącą infrastrukturą może być lepsza.</span><span class="sxs-lookup"><span data-stu-id="6596d-478">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="6596d-479">Uprość Równoważenie obciążenia i konfigurację komunikacji zabezpieczonej (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6596d-479">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="6596d-480">Tylko serwer zwrotny proxy wymaga certyfikatu X. 509, a serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6596d-480">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="6596d-481">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="6596d-481">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="6596d-482">Jak używać Kestrel w aplikacjach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6596d-482">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="6596d-483">Pakiet [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6596d-483">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6596d-484">Szablony projektów ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-484">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="6596d-485">W *program.cs*kod szablonu wywołuje <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> w tle.</span><span class="sxs-lookup"><span data-stu-id="6596d-485">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="6596d-486">Aby uzyskać więcej informacji na temat `CreateDefaultBuilder` i kompilowania hosta, zobacz sekcję *Konfigurowanie hosta* w programie <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="6596d-486">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

<span data-ttu-id="6596d-487">Aby zapewnić dodatkową konfigurację po wywołaniu `CreateDefaultBuilder`, użyj `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="6596d-487">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="6596d-488">Jeśli aplikacja nie wywoła `CreateDefaultBuilder`, aby skonfigurować hosta, wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **przed** wywołaniem `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="6596d-488">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="6596d-489">Opcje Kestrel</span><span class="sxs-lookup"><span data-stu-id="6596d-489">Kestrel options</span></span>

<span data-ttu-id="6596d-490">Serwer sieci Web Kestrel ma opcje konfiguracji ograniczeń, które są szczególnie przydatne w przypadku wdrożeń mających dostęp do Internetu.</span><span class="sxs-lookup"><span data-stu-id="6596d-490">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="6596d-491">Ustaw ograniczenia we właściwości <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="6596d-491">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="6596d-492">Właściwość `Limits` przechowuje wystąpienie klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="6596d-492">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="6596d-493">W poniższych przykładach użyto przestrzeni nazw <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="6596d-493">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="6596d-494">Opcje Kestrel, które są konfigurowane w C# kodzie w poniższych przykładach, można również ustawić za pomocą [dostawcy konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6596d-494">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="6596d-495">Na przykład dostawca konfiguracji plików może załadować konfigurację Kestrel z pliku *appSettings. JSON* lub *appSettings. { Environment} plik JSON* :</span><span class="sxs-lookup"><span data-stu-id="6596d-495">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="6596d-496">Skorzystaj z **jednej** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="6596d-496">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="6596d-497">Skonfiguruj Kestrel w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6596d-497">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="6596d-498">Wstrzyknąć wystąpienie `IConfiguration` do klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="6596d-498">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="6596d-499">W poniższym przykładzie przyjęto założenie, że wprowadzona konfiguracja jest przypisana do właściwości `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="6596d-499">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="6596d-500">W `Startup.ConfigureServices`Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="6596d-500">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

     ```csharp
     using Microsoft.Extensions.Configuration
     
     public class Startup
     {
         public Startup(IConfiguration configuration)
         {
             Configuration = configuration;
         }

         public IConfiguration Configuration { get; }

         public void ConfigureServices(IServiceCollection services)
         {
             services.Configure<KestrelServerOptions>(
                 Configuration.GetSection("Kestrel"));
         }

         public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
         {
             ...
         }
     }
     ```

* <span data-ttu-id="6596d-501">Skonfiguruj Kestrel podczas kompilowania hosta:</span><span class="sxs-lookup"><span data-stu-id="6596d-501">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="6596d-502">W programie *program.cs*Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="6596d-502">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="6596d-503">Obie powyższe podejścia współpracują z dowolnym [dostawcą konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6596d-503">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="6596d-504">Limit czasu utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="6596d-504">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="6596d-505">Pobiera lub ustawia [limit czasu utrzymywania aktywności](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="6596d-505">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="6596d-506">Wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="6596d-506">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="6596d-507">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="6596d-507">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="6596d-508">Maksymalna liczba współbieżnych otwartych połączeń TCP można ustawić dla całej aplikacji przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="6596d-508">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="6596d-509">Istnieje oddzielny limit połączeń, które zostały uaktualnione z protokołu HTTP lub HTTPS do innego protokołu (na przykład w żądaniu usługi WebSockets).</span><span class="sxs-lookup"><span data-stu-id="6596d-509">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="6596d-510">Po uaktualnieniu połączenia nie jest ono wliczane do limitu `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="6596d-510">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="6596d-511">Maksymalna liczba połączeń jest domyślnie nieograniczona (null).</span><span class="sxs-lookup"><span data-stu-id="6596d-511">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="6596d-512">Maksymalny rozmiar treści żądania</span><span class="sxs-lookup"><span data-stu-id="6596d-512">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="6596d-513">Domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="6596d-513">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="6596d-514">Zalecanym podejściem do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie atrybutu <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> dla metody akcji:</span><span class="sxs-lookup"><span data-stu-id="6596d-514">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="6596d-515">Oto przykład, który pokazuje, jak skonfigurować ograniczenie dla aplikacji w każdym żądaniu:</span><span class="sxs-lookup"><span data-stu-id="6596d-515">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="6596d-516">Zastąp ustawienie określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="6596d-516">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="6596d-517">Wyjątek jest generowany, jeśli aplikacja skonfiguruje limit żądania po rozpoczęciu uruchamiania aplikacji w celu odczytania żądania.</span><span class="sxs-lookup"><span data-stu-id="6596d-517">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="6596d-518">Istnieje właściwość `IsReadOnly`, która wskazuje, czy właściwość `MaxRequestBodySize` jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="6596d-518">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="6596d-519">Gdy aplikacja jest uruchamiana [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model) [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru treści żądania Kestrel jest wyłączony, ponieważ program IIS już ustawia limit.</span><span class="sxs-lookup"><span data-stu-id="6596d-519">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="6596d-520">Minimalna stawka danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="6596d-520">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="6596d-521">Kestrel sprawdza co sekundę, jeśli dane są odbierane z określoną szybkością w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="6596d-521">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="6596d-522">Jeśli szybkość spadnie poniżej wartości minimalnej, upłynął limit czasu połączenia. Okres prolongaty to czas, przez który Kestrel nadaje klientowi zwiększenie jego szybkości wysyłania do minimum; Ta częstotliwość nie jest sprawdzana w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="6596d-522">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="6596d-523">Okres prolongaty pozwala uniknąć porzucania połączeń, które początkowo wysyłają dane z niską szybkością.</span><span class="sxs-lookup"><span data-stu-id="6596d-523">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="6596d-524">Domyślna stawka minimalna to 240 bajtów na sekundę z 5-sekundowym okresem prolongaty.</span><span class="sxs-lookup"><span data-stu-id="6596d-524">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="6596d-525">Minimalna stawka dotyczy także odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6596d-525">A minimum rate also applies to the response.</span></span> <span data-ttu-id="6596d-526">Kod określający limit żądań i limit odpowiedzi jest taki sam, chyba że ma `RequestBody` lub `Response` w nazwach właściwości i interfejsów.</span><span class="sxs-lookup"><span data-stu-id="6596d-526">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="6596d-527">Oto przykład pokazujący sposób konfigurowania minimalnych stawek danych w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="6596d-527">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="6596d-528">Przesłoń minimalne limity szybkości dla żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="6596d-528">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="6596d-529">Żadna funkcja współczynnika, do której odwołuje się poprzednia próba nie jest obecna w `HttpContext.Features` dla żądań HTTP/2, ponieważ Modyfikowanie limitów szybkości dla każdego żądania nie jest obsługiwane w przypadku protokołu HTTP/2 ze względu na obsługę multipleksera żądania.</span><span class="sxs-lookup"><span data-stu-id="6596d-529">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="6596d-530">Limity szybkości dla całego serwera skonfigurowane za pomocą `KestrelServerOptions.Limits` nadal mają zastosowanie do połączeń HTTP/1. x i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6596d-530">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="6596d-531">Limit czasu nagłówków żądań</span><span class="sxs-lookup"><span data-stu-id="6596d-531">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="6596d-532">Pobiera lub ustawia maksymalny czas, przez jaki serwer spędza nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="6596d-532">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="6596d-533">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="6596d-533">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="6596d-534">Maksymalna liczba strumieni na połączenie</span><span class="sxs-lookup"><span data-stu-id="6596d-534">Maximum streams per connection</span></span>

<span data-ttu-id="6596d-535">`Http2.MaxStreamsPerConnection` ogranicza liczbę współbieżnych strumieni żądań dla połączenia HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6596d-535">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="6596d-536">Odrzucone strumienie są nadmiarowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-536">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="6596d-537">Wartość domyślna to 100.</span><span class="sxs-lookup"><span data-stu-id="6596d-537">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="6596d-538">Rozmiar tabeli nagłówka</span><span class="sxs-lookup"><span data-stu-id="6596d-538">Header table size</span></span>

<span data-ttu-id="6596d-539">Dekoder HPACK dekompresuje nagłówki HTTP dla połączeń HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6596d-539">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="6596d-540">`Http2.HeaderTableSize` ogranicza rozmiar tabeli kompresji nagłówka używanej przez dekoder HPACK.</span><span class="sxs-lookup"><span data-stu-id="6596d-540">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="6596d-541">Wartość jest podawana w oktetach i musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="6596d-541">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="6596d-542">Wartość domyślna to 4096.</span><span class="sxs-lookup"><span data-stu-id="6596d-542">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="6596d-543">Maksymalny rozmiar ramki</span><span class="sxs-lookup"><span data-stu-id="6596d-543">Maximum frame size</span></span>

<span data-ttu-id="6596d-544">`Http2.MaxFrameSize` wskazuje maksymalny rozmiar ładunku ramki połączenia HTTP/2 do odebrania.</span><span class="sxs-lookup"><span data-stu-id="6596d-544">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="6596d-545">Wartość jest podawana w oktetach i musi należeć do przedziału od 2 ^ 14 (16 384) do 2 ^ 24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="6596d-545">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="6596d-546">Wartość domyślna to 2 ^ 14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="6596d-546">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="6596d-547">Maksymalny rozmiar nagłówka żądania</span><span class="sxs-lookup"><span data-stu-id="6596d-547">Maximum request header size</span></span>

<span data-ttu-id="6596d-548">`Http2.MaxRequestHeaderFieldSize` wskazuje maksymalny dozwolony rozmiar w oktetach wartości nagłówka żądania.</span><span class="sxs-lookup"><span data-stu-id="6596d-548">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="6596d-549">Ten limit dotyczy zarówno nazwy, jak i wartości razem w skompresowanych i nieskompresowanych reprezentacji.</span><span class="sxs-lookup"><span data-stu-id="6596d-549">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="6596d-550">Wartość musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="6596d-550">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="6596d-551">Wartość domyślna to 8 192.</span><span class="sxs-lookup"><span data-stu-id="6596d-551">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="6596d-552">Początkowy rozmiar okna połączenia</span><span class="sxs-lookup"><span data-stu-id="6596d-552">Initial connection window size</span></span>

<span data-ttu-id="6596d-553">`Http2.InitialConnectionWindowSize` wskazuje maksymalne dane dotyczące treści żądania w bajtach, które bufory serwera w tym samym czasie są agregowane dla wszystkich żądań (strumieni) na połączenie.</span><span class="sxs-lookup"><span data-stu-id="6596d-553">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="6596d-554">Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="6596d-554">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="6596d-555">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="6596d-555">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="6596d-556">Wartość domyślna to 128 KB (131 072).</span><span class="sxs-lookup"><span data-stu-id="6596d-556">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="6596d-557">Rozmiar początkowego okna strumienia</span><span class="sxs-lookup"><span data-stu-id="6596d-557">Initial stream window size</span></span>

<span data-ttu-id="6596d-558">`Http2.InitialStreamWindowSize` wskazuje maksymalne dane dotyczące treści żądania w bajtach bufory serwerów w jednym momencie na żądanie (Stream).</span><span class="sxs-lookup"><span data-stu-id="6596d-558">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="6596d-559">Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="6596d-559">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="6596d-560">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="6596d-560">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="6596d-561">Wartość domyślna to 96 KB (98 304).</span><span class="sxs-lookup"><span data-stu-id="6596d-561">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="6596d-562">Synchroniczne we/wy</span><span class="sxs-lookup"><span data-stu-id="6596d-562">Synchronous IO</span></span>

<span data-ttu-id="6596d-563"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> kontroluje, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6596d-563"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="6596d-564">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="6596d-564">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="6596d-565">Duża liczba blokowania synchronicznych operacji we/wy może prowadzić do zablokowania puli wątków, co sprawia, że aplikacja nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="6596d-565">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="6596d-566">Włącz `AllowSynchronousIO` tylko w przypadku korzystania z biblioteki, która nie obsługuje asynchronicznych operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="6596d-566">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="6596d-567">Poniższy przykład włącza synchroniczną operację we/wy:</span><span class="sxs-lookup"><span data-stu-id="6596d-567">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="6596d-568">Aby uzyskać informacje o innych opcjach i ograniczeniach Kestrel, zobacz:</span><span class="sxs-lookup"><span data-stu-id="6596d-568">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="6596d-569">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="6596d-569">Endpoint configuration</span></span>

<span data-ttu-id="6596d-570">Domyślnie ASP.NET Core wiąże się z:</span><span class="sxs-lookup"><span data-stu-id="6596d-570">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="6596d-571">`https://localhost:5001` (gdy obecny jest lokalny certyfikat programistyczny)</span><span class="sxs-lookup"><span data-stu-id="6596d-571">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="6596d-572">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="6596d-572">Specify URLs using the:</span></span>

* <span data-ttu-id="6596d-573">Zmienna środowiskowa `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="6596d-573">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="6596d-574">`--urls` argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="6596d-574">`--urls` command-line argument.</span></span>
* <span data-ttu-id="6596d-575">klucz konfiguracji hosta `urls`.</span><span class="sxs-lookup"><span data-stu-id="6596d-575">`urls` host configuration key.</span></span>
* <span data-ttu-id="6596d-576">Metoda rozszerzenia `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6596d-576">`UseUrls` extension method.</span></span>

<span data-ttu-id="6596d-577">Wartość podana przy użyciu tych metod może być jednym lub większą liczbą punktów końcowych HTTP i HTTPS (HTTPS, jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="6596d-577">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="6596d-578">Skonfiguruj wartość jako listę rozdzieloną średnikami (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="6596d-578">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="6596d-579">Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwera](xref:fundamentals/host/web-host#server-urls) i [Zastąp konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="6596d-579">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="6596d-580">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="6596d-580">A development certificate is created:</span></span>

* <span data-ttu-id="6596d-581">Po zainstalowaniu [zestaw .NET Core SDK](/dotnet/core/sdk) .</span><span class="sxs-lookup"><span data-stu-id="6596d-581">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="6596d-582">Do utworzenia certyfikatu służy [Narzędzie dev-certs](xref:aspnetcore-2.1#https) .</span><span class="sxs-lookup"><span data-stu-id="6596d-582">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="6596d-583">Niektóre przeglądarki wymagają przyznania jawnego uprawnienia do zaufania do lokalnego certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="6596d-583">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="6596d-584">Szablony projektu konfigurują aplikacje do uruchamiania domyślnie przy użyciu protokołu HTTPS i obejmują [przekierowania https i obsługę HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="6596d-584">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="6596d-585">Wywołaj metody <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> w <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, aby skonfigurować prefiksy i porty adresów URL dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-585">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="6596d-586">`UseUrls`, `--urls` argument wiersza polecenia, klucz konfiguracji hosta `urls` oraz zmienna środowiskowa `ASPNETCORE_URLS` również działają, ale mają ograniczenia wymienione w dalszej części tej sekcji (certyfikat domyślny musi być dostępny do konfiguracji punktu końcowego HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6596d-586">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="6596d-587">Konfiguracja `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="6596d-587">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="6596d-588">ConfigureEndpointDefaults (Akcja\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="6596d-588">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="6596d-589">Określa `Action` konfiguracji do uruchomienia dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="6596d-589">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="6596d-590">Wywoływanie `ConfigureEndpointDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="6596d-590">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="6596d-591">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="6596d-591">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="6596d-592">ConfigureHttpsDefaults (Akcja\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="6596d-592">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="6596d-593">Określa `Action` konfiguracji do uruchomienia dla każdego punktu końcowego HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6596d-593">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="6596d-594">Wywoływanie `ConfigureHttpsDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="6596d-594">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="6596d-595">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="6596d-595">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>


### <a name="configureiconfiguration"></a><span data-ttu-id="6596d-596">Konfiguruj (IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="6596d-596">Configure(IConfiguration)</span></span>

<span data-ttu-id="6596d-597">Tworzy moduł ładujący konfigurację służący do konfigurowania Kestrel, który pobiera <xref:Microsoft.Extensions.Configuration.IConfiguration> jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-597">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="6596d-598">Konfiguracja musi być objęta zakresem sekcji konfiguracji dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-598">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="6596d-599">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="6596d-599">ListenOptions.UseHttps</span></span>

<span data-ttu-id="6596d-600">Skonfiguruj Kestrel do korzystania z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6596d-600">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="6596d-601">rozszerzenia `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="6596d-601">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="6596d-602">`UseHttps` &ndash; skonfigurować Kestrel do używania protokołu HTTPS z certyfikatem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="6596d-602">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="6596d-603">Zgłasza wyjątek, jeśli nie został skonfigurowany żaden certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="6596d-603">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="6596d-604">`ListenOptions.UseHttps` parametry:</span><span class="sxs-lookup"><span data-stu-id="6596d-604">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="6596d-605">`filename` to ścieżka i nazwa pliku certyfikatu odnosząca się do katalogu zawierającego pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6596d-605">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="6596d-606">`password` to hasło wymagane do uzyskania dostępu do danych certyfikatu X. 509.</span><span class="sxs-lookup"><span data-stu-id="6596d-606">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="6596d-607">`configureOptions` jest `Action`, aby skonfigurować `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="6596d-607">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="6596d-608">Zwraca `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="6596d-608">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="6596d-609">`storeName` to magazyn certyfikatów, z którego ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="6596d-609">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="6596d-610">`subject` to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="6596d-610">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="6596d-611">`allowInvalid` wskazuje, czy należy wziąć pod uwagę nieprawidłowe certyfikaty, takie jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="6596d-611">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="6596d-612">`location` to lokalizacja magazynu, z której ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="6596d-612">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="6596d-613">`serverCertificate` to certyfikat X. 509.</span><span class="sxs-lookup"><span data-stu-id="6596d-613">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="6596d-614">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6596d-614">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="6596d-615">Należy podać co najmniej certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="6596d-615">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="6596d-616">Obsługiwane konfiguracje opisane dalej:</span><span class="sxs-lookup"><span data-stu-id="6596d-616">Supported configurations described next:</span></span>

* <span data-ttu-id="6596d-617">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="6596d-617">No configuration</span></span>
* <span data-ttu-id="6596d-618">Zastąp domyślny certyfikat z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="6596d-618">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="6596d-619">Zmień wartości domyślne w kodzie</span><span class="sxs-lookup"><span data-stu-id="6596d-619">Change the defaults in code</span></span>

<span data-ttu-id="6596d-620">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="6596d-620">*No configuration*</span></span>

<span data-ttu-id="6596d-621">Kestrel nasłuchuje na `http://localhost:5000` i `https://localhost:5001` (jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="6596d-621">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="6596d-622">*Zastąp domyślny certyfikat z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="6596d-622">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="6596d-623">`CreateDefaultBuilder` wywołania `Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-623">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="6596d-624">Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępny dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-624">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="6596d-625">Konfigurowanie wielu punktów końcowych, w tym adresów URL i certyfikatów do użycia, z pliku znajdującego się na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="6596d-625">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="6596d-626">W poniższym przykładzie pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="6596d-626">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="6596d-627">Ustaw **AllowInvalid** na `true`, aby zezwolić na korzystanie z nieprawidłowych certyfikatów (na przykład certyfikatów z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="6596d-627">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="6596d-628">Wszystkie punkty końcowe HTTPS, które nie określają certyfikatu (**HttpsDefaultCert** w poniższym przykładzie) powracają do certyfikatu zdefiniowanego w obszarze **Certyfikaty** > **domyślnym** lub certyfikatem deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="6596d-628">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="6596d-629">Alternatywą dla korzystania z **ścieżki** i **hasła** dla dowolnego węzła certyfikatu jest określenie certyfikatu przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="6596d-629">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="6596d-630">Na przykład **certyfikaty** > **domyślnego** certyfikatu można określić jako:</span><span class="sxs-lookup"><span data-stu-id="6596d-630">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="6596d-631">Uwagi dotyczące schematu:</span><span class="sxs-lookup"><span data-stu-id="6596d-631">Schema notes:</span></span>

* <span data-ttu-id="6596d-632">W nazwach punktów końcowych nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="6596d-632">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="6596d-633">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-633">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="6596d-634">Dla każdego punktu końcowego jest wymagany parametr `Url`.</span><span class="sxs-lookup"><span data-stu-id="6596d-634">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="6596d-635">Format tego parametru jest taki sam jak parametr konfiguracji `Urls` najwyższego poziomu, z tą różnicą, że jest ograniczony do pojedynczej wartości.</span><span class="sxs-lookup"><span data-stu-id="6596d-635">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="6596d-636">Te punkty końcowe zastępują te zdefiniowane w konfiguracji `Urls` najwyższego poziomu zamiast dodawać do nich.</span><span class="sxs-lookup"><span data-stu-id="6596d-636">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="6596d-637">Punkty końcowe zdefiniowane w kodzie za pośrednictwem `Listen` kumulują się z punktami końcowymi zdefiniowanymi w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="6596d-637">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="6596d-638">Sekcja `Certificate` jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="6596d-638">The `Certificate` section is optional.</span></span> <span data-ttu-id="6596d-639">Jeśli sekcja `Certificate` nie zostanie określona, są używane wartości domyślne zdefiniowane we wcześniejszych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="6596d-639">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="6596d-640">Jeśli żadne wartości domyślne nie są dostępne, serwer zgłasza wyjątek i nie może się uruchomić.</span><span class="sxs-lookup"><span data-stu-id="6596d-640">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="6596d-641">Sekcja `Certificate` obsługuje zarówno **ścieżkę**&ndash;**hasło** , jak i **temat**&ndash;certyfikatów **magazynu** .</span><span class="sxs-lookup"><span data-stu-id="6596d-641">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="6596d-642">W ten sposób można zdefiniować dowolną liczbę punktów końcowych, dopóki nie spowoduje to konfliktów portów.</span><span class="sxs-lookup"><span data-stu-id="6596d-642">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="6596d-643">`options.Configure(context.Configuration.GetSection("{SECTION}"))` zwraca `KestrelConfigurationLoader` z metodą `.Endpoint(string name, listenOptions => { })`, której można użyć do uzupełnienia ustawień skonfigurowanego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="6596d-643">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="6596d-644">dostęp do `KestrelServerOptions.ConfigurationLoader` można uzyskać, aby kontynuować iterację istniejącego modułu ładującego, takiego jak dostarczony przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6596d-644">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="6596d-645">Sekcja konfiguracji dla każdego punktu końcowego jest dostępna w opcjach w `Endpoint` metodzie, aby można było odczytać ustawienia niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-645">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="6596d-646">Można załadować wiele konfiguracji, wywołując `options.Configure(context.Configuration.GetSection("{SECTION}"))` ponownie z inną sekcją.</span><span class="sxs-lookup"><span data-stu-id="6596d-646">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="6596d-647">Używana jest tylko Ostatnia konfiguracja, chyba że `Load` jest jawnie wywoływana w poprzednich wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="6596d-647">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="6596d-648">Pakiet nie wywołuje `Load`, tak aby jego sekcja konfiguracji domyślnej mogła zostać zamieniona.</span><span class="sxs-lookup"><span data-stu-id="6596d-648">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="6596d-649">`KestrelConfigurationLoader` odzwierciedla `Listen`ą rodzinę interfejsów API z `KestrelServerOptions` jako przeciążania `Endpoint`, dlatego punkty końcowe kodu i konfiguracji można skonfigurować w tym samym miejscu.</span><span class="sxs-lookup"><span data-stu-id="6596d-649">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="6596d-650">Te przeciążenia nie używają nazw i używają tylko ustawień domyślnych z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="6596d-650">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="6596d-651">*Zmień wartości domyślne w kodzie*</span><span class="sxs-lookup"><span data-stu-id="6596d-651">*Change the defaults in code*</span></span>

<span data-ttu-id="6596d-652">`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` mogą służyć do zmiany domyślnych ustawień dla `ListenOptions` i `HttpsConnectionAdapterOptions`, w tym przesłanianie domyślnego certyfikatu określonego w poprzednim scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="6596d-652">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="6596d-653">przed skonfigurowaniem punktów końcowych należy wywołać `ConfigureEndpointDefaults` i `ConfigureHttpsDefaults`.</span><span class="sxs-lookup"><span data-stu-id="6596d-653">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="6596d-654">*Obsługa usługi Kestrel dla SNI*</span><span class="sxs-lookup"><span data-stu-id="6596d-654">*Kestrel support for SNI*</span></span>

<span data-ttu-id="6596d-655">[Oznaczanie nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do hostowania wielu domen na tym samym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="6596d-655">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="6596d-656">Aby SNI działał, klient wysyła nazwę hosta dla bezpiecznej sesji do serwera podczas uzgadniania TLS, aby serwer mógł zapewnić prawidłowy certyfikat.</span><span class="sxs-lookup"><span data-stu-id="6596d-656">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="6596d-657">Klient korzysta z dostarczonego certyfikatu w celu zaszyfrowania komunikacji z serwerem podczas bezpiecznej sesji, która następuje po uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="6596d-657">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="6596d-658">Kestrel obsługuje SNI za pośrednictwem wywołania zwrotnego `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="6596d-658">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="6596d-659">Wywołanie zwrotne jest wywoływane jednokrotnie dla każdego połączenia, aby umożliwić aplikacji inspekcję nazwy hosta i wybieranie odpowiedniego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="6596d-659">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="6596d-660">Obsługa SNI wymaga:</span><span class="sxs-lookup"><span data-stu-id="6596d-660">SNI support requires:</span></span>

* <span data-ttu-id="6596d-661">Uruchomione na platformie docelowej `netcoreapp2.1` lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="6596d-661">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="6596d-662">W `net461` lub później wywołanie zwrotne jest wywoływane, ale `name` jest zawsze `null`.</span><span class="sxs-lookup"><span data-stu-id="6596d-662">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="6596d-663">`name` jest również `null`, jeśli klient nie poda parametru nazwy hosta w uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="6596d-663">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="6596d-664">Wszystkie witryny sieci Web działają na tym samym wystąpieniu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-664">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="6596d-665">Kestrel nie obsługuje udostępniania adresu IP i portu w wielu wystąpieniach bez zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="6596d-665">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="6596d-666">Rejestrowanie połączeń</span><span class="sxs-lookup"><span data-stu-id="6596d-666">Connection logging</span></span>

<span data-ttu-id="6596d-667">Wywołaj <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>, aby emitować dzienniki poziomu debugowania dla komunikacji na poziomie bajtów w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="6596d-667">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="6596d-668">Rejestrowanie połączeń ułatwia rozwiązywanie problemów z komunikacją na niskim poziomie, na przykład podczas szyfrowania TLS i za serwerami proxy.</span><span class="sxs-lookup"><span data-stu-id="6596d-668">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="6596d-669">Jeśli `UseConnectionLogging` jest umieszczony przed `UseHttps`, szyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="6596d-669">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="6596d-670">Jeśli `UseConnectionLogging` jest umieszczony po `UseHttps`, odszyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="6596d-670">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="6596d-671">Powiąż z gniazdem TCP</span><span class="sxs-lookup"><span data-stu-id="6596d-671">Bind to a TCP socket</span></span>

<span data-ttu-id="6596d-672">Metoda <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> wiąże się z gniazdem TCP, a opcja lambda zezwala na konfigurację certyfikatu X. 509:</span><span class="sxs-lookup"><span data-stu-id="6596d-672">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="6596d-673">Przykład konfiguruje HTTPS dla punktu końcowego z <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="6596d-673">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="6596d-674">Użyj tego samego interfejsu API, aby skonfigurować inne ustawienia Kestrel dla określonych punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="6596d-674">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="6596d-675">Powiąż z gniazdem systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="6596d-675">Bind to a Unix socket</span></span>

<span data-ttu-id="6596d-676">Nasłuchiwanie w gnieździe systemu UNIX za pomocą <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, aby zwiększyć wydajność za pomocą Nginx, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="6596d-676">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

* <span data-ttu-id="6596d-677">W pliku Nginx confiuguration Ustaw `proxy_pass` `server` > `location` > `http://unix:/tmp/{KESTREL SOCKET}:/;`.</span><span class="sxs-lookup"><span data-stu-id="6596d-677">In the Nginx confiuguration file, set the `server` > `location` > `proxy_pass` entry to `http://unix:/tmp/{KESTREL SOCKET}:/;`.</span></span> <span data-ttu-id="6596d-678">`{KESTREL SOCKET}` to nazwa gniazda dostarczanego do <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> (na przykład `kestrel-test.sock` w powyższym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="6596d-678">`{KESTREL SOCKET}` is the name of the socket provided to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> (for example, `kestrel-test.sock` in the preceding example).</span></span>
* <span data-ttu-id="6596d-679">Upewnij się, że gniazdo jest zapisywalne przez Nginx (na przykład `chmod go+w /tmp/kestrel-test.sock`).</span><span class="sxs-lookup"><span data-stu-id="6596d-679">Ensure that the socket is writeable by Nginx (for example, `chmod go+w /tmp/kestrel-test.sock`).</span></span> 

### <a name="port-0"></a><span data-ttu-id="6596d-680">Port 0</span><span class="sxs-lookup"><span data-stu-id="6596d-680">Port 0</span></span>

<span data-ttu-id="6596d-681">Gdy `0` jest określony numer portu, Kestrel dynamicznie wiąże się z dostępnym portem.</span><span class="sxs-lookup"><span data-stu-id="6596d-681">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="6596d-682">W poniższym przykładzie pokazano, jak określić, który port Kestrel faktycznie powiązany w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="6596d-682">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="6596d-683">Po uruchomieniu aplikacji dane wyjściowe okna konsoli wskazują port dynamiczny, w którym można uzyskać dostęp do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="6596d-683">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="6596d-684">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="6596d-684">Limitations</span></span>

<span data-ttu-id="6596d-685">Skonfiguruj punkty końcowe przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="6596d-685">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="6596d-686">`--urls` argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="6596d-686">`--urls` command-line argument</span></span>
* <span data-ttu-id="6596d-687">klucz konfiguracji hosta `urls`</span><span class="sxs-lookup"><span data-stu-id="6596d-687">`urls` host configuration key</span></span>
* <span data-ttu-id="6596d-688">Zmienna środowiskowa `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="6596d-688">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="6596d-689">Te metody są przydatne do tworzenia kodu w pracy z serwerami innymi niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-689">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="6596d-690">Należy jednak pamiętać o następujących ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="6596d-690">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="6596d-691">Nie można użyć protokołu HTTPS z tymi metodami, chyba że w konfiguracji punktu końcowego HTTPS jest podany certyfikat domyślny (na przykład za pomocą konfiguracji `KestrelServerOptions` lub pliku konfiguracji, jak pokazano wcześniej w tym temacie).</span><span class="sxs-lookup"><span data-stu-id="6596d-691">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="6596d-692">Gdy oba podejścia `Listen` i `UseUrls` są używane jednocześnie, `Listen` punkty końcowe przesłaniają `UseUrls` punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-692">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="6596d-693">Konfiguracja punktu końcowego usług IIS</span><span class="sxs-lookup"><span data-stu-id="6596d-693">IIS endpoint configuration</span></span>

<span data-ttu-id="6596d-694">W przypadku korzystania z usług IIS powiązania URL dla powiązań przesłonięć usług IIS są ustawiane przez `Listen` lub `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6596d-694">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="6596d-695">Aby uzyskać więcej informacji, zobacz temat [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="6596d-695">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="6596d-696">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="6596d-696">ListenOptions.Protocols</span></span>

<span data-ttu-id="6596d-697">Właściwość `Protocols` ustanawia protokoły HTTP (`HttpProtocols`) włączone w punkcie końcowym połączenia lub na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6596d-697">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="6596d-698">Przypisz wartość do właściwości `Protocols` z wyliczenia `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="6596d-698">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="6596d-699">`HttpProtocols` wartość wyliczenia</span><span class="sxs-lookup"><span data-stu-id="6596d-699">`HttpProtocols` enum value</span></span> | <span data-ttu-id="6596d-700">Dozwolony protokół połączenia</span><span class="sxs-lookup"><span data-stu-id="6596d-700">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="6596d-701">Tylko HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="6596d-701">HTTP/1.1 only.</span></span> <span data-ttu-id="6596d-702">Może być używany z protokołem TLS lub bez niego.</span><span class="sxs-lookup"><span data-stu-id="6596d-702">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="6596d-703">Tylko HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6596d-703">HTTP/2 only.</span></span> <span data-ttu-id="6596d-704">Mogą być używane bez protokołu TLS tylko wtedy, gdy klient obsługuje [poprzedni tryb wiedzy](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="6596d-704">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="6596d-705">HTTP/1.1 i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6596d-705">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="6596d-706">Protokół HTTP/2 wymaga połączenia protokołów TLS i [warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3) . w przeciwnym razie wartość domyślna połączenia to HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="6596d-706">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="6596d-707">Domyślnym protokołem jest HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="6596d-707">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="6596d-708">Ograniczenia protokołu TLS dla protokołu HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="6596d-708">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="6596d-709">TLS w wersji 1,2 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="6596d-709">TLS version 1.2 or later</span></span>
* <span data-ttu-id="6596d-710">Ponowne negocjowanie wyłączone</span><span class="sxs-lookup"><span data-stu-id="6596d-710">Renegotiation disabled</span></span>
* <span data-ttu-id="6596d-711">Kompresja wyłączona</span><span class="sxs-lookup"><span data-stu-id="6596d-711">Compression disabled</span></span>
* <span data-ttu-id="6596d-712">Minimalne rozmiary tymczasowych kluczy wymiany:</span><span class="sxs-lookup"><span data-stu-id="6596d-712">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="6596d-713">Krzywa eliptyczna Diffie-Hellmana (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 BITS</span><span class="sxs-lookup"><span data-stu-id="6596d-713">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="6596d-714">Ograniczone pole Diffie-Hellmana (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bitów</span><span class="sxs-lookup"><span data-stu-id="6596d-714">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="6596d-715">Mechanizm szyfrowania nie został zabroniony</span><span class="sxs-lookup"><span data-stu-id="6596d-715">Cipher suite not blacklisted</span></span>

<span data-ttu-id="6596d-716">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; z krzywą eliptyczna P-256 &lbrack;`FIPS186`&rbrack; jest domyślnie obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="6596d-716">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="6596d-717">Poniższy przykład umożliwia nawiązywanie połączeń HTTP/1.1 i HTTP/2 na porcie 8000.</span><span class="sxs-lookup"><span data-stu-id="6596d-717">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="6596d-718">Połączenia są zabezpieczone przez protokół TLS przy użyciu podanego certyfikatu:</span><span class="sxs-lookup"><span data-stu-id="6596d-718">Connections are secured by TLS with a supplied certificate:</span></span>

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

<span data-ttu-id="6596d-719">Opcjonalnie można utworzyć implementację `IConnectionAdapter` do filtrowania uzgadniania protokołu TLS dla poszczególnych połączeń dla określonych szyfrów:</span><span class="sxs-lookup"><span data-stu-id="6596d-719">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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

<span data-ttu-id="6596d-720">*Ustawianie protokołu z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="6596d-720">*Set the protocol from configuration*</span></span>

<span data-ttu-id="6596d-721"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-721"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="6596d-722">W poniższym przykładzie pliku *appSettings. JSON* jest ustanawiany domyślny protokół połączeń (http/1.1 i http/2) dla wszystkich punktów końcowych Kestrel:</span><span class="sxs-lookup"><span data-stu-id="6596d-722">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="6596d-723">Poniższy przykład pliku konfiguracji ustanawia protokół połączenia dla określonego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="6596d-723">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="6596d-724">Protokoły określone w wartościach zastąpienia kodu ustawione przez konfigurację.</span><span class="sxs-lookup"><span data-stu-id="6596d-724">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="6596d-725">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="6596d-725">Transport configuration</span></span>

<span data-ttu-id="6596d-726">W wersji platformy ASP.NET Core 2.1 transport domyślny serwera Kestrel nie jest już oparty na bibliotece libuv, ale na zarządzanych gniazdach.</span><span class="sxs-lookup"><span data-stu-id="6596d-726">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="6596d-727">Jest to istotna zmiana dla aplikacji ASP.NET Core 2,0 uaktualniana do 2,1, które wywołują <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> i zależą od jednego z następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="6596d-727">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="6596d-728">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (bezpośrednie odwołanie do pakietu)</span><span class="sxs-lookup"><span data-stu-id="6596d-728">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="6596d-729">Microsoft. AspNetCore. App</span><span class="sxs-lookup"><span data-stu-id="6596d-729">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="6596d-730">Dla projektów, które wymagają użycia Libuv:</span><span class="sxs-lookup"><span data-stu-id="6596d-730">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="6596d-731">Dodaj zależność dla pakietu [Microsoft. AspNetCore. Server. Kestrel. transport. Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="6596d-731">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="6596d-732"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>wywołania:</span><span class="sxs-lookup"><span data-stu-id="6596d-732">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="6596d-733">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="6596d-733">URL prefixes</span></span>

<span data-ttu-id="6596d-734">W przypadku korzystania z `UseUrls`, `--urls` argumentu wiersza polecenia, klucza konfiguracji hosta `urls` lub zmiennej środowiskowej `ASPNETCORE_URLS`, prefiksy adresów URL mogą mieć jeden z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="6596d-734">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="6596d-735">Tylko prefiksy adresów URL HTTP są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-735">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="6596d-736">Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązań adresów URL przy użyciu `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6596d-736">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="6596d-737">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="6596d-737">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="6596d-738">`0.0.0.0` jest szczególnym przypadkiem, który wiąże się ze wszystkimi adresami IPv4.</span><span class="sxs-lookup"><span data-stu-id="6596d-738">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="6596d-739">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="6596d-739">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="6596d-740">`[::]` to odpowiednik IPv6 `0.0.0.0`IPv4.</span><span class="sxs-lookup"><span data-stu-id="6596d-740">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="6596d-741">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="6596d-741">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="6596d-742">Nazwy hostów, `*`i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="6596d-742">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="6596d-743">Wszystkie nie są rozpoznawane jako prawidłowy adres IP lub `localhost` są powiązane z protokołem IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="6596d-743">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="6596d-744">Aby powiązać różne nazwy hostów z różnymi ASP.NET Core aplikacjami na tym samym porcie, użyj [protokołu HTTP. sys](xref:fundamentals/servers/httpsys) lub odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="6596d-744">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="6596d-745">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="6596d-745">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="6596d-746">Nazwa hosta `localhost` z numerem portu lub adresem IP sprzężenia zwrotnego z numerem portu</span><span class="sxs-lookup"><span data-stu-id="6596d-746">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="6596d-747">Gdy `localhost` jest określony, Kestrel próbuje powiązać z interfejsami sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="6596d-747">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="6596d-748">Jeśli żądany port jest używany przez inną usługę w dowolnym interfejsie sprzężenia zwrotnego, uruchomienie Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="6596d-748">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="6596d-749">Jeśli którykolwiek z innych przyczyn interfejsu sprzężenia zwrotnego jest niedostępny (najczęściej jest to spowodowane tym, że protokół IPv6 nie jest obsługiwany), Kestrel rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="6596d-749">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="6596d-750">Filtrowanie hostów</span><span class="sxs-lookup"><span data-stu-id="6596d-750">Host filtering</span></span>

<span data-ttu-id="6596d-751">Chociaż usługa Kestrel obsługuje konfigurację na podstawie prefiksów, takich jak `http://example.com:5000`, Kestrel w znacznym stopniu ignoruje nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="6596d-751">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="6596d-752">`localhost` hosta jest specjalnym przypadkiem używanym do tworzenia powiązań z adresami sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="6596d-752">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="6596d-753">Każdy host poza jawnym adresem IP tworzy powiązanie ze wszystkimi publicznymi adresami IP.</span><span class="sxs-lookup"><span data-stu-id="6596d-753">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="6596d-754">nagłówki `Host` nie są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="6596d-754">`Host` headers aren't validated.</span></span>

<span data-ttu-id="6596d-755">Aby obejść ten sposób, użyj oprogramowania pośredniczącego filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="6596d-755">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="6596d-756">Oprogramowanie pośredniczące do filtrowania hosta jest dostarczane przez pakiet [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , który jest zawarty w pakiecie [Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub 2,2).</span><span class="sxs-lookup"><span data-stu-id="6596d-756">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="6596d-757">Oprogramowanie pośredniczące jest dodawane przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="6596d-757">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="6596d-758">Programowe filtrowanie hosta jest domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="6596d-758">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="6596d-759">Aby włączyć oprogramowanie pośredniczące, zdefiniuj klucz `AllowedHosts` w pliku *appSettings. json*/*appSettings.\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="6596d-759">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="6596d-760">Wartość to rozdzielana średnikami lista nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="6596d-760">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="6596d-761">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="6596d-761">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="6596d-762">W przypadku [przekierowanych nagłówków oprogramowanie pośredniczące](xref:host-and-deploy/proxy-load-balancer) ma również opcję <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="6596d-762">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="6596d-763">Przekazane nagłówki — oprogramowanie pośredniczące i filtrowanie hostów oprogramowanie pośredniczące ma podobną funkcjonalność dla różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="6596d-763">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="6596d-764">Ustawienie `AllowedHosts` z przekierowanymi nagłówkami — oprogramowanie pośredniczące jest odpowiednie, gdy nagłówek `Host` nie jest zachowywany podczas przekazywania żądań z odwrotnym serwerem proxy lub modułem równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="6596d-764">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="6596d-765">Ustawienie `AllowedHosts` przy użyciu oprogramowania pośredniczącego filtrowania hosta jest odpowiednie, gdy Kestrel jest używany jako publiczny serwer graniczny lub gdy `Host` nagłówek jest bezpośrednio przekazywany.</span><span class="sxs-lookup"><span data-stu-id="6596d-765">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="6596d-766">Aby uzyskać więcej informacji na temat przekierowanych nagłówków, należy zapoznać się z tematem <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="6596d-766">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="6596d-767">Kestrel to Międzyplatformowy [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="6596d-767">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="6596d-768">Kestrel to serwer sieci Web, który jest domyślnie uwzględniony w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6596d-768">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="6596d-769">Kestrel obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="6596d-769">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="6596d-770">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6596d-770">HTTPS</span></span>
* <span data-ttu-id="6596d-771">Nieprzezroczyste uaktualnienie używane do włączania obsługi obiektów [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="6596d-771">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="6596d-772">Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx</span><span class="sxs-lookup"><span data-stu-id="6596d-772">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="6596d-773">Kestrel jest obsługiwana na wszystkich platformach i wersjach obsługiwanych przez platformę .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6596d-773">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="6596d-774">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6596d-774">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="6596d-775">Kiedy używać Kestrel z zwrotnym serwerem proxy</span><span class="sxs-lookup"><span data-stu-id="6596d-775">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="6596d-776">Kestrel może być używana przez siebie lub z *odwrotnym serwerem proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org)lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="6596d-776">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="6596d-777">Odwrotny serwer proxy odbiera żądania HTTP z sieci i przekazuje je do usługi Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-777">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="6596d-778">Kestrel używany jako serwer sieci Web z krawędzią (dostępną z Internetu):</span><span class="sxs-lookup"><span data-stu-id="6596d-778">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem bez serwera proxy zwrotnego](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="6596d-780">Kestrel używany w konfiguracji zwrotnego serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="6596d-780">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel komunikuje się pośrednio z Internetem za pomocą odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="6596d-782">Konfiguracja, z serwerem proxy odwrotnych lub bez niego, jest obsługiwaną konfiguracją hostingu.</span><span class="sxs-lookup"><span data-stu-id="6596d-782">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="6596d-783">Kestrel używany jako serwer graniczny bez serwera proxy odwrotnego nie obsługuje udostępniania tego samego adresu IP i portu między wieloma procesami.</span><span class="sxs-lookup"><span data-stu-id="6596d-783">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="6596d-784">Gdy Kestrel jest skonfigurowany do nasłuchiwania na porcie, Kestrel obsługuje cały ruch dla tego portu bez względu na żądania `Host` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="6596d-784">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="6596d-785">Zwrotny serwer proxy, który może udostępniać porty, ma możliwość przekazywania żądań do Kestrel na unikatowym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="6596d-785">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="6596d-786">Nawet jeśli odwrotny serwer proxy nie jest wymagany, może to być dobry wybór przy użyciu odwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="6596d-786">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="6596d-787">Zwrotny serwer proxy:</span><span class="sxs-lookup"><span data-stu-id="6596d-787">A reverse proxy:</span></span>

* <span data-ttu-id="6596d-788">Może ograniczać uwidoczniony obszar publicznej powierzchni aplikacji, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="6596d-788">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="6596d-789">Zapewnienie dodatkowej warstwy konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="6596d-789">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="6596d-790">Integracja z istniejącą infrastrukturą może być lepsza.</span><span class="sxs-lookup"><span data-stu-id="6596d-790">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="6596d-791">Uprość Równoważenie obciążenia i konfigurację komunikacji zabezpieczonej (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6596d-791">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="6596d-792">Tylko serwer zwrotny proxy wymaga certyfikatu X. 509, a serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6596d-792">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="6596d-793">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="6596d-793">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="6596d-794">Jak używać Kestrel w aplikacjach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6596d-794">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="6596d-795">Pakiet [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6596d-795">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6596d-796">Szablony projektów ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-796">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="6596d-797">W *program.cs*kod szablonu wywołuje <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> w tle.</span><span class="sxs-lookup"><span data-stu-id="6596d-797">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="6596d-798">Aby zapewnić dodatkową konfigurację po wywołaniu `CreateDefaultBuilder`, wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span><span class="sxs-lookup"><span data-stu-id="6596d-798">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="6596d-799">Aby uzyskać więcej informacji na temat `CreateDefaultBuilder` i kompilowania hosta, zobacz sekcję *Konfigurowanie hosta* w programie <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="6596d-799">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="kestrel-options"></a><span data-ttu-id="6596d-800">Opcje Kestrel</span><span class="sxs-lookup"><span data-stu-id="6596d-800">Kestrel options</span></span>

<span data-ttu-id="6596d-801">Serwer sieci Web Kestrel ma opcje konfiguracji ograniczeń, które są szczególnie przydatne w przypadku wdrożeń mających dostęp do Internetu.</span><span class="sxs-lookup"><span data-stu-id="6596d-801">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="6596d-802">Ustaw ograniczenia we właściwości <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="6596d-802">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="6596d-803">Właściwość `Limits` przechowuje wystąpienie klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="6596d-803">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="6596d-804">W poniższych przykładach użyto przestrzeni nazw <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="6596d-804">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="6596d-805">Opcje Kestrel, które są konfigurowane w C# kodzie w poniższych przykładach, można również ustawić za pomocą [dostawcy konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6596d-805">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="6596d-806">Na przykład dostawca konfiguracji plików może załadować konfigurację Kestrel z pliku *appSettings. JSON* lub *appSettings. { Environment} plik JSON* :</span><span class="sxs-lookup"><span data-stu-id="6596d-806">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="6596d-807">Skorzystaj z **jednej** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="6596d-807">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="6596d-808">Skonfiguruj Kestrel w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6596d-808">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="6596d-809">Wstrzyknąć wystąpienie `IConfiguration` do klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="6596d-809">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="6596d-810">W poniższym przykładzie przyjęto założenie, że wprowadzona konfiguracja jest przypisana do właściwości `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="6596d-810">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="6596d-811">W `Startup.ConfigureServices`Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="6596d-811">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

     ```csharp
     using Microsoft.Extensions.Configuration
     
     public class Startup
     {
         public Startup(IConfiguration configuration)
         {
             Configuration = configuration;
         }

         public IConfiguration Configuration { get; }

         public void ConfigureServices(IServiceCollection services)
         {
             services.Configure<KestrelServerOptions>(
                 Configuration.GetSection("Kestrel"));
         }

         public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
         {
             ...
         }
     }
     ```

* <span data-ttu-id="6596d-812">Skonfiguruj Kestrel podczas kompilowania hosta:</span><span class="sxs-lookup"><span data-stu-id="6596d-812">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="6596d-813">W programie *program.cs*Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="6596d-813">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="6596d-814">Obie powyższe podejścia współpracują z dowolnym [dostawcą konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6596d-814">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="6596d-815">Limit czasu utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="6596d-815">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="6596d-816">Pobiera lub ustawia [limit czasu utrzymywania aktywności](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="6596d-816">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="6596d-817">Wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="6596d-817">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="6596d-818">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="6596d-818">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="6596d-819">Maksymalna liczba współbieżnych otwartych połączeń TCP można ustawić dla całej aplikacji przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="6596d-819">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="6596d-820">Istnieje oddzielny limit połączeń, które zostały uaktualnione z protokołu HTTP lub HTTPS do innego protokołu (na przykład w żądaniu usługi WebSockets).</span><span class="sxs-lookup"><span data-stu-id="6596d-820">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="6596d-821">Po uaktualnieniu połączenia nie jest ono wliczane do limitu `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="6596d-821">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="6596d-822">Maksymalna liczba połączeń jest domyślnie nieograniczona (null).</span><span class="sxs-lookup"><span data-stu-id="6596d-822">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="6596d-823">Maksymalny rozmiar treści żądania</span><span class="sxs-lookup"><span data-stu-id="6596d-823">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="6596d-824">Domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="6596d-824">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="6596d-825">Zalecanym podejściem do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie atrybutu <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> dla metody akcji:</span><span class="sxs-lookup"><span data-stu-id="6596d-825">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="6596d-826">Oto przykład, który pokazuje, jak skonfigurować ograniczenie dla aplikacji w każdym żądaniu:</span><span class="sxs-lookup"><span data-stu-id="6596d-826">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="6596d-827">Zastąp ustawienie określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="6596d-827">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="6596d-828">Wyjątek jest generowany, jeśli aplikacja skonfiguruje limit żądania po rozpoczęciu uruchamiania aplikacji w celu odczytania żądania.</span><span class="sxs-lookup"><span data-stu-id="6596d-828">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="6596d-829">Istnieje właściwość `IsReadOnly`, która wskazuje, czy właściwość `MaxRequestBodySize` jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="6596d-829">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="6596d-830">Gdy aplikacja jest uruchamiana [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model) [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru treści żądania Kestrel jest wyłączony, ponieważ program IIS już ustawia limit.</span><span class="sxs-lookup"><span data-stu-id="6596d-830">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="6596d-831">Minimalna stawka danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="6596d-831">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="6596d-832">Kestrel sprawdza co sekundę, jeśli dane są odbierane z określoną szybkością w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="6596d-832">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="6596d-833">Jeśli szybkość spadnie poniżej wartości minimalnej, upłynął limit czasu połączenia. Okres prolongaty to czas, przez który Kestrel nadaje klientowi zwiększenie jego szybkości wysyłania do minimum; Ta częstotliwość nie jest sprawdzana w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="6596d-833">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="6596d-834">Okres prolongaty pozwala uniknąć porzucania połączeń, które początkowo wysyłają dane z niską szybkością.</span><span class="sxs-lookup"><span data-stu-id="6596d-834">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="6596d-835">Domyślna stawka minimalna to 240 bajtów na sekundę z 5-sekundowym okresem prolongaty.</span><span class="sxs-lookup"><span data-stu-id="6596d-835">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="6596d-836">Minimalna stawka dotyczy także odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6596d-836">A minimum rate also applies to the response.</span></span> <span data-ttu-id="6596d-837">Kod określający limit żądań i limit odpowiedzi jest taki sam, chyba że ma `RequestBody` lub `Response` w nazwach właściwości i interfejsów.</span><span class="sxs-lookup"><span data-stu-id="6596d-837">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="6596d-838">Oto przykład pokazujący sposób konfigurowania minimalnych stawek danych w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="6596d-838">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

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

### <a name="request-headers-timeout"></a><span data-ttu-id="6596d-839">Limit czasu nagłówków żądań</span><span class="sxs-lookup"><span data-stu-id="6596d-839">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="6596d-840">Pobiera lub ustawia maksymalny czas, przez jaki serwer spędza nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="6596d-840">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="6596d-841">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="6596d-841">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="6596d-842">Synchroniczne we/wy</span><span class="sxs-lookup"><span data-stu-id="6596d-842">Synchronous IO</span></span>

<span data-ttu-id="6596d-843"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> kontroluje, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6596d-843"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="6596d-844">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="6596d-844">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="6596d-845">Duża liczba blokowania synchronicznych operacji we/wy może prowadzić do zablokowania puli wątków, co sprawia, że aplikacja nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="6596d-845">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="6596d-846">Włącz `AllowSynchronousIO` tylko w przypadku korzystania z biblioteki, która nie obsługuje asynchronicznych operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="6596d-846">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="6596d-847">Poniższy przykład wyłącza synchroniczną operację we/wy:</span><span class="sxs-lookup"><span data-stu-id="6596d-847">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="6596d-848">Aby uzyskać informacje o innych opcjach i ograniczeniach Kestrel, zobacz:</span><span class="sxs-lookup"><span data-stu-id="6596d-848">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="6596d-849">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="6596d-849">Endpoint configuration</span></span>

<span data-ttu-id="6596d-850">Domyślnie ASP.NET Core wiąże się z:</span><span class="sxs-lookup"><span data-stu-id="6596d-850">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="6596d-851">`https://localhost:5001` (gdy obecny jest lokalny certyfikat programistyczny)</span><span class="sxs-lookup"><span data-stu-id="6596d-851">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="6596d-852">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="6596d-852">Specify URLs using the:</span></span>

* <span data-ttu-id="6596d-853">Zmienna środowiskowa `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="6596d-853">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="6596d-854">`--urls` argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="6596d-854">`--urls` command-line argument.</span></span>
* <span data-ttu-id="6596d-855">klucz konfiguracji hosta `urls`.</span><span class="sxs-lookup"><span data-stu-id="6596d-855">`urls` host configuration key.</span></span>
* <span data-ttu-id="6596d-856">Metoda rozszerzenia `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6596d-856">`UseUrls` extension method.</span></span>

<span data-ttu-id="6596d-857">Wartość podana przy użyciu tych metod może być jednym lub większą liczbą punktów końcowych HTTP i HTTPS (HTTPS, jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="6596d-857">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="6596d-858">Skonfiguruj wartość jako listę rozdzieloną średnikami (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="6596d-858">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="6596d-859">Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwera](xref:fundamentals/host/web-host#server-urls) i [Zastąp konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="6596d-859">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="6596d-860">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="6596d-860">A development certificate is created:</span></span>

* <span data-ttu-id="6596d-861">Po zainstalowaniu [zestaw .NET Core SDK](/dotnet/core/sdk) .</span><span class="sxs-lookup"><span data-stu-id="6596d-861">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="6596d-862">Do utworzenia certyfikatu służy [Narzędzie dev-certs](xref:aspnetcore-2.1#https) .</span><span class="sxs-lookup"><span data-stu-id="6596d-862">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="6596d-863">Niektóre przeglądarki wymagają przyznania jawnego uprawnienia do zaufania do lokalnego certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="6596d-863">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="6596d-864">Szablony projektu konfigurują aplikacje do uruchamiania domyślnie przy użyciu protokołu HTTPS i obejmują [przekierowania https i obsługę HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="6596d-864">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="6596d-865">Wywołaj metody <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> w <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, aby skonfigurować prefiksy i porty adresów URL dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-865">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="6596d-866">`UseUrls`, `--urls` argument wiersza polecenia, klucz konfiguracji hosta `urls` oraz zmienna środowiskowa `ASPNETCORE_URLS` również działają, ale mają ograniczenia wymienione w dalszej części tej sekcji (certyfikat domyślny musi być dostępny do konfiguracji punktu końcowego HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6596d-866">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="6596d-867">Konfiguracja `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="6596d-867">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="6596d-868">ConfigureEndpointDefaults (Akcja\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="6596d-868">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="6596d-869">Określa `Action` konfiguracji do uruchomienia dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="6596d-869">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="6596d-870">Wywoływanie `ConfigureEndpointDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="6596d-870">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="6596d-871">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="6596d-871">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="6596d-872">ConfigureHttpsDefaults (Akcja\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="6596d-872">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="6596d-873">Określa `Action` konfiguracji do uruchomienia dla każdego punktu końcowego HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6596d-873">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="6596d-874">Wywoływanie `ConfigureHttpsDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="6596d-874">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="6596d-875">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="6596d-875">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="6596d-876">Konfiguruj (IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="6596d-876">Configure(IConfiguration)</span></span>

<span data-ttu-id="6596d-877">Tworzy moduł ładujący konfigurację służący do konfigurowania Kestrel, który pobiera <xref:Microsoft.Extensions.Configuration.IConfiguration> jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-877">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="6596d-878">Konfiguracja musi być objęta zakresem sekcji konfiguracji dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-878">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="6596d-879">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="6596d-879">ListenOptions.UseHttps</span></span>

<span data-ttu-id="6596d-880">Skonfiguruj Kestrel do korzystania z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6596d-880">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="6596d-881">rozszerzenia `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="6596d-881">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="6596d-882">`UseHttps` &ndash; skonfigurować Kestrel do używania protokołu HTTPS z certyfikatem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="6596d-882">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="6596d-883">Zgłasza wyjątek, jeśli nie został skonfigurowany żaden certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="6596d-883">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="6596d-884">`ListenOptions.UseHttps` parametry:</span><span class="sxs-lookup"><span data-stu-id="6596d-884">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="6596d-885">`filename` to ścieżka i nazwa pliku certyfikatu odnosząca się do katalogu zawierającego pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6596d-885">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="6596d-886">`password` to hasło wymagane do uzyskania dostępu do danych certyfikatu X. 509.</span><span class="sxs-lookup"><span data-stu-id="6596d-886">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="6596d-887">`configureOptions` jest `Action`, aby skonfigurować `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="6596d-887">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="6596d-888">Zwraca `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="6596d-888">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="6596d-889">`storeName` to magazyn certyfikatów, z którego ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="6596d-889">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="6596d-890">`subject` to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="6596d-890">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="6596d-891">`allowInvalid` wskazuje, czy należy wziąć pod uwagę nieprawidłowe certyfikaty, takie jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="6596d-891">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="6596d-892">`location` to lokalizacja magazynu, z której ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="6596d-892">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="6596d-893">`serverCertificate` to certyfikat X. 509.</span><span class="sxs-lookup"><span data-stu-id="6596d-893">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="6596d-894">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6596d-894">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="6596d-895">Należy podać co najmniej certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="6596d-895">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="6596d-896">Obsługiwane konfiguracje opisane dalej:</span><span class="sxs-lookup"><span data-stu-id="6596d-896">Supported configurations described next:</span></span>

* <span data-ttu-id="6596d-897">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="6596d-897">No configuration</span></span>
* <span data-ttu-id="6596d-898">Zastąp domyślny certyfikat z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="6596d-898">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="6596d-899">Zmień wartości domyślne w kodzie</span><span class="sxs-lookup"><span data-stu-id="6596d-899">Change the defaults in code</span></span>

<span data-ttu-id="6596d-900">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="6596d-900">*No configuration*</span></span>

<span data-ttu-id="6596d-901">Kestrel nasłuchuje na `http://localhost:5000` i `https://localhost:5001` (jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="6596d-901">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="6596d-902">*Zastąp domyślny certyfikat z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="6596d-902">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="6596d-903">`CreateDefaultBuilder` wywołania `Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-903">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="6596d-904">Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępny dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-904">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="6596d-905">Konfigurowanie wielu punktów końcowych, w tym adresów URL i certyfikatów do użycia, z pliku znajdującego się na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="6596d-905">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="6596d-906">W poniższym przykładzie pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="6596d-906">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="6596d-907">Ustaw **AllowInvalid** na `true`, aby zezwolić na korzystanie z nieprawidłowych certyfikatów (na przykład certyfikatów z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="6596d-907">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="6596d-908">Wszystkie punkty końcowe HTTPS, które nie określają certyfikatu (**HttpsDefaultCert** w poniższym przykładzie) powracają do certyfikatu zdefiniowanego w obszarze **Certyfikaty** > **domyślnym** lub certyfikatem deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="6596d-908">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="6596d-909">Alternatywą dla korzystania z **ścieżki** i **hasła** dla dowolnego węzła certyfikatu jest określenie certyfikatu przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="6596d-909">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="6596d-910">Na przykład **certyfikaty** > **domyślnego** certyfikatu można określić jako:</span><span class="sxs-lookup"><span data-stu-id="6596d-910">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="6596d-911">Uwagi dotyczące schematu:</span><span class="sxs-lookup"><span data-stu-id="6596d-911">Schema notes:</span></span>

* <span data-ttu-id="6596d-912">W nazwach punktów końcowych nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="6596d-912">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="6596d-913">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-913">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="6596d-914">Dla każdego punktu końcowego jest wymagany parametr `Url`.</span><span class="sxs-lookup"><span data-stu-id="6596d-914">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="6596d-915">Format tego parametru jest taki sam jak parametr konfiguracji `Urls` najwyższego poziomu, z tą różnicą, że jest ograniczony do pojedynczej wartości.</span><span class="sxs-lookup"><span data-stu-id="6596d-915">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="6596d-916">Te punkty końcowe zastępują te zdefiniowane w konfiguracji `Urls` najwyższego poziomu zamiast dodawać do nich.</span><span class="sxs-lookup"><span data-stu-id="6596d-916">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="6596d-917">Punkty końcowe zdefiniowane w kodzie za pośrednictwem `Listen` kumulują się z punktami końcowymi zdefiniowanymi w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="6596d-917">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="6596d-918">Sekcja `Certificate` jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="6596d-918">The `Certificate` section is optional.</span></span> <span data-ttu-id="6596d-919">Jeśli sekcja `Certificate` nie zostanie określona, są używane wartości domyślne zdefiniowane we wcześniejszych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="6596d-919">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="6596d-920">Jeśli żadne wartości domyślne nie są dostępne, serwer zgłasza wyjątek i nie może się uruchomić.</span><span class="sxs-lookup"><span data-stu-id="6596d-920">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="6596d-921">Sekcja `Certificate` obsługuje zarówno **ścieżkę**&ndash;**hasło** , jak i **temat**&ndash;certyfikatów **magazynu** .</span><span class="sxs-lookup"><span data-stu-id="6596d-921">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="6596d-922">W ten sposób można zdefiniować dowolną liczbę punktów końcowych, dopóki nie spowoduje to konfliktów portów.</span><span class="sxs-lookup"><span data-stu-id="6596d-922">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="6596d-923">`options.Configure(context.Configuration.GetSection("{SECTION}"))` zwraca `KestrelConfigurationLoader` z metodą `.Endpoint(string name, listenOptions => { })`, której można użyć do uzupełnienia ustawień skonfigurowanego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="6596d-923">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="6596d-924">dostęp do `KestrelServerOptions.ConfigurationLoader` można uzyskać, aby kontynuować iterację istniejącego modułu ładującego, takiego jak dostarczony przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6596d-924">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="6596d-925">Sekcja konfiguracji dla każdego punktu końcowego jest dostępna w opcjach w `Endpoint` metodzie, aby można było odczytać ustawienia niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-925">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="6596d-926">Można załadować wiele konfiguracji, wywołując `options.Configure(context.Configuration.GetSection("{SECTION}"))` ponownie z inną sekcją.</span><span class="sxs-lookup"><span data-stu-id="6596d-926">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="6596d-927">Używana jest tylko Ostatnia konfiguracja, chyba że `Load` jest jawnie wywoływana w poprzednich wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="6596d-927">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="6596d-928">Pakiet nie wywołuje `Load`, tak aby jego sekcja konfiguracji domyślnej mogła zostać zamieniona.</span><span class="sxs-lookup"><span data-stu-id="6596d-928">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="6596d-929">`KestrelConfigurationLoader` odzwierciedla `Listen`ą rodzinę interfejsów API z `KestrelServerOptions` jako przeciążania `Endpoint`, dlatego punkty końcowe kodu i konfiguracji można skonfigurować w tym samym miejscu.</span><span class="sxs-lookup"><span data-stu-id="6596d-929">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="6596d-930">Te przeciążenia nie używają nazw i używają tylko ustawień domyślnych z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="6596d-930">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="6596d-931">*Zmień wartości domyślne w kodzie*</span><span class="sxs-lookup"><span data-stu-id="6596d-931">*Change the defaults in code*</span></span>

<span data-ttu-id="6596d-932">`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` mogą służyć do zmiany domyślnych ustawień dla `ListenOptions` i `HttpsConnectionAdapterOptions`, w tym przesłanianie domyślnego certyfikatu określonego w poprzednim scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="6596d-932">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="6596d-933">przed skonfigurowaniem punktów końcowych należy wywołać `ConfigureEndpointDefaults` i `ConfigureHttpsDefaults`.</span><span class="sxs-lookup"><span data-stu-id="6596d-933">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="6596d-934">*Obsługa usługi Kestrel dla SNI*</span><span class="sxs-lookup"><span data-stu-id="6596d-934">*Kestrel support for SNI*</span></span>

<span data-ttu-id="6596d-935">[Oznaczanie nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do hostowania wielu domen na tym samym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="6596d-935">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="6596d-936">Aby SNI działał, klient wysyła nazwę hosta dla bezpiecznej sesji do serwera podczas uzgadniania TLS, aby serwer mógł zapewnić prawidłowy certyfikat.</span><span class="sxs-lookup"><span data-stu-id="6596d-936">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="6596d-937">Klient korzysta z dostarczonego certyfikatu w celu zaszyfrowania komunikacji z serwerem podczas bezpiecznej sesji, która następuje po uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="6596d-937">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="6596d-938">Kestrel obsługuje SNI za pośrednictwem wywołania zwrotnego `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="6596d-938">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="6596d-939">Wywołanie zwrotne jest wywoływane jednokrotnie dla każdego połączenia, aby umożliwić aplikacji inspekcję nazwy hosta i wybieranie odpowiedniego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="6596d-939">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="6596d-940">Obsługa SNI wymaga:</span><span class="sxs-lookup"><span data-stu-id="6596d-940">SNI support requires:</span></span>

* <span data-ttu-id="6596d-941">Uruchomione na platformie docelowej `netcoreapp2.1` lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="6596d-941">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="6596d-942">W `net461` lub później wywołanie zwrotne jest wywoływane, ale `name` jest zawsze `null`.</span><span class="sxs-lookup"><span data-stu-id="6596d-942">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="6596d-943">`name` jest również `null`, jeśli klient nie poda parametru nazwy hosta w uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="6596d-943">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="6596d-944">Wszystkie witryny sieci Web działają na tym samym wystąpieniu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-944">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="6596d-945">Kestrel nie obsługuje udostępniania adresu IP i portu w wielu wystąpieniach bez zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="6596d-945">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="6596d-946">Rejestrowanie połączeń</span><span class="sxs-lookup"><span data-stu-id="6596d-946">Connection logging</span></span>

<span data-ttu-id="6596d-947">Wywołaj <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>, aby emitować dzienniki poziomu debugowania dla komunikacji na poziomie bajtów w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="6596d-947">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="6596d-948">Rejestrowanie połączeń ułatwia rozwiązywanie problemów z komunikacją na niskim poziomie, na przykład podczas szyfrowania TLS i za serwerami proxy.</span><span class="sxs-lookup"><span data-stu-id="6596d-948">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="6596d-949">Jeśli `UseConnectionLogging` jest umieszczony przed `UseHttps`, szyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="6596d-949">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="6596d-950">Jeśli `UseConnectionLogging` jest umieszczony po `UseHttps`, odszyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="6596d-950">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="6596d-951">Powiąż z gniazdem TCP</span><span class="sxs-lookup"><span data-stu-id="6596d-951">Bind to a TCP socket</span></span>

<span data-ttu-id="6596d-952">Metoda <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> wiąże się z gniazdem TCP, a opcja lambda zezwala na konfigurację certyfikatu X. 509:</span><span class="sxs-lookup"><span data-stu-id="6596d-952">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

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

<span data-ttu-id="6596d-953">Przykład konfiguruje HTTPS dla punktu końcowego z <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="6596d-953">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="6596d-954">Użyj tego samego interfejsu API, aby skonfigurować inne ustawienia Kestrel dla określonych punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="6596d-954">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="6596d-955">Powiąż z gniazdem systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="6596d-955">Bind to a Unix socket</span></span>

<span data-ttu-id="6596d-956">Nasłuchiwanie w gnieździe systemu UNIX za pomocą <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, aby zwiększyć wydajność za pomocą Nginx, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="6596d-956">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

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

* <span data-ttu-id="6596d-957">W pliku Nginx confiuguration Ustaw `proxy_pass` `server` > `location` > `http://unix:/tmp/{KESTREL SOCKET}:/;`.</span><span class="sxs-lookup"><span data-stu-id="6596d-957">In the Nginx confiuguration file, set the `server` > `location` > `proxy_pass` entry to `http://unix:/tmp/{KESTREL SOCKET}:/;`.</span></span> <span data-ttu-id="6596d-958">`{KESTREL SOCKET}` to nazwa gniazda dostarczanego do <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> (na przykład `kestrel-test.sock` w powyższym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="6596d-958">`{KESTREL SOCKET}` is the name of the socket provided to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> (for example, `kestrel-test.sock` in the preceding example).</span></span>
* <span data-ttu-id="6596d-959">Upewnij się, że gniazdo jest zapisywalne przez Nginx (na przykład `chmod go+w /tmp/kestrel-test.sock`).</span><span class="sxs-lookup"><span data-stu-id="6596d-959">Ensure that the socket is writeable by Nginx (for example, `chmod go+w /tmp/kestrel-test.sock`).</span></span> 

### <a name="port-0"></a><span data-ttu-id="6596d-960">Port 0</span><span class="sxs-lookup"><span data-stu-id="6596d-960">Port 0</span></span>

<span data-ttu-id="6596d-961">Gdy `0` jest określony numer portu, Kestrel dynamicznie wiąże się z dostępnym portem.</span><span class="sxs-lookup"><span data-stu-id="6596d-961">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="6596d-962">W poniższym przykładzie pokazano, jak określić, który port Kestrel faktycznie powiązany w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="6596d-962">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="6596d-963">Po uruchomieniu aplikacji dane wyjściowe okna konsoli wskazują port dynamiczny, w którym można uzyskać dostęp do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="6596d-963">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="6596d-964">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="6596d-964">Limitations</span></span>

<span data-ttu-id="6596d-965">Skonfiguruj punkty końcowe przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="6596d-965">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="6596d-966">`--urls` argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="6596d-966">`--urls` command-line argument</span></span>
* <span data-ttu-id="6596d-967">klucz konfiguracji hosta `urls`</span><span class="sxs-lookup"><span data-stu-id="6596d-967">`urls` host configuration key</span></span>
* <span data-ttu-id="6596d-968">Zmienna środowiskowa `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="6596d-968">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="6596d-969">Te metody są przydatne do tworzenia kodu w pracy z serwerami innymi niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6596d-969">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="6596d-970">Należy jednak pamiętać o następujących ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="6596d-970">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="6596d-971">Nie można użyć protokołu HTTPS z tymi metodami, chyba że w konfiguracji punktu końcowego HTTPS jest podany certyfikat domyślny (na przykład za pomocą konfiguracji `KestrelServerOptions` lub pliku konfiguracji, jak pokazano wcześniej w tym temacie).</span><span class="sxs-lookup"><span data-stu-id="6596d-971">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="6596d-972">Gdy oba podejścia `Listen` i `UseUrls` są używane jednocześnie, `Listen` punkty końcowe przesłaniają `UseUrls` punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-972">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="6596d-973">Konfiguracja punktu końcowego usług IIS</span><span class="sxs-lookup"><span data-stu-id="6596d-973">IIS endpoint configuration</span></span>

<span data-ttu-id="6596d-974">W przypadku korzystania z usług IIS powiązania URL dla powiązań przesłonięć usług IIS są ustawiane przez `Listen` lub `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6596d-974">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="6596d-975">Aby uzyskać więcej informacji, zobacz temat [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="6596d-975">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="6596d-976">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="6596d-976">Transport configuration</span></span>

<span data-ttu-id="6596d-977">W wersji platformy ASP.NET Core 2.1 transport domyślny serwera Kestrel nie jest już oparty na bibliotece libuv, ale na zarządzanych gniazdach.</span><span class="sxs-lookup"><span data-stu-id="6596d-977">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="6596d-978">Jest to istotna zmiana dla aplikacji ASP.NET Core 2,0 uaktualniana do 2,1, które wywołują <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> i zależą od jednego z następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="6596d-978">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="6596d-979">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (bezpośrednie odwołanie do pakietu)</span><span class="sxs-lookup"><span data-stu-id="6596d-979">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="6596d-980">Microsoft. AspNetCore. App</span><span class="sxs-lookup"><span data-stu-id="6596d-980">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="6596d-981">Dla projektów, które wymagają użycia Libuv:</span><span class="sxs-lookup"><span data-stu-id="6596d-981">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="6596d-982">Dodaj zależność dla pakietu [Microsoft. AspNetCore. Server. Kestrel. transport. Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="6596d-982">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="6596d-983"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>wywołania:</span><span class="sxs-lookup"><span data-stu-id="6596d-983">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="6596d-984">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="6596d-984">URL prefixes</span></span>

<span data-ttu-id="6596d-985">W przypadku korzystania z `UseUrls`, `--urls` argumentu wiersza polecenia, klucza konfiguracji hosta `urls` lub zmiennej środowiskowej `ASPNETCORE_URLS`, prefiksy adresów URL mogą mieć jeden z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="6596d-985">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="6596d-986">Tylko prefiksy adresów URL HTTP są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="6596d-986">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="6596d-987">Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązań adresów URL przy użyciu `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6596d-987">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="6596d-988">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="6596d-988">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="6596d-989">`0.0.0.0` jest szczególnym przypadkiem, który wiąże się ze wszystkimi adresami IPv4.</span><span class="sxs-lookup"><span data-stu-id="6596d-989">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="6596d-990">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="6596d-990">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="6596d-991">`[::]` to odpowiednik IPv6 `0.0.0.0`IPv4.</span><span class="sxs-lookup"><span data-stu-id="6596d-991">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="6596d-992">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="6596d-992">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="6596d-993">Nazwy hostów, `*`i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="6596d-993">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="6596d-994">Wszystkie nie są rozpoznawane jako prawidłowy adres IP lub `localhost` są powiązane z protokołem IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="6596d-994">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="6596d-995">Aby powiązać różne nazwy hostów z różnymi ASP.NET Core aplikacjami na tym samym porcie, użyj [protokołu HTTP. sys](xref:fundamentals/servers/httpsys) lub odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="6596d-995">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="6596d-996">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="6596d-996">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="6596d-997">Nazwa hosta `localhost` z numerem portu lub adresem IP sprzężenia zwrotnego z numerem portu</span><span class="sxs-lookup"><span data-stu-id="6596d-997">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="6596d-998">Gdy `localhost` jest określony, Kestrel próbuje powiązać z interfejsami sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="6596d-998">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="6596d-999">Jeśli żądany port jest używany przez inną usługę w dowolnym interfejsie sprzężenia zwrotnego, uruchomienie Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="6596d-999">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="6596d-1000">Jeśli którykolwiek z innych przyczyn interfejsu sprzężenia zwrotnego jest niedostępny (najczęściej jest to spowodowane tym, że protokół IPv6 nie jest obsługiwany), Kestrel rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="6596d-1000">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="6596d-1001">Filtrowanie hostów</span><span class="sxs-lookup"><span data-stu-id="6596d-1001">Host filtering</span></span>

<span data-ttu-id="6596d-1002">Chociaż usługa Kestrel obsługuje konfigurację na podstawie prefiksów, takich jak `http://example.com:5000`, Kestrel w znacznym stopniu ignoruje nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="6596d-1002">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="6596d-1003">`localhost` hosta jest specjalnym przypadkiem używanym do tworzenia powiązań z adresami sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="6596d-1003">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="6596d-1004">Każdy host poza jawnym adresem IP tworzy powiązanie ze wszystkimi publicznymi adresami IP.</span><span class="sxs-lookup"><span data-stu-id="6596d-1004">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="6596d-1005">nagłówki `Host` nie są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="6596d-1005">`Host` headers aren't validated.</span></span>

<span data-ttu-id="6596d-1006">Aby obejść ten sposób, użyj oprogramowania pośredniczącego filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="6596d-1006">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="6596d-1007">Oprogramowanie pośredniczące do filtrowania hosta jest dostarczane przez pakiet [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , który jest zawarty w pakiecie [Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub 2,2).</span><span class="sxs-lookup"><span data-stu-id="6596d-1007">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="6596d-1008">Oprogramowanie pośredniczące jest dodawane przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="6596d-1008">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="6596d-1009">Programowe filtrowanie hosta jest domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="6596d-1009">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="6596d-1010">Aby włączyć oprogramowanie pośredniczące, zdefiniuj klucz `AllowedHosts` w pliku *appSettings. json*/*appSettings.\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="6596d-1010">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="6596d-1011">Wartość to rozdzielana średnikami lista nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="6596d-1011">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="6596d-1012">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="6596d-1012">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="6596d-1013">W przypadku [przekierowanych nagłówków oprogramowanie pośredniczące](xref:host-and-deploy/proxy-load-balancer) ma również opcję <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="6596d-1013">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="6596d-1014">Przekazane nagłówki — oprogramowanie pośredniczące i filtrowanie hostów oprogramowanie pośredniczące ma podobną funkcjonalność dla różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="6596d-1014">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="6596d-1015">Ustawienie `AllowedHosts` z przekierowanymi nagłówkami — oprogramowanie pośredniczące jest odpowiednie, gdy nagłówek `Host` nie jest zachowywany podczas przekazywania żądań z odwrotnym serwerem proxy lub modułem równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="6596d-1015">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="6596d-1016">Ustawienie `AllowedHosts` przy użyciu oprogramowania pośredniczącego filtrowania hosta jest odpowiednie, gdy Kestrel jest używany jako publiczny serwer graniczny lub gdy `Host` nagłówek jest bezpośrednio przekazywany.</span><span class="sxs-lookup"><span data-stu-id="6596d-1016">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="6596d-1017">Aby uzyskać więcej informacji na temat przekierowanych nagłówków, należy zapoznać się z tematem <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="6596d-1017">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6596d-1018">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6596d-1018">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="6596d-1019">RFC 7230: Składnia i Routing komunikatu (sekcja 5,4: Host)</span><span class="sxs-lookup"><span data-stu-id="6596d-1019">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
