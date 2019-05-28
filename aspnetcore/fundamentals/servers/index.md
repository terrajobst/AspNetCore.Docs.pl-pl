---
title: Implementacje serwera sieci Web w programie ASP.NET Core
author: guardrex
description: Odnajdywanie serwerów sieci web w usługach Kestrel i sterownik HTTP.sys dla platformy ASP.NET Core. Dowiedz się, jak wybrać serwer i kiedy należy użyć zwrotnego serwera proxy.
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/24/2019
uid: fundamentals/servers/index
ms.openlocfilehash: 82a4bd0173b0aab094ac5ac9f89d5358ba585d3d
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251350"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="f6246-104">Implementacje serwera sieci Web w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6246-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="f6246-105">Przez [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Halter Autor: Stephen](https://twitter.com/halter73), i [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="f6246-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="f6246-106">Aplikacji ASP.NET Core jest uruchamiany z implementację serwera HTTP w procesie.</span><span class="sxs-lookup"><span data-stu-id="f6246-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="f6246-107">Implementacja serwera nasłuchuje żądań HTTP i wydobywa informacje dotyczące ich do aplikacji jako zbiór [funkcje na żądanie](xref:fundamentals/request-features) składające się na <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="f6246-107">The server implementation listens for HTTP requests and surfaces them to the app as a set of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="f6246-108">Windows</span><span class="sxs-lookup"><span data-stu-id="f6246-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="f6246-109">Platforma ASP.NET Core jest dostarczany z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="f6246-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="f6246-110">[Serwer kestrel](xref:fundamentals/servers/kestrel) jest to domyślne, implementację serwera HTTP dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="f6246-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server implementation.</span></span>
* <span data-ttu-id="f6246-111">Serwer HTTP usług IIS jest [wewnątrz procesowego](#in-process-hosting-model) dla usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f6246-111">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="f6246-112">[Serwer HTTP.sys](xref:fundamentals/servers/httpsys) serwera HTTP tylko do Windows opiera się na [sterownik jądra HTTP.sys i interfejsu API serwera HTTP](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="f6246-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="f6246-113">Korzystając z [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), albo uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f6246-113">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app either runs:</span></span>

* <span data-ttu-id="f6246-114">W tym samym procesie co proces roboczy usług IIS ( [modelu hostingu w trakcie](#in-process-hosting-model)) za pomocą [serwer HTTP IIS](#iis-http-server).</span><span class="sxs-lookup"><span data-stu-id="f6246-114">In the same process as the IIS worker process (the [in-process hosting model](#in-process-hosting-model)) with the [IIS HTTP Server](#iis-http-server).</span></span> <span data-ttu-id="f6246-115">*W trakcie* przedstawiono zalecaną konfigurację.</span><span class="sxs-lookup"><span data-stu-id="f6246-115">*In-process* is the recommended configuration.</span></span>
* <span data-ttu-id="f6246-116">W oddzielnym procesie z proces roboczy usług IIS ( [modelu hostingu poza procesem](#out-of-process-hosting-model)) za pomocą [serwera Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="f6246-116">In a process separate from the IIS worker process (the [out-of-process hosting model](#out-of-process-hosting-model)) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="f6246-117">[Modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) jest macierzysty moduł usług IIS, który obsługuje natywne żądań usług IIS między usług IIS i w procesie serwera HTTP usług IIS lub Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6246-117">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is a native IIS module that handles native IIS requests between IIS and the in-process IIS HTTP Server or Kestrel.</span></span> <span data-ttu-id="f6246-118">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="f6246-118">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="f6246-119">Modele hostingu</span><span class="sxs-lookup"><span data-stu-id="f6246-119">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="f6246-120">W trakcie modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="f6246-120">In-process hosting model</span></span>

<span data-ttu-id="f6246-121">Za pomocą hostingu platformy ASP.NET Core w trakcie aplikacja jest uruchamiana w tym samym procesie co jej proces roboczy usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f6246-121">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="f6246-122">W trakcie hostingu oferuje ulepszoną wydajność w hostingu poza procesem, ponieważ żądania nie są przekazywane za pośrednictwem karty sprzężenia zwrotnego, interfejsu sieciowego, które zwraca wychodzący ruch sieciowy do tej samej maszynie.</span><span class="sxs-lookup"><span data-stu-id="f6246-122">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="f6246-123">Usługi IIS obsługuje zarządzanie procesami przy użyciu [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="f6246-123">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="f6246-124">Moduł ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f6246-124">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="f6246-125">Wykonuje inicjowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6246-125">Performs app initialization.</span></span>
  * <span data-ttu-id="f6246-126">Ładunki [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="f6246-126">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="f6246-127">Wywołania `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="f6246-127">Calls `Program.Main`.</span></span>
* <span data-ttu-id="f6246-128">Obsługuje okres istnienia żądanie natywnych usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f6246-128">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="f6246-129">Model hostingu w trakcie nie jest obsługiwana dla aplikacji platformy ASP.NET Core, które obsługują program .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f6246-129">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="f6246-130">Na poniższym diagramie przedstawiono relację między usługami IIS, modułu ASP.NET Core i aplikacji obsługiwanych w procesie:</span><span class="sxs-lookup"><span data-stu-id="f6246-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Moduł ASP.NET Core](_static/ancm-inprocess.png)

<span data-ttu-id="f6246-132">Żądanie dociera z sieci web do sterownik HTTP.sys trybu jądra.</span><span class="sxs-lookup"><span data-stu-id="f6246-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="f6246-133">Sterownik kieruje żądanie macierzystego w usługach IIS na porcie skonfigurowanym witryny sieci Web, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="f6246-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="f6246-134">Moduł odbiera żądanie natywnych i przekazuje go do serwera HTTP usług IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="f6246-134">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="f6246-135">Serwer HTTP usług IIS jest implementacją w procesie serwera dla usług IIS, który konwertuje żądania z natywnego na zarządzane.</span><span class="sxs-lookup"><span data-stu-id="f6246-135">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="f6246-136">Po przetworzeniu żądania przez serwer HTTP usług IIS, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6246-136">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="f6246-137">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="f6246-137">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="f6246-138">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS za pośrednictwem serwera HTTP usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f6246-138">The app's response is passed back to IIS through IIS HTTP Server.</span></span> <span data-ttu-id="f6246-139">Usługi IIS wysyła odpowiedź do klienta, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="f6246-139">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="f6246-140">W trakcie obsługi jest zoptymalizowany pod kątem w przypadku istniejących aplikacji, ale [dotnet nowe](/dotnet/core/tools/dotnet-new) szablony domyślne do modelu hostowania w trakcie we wszystkich scenariuszach usługi IIS i usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="f6246-140">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="f6246-141">Model hostingu poza procesem</span><span class="sxs-lookup"><span data-stu-id="f6246-141">Out-of-process hosting model</span></span>

<span data-ttu-id="f6246-142">Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje zarządzanie procesem.</span><span class="sxs-lookup"><span data-stu-id="f6246-142">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="f6246-143">Moduł uruchamia proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera spowoduje ponowne uruchomienie aplikacji, jeśli kończy pracę, lub ulega awarii.</span><span class="sxs-lookup"><span data-stu-id="f6246-143">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="f6246-144">Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje, które są uruchamiane w procesie, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="f6246-144">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="f6246-145">Na poniższym diagramie przedstawiono relację między usługami IIS, modułu ASP.NET Core, a aplikacja hostowana spoza procesu:</span><span class="sxs-lookup"><span data-stu-id="f6246-145">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Moduł ASP.NET Core](_static/ancm-outofprocess.png)

<span data-ttu-id="f6246-147">Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="f6246-147">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="f6246-148">Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="f6246-148">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="f6246-149">Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80 i 443.</span><span class="sxs-lookup"><span data-stu-id="f6246-149">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="f6246-150">Moduł Określa port, za pośrednictwem zmiennej środowiskowej w momencie uruchamiania i <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> rozszerzenia konfiguruje serwer do nasłuchiwania na `http://localhost:{PORT}`.</span><span class="sxs-lookup"><span data-stu-id="f6246-150">The module specifies the port via an environment variable at startup, and the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="f6246-151">Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="f6246-151">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="f6246-152">Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f6246-152">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="f6246-153">Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6246-153">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="f6246-154">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="f6246-154">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="f6246-155">Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6246-155">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="f6246-156">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="f6246-156">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="f6246-157">Dla usług IIS i modułu ASP.NET Core wskazówki dotyczące konfiguracji zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="f6246-157">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="f6246-158">macOS</span><span class="sxs-lookup"><span data-stu-id="f6246-158">macOS</span></span>](#tab/macos)

<span data-ttu-id="f6246-159">Platforma ASP.NET Core jest dostarczana z [serwera Kestrel](xref:fundamentals/servers/kestrel), czyli domyślna, serwer HTTP dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="f6246-159">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="f6246-160">Linux</span><span class="sxs-lookup"><span data-stu-id="f6246-160">Linux</span></span>](#tab/linux)

<span data-ttu-id="f6246-161">Platforma ASP.NET Core jest dostarczana z [serwera Kestrel](xref:fundamentals/servers/kestrel), czyli domyślna, serwer HTTP dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="f6246-161">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="f6246-162">Windows</span><span class="sxs-lookup"><span data-stu-id="f6246-162">Windows</span></span>](#tab/windows)

<span data-ttu-id="f6246-163">Platforma ASP.NET Core jest dostarczany z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="f6246-163">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="f6246-164">[Serwer kestrel](xref:fundamentals/servers/kestrel) jest to domyślne, serwer HTTP dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="f6246-164">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="f6246-165">[Serwer HTTP.sys](xref:fundamentals/servers/httpsys) serwera HTTP tylko do Windows opiera się na [sterownik jądra HTTP.sys i interfejsu API serwera HTTP](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="f6246-165">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="f6246-166">Korzystając z [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), aplikacja jest uruchamiana w ramach procesu, które są niezależne od proces roboczy usług IIS (*spoza procesu*) przy użyciu [serwera Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="f6246-166">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="f6246-167">Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje zarządzanie procesem.</span><span class="sxs-lookup"><span data-stu-id="f6246-167">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="f6246-168">Moduł uruchamia proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera spowoduje ponowne uruchomienie aplikacji, jeśli kończy pracę, lub ulega awarii.</span><span class="sxs-lookup"><span data-stu-id="f6246-168">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="f6246-169">Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje, które są uruchamiane w procesie, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="f6246-169">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="f6246-170">Na poniższym diagramie przedstawiono relację między usługami IIS, modułu ASP.NET Core, a aplikacja hostowana spoza procesu:</span><span class="sxs-lookup"><span data-stu-id="f6246-170">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Moduł ASP.NET Core](_static/ancm-outofprocess.png)

<span data-ttu-id="f6246-172">Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="f6246-172">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="f6246-173">Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="f6246-173">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="f6246-174">Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80 i 443.</span><span class="sxs-lookup"><span data-stu-id="f6246-174">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="f6246-175">Moduł Określa port, za pośrednictwem zmiennej środowiskowej w momencie uruchamiania i [oprogramowania pośredniczącego integracji usługi IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) konfiguruje serwer do nasłuchiwania `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="f6246-175">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="f6246-176">Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="f6246-176">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="f6246-177">Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f6246-177">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="f6246-178">Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6246-178">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="f6246-179">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="f6246-179">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="f6246-180">Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6246-180">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="f6246-181">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="f6246-181">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="f6246-182">Dla usług IIS i modułu ASP.NET Core wskazówki dotyczące konfiguracji zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="f6246-182">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="f6246-183">macOS</span><span class="sxs-lookup"><span data-stu-id="f6246-183">macOS</span></span>](#tab/macos)

<span data-ttu-id="f6246-184">Platforma ASP.NET Core jest dostarczana z [serwera Kestrel](xref:fundamentals/servers/kestrel), czyli domyślna, serwer HTTP dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="f6246-184">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="f6246-185">Linux</span><span class="sxs-lookup"><span data-stu-id="f6246-185">Linux</span></span>](#tab/linux)

<span data-ttu-id="f6246-186">Platforma ASP.NET Core jest dostarczana z [serwera Kestrel](xref:fundamentals/servers/kestrel), czyli domyślna, serwer HTTP dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="f6246-186">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="f6246-187">Kestrel</span><span class="sxs-lookup"><span data-stu-id="f6246-187">Kestrel</span></span>

<span data-ttu-id="f6246-188">Kestrel jest domyślny serwer sieci web zawarte w szablonach projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6246-188">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f6246-189">Kestrel może być używany:</span><span class="sxs-lookup"><span data-stu-id="f6246-189">Kestrel can be used:</span></span>

* <span data-ttu-id="f6246-190">Przez siebie jako serwer graniczny przetwarzania żądań bezpośrednio z sieci, w tym Internetu.</span><span class="sxs-lookup"><span data-stu-id="f6246-190">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>

  ![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

* <span data-ttu-id="f6246-192">Za pomocą *zwrotnego serwera proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="f6246-192">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="f6246-193">Serwer proxy odwrotnej odbiera żądania HTTP z Internetu i przekazuje je do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6246-193">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

  ![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotny serwer proxy, na przykład serwer Nginx, Apache lub IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="f6246-195">Albo konfiguracji hostingu&mdash;z lub bez serwera proxy odwrotnej&mdash;jest obsługiwana w przypadku platformy ASP.NET Core 2.1 lub nowszej aplikacje.</span><span class="sxs-lookup"><span data-stu-id="f6246-195">Either hosting configuration&mdash;with or without a reverse proxy server&mdash;is supported for ASP.NET Core 2.1 or later apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f6246-196">Jeśli aplikacja będzie akceptować tylko żądania z siecią wewnętrzną, Kestrel może służyć przez siebie.</span><span class="sxs-lookup"><span data-stu-id="f6246-196">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel komunikuje się bezpośrednio z siecią wewnętrzną](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="f6246-198">Jeśli aplikacja jest uwidaczniany w Internecie, należy użyć Kestrel *zwrotnego serwera proxy*, takich jak [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), lub [Apache ](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="f6246-198">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="f6246-199">Serwer proxy odwrotnej odbiera żądania HTTP z Internetu i przekazuje je do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6246-199">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotny serwer proxy, na przykład serwer Nginx, Apache lub IIS](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="f6246-201">Najważniejszą przyczyną za używanie zwrotnego serwera proxy na potrzeby publicznego krawędzi serwera wdrożenia, które są udostępniane bezpośrednio przez Internet jest zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="f6246-201">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="f6246-202">Wersji 1.x Kestrel nie zawierają ważne funkcje zabezpieczeń do ochrony przed atakami z sieci Internet.</span><span class="sxs-lookup"><span data-stu-id="f6246-202">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="f6246-203">Obejmuje, ale nie jest ograniczona do odpowiednie limity czasu, limity rozmiaru żądania i limity liczby jednoczesnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="f6246-203">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

::: moniker-end

<span data-ttu-id="f6246-204">Wskazówki dotyczące konfiguracji Kestrel i informacji o tym, kiedy należy używać Kestrel w konfiguracji zwrotny serwer proxy, zobacz <xref:fundamentals/servers/kestrel>.</span><span class="sxs-lookup"><span data-stu-id="f6246-204">For Kestrel configuration guidance and information on when to use Kestrel in a reverse proxy configuration, see <xref:fundamentals/servers/kestrel>.</span></span>

### <a name="nginx-with-kestrel"></a><span data-ttu-id="f6246-205">Serwer Nginx z Kestrel</span><span class="sxs-lookup"><span data-stu-id="f6246-205">Nginx with Kestrel</span></span>

<span data-ttu-id="f6246-206">Aby uzyskać informacje dotyczące sposobu używania jako zwrotny serwer proxy serwera Nginx w systemie Linux dla Kestrel, zobacz <xref:host-and-deploy/linux-nginx>.</span><span class="sxs-lookup"><span data-stu-id="f6246-206">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="f6246-207">Apache z Kestrel</span><span class="sxs-lookup"><span data-stu-id="f6246-207">Apache with Kestrel</span></span>

<span data-ttu-id="f6246-208">Aby uzyskać informacje na temat sposobu na użytek Apache w systemie Linux jako serwer proxy odwrotnej Kestrel, zobacz <xref:host-and-deploy/linux-apache>.</span><span class="sxs-lookup"><span data-stu-id="f6246-208">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="iis-http-server"></a><span data-ttu-id="f6246-209">Serwer HTTP usług IIS</span><span class="sxs-lookup"><span data-stu-id="f6246-209">IIS HTTP Server</span></span>

<span data-ttu-id="f6246-210">Serwer HTTP usług IIS jest [wewnątrz procesowego](#in-process-hosting-model) dla usług IIS i jest to wymagane w przypadku wdrożeń w procesie.</span><span class="sxs-lookup"><span data-stu-id="f6246-210">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS and required for in-process deployments.</span></span> <span data-ttu-id="f6246-211">[Modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) obsługi natywnych żądań usług IIS między usługami IIS a serwer HTTP usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f6246-211">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) handles native IIS requests between IIS and IIS HTTP Server.</span></span> <span data-ttu-id="f6246-212">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="f6246-212">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="f6246-213">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="f6246-213">HTTP.sys</span></span>

<span data-ttu-id="f6246-214">Jeśli aplikacje platformy ASP.NET Core są uruchamiane na Windows, sterownik HTTP.sys stanowi alternatywę Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6246-214">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="f6246-215">Ogólnie zaleca się kestrel w celu uzyskania najlepszej wydajności.</span><span class="sxs-lookup"><span data-stu-id="f6246-215">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="f6246-216">Sterownik HTTP.sys może służyć w scenariuszach, gdzie aplikacja jest połączenie z Internetem i wymagane możliwości są obsługiwane przez rozszerzenie HTTP.sys, ale nie Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6246-216">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="f6246-217">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="f6246-217">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetu](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="f6246-219">Sterownik HTTP.sys może również dla aplikacji, które są dostępne tylko z siecią wewnętrzną.</span><span class="sxs-lookup"><span data-stu-id="f6246-219">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![Sterownik HTTP.sys komunikuje się bezpośrednio z siecią wewnętrzną](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="f6246-221">Sterownik HTTP.sys konfiguracji wskazówki, zobacz <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="f6246-221">For HTTP.sys configuration guidance, see <xref:fundamentals/servers/httpsys>.</span></span>

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="f6246-222">Infrastruktura serwerowa ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6246-222">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="f6246-223"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> Dostępne w `Startup.Configure` ujawnia metody <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> właściwości typu <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>.</span><span class="sxs-lookup"><span data-stu-id="f6246-223">The <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> available in the `Startup.Configure` method exposes the <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> property of type <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>.</span></span> <span data-ttu-id="f6246-224">Kestrel i sterownik HTTP.sys udostępniają tylko jednej funkcji, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>, ale implementacji różnych serwera może udostępnić dodatkowe funkcje.</span><span class="sxs-lookup"><span data-stu-id="f6246-224">Kestrel and HTTP.sys only expose a single feature each, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>, but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="f6246-225">`IServerAddressesFeature` można dowiedzieć się, port, który implementacji serwera została powiązana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="f6246-225">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="f6246-226">Niestandardowe serwery</span><span class="sxs-lookup"><span data-stu-id="f6246-226">Custom servers</span></span>

<span data-ttu-id="f6246-227">Jeśli wbudowane serwery nie spełniają wymagań dotyczących aplikacji, można utworzyć wdrożenia niestandardowego serwera.</span><span class="sxs-lookup"><span data-stu-id="f6246-227">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="f6246-228">[Open Web Interface for .NET (OWIN) przewodnik](xref:fundamentals/owin) pokazuje, jak napisać [Nowin](https://github.com/Bobris/Nowin)— na podstawie <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementacji.</span><span class="sxs-lookup"><span data-stu-id="f6246-228">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation.</span></span> <span data-ttu-id="f6246-229">Tylko interfejsy funkcji, których używa aplikacja wymaga wdrożenia, jeśli co najmniej <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> i <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature> muszą być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="f6246-229">Only the feature interfaces that the app uses require implementation, though at a minimum <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> and <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature> must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="f6246-230">Uruchamianie serwera</span><span class="sxs-lookup"><span data-stu-id="f6246-230">Server startup</span></span>

<span data-ttu-id="f6246-231">Serwer jest uruchamiany podczas tworzenia środowiska IDE (Integrated) lub Edytor uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f6246-231">The server is launched when the Integrated Development Environment (IDE) or editor starts the app:</span></span>

* <span data-ttu-id="f6246-232">[Program Visual Studio](https://visualstudio.microsoft.com) &ndash; profilów uruchamiania może służyć do uruchomienia aplikacji i serwera z oboma [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) lub konsoli.</span><span class="sxs-lookup"><span data-stu-id="f6246-232">[Visual Studio](https://visualstudio.microsoft.com) &ndash; Launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) or the console.</span></span>
* <span data-ttu-id="f6246-233">[Visual Studio Code](https://code.visualstudio.com/) &ndash; aplikacji i serwera są uruchamiane przez [technologię Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), które aktywuje debugera CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="f6246-233">[Visual Studio Code](https://code.visualstudio.com/) &ndash; The app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span>
* <span data-ttu-id="f6246-234">[Program Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) &ndash; aplikacji i serwera są uruchamiane przez [Mono nietrwałego Tryb debugera](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="f6246-234">[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) &ndash; The app and server are started by the [Mono Soft-Mode Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="f6246-235">Podczas uruchamiania aplikacji z poziomu wiersza polecenia w folderze projektu [dotnet, uruchom](/dotnet/core/tools/dotnet-run) spowoduje uruchomienie aplikacji i serwera (Kestrel i tylko w pliku HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="f6246-235">When launching the app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="f6246-236">Konfiguracja jest określona przez `-c|--configuration` opcja, która jest ustawiona jako `Debug` (ustawienie domyślne) lub `Release`.</span><span class="sxs-lookup"><span data-stu-id="f6246-236">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="f6246-237">Jeśli profile uruchamiania są obecne w *launchSettings.json* pliku, użyj `--launch-profile <NAME>` opcję, aby ustawić profil uruchamiania (na przykład `Development` lub `Production`).</span><span class="sxs-lookup"><span data-stu-id="f6246-237">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="f6246-238">Aby uzyskać więcej informacji, zobacz [dotnet, uruchom](/dotnet/core/tools/dotnet-run) i [tworzenie pakietów dystrybucji platformy .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="f6246-238">For more information, see [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

## <a name="http2-support"></a><span data-ttu-id="f6246-239">Obsługa protokołu HTTP/2</span><span class="sxs-lookup"><span data-stu-id="f6246-239">HTTP/2 support</span></span>

<span data-ttu-id="f6246-240">[Protokołu HTTP/2](https://httpwg.org/specs/rfc7540.html) jest obsługiwana za pomocą programu ASP.NET Core w następujących scenariuszach wdrażania:</span><span class="sxs-lookup"><span data-stu-id="f6246-240">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="f6246-241">Kestrel</span><span class="sxs-lookup"><span data-stu-id="f6246-241">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="f6246-242">System operacyjny</span><span class="sxs-lookup"><span data-stu-id="f6246-242">Operating system</span></span>
    * <span data-ttu-id="f6246-243">Windows Server 2016 i Windows 10 lub nowszym&dagger;</span><span class="sxs-lookup"><span data-stu-id="f6246-243">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="f6246-244">Linux z protokołem OpenSSL 1.0.2 lub nowszej (na przykład Ubuntu 16.04 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="f6246-244">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="f6246-245">Protokołu HTTP/2 będą obsługiwane w systemie macOS w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="f6246-245">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="f6246-246">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="f6246-246">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="f6246-247">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="f6246-247">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="f6246-248">Windows Server 2016 i Windows 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="f6246-248">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="f6246-249">Lokalizacja docelowa: Nie dotyczy wdrożeń HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="f6246-249">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="f6246-250">Usługi IIS (w procesie)</span><span class="sxs-lookup"><span data-stu-id="f6246-250">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="f6246-251">Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="f6246-251">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="f6246-252">Platforma docelowa: .NET Core 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="f6246-252">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="f6246-253">Usługi IIS (poza procesem)</span><span class="sxs-lookup"><span data-stu-id="f6246-253">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="f6246-254">Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="f6246-254">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="f6246-255">Połączenia z serwerem usługi edge publicznego używać protokołu HTTP/2, ale połączenie zwrotny serwer proxy Kestrel korzysta z protokołu HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="f6246-255">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="f6246-256">Lokalizacja docelowa: Nie dotyczy wdrożeń spoza procesu usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f6246-256">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="f6246-257">&dagger;Kestrel ma ograniczoną obsługę protokołu HTTP/2, Windows Server 2012 R2 i Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="f6246-257">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="f6246-258">Obsługa jest ograniczona, ponieważ lista obsługiwanych mechanizmów szyfrowania TLS dostępnych w tych systemach operacyjnych jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="f6246-258">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="f6246-259">Certyfikat wygenerowany za pomocą Elliptic krzywej cyfrowego Signature Algorithm (ECDSA) może być konieczne bezpiecznych połączeń TLS.</span><span class="sxs-lookup"><span data-stu-id="f6246-259">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="f6246-260">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="f6246-260">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="f6246-261">Windows Server 2016 i Windows 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="f6246-261">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="f6246-262">Lokalizacja docelowa: Nie dotyczy wdrożeń HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="f6246-262">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="f6246-263">Usługi IIS (poza procesem)</span><span class="sxs-lookup"><span data-stu-id="f6246-263">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="f6246-264">Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="f6246-264">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="f6246-265">Połączenia z serwerem usługi edge publicznego używać protokołu HTTP/2, ale połączenie zwrotny serwer proxy Kestrel korzysta z protokołu HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="f6246-265">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="f6246-266">Lokalizacja docelowa: Nie dotyczy wdrożeń spoza procesu usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f6246-266">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="f6246-267">Należy użyć połączenia protokołu HTTP/2 [negocjowania protokołu warstwy aplikacji (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) i TLS 1.2 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="f6246-267">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="f6246-268">Aby uzyskać więcej informacji zobacz tematy, które odnoszą się do scenariuszy wdrażania serwera.</span><span class="sxs-lookup"><span data-stu-id="f6246-268">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f6246-269">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f6246-269">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys>
