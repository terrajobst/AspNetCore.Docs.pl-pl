---
title: Implementacje serwera sieci Web w programie ASP.NET Core
author: rick-anderson
description: Odnajdywanie serwerów sieci web w usługach Kestrel i sterownik HTTP.sys dla platformy ASP.NET Core. Dowiedz się, jak wybrać serwer i kiedy należy użyć zwrotnego serwera proxy.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 6b6ebbe9d31d571ea470fba0989d622dcf6e68af
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758209"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="d23ce-104">Implementacje serwera sieci Web w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d23ce-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="d23ce-105">Przez [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Halter Autor: Stephen](https://twitter.com/halter73), i [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="d23ce-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="d23ce-106">Aplikacji ASP.NET Core jest uruchamiany z implementację serwera HTTP w procesie.</span><span class="sxs-lookup"><span data-stu-id="d23ce-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="d23ce-107">Implementacja serwera nasłuchuje żądań HTTP i udostępnia je do aplikacji jako zestawów [funkcje na żądanie](xref:fundamentals/request-features) składające się na <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="d23ce-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="d23ce-108">Platforma ASP.NET Core jest dostarczana z następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="d23ce-108">ASP.NET Core ships with the following server implementations:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d23ce-109">[Kestrel](xref:fundamentals/servers/kestrel) jest to domyślne, serwer HTTP dla wielu platform dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d23ce-109">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="d23ce-110">`IISHttpServer` jest używana z [modelu hostingu w trakcie](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) i [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) na Windows.</span><span class="sxs-lookup"><span data-stu-id="d23ce-110">`IISHttpServer` is used with the [in-process hosting model](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) on Windows.</span></span>
* <span data-ttu-id="d23ce-111">[Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) serwera HTTP tylko do Windows opiera się na [sterownik jądra HTTP.sys i interfejsu API serwera HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="d23ce-111">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="d23ce-112">(Nosi nazwę HTTP.sys [WebListener](xref:fundamentals/servers/weblistener) w programie ASP.NET Core 1.x.)</span><span class="sxs-lookup"><span data-stu-id="d23ce-112">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="d23ce-113">[Kestrel](xref:fundamentals/servers/kestrel) jest to domyślne, serwer HTTP dla wielu platform dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d23ce-113">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="d23ce-114">[Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) serwera HTTP tylko do Windows opiera się na [sterownik jądra HTTP.sys i interfejsu API serwera HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="d23ce-114">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="d23ce-115">(Nosi nazwę HTTP.sys [WebListener](xref:fundamentals/servers/weblistener) w programie ASP.NET Core 1.x.)</span><span class="sxs-lookup"><span data-stu-id="d23ce-115">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="d23ce-116">Kestrel</span><span class="sxs-lookup"><span data-stu-id="d23ce-116">Kestrel</span></span>

<span data-ttu-id="d23ce-117">Kestrel jest domyślny serwer sieci web zawarte w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d23ce-117">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d23ce-118">Kestrel może służyć przez siebie lub za pomocą *zwrotnego serwera proxy*, na przykład serwer Nginx, Apache lub IIS.</span><span class="sxs-lookup"><span data-stu-id="d23ce-118">Kestrel can be used by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="d23ce-119">Zwrotnego serwera proxy odbiera żądania HTTP z Internetu i przekazuje je do Kestrel po niektórych wstępnego obsługi.</span><span class="sxs-lookup"><span data-stu-id="d23ce-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotny serwer proxy, na przykład serwer Nginx, Apache lub IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="d23ce-122">Każda konfiguracja&mdash;z lub bez serwera proxy odwrotnej&mdash;jest prawidłowy i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core 2.0 lub nowszej aplikacje.</span><span class="sxs-lookup"><span data-stu-id="d23ce-122">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="d23ce-123">Aby uzyskać więcej informacji, zobacz [kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="d23ce-123">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d23ce-124">Jeśli aplikacja będzie akceptować tylko żądania z siecią wewnętrzną, Kestrel może służyć przez siebie.</span><span class="sxs-lookup"><span data-stu-id="d23ce-124">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel komunikuje się bezpośrednio z siecią wewnętrzną](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="d23ce-126">Jeśli aplikacja jest połączenie z Internetem, Kestrel należy użyć usług IIS, serwera Nginx lub Apache jako *zwrotnego serwera proxy*.</span><span class="sxs-lookup"><span data-stu-id="d23ce-126">If the app is exposed to the Internet, Kestrel must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="d23ce-127">Zwrotnego serwera proxy odbiera żądania HTTP z Internetu i przekazuje je do Kestrel po niektórych wstępnego obsługi, jak pokazano na poniższym diagramie:</span><span class="sxs-lookup"><span data-stu-id="d23ce-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram:</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotny serwer proxy, na przykład serwer Nginx, Apache lub IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="d23ce-129">Najważniejszą przyczyną za używanie zwrotnego serwera proxy na potrzeby publicznego krawędzi serwera wdrożenia, które są udostępniane bezpośrednio przez Internet jest zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="d23ce-129">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="d23ce-130">Wersji 1.x Kestrel nie mają ważne funkcje zabezpieczeń do ochrony przed atakami z sieci Internet.</span><span class="sxs-lookup"><span data-stu-id="d23ce-130">The 1.x versions of Kestrel don't have important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="d23ce-131">Obejmuje, ale nie jest ograniczona do odpowiednie limity czasu, limity rozmiaru żądania i limity liczby jednoczesnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="d23ce-131">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="d23ce-132">Aby uzyskać więcej informacji, zobacz [kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="d23ce-132">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

<span data-ttu-id="d23ce-133">Nie można używać usług IIS, serwera Nginx i Apache bez Kestrel lub [implementacji niestandardowego serwera](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="d23ce-133">IIS, Nginx, and Apache can't be used without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="d23ce-134">ASP.NET Core zaprojektowano do uruchamiania w swoim własnym procesie, aby mogła działać spójnie na platformach.</span><span class="sxs-lookup"><span data-stu-id="d23ce-134">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="d23ce-135">Usługi IIS, serwera Nginx i Apache określają własne procedurę startową i środowiska.</span><span class="sxs-lookup"><span data-stu-id="d23ce-135">IIS, Nginx, and Apache dictate their own startup procedure and environment.</span></span> <span data-ttu-id="d23ce-136">Aby przy użyciu tych technologii server bezpośrednio, platformy ASP.NET Core potrzebować do dostosowania do wymagań poszczególnych serwerów.</span><span class="sxs-lookup"><span data-stu-id="d23ce-136">To use these server technologies directly, ASP.NET Core would need to adapt to the requirements of each server.</span></span> <span data-ttu-id="d23ce-137">Przy użyciu implementacji serwera sieci web, takich jak Kestrel, ASP.NET Core ma kontrolę nad procesu uruchamiania i środowiska, gdy hostowany na innym serwerze technologiach.</span><span class="sxs-lookup"><span data-stu-id="d23ce-137">Using a web server implementation, such as Kestrel, ASP.NET Core has control over the startup process and environment when hosted on different server technologies.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="d23ce-138">Usługi IIS za pomocą Kestrel</span><span class="sxs-lookup"><span data-stu-id="d23ce-138">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d23ce-139">Korzystając z [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), aplikacja platformy ASP.NET Core albo działa w tym samym procesie co proces roboczy usług IIS ( *w trakcie* model hostingu) lub w oddzielnym procesie z proces roboczy usług IIS ( *spoza procesu* model hostingu).</span><span class="sxs-lookup"><span data-stu-id="d23ce-139">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="d23ce-140">[Modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) jest macierzysty moduł usług IIS, który obsługuje natywne żądań usług IIS między serwer Http w procesie programu IIS lub serwera Kestrel spoza procesu.</span><span class="sxs-lookup"><span data-stu-id="d23ce-140">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS Http Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="d23ce-141">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="d23ce-141">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d23ce-142">Korzystając z [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) jako zwrotny serwer proxy dla platformy ASP.NET Core w aplikacji ASP.NET Core działa w procesie oddzielnie od proces roboczy usług IIS.</span><span class="sxs-lookup"><span data-stu-id="d23ce-142">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="d23ce-143">W procesie programu IIS [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) koordynuje relacji zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="d23ce-143">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="d23ce-144">Podstawowe funkcje modułu ASP.NET Core są do uruchamiania aplikacji ASP.NET Core, uruchom ponownie aplikację uległa awarii i kierują ruch HTTP do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d23ce-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="d23ce-145">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="d23ce-145">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="d23ce-146">Serwer Nginx z Kestrel</span><span class="sxs-lookup"><span data-stu-id="d23ce-146">Nginx with Kestrel</span></span>

<span data-ttu-id="d23ce-147">Aby uzyskać informacje dotyczące sposobu używania jako zwrotny serwer proxy serwera Nginx w systemie Linux dla Kestrel, zobacz [hosting w systemie Linux przy użyciu serwera Nginx](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="d23ce-147">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="d23ce-148">Apache z Kestrel</span><span class="sxs-lookup"><span data-stu-id="d23ce-148">Apache with Kestrel</span></span>

<span data-ttu-id="d23ce-149">Aby uzyskać informacje na temat sposobu na użytek Apache w systemie Linux jako serwer proxy odwrotnej Kestrel, zobacz [hosting w systemie Linux z Apache](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="d23ce-149">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="d23ce-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d23ce-150">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d23ce-151">Jeśli aplikacje platformy ASP.NET Core są uruchamiane na Windows, sterownik HTTP.sys stanowi alternatywę Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d23ce-151">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="d23ce-152">Ogólnie zaleca się kestrel w celu uzyskania najlepszej wydajności.</span><span class="sxs-lookup"><span data-stu-id="d23ce-152">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="d23ce-153">Sterownik HTTP.sys może służyć w scenariuszach, gdzie aplikacja jest połączenie z Internetem i wymagane możliwości są obsługiwane przez rozszerzenie HTTP.sys, ale nie Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d23ce-153">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="d23ce-154">Aby uzyskać informacji na temat HTTP.sys, zobacz [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="d23ce-154">For information on HTTP.sys, see [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetu](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="d23ce-156">Sterownik HTTP.sys może również dla aplikacji, które są dostępne tylko z siecią wewnętrzną.</span><span class="sxs-lookup"><span data-stu-id="d23ce-156">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![Sterownik HTTP.sys komunikuje się bezpośrednio z siecią wewnętrzną](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d23ce-158">Nosi nazwę HTTP.sys [WebListener](xref:fundamentals/servers/weblistener) w programie ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d23ce-158">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="d23ce-159">Jeśli aplikacje platformy ASP.NET Core są uruchamiane na Windows, WebListener jest alternatywą dla scenariuszy, w której usługi IIS nie jest dostępne dla aplikacji hosta.</span><span class="sxs-lookup"><span data-stu-id="d23ce-159">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![Weblistener komunikuje się bezpośrednio z Internetu](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="d23ce-161">WebListener można również zamiast Kestrel dla aplikacji, które są dostępne tylko z siecią wewnętrzną, jeśli jest to wymagane, że funkcje są obsługiwane przez WebListener, ale nie Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d23ce-161">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="d23ce-162">Aby uzyskać informacji na temat WebListener, zobacz [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="d23ce-162">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![Weblistener komunikuje się bezpośrednio z siecią wewnętrzną](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="d23ce-164">Infrastruktura serwerowa ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d23ce-164">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="d23ce-165">[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) dostępne w `Startup.Configure` ujawnia metody [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) właściwości typu [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="d23ce-165">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="d23ce-166">Kestrel i sterownik HTTP.sys (WebListener w programie ASP.NET Core 1.x) udostępniają tylko jednej funkcji, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ale implementacji różnych serwera może udostępnić dodatkowe funkcje.</span><span class="sxs-lookup"><span data-stu-id="d23ce-166">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="d23ce-167">`IServerAddressesFeature` można dowiedzieć się, port, który implementacji serwera została powiązana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="d23ce-167">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="d23ce-168">Niestandardowe serwery</span><span class="sxs-lookup"><span data-stu-id="d23ce-168">Custom servers</span></span>

<span data-ttu-id="d23ce-169">Jeśli wbudowane serwery nie spełniają wymagań dotyczących aplikacji, można utworzyć wdrożenia niestandardowego serwera.</span><span class="sxs-lookup"><span data-stu-id="d23ce-169">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="d23ce-170">[Open Web Interface for .NET (OWIN) przewodnik](xref:fundamentals/owin) pokazuje, jak napisać [Nowin](https://github.com/Bobris/Nowin)— na podstawie [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementacji.</span><span class="sxs-lookup"><span data-stu-id="d23ce-170">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="d23ce-171">Tylko interfejsy funkcji, których używa aplikacja wymaga wdrożenia, jeśli co najmniej [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) i [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) muszą być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="d23ce-171">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="d23ce-172">Uruchamianie serwera</span><span class="sxs-lookup"><span data-stu-id="d23ce-172">Server startup</span></span>

<span data-ttu-id="d23ce-173">Korzystając z [programu Visual Studio](https://www.visualstudio.com/vs/), [programu Visual Studio dla komputerów Mac](https://www.visualstudio.com/vs/mac/), lub [programu Visual Studio Code](https://code.visualstudio.com/), serwer jest uruchamiany po uruchomieniu aplikacji przez (zintegrowane środowisko projektowe ŚRODOWISKO IDE).</span><span class="sxs-lookup"><span data-stu-id="d23ce-173">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="d23ce-174">W programie Visual Studio, Windows, profilów uruchamiania może służyć do uruchomienia aplikacji i serwera z oboma [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) lub konsoli.</span><span class="sxs-lookup"><span data-stu-id="d23ce-174">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="d23ce-175">W programie Visual Studio Code, aplikacji i serwera są uruchamiane przez [technologię Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), które aktywuje debugera CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="d23ce-175">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="d23ce-176">W programie Visual Studio dla komputerów Mac do aplikacji i serwera są uruchamiane przez [Mono nietrwałego Tryb debugera](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="d23ce-176">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="d23ce-177">Podczas uruchamiania aplikacji z poziomu wiersza polecenia w folderze projektu [dotnet, uruchom](/dotnet/core/tools/dotnet-run) spowoduje uruchomienie aplikacji i serwera (Kestrel i tylko w pliku HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="d23ce-177">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="d23ce-178">Konfiguracja jest określona przez `-c|--configuration` opcja, która jest ustawiona jako `Debug` (ustawienie domyślne) lub `Release`.</span><span class="sxs-lookup"><span data-stu-id="d23ce-178">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="d23ce-179">Jeśli profile uruchamiania są obecne w *launchSettings.json* pliku, użyj `--launch-profile <NAME>` opcję, aby ustawić profil uruchamiania (na przykład `Development` lub `Production`).</span><span class="sxs-lookup"><span data-stu-id="d23ce-179">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="d23ce-180">Aby uzyskać więcej informacji, zobacz [dotnet, uruchom](/dotnet/core/tools/dotnet-run) i [tworzenie pakietów dystrybucji platformy .NET Core](/dotnet/core/build/distribution-packaging) tematów.</span><span class="sxs-lookup"><span data-stu-id="d23ce-180">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="d23ce-181">Obsługa protokołu HTTP/2</span><span class="sxs-lookup"><span data-stu-id="d23ce-181">HTTP/2 support</span></span>

<span data-ttu-id="d23ce-182">[Protokołu HTTP/2](https://httpwg.org/specs/rfc7540.html) jest obsługiwana za pomocą programu ASP.NET Core w następujących scenariuszach wdrażania:</span><span class="sxs-lookup"><span data-stu-id="d23ce-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="d23ce-183">Kestrel</span><span class="sxs-lookup"><span data-stu-id="d23ce-183">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="d23ce-184">System operacyjny</span><span class="sxs-lookup"><span data-stu-id="d23ce-184">Operating system</span></span>
    * <span data-ttu-id="d23ce-185">Windows Server 2012 R2/Windows 8.1 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="d23ce-185">Windows Server 2012 R2/Windows 8.1 or later</span></span>
    * <span data-ttu-id="d23ce-186">Linux z protokołem OpenSSL 1.0.2 lub nowszej (na przykład Ubuntu 16.04 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="d23ce-186">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="d23ce-187">Protokołu HTTP/2 będą obsługiwane w systemie macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="d23ce-187">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="d23ce-188">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="d23ce-188">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="d23ce-189">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d23ce-189">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="d23ce-190">Windows Server 2016 i Windows 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="d23ce-190">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="d23ce-191">Platforma docelowa: nie dotyczy wdrożeń HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="d23ce-191">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="d23ce-192">Usługi IIS (w procesie)</span><span class="sxs-lookup"><span data-stu-id="d23ce-192">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="d23ce-193">Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="d23ce-193">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="d23ce-194">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="d23ce-194">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="d23ce-195">Usługi IIS (poza procesem)</span><span class="sxs-lookup"><span data-stu-id="d23ce-195">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="d23ce-196">Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="d23ce-196">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="d23ce-197">Połączenia z serwerem usługi edge publicznego używać protokołu HTTP/2, ale połączenie zwrotny serwer proxy Kestrel korzysta z protokołu HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="d23ce-197">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="d23ce-198">Platforma docelowa: nie dotyczy wdrożeń spoza procesu usług IIS.</span><span class="sxs-lookup"><span data-stu-id="d23ce-198">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="d23ce-199">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d23ce-199">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="d23ce-200">Windows Server 2016 i Windows 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="d23ce-200">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="d23ce-201">Platforma docelowa: nie dotyczy wdrożeń HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="d23ce-201">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="d23ce-202">Usługi IIS (poza procesem)</span><span class="sxs-lookup"><span data-stu-id="d23ce-202">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="d23ce-203">Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="d23ce-203">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="d23ce-204">Połączenia z serwerem usługi edge publicznego używać protokołu HTTP/2, ale połączenie zwrotny serwer proxy Kestrel korzysta z protokołu HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="d23ce-204">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="d23ce-205">Platforma docelowa: nie dotyczy wdrożeń spoza procesu usług IIS.</span><span class="sxs-lookup"><span data-stu-id="d23ce-205">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="d23ce-206">Należy użyć połączenia protokołu HTTP/2 [negocjowania protokołu warstwy aplikacji (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) i TLS 1.2 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="d23ce-206">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="d23ce-207">Aby uzyskać więcej informacji zobacz tematy, które odnoszą się do scenariuszy wdrażania serwera.</span><span class="sxs-lookup"><span data-stu-id="d23ce-207">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d23ce-208">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d23ce-208">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="d23ce-209"><xref:fundamentals/servers/httpsys> (dla platformy ASP.NET Core 1.x, zobacz <xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="d23ce-209"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
