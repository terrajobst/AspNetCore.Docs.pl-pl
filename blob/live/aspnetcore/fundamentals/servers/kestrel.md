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
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="3c523-104">Wprowadzenie do Kestrel implementacja serwera sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c523-104">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="3c523-105">Przez [Dykstra Tomasz](https://github.com/tdykstra), [Roaming Krzysztof](https://github.com/Tratcher), i [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="3c523-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="3c523-106">Kestrel się między platformami [serwera sieci web dla platformy ASP.NET Core](index.md) na podstawie [libuv](https://github.com/libuv/libuv), biblioteki i platform asynchroniczne We/Wy.</span><span class="sxs-lookup"><span data-stu-id="3c523-106">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="3c523-107">Kestrel to serwer sieci web, który jest domyślnie włączone w szablony projektów platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c523-107">Kestrel is the web server that is included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="3c523-108">Kestrel obsługuje następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="3c523-108">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="3c523-109">HTTPS</span><span class="sxs-lookup"><span data-stu-id="3c523-109">HTTPS</span></span>
  * <span data-ttu-id="3c523-110">Uaktualnienie nieprzezroczyste używane do włączania [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="3c523-110">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="3c523-111">Gniazda UNIX o wysokiej wydajności za Nginx</span><span class="sxs-lookup"><span data-stu-id="3c523-111">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="3c523-112">Kestrel jest obsługiwana na wszystkich platformach i wersje, które obsługuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c523-112">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3c523-113">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3c523-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3c523-114">[Wyświetlić lub pobrać przykładowy kod 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3c523-114">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3c523-115">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3c523-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3c523-116">[Wyświetlić lub pobrać przykładowy kod 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3c523-116">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="3c523-117">Kiedy należy używać Kestrel z zwrotny serwer proxy</span><span class="sxs-lookup"><span data-stu-id="3c523-117">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3c523-118">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3c523-118">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3c523-119">Samodzielnie lub z użyciem Kestrel *zwrotnego serwera proxy*, takie jak usługi IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="3c523-119">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="3c523-120">Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi.</span><span class="sxs-lookup"><span data-stu-id="3c523-120">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3c523-123">Albo konfiguracji &mdash; z lub bez zwrotnego serwera proxy &mdash; można również, czy Kestrel jest udostępniany tylko z siecią wewnętrzną.</span><span class="sxs-lookup"><span data-stu-id="3c523-123">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3c523-124">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3c523-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3c523-125">Jeśli aplikacja akceptuje żądania tylko z siecią wewnętrzną, można użyć Kestrel przez samego siebie.</span><span class="sxs-lookup"><span data-stu-id="3c523-125">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel komunikuje się bezpośrednio z sieci wewnętrznej](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="3c523-127">Jeśli udostępnianie aplikacji z Internetem, należy użyć usług IIS, Nginx lub Apache jako *zwrotnego serwera proxy*.</span><span class="sxs-lookup"><span data-stu-id="3c523-127">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="3c523-128">Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi.</span><span class="sxs-lookup"><span data-stu-id="3c523-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3c523-130">Zwrotny serwer proxy jest wymagany w przypadku wdrożeń krawędzi (ujawniony na ruch z Internetu) ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="3c523-130">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="3c523-131">Wersje 1.x Kestrel nie mają pełny zestaw zabezpieczenia przed atakami.</span><span class="sxs-lookup"><span data-stu-id="3c523-131">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="3c523-132">Obejmuje, ale nie jest ograniczony do odpowiednich limitów czasu, limity rozmiaru i limity liczby jednoczesnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="3c523-132">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="3c523-133">Scenariusz, który wymaga zwrotny serwer proxy jest w przypadku wielu zastosowań, które korzysta z tego samego adresu IP i port uruchomione na jednym serwerze.</span><span class="sxs-lookup"><span data-stu-id="3c523-133">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="3c523-134">To nie zadziała z Kestrel bezpośrednio ponieważ Kestrel nie obsługuje udostępniania tego samego adresu IP i portu między wiele procesów.</span><span class="sxs-lookup"><span data-stu-id="3c523-134">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="3c523-135">Po skonfigurowaniu Kestrel do nasłuchiwania na porcie obsługuje cały ruch do tego portu, niezależnie od tego nagłówka hosta.</span><span class="sxs-lookup"><span data-stu-id="3c523-135">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="3c523-136">Zwrotny serwer proxy, który można udostępniać porty następnie musi przekazywać Kestrel na unikatowy adresu IP i portu.</span><span class="sxs-lookup"><span data-stu-id="3c523-136">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="3c523-137">Nawet jeśli zwrotnego serwera proxy nie jest wymagane, przy użyciu jednej może być dobrym rozwiązaniem z innych powodów:</span><span class="sxs-lookup"><span data-stu-id="3c523-137">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="3c523-138">Można go ograniczyć dostępnego obszaru powierzchni.</span><span class="sxs-lookup"><span data-stu-id="3c523-138">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="3c523-139">Zapewnia dodatkową warstwę opcjonalne konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="3c523-139">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="3c523-140">Może ją zintegrować lepiej z istniejącej infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="3c523-140">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="3c523-141">Takie rozwiązanie upraszcza równoważenia obciążenia i Konfiguracja protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="3c523-141">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="3c523-142">Tylko zwrotnego serwera proxy wymaga certyfikatu SSL i że serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3c523-142">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="3c523-143">Jak używać Kestrel w aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c523-143">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3c523-144">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3c523-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3c523-145">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pakietu znajduje się w [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="3c523-145">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="3c523-146">Szablony projektów platformy ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3c523-146">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="3c523-147">W *Program.cs*, wywołań kodu szablonów `CreateDefaultBuilder`, które wywołuje [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) w tle.</span><span class="sxs-lookup"><span data-stu-id="3c523-147">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="3c523-148">Aby skonfigurować opcje Kestrel należy wywołać `UseKestrel` w *Program.cs* jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="3c523-148">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3c523-149">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3c523-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3c523-150">Zainstaluj [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="3c523-150">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="3c523-151">Wywołanie [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) — metoda rozszerzenia na `WebHostBuilder` w Twojej `Main` metoda określania żadnego [opcje Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) potrzebne, jak pokazano w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3c523-151">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="3c523-152">Opcje kestrel</span><span class="sxs-lookup"><span data-stu-id="3c523-152">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3c523-153">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3c523-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3c523-154">Serwer sieci web Kestrel ma ograniczenie opcje konfiguracji, które są szczególnie przydatne w przypadku wdrożeń skierowane do Internetu.</span><span class="sxs-lookup"><span data-stu-id="3c523-154">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="3c523-155">Oto niektóre ograniczenia, które można ustawić:</span><span class="sxs-lookup"><span data-stu-id="3c523-155">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="3c523-156">Maksymalna liczba połączeń klientów</span><span class="sxs-lookup"><span data-stu-id="3c523-156">Maximum client connections</span></span>
- <span data-ttu-id="3c523-157">Żądanie maksymalny rozmiar treści</span><span class="sxs-lookup"><span data-stu-id="3c523-157">Maximum request body size</span></span>
- <span data-ttu-id="3c523-158">Szybkość danych treści żądania minimalna</span><span class="sxs-lookup"><span data-stu-id="3c523-158">Minimum request body data rate</span></span>

<span data-ttu-id="3c523-159">Ustaw tych warunków ograniczających i innych reguł w `Limits` właściwość [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) klasy.</span><span class="sxs-lookup"><span data-stu-id="3c523-159">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="3c523-160">`Limits` Właściwość przechowuje wystąpienia [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) klasy.</span><span class="sxs-lookup"><span data-stu-id="3c523-160">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="3c523-161">**Maksymalna liczba połączeń klientów**</span><span class="sxs-lookup"><span data-stu-id="3c523-161">**Maximum client connections**</span></span>

<span data-ttu-id="3c523-162">Można ustawić maksymalną liczbę równoczesnych otwartych połączeń TCP dla całej aplikacji z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="3c523-162">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="3c523-163">Brak oddzielnych limit połączeń, które zostały uaktualnione do inny protokół (na przykład na żądanie Websocket) z HTTP lub HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3c523-163">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="3c523-164">Po uaktualnieniu połączenie nie jest uwzględniane `MaxConcurrentConnections` limit.</span><span class="sxs-lookup"><span data-stu-id="3c523-164">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="3c523-165">Maksymalna liczba połączeń jest nieograniczony (null) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="3c523-165">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="3c523-166">**Żądanie maksymalny rozmiar treści**</span><span class="sxs-lookup"><span data-stu-id="3c523-166">**Maximum request body size**</span></span>

<span data-ttu-id="3c523-167">Domyślny rozmiar treści żądania maksymalna jest 30,000,000 bajtów, czyli około 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="3c523-167">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="3c523-168">Zalecanym sposobem zastąpienie limit w aplikacji platformy ASP.NET Core MVC jest użycie [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atrybutu metody akcji:</span><span class="sxs-lookup"><span data-stu-id="3c523-168">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="3c523-169">Oto przykład, który pokazuje, jak skonfigurować ograniczenia dla całej aplikacji, każde żądanie:</span><span class="sxs-lookup"><span data-stu-id="3c523-169">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="3c523-170">Można zastąpić ustawienie dla określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="3c523-170">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="3c523-171">Jeśli próbujesz skonfigurować limit na żądanie, po uruchomieniu aplikacji odczytu żądania, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="3c523-171">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="3c523-172">Brak `IsReadOnly` właściwość, która informuje, jeśli `MaxRequestBodySize` właściwość jest w stanie tylko do odczytu, co oznacza jest za późno skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="3c523-172">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="3c523-173">**Szybkość danych treści żądania minimalna**</span><span class="sxs-lookup"><span data-stu-id="3c523-173">**Minimum request body data rate**</span></span>

<span data-ttu-id="3c523-174">Kestrel sprawdza co sekundę, gdy dane pochodzi szybkością określony w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="3c523-174">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="3c523-175">Jeżeli stawki spadnie poniżej wartości minimalnej, jest upłynął limit czasu połączenia. Okres prolongaty wynosi ilość czasu, że Kestrel daje klientowi zwiększyć jego częstotliwość wysyłania do minimum; w tym czasie nie zaznaczono stawki.</span><span class="sxs-lookup"><span data-stu-id="3c523-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate is not checked during that time.</span></span> <span data-ttu-id="3c523-176">Okres prolongaty pomaga uniknąć porzucenie połączeń, które początkowo wysyłania danych prędkością wolne ze względu na wolne TCP-start.</span><span class="sxs-lookup"><span data-stu-id="3c523-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="3c523-177">Minimalna szybkość domyślna jest 240 bajtów na sekundę, z okresu prolongaty 5-sekundowego.</span><span class="sxs-lookup"><span data-stu-id="3c523-177">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="3c523-178">Minimalna częstotliwość ma również zastosowanie do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3c523-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="3c523-179">Kod, aby ustawić limit żądań i odpowiedzi limit jest taki sam, z wyjątkiem o `RequestBody` lub `Response` w nazwach właściwości i interfejsu.</span><span class="sxs-lookup"><span data-stu-id="3c523-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="3c523-180">Oto przykład pokazujący sposób konfigurowania stawki minimalną ilość danych w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c523-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="3c523-181">Stawki na żądanie można skonfigurować w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="3c523-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="3c523-182">Aby uzyskać informacje o innych opcjach Kestrel zobacz następujące klasy:</span><span class="sxs-lookup"><span data-stu-id="3c523-182">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="3c523-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="3c523-183">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="3c523-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="3c523-184">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="3c523-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="3c523-185">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3c523-186">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3c523-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3c523-187">Aby uzyskać informacje o opcjach Kestrel, zobacz [KestrelServerOptions klasy](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="3c523-187">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="3c523-188">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="3c523-188">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3c523-189">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3c523-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3c523-190">Domyślnie wiąże platformy ASP.NET Core `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="3c523-190">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="3c523-191">Skonfiguruj prefiksy URL i portów dla Kestrel do nasłuchiwania przez wywołanie metody `Listen` lub `ListenUnixSocket` metody `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="3c523-191">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="3c523-192">(`UseUrls`, `urls` argumentu wiersza polecenia i zmienną środowiskową ASPNETCORE_URLS również służbowych, ale ma ograniczenia zauważyć [dalszej części tego artykułu](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="3c523-192">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="3c523-193">**Powiązać gniazda TCP**</span><span class="sxs-lookup"><span data-stu-id="3c523-193">**Bind to a TCP socket**</span></span>

<span data-ttu-id="3c523-194">`Listen` Metody wiąże gniazda TCP i lambda opcje można skonfigurować certyfikat SSL:</span><span class="sxs-lookup"><span data-stu-id="3c523-194">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="3c523-195">Zwróć uwagę, jak ten przykład konfiguruje SSL dla danego punktu końcowego za pomocą [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="3c523-195">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="3c523-196">Skonfigurować inne ustawienia Kestrel dla konkretnego punktów końcowych, można użyć tego samego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="3c523-196">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="3c523-197">**Powiązany z gniazdem systemu Unix**</span><span class="sxs-lookup"><span data-stu-id="3c523-197">**Bind to a Unix socket**</span></span>

<span data-ttu-id="3c523-198">Jak pokazano w tym przykładzie można nasłuchiwać gniazda Unix zwiększonej wydajności z Nginx:</span><span class="sxs-lookup"><span data-stu-id="3c523-198">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="3c523-199">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="3c523-199">**Port 0**</span></span>

<span data-ttu-id="3c523-200">Jeśli określisz numeru portu 0 Kestrel wiąże dynamicznie dostępnego portu.</span><span class="sxs-lookup"><span data-stu-id="3c523-200">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="3c523-201">Poniższy przykład przedstawia sposób określić port, który faktycznie powiązany Kestrel w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="3c523-201">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="3c523-202">**Ograniczenia UseUrls**</span><span class="sxs-lookup"><span data-stu-id="3c523-202">**UseUrls limitations**</span></span>

<span data-ttu-id="3c523-203">Punkty końcowe można skonfigurować, wywołując `UseUrls` metody lub używając `urls` argumentu wiersza polecenia lub zmiennej środowiskowej ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="3c523-203">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="3c523-204">Te metody są przydatne w przypadku kodu do pracy z serwerami innych niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3c523-204">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="3c523-205">Jednak należy pamiętać o tych ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="3c523-205">However, be aware of these limitations:</span></span>

* <span data-ttu-id="3c523-206">Nie można użyć protokołu SSL przy użyciu tych metod.</span><span class="sxs-lookup"><span data-stu-id="3c523-206">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="3c523-207">Jeśli używasz zarówno `Listen` — metoda i `UseUrls`, `Listen` zastąpienie punktów końcowych `UseUrls` punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="3c523-207">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="3c523-208">**Konfiguracja punktu końcowego dla usług IIS**</span><span class="sxs-lookup"><span data-stu-id="3c523-208">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="3c523-209">Jeśli używasz usług IIS, powiązania adres URL dla usług IIS zastąpienie powiązań ustawionych przez wywołanie albo `Listen` lub `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3c523-209">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="3c523-210">Aby uzyskać więcej informacji, zobacz [wprowadzenie do platformy ASP.NET Core modułu](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="3c523-210">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3c523-211">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3c523-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3c523-212">Domyślnie wiąże platformy ASP.NET Core `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="3c523-212">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="3c523-213">Istnieje możliwość skonfigurowania prefiksy URL i portów dla Kestrel nasłuchiwać przy użyciu `UseUrls` — metoda rozszerzenia, `urls` argumentu wiersza polecenia lub systemu konfiguracji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c523-213">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="3c523-214">Aby uzyskać więcej informacji na temat tych metod, zobacz [hostingu](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="3c523-214">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="3c523-215">Aby uzyskać informacji na temat działania powiązanie adresu URL podczas używania serwera IIS jako zwrotny serwer proxy, zobacz [platformy ASP.NET Core modułu](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="3c523-215">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="3c523-216">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="3c523-216">URL prefixes</span></span>

<span data-ttu-id="3c523-217">Jeśli należy wywołać `UseUrls` lub użyj `urls` argumentu wiersza polecenia lub zmiennej środowiskowej ASPNETCORE_URLS prefiksy URL może być w dowolnym z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="3c523-217">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3c523-218">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3c523-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3c523-219">Tylko prefiksy HTTP URL są ważne. Kestrel nie obsługuje protokołu SSL podczas konfigurowania powiązania adresu URL za pomocą `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3c523-219">Only HTTP URL prefixes are valid; Kestrel does not support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="3c523-220">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="3c523-220">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="3c523-221">0.0.0.0 jest szczególnych przypadkach, która jest powiązana z wszystkich adresów IPv4.</span><span class="sxs-lookup"><span data-stu-id="3c523-221">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="3c523-222">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="3c523-222">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="3c523-223">[:] jest odpowiednikiem IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="3c523-223">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="3c523-224">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="3c523-224">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="3c523-225">Host nazwy, *, a + nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="3c523-225">Host names, *, and +, are not special.</span></span> <span data-ttu-id="3c523-226">Wszystko, co nie jest rozpoznany adres IP lub "localhost" będzie powiązać wszystkich protokołów IPv4 i IPv6, adresy IP.</span><span class="sxs-lookup"><span data-stu-id="3c523-226">Anything that is not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="3c523-227">Aby powiązać różne nazwy hostów z różnych aplikacji platformy ASP.NET Core w tym samym porcie, należy użyć [HTTP.sys](httpsys.md) lub zwrotnego serwera proxy usług IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="3c523-227">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="3c523-228">Nazwa "Localhost" port numer lub sprzężenia zwrotnego protokołu IP z numerem portu</span><span class="sxs-lookup"><span data-stu-id="3c523-228">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="3c523-229">Gdy `localhost` określono Kestrel próbuje powiązać do interfejsu sprzężenia zwrotnego protokołów IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="3c523-229">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="3c523-230">Jeśli żądany port jest używany przez inną usługę albo interfejsu sprzężenia zwrotnego, Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="3c523-230">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="3c523-231">Jeśli z jakiegokolwiek powodu albo interfejsu sprzężenia zwrotnego jest niedostępny (większość często, ponieważ protokół IPv6 nie jest obsługiwany.), Kestrel dzienniki ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="3c523-231">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3c523-232">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3c523-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="3c523-233">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="3c523-233">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="3c523-234">0.0.0.0 jest szczególnych przypadkach, która jest powiązana z wszystkich adresów IPv4.</span><span class="sxs-lookup"><span data-stu-id="3c523-234">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="3c523-235">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="3c523-235">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="3c523-236">[:] jest odpowiednikiem IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="3c523-236">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="3c523-237">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="3c523-237">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="3c523-238">Nazwy, hostów \*, i + nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="3c523-238">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="3c523-239">Wszystko, co nie jest rozpoznany adres IP lub "localhost" wiąże się do wszystkich protokołów IPv4 i IPv6, adresy IP.</span><span class="sxs-lookup"><span data-stu-id="3c523-239">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="3c523-240">Aby powiązać różne nazwy hostów z różnych aplikacji platformy ASP.NET Core w tym samym porcie, należy użyć [WebListener](weblistener.md) lub zwrotnego serwera proxy usług IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="3c523-240">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="3c523-241">Nazwa "Localhost" port numer lub sprzężenia zwrotnego protokołu IP z numerem portu</span><span class="sxs-lookup"><span data-stu-id="3c523-241">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="3c523-242">Gdy `localhost` określono Kestrel próbuje powiązać do interfejsu sprzężenia zwrotnego protokołów IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="3c523-242">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="3c523-243">Jeśli żądany port jest używany przez inną usługę albo interfejsu sprzężenia zwrotnego, Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="3c523-243">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="3c523-244">Jeśli z jakiegokolwiek powodu albo interfejsu sprzężenia zwrotnego jest niedostępny (większość często, ponieważ protokół IPv6 nie jest obsługiwany.), Kestrel dzienniki ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="3c523-244">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="3c523-245">Gniazda systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="3c523-245">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="3c523-246">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="3c523-246">**Port 0**</span></span>

<span data-ttu-id="3c523-247">Jeśli określisz numeru portu 0 Kestrel wiąże dynamicznie dostępnego portu.</span><span class="sxs-lookup"><span data-stu-id="3c523-247">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="3c523-248">Powiązanie z portu 0 jest dozwolony dla dowolnej nazwy hosta lub adres IP, z wyjątkiem `localhost` nazwy.</span><span class="sxs-lookup"><span data-stu-id="3c523-248">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="3c523-249">Poniższy przykład przedstawia sposób określić port, który faktycznie powiązany Kestrel w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="3c523-249">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="3c523-250">**Prefiksy adresów URL dla protokołu SSL**</span><span class="sxs-lookup"><span data-stu-id="3c523-250">**URL prefixes for SSL**</span></span>

<span data-ttu-id="3c523-251">Należy uwzględnić prefiksy URL z `https:` połączeń `UseHttps` — metoda rozszerzenia, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="3c523-251">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="3c523-252">HTTPS i HTTP nie może być umieszczona w tym samym porcie.</span><span class="sxs-lookup"><span data-stu-id="3c523-252">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="3c523-253">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="3c523-253">Next steps</span></span>

<span data-ttu-id="3c523-254">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="3c523-254">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3c523-255">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3c523-255">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="3c523-256">Przykładowa aplikacja dla 2.x</span><span class="sxs-lookup"><span data-stu-id="3c523-256">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="3c523-257">Kod źródłowy kestrel</span><span class="sxs-lookup"><span data-stu-id="3c523-257">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3c523-258">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3c523-258">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="3c523-259">Przykładowa aplikacja dla 1.x</span><span class="sxs-lookup"><span data-stu-id="3c523-259">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="3c523-260">Kod źródłowy kestrel</span><span class="sxs-lookup"><span data-stu-id="3c523-260">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
