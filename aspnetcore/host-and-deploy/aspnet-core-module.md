---
title: ASP.NET Core Module
author: guardrex
description: Dowiedz się, jak skonfigurować modułu ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 192e4bf8e970083cc05babcd7fb3cf52985e35bf
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396327"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="ecf9c-103">ASP.NET Core Module</span><span class="sxs-lookup"><span data-stu-id="ecf9c-103">ASP.NET Core Module</span></span>

<span data-ttu-id="ecf9c-104">Przez [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ecf9c-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ecf9c-105">Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS albo:</span><span class="sxs-lookup"><span data-stu-id="ecf9c-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="ecf9c-106">Hostowanie aplikacji ASP.NET Core wewnątrz proces roboczy usług IIS (`w3wp.exe`), co jest nazywane [modelu hostingu w trakcie](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="ecf9c-107">Przesyła żądania sieci web do uruchamiania aplikacji ASP.NET Core zaplecza [serwera Kestrel](xref:fundamentals/servers/kestrel), co jest nazywane [modelu hostingu poza procesem](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="ecf9c-108">Obsługiwane wersje Windows:</span><span class="sxs-lookup"><span data-stu-id="ecf9c-108">Supported Windows versions:</span></span>

* <span data-ttu-id="ecf9c-109">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="ecf9c-109">Windows 7 or later</span></span>
* <span data-ttu-id="ecf9c-110">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="ecf9c-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="ecf9c-111">W przypadku hostowania w procesie, moduł używa implementacji w procesie serwera dla usług IIS, nazywany serwerem HTTP usług IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="ecf9c-112">Gdy w hostingu poza procesem, moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="ecf9c-113">Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="ecf9c-114">Modelach hostingu</span><span class="sxs-lookup"><span data-stu-id="ecf9c-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="ecf9c-115">W trakcie modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="ecf9c-115">In-process hosting model</span></span>

<span data-ttu-id="ecf9c-116">Aby skonfigurować aplikację do obsługi w procesie, Dodaj `<AspNetCoreHostingModel>` właściwość do pliku projektu aplikacji o wartości `InProcess` (spoza procesu hostingu została ustawiona za pomocą `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="ecf9c-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="ecf9c-117">Model hostingu w trakcie nie jest obsługiwana dla aplikacji platformy ASP.NET Core, które obsługują program .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="ecf9c-118">Jeśli `<AspNetCoreHostingModel>` właściwość nie jest obecny w pliku, wartością domyślną jest `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="ecf9c-119">Następujące właściwości mają zastosowanie w przypadku hostowania w procesie:</span><span class="sxs-lookup"><span data-stu-id="ecf9c-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="ecf9c-120">Serwer HTTP usług IIS (`IISHttpServer`) jest używany zamiast [Kestrel](xref:fundamentals/servers/kestrel) serwera.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

* <span data-ttu-id="ecf9c-121">[Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-121">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="ecf9c-122">Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-122">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="ecf9c-123">Użycie jednej puli aplikacji na aplikację.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-123">Use one app pool per app.</span></span>

* <span data-ttu-id="ecf9c-124">Korzystając z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręczne wprowadzanie [plik app_offline.htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files), aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-124">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="ecf9c-125">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-125">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="ecf9c-126">Architektura (liczba bitów) zainstalowanego środowiska uruchomieniowego (x64 lub x86) i aplikacji musi być zgodna z architekturą puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-126">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="ecf9c-127">Ustawiając hosta aplikacji ręcznie za pomocą `WebHostBuilder` (nie używa [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) i nigdy nie uruchomienia aplikacji bezpośrednio na serwerze Kestrel (Self-Hosted), wywołanie `UseKestrel` przed wywołaniem `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-127">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="ecf9c-128">Jeśli kolejność została odwrócona, host nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-128">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="ecf9c-129">Rozłącza klienta są wykrywane.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-129">Client disconnects are detected.</span></span> <span data-ttu-id="ecf9c-130">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) odwołano token anulowania, gdy klient odłączy się.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="ecf9c-131"><xref:System.IO.Directory.GetCurrentDirectory*> Zwraca katalogu roboczego proces rozpoczęty przez usługi IIS, a nie w katalogu aplikacji (na przykład *C:\Windows\System32\inetsrv* dla *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-131"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="ecf9c-132">Przykładowy kod, który ustawia bieżący katalog aplikacji, zobacz [klasy CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="ecf9c-133">Wywołaj `SetCurrentDirectory` metody.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="ecf9c-134">Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> zapewniają katalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="ecf9c-135">Model hostingu poza procesem</span><span class="sxs-lookup"><span data-stu-id="ecf9c-135">Out-of-process hosting model</span></span>

<span data-ttu-id="ecf9c-136">Aby skonfigurować aplikację dla hostingu poza procesem, użyj jednej z poniższych metod w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="ecf9c-136">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="ecf9c-137">Nie określaj `<AspNetCoreHostingModel>` właściwości.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-137">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="ecf9c-138">Jeśli `<AspNetCoreHostingModel>` właściwość nie jest obecny w pliku, wartością domyślną jest `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-138">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="ecf9c-139">Ustaw wartość `<AspNetCoreHostingModel>` właściwości `OutOfProcess` (hostowanie wewnątrzprocesowe została ustawiona za pomocą `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="ecf9c-139">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="ecf9c-140">[Kestrel](xref:fundamentals/servers/kestrel) serwer jest używany zamiast serwera HTTP usług IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-140">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="ecf9c-141">Zmiany modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="ecf9c-141">Hosting model changes</span></span>

<span data-ttu-id="ecf9c-142">Jeśli `hostingModel` ustawienie to ulegnie zmianie w *web.config* pliku (wyjaśnione w [konfiguracji z pliku web.config](#configuration-with-webconfig) sekcji), moduł odtwarzania procesu roboczego programu IIS.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-142">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="ecf9c-143">Dla usług IIS Express moduł nie odtworzyć proces roboczy, ale zamiast tego wyzwala łagodne zamykanie bieżący proces usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-143">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="ecf9c-144">Kolejne żądanie aplikacji spowoduje utworzenie nowych procesów usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-144">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="ecf9c-145">Nazwa procesu</span><span class="sxs-lookup"><span data-stu-id="ecf9c-145">Process name</span></span>

<span data-ttu-id="ecf9c-146">`Process.GetCurrentProcess().ProcessName` Raporty `w3wp` / `iisexpress` (w procesie) lub `dotnet` (poza procesem).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-146">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ecf9c-147">Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS do przekazywania żądań sieci web z zapleczem platformy ASP.NET Core z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-147">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="ecf9c-148">Obsługiwane wersje Windows:</span><span class="sxs-lookup"><span data-stu-id="ecf9c-148">Supported Windows versions:</span></span>

* <span data-ttu-id="ecf9c-149">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="ecf9c-149">Windows 7 or later</span></span>
* <span data-ttu-id="ecf9c-150">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="ecf9c-150">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="ecf9c-151">Moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-151">The module only works with Kestrel.</span></span> <span data-ttu-id="ecf9c-152">Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-152">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="ecf9c-153">Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje także zarządzanie procesem.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-153">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="ecf9c-154">Moduł rozpoczyna się proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera i ponownie uruchamia aplikację w przypadku jej awarii.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-154">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="ecf9c-155">Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje ASP.NET 4.x, działających w trakcie w usługach IIS, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-155">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="ecf9c-156">Na poniższym diagramie przedstawiono relację między usług IIS, modułu ASP.NET Core i aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ecf9c-156">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core Module](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="ecf9c-158">Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-158">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="ecf9c-159">Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-159">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="ecf9c-160">Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80 i 443.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-160">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="ecf9c-161">Moduł Określa port, za pośrednictwem zmiennej środowiskowej przy uruchamianiu i oprogramowania pośredniczącego integracji usług IIS umożliwia skonfigurowanie serwera do nasłuchiwania `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-161">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="ecf9c-162">Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-162">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="ecf9c-163">Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-163">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="ecf9c-164">Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-164">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="ecf9c-165">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-165">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="ecf9c-166">Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-166">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="ecf9c-167">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-167">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="ecf9c-168">Wiele modułów macierzystych, takie jak uwierzytelnianie Windows pozostają aktywne.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-168">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="ecf9c-169">Aby dowiedzieć się, jak aktywne moduły usług IIS przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-169">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="ecf9c-170">Modułu ASP.NET Core może również:</span><span class="sxs-lookup"><span data-stu-id="ecf9c-170">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="ecf9c-171">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-171">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="ecf9c-172">Rejestrowanie strumienia stdout, dane wyjściowe do usługi file storage dotyczące rozwiązywania problemów, uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-172">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="ecf9c-173">Przekazuj tokeny uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-173">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="ecf9c-174">Jak zainstalować i używać modułu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ecf9c-174">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="ecf9c-175">Aby uzyskać instrukcje na temat sposobu instalowania i używania modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-175">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="ecf9c-176">Konfiguracja z pliku web.config</span><span class="sxs-lookup"><span data-stu-id="ecf9c-176">Configuration with web.config</span></span>

<span data-ttu-id="ecf9c-177">Skonfigurowano modułu ASP.NET Core `aspNetCore` części `system.webServer` węzeł w tej witrynie *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-177">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="ecf9c-178">Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależny od struktury](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje modułu ASP.NET Core do obsługi żądań w lokacji:</span><span class="sxs-lookup"><span data-stu-id="ecf9c-178">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="ecf9c-179">Następujące *web.config* została opublikowana na potrzeby [niezależna wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="ecf9c-179">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="ecf9c-180"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Właściwość jest ustawiona na `false` do wskazania, że ustawienia określone w [ \<lokalizacja >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elementu nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-180">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="ecf9c-181">Gdy aplikacja jest wdrażana na [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest równa `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-181">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="ecf9c-182">Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-182">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="ecf9c-183">Aby uzyskać informacji na temat konfigurowania aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-183">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="ecf9c-184">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="ecf9c-184">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="ecf9c-185">Atrybut</span><span class="sxs-lookup"><span data-stu-id="ecf9c-185">Attribute</span></span> | <span data-ttu-id="ecf9c-186">Opis</span><span class="sxs-lookup"><span data-stu-id="ecf9c-186">Description</span></span> | <span data-ttu-id="ecf9c-187">Domyślny</span><span class="sxs-lookup"><span data-stu-id="ecf9c-187">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="ecf9c-188">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-188">Optional string attribute.</span></span></p><p><span data-ttu-id="ecf9c-189">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-189">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="ecf9c-190">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="ecf9c-191">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-191">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="ecf9c-192">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-192">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="ecf9c-193">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-193">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="ecf9c-194">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-194">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="ecf9c-195">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-195">Optional string attribute.</span></span></p><p><span data-ttu-id="ecf9c-196">Określa modelu hostingu w trakcie (`InProcess`) lub spoza procesu (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-196">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="ecf9c-197">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-197">Optional integer attribute.</span></span></p><p><span data-ttu-id="ecf9c-198">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-198">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="ecf9c-199">&dagger;W trakcie hostingu, wartość jest ograniczona do `1`.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-199">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="ecf9c-200">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-200">Default: `1`</span></span><br><span data-ttu-id="ecf9c-201">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-201">Min: `1`</span></span><br><span data-ttu-id="ecf9c-202">Maks.: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="ecf9c-202">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="ecf9c-203">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-203">Required string attribute.</span></span></p><p><span data-ttu-id="ecf9c-204">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-204">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="ecf9c-205">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-205">Relative paths are supported.</span></span> <span data-ttu-id="ecf9c-206">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-206">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="ecf9c-207">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="ecf9c-208">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-208">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="ecf9c-209">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-209">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="ecf9c-210">Nieobsługiwane za pomocą wewnątrzprocesowego hostingu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-210">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="ecf9c-211">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-211">Default: `10`</span></span><br><span data-ttu-id="ecf9c-212">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-212">Min: `0`</span></span><br><span data-ttu-id="ecf9c-213">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-213">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="ecf9c-214">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-214">Optional timespan attribute.</span></span></p><p><span data-ttu-id="ecf9c-215">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-215">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="ecf9c-216">W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-216">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="ecf9c-217">Nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-217">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="ecf9c-218">Dla hostingu w procesie modułu czeka na aplikację, aby przetworzyć żądanie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-218">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="ecf9c-219">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-219">Default: `00:02:00`</span></span><br><span data-ttu-id="ecf9c-220">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-220">Min: `00:00:00`</span></span><br><span data-ttu-id="ecf9c-221">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-221">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="ecf9c-222">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="ecf9c-223">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-223">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="ecf9c-224">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-224">Default: `10`</span></span><br><span data-ttu-id="ecf9c-225">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-225">Min: `0`</span></span><br><span data-ttu-id="ecf9c-226">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-226">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="ecf9c-227">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-227">Optional integer attribute.</span></span></p><p><span data-ttu-id="ecf9c-228">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-228">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="ecf9c-229">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-229">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="ecf9c-230">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-230">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="ecf9c-231">Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-231">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="ecf9c-232">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-232">Default: `120`</span></span><br><span data-ttu-id="ecf9c-233">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-233">Min: `0`</span></span><br><span data-ttu-id="ecf9c-234">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-234">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="ecf9c-235">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-235">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="ecf9c-236">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-236">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="ecf9c-237">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-237">Optional string attribute.</span></span></p><p><span data-ttu-id="ecf9c-238">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-238">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="ecf9c-239">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-239">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="ecf9c-240">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-240">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="ecf9c-241">Wszystkie foldery w ścieżce są tworzone przez moduł, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-241">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="ecf9c-242">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-242">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="ecf9c-243">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-243">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="ecf9c-244">Atrybut</span><span class="sxs-lookup"><span data-stu-id="ecf9c-244">Attribute</span></span> | <span data-ttu-id="ecf9c-245">Opis</span><span class="sxs-lookup"><span data-stu-id="ecf9c-245">Description</span></span> | <span data-ttu-id="ecf9c-246">Domyślny</span><span class="sxs-lookup"><span data-stu-id="ecf9c-246">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="ecf9c-247">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-247">Optional string attribute.</span></span></p><p><span data-ttu-id="ecf9c-248">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-248">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="ecf9c-249">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-249">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="ecf9c-250">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-250">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="ecf9c-251">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-251">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="ecf9c-252">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-252">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="ecf9c-253">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-253">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="ecf9c-254">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-254">Optional integer attribute.</span></span></p><p><span data-ttu-id="ecf9c-255">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-255">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="ecf9c-256">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-256">Default: `1`</span></span><br><span data-ttu-id="ecf9c-257">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-257">Min: `1`</span></span><br><span data-ttu-id="ecf9c-258">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-258">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="ecf9c-259">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-259">Required string attribute.</span></span></p><p><span data-ttu-id="ecf9c-260">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-260">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="ecf9c-261">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-261">Relative paths are supported.</span></span> <span data-ttu-id="ecf9c-262">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-262">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="ecf9c-263">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-263">Optional integer attribute.</span></span></p><p><span data-ttu-id="ecf9c-264">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-264">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="ecf9c-265">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-265">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="ecf9c-266">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-266">Default: `10`</span></span><br><span data-ttu-id="ecf9c-267">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-267">Min: `0`</span></span><br><span data-ttu-id="ecf9c-268">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-268">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="ecf9c-269">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-269">Optional timespan attribute.</span></span></p><p><span data-ttu-id="ecf9c-270">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-270">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="ecf9c-271">W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-271">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="ecf9c-272">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-272">Default: `00:02:00`</span></span><br><span data-ttu-id="ecf9c-273">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-273">Min: `00:00:00`</span></span><br><span data-ttu-id="ecf9c-274">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-274">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="ecf9c-275">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-275">Optional integer attribute.</span></span></p><p><span data-ttu-id="ecf9c-276">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-276">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="ecf9c-277">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-277">Default: `10`</span></span><br><span data-ttu-id="ecf9c-278">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-278">Min: `0`</span></span><br><span data-ttu-id="ecf9c-279">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-279">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="ecf9c-280">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-280">Optional integer attribute.</span></span></p><p><span data-ttu-id="ecf9c-281">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-281">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="ecf9c-282">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-282">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="ecf9c-283">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-283">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="ecf9c-284">Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-284">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="ecf9c-285">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-285">Default: `120`</span></span><br><span data-ttu-id="ecf9c-286">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-286">Min: `0`</span></span><br><span data-ttu-id="ecf9c-287">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-287">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="ecf9c-288">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-288">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="ecf9c-289">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-289">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="ecf9c-290">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-290">Optional string attribute.</span></span></p><p><span data-ttu-id="ecf9c-291">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-291">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="ecf9c-292">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-292">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="ecf9c-293">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-293">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="ecf9c-294">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-294">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="ecf9c-295">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-295">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="ecf9c-296">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-296">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="ecf9c-297">Atrybut</span><span class="sxs-lookup"><span data-stu-id="ecf9c-297">Attribute</span></span> | <span data-ttu-id="ecf9c-298">Opis</span><span class="sxs-lookup"><span data-stu-id="ecf9c-298">Description</span></span> | <span data-ttu-id="ecf9c-299">Domyślny</span><span class="sxs-lookup"><span data-stu-id="ecf9c-299">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="ecf9c-300">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-300">Optional string attribute.</span></span></p><p><span data-ttu-id="ecf9c-301">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-301">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="ecf9c-302">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-302">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="ecf9c-303">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-303">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="ecf9c-304">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-304">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="ecf9c-305">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-305">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="ecf9c-306">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-306">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="ecf9c-307">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-307">Optional integer attribute.</span></span></p><p><span data-ttu-id="ecf9c-308">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-308">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="ecf9c-309">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-309">Default: `1`</span></span><br><span data-ttu-id="ecf9c-310">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-310">Min: `1`</span></span><br><span data-ttu-id="ecf9c-311">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-311">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="ecf9c-312">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-312">Required string attribute.</span></span></p><p><span data-ttu-id="ecf9c-313">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-313">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="ecf9c-314">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-314">Relative paths are supported.</span></span> <span data-ttu-id="ecf9c-315">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-315">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="ecf9c-316">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-316">Optional integer attribute.</span></span></p><p><span data-ttu-id="ecf9c-317">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-317">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="ecf9c-318">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-318">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="ecf9c-319">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-319">Default: `10`</span></span><br><span data-ttu-id="ecf9c-320">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-320">Min: `0`</span></span><br><span data-ttu-id="ecf9c-321">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-321">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="ecf9c-322">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-322">Optional timespan attribute.</span></span></p><p><span data-ttu-id="ecf9c-323">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-323">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="ecf9c-324">W wersjach modułu ASP.NET Core, dostarczonej wraz z wydaniem programu ASP.NET Core 2.0 lub wcześniej `requestTimeout` musi być określona w pełnych minut, w przeciwnym razie jego wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-324">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="ecf9c-325">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-325">Default: `00:02:00`</span></span><br><span data-ttu-id="ecf9c-326">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-326">Min: `00:00:00`</span></span><br><span data-ttu-id="ecf9c-327">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-327">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="ecf9c-328">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-328">Optional integer attribute.</span></span></p><p><span data-ttu-id="ecf9c-329">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-329">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="ecf9c-330">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-330">Default: `10`</span></span><br><span data-ttu-id="ecf9c-331">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-331">Min: `0`</span></span><br><span data-ttu-id="ecf9c-332">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-332">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="ecf9c-333">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-333">Optional integer attribute.</span></span></p><p><span data-ttu-id="ecf9c-334">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-334">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="ecf9c-335">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-335">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="ecf9c-336">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-336">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="ecf9c-337">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-337">Default: `120`</span></span><br><span data-ttu-id="ecf9c-338">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-338">Min: `0`</span></span><br><span data-ttu-id="ecf9c-339">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="ecf9c-339">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="ecf9c-340">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-340">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="ecf9c-341">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-341">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="ecf9c-342">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-342">Optional string attribute.</span></span></p><p><span data-ttu-id="ecf9c-343">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-343">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="ecf9c-344">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-344">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="ecf9c-345">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-345">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="ecf9c-346">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-346">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="ecf9c-347">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-347">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="ecf9c-348">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-348">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="ecf9c-349">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="ecf9c-349">Setting environment variables</span></span>

<span data-ttu-id="ecf9c-350">Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-350">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="ecf9c-351">Określ zmienną środowiskową `environmentVariable` element podrzędny elementu `environmentVariables` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-351">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="ecf9c-352">Zmienne środowiskowe ustawione w tej sekcji pierwszeństwo systemowe zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-352">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="ecf9c-353">W poniższym przykładzie ustawiono dwóch zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-353">The following example sets two environment variables.</span></span> <span data-ttu-id="ecf9c-354">`ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji `Development`.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-354">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="ecf9c-355">Deweloper może tymczasowo ustawić tę wartość w *web.config* pliku, aby wymusić [stronie wyjątków deweloperów](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-355">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="ecf9c-356">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, gdzie deweloper ma napisany kod, który odczytuje wartość przy uruchamianiu w celu utworzenia ścieżki do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-356">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="ecf9c-357">Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej, aby `Development` przejściowym i testowania serwerów, które nie są dostępne do niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-357">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="ecf9c-358">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="ecf9c-358">app_offline.htm</span></span>

<span data-ttu-id="ecf9c-359">Jeśli plik o nazwie *app_offline.htm* wykryte w katalogu głównym aplikacji, modułu ASP.NET Core próbuje łagodne zamykanie aplikacji i zatrzymanie przetwarzania przychodzących żądań.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-359">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="ecf9c-360">Jeśli aplikacja jest nadal uruchomione po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, modułu ASP.NET Core to złe rozwiązanie uruchomionego procesu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-360">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="ecf9c-361">Gdy *app_offline.htm* występuje pliku modułu ASP.NET Core ma odpowiadać na żądania, wysyłając ponownie zawartość *app_offline.htm* pliku.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-361">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="ecf9c-362">Gdy *app_offline.htm* plik jest usuwany, kolejne żądanie uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-362">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ecf9c-363">Korzystając z modelu hostingu poza procesem, aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-363">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="ecf9c-364">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-364">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="ecf9c-365">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="ecf9c-365">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ecf9c-366">Zarówno w trakcie, jak i spoza procesu hostingu generuje strony błędów niestandardowych, gdy mogą zakończyć się niepowodzeniem uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-366">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="ecf9c-367">Jeśli modułu ASP.NET Core nie uda się znaleźć albo w trakcie lub poza procesem programu obsługi żądania, *500.0 — Błąd ładowania obsługi w-procesu/Out-Of-Process* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-367">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="ecf9c-368">W trakcie obsługi w przypadku niepowodzenia modułu ASP.NET Core uruchomić aplikację, *500.30 - Start błąd* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-368">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="ecf9c-369">Dla hostingu poza procesem, jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-369">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="ecf9c-370">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 5xx usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-370">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="ecf9c-371">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-371">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ecf9c-372">Jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-372">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="ecf9c-373">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 502 usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-373">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="ecf9c-374">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-374">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 strona kodowa stan niepowodzenia procesu](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="ecf9c-376">Tworzenia dziennika i Przekierowanie</span><span class="sxs-lookup"><span data-stu-id="ecf9c-376">Log creation and redirection</span></span>

<span data-ttu-id="ecf9c-377">Modułu ASP.NET Core przekierowuje dane wyjściowe stdout i stderr konsoli do dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu są ustawione.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-377">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="ecf9c-378">Wszystkie foldery w `stdoutLogFile` ścieżki są tworzone przez moduł, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-378">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="ecf9c-379">Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-379">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="ecf9c-380">Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-380">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="ecf9c-381">Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-381">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="ecf9c-382">Przy użyciu dziennika stdout jest zalecane tylko na potrzeby rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-382">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="ecf9c-383">Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-383">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="ecf9c-384">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-384">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="ecf9c-385">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="ecf9c-385">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="ecf9c-386">Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-386">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="ecf9c-387">Przez dodanie sygnatury czasowej, identyfikator procesu i rozszerzenie pliku składa się nazwa pliku dziennika (*.log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-387">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="ecf9c-388">Jeśli `stdoutLogFile` ścieżki kończy się *stdout*, dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzono 19:42:32 2/5/2018 o nazwie pliku *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-388">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ecf9c-389">Jeśli `stdoutLogEnabled` ma wartość FAŁSZ, błędów występujących podczas uruchamiania aplikacji są przechwytywane i wysyłanego do dziennika zdarzeń do 30 KB.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-389">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="ecf9c-390">Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-390">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="ecf9c-391">Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie strumienia stdout dla aplikacji hostowanej w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-391">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="ecf9c-392">Ścieżka lokalna lub ścieżkę do udziału sieciowego jest możliwa do logowania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-392">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="ecf9c-393">Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-393">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="ecf9c-394">Rozszerzone dzienników diagnostycznych</span><span class="sxs-lookup"><span data-stu-id="ecf9c-394">Enhanced diagnostic logs</span></span>

<span data-ttu-id="ecf9c-395">Modułu ASP.NET Core udostępnia, są konfigurowane w celu zapewnienia dzienniki diagnostyki rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-395">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="ecf9c-396">Dodaj `<handlerSettings>` elementu `<aspNetCore>` element *web.config*. Ustawienie `debugLevel` do `TRACE` udostępnia pozwala uzyskać większą wierność informacje diagnostyczne:</span><span class="sxs-lookup"><span data-stu-id="ecf9c-396">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="ecf9c-397">Poziom debugowania (`debugLevel`) wartości mogą obejmować zarówno na poziomie, jak i lokalizację.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-397">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="ecf9c-398">Poziomy (w kolejności od najmniejszej do najbardziej szczegółowy):</span><span class="sxs-lookup"><span data-stu-id="ecf9c-398">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="ecf9c-399">BŁĄD</span><span class="sxs-lookup"><span data-stu-id="ecf9c-399">ERROR</span></span>
* <span data-ttu-id="ecf9c-400">OSTRZEŻENIE</span><span class="sxs-lookup"><span data-stu-id="ecf9c-400">WARNING</span></span>
* <span data-ttu-id="ecf9c-401">INFORMACJE O</span><span class="sxs-lookup"><span data-stu-id="ecf9c-401">INFO</span></span>
* <span data-ttu-id="ecf9c-402">TRACE</span><span class="sxs-lookup"><span data-stu-id="ecf9c-402">TRACE</span></span>

<span data-ttu-id="ecf9c-403">Lokalizacje (wiele lokalizacji są dozwolone):</span><span class="sxs-lookup"><span data-stu-id="ecf9c-403">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="ecf9c-404">KONSOLA</span><span class="sxs-lookup"><span data-stu-id="ecf9c-404">CONSOLE</span></span>
* <span data-ttu-id="ecf9c-405">DZIENNIK ZDARZEŃ</span><span class="sxs-lookup"><span data-stu-id="ecf9c-405">EVENTLOG</span></span>
* <span data-ttu-id="ecf9c-406">PLIK</span><span class="sxs-lookup"><span data-stu-id="ecf9c-406">FILE</span></span>

<span data-ttu-id="ecf9c-407">Ustawienia obsługi można również podać za pośrednictwem zmienne środowiskowe:</span><span class="sxs-lookup"><span data-stu-id="ecf9c-407">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="ecf9c-408">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ścieżka do pliku dziennika debugowania.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-408">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="ecf9c-409">(Domyślnie: *czy aspnetcore*)</span><span class="sxs-lookup"><span data-stu-id="ecf9c-409">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="ecf9c-410">`ASPNETCORE_MODULE_DEBUG` &ndash; Ustawienie poziomie debugowania.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-410">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="ecf9c-411">Czy **nie** Pozostaw włączone we wdrożeniu na dłużej, niż jest to wymagane, aby rozwiązać problem rejestrowanie debugowania.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-411">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="ecf9c-412">Rozmiar dziennika nie jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-412">The size of the log isn't limited.</span></span> <span data-ttu-id="ecf9c-413">Pozostawienie dziennik debugowania włączone może wyczerpać dostępne miejsce na dysku i awarii serwera lub usługi app service.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-413">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="ecf9c-414">Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-414">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="ecf9c-415">Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token</span><span class="sxs-lookup"><span data-stu-id="ecf9c-415">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ecf9c-416">*Dotyczy tylko hostingu poza procesem.*</span><span class="sxs-lookup"><span data-stu-id="ecf9c-416">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="ecf9c-417">Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-417">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="ecf9c-418">Przy użyciu protokołu HTTP jest Optymalizacja wydajności, gdzie ruch między modułem i Kestrel odbywa się na adres sprzężenia zwrotnego zniżki w stosunku do interfejsu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-418">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="ecf9c-419">Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-419">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="ecf9c-420">Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-420">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="ecf9c-421">Parowania token jest utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-421">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="ecf9c-422">Token parowania jest również ustawiona w nagłówku (`MS-ASPNETCORE-TOKEN`) na każde żądanie serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-422">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="ecf9c-423">Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-423">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="ecf9c-424">Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-424">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="ecf9c-425">Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-425">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="ecf9c-426">Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-426">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="ecf9c-427">Moduł ASP.NET Core z programem IIS konfigurację udostępnioną</span><span class="sxs-lookup"><span data-stu-id="ecf9c-427">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="ecf9c-428">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami **systemu** konta.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-428">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="ecf9c-429">Ponieważ lokalne konto systemowe nie ma uprawnienia do modyfikowania dla ścieżki udziału konfiguracji udostępnionej usług IIS, Instalator trafienia odmowa dostępu błąd podczas próby skonfigurowania ustawień modułu w *applicationHost.config* w udziale.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-429">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="ecf9c-430">Korzystając z konfiguracji udostępnionej usług IIS, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="ecf9c-430">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="ecf9c-431">Wyłącz konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-431">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="ecf9c-432">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-432">Run the installer.</span></span>
1. <span data-ttu-id="ecf9c-433">Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-433">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="ecf9c-434">Ponownie włączyć konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-434">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="ecf9c-435">Wersja modułu oraz Dzienniki Instalatora pakietu hostingu</span><span class="sxs-lookup"><span data-stu-id="ecf9c-435">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="ecf9c-436">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="ecf9c-436">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="ecf9c-437">W systemie hostingu, przejdź do *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-437">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="ecf9c-438">Znajdź *aspnetcore.dll* pliku.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-438">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="ecf9c-439">Kliknij prawym przyciskiem myszy plik i wybierz **właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-439">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="ecf9c-440">Wybierz **szczegóły** kartę. **Wersja pliku** i **wersji produktu** reprezentują zainstalowanej wersji modułu.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-440">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="ecf9c-441">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się w *C:\\użytkowników\\% UserName %\\AppData\\lokalnego\\Temp*. Plik *dd_DotNetCoreWinSvrHosting__\<sygnatura czasowa > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-441">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="ecf9c-442">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="ecf9c-442">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="ecf9c-443">Moduł</span><span class="sxs-lookup"><span data-stu-id="ecf9c-443">Module</span></span>

<span data-ttu-id="ecf9c-444">**Usługi IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="ecf9c-444">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="ecf9c-445">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="ecf9c-445">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="ecf9c-446">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="ecf9c-446">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="ecf9c-447">%ProgramFiles%\IIS\Asp.NET core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="ecf9c-447">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="ecf9c-448">% ProgramFiles (x86) %\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="ecf9c-448">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="ecf9c-449">**Usługi IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="ecf9c-449">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="ecf9c-450">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="ecf9c-450">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="ecf9c-451">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="ecf9c-451">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="ecf9c-452">%ProgramFiles%\iis Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="ecf9c-452">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="ecf9c-453">% ProgramFiles (x86) %\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="ecf9c-453">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="ecf9c-454">Schemat</span><span class="sxs-lookup"><span data-stu-id="ecf9c-454">Schema</span></span>

<span data-ttu-id="ecf9c-455">**IIS**</span><span class="sxs-lookup"><span data-stu-id="ecf9c-455">**IIS**</span></span>

   * <span data-ttu-id="ecf9c-456">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="ecf9c-456">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="ecf9c-457">%Windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="ecf9c-457">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="ecf9c-458">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="ecf9c-458">**IIS Express**</span></span>

   * <span data-ttu-id="ecf9c-459">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="ecf9c-459">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="ecf9c-460">%ProgramFiles%\iis Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="ecf9c-460">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="ecf9c-461">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="ecf9c-461">Configuration</span></span>

<span data-ttu-id="ecf9c-462">**IIS**</span><span class="sxs-lookup"><span data-stu-id="ecf9c-462">**IIS**</span></span>

   * <span data-ttu-id="ecf9c-463">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="ecf9c-463">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="ecf9c-464">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="ecf9c-464">**IIS Express**</span></span>

   * <span data-ttu-id="ecf9c-465">Visual Studio: {Katalog główny aplikacji}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="ecf9c-465">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>
   
   * <span data-ttu-id="ecf9c-466">*iisexpress.exe* interfejsu wiersza polecenia: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="ecf9c-466">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="ecf9c-467">Pliki można znaleźć, wyszukując *aspnetcore* w *applicationHost.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="ecf9c-467">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ecf9c-468">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ecf9c-468">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="ecf9c-469">Repozytorium GitHub modułów Core ASP.NET (źródło odwołania)</span><span class="sxs-lookup"><span data-stu-id="ecf9c-469">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
