---
title: Moduł ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować moduł ASP.NET Core na potrzeby hostowania aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/13/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 917ee462a8f9592120685b53d059a661cb4a7452
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333897"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="a0d0f-103">Moduł ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0d0f-103">ASP.NET Core Module</span></span>

<span data-ttu-id="a0d0f-104">Autorzy [Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Krzysztof Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik)i [Luke](https://github.com/guardrex) Latham</span><span class="sxs-lookup"><span data-stu-id="a0d0f-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a0d0f-105">Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="a0d0f-106">Hostowanie aplikacji ASP.NET Core w procesie roboczym usług IIS (`w3wp.exe`), nazywanym [modelem hostingu w procesie](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="a0d0f-107">Przekazuj żądania sieci Web do zaplecza ASP.NET Core aplikacji, na której uruchomiono [serwer Kestrel](xref:fundamentals/servers/kestrel), nazywany [modelem hostingu poza procesem](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="a0d0f-108">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-108">Supported Windows versions:</span></span>

* <span data-ttu-id="a0d0f-109">System Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a0d0f-109">Windows 7 or later</span></span>
* <span data-ttu-id="a0d0f-110">System Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a0d0f-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="a0d0f-111">W przypadku hostingu w procesie moduł używa implementacji serwera w procesie dla usług IIS o nazwie serwer HTTP IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="a0d0f-112">Podczas hostingu poza procesem moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="a0d0f-113">Moduł nie działa w przypadku [protokołu HTTP. sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-113">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="a0d0f-114">Modele hostingu</span><span class="sxs-lookup"><span data-stu-id="a0d0f-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="a0d0f-115">Model hostingu w procesie</span><span class="sxs-lookup"><span data-stu-id="a0d0f-115">In-process hosting model</span></span>

<span data-ttu-id="a0d0f-116">Domyślnie ASP.NET Core aplikacje do modelu hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-116">ASP.NET Core apps default to the in-process hosting model.</span></span>

<span data-ttu-id="a0d0f-117">Następujące cechy są stosowane podczas hostingu w procesie:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-117">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="a0d0f-118">Serwer HTTP IIS (`IISHttpServer`) jest używany zamiast serwera [Kestrel](xref:fundamentals/servers/kestrel) .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-118">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="a0d0f-119">W przypadku wywołań [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> do:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-119">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="a0d0f-120">Zarejestruj `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-120">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="a0d0f-121">Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-121">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="a0d0f-122">Skonfiguruj hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-122">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="a0d0f-123">[Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-123">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="a0d0f-124">Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-124">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="a0d0f-125">Użyj jednej puli aplikacji na aplikację.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-125">Use one app pool per app.</span></span>

* <span data-ttu-id="a0d0f-126">W przypadku korzystania z [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręcznego umieszczania [pliku app_offline. htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files)aplikacja może nie zostać wyłączona natychmiast, jeśli istnieje otwarte połączenie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-126">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="a0d0f-127">Na przykład połączenie protokołu WebSocket może opóźnić zamykanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-127">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="a0d0f-128">Architektura (bitową) aplikacji i zainstalowane środowisko uruchomieniowe (x64 lub x86) musi być zgodna z architekturą puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-128">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="a0d0f-129">Wykryto rozłączenia klientów.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-129">Client disconnects are detected.</span></span> <span data-ttu-id="a0d0f-130">Token anulowania [HttpContext. RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) został anulowany po rozłączeniu klienta.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="a0d0f-131">W ASP.NET Core 2.2.1 lub wcześniejszym, <xref:System.IO.Directory.GetCurrentDirectory*> zwraca katalog procesów roboczych procesu uruchomionego przez usługi IIS, a nie katalog aplikacji (na przykład *C:\Windows\System32\inetsrv* for *w3wp. exe*).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-131">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="a0d0f-132">Przykładowy kod, który ustawia bieżący katalog aplikacji, znajduje się w [klasie CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="a0d0f-133">Wywołaj metodę `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="a0d0f-134">Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> zapewniają katalog aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="a0d0f-135">Podczas hostingu w procesie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wywoływana wewnętrznie w celu zainicjowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-135">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="a0d0f-136">W związku z tym, implementacja <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> używana do przekształcania oświadczeń po każdym uwierzytelnieniu nie jest domyślnie aktywowana.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-136">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="a0d0f-137">Podczas przekształcania oświadczeń z implementacją <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> Wywołaj <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, aby dodać usługi uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-137">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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
  
  * <span data-ttu-id="a0d0f-138">[Wdrożenia pakietów sieci Web (jednego pliku)](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-138">[Web Package (single-file) deployments](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) aren't supported.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="a0d0f-139">Model hostingu poza procesem</span><span class="sxs-lookup"><span data-stu-id="a0d0f-139">Out-of-process hosting model</span></span>

<span data-ttu-id="a0d0f-140">Aby skonfigurować aplikację do hostingu poza procesem, ustaw wartość właściwości `<AspNetCoreHostingModel>` na `OutOfProcess` w pliku projektu ( *. csproj*):</span><span class="sxs-lookup"><span data-stu-id="a0d0f-140">To configure an app for out-of-process hosting, set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` in the project file (*.csproj*):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="a0d0f-141">Hosting w procesie jest ustawiany z `InProcess`, czyli wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-141">In-process hosting is set with `InProcess`, which is the default value.</span></span>

<span data-ttu-id="a0d0f-142">Serwer [Kestrel](xref:fundamentals/servers/kestrel) jest używany zamiast serwera http IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-142">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="a0d0f-143">W przypadku pozaprocesowych wywołań [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> do:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-143">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="a0d0f-144">Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-144">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="a0d0f-145">Skonfiguruj hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-145">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="a0d0f-146">Zmiany modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="a0d0f-146">Hosting model changes</span></span>

<span data-ttu-id="a0d0f-147">Jeśli ustawienie `hostingModel` zostanie zmienione w pliku *Web. config* (wyjaśniono w sekcji [Konfiguracja z plikiem Web. config](#configuration-with-webconfig) ), moduł odtwarza proces roboczy dla usług IIS.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-147">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="a0d0f-148">W przypadku IIS Express moduł nie odtwarza procesu roboczego, ale zamiast tego wyzwala bezpieczne zamknięcie bieżącego procesu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-148">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="a0d0f-149">Następne żądanie do aplikacji powoduje zduplikowanie nowego procesu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-149">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="a0d0f-150">Nazwa procesu</span><span class="sxs-lookup"><span data-stu-id="a0d0f-150">Process name</span></span>

<span data-ttu-id="a0d0f-151">Raporty `Process.GetCurrentProcess().ProcessName` `w3wp` @ no__t-2 @ no__t-3 (wewnątrzprocesowe-Process) lub `dotnet` (pozaprocesowe).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-151">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="a0d0f-152">Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-152">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="a0d0f-153">Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-153">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="a0d0f-154">Moduł ASP.NET Core może również:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-154">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="a0d0f-155">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-155">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="a0d0f-156">Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-156">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="a0d0f-157">Przekazuj tokeny uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-157">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="a0d0f-158">Jak zainstalować moduł ASP.NET Core i korzystać z niego</span><span class="sxs-lookup"><span data-stu-id="a0d0f-158">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="a0d0f-159">Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-159">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="a0d0f-160">Konfiguracja z pliku Web. config</span><span class="sxs-lookup"><span data-stu-id="a0d0f-160">Configuration with web.config</span></span>

<span data-ttu-id="a0d0f-161">Moduł ASP.NET Core jest skonfigurowany z sekcją `aspNetCore` węzła `system.webServer` w pliku *Web. config* witryny.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-161">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="a0d0f-162">Następujący plik *Web. config* jest publikowany dla [wdrożenia zależnego od platformy](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje moduł ASP.NET Core do obsługi żądań lokacji:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-162">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="a0d0f-163">Następująca *plik Web. config* jest publikowana dla [samodzielnego wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="a0d0f-163">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="a0d0f-164">Właściwość <xref:System.Configuration.SectionInformation.InheritInChildApplications*> ma wartość `false` w celu wskazania, że ustawienia określone w elemencie [> \<location](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-164">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="a0d0f-165">Po wdrożeniu aplikacji do [Azure App Service](https://azure.microsoft.com/services/app-service/)ścieżka `stdoutLogFile` jest ustawiona na `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-165">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="a0d0f-166">Ścieżka zapisuje dzienniki stdout do folderu *LogFiles* , który jest lokalizacją automatycznie utworzoną przez usługę.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-166">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="a0d0f-167">Aby uzyskać informacje na temat konfiguracji aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-167">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="a0d0f-168">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="a0d0f-168">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="a0d0f-169">Atrybut</span><span class="sxs-lookup"><span data-stu-id="a0d0f-169">Attribute</span></span> | <span data-ttu-id="a0d0f-170">Opis</span><span class="sxs-lookup"><span data-stu-id="a0d0f-170">Description</span></span> | <span data-ttu-id="a0d0f-171">Domyślny</span><span class="sxs-lookup"><span data-stu-id="a0d0f-171">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="a0d0f-172">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-172">Optional string attribute.</span></span></p><p><span data-ttu-id="a0d0f-173">Argumenty do pliku wykonywalnego określonego w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-173">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="a0d0f-174">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-174">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a0d0f-175">W przypadku wartości true strona **błędu 502,5 procesu** jest pomijana, a strona kodowa stanu 502 skonfigurowana w *pliku Web. config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-175">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="a0d0f-176">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-176">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a0d0f-177">Jeśli wartość jest równa true, token jest przekazywany do procesu podrzędnego, który nasłuchuje na% ASPNETCORE_PORT% jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" dla żądania.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-177">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="a0d0f-178">Jest odpowiedzialny za ten proces, aby wywołać metodę CloseHandle na tym tokenie na żądanie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-178">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="a0d0f-179">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-179">Optional string attribute.</span></span></p><p><span data-ttu-id="a0d0f-180">Określa model hostingu jako proces (`InProcess`) lub out-of-Process (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-180">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `InProcess` |
| `processesPerApplication` | <p><span data-ttu-id="a0d0f-181">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-181">Optional integer attribute.</span></span></p><p><span data-ttu-id="a0d0f-182">Określa liczbę wystąpień procesu określonego w ustawieniu **processPath** , które może być przypadające na aplikację.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-182">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="a0d0f-183">@no__t 0For hosting w procesie, wartość jest ograniczona do `1`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-183">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="a0d0f-184">Ustawienie `processesPerApplication` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-184">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="a0d0f-185">Ten atrybut zostanie usunięty w przyszłych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-185">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="a0d0f-186">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-186">Default: `1`</span></span><br><span data-ttu-id="a0d0f-187">Minimum: `1`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-187">Min: `1`</span></span><br><span data-ttu-id="a0d0f-188">Maks.: `100` @ no__t-1</span><span class="sxs-lookup"><span data-stu-id="a0d0f-188">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="a0d0f-189">Wymagany atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-189">Required string attribute.</span></span></p><p><span data-ttu-id="a0d0f-190">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-190">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="a0d0f-191">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-191">Relative paths are supported.</span></span> <span data-ttu-id="a0d0f-192">Jeśli ścieżka rozpoczyna się od `.`, ścieżka jest uznawana za względną względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-192">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="a0d0f-193">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-193">Optional integer attribute.</span></span></p><p><span data-ttu-id="a0d0f-194">Określa, ile razy proces określony w **processPath** może ulec awarii na minutę.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-194">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="a0d0f-195">W przypadku przekroczenia tego limitu moduł przestaje uruchomić proces przez pozostałą część minuty.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-195">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="a0d0f-196">Nieobsługiwane w przypadku hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-196">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="a0d0f-197">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-197">Default: `10`</span></span><br><span data-ttu-id="a0d0f-198">Minimum: `0`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-198">Min: `0`</span></span><br><span data-ttu-id="a0d0f-199">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-199">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="a0d0f-200">Opcjonalny atrybut TimeSpan.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-200">Optional timespan attribute.</span></span></p><p><span data-ttu-id="a0d0f-201">Określa czas, przez który moduł ASP.NET Core czeka na odpowiedź z procesu nasłuchiwania na% ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-201">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="a0d0f-202">W wersjach modułu ASP.NET Core, który został dostarczony z wersją ASP.NET Core 2,1 lub nowszą, `requestTimeout` jest określony w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-202">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="a0d0f-203">Nie dotyczy hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-203">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="a0d0f-204">W przypadku hostingu w procesie moduł czeka na aplikację w celu przetworzenia żądania.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-204">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="a0d0f-205">Prawidłowe wartości segmentów minut i sekund ciągu mieszczą się w zakresie 0-59.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-205">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="a0d0f-206">Użycie **60** w wartości minut lub sekund skutkuje *błędem wewnętrznego serwera 500*.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-206">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="a0d0f-207">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-207">Default: `00:02:00`</span></span><br><span data-ttu-id="a0d0f-208">Minimum: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-208">Min: `00:00:00`</span></span><br><span data-ttu-id="a0d0f-209">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-209">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="a0d0f-210">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-210">Optional integer attribute.</span></span></p><p><span data-ttu-id="a0d0f-211">Czas w sekundach, przez który moduł czeka, aż plik wykonywalny zostanie bezpiecznie zamknięty po wykryciu pliku *app_offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-211">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="a0d0f-212">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-212">Default: `10`</span></span><br><span data-ttu-id="a0d0f-213">Minimum: `0`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-213">Min: `0`</span></span><br><span data-ttu-id="a0d0f-214">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-214">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="a0d0f-215">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-215">Optional integer attribute.</span></span></p><p><span data-ttu-id="a0d0f-216">Czas w sekundach, przez który moduł czeka, aż plik wykonywalny uruchomi proces nasłuchujący na porcie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-216">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="a0d0f-217">Jeśli ten limit czasu zostanie przekroczony, moduł zakasuje proces.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-217">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="a0d0f-218">Moduł podejmuje próbę ponownego uruchomienia procesu, gdy odbierze nowe żądanie i kontynuuje ponowne uruchomienie procesu na kolejnych żądaniach przychodzących, chyba że aplikacja nie będzie mogła uruchomić **rapidFailsPerMinute** liczbę razy w ostatniej minucie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-218">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="a0d0f-219">Wartość 0 (zero) **nie** jest uważana za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-219">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="a0d0f-220">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-220">Default: `120`</span></span><br><span data-ttu-id="a0d0f-221">Minimum: `0`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-221">Min: `0`</span></span><br><span data-ttu-id="a0d0f-222">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-222">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="a0d0f-223">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-223">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a0d0f-224">Jeśli wartość jest równa true, **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-224">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="a0d0f-225">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-225">Optional string attribute.</span></span></p><p><span data-ttu-id="a0d0f-226">Określa względną lub bezwzględną ścieżkę do pliku, dla którego jest rejestrowany **stdout** i **stderr** z procesu określonego w **processPath** .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-226">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="a0d0f-227">Ścieżki względne są względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-227">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="a0d0f-228">Każda ścieżka rozpoczynająca się od `.` jest określana względem katalogu głównego witryny, a wszystkie inne ścieżki są traktowane jako ścieżki bezwzględne.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-228">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="a0d0f-229">Wszystkie foldery podane w ścieżce są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-229">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="a0d0f-230">Przy użyciu ograniczników podkreślenia, sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) są dodawane do ostatniego segmentu ścieżki **stdoutLogFile** .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-230">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="a0d0f-231">Jeśli wartość `.\logs\stdout` jest podawana w postaci wartości, przykładowy dziennik stdout jest zapisywany jako *stdout_20180205194132_1934. log* w folderze *Logs* , gdy zapisywany na 2/5/2018 o 19:41:32 z identyfikatorem procesu 1934.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-231">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a><span data-ttu-id="a0d0f-232">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="a0d0f-232">Set environment variables</span></span>

<span data-ttu-id="a0d0f-233">Zmienne środowiskowe można określić dla procesu w atrybucie `processPath`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-233">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="a0d0f-234">Określ zmienną środowiskową z elementem podrzędnym `<environmentVariable>` elementu kolekcji `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-234">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="a0d0f-235">Zmienne środowiskowe ustawione w tej sekcji mają pierwszeństwo przed zmiennymi środowiskowymi systemowymi.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-235">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="a0d0f-236">W poniższym przykładzie są ustawiane dwie zmienne środowiskowe w *pliku Web. config*. `ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji do `Development`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-236">The following example sets two environment variables in *web.config*. `ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="a0d0f-237">Deweloper może tymczasowo ustawić tę wartość w pliku *Web. config* w celu wymuszenia załadowania [strony wyjątku dewelopera](xref:fundamentals/error-handling) podczas debugowania wyjątku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-237">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="a0d0f-238">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, w której programista zapisał kod, który odczytuje wartość przy uruchamianiu, aby utworzyć ścieżkę do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-238">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="a0d0f-239">Alternatywą dla ustawienia środowiska bezpośrednio w pliku *Web. config* jest uwzględnienie właściwości `<EnvironmentName>` w profilu publikacji ( *. pubxml*) lub plik projektu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-239">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="a0d0f-240">To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-240">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="a0d0f-241">Dla zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` należy ustawić wartość `Development` na serwerach przejściowych i testowych, które nie są dostępne dla niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-241">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="a0d0f-242">app_offline. htm</span><span class="sxs-lookup"><span data-stu-id="a0d0f-242">app_offline.htm</span></span>

<span data-ttu-id="a0d0f-243">Jeśli plik o nazwie *app_offline. htm* zostanie wykryty w katalogu głównym aplikacji, moduł ASP.NET Core próbuje bezpiecznie zamknąć aplikację i zatrzymać przetwarzanie żądań przychodzących.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-243">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="a0d0f-244">Jeśli aplikacja nadal działa po upływie liczby sekund zdefiniowanej w `shutdownTimeLimit`, moduł ASP.NET Core kasuje uruchomiony proces.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-244">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="a0d0f-245">Gdy jest obecny plik *app_offline. htm* , moduł ASP.NET Core reaguje na żądania, wysyłając z powrotem zawartość pliku *app_offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-245">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="a0d0f-246">Po usunięciu pliku *app_offline. htm* następnym żądaniu zostanie uruchomiona aplikacja.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-246">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="a0d0f-247">W przypadku korzystania z modelu hostingu poza procesem aplikacja może nie zostać wyłączona natychmiast, jeśli istnieje otwarte połączenie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-247">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="a0d0f-248">Na przykład połączenie protokołu WebSocket może opóźnić zamykanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-248">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="a0d0f-249">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="a0d0f-249">Start-up error page</span></span>

<span data-ttu-id="a0d0f-250">Hosting zarówno w procesie, jak i poza procesem tworzy niestandardowe strony błędów, gdy nie można uruchomić aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-250">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="a0d0f-251">Jeśli moduł ASP.NET Core nie może odnaleźć programu obsługi żądania w procesie lub out-of-process, zostanie wyświetlona strona kodowa stanu *niepowodzenia ładowania 500,0-procesowego/wyjściowego* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-251">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="a0d0f-252">W przypadku hostingu w procesie, Jeśli uruchomienie aplikacji przez moduł ASP.NET Core nie powiedzie się, zostanie wyświetlona strona kod stanu *niepowodzenia 500,30* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-252">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="a0d0f-253">W przypadku hostingu poza procesem, jeśli moduł ASP.NET Core nie może uruchomić procesu zaplecza lub proces zaplecza zostanie uruchomiony, ale nie nasłuchuje na skonfigurowanym porcie, zostanie wyświetlona strona kod stanu *niepowodzenia procesu 502,5* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-253">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="a0d0f-254">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stanu 5xx IIS, Użyj atrybutu `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-254">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="a0d0f-255">Aby uzyskać więcej informacji o konfigurowaniu niestandardowych komunikatów o błędach, zobacz [Błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-255">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="a0d0f-256">Tworzenie i przekierowywanie dzienników</span><span class="sxs-lookup"><span data-stu-id="a0d0f-256">Log creation and redirection</span></span>

<span data-ttu-id="a0d0f-257">Moduł ASP.NET Core przekierowuje dane wyjściowe z konsoli stdout i stderr do dysku, jeśli są ustawione atrybuty `stdoutLogEnabled` i `stdoutLogFile` elementu `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-257">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="a0d0f-258">Wszystkie foldery w ścieżce `stdoutLogFile` są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-258">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="a0d0f-259">Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\<app_pool_name>`, aby zapewnić uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-259">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="a0d0f-260">Dzienniki nie są obracane, chyba że zostanie wykonane odtwarzanie procesów/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-260">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="a0d0f-261">Ponosisz odpowiedzialność dostawcy usług hostingowych, aby ograniczyć ilość miejsca na dysku zużywanej przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-261">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="a0d0f-262">Użycie dziennika stdout jest zalecane tylko w przypadku rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-262">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="a0d0f-263">Nie używaj dziennika stdout w celu uzyskania ogólnych celów rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-263">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="a0d0f-264">Aby rejestrować procedury w aplikacji ASP.NET Core, użyj biblioteki rejestrowania, która ogranicza rozmiar pliku dziennika i obraca dzienniki.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-264">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="a0d0f-265">Aby uzyskać więcej informacji, zobacz [dostawców rejestrowania innych](xref:fundamentals/logging/index#third-party-logging-providers)firm.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-265">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="a0d0f-266">Sygnatura czasowa i rozszerzenie pliku są dodawane automatycznie podczas tworzenia pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-266">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="a0d0f-267">Nazwa pliku dziennika składa się z dołączania sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) do ostatniego segmentu ścieżki `stdoutLogFile` (zazwyczaj *stdout*) rozdzielanej znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-267">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="a0d0f-268">Jeśli ścieżka `stdoutLogFile` kończy się na *stdout*, dziennik aplikacji o identyfikatorze PID 1934 utworzony w dniu 2/5/2018 o 19:42:32 ma nazwę pliku *stdout_20180205194132_1934. log*.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-268">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="a0d0f-269">Jeśli `stdoutLogEnabled` ma wartość false, błędy występujące podczas uruchamiania aplikacji są przechwytywane i emitowane do dziennika zdarzeń do 30 KB.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-269">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="a0d0f-270">Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-270">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="a0d0f-271">Poniższy przykład `aspNetCore` elementu w pliku *Web. config* konfiguruje rejestrowanie stdout dla aplikacji hostowanej w Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-271">The following sample `aspNetCore` element in a *web.config* file configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="a0d0f-272">Ścieżka lokalna lub ścieżka udziału sieciowego jest akceptowalna do rejestrowania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-272">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="a0d0f-273">Upewnij się, że tożsamość użytkownika puli aplikacji ma uprawnienia do zapisu w podanej ścieżce.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-273">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="a0d0f-274">Udoskonalone dzienniki diagnostyczne</span><span class="sxs-lookup"><span data-stu-id="a0d0f-274">Enhanced diagnostic logs</span></span>

<span data-ttu-id="a0d0f-275">Moduł ASP.NET Core można skonfigurować w celu udostępnienia dzienników diagnostyki rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-275">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="a0d0f-276">Dodaj element `<handlerSettings>` do elementu `<aspNetCore>` w *pliku Web. config*. Ustawienie `debugLevel` na `TRACE` uwidacznia większą wierność informacji diagnostycznych:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-276">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="a0d0f-277">Wszystkie foldery w ścieżce (*dzienniki* w poprzednim przykładzie) są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-277">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="a0d0f-278">Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\<app_pool_name>`, aby zapewnić uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-278">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="a0d0f-279">Wartości poziomu debugowania (`debugLevel`) mogą zawierać zarówno poziom, jak i lokalizację.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-279">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="a0d0f-280">Poziomy (w kolejności od najmniejszej do największej szczegółowości):</span><span class="sxs-lookup"><span data-stu-id="a0d0f-280">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="a0d0f-281">Porn</span><span class="sxs-lookup"><span data-stu-id="a0d0f-281">ERROR</span></span>
* <span data-ttu-id="a0d0f-282">WYŚWIETLANIA</span><span class="sxs-lookup"><span data-stu-id="a0d0f-282">WARNING</span></span>
* <span data-ttu-id="a0d0f-283">INFORMACJE</span><span class="sxs-lookup"><span data-stu-id="a0d0f-283">INFO</span></span>
* <span data-ttu-id="a0d0f-284">TRACE</span><span class="sxs-lookup"><span data-stu-id="a0d0f-284">TRACE</span></span>

<span data-ttu-id="a0d0f-285">Lokalizacje (wiele lokalizacji jest dozwolonych):</span><span class="sxs-lookup"><span data-stu-id="a0d0f-285">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="a0d0f-286">KONSOLI</span><span class="sxs-lookup"><span data-stu-id="a0d0f-286">CONSOLE</span></span>
* <span data-ttu-id="a0d0f-287">ELEMENCIE</span><span class="sxs-lookup"><span data-stu-id="a0d0f-287">EVENTLOG</span></span>
* <span data-ttu-id="a0d0f-288">ROZSZERZENIEM</span><span class="sxs-lookup"><span data-stu-id="a0d0f-288">FILE</span></span>

<span data-ttu-id="a0d0f-289">Ustawienia programu obsługi można także zapewnić za pomocą zmiennych środowiskowych:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-289">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="a0d0f-290">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; ścieżka do pliku dziennika debugowania.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-290">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="a0d0f-291">(Domyślnie: *aspnetcore-Debug. log*)</span><span class="sxs-lookup"><span data-stu-id="a0d0f-291">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="a0d0f-292">`ASPNETCORE_MODULE_DEBUG` &ndash; ustawienia poziomu debugowania.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-292">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="a0d0f-293">**Nie** pozostawiaj włączonej rejestracji debugowania w ramach wdrożenia dłużej niż jest to wymagane, aby rozwiązać problem.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-293">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="a0d0f-294">Rozmiar dziennika nie jest ograniczony.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-294">The size of the log isn't limited.</span></span> <span data-ttu-id="a0d0f-295">Pozostawienie włączonego dziennika debugowania może spowodować wyczerpanie dostępnego miejsca na dysku i awarię serwera lub usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-295">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="a0d0f-296">Zobacz [Konfiguracja z plikiem Web. config](#configuration-with-webconfig) , aby zapoznać się z przykładem elementu `aspNetCore` w pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-296">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="modify-the-stack-size"></a><span data-ttu-id="a0d0f-297">Modyfikowanie rozmiaru stosu</span><span class="sxs-lookup"><span data-stu-id="a0d0f-297">Modify the stack size</span></span>

<span data-ttu-id="a0d0f-298">*Stosuje się tylko w przypadku korzystania z modelu hostingu w procesie.*</span><span class="sxs-lookup"><span data-stu-id="a0d0f-298">*Only applies when using the in-process hosting model.*</span></span>

<span data-ttu-id="a0d0f-299">Skonfiguruj rozmiar zarządzanego stosu przy użyciu ustawienia `stackSize` w bajtach w *pliku Web. config*. Domyślny rozmiar to `1048576` bajty (1 MB).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-299">Configure the managed stack size using the `stackSize` setting in bytes in *web.config*. The default size is `1048576` bytes (1 MB).</span></span>

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

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="a0d0f-300">Konfiguracja serwera proxy używa protokołu HTTP i tokenu parowania</span><span class="sxs-lookup"><span data-stu-id="a0d0f-300">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="a0d0f-301">*Dotyczy tylko hostingu poza procesem.*</span><span class="sxs-lookup"><span data-stu-id="a0d0f-301">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="a0d0f-302">Serwer proxy utworzony między modułem ASP.NET Core a Kestrel używa protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-302">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="a0d0f-303">Nie ma ryzyka podsłuchiwanie ruchu między modułem i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-303">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="a0d0f-304">Token parowania jest używany w celu zagwarantowania, że żądania odbierane przez Kestrel zostały przekazane przez usługi IIS i nie pochodzą z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-304">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="a0d0f-305">Token parowania jest tworzony i ustawiany w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-305">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="a0d0f-306">Token parowania jest również ustawiany w nagłówku (`MS-ASPNETCORE-TOKEN`) w każdym żądaniu z serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-306">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="a0d0f-307">Oprogramowanie pośredniczące usług IIS sprawdza każde odebrane żądanie, aby potwierdzić, że wartość nagłówka tokenu parowania jest zgodna z wartością zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-307">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="a0d0f-308">Jeśli wartości tokenu są niezgodne, żądanie zostanie zarejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-308">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="a0d0f-309">Zmienna środowiskowa tokena parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-309">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="a0d0f-310">Bez znajomości wartości tokenu parowania osoba atakująca nie może przesłać żądań, które pomijają Ewidencjonowanie oprogramowania pośredniczącego usług IIS.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-310">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="a0d0f-311">Moduł ASP.NET Core z konfiguracją udostępnioną usług IIS</span><span class="sxs-lookup"><span data-stu-id="a0d0f-311">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="a0d0f-312">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-312">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="a0d0f-313">Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w pliku *ApplicationHost. config* na udział.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-313">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="a0d0f-314">W przypadku korzystania z konfiguracji udostępnionej usług IIS na tym samym komputerze, na którym znajduje się instalacja usług IIS, uruchom Instalatora pakietu ASP.NET Core hostowania z parametrem `OPT_NO_SHARED_CONFIG_CHECK` ustawionym na `1`:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-314">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="a0d0f-315">Jeśli ścieżka do konfiguracji udostępnionej nie znajduje się na tym samym komputerze, na którym znajduje się instalacja usług IIS, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-315">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="a0d0f-316">Wyłącz konfigurację udostępnioną usług IIS.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-316">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="a0d0f-317">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-317">Run the installer.</span></span>
1. <span data-ttu-id="a0d0f-318">Wyeksportuj zaktualizowany plik *ApplicationHost. config* do udziału.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-318">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="a0d0f-319">Włącz ponownie konfigurację udostępnioną usług IIS.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-319">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="a0d0f-320">Dzienniki instalacji pakietu i pakietów hostingu</span><span class="sxs-lookup"><span data-stu-id="a0d0f-320">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="a0d0f-321">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-321">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="a0d0f-322">W systemie hostingu przejdź do *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-322">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="a0d0f-323">Znajdź plik *aspnetcore. dll* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-323">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="a0d0f-324">Kliknij prawym przyciskiem myszy plik i wybierz polecenie **Właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-324">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="a0d0f-325">Wybierz kartę **szczegóły** . **Wersja pliku** i **Wersja produktu** reprezentują zainstalowaną wersję modułu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-325">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="a0d0f-326">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się pod adresem *C: \\Users @ no__t-2% username% \\AppData @ no__t-4Local @ no__t-5Temp*. Plik ma nazwę *dd_DotNetCoreWinSvrHosting__ @ no__t-7timestamp > _000_AspNetCoreModule_x64. log*.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-326">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="a0d0f-327">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="a0d0f-327">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="a0d0f-328">Moduł</span><span class="sxs-lookup"><span data-stu-id="a0d0f-328">Module</span></span>

<span data-ttu-id="a0d0f-329">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-329">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="a0d0f-330">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-330">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="a0d0f-331">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-331">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="a0d0f-332">%ProgramFiles%\IIS\Asp.Net rdzeń Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-332">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="a0d0f-333">% ProgramFiles (x86)% \ IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-333">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="a0d0f-334">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-334">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="a0d0f-335">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-335">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="a0d0f-336">% ProgramFiles (x86)% \ Express\aspnetcore.dll IIS</span><span class="sxs-lookup"><span data-stu-id="a0d0f-336">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="a0d0f-337">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-337">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="a0d0f-338">% ProgramFiles (x86)% \ IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-338">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="a0d0f-339">Schemat</span><span class="sxs-lookup"><span data-stu-id="a0d0f-339">Schema</span></span>

<span data-ttu-id="a0d0f-340">**SERWERZE**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-340">**IIS**</span></span>

* <span data-ttu-id="a0d0f-341">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="a0d0f-341">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="a0d0f-342">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="a0d0f-342">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="a0d0f-343">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-343">**IIS Express**</span></span>

* <span data-ttu-id="a0d0f-344">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="a0d0f-344">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="a0d0f-345">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="a0d0f-345">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="a0d0f-346">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="a0d0f-346">Configuration</span></span>

<span data-ttu-id="a0d0f-347">**SERWERZE**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-347">**IIS**</span></span>

* <span data-ttu-id="a0d0f-348">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="a0d0f-348">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="a0d0f-349">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-349">**IIS Express**</span></span>

* <span data-ttu-id="a0d0f-350">Visual Studio: {Aplikacja główna} \\. vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="a0d0f-350">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="a0d0f-351">Interfejs wiersza polecenia *iisexpress. exe* :%USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="a0d0f-351">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="a0d0f-352">Pliki można znaleźć, wyszukując *aspnetcore* w pliku *ApplicationHost. config* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-352">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="a0d0f-353">Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-353">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="a0d0f-354">Hostowanie aplikacji ASP.NET Core w procesie roboczym usług IIS (`w3wp.exe`), nazywanym [modelem hostingu w procesie](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-354">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="a0d0f-355">Przekazuj żądania sieci Web do zaplecza ASP.NET Core aplikacji, na której uruchomiono [serwer Kestrel](xref:fundamentals/servers/kestrel), nazywany [modelem hostingu poza procesem](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-355">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="a0d0f-356">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-356">Supported Windows versions:</span></span>

* <span data-ttu-id="a0d0f-357">System Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a0d0f-357">Windows 7 or later</span></span>
* <span data-ttu-id="a0d0f-358">System Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a0d0f-358">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="a0d0f-359">W przypadku hostingu w procesie moduł używa implementacji serwera w procesie dla usług IIS o nazwie serwer HTTP IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-359">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="a0d0f-360">Podczas hostingu poza procesem moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-360">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="a0d0f-361">Moduł nie działa w przypadku [protokołu HTTP. sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-361">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="a0d0f-362">Modele hostingu</span><span class="sxs-lookup"><span data-stu-id="a0d0f-362">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="a0d0f-363">Model hostingu w procesie</span><span class="sxs-lookup"><span data-stu-id="a0d0f-363">In-process hosting model</span></span>

<span data-ttu-id="a0d0f-364">Aby skonfigurować aplikację do hostingu w procesie, należy dodać właściwość `<AspNetCoreHostingModel>` do pliku projektu aplikacji o wartości `InProcess` (hosting zewnętrzny jest ustawiony z `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="a0d0f-364">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="a0d0f-365">Model hostingu w procesie nie jest obsługiwany w przypadku aplikacji ASP.NET Core przeznaczonych dla .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-365">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="a0d0f-366">Jeśli właściwość `<AspNetCoreHostingModel>` nie istnieje w pliku, wartość domyślna to `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-366">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="a0d0f-367">Następujące cechy są stosowane podczas hostingu w procesie:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-367">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="a0d0f-368">Serwer HTTP IIS (`IISHttpServer`) jest używany zamiast serwera [Kestrel](xref:fundamentals/servers/kestrel) .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-368">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="a0d0f-369">W przypadku wywołań [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> do:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-369">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="a0d0f-370">Zarejestruj `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-370">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="a0d0f-371">Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-371">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="a0d0f-372">Skonfiguruj hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-372">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="a0d0f-373">[Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-373">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="a0d0f-374">Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-374">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="a0d0f-375">Użyj jednej puli aplikacji na aplikację.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-375">Use one app pool per app.</span></span>

* <span data-ttu-id="a0d0f-376">W przypadku korzystania z [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręcznego umieszczania [pliku app_offline. htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files)aplikacja może nie zostać wyłączona natychmiast, jeśli istnieje otwarte połączenie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-376">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="a0d0f-377">Na przykład połączenie protokołu WebSocket może opóźnić zamykanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-377">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="a0d0f-378">Architektura (bitową) aplikacji i zainstalowane środowisko uruchomieniowe (x64 lub x86) musi być zgodna z architekturą puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-378">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="a0d0f-379">Wykryto rozłączenia klientów.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-379">Client disconnects are detected.</span></span> <span data-ttu-id="a0d0f-380">Token anulowania [HttpContext. RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) został anulowany po rozłączeniu klienta.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-380">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="a0d0f-381">W ASP.NET Core 2.2.1 lub wcześniejszym, <xref:System.IO.Directory.GetCurrentDirectory*> zwraca katalog procesów roboczych procesu uruchomionego przez usługi IIS, a nie katalog aplikacji (na przykład *C:\Windows\System32\inetsrv* for *w3wp. exe*).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-381">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="a0d0f-382">Przykładowy kod, który ustawia bieżący katalog aplikacji, znajduje się w [klasie CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-382">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="a0d0f-383">Wywołaj metodę `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-383">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="a0d0f-384">Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> zapewniają katalog aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-384">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="a0d0f-385">Podczas hostingu w procesie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wywoływana wewnętrznie w celu zainicjowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-385">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="a0d0f-386">W związku z tym, implementacja <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> używana do przekształcania oświadczeń po każdym uwierzytelnieniu nie jest domyślnie aktywowana.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-386">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="a0d0f-387">Podczas przekształcania oświadczeń z implementacją <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> Wywołaj <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, aby dodać usługi uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-387">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="a0d0f-388">Model hostingu poza procesem</span><span class="sxs-lookup"><span data-stu-id="a0d0f-388">Out-of-process hosting model</span></span>

<span data-ttu-id="a0d0f-389">Aby skonfigurować aplikację do hostingu poza procesem, użyj jednego z następujących metod w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-389">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="a0d0f-390">Nie określaj właściwości `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-390">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="a0d0f-391">Jeśli właściwość `<AspNetCoreHostingModel>` nie istnieje w pliku, wartość domyślna to `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-391">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="a0d0f-392">Ustaw wartość właściwości `<AspNetCoreHostingModel>` na `OutOfProcess` (hosting w procesie jest ustawiony z `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="a0d0f-392">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="a0d0f-393">Serwer [Kestrel](xref:fundamentals/servers/kestrel) jest używany zamiast serwera http IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-393">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="a0d0f-394">W przypadku pozaprocesowych wywołań [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> do:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-394">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="a0d0f-395">Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-395">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="a0d0f-396">Skonfiguruj hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-396">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="a0d0f-397">Zmiany modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="a0d0f-397">Hosting model changes</span></span>

<span data-ttu-id="a0d0f-398">Jeśli ustawienie `hostingModel` zostanie zmienione w pliku *Web. config* (wyjaśniono w sekcji [Konfiguracja z plikiem Web. config](#configuration-with-webconfig) ), moduł odtwarza proces roboczy dla usług IIS.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-398">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="a0d0f-399">W przypadku IIS Express moduł nie odtwarza procesu roboczego, ale zamiast tego wyzwala bezpieczne zamknięcie bieżącego procesu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-399">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="a0d0f-400">Następne żądanie do aplikacji powoduje zduplikowanie nowego procesu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-400">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="a0d0f-401">Nazwa procesu</span><span class="sxs-lookup"><span data-stu-id="a0d0f-401">Process name</span></span>

<span data-ttu-id="a0d0f-402">Raporty `Process.GetCurrentProcess().ProcessName` `w3wp` @ no__t-2 @ no__t-3 (wewnątrzprocesowe-Process) lub `dotnet` (pozaprocesowe).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-402">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="a0d0f-403">Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-403">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="a0d0f-404">Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-404">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="a0d0f-405">Moduł ASP.NET Core może również:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-405">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="a0d0f-406">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-406">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="a0d0f-407">Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-407">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="a0d0f-408">Przekazuj tokeny uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-408">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="a0d0f-409">Jak zainstalować moduł ASP.NET Core i korzystać z niego</span><span class="sxs-lookup"><span data-stu-id="a0d0f-409">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="a0d0f-410">Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-410">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="a0d0f-411">Konfiguracja z pliku Web. config</span><span class="sxs-lookup"><span data-stu-id="a0d0f-411">Configuration with web.config</span></span>

<span data-ttu-id="a0d0f-412">Moduł ASP.NET Core jest skonfigurowany z sekcją `aspNetCore` węzła `system.webServer` w pliku *Web. config* witryny.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-412">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="a0d0f-413">Następujący plik *Web. config* jest publikowany dla [wdrożenia zależnego od platformy](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje moduł ASP.NET Core do obsługi żądań lokacji:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-413">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="a0d0f-414">Następująca *plik Web. config* jest publikowana dla [samodzielnego wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="a0d0f-414">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="a0d0f-415">Właściwość <xref:System.Configuration.SectionInformation.InheritInChildApplications*> ma wartość `false` w celu wskazania, że ustawienia określone w elemencie [> \<location](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-415">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="a0d0f-416">Po wdrożeniu aplikacji do [Azure App Service](https://azure.microsoft.com/services/app-service/)ścieżka `stdoutLogFile` jest ustawiona na `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-416">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="a0d0f-417">Ścieżka zapisuje dzienniki stdout do folderu *LogFiles* , który jest lokalizacją automatycznie utworzoną przez usługę.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-417">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="a0d0f-418">Aby uzyskać informacje na temat konfiguracji aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-418">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="a0d0f-419">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="a0d0f-419">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="a0d0f-420">Atrybut</span><span class="sxs-lookup"><span data-stu-id="a0d0f-420">Attribute</span></span> | <span data-ttu-id="a0d0f-421">Opis</span><span class="sxs-lookup"><span data-stu-id="a0d0f-421">Description</span></span> | <span data-ttu-id="a0d0f-422">Domyślny</span><span class="sxs-lookup"><span data-stu-id="a0d0f-422">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="a0d0f-423">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-423">Optional string attribute.</span></span></p><p><span data-ttu-id="a0d0f-424">Argumenty do pliku wykonywalnego określonego w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-424">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="a0d0f-425">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-425">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a0d0f-426">W przypadku wartości true strona **błędu 502,5 procesu** jest pomijana, a strona kodowa stanu 502 skonfigurowana w *pliku Web. config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-426">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="a0d0f-427">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-427">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a0d0f-428">Jeśli wartość jest równa true, token jest przekazywany do procesu podrzędnego, który nasłuchuje na% ASPNETCORE_PORT% jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" dla żądania.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-428">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="a0d0f-429">Jest odpowiedzialny za ten proces, aby wywołać metodę CloseHandle na tym tokenie na żądanie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-429">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="a0d0f-430">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-430">Optional string attribute.</span></span></p><p><span data-ttu-id="a0d0f-431">Określa model hostingu jako proces (`InProcess`) lub out-of-Process (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-431">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="a0d0f-432">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-432">Optional integer attribute.</span></span></p><p><span data-ttu-id="a0d0f-433">Określa liczbę wystąpień procesu określonego w ustawieniu **processPath** , które może być przypadające na aplikację.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-433">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="a0d0f-434">@no__t 0For hosting w procesie, wartość jest ograniczona do `1`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-434">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="a0d0f-435">Ustawienie `processesPerApplication` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-435">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="a0d0f-436">Ten atrybut zostanie usunięty w przyszłych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-436">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="a0d0f-437">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-437">Default: `1`</span></span><br><span data-ttu-id="a0d0f-438">Minimum: `1`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-438">Min: `1`</span></span><br><span data-ttu-id="a0d0f-439">Maks.: `100` @ no__t-1</span><span class="sxs-lookup"><span data-stu-id="a0d0f-439">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="a0d0f-440">Wymagany atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-440">Required string attribute.</span></span></p><p><span data-ttu-id="a0d0f-441">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-441">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="a0d0f-442">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-442">Relative paths are supported.</span></span> <span data-ttu-id="a0d0f-443">Jeśli ścieżka rozpoczyna się od `.`, ścieżka jest uznawana za względną względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-443">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="a0d0f-444">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-444">Optional integer attribute.</span></span></p><p><span data-ttu-id="a0d0f-445">Określa, ile razy proces określony w **processPath** może ulec awarii na minutę.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-445">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="a0d0f-446">W przypadku przekroczenia tego limitu moduł przestaje uruchomić proces przez pozostałą część minuty.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-446">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="a0d0f-447">Nieobsługiwane w przypadku hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-447">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="a0d0f-448">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-448">Default: `10`</span></span><br><span data-ttu-id="a0d0f-449">Minimum: `0`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-449">Min: `0`</span></span><br><span data-ttu-id="a0d0f-450">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-450">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="a0d0f-451">Opcjonalny atrybut TimeSpan.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-451">Optional timespan attribute.</span></span></p><p><span data-ttu-id="a0d0f-452">Określa czas, przez który moduł ASP.NET Core czeka na odpowiedź z procesu nasłuchiwania na% ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-452">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="a0d0f-453">W wersjach modułu ASP.NET Core, który został dostarczony z wersją ASP.NET Core 2,1 lub nowszą, `requestTimeout` jest określony w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-453">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="a0d0f-454">Nie dotyczy hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-454">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="a0d0f-455">W przypadku hostingu w procesie moduł czeka na aplikację w celu przetworzenia żądania.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-455">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="a0d0f-456">Prawidłowe wartości segmentów minut i sekund ciągu mieszczą się w zakresie 0-59.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-456">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="a0d0f-457">Użycie **60** w wartości minut lub sekund skutkuje *błędem wewnętrznego serwera 500*.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-457">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="a0d0f-458">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-458">Default: `00:02:00`</span></span><br><span data-ttu-id="a0d0f-459">Minimum: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-459">Min: `00:00:00`</span></span><br><span data-ttu-id="a0d0f-460">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-460">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="a0d0f-461">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-461">Optional integer attribute.</span></span></p><p><span data-ttu-id="a0d0f-462">Czas w sekundach, przez który moduł czeka, aż plik wykonywalny zostanie bezpiecznie zamknięty po wykryciu pliku *app_offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-462">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="a0d0f-463">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-463">Default: `10`</span></span><br><span data-ttu-id="a0d0f-464">Minimum: `0`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-464">Min: `0`</span></span><br><span data-ttu-id="a0d0f-465">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-465">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="a0d0f-466">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-466">Optional integer attribute.</span></span></p><p><span data-ttu-id="a0d0f-467">Czas w sekundach, przez który moduł czeka, aż plik wykonywalny uruchomi proces nasłuchujący na porcie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-467">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="a0d0f-468">Jeśli ten limit czasu zostanie przekroczony, moduł zakasuje proces.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-468">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="a0d0f-469">Moduł podejmuje próbę ponownego uruchomienia procesu, gdy odbierze nowe żądanie i kontynuuje ponowne uruchomienie procesu na kolejnych żądaniach przychodzących, chyba że aplikacja nie będzie mogła uruchomić **rapidFailsPerMinute** liczbę razy w ostatniej minucie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-469">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="a0d0f-470">Wartość 0 (zero) **nie** jest uważana za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-470">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="a0d0f-471">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-471">Default: `120`</span></span><br><span data-ttu-id="a0d0f-472">Minimum: `0`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-472">Min: `0`</span></span><br><span data-ttu-id="a0d0f-473">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-473">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="a0d0f-474">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-474">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a0d0f-475">Jeśli wartość jest równa true, **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-475">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="a0d0f-476">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-476">Optional string attribute.</span></span></p><p><span data-ttu-id="a0d0f-477">Określa względną lub bezwzględną ścieżkę do pliku, dla którego jest rejestrowany **stdout** i **stderr** z procesu określonego w **processPath** .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-477">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="a0d0f-478">Ścieżki względne są względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-478">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="a0d0f-479">Każda ścieżka rozpoczynająca się od `.` jest określana względem katalogu głównego witryny, a wszystkie inne ścieżki są traktowane jako ścieżki bezwzględne.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-479">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="a0d0f-480">Wszystkie foldery podane w ścieżce są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-480">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="a0d0f-481">Przy użyciu ograniczników podkreślenia, sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) są dodawane do ostatniego segmentu ścieżki **stdoutLogFile** .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-481">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="a0d0f-482">Jeśli wartość `.\logs\stdout` jest podawana w postaci wartości, przykładowy dziennik stdout jest zapisywany jako *stdout_20180205194132_1934. log* w folderze *Logs* , gdy zapisywany na 2/5/2018 o 19:41:32 z identyfikatorem procesu 1934.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-482">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="a0d0f-483">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="a0d0f-483">Setting environment variables</span></span>

<span data-ttu-id="a0d0f-484">Zmienne środowiskowe można określić dla procesu w atrybucie `processPath`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-484">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="a0d0f-485">Określ zmienną środowiskową z elementem podrzędnym `<environmentVariable>` elementu kolekcji `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-485">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="a0d0f-486">Zmienne środowiskowe ustawione w tej sekcji mają pierwszeństwo przed zmiennymi środowiskowymi systemowymi.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-486">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="a0d0f-487">W poniższym przykładzie są ustawiane dwie zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-487">The following example sets two environment variables.</span></span> <span data-ttu-id="a0d0f-488">`ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji do `Development`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-488">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="a0d0f-489">Deweloper może tymczasowo ustawić tę wartość w pliku *Web. config* w celu wymuszenia załadowania [strony wyjątku dewelopera](xref:fundamentals/error-handling) podczas debugowania wyjątku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-489">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="a0d0f-490">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, w której programista zapisał kod, który odczytuje wartość przy uruchamianiu, aby utworzyć ścieżkę do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-490">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="a0d0f-491">Alternatywą dla ustawienia środowiska bezpośrednio w pliku *Web. config* jest uwzględnienie właściwości `<EnvironmentName>` w profilu publikacji ( *. pubxml*) lub plik projektu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-491">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="a0d0f-492">To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-492">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="a0d0f-493">Dla zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` należy ustawić wartość `Development` na serwerach przejściowych i testowych, które nie są dostępne dla niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-493">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="a0d0f-494">app_offline. htm</span><span class="sxs-lookup"><span data-stu-id="a0d0f-494">app_offline.htm</span></span>

<span data-ttu-id="a0d0f-495">Jeśli plik o nazwie *app_offline. htm* zostanie wykryty w katalogu głównym aplikacji, moduł ASP.NET Core próbuje bezpiecznie zamknąć aplikację i zatrzymać przetwarzanie żądań przychodzących.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-495">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="a0d0f-496">Jeśli aplikacja nadal działa po upływie liczby sekund zdefiniowanej w `shutdownTimeLimit`, moduł ASP.NET Core kasuje uruchomiony proces.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-496">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="a0d0f-497">Gdy jest obecny plik *app_offline. htm* , moduł ASP.NET Core reaguje na żądania, wysyłając z powrotem zawartość pliku *app_offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-497">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="a0d0f-498">Po usunięciu pliku *app_offline. htm* następnym żądaniu zostanie uruchomiona aplikacja.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-498">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="a0d0f-499">W przypadku korzystania z modelu hostingu poza procesem aplikacja może nie zostać wyłączona natychmiast, jeśli istnieje otwarte połączenie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-499">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="a0d0f-500">Na przykład połączenie protokołu WebSocket może opóźnić zamykanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-500">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="a0d0f-501">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="a0d0f-501">Start-up error page</span></span>

<span data-ttu-id="a0d0f-502">Hosting zarówno w procesie, jak i poza procesem tworzy niestandardowe strony błędów, gdy nie można uruchomić aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-502">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="a0d0f-503">Jeśli moduł ASP.NET Core nie może odnaleźć programu obsługi żądania w procesie lub out-of-process, zostanie wyświetlona strona kodowa stanu *niepowodzenia ładowania 500,0-procesowego/wyjściowego* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-503">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="a0d0f-504">W przypadku hostingu w procesie, Jeśli uruchomienie aplikacji przez moduł ASP.NET Core nie powiedzie się, zostanie wyświetlona strona kod stanu *niepowodzenia 500,30* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-504">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="a0d0f-505">W przypadku hostingu poza procesem, jeśli moduł ASP.NET Core nie może uruchomić procesu zaplecza lub proces zaplecza zostanie uruchomiony, ale nie nasłuchuje na skonfigurowanym porcie, zostanie wyświetlona strona kod stanu *niepowodzenia procesu 502,5* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-505">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="a0d0f-506">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stanu 5xx IIS, Użyj atrybutu `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-506">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="a0d0f-507">Aby uzyskać więcej informacji o konfigurowaniu niestandardowych komunikatów o błędach, zobacz [Błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-507">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="a0d0f-508">Tworzenie i przekierowywanie dzienników</span><span class="sxs-lookup"><span data-stu-id="a0d0f-508">Log creation and redirection</span></span>

<span data-ttu-id="a0d0f-509">Moduł ASP.NET Core przekierowuje dane wyjściowe z konsoli stdout i stderr do dysku, jeśli są ustawione atrybuty `stdoutLogEnabled` i `stdoutLogFile` elementu `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-509">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="a0d0f-510">Wszystkie foldery w ścieżce `stdoutLogFile` są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-510">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="a0d0f-511">Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\<app_pool_name>`, aby zapewnić uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-511">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="a0d0f-512">Dzienniki nie są obracane, chyba że zostanie wykonane odtwarzanie procesów/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-512">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="a0d0f-513">Ponosisz odpowiedzialność dostawcy usług hostingowych, aby ograniczyć ilość miejsca na dysku zużywanej przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-513">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="a0d0f-514">Użycie dziennika stdout jest zalecane tylko w przypadku rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-514">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="a0d0f-515">Nie używaj dziennika stdout w celu uzyskania ogólnych celów rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-515">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="a0d0f-516">Aby rejestrować procedury w aplikacji ASP.NET Core, użyj biblioteki rejestrowania, która ogranicza rozmiar pliku dziennika i obraca dzienniki.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-516">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="a0d0f-517">Aby uzyskać więcej informacji, zobacz [dostawców rejestrowania innych](xref:fundamentals/logging/index#third-party-logging-providers)firm.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-517">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="a0d0f-518">Sygnatura czasowa i rozszerzenie pliku są dodawane automatycznie podczas tworzenia pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-518">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="a0d0f-519">Nazwa pliku dziennika składa się z dołączania sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) do ostatniego segmentu ścieżki `stdoutLogFile` (zazwyczaj *stdout*) rozdzielanej znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-519">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="a0d0f-520">Jeśli ścieżka `stdoutLogFile` kończy się na *stdout*, dziennik aplikacji o identyfikatorze PID 1934 utworzony w dniu 2/5/2018 o 19:42:32 ma nazwę pliku *stdout_20180205194132_1934. log*.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-520">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="a0d0f-521">Jeśli `stdoutLogEnabled` ma wartość false, błędy występujące podczas uruchamiania aplikacji są przechwytywane i emitowane do dziennika zdarzeń do 30 KB.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-521">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="a0d0f-522">Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-522">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="a0d0f-523">Poniższy przykład `aspNetCore` umożliwia skonfigurowanie rejestrowania stdout dla aplikacji hostowanej w Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-523">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="a0d0f-524">Ścieżka lokalna lub ścieżka udziału sieciowego jest akceptowalna do rejestrowania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-524">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="a0d0f-525">Upewnij się, że tożsamość użytkownika puli aplikacji ma uprawnienia do zapisu w podanej ścieżce.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-525">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="a0d0f-526">Udoskonalone dzienniki diagnostyczne</span><span class="sxs-lookup"><span data-stu-id="a0d0f-526">Enhanced diagnostic logs</span></span>

<span data-ttu-id="a0d0f-527">Moduł ASP.NET Core można skonfigurować w celu udostępnienia dzienników diagnostyki rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-527">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="a0d0f-528">Dodaj element `<handlerSettings>` do elementu `<aspNetCore>` w *pliku Web. config*. Ustawienie `debugLevel` na `TRACE` uwidacznia większą wierność informacji diagnostycznych:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-528">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="a0d0f-529">Foldery w ścieżce podane do wartości `<handlerSetting>` (*dzienniki* w powyższym przykładzie) nie są automatycznie tworzone przez moduł i powinny być wcześniej dostępne we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-529">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="a0d0f-530">Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\<app_pool_name>`, aby zapewnić uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-530">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="a0d0f-531">Wartości poziomu debugowania (`debugLevel`) mogą zawierać zarówno poziom, jak i lokalizację.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-531">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="a0d0f-532">Poziomy (w kolejności od najmniejszej do największej szczegółowości):</span><span class="sxs-lookup"><span data-stu-id="a0d0f-532">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="a0d0f-533">Porn</span><span class="sxs-lookup"><span data-stu-id="a0d0f-533">ERROR</span></span>
* <span data-ttu-id="a0d0f-534">WYŚWIETLANIA</span><span class="sxs-lookup"><span data-stu-id="a0d0f-534">WARNING</span></span>
* <span data-ttu-id="a0d0f-535">INFORMACJE</span><span class="sxs-lookup"><span data-stu-id="a0d0f-535">INFO</span></span>
* <span data-ttu-id="a0d0f-536">TRACE</span><span class="sxs-lookup"><span data-stu-id="a0d0f-536">TRACE</span></span>

<span data-ttu-id="a0d0f-537">Lokalizacje (wiele lokalizacji jest dozwolonych):</span><span class="sxs-lookup"><span data-stu-id="a0d0f-537">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="a0d0f-538">KONSOLI</span><span class="sxs-lookup"><span data-stu-id="a0d0f-538">CONSOLE</span></span>
* <span data-ttu-id="a0d0f-539">ELEMENCIE</span><span class="sxs-lookup"><span data-stu-id="a0d0f-539">EVENTLOG</span></span>
* <span data-ttu-id="a0d0f-540">ROZSZERZENIEM</span><span class="sxs-lookup"><span data-stu-id="a0d0f-540">FILE</span></span>

<span data-ttu-id="a0d0f-541">Ustawienia programu obsługi można także zapewnić za pomocą zmiennych środowiskowych:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-541">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="a0d0f-542">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; ścieżka do pliku dziennika debugowania.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-542">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="a0d0f-543">(Domyślnie: *aspnetcore-Debug. log*)</span><span class="sxs-lookup"><span data-stu-id="a0d0f-543">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="a0d0f-544">`ASPNETCORE_MODULE_DEBUG` &ndash; ustawienia poziomu debugowania.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-544">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="a0d0f-545">**Nie** pozostawiaj włączonej rejestracji debugowania w ramach wdrożenia dłużej niż jest to wymagane, aby rozwiązać problem.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-545">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="a0d0f-546">Rozmiar dziennika nie jest ograniczony.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-546">The size of the log isn't limited.</span></span> <span data-ttu-id="a0d0f-547">Pozostawienie włączonego dziennika debugowania może spowodować wyczerpanie dostępnego miejsca na dysku i awarię serwera lub usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-547">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="a0d0f-548">Zobacz [Konfiguracja z plikiem Web. config](#configuration-with-webconfig) , aby zapoznać się z przykładem elementu `aspNetCore` w pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-548">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="a0d0f-549">Konfiguracja serwera proxy używa protokołu HTTP i tokenu parowania</span><span class="sxs-lookup"><span data-stu-id="a0d0f-549">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="a0d0f-550">*Dotyczy tylko hostingu poza procesem.*</span><span class="sxs-lookup"><span data-stu-id="a0d0f-550">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="a0d0f-551">Serwer proxy utworzony między modułem ASP.NET Core a Kestrel używa protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-551">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="a0d0f-552">Nie ma ryzyka podsłuchiwanie ruchu między modułem i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-552">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="a0d0f-553">Token parowania jest używany w celu zagwarantowania, że żądania odbierane przez Kestrel zostały przekazane przez usługi IIS i nie pochodzą z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-553">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="a0d0f-554">Token parowania jest tworzony i ustawiany w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-554">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="a0d0f-555">Token parowania jest również ustawiany w nagłówku (`MS-ASPNETCORE-TOKEN`) w każdym żądaniu z serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-555">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="a0d0f-556">Oprogramowanie pośredniczące usług IIS sprawdza każde odebrane żądanie, aby potwierdzić, że wartość nagłówka tokenu parowania jest zgodna z wartością zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-556">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="a0d0f-557">Jeśli wartości tokenu są niezgodne, żądanie zostanie zarejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-557">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="a0d0f-558">Zmienna środowiskowa tokena parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-558">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="a0d0f-559">Bez znajomości wartości tokenu parowania osoba atakująca nie może przesłać żądań, które pomijają Ewidencjonowanie oprogramowania pośredniczącego usług IIS.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-559">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="a0d0f-560">Moduł ASP.NET Core z konfiguracją udostępnioną usług IIS</span><span class="sxs-lookup"><span data-stu-id="a0d0f-560">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="a0d0f-561">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-561">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="a0d0f-562">Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w pliku *ApplicationHost. config* na udział.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-562">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="a0d0f-563">W przypadku korzystania z konfiguracji udostępnionej usług IIS na tym samym komputerze, na którym znajduje się instalacja usług IIS, uruchom Instalatora pakietu ASP.NET Core hostowania z parametrem `OPT_NO_SHARED_CONFIG_CHECK` ustawionym na `1`:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-563">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="a0d0f-564">Jeśli ścieżka do konfiguracji udostępnionej nie znajduje się na tym samym komputerze, na którym znajduje się instalacja usług IIS, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-564">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="a0d0f-565">Wyłącz konfigurację udostępnioną usług IIS.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-565">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="a0d0f-566">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-566">Run the installer.</span></span>
1. <span data-ttu-id="a0d0f-567">Wyeksportuj zaktualizowany plik *ApplicationHost. config* do udziału.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-567">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="a0d0f-568">Włącz ponownie konfigurację udostępnioną usług IIS.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-568">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="a0d0f-569">Dzienniki instalacji pakietu i pakietów hostingu</span><span class="sxs-lookup"><span data-stu-id="a0d0f-569">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="a0d0f-570">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-570">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="a0d0f-571">W systemie hostingu przejdź do *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-571">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="a0d0f-572">Znajdź plik *aspnetcore. dll* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-572">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="a0d0f-573">Kliknij prawym przyciskiem myszy plik i wybierz polecenie **Właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-573">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="a0d0f-574">Wybierz kartę **szczegóły** . **Wersja pliku** i **Wersja produktu** reprezentują zainstalowaną wersję modułu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-574">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="a0d0f-575">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się pod adresem *C: \\Users @ no__t-2% username% \\AppData @ no__t-4Local @ no__t-5Temp*. Plik ma nazwę *dd_DotNetCoreWinSvrHosting__ @ no__t-7timestamp > _000_AspNetCoreModule_x64. log*.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-575">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="a0d0f-576">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="a0d0f-576">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="a0d0f-577">Moduł</span><span class="sxs-lookup"><span data-stu-id="a0d0f-577">Module</span></span>

<span data-ttu-id="a0d0f-578">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-578">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="a0d0f-579">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-579">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="a0d0f-580">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-580">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="a0d0f-581">%ProgramFiles%\IIS\Asp.Net rdzeń Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-581">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="a0d0f-582">% ProgramFiles (x86)% \ IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-582">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="a0d0f-583">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-583">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="a0d0f-584">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-584">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="a0d0f-585">% ProgramFiles (x86)% \ Express\aspnetcore.dll IIS</span><span class="sxs-lookup"><span data-stu-id="a0d0f-585">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="a0d0f-586">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-586">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="a0d0f-587">% ProgramFiles (x86)% \ IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-587">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="a0d0f-588">Schemat</span><span class="sxs-lookup"><span data-stu-id="a0d0f-588">Schema</span></span>

<span data-ttu-id="a0d0f-589">**SERWERZE**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-589">**IIS**</span></span>

* <span data-ttu-id="a0d0f-590">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="a0d0f-590">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="a0d0f-591">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="a0d0f-591">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="a0d0f-592">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-592">**IIS Express**</span></span>

* <span data-ttu-id="a0d0f-593">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="a0d0f-593">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="a0d0f-594">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="a0d0f-594">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="a0d0f-595">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="a0d0f-595">Configuration</span></span>

<span data-ttu-id="a0d0f-596">**SERWERZE**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-596">**IIS**</span></span>

* <span data-ttu-id="a0d0f-597">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="a0d0f-597">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="a0d0f-598">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-598">**IIS Express**</span></span>

* <span data-ttu-id="a0d0f-599">Visual Studio: {Aplikacja główna} \\. vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="a0d0f-599">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="a0d0f-600">Interfejs wiersza polecenia *iisexpress. exe* :%USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="a0d0f-600">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="a0d0f-601">Pliki można znaleźć, wyszukując *aspnetcore* w pliku *ApplicationHost. config* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-601">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a0d0f-602">Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS do przesyłania dalej żądań sieci Web do zaplecza ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-602">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="a0d0f-603">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-603">Supported Windows versions:</span></span>

* <span data-ttu-id="a0d0f-604">System Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a0d0f-604">Windows 7 or later</span></span>
* <span data-ttu-id="a0d0f-605">System Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a0d0f-605">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="a0d0f-606">Moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-606">The module only works with Kestrel.</span></span> <span data-ttu-id="a0d0f-607">Moduł jest niezgodny z [protokołem HTTP. sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-607">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="a0d0f-608">Ponieważ ASP.NET Core aplikacje działają w procesie innym niż proces roboczy usług IIS, moduł obsługuje również zarządzanie procesami.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-608">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="a0d0f-609">Moduł uruchamia proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie zostanie odebrane i ponownie uruchomiony, jeśli wystąpi awaria.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-609">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="a0d0f-610">Jest to zasadniczo takie samo zachowanie jak w przypadku aplikacji ASP.NET 4. x, które działają w procesie w usługach IIS, które są zarządzane przez [usługę aktywacji procesów systemu Windows (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-610">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a0d0f-611">Na poniższym diagramie przedstawiono relację między usługami IIS, modułem ASP.NET Core i aplikacją:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-611">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="a0d0f-613">Żądania docierają do sieci Web do sterownika HTTP. sys trybu jądra.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-613">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a0d0f-614">Sterownik kieruje żądania do usług IIS na skonfigurowanym porcie witryny sieci Web, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-614">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a0d0f-615">Moduł przekazuje żądania do Kestrel na losowo wybranym porcie dla aplikacji, która nie jest portem 80 lub 443.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-615">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="a0d0f-616">Moduł określa port za pośrednictwem zmiennej środowiskowej podczas uruchamiania, a [oprogramowanie pośredniczące integracji usług IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) konfiguruje serwer do nasłuchiwania na `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-616">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="a0d0f-617">Dodatkowe sprawdzenia są wykonywane, a żądania, które nie pochodzą z modułu, są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-617">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="a0d0f-618">Moduł nie obsługuje przekazywania HTTPS, dlatego żądania są przekazywane przez protokół HTTP nawet wtedy, gdy są odbierane przez usługę IIS przez protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-618">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="a0d0f-619">Po podaniu przez Kestrel żądania z modułu żądanie jest wypychane do potoku ASP.NET Core pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-619">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a0d0f-620">Potok oprogramowania pośredniczącego obsługuje żądanie i przekazuje go jako wystąpienie `HttpContext` do logiki aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-620">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a0d0f-621">Oprogramowanie pośredniczące dodane przez integrację usług IIS aktualizuje schemat, zdalny adres IP i pathbase, aby można było przesłać żądanie do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-621">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="a0d0f-622">Odpowiedź aplikacji jest przesyłana z powrotem do usług IIS, która wypycha ją z powrotem do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-622">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="a0d0f-623">Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-623">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="a0d0f-624">Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-624">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="a0d0f-625">Moduł ASP.NET Core może również:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-625">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="a0d0f-626">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-626">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="a0d0f-627">Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-627">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="a0d0f-628">Przekazuj tokeny uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-628">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="a0d0f-629">Jak zainstalować moduł ASP.NET Core i korzystać z niego</span><span class="sxs-lookup"><span data-stu-id="a0d0f-629">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="a0d0f-630">Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-630">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="a0d0f-631">Konfiguracja z pliku Web. config</span><span class="sxs-lookup"><span data-stu-id="a0d0f-631">Configuration with web.config</span></span>

<span data-ttu-id="a0d0f-632">Moduł ASP.NET Core jest skonfigurowany z sekcją `aspNetCore` węzła `system.webServer` w pliku *Web. config* witryny.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-632">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="a0d0f-633">Następujący plik *Web. config* jest publikowany dla [wdrożenia zależnego od platformy](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje moduł ASP.NET Core do obsługi żądań lokacji:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-633">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="a0d0f-634">Następująca *plik Web. config* jest publikowana dla [samodzielnego wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="a0d0f-634">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="a0d0f-635">Po wdrożeniu aplikacji do [Azure App Service](https://azure.microsoft.com/services/app-service/)ścieżka `stdoutLogFile` jest ustawiona na `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-635">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="a0d0f-636">Ścieżka zapisuje dzienniki stdout do folderu *LogFiles* , który jest lokalizacją automatycznie utworzoną przez usługę.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-636">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="a0d0f-637">Aby uzyskać informacje na temat konfiguracji aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-637">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="a0d0f-638">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="a0d0f-638">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="a0d0f-639">Atrybut</span><span class="sxs-lookup"><span data-stu-id="a0d0f-639">Attribute</span></span> | <span data-ttu-id="a0d0f-640">Opis</span><span class="sxs-lookup"><span data-stu-id="a0d0f-640">Description</span></span> | <span data-ttu-id="a0d0f-641">Domyślny</span><span class="sxs-lookup"><span data-stu-id="a0d0f-641">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="a0d0f-642">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-642">Optional string attribute.</span></span></p><p><span data-ttu-id="a0d0f-643">Argumenty do pliku wykonywalnego określonego w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-643">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="a0d0f-644">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-644">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a0d0f-645">W przypadku wartości true strona **błędu 502,5 procesu** jest pomijana, a strona kodowa stanu 502 skonfigurowana w *pliku Web. config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-645">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="a0d0f-646">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-646">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a0d0f-647">Jeśli wartość jest równa true, token jest przekazywany do procesu podrzędnego, który nasłuchuje na% ASPNETCORE_PORT% jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" dla żądania.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-647">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="a0d0f-648">Jest odpowiedzialny za ten proces, aby wywołać metodę CloseHandle na tym tokenie na żądanie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-648">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="a0d0f-649">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-649">Optional integer attribute.</span></span></p><p><span data-ttu-id="a0d0f-650">Określa liczbę wystąpień procesu określonego w ustawieniu **processPath** , które może być przypadające na aplikację.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-650">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="a0d0f-651">Ustawienie `processesPerApplication` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-651">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="a0d0f-652">Ten atrybut zostanie usunięty w przyszłych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-652">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="a0d0f-653">Wartość domyślna: `1`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-653">Default: `1`</span></span><br><span data-ttu-id="a0d0f-654">Minimum: `1`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-654">Min: `1`</span></span><br><span data-ttu-id="a0d0f-655">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-655">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="a0d0f-656">Wymagany atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-656">Required string attribute.</span></span></p><p><span data-ttu-id="a0d0f-657">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-657">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="a0d0f-658">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-658">Relative paths are supported.</span></span> <span data-ttu-id="a0d0f-659">Jeśli ścieżka rozpoczyna się od `.`, ścieżka jest uznawana za względną względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-659">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="a0d0f-660">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-660">Optional integer attribute.</span></span></p><p><span data-ttu-id="a0d0f-661">Określa, ile razy proces określony w **processPath** może ulec awarii na minutę.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-661">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="a0d0f-662">W przypadku przekroczenia tego limitu moduł przestaje uruchomić proces przez pozostałą część minuty.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-662">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="a0d0f-663">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-663">Default: `10`</span></span><br><span data-ttu-id="a0d0f-664">Minimum: `0`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-664">Min: `0`</span></span><br><span data-ttu-id="a0d0f-665">Maks.: `100`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-665">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="a0d0f-666">Opcjonalny atrybut TimeSpan.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-666">Optional timespan attribute.</span></span></p><p><span data-ttu-id="a0d0f-667">Określa czas, przez który moduł ASP.NET Core czeka na odpowiedź z procesu nasłuchiwania na% ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-667">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="a0d0f-668">W wersjach modułu ASP.NET Core, który został dostarczony z wersją ASP.NET Core 2,1 lub nowszą, `requestTimeout` jest określony w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-668">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="a0d0f-669">Wartość domyślna: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-669">Default: `00:02:00`</span></span><br><span data-ttu-id="a0d0f-670">Minimum: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-670">Min: `00:00:00`</span></span><br><span data-ttu-id="a0d0f-671">Maks.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-671">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="a0d0f-672">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-672">Optional integer attribute.</span></span></p><p><span data-ttu-id="a0d0f-673">Czas w sekundach, przez który moduł czeka, aż plik wykonywalny zostanie bezpiecznie zamknięty po wykryciu pliku *app_offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-673">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="a0d0f-674">Wartość domyślna: `10`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-674">Default: `10`</span></span><br><span data-ttu-id="a0d0f-675">Minimum: `0`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-675">Min: `0`</span></span><br><span data-ttu-id="a0d0f-676">Maks.: `600`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-676">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="a0d0f-677">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-677">Optional integer attribute.</span></span></p><p><span data-ttu-id="a0d0f-678">Czas w sekundach, przez który moduł czeka, aż plik wykonywalny uruchomi proces nasłuchujący na porcie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-678">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="a0d0f-679">Jeśli ten limit czasu zostanie przekroczony, moduł zakasuje proces.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-679">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="a0d0f-680">Moduł podejmuje próbę ponownego uruchomienia procesu, gdy odbierze nowe żądanie i kontynuuje ponowne uruchomienie procesu na kolejnych żądaniach przychodzących, chyba że aplikacja nie będzie mogła uruchomić **rapidFailsPerMinute** liczbę razy w ostatniej minucie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-680">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="a0d0f-681">Wartość 0 (zero) **nie** jest uważana za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-681">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="a0d0f-682">Wartość domyślna: `120`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-682">Default: `120`</span></span><br><span data-ttu-id="a0d0f-683">Minimum: `0`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-683">Min: `0`</span></span><br><span data-ttu-id="a0d0f-684">Maks.: `3600`</span><span class="sxs-lookup"><span data-stu-id="a0d0f-684">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="a0d0f-685">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-685">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a0d0f-686">Jeśli wartość jest równa true, **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-686">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="a0d0f-687">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-687">Optional string attribute.</span></span></p><p><span data-ttu-id="a0d0f-688">Określa względną lub bezwzględną ścieżkę do pliku, dla którego jest rejestrowany **stdout** i **stderr** z procesu określonego w **processPath** .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-688">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="a0d0f-689">Ścieżki względne są względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-689">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="a0d0f-690">Każda ścieżka rozpoczynająca się od `.` jest określana względem katalogu głównego witryny, a wszystkie inne ścieżki są traktowane jako ścieżki bezwzględne.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-690">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="a0d0f-691">Wszystkie foldery podane w ścieżce muszą istnieć, aby moduł mógł utworzyć plik dziennika.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-691">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="a0d0f-692">Przy użyciu ograniczników podkreślenia, sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) są dodawane do ostatniego segmentu ścieżki **stdoutLogFile** .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-692">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="a0d0f-693">Jeśli wartość `.\logs\stdout` jest podawana w postaci wartości, przykładowy dziennik stdout jest zapisywany jako *stdout_20180205194132_1934. log* w folderze *Logs* , gdy zapisywany na 2/5/2018 o 19:41:32 z identyfikatorem procesu 1934.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-693">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="a0d0f-694">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="a0d0f-694">Setting environment variables</span></span>

<span data-ttu-id="a0d0f-695">Zmienne środowiskowe można określić dla procesu w atrybucie `processPath`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-695">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="a0d0f-696">Określ zmienną środowiskową z elementem podrzędnym `<environmentVariable>` elementu kolekcji `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-696">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="a0d0f-697">Zmienne środowiskowe ustawione w tej sekcji powodują konflikt z systemowymi zmiennymi środowiskowymi ustawionymi z tą samą nazwą.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-697">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="a0d0f-698">Jeśli zmienna środowiskowa jest ustawiona zarówno w pliku *Web. config* , jak i na poziomie systemu w systemie Windows, wartość z pliku *Web. config* zostanie dołączona do wartości zmiennej środowiskowej systemowej (na przykład `ASPNETCORE_ENVIRONMENT: Development;Development`), która uniemożliwia aplikacji Uruchamianie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-698">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

<span data-ttu-id="a0d0f-699">W poniższym przykładzie są ustawiane dwie zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-699">The following example sets two environment variables.</span></span> <span data-ttu-id="a0d0f-700">`ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji do `Development`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-700">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="a0d0f-701">Deweloper może tymczasowo ustawić tę wartość w pliku *Web. config* w celu wymuszenia załadowania [strony wyjątku dewelopera](xref:fundamentals/error-handling) podczas debugowania wyjątku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-701">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="a0d0f-702">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, w której programista zapisał kod, który odczytuje wartość przy uruchamianiu, aby utworzyć ścieżkę do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-702">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="a0d0f-703">Dla zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` należy ustawić wartość `Development` na serwerach przejściowych i testowych, które nie są dostępne dla niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-703">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="a0d0f-704">app_offline. htm</span><span class="sxs-lookup"><span data-stu-id="a0d0f-704">app_offline.htm</span></span>

<span data-ttu-id="a0d0f-705">Jeśli plik o nazwie *app_offline. htm* zostanie wykryty w katalogu głównym aplikacji, moduł ASP.NET Core próbuje bezpiecznie zamknąć aplikację i zatrzymać przetwarzanie żądań przychodzących.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-705">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="a0d0f-706">Jeśli aplikacja nadal działa po upływie liczby sekund zdefiniowanej w `shutdownTimeLimit`, moduł ASP.NET Core kasuje uruchomiony proces.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-706">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="a0d0f-707">Gdy jest obecny plik *app_offline. htm* , moduł ASP.NET Core reaguje na żądania, wysyłając z powrotem zawartość pliku *app_offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-707">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="a0d0f-708">Po usunięciu pliku *app_offline. htm* następnym żądaniu zostanie uruchomiona aplikacja.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-708">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="a0d0f-709">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="a0d0f-709">Start-up error page</span></span>

<span data-ttu-id="a0d0f-710">Jeśli moduł ASP.NET Core nie może uruchomić procesu zaplecza lub proces zaplecza zostanie uruchomiony, ale nie nasłuchuje na skonfigurowanym porcie, zostanie wyświetlona strona kod stanu *niepowodzenia procesu 502,5* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-710">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="a0d0f-711">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stanu 502 usług IIS, Użyj atrybutu `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-711">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="a0d0f-712">Aby uzyskać więcej informacji o konfigurowaniu niestandardowych komunikatów o błędach, zobacz [Błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-712">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Strona kodowa stanu niepowodzenia procesów 502,5](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="a0d0f-714">Tworzenie i przekierowywanie dzienników</span><span class="sxs-lookup"><span data-stu-id="a0d0f-714">Log creation and redirection</span></span>

<span data-ttu-id="a0d0f-715">Moduł ASP.NET Core przekierowuje dane wyjściowe z konsoli stdout i stderr do dysku, jeśli są ustawione atrybuty `stdoutLogEnabled` i `stdoutLogFile` elementu `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-715">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="a0d0f-716">Wszystkie foldery w ścieżce `stdoutLogFile` są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-716">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="a0d0f-717">Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\<app_pool_name>`, aby zapewnić uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-717">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="a0d0f-718">Dzienniki nie są obracane, chyba że zostanie wykonane odtwarzanie procesów/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-718">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="a0d0f-719">Ponosisz odpowiedzialność dostawcy usług hostingowych, aby ograniczyć ilość miejsca na dysku zużywanej przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-719">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="a0d0f-720">Użycie dziennika stdout jest zalecane tylko w przypadku rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-720">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="a0d0f-721">Nie używaj dziennika stdout w celu uzyskania ogólnych celów rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-721">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="a0d0f-722">Aby rejestrować procedury w aplikacji ASP.NET Core, użyj biblioteki rejestrowania, która ogranicza rozmiar pliku dziennika i obraca dzienniki.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-722">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="a0d0f-723">Aby uzyskać więcej informacji, zobacz [dostawców rejestrowania innych](xref:fundamentals/logging/index#third-party-logging-providers)firm.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-723">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="a0d0f-724">Sygnatura czasowa i rozszerzenie pliku są dodawane automatycznie podczas tworzenia pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-724">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="a0d0f-725">Nazwa pliku dziennika składa się z dołączania sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) do ostatniego segmentu ścieżki `stdoutLogFile` (zazwyczaj *stdout*) rozdzielanej znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-725">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="a0d0f-726">Jeśli ścieżka `stdoutLogFile` kończy się na *stdout*, dziennik aplikacji o identyfikatorze PID 1934 utworzony w dniu 2/5/2018 o 19:42:32 ma nazwę pliku *stdout_20180205194132_1934. log*.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-726">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="a0d0f-727">Poniższy przykład `aspNetCore` umożliwia skonfigurowanie rejestrowania stdout dla aplikacji hostowanej w Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-727">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="a0d0f-728">Ścieżka lokalna lub ścieżka udziału sieciowego jest akceptowalna do rejestrowania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-728">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="a0d0f-729">Upewnij się, że tożsamość użytkownika puli aplikacji ma uprawnienia do zapisu w podanej ścieżce.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-729">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="a0d0f-730">Foldery w ścieżce podane do wartości `<handlerSetting>` (*dzienniki* w powyższym przykładzie) nie są automatycznie tworzone przez moduł i powinny być wcześniej dostępne we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-730">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="a0d0f-731">Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\<app_pool_name>`, aby zapewnić uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="a0d0f-731">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="a0d0f-732">Zobacz [Konfiguracja z plikiem Web. config](#configuration-with-webconfig) , aby zapoznać się z przykładem elementu `aspNetCore` w pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-732">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="a0d0f-733">Konfiguracja serwera proxy używa protokołu HTTP i tokenu parowania</span><span class="sxs-lookup"><span data-stu-id="a0d0f-733">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="a0d0f-734">Serwer proxy utworzony między modułem ASP.NET Core a Kestrel używa protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-734">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="a0d0f-735">Nie ma ryzyka podsłuchiwanie ruchu między modułem i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-735">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="a0d0f-736">Token parowania jest używany w celu zagwarantowania, że żądania odbierane przez Kestrel zostały przekazane przez usługi IIS i nie pochodzą z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-736">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="a0d0f-737">Token parowania jest tworzony i ustawiany w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-737">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="a0d0f-738">Token parowania jest również ustawiany w nagłówku (`MS-ASPNETCORE-TOKEN`) w każdym żądaniu z serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-738">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="a0d0f-739">Oprogramowanie pośredniczące usług IIS sprawdza każde odebrane żądanie, aby potwierdzić, że wartość nagłówka tokenu parowania jest zgodna z wartością zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-739">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="a0d0f-740">Jeśli wartości tokenu są niezgodne, żądanie zostanie zarejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-740">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="a0d0f-741">Zmienna środowiskowa tokena parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-741">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="a0d0f-742">Bez znajomości wartości tokenu parowania osoba atakująca nie może przesłać żądań, które pomijają Ewidencjonowanie oprogramowania pośredniczącego usług IIS.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-742">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="a0d0f-743">Moduł ASP.NET Core z konfiguracją udostępnioną usług IIS</span><span class="sxs-lookup"><span data-stu-id="a0d0f-743">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="a0d0f-744">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-744">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="a0d0f-745">Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w pliku *ApplicationHost. config* na udział.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-745">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="a0d0f-746">W przypadku korzystania z konfiguracji udostępnionej usług IIS wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-746">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="a0d0f-747">Wyłącz konfigurację udostępnioną usług IIS.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-747">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="a0d0f-748">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-748">Run the installer.</span></span>
1. <span data-ttu-id="a0d0f-749">Wyeksportuj zaktualizowany plik *ApplicationHost. config* do udziału.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-749">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="a0d0f-750">Włącz ponownie konfigurację udostępnioną usług IIS.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-750">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="a0d0f-751">Dzienniki instalacji pakietu i pakietów hostingu</span><span class="sxs-lookup"><span data-stu-id="a0d0f-751">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="a0d0f-752">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a0d0f-752">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="a0d0f-753">W systemie hostingu przejdź do *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-753">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="a0d0f-754">Znajdź plik *aspnetcore. dll* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-754">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="a0d0f-755">Kliknij prawym przyciskiem myszy plik i wybierz polecenie **Właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-755">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="a0d0f-756">Wybierz kartę **szczegóły** . **Wersja pliku** i **Wersja produktu** reprezentują zainstalowaną wersję modułu.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-756">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="a0d0f-757">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się pod adresem *C: \\Users @ no__t-2% username% \\AppData @ no__t-4Local @ no__t-5Temp*. Plik ma nazwę *dd_DotNetCoreWinSvrHosting__ @ no__t-7timestamp > _000_AspNetCoreModule_x64. log*.</span><span class="sxs-lookup"><span data-stu-id="a0d0f-757">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="a0d0f-758">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="a0d0f-758">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="a0d0f-759">Moduł</span><span class="sxs-lookup"><span data-stu-id="a0d0f-759">Module</span></span>

<span data-ttu-id="a0d0f-760">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-760">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="a0d0f-761">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-761">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="a0d0f-762">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-762">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="a0d0f-763">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-763">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="a0d0f-764">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a0d0f-764">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="a0d0f-765">% ProgramFiles (x86)% \ Express\aspnetcore.dll IIS</span><span class="sxs-lookup"><span data-stu-id="a0d0f-765">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="a0d0f-766">Schemat</span><span class="sxs-lookup"><span data-stu-id="a0d0f-766">Schema</span></span>

<span data-ttu-id="a0d0f-767">**SERWERZE**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-767">**IIS**</span></span>

* <span data-ttu-id="a0d0f-768">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="a0d0f-768">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="a0d0f-769">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-769">**IIS Express**</span></span>

* <span data-ttu-id="a0d0f-770">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="a0d0f-770">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="a0d0f-771">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="a0d0f-771">Configuration</span></span>

<span data-ttu-id="a0d0f-772">**SERWERZE**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-772">**IIS**</span></span>

* <span data-ttu-id="a0d0f-773">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="a0d0f-773">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="a0d0f-774">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="a0d0f-774">**IIS Express**</span></span>

* <span data-ttu-id="a0d0f-775">Visual Studio: {Aplikacja główna} \\. vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="a0d0f-775">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="a0d0f-776">Interfejs wiersza polecenia *iisexpress. exe* :%USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="a0d0f-776">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="a0d0f-777">Pliki można znaleźć, wyszukując *aspnetcore* w pliku *ApplicationHost. config* .</span><span class="sxs-lookup"><span data-stu-id="a0d0f-777">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a0d0f-778">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a0d0f-778">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="a0d0f-779">Repozytorium GitHub modułu ASP.NET Core (Źródło odwołania)</span><span class="sxs-lookup"><span data-stu-id="a0d0f-779">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
