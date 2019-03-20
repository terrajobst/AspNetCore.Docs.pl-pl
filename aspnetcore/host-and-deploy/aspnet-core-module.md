---
title: Moduł ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować modułu ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: ead3732d2c272f0868e823726ffd415a6c74f444
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265459"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="09c47-103">Moduł ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09c47-103">ASP.NET Core Module</span></span>

<span data-ttu-id="09c47-104">Przez [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="09c47-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="09c47-105">Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS albo:</span><span class="sxs-lookup"><span data-stu-id="09c47-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="09c47-106">Hostowanie aplikacji ASP.NET Core wewnątrz proces roboczy usług IIS (`w3wp.exe`), co jest nazywane [modelu hostingu w trakcie](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="09c47-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="09c47-107">Przesyła żądania sieci web do uruchamiania aplikacji ASP.NET Core zaplecza [serwera Kestrel](xref:fundamentals/servers/kestrel), co jest nazywane [modelu hostingu poza procesem](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="09c47-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="09c47-108">Obsługiwane wersje Windows:</span><span class="sxs-lookup"><span data-stu-id="09c47-108">Supported Windows versions:</span></span>

* <span data-ttu-id="09c47-109">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="09c47-109">Windows 7 or later</span></span>
* <span data-ttu-id="09c47-110">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="09c47-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="09c47-111">W przypadku hostowania w procesie, moduł używa implementacji w procesie serwera dla usług IIS, nazywany serwerem HTTP usług IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="09c47-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="09c47-112">Gdy w hostingu poza procesem, moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="09c47-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="09c47-113">Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="09c47-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="09c47-114">Modele hostingu</span><span class="sxs-lookup"><span data-stu-id="09c47-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="09c47-115">W trakcie modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="09c47-115">In-process hosting model</span></span>

<span data-ttu-id="09c47-116">Aby skonfigurować aplikację do obsługi w procesie, Dodaj `<AspNetCoreHostingModel>` właściwość do pliku projektu aplikacji o wartości `InProcess` (spoza procesu hostingu została ustawiona za pomocą `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="09c47-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="09c47-117">Model hostingu w trakcie nie jest obsługiwana dla aplikacji platformy ASP.NET Core, które obsługują program .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="09c47-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="09c47-118">Jeśli `<AspNetCoreHostingModel>` właściwość nie jest obecny w pliku, wartością domyślną jest `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="09c47-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="09c47-119">Następujące właściwości mają zastosowanie w przypadku hostowania w procesie:</span><span class="sxs-lookup"><span data-stu-id="09c47-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="09c47-120">Serwer HTTP usług IIS (`IISHttpServer`) jest używany zamiast [Kestrel](xref:fundamentals/servers/kestrel) serwera.</span><span class="sxs-lookup"><span data-stu-id="09c47-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="09c47-121">W procesie [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) wywołania <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> do:</span><span class="sxs-lookup"><span data-stu-id="09c47-121">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="09c47-122">Zarejestruj `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="09c47-122">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="09c47-123">Skonfiguruj port i ścieżka podstawowa serwera powinien nasłuchiwać podczas uruchamiania za modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09c47-123">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="09c47-124">Konfigurowanie hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="09c47-124">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="09c47-125">[Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="09c47-125">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="09c47-126">Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="09c47-126">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="09c47-127">Użycie jednej puli aplikacji na aplikację.</span><span class="sxs-lookup"><span data-stu-id="09c47-127">Use one app pool per app.</span></span>

* <span data-ttu-id="09c47-128">Korzystając z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręczne wprowadzanie [plik app_offline.htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files), aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="09c47-128">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="09c47-129">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09c47-129">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="09c47-130">Architektura (liczba bitów) zainstalowanego środowiska uruchomieniowego (x64 lub x86) i aplikacji musi być zgodna z architekturą puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09c47-130">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="09c47-131">Ustawiając hosta aplikacji ręcznie za pomocą `WebHostBuilder` (nie używa [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) i nigdy nie uruchomienia aplikacji bezpośrednio na serwerze Kestrel (Self-Hosted), wywołanie `UseKestrel` przed wywołaniem `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="09c47-131">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="09c47-132">Jeśli kolejność została odwrócona, host nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="09c47-132">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="09c47-133">Rozłącza klienta są wykrywane.</span><span class="sxs-lookup"><span data-stu-id="09c47-133">Client disconnects are detected.</span></span> <span data-ttu-id="09c47-134">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) odwołano token anulowania, gdy klient odłączy się.</span><span class="sxs-lookup"><span data-stu-id="09c47-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="09c47-135">W programie ASP.NET Core 2.2.1 lub wcześniej, <xref:System.IO.Directory.GetCurrentDirectory*> zwraca katalogu roboczego proces rozpoczęty przez usługi IIS, a nie w katalogu aplikacji (na przykład *C:\Windows\System32\inetsrv* dla *w3wp.exe*) .</span><span class="sxs-lookup"><span data-stu-id="09c47-135">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="09c47-136">Przykładowy kod, który ustawia bieżący katalog aplikacji, zobacz [klasy CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="09c47-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="09c47-137">Wywołaj `SetCurrentDirectory` metody.</span><span class="sxs-lookup"><span data-stu-id="09c47-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="09c47-138">Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> zapewniają katalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09c47-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="09c47-139">W przypadku hostowania w trakcie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wewnętrznie wywoływana w celu zainicjowania przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="09c47-139">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="09c47-140">W związku z tym <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacji używanego do przekształcania oświadczeń, po każdym uwierzytelniania nie jest aktywowana domyślnie.</span><span class="sxs-lookup"><span data-stu-id="09c47-140">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="09c47-141">Podczas transformowania oświadczenia, za pomocą <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacji, wywołanie <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> nad dodaniem usług uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="09c47-141">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }

  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="09c47-142">Model hostingu poza procesem</span><span class="sxs-lookup"><span data-stu-id="09c47-142">Out-of-process hosting model</span></span>

<span data-ttu-id="09c47-143">Aby skonfigurować aplikację dla hostingu poza procesem, użyj jednej z poniższych metod w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="09c47-143">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="09c47-144">Nie określaj `<AspNetCoreHostingModel>` właściwości.</span><span class="sxs-lookup"><span data-stu-id="09c47-144">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="09c47-145">Jeśli `<AspNetCoreHostingModel>` właściwość nie jest obecny w pliku, wartością domyślną jest `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="09c47-145">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="09c47-146">Ustaw wartość `<AspNetCoreHostingModel>` właściwości `OutOfProcess` (hostowanie wewnątrzprocesowe została ustawiona za pomocą `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="09c47-146">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="09c47-147">[Kestrel](xref:fundamentals/servers/kestrel) serwer jest używany zamiast serwera HTTP usług IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="09c47-147">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="09c47-148">Dla procesu,- [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) wywołania <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> do:</span><span class="sxs-lookup"><span data-stu-id="09c47-148">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="09c47-149">Skonfiguruj port i ścieżka podstawowa serwera powinien nasłuchiwać podczas uruchamiania za modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09c47-149">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="09c47-150">Konfigurowanie hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="09c47-150">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="09c47-151">Zmiany modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="09c47-151">Hosting model changes</span></span>

<span data-ttu-id="09c47-152">Jeśli `hostingModel` ustawienie to ulegnie zmianie w *web.config* pliku (wyjaśnione w [konfiguracji z pliku web.config](#configuration-with-webconfig) sekcji), moduł odtwarzania procesu roboczego programu IIS.</span><span class="sxs-lookup"><span data-stu-id="09c47-152">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="09c47-153">Dla usług IIS Express moduł nie odtworzyć proces roboczy, ale zamiast tego wyzwala łagodne zamykanie bieżący proces usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="09c47-153">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="09c47-154">Kolejne żądanie aplikacji spowoduje utworzenie nowych procesów usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="09c47-154">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="09c47-155">Nazwa procesu</span><span class="sxs-lookup"><span data-stu-id="09c47-155">Process name</span></span>

<span data-ttu-id="09c47-156">`Process.GetCurrentProcess().ProcessName` Raporty `w3wp` / `iisexpress` (w procesie) lub `dotnet` (poza procesem).</span><span class="sxs-lookup"><span data-stu-id="09c47-156">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="09c47-157">Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS do przekazywania żądań sieci web z zapleczem platformy ASP.NET Core z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09c47-157">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="09c47-158">Obsługiwane wersje Windows:</span><span class="sxs-lookup"><span data-stu-id="09c47-158">Supported Windows versions:</span></span>

* <span data-ttu-id="09c47-159">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="09c47-159">Windows 7 or later</span></span>
* <span data-ttu-id="09c47-160">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="09c47-160">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="09c47-161">Moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="09c47-161">The module only works with Kestrel.</span></span> <span data-ttu-id="09c47-162">Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="09c47-162">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="09c47-163">Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje także zarządzanie procesem.</span><span class="sxs-lookup"><span data-stu-id="09c47-163">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="09c47-164">Moduł rozpoczyna się proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera i ponownie uruchamia aplikację w przypadku jej awarii.</span><span class="sxs-lookup"><span data-stu-id="09c47-164">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="09c47-165">Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje ASP.NET 4.x, działających w trakcie w usługach IIS, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="09c47-165">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="09c47-166">Na poniższym diagramie przedstawiono relację między usług IIS, modułu ASP.NET Core i aplikacji:</span><span class="sxs-lookup"><span data-stu-id="09c47-166">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="09c47-168">Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="09c47-168">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="09c47-169">Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="09c47-169">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="09c47-170">Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80 i 443.</span><span class="sxs-lookup"><span data-stu-id="09c47-170">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="09c47-171">Moduł Określa port, za pośrednictwem zmiennej środowiskowej przy uruchamianiu i oprogramowania pośredniczącego integracji usług IIS umożliwia skonfigurowanie serwera do nasłuchiwania `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="09c47-171">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="09c47-172">Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="09c47-172">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="09c47-173">Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="09c47-173">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="09c47-174">Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09c47-174">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="09c47-175">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="09c47-175">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="09c47-176">Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="09c47-176">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="09c47-177">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="09c47-177">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="09c47-178">Wiele modułów macierzystych, takie jak uwierzytelnianie Windows pozostają aktywne.</span><span class="sxs-lookup"><span data-stu-id="09c47-178">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="09c47-179">Aby dowiedzieć się, jak aktywne moduły usług IIS przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="09c47-179">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="09c47-180">Modułu ASP.NET Core może również:</span><span class="sxs-lookup"><span data-stu-id="09c47-180">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="09c47-181">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="09c47-181">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="09c47-182">Rejestrowanie strumienia stdout, dane wyjściowe do usługi file storage dotyczące rozwiązywania problemów, uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="09c47-182">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="09c47-183">Przekazuj tokeny uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="09c47-183">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="09c47-184">Jak zainstalować i używać modułu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09c47-184">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="09c47-185">Aby uzyskać instrukcje na temat sposobu instalowania i używania modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="09c47-185">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="09c47-186">Konfiguracja z pliku web.config</span><span class="sxs-lookup"><span data-stu-id="09c47-186">Configuration with web.config</span></span>

<span data-ttu-id="09c47-187">Skonfigurowano modułu ASP.NET Core `aspNetCore` części `system.webServer` węzeł w tej witrynie *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="09c47-187">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="09c47-188">Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależny od struktury](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje modułu ASP.NET Core do obsługi żądań w lokacji:</span><span class="sxs-lookup"><span data-stu-id="09c47-188">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="09c47-189">Następujące *web.config* została opublikowana na potrzeby [niezależna wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="09c47-189">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="09c47-190"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Właściwość jest ustawiona na `false` do wskazania, że ustawienia określone w [ \<lokalizacja >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elementu nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09c47-190">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="09c47-191">Gdy aplikacja jest wdrażana na [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest równa `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="09c47-191">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="09c47-192">Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.</span><span class="sxs-lookup"><span data-stu-id="09c47-192">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="09c47-193">Aby uzyskać informacji na temat konfigurowania aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="09c47-193">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="09c47-194">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="09c47-194">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="09c47-195">Atrybut</span><span class="sxs-lookup"><span data-stu-id="09c47-195">Attribute</span></span> | <span data-ttu-id="09c47-196">Opis</span><span class="sxs-lookup"><span data-stu-id="09c47-196">Description</span></span> | <span data-ttu-id="09c47-197">Domyślny</span><span class="sxs-lookup"><span data-stu-id="09c47-197">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="09c47-198">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="09c47-198">Optional string attribute.</span></span></p><p><span data-ttu-id="09c47-199">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="09c47-199">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="09c47-200">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="09c47-200">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09c47-201">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="09c47-201">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="09c47-202">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="09c47-202">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09c47-203">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="09c47-203">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="09c47-204">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="09c47-204">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="09c47-205">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="09c47-205">Optional string attribute.</span></span></p><p><span data-ttu-id="09c47-206">Określa modelu hostingu w trakcie (`InProcess`) lub spoza procesu (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="09c47-206">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="09c47-207">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="09c47-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="09c47-208">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="09c47-208">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="09c47-209">&dagger;W trakcie hostingu, wartość jest ograniczona do `1`.</span><span class="sxs-lookup"><span data-stu-id="09c47-209">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="09c47-210">Ustawienie `processesPerApplication` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="09c47-210">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="09c47-211">Ten atrybut zostanie usunięta w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="09c47-211">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="09c47-212">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="09c47-212">Default: `1`</span></span><br><span data-ttu-id="09c47-213">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="09c47-213">Min: `1`</span></span><br><span data-ttu-id="09c47-214">Maks.: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="09c47-214">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="09c47-215">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="09c47-215">Required string attribute.</span></span></p><p><span data-ttu-id="09c47-216">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="09c47-216">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="09c47-217">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="09c47-217">Relative paths are supported.</span></span> <span data-ttu-id="09c47-218">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="09c47-218">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="09c47-219">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="09c47-219">Optional integer attribute.</span></span></p><p><span data-ttu-id="09c47-220">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="09c47-220">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="09c47-221">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="09c47-221">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="09c47-222">Nieobsługiwane za pomocą wewnątrzprocesowego hostingu.</span><span class="sxs-lookup"><span data-stu-id="09c47-222">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="09c47-223">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="09c47-223">Default: `10`</span></span><br><span data-ttu-id="09c47-224">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="09c47-224">Min: `0`</span></span><br><span data-ttu-id="09c47-225">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="09c47-225">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="09c47-226">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="09c47-226">Optional timespan attribute.</span></span></p><p><span data-ttu-id="09c47-227">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="09c47-227">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="09c47-228">W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="09c47-228">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="09c47-229">Nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="09c47-229">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="09c47-230">Dla hostingu w procesie modułu czeka na aplikację, aby przetworzyć żądanie.</span><span class="sxs-lookup"><span data-stu-id="09c47-230">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="09c47-231">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="09c47-231">Default: `00:02:00`</span></span><br><span data-ttu-id="09c47-232">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="09c47-232">Min: `00:00:00`</span></span><br><span data-ttu-id="09c47-233">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="09c47-233">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="09c47-234">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="09c47-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="09c47-235">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="09c47-235">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="09c47-236">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="09c47-236">Default: `10`</span></span><br><span data-ttu-id="09c47-237">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="09c47-237">Min: `0`</span></span><br><span data-ttu-id="09c47-238">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="09c47-238">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="09c47-239">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="09c47-239">Optional integer attribute.</span></span></p><p><span data-ttu-id="09c47-240">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="09c47-240">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="09c47-241">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="09c47-241">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="09c47-242">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="09c47-242">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="09c47-243">Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="09c47-243">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="09c47-244">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="09c47-244">Default: `120`</span></span><br><span data-ttu-id="09c47-245">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="09c47-245">Min: `0`</span></span><br><span data-ttu-id="09c47-246">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="09c47-246">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="09c47-247">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="09c47-247">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09c47-248">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="09c47-248">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="09c47-249">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="09c47-249">Optional string attribute.</span></span></p><p><span data-ttu-id="09c47-250">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="09c47-250">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="09c47-251">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="09c47-251">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="09c47-252">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="09c47-252">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="09c47-253">Wszystkie foldery w ścieżce są tworzone przez moduł, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="09c47-253">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="09c47-254">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="09c47-254">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="09c47-255">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="09c47-255">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="09c47-256">Atrybut</span><span class="sxs-lookup"><span data-stu-id="09c47-256">Attribute</span></span> | <span data-ttu-id="09c47-257">Opis</span><span class="sxs-lookup"><span data-stu-id="09c47-257">Description</span></span> | <span data-ttu-id="09c47-258">Domyślny</span><span class="sxs-lookup"><span data-stu-id="09c47-258">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="09c47-259">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="09c47-259">Optional string attribute.</span></span></p><p><span data-ttu-id="09c47-260">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="09c47-260">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="09c47-261">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="09c47-261">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09c47-262">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="09c47-262">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="09c47-263">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="09c47-263">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09c47-264">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="09c47-264">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="09c47-265">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="09c47-265">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="09c47-266">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="09c47-266">Optional integer attribute.</span></span></p><p><span data-ttu-id="09c47-267">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="09c47-267">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="09c47-268">Ustawienie `processesPerApplication` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="09c47-268">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="09c47-269">Ten atrybut zostanie usunięta w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="09c47-269">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="09c47-270">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="09c47-270">Default: `1`</span></span><br><span data-ttu-id="09c47-271">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="09c47-271">Min: `1`</span></span><br><span data-ttu-id="09c47-272">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="09c47-272">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="09c47-273">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="09c47-273">Required string attribute.</span></span></p><p><span data-ttu-id="09c47-274">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="09c47-274">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="09c47-275">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="09c47-275">Relative paths are supported.</span></span> <span data-ttu-id="09c47-276">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="09c47-276">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="09c47-277">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="09c47-277">Optional integer attribute.</span></span></p><p><span data-ttu-id="09c47-278">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="09c47-278">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="09c47-279">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="09c47-279">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="09c47-280">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="09c47-280">Default: `10`</span></span><br><span data-ttu-id="09c47-281">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="09c47-281">Min: `0`</span></span><br><span data-ttu-id="09c47-282">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="09c47-282">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="09c47-283">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="09c47-283">Optional timespan attribute.</span></span></p><p><span data-ttu-id="09c47-284">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="09c47-284">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="09c47-285">W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="09c47-285">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="09c47-286">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="09c47-286">Default: `00:02:00`</span></span><br><span data-ttu-id="09c47-287">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="09c47-287">Min: `00:00:00`</span></span><br><span data-ttu-id="09c47-288">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="09c47-288">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="09c47-289">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="09c47-289">Optional integer attribute.</span></span></p><p><span data-ttu-id="09c47-290">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="09c47-290">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="09c47-291">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="09c47-291">Default: `10`</span></span><br><span data-ttu-id="09c47-292">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="09c47-292">Min: `0`</span></span><br><span data-ttu-id="09c47-293">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="09c47-293">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="09c47-294">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="09c47-294">Optional integer attribute.</span></span></p><p><span data-ttu-id="09c47-295">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="09c47-295">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="09c47-296">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="09c47-296">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="09c47-297">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="09c47-297">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="09c47-298">Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="09c47-298">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="09c47-299">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="09c47-299">Default: `120`</span></span><br><span data-ttu-id="09c47-300">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="09c47-300">Min: `0`</span></span><br><span data-ttu-id="09c47-301">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="09c47-301">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="09c47-302">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="09c47-302">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09c47-303">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="09c47-303">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="09c47-304">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="09c47-304">Optional string attribute.</span></span></p><p><span data-ttu-id="09c47-305">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="09c47-305">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="09c47-306">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="09c47-306">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="09c47-307">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="09c47-307">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="09c47-308">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="09c47-308">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="09c47-309">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="09c47-309">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="09c47-310">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="09c47-310">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="09c47-311">Atrybut</span><span class="sxs-lookup"><span data-stu-id="09c47-311">Attribute</span></span> | <span data-ttu-id="09c47-312">Opis</span><span class="sxs-lookup"><span data-stu-id="09c47-312">Description</span></span> | <span data-ttu-id="09c47-313">Domyślny</span><span class="sxs-lookup"><span data-stu-id="09c47-313">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="09c47-314">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="09c47-314">Optional string attribute.</span></span></p><p><span data-ttu-id="09c47-315">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="09c47-315">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="09c47-316">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="09c47-316">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09c47-317">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="09c47-317">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="09c47-318">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="09c47-318">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09c47-319">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="09c47-319">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="09c47-320">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="09c47-320">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="09c47-321">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="09c47-321">Optional integer attribute.</span></span></p><p><span data-ttu-id="09c47-322">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="09c47-322">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="09c47-323">Ustawienie `processesPerApplication` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="09c47-323">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="09c47-324">Ten atrybut zostanie usunięta w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="09c47-324">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="09c47-325">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="09c47-325">Default: `1`</span></span><br><span data-ttu-id="09c47-326">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="09c47-326">Min: `1`</span></span><br><span data-ttu-id="09c47-327">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="09c47-327">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="09c47-328">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="09c47-328">Required string attribute.</span></span></p><p><span data-ttu-id="09c47-329">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="09c47-329">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="09c47-330">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="09c47-330">Relative paths are supported.</span></span> <span data-ttu-id="09c47-331">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="09c47-331">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="09c47-332">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="09c47-332">Optional integer attribute.</span></span></p><p><span data-ttu-id="09c47-333">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="09c47-333">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="09c47-334">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="09c47-334">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="09c47-335">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="09c47-335">Default: `10`</span></span><br><span data-ttu-id="09c47-336">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="09c47-336">Min: `0`</span></span><br><span data-ttu-id="09c47-337">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="09c47-337">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="09c47-338">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="09c47-338">Optional timespan attribute.</span></span></p><p><span data-ttu-id="09c47-339">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="09c47-339">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="09c47-340">W wersjach modułu ASP.NET Core, dostarczonej wraz z wydaniem programu ASP.NET Core 2.0 lub wcześniej `requestTimeout` musi być określona w pełnych minut, w przeciwnym razie jego wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="09c47-340">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="09c47-341">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="09c47-341">Default: `00:02:00`</span></span><br><span data-ttu-id="09c47-342">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="09c47-342">Min: `00:00:00`</span></span><br><span data-ttu-id="09c47-343">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="09c47-343">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="09c47-344">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="09c47-344">Optional integer attribute.</span></span></p><p><span data-ttu-id="09c47-345">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="09c47-345">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="09c47-346">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="09c47-346">Default: `10`</span></span><br><span data-ttu-id="09c47-347">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="09c47-347">Min: `0`</span></span><br><span data-ttu-id="09c47-348">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="09c47-348">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="09c47-349">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="09c47-349">Optional integer attribute.</span></span></p><p><span data-ttu-id="09c47-350">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="09c47-350">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="09c47-351">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="09c47-351">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="09c47-352">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="09c47-352">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="09c47-353">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="09c47-353">Default: `120`</span></span><br><span data-ttu-id="09c47-354">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="09c47-354">Min: `0`</span></span><br><span data-ttu-id="09c47-355">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="09c47-355">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="09c47-356">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="09c47-356">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09c47-357">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="09c47-357">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="09c47-358">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="09c47-358">Optional string attribute.</span></span></p><p><span data-ttu-id="09c47-359">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="09c47-359">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="09c47-360">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="09c47-360">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="09c47-361">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="09c47-361">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="09c47-362">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="09c47-362">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="09c47-363">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="09c47-363">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="09c47-364">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="09c47-364">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="09c47-365">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="09c47-365">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="09c47-366">Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="09c47-366">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="09c47-367">Określ zmienną środowiskową `<environmentVariable>` element podrzędny elementu `<environmentVariables>` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="09c47-367">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="09c47-368">Zmienne środowiskowe ustawione w tej sekcji pierwszeństwo systemowe zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="09c47-368">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="09c47-369">Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="09c47-369">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="09c47-370">Określ zmienną środowiskową `<environmentVariable>` element podrzędny elementu `<environmentVariables>` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="09c47-370">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="09c47-371">Ustaw zmienne środowiskowe ustawione w tej sekcji wystąpienie konfliktu, za pomocą zmiennych środowiskowych systemu o takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="09c47-371">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="09c47-372">Jeśli zmienna środowiskowa jest ustawiana w obu *web.config* plik, a w systemie poziomu w Windows, wartość *web.config* plik staje się dołączany do wartości zmiennej środowiskowej systemu (na potrzeby przykład `ASPNETCORE_ENVIRONMENT: Development;Development`), który uniemożliwia uruchomienie przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="09c47-372">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="09c47-373">W poniższym przykładzie ustawiono dwóch zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="09c47-373">The following example sets two environment variables.</span></span> <span data-ttu-id="09c47-374">`ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji `Development`.</span><span class="sxs-lookup"><span data-stu-id="09c47-374">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="09c47-375">Deweloper może tymczasowo ustawić tę wartość w *web.config* pliku, aby wymusić [stronie wyjątków deweloperów](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek.</span><span class="sxs-lookup"><span data-stu-id="09c47-375">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="09c47-376">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, gdzie deweloper ma napisany kod, który odczytuje wartość przy uruchamianiu w celu utworzenia ścieżki do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09c47-376">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="09c47-377">Zamiast bezpośrednio w konfiguracji środowiska *web.config* ma zawierać `<EnvironmentName>` właściwości w profilu publikowania (*.pubxml*) lub pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="09c47-377">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="09c47-378">To podejście ustawia środowisko *web.config* po opublikowaniu projektu:</span><span class="sxs-lookup"><span data-stu-id="09c47-378">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="09c47-379">Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej, aby `Development` przejściowym i testowania serwerów, które nie są dostępne do niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="09c47-379">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="09c47-380">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="09c47-380">app_offline.htm</span></span>

<span data-ttu-id="09c47-381">Jeśli plik o nazwie *app_offline.htm* wykryte w katalogu głównym aplikacji, modułu ASP.NET Core próbuje łagodne zamykanie aplikacji i zatrzymanie przetwarzania przychodzących żądań.</span><span class="sxs-lookup"><span data-stu-id="09c47-381">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="09c47-382">Jeśli aplikacja jest nadal uruchomione po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, modułu ASP.NET Core to złe rozwiązanie uruchomionego procesu.</span><span class="sxs-lookup"><span data-stu-id="09c47-382">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="09c47-383">Gdy *app_offline.htm* występuje pliku modułu ASP.NET Core ma odpowiadać na żądania, wysyłając ponownie zawartość *app_offline.htm* pliku.</span><span class="sxs-lookup"><span data-stu-id="09c47-383">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="09c47-384">Gdy *app_offline.htm* plik jest usuwany, kolejne żądanie uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="09c47-384">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="09c47-385">Korzystając z modelu hostingu poza procesem, aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="09c47-385">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="09c47-386">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09c47-386">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="09c47-387">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="09c47-387">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="09c47-388">Zarówno w trakcie, jak i spoza procesu hostingu generuje strony błędów niestandardowych, gdy mogą zakończyć się niepowodzeniem uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="09c47-388">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="09c47-389">Jeśli modułu ASP.NET Core nie uda się znaleźć albo w trakcie lub poza procesem programu obsługi żądania, *500.0 — Błąd ładowania obsługi w-procesu/Out-Of-Process* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="09c47-389">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="09c47-390">W trakcie obsługi w przypadku niepowodzenia modułu ASP.NET Core uruchomić aplikację, *500.30 - Start błąd* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="09c47-390">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="09c47-391">Dla hostingu poza procesem, jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="09c47-391">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="09c47-392">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 5xx usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="09c47-392">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="09c47-393">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="09c47-393">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="09c47-394">Jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="09c47-394">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="09c47-395">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 502 usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="09c47-395">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="09c47-396">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="09c47-396">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 strona kodowa stan niepowodzenia procesu](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="09c47-398">Tworzenia dziennika i Przekierowanie</span><span class="sxs-lookup"><span data-stu-id="09c47-398">Log creation and redirection</span></span>

<span data-ttu-id="09c47-399">Modułu ASP.NET Core przekierowuje dane wyjściowe stdout i stderr konsoli do dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu są ustawione.</span><span class="sxs-lookup"><span data-stu-id="09c47-399">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="09c47-400">Wszystkie foldery w `stdoutLogFile` ścieżki są tworzone przez moduł, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="09c47-400">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="09c47-401">Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="09c47-401">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="09c47-402">Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="09c47-402">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="09c47-403">Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="09c47-403">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="09c47-404">Przy użyciu dziennika stdout jest zalecane tylko na potrzeby rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09c47-404">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="09c47-405">Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09c47-405">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="09c47-406">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="09c47-406">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="09c47-407">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="09c47-407">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="09c47-408">Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="09c47-408">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="09c47-409">Przez dodanie sygnatury czasowej, identyfikator procesu i rozszerzenie pliku składa się nazwa pliku dziennika (*.log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="09c47-409">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="09c47-410">Jeśli `stdoutLogFile` ścieżki kończy się *stdout*, dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzono 19:42:32 2/5/2018 o nazwie pliku *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="09c47-410">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="09c47-411">Jeśli `stdoutLogEnabled` ma wartość FAŁSZ, błędów występujących podczas uruchamiania aplikacji są przechwytywane i wysyłanego do dziennika zdarzeń do 30 KB.</span><span class="sxs-lookup"><span data-stu-id="09c47-411">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="09c47-412">Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="09c47-412">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="09c47-413">Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie strumienia stdout dla aplikacji hostowanej w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="09c47-413">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="09c47-414">Ścieżka lokalna lub ścieżkę do udziału sieciowego jest możliwa do logowania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="09c47-414">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="09c47-415">Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.</span><span class="sxs-lookup"><span data-stu-id="09c47-415">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="09c47-416">Rozszerzone dzienników diagnostycznych</span><span class="sxs-lookup"><span data-stu-id="09c47-416">Enhanced diagnostic logs</span></span>

<span data-ttu-id="09c47-417">Modułu ASP.NET Core jest konfigurowane w celu zapewnienia dzienniki diagnostyki rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="09c47-417">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="09c47-418">Dodaj `<handlerSettings>` elementu `<aspNetCore>` element *web.config*. Ustawienie `debugLevel` do `TRACE` udostępnia pozwala uzyskać większą wierność informacje diagnostyczne:</span><span class="sxs-lookup"><span data-stu-id="09c47-418">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="09c47-419">Poziom debugowania (`debugLevel`) wartości mogą obejmować zarówno na poziomie, jak i lokalizację.</span><span class="sxs-lookup"><span data-stu-id="09c47-419">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="09c47-420">Poziomy (w kolejności od najmniejszej do najbardziej szczegółowy):</span><span class="sxs-lookup"><span data-stu-id="09c47-420">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="09c47-421">BŁĄD</span><span class="sxs-lookup"><span data-stu-id="09c47-421">ERROR</span></span>
* <span data-ttu-id="09c47-422">OSTRZEŻENIE</span><span class="sxs-lookup"><span data-stu-id="09c47-422">WARNING</span></span>
* <span data-ttu-id="09c47-423">INFORMACJE O</span><span class="sxs-lookup"><span data-stu-id="09c47-423">INFO</span></span>
* <span data-ttu-id="09c47-424">TRACE</span><span class="sxs-lookup"><span data-stu-id="09c47-424">TRACE</span></span>

<span data-ttu-id="09c47-425">Lokalizacje (wiele lokalizacji są dozwolone):</span><span class="sxs-lookup"><span data-stu-id="09c47-425">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="09c47-426">KONSOLA</span><span class="sxs-lookup"><span data-stu-id="09c47-426">CONSOLE</span></span>
* <span data-ttu-id="09c47-427">DZIENNIK ZDARZEŃ</span><span class="sxs-lookup"><span data-stu-id="09c47-427">EVENTLOG</span></span>
* <span data-ttu-id="09c47-428">PLIK</span><span class="sxs-lookup"><span data-stu-id="09c47-428">FILE</span></span>

<span data-ttu-id="09c47-429">Ustawienia obsługi można również podać za pośrednictwem zmienne środowiskowe:</span><span class="sxs-lookup"><span data-stu-id="09c47-429">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="09c47-430">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ścieżka do pliku dziennika debugowania.</span><span class="sxs-lookup"><span data-stu-id="09c47-430">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="09c47-431">(Domyślnie: *czy aspnetcore*)</span><span class="sxs-lookup"><span data-stu-id="09c47-431">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="09c47-432">`ASPNETCORE_MODULE_DEBUG` &ndash; Ustawienie poziomie debugowania.</span><span class="sxs-lookup"><span data-stu-id="09c47-432">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="09c47-433">Czy **nie** Pozostaw włączone we wdrożeniu na dłużej, niż jest to wymagane, aby rozwiązać problem rejestrowanie debugowania.</span><span class="sxs-lookup"><span data-stu-id="09c47-433">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="09c47-434">Rozmiar dziennika nie jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="09c47-434">The size of the log isn't limited.</span></span> <span data-ttu-id="09c47-435">Pozostawienie dziennik debugowania włączone może wyczerpać dostępne miejsce na dysku i awarii serwera lub usługi app service.</span><span class="sxs-lookup"><span data-stu-id="09c47-435">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="09c47-436">Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="09c47-436">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="09c47-437">Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token</span><span class="sxs-lookup"><span data-stu-id="09c47-437">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="09c47-438">*Dotyczy tylko hostingu poza procesem.*</span><span class="sxs-lookup"><span data-stu-id="09c47-438">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="09c47-439">Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="09c47-439">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="09c47-440">Przy użyciu protokołu HTTP jest Optymalizacja wydajności, gdzie ruch między modułem i Kestrel odbywa się na adres sprzężenia zwrotnego zniżki w stosunku do interfejsu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="09c47-440">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="09c47-441">Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="09c47-441">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="09c47-442">Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="09c47-442">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="09c47-443">Parowania token jest utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="09c47-443">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="09c47-444">Token parowania jest również ustawiona w nagłówku (`MS-ASPNETCORE-TOKEN`) na każde żądanie serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="09c47-444">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="09c47-445">Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="09c47-445">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="09c47-446">Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="09c47-446">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="09c47-447">Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="09c47-447">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="09c47-448">Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="09c47-448">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="09c47-449">Moduł ASP.NET Core z programem IIS konfigurację udostępnioną</span><span class="sxs-lookup"><span data-stu-id="09c47-449">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="09c47-450">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami **zaufany Instalator** konta.</span><span class="sxs-lookup"><span data-stu-id="09c47-450">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="09c47-451">Ponieważ lokalne konto systemowe nie ma uprawnienia do modyfikowania dla ścieżki udziału konfiguracji udostępnionej usług IIS, Instalator zgłasza odmowa dostępu błąd podczas próby skonfigurowania ustawień modułu w *applicationHost.config*  pliku w udziale.</span><span class="sxs-lookup"><span data-stu-id="09c47-451">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="09c47-452">Podczas korzystania z konfiguracji udostępnionej usług IIS, w tym samym komputerze co instalacji usług IIS, uruchom Instalatora programu ASP.NET Core hostingu pakietu z `OPT_NO_SHARED_CONFIG_CHECK` parametr `1`:</span><span class="sxs-lookup"><span data-stu-id="09c47-452">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="09c47-453">Gdy na tym samym komputerze co instalacji usług IIS nie jest ścieżką do konfiguracji udostępnionej, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="09c47-453">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="09c47-454">Wyłącz konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="09c47-454">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="09c47-455">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="09c47-455">Run the installer.</span></span>
1. <span data-ttu-id="09c47-456">Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.</span><span class="sxs-lookup"><span data-stu-id="09c47-456">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="09c47-457">Ponownie włączyć konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="09c47-457">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="09c47-458">Korzystając z konfiguracji udostępnionej usług IIS, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="09c47-458">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="09c47-459">Wyłącz konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="09c47-459">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="09c47-460">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="09c47-460">Run the installer.</span></span>
1. <span data-ttu-id="09c47-461">Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.</span><span class="sxs-lookup"><span data-stu-id="09c47-461">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="09c47-462">Ponownie włączyć konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="09c47-462">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization"></a><span data-ttu-id="09c47-463">Inicjowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="09c47-463">Application Initialization</span></span>

<span data-ttu-id="09c47-464">[Inicjowanie aplikacji usług IIS](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) to funkcja usług IIS, która wysyła żądania HTTP do aplikacji, gdy pula aplikacji rozpoczyna się lub zostanie odtworzona.</span><span class="sxs-lookup"><span data-stu-id="09c47-464">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="09c47-465">Żądanie wyzwala uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09c47-465">The request triggers the app to start.</span></span> <span data-ttu-id="09c47-466">Inicjowanie aplikacji mogą być używane przez oba [modelu hostingu w trakcie](xref:fundamentals/servers/index#in-process-hosting-model) i [modelu hostingu poza procesem](xref:fundamentals/servers/index#out-of-process-hosting-model) przy użyciu modułu ASP.NET Core w wersji 2.</span><span class="sxs-lookup"><span data-stu-id="09c47-466">Application Initialization can be used by both the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model) and [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) with the ASP.NET Core Module version 2.</span></span>

<span data-ttu-id="09c47-467">Aby włączyć inicjowania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="09c47-467">To enable Application Initialization:</span></span>

1. <span data-ttu-id="09c47-468">Upewnij się, że włączona funkcja roli Inicjowanie aplikacji usług IIS w:</span><span class="sxs-lookup"><span data-stu-id="09c47-468">Confirm that the IIS Application Initialization role feature in enabled:</span></span>
   * <span data-ttu-id="09c47-469">Windows 7 lub nowszy: Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Windows Włącz funkcje w lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="09c47-469">On Windows 7 or later: Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="09c47-470">Otwórz **Internetowe usługi informacyjne** > **usługi World Wide Web** > **funkcje tworzenia aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="09c47-470">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="09c47-471">Zaznacz pole wyboru dla **Inicjowanie aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="09c47-471">Select the check box for **Application Initialization**.</span></span>
   * <span data-ttu-id="09c47-472">W systemie Windows Server 2008 R2 lub nowszym, otwórz **Kreatora dodawania ról i funkcji**.</span><span class="sxs-lookup"><span data-stu-id="09c47-472">On Windows Server 2008 R2 or later, open the **Add Roles and Features Wizard**.</span></span> <span data-ttu-id="09c47-473">Po przejściu **Wybieranie usług ról** panelu Otwórz **opracowywanie aplikacji** a następnie wybierz węzeł **Inicjowanie aplikacji** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="09c47-473">When you reach the **Select role services** panel, open the **Application Development** node and select the **Application Initialization** check box.</span></span>
1. <span data-ttu-id="09c47-474">W Menedżerze usług IIS wybierz **pul aplikacji** w **połączeń** panelu.</span><span class="sxs-lookup"><span data-stu-id="09c47-474">In IIS Manager, select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="09c47-475">Wybierz pulę aplikacji przez aplikację na liście.</span><span class="sxs-lookup"><span data-stu-id="09c47-475">Select the app's app pool in the list.</span></span>
1. <span data-ttu-id="09c47-476">Wybierz **Zaawansowane ustawienia** w obszarze **edytowanie puli aplikacji** w **akcje** panelu.</span><span class="sxs-lookup"><span data-stu-id="09c47-476">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="09c47-477">Ustaw **tryb uruchamiania** do **AlwaysRunning**.</span><span class="sxs-lookup"><span data-stu-id="09c47-477">Set **Start Mode** to **AlwaysRunning**.</span></span>
1. <span data-ttu-id="09c47-478">Otwórz **witryn** w węźle **połączeń** panelu.</span><span class="sxs-lookup"><span data-stu-id="09c47-478">Open the **Sites** node in the **Connections** panel.</span></span>
1. <span data-ttu-id="09c47-479">Wybierz aplikację.</span><span class="sxs-lookup"><span data-stu-id="09c47-479">Select the app.</span></span>
1. <span data-ttu-id="09c47-480">Wybierz **Zaawansowane ustawienia** w obszarze **Zarządzaj witryną internetową** w **akcje** panelu.</span><span class="sxs-lookup"><span data-stu-id="09c47-480">Select **Advanced Settings** under **Manage Website** in the **Actions** panel.</span></span>
1. <span data-ttu-id="09c47-481">Ustaw **wstępne załadowanie włączone** do **True**.</span><span class="sxs-lookup"><span data-stu-id="09c47-481">Set **Preload Enabled** to **True**.</span></span>

<span data-ttu-id="09c47-482">Aby uzyskać więcej informacji, zobacz [Inicjowanie aplikacji programu IIS 8.0](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span><span class="sxs-lookup"><span data-stu-id="09c47-482">For more information, see [IIS 8.0 Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span></span>

<span data-ttu-id="09c47-483">Aplikacje korzystające z [modelu hostingu poza procesem](xref:fundamentals/servers/index#out-of-process-hosting-model) musi być okresowo wysyłać polecenie ping w aplikacji w celu zapewnienia jego działania usługi zewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="09c47-483">Apps that use the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) must use an external service to periodically ping the app in order to keep it running.</span></span>

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="09c47-484">Wersja modułu oraz Dzienniki Instalatora pakietu hostingu</span><span class="sxs-lookup"><span data-stu-id="09c47-484">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="09c47-485">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="09c47-485">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="09c47-486">W systemie hostingu, przejdź do *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="09c47-486">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="09c47-487">Znajdź *aspnetcore.dll* pliku.</span><span class="sxs-lookup"><span data-stu-id="09c47-487">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="09c47-488">Kliknij prawym przyciskiem myszy plik i wybierz **właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="09c47-488">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="09c47-489">Wybierz **szczegóły** kartę. **Wersja pliku** i **wersji produktu** reprezentują zainstalowanej wersji modułu.</span><span class="sxs-lookup"><span data-stu-id="09c47-489">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="09c47-490">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się w *C:\\użytkowników\\% UserName %\\AppData\\lokalnego\\Temp*. Plik *dd_DotNetCoreWinSvrHosting__\<sygnatura czasowa > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="09c47-490">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="09c47-491">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="09c47-491">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="09c47-492">Moduł</span><span class="sxs-lookup"><span data-stu-id="09c47-492">Module</span></span>

<span data-ttu-id="09c47-493">**Usługi IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="09c47-493">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="09c47-494">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="09c47-494">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="09c47-495">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="09c47-495">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="09c47-496">%ProgramFiles%\IIS\Asp.NET core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="09c47-496">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="09c47-497">% ProgramFiles (x86) %\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="09c47-497">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="09c47-498">**Usługi IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="09c47-498">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="09c47-499">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="09c47-499">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="09c47-500">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="09c47-500">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="09c47-501">%ProgramFiles%\iis Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="09c47-501">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="09c47-502">% ProgramFiles (x86) %\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="09c47-502">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="09c47-503">Schemat</span><span class="sxs-lookup"><span data-stu-id="09c47-503">Schema</span></span>

<span data-ttu-id="09c47-504">**IIS**</span><span class="sxs-lookup"><span data-stu-id="09c47-504">**IIS**</span></span>

* <span data-ttu-id="09c47-505">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="09c47-505">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="09c47-506">%Windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="09c47-506">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

<span data-ttu-id="09c47-507">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="09c47-507">**IIS Express**</span></span>

* <span data-ttu-id="09c47-508">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="09c47-508">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="09c47-509">%ProgramFiles%\iis Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="09c47-509">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="09c47-510">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="09c47-510">Configuration</span></span>

<span data-ttu-id="09c47-511">**IIS**</span><span class="sxs-lookup"><span data-stu-id="09c47-511">**IIS**</span></span>

* <span data-ttu-id="09c47-512">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="09c47-512">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="09c47-513">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="09c47-513">**IIS Express**</span></span>

* <span data-ttu-id="09c47-514">Visual Studio: {Katalog główny aplikacji}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="09c47-514">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="09c47-515">*iisexpress.exe* interfejsu wiersza polecenia: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="09c47-515">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="09c47-516">Pliki można znaleźć, wyszukując *aspnetcore* w *applicationHost.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="09c47-516">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="09c47-517">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="09c47-517">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="09c47-518">Repozytorium GitHub modułów Core ASP.NET (źródło odwołania)</span><span class="sxs-lookup"><span data-stu-id="09c47-518">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
