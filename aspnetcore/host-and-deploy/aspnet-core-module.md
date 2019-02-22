---
title: Moduł ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować modułu ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/19/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: e7eed467a0f54df5d0e067efabf6f821b7647d70
ms.sourcegitcommit: 0945078a09c372f17e9b003758ed87e99c2449f4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56647970"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="1439f-103">Moduł ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1439f-103">ASP.NET Core Module</span></span>

<span data-ttu-id="1439f-104">Przez [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1439f-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1439f-105">Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS albo:</span><span class="sxs-lookup"><span data-stu-id="1439f-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="1439f-106">Hostowanie aplikacji ASP.NET Core wewnątrz proces roboczy usług IIS (`w3wp.exe`), co jest nazywane [modelu hostingu w trakcie](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="1439f-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="1439f-107">Przesyła żądania sieci web do uruchamiania aplikacji ASP.NET Core zaplecza [serwera Kestrel](xref:fundamentals/servers/kestrel), co jest nazywane [modelu hostingu poza procesem](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="1439f-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="1439f-108">Obsługiwane wersje Windows:</span><span class="sxs-lookup"><span data-stu-id="1439f-108">Supported Windows versions:</span></span>

* <span data-ttu-id="1439f-109">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="1439f-109">Windows 7 or later</span></span>
* <span data-ttu-id="1439f-110">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="1439f-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="1439f-111">W przypadku hostowania w procesie, moduł używa implementacji w procesie serwera dla usług IIS, nazywany serwerem HTTP usług IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="1439f-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="1439f-112">Gdy w hostingu poza procesem, moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1439f-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="1439f-113">Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="1439f-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="1439f-114">Modele hostingu</span><span class="sxs-lookup"><span data-stu-id="1439f-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="1439f-115">W trakcie modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="1439f-115">In-process hosting model</span></span>

<span data-ttu-id="1439f-116">Aby skonfigurować aplikację do obsługi w procesie, Dodaj `<AspNetCoreHostingModel>` właściwość do pliku projektu aplikacji o wartości `InProcess` (spoza procesu hostingu została ustawiona za pomocą `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="1439f-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="1439f-117">Model hostingu w trakcie nie jest obsługiwana dla aplikacji platformy ASP.NET Core, które obsługują program .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1439f-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="1439f-118">Jeśli `<AspNetCoreHostingModel>` właściwość nie jest obecny w pliku, wartością domyślną jest `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="1439f-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="1439f-119">Następujące właściwości mają zastosowanie w przypadku hostowania w procesie:</span><span class="sxs-lookup"><span data-stu-id="1439f-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="1439f-120">Serwer HTTP usług IIS (`IISHttpServer`) jest używany zamiast [Kestrel](xref:fundamentals/servers/kestrel) serwera.</span><span class="sxs-lookup"><span data-stu-id="1439f-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="1439f-121">W procesie [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) wywołania <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> do:</span><span class="sxs-lookup"><span data-stu-id="1439f-121">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="1439f-122">Zarejestruj `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="1439f-122">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="1439f-123">Skonfiguruj port i ścieżka podstawowa serwera powinien nasłuchiwać podczas uruchamiania za modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1439f-123">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="1439f-124">Konfigurowanie hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1439f-124">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="1439f-125">[Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="1439f-125">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="1439f-126">Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="1439f-126">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="1439f-127">Użycie jednej puli aplikacji na aplikację.</span><span class="sxs-lookup"><span data-stu-id="1439f-127">Use one app pool per app.</span></span>

* <span data-ttu-id="1439f-128">Korzystając z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręczne wprowadzanie [plik app_offline.htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files), aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="1439f-128">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="1439f-129">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1439f-129">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="1439f-130">Architektura (liczba bitów) zainstalowanego środowiska uruchomieniowego (x64 lub x86) i aplikacji musi być zgodna z architekturą puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1439f-130">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="1439f-131">Ustawiając hosta aplikacji ręcznie za pomocą `WebHostBuilder` (nie używa [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) i nigdy nie uruchomienia aplikacji bezpośrednio na serwerze Kestrel (Self-Hosted), wywołanie `UseKestrel` przed wywołaniem `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="1439f-131">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="1439f-132">Jeśli kolejność została odwrócona, host nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="1439f-132">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="1439f-133">Rozłącza klienta są wykrywane.</span><span class="sxs-lookup"><span data-stu-id="1439f-133">Client disconnects are detected.</span></span> <span data-ttu-id="1439f-134">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) odwołano token anulowania, gdy klient odłączy się.</span><span class="sxs-lookup"><span data-stu-id="1439f-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="1439f-135"><xref:System.IO.Directory.GetCurrentDirectory*> Zwraca katalogu roboczego proces rozpoczęty przez usługi IIS, a nie w katalogu aplikacji (na przykład *C:\Windows\System32\inetsrv* dla *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="1439f-135"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="1439f-136">Przykładowy kod, który ustawia bieżący katalog aplikacji, zobacz [klasy CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="1439f-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="1439f-137">Wywołaj `SetCurrentDirectory` metody.</span><span class="sxs-lookup"><span data-stu-id="1439f-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="1439f-138">Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> zapewniają katalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1439f-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="1439f-139">Model hostingu poza procesem</span><span class="sxs-lookup"><span data-stu-id="1439f-139">Out-of-process hosting model</span></span>

<span data-ttu-id="1439f-140">Aby skonfigurować aplikację dla hostingu poza procesem, użyj jednej z poniższych metod w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="1439f-140">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="1439f-141">Nie określaj `<AspNetCoreHostingModel>` właściwości.</span><span class="sxs-lookup"><span data-stu-id="1439f-141">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="1439f-142">Jeśli `<AspNetCoreHostingModel>` właściwość nie jest obecny w pliku, wartością domyślną jest `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="1439f-142">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="1439f-143">Ustaw wartość `<AspNetCoreHostingModel>` właściwości `OutOfProcess` (hostowanie wewnątrzprocesowe została ustawiona za pomocą `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="1439f-143">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="1439f-144">[Kestrel](xref:fundamentals/servers/kestrel) serwer jest używany zamiast serwera HTTP usług IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="1439f-144">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="1439f-145">Dla procesu,- [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) wywołania <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> do:</span><span class="sxs-lookup"><span data-stu-id="1439f-145">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="1439f-146">Skonfiguruj port i ścieżka podstawowa serwera powinien nasłuchiwać podczas uruchamiania za modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1439f-146">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="1439f-147">Konfigurowanie hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1439f-147">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="1439f-148">Zmiany modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="1439f-148">Hosting model changes</span></span>

<span data-ttu-id="1439f-149">Jeśli `hostingModel` ustawienie to ulegnie zmianie w *web.config* pliku (wyjaśnione w [konfiguracji z pliku web.config](#configuration-with-webconfig) sekcji), moduł odtwarzania procesu roboczego programu IIS.</span><span class="sxs-lookup"><span data-stu-id="1439f-149">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="1439f-150">Dla usług IIS Express moduł nie odtworzyć proces roboczy, ale zamiast tego wyzwala łagodne zamykanie bieżący proces usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1439f-150">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="1439f-151">Kolejne żądanie aplikacji spowoduje utworzenie nowych procesów usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1439f-151">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="1439f-152">Nazwa procesu</span><span class="sxs-lookup"><span data-stu-id="1439f-152">Process name</span></span>

<span data-ttu-id="1439f-153">`Process.GetCurrentProcess().ProcessName` Raporty `w3wp` / `iisexpress` (w procesie) lub `dotnet` (poza procesem).</span><span class="sxs-lookup"><span data-stu-id="1439f-153">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1439f-154">Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS do przekazywania żądań sieci web z zapleczem platformy ASP.NET Core z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1439f-154">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="1439f-155">Obsługiwane wersje Windows:</span><span class="sxs-lookup"><span data-stu-id="1439f-155">Supported Windows versions:</span></span>

* <span data-ttu-id="1439f-156">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="1439f-156">Windows 7 or later</span></span>
* <span data-ttu-id="1439f-157">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="1439f-157">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="1439f-158">Moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1439f-158">The module only works with Kestrel.</span></span> <span data-ttu-id="1439f-159">Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="1439f-159">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="1439f-160">Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje także zarządzanie procesem.</span><span class="sxs-lookup"><span data-stu-id="1439f-160">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="1439f-161">Moduł rozpoczyna się proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera i ponownie uruchamia aplikację w przypadku jej awarii.</span><span class="sxs-lookup"><span data-stu-id="1439f-161">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="1439f-162">Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje ASP.NET 4.x, działających w trakcie w usługach IIS, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="1439f-162">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="1439f-163">Na poniższym diagramie przedstawiono relację między usług IIS, modułu ASP.NET Core i aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1439f-163">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="1439f-165">Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="1439f-165">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="1439f-166">Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="1439f-166">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="1439f-167">Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80 i 443.</span><span class="sxs-lookup"><span data-stu-id="1439f-167">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="1439f-168">Moduł Określa port, za pośrednictwem zmiennej środowiskowej przy uruchamianiu i oprogramowania pośredniczącego integracji usług IIS umożliwia skonfigurowanie serwera do nasłuchiwania `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="1439f-168">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="1439f-169">Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="1439f-169">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="1439f-170">Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1439f-170">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="1439f-171">Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1439f-171">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="1439f-172">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="1439f-172">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="1439f-173">Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1439f-173">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="1439f-174">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="1439f-174">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="1439f-175">Wiele modułów macierzystych, takie jak uwierzytelnianie Windows pozostają aktywne.</span><span class="sxs-lookup"><span data-stu-id="1439f-175">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="1439f-176">Aby dowiedzieć się, jak aktywne moduły usług IIS przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="1439f-176">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="1439f-177">Modułu ASP.NET Core może również:</span><span class="sxs-lookup"><span data-stu-id="1439f-177">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="1439f-178">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="1439f-178">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="1439f-179">Rejestrowanie strumienia stdout, dane wyjściowe do usługi file storage dotyczące rozwiązywania problemów, uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="1439f-179">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="1439f-180">Przekazuj tokeny uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="1439f-180">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="1439f-181">Jak zainstalować i używać modułu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1439f-181">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="1439f-182">Aby uzyskać instrukcje na temat sposobu instalowania i używania modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="1439f-182">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="1439f-183">Konfiguracja z pliku web.config</span><span class="sxs-lookup"><span data-stu-id="1439f-183">Configuration with web.config</span></span>

<span data-ttu-id="1439f-184">Skonfigurowano modułu ASP.NET Core `aspNetCore` części `system.webServer` węzeł w tej witrynie *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="1439f-184">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="1439f-185">Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależny od struktury](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje modułu ASP.NET Core do obsługi żądań w lokacji:</span><span class="sxs-lookup"><span data-stu-id="1439f-185">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="1439f-186">Następujące *web.config* została opublikowana na potrzeby [niezależna wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="1439f-186">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="1439f-187"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Właściwość jest ustawiona na `false` do wskazania, że ustawienia określone w [ \<lokalizacja >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elementu nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1439f-187">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="1439f-188">Gdy aplikacja jest wdrażana na [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest równa `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="1439f-188">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="1439f-189">Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.</span><span class="sxs-lookup"><span data-stu-id="1439f-189">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="1439f-190">Aby uzyskać informacji na temat konfigurowania aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="1439f-190">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="1439f-191">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="1439f-191">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="1439f-192">Atrybut</span><span class="sxs-lookup"><span data-stu-id="1439f-192">Attribute</span></span> | <span data-ttu-id="1439f-193">Opis</span><span class="sxs-lookup"><span data-stu-id="1439f-193">Description</span></span> | <span data-ttu-id="1439f-194">Domyślny</span><span class="sxs-lookup"><span data-stu-id="1439f-194">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1439f-195">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="1439f-195">Optional string attribute.</span></span></p><p><span data-ttu-id="1439f-196">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1439f-196">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1439f-197">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="1439f-197">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1439f-198">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="1439f-198">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1439f-199">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="1439f-199">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1439f-200">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="1439f-200">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1439f-201">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="1439f-201">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="1439f-202">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="1439f-202">Optional string attribute.</span></span></p><p><span data-ttu-id="1439f-203">Określa modelu hostingu w trakcie (`InProcess`) lub spoza procesu (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="1439f-203">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="1439f-204">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="1439f-204">Optional integer attribute.</span></span></p><p><span data-ttu-id="1439f-205">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="1439f-205">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="1439f-206">&dagger;W trakcie hostingu, wartość jest ograniczona do `1`.</span><span class="sxs-lookup"><span data-stu-id="1439f-206">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="1439f-207">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="1439f-207">Default: `1`</span></span><br><span data-ttu-id="1439f-208">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="1439f-208">Min: `1`</span></span><br><span data-ttu-id="1439f-209">Maks.: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="1439f-209">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="1439f-210">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="1439f-210">Required string attribute.</span></span></p><p><span data-ttu-id="1439f-211">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="1439f-211">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1439f-212">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="1439f-212">Relative paths are supported.</span></span> <span data-ttu-id="1439f-213">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="1439f-213">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1439f-214">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="1439f-214">Optional integer attribute.</span></span></p><p><span data-ttu-id="1439f-215">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="1439f-215">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1439f-216">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="1439f-216">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="1439f-217">Nieobsługiwane za pomocą wewnątrzprocesowego hostingu.</span><span class="sxs-lookup"><span data-stu-id="1439f-217">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="1439f-218">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="1439f-218">Default: `10`</span></span><br><span data-ttu-id="1439f-219">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="1439f-219">Min: `0`</span></span><br><span data-ttu-id="1439f-220">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="1439f-220">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1439f-221">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="1439f-221">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1439f-222">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="1439f-222">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1439f-223">W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="1439f-223">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="1439f-224">Nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="1439f-224">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="1439f-225">Dla hostingu w procesie modułu czeka na aplikację, aby przetworzyć żądanie.</span><span class="sxs-lookup"><span data-stu-id="1439f-225">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="1439f-226">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1439f-226">Default: `00:02:00`</span></span><br><span data-ttu-id="1439f-227">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1439f-227">Min: `00:00:00`</span></span><br><span data-ttu-id="1439f-228">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1439f-228">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1439f-229">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="1439f-229">Optional integer attribute.</span></span></p><p><span data-ttu-id="1439f-230">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="1439f-230">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1439f-231">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="1439f-231">Default: `10`</span></span><br><span data-ttu-id="1439f-232">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="1439f-232">Min: `0`</span></span><br><span data-ttu-id="1439f-233">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="1439f-233">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1439f-234">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="1439f-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="1439f-235">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="1439f-235">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1439f-236">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="1439f-236">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1439f-237">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="1439f-237">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="1439f-238">Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="1439f-238">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="1439f-239">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="1439f-239">Default: `120`</span></span><br><span data-ttu-id="1439f-240">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="1439f-240">Min: `0`</span></span><br><span data-ttu-id="1439f-241">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="1439f-241">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1439f-242">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="1439f-242">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1439f-243">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1439f-243">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1439f-244">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="1439f-244">Optional string attribute.</span></span></p><p><span data-ttu-id="1439f-245">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="1439f-245">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1439f-246">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="1439f-246">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1439f-247">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="1439f-247">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1439f-248">Wszystkie foldery w ścieżce są tworzone przez moduł, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="1439f-248">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="1439f-249">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="1439f-249">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1439f-250">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="1439f-250">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="1439f-251">Atrybut</span><span class="sxs-lookup"><span data-stu-id="1439f-251">Attribute</span></span> | <span data-ttu-id="1439f-252">Opis</span><span class="sxs-lookup"><span data-stu-id="1439f-252">Description</span></span> | <span data-ttu-id="1439f-253">Domyślny</span><span class="sxs-lookup"><span data-stu-id="1439f-253">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1439f-254">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="1439f-254">Optional string attribute.</span></span></p><p><span data-ttu-id="1439f-255">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1439f-255">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1439f-256">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="1439f-256">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1439f-257">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="1439f-257">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1439f-258">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="1439f-258">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1439f-259">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="1439f-259">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1439f-260">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="1439f-260">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="1439f-261">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="1439f-261">Optional integer attribute.</span></span></p><p><span data-ttu-id="1439f-262">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="1439f-262">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="1439f-263">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="1439f-263">Default: `1`</span></span><br><span data-ttu-id="1439f-264">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="1439f-264">Min: `1`</span></span><br><span data-ttu-id="1439f-265">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="1439f-265">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="1439f-266">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="1439f-266">Required string attribute.</span></span></p><p><span data-ttu-id="1439f-267">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="1439f-267">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1439f-268">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="1439f-268">Relative paths are supported.</span></span> <span data-ttu-id="1439f-269">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="1439f-269">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1439f-270">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="1439f-270">Optional integer attribute.</span></span></p><p><span data-ttu-id="1439f-271">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="1439f-271">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1439f-272">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="1439f-272">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="1439f-273">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="1439f-273">Default: `10`</span></span><br><span data-ttu-id="1439f-274">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="1439f-274">Min: `0`</span></span><br><span data-ttu-id="1439f-275">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="1439f-275">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1439f-276">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="1439f-276">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1439f-277">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="1439f-277">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1439f-278">W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="1439f-278">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="1439f-279">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1439f-279">Default: `00:02:00`</span></span><br><span data-ttu-id="1439f-280">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1439f-280">Min: `00:00:00`</span></span><br><span data-ttu-id="1439f-281">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1439f-281">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1439f-282">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="1439f-282">Optional integer attribute.</span></span></p><p><span data-ttu-id="1439f-283">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="1439f-283">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1439f-284">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="1439f-284">Default: `10`</span></span><br><span data-ttu-id="1439f-285">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="1439f-285">Min: `0`</span></span><br><span data-ttu-id="1439f-286">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="1439f-286">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1439f-287">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="1439f-287">Optional integer attribute.</span></span></p><p><span data-ttu-id="1439f-288">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="1439f-288">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1439f-289">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="1439f-289">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1439f-290">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="1439f-290">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="1439f-291">Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="1439f-291">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="1439f-292">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="1439f-292">Default: `120`</span></span><br><span data-ttu-id="1439f-293">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="1439f-293">Min: `0`</span></span><br><span data-ttu-id="1439f-294">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="1439f-294">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1439f-295">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="1439f-295">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1439f-296">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1439f-296">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1439f-297">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="1439f-297">Optional string attribute.</span></span></p><p><span data-ttu-id="1439f-298">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="1439f-298">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1439f-299">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="1439f-299">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1439f-300">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="1439f-300">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1439f-301">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="1439f-301">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1439f-302">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="1439f-302">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1439f-303">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="1439f-303">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="1439f-304">Atrybut</span><span class="sxs-lookup"><span data-stu-id="1439f-304">Attribute</span></span> | <span data-ttu-id="1439f-305">Opis</span><span class="sxs-lookup"><span data-stu-id="1439f-305">Description</span></span> | <span data-ttu-id="1439f-306">Domyślny</span><span class="sxs-lookup"><span data-stu-id="1439f-306">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1439f-307">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="1439f-307">Optional string attribute.</span></span></p><p><span data-ttu-id="1439f-308">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1439f-308">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1439f-309">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="1439f-309">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1439f-310">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="1439f-310">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1439f-311">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="1439f-311">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1439f-312">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="1439f-312">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1439f-313">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="1439f-313">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="1439f-314">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="1439f-314">Optional integer attribute.</span></span></p><p><span data-ttu-id="1439f-315">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="1439f-315">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="1439f-316">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="1439f-316">Default: `1`</span></span><br><span data-ttu-id="1439f-317">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="1439f-317">Min: `1`</span></span><br><span data-ttu-id="1439f-318">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="1439f-318">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="1439f-319">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="1439f-319">Required string attribute.</span></span></p><p><span data-ttu-id="1439f-320">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="1439f-320">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1439f-321">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="1439f-321">Relative paths are supported.</span></span> <span data-ttu-id="1439f-322">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="1439f-322">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1439f-323">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="1439f-323">Optional integer attribute.</span></span></p><p><span data-ttu-id="1439f-324">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="1439f-324">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1439f-325">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="1439f-325">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="1439f-326">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="1439f-326">Default: `10`</span></span><br><span data-ttu-id="1439f-327">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="1439f-327">Min: `0`</span></span><br><span data-ttu-id="1439f-328">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="1439f-328">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1439f-329">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="1439f-329">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1439f-330">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="1439f-330">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1439f-331">W wersjach modułu ASP.NET Core, dostarczonej wraz z wydaniem programu ASP.NET Core 2.0 lub wcześniej `requestTimeout` musi być określona w pełnych minut, w przeciwnym razie jego wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="1439f-331">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="1439f-332">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1439f-332">Default: `00:02:00`</span></span><br><span data-ttu-id="1439f-333">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1439f-333">Min: `00:00:00`</span></span><br><span data-ttu-id="1439f-334">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1439f-334">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1439f-335">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="1439f-335">Optional integer attribute.</span></span></p><p><span data-ttu-id="1439f-336">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="1439f-336">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1439f-337">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="1439f-337">Default: `10`</span></span><br><span data-ttu-id="1439f-338">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="1439f-338">Min: `0`</span></span><br><span data-ttu-id="1439f-339">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="1439f-339">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1439f-340">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="1439f-340">Optional integer attribute.</span></span></p><p><span data-ttu-id="1439f-341">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="1439f-341">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1439f-342">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="1439f-342">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1439f-343">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="1439f-343">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="1439f-344">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="1439f-344">Default: `120`</span></span><br><span data-ttu-id="1439f-345">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="1439f-345">Min: `0`</span></span><br><span data-ttu-id="1439f-346">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="1439f-346">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1439f-347">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="1439f-347">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1439f-348">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1439f-348">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1439f-349">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="1439f-349">Optional string attribute.</span></span></p><p><span data-ttu-id="1439f-350">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="1439f-350">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1439f-351">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="1439f-351">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1439f-352">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="1439f-352">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1439f-353">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="1439f-353">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1439f-354">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="1439f-354">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1439f-355">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="1439f-355">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="1439f-356">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="1439f-356">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1439f-357">Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1439f-357">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="1439f-358">Określ zmienną środowiskową `<environmentVariable>` element podrzędny elementu `<environmentVariables>` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="1439f-358">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="1439f-359">Zmienne środowiskowe ustawione w tej sekcji pierwszeństwo systemowe zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="1439f-359">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1439f-360">Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1439f-360">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="1439f-361">Określ zmienną środowiskową `<environmentVariable>` element podrzędny elementu `<environmentVariables>` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="1439f-361">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="1439f-362">Ustaw zmienne środowiskowe ustawione w tej sekcji wystąpienie konfliktu, za pomocą zmiennych środowiskowych systemu o takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="1439f-362">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="1439f-363">Jeśli zmienna środowiskowa jest ustawiana w obu *web.config* plik, a w systemie poziomu w Windows, wartość *web.config* plik staje się dołączany do wartości zmiennej środowiskowej systemu (na potrzeby przykład `ASPNETCORE_ENVIRONMENT: Development;Development`), który uniemożliwia uruchomienie przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="1439f-363">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="1439f-364">W poniższym przykładzie ustawiono dwóch zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="1439f-364">The following example sets two environment variables.</span></span> <span data-ttu-id="1439f-365">`ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji `Development`.</span><span class="sxs-lookup"><span data-stu-id="1439f-365">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="1439f-366">Deweloper może tymczasowo ustawić tę wartość w *web.config* pliku, aby wymusić [stronie wyjątków deweloperów](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek.</span><span class="sxs-lookup"><span data-stu-id="1439f-366">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="1439f-367">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, gdzie deweloper ma napisany kod, który odczytuje wartość przy uruchamianiu w celu utworzenia ścieżki do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1439f-367">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="1439f-368">Zamiast bezpośrednio w konfiguracji środowiska *web.config* ma zawierać `<EnvironmentName>` właściwości w profilu publikowania (*.pubxml*) lub pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="1439f-368">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="1439f-369">To podejście ustawia środowisko *web.config* po opublikowaniu projektu:</span><span class="sxs-lookup"><span data-stu-id="1439f-369">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="1439f-370">Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej, aby `Development` przejściowym i testowania serwerów, które nie są dostępne do niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="1439f-370">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="1439f-371">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="1439f-371">app_offline.htm</span></span>

<span data-ttu-id="1439f-372">Jeśli plik o nazwie *app_offline.htm* wykryte w katalogu głównym aplikacji, modułu ASP.NET Core próbuje łagodne zamykanie aplikacji i zatrzymanie przetwarzania przychodzących żądań.</span><span class="sxs-lookup"><span data-stu-id="1439f-372">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="1439f-373">Jeśli aplikacja jest nadal uruchomione po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, modułu ASP.NET Core to złe rozwiązanie uruchomionego procesu.</span><span class="sxs-lookup"><span data-stu-id="1439f-373">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="1439f-374">Gdy *app_offline.htm* występuje pliku modułu ASP.NET Core ma odpowiadać na żądania, wysyłając ponownie zawartość *app_offline.htm* pliku.</span><span class="sxs-lookup"><span data-stu-id="1439f-374">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="1439f-375">Gdy *app_offline.htm* plik jest usuwany, kolejne żądanie uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="1439f-375">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1439f-376">Korzystając z modelu hostingu poza procesem, aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="1439f-376">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="1439f-377">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1439f-377">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="1439f-378">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="1439f-378">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1439f-379">Zarówno w trakcie, jak i spoza procesu hostingu generuje strony błędów niestandardowych, gdy mogą zakończyć się niepowodzeniem uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="1439f-379">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="1439f-380">Jeśli modułu ASP.NET Core nie uda się znaleźć albo w trakcie lub poza procesem programu obsługi żądania, *500.0 — Błąd ładowania obsługi w-procesu/Out-Of-Process* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="1439f-380">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="1439f-381">W trakcie obsługi w przypadku niepowodzenia modułu ASP.NET Core uruchomić aplikację, *500.30 - Start błąd* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="1439f-381">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="1439f-382">Dla hostingu poza procesem, jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="1439f-382">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="1439f-383">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 5xx usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1439f-383">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="1439f-384">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="1439f-384">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1439f-385">Jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="1439f-385">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="1439f-386">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 502 usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1439f-386">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="1439f-387">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="1439f-387">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 strona kodowa stan niepowodzenia procesu](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="1439f-389">Tworzenia dziennika i Przekierowanie</span><span class="sxs-lookup"><span data-stu-id="1439f-389">Log creation and redirection</span></span>

<span data-ttu-id="1439f-390">Modułu ASP.NET Core przekierowuje dane wyjściowe stdout i stderr konsoli do dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu są ustawione.</span><span class="sxs-lookup"><span data-stu-id="1439f-390">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="1439f-391">Wszystkie foldery w `stdoutLogFile` ścieżki są tworzone przez moduł, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="1439f-391">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="1439f-392">Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="1439f-392">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="1439f-393">Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="1439f-393">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="1439f-394">Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="1439f-394">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="1439f-395">Przy użyciu dziennika stdout jest zalecane tylko na potrzeby rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1439f-395">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="1439f-396">Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1439f-396">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="1439f-397">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="1439f-397">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="1439f-398">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="1439f-398">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="1439f-399">Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="1439f-399">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="1439f-400">Przez dodanie sygnatury czasowej, identyfikator procesu i rozszerzenie pliku składa się nazwa pliku dziennika (*.log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="1439f-400">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="1439f-401">Jeśli `stdoutLogFile` ścieżki kończy się *stdout*, dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzono 19:42:32 2/5/2018 o nazwie pliku *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="1439f-401">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1439f-402">Jeśli `stdoutLogEnabled` ma wartość FAŁSZ, błędów występujących podczas uruchamiania aplikacji są przechwytywane i wysyłanego do dziennika zdarzeń do 30 KB.</span><span class="sxs-lookup"><span data-stu-id="1439f-402">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="1439f-403">Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="1439f-403">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="1439f-404">Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie strumienia stdout dla aplikacji hostowanej w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="1439f-404">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="1439f-405">Ścieżka lokalna lub ścieżkę do udziału sieciowego jest możliwa do logowania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1439f-405">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="1439f-406">Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.</span><span class="sxs-lookup"><span data-stu-id="1439f-406">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="1439f-407">Rozszerzone dzienników diagnostycznych</span><span class="sxs-lookup"><span data-stu-id="1439f-407">Enhanced diagnostic logs</span></span>

<span data-ttu-id="1439f-408">Modułu ASP.NET Core jest konfigurowane w celu zapewnienia dzienniki diagnostyki rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="1439f-408">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="1439f-409">Dodaj `<handlerSettings>` elementu `<aspNetCore>` element *web.config*. Ustawienie `debugLevel` do `TRACE` udostępnia pozwala uzyskać większą wierność informacje diagnostyczne:</span><span class="sxs-lookup"><span data-stu-id="1439f-409">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="1439f-410">Poziom debugowania (`debugLevel`) wartości mogą obejmować zarówno na poziomie, jak i lokalizację.</span><span class="sxs-lookup"><span data-stu-id="1439f-410">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="1439f-411">Poziomy (w kolejności od najmniejszej do najbardziej szczegółowy):</span><span class="sxs-lookup"><span data-stu-id="1439f-411">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="1439f-412">BŁĄD</span><span class="sxs-lookup"><span data-stu-id="1439f-412">ERROR</span></span>
* <span data-ttu-id="1439f-413">OSTRZEŻENIE</span><span class="sxs-lookup"><span data-stu-id="1439f-413">WARNING</span></span>
* <span data-ttu-id="1439f-414">INFORMACJE O</span><span class="sxs-lookup"><span data-stu-id="1439f-414">INFO</span></span>
* <span data-ttu-id="1439f-415">TRACE</span><span class="sxs-lookup"><span data-stu-id="1439f-415">TRACE</span></span>

<span data-ttu-id="1439f-416">Lokalizacje (wiele lokalizacji są dozwolone):</span><span class="sxs-lookup"><span data-stu-id="1439f-416">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="1439f-417">KONSOLA</span><span class="sxs-lookup"><span data-stu-id="1439f-417">CONSOLE</span></span>
* <span data-ttu-id="1439f-418">DZIENNIK ZDARZEŃ</span><span class="sxs-lookup"><span data-stu-id="1439f-418">EVENTLOG</span></span>
* <span data-ttu-id="1439f-419">PLIK</span><span class="sxs-lookup"><span data-stu-id="1439f-419">FILE</span></span>

<span data-ttu-id="1439f-420">Ustawienia obsługi można również podać za pośrednictwem zmienne środowiskowe:</span><span class="sxs-lookup"><span data-stu-id="1439f-420">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="1439f-421">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ścieżka do pliku dziennika debugowania.</span><span class="sxs-lookup"><span data-stu-id="1439f-421">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="1439f-422">(Domyślnie: *czy aspnetcore*)</span><span class="sxs-lookup"><span data-stu-id="1439f-422">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="1439f-423">`ASPNETCORE_MODULE_DEBUG` &ndash; Ustawienie poziomie debugowania.</span><span class="sxs-lookup"><span data-stu-id="1439f-423">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="1439f-424">Czy **nie** Pozostaw włączone we wdrożeniu na dłużej, niż jest to wymagane, aby rozwiązać problem rejestrowanie debugowania.</span><span class="sxs-lookup"><span data-stu-id="1439f-424">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="1439f-425">Rozmiar dziennika nie jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="1439f-425">The size of the log isn't limited.</span></span> <span data-ttu-id="1439f-426">Pozostawienie dziennik debugowania włączone może wyczerpać dostępne miejsce na dysku i awarii serwera lub usługi app service.</span><span class="sxs-lookup"><span data-stu-id="1439f-426">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="1439f-427">Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="1439f-427">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="1439f-428">Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token</span><span class="sxs-lookup"><span data-stu-id="1439f-428">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1439f-429">*Dotyczy tylko hostingu poza procesem.*</span><span class="sxs-lookup"><span data-stu-id="1439f-429">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="1439f-430">Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1439f-430">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="1439f-431">Przy użyciu protokołu HTTP jest Optymalizacja wydajności, gdzie ruch między modułem i Kestrel odbywa się na adres sprzężenia zwrotnego zniżki w stosunku do interfejsu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="1439f-431">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="1439f-432">Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="1439f-432">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="1439f-433">Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="1439f-433">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="1439f-434">Parowania token jest utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="1439f-434">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="1439f-435">Token parowania jest również ustawiona w nagłówku (`MS-ASPNETCORE-TOKEN`) na każde żądanie serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="1439f-435">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="1439f-436">Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="1439f-436">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="1439f-437">Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="1439f-437">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="1439f-438">Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="1439f-438">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="1439f-439">Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="1439f-439">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="1439f-440">Moduł ASP.NET Core z programem IIS konfigurację udostępnioną</span><span class="sxs-lookup"><span data-stu-id="1439f-440">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="1439f-441">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami **zaufany Instalator** konta.</span><span class="sxs-lookup"><span data-stu-id="1439f-441">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="1439f-442">Ponieważ lokalne konto systemowe nie ma uprawnienia do modyfikowania dla ścieżki udziału konfiguracji udostępnionej usług IIS, Instalator zgłasza odmowa dostępu błąd podczas próby skonfigurowania ustawień modułu w *applicationHost.config*  pliku w udziale.</span><span class="sxs-lookup"><span data-stu-id="1439f-442">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1439f-443">Podczas korzystania z konfiguracji udostępnionej usług IIS, w tym samym komputerze co instalacji usług IIS, uruchom Instalatora programu ASP.NET Core hostingu pakietu z `OPT_NO_SHARED_CONFIG_CHECK` parametr `1`:</span><span class="sxs-lookup"><span data-stu-id="1439f-443">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="1439f-444">Gdy na tym samym komputerze co instalacji usług IIS nie jest ścieżką do konfiguracji udostępnionej, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="1439f-444">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="1439f-445">Wyłącz konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="1439f-445">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="1439f-446">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="1439f-446">Run the installer.</span></span>
1. <span data-ttu-id="1439f-447">Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.</span><span class="sxs-lookup"><span data-stu-id="1439f-447">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="1439f-448">Ponownie włączyć konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="1439f-448">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1439f-449">Korzystając z konfiguracji udostępnionej usług IIS, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="1439f-449">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="1439f-450">Wyłącz konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="1439f-450">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="1439f-451">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="1439f-451">Run the installer.</span></span>
1. <span data-ttu-id="1439f-452">Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.</span><span class="sxs-lookup"><span data-stu-id="1439f-452">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="1439f-453">Ponownie włączyć konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="1439f-453">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization"></a><span data-ttu-id="1439f-454">Inicjowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="1439f-454">Application Initialization</span></span>

<span data-ttu-id="1439f-455">[Inicjowanie aplikacji usług IIS](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) to funkcja usług IIS, która wysyła żądania HTTP do aplikacji, gdy pula aplikacji rozpoczyna się lub zostanie odtworzona.</span><span class="sxs-lookup"><span data-stu-id="1439f-455">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="1439f-456">Żądanie wyzwala uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1439f-456">The request triggers the app to start.</span></span> <span data-ttu-id="1439f-457">Inicjowanie aplikacji mogą być używane przez oba [modelu hostingu w trakcie](xref:fundamentals/servers/index#in-process-hosting-model) i [modelu hostingu poza procesem](xref:fundamentals/servers/index#out-of-process-hosting-model) przy użyciu modułu ASP.NET Core w wersji 2.</span><span class="sxs-lookup"><span data-stu-id="1439f-457">Application Initialization can be used by both the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model) and [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) with the ASP.NET Core Module version 2.</span></span>

<span data-ttu-id="1439f-458">Aby włączyć inicjowania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1439f-458">To enable Application Initialization:</span></span>

1. <span data-ttu-id="1439f-459">Upewnij się, że włączona funkcja roli Inicjowanie aplikacji usług IIS w:</span><span class="sxs-lookup"><span data-stu-id="1439f-459">Confirm that the IIS Application Initialization role feature in enabled:</span></span>
   * <span data-ttu-id="1439f-460">Windows 7 lub nowszy: Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Windows Włącz funkcje w lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="1439f-460">On Windows 7 or later: Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="1439f-461">Otwórz **Internetowe usługi informacyjne** > **usługi World Wide Web** > **funkcje tworzenia aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="1439f-461">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="1439f-462">Zaznacz pole wyboru dla **Inicjowanie aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="1439f-462">Select the check box for **Application Initialization**.</span></span>
   * <span data-ttu-id="1439f-463">W systemie Windows Server 2008 R2 lub nowszym, otwórz **Kreatora dodawania ról i funkcji**.</span><span class="sxs-lookup"><span data-stu-id="1439f-463">On Windows Server 2008 R2 or later, open the **Add Roles and Features Wizard**.</span></span> <span data-ttu-id="1439f-464">Po przejściu **Wybieranie usług ról** panelu Otwórz **opracowywanie aplikacji** a następnie wybierz węzeł **Inicjowanie aplikacji** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="1439f-464">When you reach the **Select role services** panel, open the **Application Development** node and select the **Application Initialization** check box.</span></span>
1. <span data-ttu-id="1439f-465">W Menedżerze usług IIS wybierz **pul aplikacji** w **połączeń** panelu.</span><span class="sxs-lookup"><span data-stu-id="1439f-465">In IIS Manager, select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="1439f-466">Wybierz pulę aplikacji przez aplikację na liście.</span><span class="sxs-lookup"><span data-stu-id="1439f-466">Select the app's app pool in the list.</span></span>
1. <span data-ttu-id="1439f-467">Wybierz **Zaawansowane ustawienia** w obszarze **edytowanie puli aplikacji** w **akcje** panelu.</span><span class="sxs-lookup"><span data-stu-id="1439f-467">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="1439f-468">Ustaw **tryb uruchamiania** do **AlwaysRunning**.</span><span class="sxs-lookup"><span data-stu-id="1439f-468">Set **Start Mode** to **AlwaysRunning**.</span></span>
1. <span data-ttu-id="1439f-469">Otwórz **witryn** w węźle **połączeń** panelu.</span><span class="sxs-lookup"><span data-stu-id="1439f-469">Open the **Sites** node in the **Connections** panel.</span></span>
1. <span data-ttu-id="1439f-470">Wybierz aplikację.</span><span class="sxs-lookup"><span data-stu-id="1439f-470">Select the app.</span></span>
1. <span data-ttu-id="1439f-471">Wybierz **Zaawansowane ustawienia** w obszarze **Zarządzaj witryną internetową** w **akcje** panelu.</span><span class="sxs-lookup"><span data-stu-id="1439f-471">Select **Advanced Settings** under **Manage Website** in the **Actions** panel.</span></span>
1. <span data-ttu-id="1439f-472">Ustaw **wstępne załadowanie włączone** do **True**.</span><span class="sxs-lookup"><span data-stu-id="1439f-472">Set **Preload Enabled** to **True**.</span></span>

<span data-ttu-id="1439f-473">Aby uzyskać więcej informacji, zobacz [Inicjowanie aplikacji programu IIS 8.0](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span><span class="sxs-lookup"><span data-stu-id="1439f-473">For more information, see [IIS 8.0 Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span></span>

<span data-ttu-id="1439f-474">Aplikacje korzystające z [modelu hostingu poza procesem](xref:fundamentals/servers/index#out-of-process-hosting-model) musi być okresowo wysyłać polecenie ping w aplikacji w celu zapewnienia jego działania usługi zewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="1439f-474">Apps that use the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) must use an external service to periodically ping the app in order to keep it running.</span></span>

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="1439f-475">Wersja modułu oraz Dzienniki Instalatora pakietu hostingu</span><span class="sxs-lookup"><span data-stu-id="1439f-475">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="1439f-476">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="1439f-476">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="1439f-477">W systemie hostingu, przejdź do *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="1439f-477">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="1439f-478">Znajdź *aspnetcore.dll* pliku.</span><span class="sxs-lookup"><span data-stu-id="1439f-478">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="1439f-479">Kliknij prawym przyciskiem myszy plik i wybierz **właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="1439f-479">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="1439f-480">Wybierz **szczegóły** kartę. **Wersja pliku** i **wersji produktu** reprezentują zainstalowanej wersji modułu.</span><span class="sxs-lookup"><span data-stu-id="1439f-480">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="1439f-481">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się w *C:\\użytkowników\\% UserName %\\AppData\\lokalnego\\Temp*. Plik *dd_DotNetCoreWinSvrHosting__\<sygnatura czasowa > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="1439f-481">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="1439f-482">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="1439f-482">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="1439f-483">Moduł</span><span class="sxs-lookup"><span data-stu-id="1439f-483">Module</span></span>

<span data-ttu-id="1439f-484">**Usługi IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="1439f-484">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="1439f-485">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1439f-485">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="1439f-486">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1439f-486">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1439f-487">%ProgramFiles%\IIS\Asp.NET core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1439f-487">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="1439f-488">% ProgramFiles (x86) %\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1439f-488">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="1439f-489">**Usługi IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="1439f-489">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="1439f-490">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1439f-490">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="1439f-491">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1439f-491">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1439f-492">%ProgramFiles%\iis Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1439f-492">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="1439f-493">% ProgramFiles (x86) %\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1439f-493">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="1439f-494">Schemat</span><span class="sxs-lookup"><span data-stu-id="1439f-494">Schema</span></span>

<span data-ttu-id="1439f-495">**IIS**</span><span class="sxs-lookup"><span data-stu-id="1439f-495">**IIS**</span></span>

   * <span data-ttu-id="1439f-496">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="1439f-496">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1439f-497">%Windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="1439f-497">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="1439f-498">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="1439f-498">**IIS Express**</span></span>

   * <span data-ttu-id="1439f-499">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="1439f-499">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1439f-500">%ProgramFiles%\iis Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="1439f-500">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="1439f-501">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="1439f-501">Configuration</span></span>

<span data-ttu-id="1439f-502">**IIS**</span><span class="sxs-lookup"><span data-stu-id="1439f-502">**IIS**</span></span>

   * <span data-ttu-id="1439f-503">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="1439f-503">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="1439f-504">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="1439f-504">**IIS Express**</span></span>

   * <span data-ttu-id="1439f-505">Visual Studio: {Katalog główny aplikacji}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="1439f-505">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>
   
   * <span data-ttu-id="1439f-506">*iisexpress.exe* interfejsu wiersza polecenia: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="1439f-506">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="1439f-507">Pliki można znaleźć, wyszukując *aspnetcore* w *applicationHost.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="1439f-507">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1439f-508">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1439f-508">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="1439f-509">Repozytorium GitHub modułów Core ASP.NET (źródło odwołania)</span><span class="sxs-lookup"><span data-stu-id="1439f-509">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
