---
title: Implementacje serwera sieci Web w ASP.NET Core
author: tdykstra
description: "Wprowadza serwerów sieci web Kestrel i WebListener dla platformy ASP.NET Core. Zawiera wskazówki dotyczące sposobu wybierz jedną i kiedy należy użyć jednej z zwrotnego serwera proxy."
keywords: Serwer sieci web platformy ASP.NET Core IServer, Kestrel, WebListener, odwrotny serwer proxy
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: dba74f39-58cd-4dee-a061-6d15f7346959
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: 04dee100dff91f7868175ff4be01156787e13e81
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="f7dad-105">Implementacje serwera sieci Web w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7dad-105">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="f7dad-106">Przez [Dykstra Tomasz](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), i [Roaming Krzysztof](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="f7dad-106">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="f7dad-107">Aplikacja platformy ASP.NET Core jest uruchamiana z implementację serwera HTTP w procesie.</span><span class="sxs-lookup"><span data-stu-id="f7dad-107">An ASP.NET Core application runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="f7dad-108">Nasłuchuje implementację serwera HTTP żądania i udostępnia je do aplikacji jako zestawy [żądania funkcji](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) składające się na `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="f7dad-108">The server implementation listens for HTTP requests and surfaces them to the application as sets of [request features](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composed into an `HttpContext`.</span></span>

<span data-ttu-id="f7dad-109">Platformy ASP.NET Core dostarczany dwóch implementacji serwera:</span><span class="sxs-lookup"><span data-stu-id="f7dad-109">ASP.NET Core ships two server implementations:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7dad-110">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7dad-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="f7dad-111">[Kestrel](kestrel.md) serwera HTTP i platform opiera się na [libuv](https://github.com/libuv/libuv), biblioteki i platform asynchroniczne We/Wy.</span><span class="sxs-lookup"><span data-stu-id="f7dad-111">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="f7dad-112">[Sterownik HTTP.sys](httpsys.md) serwera HTTP systemu Windows opiera się na [sterownik Http.Sys jądra](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7dad-112">[HTTP.sys](httpsys.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7dad-113">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7dad-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="f7dad-114">[Kestrel](kestrel.md) serwera HTTP i platform opiera się na [libuv](https://github.com/libuv/libuv), biblioteki i platform asynchroniczne We/Wy.</span><span class="sxs-lookup"><span data-stu-id="f7dad-114">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="f7dad-115">[WebListener](weblistener.md) serwera HTTP systemu Windows opiera się na [sterownik Http.Sys jądra](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7dad-115">[WebListener](weblistener.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

---

## <a name="kestrel"></a><span data-ttu-id="f7dad-116">kestrel</span><span class="sxs-lookup"><span data-stu-id="f7dad-116">Kestrel</span></span>

<span data-ttu-id="f7dad-117">Kestrel to serwer sieci web, który jest domyślnie włączone w szablonach nowy projekt platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f7dad-117">Kestrel is the web server that is included by default in ASP.NET Core new-project templates.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7dad-118">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7dad-118">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f7dad-119">Samodzielnie lub z użyciem Kestrel *zwrotnego serwera proxy*, takie jak usługi IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="f7dad-119">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="f7dad-120">Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi.</span><span class="sxs-lookup"><span data-stu-id="f7dad-120">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="f7dad-123">Albo konfiguracji &mdash; z lub bez zwrotnego serwera proxy &mdash; można również, czy Kestrel jest udostępniany tylko z siecią wewnętrzną.</span><span class="sxs-lookup"><span data-stu-id="f7dad-123">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

<span data-ttu-id="f7dad-124">Aby uzyskać informacje o tym, kiedy używać Kestrel z zwrotny serwer proxy, zobacz [wprowadzenie do Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="f7dad-124">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7dad-125">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7dad-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f7dad-126">Jeśli aplikacja akceptuje żądania tylko z siecią wewnętrzną, można użyć Kestrel przez samego siebie.</span><span class="sxs-lookup"><span data-stu-id="f7dad-126">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel komunikuje się bezpośrednio z sieci wewnętrznej](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="f7dad-128">Jeśli udostępnianie aplikacji z Internetem, należy użyć usług IIS, Nginx lub Apache jako *zwrotnego serwera proxy*.</span><span class="sxs-lookup"><span data-stu-id="f7dad-128">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="f7dad-129">Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi, jak pokazano na poniższym diagramie.</span><span class="sxs-lookup"><span data-stu-id="f7dad-129">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram.</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="f7dad-131">Najważniejsze przyczyny wdrożeń krawędzi (ujawniony na ruch z Internetu) przy użyciu zwrotnego serwera proxy jest zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="f7dad-131">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="f7dad-132">Wersje 1.x Kestrel nie mają pełny zestaw zabezpieczenia przed atakami.</span><span class="sxs-lookup"><span data-stu-id="f7dad-132">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="f7dad-133">Obejmuje, ale nie jest ograniczony do odpowiednich limitów czasu, limity rozmiaru i limity liczby jednoczesnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="f7dad-133">This includes, but isn't limited to, appropriate timeouts, size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="f7dad-134">Aby uzyskać informacje o tym, kiedy używać Kestrel z zwrotny serwer proxy, zobacz [wprowadzenie do Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="f7dad-134">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

---

<span data-ttu-id="f7dad-135">Nie można użyć usług IIS, Nginx lub Apache bez Kestrel lub [implementacji niestandardowego serwera](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="f7dad-135">You can't use IIS, Nginx, or Apache without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="f7dad-136">Platformy ASP.NET Core był przeznaczony do działania w swoim własnym procesie, tak, aby mogła działać spójnie na platformach.</span><span class="sxs-lookup"><span data-stu-id="f7dad-136">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="f7dad-137">Usługi IIS, Nginx i Apache dyktowania własne procesu uruchamiania i środowiska; z nich korzystać bezpośrednio, platformy ASP.NET Core musi dostosowania do potrzeb każdej z nich.</span><span class="sxs-lookup"><span data-stu-id="f7dad-137">IIS, Nginx, and Apache dictate their own startup process and environment; to use them directly, ASP.NET Core would have to adapt to the needs of each one.</span></span> <span data-ttu-id="f7dad-138">Przy użyciu implementacji serwera sieci web, takich jak Kestrel zapewnia platformy ASP.NET Core kontrolę nad procesu uruchamiania i środowiska.</span><span class="sxs-lookup"><span data-stu-id="f7dad-138">Using a web server implementation such as Kestrel gives ASP.NET Core control over the startup process and environment.</span></span> <span data-ttu-id="f7dad-139">Dlatego zamiast w trakcie dostosowania platformy ASP.NET Core internetowych usług informacyjnych, Nginx lub Apache, tylko konfigurowania tych serwerów sieci web do serwera proxy żądań Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f7dad-139">So rather than trying to adapt ASP.NET Core to IIS, Nginx, or Apache, you just set up those web servers to proxy requests to Kestrel.</span></span> <span data-ttu-id="f7dad-140">Umożliwia to rozmieszczenie Twojej `Program.Main` i `Startup` klasy były zasadniczo taki sam, niezależnie od tego, w którym jest wdrażany.</span><span class="sxs-lookup"><span data-stu-id="f7dad-140">This arrangement allows your `Program.Main` and `Startup` classes to be essentially the same no matter where you deploy.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="f7dad-141">Usługi IIS z Kestrel</span><span class="sxs-lookup"><span data-stu-id="f7dad-141">IIS with Kestrel</span></span>

<span data-ttu-id="f7dad-142">Korzystając z usług IIS lub usług IIS Express jako zwrotny serwer proxy dla platformy ASP.NET Core, aplikacji platformy ASP.NET Core uruchamia się w oddzielnej procesu z proces roboczy usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f7dad-142">When you use IIS or IIS Express as a reverse proxy for ASP.NET Core, the ASP.NET Core application runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="f7dad-143">W procesie IIS specjalne Moduł IIS uruchamia do koordynowania relacji zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="f7dad-143">In the IIS process, a special IIS module runs to coordinate the reverse proxy relationship.</span></span>  <span data-ttu-id="f7dad-144">Jest to *platformy ASP.NET Core modułu*.</span><span class="sxs-lookup"><span data-stu-id="f7dad-144">This is the *ASP.NET Core Module*.</span></span> <span data-ttu-id="f7dad-145">Podstawowe funkcje platformy ASP.NET Core modułu są do uruchamiania aplikacji platformy ASP.NET Core, uruchom go ponownie uległa awarii i przesyłania dalej ruchu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f7dad-145">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core application, restart it when it crashes, and forward HTTP traffic to it.</span></span> <span data-ttu-id="f7dad-146">Aby uzyskać więcej informacji, zobacz [platformy ASP.NET Core modułu](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="f7dad-146">For more information, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="f7dad-147">Nginx z Kestrel</span><span class="sxs-lookup"><span data-stu-id="f7dad-147">Nginx with Kestrel</span></span>

<span data-ttu-id="f7dad-148">Aby uzyskać informacje o sposobie używania Nginx w systemie Linux jako zwrotnego serwera proxy dla Kestrel, zobacz [publikowania w środowisku produkcyjnym Linux](../../publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="f7dad-148">For information about how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Publish to a Linux Production Environment](../../publishing/linuxproduction.md).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="f7dad-149">Apache z Kestrel</span><span class="sxs-lookup"><span data-stu-id="f7dad-149">Apache with Kestrel</span></span>

<span data-ttu-id="f7dad-150">Aby uzyskać informacje o sposobie używania Apache w systemie Linux jako zwrotnego serwera proxy dla Kestrel, zobacz [przy użyciu serwera sieci Web Apache jako zwrotny serwer proxy](../../publishing/apache-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="f7dad-150">For information about how to use Apache on Linux as a reverse proxy server for Kestrel, see [Using Apache Web Server as a reverse proxy](../../publishing/apache-proxy.md).</span></span>

## <a name="httpsys"></a><span data-ttu-id="f7dad-151">Sterownik HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="f7dad-151">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7dad-152">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7dad-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f7dad-153">Jeśli uruchamianie aplikacji platformy ASP.NET Core w systemie Windows, sterownik HTTP.sys stanowi alternatywę Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f7dad-153">If you run your ASP.NET Core app on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="f7dad-154">Sterownik HTTP.sys służy do scenariuszy, w którym udostępnianie aplikacji z Internetem i potrzebne funkcje HTTP.sys, które Kestrel nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="f7dad-154">You can use HTTP.sys for scenarios where you expose your app to the Internet and you need HTTP.sys features that Kestrel doesn't support.</span></span> 

![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetem](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="f7dad-156">Można także HTTP.sys dla aplikacji, które są dostępne tylko z siecią wewnętrzną.</span><span class="sxs-lookup"><span data-stu-id="f7dad-156">HTTP.sys can also be used for applications that are exposed only to an internal network.</span></span> 

![Sterownik HTTP.sys komunikuje się bezpośrednio z sieci wewnętrznej](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="f7dad-158">W przypadku scenariuszy sieci wewnętrznej Kestrel zazwyczaj jest zalecane w przypadku najlepszą wydajność; Jednak w niektórych scenariuszach, można użyć funkcji, która oferuje tylko w pliku HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="f7dad-158">For internal network scenarios, Kestrel is generally recommended for best performance; but in some scenarios, you might want to use a feature that only HTTP.sys offers.</span></span> <span data-ttu-id="f7dad-159">Aby uzyskać informacje o funkcjach HTTP.sys, zobacz [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="f7dad-159">For information about HTTP.sys features, see [HTTP.sys](httpsys.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7dad-160">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7dad-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f7dad-161">Sterownik HTTP.sys nosi nazwę WebListener w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="f7dad-161">HTTP.sys is named WebListener in ASP.NET Core 1.x.</span></span> <span data-ttu-id="f7dad-162">Po uruchomieniu programu ASP.NET Core aplikacji w systemie Windows, WebListener stanowi alternatywę, który służy do scenariuszy, w którym chcesz udostępnić aplikację do Internetu, ale nie można używać usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f7dad-162">If you run your ASP.NET Core app on Windows, WebListener is an alternative that you can use for scenarios where you want to expose your app to the Internet but you can't use IIS.</span></span>

![Weblistener komunikuje się bezpośrednio z Internetem](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="f7dad-164">WebListener można również zamiast Kestrel dla aplikacji, które są dostępne tylko z siecią wewnętrzną, jeśli potrzebujesz funkcji WebListener, które Kestrel nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="f7dad-164">WebListener can also be used in place of Kestrel for applications that are exposed only to an internal network, if you need WebListener features that Kestrel doesn't support.</span></span> 

![Weblistener komunikuje się bezpośrednio z sieci wewnętrznej](weblistener/_static/weblistener-to-internal.png)

<span data-ttu-id="f7dad-166">W przypadku scenariuszy sieci wewnętrznej ogólnie zaleca się Kestrel, aby uzyskać najlepszą wydajność, ale w niektórych sytuacjach możesz chcieć użyć funkcji, która oferuje tylko WebListener.</span><span class="sxs-lookup"><span data-stu-id="f7dad-166">For internal network scenarios, Kestrel is generally recommended for best performance, but in some scenarios you might want to use a feature that only WebListener offers.</span></span> <span data-ttu-id="f7dad-167">Aby uzyskać informacje o funkcjach WebListener, zobacz [WebListener](weblistener.md).</span><span class="sxs-lookup"><span data-stu-id="f7dad-167">For information about WebListener features, see [WebListener](weblistener.md).</span></span>

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a><span data-ttu-id="f7dad-168">Uwagi dotyczące infrastruktury serwera ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7dad-168">Notes about ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="f7dad-169">[ `IApplicationBuilder` ](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) Dostępne w `Startup` klasy `Configure` ujawnia metody `ServerFeatures` właściwości typu [ `IFeatureCollection` ](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="f7dad-169">The [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup` class `Configure` method exposes the `ServerFeatures` property of type [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="f7dad-170">Kestrel i WebListener uwidacznia tylko jednej funkcji [ `IServerAddressesFeature` ](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ale implementacje inny serwer może udostępniać dodatkowe funkcje.</span><span class="sxs-lookup"><span data-stu-id="f7dad-170">Kestrel and WebListener both expose only a single feature, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="f7dad-171">`IServerAddressesFeature`można sprawdzić, port, który ma powiązany implementacją serwera w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="f7dad-171">`IServerAddressesFeature` can be used to find out which port the server implementation has bound to at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="f7dad-172">Niestandardowe serwerów</span><span class="sxs-lookup"><span data-stu-id="f7dad-172">Custom servers</span></span>

<span data-ttu-id="f7dad-173">Jeśli wbudowane serwery nie spełniają potrzeb użytkownika, można utworzyć implementacji niestandardowego serwera.</span><span class="sxs-lookup"><span data-stu-id="f7dad-173">If the built-in servers don't meet your needs, you can create a custom server implementation.</span></span> <span data-ttu-id="f7dad-174">[Interfejsu Open Web dla platformy .NET (OWIN) przewodnik](../owin.md) pokazano, jak napisać [Nowin](https://github.com/Bobris/Nowin)— na podstawie [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementacji.</span><span class="sxs-lookup"><span data-stu-id="f7dad-174">The [Open Web Interface for .NET (OWIN) guide](../owin.md) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="f7dad-175">Można go dowolnie implementuje tylko interfejsy funkcji aplikacja wymaga, że co najmniej musi obsługiwać [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) i [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="f7dad-175">You're free to implement just the feature interfaces your application needs, though at a minimum you must support [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f7dad-176">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="f7dad-176">Next steps</span></span>

<span data-ttu-id="f7dad-177">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="f7dad-177">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7dad-178">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7dad-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

- [<span data-ttu-id="f7dad-179">Kestrel</span><span class="sxs-lookup"><span data-stu-id="f7dad-179">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="f7dad-180">Kestrel z usługami IIS</span><span class="sxs-lookup"><span data-stu-id="f7dad-180">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="f7dad-181">Kestrel z Nginx</span><span class="sxs-lookup"><span data-stu-id="f7dad-181">Kestrel with Nginx</span></span>](../../publishing/linuxproduction.md)
- [<span data-ttu-id="f7dad-182">Kestrel z Apache</span><span class="sxs-lookup"><span data-stu-id="f7dad-182">Kestrel with Apache</span></span>](../../publishing/apache-proxy.md)
- [<span data-ttu-id="f7dad-183">Sterownik HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="f7dad-183">HTTP.sys</span></span>](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7dad-184">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7dad-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

- [<span data-ttu-id="f7dad-185">Kestrel</span><span class="sxs-lookup"><span data-stu-id="f7dad-185">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="f7dad-186">Kestrel z usługami IIS</span><span class="sxs-lookup"><span data-stu-id="f7dad-186">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="f7dad-187">Kestrel z Nginx</span><span class="sxs-lookup"><span data-stu-id="f7dad-187">Kestrel with Nginx</span></span>](../../publishing/linuxproduction.md)
- [<span data-ttu-id="f7dad-188">Kestrel z Apache</span><span class="sxs-lookup"><span data-stu-id="f7dad-188">Kestrel with Apache</span></span>](../../publishing/apache-proxy.md)
- [<span data-ttu-id="f7dad-189">WebListener</span><span class="sxs-lookup"><span data-stu-id="f7dad-189">WebListener</span></span>](weblistener.md)

---
