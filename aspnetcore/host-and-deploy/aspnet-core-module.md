---
title: Moduł ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować modułu ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: dee4fe7a498d211cb8ef6a3c49017c3cc8a56847
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637862"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="2a720-103">Moduł ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a720-103">ASP.NET Core Module</span></span>

<span data-ttu-id="2a720-104">Przez [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2a720-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2a720-105">Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS albo:</span><span class="sxs-lookup"><span data-stu-id="2a720-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="2a720-106">Hostowanie aplikacji ASP.NET Core wewnątrz proces roboczy usług IIS (`w3wp.exe`), co jest nazywane [modelu hostingu w trakcie](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="2a720-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="2a720-107">Przesyła żądania sieci web do uruchamiania aplikacji ASP.NET Core zaplecza [serwera Kestrel](xref:fundamentals/servers/kestrel), co jest nazywane [modelu hostingu poza procesem](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="2a720-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="2a720-108">Obsługiwane wersje Windows:</span><span class="sxs-lookup"><span data-stu-id="2a720-108">Supported Windows versions:</span></span>

* <span data-ttu-id="2a720-109">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="2a720-109">Windows 7 or later</span></span>
* <span data-ttu-id="2a720-110">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="2a720-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="2a720-111">W przypadku hostowania w procesie, moduł używa implementacji w procesie serwera dla usług IIS, nazywany serwerem HTTP usług IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="2a720-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="2a720-112">Gdy w hostingu poza procesem, moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2a720-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="2a720-113">Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="2a720-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="2a720-114">Modelach hostingu</span><span class="sxs-lookup"><span data-stu-id="2a720-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="2a720-115">W trakcie modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="2a720-115">In-process hosting model</span></span>

<span data-ttu-id="2a720-116">Aby skonfigurować aplikację do obsługi w procesie, Dodaj `<AspNetCoreHostingModel>` właściwość do pliku projektu aplikacji o wartości `InProcess` (spoza procesu hostingu została ustawiona za pomocą `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="2a720-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="2a720-117">Jeśli `<AspNetCoreHostingModel>` właściwość nie jest obecny w pliku, wartością domyślną jest `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="2a720-117">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="2a720-118">Następujące właściwości mają zastosowanie w przypadku hostowania w procesie:</span><span class="sxs-lookup"><span data-stu-id="2a720-118">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="2a720-119">Serwer HTTP usług IIS (`IISHttpServer`) jest używany zamiast [Kestrel](xref:fundamentals/servers/kestrel) serwera.</span><span class="sxs-lookup"><span data-stu-id="2a720-119">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

* <span data-ttu-id="2a720-120">[Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="2a720-120">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="2a720-121">Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="2a720-121">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="2a720-122">Użycie jednej puli aplikacji na aplikację.</span><span class="sxs-lookup"><span data-stu-id="2a720-122">Use one app pool per app.</span></span>

* <span data-ttu-id="2a720-123">Korzystając z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręczne wprowadzanie [plik app_offline.htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files), aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="2a720-123">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="2a720-124">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a720-124">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="2a720-125">Architektura (liczba bitów) zainstalowanego środowiska uruchomieniowego (x64 lub x86) i aplikacji musi być zgodna z architekturą puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a720-125">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="2a720-126">Ustawiając hosta aplikacji ręcznie za pomocą `WebHostBuilder` (nie używa [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) i nigdy nie uruchomienia aplikacji bezpośrednio na serwerze Kestrel (Self-Hosted), wywołanie `UseKestrel` przed wywołaniem `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="2a720-126">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="2a720-127">Jeśli kolejność została odwrócona, host nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="2a720-127">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="2a720-128">Rozłącza klienta są wykrywane.</span><span class="sxs-lookup"><span data-stu-id="2a720-128">Client disconnects are detected.</span></span> <span data-ttu-id="2a720-129">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) odwołano token anulowania, gdy klient odłączy się.</span><span class="sxs-lookup"><span data-stu-id="2a720-129">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="2a720-130"><xref:System.IO.Directory.GetCurrentDirectory*> Zwraca katalogu roboczego proces rozpoczęty przez usługi IIS, a nie w katalogu aplikacji (na przykład *C:\Windows\System32\inetsrv* dla *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="2a720-130"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="2a720-131">Przykładowy kod, który ustawia bieżący katalog aplikacji, zobacz [klasy CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="2a720-131">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="2a720-132">Wywołaj `SetCurrentDirectory` metody.</span><span class="sxs-lookup"><span data-stu-id="2a720-132">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="2a720-133">Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> zapewniają katalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a720-133">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="2a720-134">Model hostingu poza procesem</span><span class="sxs-lookup"><span data-stu-id="2a720-134">Out-of-process hosting model</span></span>

<span data-ttu-id="2a720-135">Aby skonfigurować aplikację dla hostingu poza procesem, użyj jednej z poniższych metod w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="2a720-135">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="2a720-136">Nie określaj `<AspNetCoreHostingModel>` właściwości.</span><span class="sxs-lookup"><span data-stu-id="2a720-136">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="2a720-137">Jeśli `<AspNetCoreHostingModel>` właściwość nie jest obecny w pliku, wartością domyślną jest `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="2a720-137">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="2a720-138">Ustaw wartość `<AspNetCoreHostingModel>` właściwości `OutOfProcess` (hostowanie wewnątrzprocesowe została ustawiona za pomocą `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="2a720-138">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="2a720-139">[Kestrel](xref:fundamentals/servers/kestrel) serwer jest używany zamiast serwera HTTP usług IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="2a720-139">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="2a720-140">Zmiany modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="2a720-140">Hosting model changes</span></span>

<span data-ttu-id="2a720-141">Jeśli `hostingModel` ustawienie to ulegnie zmianie w *web.config* pliku (wyjaśnione w [konfiguracji z pliku web.config](#configuration-with-webconfig) sekcji), moduł odtwarzania procesu roboczego programu IIS.</span><span class="sxs-lookup"><span data-stu-id="2a720-141">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="2a720-142">Dla usług IIS Express moduł nie odtworzyć proces roboczy, ale zamiast tego wyzwala łagodne zamykanie bieżący proces usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2a720-142">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="2a720-143">Kolejne żądanie aplikacji spowoduje utworzenie nowych procesów usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2a720-143">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="2a720-144">Nazwa procesu</span><span class="sxs-lookup"><span data-stu-id="2a720-144">Process name</span></span>

<span data-ttu-id="2a720-145">`Process.GetCurrentProcess().ProcessName` Raporty `w3wp` / `iisexpress` (w procesie) lub `dotnet` (poza procesem).</span><span class="sxs-lookup"><span data-stu-id="2a720-145">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="2a720-146">Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS do przekazywania żądań sieci web z zapleczem platformy ASP.NET Core z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a720-146">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="2a720-147">Obsługiwane wersje Windows:</span><span class="sxs-lookup"><span data-stu-id="2a720-147">Supported Windows versions:</span></span>

* <span data-ttu-id="2a720-148">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="2a720-148">Windows 7 or later</span></span>
* <span data-ttu-id="2a720-149">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="2a720-149">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="2a720-150">Moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2a720-150">The module only works with Kestrel.</span></span> <span data-ttu-id="2a720-151">Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="2a720-151">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="2a720-152">Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje także zarządzanie procesem.</span><span class="sxs-lookup"><span data-stu-id="2a720-152">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="2a720-153">Moduł rozpoczyna się proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera i ponownie uruchamia aplikację w przypadku jej awarii.</span><span class="sxs-lookup"><span data-stu-id="2a720-153">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="2a720-154">Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje ASP.NET 4.x, działających w trakcie w usługach IIS, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="2a720-154">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="2a720-155">Na poniższym diagramie przedstawiono relację między usług IIS, modułu ASP.NET Core i aplikacji:</span><span class="sxs-lookup"><span data-stu-id="2a720-155">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="2a720-157">Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="2a720-157">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="2a720-158">Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="2a720-158">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="2a720-159">Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80 i 443.</span><span class="sxs-lookup"><span data-stu-id="2a720-159">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="2a720-160">Moduł Określa port, za pośrednictwem zmiennej środowiskowej przy uruchamianiu i oprogramowania pośredniczącego integracji usług IIS umożliwia skonfigurowanie serwera do nasłuchiwania `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="2a720-160">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="2a720-161">Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="2a720-161">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="2a720-162">Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2a720-162">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="2a720-163">Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a720-163">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="2a720-164">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="2a720-164">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="2a720-165">Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2a720-165">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="2a720-166">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="2a720-166">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="2a720-167">Wiele modułów macierzystych, takie jak uwierzytelnianie Windows pozostają aktywne.</span><span class="sxs-lookup"><span data-stu-id="2a720-167">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="2a720-168">Aby dowiedzieć się, jak aktywne moduły usług IIS przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="2a720-168">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="2a720-169">Modułu ASP.NET Core może również:</span><span class="sxs-lookup"><span data-stu-id="2a720-169">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="2a720-170">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="2a720-170">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="2a720-171">Rejestrowanie strumienia stdout, dane wyjściowe do usługi file storage dotyczące rozwiązywania problemów, uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="2a720-171">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="2a720-172">Przekazuj tokeny uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="2a720-172">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="2a720-173">Jak zainstalować i używać modułu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a720-173">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="2a720-174">Aby uzyskać instrukcje na temat sposobu instalowania i używania modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="2a720-174">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="2a720-175">Konfiguracja z pliku web.config</span><span class="sxs-lookup"><span data-stu-id="2a720-175">Configuration with web.config</span></span>

<span data-ttu-id="2a720-176">Skonfigurowano modułu ASP.NET Core `aspNetCore` części `system.webServer` węzeł w tej witrynie *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="2a720-176">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="2a720-177">Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależny od struktury](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje modułu ASP.NET Core do obsługi żądań w lokacji:</span><span class="sxs-lookup"><span data-stu-id="2a720-177">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="2a720-178">Następujące *web.config* została opublikowana na potrzeby [niezależna wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="2a720-178">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="2a720-179"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Właściwość jest ustawiona na `false` do wskazania, że ustawienia określone w [ \<lokalizacja >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elementu nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a720-179">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="2a720-180">Gdy aplikacja jest wdrażana na [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest równa `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="2a720-180">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="2a720-181">Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.</span><span class="sxs-lookup"><span data-stu-id="2a720-181">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="2a720-182">Aby uzyskać informacji na temat konfigurowania aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="2a720-182">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="2a720-183">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="2a720-183">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="2a720-184">Atrybut</span><span class="sxs-lookup"><span data-stu-id="2a720-184">Attribute</span></span> | <span data-ttu-id="2a720-185">Opis</span><span class="sxs-lookup"><span data-stu-id="2a720-185">Description</span></span> | <span data-ttu-id="2a720-186">Domyślny</span><span class="sxs-lookup"><span data-stu-id="2a720-186">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="2a720-187">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="2a720-187">Optional string attribute.</span></span></p><p><span data-ttu-id="2a720-188">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2a720-188">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="2a720-189">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="2a720-189">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2a720-190">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="2a720-190">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="2a720-191">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="2a720-191">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2a720-192">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="2a720-192">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="2a720-193">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="2a720-193">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="2a720-194">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="2a720-194">Optional string attribute.</span></span></p><p><span data-ttu-id="2a720-195">Określa modelu hostingu w trakcie (`InProcess`) lub spoza procesu (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="2a720-195">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="2a720-196">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="2a720-196">Optional integer attribute.</span></span></p><p><span data-ttu-id="2a720-197">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="2a720-197">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="2a720-198">&dagger;W trakcie hostingu, wartość jest ograniczona do `1`.</span><span class="sxs-lookup"><span data-stu-id="2a720-198">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="2a720-199">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="2a720-199">Default: `1`</span></span><br><span data-ttu-id="2a720-200">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="2a720-200">Min: `1`</span></span><br><span data-ttu-id="2a720-201">Maks.: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="2a720-201">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="2a720-202">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="2a720-202">Required string attribute.</span></span></p><p><span data-ttu-id="2a720-203">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a720-203">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="2a720-204">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="2a720-204">Relative paths are supported.</span></span> <span data-ttu-id="2a720-205">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="2a720-205">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="2a720-206">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="2a720-206">Optional integer attribute.</span></span></p><p><span data-ttu-id="2a720-207">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="2a720-207">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="2a720-208">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="2a720-208">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="2a720-209">Nieobsługiwane za pomocą wewnątrzprocesowego hostingu.</span><span class="sxs-lookup"><span data-stu-id="2a720-209">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="2a720-210">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="2a720-210">Default: `10`</span></span><br><span data-ttu-id="2a720-211">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="2a720-211">Min: `0`</span></span><br><span data-ttu-id="2a720-212">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="2a720-212">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="2a720-213">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="2a720-213">Optional timespan attribute.</span></span></p><p><span data-ttu-id="2a720-214">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="2a720-214">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="2a720-215">W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="2a720-215">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="2a720-216">Nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="2a720-216">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="2a720-217">Dla hostingu w procesie modułu czeka na aplikację, aby przetworzyć żądanie.</span><span class="sxs-lookup"><span data-stu-id="2a720-217">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="2a720-218">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="2a720-218">Default: `00:02:00`</span></span><br><span data-ttu-id="2a720-219">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="2a720-219">Min: `00:00:00`</span></span><br><span data-ttu-id="2a720-220">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="2a720-220">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="2a720-221">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="2a720-221">Optional integer attribute.</span></span></p><p><span data-ttu-id="2a720-222">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="2a720-222">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="2a720-223">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="2a720-223">Default: `10`</span></span><br><span data-ttu-id="2a720-224">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="2a720-224">Min: `0`</span></span><br><span data-ttu-id="2a720-225">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="2a720-225">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="2a720-226">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="2a720-226">Optional integer attribute.</span></span></p><p><span data-ttu-id="2a720-227">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="2a720-227">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="2a720-228">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="2a720-228">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="2a720-229">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="2a720-229">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="2a720-230">Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="2a720-230">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="2a720-231">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="2a720-231">Default: `120`</span></span><br><span data-ttu-id="2a720-232">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="2a720-232">Min: `0`</span></span><br><span data-ttu-id="2a720-233">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="2a720-233">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="2a720-234">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="2a720-234">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2a720-235">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2a720-235">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="2a720-236">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="2a720-236">Optional string attribute.</span></span></p><p><span data-ttu-id="2a720-237">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="2a720-237">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="2a720-238">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="2a720-238">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="2a720-239">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="2a720-239">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="2a720-240">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="2a720-240">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2a720-241">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="2a720-241">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="2a720-242">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="2a720-242">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="2a720-243">Atrybut</span><span class="sxs-lookup"><span data-stu-id="2a720-243">Attribute</span></span> | <span data-ttu-id="2a720-244">Opis</span><span class="sxs-lookup"><span data-stu-id="2a720-244">Description</span></span> | <span data-ttu-id="2a720-245">Domyślny</span><span class="sxs-lookup"><span data-stu-id="2a720-245">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="2a720-246">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="2a720-246">Optional string attribute.</span></span></p><p><span data-ttu-id="2a720-247">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2a720-247">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="2a720-248">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="2a720-248">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2a720-249">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="2a720-249">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="2a720-250">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="2a720-250">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2a720-251">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="2a720-251">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="2a720-252">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="2a720-252">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="2a720-253">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="2a720-253">Optional integer attribute.</span></span></p><p><span data-ttu-id="2a720-254">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="2a720-254">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="2a720-255">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="2a720-255">Default: `1`</span></span><br><span data-ttu-id="2a720-256">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="2a720-256">Min: `1`</span></span><br><span data-ttu-id="2a720-257">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="2a720-257">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="2a720-258">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="2a720-258">Required string attribute.</span></span></p><p><span data-ttu-id="2a720-259">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a720-259">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="2a720-260">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="2a720-260">Relative paths are supported.</span></span> <span data-ttu-id="2a720-261">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="2a720-261">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="2a720-262">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="2a720-262">Optional integer attribute.</span></span></p><p><span data-ttu-id="2a720-263">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="2a720-263">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="2a720-264">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="2a720-264">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="2a720-265">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="2a720-265">Default: `10`</span></span><br><span data-ttu-id="2a720-266">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="2a720-266">Min: `0`</span></span><br><span data-ttu-id="2a720-267">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="2a720-267">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="2a720-268">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="2a720-268">Optional timespan attribute.</span></span></p><p><span data-ttu-id="2a720-269">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="2a720-269">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="2a720-270">W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="2a720-270">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="2a720-271">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="2a720-271">Default: `00:02:00`</span></span><br><span data-ttu-id="2a720-272">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="2a720-272">Min: `00:00:00`</span></span><br><span data-ttu-id="2a720-273">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="2a720-273">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="2a720-274">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="2a720-274">Optional integer attribute.</span></span></p><p><span data-ttu-id="2a720-275">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="2a720-275">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="2a720-276">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="2a720-276">Default: `10`</span></span><br><span data-ttu-id="2a720-277">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="2a720-277">Min: `0`</span></span><br><span data-ttu-id="2a720-278">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="2a720-278">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="2a720-279">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="2a720-279">Optional integer attribute.</span></span></p><p><span data-ttu-id="2a720-280">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="2a720-280">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="2a720-281">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="2a720-281">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="2a720-282">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="2a720-282">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="2a720-283">Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="2a720-283">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="2a720-284">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="2a720-284">Default: `120`</span></span><br><span data-ttu-id="2a720-285">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="2a720-285">Min: `0`</span></span><br><span data-ttu-id="2a720-286">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="2a720-286">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="2a720-287">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="2a720-287">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2a720-288">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2a720-288">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="2a720-289">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="2a720-289">Optional string attribute.</span></span></p><p><span data-ttu-id="2a720-290">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="2a720-290">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="2a720-291">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="2a720-291">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="2a720-292">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="2a720-292">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="2a720-293">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="2a720-293">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2a720-294">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="2a720-294">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="2a720-295">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="2a720-295">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="2a720-296">Atrybut</span><span class="sxs-lookup"><span data-stu-id="2a720-296">Attribute</span></span> | <span data-ttu-id="2a720-297">Opis</span><span class="sxs-lookup"><span data-stu-id="2a720-297">Description</span></span> | <span data-ttu-id="2a720-298">Domyślny</span><span class="sxs-lookup"><span data-stu-id="2a720-298">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="2a720-299">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="2a720-299">Optional string attribute.</span></span></p><p><span data-ttu-id="2a720-300">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2a720-300">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="2a720-301">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="2a720-301">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2a720-302">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="2a720-302">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="2a720-303">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="2a720-303">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2a720-304">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="2a720-304">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="2a720-305">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="2a720-305">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="2a720-306">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="2a720-306">Optional integer attribute.</span></span></p><p><span data-ttu-id="2a720-307">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="2a720-307">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="2a720-308">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="2a720-308">Default: `1`</span></span><br><span data-ttu-id="2a720-309">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="2a720-309">Min: `1`</span></span><br><span data-ttu-id="2a720-310">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="2a720-310">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="2a720-311">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="2a720-311">Required string attribute.</span></span></p><p><span data-ttu-id="2a720-312">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a720-312">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="2a720-313">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="2a720-313">Relative paths are supported.</span></span> <span data-ttu-id="2a720-314">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="2a720-314">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="2a720-315">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="2a720-315">Optional integer attribute.</span></span></p><p><span data-ttu-id="2a720-316">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="2a720-316">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="2a720-317">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="2a720-317">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="2a720-318">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="2a720-318">Default: `10`</span></span><br><span data-ttu-id="2a720-319">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="2a720-319">Min: `0`</span></span><br><span data-ttu-id="2a720-320">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="2a720-320">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="2a720-321">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="2a720-321">Optional timespan attribute.</span></span></p><p><span data-ttu-id="2a720-322">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="2a720-322">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="2a720-323">W wersjach modułu ASP.NET Core, dostarczonej wraz z wydaniem programu ASP.NET Core 2.0 lub wcześniej `requestTimeout` musi być określona w pełnych minut, w przeciwnym razie jego wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="2a720-323">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="2a720-324">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="2a720-324">Default: `00:02:00`</span></span><br><span data-ttu-id="2a720-325">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="2a720-325">Min: `00:00:00`</span></span><br><span data-ttu-id="2a720-326">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="2a720-326">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="2a720-327">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="2a720-327">Optional integer attribute.</span></span></p><p><span data-ttu-id="2a720-328">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="2a720-328">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="2a720-329">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="2a720-329">Default: `10`</span></span><br><span data-ttu-id="2a720-330">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="2a720-330">Min: `0`</span></span><br><span data-ttu-id="2a720-331">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="2a720-331">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="2a720-332">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="2a720-332">Optional integer attribute.</span></span></p><p><span data-ttu-id="2a720-333">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="2a720-333">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="2a720-334">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="2a720-334">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="2a720-335">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="2a720-335">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="2a720-336">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="2a720-336">Default: `120`</span></span><br><span data-ttu-id="2a720-337">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="2a720-337">Min: `0`</span></span><br><span data-ttu-id="2a720-338">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="2a720-338">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="2a720-339">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="2a720-339">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2a720-340">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2a720-340">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="2a720-341">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="2a720-341">Optional string attribute.</span></span></p><p><span data-ttu-id="2a720-342">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="2a720-342">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="2a720-343">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="2a720-343">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="2a720-344">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="2a720-344">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="2a720-345">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="2a720-345">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2a720-346">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="2a720-346">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="2a720-347">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="2a720-347">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="2a720-348">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="2a720-348">Setting environment variables</span></span>

<span data-ttu-id="2a720-349">Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="2a720-349">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="2a720-350">Określ zmienną środowiskową `environmentVariable` element podrzędny elementu `environmentVariables` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="2a720-350">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="2a720-351">Zmienne środowiskowe ustawione w tej sekcji pierwszeństwo systemowe zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="2a720-351">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="2a720-352">W poniższym przykładzie ustawiono dwóch zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="2a720-352">The following example sets two environment variables.</span></span> <span data-ttu-id="2a720-353">`ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji `Development`.</span><span class="sxs-lookup"><span data-stu-id="2a720-353">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="2a720-354">Deweloper może tymczasowo ustawić tę wartość w *web.config* pliku, aby wymusić [stronie wyjątków deweloperów](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek.</span><span class="sxs-lookup"><span data-stu-id="2a720-354">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="2a720-355">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, gdzie deweloper ma napisany kod, który odczytuje wartość przy uruchamianiu w celu utworzenia ścieżki do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a720-355">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!WARNING]
> <span data-ttu-id="2a720-356">Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej, aby `Development` przejściowym i testowania serwerów, które nie są dostępne do niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="2a720-356">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="2a720-357">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="2a720-357">app_offline.htm</span></span>

<span data-ttu-id="2a720-358">Jeśli plik o nazwie *app_offline.htm* wykryte w katalogu głównym aplikacji, modułu ASP.NET Core próbuje łagodne zamykanie aplikacji i zatrzymanie przetwarzania przychodzących żądań.</span><span class="sxs-lookup"><span data-stu-id="2a720-358">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="2a720-359">Jeśli aplikacja jest nadal uruchomione po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, modułu ASP.NET Core to złe rozwiązanie uruchomionego procesu.</span><span class="sxs-lookup"><span data-stu-id="2a720-359">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="2a720-360">Gdy *app_offline.htm* występuje pliku modułu ASP.NET Core ma odpowiadać na żądania, wysyłając ponownie zawartość *app_offline.htm* pliku.</span><span class="sxs-lookup"><span data-stu-id="2a720-360">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="2a720-361">Gdy *app_offline.htm* plik jest usuwany, kolejne żądanie uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="2a720-361">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2a720-362">Korzystając z modelu hostingu poza procesem, aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="2a720-362">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="2a720-363">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a720-363">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="2a720-364">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="2a720-364">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2a720-365">Zarówno w trakcie, jak i spoza procesu hostingu generuje strony błędów niestandardowych, gdy mogą zakończyć się niepowodzeniem uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="2a720-365">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="2a720-366">Jeśli modułu ASP.NET Core nie uda się znaleźć albo w trakcie lub poza procesem programu obsługi żądania, *500.0 — Błąd ładowania obsługi w-procesu/Out-Of-Process* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="2a720-366">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="2a720-367">W trakcie obsługi w przypadku niepowodzenia modułu ASP.NET Core uruchomić aplikację, *500.30 - Start błąd* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="2a720-367">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="2a720-368">Dla hostingu poza procesem, jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="2a720-368">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="2a720-369">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 5xx usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="2a720-369">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="2a720-370">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="2a720-370">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="2a720-371">Jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="2a720-371">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="2a720-372">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 502 usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="2a720-372">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="2a720-373">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="2a720-373">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 strona kodowa stan niepowodzenia procesu](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="2a720-375">Tworzenia dziennika i Przekierowanie</span><span class="sxs-lookup"><span data-stu-id="2a720-375">Log creation and redirection</span></span>

<span data-ttu-id="2a720-376">Modułu ASP.NET Core przekierowuje dane wyjściowe stdout i stderr konsoli do dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu są ustawione.</span><span class="sxs-lookup"><span data-stu-id="2a720-376">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="2a720-377">Wszystkie foldery w `stdoutLogFile` ścieżka musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="2a720-377">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2a720-378">Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="2a720-378">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="2a720-379">Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="2a720-379">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="2a720-380">Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="2a720-380">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="2a720-381">Przy użyciu dziennika stdout jest zalecane tylko na potrzeby rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a720-381">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="2a720-382">Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a720-382">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="2a720-383">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="2a720-383">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="2a720-384">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="2a720-384">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="2a720-385">Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="2a720-385">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="2a720-386">Przez dodanie sygnatury czasowej, identyfikator procesu i rozszerzenie pliku składa się nazwa pliku dziennika (*.log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="2a720-386">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="2a720-387">Jeśli `stdoutLogFile` ścieżki kończy się *stdout*, dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzono 19:42:32 2/5/2018 o nazwie pliku *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="2a720-387">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2a720-388">Jeśli `stdoutLogEnabled` ma wartość FAŁSZ, błędów występujących podczas uruchamiania aplikacji są przechwytywane i wysyłanego do dziennika zdarzeń do 30 KB.</span><span class="sxs-lookup"><span data-stu-id="2a720-388">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="2a720-389">Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="2a720-389">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="2a720-390">Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie strumienia stdout dla aplikacji hostowanej w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2a720-390">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="2a720-391">Ścieżka lokalna lub ścieżkę do udziału sieciowego jest możliwa do logowania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="2a720-391">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="2a720-392">Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.</span><span class="sxs-lookup"><span data-stu-id="2a720-392">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="2a720-393">Rozszerzone dzienników diagnostycznych</span><span class="sxs-lookup"><span data-stu-id="2a720-393">Enhanced diagnostic logs</span></span>

<span data-ttu-id="2a720-394">Modułu ASP.NET Core udostępnia, są konfigurowane w celu zapewnienia dzienniki diagnostyki rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="2a720-394">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="2a720-395">Dodaj `<handlerSettings>` elementu `<aspNetCore>` element *web.config*. Ustawienie `debugLevel` do `TRACE` udostępnia pozwala uzyskać większą wierność informacje diagnostyczne:</span><span class="sxs-lookup"><span data-stu-id="2a720-395">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="2a720-396">Poziom debugowania (`debugLevel`) wartości mogą obejmować zarówno na poziomie, jak i lokalizację.</span><span class="sxs-lookup"><span data-stu-id="2a720-396">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="2a720-397">Poziomy (w kolejności od najmniejszej do najbardziej szczegółowy):</span><span class="sxs-lookup"><span data-stu-id="2a720-397">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="2a720-398">BŁĄD</span><span class="sxs-lookup"><span data-stu-id="2a720-398">ERROR</span></span>
* <span data-ttu-id="2a720-399">OSTRZEŻENIE</span><span class="sxs-lookup"><span data-stu-id="2a720-399">WARNING</span></span>
* <span data-ttu-id="2a720-400">INFORMACJE O</span><span class="sxs-lookup"><span data-stu-id="2a720-400">INFO</span></span>
* <span data-ttu-id="2a720-401">TRACE</span><span class="sxs-lookup"><span data-stu-id="2a720-401">TRACE</span></span>

<span data-ttu-id="2a720-402">Lokalizacje (wiele lokalizacji są dozwolone):</span><span class="sxs-lookup"><span data-stu-id="2a720-402">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="2a720-403">KONSOLA</span><span class="sxs-lookup"><span data-stu-id="2a720-403">CONSOLE</span></span>
* <span data-ttu-id="2a720-404">DZIENNIK ZDARZEŃ</span><span class="sxs-lookup"><span data-stu-id="2a720-404">EVENTLOG</span></span>
* <span data-ttu-id="2a720-405">PLIK</span><span class="sxs-lookup"><span data-stu-id="2a720-405">FILE</span></span>

<span data-ttu-id="2a720-406">Ustawienia obsługi można również podać za pośrednictwem zmienne środowiskowe:</span><span class="sxs-lookup"><span data-stu-id="2a720-406">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="2a720-407">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ścieżka do pliku dziennika debugowania.</span><span class="sxs-lookup"><span data-stu-id="2a720-407">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="2a720-408">(Domyślnie: *czy aspnetcore*)</span><span class="sxs-lookup"><span data-stu-id="2a720-408">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="2a720-409">`ASPNETCORE_MODULE_DEBUG` &ndash; Ustawienie poziomie debugowania.</span><span class="sxs-lookup"><span data-stu-id="2a720-409">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="2a720-410">Czy **nie** Pozostaw włączone we wdrożeniu na dłużej, niż jest to wymagane, aby rozwiązać problem rejestrowanie debugowania.</span><span class="sxs-lookup"><span data-stu-id="2a720-410">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="2a720-411">Rozmiar dziennika nie jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="2a720-411">The size of the log isn't limited.</span></span> <span data-ttu-id="2a720-412">Pozostawienie dziennik debugowania włączone może wyczerpać dostępne miejsce na dysku i awarii serwera lub usługi app service.</span><span class="sxs-lookup"><span data-stu-id="2a720-412">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="2a720-413">Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="2a720-413">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="2a720-414">Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token</span><span class="sxs-lookup"><span data-stu-id="2a720-414">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2a720-415">*Dotyczy tylko hostingu poza procesem.*</span><span class="sxs-lookup"><span data-stu-id="2a720-415">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="2a720-416">Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a720-416">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="2a720-417">Przy użyciu protokołu HTTP jest Optymalizacja wydajności, gdzie ruch między modułem i Kestrel odbywa się na adres sprzężenia zwrotnego zniżki w stosunku do interfejsu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="2a720-417">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="2a720-418">Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="2a720-418">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="2a720-419">Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="2a720-419">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="2a720-420">Parowania token jest utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="2a720-420">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="2a720-421">Token parowania jest również ustawiona w nagłówku (`MS-ASPNETCORE-TOKEN`) na każde żądanie serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="2a720-421">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="2a720-422">Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="2a720-422">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="2a720-423">Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="2a720-423">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="2a720-424">Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="2a720-424">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="2a720-425">Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="2a720-425">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="2a720-426">Moduł ASP.NET Core z programem IIS konfigurację udostępnioną</span><span class="sxs-lookup"><span data-stu-id="2a720-426">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="2a720-427">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami **systemu** konta.</span><span class="sxs-lookup"><span data-stu-id="2a720-427">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="2a720-428">Ponieważ lokalne konto systemowe nie ma uprawnienia do modyfikowania dla ścieżki udziału konfiguracji udostępnionej usług IIS, Instalator trafienia odmowa dostępu błąd podczas próby skonfigurowania ustawień modułu w *applicationHost.config* w udziale.</span><span class="sxs-lookup"><span data-stu-id="2a720-428">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="2a720-429">Korzystając z konfiguracji udostępnionej usług IIS, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="2a720-429">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="2a720-430">Wyłącz konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="2a720-430">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="2a720-431">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="2a720-431">Run the installer.</span></span>
1. <span data-ttu-id="2a720-432">Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.</span><span class="sxs-lookup"><span data-stu-id="2a720-432">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="2a720-433">Ponownie włączyć konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="2a720-433">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="2a720-434">Wersja modułu oraz Dzienniki Instalatora pakietu hostingu</span><span class="sxs-lookup"><span data-stu-id="2a720-434">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="2a720-435">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="2a720-435">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="2a720-436">W systemie hostingu, przejdź do *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="2a720-436">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="2a720-437">Znajdź *aspnetcore.dll* pliku.</span><span class="sxs-lookup"><span data-stu-id="2a720-437">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="2a720-438">Kliknij prawym przyciskiem myszy plik i wybierz **właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="2a720-438">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="2a720-439">Wybierz **szczegóły** kartę. **Wersja pliku** i **wersji produktu** reprezentują zainstalowanej wersji modułu.</span><span class="sxs-lookup"><span data-stu-id="2a720-439">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="2a720-440">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się w *C:\\użytkowników\\% UserName %\\AppData\\lokalnego\\Temp*. Plik *dd_DotNetCoreWinSvrHosting__\<sygnatura czasowa > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="2a720-440">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="2a720-441">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="2a720-441">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="2a720-442">Moduł</span><span class="sxs-lookup"><span data-stu-id="2a720-442">Module</span></span>

<span data-ttu-id="2a720-443">**Usługi IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="2a720-443">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="2a720-444">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2a720-444">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="2a720-445">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2a720-445">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2a720-446">%ProgramFiles%\IIS\Asp.NET core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2a720-446">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="2a720-447">% ProgramFiles (x86) %\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2a720-447">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="2a720-448">**Usługi IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="2a720-448">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="2a720-449">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2a720-449">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="2a720-450">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2a720-450">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2a720-451">%ProgramFiles%\iis Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2a720-451">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="2a720-452">% ProgramFiles (x86) %\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2a720-452">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="2a720-453">Schemat</span><span class="sxs-lookup"><span data-stu-id="2a720-453">Schema</span></span>

<span data-ttu-id="2a720-454">**IIS**</span><span class="sxs-lookup"><span data-stu-id="2a720-454">**IIS**</span></span>

   * <span data-ttu-id="2a720-455">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="2a720-455">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2a720-456">%Windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="2a720-456">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="2a720-457">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="2a720-457">**IIS Express**</span></span>

   * <span data-ttu-id="2a720-458">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="2a720-458">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2a720-459">%ProgramFiles%\iis Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="2a720-459">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="2a720-460">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="2a720-460">Configuration</span></span>

<span data-ttu-id="2a720-461">**IIS**</span><span class="sxs-lookup"><span data-stu-id="2a720-461">**IIS**</span></span>

   * <span data-ttu-id="2a720-462">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="2a720-462">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="2a720-463">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="2a720-463">**IIS Express**</span></span>

   * <span data-ttu-id="2a720-464">%ProgramFiles%\iis Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="2a720-464">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="2a720-465">Pliki można znaleźć, wyszukując *aspnetcore* w *applicationHost.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="2a720-465">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2a720-466">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2a720-466">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="2a720-467">Repozytorium GitHub modułów Core ASP.NET (źródło odwołania)</span><span class="sxs-lookup"><span data-stu-id="2a720-467">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>