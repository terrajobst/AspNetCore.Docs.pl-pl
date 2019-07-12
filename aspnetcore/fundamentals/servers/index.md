---
title: Implementacje serwera sieci Web w programie ASP.NET Core
author: guardrex
description: Odnajdywanie serwerów sieci web w usługach Kestrel i sterownik HTTP.sys dla platformy ASP.NET Core. Dowiedz się, jak wybrać serwer i kiedy należy użyć zwrotnego serwera proxy.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/01/2019
uid: fundamentals/servers/index
ms.openlocfilehash: 10876a61d40679b1a022ce9c58329bf53c36c1bb
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855981"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="b364b-104">Implementacje serwera sieci Web w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b364b-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="b364b-105">Przez [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Halter Autor: Stephen](https://twitter.com/halter73), i [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="b364b-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="b364b-106">Aplikacji ASP.NET Core jest uruchamiany z implementację serwera HTTP w procesie.</span><span class="sxs-lookup"><span data-stu-id="b364b-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="b364b-107">Implementacja serwera nasłuchuje żądań HTTP i wydobywa informacje dotyczące ich do aplikacji jako zbiór [funkcje na żądanie](xref:fundamentals/request-features) składające się na <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="b364b-107">The server implementation listens for HTTP requests and surfaces them to the app as a set of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

## <a name="kestrel"></a><span data-ttu-id="b364b-108">Kestrel</span><span class="sxs-lookup"><span data-stu-id="b364b-108">Kestrel</span></span>

<span data-ttu-id="b364b-109">Kestrel jest domyślny serwer sieci web zawarte w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b364b-109">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

<span data-ttu-id="b364b-110">Użyj Kestrel:</span><span class="sxs-lookup"><span data-stu-id="b364b-110">Use Kestrel:</span></span>

* <span data-ttu-id="b364b-111">Przez siebie jako serwer graniczny przetwarzania żądań bezpośrednio z sieci, w tym Internetu.</span><span class="sxs-lookup"><span data-stu-id="b364b-111">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>

  ![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

* <span data-ttu-id="b364b-113">Za pomocą *zwrotnego serwera proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="b364b-113">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="b364b-114">Serwer proxy odwrotnej odbiera żądania HTTP z Internetu i przekazuje je do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b364b-114">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

  ![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotny serwer proxy, na przykład serwer Nginx, Apache lub IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="b364b-116">Albo konfiguracji hostingu&mdash;z lub bez serwera proxy odwrotnej&mdash;jest obsługiwana w przypadku platformy ASP.NET Core 2.1 lub nowszej aplikacje.</span><span class="sxs-lookup"><span data-stu-id="b364b-116">Either hosting configuration&mdash;with or without a reverse proxy server&mdash;is supported for ASP.NET Core 2.1 or later apps.</span></span>

<span data-ttu-id="b364b-117">Wskazówki dotyczące konfiguracji Kestrel i informacji o tym, kiedy należy używać Kestrel w konfiguracji zwrotny serwer proxy, zobacz <xref:fundamentals/servers/kestrel>.</span><span class="sxs-lookup"><span data-stu-id="b364b-117">For Kestrel configuration guidance and information on when to use Kestrel in a reverse proxy configuration, see <xref:fundamentals/servers/kestrel>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="b364b-118">Windows</span><span class="sxs-lookup"><span data-stu-id="b364b-118">Windows</span></span>](#tab/windows)

<span data-ttu-id="b364b-119">Platforma ASP.NET Core jest dostarczany z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b364b-119">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="b364b-120">[Serwer kestrel](xref:fundamentals/servers/kestrel) jest to domyślne, implementację serwera HTTP dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="b364b-120">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server implementation.</span></span>
* <span data-ttu-id="b364b-121">Serwer HTTP usług IIS jest [wewnątrz procesowego](#hosting-models) dla usług IIS.</span><span class="sxs-lookup"><span data-stu-id="b364b-121">IIS HTTP Server is an [in-process server](#hosting-models) for IIS.</span></span>
* <span data-ttu-id="b364b-122">[Serwer HTTP.sys](xref:fundamentals/servers/httpsys) serwera HTTP tylko do Windows opiera się na [sterownik jądra HTTP.sys i interfejsu API serwera HTTP](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="b364b-122">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="b364b-123">Korzystając z [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), albo uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="b364b-123">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app either runs:</span></span>

* <span data-ttu-id="b364b-124">W tym samym procesie co proces roboczy usług IIS ( [modelu hostingu w trakcie](#hosting-models)) z serwerem HTTP usług IIS.</span><span class="sxs-lookup"><span data-stu-id="b364b-124">In the same process as the IIS worker process (the [in-process hosting model](#hosting-models)) with the IIS HTTP Server.</span></span> <span data-ttu-id="b364b-125">*W trakcie* przedstawiono zalecaną konfigurację.</span><span class="sxs-lookup"><span data-stu-id="b364b-125">*In-process* is the recommended configuration.</span></span>
* <span data-ttu-id="b364b-126">W oddzielnym procesie z proces roboczy usług IIS ( [modelu hostingu poza procesem](#hosting-models)) za pomocą [serwera Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="b364b-126">In a process separate from the IIS worker process (the [out-of-process hosting model](#hosting-models)) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="b364b-127">[Modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) jest macierzysty moduł usług IIS, który obsługuje natywne żądań usług IIS między usług IIS i w procesie serwera HTTP usług IIS lub Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b364b-127">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is a native IIS module that handles native IIS requests between IIS and the in-process IIS HTTP Server or Kestrel.</span></span> <span data-ttu-id="b364b-128">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="b364b-128">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="b364b-129">Modele hostingu</span><span class="sxs-lookup"><span data-stu-id="b364b-129">Hosting models</span></span>

<span data-ttu-id="b364b-130">Za pomocą hostingu platformy ASP.NET Core w trakcie aplikacja jest uruchamiana w tym samym procesie co jej proces roboczy usług IIS.</span><span class="sxs-lookup"><span data-stu-id="b364b-130">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="b364b-131">W trakcie hostingu oferuje ulepszoną wydajność w hostingu poza procesem, ponieważ żądania nie są przekazywane za pośrednictwem karty sprzężenia zwrotnego, interfejsu sieciowego, które zwraca wychodzący ruch sieciowy do tej samej maszynie.</span><span class="sxs-lookup"><span data-stu-id="b364b-131">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="b364b-132">Usługi IIS obsługuje zarządzanie procesami przy użyciu [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="b364b-132">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="b364b-133">Używane w hostingu poza procesem, aplikacje platformy ASP.NET Core, uruchom w procesie oddzielny proces roboczy usług IIS i zarządzanie procesem uchwyty modułu.</span><span class="sxs-lookup"><span data-stu-id="b364b-133">Using out-of-process hosting, ASP.NET Core apps run in a process separate from the IIS worker process, and the module handles process management.</span></span> <span data-ttu-id="b364b-134">Moduł uruchamia proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera spowoduje ponowne uruchomienie aplikacji, jeśli kończy pracę, lub ulega awarii.</span><span class="sxs-lookup"><span data-stu-id="b364b-134">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="b364b-135">Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje, które są uruchamiane w procesie, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="b364b-135">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="b364b-136">Aby uzyskać więcej informacji i konfiguracji wskazówek zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="b364b-136">For more information and configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="b364b-137">macOS</span><span class="sxs-lookup"><span data-stu-id="b364b-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="b364b-138">Platforma ASP.NET Core jest dostarczana z [serwera Kestrel](xref:fundamentals/servers/kestrel), czyli domyślna, serwer HTTP dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="b364b-138">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="b364b-139">Linux</span><span class="sxs-lookup"><span data-stu-id="b364b-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="b364b-140">Platforma ASP.NET Core jest dostarczana z [serwera Kestrel](xref:fundamentals/servers/kestrel), czyli domyślna, serwer HTTP dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="b364b-140">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="b364b-141">Windows</span><span class="sxs-lookup"><span data-stu-id="b364b-141">Windows</span></span>](#tab/windows)

<span data-ttu-id="b364b-142">Platforma ASP.NET Core jest dostarczany z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b364b-142">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="b364b-143">[Serwer kestrel](xref:fundamentals/servers/kestrel) jest to domyślne, serwer HTTP dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="b364b-143">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="b364b-144">[Serwer HTTP.sys](xref:fundamentals/servers/httpsys) serwera HTTP tylko do Windows opiera się na [sterownik jądra HTTP.sys i interfejsu API serwera HTTP](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="b364b-144">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="b364b-145">Korzystając z [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), aplikacja jest uruchamiana w ramach procesu, które są niezależne od proces roboczy usług IIS (*spoza procesu*) przy użyciu [serwera Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="b364b-145">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="b364b-146">Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje zarządzanie procesem.</span><span class="sxs-lookup"><span data-stu-id="b364b-146">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="b364b-147">Moduł uruchamia proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera spowoduje ponowne uruchomienie aplikacji, jeśli kończy pracę, lub ulega awarii.</span><span class="sxs-lookup"><span data-stu-id="b364b-147">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="b364b-148">Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje, które są uruchamiane w procesie, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="b364b-148">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="b364b-149">Na poniższym diagramie przedstawiono relację między usługami IIS, modułu ASP.NET Core, a aplikacja hostowana spoza procesu:</span><span class="sxs-lookup"><span data-stu-id="b364b-149">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Moduł ASP.NET Core](_static/ancm-outofprocess.png)

<span data-ttu-id="b364b-151">Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="b364b-151">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="b364b-152">Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="b364b-152">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="b364b-153">Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80 i 443.</span><span class="sxs-lookup"><span data-stu-id="b364b-153">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="b364b-154">Moduł Określa port, za pośrednictwem zmiennej środowiskowej w momencie uruchamiania i [oprogramowania pośredniczącego integracji usługi IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) konfiguruje serwer do nasłuchiwania `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="b364b-154">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="b364b-155">Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="b364b-155">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="b364b-156">Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b364b-156">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="b364b-157">Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b364b-157">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="b364b-158">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="b364b-158">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="b364b-159">Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b364b-159">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="b364b-160">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="b364b-160">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="b364b-161">Dla usług IIS i modułu ASP.NET Core wskazówki dotyczące konfiguracji zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="b364b-161">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="b364b-162">macOS</span><span class="sxs-lookup"><span data-stu-id="b364b-162">macOS</span></span>](#tab/macos)

<span data-ttu-id="b364b-163">Platforma ASP.NET Core jest dostarczana z [serwera Kestrel](xref:fundamentals/servers/kestrel), czyli domyślna, serwer HTTP dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="b364b-163">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="b364b-164">Linux</span><span class="sxs-lookup"><span data-stu-id="b364b-164">Linux</span></span>](#tab/linux)

<span data-ttu-id="b364b-165">Platforma ASP.NET Core jest dostarczana z [serwera Kestrel](xref:fundamentals/servers/kestrel), czyli domyślna, serwer HTTP dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="b364b-165">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="b364b-166">Serwer Nginx z Kestrel</span><span class="sxs-lookup"><span data-stu-id="b364b-166">Nginx with Kestrel</span></span>

<span data-ttu-id="b364b-167">Aby uzyskać informacje dotyczące sposobu używania jako zwrotny serwer proxy serwera Nginx w systemie Linux dla Kestrel, zobacz <xref:host-and-deploy/linux-nginx>.</span><span class="sxs-lookup"><span data-stu-id="b364b-167">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="b364b-168">Apache z Kestrel</span><span class="sxs-lookup"><span data-stu-id="b364b-168">Apache with Kestrel</span></span>

<span data-ttu-id="b364b-169">Aby uzyskać informacje na temat sposobu na użytek Apache w systemie Linux jako serwer proxy odwrotnej Kestrel, zobacz <xref:host-and-deploy/linux-apache>.</span><span class="sxs-lookup"><span data-stu-id="b364b-169">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

## <a name="httpsys"></a><span data-ttu-id="b364b-170">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="b364b-170">HTTP.sys</span></span>

<span data-ttu-id="b364b-171">Jeśli aplikacje platformy ASP.NET Core są uruchamiane na Windows, sterownik HTTP.sys stanowi alternatywę Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b364b-171">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="b364b-172">Ogólnie zaleca się kestrel w celu uzyskania najlepszej wydajności.</span><span class="sxs-lookup"><span data-stu-id="b364b-172">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="b364b-173">Sterownik HTTP.sys może służyć w scenariuszach, gdzie aplikacja jest połączenie z Internetem i wymagane możliwości są obsługiwane przez rozszerzenie HTTP.sys, ale nie Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b364b-173">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="b364b-174">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="b364b-174">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetu](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="b364b-176">Sterownik HTTP.sys może również dla aplikacji, które są dostępne tylko z siecią wewnętrzną.</span><span class="sxs-lookup"><span data-stu-id="b364b-176">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![Sterownik HTTP.sys komunikuje się bezpośrednio z siecią wewnętrzną](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="b364b-178">Sterownik HTTP.sys konfiguracji wskazówki, zobacz <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="b364b-178">For HTTP.sys configuration guidance, see <xref:fundamentals/servers/httpsys>.</span></span>

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="b364b-179">Infrastruktura serwerowa ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b364b-179">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="b364b-180"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> Dostępne w `Startup.Configure` ujawnia metody <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> właściwości typu <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>.</span><span class="sxs-lookup"><span data-stu-id="b364b-180">The <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> available in the `Startup.Configure` method exposes the <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> property of type <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>.</span></span> <span data-ttu-id="b364b-181">Kestrel i sterownik HTTP.sys udostępniają tylko jednej funkcji, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>, ale implementacji różnych serwera może udostępnić dodatkowe funkcje.</span><span class="sxs-lookup"><span data-stu-id="b364b-181">Kestrel and HTTP.sys only expose a single feature each, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>, but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="b364b-182">`IServerAddressesFeature` można dowiedzieć się, port, który implementacji serwera została powiązana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="b364b-182">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="b364b-183">Niestandardowe serwery</span><span class="sxs-lookup"><span data-stu-id="b364b-183">Custom servers</span></span>

<span data-ttu-id="b364b-184">Jeśli wbudowane serwery nie spełniają wymagań dotyczących aplikacji, można utworzyć wdrożenia niestandardowego serwera.</span><span class="sxs-lookup"><span data-stu-id="b364b-184">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="b364b-185">[Open Web Interface for .NET (OWIN) przewodnik](xref:fundamentals/owin) pokazuje, jak napisać [Nowin](https://github.com/Bobris/Nowin)— na podstawie <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementacji.</span><span class="sxs-lookup"><span data-stu-id="b364b-185">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation.</span></span> <span data-ttu-id="b364b-186">Tylko interfejsy funkcji, których używa aplikacja wymaga wdrożenia, jeśli co najmniej <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> i <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature> muszą być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="b364b-186">Only the feature interfaces that the app uses require implementation, though at a minimum <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> and <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature> must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="b364b-187">Uruchamianie serwera</span><span class="sxs-lookup"><span data-stu-id="b364b-187">Server startup</span></span>

<span data-ttu-id="b364b-188">Serwer jest uruchamiany podczas tworzenia środowiska IDE (Integrated) lub Edytor uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="b364b-188">The server is launched when the Integrated Development Environment (IDE) or editor starts the app:</span></span>

* <span data-ttu-id="b364b-189">[Program Visual Studio](https://visualstudio.microsoft.com) &ndash; profilów uruchamiania może służyć do uruchomienia aplikacji i serwera z oboma [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) lub konsoli.</span><span class="sxs-lookup"><span data-stu-id="b364b-189">[Visual Studio](https://visualstudio.microsoft.com) &ndash; Launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) or the console.</span></span>
* <span data-ttu-id="b364b-190">[Visual Studio Code](https://code.visualstudio.com/) &ndash; aplikacji i serwera są uruchamiane przez [technologię Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), które aktywuje debugera CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="b364b-190">[Visual Studio Code](https://code.visualstudio.com/) &ndash; The app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span>
* <span data-ttu-id="b364b-191">[Program Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) &ndash; aplikacji i serwera są uruchamiane przez [Mono nietrwałego Tryb debugera](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="b364b-191">[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) &ndash; The app and server are started by the [Mono Soft-Mode Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="b364b-192">Podczas uruchamiania aplikacji z poziomu wiersza polecenia w folderze projektu [dotnet, uruchom](/dotnet/core/tools/dotnet-run) spowoduje uruchomienie aplikacji i serwera (Kestrel i tylko w pliku HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="b364b-192">When launching the app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="b364b-193">Konfiguracja jest określona przez `-c|--configuration` opcja, która jest ustawiona jako `Debug` (ustawienie domyślne) lub `Release`.</span><span class="sxs-lookup"><span data-stu-id="b364b-193">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="b364b-194">Jeśli profile uruchamiania są obecne w *launchSettings.json* pliku, użyj `--launch-profile <NAME>` opcję, aby ustawić profil uruchamiania (na przykład `Development` lub `Production`).</span><span class="sxs-lookup"><span data-stu-id="b364b-194">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="b364b-195">Aby uzyskać więcej informacji, zobacz [dotnet, uruchom](/dotnet/core/tools/dotnet-run) i [tworzenie pakietów dystrybucji platformy .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="b364b-195">For more information, see [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

## <a name="http2-support"></a><span data-ttu-id="b364b-196">Obsługa protokołu HTTP/2</span><span class="sxs-lookup"><span data-stu-id="b364b-196">HTTP/2 support</span></span>

<span data-ttu-id="b364b-197">[Protokołu HTTP/2](https://httpwg.org/specs/rfc7540.html) jest obsługiwana za pomocą programu ASP.NET Core w następujących scenariuszach wdrażania:</span><span class="sxs-lookup"><span data-stu-id="b364b-197">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="b364b-198">Kestrel</span><span class="sxs-lookup"><span data-stu-id="b364b-198">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="b364b-199">System operacyjny</span><span class="sxs-lookup"><span data-stu-id="b364b-199">Operating system</span></span>
    * <span data-ttu-id="b364b-200">Windows Server 2016 i Windows 10 lub nowszym&dagger;</span><span class="sxs-lookup"><span data-stu-id="b364b-200">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="b364b-201">Linux z protokołem OpenSSL 1.0.2 lub nowszej (na przykład Ubuntu 16.04 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="b364b-201">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="b364b-202">Protokołu HTTP/2 będą obsługiwane w systemie macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="b364b-202">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="b364b-203">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="b364b-203">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="b364b-204">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="b364b-204">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="b364b-205">Windows Server 2016 i Windows 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="b364b-205">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="b364b-206">Lokalizacja docelowa: Nie dotyczy wdrożeń HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="b364b-206">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="b364b-207">Usługi IIS (w procesie)</span><span class="sxs-lookup"><span data-stu-id="b364b-207">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="b364b-208">Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="b364b-208">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="b364b-209">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="b364b-209">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="b364b-210">Usługi IIS (poza procesem)</span><span class="sxs-lookup"><span data-stu-id="b364b-210">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="b364b-211">Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="b364b-211">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="b364b-212">Połączenia z serwerem usługi edge publicznego używać protokołu HTTP/2, ale połączenie zwrotny serwer proxy Kestrel korzysta z protokołu HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="b364b-212">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="b364b-213">Lokalizacja docelowa: Nie dotyczy wdrożeń spoza procesu usług IIS.</span><span class="sxs-lookup"><span data-stu-id="b364b-213">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="b364b-214">&dagger;Kestrel ma ograniczoną obsługę protokołu HTTP/2, Windows Server 2012 R2 i Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="b364b-214">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="b364b-215">Obsługa jest ograniczona, ponieważ lista obsługiwanych mechanizmów szyfrowania TLS dostępnych w tych systemach operacyjnych jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="b364b-215">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="b364b-216">Certyfikat wygenerowany za pomocą Elliptic krzywej cyfrowego Signature Algorithm (ECDSA) może być konieczne bezpiecznych połączeń TLS.</span><span class="sxs-lookup"><span data-stu-id="b364b-216">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="b364b-217">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="b364b-217">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="b364b-218">Windows Server 2016 i Windows 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="b364b-218">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="b364b-219">Lokalizacja docelowa: Nie dotyczy wdrożeń HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="b364b-219">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="b364b-220">Usługi IIS (poza procesem)</span><span class="sxs-lookup"><span data-stu-id="b364b-220">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="b364b-221">Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="b364b-221">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="b364b-222">Połączenia z serwerem usługi edge publicznego używać protokołu HTTP/2, ale połączenie zwrotny serwer proxy Kestrel korzysta z protokołu HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="b364b-222">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="b364b-223">Lokalizacja docelowa: Nie dotyczy wdrożeń spoza procesu usług IIS.</span><span class="sxs-lookup"><span data-stu-id="b364b-223">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="b364b-224">Należy użyć połączenia protokołu HTTP/2 [negocjowania protokołu warstwy aplikacji (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) i TLS 1.2 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="b364b-224">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="b364b-225">Aby uzyskać więcej informacji zobacz tematy, które odnoszą się do scenariuszy wdrażania serwera.</span><span class="sxs-lookup"><span data-stu-id="b364b-225">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b364b-226">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b364b-226">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys>
