---
title: Implementacje serwera sieci Web w ASP.NET Core
author: tdykstra
description: "Wprowadza serwerów sieci web Kestrel i WebListener dla platformy ASP.NET Core. Zawiera wskazówki dotyczące sposobu wybierz jedną i kiedy należy użyć jednej z zwrotnego serwera proxy."
manager: wpickett
ms.author: tdykstra
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/index
ms.openlocfilehash: 9e2bea396e50615bd02affad93f0ee55255d299f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="419c9-104">Implementacje serwera sieci Web w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="419c9-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="419c9-105">Przez [Dykstra Tomasz](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), i [Roaming Krzysztof](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="419c9-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="419c9-106">Aplikacja platformy ASP.NET Core jest uruchamiana z implementację serwera HTTP w procesie.</span><span class="sxs-lookup"><span data-stu-id="419c9-106">An ASP.NET Core application runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="419c9-107">Nasłuchuje implementację serwera HTTP żądania i udostępnia je do aplikacji jako zestawy [żądania funkcji](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) składające się na `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="419c9-107">The server implementation listens for HTTP requests and surfaces them to the application as sets of [request features](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composed into an `HttpContext`.</span></span>

<span data-ttu-id="419c9-108">Platformy ASP.NET Core dostarczany dwóch implementacji serwera:</span><span class="sxs-lookup"><span data-stu-id="419c9-108">ASP.NET Core ships two server implementations:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="419c9-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="419c9-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="419c9-110">[Kestrel](kestrel.md) serwera HTTP i platform opiera się na [libuv](https://github.com/libuv/libuv), biblioteki i platform asynchroniczne We/Wy.</span><span class="sxs-lookup"><span data-stu-id="419c9-110">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="419c9-111">[Sterownik HTTP.sys](httpsys.md) serwera HTTP systemu Windows opiera się na [sterownik Http.Sys jądra](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="419c9-111">[HTTP.sys](httpsys.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="419c9-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="419c9-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="419c9-113">[Kestrel](kestrel.md) serwera HTTP i platform opiera się na [libuv](https://github.com/libuv/libuv), biblioteki i platform asynchroniczne We/Wy.</span><span class="sxs-lookup"><span data-stu-id="419c9-113">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="419c9-114">[WebListener](weblistener.md) serwera HTTP systemu Windows opiera się na [sterownik Http.Sys jądra](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="419c9-114">[WebListener](weblistener.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

---

## <a name="kestrel"></a><span data-ttu-id="419c9-115">Kestrel</span><span class="sxs-lookup"><span data-stu-id="419c9-115">Kestrel</span></span>

<span data-ttu-id="419c9-116">Kestrel to serwer sieci web, który jest domyślnie włączone w szablonach nowy projekt platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="419c9-116">Kestrel is the web server that's included by default in ASP.NET Core new-project templates.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="419c9-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="419c9-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="419c9-118">Samodzielnie lub z użyciem Kestrel *zwrotnego serwera proxy*, takie jak usługi IIS, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="419c9-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="419c9-119">Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi.</span><span class="sxs-lookup"><span data-stu-id="419c9-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="419c9-122">Albo konfiguracji &mdash; z lub bez zwrotnego serwera proxy &mdash; można również, czy Kestrel jest udostępniany tylko z siecią wewnętrzną.</span><span class="sxs-lookup"><span data-stu-id="419c9-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

<span data-ttu-id="419c9-123">Aby uzyskać informacje o tym, kiedy używać Kestrel z zwrotny serwer proxy, zobacz [wprowadzenie do Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="419c9-123">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="419c9-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="419c9-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="419c9-125">Jeśli aplikacja akceptuje żądania tylko z siecią wewnętrzną, można użyć Kestrel przez samego siebie.</span><span class="sxs-lookup"><span data-stu-id="419c9-125">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel komunikuje się bezpośrednio z sieci wewnętrznej](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="419c9-127">Jeśli udostępnianie aplikacji z Internetem, należy użyć usług IIS, Nginx lub Apache jako *zwrotnego serwera proxy*.</span><span class="sxs-lookup"><span data-stu-id="419c9-127">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="419c9-128">Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi, jak pokazano na poniższym diagramie.</span><span class="sxs-lookup"><span data-stu-id="419c9-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram.</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="419c9-130">Najważniejsze przyczyny wdrożeń krawędzi (ujawniony na ruch z Internetu) przy użyciu zwrotnego serwera proxy jest zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="419c9-130">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="419c9-131">Wersje 1.x Kestrel nie mają pełny zestaw zabezpieczenia przed atakami.</span><span class="sxs-lookup"><span data-stu-id="419c9-131">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="419c9-132">Obejmuje, ale nie jest ograniczony do odpowiednich limitów czasu, limity rozmiaru i limity liczby jednoczesnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="419c9-132">This includes, but isn't limited to, appropriate timeouts, size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="419c9-133">Aby uzyskać informacje o tym, kiedy używać Kestrel z zwrotny serwer proxy, zobacz [wprowadzenie do Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="419c9-133">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

---

<span data-ttu-id="419c9-134">Nie można użyć usług IIS, Nginx lub Apache bez Kestrel lub [implementacji niestandardowego serwera](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="419c9-134">You can't use IIS, Nginx, or Apache without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="419c9-135">Platformy ASP.NET Core był przeznaczony do działania w swoim własnym procesie, tak, aby mogła działać spójnie na platformach.</span><span class="sxs-lookup"><span data-stu-id="419c9-135">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="419c9-136">Usługi IIS, Nginx i Apache dyktowania własne procesu uruchamiania i środowiska; z nich korzystać bezpośrednio, platformy ASP.NET Core musi dostosowania do potrzeb każdej z nich.</span><span class="sxs-lookup"><span data-stu-id="419c9-136">IIS, Nginx, and Apache dictate their own startup process and environment; to use them directly, ASP.NET Core would have to adapt to the needs of each one.</span></span> <span data-ttu-id="419c9-137">Przy użyciu implementacji serwera sieci web, takich jak Kestrel zapewnia platformy ASP.NET Core kontrolę nad procesu uruchamiania i środowiska.</span><span class="sxs-lookup"><span data-stu-id="419c9-137">Using a web server implementation such as Kestrel gives ASP.NET Core control over the startup process and environment.</span></span> <span data-ttu-id="419c9-138">Dlatego zamiast w trakcie dostosowania platformy ASP.NET Core internetowych usług informacyjnych, Nginx lub Apache, tylko konfigurowania tych serwerów sieci web do serwera proxy żądań Kestrel.</span><span class="sxs-lookup"><span data-stu-id="419c9-138">So rather than trying to adapt ASP.NET Core to IIS, Nginx, or Apache, you just set up those web servers to proxy requests to Kestrel.</span></span> <span data-ttu-id="419c9-139">Umożliwia to rozmieszczenie Twojej `Program.Main` i `Startup` klasy były zasadniczo taki sam, niezależnie od tego, w którym jest wdrażany.</span><span class="sxs-lookup"><span data-stu-id="419c9-139">This arrangement allows your `Program.Main` and `Startup` classes to be essentially the same no matter where you deploy.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="419c9-140">Usługi IIS z Kestrel</span><span class="sxs-lookup"><span data-stu-id="419c9-140">IIS with Kestrel</span></span>

<span data-ttu-id="419c9-141">Korzystając z usług IIS lub usług IIS Express jako zwrotny serwer proxy dla platformy ASP.NET Core, aplikacji platformy ASP.NET Core uruchamia się w oddzielnej procesu z proces roboczy usług IIS.</span><span class="sxs-lookup"><span data-stu-id="419c9-141">When you use IIS or IIS Express as a reverse proxy for ASP.NET Core, the ASP.NET Core application runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="419c9-142">W procesie IIS specjalne Moduł IIS uruchamia do koordynowania relacji zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="419c9-142">In the IIS process, a special IIS module runs to coordinate the reverse proxy relationship.</span></span>  <span data-ttu-id="419c9-143">Jest to *platformy ASP.NET Core modułu*.</span><span class="sxs-lookup"><span data-stu-id="419c9-143">This is the *ASP.NET Core Module*.</span></span> <span data-ttu-id="419c9-144">Podstawowe funkcje platformy ASP.NET Core modułu są do uruchamiania aplikacji platformy ASP.NET Core, uruchom go ponownie uległa awarii i przesyłania dalej ruchu HTTP.</span><span class="sxs-lookup"><span data-stu-id="419c9-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core application, restart it when it crashes, and forward HTTP traffic to it.</span></span> <span data-ttu-id="419c9-145">Aby uzyskać więcej informacji, zobacz [platformy ASP.NET Core modułu](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="419c9-145">For more information, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="419c9-146">Nginx z Kestrel</span><span class="sxs-lookup"><span data-stu-id="419c9-146">Nginx with Kestrel</span></span>

<span data-ttu-id="419c9-147">Aby uzyskać informacje o sposobie używania Nginx w systemie Linux jako zwrotnego serwera proxy dla Kestrel, zobacz [hosta w systemie Linux z Nginx](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="419c9-147">For information about how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="419c9-148">Apache z Kestrel</span><span class="sxs-lookup"><span data-stu-id="419c9-148">Apache with Kestrel</span></span>

<span data-ttu-id="419c9-149">Aby uzyskać informacje o sposobie używania Apache w systemie Linux jako zwrotnego serwera proxy dla Kestrel, zobacz [hosta w systemie Linux z Apache](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="419c9-149">For information about how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="419c9-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="419c9-150">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="419c9-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="419c9-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="419c9-152">Jeśli uruchamianie aplikacji platformy ASP.NET Core w systemie Windows, sterownik HTTP.sys stanowi alternatywę Kestrel.</span><span class="sxs-lookup"><span data-stu-id="419c9-152">If you run your ASP.NET Core app on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="419c9-153">Sterownik HTTP.sys służy do scenariuszy, w którym udostępnianie aplikacji z Internetem i potrzebne funkcje HTTP.sys, które Kestrel nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="419c9-153">You can use HTTP.sys for scenarios where you expose your app to the Internet and you need HTTP.sys features that Kestrel doesn't support.</span></span> 

![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetem](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="419c9-155">Można także HTTP.sys dla aplikacji, które są dostępne tylko z siecią wewnętrzną.</span><span class="sxs-lookup"><span data-stu-id="419c9-155">HTTP.sys can also be used for applications that are exposed only to an internal network.</span></span> 

![Sterownik HTTP.sys komunikuje się bezpośrednio z sieci wewnętrznej](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="419c9-157">W przypadku scenariuszy sieci wewnętrznej Kestrel zazwyczaj jest zalecane w przypadku najlepszą wydajność; Jednak w niektórych scenariuszach, można użyć funkcji, która oferuje tylko w pliku HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="419c9-157">For internal network scenarios, Kestrel is generally recommended for best performance; but in some scenarios, you might want to use a feature that only HTTP.sys offers.</span></span> <span data-ttu-id="419c9-158">Aby uzyskać informacje o funkcjach HTTP.sys, zobacz [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="419c9-158">For information about HTTP.sys features, see [HTTP.sys](httpsys.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="419c9-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="419c9-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="419c9-160">Sterownik HTTP.sys nosi nazwę WebListener w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="419c9-160">HTTP.sys is named WebListener in ASP.NET Core 1.x.</span></span> <span data-ttu-id="419c9-161">Po uruchomieniu programu ASP.NET Core aplikacji w systemie Windows, WebListener stanowi alternatywę, który służy do scenariuszy, w którym chcesz udostępnić aplikację do Internetu, ale nie można używać usług IIS.</span><span class="sxs-lookup"><span data-stu-id="419c9-161">If you run your ASP.NET Core app on Windows, WebListener is an alternative that you can use for scenarios where you want to expose your app to the Internet but you can't use IIS.</span></span>

![Weblistener komunikuje się bezpośrednio z Internetem](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="419c9-163">WebListener można również zamiast Kestrel dla aplikacji, które są dostępne tylko z siecią wewnętrzną, jeśli potrzebujesz funkcji WebListener, które Kestrel nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="419c9-163">WebListener can also be used in place of Kestrel for applications that are exposed only to an internal network, if you need WebListener features that Kestrel doesn't support.</span></span> 

![Weblistener komunikuje się bezpośrednio z sieci wewnętrznej](weblistener/_static/weblistener-to-internal.png)

<span data-ttu-id="419c9-165">W przypadku scenariuszy sieci wewnętrznej ogólnie zaleca się Kestrel, aby uzyskać najlepszą wydajność, ale w niektórych sytuacjach możesz chcieć użyć funkcji, która oferuje tylko WebListener.</span><span class="sxs-lookup"><span data-stu-id="419c9-165">For internal network scenarios, Kestrel is generally recommended for best performance, but in some scenarios you might want to use a feature that only WebListener offers.</span></span> <span data-ttu-id="419c9-166">Aby uzyskać informacje o funkcjach WebListener, zobacz [WebListener](weblistener.md).</span><span class="sxs-lookup"><span data-stu-id="419c9-166">For information about WebListener features, see [WebListener](weblistener.md).</span></span>

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a><span data-ttu-id="419c9-167">Uwagi dotyczące infrastruktury serwera ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="419c9-167">Notes about ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="419c9-168">[ `IApplicationBuilder` ](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) Dostępne w `Startup` klasy `Configure` ujawnia metody `ServerFeatures` właściwości typu [ `IFeatureCollection` ](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="419c9-168">The [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup` class `Configure` method exposes the `ServerFeatures` property of type [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="419c9-169">Kestrel i WebListener uwidacznia tylko jednej funkcji [ `IServerAddressesFeature` ](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ale implementacje inny serwer może udostępniać dodatkowe funkcje.</span><span class="sxs-lookup"><span data-stu-id="419c9-169">Kestrel and WebListener both expose only a single feature, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="419c9-170">`IServerAddressesFeature`można sprawdzić, port, który ma powiązany implementacją serwera w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="419c9-170">`IServerAddressesFeature` can be used to find out which port the server implementation has bound to at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="419c9-171">Niestandardowe serwerów</span><span class="sxs-lookup"><span data-stu-id="419c9-171">Custom servers</span></span>

<span data-ttu-id="419c9-172">Jeśli wbudowane serwery nie spełniają potrzeb użytkownika, można utworzyć implementacji niestandardowego serwera.</span><span class="sxs-lookup"><span data-stu-id="419c9-172">If the built-in servers don't meet your needs, you can create a custom server implementation.</span></span> <span data-ttu-id="419c9-173">[Interfejsu Open Web dla platformy .NET (OWIN) przewodnik](../owin.md) pokazano, jak napisać [Nowin](https://github.com/Bobris/Nowin)— na podstawie [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementacji.</span><span class="sxs-lookup"><span data-stu-id="419c9-173">The [Open Web Interface for .NET (OWIN) guide](../owin.md) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="419c9-174">Można go dowolnie implementuje tylko interfejsy funkcji aplikacja wymaga, że co najmniej musi obsługiwać [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) i [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="419c9-174">You're free to implement just the feature interfaces your application needs, though at a minimum you must support [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span></span>

## <a name="next-steps"></a><span data-ttu-id="419c9-175">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="419c9-175">Next steps</span></span>

<span data-ttu-id="419c9-176">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="419c9-176">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="419c9-177">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="419c9-177">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

- [<span data-ttu-id="419c9-178">Kestrel</span><span class="sxs-lookup"><span data-stu-id="419c9-178">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="419c9-179">Kestrel z usługami IIS</span><span class="sxs-lookup"><span data-stu-id="419c9-179">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="419c9-180">Hosting w systemie Linux z Nginx</span><span class="sxs-lookup"><span data-stu-id="419c9-180">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="419c9-181">Hosting w systemie Linux z Apache</span><span class="sxs-lookup"><span data-stu-id="419c9-181">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="419c9-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="419c9-182">HTTP.sys</span></span>](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="419c9-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="419c9-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

- [<span data-ttu-id="419c9-184">Kestrel</span><span class="sxs-lookup"><span data-stu-id="419c9-184">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="419c9-185">Kestrel z usługami IIS</span><span class="sxs-lookup"><span data-stu-id="419c9-185">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="419c9-186">Hosting w systemie Linux z Nginx</span><span class="sxs-lookup"><span data-stu-id="419c9-186">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="419c9-187">Hosting w systemie Linux z Apache</span><span class="sxs-lookup"><span data-stu-id="419c9-187">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="419c9-188">WebListener</span><span class="sxs-lookup"><span data-stu-id="419c9-188">WebListener</span></span>](weblistener.md)

---
