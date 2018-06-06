---
title: Kestrel implementacja serwera sieci web platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat Kestrel, serwer sieci web i platform dla platformy ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 22311d90fb327fc1e01f82759ad62551501a10c1
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734565"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="a0820-103">Kestrel implementacja serwera sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0820-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="a0820-104">Przez [Dykstra Tomasz](https://github.com/tdykstra), [Roaming Krzysztof](https://github.com/Tratcher), i [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="a0820-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="a0820-105">Obsługujący wiele platform jest kestrel [serwera sieci web dla platformy ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="a0820-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="a0820-106">Kestrel to serwer sieci web, który jest domyślnie włączone w szablony projektów platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0820-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="a0820-107">Kestrel obsługuje następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="a0820-107">Kestrel supports the following features:</span></span>

* <span data-ttu-id="a0820-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="a0820-108">HTTPS</span></span>
* <span data-ttu-id="a0820-109">Uaktualnienie nieprzezroczyste używane do włączania [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="a0820-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="a0820-110">Gniazda UNIX o wysokiej wydajności za Nginx</span><span class="sxs-lookup"><span data-stu-id="a0820-110">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="a0820-111">Kestrel jest obsługiwana na wszystkich platformach i wersje, które obsługuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0820-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="a0820-112">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a0820-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="a0820-113">Kiedy należy używać Kestrel z zwrotny serwer proxy</span><span class="sxs-lookup"><span data-stu-id="a0820-113">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a0820-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a0820-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a0820-115">Samodzielnie lub z użyciem Kestrel *zwrotnego serwera proxy*, takie jak usługi IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="a0820-115">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="a0820-116">Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi.</span><span class="sxs-lookup"><span data-stu-id="a0820-116">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a0820-119">Albo konfiguracji&mdash;z lub bez zwrotnego serwera proxy&mdash;jest prawidłowa i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core w wersji 2.0 lub nowszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0820-119">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a0820-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a0820-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a0820-121">Jeśli aplikacja akceptuje żądania tylko z siecią wewnętrzną, Kestrel można bezpośrednio jako serwer aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0820-121">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![Kestrel komunikuje się bezpośrednio z sieci wewnętrznej](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="a0820-123">Jeśli udostępnianie aplikacji z Internetem, za pomocą usług IIS, Nginx lub Apache jako *zwrotnego serwera proxy*.</span><span class="sxs-lookup"><span data-stu-id="a0820-123">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="a0820-124">Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi.</span><span class="sxs-lookup"><span data-stu-id="a0820-124">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a0820-126">Zwrotny serwer proxy jest wymagany w przypadku wdrożeń krawędzi (ujawniony na ruch z Internetu) ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="a0820-126">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="a0820-127">Wersje 1.x Kestrel nie mają pełny zestaw ochronę przed atakami, takimi jak odpowiednie limity czasu, limity rozmiaru i limity liczby jednoczesnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="a0820-127">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="a0820-128">W przypadku wielu aplikacji, które korzysta z tego samego adresu IP i port uruchomione na jednym serwerze istnieje scenariusz zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="a0820-128">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="a0820-129">Kestrel nie obsługuje ten scenariusz, ponieważ Kestrel nie obsługuje udostępniania tego samego adresu IP i portu między wiele procesów.</span><span class="sxs-lookup"><span data-stu-id="a0820-129">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="a0820-130">Po skonfigurowaniu Kestrel do nasłuchiwania na porcie Kestrel obsługuje cały ruch do tego portu, niezależnie od tego, żądania nagłówek hosta.</span><span class="sxs-lookup"><span data-stu-id="a0820-130">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="a0820-131">Zwrotny serwer proxy, który można udostępniać porty zdolność do przekazywania żądań do Kestrel na unikatowy adresu IP i portu.</span><span class="sxs-lookup"><span data-stu-id="a0820-131">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="a0820-132">Nawet jeśli zwrotnego serwera proxy nie jest wymagane, dobrym rozwiązaniem może być przy użyciu zwrotnego serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="a0820-132">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="a0820-133">Można go ograniczyć narażonych publicznej powierzchni aplikacje, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="a0820-133">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="a0820-134">Zapewnia dodatkową warstwę konfiguracji i obrony.</span><span class="sxs-lookup"><span data-stu-id="a0820-134">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="a0820-135">Może ją zintegrować lepiej z istniejącej infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="a0820-135">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="a0820-136">Takie rozwiązanie upraszcza równoważenia obciążenia i konfiguracja SSL.</span><span class="sxs-lookup"><span data-stu-id="a0820-136">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="a0820-137">Tylko zwrotnego serwera proxy wymaga certyfikatu SSL i że serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0820-137">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="a0820-138">Jeśli nie używa zwrotny serwer proxy z hostem Filtrowanie włączone, [filtrowania host](#host-filtering) musi być włączona.</span><span class="sxs-lookup"><span data-stu-id="a0820-138">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="a0820-139">Jak używać Kestrel w aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0820-139">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a0820-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a0820-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="a0820-141">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) dodatku [Microsoft.AspNetCore.App metapackage] (xref:fundamentals / metapackage aplikacji) (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="a0820-141">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage] (xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="a0820-142">Szablony projektów platformy ASP.NET Core domyślnie używają Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a0820-142">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="a0820-143">W *Program.cs*, wywołań kodu szablonów [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), które wywołuje [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) w tle.</span><span class="sxs-lookup"><span data-stu-id="a0820-143">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a0820-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a0820-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="a0820-145">Zainstaluj [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="a0820-145">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="a0820-146">Wywołanie [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) — metoda rozszerzenia na [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) w `Main` metoda określania żadnego [opcje Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) wymagane, jak pokazano w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="a0820-146">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="a0820-147">Opcje kestrel</span><span class="sxs-lookup"><span data-stu-id="a0820-147">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a0820-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a0820-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="a0820-149">Serwer sieci web Kestrel ma ograniczenie opcje konfiguracji, które są szczególnie przydatne w przypadku wdrożeń skierowane do Internetu.</span><span class="sxs-lookup"><span data-stu-id="a0820-149">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="a0820-150">Kilka ważnych ograniczeń, które można dostosować:</span><span class="sxs-lookup"><span data-stu-id="a0820-150">A few important limits that can be customized:</span></span>

* <span data-ttu-id="a0820-151">Maksymalna liczba połączeń klientów</span><span class="sxs-lookup"><span data-stu-id="a0820-151">Maximum client connections</span></span>
* <span data-ttu-id="a0820-152">Żądanie maksymalny rozmiar treści</span><span class="sxs-lookup"><span data-stu-id="a0820-152">Maximum request body size</span></span>
* <span data-ttu-id="a0820-153">Szybkość danych treści żądania minimalna</span><span class="sxs-lookup"><span data-stu-id="a0820-153">Minimum request body data rate</span></span>

<span data-ttu-id="a0820-154">Ustaw na tych i innych ograniczeń [limity](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) właściwość [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) klasy.</span><span class="sxs-lookup"><span data-stu-id="a0820-154">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="a0820-155">`Limits` Właściwość przechowuje wystąpienia [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) klasy.</span><span class="sxs-lookup"><span data-stu-id="a0820-155">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

<span data-ttu-id="a0820-156">**Maksymalna liczba połączeń klientów**</span><span class="sxs-lookup"><span data-stu-id="a0820-156">**Maximum client connections**</span></span>

[<span data-ttu-id="a0820-157">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="a0820-157">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="a0820-158">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="a0820-158">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="a0820-159">Można ustawić maksymalną liczbę równoczesnych połączeń TCP otwarte dla całej aplikacji z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a0820-159">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="a0820-160">Brak oddzielnych limit połączeń, które zostały uaktualnione do inny protokół (na przykład na żądanie Websocket) z HTTP lub HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a0820-160">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="a0820-161">Po uaktualnieniu połączenie nie jest uwzględniane `MaxConcurrentConnections` limit.</span><span class="sxs-lookup"><span data-stu-id="a0820-161">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="a0820-162">Maksymalna liczba połączeń jest nieograniczony (null) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="a0820-162">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="a0820-163">**Żądanie maksymalny rozmiar treści**</span><span class="sxs-lookup"><span data-stu-id="a0820-163">**Maximum request body size**</span></span>

[<span data-ttu-id="a0820-164">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="a0820-164">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="a0820-165">Domyślny rozmiar treści żądania maksymalna jest 30,000,000 bajtów, czyli około 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="a0820-165">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="a0820-166">Zalecane podejście do zastąpienia limitu w aplikacji platformy ASP.NET Core MVC jest użycie [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) atrybutu metody akcji:</span><span class="sxs-lookup"><span data-stu-id="a0820-166">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="a0820-167">Oto przykład, który pokazuje, jak skonfigurować ograniczenia dla aplikacji na każde żądanie:</span><span class="sxs-lookup"><span data-stu-id="a0820-167">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="a0820-168">Można zastąpić ustawienie dla określonego żądania w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="a0820-168">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="a0820-169">Przy próbie skonfigurować limit na żądanie, po uruchomieniu aplikacji można odczytać żądania, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="a0820-169">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="a0820-170">Brak `IsReadOnly` właściwość, która wskazuje, czy `MaxRequestBodySize` właściwość jest w stanie tylko do odczytu, co oznacza jest za późno skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="a0820-170">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="a0820-171">**Szybkość danych treści żądania minimalna**</span><span class="sxs-lookup"><span data-stu-id="a0820-171">**Minimum request body data rate**</span></span>

[<span data-ttu-id="a0820-172">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="a0820-172">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="a0820-173">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="a0820-173">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="a0820-174">Kestrel sprawdza co sekundę, jeśli dane jest otrzymywanych określonej szybkości w bajtach na sekundę.</span><span class="sxs-lookup"><span data-stu-id="a0820-174">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="a0820-175">Jeżeli stawki spadnie poniżej wartości minimalnej, jest upłynął limit czasu połączenia. Okres prolongaty wynosi ilość czasu, że Kestrel daje klientowi zwiększyć jego częstotliwość wysyłania do minimum; wskaźnik nie jest zaznaczone, w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="a0820-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="a0820-176">Okres prolongaty pomaga uniknąć porzucenie połączeń, które początkowo wysyłania danych prędkością wolne ze względu na wolne TCP-start.</span><span class="sxs-lookup"><span data-stu-id="a0820-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="a0820-177">Minimalna szybkość domyślna jest 240 bajtów na sekundę z 5-sekundowego okres prolongaty.</span><span class="sxs-lookup"><span data-stu-id="a0820-177">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="a0820-178">Minimalna częstotliwość ma również zastosowanie do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a0820-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="a0820-179">Kod, aby ustawić limit żądań i odpowiedzi limit jest taki sam, z wyjątkiem o `RequestBody` lub `Response` w nazwach właściwości i interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a0820-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="a0820-180">Oto przykład pokazujący sposób konfigurowania stawki minimalną ilość danych w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a0820-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-7)]

<span data-ttu-id="a0820-181">Stawki na żądanie można skonfigurować w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="a0820-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="a0820-182">Aby uzyskać informacje o innych opcjach Kestrel i limity zobacz:</span><span class="sxs-lookup"><span data-stu-id="a0820-182">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="a0820-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="a0820-183">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="a0820-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="a0820-184">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="a0820-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="a0820-185">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a0820-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a0820-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="a0820-187">Aby uzyskać informacji o opcjach Kestrel i limity zobacz:</span><span class="sxs-lookup"><span data-stu-id="a0820-187">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="a0820-188">Klasa KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="a0820-188">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="a0820-189">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="a0820-189">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a><span data-ttu-id="a0820-190">Konfiguracja punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="a0820-190">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a0820-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a0820-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="a0820-192">Domyślnie program ASP.NET Core wiąże `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a0820-192">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="a0820-193">Wywołanie [nasłuchiwania](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) lub [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metod [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) do konfiguracji prefiksy URL i portów dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a0820-193">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="a0820-194">`UseUrls`, `--urls` argumentu wiersza polecenia `urls` klucz konfiguracji hosta, a `ASPNETCORE_URLS` zmiennej środowiskowej również służbowych, ale ma ograniczenia później wymienionych w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="a0820-194">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="a0820-195">`urls` Klucz konfiguracji hosta muszą pochodzić z konfiguracją hosta, nie konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0820-195">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="a0820-196">Dodawanie `urls` klucza i wartości do *appsettings.json* nie wpływa na konfigurację hosta, ponieważ host jest w pełni zainicjowany w czasie konfiguracji zostanie odczytany z pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="a0820-196">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="a0820-197">Jednak `urls` klucza w *appsettings.json* mogą być używane z [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) na Konstruktor hosta, aby skonfigurować hosta:</span><span class="sxs-lookup"><span data-stu-id="a0820-197">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

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

<span data-ttu-id="a0820-198">Domyślnie program ASP.NET Core wiąże:</span><span class="sxs-lookup"><span data-stu-id="a0820-198">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="a0820-199">`https://localhost:5001` (jeśli lokalne działania projektowe certyfikat znajduje się)</span><span class="sxs-lookup"><span data-stu-id="a0820-199">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="a0820-200">Utworzono certyfikatu deweloperskiego:</span><span class="sxs-lookup"><span data-stu-id="a0820-200">A development certificate is created:</span></span>

* <span data-ttu-id="a0820-201">Gdy [.NET Core SDK](/dotnet/core/sdk) jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="a0820-201">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="a0820-202">[Narzędzie certyfikaty deweloperów](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs) służy do tworzenia certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="a0820-202">The [dev-certs tool](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs) is used to create a certificate.</span></span>

<span data-ttu-id="a0820-203">Niektóre przeglądarki wymagają przyznanie jawnym uprawnieniem do przeglądarki do ufać certyfikatowi rozwoju lokalnego.</span><span class="sxs-lookup"><span data-stu-id="a0820-203">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="a0820-204">Platformy ASP.NET Core 2.1 i nowsze szablony projektów Konfigurowanie aplikacji są domyślnie uruchamiane na HTTPS i zawierać [obsługuje przekierowania protokołu HTTPS i HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="a0820-204">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="a0820-205">Wywołanie [nasłuchiwania](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) lub [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metod [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) do konfiguracji prefiksy URL i portów dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a0820-205">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="a0820-206">`UseUrls`, `--urls` argumentu wiersza polecenia `urls` klucz konfiguracji hosta, a `ASPNETCORE_URLS` zmiennej środowiskowej również służbowych, ale ma ograniczenia później wymienionych w tej sekcji (domyślny certyfikat musi być dostępny dla punktu końcowego protokołu HTTPS Konfiguracja).</span><span class="sxs-lookup"><span data-stu-id="a0820-206">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="a0820-207">Platformy ASP.NET Core 2.1 `KestrelServerOptions` konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="a0820-207">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

<span data-ttu-id="a0820-208">**ConfigureEndpointDefaults (akcji&lt;ListenOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="a0820-208">**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**</span></span>  
<span data-ttu-id="a0820-209">Określa konfigurację `Action` do uruchamiania dla każdego określonego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="a0820-209">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="a0820-210">Wywoływanie `ConfigureEndpointDefaults` wielokrotnie zastępuje przed `Action`s w ostatnich `Action` określony.</span><span class="sxs-lookup"><span data-stu-id="a0820-210">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="a0820-211">**ConfigureHttpsDefaults (akcji&lt;HttpsConnectionAdapterOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="a0820-211">**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**</span></span>  
<span data-ttu-id="a0820-212">Określa konfigurację `Action` do uruchamiania dla każdego punktu końcowego protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a0820-212">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="a0820-213">Wywoływanie `ConfigureHttpsDefaults` wielokrotnie zastępuje przed `Action`s w ostatnich `Action` określony.</span><span class="sxs-lookup"><span data-stu-id="a0820-213">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="a0820-214">**Configure(IConfiguration)**</span><span class="sxs-lookup"><span data-stu-id="a0820-214">**Configure(IConfiguration)**</span></span>  
<span data-ttu-id="a0820-215">Tworzy ładujący konfiguracji, do konfigurowania Kestrel, która przyjmuje [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) jako dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="a0820-215">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="a0820-216">Konfiguracji należy określić zakres sekcji konfiguracyjnej dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a0820-216">The configuration must be scoped to the configuration section for Kestrel.</span></span>

<span data-ttu-id="a0820-217">**ListenOptions.UseHttps**</span><span class="sxs-lookup"><span data-stu-id="a0820-217">**ListenOptions.UseHttps**</span></span>  
<span data-ttu-id="a0820-218">Skonfiguruj Kestrel do używania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a0820-218">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="a0820-219">`ListenOptions.UseHttps` rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="a0820-219">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="a0820-220">`UseHttps` &ndash; Skonfiguruj Kestrel do używania protokołu HTTPS z domyślnego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="a0820-220">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="a0820-221">Zgłasza wyjątek, jeśli jest skonfigurowany żaden certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="a0820-221">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="a0820-222">`ListenOptions.UseHttps` Parametry:</span><span class="sxs-lookup"><span data-stu-id="a0820-222">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="a0820-223">`filename` jest ścieżka i nazwa pliku certyfikatu względem katalogu, który zawiera pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0820-223">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="a0820-224">`password` to hasło wymagane do uzyskania dostępu do danych certyfikatu X.509.</span><span class="sxs-lookup"><span data-stu-id="a0820-224">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="a0820-225">`configureOptions` jest `Action` skonfigurować `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="a0820-225">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="a0820-226">Zwraca `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="a0820-226">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="a0820-227">`storeName` jest magazyn certyfikatów, z którego można załadować certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="a0820-227">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="a0820-228">`subject` jest to nazwa podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="a0820-228">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="a0820-229">`allowInvalid` Wskazuje, czy nieprawidłowe certyfikaty należy rozważyć, takich jak certyfikaty z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="a0820-229">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="a0820-230">`location` jest to lokalizacja magazynu można załadować certyfikatu z.</span><span class="sxs-lookup"><span data-stu-id="a0820-230">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="a0820-231">`serverCertificate` jest to certyfikat X.509.</span><span class="sxs-lookup"><span data-stu-id="a0820-231">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="a0820-232">W środowisku produkcyjnym należy jawnie skonfigurować HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a0820-232">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="a0820-233">Co najmniej należy podać certyfikat domyślny.</span><span class="sxs-lookup"><span data-stu-id="a0820-233">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="a0820-234">Obsługiwane konfiguracje opisane w dalszej części:</span><span class="sxs-lookup"><span data-stu-id="a0820-234">Supported configurations described next:</span></span>

* <span data-ttu-id="a0820-235">Brak konfiguracji</span><span class="sxs-lookup"><span data-stu-id="a0820-235">No configuration</span></span>
* <span data-ttu-id="a0820-236">Zastąp domyślnego certyfikatu z konfiguracji</span><span class="sxs-lookup"><span data-stu-id="a0820-236">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="a0820-237">Zmiana ustawień domyślnych w kodzie</span><span class="sxs-lookup"><span data-stu-id="a0820-237">Change the defaults in code</span></span>

<span data-ttu-id="a0820-238">*Brak konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="a0820-238">*No configuration*</span></span>

<span data-ttu-id="a0820-239">Nasłuchuje kestrel `http://localhost:5000` i `https://localhost:5001` (jeśli cert domyślny jest dostępna).</span><span class="sxs-lookup"><span data-stu-id="a0820-239">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="a0820-240">Określ adresy URL przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="a0820-240">Specify URLs using the:</span></span>

* <span data-ttu-id="a0820-241">`ASPNETCORE_URLS` Zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="a0820-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="a0820-242">`--urls` argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="a0820-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="a0820-243">`urls` Klucz konfiguracji hosta.</span><span class="sxs-lookup"><span data-stu-id="a0820-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="a0820-244">`UseUrls` metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="a0820-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="a0820-245">Aby uzyskać więcej informacji, zobacz [adresów URL serwera](xref:fundamentals/host/web-host#server-urls) i [zastępczą konfigurację](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="a0820-245">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="a0820-246">Wartość podana przy użyciu tych metod może być co najmniej jeden protokół HTTP i HTTPS punktów końcowych (HTTPS jeśli cert domyślny jest dostępna).</span><span class="sxs-lookup"><span data-stu-id="a0820-246">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="a0820-247">Skonfiguruj wartość jako lista Rozdzielana średnikami (na przykład `"Urls": "http://localhost:8000;http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="a0820-247">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="a0820-248">*Zastąp domyślnego certyfikatu z konfiguracji*</span><span class="sxs-lookup"><span data-stu-id="a0820-248">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="a0820-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie załadować Kestrel konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="a0820-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="a0820-250">Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępna dla Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a0820-250">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="a0820-251">Skonfiguruj wiele punktów końcowych, w tym adresy URL i certyfikaty do użycia z pliku na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="a0820-251">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="a0820-252">W następujących *appsettings.json* przykład:</span><span class="sxs-lookup"><span data-stu-id="a0820-252">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="a0820-253">Ustaw **AllowInvalid** do `true` Aby zezwolić na korzystanie z certyfikatów nieprawidłowe (na przykład certyfikaty z podpisem własnym).</span><span class="sxs-lookup"><span data-stu-id="a0820-253">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="a0820-254">Punkt końcowy HTTPS, którego nie można określić certyfikatu (**HttpsDefaultCert** w następującym przykładzie) powraca do certyfikatu zdefiniowane w obszarze **certyfikaty** > **domyślne**  lub certyfikatu deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="a0820-254">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="a0820-255">Zamiast **ścieżki** i **hasło** dla każdego certyfikatu węzła jest określenie certyfikatu przy użyciu pól magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="a0820-255">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="a0820-256">Na przykład **certyfikaty** > **domyślne** certyfikatu może być określona jako:</span><span class="sxs-lookup"><span data-stu-id="a0820-256">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="a0820-257">Informacje o schemacie:</span><span class="sxs-lookup"><span data-stu-id="a0820-257">Schema notes:</span></span>

* <span data-ttu-id="a0820-258">Nazwy punktów końcowych jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="a0820-258">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="a0820-259">Na przykład `HTTPS` i `Https` są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="a0820-259">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="a0820-260">`Url` Parametr jest wymagany dla każdego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="a0820-260">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="a0820-261">Format dla tego parametru jest taka sama jak lokacja najwyższego poziomu `Urls` parametru konfiguracji, ale na maksymalnie jedną wartość.</span><span class="sxs-lookup"><span data-stu-id="a0820-261">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="a0820-262">Te punkty końcowe zastąpienia zdefiniowane w najwyższego poziomu `Urls` konfiguracji, zamiast dodawania do nich.</span><span class="sxs-lookup"><span data-stu-id="a0820-262">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="a0820-263">Punkty końcowe zdefiniowane w kodzie za pomocą `Listen` kumulują się z punktami końcowymi, zdefiniowane w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="a0820-263">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="a0820-264">`Certificate` Sekcja jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="a0820-264">The `Certificate` section is optional.</span></span> <span data-ttu-id="a0820-265">Jeśli `Certificate` sekcji nie jest określony, będą używane wartości domyślne, zdefiniowane w scenariuszach wcześniej.</span><span class="sxs-lookup"><span data-stu-id="a0820-265">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="a0820-266">Jeśli domyślne nie są dostępne, serwer zgłasza wyjątek i nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="a0820-266">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="a0820-267">`Certificate` Sekcji obsługuje zarówno **ścieżki**&ndash;**hasło** i **podmiotu**&ndash;**magazynu** certyfikaty.</span><span class="sxs-lookup"><span data-stu-id="a0820-267">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="a0820-268">W ten sposób może być zdefiniowana dowolną liczbę punktów końcowych, tak długo, jak ich nie powodują konfliktów portów.</span><span class="sxs-lookup"><span data-stu-id="a0820-268">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="a0820-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` Zwraca `KestrelConfigurationLoader` z `.Endpoint(string name, options => { })` metodę, która może służyć do uzupełnienia ustawienia skonfigurowanego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="a0820-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="a0820-270">Można również uzyskać dostęp do `KestrelServerOptions.ConfigurationLoader` Aby zachować iteracja na istniejący moduł ładujący, takie jak udostępnione przez [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="a0820-270">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="a0820-271">Jest dostępna w opcjach dostępnych w sekcji konfiguracyjnej dla każdego punktu końcowego `Endpoint` metody, dzięki czemu mogą być odczytywane ustawienia niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="a0820-271">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="a0820-272">Wiele konfiguracji mogą być ładowane przez wywołanie metody `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` ponownie, używając innej sekcji.</span><span class="sxs-lookup"><span data-stu-id="a0820-272">Multiple configurations may be loaded by calling `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="a0820-273">Ostatniej konfiguracji jest używana, chyba że `Load` jawnie jest wywoływana w poprzednich wystąpień.</span><span class="sxs-lookup"><span data-stu-id="a0820-273">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="a0820-274">Wywołanie nie metapackage `Load` , dzięki czemu można zastąpić jego sekcji konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="a0820-274">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="a0820-275">`KestrelConfigurationLoader` wstecznych `Listen` rodziny interfejsów API z `KestrelServerOptions` jako `Endpoint` overloads, więc punktów końcowych kodu i konfiguracji mogą być konfigurowane w tym samym miejscu.</span><span class="sxs-lookup"><span data-stu-id="a0820-275">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="a0820-276">Te przeciążenia nie używaj nazw i tylko używać domyślnych ustawień konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="a0820-276">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="a0820-277">*Zmiana ustawień domyślnych w kodzie*</span><span class="sxs-lookup"><span data-stu-id="a0820-277">*Change the defaults in code*</span></span>

<span data-ttu-id="a0820-278">`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` służy do zmiany ustawień domyślnych dla `ListenOptions` i `HttpsConnectionAdapterOptions`, tym przesłanianie domyślnego certyfikatu określonej w poprzednich scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="a0820-278">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="a0820-279">`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` powinna być wywoływana przed żadnych punktów końcowych są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="a0820-279">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="a0820-280">*Obsługa kestrel SNI*</span><span class="sxs-lookup"><span data-stu-id="a0820-280">*Kestrel support for SNI*</span></span>

<span data-ttu-id="a0820-281">[Wskazuje nazwę serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do obsługi wielu domen na tego samego adresu IP i portu.</span><span class="sxs-lookup"><span data-stu-id="a0820-281">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="a0820-282">Dla SNI funkcji Klient wysyła nazwę hosta dla bezpiecznej sesji na serwerze podczas uzgadniania TLS, dzięki czemu serwer może dostarczyć prawidłowego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="a0820-282">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="a0820-283">Klient używa certyfikatu złożonych szyfrowaną komunikację z serwerem podczas nawiązywania bezpiecznej sesji znajdujący się na uzgadnianie protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="a0820-283">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="a0820-284">Kestrel obsługuje SNI za pośrednictwem `ServerCertificateSelector` wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="a0820-284">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="a0820-285">Wywołanie zwrotne jest wywoływana raz dla każdego połączenia, aby umożliwić aplikacji Sprawdź nazwę hosta, a następnie wybierz odpowiedni certyfikat.</span><span class="sxs-lookup"><span data-stu-id="a0820-285">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="a0820-286">Obsługa SNI wymaga uruchomiona na platformie docelowej `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="a0820-286">SNI support requires running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="a0820-287">Na `netcoreapp2.0` i `net461`, wywołania zwrotnego jest wywoływane, ale `name` jest zawsze `null`.</span><span class="sxs-lookup"><span data-stu-id="a0820-287">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="a0820-288">`name` Jest również `null` Jeśli klient nie udostępnia hosta nazwę parametru w uzgadniania TLS.</span><span class="sxs-lookup"><span data-stu-id="a0820-288">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>

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
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
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

<span data-ttu-id="a0820-289">**Powiązać gniazda TCP**</span><span class="sxs-lookup"><span data-stu-id="a0820-289">**Bind to a TCP socket**</span></span>

<span data-ttu-id="a0820-290">[Nasłuchiwania](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) metody wiąże gniazda TCP i zezwala na lambda opcje konfiguracji certyfikatu SSL:</span><span class="sxs-lookup"><span data-stu-id="a0820-290">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="a0820-291">Przykład konfiguruje SSL dla punktu końcowego z [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="a0820-291">The example configures SSL for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="a0820-292">Skonfiguruj inne ustawienia Kestrel dla określonych punktów końcowych za pomocą tego samego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="a0820-292">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

<span data-ttu-id="a0820-293">**Powiązany z gniazdem systemu Unix**</span><span class="sxs-lookup"><span data-stu-id="a0820-293">**Bind to a Unix socket**</span></span>

<span data-ttu-id="a0820-294">Nasłuchiwanie na gnieździe Unix z [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) zwiększonej wydajności z Nginx, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="a0820-294">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="a0820-295">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="a0820-295">**Port 0**</span></span>

<span data-ttu-id="a0820-296">Jeśli numer portu `0` określono Kestrel dynamicznie wiąże dostępnego portu.</span><span class="sxs-lookup"><span data-stu-id="a0820-296">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="a0820-297">Poniższy przykład przedstawia sposób określić port, który Kestrel faktycznie powiązana w czasie wykonywania:</span><span class="sxs-lookup"><span data-stu-id="a0820-297">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Port0&highlight=3)]

<span data-ttu-id="a0820-298">Gdy aplikacja jest uruchamiana, dane wyjściowe z okna konsoli wskazuje port dynamiczny, gdzie można połączyć aplikacji:</span><span class="sxs-lookup"><span data-stu-id="a0820-298">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="a0820-299">**UseUrls, argument wiersza polecenia — adresów URL, klucz konfiguracji hosta adresy URL i ograniczenia zmiennej środowiskowej ASPNETCORE_URLS**</span><span class="sxs-lookup"><span data-stu-id="a0820-299">**UseUrls, --urls command-line argument, urls host configuration key, and ASPNETCORE_URLS environment variable limitations**</span></span>

<span data-ttu-id="a0820-300">Skonfiguruj punkty końcowe z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="a0820-300">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="a0820-301">UseUrls</span><span class="sxs-lookup"><span data-stu-id="a0820-301">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="a0820-302">`--urls` Argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="a0820-302">`--urls` command-line argument</span></span>
* <span data-ttu-id="a0820-303">`urls` Klucz konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="a0820-303">`urls` host configuration key</span></span>
* <span data-ttu-id="a0820-304">`ASPNETCORE_URLS` Zmienna środowiskowa</span><span class="sxs-lookup"><span data-stu-id="a0820-304">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="a0820-305">Te metody są przydatne w przypadku wprowadzania kodu pracy z serwerami innych niż Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a0820-305">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="a0820-306">Jednak należy pamiętać o tych ograniczeniach:</span><span class="sxs-lookup"><span data-stu-id="a0820-306">However, be aware of these limitations:</span></span>

* <span data-ttu-id="a0820-307">SSL nie można używać z tych metod, chyba że domyślny certyfikat znajduje się w konfiguracji punktu końcowego protokołu HTTPS (na przykład za pomocą `KestrelServerOptions` konfiguracji lub plik konfiguracji, jak pokazano wcześniej w tym temacie).</span><span class="sxs-lookup"><span data-stu-id="a0820-307">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="a0820-308">Gdy zarówno `Listen` i `UseUrls` podejścia są używane jednocześnie, `Listen` zastąpienie punktów końcowych `UseUrls` punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="a0820-308">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="a0820-309">**Konfiguracja punktu końcowego IIS**</span><span class="sxs-lookup"><span data-stu-id="a0820-309">**IIS endpoint configuration**</span></span>

<span data-ttu-id="a0820-310">Korzystając z usług IIS, powiązania adres URL dla zastąpienia IIS powiązania są ustawiane przez `Listen` lub `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="a0820-310">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="a0820-311">Aby uzyskać więcej informacji, zobacz [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) tematu.</span><span class="sxs-lookup"><span data-stu-id="a0820-311">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a0820-312">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a0820-312">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="a0820-313">Domyślnie program ASP.NET Core wiąże `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a0820-313">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="a0820-314">Skonfiguruj prefiksy URL i portów dla Kestrel przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="a0820-314">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="a0820-315">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) — metoda rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="a0820-315">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="a0820-316">`--urls` Argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="a0820-316">`--urls` command-line argument</span></span>
* <span data-ttu-id="a0820-317">`urls` Klucz konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="a0820-317">`urls` host configuration key</span></span>
* <span data-ttu-id="a0820-318">System konfiguracji platformy ASP.NET Core, w tym `ASPNETCORE_URLS` zmienna środowiskowa</span><span class="sxs-lookup"><span data-stu-id="a0820-318">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="a0820-319">Aby uzyskać więcej informacji na temat tych metod, zobacz [hostingu](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="a0820-319">For more information on these methods, see [Hosting](xref:fundamentals/host/index).</span></span>

<span data-ttu-id="a0820-320">**Konfiguracja punktu końcowego IIS**</span><span class="sxs-lookup"><span data-stu-id="a0820-320">**IIS endpoint configuration**</span></span>

<span data-ttu-id="a0820-321">Podczas korzystania z usług IIS, powiązania adres URL dla usług IIS zastąpienie powiązania ustawiony `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="a0820-321">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="a0820-322">Aby uzyskać więcej informacji, zobacz [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) tematu.</span><span class="sxs-lookup"><span data-stu-id="a0820-322">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a><span data-ttu-id="a0820-323">Konfiguracja transportu</span><span class="sxs-lookup"><span data-stu-id="a0820-323">Transport configuration</span></span>

<span data-ttu-id="a0820-324">Od wersji platformy ASP.NET Core 2.1 Kestrel przez domyślny transport jest już oparta na Libuv, ale zamiast tego oparty na zarządzanych gniazda.</span><span class="sxs-lookup"><span data-stu-id="a0820-324">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="a0820-325">Jest to istotne zmiany dla aplikacji programu ASP.NET 2.0 Core uaktualniania 2.1, który [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) i zależą od jednej z następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="a0820-325">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="a0820-326">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (bezpośrednie odwołanie do pakietu)</span><span class="sxs-lookup"><span data-stu-id="a0820-326">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="a0820-327">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="a0820-327">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="a0820-328">Dla platformy ASP.NET Core 2.1 lub nowszej projektów używające [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) i wymagają użycia Libuv:</span><span class="sxs-lookup"><span data-stu-id="a0820-328">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="a0820-329">Dodawanie zależności dla [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) pakietu aplikacji w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="a0820-329">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="2.1.0" />
    ```

* <span data-ttu-id="a0820-330">Wywołanie [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span><span class="sxs-lookup"><span data-stu-id="a0820-330">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="a0820-331">Prefiksy adresów URL</span><span class="sxs-lookup"><span data-stu-id="a0820-331">URL prefixes</span></span>

<span data-ttu-id="a0820-332">Korzystając z `UseUrls`, `--urls` argumentu wiersza polecenia `urls` klucz konfiguracji hosta, lub `ASPNETCORE_URLS` zmiennej środowiskowej prefiksy URL może być w dowolnym z następujących formatów.</span><span class="sxs-lookup"><span data-stu-id="a0820-332">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a0820-333">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a0820-333">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="a0820-334">Prawidłowe są tylko prefiksy URL protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0820-334">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="a0820-335">Kestrel nie obsługuje protokołu SSL podczas konfigurowania powiązania adresu URL za pomocą `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="a0820-335">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="a0820-336">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="a0820-336">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="a0820-337">`0.0.0.0` jest szczególnych przypadkach, która jest powiązana z wszystkich adresów IPv4.</span><span class="sxs-lookup"><span data-stu-id="a0820-337">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="a0820-338">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="a0820-338">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="a0820-339">`[::]` jest to równoważne IPv6 IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="a0820-339">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="a0820-340">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="a0820-340">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="a0820-341">Nazwy, hostów `*`, i `+`, nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="a0820-341">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="a0820-342">Niczego nie rozpoznany jako prawidłowy adres IP lub `localhost` wiąże wszystkich protokołów IPv4 i IPv6, adresy IP.</span><span class="sxs-lookup"><span data-stu-id="a0820-342">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="a0820-343">Aby powiązać różne nazwy hostów z różnych aplikacji platformy ASP.NET Core w tym samym porcie, użyj [HTTP.sys](xref:fundamentals/servers/httpsys) lub serwer zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS.</span><span class="sxs-lookup"><span data-stu-id="a0820-343">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="a0820-344">Jeśli nie używa zwrotny serwer proxy z hostem Filtrowanie włączone, Włącz [filtrowania host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="a0820-344">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="a0820-345">Host `localhost` nazwa portu numer lub sprzężenia zwrotnego protokołu IP z numerem portu</span><span class="sxs-lookup"><span data-stu-id="a0820-345">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="a0820-346">Gdy `localhost` określono Kestrel próbuje powiązać interfejsy sprzężenia zwrotnego protokołów IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="a0820-346">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="a0820-347">Jeśli żądany port jest używany przez inną usługę albo interfejsu sprzężenia zwrotnego, Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="a0820-347">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="a0820-348">Jeśli z jakiegokolwiek powodu albo interfejsu sprzężenia zwrotnego jest niedostępny (większość często, ponieważ nie jest obsługiwany protokół IPv6), Kestrel dzienniki ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="a0820-348">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a0820-349">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a0820-349">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="a0820-350">Adres IPv4 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="a0820-350">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="a0820-351">`0.0.0.0` jest szczególnych przypadkach, która jest powiązana z wszystkich adresów IPv4.</span><span class="sxs-lookup"><span data-stu-id="a0820-351">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="a0820-352">Adres IPv6 z numerem portu</span><span class="sxs-lookup"><span data-stu-id="a0820-352">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="a0820-353">`[::]` jest to równoważne IPv6 IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="a0820-353">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="a0820-354">Nazwa hosta z numerem portu</span><span class="sxs-lookup"><span data-stu-id="a0820-354">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="a0820-355">Nazwy, hostów `*`, i `+` nie są specjalne.</span><span class="sxs-lookup"><span data-stu-id="a0820-355">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="a0820-356">Wszystko, co nie jest rozpoznany adres IP lub `localhost` wiąże wszystkich protokołów IPv4 i IPv6, adresy IP.</span><span class="sxs-lookup"><span data-stu-id="a0820-356">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="a0820-357">Aby powiązać różne nazwy hostów z różnych aplikacji platformy ASP.NET Core w tym samym porcie, użyj [WebListener](xref:fundamentals/servers/weblistener) lub serwer zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS.</span><span class="sxs-lookup"><span data-stu-id="a0820-357">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="a0820-358">Host `localhost` nazwa portu numer lub sprzężenia zwrotnego protokołu IP z numerem portu</span><span class="sxs-lookup"><span data-stu-id="a0820-358">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="a0820-359">Gdy `localhost` określono Kestrel próbuje powiązać interfejsy sprzężenia zwrotnego protokołów IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="a0820-359">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="a0820-360">Jeśli żądany port jest używany przez inną usługę albo interfejsu sprzężenia zwrotnego, Kestrel nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="a0820-360">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="a0820-361">Jeśli z jakiegokolwiek powodu albo interfejsu sprzężenia zwrotnego jest niedostępny (większość często, ponieważ nie jest obsługiwany protokół IPv6), Kestrel dzienniki ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="a0820-361">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="a0820-362">Gniazda systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="a0820-362">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="a0820-363">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="a0820-363">**Port 0**</span></span>

<span data-ttu-id="a0820-364">Jeśli numer portu jest `0` określono Kestrel dynamicznie wiąże dostępnego portu.</span><span class="sxs-lookup"><span data-stu-id="a0820-364">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="a0820-365">Powiązanie z portu `0` jest dozwolona dla dowolnej nazwy hosta lub adres IP, z wyjątkiem `localhost`.</span><span class="sxs-lookup"><span data-stu-id="a0820-365">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="a0820-366">Gdy aplikacja jest uruchamiana, dane wyjściowe z okna konsoli wskazuje port dynamiczny, gdzie można połączyć aplikacji:</span><span class="sxs-lookup"><span data-stu-id="a0820-366">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="a0820-367">**Prefiksy adresów URL dla protokołu SSL**</span><span class="sxs-lookup"><span data-stu-id="a0820-367">**URL prefixes for SSL**</span></span>

<span data-ttu-id="a0820-368">Jeśli wywołanie `UseHttps` — metoda rozszerzenia, należy uwzględnić prefiksy URL z `https:`:</span><span class="sxs-lookup"><span data-stu-id="a0820-368">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

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
> <span data-ttu-id="a0820-369">HTTPS i HTTP nie może być umieszczona w tym samym porcie.</span><span class="sxs-lookup"><span data-stu-id="a0820-369">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a><span data-ttu-id="a0820-370">Filtrowanie hosta</span><span class="sxs-lookup"><span data-stu-id="a0820-370">Host filtering</span></span>

<span data-ttu-id="a0820-371">Gdy Kestrel obsługuje konfigurację oparte na prefiksy, takich jak `http://example.com:5000`, Kestrel przede wszystkim ignoruje nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="a0820-371">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="a0820-372">Host `localhost` jest szczególnych przypadkach używane do wiązania na adresy sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="a0820-372">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="a0820-373">Host żadnego, innych niż jawny adres IP jest powiązany z wszystkich publicznych adresów IP.</span><span class="sxs-lookup"><span data-stu-id="a0820-373">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="a0820-374">Żadne z tych informacji jest używany do sprawdzania poprawności żądania `Host` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="a0820-374">None of this information is used to validate request `Host` headers.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a0820-375">Jako rozwiązanie alternatywne hosta pod zwrotny serwer proxy z filtrowania nagłówka hosta.</span><span class="sxs-lookup"><span data-stu-id="a0820-375">As a workaround, host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="a0820-376">Jest to jedyny obsługiwany scenariusz Kestrel w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a0820-376">This is the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a0820-377">Jako obejście, za pomocą oprogramowania pośredniczącego Filtrowanie żądań przez `Host` nagłówka:</span><span class="sxs-lookup"><span data-stu-id="a0820-377">As a workaround, use middleware to filter requests by the `Host` header:</span></span>

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

<span data-ttu-id="a0820-378">Zarejestruj poprzedniego `HostFilteringMiddleware` w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a0820-378">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="a0820-379">Należy pamiętać, że [szeregowanie rejestracji oprogramowania pośredniczącego](xref:fundamentals/middleware/index#ordering) jest ważna.</span><span class="sxs-lookup"><span data-stu-id="a0820-379">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#ordering) is important.</span></span> <span data-ttu-id="a0820-380">Rejestracja powinna wystąpić natychmiast po rejestracji diagnostycznych oprogramowanie pośredniczące (na przykład `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="a0820-380">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

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

<span data-ttu-id="a0820-381">Oprogramowanie pośredniczące oczekuje `AllowedHosts` klucza w *appsettings.json*/*appsettings.\< EnvironmentName > JSON*.</span><span class="sxs-lookup"><span data-stu-id="a0820-381">The middleware expects an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="a0820-382">Wartość jest rozdzielaną średnikami listę nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="a0820-382">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a0820-383">Obejście tego problemu za pomocą oprogramowania pośredniczącego hosta filtrowania.</span><span class="sxs-lookup"><span data-stu-id="a0820-383">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="a0820-384">Oprogramowanie pośredniczące hosta filtrowania są dostarczane przez [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) pakietu, który jest dostępny w [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="a0820-384">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="a0820-385">Oprogramowanie pośredniczące jest dodawany przez [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), które wywołuje [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span><span class="sxs-lookup"><span data-stu-id="a0820-385">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

<span data-ttu-id="a0820-386">[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="a0820-386">[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]</span></span>

<span data-ttu-id="a0820-387">Oprogramowanie pośredniczące hosta filtrowanie jest domyślnie wyłączona.</span><span class="sxs-lookup"><span data-stu-id="a0820-387">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="a0820-388">Aby włączyć oprogramowanie pośredniczące, zdefiniuj `AllowedHosts` klucza w *appsettings.json*/*appsettings.\< EnvironmentName > JSON*.</span><span class="sxs-lookup"><span data-stu-id="a0820-388">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="a0820-389">Wartość jest rozdzielaną średnikami listę nazw hostów bez numerów portów:</span><span class="sxs-lookup"><span data-stu-id="a0820-389">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a0820-390">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="a0820-390">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="a0820-391">[Nagłówki oprogramowanie pośredniczące przekazane](xref:host-and-deploy/proxy-load-balancer) ma również [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) opcji.</span><span class="sxs-lookup"><span data-stu-id="a0820-391">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="a0820-392">Przekazany dalej oprogramowanie pośredniczące nagłówki i oprogramowanie pośredniczące hosta filtrowania mają podobne funkcje w różnych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="a0820-392">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="a0820-393">Ustawienie `AllowedHosts` z przekazywane oprogramowanie pośredniczące nagłówki jest odpowiednie, gdy nagłówek hosta nie jest zachowywane podczas przekazywania żądań z serwera zwrotnego serwera proxy lub usługę równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="a0820-393">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the Host header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="a0820-394">Ustawienie `AllowedHosts` z oprogramowania pośredniczącego hosta filtrowanie jest gdy Kestrel zostanie użyty jako serwer graniczny lub gdy nagłówek hosta był kierowany bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="a0820-394">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as an edge server or when the Host header is directly forwarded.</span></span>
>
> <span data-ttu-id="a0820-395">Aby uzyskać więcej informacji na przekazywane oprogramowanie pośredniczące nagłówków, zobacz [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="a0820-395">For more information on Forwarded Headers Middleware, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a0820-396">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a0820-396">Additional resources</span></span>

* [<span data-ttu-id="a0820-397">Wymuszanie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="a0820-397">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="a0820-398">Kod źródłowy kestrel</span><span class="sxs-lookup"><span data-stu-id="a0820-398">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* [<span data-ttu-id="a0820-399">RFC 7230: Składnia i Routing komunikatów (5.4 sekcji: Host)</span><span class="sxs-lookup"><span data-stu-id="a0820-399">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
* [<span data-ttu-id="a0820-400">Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="a0820-400">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
