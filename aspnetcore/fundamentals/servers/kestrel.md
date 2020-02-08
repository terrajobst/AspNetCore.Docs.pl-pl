---
title: Implementacja serwera sieci Web Kestrel w ASP.NET Core
author: guardrex
description: Dowiedz się więcej na temat Kestrel, międzyplatformowego serwera sieci Web dla ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/06/2020
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 0c5d16b1901a8a8e5ae1914e5eaa86f71fa3a90b
ms.sourcegitcommit: 80286715afb93c4d13c931b008016d6086c0312b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074539"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="204ab-103">Implementacja serwera sieci Web Kestrel w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="204ab-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="204ab-104">Autorzy [Dykstra](https://github.com/tdykstra), [Krzysztof Ross](https://github.com/Tratcher), [Stephen](https://twitter.com/halter73), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="204ab-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="204ab-105">Kestrel to Międzyplatformowy [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="204ab-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="204ab-106">Kestrel to serwer sieci Web, który jest domyślnie uwzględniony w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="204ab-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="204ab-107">Kestrel obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="204ab-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="204ab-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="204ab-108">HTTPS</span></span>
* <span data-ttu-id="204ab-109">Nieprzezroczyste uaktualnienie używane do włączania obsługi obiektów [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="204ab-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="204ab-110">Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx</span><span class="sxs-lookup"><span data-stu-id="204ab-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="204ab-111">HTTP/2 (z wyjątkiem macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="204ab-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="204ab-112">&dagger;HTTP/2 będą obsługiwane w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="204ab-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="204ab-113">Kestrel jest obsługiwana na wszystkich platformach i wersjach obsługiwanych przez platformę .NET Core.</span><span class="sxs-lookup"><span data-stu-id="204ab-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="204ab-114">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="204ab-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="204ab-115">Obsługa protokołu HTTP/2</span><span class="sxs-lookup"><span data-stu-id="204ab-115">HTTP/2 support</span></span>

<span data-ttu-id="204ab-116">[Protokół HTTP/2](https://httpwg.org/specs/rfc7540.html) jest dostępny dla aplikacji ASP.NET Core, jeśli spełnione są następujące wymagania podstawowe:</span><span class="sxs-lookup"><span data-stu-id="204ab-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="204ab-117">&dagger; systemu operacyjnego</span><span class="sxs-lookup"><span data-stu-id="204ab-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="204ab-118">Windows Server 2016/Windows 10 lub nowszy&Dagger;</span><span class="sxs-lookup"><span data-stu-id="204ab-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="204ab-119">Linux z OpenSSL 1.0.2 lub nowszym (na przykład Ubuntu 16,04 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="204ab-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="204ab-120">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="204ab-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="204ab-121">Połączenie [negocjowania protokołu warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="204ab-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="204ab-122">Protokół TLS 1.2 lub nowszej połączenia</span><span class="sxs-lookup"><span data-stu-id="204ab-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="204ab-123">&dagger;HTTP/2 będą obsługiwane w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="204ab-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="204ab-124">&Dagger;Kestrel ma ograniczoną obsługę protokołu HTTP/2 w systemie Windows Server 2012 R2 i Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="204ab-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="204ab-125">Obsługa jest ograniczona, ponieważ lista obsługiwanych mechanizmów szyfrowania TLS dostępnych w tych systemach operacyjnych jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="204ab-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="204ab-126">Do zabezpieczenia połączeń TLS może być wymagany certyfikat wygenerowany przy użyciu algorytmu podpisu cyfrowego (ECDSA) krzywej eliptycznej.</span><span class="sxs-lookup"><span data-stu-id="204ab-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="204ab-127">W przypadku ustanowienia połączenia HTTP/2 raporty [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="204ab-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="204ab-128">Protokół HTTP/2 jest domyślnie wyłączony.</span><span class="sxs-lookup"><span data-stu-id="204ab-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="204ab-129">Więcej informacji na temat konfiguracji można znaleźć w sekcjach [Kestrel Options](#kestrel-options) and [ListenOptions. Protocols](#listenoptionsprotocols) .</span><span class="sxs-lookup"><span data-stu-id="204ab-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="204ab-130">Kiedy używać Kestrel z zwrotnym serwerem proxy</span><span class="sxs-lookup"><span data-stu-id="204ab-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="204ab-131">Kestrel może być używana przez siebie lub z *odwrotnym serwerem proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org)lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="204ab-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="204ab-132">Odwrotny serwer proxy odbiera żądania HTTP z sieci i przekazuje je do usługi Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="204ab-133">Kestrel używany jako serwer sieci Web z krawędzią (dostępną z Internetu):</span><span class="sxs-lookup"><span data-stu-id="204ab-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem bez serwera proxy zwrotnego](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="204ab-135">Kestrel używany w konfiguracji zwrotnego serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="204ab-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel komunikuje się pośrednio z Internetem za pomocą odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="204ab-137">Konfiguracja, z serwerem proxy odwrotnych lub bez niego, jest obsługiwaną konfiguracją hostingu.</span><span class="sxs-lookup"><span data-stu-id="204ab-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="204ab-138">Kestrel używany jako serwer graniczny bez serwera proxy odwrotnego nie obsługuje udostępniania tego samego adresu IP i portu między wieloma procesami.</span><span class="sxs-lookup"><span data-stu-id="204ab-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="204ab-139">Gdy Kestrel jest skonfigurowany do nasłuchiwania na porcie, Kestrel obsługuje cały ruch dla tego portu bez względu na żądania `Host` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="204ab-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="204ab-140">Zwrotny serwer proxy, który może udostępniać porty, ma możliwość przekazywania żądań do Kestrel na unikatowym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="204ab-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="204ab-141">Nawet jeśli odwrotny serwer proxy nie jest wymagany, może to być dobry wybór przy użyciu odwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="204ab-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="204ab-142">Zwrotny serwer proxy:</span><span class="sxs-lookup"><span data-stu-id="204ab-142">A reverse proxy:</span></span>

* <span data-ttu-id="204ab-143">Może ograniczać uwidoczniony obszar publicznej powierzchni aplikacji, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="204ab-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="204ab-144">Zapewnienie dodatkowej warstwy konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="204ab-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="204ab-145">Integracja z istniejącą infrastrukturą może być lepsza.</span><span class="sxs-lookup"><span data-stu-id="204ab-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="204ab-146">Uprość Równoważenie obciążenia i konfigurację komunikacji zabezpieczonej (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="204ab-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="204ab-147">Tylko serwer zwrotny proxy wymaga certyfikatu X. 509, a serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="204ab-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="204ab-148">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="204ab-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="kestrel-in-aspnet-core-apps"></a><span data-ttu-id="204ab-149">Kestrel w aplikacjach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="204ab-149">Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="204ab-150">Szablony projektów ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="204ab-151">W *program.cs*Metoda <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> wywołuje <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span><span class="sxs-lookup"><span data-stu-id="204ab-151">In *Program.cs*, the <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> method calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=8)]

<span data-ttu-id="204ab-152">Aby uzyskać więcej informacji na temat tworzenia hosta, zobacz sekcję *Konfigurowanie ustawień hosta* i *domyślnego konstruktora* dla <xref:fundamentals/host/generic-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="204ab-152">For more information on building the host, see the *Set up a host* and *Default builder settings* sections of <xref:fundamentals/host/generic-host#set-up-a-host>.</span></span>

<span data-ttu-id="204ab-153">Aby zapewnić dodatkową konfigurację po wywołaniu `ConfigureWebHostDefaults`, użyj `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="204ab-153">To provide additional configuration after calling `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="204ab-154">Opcje Kestrel</span><span class="sxs-lookup"><span data-stu-id="204ab-154">Kestrel options</span></span>

<span data-ttu-id="204ab-155">Serwer sieci Web Kestrel ma opcje konfiguracji ograniczeń, które są szczególnie przydatne w przypadku wdrożeń mających dostęp do Internetu.</span><span class="sxs-lookup"><span data-stu-id="204ab-155">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="204ab-156">Ustaw ograniczenia we właściwości <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="204ab-156">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="204ab-157">Właściwość `Limits` przechowuje wystąpienie klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="204ab-157">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="204ab-158">W poniższych przykładach użyto przestrzeni nazw <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="204ab-158">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="204ab-159">W przykładach przedstawionych w dalszej części tego artykułu opcje Kestrel C# są konfigurowane w kodzie.</span><span class="sxs-lookup"><span data-stu-id="204ab-159">In examples shown later in this article, Kestrel options are configured in C# code.</span></span> <span data-ttu-id="204ab-160">Opcje Kestrel można również ustawić za pomocą [dostawcy konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="204ab-160">Kestrel options can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="204ab-161">Na przykład [dostawca konfiguracji plików](xref:fundamentals/configuration/index#file-configuration-provider) może załadować konfigurację Kestrel z pliku *appSettings. JSON* lub *appSettings. { Environment} plik JSON* :</span><span class="sxs-lookup"><span data-stu-id="204ab-161">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider) can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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
> <span data-ttu-id="204ab-162">Konfiguracja <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> i [punktu końcowego](#endpoint-configuration) można konfigurować z poziomu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="204ab-162"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> and [endpoint configuration](#endpoint-configuration) are configurable from configuration providers.</span></span> <span data-ttu-id="204ab-163">Pozostała konfiguracja Kestrel musi być C# skonfigurowana w kodzie.</span><span class="sxs-lookup"><span data-stu-id="204ab-163">Remaining Kestrel configuration must be configured in C# code.</span></span>

<span data-ttu-id="204ab-164">Skorzystaj z **jednej** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="204ab-164">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="204ab-165">Skonfiguruj Kestrel w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="204ab-165">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="204ab-166">Wstrzyknąć wystąpienie `IConfiguration` do klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="204ab-166">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="204ab-167">W poniższym przykładzie przyjęto założenie, że wprowadzona konfiguracja jest przypisana do właściwości `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="204ab-167">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="204ab-168">W `Startup.ConfigureServices`Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="204ab-168">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

* <span data-ttu-id="204ab-169">Skonfiguruj Kestrel podczas kompilowania hosta:</span><span class="sxs-lookup"><span data-stu-id="204ab-169">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="204ab-170">W programie *program.cs*Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="204ab-170">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="204ab-171">Obie powyższe podejścia współpracują z dowolnym [dostawcą konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="204ab-171">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="204ab-172">Limit czasu utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="204ab-172">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="204ab-173">Pobiera lub ustawia [limit czasu utrzymywania aktywności](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="204ab-173">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="204ab-174">Wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="204ab-174">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="204ab-175">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="204ab-175">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="204ab-176">Maksymalna liczba współbieżnych otwartych połączeń TCP można ustawić dla całej aplikacji przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="204ab-176">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="204ab-177">Istnieje oddzielny limit połączeń, które zostały uaktualnione z protokołu HTTP lub HTTPS do innego protokołu (na przykład w żądaniu usługi WebSockets).</span><span class="sxs-lookup"><span data-stu-id="204ab-177">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="204ab-178">Po uaktualnieniu połączenia nie jest ono wliczane do limitu `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="204ab-178">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="204ab-179">Maksymalna liczba połączeń jest domyślnie nieograniczona (null).</span><span class="sxs-lookup"><span data-stu-id="204ab-179">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="204ab-180">Maksymalny rozmiar treści żądania</span><span class="sxs-lookup"><span data-stu-id="204ab-180">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="204ab-181">Domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="204ab-181">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="204ab-182">Zalecanym podejściem do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie atrybutu <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> dla metody akcji:</span><span class="sxs-lookup"><span data-stu-id="204ab-182">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="204ab-183">Oto przykład, który pokazuje, jak skonfigurować ograniczenie dla aplikacji w każdym żądaniu:</span><span class="sxs-lookup"><span data-stu-id="204ab-183">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="204ab-184">Zastąp ustawienie określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="204ab-184">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="204ab-185">Wyjątek jest generowany, jeśli aplikacja skonfiguruje limit żądania po rozpoczęciu uruchamiania aplikacji w celu odczytania żądania.</span><span class="sxs-lookup"><span data-stu-id="204ab-185">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="204ab-186">Istnieje właściwość `IsReadOnly`, która wskazuje, czy właściwość `MaxRequestBodySize` jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="204ab-186">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="204ab-187">Gdy aplikacja jest uruchamiana [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model) [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru treści żądania Kestrel jest wyłączony, ponieważ program IIS już ustawia limit.</span><span class="sxs-lookup"><span data-stu-id="204ab-187">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="204ab-188">Minimalna stawka danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="204ab-188">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="204ab-189">Kestrel sprawdza co sekundę, jeśli dane są odbierane z określoną szybkością w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="204ab-189">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="204ab-190">Jeśli szybkość spadnie poniżej wartości minimalnej, upłynął limit czasu połączenia. Okres prolongaty to czas, przez który Kestrel nadaje klientowi zwiększenie jego szybkości wysyłania do minimum; Ta częstotliwość nie jest sprawdzana w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="204ab-190">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="204ab-191">Okres prolongaty pozwala uniknąć porzucania połączeń, które początkowo wysyłają dane z niską szybkością.</span><span class="sxs-lookup"><span data-stu-id="204ab-191">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="204ab-192">Domyślna stawka minimalna to 240 bajtów na sekundę z 5-sekundowym okresem prolongaty.</span><span class="sxs-lookup"><span data-stu-id="204ab-192">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="204ab-193">Minimalna stawka dotyczy także odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="204ab-193">A minimum rate also applies to the response.</span></span> <span data-ttu-id="204ab-194">Kod określający limit żądań i limit odpowiedzi jest taki sam, chyba że ma `RequestBody` lub `Response` w nazwach właściwości i interfejsów.</span><span class="sxs-lookup"><span data-stu-id="204ab-194">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="204ab-195">Oto przykład pokazujący sposób konfigurowania minimalnych stawek danych w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="204ab-195">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="204ab-196">Przesłoń minimalne limity szybkości dla żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="204ab-196">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="204ab-197"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature>, do których odwołuje się Poprzednia próbka nie występuje w `HttpContext.Features` dla żądań HTTP/2, ponieważ Modyfikowanie limitów szybkości dla poszczególnych żądań jest ogólnie nieobsługiwane w przypadku protokołu HTTP/2 z powodu obsługi żądania multipleksowania żądań.</span><span class="sxs-lookup"><span data-stu-id="204ab-197">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="204ab-198">Jednak <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> nadal obecne `HttpContext.Features` dla żądań HTTP/2, ponieważ limit liczby odczytów nadal można *wyłączyć całkowicie* dla poszczególnych żądań, ustawiając `IHttpMinRequestBodyDataRateFeature.MinDataRate` `null` nawet dla żądania HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="204ab-198">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="204ab-199">Próba odczytania `IHttpMinRequestBodyDataRateFeature.MinDataRate` lub próby ustawienia jej na wartość inną niż `null` spowoduje, że `NotSupportedException` zostanie zgłoszone żądanie HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="204ab-199">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="204ab-200">Limity szybkości dla całego serwera skonfigurowane za pomocą `KestrelServerOptions.Limits` nadal mają zastosowanie do połączeń HTTP/1. x i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="204ab-200">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="204ab-201">Limit czasu nagłówków żądań</span><span class="sxs-lookup"><span data-stu-id="204ab-201">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="204ab-202">Pobiera lub ustawia maksymalny czas, przez jaki serwer spędza nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="204ab-202">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="204ab-203">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="204ab-203">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="204ab-204">Maksymalna liczba strumieni na połączenie</span><span class="sxs-lookup"><span data-stu-id="204ab-204">Maximum streams per connection</span></span>

<span data-ttu-id="204ab-205">`Http2.MaxStreamsPerConnection` ogranicza liczbę współbieżnych strumieni żądań dla połączenia HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="204ab-205">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="204ab-206">Odrzucone strumienie są nadmiarowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-206">Excess streams are refused.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="204ab-207">Wartość domyślna to 100.</span><span class="sxs-lookup"><span data-stu-id="204ab-207">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="204ab-208">Rozmiar tabeli nagłówka</span><span class="sxs-lookup"><span data-stu-id="204ab-208">Header table size</span></span>

<span data-ttu-id="204ab-209">Dekoder HPACK dekompresuje nagłówki HTTP dla połączeń HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="204ab-209">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="204ab-210">`Http2.HeaderTableSize` ogranicza rozmiar tabeli kompresji nagłówka używanej przez dekoder HPACK.</span><span class="sxs-lookup"><span data-stu-id="204ab-210">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="204ab-211">Wartość jest podawana w oktetach i musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="204ab-211">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="204ab-212">Wartość domyślna to 4096.</span><span class="sxs-lookup"><span data-stu-id="204ab-212">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="204ab-213">Maksymalny rozmiar ramki</span><span class="sxs-lookup"><span data-stu-id="204ab-213">Maximum frame size</span></span>

<span data-ttu-id="204ab-214">`Http2.MaxFrameSize` wskazuje maksymalny dozwolony rozmiar ładunku ramki połączenia HTTP/2 otrzymanego lub wysyłanego przez serwer.</span><span class="sxs-lookup"><span data-stu-id="204ab-214">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="204ab-215">Wartość jest podawana w oktetach i musi należeć do przedziału od 2 ^ 14 (16 384) do 2 ^ 24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="204ab-215">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="204ab-216">Wartość domyślna to 2 ^ 14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="204ab-216">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="204ab-217">Maksymalny rozmiar nagłówka żądania</span><span class="sxs-lookup"><span data-stu-id="204ab-217">Maximum request header size</span></span>

<span data-ttu-id="204ab-218">`Http2.MaxRequestHeaderFieldSize` wskazuje maksymalny dozwolony rozmiar w oktetach wartości nagłówka żądania.</span><span class="sxs-lookup"><span data-stu-id="204ab-218">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="204ab-219">Ten limit dotyczy zarówno nazwy, jak i wartości w skompresowanych i nieskompresowanych reprezentacji.</span><span class="sxs-lookup"><span data-stu-id="204ab-219">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="204ab-220">Wartość musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="204ab-220">The value must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="204ab-221">Wartość domyślna to 8 192.</span><span class="sxs-lookup"><span data-stu-id="204ab-221">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="204ab-222">Początkowy rozmiar okna połączenia</span><span class="sxs-lookup"><span data-stu-id="204ab-222">Initial connection window size</span></span>

<span data-ttu-id="204ab-223">`Http2.InitialConnectionWindowSize` wskazuje maksymalne dane dotyczące treści żądania w bajtach, które bufory serwera w tym samym czasie są agregowane dla wszystkich żądań (strumieni) na połączenie.</span><span class="sxs-lookup"><span data-stu-id="204ab-223">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="204ab-224">Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="204ab-224">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="204ab-225">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="204ab-225">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="204ab-226">Wartość domyślna to 128 KB (131 072).</span><span class="sxs-lookup"><span data-stu-id="204ab-226">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="204ab-227">Rozmiar początkowego okna strumienia</span><span class="sxs-lookup"><span data-stu-id="204ab-227">Initial stream window size</span></span>

<span data-ttu-id="204ab-228">`Http2.InitialStreamWindowSize` wskazuje maksymalne dane dotyczące treści żądania w bajtach bufory serwerów w jednym momencie na żądanie (Stream).</span><span class="sxs-lookup"><span data-stu-id="204ab-228">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="204ab-229">Żądania są również ograniczone przez `Http2.InitialConnectionWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="204ab-229">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="204ab-230">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="204ab-230">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="204ab-231">Wartość domyślna to 96 KB (98 304).</span><span class="sxs-lookup"><span data-stu-id="204ab-231">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="204ab-232">Synchroniczne we/wy</span><span class="sxs-lookup"><span data-stu-id="204ab-232">Synchronous IO</span></span>

<span data-ttu-id="204ab-233"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> kontroluje, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="204ab-233"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="204ab-234">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="204ab-234">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="204ab-235">Duża liczba blokowania synchronicznych operacji we/wy może prowadzić do zablokowania puli wątków, co sprawia, że aplikacja nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="204ab-235">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="204ab-236">Włącz `AllowSynchronousIO` tylko w przypadku korzystania z biblioteki, która nie obsługuje asynchronicznych operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="204ab-236">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="204ab-237">Poniższy przykład włącza synchroniczną operację we/wy:</span><span class="sxs-lookup"><span data-stu-id="204ab-237">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="204ab-238">Aby uzyskać informacje o innych opcjach i ograniczeniach Kestrel, zobacz:</span><span class="sxs-lookup"><span data-stu-id="204ab-238">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="204ab-239">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="204ab-239">Endpoint configuration</span></span>

<span data-ttu-id="204ab-240">Domyślnie ASP.NET Core wiąże się z:</span><span class="sxs-lookup"><span data-stu-id="204ab-240">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="204ab-241">`https://localhost:5001` (gdy obecny jest lokalny certyfikat programistyczny)</span><span class="sxs-lookup"><span data-stu-id="204ab-241">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="204ab-242">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="204ab-242">Specify URLs using the:</span></span>

* <span data-ttu-id="204ab-243">Zmienna środowiskowa `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="204ab-243">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="204ab-244">`--urls` argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="204ab-244">`--urls` command-line argument.</span></span>
* <span data-ttu-id="204ab-245">klucz konfiguracji hosta `urls`.</span><span class="sxs-lookup"><span data-stu-id="204ab-245">`urls` host configuration key.</span></span>
* <span data-ttu-id="204ab-246">Metoda rozszerzenia `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="204ab-246">`UseUrls` extension method.</span></span>

<span data-ttu-id="204ab-247">Wartość podana przy użyciu tych metod może być jednym lub większą liczbą punktów końcowych HTTP i HTTPS (HTTPS, jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="204ab-247">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="204ab-248">Skonfiguruj wartość jako listę rozdzieloną średnikami (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="204ab-248">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="204ab-249">Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwera](xref:fundamentals/host/web-host#server-urls) i [Zastąp konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="204ab-249">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="204ab-250">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="204ab-250">A development certificate is created:</span></span>

* <span data-ttu-id="204ab-251">Po zainstalowaniu [zestaw .NET Core SDK](/dotnet/core/sdk) .</span><span class="sxs-lookup"><span data-stu-id="204ab-251">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="204ab-252">Do utworzenia certyfikatu służy [Narzędzie dev-certs](xref:aspnetcore-2.1#https) .</span><span class="sxs-lookup"><span data-stu-id="204ab-252">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="204ab-253">Niektóre przeglądarki wymagają przyznania jawnego uprawnienia do zaufania do lokalnego certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="204ab-253">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="204ab-254">Szablony projektu konfigurują aplikacje do uruchamiania domyślnie przy użyciu protokołu HTTPS i obejmują [przekierowania https i obsługę HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="204ab-254">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="204ab-255">Wywołaj metody <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> w <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, aby skonfigurować prefiksy i porty adresów URL dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-255">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="204ab-256">`UseUrls`, `--urls` argument wiersza polecenia, klucz konfiguracji hosta `urls` oraz zmienna środowiskowa `ASPNETCORE_URLS` również działają, ale mają ograniczenia wymienione w dalszej części tej sekcji (certyfikat domyślny musi być dostępny do konfiguracji punktu końcowego HTTPS).</span><span class="sxs-lookup"><span data-stu-id="204ab-256">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="204ab-257">Konfiguracja `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="204ab-257">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="204ab-258">ConfigureEndpointDefaults (Akcja\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="204ab-258">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="204ab-259">Określa `Action` konfiguracji do uruchomienia dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="204ab-259">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="204ab-260">Wywoływanie `ConfigureEndpointDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="204ab-260">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="204ab-261">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="204ab-261">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="204ab-262">ConfigureHttpsDefaults (Akcja\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="204ab-262">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="204ab-263">Określa `Action` konfiguracji do uruchomienia dla każdego punktu końcowego HTTPS.</span><span class="sxs-lookup"><span data-stu-id="204ab-263">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="204ab-264">Wywoływanie `ConfigureHttpsDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="204ab-264">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="204ab-265">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="204ab-265">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="204ab-266">Konfiguruj (IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="204ab-266">Configure(IConfiguration)</span></span>

<span data-ttu-id="204ab-267">Tworzy moduł ładujący konfigurację służący do konfigurowania Kestrel, który pobiera <xref:Microsoft.Extensions.Configuration.IConfiguration> jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-267">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="204ab-268">Konfiguracja musi być objęta zakresem sekcji konfiguracji dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-268">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="204ab-269">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="204ab-269">ListenOptions.UseHttps</span></span>

<span data-ttu-id="204ab-270">Skonfiguruj Kestrel do korzystania z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="204ab-270">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="204ab-271">rozszerzenia `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="204ab-271">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="204ab-272">`UseHttps` &ndash; skonfigurować Kestrel do używania protokołu HTTPS z certyfikatem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="204ab-272">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="204ab-273">Zgłasza wyjątek, jeśli nie został skonfigurowany żaden certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="204ab-273">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="204ab-274">`ListenOptions.UseHttps` parametry:</span><span class="sxs-lookup"><span data-stu-id="204ab-274">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="204ab-275">`filename` to ścieżka i nazwa pliku certyfikatu odnosząca się do katalogu zawierającego pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="204ab-275">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="204ab-276">`password` to hasło wymagane do uzyskania dostępu do danych certyfikatu X. 509.</span><span class="sxs-lookup"><span data-stu-id="204ab-276">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="204ab-277">`configureOptions` jest `Action`, aby skonfigurować `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="204ab-277">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="204ab-278">Zwraca `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="204ab-278">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="204ab-279">`storeName` to magazyn certyfikatów, z którego ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="204ab-279">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="204ab-280">`subject` to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="204ab-280">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="204ab-281">`allowInvalid` wskazuje, czy należy wziąć pod uwagę nieprawidłowe certyfikaty, takie jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="204ab-281">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="204ab-282">`location` to lokalizacja magazynu, z której ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="204ab-282">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="204ab-283">`serverCertificate` to certyfikat X. 509.</span><span class="sxs-lookup"><span data-stu-id="204ab-283">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="204ab-284">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="204ab-284">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="204ab-285">Należy podać co najmniej certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="204ab-285">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="204ab-286">Obsługiwane konfiguracje opisane dalej:</span><span class="sxs-lookup"><span data-stu-id="204ab-286">Supported configurations described next:</span></span>

* <span data-ttu-id="204ab-287">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="204ab-287">No configuration</span></span>
* <span data-ttu-id="204ab-288">Zastąp domyślny certyfikat z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="204ab-288">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="204ab-289">Zmień wartości domyślne w kodzie</span><span class="sxs-lookup"><span data-stu-id="204ab-289">Change the defaults in code</span></span>

<span data-ttu-id="204ab-290">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="204ab-290">*No configuration*</span></span>

<span data-ttu-id="204ab-291">Kestrel nasłuchuje na `http://localhost:5000` i `https://localhost:5001` (jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="204ab-291">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="204ab-292">*Zastąp domyślny certyfikat z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="204ab-292">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="204ab-293">`CreateDefaultBuilder` wywołania `Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-293">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="204ab-294">Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępny dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-294">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="204ab-295">Konfigurowanie wielu punktów końcowych, w tym adresów URL i certyfikatów do użycia, z pliku znajdującego się na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="204ab-295">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="204ab-296">W poniższym przykładzie pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="204ab-296">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="204ab-297">Ustaw **AllowInvalid** na `true`, aby zezwolić na korzystanie z nieprawidłowych certyfikatów (na przykład certyfikatów z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="204ab-297">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="204ab-298">Wszystkie punkty końcowe HTTPS, które nie określają certyfikatu (**HttpsDefaultCert** w poniższym przykładzie) powracają do certyfikatu zdefiniowanego w obszarze **Certyfikaty** > **domyślnym** lub certyfikatem deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="204ab-298">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="204ab-299">Alternatywą dla korzystania z **ścieżki** i **hasła** dla dowolnego węzła certyfikatu jest określenie certyfikatu przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="204ab-299">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="204ab-300">Na przykład **certyfikaty** > **domyślnego** certyfikatu można określić jako:</span><span class="sxs-lookup"><span data-stu-id="204ab-300">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="204ab-301">Uwagi dotyczące schematu:</span><span class="sxs-lookup"><span data-stu-id="204ab-301">Schema notes:</span></span>

* <span data-ttu-id="204ab-302">W nazwach punktów końcowych nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="204ab-302">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="204ab-303">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-303">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="204ab-304">Dla każdego punktu końcowego jest wymagany parametr `Url`.</span><span class="sxs-lookup"><span data-stu-id="204ab-304">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="204ab-305">Format tego parametru jest taki sam jak parametr konfiguracji `Urls` najwyższego poziomu, z tą różnicą, że jest ograniczony do pojedynczej wartości.</span><span class="sxs-lookup"><span data-stu-id="204ab-305">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="204ab-306">Te punkty końcowe zastępują te zdefiniowane w konfiguracji `Urls` najwyższego poziomu zamiast dodawać do nich.</span><span class="sxs-lookup"><span data-stu-id="204ab-306">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="204ab-307">Punkty końcowe zdefiniowane w kodzie za pośrednictwem `Listen` kumulują się z punktami końcowymi zdefiniowanymi w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="204ab-307">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="204ab-308">Sekcja `Certificate` jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="204ab-308">The `Certificate` section is optional.</span></span> <span data-ttu-id="204ab-309">Jeśli sekcja `Certificate` nie zostanie określona, są używane wartości domyślne zdefiniowane we wcześniejszych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="204ab-309">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="204ab-310">Jeśli żadne wartości domyślne nie są dostępne, serwer zgłasza wyjątek i nie może się uruchomić.</span><span class="sxs-lookup"><span data-stu-id="204ab-310">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="204ab-311">Sekcja `Certificate` obsługuje zarówno **ścieżkę**&ndash;**hasło** , jak i **temat**&ndash;certyfikatów **magazynu** .</span><span class="sxs-lookup"><span data-stu-id="204ab-311">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="204ab-312">W ten sposób można zdefiniować dowolną liczbę punktów końcowych, dopóki nie spowoduje to konfliktów portów.</span><span class="sxs-lookup"><span data-stu-id="204ab-312">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="204ab-313">`options.Configure(context.Configuration.GetSection("{SECTION}"))` zwraca `KestrelConfigurationLoader` z metodą `.Endpoint(string name, listenOptions => { })`, której można użyć do uzupełnienia ustawień skonfigurowanego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="204ab-313">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="204ab-314">dostęp do `KestrelServerOptions.ConfigurationLoader` można uzyskać, aby kontynuować iterację istniejącego modułu ładującego, takiego jak dostarczony przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="204ab-314">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="204ab-315">Sekcja konfiguracji dla każdego punktu końcowego jest dostępna w opcjach w `Endpoint` metodzie, aby można było odczytać ustawienia niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-315">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="204ab-316">Można załadować wiele konfiguracji, wywołując `options.Configure(context.Configuration.GetSection("{SECTION}"))` ponownie z inną sekcją.</span><span class="sxs-lookup"><span data-stu-id="204ab-316">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="204ab-317">Używana jest tylko Ostatnia konfiguracja, chyba że `Load` jest jawnie wywoływana w poprzednich wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="204ab-317">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="204ab-318">Pakiet nie wywołuje `Load`, tak aby jego sekcja konfiguracji domyślnej mogła zostać zamieniona.</span><span class="sxs-lookup"><span data-stu-id="204ab-318">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="204ab-319">`KestrelConfigurationLoader` odzwierciedla `Listen`ą rodzinę interfejsów API z `KestrelServerOptions` jako przeciążania `Endpoint`, dlatego punkty końcowe kodu i konfiguracji można skonfigurować w tym samym miejscu.</span><span class="sxs-lookup"><span data-stu-id="204ab-319">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="204ab-320">Te przeciążenia nie używają nazw i używają tylko ustawień domyślnych z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="204ab-320">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="204ab-321">*Zmień wartości domyślne w kodzie*</span><span class="sxs-lookup"><span data-stu-id="204ab-321">*Change the defaults in code*</span></span>

<span data-ttu-id="204ab-322">`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` mogą służyć do zmiany domyślnych ustawień dla `ListenOptions` i `HttpsConnectionAdapterOptions`, w tym przesłanianie domyślnego certyfikatu określonego w poprzednim scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="204ab-322">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="204ab-323">przed skonfigurowaniem punktów końcowych należy wywołać `ConfigureEndpointDefaults` i `ConfigureHttpsDefaults`.</span><span class="sxs-lookup"><span data-stu-id="204ab-323">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="204ab-324">*Obsługa usługi Kestrel dla SNI*</span><span class="sxs-lookup"><span data-stu-id="204ab-324">*Kestrel support for SNI*</span></span>

<span data-ttu-id="204ab-325">[Oznaczanie nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do hostowania wielu domen na tym samym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="204ab-325">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="204ab-326">Aby SNI działał, klient wysyła nazwę hosta dla bezpiecznej sesji do serwera podczas uzgadniania TLS, aby serwer mógł zapewnić prawidłowy certyfikat.</span><span class="sxs-lookup"><span data-stu-id="204ab-326">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="204ab-327">Klient korzysta z dostarczonego certyfikatu w celu zaszyfrowania komunikacji z serwerem podczas bezpiecznej sesji, która następuje po uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="204ab-327">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="204ab-328">Kestrel obsługuje SNI za pośrednictwem wywołania zwrotnego `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="204ab-328">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="204ab-329">Wywołanie zwrotne jest wywoływane jednokrotnie dla każdego połączenia, aby umożliwić aplikacji inspekcję nazwy hosta i wybieranie odpowiedniego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="204ab-329">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="204ab-330">Obsługa SNI wymaga:</span><span class="sxs-lookup"><span data-stu-id="204ab-330">SNI support requires:</span></span>

* <span data-ttu-id="204ab-331">Uruchomione na platformie docelowej `netcoreapp2.1` lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="204ab-331">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="204ab-332">W `net461` lub później wywołanie zwrotne jest wywoływane, ale `name` jest zawsze `null`.</span><span class="sxs-lookup"><span data-stu-id="204ab-332">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="204ab-333">`name` jest również `null`, jeśli klient nie poda parametru nazwy hosta w uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="204ab-333">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="204ab-334">Wszystkie witryny sieci Web działają na tym samym wystąpieniu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-334">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="204ab-335">Kestrel nie obsługuje udostępniania adresu IP i portu w wielu wystąpieniach bez zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="204ab-335">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="204ab-336">Rejestrowanie połączeń</span><span class="sxs-lookup"><span data-stu-id="204ab-336">Connection logging</span></span>

<span data-ttu-id="204ab-337">Wywołaj <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>, aby emitować dzienniki poziomu debugowania dla komunikacji na poziomie bajtów w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="204ab-337">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="204ab-338">Rejestrowanie połączeń ułatwia rozwiązywanie problemów z komunikacją na niskim poziomie, na przykład podczas szyfrowania TLS i za serwerami proxy.</span><span class="sxs-lookup"><span data-stu-id="204ab-338">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="204ab-339">Jeśli `UseConnectionLogging` jest umieszczony przed `UseHttps`, szyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="204ab-339">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="204ab-340">Jeśli `UseConnectionLogging` jest umieszczony po `UseHttps`, odszyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="204ab-340">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="204ab-341">Powiąż z gniazdem TCP</span><span class="sxs-lookup"><span data-stu-id="204ab-341">Bind to a TCP socket</span></span>

<span data-ttu-id="204ab-342">Metoda <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> wiąże się z gniazdem TCP, a opcja lambda zezwala na konfigurację certyfikatu X. 509:</span><span class="sxs-lookup"><span data-stu-id="204ab-342">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=12-18)]

<span data-ttu-id="204ab-343">Przykład konfiguruje HTTPS dla punktu końcowego z <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="204ab-343">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="204ab-344">Użyj tego samego interfejsu API, aby skonfigurować inne ustawienia Kestrel dla określonych punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="204ab-344">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="204ab-345">Powiąż z gniazdem systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="204ab-345">Bind to a Unix socket</span></span>

<span data-ttu-id="204ab-346">Nasłuchiwanie w gnieździe systemu UNIX za pomocą <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, aby zwiększyć wydajność za pomocą Nginx, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="204ab-346">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="204ab-347">Port 0</span><span class="sxs-lookup"><span data-stu-id="204ab-347">Port 0</span></span>

<span data-ttu-id="204ab-348">Gdy `0` jest określony numer portu, Kestrel dynamicznie wiąże się z dostępnym portem.</span><span class="sxs-lookup"><span data-stu-id="204ab-348">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="204ab-349">W poniższym przykładzie pokazano, jak określić, który port Kestrel faktycznie powiązany w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="204ab-349">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="204ab-350">Po uruchomieniu aplikacji dane wyjściowe okna konsoli wskazują port dynamiczny, w którym można uzyskać dostęp do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="204ab-350">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="204ab-351">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="204ab-351">Limitations</span></span>

<span data-ttu-id="204ab-352">Skonfiguruj punkty końcowe przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="204ab-352">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="204ab-353">`--urls` argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="204ab-353">`--urls` command-line argument</span></span>
* <span data-ttu-id="204ab-354">klucz konfiguracji hosta `urls`</span><span class="sxs-lookup"><span data-stu-id="204ab-354">`urls` host configuration key</span></span>
* <span data-ttu-id="204ab-355">Zmienna środowiskowa `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="204ab-355">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="204ab-356">Te metody są przydatne do tworzenia kodu w pracy z serwerami innymi niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-356">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="204ab-357">Należy jednak pamiętać o następujących ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="204ab-357">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="204ab-358">Nie można użyć protokołu HTTPS z tymi metodami, chyba że w konfiguracji punktu końcowego HTTPS jest podany certyfikat domyślny (na przykład za pomocą konfiguracji `KestrelServerOptions` lub pliku konfiguracji, jak pokazano wcześniej w tym temacie).</span><span class="sxs-lookup"><span data-stu-id="204ab-358">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="204ab-359">Gdy oba podejścia `Listen` i `UseUrls` są używane jednocześnie, `Listen` punkty końcowe przesłaniają `UseUrls` punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-359">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="204ab-360">Konfiguracja punktu końcowego usług IIS</span><span class="sxs-lookup"><span data-stu-id="204ab-360">IIS endpoint configuration</span></span>

<span data-ttu-id="204ab-361">W przypadku korzystania z usług IIS powiązania URL dla powiązań przesłonięć usług IIS są ustawiane przez `Listen` lub `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="204ab-361">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="204ab-362">Aby uzyskać więcej informacji, zobacz temat [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="204ab-362">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="204ab-363">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="204ab-363">ListenOptions.Protocols</span></span>

<span data-ttu-id="204ab-364">Właściwość `Protocols` ustanawia protokoły HTTP (`HttpProtocols`) włączone w punkcie końcowym połączenia lub na serwerze.</span><span class="sxs-lookup"><span data-stu-id="204ab-364">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="204ab-365">Przypisz wartość do właściwości `Protocols` z wyliczenia `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="204ab-365">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="204ab-366">`HttpProtocols` wartość wyliczenia</span><span class="sxs-lookup"><span data-stu-id="204ab-366">`HttpProtocols` enum value</span></span> | <span data-ttu-id="204ab-367">Dozwolony protokół połączenia</span><span class="sxs-lookup"><span data-stu-id="204ab-367">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="204ab-368">Tylko HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="204ab-368">HTTP/1.1 only.</span></span> <span data-ttu-id="204ab-369">Może być używany z protokołem TLS lub bez niego.</span><span class="sxs-lookup"><span data-stu-id="204ab-369">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="204ab-370">Tylko HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="204ab-370">HTTP/2 only.</span></span> <span data-ttu-id="204ab-371">Mogą być używane bez protokołu TLS tylko wtedy, gdy klient obsługuje [poprzedni tryb wiedzy](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="204ab-371">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="204ab-372">HTTP/1.1 i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="204ab-372">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="204ab-373">Protokół HTTP/2 wymaga od klienta wyboru protokołu HTTP/2 w uzgadnianiu [negocjowania protokołu TLS aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3) . w przeciwnym razie wartość domyślna połączenia to HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="204ab-373">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="204ab-374">Domyślna wartość `ListenOptions.Protocols` dla dowolnego punktu końcowego jest `HttpProtocols.Http1AndHttp2`.</span><span class="sxs-lookup"><span data-stu-id="204ab-374">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="204ab-375">Ograniczenia protokołu TLS dla protokołu HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="204ab-375">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="204ab-376">TLS w wersji 1,2 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="204ab-376">TLS version 1.2 or later</span></span>
* <span data-ttu-id="204ab-377">Ponowne negocjowanie wyłączone</span><span class="sxs-lookup"><span data-stu-id="204ab-377">Renegotiation disabled</span></span>
* <span data-ttu-id="204ab-378">Kompresja wyłączona</span><span class="sxs-lookup"><span data-stu-id="204ab-378">Compression disabled</span></span>
* <span data-ttu-id="204ab-379">Minimalne rozmiary tymczasowych kluczy wymiany:</span><span class="sxs-lookup"><span data-stu-id="204ab-379">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="204ab-380">Krzywa eliptyczna Diffie-Hellmana (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 BITS</span><span class="sxs-lookup"><span data-stu-id="204ab-380">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="204ab-381">Ograniczone pole Diffie-Hellmana (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bitów</span><span class="sxs-lookup"><span data-stu-id="204ab-381">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="204ab-382">Mechanizm szyfrowania nie został zabroniony</span><span class="sxs-lookup"><span data-stu-id="204ab-382">Cipher suite not blacklisted</span></span>

<span data-ttu-id="204ab-383">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; z krzywą eliptyczna P-256 &lbrack;`FIPS186`&rbrack; jest domyślnie obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="204ab-383">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="204ab-384">Poniższy przykład umożliwia nawiązywanie połączeń HTTP/1.1 i HTTP/2 na porcie 8000.</span><span class="sxs-lookup"><span data-stu-id="204ab-384">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="204ab-385">Połączenia są zabezpieczone przez protokół TLS przy użyciu podanego certyfikatu:</span><span class="sxs-lookup"><span data-stu-id="204ab-385">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="204ab-386">Używaj oprogramowania pośredniczącego do filtrowania uzgadniania protokołu TLS dla poszczególnych połączeń dla określonych szyfrów, jeśli jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="204ab-386">Use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers if required.</span></span>

<span data-ttu-id="204ab-387">Poniższy przykład zgłasza <xref:System.NotSupportedException> dla dowolnego algorytmu szyfrowania, który nie jest obsługiwany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="204ab-387">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="204ab-388">Alternatywnie można zdefiniować i porównać [ITlsHandshakeFeature. CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) z listą akceptowalnych mechanizmów szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="204ab-388">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="204ab-389">Nie jest używane szyfrowanie z algorytmem szyfrowania [CipherAlgorithmType. null](xref:System.Security.Authentication.CipherAlgorithmType) .</span><span class="sxs-lookup"><span data-stu-id="204ab-389">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

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

<span data-ttu-id="204ab-390">Filtrowanie połączeń można również skonfigurować za pomocą <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span><span class="sxs-lookup"><span data-stu-id="204ab-390">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

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

<span data-ttu-id="204ab-391">W systemie Linux <xref:System.Net.Security.CipherSuitesPolicy> może służyć do filtrowania uzgadniania protokołu TLS dla poszczególnych połączeń:</span><span class="sxs-lookup"><span data-stu-id="204ab-391">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

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

<span data-ttu-id="204ab-392">*Ustawianie protokołu z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="204ab-392">*Set the protocol from configuration*</span></span>

<span data-ttu-id="204ab-393">`CreateDefaultBuilder` wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-393">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="204ab-394">W poniższym przykładzie pliku *appSettings. JSON* jako domyślny protokół połączenia dla wszystkich punktów końcowych:</span><span class="sxs-lookup"><span data-stu-id="204ab-394">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="204ab-395">Poniższy przykład *appSettings. JSON* ustanawia protokół połączeń HTTP/1.1 dla określonego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="204ab-395">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="204ab-396">Protokoły określone w wartościach zastąpienia kodu ustawione przez konfigurację.</span><span class="sxs-lookup"><span data-stu-id="204ab-396">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="204ab-397">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="204ab-397">Transport configuration</span></span>

<span data-ttu-id="204ab-398">Dla projektów, które wymagają użycia Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span><span class="sxs-lookup"><span data-stu-id="204ab-398">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="204ab-399">Dodaj zależność dla pakietu [Microsoft. AspNetCore. Server. Kestrel. transport. Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="204ab-399">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="204ab-400">Wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> na `IWebHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="204ab-400">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="204ab-401">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="204ab-401">URL prefixes</span></span>

<span data-ttu-id="204ab-402">W przypadku korzystania z `UseUrls`, `--urls` argumentu wiersza polecenia, klucza konfiguracji hosta `urls` lub zmiennej środowiskowej `ASPNETCORE_URLS`, prefiksy adresów URL mogą mieć jeden z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="204ab-402">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="204ab-403">Tylko prefiksy adresów URL HTTP są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-403">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="204ab-404">Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązań adresów URL przy użyciu `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="204ab-404">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="204ab-405">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="204ab-405">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="204ab-406">`0.0.0.0` jest szczególnym przypadkiem, który wiąże się ze wszystkimi adresami IPv4.</span><span class="sxs-lookup"><span data-stu-id="204ab-406">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="204ab-407">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="204ab-407">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="204ab-408">`[::]` to odpowiednik IPv6 `0.0.0.0`IPv4.</span><span class="sxs-lookup"><span data-stu-id="204ab-408">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="204ab-409">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="204ab-409">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="204ab-410">Nazwy hostów, `*`i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="204ab-410">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="204ab-411">Wszystkie nie są rozpoznawane jako prawidłowy adres IP lub `localhost` są powiązane z protokołem IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="204ab-411">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="204ab-412">Aby powiązać różne nazwy hostów z różnymi ASP.NET Core aplikacjami na tym samym porcie, użyj [protokołu HTTP. sys](xref:fundamentals/servers/httpsys) lub odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="204ab-412">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="204ab-413">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="204ab-413">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="204ab-414">Nazwa hosta `localhost` z numerem portu lub adresem IP sprzężenia zwrotnego z numerem portu</span><span class="sxs-lookup"><span data-stu-id="204ab-414">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="204ab-415">Gdy `localhost` jest określony, Kestrel próbuje powiązać z interfejsami sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="204ab-415">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="204ab-416">Jeśli żądany port jest używany przez inną usługę w dowolnym interfejsie sprzężenia zwrotnego, uruchomienie Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="204ab-416">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="204ab-417">Jeśli którykolwiek z innych przyczyn interfejsu sprzężenia zwrotnego jest niedostępny (najczęściej jest to spowodowane tym, że protokół IPv6 nie jest obsługiwany), Kestrel rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="204ab-417">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="204ab-418">Filtrowanie hostów</span><span class="sxs-lookup"><span data-stu-id="204ab-418">Host filtering</span></span>

<span data-ttu-id="204ab-419">Chociaż usługa Kestrel obsługuje konfigurację na podstawie prefiksów, takich jak `http://example.com:5000`, Kestrel w znacznym stopniu ignoruje nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="204ab-419">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="204ab-420">`localhost` hosta jest specjalnym przypadkiem używanym do tworzenia powiązań z adresami sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="204ab-420">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="204ab-421">Każdy host poza jawnym adresem IP tworzy powiązanie ze wszystkimi publicznymi adresami IP.</span><span class="sxs-lookup"><span data-stu-id="204ab-421">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="204ab-422">nagłówki `Host` nie są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="204ab-422">`Host` headers aren't validated.</span></span>

<span data-ttu-id="204ab-423">Aby obejść ten sposób, użyj oprogramowania pośredniczącego filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="204ab-423">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="204ab-424">Oprogramowanie pośredniczące do filtrowania hosta jest dostarczane przez pakiet [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , który jest niejawnie dostarczany dla ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="204ab-424">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="204ab-425">Oprogramowanie pośredniczące jest dodawane przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="204ab-425">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="204ab-426">Programowe filtrowanie hosta jest domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="204ab-426">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="204ab-427">Aby włączyć oprogramowanie pośredniczące, zdefiniuj klucz `AllowedHosts` w pliku *appSettings. json*/*appSettings.\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="204ab-427">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="204ab-428">Wartość to rozdzielana średnikami lista nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="204ab-428">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="204ab-429">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="204ab-429">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="204ab-430">W przypadku [przekierowanych nagłówków oprogramowanie pośredniczące](xref:host-and-deploy/proxy-load-balancer) ma również opcję <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="204ab-430">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="204ab-431">Przekazane nagłówki — oprogramowanie pośredniczące i filtrowanie hostów oprogramowanie pośredniczące ma podobną funkcjonalność dla różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="204ab-431">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="204ab-432">Ustawienie `AllowedHosts` z przekierowanymi nagłówkami — oprogramowanie pośredniczące jest odpowiednie, gdy nagłówek `Host` nie jest zachowywany podczas przekazywania żądań z odwrotnym serwerem proxy lub modułem równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="204ab-432">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="204ab-433">Ustawienie `AllowedHosts` przy użyciu oprogramowania pośredniczącego filtrowania hosta jest odpowiednie, gdy Kestrel jest używany jako publiczny serwer graniczny lub gdy `Host` nagłówek jest bezpośrednio przekazywany.</span><span class="sxs-lookup"><span data-stu-id="204ab-433">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="204ab-434">Aby uzyskać więcej informacji na temat przekierowanych nagłówków, należy zapoznać się z tematem <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="204ab-434">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="204ab-435">Kestrel to Międzyplatformowy [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="204ab-435">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="204ab-436">Kestrel to serwer sieci Web, który jest domyślnie uwzględniony w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="204ab-436">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="204ab-437">Kestrel obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="204ab-437">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="204ab-438">HTTPS</span><span class="sxs-lookup"><span data-stu-id="204ab-438">HTTPS</span></span>
* <span data-ttu-id="204ab-439">Nieprzezroczyste uaktualnienie używane do włączania obsługi obiektów [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="204ab-439">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="204ab-440">Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx</span><span class="sxs-lookup"><span data-stu-id="204ab-440">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="204ab-441">HTTP/2 (z wyjątkiem macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="204ab-441">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="204ab-442">&dagger;HTTP/2 będą obsługiwane w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="204ab-442">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="204ab-443">Kestrel jest obsługiwana na wszystkich platformach i wersjach obsługiwanych przez platformę .NET Core.</span><span class="sxs-lookup"><span data-stu-id="204ab-443">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="204ab-444">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="204ab-444">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="204ab-445">Obsługa protokołu HTTP/2</span><span class="sxs-lookup"><span data-stu-id="204ab-445">HTTP/2 support</span></span>

<span data-ttu-id="204ab-446">[Protokół HTTP/2](https://httpwg.org/specs/rfc7540.html) jest dostępny dla aplikacji ASP.NET Core, jeśli spełnione są następujące wymagania podstawowe:</span><span class="sxs-lookup"><span data-stu-id="204ab-446">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="204ab-447">&dagger; systemu operacyjnego</span><span class="sxs-lookup"><span data-stu-id="204ab-447">Operating system&dagger;</span></span>
  * <span data-ttu-id="204ab-448">Windows Server 2016/Windows 10 lub nowszy&Dagger;</span><span class="sxs-lookup"><span data-stu-id="204ab-448">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="204ab-449">Linux z OpenSSL 1.0.2 lub nowszym (na przykład Ubuntu 16,04 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="204ab-449">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="204ab-450">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="204ab-450">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="204ab-451">Połączenie [negocjowania protokołu warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="204ab-451">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="204ab-452">Protokół TLS 1.2 lub nowszej połączenia</span><span class="sxs-lookup"><span data-stu-id="204ab-452">TLS 1.2 or later connection</span></span>

<span data-ttu-id="204ab-453">&dagger;HTTP/2 będą obsługiwane w przypadku macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="204ab-453">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="204ab-454">&Dagger;Kestrel ma ograniczoną obsługę protokołu HTTP/2 w systemie Windows Server 2012 R2 i Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="204ab-454">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="204ab-455">Obsługa jest ograniczona, ponieważ lista obsługiwanych mechanizmów szyfrowania TLS dostępnych w tych systemach operacyjnych jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="204ab-455">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="204ab-456">Do zabezpieczenia połączeń TLS może być wymagany certyfikat wygenerowany przy użyciu algorytmu podpisu cyfrowego (ECDSA) krzywej eliptycznej.</span><span class="sxs-lookup"><span data-stu-id="204ab-456">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="204ab-457">W przypadku ustanowienia połączenia HTTP/2 raporty [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="204ab-457">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="204ab-458">Protokół HTTP/2 jest domyślnie wyłączony.</span><span class="sxs-lookup"><span data-stu-id="204ab-458">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="204ab-459">Więcej informacji na temat konfiguracji można znaleźć w sekcjach [Kestrel Options](#kestrel-options) and [ListenOptions. Protocols](#listenoptionsprotocols) .</span><span class="sxs-lookup"><span data-stu-id="204ab-459">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="204ab-460">Kiedy używać Kestrel z zwrotnym serwerem proxy</span><span class="sxs-lookup"><span data-stu-id="204ab-460">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="204ab-461">Kestrel może być używana przez siebie lub z *odwrotnym serwerem proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org)lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="204ab-461">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="204ab-462">Odwrotny serwer proxy odbiera żądania HTTP z sieci i przekazuje je do usługi Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-462">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="204ab-463">Kestrel używany jako serwer sieci Web z krawędzią (dostępną z Internetu):</span><span class="sxs-lookup"><span data-stu-id="204ab-463">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem bez serwera proxy zwrotnego](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="204ab-465">Kestrel używany w konfiguracji zwrotnego serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="204ab-465">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel komunikuje się pośrednio z Internetem za pomocą odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="204ab-467">Konfiguracja, z serwerem proxy odwrotnych lub bez niego, jest obsługiwaną konfiguracją hostingu.</span><span class="sxs-lookup"><span data-stu-id="204ab-467">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="204ab-468">Kestrel używany jako serwer graniczny bez serwera proxy odwrotnego nie obsługuje udostępniania tego samego adresu IP i portu między wieloma procesami.</span><span class="sxs-lookup"><span data-stu-id="204ab-468">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="204ab-469">Gdy Kestrel jest skonfigurowany do nasłuchiwania na porcie, Kestrel obsługuje cały ruch dla tego portu bez względu na żądania `Host` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="204ab-469">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="204ab-470">Zwrotny serwer proxy, który może udostępniać porty, ma możliwość przekazywania żądań do Kestrel na unikatowym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="204ab-470">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="204ab-471">Nawet jeśli odwrotny serwer proxy nie jest wymagany, może to być dobry wybór przy użyciu odwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="204ab-471">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="204ab-472">Zwrotny serwer proxy:</span><span class="sxs-lookup"><span data-stu-id="204ab-472">A reverse proxy:</span></span>

* <span data-ttu-id="204ab-473">Może ograniczać uwidoczniony obszar publicznej powierzchni aplikacji, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="204ab-473">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="204ab-474">Zapewnienie dodatkowej warstwy konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="204ab-474">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="204ab-475">Integracja z istniejącą infrastrukturą może być lepsza.</span><span class="sxs-lookup"><span data-stu-id="204ab-475">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="204ab-476">Uprość Równoważenie obciążenia i konfigurację komunikacji zabezpieczonej (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="204ab-476">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="204ab-477">Tylko serwer zwrotny proxy wymaga certyfikatu X. 509, a serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="204ab-477">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="204ab-478">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="204ab-478">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="204ab-479">Jak używać Kestrel w aplikacjach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="204ab-479">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="204ab-480">Pakiet [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="204ab-480">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="204ab-481">Szablony projektów ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-481">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="204ab-482">W *program.cs*kod szablonu wywołuje <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> w tle.</span><span class="sxs-lookup"><span data-stu-id="204ab-482">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="204ab-483">Aby uzyskać więcej informacji na temat `CreateDefaultBuilder` i kompilowania hosta, zobacz sekcję *Konfigurowanie hosta* w programie <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="204ab-483">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

<span data-ttu-id="204ab-484">Aby zapewnić dodatkową konfigurację po wywołaniu `CreateDefaultBuilder`, użyj `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="204ab-484">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="204ab-485">Jeśli aplikacja nie wywoła `CreateDefaultBuilder`, aby skonfigurować hosta, wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **przed** wywołaniem `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="204ab-485">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="204ab-486">Opcje Kestrel</span><span class="sxs-lookup"><span data-stu-id="204ab-486">Kestrel options</span></span>

<span data-ttu-id="204ab-487">Serwer sieci Web Kestrel ma opcje konfiguracji ograniczeń, które są szczególnie przydatne w przypadku wdrożeń mających dostęp do Internetu.</span><span class="sxs-lookup"><span data-stu-id="204ab-487">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="204ab-488">Ustaw ograniczenia we właściwości <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="204ab-488">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="204ab-489">Właściwość `Limits` przechowuje wystąpienie klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="204ab-489">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="204ab-490">W poniższych przykładach użyto przestrzeni nazw <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="204ab-490">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="204ab-491">Opcje Kestrel, które są konfigurowane w C# kodzie w poniższych przykładach, można również ustawić za pomocą [dostawcy konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="204ab-491">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="204ab-492">Na przykład dostawca konfiguracji plików może załadować konfigurację Kestrel z pliku *appSettings. JSON* lub *appSettings. { Environment} plik JSON* :</span><span class="sxs-lookup"><span data-stu-id="204ab-492">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="204ab-493">Skorzystaj z **jednej** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="204ab-493">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="204ab-494">Skonfiguruj Kestrel w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="204ab-494">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="204ab-495">Wstrzyknąć wystąpienie `IConfiguration` do klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="204ab-495">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="204ab-496">W poniższym przykładzie przyjęto założenie, że wprowadzona konfiguracja jest przypisana do właściwości `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="204ab-496">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="204ab-497">W `Startup.ConfigureServices`Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="204ab-497">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

* <span data-ttu-id="204ab-498">Skonfiguruj Kestrel podczas kompilowania hosta:</span><span class="sxs-lookup"><span data-stu-id="204ab-498">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="204ab-499">W programie *program.cs*Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="204ab-499">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="204ab-500">Obie powyższe podejścia współpracują z dowolnym [dostawcą konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="204ab-500">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="204ab-501">Limit czasu utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="204ab-501">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="204ab-502">Pobiera lub ustawia [limit czasu utrzymywania aktywności](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="204ab-502">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="204ab-503">Wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="204ab-503">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="204ab-504">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="204ab-504">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="204ab-505">Maksymalna liczba współbieżnych otwartych połączeń TCP można ustawić dla całej aplikacji przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="204ab-505">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="204ab-506">Istnieje oddzielny limit połączeń, które zostały uaktualnione z protokołu HTTP lub HTTPS do innego protokołu (na przykład w żądaniu usługi WebSockets).</span><span class="sxs-lookup"><span data-stu-id="204ab-506">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="204ab-507">Po uaktualnieniu połączenia nie jest ono wliczane do limitu `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="204ab-507">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="204ab-508">Maksymalna liczba połączeń jest domyślnie nieograniczona (null).</span><span class="sxs-lookup"><span data-stu-id="204ab-508">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="204ab-509">Maksymalny rozmiar treści żądania</span><span class="sxs-lookup"><span data-stu-id="204ab-509">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="204ab-510">Domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="204ab-510">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="204ab-511">Zalecanym podejściem do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie atrybutu <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> dla metody akcji:</span><span class="sxs-lookup"><span data-stu-id="204ab-511">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="204ab-512">Oto przykład, który pokazuje, jak skonfigurować ograniczenie dla aplikacji w każdym żądaniu:</span><span class="sxs-lookup"><span data-stu-id="204ab-512">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="204ab-513">Zastąp ustawienie określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="204ab-513">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="204ab-514">Wyjątek jest generowany, jeśli aplikacja skonfiguruje limit żądania po rozpoczęciu uruchamiania aplikacji w celu odczytania żądania.</span><span class="sxs-lookup"><span data-stu-id="204ab-514">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="204ab-515">Istnieje właściwość `IsReadOnly`, która wskazuje, czy właściwość `MaxRequestBodySize` jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="204ab-515">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="204ab-516">Gdy aplikacja jest uruchamiana [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model) [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru treści żądania Kestrel jest wyłączony, ponieważ program IIS już ustawia limit.</span><span class="sxs-lookup"><span data-stu-id="204ab-516">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="204ab-517">Minimalna stawka danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="204ab-517">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="204ab-518">Kestrel sprawdza co sekundę, jeśli dane są odbierane z określoną szybkością w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="204ab-518">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="204ab-519">Jeśli szybkość spadnie poniżej wartości minimalnej, upłynął limit czasu połączenia. Okres prolongaty to czas, przez który Kestrel nadaje klientowi zwiększenie jego szybkości wysyłania do minimum; Ta częstotliwość nie jest sprawdzana w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="204ab-519">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="204ab-520">Okres prolongaty pozwala uniknąć porzucania połączeń, które początkowo wysyłają dane z niską szybkością.</span><span class="sxs-lookup"><span data-stu-id="204ab-520">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="204ab-521">Domyślna stawka minimalna to 240 bajtów na sekundę z 5-sekundowym okresem prolongaty.</span><span class="sxs-lookup"><span data-stu-id="204ab-521">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="204ab-522">Minimalna stawka dotyczy także odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="204ab-522">A minimum rate also applies to the response.</span></span> <span data-ttu-id="204ab-523">Kod określający limit żądań i limit odpowiedzi jest taki sam, chyba że ma `RequestBody` lub `Response` w nazwach właściwości i interfejsów.</span><span class="sxs-lookup"><span data-stu-id="204ab-523">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="204ab-524">Oto przykład pokazujący sposób konfigurowania minimalnych stawek danych w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="204ab-524">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="204ab-525">Przesłoń minimalne limity szybkości dla żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="204ab-525">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="204ab-526">Żadna funkcja współczynnika, do której odwołuje się poprzednia próba nie jest obecna w `HttpContext.Features` dla żądań HTTP/2, ponieważ Modyfikowanie limitów szybkości dla każdego żądania nie jest obsługiwane w przypadku protokołu HTTP/2 ze względu na obsługę multipleksera żądania.</span><span class="sxs-lookup"><span data-stu-id="204ab-526">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="204ab-527">Limity szybkości dla całego serwera skonfigurowane za pomocą `KestrelServerOptions.Limits` nadal mają zastosowanie do połączeń HTTP/1. x i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="204ab-527">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="204ab-528">Limit czasu nagłówków żądań</span><span class="sxs-lookup"><span data-stu-id="204ab-528">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="204ab-529">Pobiera lub ustawia maksymalny czas, przez jaki serwer spędza nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="204ab-529">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="204ab-530">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="204ab-530">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="204ab-531">Maksymalna liczba strumieni na połączenie</span><span class="sxs-lookup"><span data-stu-id="204ab-531">Maximum streams per connection</span></span>

<span data-ttu-id="204ab-532">`Http2.MaxStreamsPerConnection` ogranicza liczbę współbieżnych strumieni żądań dla połączenia HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="204ab-532">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="204ab-533">Odrzucone strumienie są nadmiarowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-533">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="204ab-534">Wartość domyślna to 100.</span><span class="sxs-lookup"><span data-stu-id="204ab-534">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="204ab-535">Rozmiar tabeli nagłówka</span><span class="sxs-lookup"><span data-stu-id="204ab-535">Header table size</span></span>

<span data-ttu-id="204ab-536">Dekoder HPACK dekompresuje nagłówki HTTP dla połączeń HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="204ab-536">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="204ab-537">`Http2.HeaderTableSize` ogranicza rozmiar tabeli kompresji nagłówka używanej przez dekoder HPACK.</span><span class="sxs-lookup"><span data-stu-id="204ab-537">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="204ab-538">Wartość jest podawana w oktetach i musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="204ab-538">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="204ab-539">Wartość domyślna to 4096.</span><span class="sxs-lookup"><span data-stu-id="204ab-539">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="204ab-540">Maksymalny rozmiar ramki</span><span class="sxs-lookup"><span data-stu-id="204ab-540">Maximum frame size</span></span>

<span data-ttu-id="204ab-541">`Http2.MaxFrameSize` wskazuje maksymalny rozmiar ładunku ramki połączenia HTTP/2 do odebrania.</span><span class="sxs-lookup"><span data-stu-id="204ab-541">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="204ab-542">Wartość jest podawana w oktetach i musi należeć do przedziału od 2 ^ 14 (16 384) do 2 ^ 24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="204ab-542">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="204ab-543">Wartość domyślna to 2 ^ 14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="204ab-543">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="204ab-544">Maksymalny rozmiar nagłówka żądania</span><span class="sxs-lookup"><span data-stu-id="204ab-544">Maximum request header size</span></span>

<span data-ttu-id="204ab-545">`Http2.MaxRequestHeaderFieldSize` wskazuje maksymalny dozwolony rozmiar w oktetach wartości nagłówka żądania.</span><span class="sxs-lookup"><span data-stu-id="204ab-545">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="204ab-546">Ten limit dotyczy zarówno nazwy, jak i wartości razem w skompresowanych i nieskompresowanych reprezentacji.</span><span class="sxs-lookup"><span data-stu-id="204ab-546">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="204ab-547">Wartość musi być większa od zera (0).</span><span class="sxs-lookup"><span data-stu-id="204ab-547">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="204ab-548">Wartość domyślna to 8 192.</span><span class="sxs-lookup"><span data-stu-id="204ab-548">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="204ab-549">Początkowy rozmiar okna połączenia</span><span class="sxs-lookup"><span data-stu-id="204ab-549">Initial connection window size</span></span>

<span data-ttu-id="204ab-550">`Http2.InitialConnectionWindowSize` wskazuje maksymalne dane dotyczące treści żądania w bajtach, które bufory serwera w tym samym czasie są agregowane dla wszystkich żądań (strumieni) na połączenie.</span><span class="sxs-lookup"><span data-stu-id="204ab-550">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="204ab-551">Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="204ab-551">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="204ab-552">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="204ab-552">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="204ab-553">Wartość domyślna to 128 KB (131 072).</span><span class="sxs-lookup"><span data-stu-id="204ab-553">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="204ab-554">Rozmiar początkowego okna strumienia</span><span class="sxs-lookup"><span data-stu-id="204ab-554">Initial stream window size</span></span>

<span data-ttu-id="204ab-555">`Http2.InitialStreamWindowSize` wskazuje maksymalne dane dotyczące treści żądania w bajtach bufory serwerów w jednym momencie na żądanie (Stream).</span><span class="sxs-lookup"><span data-stu-id="204ab-555">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="204ab-556">Żądania są również ograniczone przez `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="204ab-556">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="204ab-557">Wartość musi być większa lub równa 65 535 i mniejsza niż 2 ^ 31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="204ab-557">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="204ab-558">Wartość domyślna to 96 KB (98 304).</span><span class="sxs-lookup"><span data-stu-id="204ab-558">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="204ab-559">Synchroniczne we/wy</span><span class="sxs-lookup"><span data-stu-id="204ab-559">Synchronous IO</span></span>

<span data-ttu-id="204ab-560"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> kontroluje, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="204ab-560"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="204ab-561">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="204ab-561">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="204ab-562">Duża liczba blokowania synchronicznych operacji we/wy może prowadzić do zablokowania puli wątków, co sprawia, że aplikacja nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="204ab-562">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="204ab-563">Włącz `AllowSynchronousIO` tylko w przypadku korzystania z biblioteki, która nie obsługuje asynchronicznych operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="204ab-563">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="204ab-564">Poniższy przykład włącza synchroniczną operację we/wy:</span><span class="sxs-lookup"><span data-stu-id="204ab-564">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="204ab-565">Aby uzyskać informacje o innych opcjach i ograniczeniach Kestrel, zobacz:</span><span class="sxs-lookup"><span data-stu-id="204ab-565">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="204ab-566">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="204ab-566">Endpoint configuration</span></span>

<span data-ttu-id="204ab-567">Domyślnie ASP.NET Core wiąże się z:</span><span class="sxs-lookup"><span data-stu-id="204ab-567">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="204ab-568">`https://localhost:5001` (gdy obecny jest lokalny certyfikat programistyczny)</span><span class="sxs-lookup"><span data-stu-id="204ab-568">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="204ab-569">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="204ab-569">Specify URLs using the:</span></span>

* <span data-ttu-id="204ab-570">Zmienna środowiskowa `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="204ab-570">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="204ab-571">`--urls` argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="204ab-571">`--urls` command-line argument.</span></span>
* <span data-ttu-id="204ab-572">klucz konfiguracji hosta `urls`.</span><span class="sxs-lookup"><span data-stu-id="204ab-572">`urls` host configuration key.</span></span>
* <span data-ttu-id="204ab-573">Metoda rozszerzenia `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="204ab-573">`UseUrls` extension method.</span></span>

<span data-ttu-id="204ab-574">Wartość podana przy użyciu tych metod może być jednym lub większą liczbą punktów końcowych HTTP i HTTPS (HTTPS, jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="204ab-574">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="204ab-575">Skonfiguruj wartość jako listę rozdzieloną średnikami (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="204ab-575">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="204ab-576">Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwera](xref:fundamentals/host/web-host#server-urls) i [Zastąp konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="204ab-576">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="204ab-577">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="204ab-577">A development certificate is created:</span></span>

* <span data-ttu-id="204ab-578">Po zainstalowaniu [zestaw .NET Core SDK](/dotnet/core/sdk) .</span><span class="sxs-lookup"><span data-stu-id="204ab-578">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="204ab-579">Do utworzenia certyfikatu służy [Narzędzie dev-certs](xref:aspnetcore-2.1#https) .</span><span class="sxs-lookup"><span data-stu-id="204ab-579">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="204ab-580">Niektóre przeglądarki wymagają przyznania jawnego uprawnienia do zaufania do lokalnego certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="204ab-580">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="204ab-581">Szablony projektu konfigurują aplikacje do uruchamiania domyślnie przy użyciu protokołu HTTPS i obejmują [przekierowania https i obsługę HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="204ab-581">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="204ab-582">Wywołaj metody <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> w <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, aby skonfigurować prefiksy i porty adresów URL dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-582">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="204ab-583">`UseUrls`, `--urls` argument wiersza polecenia, klucz konfiguracji hosta `urls` oraz zmienna środowiskowa `ASPNETCORE_URLS` również działają, ale mają ograniczenia wymienione w dalszej części tej sekcji (certyfikat domyślny musi być dostępny do konfiguracji punktu końcowego HTTPS).</span><span class="sxs-lookup"><span data-stu-id="204ab-583">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="204ab-584">Konfiguracja `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="204ab-584">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="204ab-585">ConfigureEndpointDefaults (Akcja\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="204ab-585">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="204ab-586">Określa `Action` konfiguracji do uruchomienia dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="204ab-586">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="204ab-587">Wywoływanie `ConfigureEndpointDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="204ab-587">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="204ab-588">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="204ab-588">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="204ab-589">ConfigureHttpsDefaults (Akcja\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="204ab-589">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="204ab-590">Określa `Action` konfiguracji do uruchomienia dla każdego punktu końcowego HTTPS.</span><span class="sxs-lookup"><span data-stu-id="204ab-590">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="204ab-591">Wywoływanie `ConfigureHttpsDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="204ab-591">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="204ab-592">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="204ab-592">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>


### <a name="configureiconfiguration"></a><span data-ttu-id="204ab-593">Konfiguruj (IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="204ab-593">Configure(IConfiguration)</span></span>

<span data-ttu-id="204ab-594">Tworzy moduł ładujący konfigurację służący do konfigurowania Kestrel, który pobiera <xref:Microsoft.Extensions.Configuration.IConfiguration> jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-594">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="204ab-595">Konfiguracja musi być objęta zakresem sekcji konfiguracji dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-595">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="204ab-596">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="204ab-596">ListenOptions.UseHttps</span></span>

<span data-ttu-id="204ab-597">Skonfiguruj Kestrel do korzystania z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="204ab-597">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="204ab-598">rozszerzenia `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="204ab-598">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="204ab-599">`UseHttps` &ndash; skonfigurować Kestrel do używania protokołu HTTPS z certyfikatem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="204ab-599">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="204ab-600">Zgłasza wyjątek, jeśli nie został skonfigurowany żaden certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="204ab-600">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="204ab-601">`ListenOptions.UseHttps` parametry:</span><span class="sxs-lookup"><span data-stu-id="204ab-601">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="204ab-602">`filename` to ścieżka i nazwa pliku certyfikatu odnosząca się do katalogu zawierającego pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="204ab-602">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="204ab-603">`password` to hasło wymagane do uzyskania dostępu do danych certyfikatu X. 509.</span><span class="sxs-lookup"><span data-stu-id="204ab-603">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="204ab-604">`configureOptions` jest `Action`, aby skonfigurować `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="204ab-604">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="204ab-605">Zwraca `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="204ab-605">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="204ab-606">`storeName` to magazyn certyfikatów, z którego ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="204ab-606">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="204ab-607">`subject` to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="204ab-607">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="204ab-608">`allowInvalid` wskazuje, czy należy wziąć pod uwagę nieprawidłowe certyfikaty, takie jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="204ab-608">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="204ab-609">`location` to lokalizacja magazynu, z której ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="204ab-609">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="204ab-610">`serverCertificate` to certyfikat X. 509.</span><span class="sxs-lookup"><span data-stu-id="204ab-610">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="204ab-611">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="204ab-611">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="204ab-612">Należy podać co najmniej certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="204ab-612">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="204ab-613">Obsługiwane konfiguracje opisane dalej:</span><span class="sxs-lookup"><span data-stu-id="204ab-613">Supported configurations described next:</span></span>

* <span data-ttu-id="204ab-614">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="204ab-614">No configuration</span></span>
* <span data-ttu-id="204ab-615">Zastąp domyślny certyfikat z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="204ab-615">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="204ab-616">Zmień wartości domyślne w kodzie</span><span class="sxs-lookup"><span data-stu-id="204ab-616">Change the defaults in code</span></span>

<span data-ttu-id="204ab-617">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="204ab-617">*No configuration*</span></span>

<span data-ttu-id="204ab-618">Kestrel nasłuchuje na `http://localhost:5000` i `https://localhost:5001` (jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="204ab-618">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="204ab-619">*Zastąp domyślny certyfikat z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="204ab-619">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="204ab-620">`CreateDefaultBuilder` wywołania `Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-620">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="204ab-621">Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępny dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-621">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="204ab-622">Konfigurowanie wielu punktów końcowych, w tym adresów URL i certyfikatów do użycia, z pliku znajdującego się na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="204ab-622">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="204ab-623">W poniższym przykładzie pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="204ab-623">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="204ab-624">Ustaw **AllowInvalid** na `true`, aby zezwolić na korzystanie z nieprawidłowych certyfikatów (na przykład certyfikatów z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="204ab-624">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="204ab-625">Wszystkie punkty końcowe HTTPS, które nie określają certyfikatu (**HttpsDefaultCert** w poniższym przykładzie) powracają do certyfikatu zdefiniowanego w obszarze **Certyfikaty** > **domyślnym** lub certyfikatem deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="204ab-625">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="204ab-626">Alternatywą dla korzystania z **ścieżki** i **hasła** dla dowolnego węzła certyfikatu jest określenie certyfikatu przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="204ab-626">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="204ab-627">Na przykład **certyfikaty** > **domyślnego** certyfikatu można określić jako:</span><span class="sxs-lookup"><span data-stu-id="204ab-627">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="204ab-628">Uwagi dotyczące schematu:</span><span class="sxs-lookup"><span data-stu-id="204ab-628">Schema notes:</span></span>

* <span data-ttu-id="204ab-629">W nazwach punktów końcowych nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="204ab-629">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="204ab-630">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-630">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="204ab-631">Dla każdego punktu końcowego jest wymagany parametr `Url`.</span><span class="sxs-lookup"><span data-stu-id="204ab-631">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="204ab-632">Format tego parametru jest taki sam jak parametr konfiguracji `Urls` najwyższego poziomu, z tą różnicą, że jest ograniczony do pojedynczej wartości.</span><span class="sxs-lookup"><span data-stu-id="204ab-632">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="204ab-633">Te punkty końcowe zastępują te zdefiniowane w konfiguracji `Urls` najwyższego poziomu zamiast dodawać do nich.</span><span class="sxs-lookup"><span data-stu-id="204ab-633">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="204ab-634">Punkty końcowe zdefiniowane w kodzie za pośrednictwem `Listen` kumulują się z punktami końcowymi zdefiniowanymi w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="204ab-634">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="204ab-635">Sekcja `Certificate` jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="204ab-635">The `Certificate` section is optional.</span></span> <span data-ttu-id="204ab-636">Jeśli sekcja `Certificate` nie zostanie określona, są używane wartości domyślne zdefiniowane we wcześniejszych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="204ab-636">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="204ab-637">Jeśli żadne wartości domyślne nie są dostępne, serwer zgłasza wyjątek i nie może się uruchomić.</span><span class="sxs-lookup"><span data-stu-id="204ab-637">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="204ab-638">Sekcja `Certificate` obsługuje zarówno **ścieżkę**&ndash;**hasło** , jak i **temat**&ndash;certyfikatów **magazynu** .</span><span class="sxs-lookup"><span data-stu-id="204ab-638">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="204ab-639">W ten sposób można zdefiniować dowolną liczbę punktów końcowych, dopóki nie spowoduje to konfliktów portów.</span><span class="sxs-lookup"><span data-stu-id="204ab-639">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="204ab-640">`options.Configure(context.Configuration.GetSection("{SECTION}"))` zwraca `KestrelConfigurationLoader` z metodą `.Endpoint(string name, listenOptions => { })`, której można użyć do uzupełnienia ustawień skonfigurowanego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="204ab-640">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="204ab-641">dostęp do `KestrelServerOptions.ConfigurationLoader` można uzyskać, aby kontynuować iterację istniejącego modułu ładującego, takiego jak dostarczony przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="204ab-641">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="204ab-642">Sekcja konfiguracji dla każdego punktu końcowego jest dostępna w opcjach w `Endpoint` metodzie, aby można było odczytać ustawienia niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-642">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="204ab-643">Można załadować wiele konfiguracji, wywołując `options.Configure(context.Configuration.GetSection("{SECTION}"))` ponownie z inną sekcją.</span><span class="sxs-lookup"><span data-stu-id="204ab-643">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="204ab-644">Używana jest tylko Ostatnia konfiguracja, chyba że `Load` jest jawnie wywoływana w poprzednich wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="204ab-644">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="204ab-645">Pakiet nie wywołuje `Load`, tak aby jego sekcja konfiguracji domyślnej mogła zostać zamieniona.</span><span class="sxs-lookup"><span data-stu-id="204ab-645">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="204ab-646">`KestrelConfigurationLoader` odzwierciedla `Listen`ą rodzinę interfejsów API z `KestrelServerOptions` jako przeciążania `Endpoint`, dlatego punkty końcowe kodu i konfiguracji można skonfigurować w tym samym miejscu.</span><span class="sxs-lookup"><span data-stu-id="204ab-646">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="204ab-647">Te przeciążenia nie używają nazw i używają tylko ustawień domyślnych z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="204ab-647">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="204ab-648">*Zmień wartości domyślne w kodzie*</span><span class="sxs-lookup"><span data-stu-id="204ab-648">*Change the defaults in code*</span></span>

<span data-ttu-id="204ab-649">`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` mogą służyć do zmiany domyślnych ustawień dla `ListenOptions` i `HttpsConnectionAdapterOptions`, w tym przesłanianie domyślnego certyfikatu określonego w poprzednim scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="204ab-649">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="204ab-650">przed skonfigurowaniem punktów końcowych należy wywołać `ConfigureEndpointDefaults` i `ConfigureHttpsDefaults`.</span><span class="sxs-lookup"><span data-stu-id="204ab-650">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="204ab-651">*Obsługa usługi Kestrel dla SNI*</span><span class="sxs-lookup"><span data-stu-id="204ab-651">*Kestrel support for SNI*</span></span>

<span data-ttu-id="204ab-652">[Oznaczanie nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do hostowania wielu domen na tym samym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="204ab-652">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="204ab-653">Aby SNI działał, klient wysyła nazwę hosta dla bezpiecznej sesji do serwera podczas uzgadniania TLS, aby serwer mógł zapewnić prawidłowy certyfikat.</span><span class="sxs-lookup"><span data-stu-id="204ab-653">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="204ab-654">Klient korzysta z dostarczonego certyfikatu w celu zaszyfrowania komunikacji z serwerem podczas bezpiecznej sesji, która następuje po uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="204ab-654">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="204ab-655">Kestrel obsługuje SNI za pośrednictwem wywołania zwrotnego `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="204ab-655">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="204ab-656">Wywołanie zwrotne jest wywoływane jednokrotnie dla każdego połączenia, aby umożliwić aplikacji inspekcję nazwy hosta i wybieranie odpowiedniego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="204ab-656">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="204ab-657">Obsługa SNI wymaga:</span><span class="sxs-lookup"><span data-stu-id="204ab-657">SNI support requires:</span></span>

* <span data-ttu-id="204ab-658">Uruchomione na platformie docelowej `netcoreapp2.1` lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="204ab-658">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="204ab-659">W `net461` lub później wywołanie zwrotne jest wywoływane, ale `name` jest zawsze `null`.</span><span class="sxs-lookup"><span data-stu-id="204ab-659">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="204ab-660">`name` jest również `null`, jeśli klient nie poda parametru nazwy hosta w uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="204ab-660">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="204ab-661">Wszystkie witryny sieci Web działają na tym samym wystąpieniu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-661">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="204ab-662">Kestrel nie obsługuje udostępniania adresu IP i portu w wielu wystąpieniach bez zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="204ab-662">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="204ab-663">Rejestrowanie połączeń</span><span class="sxs-lookup"><span data-stu-id="204ab-663">Connection logging</span></span>

<span data-ttu-id="204ab-664">Wywołaj <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>, aby emitować dzienniki poziomu debugowania dla komunikacji na poziomie bajtów w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="204ab-664">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="204ab-665">Rejestrowanie połączeń ułatwia rozwiązywanie problemów z komunikacją na niskim poziomie, na przykład podczas szyfrowania TLS i za serwerami proxy.</span><span class="sxs-lookup"><span data-stu-id="204ab-665">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="204ab-666">Jeśli `UseConnectionLogging` jest umieszczony przed `UseHttps`, szyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="204ab-666">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="204ab-667">Jeśli `UseConnectionLogging` jest umieszczony po `UseHttps`, odszyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="204ab-667">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="204ab-668">Powiąż z gniazdem TCP</span><span class="sxs-lookup"><span data-stu-id="204ab-668">Bind to a TCP socket</span></span>

<span data-ttu-id="204ab-669">Metoda <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> wiąże się z gniazdem TCP, a opcja lambda zezwala na konfigurację certyfikatu X. 509:</span><span class="sxs-lookup"><span data-stu-id="204ab-669">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="204ab-670">Przykład konfiguruje HTTPS dla punktu końcowego z <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="204ab-670">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="204ab-671">Użyj tego samego interfejsu API, aby skonfigurować inne ustawienia Kestrel dla określonych punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="204ab-671">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="204ab-672">Powiąż z gniazdem systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="204ab-672">Bind to a Unix socket</span></span>

<span data-ttu-id="204ab-673">Nasłuchiwanie w gnieździe systemu UNIX za pomocą <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, aby zwiększyć wydajność za pomocą Nginx, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="204ab-673">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="204ab-674">Port 0</span><span class="sxs-lookup"><span data-stu-id="204ab-674">Port 0</span></span>

<span data-ttu-id="204ab-675">Gdy `0` jest określony numer portu, Kestrel dynamicznie wiąże się z dostępnym portem.</span><span class="sxs-lookup"><span data-stu-id="204ab-675">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="204ab-676">W poniższym przykładzie pokazano, jak określić, który port Kestrel faktycznie powiązany w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="204ab-676">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="204ab-677">Po uruchomieniu aplikacji dane wyjściowe okna konsoli wskazują port dynamiczny, w którym można uzyskać dostęp do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="204ab-677">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="204ab-678">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="204ab-678">Limitations</span></span>

<span data-ttu-id="204ab-679">Skonfiguruj punkty końcowe przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="204ab-679">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="204ab-680">`--urls` argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="204ab-680">`--urls` command-line argument</span></span>
* <span data-ttu-id="204ab-681">klucz konfiguracji hosta `urls`</span><span class="sxs-lookup"><span data-stu-id="204ab-681">`urls` host configuration key</span></span>
* <span data-ttu-id="204ab-682">Zmienna środowiskowa `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="204ab-682">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="204ab-683">Te metody są przydatne do tworzenia kodu w pracy z serwerami innymi niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-683">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="204ab-684">Należy jednak pamiętać o następujących ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="204ab-684">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="204ab-685">Nie można użyć protokołu HTTPS z tymi metodami, chyba że w konfiguracji punktu końcowego HTTPS jest podany certyfikat domyślny (na przykład za pomocą konfiguracji `KestrelServerOptions` lub pliku konfiguracji, jak pokazano wcześniej w tym temacie).</span><span class="sxs-lookup"><span data-stu-id="204ab-685">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="204ab-686">Gdy oba podejścia `Listen` i `UseUrls` są używane jednocześnie, `Listen` punkty końcowe przesłaniają `UseUrls` punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-686">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="204ab-687">Konfiguracja punktu końcowego usług IIS</span><span class="sxs-lookup"><span data-stu-id="204ab-687">IIS endpoint configuration</span></span>

<span data-ttu-id="204ab-688">W przypadku korzystania z usług IIS powiązania URL dla powiązań przesłonięć usług IIS są ustawiane przez `Listen` lub `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="204ab-688">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="204ab-689">Aby uzyskać więcej informacji, zobacz temat [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="204ab-689">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="204ab-690">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="204ab-690">ListenOptions.Protocols</span></span>

<span data-ttu-id="204ab-691">Właściwość `Protocols` ustanawia protokoły HTTP (`HttpProtocols`) włączone w punkcie końcowym połączenia lub na serwerze.</span><span class="sxs-lookup"><span data-stu-id="204ab-691">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="204ab-692">Przypisz wartość do właściwości `Protocols` z wyliczenia `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="204ab-692">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="204ab-693">`HttpProtocols` wartość wyliczenia</span><span class="sxs-lookup"><span data-stu-id="204ab-693">`HttpProtocols` enum value</span></span> | <span data-ttu-id="204ab-694">Dozwolony protokół połączenia</span><span class="sxs-lookup"><span data-stu-id="204ab-694">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="204ab-695">Tylko HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="204ab-695">HTTP/1.1 only.</span></span> <span data-ttu-id="204ab-696">Może być używany z protokołem TLS lub bez niego.</span><span class="sxs-lookup"><span data-stu-id="204ab-696">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="204ab-697">Tylko HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="204ab-697">HTTP/2 only.</span></span> <span data-ttu-id="204ab-698">Mogą być używane bez protokołu TLS tylko wtedy, gdy klient obsługuje [poprzedni tryb wiedzy](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="204ab-698">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="204ab-699">HTTP/1.1 i HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="204ab-699">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="204ab-700">Protokół HTTP/2 wymaga połączenia protokołów TLS i [warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3) . w przeciwnym razie wartość domyślna połączenia to HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="204ab-700">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="204ab-701">Domyślnym protokołem jest HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="204ab-701">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="204ab-702">Ograniczenia protokołu TLS dla protokołu HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="204ab-702">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="204ab-703">TLS w wersji 1,2 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="204ab-703">TLS version 1.2 or later</span></span>
* <span data-ttu-id="204ab-704">Ponowne negocjowanie wyłączone</span><span class="sxs-lookup"><span data-stu-id="204ab-704">Renegotiation disabled</span></span>
* <span data-ttu-id="204ab-705">Kompresja wyłączona</span><span class="sxs-lookup"><span data-stu-id="204ab-705">Compression disabled</span></span>
* <span data-ttu-id="204ab-706">Minimalne rozmiary tymczasowych kluczy wymiany:</span><span class="sxs-lookup"><span data-stu-id="204ab-706">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="204ab-707">Krzywa eliptyczna Diffie-Hellmana (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 BITS</span><span class="sxs-lookup"><span data-stu-id="204ab-707">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="204ab-708">Ograniczone pole Diffie-Hellmana (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bitów</span><span class="sxs-lookup"><span data-stu-id="204ab-708">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="204ab-709">Mechanizm szyfrowania nie został zabroniony</span><span class="sxs-lookup"><span data-stu-id="204ab-709">Cipher suite not blacklisted</span></span>

<span data-ttu-id="204ab-710">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; z krzywą eliptyczna P-256 &lbrack;`FIPS186`&rbrack; jest domyślnie obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="204ab-710">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="204ab-711">Poniższy przykład umożliwia nawiązywanie połączeń HTTP/1.1 i HTTP/2 na porcie 8000.</span><span class="sxs-lookup"><span data-stu-id="204ab-711">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="204ab-712">Połączenia są zabezpieczone przez protokół TLS przy użyciu podanego certyfikatu:</span><span class="sxs-lookup"><span data-stu-id="204ab-712">Connections are secured by TLS with a supplied certificate:</span></span>

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

<span data-ttu-id="204ab-713">Opcjonalnie można utworzyć implementację `IConnectionAdapter` do filtrowania uzgadniania protokołu TLS dla poszczególnych połączeń dla określonych szyfrów:</span><span class="sxs-lookup"><span data-stu-id="204ab-713">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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

<span data-ttu-id="204ab-714">*Ustawianie protokołu z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="204ab-714">*Set the protocol from configuration*</span></span>

<span data-ttu-id="204ab-715"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-715"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="204ab-716">W poniższym przykładzie pliku *appSettings. JSON* jest ustanawiany domyślny protokół połączeń (http/1.1 i http/2) dla wszystkich punktów końcowych Kestrel:</span><span class="sxs-lookup"><span data-stu-id="204ab-716">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="204ab-717">Poniższy przykład pliku konfiguracji ustanawia protokół połączenia dla określonego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="204ab-717">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="204ab-718">Protokoły określone w wartościach zastąpienia kodu ustawione przez konfigurację.</span><span class="sxs-lookup"><span data-stu-id="204ab-718">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="204ab-719">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="204ab-719">Transport configuration</span></span>

<span data-ttu-id="204ab-720">W wersji platformy ASP.NET Core 2.1 transport domyślny serwera Kestrel nie jest już oparty na bibliotece libuv, ale na zarządzanych gniazdach.</span><span class="sxs-lookup"><span data-stu-id="204ab-720">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="204ab-721">Jest to istotna zmiana dla aplikacji ASP.NET Core 2,0 uaktualniana do 2,1, które wywołują <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> i zależą od jednego z następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="204ab-721">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="204ab-722">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (bezpośrednie odwołanie do pakietu)</span><span class="sxs-lookup"><span data-stu-id="204ab-722">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="204ab-723">Microsoft. AspNetCore. App</span><span class="sxs-lookup"><span data-stu-id="204ab-723">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="204ab-724">Dla projektów, które wymagają użycia Libuv:</span><span class="sxs-lookup"><span data-stu-id="204ab-724">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="204ab-725">Dodaj zależność dla pakietu [Microsoft. AspNetCore. Server. Kestrel. transport. Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="204ab-725">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="204ab-726"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>wywołania:</span><span class="sxs-lookup"><span data-stu-id="204ab-726">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="204ab-727">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="204ab-727">URL prefixes</span></span>

<span data-ttu-id="204ab-728">W przypadku korzystania z `UseUrls`, `--urls` argumentu wiersza polecenia, klucza konfiguracji hosta `urls` lub zmiennej środowiskowej `ASPNETCORE_URLS`, prefiksy adresów URL mogą mieć jeden z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="204ab-728">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="204ab-729">Tylko prefiksy adresów URL HTTP są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-729">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="204ab-730">Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązań adresów URL przy użyciu `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="204ab-730">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="204ab-731">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="204ab-731">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="204ab-732">`0.0.0.0` jest szczególnym przypadkiem, który wiąże się ze wszystkimi adresami IPv4.</span><span class="sxs-lookup"><span data-stu-id="204ab-732">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="204ab-733">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="204ab-733">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="204ab-734">`[::]` to odpowiednik IPv6 `0.0.0.0`IPv4.</span><span class="sxs-lookup"><span data-stu-id="204ab-734">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="204ab-735">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="204ab-735">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="204ab-736">Nazwy hostów, `*`i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="204ab-736">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="204ab-737">Wszystkie nie są rozpoznawane jako prawidłowy adres IP lub `localhost` są powiązane z protokołem IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="204ab-737">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="204ab-738">Aby powiązać różne nazwy hostów z różnymi ASP.NET Core aplikacjami na tym samym porcie, użyj [protokołu HTTP. sys](xref:fundamentals/servers/httpsys) lub odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="204ab-738">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="204ab-739">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="204ab-739">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="204ab-740">Nazwa hosta `localhost` z numerem portu lub adresem IP sprzężenia zwrotnego z numerem portu</span><span class="sxs-lookup"><span data-stu-id="204ab-740">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="204ab-741">Gdy `localhost` jest określony, Kestrel próbuje powiązać z interfejsami sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="204ab-741">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="204ab-742">Jeśli żądany port jest używany przez inną usługę w dowolnym interfejsie sprzężenia zwrotnego, uruchomienie Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="204ab-742">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="204ab-743">Jeśli którykolwiek z innych przyczyn interfejsu sprzężenia zwrotnego jest niedostępny (najczęściej jest to spowodowane tym, że protokół IPv6 nie jest obsługiwany), Kestrel rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="204ab-743">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="204ab-744">Filtrowanie hostów</span><span class="sxs-lookup"><span data-stu-id="204ab-744">Host filtering</span></span>

<span data-ttu-id="204ab-745">Chociaż usługa Kestrel obsługuje konfigurację na podstawie prefiksów, takich jak `http://example.com:5000`, Kestrel w znacznym stopniu ignoruje nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="204ab-745">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="204ab-746">`localhost` hosta jest specjalnym przypadkiem używanym do tworzenia powiązań z adresami sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="204ab-746">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="204ab-747">Każdy host poza jawnym adresem IP tworzy powiązanie ze wszystkimi publicznymi adresami IP.</span><span class="sxs-lookup"><span data-stu-id="204ab-747">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="204ab-748">nagłówki `Host` nie są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="204ab-748">`Host` headers aren't validated.</span></span>

<span data-ttu-id="204ab-749">Aby obejść ten sposób, użyj oprogramowania pośredniczącego filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="204ab-749">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="204ab-750">Oprogramowanie pośredniczące do filtrowania hosta jest dostarczane przez pakiet [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , który jest zawarty w pakiecie [Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub 2,2).</span><span class="sxs-lookup"><span data-stu-id="204ab-750">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="204ab-751">Oprogramowanie pośredniczące jest dodawane przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="204ab-751">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="204ab-752">Programowe filtrowanie hosta jest domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="204ab-752">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="204ab-753">Aby włączyć oprogramowanie pośredniczące, zdefiniuj klucz `AllowedHosts` w pliku *appSettings. json*/*appSettings.\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="204ab-753">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="204ab-754">Wartość to rozdzielana średnikami lista nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="204ab-754">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="204ab-755">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="204ab-755">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="204ab-756">W przypadku [przekierowanych nagłówków oprogramowanie pośredniczące](xref:host-and-deploy/proxy-load-balancer) ma również opcję <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="204ab-756">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="204ab-757">Przekazane nagłówki — oprogramowanie pośredniczące i filtrowanie hostów oprogramowanie pośredniczące ma podobną funkcjonalność dla różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="204ab-757">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="204ab-758">Ustawienie `AllowedHosts` z przekierowanymi nagłówkami — oprogramowanie pośredniczące jest odpowiednie, gdy nagłówek `Host` nie jest zachowywany podczas przekazywania żądań z odwrotnym serwerem proxy lub modułem równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="204ab-758">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="204ab-759">Ustawienie `AllowedHosts` przy użyciu oprogramowania pośredniczącego filtrowania hosta jest odpowiednie, gdy Kestrel jest używany jako publiczny serwer graniczny lub gdy `Host` nagłówek jest bezpośrednio przekazywany.</span><span class="sxs-lookup"><span data-stu-id="204ab-759">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="204ab-760">Aby uzyskać więcej informacji na temat przekierowanych nagłówków, należy zapoznać się z tematem <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="204ab-760">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="204ab-761">Kestrel to Międzyplatformowy [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="204ab-761">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="204ab-762">Kestrel to serwer sieci Web, który jest domyślnie uwzględniony w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="204ab-762">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="204ab-763">Kestrel obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="204ab-763">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="204ab-764">HTTPS</span><span class="sxs-lookup"><span data-stu-id="204ab-764">HTTPS</span></span>
* <span data-ttu-id="204ab-765">Nieprzezroczyste uaktualnienie używane do włączania obsługi obiektów [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="204ab-765">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="204ab-766">Gniazda systemu UNIX w celu zapewnienia wysokiej wydajności w tle Nginx</span><span class="sxs-lookup"><span data-stu-id="204ab-766">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="204ab-767">Kestrel jest obsługiwana na wszystkich platformach i wersjach obsługiwanych przez platformę .NET Core.</span><span class="sxs-lookup"><span data-stu-id="204ab-767">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="204ab-768">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="204ab-768">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="204ab-769">Kiedy używać Kestrel z zwrotnym serwerem proxy</span><span class="sxs-lookup"><span data-stu-id="204ab-769">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="204ab-770">Kestrel może być używana przez siebie lub z *odwrotnym serwerem proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org)lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="204ab-770">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="204ab-771">Odwrotny serwer proxy odbiera żądania HTTP z sieci i przekazuje je do usługi Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-771">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="204ab-772">Kestrel używany jako serwer sieci Web z krawędzią (dostępną z Internetu):</span><span class="sxs-lookup"><span data-stu-id="204ab-772">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem bez serwera proxy zwrotnego](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="204ab-774">Kestrel używany w konfiguracji zwrotnego serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="204ab-774">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel komunikuje się pośrednio z Internetem za pomocą odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="204ab-776">Konfiguracja, z serwerem proxy odwrotnych lub bez niego, jest obsługiwaną konfiguracją hostingu.</span><span class="sxs-lookup"><span data-stu-id="204ab-776">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="204ab-777">Kestrel używany jako serwer graniczny bez serwera proxy odwrotnego nie obsługuje udostępniania tego samego adresu IP i portu między wieloma procesami.</span><span class="sxs-lookup"><span data-stu-id="204ab-777">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="204ab-778">Gdy Kestrel jest skonfigurowany do nasłuchiwania na porcie, Kestrel obsługuje cały ruch dla tego portu bez względu na żądania `Host` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="204ab-778">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="204ab-779">Zwrotny serwer proxy, który może udostępniać porty, ma możliwość przekazywania żądań do Kestrel na unikatowym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="204ab-779">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="204ab-780">Nawet jeśli odwrotny serwer proxy nie jest wymagany, może to być dobry wybór przy użyciu odwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="204ab-780">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="204ab-781">Zwrotny serwer proxy:</span><span class="sxs-lookup"><span data-stu-id="204ab-781">A reverse proxy:</span></span>

* <span data-ttu-id="204ab-782">Może ograniczać uwidoczniony obszar publicznej powierzchni aplikacji, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="204ab-782">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="204ab-783">Zapewnienie dodatkowej warstwy konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="204ab-783">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="204ab-784">Integracja z istniejącą infrastrukturą może być lepsza.</span><span class="sxs-lookup"><span data-stu-id="204ab-784">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="204ab-785">Uprość Równoważenie obciążenia i konfigurację komunikacji zabezpieczonej (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="204ab-785">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="204ab-786">Tylko serwer zwrotny proxy wymaga certyfikatu X. 509, a serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="204ab-786">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="204ab-787">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="204ab-787">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="204ab-788">Jak używać Kestrel w aplikacjach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="204ab-788">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="204ab-789">Pakiet [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="204ab-789">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="204ab-790">Szablony projektów ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-790">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="204ab-791">W *program.cs*kod szablonu wywołuje <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> w tle.</span><span class="sxs-lookup"><span data-stu-id="204ab-791">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="204ab-792">Aby zapewnić dodatkową konfigurację po wywołaniu `CreateDefaultBuilder`, wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span><span class="sxs-lookup"><span data-stu-id="204ab-792">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="204ab-793">Aby uzyskać więcej informacji na temat `CreateDefaultBuilder` i kompilowania hosta, zobacz sekcję *Konfigurowanie hosta* w programie <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="204ab-793">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="kestrel-options"></a><span data-ttu-id="204ab-794">Opcje Kestrel</span><span class="sxs-lookup"><span data-stu-id="204ab-794">Kestrel options</span></span>

<span data-ttu-id="204ab-795">Serwer sieci Web Kestrel ma opcje konfiguracji ograniczeń, które są szczególnie przydatne w przypadku wdrożeń mających dostęp do Internetu.</span><span class="sxs-lookup"><span data-stu-id="204ab-795">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="204ab-796">Ustaw ograniczenia we właściwości <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="204ab-796">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="204ab-797">Właściwość `Limits` przechowuje wystąpienie klasy <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="204ab-797">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="204ab-798">W poniższych przykładach użyto przestrzeni nazw <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="204ab-798">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="204ab-799">Opcje Kestrel, które są konfigurowane w C# kodzie w poniższych przykładach, można również ustawić za pomocą [dostawcy konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="204ab-799">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="204ab-800">Na przykład dostawca konfiguracji plików może załadować konfigurację Kestrel z pliku *appSettings. JSON* lub *appSettings. { Environment} plik JSON* :</span><span class="sxs-lookup"><span data-stu-id="204ab-800">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="204ab-801">Skorzystaj z **jednej** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="204ab-801">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="204ab-802">Skonfiguruj Kestrel w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="204ab-802">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="204ab-803">Wstrzyknąć wystąpienie `IConfiguration` do klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="204ab-803">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="204ab-804">W poniższym przykładzie przyjęto założenie, że wprowadzona konfiguracja jest przypisana do właściwości `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="204ab-804">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="204ab-805">W `Startup.ConfigureServices`Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="204ab-805">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

* <span data-ttu-id="204ab-806">Skonfiguruj Kestrel podczas kompilowania hosta:</span><span class="sxs-lookup"><span data-stu-id="204ab-806">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="204ab-807">W programie *program.cs*Załaduj sekcję `Kestrel` konfiguracji do konfiguracji Kestrel:</span><span class="sxs-lookup"><span data-stu-id="204ab-807">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="204ab-808">Obie powyższe podejścia współpracują z dowolnym [dostawcą konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="204ab-808">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="204ab-809">Limit czasu utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="204ab-809">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="204ab-810">Pobiera lub ustawia [limit czasu utrzymywania aktywności](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="204ab-810">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="204ab-811">Wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="204ab-811">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="204ab-812">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="204ab-812">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="204ab-813">Maksymalna liczba współbieżnych otwartych połączeń TCP można ustawić dla całej aplikacji przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="204ab-813">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="204ab-814">Istnieje oddzielny limit połączeń, które zostały uaktualnione z protokołu HTTP lub HTTPS do innego protokołu (na przykład w żądaniu usługi WebSockets).</span><span class="sxs-lookup"><span data-stu-id="204ab-814">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="204ab-815">Po uaktualnieniu połączenia nie jest ono wliczane do limitu `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="204ab-815">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="204ab-816">Maksymalna liczba połączeń jest domyślnie nieograniczona (null).</span><span class="sxs-lookup"><span data-stu-id="204ab-816">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="204ab-817">Maksymalny rozmiar treści żądania</span><span class="sxs-lookup"><span data-stu-id="204ab-817">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="204ab-818">Domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="204ab-818">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="204ab-819">Zalecanym podejściem do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie atrybutu <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> dla metody akcji:</span><span class="sxs-lookup"><span data-stu-id="204ab-819">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="204ab-820">Oto przykład, który pokazuje, jak skonfigurować ograniczenie dla aplikacji w każdym żądaniu:</span><span class="sxs-lookup"><span data-stu-id="204ab-820">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="204ab-821">Zastąp ustawienie określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="204ab-821">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="204ab-822">Wyjątek jest generowany, jeśli aplikacja skonfiguruje limit żądania po rozpoczęciu uruchamiania aplikacji w celu odczytania żądania.</span><span class="sxs-lookup"><span data-stu-id="204ab-822">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="204ab-823">Istnieje właściwość `IsReadOnly`, która wskazuje, czy właściwość `MaxRequestBodySize` jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="204ab-823">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="204ab-824">Gdy aplikacja jest uruchamiana [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model) [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), limit rozmiaru treści żądania Kestrel jest wyłączony, ponieważ program IIS już ustawia limit.</span><span class="sxs-lookup"><span data-stu-id="204ab-824">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="204ab-825">Minimalna stawka danych treści żądania</span><span class="sxs-lookup"><span data-stu-id="204ab-825">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="204ab-826">Kestrel sprawdza co sekundę, jeśli dane są odbierane z określoną szybkością w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="204ab-826">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="204ab-827">Jeśli szybkość spadnie poniżej wartości minimalnej, upłynął limit czasu połączenia. Okres prolongaty to czas, przez który Kestrel nadaje klientowi zwiększenie jego szybkości wysyłania do minimum; Ta częstotliwość nie jest sprawdzana w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="204ab-827">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="204ab-828">Okres prolongaty pozwala uniknąć porzucania połączeń, które początkowo wysyłają dane z niską szybkością.</span><span class="sxs-lookup"><span data-stu-id="204ab-828">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="204ab-829">Domyślna stawka minimalna to 240 bajtów na sekundę z 5-sekundowym okresem prolongaty.</span><span class="sxs-lookup"><span data-stu-id="204ab-829">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="204ab-830">Minimalna stawka dotyczy także odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="204ab-830">A minimum rate also applies to the response.</span></span> <span data-ttu-id="204ab-831">Kod określający limit żądań i limit odpowiedzi jest taki sam, chyba że ma `RequestBody` lub `Response` w nazwach właściwości i interfejsów.</span><span class="sxs-lookup"><span data-stu-id="204ab-831">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="204ab-832">Oto przykład pokazujący sposób konfigurowania minimalnych stawek danych w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="204ab-832">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

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

### <a name="request-headers-timeout"></a><span data-ttu-id="204ab-833">Limit czasu nagłówków żądań</span><span class="sxs-lookup"><span data-stu-id="204ab-833">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="204ab-834">Pobiera lub ustawia maksymalny czas, przez jaki serwer spędza nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="204ab-834">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="204ab-835">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="204ab-835">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="204ab-836">Synchroniczne we/wy</span><span class="sxs-lookup"><span data-stu-id="204ab-836">Synchronous IO</span></span>

<span data-ttu-id="204ab-837"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> kontroluje, czy synchroniczna operacja we/wy jest dozwolona dla żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="204ab-837"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="204ab-838">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="204ab-838">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="204ab-839">Duża liczba blokowania synchronicznych operacji we/wy może prowadzić do zablokowania puli wątków, co sprawia, że aplikacja nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="204ab-839">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="204ab-840">Włącz `AllowSynchronousIO` tylko w przypadku korzystania z biblioteki, która nie obsługuje asynchronicznych operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="204ab-840">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="204ab-841">Poniższy przykład wyłącza synchroniczną operację we/wy:</span><span class="sxs-lookup"><span data-stu-id="204ab-841">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="204ab-842">Aby uzyskać informacje o innych opcjach i ograniczeniach Kestrel, zobacz:</span><span class="sxs-lookup"><span data-stu-id="204ab-842">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="204ab-843">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="204ab-843">Endpoint configuration</span></span>

<span data-ttu-id="204ab-844">Domyślnie ASP.NET Core wiąże się z:</span><span class="sxs-lookup"><span data-stu-id="204ab-844">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="204ab-845">`https://localhost:5001` (gdy obecny jest lokalny certyfikat programistyczny)</span><span class="sxs-lookup"><span data-stu-id="204ab-845">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="204ab-846">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="204ab-846">Specify URLs using the:</span></span>

* <span data-ttu-id="204ab-847">Zmienna środowiskowa `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="204ab-847">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="204ab-848">`--urls` argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="204ab-848">`--urls` command-line argument.</span></span>
* <span data-ttu-id="204ab-849">klucz konfiguracji hosta `urls`.</span><span class="sxs-lookup"><span data-stu-id="204ab-849">`urls` host configuration key.</span></span>
* <span data-ttu-id="204ab-850">Metoda rozszerzenia `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="204ab-850">`UseUrls` extension method.</span></span>

<span data-ttu-id="204ab-851">Wartość podana przy użyciu tych metod może być jednym lub większą liczbą punktów końcowych HTTP i HTTPS (HTTPS, jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="204ab-851">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="204ab-852">Skonfiguruj wartość jako listę rozdzieloną średnikami (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="204ab-852">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="204ab-853">Aby uzyskać więcej informacji na temat tych metod, zobacz [adresy URL serwera](xref:fundamentals/host/web-host#server-urls) i [Zastąp konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="204ab-853">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="204ab-854">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="204ab-854">A development certificate is created:</span></span>

* <span data-ttu-id="204ab-855">Po zainstalowaniu [zestaw .NET Core SDK](/dotnet/core/sdk) .</span><span class="sxs-lookup"><span data-stu-id="204ab-855">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="204ab-856">Do utworzenia certyfikatu służy [Narzędzie dev-certs](xref:aspnetcore-2.1#https) .</span><span class="sxs-lookup"><span data-stu-id="204ab-856">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="204ab-857">Niektóre przeglądarki wymagają przyznania jawnego uprawnienia do zaufania do lokalnego certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="204ab-857">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="204ab-858">Szablony projektu konfigurują aplikacje do uruchamiania domyślnie przy użyciu protokołu HTTPS i obejmują [przekierowania https i obsługę HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="204ab-858">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="204ab-859">Wywołaj metody <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> lub <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> w <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, aby skonfigurować prefiksy i porty adresów URL dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-859">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="204ab-860">`UseUrls`, `--urls` argument wiersza polecenia, klucz konfiguracji hosta `urls` oraz zmienna środowiskowa `ASPNETCORE_URLS` również działają, ale mają ograniczenia wymienione w dalszej części tej sekcji (certyfikat domyślny musi być dostępny do konfiguracji punktu końcowego HTTPS).</span><span class="sxs-lookup"><span data-stu-id="204ab-860">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="204ab-861">Konfiguracja `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="204ab-861">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="204ab-862">ConfigureEndpointDefaults (Akcja\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="204ab-862">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="204ab-863">Określa `Action` konfiguracji do uruchomienia dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="204ab-863">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="204ab-864">Wywoływanie `ConfigureEndpointDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="204ab-864">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="204ab-865">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="204ab-865">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="204ab-866">ConfigureHttpsDefaults (Akcja\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="204ab-866">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="204ab-867">Określa `Action` konfiguracji do uruchomienia dla każdego punktu końcowego HTTPS.</span><span class="sxs-lookup"><span data-stu-id="204ab-867">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="204ab-868">Wywoływanie `ConfigureHttpsDefaults` wiele razy zastępuje wcześniejsze `Action`s ostatnimi określonymi `Action`.</span><span class="sxs-lookup"><span data-stu-id="204ab-868">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="204ab-869">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="204ab-869">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="204ab-870">Konfiguruj (IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="204ab-870">Configure(IConfiguration)</span></span>

<span data-ttu-id="204ab-871">Tworzy moduł ładujący konfigurację służący do konfigurowania Kestrel, który pobiera <xref:Microsoft.Extensions.Configuration.IConfiguration> jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-871">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="204ab-872">Konfiguracja musi być objęta zakresem sekcji konfiguracji dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-872">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="204ab-873">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="204ab-873">ListenOptions.UseHttps</span></span>

<span data-ttu-id="204ab-874">Skonfiguruj Kestrel do korzystania z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="204ab-874">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="204ab-875">rozszerzenia `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="204ab-875">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="204ab-876">`UseHttps` &ndash; skonfigurować Kestrel do używania protokołu HTTPS z certyfikatem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="204ab-876">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="204ab-877">Zgłasza wyjątek, jeśli nie został skonfigurowany żaden certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="204ab-877">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="204ab-878">`ListenOptions.UseHttps` parametry:</span><span class="sxs-lookup"><span data-stu-id="204ab-878">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="204ab-879">`filename` to ścieżka i nazwa pliku certyfikatu odnosząca się do katalogu zawierającego pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="204ab-879">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="204ab-880">`password` to hasło wymagane do uzyskania dostępu do danych certyfikatu X. 509.</span><span class="sxs-lookup"><span data-stu-id="204ab-880">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="204ab-881">`configureOptions` jest `Action`, aby skonfigurować `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="204ab-881">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="204ab-882">Zwraca `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="204ab-882">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="204ab-883">`storeName` to magazyn certyfikatów, z którego ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="204ab-883">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="204ab-884">`subject` to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="204ab-884">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="204ab-885">`allowInvalid` wskazuje, czy należy wziąć pod uwagę nieprawidłowe certyfikaty, takie jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="204ab-885">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="204ab-886">`location` to lokalizacja magazynu, z której ma zostać załadowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="204ab-886">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="204ab-887">`serverCertificate` to certyfikat X. 509.</span><span class="sxs-lookup"><span data-stu-id="204ab-887">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="204ab-888">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="204ab-888">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="204ab-889">Należy podać co najmniej certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="204ab-889">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="204ab-890">Obsługiwane konfiguracje opisane dalej:</span><span class="sxs-lookup"><span data-stu-id="204ab-890">Supported configurations described next:</span></span>

* <span data-ttu-id="204ab-891">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="204ab-891">No configuration</span></span>
* <span data-ttu-id="204ab-892">Zastąp domyślny certyfikat z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="204ab-892">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="204ab-893">Zmień wartości domyślne w kodzie</span><span class="sxs-lookup"><span data-stu-id="204ab-893">Change the defaults in code</span></span>

<span data-ttu-id="204ab-894">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="204ab-894">*No configuration*</span></span>

<span data-ttu-id="204ab-895">Kestrel nasłuchuje na `http://localhost:5000` i `https://localhost:5001` (jeśli jest dostępny domyślny certyfikat).</span><span class="sxs-lookup"><span data-stu-id="204ab-895">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="204ab-896">*Zastąp domyślny certyfikat z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="204ab-896">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="204ab-897">`CreateDefaultBuilder` wywołania `Configure(context.Configuration.GetSection("Kestrel"))` domyślnie do ładowania konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-897">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="204ab-898">Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępny dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-898">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="204ab-899">Konfigurowanie wielu punktów końcowych, w tym adresów URL i certyfikatów do użycia, z pliku znajdującego się na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="204ab-899">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="204ab-900">W poniższym przykładzie pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="204ab-900">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="204ab-901">Ustaw **AllowInvalid** na `true`, aby zezwolić na korzystanie z nieprawidłowych certyfikatów (na przykład certyfikatów z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="204ab-901">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="204ab-902">Wszystkie punkty końcowe HTTPS, które nie określają certyfikatu (**HttpsDefaultCert** w poniższym przykładzie) powracają do certyfikatu zdefiniowanego w obszarze **Certyfikaty** > **domyślnym** lub certyfikatem deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="204ab-902">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="204ab-903">Alternatywą dla korzystania z **ścieżki** i **hasła** dla dowolnego węzła certyfikatu jest określenie certyfikatu przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="204ab-903">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="204ab-904">Na przykład **certyfikaty** > **domyślnego** certyfikatu można określić jako:</span><span class="sxs-lookup"><span data-stu-id="204ab-904">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="204ab-905">Uwagi dotyczące schematu:</span><span class="sxs-lookup"><span data-stu-id="204ab-905">Schema notes:</span></span>

* <span data-ttu-id="204ab-906">W nazwach punktów końcowych nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="204ab-906">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="204ab-907">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-907">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="204ab-908">Dla każdego punktu końcowego jest wymagany parametr `Url`.</span><span class="sxs-lookup"><span data-stu-id="204ab-908">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="204ab-909">Format tego parametru jest taki sam jak parametr konfiguracji `Urls` najwyższego poziomu, z tą różnicą, że jest ograniczony do pojedynczej wartości.</span><span class="sxs-lookup"><span data-stu-id="204ab-909">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="204ab-910">Te punkty końcowe zastępują te zdefiniowane w konfiguracji `Urls` najwyższego poziomu zamiast dodawać do nich.</span><span class="sxs-lookup"><span data-stu-id="204ab-910">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="204ab-911">Punkty końcowe zdefiniowane w kodzie za pośrednictwem `Listen` kumulują się z punktami końcowymi zdefiniowanymi w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="204ab-911">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="204ab-912">Sekcja `Certificate` jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="204ab-912">The `Certificate` section is optional.</span></span> <span data-ttu-id="204ab-913">Jeśli sekcja `Certificate` nie zostanie określona, są używane wartości domyślne zdefiniowane we wcześniejszych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="204ab-913">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="204ab-914">Jeśli żadne wartości domyślne nie są dostępne, serwer zgłasza wyjątek i nie może się uruchomić.</span><span class="sxs-lookup"><span data-stu-id="204ab-914">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="204ab-915">Sekcja `Certificate` obsługuje zarówno **ścieżkę**&ndash;**hasło** , jak i **temat**&ndash;certyfikatów **magazynu** .</span><span class="sxs-lookup"><span data-stu-id="204ab-915">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="204ab-916">W ten sposób można zdefiniować dowolną liczbę punktów końcowych, dopóki nie spowoduje to konfliktów portów.</span><span class="sxs-lookup"><span data-stu-id="204ab-916">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="204ab-917">`options.Configure(context.Configuration.GetSection("{SECTION}"))` zwraca `KestrelConfigurationLoader` z metodą `.Endpoint(string name, listenOptions => { })`, której można użyć do uzupełnienia ustawień skonfigurowanego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="204ab-917">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="204ab-918">dostęp do `KestrelServerOptions.ConfigurationLoader` można uzyskać, aby kontynuować iterację istniejącego modułu ładującego, takiego jak dostarczony przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="204ab-918">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="204ab-919">Sekcja konfiguracji dla każdego punktu końcowego jest dostępna w opcjach w `Endpoint` metodzie, aby można było odczytać ustawienia niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-919">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="204ab-920">Można załadować wiele konfiguracji, wywołując `options.Configure(context.Configuration.GetSection("{SECTION}"))` ponownie z inną sekcją.</span><span class="sxs-lookup"><span data-stu-id="204ab-920">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="204ab-921">Używana jest tylko Ostatnia konfiguracja, chyba że `Load` jest jawnie wywoływana w poprzednich wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="204ab-921">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="204ab-922">Pakiet nie wywołuje `Load`, tak aby jego sekcja konfiguracji domyślnej mogła zostać zamieniona.</span><span class="sxs-lookup"><span data-stu-id="204ab-922">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="204ab-923">`KestrelConfigurationLoader` odzwierciedla `Listen`ą rodzinę interfejsów API z `KestrelServerOptions` jako przeciążania `Endpoint`, dlatego punkty końcowe kodu i konfiguracji można skonfigurować w tym samym miejscu.</span><span class="sxs-lookup"><span data-stu-id="204ab-923">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="204ab-924">Te przeciążenia nie używają nazw i używają tylko ustawień domyślnych z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="204ab-924">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="204ab-925">*Zmień wartości domyślne w kodzie*</span><span class="sxs-lookup"><span data-stu-id="204ab-925">*Change the defaults in code*</span></span>

<span data-ttu-id="204ab-926">`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` mogą służyć do zmiany domyślnych ustawień dla `ListenOptions` i `HttpsConnectionAdapterOptions`, w tym przesłanianie domyślnego certyfikatu określonego w poprzednim scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="204ab-926">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="204ab-927">przed skonfigurowaniem punktów końcowych należy wywołać `ConfigureEndpointDefaults` i `ConfigureHttpsDefaults`.</span><span class="sxs-lookup"><span data-stu-id="204ab-927">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="204ab-928">*Obsługa usługi Kestrel dla SNI*</span><span class="sxs-lookup"><span data-stu-id="204ab-928">*Kestrel support for SNI*</span></span>

<span data-ttu-id="204ab-929">[Oznaczanie nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do hostowania wielu domen na tym samym adresie IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="204ab-929">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="204ab-930">Aby SNI działał, klient wysyła nazwę hosta dla bezpiecznej sesji do serwera podczas uzgadniania TLS, aby serwer mógł zapewnić prawidłowy certyfikat.</span><span class="sxs-lookup"><span data-stu-id="204ab-930">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="204ab-931">Klient korzysta z dostarczonego certyfikatu w celu zaszyfrowania komunikacji z serwerem podczas bezpiecznej sesji, która następuje po uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="204ab-931">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="204ab-932">Kestrel obsługuje SNI za pośrednictwem wywołania zwrotnego `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="204ab-932">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="204ab-933">Wywołanie zwrotne jest wywoływane jednokrotnie dla każdego połączenia, aby umożliwić aplikacji inspekcję nazwy hosta i wybieranie odpowiedniego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="204ab-933">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="204ab-934">Obsługa SNI wymaga:</span><span class="sxs-lookup"><span data-stu-id="204ab-934">SNI support requires:</span></span>

* <span data-ttu-id="204ab-935">Uruchomione na platformie docelowej `netcoreapp2.1` lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="204ab-935">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="204ab-936">W `net461` lub później wywołanie zwrotne jest wywoływane, ale `name` jest zawsze `null`.</span><span class="sxs-lookup"><span data-stu-id="204ab-936">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="204ab-937">`name` jest również `null`, jeśli klient nie poda parametru nazwy hosta w uzgadnianiu protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="204ab-937">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="204ab-938">Wszystkie witryny sieci Web działają na tym samym wystąpieniu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-938">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="204ab-939">Kestrel nie obsługuje udostępniania adresu IP i portu w wielu wystąpieniach bez zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="204ab-939">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="204ab-940">Rejestrowanie połączeń</span><span class="sxs-lookup"><span data-stu-id="204ab-940">Connection logging</span></span>

<span data-ttu-id="204ab-941">Wywołaj <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>, aby emitować dzienniki poziomu debugowania dla komunikacji na poziomie bajtów w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="204ab-941">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="204ab-942">Rejestrowanie połączeń ułatwia rozwiązywanie problemów z komunikacją na niskim poziomie, na przykład podczas szyfrowania TLS i za serwerami proxy.</span><span class="sxs-lookup"><span data-stu-id="204ab-942">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="204ab-943">Jeśli `UseConnectionLogging` jest umieszczony przed `UseHttps`, szyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="204ab-943">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="204ab-944">Jeśli `UseConnectionLogging` jest umieszczony po `UseHttps`, odszyfrowany ruch jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="204ab-944">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="204ab-945">Powiąż z gniazdem TCP</span><span class="sxs-lookup"><span data-stu-id="204ab-945">Bind to a TCP socket</span></span>

<span data-ttu-id="204ab-946">Metoda <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> wiąże się z gniazdem TCP, a opcja lambda zezwala na konfigurację certyfikatu X. 509:</span><span class="sxs-lookup"><span data-stu-id="204ab-946">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

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

<span data-ttu-id="204ab-947">Przykład konfiguruje HTTPS dla punktu końcowego z <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="204ab-947">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="204ab-948">Użyj tego samego interfejsu API, aby skonfigurować inne ustawienia Kestrel dla określonych punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="204ab-948">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="204ab-949">Powiąż z gniazdem systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="204ab-949">Bind to a Unix socket</span></span>

<span data-ttu-id="204ab-950">Nasłuchiwanie w gnieździe systemu UNIX za pomocą <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, aby zwiększyć wydajność za pomocą Nginx, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="204ab-950">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

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

### <a name="port-0"></a><span data-ttu-id="204ab-951">Port 0</span><span class="sxs-lookup"><span data-stu-id="204ab-951">Port 0</span></span>

<span data-ttu-id="204ab-952">Gdy `0` jest określony numer portu, Kestrel dynamicznie wiąże się z dostępnym portem.</span><span class="sxs-lookup"><span data-stu-id="204ab-952">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="204ab-953">W poniższym przykładzie pokazano, jak określić, który port Kestrel faktycznie powiązany w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="204ab-953">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="204ab-954">Po uruchomieniu aplikacji dane wyjściowe okna konsoli wskazują port dynamiczny, w którym można uzyskać dostęp do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="204ab-954">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="204ab-955">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="204ab-955">Limitations</span></span>

<span data-ttu-id="204ab-956">Skonfiguruj punkty końcowe przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="204ab-956">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="204ab-957">`--urls` argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="204ab-957">`--urls` command-line argument</span></span>
* <span data-ttu-id="204ab-958">klucz konfiguracji hosta `urls`</span><span class="sxs-lookup"><span data-stu-id="204ab-958">`urls` host configuration key</span></span>
* <span data-ttu-id="204ab-959">Zmienna środowiskowa `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="204ab-959">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="204ab-960">Te metody są przydatne do tworzenia kodu w pracy z serwerami innymi niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="204ab-960">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="204ab-961">Należy jednak pamiętać o następujących ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="204ab-961">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="204ab-962">Nie można użyć protokołu HTTPS z tymi metodami, chyba że w konfiguracji punktu końcowego HTTPS jest podany certyfikat domyślny (na przykład za pomocą konfiguracji `KestrelServerOptions` lub pliku konfiguracji, jak pokazano wcześniej w tym temacie).</span><span class="sxs-lookup"><span data-stu-id="204ab-962">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="204ab-963">Gdy oba podejścia `Listen` i `UseUrls` są używane jednocześnie, `Listen` punkty końcowe przesłaniają `UseUrls` punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-963">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="204ab-964">Konfiguracja punktu końcowego usług IIS</span><span class="sxs-lookup"><span data-stu-id="204ab-964">IIS endpoint configuration</span></span>

<span data-ttu-id="204ab-965">W przypadku korzystania z usług IIS powiązania URL dla powiązań przesłonięć usług IIS są ustawiane przez `Listen` lub `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="204ab-965">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="204ab-966">Aby uzyskać więcej informacji, zobacz temat [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="204ab-966">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="204ab-967">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="204ab-967">Transport configuration</span></span>

<span data-ttu-id="204ab-968">W wersji platformy ASP.NET Core 2.1 transport domyślny serwera Kestrel nie jest już oparty na bibliotece libuv, ale na zarządzanych gniazdach.</span><span class="sxs-lookup"><span data-stu-id="204ab-968">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="204ab-969">Jest to istotna zmiana dla aplikacji ASP.NET Core 2,0 uaktualniana do 2,1, które wywołują <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> i zależą od jednego z następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="204ab-969">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="204ab-970">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (bezpośrednie odwołanie do pakietu)</span><span class="sxs-lookup"><span data-stu-id="204ab-970">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="204ab-971">Microsoft. AspNetCore. App</span><span class="sxs-lookup"><span data-stu-id="204ab-971">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="204ab-972">Dla projektów, które wymagają użycia Libuv:</span><span class="sxs-lookup"><span data-stu-id="204ab-972">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="204ab-973">Dodaj zależność dla pakietu [Microsoft. AspNetCore. Server. Kestrel. transport. Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="204ab-973">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="204ab-974"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>wywołania:</span><span class="sxs-lookup"><span data-stu-id="204ab-974">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="204ab-975">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="204ab-975">URL prefixes</span></span>

<span data-ttu-id="204ab-976">W przypadku korzystania z `UseUrls`, `--urls` argumentu wiersza polecenia, klucza konfiguracji hosta `urls` lub zmiennej środowiskowej `ASPNETCORE_URLS`, prefiksy adresów URL mogą mieć jeden z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="204ab-976">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="204ab-977">Tylko prefiksy adresów URL HTTP są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="204ab-977">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="204ab-978">Kestrel nie obsługuje protokołu HTTPS podczas konfigurowania powiązań adresów URL przy użyciu `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="204ab-978">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="204ab-979">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="204ab-979">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="204ab-980">`0.0.0.0` jest szczególnym przypadkiem, który wiąże się ze wszystkimi adresami IPv4.</span><span class="sxs-lookup"><span data-stu-id="204ab-980">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="204ab-981">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="204ab-981">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="204ab-982">`[::]` to odpowiednik IPv6 `0.0.0.0`IPv4.</span><span class="sxs-lookup"><span data-stu-id="204ab-982">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="204ab-983">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="204ab-983">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="204ab-984">Nazwy hostów, `*`i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="204ab-984">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="204ab-985">Wszystkie nie są rozpoznawane jako prawidłowy adres IP lub `localhost` są powiązane z protokołem IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="204ab-985">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="204ab-986">Aby powiązać różne nazwy hostów z różnymi ASP.NET Core aplikacjami na tym samym porcie, użyj [protokołu HTTP. sys](xref:fundamentals/servers/httpsys) lub odwrotnego serwera proxy, takiego jak IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="204ab-986">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="204ab-987">Hostowanie w konfiguracji zwrotnego serwera proxy wymaga [filtrowania hosta](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="204ab-987">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="204ab-988">Nazwa hosta `localhost` z numerem portu lub adresem IP sprzężenia zwrotnego z numerem portu</span><span class="sxs-lookup"><span data-stu-id="204ab-988">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="204ab-989">Gdy `localhost` jest określony, Kestrel próbuje powiązać z interfejsami sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="204ab-989">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="204ab-990">Jeśli żądany port jest używany przez inną usługę w dowolnym interfejsie sprzężenia zwrotnego, uruchomienie Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="204ab-990">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="204ab-991">Jeśli którykolwiek z innych przyczyn interfejsu sprzężenia zwrotnego jest niedostępny (najczęściej jest to spowodowane tym, że protokół IPv6 nie jest obsługiwany), Kestrel rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="204ab-991">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="204ab-992">Filtrowanie hostów</span><span class="sxs-lookup"><span data-stu-id="204ab-992">Host filtering</span></span>

<span data-ttu-id="204ab-993">Chociaż usługa Kestrel obsługuje konfigurację na podstawie prefiksów, takich jak `http://example.com:5000`, Kestrel w znacznym stopniu ignoruje nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="204ab-993">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="204ab-994">`localhost` hosta jest specjalnym przypadkiem używanym do tworzenia powiązań z adresami sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="204ab-994">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="204ab-995">Każdy host poza jawnym adresem IP tworzy powiązanie ze wszystkimi publicznymi adresami IP.</span><span class="sxs-lookup"><span data-stu-id="204ab-995">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="204ab-996">nagłówki `Host` nie są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="204ab-996">`Host` headers aren't validated.</span></span>

<span data-ttu-id="204ab-997">Aby obejść ten sposób, użyj oprogramowania pośredniczącego filtrowania hosta.</span><span class="sxs-lookup"><span data-stu-id="204ab-997">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="204ab-998">Oprogramowanie pośredniczące do filtrowania hosta jest dostarczane przez pakiet [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , który jest zawarty w pakiecie [Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub 2,2).</span><span class="sxs-lookup"><span data-stu-id="204ab-998">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="204ab-999">Oprogramowanie pośredniczące jest dodawane przez <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, które wywołuje <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="204ab-999">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="204ab-1000">Programowe filtrowanie hosta jest domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="204ab-1000">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="204ab-1001">Aby włączyć oprogramowanie pośredniczące, zdefiniuj klucz `AllowedHosts` w pliku *appSettings. json*/*appSettings.\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="204ab-1001">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="204ab-1002">Wartość to rozdzielana średnikami lista nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="204ab-1002">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="204ab-1003">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="204ab-1003">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="204ab-1004">W przypadku [przekierowanych nagłówków oprogramowanie pośredniczące](xref:host-and-deploy/proxy-load-balancer) ma również opcję <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="204ab-1004">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="204ab-1005">Przekazane nagłówki — oprogramowanie pośredniczące i filtrowanie hostów oprogramowanie pośredniczące ma podobną funkcjonalność dla różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="204ab-1005">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="204ab-1006">Ustawienie `AllowedHosts` z przekierowanymi nagłówkami — oprogramowanie pośredniczące jest odpowiednie, gdy nagłówek `Host` nie jest zachowywany podczas przekazywania żądań z odwrotnym serwerem proxy lub modułem równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="204ab-1006">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="204ab-1007">Ustawienie `AllowedHosts` przy użyciu oprogramowania pośredniczącego filtrowania hosta jest odpowiednie, gdy Kestrel jest używany jako publiczny serwer graniczny lub gdy `Host` nagłówek jest bezpośrednio przekazywany.</span><span class="sxs-lookup"><span data-stu-id="204ab-1007">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="204ab-1008">Aby uzyskać więcej informacji na temat przekierowanych nagłówków, należy zapoznać się z tematem <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="204ab-1008">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="204ab-1009">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="204ab-1009">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="204ab-1010">RFC 7230: Składnia i Routing komunikatu (sekcja 5,4: Host)</span><span class="sxs-lookup"><span data-stu-id="204ab-1010">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
