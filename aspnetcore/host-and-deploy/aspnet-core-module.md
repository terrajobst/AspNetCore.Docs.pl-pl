---
title: Informacje o konfiguracji ASP.NET Core modułu
author: guardrex
description: Dowiedz się, jak skonfigurować modułu ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/10/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: b6fdca7e5d59fbfb80428f77b5c772cc04daf4b0
ms.sourcegitcommit: 1872d2e6f299093c78a6795a486929ffb0bbffff
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53216888"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="db149-103">Informacje o konfiguracji ASP.NET Core modułu</span><span class="sxs-lookup"><span data-stu-id="db149-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="db149-104">Przez [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), i [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="db149-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="db149-105">Niniejszy dokument zawiera instrukcje dotyczące sposobu konfigurowania modułu ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db149-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="db149-106">Aby zapoznać się z wprowadzeniem do modułu ASP.NET Core i instrukcje dotyczące instalacji, zobacz [Przegląd modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="db149-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="db149-107">Model hostingu</span><span class="sxs-lookup"><span data-stu-id="db149-107">Hosting model</span></span>

<span data-ttu-id="db149-108">W przypadku aplikacji uruchomiony na platformy .NET Core 2.2 lub nowszym ten moduł umożliwia w trakcie modelu hostingu, w celu zwiększenia wydajności w porównaniu do hostingu (poza procesem) zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="db149-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="db149-109">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="db149-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="db149-110">Hosting w procsess jest zoptymalizowany pod kątem w przypadku istniejących aplikacji, ale [dotnet nowe](/dotnet/core/tools/dotnet-new) szablony domyślne do modelu hostowania w trakcie scenariusze dotyczące wszystkich usług IIS oraz usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="db149-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="db149-111">Aby skonfigurować aplikację do obsługi w procesie, Dodaj `<AspNetCoreHostingModel>` właściwość do pliku projektu aplikacji (na przykład *MyApp.csproj*) z wartością `InProcess` (spoza procesu hostingu została ustawiona za pomocą `outofprocess`):</span><span class="sxs-lookup"><span data-stu-id="db149-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `InProcess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="db149-112">Następujące właściwości mają zastosowanie w przypadku hostowania w procesie:</span><span class="sxs-lookup"><span data-stu-id="db149-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="db149-113">Serwer HTTP usług IIS (`IISHttpServer`) jest używany zamiast [Kestrel](xref:fundamentals/servers/kestrel) serwera.</span><span class="sxs-lookup"><span data-stu-id="db149-113">IIS HTTP Server (`IISHttpServer`) is used instead of the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="db149-114">Serwer HTTP usług IIS (`IISHttpServer`) jest kolejnym <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementację, która konwertuje natywnych żądań usług IIS platformy ASP.NET Core zarządzane żądania do przetworzenia przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="db149-114">IIS HTTP Server (`IISHttpServer`) is another <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation that converts IIS native requests into ASP.NET Core managed requests for processing by the app.</span></span>

* <span data-ttu-id="db149-115">[Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="db149-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="db149-116">Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="db149-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="db149-117">Użycie jednej puli aplikacji na aplikację.</span><span class="sxs-lookup"><span data-stu-id="db149-117">Use one app pool per app.</span></span>

* <span data-ttu-id="db149-118">Korzystając z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręczne wprowadzanie [plik app_offline.htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files), aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="db149-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="db149-119">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db149-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="db149-120">Architektura (liczba bitów) zainstalowanego środowiska uruchomieniowego (x64 lub x86) i aplikacji musi być zgodna z architekturą puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db149-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="db149-121">Ustawiając hosta aplikacji ręcznie za pomocą `WebHostBuilder` (nie używa [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) i nigdy nie uruchomienia aplikacji bezpośrednio na serwerze Kestrel (Self-Hosted), wywołanie `UseKestrel` przed wywołaniem `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="db149-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="db149-122">Jeśli kolejność została odwrócona, host nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="db149-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="db149-123">Rozłącza klienta są wykrywane.</span><span class="sxs-lookup"><span data-stu-id="db149-123">Client disconnects are detected.</span></span> <span data-ttu-id="db149-124">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) odwołano token anulowania, gdy klient odłączy się.</span><span class="sxs-lookup"><span data-stu-id="db149-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="db149-125"><xref:System.IO.Directory.GetCurrentDirectory*> Zwraca katalogu roboczego proces rozpoczęty przez usługi IIS, a nie w katalogu aplikacji (na przykład *C:\Windows\System32\inetsrv* dla *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="db149-125"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="db149-126">Przykładowy kod, który ustawia bieżący katalog aplikacji, zobacz [klasy CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="db149-126">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="db149-127">Wywołaj `SetCurrentDirectory` metody.</span><span class="sxs-lookup"><span data-stu-id="db149-127">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="db149-128">Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> zapewniają katalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db149-128">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="db149-129">Zmiany modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="db149-129">Hosting model changes</span></span>

<span data-ttu-id="db149-130">Jeśli `hostingModel` ustawienie to ulegnie zmianie w *web.config* pliku (wyjaśnione w [konfiguracji z pliku web.config](#configuration-with-webconfig) sekcji), moduł odtwarzania procesu roboczego programu IIS.</span><span class="sxs-lookup"><span data-stu-id="db149-130">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="db149-131">Dla usług IIS Express moduł nie odtworzyć proces roboczy, ale zamiast tego wyzwala łagodne zamykanie bieżący proces usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="db149-131">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="db149-132">Kolejne żądanie aplikacji spowoduje utworzenie nowych procesów usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="db149-132">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="db149-133">Nazwa procesu</span><span class="sxs-lookup"><span data-stu-id="db149-133">Process name</span></span>

<span data-ttu-id="db149-134">`Process.GetCurrentProcess().ProcessName` Raporty `w3wp` / `iisexpress` (w procesie) lub `dotnet` (poza procesem).</span><span class="sxs-lookup"><span data-stu-id="db149-134">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="db149-135">Konfiguracja z pliku web.config</span><span class="sxs-lookup"><span data-stu-id="db149-135">Configuration with web.config</span></span>

<span data-ttu-id="db149-136">Skonfigurowano modułu ASP.NET Core `aspNetCore` części `system.webServer` węzeł w tej witrynie *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="db149-136">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="db149-137">Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależny od struktury](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje modułu ASP.NET Core do obsługi żądań w lokacji:</span><span class="sxs-lookup"><span data-stu-id="db149-137">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="db149-138">Następujące *web.config* została opublikowana na potrzeby [niezależna wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="db149-138">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="db149-139"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Właściwość jest ustawiona na `false` do wskazania, że ustawienia określone w [ \<lokalizacja >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elementu nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db149-139">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="db149-140">Gdy aplikacja jest wdrażana na [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest równa `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="db149-140">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="db149-141">Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.</span><span class="sxs-lookup"><span data-stu-id="db149-141">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="db149-142">Aby uzyskać informacji na temat konfigurowania aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="db149-142">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="db149-143">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="db149-143">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="db149-144">Atrybut</span><span class="sxs-lookup"><span data-stu-id="db149-144">Attribute</span></span> | <span data-ttu-id="db149-145">Opis</span><span class="sxs-lookup"><span data-stu-id="db149-145">Description</span></span> | <span data-ttu-id="db149-146">Domyślny</span><span class="sxs-lookup"><span data-stu-id="db149-146">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="db149-147">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="db149-147">Optional string attribute.</span></span></p><p><span data-ttu-id="db149-148">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="db149-148">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="db149-149">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="db149-149">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="db149-150">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="db149-150">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="db149-151">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="db149-151">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="db149-152">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="db149-152">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="db149-153">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="db149-153">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="db149-154">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="db149-154">Optional string attribute.</span></span></p><p><span data-ttu-id="db149-155">Określa modelu hostingu w trakcie (`InProcess`) lub spoza procesu (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="db149-155">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="db149-156">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="db149-156">Optional integer attribute.</span></span></p><p><span data-ttu-id="db149-157">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="db149-157">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="db149-158">&dagger;W trakcie hostingu, wartość jest ograniczona do `1`.</span><span class="sxs-lookup"><span data-stu-id="db149-158">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="db149-159">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="db149-159">Default: `1`</span></span><br><span data-ttu-id="db149-160">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="db149-160">Min: `1`</span></span><br><span data-ttu-id="db149-161">Maks.: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="db149-161">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="db149-162">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="db149-162">Required string attribute.</span></span></p><p><span data-ttu-id="db149-163">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="db149-163">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="db149-164">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="db149-164">Relative paths are supported.</span></span> <span data-ttu-id="db149-165">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="db149-165">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="db149-166">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="db149-166">Optional integer attribute.</span></span></p><p><span data-ttu-id="db149-167">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="db149-167">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="db149-168">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="db149-168">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="db149-169">Nieobsługiwane za pomocą wewnątrzprocesowego hostingu.</span><span class="sxs-lookup"><span data-stu-id="db149-169">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="db149-170">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="db149-170">Default: `10`</span></span><br><span data-ttu-id="db149-171">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="db149-171">Min: `0`</span></span><br><span data-ttu-id="db149-172">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="db149-172">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="db149-173">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="db149-173">Optional timespan attribute.</span></span></p><p><span data-ttu-id="db149-174">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="db149-174">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="db149-175">W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="db149-175">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="db149-176">Nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="db149-176">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="db149-177">Dla hostingu w procesie modułu czeka na aplikację, aby przetworzyć żądanie.</span><span class="sxs-lookup"><span data-stu-id="db149-177">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="db149-178">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="db149-178">Default: `00:02:00`</span></span><br><span data-ttu-id="db149-179">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="db149-179">Min: `00:00:00`</span></span><br><span data-ttu-id="db149-180">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="db149-180">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="db149-181">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="db149-181">Optional integer attribute.</span></span></p><p><span data-ttu-id="db149-182">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="db149-182">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="db149-183">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="db149-183">Default: `10`</span></span><br><span data-ttu-id="db149-184">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="db149-184">Min: `0`</span></span><br><span data-ttu-id="db149-185">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="db149-185">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="db149-186">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="db149-186">Optional integer attribute.</span></span></p><p><span data-ttu-id="db149-187">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="db149-187">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="db149-188">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="db149-188">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="db149-189">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="db149-189">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="db149-190">Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="db149-190">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="db149-191">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="db149-191">Default: `120`</span></span><br><span data-ttu-id="db149-192">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="db149-192">Min: `0`</span></span><br><span data-ttu-id="db149-193">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="db149-193">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="db149-194">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="db149-194">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="db149-195">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="db149-195">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="db149-196">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="db149-196">Optional string attribute.</span></span></p><p><span data-ttu-id="db149-197">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="db149-197">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="db149-198">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="db149-198">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="db149-199">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="db149-199">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="db149-200">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="db149-200">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="db149-201">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="db149-201">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="db149-202">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="db149-202">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="db149-203">Atrybut</span><span class="sxs-lookup"><span data-stu-id="db149-203">Attribute</span></span> | <span data-ttu-id="db149-204">Opis</span><span class="sxs-lookup"><span data-stu-id="db149-204">Description</span></span> | <span data-ttu-id="db149-205">Domyślny</span><span class="sxs-lookup"><span data-stu-id="db149-205">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="db149-206">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="db149-206">Optional string attribute.</span></span></p><p><span data-ttu-id="db149-207">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="db149-207">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="db149-208">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="db149-208">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="db149-209">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="db149-209">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="db149-210">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="db149-210">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="db149-211">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="db149-211">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="db149-212">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="db149-212">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="db149-213">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="db149-213">Optional integer attribute.</span></span></p><p><span data-ttu-id="db149-214">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="db149-214">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="db149-215">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="db149-215">Default: `1`</span></span><br><span data-ttu-id="db149-216">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="db149-216">Min: `1`</span></span><br><span data-ttu-id="db149-217">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="db149-217">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="db149-218">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="db149-218">Required string attribute.</span></span></p><p><span data-ttu-id="db149-219">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="db149-219">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="db149-220">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="db149-220">Relative paths are supported.</span></span> <span data-ttu-id="db149-221">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="db149-221">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="db149-222">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="db149-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="db149-223">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="db149-223">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="db149-224">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="db149-224">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="db149-225">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="db149-225">Default: `10`</span></span><br><span data-ttu-id="db149-226">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="db149-226">Min: `0`</span></span><br><span data-ttu-id="db149-227">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="db149-227">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="db149-228">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="db149-228">Optional timespan attribute.</span></span></p><p><span data-ttu-id="db149-229">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="db149-229">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="db149-230">W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="db149-230">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="db149-231">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="db149-231">Default: `00:02:00`</span></span><br><span data-ttu-id="db149-232">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="db149-232">Min: `00:00:00`</span></span><br><span data-ttu-id="db149-233">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="db149-233">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="db149-234">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="db149-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="db149-235">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="db149-235">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="db149-236">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="db149-236">Default: `10`</span></span><br><span data-ttu-id="db149-237">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="db149-237">Min: `0`</span></span><br><span data-ttu-id="db149-238">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="db149-238">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="db149-239">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="db149-239">Optional integer attribute.</span></span></p><p><span data-ttu-id="db149-240">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="db149-240">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="db149-241">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="db149-241">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="db149-242">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="db149-242">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="db149-243">Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="db149-243">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="db149-244">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="db149-244">Default: `120`</span></span><br><span data-ttu-id="db149-245">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="db149-245">Min: `0`</span></span><br><span data-ttu-id="db149-246">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="db149-246">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="db149-247">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="db149-247">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="db149-248">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="db149-248">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="db149-249">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="db149-249">Optional string attribute.</span></span></p><p><span data-ttu-id="db149-250">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="db149-250">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="db149-251">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="db149-251">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="db149-252">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="db149-252">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="db149-253">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="db149-253">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="db149-254">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="db149-254">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="db149-255">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="db149-255">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="db149-256">Atrybut</span><span class="sxs-lookup"><span data-stu-id="db149-256">Attribute</span></span> | <span data-ttu-id="db149-257">Opis</span><span class="sxs-lookup"><span data-stu-id="db149-257">Description</span></span> | <span data-ttu-id="db149-258">Domyślny</span><span class="sxs-lookup"><span data-stu-id="db149-258">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="db149-259">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="db149-259">Optional string attribute.</span></span></p><p><span data-ttu-id="db149-260">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="db149-260">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="db149-261">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="db149-261">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="db149-262">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="db149-262">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="db149-263">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="db149-263">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="db149-264">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="db149-264">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="db149-265">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="db149-265">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="db149-266">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="db149-266">Optional integer attribute.</span></span></p><p><span data-ttu-id="db149-267">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="db149-267">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="db149-268">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="db149-268">Default: `1`</span></span><br><span data-ttu-id="db149-269">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="db149-269">Min: `1`</span></span><br><span data-ttu-id="db149-270">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="db149-270">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="db149-271">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="db149-271">Required string attribute.</span></span></p><p><span data-ttu-id="db149-272">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="db149-272">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="db149-273">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="db149-273">Relative paths are supported.</span></span> <span data-ttu-id="db149-274">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="db149-274">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="db149-275">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="db149-275">Optional integer attribute.</span></span></p><p><span data-ttu-id="db149-276">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="db149-276">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="db149-277">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="db149-277">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="db149-278">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="db149-278">Default: `10`</span></span><br><span data-ttu-id="db149-279">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="db149-279">Min: `0`</span></span><br><span data-ttu-id="db149-280">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="db149-280">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="db149-281">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="db149-281">Optional timespan attribute.</span></span></p><p><span data-ttu-id="db149-282">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="db149-282">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="db149-283">W wersjach modułu ASP.NET Core, dostarczonej wraz z wydaniem programu ASP.NET Core 2.0 lub wcześniej `requestTimeout` musi być określona w pełnych minut, w przeciwnym razie jego wartość domyślna to 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="db149-283">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="db149-284">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="db149-284">Default: `00:02:00`</span></span><br><span data-ttu-id="db149-285">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="db149-285">Min: `00:00:00`</span></span><br><span data-ttu-id="db149-286">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="db149-286">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="db149-287">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="db149-287">Optional integer attribute.</span></span></p><p><span data-ttu-id="db149-288">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="db149-288">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="db149-289">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="db149-289">Default: `10`</span></span><br><span data-ttu-id="db149-290">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="db149-290">Min: `0`</span></span><br><span data-ttu-id="db149-291">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="db149-291">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="db149-292">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="db149-292">Optional integer attribute.</span></span></p><p><span data-ttu-id="db149-293">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="db149-293">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="db149-294">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="db149-294">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="db149-295">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="db149-295">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="db149-296">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="db149-296">Default: `120`</span></span><br><span data-ttu-id="db149-297">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="db149-297">Min: `0`</span></span><br><span data-ttu-id="db149-298">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="db149-298">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="db149-299">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="db149-299">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="db149-300">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="db149-300">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="db149-301">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="db149-301">Optional string attribute.</span></span></p><p><span data-ttu-id="db149-302">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="db149-302">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="db149-303">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="db149-303">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="db149-304">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="db149-304">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="db149-305">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="db149-305">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="db149-306">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="db149-306">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="db149-307">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="db149-307">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="db149-308">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="db149-308">Setting environment variables</span></span>

<span data-ttu-id="db149-309">Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="db149-309">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="db149-310">Określ zmienną środowiskową `environmentVariable` element podrzędny elementu `environmentVariables` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="db149-310">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="db149-311">Zmienne środowiskowe ustawione w tej sekcji pierwszeństwo systemowe zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="db149-311">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="db149-312">W poniższym przykładzie ustawiono dwóch zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="db149-312">The following example sets two environment variables.</span></span> <span data-ttu-id="db149-313">`ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji `Development`.</span><span class="sxs-lookup"><span data-stu-id="db149-313">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="db149-314">Deweloper może tymczasowo ustawić tę wartość w *web.config* pliku, aby wymusić [stronie wyjątków deweloperów](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek.</span><span class="sxs-lookup"><span data-stu-id="db149-314">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="db149-315">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, gdzie deweloper ma napisany kod, który odczytuje wartość przy uruchamianiu w celu utworzenia ścieżki do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db149-315">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="db149-316">Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej, aby `Development` przejściowym i testowania serwerów, które nie są dostępne do niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="db149-316">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="db149-317">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="db149-317">app_offline.htm</span></span>

<span data-ttu-id="db149-318">Jeśli plik o nazwie *app_offline.htm* wykryte w katalogu głównym aplikacji, modułu ASP.NET Core próbuje łagodne zamykanie aplikacji i zatrzymanie przetwarzania przychodzących żądań.</span><span class="sxs-lookup"><span data-stu-id="db149-318">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="db149-319">Jeśli aplikacja jest nadal uruchomione po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, modułu ASP.NET Core to złe rozwiązanie uruchomionego procesu.</span><span class="sxs-lookup"><span data-stu-id="db149-319">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="db149-320">Gdy *app_offline.htm* występuje pliku modułu ASP.NET Core ma odpowiadać na żądania, wysyłając ponownie zawartość *app_offline.htm* pliku.</span><span class="sxs-lookup"><span data-stu-id="db149-320">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="db149-321">Gdy *app_offline.htm* plik jest usuwany, kolejne żądanie uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="db149-321">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="db149-322">Korzystając z modelu hostingu poza procesem, aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="db149-322">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="db149-323">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db149-323">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="db149-324">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="db149-324">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="db149-325">Zarówno w trakcie, jak i spoza procesu hostingu generuje strony błędów niestandardowych, gdy mogą zakończyć się niepowodzeniem uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="db149-325">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="db149-326">Jeśli modułu ASP.NET Core nie uda się znaleźć albo w trakcie lub poza procesem programu obsługi żądania, *500.0 — Błąd ładowania obsługi w-procesu/Out-Of-Process* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="db149-326">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="db149-327">W trakcie obsługi w przypadku niepowodzenia modułu ASP.NET Core uruchomić aplikację, *500.30 - Start błąd* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="db149-327">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="db149-328">Dla hostingu poza procesem, jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="db149-328">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="db149-329">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 5xx usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="db149-329">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="db149-330">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="db149-330">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="db149-331">Jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="db149-331">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="db149-332">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 502 usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="db149-332">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="db149-333">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="db149-333">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 strona kodowa stan niepowodzenia procesu](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="db149-335">Tworzenia dziennika i Przekierowanie</span><span class="sxs-lookup"><span data-stu-id="db149-335">Log creation and redirection</span></span>

<span data-ttu-id="db149-336">Modułu ASP.NET Core przekierowuje dane wyjściowe stdout i stderr konsoli do dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu są ustawione.</span><span class="sxs-lookup"><span data-stu-id="db149-336">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="db149-337">Wszystkie foldery w `stdoutLogFile` ścieżka musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="db149-337">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="db149-338">Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="db149-338">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="db149-339">Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="db149-339">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="db149-340">Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="db149-340">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="db149-341">Przy użyciu dziennika stdout jest zalecane tylko na potrzeby rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db149-341">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="db149-342">Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db149-342">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="db149-343">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="db149-343">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="db149-344">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="db149-344">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="db149-345">Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="db149-345">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="db149-346">Przez dodanie sygnatury czasowej, identyfikator procesu i rozszerzenie pliku składa się nazwa pliku dziennika (*.log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="db149-346">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="db149-347">Jeśli `stdoutLogFile` ścieżki kończy się *stdout*, dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzono 19:42:32 2/5/2018 o nazwie pliku *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="db149-347">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="db149-348">Jeśli `stdoutLogEnabled` ma wartość FAŁSZ, błędów występujących podczas uruchamiania aplikacji są przechwytywane i wysyłanego do dziennika zdarzeń do 30 KB.</span><span class="sxs-lookup"><span data-stu-id="db149-348">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="db149-349">Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="db149-349">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="db149-350">Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie strumienia stdout dla aplikacji hostowanej w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="db149-350">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="db149-351">Ścieżka lokalna lub ścieżkę do udziału sieciowego jest możliwa do logowania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="db149-351">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="db149-352">Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.</span><span class="sxs-lookup"><span data-stu-id="db149-352">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="db149-353">Rozszerzone dzienników diagnostycznych</span><span class="sxs-lookup"><span data-stu-id="db149-353">Enhanced diagnostic logs</span></span>

<span data-ttu-id="db149-354">Modułu ASP.NET Core udostępnia, są konfigurowane w celu zapewnienia dzienniki diagnostyki rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="db149-354">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="db149-355">Dodaj `<handlerSettings>` elementu `<aspNetCore>` element *web.config*. Ustawienie `debugLevel` do `TRACE` udostępnia pozwala uzyskać większą wierność informacje diagnostyczne:</span><span class="sxs-lookup"><span data-stu-id="db149-355">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="db149-356">Poziom debugowania (`debugLevel`) wartości mogą obejmować zarówno na poziomie, jak i lokalizację.</span><span class="sxs-lookup"><span data-stu-id="db149-356">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="db149-357">Poziomy (w kolejności od najmniejszej do najbardziej szczegółowy):</span><span class="sxs-lookup"><span data-stu-id="db149-357">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="db149-358">BŁĄD</span><span class="sxs-lookup"><span data-stu-id="db149-358">ERROR</span></span>
* <span data-ttu-id="db149-359">OSTRZEŻENIE</span><span class="sxs-lookup"><span data-stu-id="db149-359">WARNING</span></span>
* <span data-ttu-id="db149-360">INFORMACJE O</span><span class="sxs-lookup"><span data-stu-id="db149-360">INFO</span></span>
* <span data-ttu-id="db149-361">TRACE</span><span class="sxs-lookup"><span data-stu-id="db149-361">TRACE</span></span>

<span data-ttu-id="db149-362">Lokalizacje (wiele lokalizacji są dozwolone):</span><span class="sxs-lookup"><span data-stu-id="db149-362">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="db149-363">KONSOLA</span><span class="sxs-lookup"><span data-stu-id="db149-363">CONSOLE</span></span>
* <span data-ttu-id="db149-364">DZIENNIK ZDARZEŃ</span><span class="sxs-lookup"><span data-stu-id="db149-364">EVENTLOG</span></span>
* <span data-ttu-id="db149-365">PLIK</span><span class="sxs-lookup"><span data-stu-id="db149-365">FILE</span></span>

<span data-ttu-id="db149-366">Ustawienia obsługi można również podać za pośrednictwem zmienne środowiskowe:</span><span class="sxs-lookup"><span data-stu-id="db149-366">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="db149-367">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ścieżka do pliku dziennika debugowania.</span><span class="sxs-lookup"><span data-stu-id="db149-367">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="db149-368">(Domyślnie: *czy aspnetcore*)</span><span class="sxs-lookup"><span data-stu-id="db149-368">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="db149-369">`ASPNETCORE_MODULE_DEBUG` &ndash; Ustawienie poziomie debugowania.</span><span class="sxs-lookup"><span data-stu-id="db149-369">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="db149-370">Czy **nie** Pozostaw włączone we wdrożeniu na dłużej, niż jest to wymagane, aby rozwiązać problem rejestrowanie debugowania.</span><span class="sxs-lookup"><span data-stu-id="db149-370">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="db149-371">Rozmiar dziennika nie jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="db149-371">The size of the log isn't limited.</span></span> <span data-ttu-id="db149-372">Pozostawienie dziennik debugowania włączone może wyczerpać dostępne miejsce na dysku i awarii serwera lub usługi app service.</span><span class="sxs-lookup"><span data-stu-id="db149-372">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="db149-373">Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="db149-373">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="db149-374">Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token</span><span class="sxs-lookup"><span data-stu-id="db149-374">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="db149-375">*Dotyczy tylko hostingu poza procesem.*</span><span class="sxs-lookup"><span data-stu-id="db149-375">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="db149-376">Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="db149-376">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="db149-377">Przy użyciu protokołu HTTP jest Optymalizacja wydajności, gdzie ruch między modułem i Kestrel odbywa się na adres sprzężenia zwrotnego zniżki w stosunku do interfejsu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="db149-377">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="db149-378">Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="db149-378">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="db149-379">Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="db149-379">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="db149-380">Parowania token jest utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="db149-380">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="db149-381">Token parowania jest również ustawiona w nagłówku (`MS-ASPNETCORE-TOKEN`) na każde żądanie serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="db149-381">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="db149-382">Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="db149-382">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="db149-383">Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="db149-383">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="db149-384">Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="db149-384">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="db149-385">Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="db149-385">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="db149-386">Moduł ASP.NET Core z programem IIS konfigurację udostępnioną</span><span class="sxs-lookup"><span data-stu-id="db149-386">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="db149-387">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami **systemu** konta.</span><span class="sxs-lookup"><span data-stu-id="db149-387">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="db149-388">Ponieważ lokalne konto systemowe nie ma uprawnienia do modyfikowania dla ścieżki udziału konfiguracji udostępnionej usług IIS, Instalator trafienia odmowa dostępu błąd podczas próby skonfigurowania ustawień modułu w *applicationHost.config* w udziale.</span><span class="sxs-lookup"><span data-stu-id="db149-388">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="db149-389">Korzystając z konfiguracji udostępnionej usług IIS, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="db149-389">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="db149-390">Wyłącz konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="db149-390">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="db149-391">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="db149-391">Run the installer.</span></span>
1. <span data-ttu-id="db149-392">Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.</span><span class="sxs-lookup"><span data-stu-id="db149-392">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="db149-393">Ponownie włączyć konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="db149-393">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="db149-394">Wersja modułu oraz Dzienniki Instalatora pakietu hostingu</span><span class="sxs-lookup"><span data-stu-id="db149-394">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="db149-395">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="db149-395">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="db149-396">W systemie hostingu, przejdź do *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="db149-396">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="db149-397">Znajdź *aspnetcore.dll* pliku.</span><span class="sxs-lookup"><span data-stu-id="db149-397">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="db149-398">Kliknij prawym przyciskiem myszy plik i wybierz **właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="db149-398">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="db149-399">Wybierz **szczegóły** kartę. **Wersja pliku** i **wersji produktu** reprezentują zainstalowanej wersji modułu.</span><span class="sxs-lookup"><span data-stu-id="db149-399">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="db149-400">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się w *C:\\użytkowników\\% UserName %\\AppData\\lokalnego\\Temp*. Plik *dd_DotNetCoreWinSvrHosting__\<sygnatura czasowa > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="db149-400">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="db149-401">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="db149-401">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="db149-402">Moduł</span><span class="sxs-lookup"><span data-stu-id="db149-402">Module</span></span>

<span data-ttu-id="db149-403">**Usługi IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="db149-403">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="db149-404">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="db149-404">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="db149-405">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="db149-405">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="db149-406">%ProgramFiles%\IIS\Asp.NET core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="db149-406">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="db149-407">% ProgramFiles (x86) %\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="db149-407">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="db149-408">**Usługi IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="db149-408">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="db149-409">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="db149-409">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="db149-410">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="db149-410">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="db149-411">%ProgramFiles%\iis Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="db149-411">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="db149-412">% ProgramFiles (x86) %\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="db149-412">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="db149-413">Schemat</span><span class="sxs-lookup"><span data-stu-id="db149-413">Schema</span></span>

<span data-ttu-id="db149-414">**IIS**</span><span class="sxs-lookup"><span data-stu-id="db149-414">**IIS**</span></span>

   * <span data-ttu-id="db149-415">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="db149-415">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="db149-416">%Windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="db149-416">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="db149-417">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="db149-417">**IIS Express**</span></span>

   * <span data-ttu-id="db149-418">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="db149-418">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="db149-419">%ProgramFiles%\iis Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="db149-419">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="db149-420">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="db149-420">Configuration</span></span>

<span data-ttu-id="db149-421">**IIS**</span><span class="sxs-lookup"><span data-stu-id="db149-421">**IIS**</span></span>

   * <span data-ttu-id="db149-422">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="db149-422">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="db149-423">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="db149-423">**IIS Express**</span></span>

   * <span data-ttu-id="db149-424">%ProgramFiles%\iis Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="db149-424">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="db149-425">Pliki można znaleźć, wyszukując *aspnetcore* w *applicationHost.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="db149-425">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>
