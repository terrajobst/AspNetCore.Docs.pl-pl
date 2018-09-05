---
title: Implementacja serwera sieci web kestrel w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat Kestrel, serwer sieci web dla wielu platform dla platformy ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: a32b6e047ed0c2295f9f0bd95ffac010a7570ca5
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336136"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="1d385-103">Implementacja serwera sieci web kestrel w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1d385-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="1d385-104">Przez [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), i [Halter Autor: Stephen](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="1d385-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="1d385-105">Kestrel jest dla wielu platform [serwera sieci web dla platformy ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="1d385-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="1d385-106">Kestrel jest serwer sieci web, który znajduje się domyślnie w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1d385-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="1d385-107">Kestrel obsługuje następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="1d385-107">Kestrel supports the following features:</span></span>

* <span data-ttu-id="1d385-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="1d385-108">HTTPS</span></span>
* <span data-ttu-id="1d385-109">Nieprzezroczysty uaktualnienia, które umożliwiają [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="1d385-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="1d385-110">Gniazda systemu UNIX, dla wysoko wydajnych za serwera Nginx</span><span class="sxs-lookup"><span data-stu-id="1d385-110">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="1d385-111">Kestrel jest obsługiwane na wszystkich platformach i wersjach, które obsługuje platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1d385-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="1d385-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1d385-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="1d385-113">Kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="1d385-113">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d385-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d385-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1d385-115">Można użyć Kestrel, samodzielnie lub z *zwrotnego serwera proxy*, na przykład serwer Nginx, Apache lub IIS.</span><span class="sxs-lookup"><span data-stu-id="1d385-115">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="1d385-116">Zwrotnego serwera proxy odbiera żądania HTTP z Internetu i przekazuje je do Kestrel po niektórych wstępnego obsługi.</span><span class="sxs-lookup"><span data-stu-id="1d385-116">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotny serwer proxy, na przykład serwer Nginx, Apache lub IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="1d385-119">Każda konfiguracja&mdash;z lub bez serwera proxy odwrotnej&mdash;jest prawidłowy i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core 2.0 lub nowszej aplikacje.</span><span class="sxs-lookup"><span data-stu-id="1d385-119">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d385-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d385-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1d385-121">Jeśli aplikacja akceptuje żądania tylko z sieci wewnętrznej, Kestrel używać bezpośrednio jako serwer aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1d385-121">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![Kestrel komunikuje się bezpośrednio z sieci wewnętrznej](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="1d385-123">Jeśli udostępnisz aplikację, aby przez Internet, użyj usług IIS, serwera Nginx lub Apache jako *zwrotnego serwera proxy*.</span><span class="sxs-lookup"><span data-stu-id="1d385-123">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="1d385-124">Zwrotnego serwera proxy odbiera żądania HTTP z Internetu i przekazuje je do Kestrel po niektórych wstępnego obsługi.</span><span class="sxs-lookup"><span data-stu-id="1d385-124">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotny serwer proxy, na przykład serwer Nginx, Apache lub IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="1d385-126">Zwrotny serwer proxy jest wymagany w przypadku wdrożeń usługi edge (ujawniony na ruch z Internetu) ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="1d385-126">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="1d385-127">Wersji 1.x Kestrel nie mają pełny zestaw zabezpieczeń przed atakami, takie jak odpowiednie limity czasu, limity rozmiaru i limity liczby jednoczesnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="1d385-127">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="1d385-128">Istnieje scenariusz zwrotny serwer proxy, gdy wiele aplikacji, które współużytkują ten sam adres IP i port uruchomione na jednym serwerze.</span><span class="sxs-lookup"><span data-stu-id="1d385-128">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="1d385-129">Kestrel nie obsługuje tego scenariusza, ponieważ Kestrel nie obsługuje udostępniania tych samych adresów IP i port między różne procesy.</span><span class="sxs-lookup"><span data-stu-id="1d385-129">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="1d385-130">Kestrel Kestrel została skonfigurowana do nasłuchiwania na porcie, obsługuje cały ruch do tego portu, niezależnie od tego, nagłówek hosta żądania.</span><span class="sxs-lookup"><span data-stu-id="1d385-130">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="1d385-131">Zwrotny serwer proxy, który można udostępniać porty zdolność do przekazywania żądań do Kestrel na unikatowych adresów IP i porcie.</span><span class="sxs-lookup"><span data-stu-id="1d385-131">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="1d385-132">Nawet jeśli zwrotnego serwera proxy nie jest wymagane, przy użyciu zwrotnego serwera proxy może być dobrym wyborem:</span><span class="sxs-lookup"><span data-stu-id="1d385-132">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="1d385-133">Może ograniczyć narażonych publiczny obszar powierzchni aplikacji, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="1d385-133">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="1d385-134">Zapewnia dodatkową warstwę konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="1d385-134">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="1d385-135">Być może ją zintegrować lepiej z istniejącą infrastrukturą.</span><span class="sxs-lookup"><span data-stu-id="1d385-135">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="1d385-136">Upraszcza ona konfiguracji protokołu SSL i równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="1d385-136">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="1d385-137">Tylko zwrotnego serwera proxy wymaga certyfikatu SSL, a ten serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej za pomocą zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1d385-137">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="1d385-138">Jeśli nie używa włączony zwrotny serwer proxy z hostem, filtrowanie, [filtrowania host](#host-filtering) musi być włączona.</span><span class="sxs-lookup"><span data-stu-id="1d385-138">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="1d385-139">Jak używać Kestrel w aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1d385-139">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d385-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d385-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="1d385-141">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pakietu znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="1d385-141">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="1d385-142">Szablony projektów programu ASP.NET Core używa Kestrel domyślnie.</span><span class="sxs-lookup"><span data-stu-id="1d385-142">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="1d385-143">W *Program.cs*, kod wywołuje szablon [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), która wywołuje metodę [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) w tle.</span><span class="sxs-lookup"><span data-stu-id="1d385-143">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d385-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d385-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="1d385-145">Zainstaluj [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="1d385-145">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="1d385-146">Wywołaj [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) metody rozszerzenia [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) w `Main` metody, określając jedną [opcje Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) wymaganych, jak pokazano w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="1d385-146">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="1d385-147">Opcje kestrel</span><span class="sxs-lookup"><span data-stu-id="1d385-147">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d385-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d385-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="1d385-149">Serwer sieci web Kestrel ma ograniczenie opcje konfiguracji, które są szczególnie przydatne w przypadku wdrożeń z Internetu.</span><span class="sxs-lookup"><span data-stu-id="1d385-149">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="1d385-150">Kilka ważnych limity, które można dostosować:</span><span class="sxs-lookup"><span data-stu-id="1d385-150">A few important limits that can be customized:</span></span>

* <span data-ttu-id="1d385-151">Maksymalna liczba połączeń klientów</span><span class="sxs-lookup"><span data-stu-id="1d385-151">Maximum client connections</span></span>
* <span data-ttu-id="1d385-152">Rozmiar treści żądania maksymalna</span><span class="sxs-lookup"><span data-stu-id="1d385-152">Maximum request body size</span></span>
* <span data-ttu-id="1d385-153">Szybkość danych treści żądania minimalne</span><span class="sxs-lookup"><span data-stu-id="1d385-153">Minimum request body data rate</span></span>

<span data-ttu-id="1d385-154">Ustaw na te i inne ograniczenia [limity](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) właściwość [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) klasy.</span><span class="sxs-lookup"><span data-stu-id="1d385-154">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="1d385-155">`Limits` Właściwość posiada wystąpienie [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) klasy.</span><span class="sxs-lookup"><span data-stu-id="1d385-155">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

<span data-ttu-id="1d385-156">**Maksymalna liczba połączeń klientów**</span><span class="sxs-lookup"><span data-stu-id="1d385-156">**Maximum client connections**</span></span>

[<span data-ttu-id="1d385-157">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="1d385-157">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="1d385-158">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="1d385-158">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="1d385-159">Można ustawić maksymalną liczbę równoczesnych otwarte połączenia TCP dla całej aplikacji, używając następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="1d385-159">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="1d385-160">Ma osobnych limitu połączeń, które zostały uaktualnione do innego protokołu (na przykład na żądanie funkcji WebSockets) z protokołu HTTP lub HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1d385-160">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="1d385-161">Po uaktualnieniu połączenie nie jest uwzględniane `MaxConcurrentConnections` limit.</span><span class="sxs-lookup"><span data-stu-id="1d385-161">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="1d385-162">Maksymalna liczba połączeń jest nieograniczona (null) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="1d385-162">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="1d385-163">**Rozmiar treści żądania maksymalna**</span><span class="sxs-lookup"><span data-stu-id="1d385-163">**Maximum request body size**</span></span>

[<span data-ttu-id="1d385-164">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="1d385-164">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="1d385-165">Domyślny rozmiar treści żądania maksymalna jest 30,000,000 bajtów, czyli około 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="1d385-165">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="1d385-166">Zalecane podejście do zastąpienia limitu w aplikacji ASP.NET Core MVC jest użycie [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) atrybutu metody akcji:</span><span class="sxs-lookup"><span data-stu-id="1d385-166">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="1d385-167">Oto przykład pokazujący sposób konfigurowania ograniczeń aplikacji na każde żądanie:</span><span class="sxs-lookup"><span data-stu-id="1d385-167">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="1d385-168">Można zastąpić ustawienia dla konkretnego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="1d385-168">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="1d385-169">Wyjątek jest generowany, Jeśli spróbujesz skonfigurować limit na żądanie, po uruchomieniu aplikacji do odczytania żądania.</span><span class="sxs-lookup"><span data-stu-id="1d385-169">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="1d385-170">Brak `IsReadOnly` właściwość, która wskazuje, czy `MaxRequestBodySize` właściwość jest w stanie tylko do odczytu, co oznacza, jest za późno, aby skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="1d385-170">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="1d385-171">**Szybkość danych treści żądania minimalne**</span><span class="sxs-lookup"><span data-stu-id="1d385-171">**Minimum request body data rate**</span></span>

[<span data-ttu-id="1d385-172">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="1d385-172">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="1d385-173">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="1d385-173">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="1d385-174">Kestrel sprawdza co sekundę, jeśli danych jest odebranych zgodnie ze stawką określoną w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="1d385-174">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="1d385-175">Jeśli szybkość spadnie poniżej minimum, jest upłynął limit czasu połączenia. Okres prolongaty to ilość czasu, że Kestrel daje klienta, aby zwiększyć jej szybkość wysyłania do minimum; wskaźnik nie jest sprawdzana w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="1d385-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="1d385-176">Okres prolongaty pomaga uniknąć porzucenie połączeń, które początkowo wysyłania danych stawki wolno z powodu start wolne TCP.</span><span class="sxs-lookup"><span data-stu-id="1d385-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="1d385-177">Minimalna szybkość domyślna jest 240 bajtów na sekundę z 5-sekundowego okresu prolongaty.</span><span class="sxs-lookup"><span data-stu-id="1d385-177">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="1d385-178">Minimalna szybkość dotyczy również odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1d385-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="1d385-179">Kod, aby ustawić limit żądań i limit odpowiedzi jest taki sam, z wyjątkiem o `RequestBody` lub `Response` w nazwach właściwości i interfejsu.</span><span class="sxs-lookup"><span data-stu-id="1d385-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="1d385-180">Oto przykład pokazujący sposób konfigurowania kursów minimalną ilość danych w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1d385-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-7)]

<span data-ttu-id="1d385-181">Kursy na żądanie można skonfigurować w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="1d385-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="1d385-182">Aby uzyskać informacje o innych opcjach Kestrel i limity zobacz:</span><span class="sxs-lookup"><span data-stu-id="1d385-182">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="1d385-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="1d385-183">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="1d385-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="1d385-184">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="1d385-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="1d385-185">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d385-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d385-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="1d385-187">Aby uzyskać informacji o opcjach Kestrel i ograniczeń zobacz:</span><span class="sxs-lookup"><span data-stu-id="1d385-187">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="1d385-188">Klasa KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="1d385-188">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="1d385-189">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="1d385-189">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a><span data-ttu-id="1d385-190">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="1d385-190">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d385-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d385-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="1d385-192">Domyślnie platforma ASP.NET Core wiąże `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1d385-192">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="1d385-193">Wywołaj [nasłuchiwania](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) lub [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metod [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) skonfigurować przedrostki adresów URL i portów dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1d385-193">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="1d385-194">`UseUrls`, `--urls` argument wiersza polecenia `urls` klucz konfiguracji hosta, a `ASPNETCORE_URLS` zmiennej środowiskowej również działają, ale ma ograniczenia, zauważyć później, w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="1d385-194">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="1d385-195">`urls` Klucz konfiguracji hosta muszą pochodzić z konfiguracji hosta, a nie konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1d385-195">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="1d385-196">Dodawanie `urls` klucza i wartości do *appsettings.json* nie wpływa na konfigurację hosta, ponieważ host jest w pełni zainicjowany przez czas konfiguracji są odczytywane z pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1d385-196">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="1d385-197">Jednak `urls` w *appsettings.json* mogą być używane z [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) na Konstruktor hosta, aby skonfigurować hosta:</span><span class="sxs-lookup"><span data-stu-id="1d385-197">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1d385-198">Domyślnie platforma ASP.NET Core wiąże:</span><span class="sxs-lookup"><span data-stu-id="1d385-198">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="1d385-199">`https://localhost:5001` (gdy certyfikat rozwoju lokalnego znajduje się)</span><span class="sxs-lookup"><span data-stu-id="1d385-199">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="1d385-200">Utworzono certyfikatu deweloperskiego:</span><span class="sxs-lookup"><span data-stu-id="1d385-200">A development certificate is created:</span></span>

* <span data-ttu-id="1d385-201">Gdy [zestawu .NET Core SDK](/dotnet/core/sdk) jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="1d385-201">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="1d385-202">[Narzędzia dev-certs](xref:aspnetcore-2.1#https) służy do tworzenia certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="1d385-202">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="1d385-203">Niektóre przeglądarki wymagają przyznanie jawnych uprawnień w przeglądarce ufać certyfikatowi rozwoju lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1d385-203">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="1d385-204">Nowsze szablony projektów i ASP.NET Core 2.1 Konfigurowanie aplikacji są domyślnie uruchamiane przy użyciu protokołu HTTPS i dołączenie [przekierowania protokołu HTTPS i HSTS obsługują](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="1d385-204">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="1d385-205">Wywołaj [nasłuchiwania](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) lub [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metod [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) skonfigurować przedrostki adresów URL i portów dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1d385-205">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="1d385-206">`UseUrls`, `--urls` argument wiersza polecenia `urls` klucz konfiguracji hosta, a `ASPNETCORE_URLS` zmiennej środowiskowej również działają, ale ma ograniczenia, zauważyć później, w tej sekcji (domyślny certyfikat musi być dostępny dla punktu końcowego protokołu HTTPS Konfiguracja).</span><span class="sxs-lookup"><span data-stu-id="1d385-206">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="1d385-207">Platforma ASP.NET Core 2.1 `KestrelServerOptions` konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="1d385-207">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

<span data-ttu-id="1d385-208">**ConfigureEndpointDefaults (Akcja&lt;ListenOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="1d385-208">**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**</span></span>  
<span data-ttu-id="1d385-209">Określa konfigurację `Action` do uruchamiania dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="1d385-209">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="1d385-210">Wywoływanie `ConfigureEndpointDefaults` wielokrotnie zastępuje starszy `Action`s w ostatnich `Action` określony.</span><span class="sxs-lookup"><span data-stu-id="1d385-210">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="1d385-211">**ConfigureHttpsDefaults (Akcja&lt;HttpsConnectionAdapterOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="1d385-211">**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**</span></span>  
<span data-ttu-id="1d385-212">Określa konfigurację `Action` do uruchamiania dla każdego punktu końcowego protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1d385-212">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="1d385-213">Wywoływanie `ConfigureHttpsDefaults` wielokrotnie zastępuje starszy `Action`s w ostatnich `Action` określony.</span><span class="sxs-lookup"><span data-stu-id="1d385-213">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="1d385-214">**Configure(IConfiguration)**</span><span class="sxs-lookup"><span data-stu-id="1d385-214">**Configure(IConfiguration)**</span></span>  
<span data-ttu-id="1d385-215">Tworzy moduł ładujący konfiguracji dotyczące konfigurowania Kestrel, która przyjmuje [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="1d385-215">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="1d385-216">Konfiguracja musi należeć do sekcji konfiguracji zakresu dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1d385-216">The configuration must be scoped to the configuration section for Kestrel.</span></span>

<span data-ttu-id="1d385-217">**ListenOptions.UseHttps**</span><span class="sxs-lookup"><span data-stu-id="1d385-217">**ListenOptions.UseHttps**</span></span>  
<span data-ttu-id="1d385-218">Konfigurowanie usługi Kestrel do używania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1d385-218">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="1d385-219">`ListenOptions.UseHttps` rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="1d385-219">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="1d385-220">`UseHttps` &ndash; Konfigurowanie usługi Kestrel do używania protokołu HTTPS przy użyciu domyślnego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="1d385-220">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="1d385-221">Zgłasza wyjątek, jeśli nie domyślnego certyfikatu jest skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="1d385-221">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="1d385-222">`ListenOptions.UseHttps` Parametry:</span><span class="sxs-lookup"><span data-stu-id="1d385-222">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="1d385-223">`filename` jest ścieżka i nazwa pliku certyfikatu, względem katalogu zawierającego pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1d385-223">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="1d385-224">`password` to hasło wymagane do uzyskania dostępu do danych certyfikatu X.509.</span><span class="sxs-lookup"><span data-stu-id="1d385-224">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="1d385-225">`configureOptions` jest `Action` skonfigurować `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="1d385-225">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="1d385-226">Zwraca `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="1d385-226">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="1d385-227">`storeName` jest magazyn certyfikatów, z którego można załadować certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="1d385-227">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="1d385-228">`subject` jest to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="1d385-228">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="1d385-229">`allowInvalid` Wskazuje, jeżeli nieprawidłowe certyfikaty należy rozważyć, takich jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="1d385-229">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="1d385-230">`location` jest to lokalizacja magazynu można załadować certyfikatu z.</span><span class="sxs-lookup"><span data-stu-id="1d385-230">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="1d385-231">`serverCertificate` jest to certyfikat X.509.</span><span class="sxs-lookup"><span data-stu-id="1d385-231">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="1d385-232">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1d385-232">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="1d385-233">Jako minimum należy podać certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="1d385-233">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="1d385-234">Obsługiwane konfiguracje opisane w dalszej części:</span><span class="sxs-lookup"><span data-stu-id="1d385-234">Supported configurations described next:</span></span>

* <span data-ttu-id="1d385-235">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="1d385-235">No configuration</span></span>
* <span data-ttu-id="1d385-236">Zamień domyślny certyfikat z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="1d385-236">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="1d385-237">Zmiana wartości domyślnych w kodzie</span><span class="sxs-lookup"><span data-stu-id="1d385-237">Change the defaults in code</span></span>

<span data-ttu-id="1d385-238">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="1d385-238">*No configuration*</span></span>

<span data-ttu-id="1d385-239">Nasłuchuje kestrel `http://localhost:5000` i `https://localhost:5001` (jeśli jest to domyślny certyfikat jest dostępny).</span><span class="sxs-lookup"><span data-stu-id="1d385-239">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="1d385-240">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="1d385-240">Specify URLs using the:</span></span>

* <span data-ttu-id="1d385-241">`ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="1d385-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="1d385-242">`--urls` argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="1d385-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="1d385-243">`urls` Klucz konfiguracji hosta.</span><span class="sxs-lookup"><span data-stu-id="1d385-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="1d385-244">`UseUrls` metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1d385-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="1d385-245">Aby uzyskać więcej informacji, zobacz [adresy URL serwerów](xref:fundamentals/host/web-host#server-urls) i [zastępczą konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="1d385-245">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="1d385-246">Podana wartość przy użyciu tych metod może być co najmniej jeden protokół HTTP i HTTPS punktów końcowych (HTTPS Jeśli dostępny jest certyfikatu z domyślną).</span><span class="sxs-lookup"><span data-stu-id="1d385-246">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="1d385-247">Konfiguracja uwzględnia wartość będącą rozdzielonej średnikami liście (na przykład `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="1d385-247">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="1d385-248">*Zamień domyślny certyfikat z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="1d385-248">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="1d385-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie można załadować konfiguracji Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1d385-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="1d385-250">Schemat konfiguracji ustawień aplikacji domyślnej HTTPS jest dostępna dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1d385-250">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="1d385-251">Skonfigurować wiele punktów końcowych, w tym adresy URL i certyfikaty, aby korzystać z pliku na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="1d385-251">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="1d385-252">W następującym *appsettings.json* przykładu:</span><span class="sxs-lookup"><span data-stu-id="1d385-252">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="1d385-253">Ustaw **AllowInvalid** do `true` Aby zezwolić na korzystanie z certyfikatów nieprawidłowa (na przykład certyfikatów z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="1d385-253">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="1d385-254">Punkt końcowy HTTPS, który nie określa certyfikat (**HttpsDefaultCert** w poniższym przykładzie) powraca do certyfikatu zdefiniowane w obszarze **certyfikaty** > **domyślne**  lub certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="1d385-254">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
"Kestrel": {
  "EndPoints": {
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
        "Store": "<certificate store; defaults to My>",
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

<span data-ttu-id="1d385-255">Alternatywa dla użycia **ścieżki** i **hasło** dla każdego certyfikatu węzeł jest aby określić certyfikat przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="1d385-255">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="1d385-256">Na przykład **certyfikaty** > **domyślne** certyfikat może być określony jako:</span><span class="sxs-lookup"><span data-stu-id="1d385-256">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="1d385-257">Informacje o schemacie:</span><span class="sxs-lookup"><span data-stu-id="1d385-257">Schema notes:</span></span>

* <span data-ttu-id="1d385-258">Nazwy punktów końcowych jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="1d385-258">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="1d385-259">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="1d385-259">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="1d385-260">`Url` Parametr jest wymagany dla każdego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="1d385-260">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="1d385-261">Format dla tego parametru jest taka sama jak najwyższego poziomu `Urls` parametru konfiguracji, ale jego ograniczone do pojedynczej wartości.</span><span class="sxs-lookup"><span data-stu-id="1d385-261">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="1d385-262">Zastąp te punkty końcowe, identyczne ze zdefiniowanymi w najwyższego poziomu `Urls` konfiguracji zamiast dodawać do nich.</span><span class="sxs-lookup"><span data-stu-id="1d385-262">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="1d385-263">Punkty końcowe zdefiniowane w kodzie za pomocą `Listen` kumulują się z punktami końcowymi, zdefiniowane w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1d385-263">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="1d385-264">`Certificate` Sekcja jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="1d385-264">The `Certificate` section is optional.</span></span> <span data-ttu-id="1d385-265">Jeśli `Certificate` sekcji nie jest określona, używane są zdefiniowane w scenariuszach wcześniej wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="1d385-265">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="1d385-266">Jeśli dostępnych żadnych ustawień domyślnych, serwer zgłasza wyjątek i nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="1d385-266">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="1d385-267">`Certificate` Sekcji obsługuje zarówno **ścieżki**&ndash;**hasło** i **podmiotu**&ndash;**Store** certyfikaty.</span><span class="sxs-lookup"><span data-stu-id="1d385-267">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="1d385-268">Tak długo, jak one nie powodują konfliktów portów można zdefiniować dowolną liczbę punktów końcowych w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="1d385-268">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="1d385-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` Zwraca `KestrelConfigurationLoader` z `.Endpoint(string name, options => { })` metodę, która może służyć jako uzupełnienie ustawienia skonfigurowanego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="1d385-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="1d385-270">Można również uzyskać dostęp do `KestrelServerOptions.ConfigurationLoader` można zachować iteracja na istniejące modułu ładującego, takiego jak dostarczone przez [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="1d385-270">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="1d385-271">Sekcja konfiguracji dla każdego punktu końcowego jest dostępna na temat opcji w `Endpoint` metody, aby ustawienia niestandardowe, które mogą być odczytywane.</span><span class="sxs-lookup"><span data-stu-id="1d385-271">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="1d385-272">Wiele konfiguracji mogą być ładowane przez wywołanie metody `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` ponownie, używając innej sekcji.</span><span class="sxs-lookup"><span data-stu-id="1d385-272">Multiple configurations may be loaded by calling `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="1d385-273">Ostatnia konfiguracja jest używany, chyba że `Load` zostanie jawnie wywołany w poprzednich wystąpień.</span><span class="sxs-lookup"><span data-stu-id="1d385-273">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="1d385-274">Nie wywołuje meta Microsoft.aspnetcore.all `Load` tak, aby jego sekcji konfiguracji domyślnej może zostać zastąpiony.</span><span class="sxs-lookup"><span data-stu-id="1d385-274">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="1d385-275">`KestrelConfigurationLoader` wstecznych `Listen` rodziny interfejsów API z `KestrelServerOptions` jako `Endpoint` przeciążeń, dzięki czemu kod i konfiguracji punktów końcowych może zostać skonfigurowana w tym samym miejscu.</span><span class="sxs-lookup"><span data-stu-id="1d385-275">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="1d385-276">Te przeciążenia, nie używaj nazw i tylko używać domyślnych ustawień konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1d385-276">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="1d385-277">*Zmiana wartości domyślnych w kodzie*</span><span class="sxs-lookup"><span data-stu-id="1d385-277">*Change the defaults in code*</span></span>

<span data-ttu-id="1d385-278">`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` może służyć do zmiany ustawień domyślnych dla `ListenOptions` i `HttpsConnectionAdapterOptions`, tym przesłanianie domyślnego certyfikatu określone w poprzednich scenariusza.</span><span class="sxs-lookup"><span data-stu-id="1d385-278">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="1d385-279">`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` powinna być wywoływana przed wszystkie punkty końcowe zostały skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="1d385-279">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="1d385-280">*Obsługa kestrel SNI*</span><span class="sxs-lookup"><span data-stu-id="1d385-280">*Kestrel support for SNI*</span></span>

<span data-ttu-id="1d385-281">[Oznaczaniem nazwy serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) mogą być używane do obsługi wielu domen na tym samym adresie IP i port.</span><span class="sxs-lookup"><span data-stu-id="1d385-281">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="1d385-282">Dla SNI do funkcji Klient wysyła nazwę hosta dla bezpiecznej sesji na serwerze podczas uzgadniania TLS tak, aby serwer może zapewnić poprawny certyfikat.</span><span class="sxs-lookup"><span data-stu-id="1d385-282">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="1d385-283">Klient korzysta z certyfikatu złożonych szyfrowaną komunikację z serwerem podczas nawiązywania bezpiecznej sesji, który następuje po TLS handshake.</span><span class="sxs-lookup"><span data-stu-id="1d385-283">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="1d385-284">Kestrel obsługuje SNI za pośrednictwem `ServerCertificateSelector` wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="1d385-284">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="1d385-285">Wywołanie zwrotne jest wywoływana jeden raz na połączenie, aby umożliwić aplikacji do sprawdzania nazwy hosta i wybierz odpowiedni certyfikat.</span><span class="sxs-lookup"><span data-stu-id="1d385-285">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="1d385-286">Obsługa SNI wymaga:</span><span class="sxs-lookup"><span data-stu-id="1d385-286">SNI support requires:</span></span>

* <span data-ttu-id="1d385-287">Uruchamianie na platformie docelowej `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="1d385-287">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="1d385-288">Na `netcoreapp2.0` i `net461`, wywołanie zwrotne jest wywoływane, ale `name` jest zawsze `null`.</span><span class="sxs-lookup"><span data-stu-id="1d385-288">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="1d385-289">`name` Jest również `null` Jeśli klient nie udostępnia hosta, nazwy parametru w uzgadniania TLS.</span><span class="sxs-lookup"><span data-stu-id="1d385-289">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="1d385-290">Wszystkie witryny sieci Web uruchamiane na tym samym wystąpieniu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1d385-290">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="1d385-291">Kestrel nie obsługuje udostępniania adresu IP i portów w wielu wystąpieniach bez zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="1d385-291">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
WebHost.CreateDefaultBuilder()
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
    });
```

::: moniker-end

<span data-ttu-id="1d385-292">**Powiąż z gniazda TCP**</span><span class="sxs-lookup"><span data-stu-id="1d385-292">**Bind to a TCP socket**</span></span>

<span data-ttu-id="1d385-293">[Nasłuchiwania](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) metoda wiąże gniazda TCP i lambda opcje zezwala na konfigurację certyfikatu SSL:</span><span class="sxs-lookup"><span data-stu-id="1d385-293">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="1d385-294">Przykład umożliwia skonfigurowanie protokołu SSL dla punktu końcowego za pomocą [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="1d385-294">The example configures SSL for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="1d385-295">Skonfiguruj inne ustawienia Kestrel do określonych punktów końcowych za pomocą tego samego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1d385-295">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

<span data-ttu-id="1d385-296">**Powiązany z gniazdem systemu Unix**</span><span class="sxs-lookup"><span data-stu-id="1d385-296">**Bind to a Unix socket**</span></span>

<span data-ttu-id="1d385-297">Nasłuchiwanie na gniazdo systemu Unix za pomocą [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) w celu zwiększenia wydajności przy użyciu serwera Nginx, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="1d385-297">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="1d385-298">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="1d385-298">**Port 0**</span></span>

<span data-ttu-id="1d385-299">Gdy numer portu `0` określono Kestrel dynamicznie wiąże dostępny port.</span><span class="sxs-lookup"><span data-stu-id="1d385-299">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="1d385-300">Poniższy przykład pokazuje, jak określić port, który Kestrel faktycznie powiązany w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="1d385-300">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="1d385-301">Gdy aplikacja jest uruchamiana, dane wyjściowe z okna konsoli wskazuje port dynamiczny, gdzie można połączyć aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1d385-301">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

<span data-ttu-id="1d385-302">**Klucz konfiguracji i ograniczenia zmiennych środowiskowych ASPNETCORE_URLS hostów UseUrls, argument wiersza polecenia — adresów URL, adresy URL**</span><span class="sxs-lookup"><span data-stu-id="1d385-302">**UseUrls, --urls command-line argument, urls host configuration key, and ASPNETCORE_URLS environment variable limitations**</span></span>

<span data-ttu-id="1d385-303">Konfigurowanie punktów końcowych przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="1d385-303">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="1d385-304">UseUrls</span><span class="sxs-lookup"><span data-stu-id="1d385-304">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="1d385-305">`--urls` argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1d385-305">`--urls` command-line argument</span></span>
* <span data-ttu-id="1d385-306">`urls` Klucz konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="1d385-306">`urls` host configuration key</span></span>
* <span data-ttu-id="1d385-307">`ASPNETCORE_URLS` Zmienna środowiskowa</span><span class="sxs-lookup"><span data-stu-id="1d385-307">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="1d385-308">Te metody są przydatne do tworzenia kodu, pracy z serwerami innych niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1d385-308">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="1d385-309">Jednak należy pamiętać o tych ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="1d385-309">However, be aware of these limitations:</span></span>

* <span data-ttu-id="1d385-310">Nie można użyć protokołu SSL przy użyciu tych metod, jeśli domyślny certyfikat znajduje się w konfiguracji punktu końcowego protokołu HTTPS (na przykład za pomocą `KestrelServerOptions` konfiguracji lub plik konfiguracji, jak pokazano we wcześniejszej części tego tematu).</span><span class="sxs-lookup"><span data-stu-id="1d385-310">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="1d385-311">Gdy zarówno `Listen` i `UseUrls` podejścia są używane równocześnie, `Listen` zastąpienia punktów końcowych `UseUrls` punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="1d385-311">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="1d385-312">**Konfiguracja punktu końcowego usług IIS**</span><span class="sxs-lookup"><span data-stu-id="1d385-312">**IIS endpoint configuration**</span></span>

<span data-ttu-id="1d385-313">Korzystając z usług IIS, powiązania adres URL dla zastąpienia IIS powiązania są ustawiane przez `Listen` lub `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="1d385-313">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="1d385-314">Aby uzyskać więcej informacji, zobacz [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) tematu.</span><span class="sxs-lookup"><span data-stu-id="1d385-314">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d385-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d385-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="1d385-316">Domyślnie platforma ASP.NET Core wiąże `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1d385-316">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="1d385-317">Skonfiguruj przedrostki adresów URL i portów dla Kestrel przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="1d385-317">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="1d385-318">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) — metoda rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="1d385-318">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="1d385-319">`--urls` argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1d385-319">`--urls` command-line argument</span></span>
* <span data-ttu-id="1d385-320">`urls` Klucz konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="1d385-320">`urls` host configuration key</span></span>
* <span data-ttu-id="1d385-321">System konfiguracji platformy ASP.NET Core, w tym `ASPNETCORE_URLS` zmiennej środowiskowej</span><span class="sxs-lookup"><span data-stu-id="1d385-321">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="1d385-322">Aby uzyskać więcej informacji na temat tych metod, zobacz [hostingu](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="1d385-322">For more information on these methods, see [Hosting](xref:fundamentals/host/index).</span></span>

<span data-ttu-id="1d385-323">**Konfiguracja punktu końcowego usług IIS**</span><span class="sxs-lookup"><span data-stu-id="1d385-323">**IIS endpoint configuration**</span></span>

<span data-ttu-id="1d385-324">Korzystając z usług IIS, powiązania adres URL dla usług IIS zastąpienia powiązania ustawione przez `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="1d385-324">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="1d385-325">Aby uzyskać więcej informacji, zobacz [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) tematu.</span><span class="sxs-lookup"><span data-stu-id="1d385-325">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a><span data-ttu-id="1d385-326">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="1d385-326">Transport configuration</span></span>

<span data-ttu-id="1d385-327">W wersji platformy ASP.NET Core 2.1 firmy Kestrel transportu domyślnego jest już oparte na Libuv, ale oparta na zarządzanych gniazda.</span><span class="sxs-lookup"><span data-stu-id="1d385-327">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="1d385-328">Jest to istotną zmianę dla aplikacji ASP.NET Core 2.0, uaktualnienie do wersji 2.1, który [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) i są zależne od jednej z następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="1d385-328">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="1d385-329">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (bezpośrednie odwołanie do pakietu)</span><span class="sxs-lookup"><span data-stu-id="1d385-329">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="1d385-330">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="1d385-330">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="1d385-331">Dla platformy ASP.NET Core 2.1 lub nowszej projektów używających [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) i wymagają użycia Libuv:</span><span class="sxs-lookup"><span data-stu-id="1d385-331">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="1d385-332">Dodawanie zależności dla [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) pakietu do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1d385-332">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="2.1.0" />
    ```

* <span data-ttu-id="1d385-333">Wywołaj [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span><span class="sxs-lookup"><span data-stu-id="1d385-333">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

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

::: moniker-end

### <a name="url-prefixes"></a><span data-ttu-id="1d385-334">Przedrostki adresów URL</span><span class="sxs-lookup"><span data-stu-id="1d385-334">URL prefixes</span></span>

<span data-ttu-id="1d385-335">Korzystając z `UseUrls`, `--urls` argument wiersza polecenia `urls` klucz konfiguracji hosta, lub `ASPNETCORE_URLS` zmiennej środowiskowej przedrostki adresów URL może być w dowolnym z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="1d385-335">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d385-336">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d385-336">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="1d385-337">Prawidłowe są tylko prefiksy URL protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1d385-337">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="1d385-338">Kestrel nie obsługuje protokołu SSL, podczas konfigurowania powiązania adresu URL używającego `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="1d385-338">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="1d385-339">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="1d385-339">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="1d385-340">`0.0.0.0` jest szczególnym przypadkiem, która jest powiązywana z wszystkich adresów IPv4.</span><span class="sxs-lookup"><span data-stu-id="1d385-340">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="1d385-341">Numer portu w adresie IPv6</span><span class="sxs-lookup"><span data-stu-id="1d385-341">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="1d385-342">`[::]` jest to równoważne IPv6 IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="1d385-342">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="1d385-343">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="1d385-343">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="1d385-344">Nazwy, hostów `*`, i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="1d385-344">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="1d385-345">Niczego nie został rozpoznany jako prawidłowy adres IP lub `localhost` wiąże wszystkich protokołów IPv4 i IPv6, adresy IP.</span><span class="sxs-lookup"><span data-stu-id="1d385-345">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="1d385-346">Aby powiązać różne nazwy hostów do różnych aplikacji platformy ASP.NET Core przy użyciu tego samego portu, należy użyć [HTTP.sys](xref:fundamentals/servers/httpsys) lub serwer zwrotny serwer proxy, na przykład serwer Nginx, Apache lub IIS.</span><span class="sxs-lookup"><span data-stu-id="1d385-346">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="1d385-347">Jeśli nie używa włączony zwrotny serwer proxy z hostem, filtrowanie, Włącz [filtrowania host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="1d385-347">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="1d385-348">Host `localhost` nazwa portu numer lub sprzężenia zwrotnego protokołu IP z numerem portu</span><span class="sxs-lookup"><span data-stu-id="1d385-348">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="1d385-349">Gdy `localhost` określono Kestrel próbuje powiązać interfejsów sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="1d385-349">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="1d385-350">Jeśli żądany port jest używany przez inną usługę na albo interfejsu sprzężenia zwrotnego, Kestrel uruchomienie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="1d385-350">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="1d385-351">Jeśli albo interfejsu sprzężenia zwrotnego jest niedostępny z innego powodu (większość często, ponieważ nie jest obsługiwany protokół IPv6), Kestrel dzienniki ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="1d385-351">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d385-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d385-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="1d385-353">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="1d385-353">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="1d385-354">`0.0.0.0` jest szczególnym przypadkiem, która jest powiązywana z wszystkich adresów IPv4.</span><span class="sxs-lookup"><span data-stu-id="1d385-354">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="1d385-355">Numer portu w adresie IPv6</span><span class="sxs-lookup"><span data-stu-id="1d385-355">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="1d385-356">`[::]` jest to równoważne IPv6 IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="1d385-356">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="1d385-357">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="1d385-357">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="1d385-358">Nazwy, hostów `*`, i `+` nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="1d385-358">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="1d385-359">Wszystko, co nie jest rozpoznany adres IP lub `localhost` wiąże wszystkich protokołów IPv4 i IPv6, adresy IP.</span><span class="sxs-lookup"><span data-stu-id="1d385-359">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="1d385-360">Aby powiązać różne nazwy hostów do różnych aplikacji platformy ASP.NET Core przy użyciu tego samego portu, należy użyć [WebListener](xref:fundamentals/servers/weblistener) lub serwer zwrotny serwer proxy, na przykład serwer Nginx, Apache lub IIS.</span><span class="sxs-lookup"><span data-stu-id="1d385-360">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="1d385-361">Host `localhost` nazwa portu numer lub sprzężenia zwrotnego protokołu IP z numerem portu</span><span class="sxs-lookup"><span data-stu-id="1d385-361">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="1d385-362">Gdy `localhost` określono Kestrel próbuje powiązać interfejsów sprzężenia zwrotnego IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="1d385-362">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="1d385-363">Jeśli żądany port jest używany przez inną usługę na albo interfejsu sprzężenia zwrotnego, Kestrel uruchomienie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="1d385-363">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="1d385-364">Jeśli albo interfejsu sprzężenia zwrotnego jest niedostępny z innego powodu (większość często, ponieważ nie jest obsługiwany protokół IPv6), Kestrel dzienniki ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="1d385-364">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="1d385-365">Gniazda systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="1d385-365">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="1d385-366">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="1d385-366">**Port 0**</span></span>

<span data-ttu-id="1d385-367">Jeśli numer portu to `0` określono Kestrel dynamicznie wiąże dostępny port.</span><span class="sxs-lookup"><span data-stu-id="1d385-367">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="1d385-368">Powiązania z portem `0` jest dozwolona dla dowolnej nazwy hosta lub adres IP, z wyjątkiem `localhost`.</span><span class="sxs-lookup"><span data-stu-id="1d385-368">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="1d385-369">Gdy aplikacja jest uruchamiana, dane wyjściowe z okna konsoli wskazuje port dynamiczny, gdzie można połączyć aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1d385-369">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="1d385-370">**Przedrostki adresów URL dla protokołu SSL**</span><span class="sxs-lookup"><span data-stu-id="1d385-370">**URL prefixes for SSL**</span></span>

<span data-ttu-id="1d385-371">Jeśli wywołanie `UseHttps` metodę rozszerzenia, należy uwzględnić przedrostki adresów URL przy użyciu `https:`:</span><span class="sxs-lookup"><span data-stu-id="1d385-371">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

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
> <span data-ttu-id="1d385-372">Protokoły HTTPS i HTTP nie mogą być hostowane na tym samym porcie.</span><span class="sxs-lookup"><span data-stu-id="1d385-372">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a><span data-ttu-id="1d385-373">Filtrowanie hosta</span><span class="sxs-lookup"><span data-stu-id="1d385-373">Host filtering</span></span>

<span data-ttu-id="1d385-374">Natomiast Kestrel obsługuje konfigurację oparte na prefiksy takich jak `http://example.com:5000`, Kestrel ignoruje głównie nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="1d385-374">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="1d385-375">Host `localhost` stanowią specjalny przypadek, używane do powiązania z adresami sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="1d385-375">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="1d385-376">Hostowanie dowolnego, inne niż jawnych adresów IP, który tworzy powiązanie z wszystkich publicznych adresów IP.</span><span class="sxs-lookup"><span data-stu-id="1d385-376">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="1d385-377">Żadne z tych informacji jest używana do zweryfikowania żądania `Host` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="1d385-377">None of this information is used to validate request `Host` headers.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1d385-378">Jako obejście hosta pod zwrotny serwer proxy przy użyciu filtrowania nagłówka hosta.</span><span class="sxs-lookup"><span data-stu-id="1d385-378">As a workaround, host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="1d385-379">Jest to jedyny obsługiwany scenariusz Kestrel w programie ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1d385-379">This is the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1d385-380">Obejść ten problem, należy użyć oprogramowania pośredniczącego do filtrowania żądań za pomocą `Host` nagłówka:</span><span class="sxs-lookup"><span data-stu-id="1d385-380">As a workaround, use middleware to filter requests by the `Host` header:</span></span>

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

<span data-ttu-id="1d385-381">Zarejestruj poprzednim `HostFilteringMiddleware` w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="1d385-381">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="1d385-382">Należy pamiętać, że [kolejność oprogramowania pośredniczącego rejestracji](xref:fundamentals/middleware/index#order) jest ważne.</span><span class="sxs-lookup"><span data-stu-id="1d385-382">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#order) is important.</span></span> <span data-ttu-id="1d385-383">Rejestracja powinno nastąpić od razu po zarejestrowaniu diagnostycznych oprogramowania pośredniczącego (na przykład `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="1d385-383">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

<span data-ttu-id="1d385-384">Oczekuje, że oprogramowanie pośredniczące `AllowedHosts` w *appsettings.json*/*appsettings.\< EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="1d385-384">The middleware expects an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="1d385-385">Wartość jest rozdzieloną średnikami listę nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="1d385-385">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1d385-386">Obejść ten problem użyj filtrowania hosta w oprogramowaniu pośredniczącym.</span><span class="sxs-lookup"><span data-stu-id="1d385-386">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="1d385-387">Oprogramowanie pośredniczące filtrowania w hoście są dostarczane przez [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="1d385-387">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="1d385-388">Oprogramowanie pośredniczące jest dodawany przez [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), która wywołuje metodę [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span><span class="sxs-lookup"><span data-stu-id="1d385-388">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="1d385-389">Oprogramowanie pośredniczące filtrowania w hosta jest domyślnie wyłączona.</span><span class="sxs-lookup"><span data-stu-id="1d385-389">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="1d385-390">Aby włączyć oprogramowanie pośredniczące, zdefiniuj `AllowedHosts` w *appsettings.json*/*appsettings.\< EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="1d385-390">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="1d385-391">Wartość jest rozdzieloną średnikami listę nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="1d385-391">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1d385-392">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="1d385-392">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="1d385-393">[Oprogramowanie pośredniczące nagłówki przekazywane](xref:host-and-deploy/proxy-load-balancer) ma również [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) opcji.</span><span class="sxs-lookup"><span data-stu-id="1d385-393">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="1d385-394">Przekazane oprogramowanie pośredniczące nagłówków i hosta filtrowanie oprogramowanie pośredniczące mają podobną funkcjonalność dla różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="1d385-394">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="1d385-395">Ustawienie `AllowedHosts` z przekazywane oprogramowania pośredniczącego nagłówki jest odpowiednie, jeśli nagłówek hosta nie są zachowywane podczas przekazywania żądań przy użyciu zwrotnego serwera proxy lub moduł równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="1d385-395">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the Host header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="1d385-396">Ustawienie `AllowedHosts` z hosta filtrowania w oprogramowaniu pośredniczącym przydaje się, gdy Kestrel jest używany jako serwer graniczny, lub gdy nagłówek hosta był kierowany bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="1d385-396">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as an edge server or when the Host header is directly forwarded.</span></span>
>
> <span data-ttu-id="1d385-397">Aby uzyskać więcej informacji na temat przekazywane oprogramowania pośredniczącego nagłówków, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="1d385-397">For more information on Forwarded Headers Middleware, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="1d385-398">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1d385-398">Additional resources</span></span>

* [<span data-ttu-id="1d385-399">Wymuszanie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="1d385-399">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="1d385-400">Kod źródłowy kestrel</span><span class="sxs-lookup"><span data-stu-id="1d385-400">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* [<span data-ttu-id="1d385-401">RFC 7230: Komunikat o składni i routingu (ppkt 5.4: Host)</span><span class="sxs-lookup"><span data-stu-id="1d385-401">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
* [<span data-ttu-id="1d385-402">Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="1d385-402">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
