---
title: Moduł ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować modułu ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/08/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c1c34f368cb3f7767bf0f229ff70c5ab53c6005f
ms.sourcegitcommit: fcdf9aaa6c45c1a926bd870ed8f893bdb4935152
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72165326"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="6284d-103">Moduł ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6284d-103">ASP.NET Core Module</span></span>

<span data-ttu-id="6284d-104">Autorzy [Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Krzysztof Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik)i [Luke](https://github.com/guardrex) Latham</span><span class="sxs-lookup"><span data-stu-id="6284d-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6284d-105">Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS:</span><span class="sxs-lookup"><span data-stu-id="6284d-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="6284d-106">Hostowanie aplikacji ASP.NET Core w procesie roboczym usług IIS (`w3wp.exe`), nazywanym [modelem hostingu w procesie](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="6284d-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="6284d-107">Przekazuj żądania sieci Web do zaplecza ASP.NET Core aplikacji, na której uruchomiono [serwer Kestrel](xref:fundamentals/servers/kestrel), nazywany [modelem hostingu poza procesem](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="6284d-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="6284d-108">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="6284d-108">Supported Windows versions:</span></span>

* <span data-ttu-id="6284d-109">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="6284d-109">Windows 7 or later</span></span>
* <span data-ttu-id="6284d-110">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="6284d-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="6284d-111">W przypadku hostingu w procesie moduł używa implementacji serwera w procesie dla usług IIS o nazwie serwer HTTP IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="6284d-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="6284d-112">Podczas hostingu poza procesem moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6284d-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="6284d-113">Moduł nie działa w przypadku [protokołu HTTP. sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6284d-113">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="6284d-114">Modele hostingu</span><span class="sxs-lookup"><span data-stu-id="6284d-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="6284d-115">Model hostingu w procesie</span><span class="sxs-lookup"><span data-stu-id="6284d-115">In-process hosting model</span></span>

<span data-ttu-id="6284d-116">Domyślnie ASP.NET Core aplikacje do modelu hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="6284d-116">ASP.NET Core apps default to the in-process hosting model.</span></span>

<span data-ttu-id="6284d-117">Następujące właściwości mają zastosowanie w przypadku hostowania w procesie:</span><span class="sxs-lookup"><span data-stu-id="6284d-117">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="6284d-118">Serwer HTTP IIS (`IISHttpServer`) jest używany zamiast serwera [Kestrel](xref:fundamentals/servers/kestrel) .</span><span class="sxs-lookup"><span data-stu-id="6284d-118">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="6284d-119">W przypadku wywołań [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> do:</span><span class="sxs-lookup"><span data-stu-id="6284d-119">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="6284d-120">Zarejestruj `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="6284d-120">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="6284d-121">Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6284d-121">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="6284d-122">Skonfiguruj hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="6284d-122">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="6284d-123">[Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="6284d-123">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="6284d-124">Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="6284d-124">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="6284d-125">Użycie jednej puli aplikacji na aplikację.</span><span class="sxs-lookup"><span data-stu-id="6284d-125">Use one app pool per app.</span></span>

* <span data-ttu-id="6284d-126">Korzystając z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręczne wprowadzanie [plik app_offline.htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files), aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="6284d-126">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6284d-127">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-127">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="6284d-128">Architektura (liczba bitów) zainstalowanego środowiska uruchomieniowego (x64 lub x86) i aplikacji musi być zgodna z architekturą puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-128">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="6284d-129">Rozłącza klienta są wykrywane.</span><span class="sxs-lookup"><span data-stu-id="6284d-129">Client disconnects are detected.</span></span> <span data-ttu-id="6284d-130">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) odwołano token anulowania, gdy klient odłączy się.</span><span class="sxs-lookup"><span data-stu-id="6284d-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="6284d-131">W ASP.NET Core 2.2.1 lub wcześniejszym, <xref:System.IO.Directory.GetCurrentDirectory*> zwraca katalog procesów roboczych procesu uruchomionego przez usługi IIS, a nie katalog aplikacji (na przykład *C:\Windows\System32\inetsrv* for *w3wp. exe*).</span><span class="sxs-lookup"><span data-stu-id="6284d-131">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="6284d-132">Przykładowy kod, który ustawia bieżący katalog aplikacji, zobacz [klasy CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="6284d-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="6284d-133">Wywołaj `SetCurrentDirectory` metody.</span><span class="sxs-lookup"><span data-stu-id="6284d-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="6284d-134">Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> zapewniają katalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="6284d-135">Podczas hostingu w procesie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wywoływana wewnętrznie w celu zainicjowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6284d-135">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="6284d-136">W związku z tym, implementacja <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> używana do przekształcania oświadczeń po każdym uwierzytelnieniu nie jest domyślnie aktywowana.</span><span class="sxs-lookup"><span data-stu-id="6284d-136">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="6284d-137">Podczas przekształcania oświadczeń z implementacją <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> Wywołaj <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, aby dodać usługi uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="6284d-137">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="6284d-138">Model hostingu poza procesem</span><span class="sxs-lookup"><span data-stu-id="6284d-138">Out-of-process hosting model</span></span>

<span data-ttu-id="6284d-139">Aby skonfigurować aplikację do hostingu poza procesem, ustaw wartość właściwości `<AspNetCoreHostingModel>` na `OutOfProcess` w pliku projektu ( *. csproj*):</span><span class="sxs-lookup"><span data-stu-id="6284d-139">To configure an app for out-of-process hosting, set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` in the project file (*.csproj*):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="6284d-140">Hosting w procesie jest ustawiany z `InProcess`, czyli wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="6284d-140">In-process hosting is set with `InProcess`, which is the default value.</span></span>

<span data-ttu-id="6284d-141">Serwer [Kestrel](xref:fundamentals/servers/kestrel) jest używany zamiast serwera http IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="6284d-141">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="6284d-142">W przypadku pozaprocesowych wywołań [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> do:</span><span class="sxs-lookup"><span data-stu-id="6284d-142">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="6284d-143">Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6284d-143">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="6284d-144">Skonfiguruj hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="6284d-144">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="6284d-145">Zmiany modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="6284d-145">Hosting model changes</span></span>

<span data-ttu-id="6284d-146">Jeśli `hostingModel` ustawienie to ulegnie zmianie w *web.config* pliku (wyjaśnione w [konfiguracji z pliku web.config](#configuration-with-webconfig) sekcji), moduł odtwarzania procesu roboczego programu IIS.</span><span class="sxs-lookup"><span data-stu-id="6284d-146">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="6284d-147">Dla usług IIS Express moduł nie odtworzyć proces roboczy, ale zamiast tego wyzwala łagodne zamykanie bieżący proces usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6284d-147">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="6284d-148">Kolejne żądanie aplikacji spowoduje utworzenie nowych procesów usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6284d-148">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="6284d-149">Nazwa procesu</span><span class="sxs-lookup"><span data-stu-id="6284d-149">Process name</span></span>

<span data-ttu-id="6284d-150">`Process.GetCurrentProcess().ProcessName` Raporty `w3wp` / `iisexpress` (w procesie) lub `dotnet` (poza procesem).</span><span class="sxs-lookup"><span data-stu-id="6284d-150">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="6284d-151">Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych.</span><span class="sxs-lookup"><span data-stu-id="6284d-151">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="6284d-152">Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="6284d-152">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="6284d-153">Moduł ASP.NET Core może również:</span><span class="sxs-lookup"><span data-stu-id="6284d-153">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="6284d-154">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="6284d-154">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="6284d-155">Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.</span><span class="sxs-lookup"><span data-stu-id="6284d-155">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="6284d-156">Przekazuj tokeny uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="6284d-156">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="6284d-157">Jak zainstalować moduł ASP.NET Core i korzystać z niego</span><span class="sxs-lookup"><span data-stu-id="6284d-157">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="6284d-158">Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="6284d-158">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="6284d-159">Konfiguracja z pliku web.config</span><span class="sxs-lookup"><span data-stu-id="6284d-159">Configuration with web.config</span></span>

<span data-ttu-id="6284d-160">Skonfigurowano modułu ASP.NET Core `aspNetCore` części `system.webServer` węzeł w tej witrynie *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-160">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="6284d-161">Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależny od struktury](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje modułu ASP.NET Core do obsługi żądań w lokacji:</span><span class="sxs-lookup"><span data-stu-id="6284d-161">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="6284d-162">Następujące *web.config* została opublikowana na potrzeby [niezależna wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="6284d-162">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="6284d-163"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Właściwość jest ustawiona na `false` do wskazania, że ustawienia określone w [ \<lokalizacja >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elementu nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-163">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="6284d-164">Gdy aplikacja jest wdrażana na [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest równa `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="6284d-164">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="6284d-165">Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.</span><span class="sxs-lookup"><span data-stu-id="6284d-165">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="6284d-166">Aby uzyskać informacji na temat konfigurowania aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="6284d-166">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="6284d-167">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="6284d-167">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="6284d-168">Atrybut</span><span class="sxs-lookup"><span data-stu-id="6284d-168">Attribute</span></span> | <span data-ttu-id="6284d-169">Opis</span><span class="sxs-lookup"><span data-stu-id="6284d-169">Description</span></span> | <span data-ttu-id="6284d-170">Domyślny</span><span class="sxs-lookup"><span data-stu-id="6284d-170">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6284d-171">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="6284d-171">Optional string attribute.</span></span></p><p><span data-ttu-id="6284d-172">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6284d-172">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6284d-173">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="6284d-173">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6284d-174">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="6284d-174">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6284d-175">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="6284d-175">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6284d-176">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="6284d-176">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6284d-177">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="6284d-177">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="6284d-178">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="6284d-178">Optional string attribute.</span></span></p><p><span data-ttu-id="6284d-179">Określa modelu hostingu w trakcie (`InProcess`) lub spoza procesu (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="6284d-179">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `InProcess` |
| `processesPerApplication` | <p><span data-ttu-id="6284d-180">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6284d-180">Optional integer attribute.</span></span></p><p><span data-ttu-id="6284d-181">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="6284d-181">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="6284d-182">&dagger;W trakcie hostingu, wartość jest ograniczona do `1`.</span><span class="sxs-lookup"><span data-stu-id="6284d-182">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="6284d-183">Ustawienie `processesPerApplication` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="6284d-183">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="6284d-184">Ten atrybut zostanie usunięty w przyszłych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="6284d-184">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="6284d-185">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="6284d-185">Default: `1`</span></span><br><span data-ttu-id="6284d-186">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="6284d-186">Min: `1`</span></span><br><span data-ttu-id="6284d-187">Maks.: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="6284d-187">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="6284d-188">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="6284d-188">Required string attribute.</span></span></p><p><span data-ttu-id="6284d-189">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="6284d-189">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6284d-190">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="6284d-190">Relative paths are supported.</span></span> <span data-ttu-id="6284d-191">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="6284d-191">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6284d-192">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6284d-192">Optional integer attribute.</span></span></p><p><span data-ttu-id="6284d-193">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="6284d-193">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6284d-194">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="6284d-194">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="6284d-195">Nieobsługiwane za pomocą wewnątrzprocesowego hostingu.</span><span class="sxs-lookup"><span data-stu-id="6284d-195">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="6284d-196">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="6284d-196">Default: `10`</span></span><br><span data-ttu-id="6284d-197">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="6284d-197">Min: `0`</span></span><br><span data-ttu-id="6284d-198">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="6284d-198">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6284d-199">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="6284d-199">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6284d-200">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="6284d-200">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6284d-201">W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="6284d-201">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="6284d-202">Nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="6284d-202">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="6284d-203">Dla hostingu w procesie modułu czeka na aplikację, aby przetworzyć żądanie.</span><span class="sxs-lookup"><span data-stu-id="6284d-203">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="6284d-204">Prawidłowe wartości segmentów minut i sekund ciągu mieszczą się w zakresie 0-59.</span><span class="sxs-lookup"><span data-stu-id="6284d-204">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="6284d-205">Użycie **60** w wartości minut lub sekund skutkuje *błędem wewnętrznego serwera 500*.</span><span class="sxs-lookup"><span data-stu-id="6284d-205">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="6284d-206">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6284d-206">Default: `00:02:00`</span></span><br><span data-ttu-id="6284d-207">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6284d-207">Min: `00:00:00`</span></span><br><span data-ttu-id="6284d-208">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6284d-208">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6284d-209">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6284d-209">Optional integer attribute.</span></span></p><p><span data-ttu-id="6284d-210">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="6284d-210">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6284d-211">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="6284d-211">Default: `10`</span></span><br><span data-ttu-id="6284d-212">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="6284d-212">Min: `0`</span></span><br><span data-ttu-id="6284d-213">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="6284d-213">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6284d-214">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6284d-214">Optional integer attribute.</span></span></p><p><span data-ttu-id="6284d-215">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="6284d-215">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6284d-216">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="6284d-216">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6284d-217">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="6284d-217">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="6284d-218">Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="6284d-218">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="6284d-219">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="6284d-219">Default: `120`</span></span><br><span data-ttu-id="6284d-220">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="6284d-220">Min: `0`</span></span><br><span data-ttu-id="6284d-221">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="6284d-221">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6284d-222">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="6284d-222">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6284d-223">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6284d-223">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6284d-224">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="6284d-224">Optional string attribute.</span></span></p><p><span data-ttu-id="6284d-225">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="6284d-225">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6284d-226">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="6284d-226">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6284d-227">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="6284d-227">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6284d-228">Wszystkie foldery podane w ścieżce są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="6284d-228">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="6284d-229">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku ( *.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="6284d-229">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6284d-230">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="6284d-230">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a><span data-ttu-id="6284d-231">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="6284d-231">Set environment variables</span></span>

<span data-ttu-id="6284d-232">Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6284d-232">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="6284d-233">Określ zmienną środowiskową `<environmentVariable>` element podrzędny elementu `<environmentVariables>` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="6284d-233">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="6284d-234">Zmienne środowiskowe ustawione w tej sekcji pierwszeństwo systemowe zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="6284d-234">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="6284d-235">W poniższym przykładzie są ustawiane dwie zmienne środowiskowe w *pliku Web. config*. `ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji do `Development`.</span><span class="sxs-lookup"><span data-stu-id="6284d-235">The following example sets two environment variables in *web.config*. `ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="6284d-236">Deweloper może tymczasowo ustawić tę wartość w *web.config* pliku, aby wymusić [stronie wyjątków deweloperów](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek.</span><span class="sxs-lookup"><span data-stu-id="6284d-236">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="6284d-237">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, gdzie deweloper ma napisany kod, który odczytuje wartość przy uruchamianiu w celu utworzenia ścieżki do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-237">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!NOTE]
> <span data-ttu-id="6284d-238">Alternatywą dla ustawienia środowiska bezpośrednio w pliku *Web. config* jest uwzględnienie właściwości `<EnvironmentName>` w profilu publikacji ( *. pubxml*) lub plik projektu.</span><span class="sxs-lookup"><span data-stu-id="6284d-238">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="6284d-239">To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:</span><span class="sxs-lookup"><span data-stu-id="6284d-239">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="6284d-240">Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej, aby `Development` przejściowym i testowania serwerów, które nie są dostępne do niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="6284d-240">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="6284d-241">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="6284d-241">app_offline.htm</span></span>

<span data-ttu-id="6284d-242">Jeśli plik o nazwie *app_offline.htm* wykryte w katalogu głównym aplikacji, modułu ASP.NET Core próbuje łagodne zamykanie aplikacji i zatrzymanie przetwarzania przychodzących żądań.</span><span class="sxs-lookup"><span data-stu-id="6284d-242">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="6284d-243">Jeśli aplikacja jest nadal uruchomione po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, modułu ASP.NET Core to złe rozwiązanie uruchomionego procesu.</span><span class="sxs-lookup"><span data-stu-id="6284d-243">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="6284d-244">Gdy *app_offline.htm* występuje pliku modułu ASP.NET Core ma odpowiadać na żądania, wysyłając ponownie zawartość *app_offline.htm* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-244">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="6284d-245">Gdy *app_offline.htm* plik jest usuwany, kolejne żądanie uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="6284d-245">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="6284d-246">Korzystając z modelu hostingu poza procesem, aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="6284d-246">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6284d-247">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-247">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="6284d-248">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="6284d-248">Start-up error page</span></span>

<span data-ttu-id="6284d-249">Zarówno w trakcie, jak i spoza procesu hostingu generuje strony błędów niestandardowych, gdy mogą zakończyć się niepowodzeniem uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6284d-249">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="6284d-250">Jeśli modułu ASP.NET Core nie uda się znaleźć albo w trakcie lub poza procesem programu obsługi żądania, *500.0 — Błąd ładowania obsługi w-procesu/Out-Of-Process* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="6284d-250">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="6284d-251">W trakcie obsługi w przypadku niepowodzenia modułu ASP.NET Core uruchomić aplikację, *500.30 - Start błąd* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="6284d-251">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="6284d-252">Dla hostingu poza procesem, jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="6284d-252">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="6284d-253">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 5xx usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6284d-253">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="6284d-254">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="6284d-254">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="6284d-255">Tworzenia dziennika i Przekierowanie</span><span class="sxs-lookup"><span data-stu-id="6284d-255">Log creation and redirection</span></span>

<span data-ttu-id="6284d-256">Modułu ASP.NET Core przekierowuje dane wyjściowe stdout i stderr konsoli do dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu są ustawione.</span><span class="sxs-lookup"><span data-stu-id="6284d-256">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="6284d-257">Wszystkie foldery w ścieżce `stdoutLogFile` są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="6284d-257">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="6284d-258">Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="6284d-258">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6284d-259">Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="6284d-259">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="6284d-260">Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="6284d-260">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="6284d-261">Przy użyciu dziennika stdout jest zalecane tylko na potrzeby rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-261">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="6284d-262">Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-262">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="6284d-263">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="6284d-263">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="6284d-264">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="6284d-264">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="6284d-265">Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="6284d-265">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="6284d-266">Przez dodanie sygnatury czasowej, identyfikator procesu i rozszerzenie pliku składa się nazwa pliku dziennika ( *.log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="6284d-266">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="6284d-267">Jeśli `stdoutLogFile` ścieżki kończy się *stdout*, dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzono 19:42:32 2/5/2018 o nazwie pliku *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="6284d-267">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="6284d-268">Jeśli `stdoutLogEnabled` ma wartość FAŁSZ, błędów występujących podczas uruchamiania aplikacji są przechwytywane i wysyłanego do dziennika zdarzeń do 30 KB.</span><span class="sxs-lookup"><span data-stu-id="6284d-268">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="6284d-269">Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="6284d-269">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="6284d-270">Poniższy przykład `aspNetCore` elementu w pliku *Web. config* konfiguruje rejestrowanie stdout dla aplikacji hostowanej w Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="6284d-270">The following sample `aspNetCore` element in a *web.config* file configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="6284d-271">Ścieżka lokalna lub ścieżkę do udziału sieciowego jest możliwa do logowania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="6284d-271">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="6284d-272">Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.</span><span class="sxs-lookup"><span data-stu-id="6284d-272">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="6284d-273">Rozszerzone dzienników diagnostycznych</span><span class="sxs-lookup"><span data-stu-id="6284d-273">Enhanced diagnostic logs</span></span>

<span data-ttu-id="6284d-274">Moduł ASP.NET Core można skonfigurować w celu udostępnienia dzienników diagnostyki rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="6284d-274">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="6284d-275">Dodaj `<handlerSettings>` elementu `<aspNetCore>` element *web.config*. Ustawienie `debugLevel` do `TRACE` udostępnia pozwala uzyskać większą wierność informacje diagnostyczne:</span><span class="sxs-lookup"><span data-stu-id="6284d-275">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="6284d-276">Wszystkie foldery w ścieżce (*dzienniki* w poprzednim przykładzie) są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="6284d-276">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="6284d-277">Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="6284d-277">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6284d-278">Poziom debugowania (`debugLevel`) wartości mogą obejmować zarówno na poziomie, jak i lokalizację.</span><span class="sxs-lookup"><span data-stu-id="6284d-278">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="6284d-279">Poziomy (w kolejności od najmniejszej do najbardziej szczegółowy):</span><span class="sxs-lookup"><span data-stu-id="6284d-279">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="6284d-280">BŁĄD</span><span class="sxs-lookup"><span data-stu-id="6284d-280">ERROR</span></span>
* <span data-ttu-id="6284d-281">OSTRZEŻENIE</span><span class="sxs-lookup"><span data-stu-id="6284d-281">WARNING</span></span>
* <span data-ttu-id="6284d-282">INFORMACJE O</span><span class="sxs-lookup"><span data-stu-id="6284d-282">INFO</span></span>
* <span data-ttu-id="6284d-283">TRACE</span><span class="sxs-lookup"><span data-stu-id="6284d-283">TRACE</span></span>

<span data-ttu-id="6284d-284">Lokalizacje (wiele lokalizacji są dozwolone):</span><span class="sxs-lookup"><span data-stu-id="6284d-284">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="6284d-285">KONSOLA</span><span class="sxs-lookup"><span data-stu-id="6284d-285">CONSOLE</span></span>
* <span data-ttu-id="6284d-286">DZIENNIK ZDARZEŃ</span><span class="sxs-lookup"><span data-stu-id="6284d-286">EVENTLOG</span></span>
* <span data-ttu-id="6284d-287">PLIK</span><span class="sxs-lookup"><span data-stu-id="6284d-287">FILE</span></span>

<span data-ttu-id="6284d-288">Ustawienia obsługi można również podać za pośrednictwem zmienne środowiskowe:</span><span class="sxs-lookup"><span data-stu-id="6284d-288">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="6284d-289">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ścieżka do pliku dziennika debugowania.</span><span class="sxs-lookup"><span data-stu-id="6284d-289">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="6284d-290">(Domyślnie: *czy aspnetcore*)</span><span class="sxs-lookup"><span data-stu-id="6284d-290">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="6284d-291">`ASPNETCORE_MODULE_DEBUG` &ndash; Ustawienie poziomie debugowania.</span><span class="sxs-lookup"><span data-stu-id="6284d-291">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="6284d-292">Czy **nie** Pozostaw włączone we wdrożeniu na dłużej, niż jest to wymagane, aby rozwiązać problem rejestrowanie debugowania.</span><span class="sxs-lookup"><span data-stu-id="6284d-292">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="6284d-293">Rozmiar dziennika nie jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="6284d-293">The size of the log isn't limited.</span></span> <span data-ttu-id="6284d-294">Pozostawienie dziennik debugowania włączone może wyczerpać dostępne miejsce na dysku i awarii serwera lub usługi app service.</span><span class="sxs-lookup"><span data-stu-id="6284d-294">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="6284d-295">Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-295">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="modify-the-stack-size"></a><span data-ttu-id="6284d-296">Modyfikowanie rozmiaru stosu</span><span class="sxs-lookup"><span data-stu-id="6284d-296">Modify the stack size</span></span>

<span data-ttu-id="6284d-297">*Stosuje się tylko w przypadku korzystania z modelu hostingu w procesie.*</span><span class="sxs-lookup"><span data-stu-id="6284d-297">*Only applies when using the in-process hosting model.*</span></span>

<span data-ttu-id="6284d-298">Skonfiguruj rozmiar zarządzanego stosu przy użyciu ustawienia `stackSize` w bajtach w *pliku Web. config*. Domyślny rozmiar to `1048576` bajtów (1 MB).</span><span class="sxs-lookup"><span data-stu-id="6284d-298">Configure the managed stack size using the `stackSize` setting in bytes in *web.config*. The default size is `1048576` bytes (1 MB).</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="6284d-299">Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token</span><span class="sxs-lookup"><span data-stu-id="6284d-299">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="6284d-300">*Dotyczy tylko hostingu poza procesem.*</span><span class="sxs-lookup"><span data-stu-id="6284d-300">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="6284d-301">Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6284d-301">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="6284d-302">Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="6284d-302">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="6284d-303">Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="6284d-303">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="6284d-304">Parowania token jest utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="6284d-304">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="6284d-305">Token parowania jest również ustawiona w nagłówku (`MS-ASPNETCORE-TOKEN`) na każde żądanie serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="6284d-305">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="6284d-306">Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="6284d-306">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="6284d-307">Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="6284d-307">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="6284d-308">Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="6284d-308">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="6284d-309">Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="6284d-309">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="6284d-310">Moduł ASP.NET Core z programem IIS konfigurację udostępnioną</span><span class="sxs-lookup"><span data-stu-id="6284d-310">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="6284d-311">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** .</span><span class="sxs-lookup"><span data-stu-id="6284d-311">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="6284d-312">Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w pliku *ApplicationHost. config* na udział.</span><span class="sxs-lookup"><span data-stu-id="6284d-312">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="6284d-313">W przypadku korzystania z konfiguracji udostępnionej usług IIS na tym samym komputerze, na którym znajduje się instalacja usług IIS, uruchom Instalatora pakietu ASP.NET Core hostowania z parametrem `OPT_NO_SHARED_CONFIG_CHECK` ustawionym na `1`:</span><span class="sxs-lookup"><span data-stu-id="6284d-313">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="6284d-314">Jeśli ścieżka do konfiguracji udostępnionej nie znajduje się na tym samym komputerze, na którym znajduje się instalacja usług IIS, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="6284d-314">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="6284d-315">Wyłącz konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="6284d-315">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="6284d-316">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="6284d-316">Run the installer.</span></span>
1. <span data-ttu-id="6284d-317">Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.</span><span class="sxs-lookup"><span data-stu-id="6284d-317">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="6284d-318">Ponownie włączyć konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="6284d-318">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="6284d-319">Wersja modułu oraz Dzienniki Instalatora pakietu hostingu</span><span class="sxs-lookup"><span data-stu-id="6284d-319">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="6284d-320">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="6284d-320">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="6284d-321">W systemie hostingu, przejdź do *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="6284d-321">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="6284d-322">Znajdź *aspnetcore.dll* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-322">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="6284d-323">Kliknij prawym przyciskiem myszy plik i wybierz **właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="6284d-323">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="6284d-324">Wybierz **szczegóły** kartę. **Wersja pliku** i **wersji produktu** reprezentują zainstalowanej wersji modułu.</span><span class="sxs-lookup"><span data-stu-id="6284d-324">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="6284d-325">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się w *C:\\użytkowników\\% UserName %\\AppData\\lokalnego\\Temp*. Plik *dd_DotNetCoreWinSvrHosting__\<sygnatura czasowa > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="6284d-325">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="6284d-326">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="6284d-326">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="6284d-327">Moduł</span><span class="sxs-lookup"><span data-stu-id="6284d-327">Module</span></span>

<span data-ttu-id="6284d-328">**Usługi IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6284d-328">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="6284d-329">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-329">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="6284d-330">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-330">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="6284d-331">%ProgramFiles%\IIS\Asp.NET core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-331">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="6284d-332">% ProgramFiles (x86) %\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-332">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="6284d-333">**Usługi IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6284d-333">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="6284d-334">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-334">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="6284d-335">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-335">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="6284d-336">%ProgramFiles%\iis Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-336">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="6284d-337">% ProgramFiles (x86) %\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-337">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="6284d-338">Schemat</span><span class="sxs-lookup"><span data-stu-id="6284d-338">Schema</span></span>

<span data-ttu-id="6284d-339">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6284d-339">**IIS**</span></span>

* <span data-ttu-id="6284d-340">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6284d-340">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="6284d-341">%Windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="6284d-341">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="6284d-342">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6284d-342">**IIS Express**</span></span>

* <span data-ttu-id="6284d-343">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6284d-343">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="6284d-344">%ProgramFiles%\iis Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="6284d-344">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="6284d-345">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="6284d-345">Configuration</span></span>

<span data-ttu-id="6284d-346">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6284d-346">**IIS**</span></span>

* <span data-ttu-id="6284d-347">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6284d-347">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="6284d-348">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6284d-348">**IIS Express**</span></span>

* <span data-ttu-id="6284d-349">Visual Studio: {Aplikacja główna} \\. vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6284d-349">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="6284d-350">Interfejs wiersza polecenia *iisexpress. exe* :%USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="6284d-350">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="6284d-351">Pliki można znaleźć, wyszukując *aspnetcore* w *applicationHost.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-351">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="6284d-352">Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS:</span><span class="sxs-lookup"><span data-stu-id="6284d-352">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="6284d-353">Hostowanie aplikacji ASP.NET Core w procesie roboczym usług IIS (`w3wp.exe`), nazywanym [modelem hostingu w procesie](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="6284d-353">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="6284d-354">Przekazuj żądania sieci Web do zaplecza ASP.NET Core aplikacji, na której uruchomiono [serwer Kestrel](xref:fundamentals/servers/kestrel), nazywany [modelem hostingu poza procesem](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="6284d-354">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="6284d-355">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="6284d-355">Supported Windows versions:</span></span>

* <span data-ttu-id="6284d-356">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="6284d-356">Windows 7 or later</span></span>
* <span data-ttu-id="6284d-357">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="6284d-357">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="6284d-358">W przypadku hostingu w procesie moduł używa implementacji serwera w procesie dla usług IIS o nazwie serwer HTTP IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="6284d-358">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="6284d-359">Podczas hostingu poza procesem moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6284d-359">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="6284d-360">Moduł nie działa w przypadku [protokołu HTTP. sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6284d-360">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="6284d-361">Modele hostingu</span><span class="sxs-lookup"><span data-stu-id="6284d-361">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="6284d-362">Model hostingu w procesie</span><span class="sxs-lookup"><span data-stu-id="6284d-362">In-process hosting model</span></span>

<span data-ttu-id="6284d-363">Aby skonfigurować aplikację do hostingu w procesie, należy dodać właściwość `<AspNetCoreHostingModel>` do pliku projektu aplikacji o wartości `InProcess` (hosting zewnętrzny jest ustawiony z `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="6284d-363">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="6284d-364">Model hostingu w procesie nie jest obsługiwany w przypadku aplikacji ASP.NET Core przeznaczonych dla .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6284d-364">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="6284d-365">Jeśli właściwość `<AspNetCoreHostingModel>` nie istnieje w pliku, wartość domyślna to `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="6284d-365">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="6284d-366">Następujące właściwości mają zastosowanie w przypadku hostowania w procesie:</span><span class="sxs-lookup"><span data-stu-id="6284d-366">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="6284d-367">Serwer HTTP IIS (`IISHttpServer`) jest używany zamiast serwera [Kestrel](xref:fundamentals/servers/kestrel) .</span><span class="sxs-lookup"><span data-stu-id="6284d-367">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="6284d-368">W przypadku wywołań [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> do:</span><span class="sxs-lookup"><span data-stu-id="6284d-368">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="6284d-369">Zarejestruj `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="6284d-369">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="6284d-370">Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6284d-370">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="6284d-371">Skonfiguruj hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="6284d-371">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="6284d-372">[Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="6284d-372">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="6284d-373">Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="6284d-373">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="6284d-374">Użycie jednej puli aplikacji na aplikację.</span><span class="sxs-lookup"><span data-stu-id="6284d-374">Use one app pool per app.</span></span>

* <span data-ttu-id="6284d-375">Korzystając z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręczne wprowadzanie [plik app_offline.htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files), aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="6284d-375">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6284d-376">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-376">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="6284d-377">Architektura (liczba bitów) zainstalowanego środowiska uruchomieniowego (x64 lub x86) i aplikacji musi być zgodna z architekturą puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-377">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="6284d-378">Rozłącza klienta są wykrywane.</span><span class="sxs-lookup"><span data-stu-id="6284d-378">Client disconnects are detected.</span></span> <span data-ttu-id="6284d-379">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) odwołano token anulowania, gdy klient odłączy się.</span><span class="sxs-lookup"><span data-stu-id="6284d-379">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="6284d-380">W ASP.NET Core 2.2.1 lub wcześniejszym, <xref:System.IO.Directory.GetCurrentDirectory*> zwraca katalog procesów roboczych procesu uruchomionego przez usługi IIS, a nie katalog aplikacji (na przykład *C:\Windows\System32\inetsrv* for *w3wp. exe*).</span><span class="sxs-lookup"><span data-stu-id="6284d-380">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="6284d-381">Przykładowy kod, który ustawia bieżący katalog aplikacji, zobacz [klasy CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="6284d-381">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="6284d-382">Wywołaj `SetCurrentDirectory` metody.</span><span class="sxs-lookup"><span data-stu-id="6284d-382">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="6284d-383">Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> zapewniają katalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-383">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="6284d-384">Podczas hostingu w procesie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wywoływana wewnętrznie w celu zainicjowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6284d-384">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="6284d-385">W związku z tym, implementacja <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> używana do przekształcania oświadczeń po każdym uwierzytelnieniu nie jest domyślnie aktywowana.</span><span class="sxs-lookup"><span data-stu-id="6284d-385">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="6284d-386">Podczas przekształcania oświadczeń z implementacją <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> Wywołaj <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, aby dodać usługi uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="6284d-386">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="6284d-387">Model hostingu poza procesem</span><span class="sxs-lookup"><span data-stu-id="6284d-387">Out-of-process hosting model</span></span>

<span data-ttu-id="6284d-388">Aby skonfigurować aplikację do hostingu poza procesem, użyj jednego z następujących metod w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="6284d-388">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="6284d-389">Nie określaj właściwości `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="6284d-389">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="6284d-390">Jeśli właściwość `<AspNetCoreHostingModel>` nie istnieje w pliku, wartość domyślna to `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="6284d-390">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="6284d-391">Ustaw wartość właściwości `<AspNetCoreHostingModel>` na `OutOfProcess` (hosting w procesie jest ustawiony z `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="6284d-391">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="6284d-392">Serwer [Kestrel](xref:fundamentals/servers/kestrel) jest używany zamiast serwera http IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="6284d-392">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="6284d-393">W przypadku pozaprocesowych wywołań [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> do:</span><span class="sxs-lookup"><span data-stu-id="6284d-393">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="6284d-394">Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6284d-394">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="6284d-395">Skonfiguruj hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="6284d-395">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="6284d-396">Zmiany modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="6284d-396">Hosting model changes</span></span>

<span data-ttu-id="6284d-397">Jeśli `hostingModel` ustawienie to ulegnie zmianie w *web.config* pliku (wyjaśnione w [konfiguracji z pliku web.config](#configuration-with-webconfig) sekcji), moduł odtwarzania procesu roboczego programu IIS.</span><span class="sxs-lookup"><span data-stu-id="6284d-397">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="6284d-398">Dla usług IIS Express moduł nie odtworzyć proces roboczy, ale zamiast tego wyzwala łagodne zamykanie bieżący proces usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6284d-398">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="6284d-399">Kolejne żądanie aplikacji spowoduje utworzenie nowych procesów usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6284d-399">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="6284d-400">Nazwa procesu</span><span class="sxs-lookup"><span data-stu-id="6284d-400">Process name</span></span>

<span data-ttu-id="6284d-401">`Process.GetCurrentProcess().ProcessName` Raporty `w3wp` / `iisexpress` (w procesie) lub `dotnet` (poza procesem).</span><span class="sxs-lookup"><span data-stu-id="6284d-401">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="6284d-402">Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych.</span><span class="sxs-lookup"><span data-stu-id="6284d-402">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="6284d-403">Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="6284d-403">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="6284d-404">Moduł ASP.NET Core może również:</span><span class="sxs-lookup"><span data-stu-id="6284d-404">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="6284d-405">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="6284d-405">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="6284d-406">Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.</span><span class="sxs-lookup"><span data-stu-id="6284d-406">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="6284d-407">Przekazuj tokeny uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="6284d-407">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="6284d-408">Jak zainstalować moduł ASP.NET Core i korzystać z niego</span><span class="sxs-lookup"><span data-stu-id="6284d-408">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="6284d-409">Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="6284d-409">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="6284d-410">Konfiguracja z pliku web.config</span><span class="sxs-lookup"><span data-stu-id="6284d-410">Configuration with web.config</span></span>

<span data-ttu-id="6284d-411">Skonfigurowano modułu ASP.NET Core `aspNetCore` części `system.webServer` węzeł w tej witrynie *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-411">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="6284d-412">Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależny od struktury](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje modułu ASP.NET Core do obsługi żądań w lokacji:</span><span class="sxs-lookup"><span data-stu-id="6284d-412">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="6284d-413">Następujące *web.config* została opublikowana na potrzeby [niezależna wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="6284d-413">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="6284d-414"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Właściwość jest ustawiona na `false` do wskazania, że ustawienia określone w [ \<lokalizacja >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elementu nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-414">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="6284d-415">Gdy aplikacja jest wdrażana na [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest równa `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="6284d-415">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="6284d-416">Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.</span><span class="sxs-lookup"><span data-stu-id="6284d-416">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="6284d-417">Aby uzyskać informacji na temat konfigurowania aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="6284d-417">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="6284d-418">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="6284d-418">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="6284d-419">Atrybut</span><span class="sxs-lookup"><span data-stu-id="6284d-419">Attribute</span></span> | <span data-ttu-id="6284d-420">Opis</span><span class="sxs-lookup"><span data-stu-id="6284d-420">Description</span></span> | <span data-ttu-id="6284d-421">Domyślny</span><span class="sxs-lookup"><span data-stu-id="6284d-421">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6284d-422">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="6284d-422">Optional string attribute.</span></span></p><p><span data-ttu-id="6284d-423">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6284d-423">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6284d-424">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="6284d-424">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6284d-425">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="6284d-425">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6284d-426">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="6284d-426">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6284d-427">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="6284d-427">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6284d-428">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="6284d-428">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="6284d-429">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="6284d-429">Optional string attribute.</span></span></p><p><span data-ttu-id="6284d-430">Określa modelu hostingu w trakcie (`InProcess`) lub spoza procesu (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="6284d-430">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="6284d-431">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6284d-431">Optional integer attribute.</span></span></p><p><span data-ttu-id="6284d-432">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="6284d-432">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="6284d-433">&dagger;W trakcie hostingu, wartość jest ograniczona do `1`.</span><span class="sxs-lookup"><span data-stu-id="6284d-433">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="6284d-434">Ustawienie `processesPerApplication` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="6284d-434">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="6284d-435">Ten atrybut zostanie usunięty w przyszłych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="6284d-435">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="6284d-436">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="6284d-436">Default: `1`</span></span><br><span data-ttu-id="6284d-437">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="6284d-437">Min: `1`</span></span><br><span data-ttu-id="6284d-438">Maks.: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="6284d-438">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="6284d-439">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="6284d-439">Required string attribute.</span></span></p><p><span data-ttu-id="6284d-440">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="6284d-440">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6284d-441">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="6284d-441">Relative paths are supported.</span></span> <span data-ttu-id="6284d-442">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="6284d-442">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6284d-443">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6284d-443">Optional integer attribute.</span></span></p><p><span data-ttu-id="6284d-444">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="6284d-444">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6284d-445">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="6284d-445">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="6284d-446">Nieobsługiwane za pomocą wewnątrzprocesowego hostingu.</span><span class="sxs-lookup"><span data-stu-id="6284d-446">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="6284d-447">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="6284d-447">Default: `10`</span></span><br><span data-ttu-id="6284d-448">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="6284d-448">Min: `0`</span></span><br><span data-ttu-id="6284d-449">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="6284d-449">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6284d-450">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="6284d-450">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6284d-451">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="6284d-451">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6284d-452">W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="6284d-452">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="6284d-453">Nie ma zastosowania do hostowania w procesie.</span><span class="sxs-lookup"><span data-stu-id="6284d-453">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="6284d-454">Dla hostingu w procesie modułu czeka na aplikację, aby przetworzyć żądanie.</span><span class="sxs-lookup"><span data-stu-id="6284d-454">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="6284d-455">Prawidłowe wartości segmentów minut i sekund ciągu mieszczą się w zakresie 0-59.</span><span class="sxs-lookup"><span data-stu-id="6284d-455">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="6284d-456">Użycie **60** w wartości minut lub sekund skutkuje *błędem wewnętrznego serwera 500*.</span><span class="sxs-lookup"><span data-stu-id="6284d-456">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="6284d-457">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6284d-457">Default: `00:02:00`</span></span><br><span data-ttu-id="6284d-458">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6284d-458">Min: `00:00:00`</span></span><br><span data-ttu-id="6284d-459">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6284d-459">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6284d-460">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6284d-460">Optional integer attribute.</span></span></p><p><span data-ttu-id="6284d-461">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="6284d-461">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6284d-462">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="6284d-462">Default: `10`</span></span><br><span data-ttu-id="6284d-463">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="6284d-463">Min: `0`</span></span><br><span data-ttu-id="6284d-464">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="6284d-464">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6284d-465">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6284d-465">Optional integer attribute.</span></span></p><p><span data-ttu-id="6284d-466">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="6284d-466">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6284d-467">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="6284d-467">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6284d-468">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="6284d-468">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="6284d-469">Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="6284d-469">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="6284d-470">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="6284d-470">Default: `120`</span></span><br><span data-ttu-id="6284d-471">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="6284d-471">Min: `0`</span></span><br><span data-ttu-id="6284d-472">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="6284d-472">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6284d-473">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="6284d-473">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6284d-474">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6284d-474">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6284d-475">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="6284d-475">Optional string attribute.</span></span></p><p><span data-ttu-id="6284d-476">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="6284d-476">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6284d-477">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="6284d-477">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6284d-478">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="6284d-478">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6284d-479">Wszystkie foldery podane w ścieżce są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="6284d-479">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="6284d-480">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku ( *.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="6284d-480">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6284d-481">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="6284d-481">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="6284d-482">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="6284d-482">Setting environment variables</span></span>

<span data-ttu-id="6284d-483">Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6284d-483">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="6284d-484">Określ zmienną środowiskową `<environmentVariable>` element podrzędny elementu `<environmentVariables>` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="6284d-484">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="6284d-485">Zmienne środowiskowe ustawione w tej sekcji pierwszeństwo systemowe zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="6284d-485">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="6284d-486">W poniższym przykładzie ustawiono dwóch zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="6284d-486">The following example sets two environment variables.</span></span> <span data-ttu-id="6284d-487">`ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji `Development`.</span><span class="sxs-lookup"><span data-stu-id="6284d-487">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="6284d-488">Deweloper może tymczasowo ustawić tę wartość w *web.config* pliku, aby wymusić [stronie wyjątków deweloperów](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek.</span><span class="sxs-lookup"><span data-stu-id="6284d-488">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="6284d-489">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, gdzie deweloper ma napisany kod, który odczytuje wartość przy uruchamianiu w celu utworzenia ścieżki do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-489">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!NOTE]
> <span data-ttu-id="6284d-490">Alternatywą dla ustawienia środowiska bezpośrednio w pliku *Web. config* jest uwzględnienie właściwości `<EnvironmentName>` w profilu publikacji ( *. pubxml*) lub plik projektu.</span><span class="sxs-lookup"><span data-stu-id="6284d-490">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="6284d-491">To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:</span><span class="sxs-lookup"><span data-stu-id="6284d-491">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="6284d-492">Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej, aby `Development` przejściowym i testowania serwerów, które nie są dostępne do niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="6284d-492">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="6284d-493">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="6284d-493">app_offline.htm</span></span>

<span data-ttu-id="6284d-494">Jeśli plik o nazwie *app_offline.htm* wykryte w katalogu głównym aplikacji, modułu ASP.NET Core próbuje łagodne zamykanie aplikacji i zatrzymanie przetwarzania przychodzących żądań.</span><span class="sxs-lookup"><span data-stu-id="6284d-494">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="6284d-495">Jeśli aplikacja jest nadal uruchomione po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, modułu ASP.NET Core to złe rozwiązanie uruchomionego procesu.</span><span class="sxs-lookup"><span data-stu-id="6284d-495">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="6284d-496">Gdy *app_offline.htm* występuje pliku modułu ASP.NET Core ma odpowiadać na żądania, wysyłając ponownie zawartość *app_offline.htm* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-496">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="6284d-497">Gdy *app_offline.htm* plik jest usuwany, kolejne żądanie uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="6284d-497">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="6284d-498">Korzystając z modelu hostingu poza procesem, aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia.</span><span class="sxs-lookup"><span data-stu-id="6284d-498">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6284d-499">Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-499">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="6284d-500">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="6284d-500">Start-up error page</span></span>

<span data-ttu-id="6284d-501">Zarówno w trakcie, jak i spoza procesu hostingu generuje strony błędów niestandardowych, gdy mogą zakończyć się niepowodzeniem uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6284d-501">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="6284d-502">Jeśli modułu ASP.NET Core nie uda się znaleźć albo w trakcie lub poza procesem programu obsługi żądania, *500.0 — Błąd ładowania obsługi w-procesu/Out-Of-Process* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="6284d-502">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="6284d-503">W trakcie obsługi w przypadku niepowodzenia modułu ASP.NET Core uruchomić aplikację, *500.30 - Start błąd* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="6284d-503">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="6284d-504">Dla hostingu poza procesem, jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="6284d-504">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="6284d-505">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 5xx usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6284d-505">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="6284d-506">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="6284d-506">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="6284d-507">Tworzenia dziennika i Przekierowanie</span><span class="sxs-lookup"><span data-stu-id="6284d-507">Log creation and redirection</span></span>

<span data-ttu-id="6284d-508">Modułu ASP.NET Core przekierowuje dane wyjściowe stdout i stderr konsoli do dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu są ustawione.</span><span class="sxs-lookup"><span data-stu-id="6284d-508">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="6284d-509">Wszystkie foldery w ścieżce `stdoutLogFile` są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="6284d-509">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="6284d-510">Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="6284d-510">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6284d-511">Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="6284d-511">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="6284d-512">Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="6284d-512">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="6284d-513">Przy użyciu dziennika stdout jest zalecane tylko na potrzeby rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-513">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="6284d-514">Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-514">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="6284d-515">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="6284d-515">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="6284d-516">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="6284d-516">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="6284d-517">Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="6284d-517">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="6284d-518">Przez dodanie sygnatury czasowej, identyfikator procesu i rozszerzenie pliku składa się nazwa pliku dziennika ( *.log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="6284d-518">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="6284d-519">Jeśli `stdoutLogFile` ścieżki kończy się *stdout*, dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzono 19:42:32 2/5/2018 o nazwie pliku *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="6284d-519">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="6284d-520">Jeśli `stdoutLogEnabled` ma wartość FAŁSZ, błędów występujących podczas uruchamiania aplikacji są przechwytywane i wysyłanego do dziennika zdarzeń do 30 KB.</span><span class="sxs-lookup"><span data-stu-id="6284d-520">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="6284d-521">Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="6284d-521">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="6284d-522">Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie strumienia stdout dla aplikacji hostowanej w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="6284d-522">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="6284d-523">Ścieżka lokalna lub ścieżkę do udziału sieciowego jest możliwa do logowania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="6284d-523">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="6284d-524">Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.</span><span class="sxs-lookup"><span data-stu-id="6284d-524">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="6284d-525">Rozszerzone dzienników diagnostycznych</span><span class="sxs-lookup"><span data-stu-id="6284d-525">Enhanced diagnostic logs</span></span>

<span data-ttu-id="6284d-526">Moduł ASP.NET Core można skonfigurować w celu udostępnienia dzienników diagnostyki rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="6284d-526">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="6284d-527">Dodaj `<handlerSettings>` elementu `<aspNetCore>` element *web.config*. Ustawienie `debugLevel` do `TRACE` udostępnia pozwala uzyskać większą wierność informacje diagnostyczne:</span><span class="sxs-lookup"><span data-stu-id="6284d-527">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="6284d-528">Foldery w ścieżce podane do wartości `<handlerSetting>` (*dzienniki* w powyższym przykładzie) nie są automatycznie tworzone przez moduł i powinny być wcześniej dostępne we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="6284d-528">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="6284d-529">Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="6284d-529">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6284d-530">Poziom debugowania (`debugLevel`) wartości mogą obejmować zarówno na poziomie, jak i lokalizację.</span><span class="sxs-lookup"><span data-stu-id="6284d-530">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="6284d-531">Poziomy (w kolejności od najmniejszej do najbardziej szczegółowy):</span><span class="sxs-lookup"><span data-stu-id="6284d-531">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="6284d-532">BŁĄD</span><span class="sxs-lookup"><span data-stu-id="6284d-532">ERROR</span></span>
* <span data-ttu-id="6284d-533">OSTRZEŻENIE</span><span class="sxs-lookup"><span data-stu-id="6284d-533">WARNING</span></span>
* <span data-ttu-id="6284d-534">INFORMACJE O</span><span class="sxs-lookup"><span data-stu-id="6284d-534">INFO</span></span>
* <span data-ttu-id="6284d-535">TRACE</span><span class="sxs-lookup"><span data-stu-id="6284d-535">TRACE</span></span>

<span data-ttu-id="6284d-536">Lokalizacje (wiele lokalizacji są dozwolone):</span><span class="sxs-lookup"><span data-stu-id="6284d-536">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="6284d-537">KONSOLA</span><span class="sxs-lookup"><span data-stu-id="6284d-537">CONSOLE</span></span>
* <span data-ttu-id="6284d-538">DZIENNIK ZDARZEŃ</span><span class="sxs-lookup"><span data-stu-id="6284d-538">EVENTLOG</span></span>
* <span data-ttu-id="6284d-539">PLIK</span><span class="sxs-lookup"><span data-stu-id="6284d-539">FILE</span></span>

<span data-ttu-id="6284d-540">Ustawienia obsługi można również podać za pośrednictwem zmienne środowiskowe:</span><span class="sxs-lookup"><span data-stu-id="6284d-540">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="6284d-541">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ścieżka do pliku dziennika debugowania.</span><span class="sxs-lookup"><span data-stu-id="6284d-541">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="6284d-542">(Domyślnie: *czy aspnetcore*)</span><span class="sxs-lookup"><span data-stu-id="6284d-542">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="6284d-543">`ASPNETCORE_MODULE_DEBUG` &ndash; Ustawienie poziomie debugowania.</span><span class="sxs-lookup"><span data-stu-id="6284d-543">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="6284d-544">Czy **nie** Pozostaw włączone we wdrożeniu na dłużej, niż jest to wymagane, aby rozwiązać problem rejestrowanie debugowania.</span><span class="sxs-lookup"><span data-stu-id="6284d-544">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="6284d-545">Rozmiar dziennika nie jest ograniczona.</span><span class="sxs-lookup"><span data-stu-id="6284d-545">The size of the log isn't limited.</span></span> <span data-ttu-id="6284d-546">Pozostawienie dziennik debugowania włączone może wyczerpać dostępne miejsce na dysku i awarii serwera lub usługi app service.</span><span class="sxs-lookup"><span data-stu-id="6284d-546">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="6284d-547">Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-547">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="6284d-548">Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token</span><span class="sxs-lookup"><span data-stu-id="6284d-548">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="6284d-549">*Dotyczy tylko hostingu poza procesem.*</span><span class="sxs-lookup"><span data-stu-id="6284d-549">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="6284d-550">Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6284d-550">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="6284d-551">Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="6284d-551">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="6284d-552">Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="6284d-552">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="6284d-553">Parowania token jest utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="6284d-553">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="6284d-554">Token parowania jest również ustawiona w nagłówku (`MS-ASPNETCORE-TOKEN`) na każde żądanie serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="6284d-554">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="6284d-555">Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="6284d-555">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="6284d-556">Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="6284d-556">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="6284d-557">Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="6284d-557">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="6284d-558">Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="6284d-558">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="6284d-559">Moduł ASP.NET Core z programem IIS konfigurację udostępnioną</span><span class="sxs-lookup"><span data-stu-id="6284d-559">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="6284d-560">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** .</span><span class="sxs-lookup"><span data-stu-id="6284d-560">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="6284d-561">Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w pliku *ApplicationHost. config* na udział.</span><span class="sxs-lookup"><span data-stu-id="6284d-561">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="6284d-562">W przypadku korzystania z konfiguracji udostępnionej usług IIS na tym samym komputerze, na którym znajduje się instalacja usług IIS, uruchom Instalatora pakietu ASP.NET Core hostowania z parametrem `OPT_NO_SHARED_CONFIG_CHECK` ustawionym na `1`:</span><span class="sxs-lookup"><span data-stu-id="6284d-562">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="6284d-563">Jeśli ścieżka do konfiguracji udostępnionej nie znajduje się na tym samym komputerze, na którym znajduje się instalacja usług IIS, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="6284d-563">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="6284d-564">Wyłącz konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="6284d-564">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="6284d-565">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="6284d-565">Run the installer.</span></span>
1. <span data-ttu-id="6284d-566">Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.</span><span class="sxs-lookup"><span data-stu-id="6284d-566">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="6284d-567">Ponownie włączyć konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="6284d-567">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="6284d-568">Wersja modułu oraz Dzienniki Instalatora pakietu hostingu</span><span class="sxs-lookup"><span data-stu-id="6284d-568">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="6284d-569">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="6284d-569">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="6284d-570">W systemie hostingu, przejdź do *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="6284d-570">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="6284d-571">Znajdź *aspnetcore.dll* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-571">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="6284d-572">Kliknij prawym przyciskiem myszy plik i wybierz **właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="6284d-572">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="6284d-573">Wybierz **szczegóły** kartę. **Wersja pliku** i **wersji produktu** reprezentują zainstalowanej wersji modułu.</span><span class="sxs-lookup"><span data-stu-id="6284d-573">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="6284d-574">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się w *C:\\użytkowników\\% UserName %\\AppData\\lokalnego\\Temp*. Plik *dd_DotNetCoreWinSvrHosting__\<sygnatura czasowa > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="6284d-574">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="6284d-575">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="6284d-575">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="6284d-576">Moduł</span><span class="sxs-lookup"><span data-stu-id="6284d-576">Module</span></span>

<span data-ttu-id="6284d-577">**Usługi IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6284d-577">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="6284d-578">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-578">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="6284d-579">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-579">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="6284d-580">%ProgramFiles%\IIS\Asp.NET core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-580">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="6284d-581">% ProgramFiles (x86) %\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-581">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="6284d-582">**Usługi IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6284d-582">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="6284d-583">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-583">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="6284d-584">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-584">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="6284d-585">%ProgramFiles%\iis Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-585">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="6284d-586">% ProgramFiles (x86) %\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-586">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="6284d-587">Schemat</span><span class="sxs-lookup"><span data-stu-id="6284d-587">Schema</span></span>

<span data-ttu-id="6284d-588">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6284d-588">**IIS**</span></span>

* <span data-ttu-id="6284d-589">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6284d-589">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="6284d-590">%Windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="6284d-590">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="6284d-591">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6284d-591">**IIS Express**</span></span>

* <span data-ttu-id="6284d-592">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6284d-592">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="6284d-593">%ProgramFiles%\iis Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="6284d-593">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="6284d-594">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="6284d-594">Configuration</span></span>

<span data-ttu-id="6284d-595">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6284d-595">**IIS**</span></span>

* <span data-ttu-id="6284d-596">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6284d-596">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="6284d-597">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6284d-597">**IIS Express**</span></span>

* <span data-ttu-id="6284d-598">Visual Studio: {Aplikacja główna} \\. vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6284d-598">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="6284d-599">Interfejs wiersza polecenia *iisexpress. exe* :%USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="6284d-599">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="6284d-600">Pliki można znaleźć, wyszukując *aspnetcore* w *applicationHost.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-600">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6284d-601">Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS do przesyłania dalej żądań sieci Web do zaplecza ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-601">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="6284d-602">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="6284d-602">Supported Windows versions:</span></span>

* <span data-ttu-id="6284d-603">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="6284d-603">Windows 7 or later</span></span>
* <span data-ttu-id="6284d-604">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="6284d-604">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="6284d-605">Moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6284d-605">The module only works with Kestrel.</span></span> <span data-ttu-id="6284d-606">Moduł jest niezgodny z [protokołem HTTP. sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6284d-606">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="6284d-607">Ponieważ ASP.NET Core aplikacje działają w procesie innym niż proces roboczy usług IIS, moduł obsługuje również zarządzanie procesami.</span><span class="sxs-lookup"><span data-stu-id="6284d-607">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="6284d-608">Moduł uruchamia proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie zostanie odebrane i ponownie uruchomiony, jeśli wystąpi awaria.</span><span class="sxs-lookup"><span data-stu-id="6284d-608">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="6284d-609">Jest to zasadniczo takie samo zachowanie jak w przypadku aplikacji ASP.NET 4. x, które działają w procesie w usługach IIS, które są zarządzane przez [usługę aktywacji procesów systemu Windows (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="6284d-609">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="6284d-610">Na poniższym diagramie przedstawiono relację między usługami IIS, modułem ASP.NET Core i aplikacją:</span><span class="sxs-lookup"><span data-stu-id="6284d-610">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="6284d-612">Żądania docierają do sieci Web do sterownika HTTP. sys trybu jądra.</span><span class="sxs-lookup"><span data-stu-id="6284d-612">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="6284d-613">Sterownik kieruje żądania do usług IIS na skonfigurowanym porcie witryny sieci Web, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6284d-613">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="6284d-614">Moduł przekazuje żądania do Kestrel na losowo wybranym porcie dla aplikacji, która nie jest portem 80 lub 443.</span><span class="sxs-lookup"><span data-stu-id="6284d-614">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="6284d-615">Moduł określa port za pośrednictwem zmiennej środowiskowej podczas uruchamiania, a [oprogramowanie pośredniczące integracji usług IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) konfiguruje serwer do nasłuchiwania na `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="6284d-615">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="6284d-616">Dodatkowe sprawdzenia są wykonywane, a żądania, które nie pochodzą z modułu, są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="6284d-616">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="6284d-617">Moduł nie obsługuje przekazywania HTTPS, dlatego żądania są przekazywane przez protokół HTTP nawet wtedy, gdy są odbierane przez usługę IIS przez protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6284d-617">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="6284d-618">Po podaniu przez Kestrel żądania z modułu żądanie jest wypychane do potoku ASP.NET Core pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="6284d-618">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="6284d-619">Potok oprogramowania pośredniczącego obsługuje żądanie i przekazuje go jako wystąpienie `HttpContext` do logiki aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-619">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="6284d-620">Oprogramowanie pośredniczące dodane przez integrację usług IIS aktualizuje schemat, zdalny adres IP i pathbase, aby można było przesłać żądanie do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6284d-620">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="6284d-621">Odpowiedź aplikacji jest przesyłana z powrotem do usług IIS, która wypycha ją z powrotem do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="6284d-621">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="6284d-622">Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych.</span><span class="sxs-lookup"><span data-stu-id="6284d-622">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="6284d-623">Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="6284d-623">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="6284d-624">Moduł ASP.NET Core może również:</span><span class="sxs-lookup"><span data-stu-id="6284d-624">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="6284d-625">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="6284d-625">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="6284d-626">Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.</span><span class="sxs-lookup"><span data-stu-id="6284d-626">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="6284d-627">Przekazuj tokeny uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="6284d-627">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="6284d-628">Jak zainstalować moduł ASP.NET Core i korzystać z niego</span><span class="sxs-lookup"><span data-stu-id="6284d-628">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="6284d-629">Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="6284d-629">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="6284d-630">Konfiguracja z pliku web.config</span><span class="sxs-lookup"><span data-stu-id="6284d-630">Configuration with web.config</span></span>

<span data-ttu-id="6284d-631">Skonfigurowano modułu ASP.NET Core `aspNetCore` części `system.webServer` węzeł w tej witrynie *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-631">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="6284d-632">Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależny od struktury](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje modułu ASP.NET Core do obsługi żądań w lokacji:</span><span class="sxs-lookup"><span data-stu-id="6284d-632">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="6284d-633">Następujące *web.config* została opublikowana na potrzeby [niezależna wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="6284d-633">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="6284d-634">Gdy aplikacja jest wdrażana na [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest równa `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="6284d-634">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="6284d-635">Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.</span><span class="sxs-lookup"><span data-stu-id="6284d-635">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="6284d-636">Aby uzyskać informacji na temat konfigurowania aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="6284d-636">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="6284d-637">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="6284d-637">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="6284d-638">Atrybut</span><span class="sxs-lookup"><span data-stu-id="6284d-638">Attribute</span></span> | <span data-ttu-id="6284d-639">Opis</span><span class="sxs-lookup"><span data-stu-id="6284d-639">Description</span></span> | <span data-ttu-id="6284d-640">Domyślny</span><span class="sxs-lookup"><span data-stu-id="6284d-640">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6284d-641">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="6284d-641">Optional string attribute.</span></span></p><p><span data-ttu-id="6284d-642">Argumenty do pliku wykonywalnego, określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6284d-642">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6284d-643">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="6284d-643">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6284d-644">W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="6284d-644">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6284d-645">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="6284d-645">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6284d-646">W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="6284d-646">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6284d-647">Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="6284d-647">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="6284d-648">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6284d-648">Optional integer attribute.</span></span></p><p><span data-ttu-id="6284d-649">Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</span><span class="sxs-lookup"><span data-stu-id="6284d-649">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="6284d-650">Ustawienie `processesPerApplication` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="6284d-650">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="6284d-651">Ten atrybut zostanie usunięty w przyszłych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="6284d-651">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="6284d-652">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="6284d-652">Default: `1`</span></span><br><span data-ttu-id="6284d-653">Minimalna: `1`</span><span class="sxs-lookup"><span data-stu-id="6284d-653">Min: `1`</span></span><br><span data-ttu-id="6284d-654">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="6284d-654">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="6284d-655">Atrybut wymagany ciąg.</span><span class="sxs-lookup"><span data-stu-id="6284d-655">Required string attribute.</span></span></p><p><span data-ttu-id="6284d-656">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="6284d-656">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6284d-657">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="6284d-657">Relative paths are supported.</span></span> <span data-ttu-id="6284d-658">Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="6284d-658">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6284d-659">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6284d-659">Optional integer attribute.</span></span></p><p><span data-ttu-id="6284d-660">Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę.</span><span class="sxs-lookup"><span data-stu-id="6284d-660">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6284d-661">W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</span><span class="sxs-lookup"><span data-stu-id="6284d-661">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="6284d-662">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="6284d-662">Default: `10`</span></span><br><span data-ttu-id="6284d-663">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="6284d-663">Min: `0`</span></span><br><span data-ttu-id="6284d-664">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="6284d-664">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6284d-665">Atrybut opcjonalny przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="6284d-665">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6284d-666">Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="6284d-666">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6284d-667">W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="6284d-667">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="6284d-668">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6284d-668">Default: `00:02:00`</span></span><br><span data-ttu-id="6284d-669">Minimalna: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6284d-669">Min: `00:00:00`</span></span><br><span data-ttu-id="6284d-670">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6284d-670">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6284d-671">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6284d-671">Optional integer attribute.</span></span></p><p><span data-ttu-id="6284d-672">Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="6284d-672">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6284d-673">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="6284d-673">Default: `10`</span></span><br><span data-ttu-id="6284d-674">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="6284d-674">Min: `0`</span></span><br><span data-ttu-id="6284d-675">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="6284d-675">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6284d-676">Atrybut opcjonalną liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="6284d-676">Optional integer attribute.</span></span></p><p><span data-ttu-id="6284d-677">Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie.</span><span class="sxs-lookup"><span data-stu-id="6284d-677">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6284d-678">Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="6284d-678">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6284d-679">Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</span><span class="sxs-lookup"><span data-stu-id="6284d-679">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="6284d-680">Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="6284d-680">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="6284d-681">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="6284d-681">Default: `120`</span></span><br><span data-ttu-id="6284d-682">Minimalna: `0`</span><span class="sxs-lookup"><span data-stu-id="6284d-682">Min: `0`</span></span><br><span data-ttu-id="6284d-683">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="6284d-683">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6284d-684">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="6284d-684">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6284d-685">W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6284d-685">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6284d-686">Atrybut opcjonalny ciąg.</span><span class="sxs-lookup"><span data-stu-id="6284d-686">Optional string attribute.</span></span></p><p><span data-ttu-id="6284d-687">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="6284d-687">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6284d-688">Są ścieżki względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="6284d-688">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6284d-689">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych.</span><span class="sxs-lookup"><span data-stu-id="6284d-689">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6284d-690">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="6284d-690">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="6284d-691">Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku ( *.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="6284d-691">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6284d-692">Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="6284d-692">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="6284d-693">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="6284d-693">Setting environment variables</span></span>

<span data-ttu-id="6284d-694">Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6284d-694">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="6284d-695">Określ zmienną środowiskową `<environmentVariable>` element podrzędny elementu `<environmentVariables>` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="6284d-695">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="6284d-696">Zmienne środowiskowe ustawione w tej sekcji powodują konflikt z systemowymi zmiennymi środowiskowymi ustawionymi z tą samą nazwą.</span><span class="sxs-lookup"><span data-stu-id="6284d-696">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="6284d-697">Jeśli zmienna środowiskowa jest ustawiona zarówno w pliku *Web. config* , jak i na poziomie systemu w systemie Windows, wartość z pliku *Web. config* zostanie dołączona do wartości zmiennej środowiskowej systemowej (na przykład `ASPNETCORE_ENVIRONMENT: Development;Development`), która uniemożliwia aplikacji Uruchamianie.</span><span class="sxs-lookup"><span data-stu-id="6284d-697">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

<span data-ttu-id="6284d-698">W poniższym przykładzie ustawiono dwóch zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="6284d-698">The following example sets two environment variables.</span></span> <span data-ttu-id="6284d-699">`ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji `Development`.</span><span class="sxs-lookup"><span data-stu-id="6284d-699">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="6284d-700">Deweloper może tymczasowo ustawić tę wartość w *web.config* pliku, aby wymusić [stronie wyjątków deweloperów](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek.</span><span class="sxs-lookup"><span data-stu-id="6284d-700">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="6284d-701">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, gdzie deweloper ma napisany kod, który odczytuje wartość przy uruchamianiu w celu utworzenia ścieżki do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-701">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!WARNING]
> <span data-ttu-id="6284d-702">Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej, aby `Development` przejściowym i testowania serwerów, które nie są dostępne do niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="6284d-702">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="6284d-703">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="6284d-703">app_offline.htm</span></span>

<span data-ttu-id="6284d-704">Jeśli plik o nazwie *app_offline.htm* wykryte w katalogu głównym aplikacji, modułu ASP.NET Core próbuje łagodne zamykanie aplikacji i zatrzymanie przetwarzania przychodzących żądań.</span><span class="sxs-lookup"><span data-stu-id="6284d-704">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="6284d-705">Jeśli aplikacja jest nadal uruchomione po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, modułu ASP.NET Core to złe rozwiązanie uruchomionego procesu.</span><span class="sxs-lookup"><span data-stu-id="6284d-705">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="6284d-706">Gdy *app_offline.htm* występuje pliku modułu ASP.NET Core ma odpowiadać na żądania, wysyłając ponownie zawartość *app_offline.htm* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-706">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="6284d-707">Gdy *app_offline.htm* plik jest usuwany, kolejne żądanie uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="6284d-707">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="6284d-708">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="6284d-708">Start-up error page</span></span>

<span data-ttu-id="6284d-709">Jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="6284d-709">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="6284d-710">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 502 usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6284d-710">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="6284d-711">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="6284d-711">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 strona kodowa stan niepowodzenia procesu](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="6284d-713">Tworzenia dziennika i Przekierowanie</span><span class="sxs-lookup"><span data-stu-id="6284d-713">Log creation and redirection</span></span>

<span data-ttu-id="6284d-714">Modułu ASP.NET Core przekierowuje dane wyjściowe stdout i stderr konsoli do dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu są ustawione.</span><span class="sxs-lookup"><span data-stu-id="6284d-714">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="6284d-715">Wszystkie foldery w ścieżce `stdoutLogFile` są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="6284d-715">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="6284d-716">Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="6284d-716">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6284d-717">Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="6284d-717">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="6284d-718">Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="6284d-718">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="6284d-719">Przy użyciu dziennika stdout jest zalecane tylko na potrzeby rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-719">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="6284d-720">Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6284d-720">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="6284d-721">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="6284d-721">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="6284d-722">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="6284d-722">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="6284d-723">Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="6284d-723">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="6284d-724">Przez dodanie sygnatury czasowej, identyfikator procesu i rozszerzenie pliku składa się nazwa pliku dziennika ( *.log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="6284d-724">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="6284d-725">Jeśli `stdoutLogFile` ścieżki kończy się *stdout*, dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzono 19:42:32 2/5/2018 o nazwie pliku *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="6284d-725">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="6284d-726">Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie strumienia stdout dla aplikacji hostowanej w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="6284d-726">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="6284d-727">Ścieżka lokalna lub ścieżkę do udziału sieciowego jest możliwa do logowania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="6284d-727">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="6284d-728">Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.</span><span class="sxs-lookup"><span data-stu-id="6284d-728">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="6284d-729">Foldery w ścieżce podane do wartości `<handlerSetting>` (*dzienniki* w powyższym przykładzie) nie są automatycznie tworzone przez moduł i powinny być wcześniej dostępne we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="6284d-729">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="6284d-730">Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="6284d-730">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6284d-731">Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-731">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="6284d-732">Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token</span><span class="sxs-lookup"><span data-stu-id="6284d-732">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="6284d-733">Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6284d-733">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="6284d-734">Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="6284d-734">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="6284d-735">Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="6284d-735">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="6284d-736">Parowania token jest utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="6284d-736">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="6284d-737">Token parowania jest również ustawiona w nagłówku (`MS-ASPNETCORE-TOKEN`) na każde żądanie serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="6284d-737">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="6284d-738">Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="6284d-738">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="6284d-739">Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="6284d-739">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="6284d-740">Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="6284d-740">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="6284d-741">Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="6284d-741">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="6284d-742">Moduł ASP.NET Core z programem IIS konfigurację udostępnioną</span><span class="sxs-lookup"><span data-stu-id="6284d-742">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="6284d-743">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** .</span><span class="sxs-lookup"><span data-stu-id="6284d-743">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="6284d-744">Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w pliku *ApplicationHost. config* na udział.</span><span class="sxs-lookup"><span data-stu-id="6284d-744">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="6284d-745">Korzystając z konfiguracji udostępnionej usług IIS, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="6284d-745">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="6284d-746">Wyłącz konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="6284d-746">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="6284d-747">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="6284d-747">Run the installer.</span></span>
1. <span data-ttu-id="6284d-748">Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.</span><span class="sxs-lookup"><span data-stu-id="6284d-748">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="6284d-749">Ponownie włączyć konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="6284d-749">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="6284d-750">Wersja modułu oraz Dzienniki Instalatora pakietu hostingu</span><span class="sxs-lookup"><span data-stu-id="6284d-750">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="6284d-751">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="6284d-751">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="6284d-752">W systemie hostingu, przejdź do *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="6284d-752">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="6284d-753">Znajdź *aspnetcore.dll* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-753">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="6284d-754">Kliknij prawym przyciskiem myszy plik i wybierz **właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="6284d-754">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="6284d-755">Wybierz **szczegóły** kartę. **Wersja pliku** i **wersji produktu** reprezentują zainstalowanej wersji modułu.</span><span class="sxs-lookup"><span data-stu-id="6284d-755">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="6284d-756">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się w *C:\\użytkowników\\% UserName %\\AppData\\lokalnego\\Temp*. Plik *dd_DotNetCoreWinSvrHosting__\<sygnatura czasowa > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="6284d-756">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="6284d-757">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="6284d-757">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="6284d-758">Moduł</span><span class="sxs-lookup"><span data-stu-id="6284d-758">Module</span></span>

<span data-ttu-id="6284d-759">**Usługi IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6284d-759">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="6284d-760">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-760">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="6284d-761">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-761">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="6284d-762">**Usługi IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="6284d-762">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="6284d-763">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-763">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="6284d-764">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6284d-764">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="6284d-765">Schemat</span><span class="sxs-lookup"><span data-stu-id="6284d-765">Schema</span></span>

<span data-ttu-id="6284d-766">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6284d-766">**IIS**</span></span>

* <span data-ttu-id="6284d-767">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6284d-767">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="6284d-768">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6284d-768">**IIS Express**</span></span>

* <span data-ttu-id="6284d-769">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6284d-769">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="6284d-770">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="6284d-770">Configuration</span></span>

<span data-ttu-id="6284d-771">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6284d-771">**IIS**</span></span>

* <span data-ttu-id="6284d-772">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6284d-772">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="6284d-773">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6284d-773">**IIS Express**</span></span>

* <span data-ttu-id="6284d-774">Visual Studio: {Aplikacja główna} \\. vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6284d-774">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="6284d-775">Interfejs wiersza polecenia *iisexpress. exe* :%USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="6284d-775">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="6284d-776">Pliki można znaleźć, wyszukując *aspnetcore* w *applicationHost.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="6284d-776">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6284d-777">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6284d-777">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="6284d-778">Repozytorium GitHub modułu ASP.NET Core (Źródło odwołania)</span><span class="sxs-lookup"><span data-stu-id="6284d-778">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
