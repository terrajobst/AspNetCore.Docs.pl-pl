---
title: Hosting w platformy ASP.NET Core
author: guardrex
description: "Więcej informacji na temat hosta sieci web w ASP.NET Core, która jest odpowiedzialna za uruchamianie i okresem istnienia zarządzania aplikacjami."
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: fe9935eaa3529c513dfb2d111a38a4452012b364
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="aa016-103">Hosting w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa016-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="aa016-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="aa016-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="aa016-105">Aplikacje platformy ASP.NET Core skonfigurować i uruchomić *hosta*.</span><span class="sxs-lookup"><span data-stu-id="aa016-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="aa016-106">Host jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="aa016-107">Co najmniej hosta konfiguruje serwer i potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="aa016-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="aa016-108">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="aa016-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="aa016-110">Utwórz hosta za pomocą wystąpienia [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="aa016-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="aa016-111">Jest to najczęściej wykonywane w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="aa016-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="aa016-112">W szablonach projektu `Main` znajduje się w *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="aa016-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="aa016-113">Typowe *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) do rozpoczęcia konfigurowania hosta:</span><span class="sxs-lookup"><span data-stu-id="aa016-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="aa016-114">`CreateDefaultBuilder` wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="aa016-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="aa016-115">Konfiguruje [Kestrel](servers/kestrel.md) jako serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="aa016-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="aa016-116">Aby Kestrel domyślnych opcji, zobacz [Kestrel opcje sekcji Kestrel implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="aa016-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="aa016-117">Ustawia głównego zawartości w ścieżce zwracanej przez [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="aa016-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="aa016-118">Konfiguracja opcjonalna obciążeń z:</span><span class="sxs-lookup"><span data-stu-id="aa016-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="aa016-119">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="aa016-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="aa016-120">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="aa016-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="aa016-121">[Klucze tajne użytkownika](xref:security/app-secrets) po uruchomieniu aplikacji `Development` środowiska.</span><span class="sxs-lookup"><span data-stu-id="aa016-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="aa016-122">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="aa016-122">Environment variables.</span></span>
  * <span data-ttu-id="aa016-123">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="aa016-123">Command-line arguments.</span></span>
* <span data-ttu-id="aa016-124">Konfiguruje [rejestrowanie](xref:fundamentals/logging/index) dla danych wyjściowych konsoli i debugowania.</span><span class="sxs-lookup"><span data-stu-id="aa016-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="aa016-125">Dostęp do rejestrów uwzględniających [filtrowania dziennika](xref:fundamentals/logging/index#log-filtering) reguły określone w sekcji konfiguracji rejestrowania *appsettings.json* lub *appsettings. { Środowisko} JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="aa016-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="aa016-126">Podczas uruchamiania za usług IIS, umożliwia [integracji usług IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="aa016-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="aa016-127">Konfiguruje serwer nasłuchuje na przy użyciu portu i ścieżki bazowej [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="aa016-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="aa016-128">Moduł tworzy zwrotny serwer proxy między usługami IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="aa016-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="aa016-129">Konfiguruje również aplikacji na [przechwytywania błędy uruchamiania](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="aa016-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="aa016-130">Dla opcji domyślnych usług IIS, zobacz [IIS opcje sekcji hosta platformy ASP.NET Core w systemie Windows z programem IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="aa016-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="aa016-131">Ustawia [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) do `true` Jeśli programowanie jest środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-131">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="aa016-132">Aby uzyskać więcej informacji, zobacz [zakres weryfikacji](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="aa016-132">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="aa016-133">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="aa016-133">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="aa016-134">Po uruchomieniu aplikacji w folderze głównym projektu folderu głównego projektu jest używany jako główny zawartości.</span><span class="sxs-lookup"><span data-stu-id="aa016-134">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="aa016-135">Jest to wartość domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="aa016-135">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="aa016-136">Aby uzyskać więcej informacji o konfiguracji aplikacji, zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="aa016-136">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="aa016-137">Alternatywą wobec przy użyciu statycznych `CreateDefaultBuilder` metody tworzenia hosta z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) jest to obsługiwane podejście z platformy ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="aa016-137">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="aa016-138">Aby uzyskać więcej informacji zobacz kartę 1.x platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa016-138">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="aa016-140">Utwórz hosta za pomocą wystąpienia [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="aa016-140">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="aa016-141">Tworzenie hosta jest zwykle wykonywany w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="aa016-141">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="aa016-142">W szablonach projektu `Main` znajduje się w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa016-142">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="aa016-143">`WebHostBuilder` wymaga [serwera, który implementuje IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="aa016-143">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="aa016-144">Wbudowane serwery są [Kestrel](servers/kestrel.md) i [HTTP.sys](servers/httpsys.md) (przed wydaniem programu ASP.NET 2.0 Core została wywołana HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="aa016-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="aa016-145">W tym przykładzie [— metoda rozszerzenia UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Określa serwer Kestrel.</span><span class="sxs-lookup"><span data-stu-id="aa016-145">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="aa016-146">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="aa016-146">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="aa016-147">Domyślny element zawartości są uzyskiwane dla `UseContentRoot` przez [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="aa016-147">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="aa016-148">Po uruchomieniu aplikacji w folderze głównym projektu folderu głównego projektu jest używany jako główny zawartości.</span><span class="sxs-lookup"><span data-stu-id="aa016-148">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="aa016-149">Jest to wartość domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="aa016-149">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="aa016-150">Do używania serwera IIS jako zwrotny serwer proxy, należy wywołać [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) w ramach tworzenia hosta.</span><span class="sxs-lookup"><span data-stu-id="aa016-150">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="aa016-151">`UseIISIntegration` nie Konfiguruj *serwera*, takiej jak [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) jest.</span><span class="sxs-lookup"><span data-stu-id="aa016-151">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="aa016-152">`UseIISIntegration` Konfiguruje serwer nasłuchuje na przy użyciu portu i ścieżki bazowej [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) utworzyć zwrotnego serwera proxy między Kestrel i IIS.</span><span class="sxs-lookup"><span data-stu-id="aa016-152">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="aa016-153">Aby używać usług IIS z platformy ASP.NET Core `UseKestrel` i `UseIISIntegration` musi być określona.</span><span class="sxs-lookup"><span data-stu-id="aa016-153">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="aa016-154">`UseIISIntegration` aktywuje tylko podczas uruchamiania usług IIS lub usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="aa016-154">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="aa016-155">Aby uzyskać więcej informacji, zobacz [wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module) i [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="aa016-155">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="aa016-156">Minimalny wdrożenia, który konfiguruje hosta (a aplikacji platformy ASP.NET Core) obejmuje określenie serwera i konfiguracji Potok żądań aplikacji:</span><span class="sxs-lookup"><span data-stu-id="aa016-156">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

<span data-ttu-id="aa016-157">Podczas konfigurowania hosta, [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) można przedstawić metody.</span><span class="sxs-lookup"><span data-stu-id="aa016-157">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="aa016-158">Jeśli `Startup` jest określona, muszą być zdefiniowane `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="aa016-158">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="aa016-159">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji w ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="aa016-159">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="aa016-160">Wiele wywołań `ConfigureServices` dołączyć do siebie.</span><span class="sxs-lookup"><span data-stu-id="aa016-160">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="aa016-161">Wiele wywołań `Configure` lub `UseStartup` na `WebHostBuilder` zastąpić poprzednie ustawienia.</span><span class="sxs-lookup"><span data-stu-id="aa016-161">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="aa016-162">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="aa016-162">Host configuration values</span></span>

<span data-ttu-id="aa016-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) zależy od następujących metod można ustawić wartości konfiguracji hosta:</span><span class="sxs-lookup"><span data-stu-id="aa016-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="aa016-164">Konfiguracja Konstruktora hosta, która zawiera zmienne środowiskowe w formacie `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="aa016-164">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="aa016-165">Na przykład `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="aa016-165">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="aa016-166">Jawne metod, takich jak `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="aa016-166">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="aa016-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) i skojarzony klucz.</span><span class="sxs-lookup"><span data-stu-id="aa016-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="aa016-168">Podczas ustawiania wartości z `UseSetting`, wartość jest ustawiona jako ciąg, niezależnie od typu.</span><span class="sxs-lookup"><span data-stu-id="aa016-168">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="aa016-169">Host używa jednego z tych opcji ustawia wartość ostatnio.</span><span class="sxs-lookup"><span data-stu-id="aa016-169">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="aa016-170">Aby uzyskać więcej informacji, zobacz [konfiguracji zastąpiona](#overriding-configuration) w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="aa016-170">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="aa016-171">Przechwyć błędy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="aa016-171">Capture Startup Errors</span></span>

<span data-ttu-id="aa016-172">To ustawienie określa przechwytywania błędy uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="aa016-172">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="aa016-173">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="aa016-173">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="aa016-174">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="aa016-174">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="aa016-175">**Domyślne**: Domyślnie `false` chyba, że aplikacja jest uruchamiana z Kestrel za usług IIS, gdzie wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="aa016-175">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="aa016-176">**Ustawić za pomocą**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="aa016-176">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="aa016-177">**Zmienna środowiskowa**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="aa016-177">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="aa016-178">Gdy `false`, błędy podczas wynik uruchomienia na hoście został zakończony.</span><span class="sxs-lookup"><span data-stu-id="aa016-178">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="aa016-179">Gdy `true`, host przechwytuje wyjątków podczas uruchamiania i podejmie próbę uruchomienia serwera.</span><span class="sxs-lookup"><span data-stu-id="aa016-179">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-180">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="aa016-182">Główny zawartości</span><span class="sxs-lookup"><span data-stu-id="aa016-182">Content Root</span></span>

<span data-ttu-id="aa016-183">To ustawienie określa, gdzie platformy ASP.NET Core rozpocznie się wyszukiwanie plików zawartości, np. widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="aa016-183">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="aa016-184">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="aa016-184">**Key**: contentRoot</span></span>  
<span data-ttu-id="aa016-185">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="aa016-185">**Type**: *string*</span></span>  
<span data-ttu-id="aa016-186">**Domyślna**: domyślne do folderu, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-186">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="aa016-187">**Ustawić za pomocą**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="aa016-187">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="aa016-188">**Zmienna środowiskowa**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="aa016-188">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="aa016-189">Główny zawartości jest również używany jako podstawowa ścieżka dla [ustawienia sieci Web głównego](#web-root).</span><span class="sxs-lookup"><span data-stu-id="aa016-189">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="aa016-190">Jeśli ścieżka nie istnieje, host nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="aa016-190">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="aa016-193">Błędy szczegółowe</span><span class="sxs-lookup"><span data-stu-id="aa016-193">Detailed Errors</span></span>

<span data-ttu-id="aa016-194">Określa, czy szczegółowe błędy, które mają być przechwytywane.</span><span class="sxs-lookup"><span data-stu-id="aa016-194">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="aa016-195">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="aa016-195">**Key**: detailedErrors</span></span>  
<span data-ttu-id="aa016-196">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="aa016-196">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="aa016-197">**Domyślna**: false</span><span class="sxs-lookup"><span data-stu-id="aa016-197">**Default**: false</span></span>  
<span data-ttu-id="aa016-198">**Ustawić za pomocą**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="aa016-198">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="aa016-199">**Zmienna środowiskowa**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="aa016-199">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="aa016-200">Po włączeniu (lub gdy <a href="#environment">środowiska</a> ma ustawioną wartość `Development`), aplikacji znajdują się szczegółowe wyjątki.</span><span class="sxs-lookup"><span data-stu-id="aa016-200">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="aa016-203">Środowisko</span><span class="sxs-lookup"><span data-stu-id="aa016-203">Environment</span></span>

<span data-ttu-id="aa016-204">Ustawia środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-204">Sets the app's environment.</span></span>

<span data-ttu-id="aa016-205">**Klucz**: środowiska</span><span class="sxs-lookup"><span data-stu-id="aa016-205">**Key**: environment</span></span>  
<span data-ttu-id="aa016-206">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="aa016-206">**Type**: *string*</span></span>  
<span data-ttu-id="aa016-207">**Domyślna**: produkcji</span><span class="sxs-lookup"><span data-stu-id="aa016-207">**Default**: Production</span></span>  
<span data-ttu-id="aa016-208">**Ustawić za pomocą**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="aa016-208">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="aa016-209">**Zmienna środowiskowa**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="aa016-209">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="aa016-210">Środowisko można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="aa016-210">The environment can be set to any value.</span></span> <span data-ttu-id="aa016-211">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="aa016-211">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="aa016-212">Wartości nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="aa016-212">Values aren't case sensitive.</span></span> <span data-ttu-id="aa016-213">Domyślnie *środowiska* są odczytywane z `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="aa016-213">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="aa016-214">Korzystając z [programu Visual Studio](https://www.visualstudio.com/), zmienne środowiskowe może być ustawiona w *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="aa016-214">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="aa016-215">Aby uzyskać więcej informacji, zobacz [Praca w środowiskach wielu](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="aa016-215">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="aa016-218">Zestawy uruchomienia hostingu</span><span class="sxs-lookup"><span data-stu-id="aa016-218">Hosting Startup Assemblies</span></span>

<span data-ttu-id="aa016-219">Ustawia hostingu zestawy uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-219">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="aa016-220">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="aa016-220">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="aa016-221">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="aa016-221">**Type**: *string*</span></span>  
<span data-ttu-id="aa016-222">**Domyślna**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="aa016-222">**Default**: Empty string</span></span>  
<span data-ttu-id="aa016-223">**Ustawić za pomocą**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="aa016-223">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="aa016-224">**Zmienna środowiskowa**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="aa016-224">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="aa016-225">Ciąg rozdzielany średnikami obsługi zestawów uruchamiania załadować podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="aa016-225">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="aa016-226">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="aa016-226">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="aa016-227">Mimo że konfiguracja domyślnie przyjmowana jest wartość pustego ciągu, hostingu zestawy uruchamiania zawsze należy uwzględniać zestawu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-227">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="aa016-228">Jeśli zostały podane zestawy uruchomienia hostingu, są one dodane do zestawu aplikacji ładowania, gdy aplikacja tworzy jego wspólne usługi podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="aa016-228">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-229">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-229">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-230">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-230">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="aa016-231">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="aa016-231">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="aa016-232">Preferowane jest Hosting adresy URL</span><span class="sxs-lookup"><span data-stu-id="aa016-232">Prefer Hosting URLs</span></span>

<span data-ttu-id="aa016-233">Wskazuje, czy host powinien nasłuchiwać adresy URL skonfigurowano `WebHostBuilder` zamiast ustawień skonfigurowanych z `IServer` implementacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-233">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="aa016-234">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="aa016-234">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="aa016-235">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="aa016-235">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="aa016-236">**Domyślna**: true</span><span class="sxs-lookup"><span data-stu-id="aa016-236">**Default**: true</span></span>  
<span data-ttu-id="aa016-237">**Ustawić za pomocą**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="aa016-237">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="aa016-238">**Zmienna środowiskowa**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="aa016-238">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="aa016-239">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="aa016-239">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-240">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-240">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="aa016-242">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="aa016-242">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="aa016-243">Zapobiegaj Hosting uruchamiania</span><span class="sxs-lookup"><span data-stu-id="aa016-243">Prevent Hosting Startup</span></span>

<span data-ttu-id="aa016-244">Uniemożliwia automatyczne ładowanie hosting zestawy uruchamiania, tym hosting zestawy uruchamiania konfigurowane za pomocą zestawu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-244">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="aa016-245">Zobacz [Dodawanie funkcji aplikacji za pomocą konfiguracji specyficzne dla platformy](xref:host-and-deploy/platform-specific-configuration) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-245">See [Add app features using a platform-specific configuration](xref:host-and-deploy/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="aa016-246">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="aa016-246">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="aa016-247">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="aa016-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="aa016-248">**Domyślna**: false</span><span class="sxs-lookup"><span data-stu-id="aa016-248">**Default**: false</span></span>  
<span data-ttu-id="aa016-249">**Ustawić za pomocą**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="aa016-249">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="aa016-250">**Zmienna środowiskowa**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="aa016-250">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="aa016-251">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="aa016-251">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-252">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-252">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="aa016-254">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="aa016-254">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="aa016-255">Adresy URL serwerów</span><span class="sxs-lookup"><span data-stu-id="aa016-255">Server URLs</span></span>

<span data-ttu-id="aa016-256">Wskazuje adresy IP lub adresy hostów, porty i protokoły, które serwer powinien nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="aa016-256">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="aa016-257">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="aa016-257">**Key**: urls</span></span>  
<span data-ttu-id="aa016-258">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="aa016-258">**Type**: *string*</span></span>  
<span data-ttu-id="aa016-259">**Domyślna**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="aa016-259">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="aa016-260">**Ustawić za pomocą**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="aa016-260">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="aa016-261">**Zmienna środowiskowa**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="aa016-261">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="aa016-262">Ustaw rozdzielonych średnikami (;) lista adresów URL prefiksy powinny odpowiadać serwera.</span><span class="sxs-lookup"><span data-stu-id="aa016-262">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="aa016-263">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="aa016-263">For example, `http://localhost:123`.</span></span> <span data-ttu-id="aa016-264">Użyj "\*" Aby wskazać, że serwer powinien nasłuchiwać żądań adresy IP lub nazwa hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="aa016-264">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="aa016-265">Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="aa016-265">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="aa016-266">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="aa016-266">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-267">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-267">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="aa016-268">Kestrel ma własny interfejs API konfiguracji punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="aa016-268">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="aa016-269">Aby uzyskać więcej informacji, zobacz [Kestrel implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="aa016-269">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-270">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-270">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="aa016-271">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="aa016-271">Shutdown Timeout</span></span>

<span data-ttu-id="aa016-272">Określa czas oczekiwania na zamknięcie hosta sieci web.</span><span class="sxs-lookup"><span data-stu-id="aa016-272">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="aa016-273">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="aa016-273">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="aa016-274">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="aa016-274">**Type**: *int*</span></span>  
<span data-ttu-id="aa016-275">**Domyślna**: 5</span><span class="sxs-lookup"><span data-stu-id="aa016-275">**Default**: 5</span></span>  
<span data-ttu-id="aa016-276">**Ustawić za pomocą**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="aa016-276">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="aa016-277">**Zmienna środowiskowa**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="aa016-277">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="aa016-278">Mimo że akceptuje klucz *int* z `UseSetting` (na przykład `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) — metoda rozszerzenia przyjmuje [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="aa016-278">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="aa016-279">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="aa016-279">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="aa016-280">Podczas limit czasu, hostingu:</span><span class="sxs-lookup"><span data-stu-id="aa016-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="aa016-281">Wyzwalacze [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="aa016-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="aa016-282">Podejmuje próby zatrzymania usług hostowanych rejestrowania błędów dla usług, których nie udało się zatrzymać.</span><span class="sxs-lookup"><span data-stu-id="aa016-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="aa016-283">Jeśli okres limitu czasu przed wszystkimi zatrzymania usług hostowanych, wszystkie pozostałe usługi active zostały zatrzymane podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="aa016-284">Usługi zatrzymać, nawet jeśli ich przetwarzanie nie zostało ukończone.</span><span class="sxs-lookup"><span data-stu-id="aa016-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="aa016-285">Jeśli usługi wymagają więcej czasu, aby zatrzymać, należy zwiększyć limit czasu.</span><span class="sxs-lookup"><span data-stu-id="aa016-285">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-286">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-286">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-287">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-287">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="aa016-288">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="aa016-288">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="aa016-289">Zestaw startowy</span><span class="sxs-lookup"><span data-stu-id="aa016-289">Startup Assembly</span></span>

<span data-ttu-id="aa016-290">Określa zestaw do wyszukania `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="aa016-290">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="aa016-291">**Klucz**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="aa016-291">**Key**: startupAssembly</span></span>  
<span data-ttu-id="aa016-292">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="aa016-292">**Type**: *string*</span></span>  
<span data-ttu-id="aa016-293">**Domyślna**: zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="aa016-293">**Default**: The app's assembly</span></span>  
<span data-ttu-id="aa016-294">**Ustawić za pomocą**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="aa016-294">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="aa016-295">**Zmienna środowiskowa**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="aa016-295">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="aa016-296">Zestaw o nazwie (`string`) lub typu (`TStartup`) można odwoływać się.</span><span class="sxs-lookup"><span data-stu-id="aa016-296">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="aa016-297">Jeśli wiele `UseStartup` metody są nazywane, pierwszeństwo ma ostatnią.</span><span class="sxs-lookup"><span data-stu-id="aa016-297">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-298">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-299">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-299">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a><span data-ttu-id="aa016-300">Główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="aa016-300">Web Root</span></span>

<span data-ttu-id="aa016-301">Ustawia ścieżkę względną do statycznego zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-301">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="aa016-302">**Klucz**: webroot</span><span class="sxs-lookup"><span data-stu-id="aa016-302">**Key**: webroot</span></span>  
<span data-ttu-id="aa016-303">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="aa016-303">**Type**: *string*</span></span>  
<span data-ttu-id="aa016-304">**Domyślne**: Jeśli nie jest określony, wartością domyślną jest "(Content Root)/wwwroot", jeśli ścieżka istnieje.</span><span class="sxs-lookup"><span data-stu-id="aa016-304">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="aa016-305">Jeśli ścieżka nie istnieje, jest używany dostawca pliku zerowa.</span><span class="sxs-lookup"><span data-stu-id="aa016-305">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="aa016-306">**Ustawić za pomocą**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="aa016-306">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="aa016-307">**Zmienna środowiskowa**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="aa016-307">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-309">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-309">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="aa016-310">Zastępowanie konfiguracji</span><span class="sxs-lookup"><span data-stu-id="aa016-310">Overriding configuration</span></span>

<span data-ttu-id="aa016-311">Użyj [konfiguracji](xref:fundamentals/configuration/index) Aby skonfigurować hosta.</span><span class="sxs-lookup"><span data-stu-id="aa016-311">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="aa016-312">W poniższym przykładzie konfiguracja hosta jest opcjonalnie określić w *hosting.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="aa016-312">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="aa016-313">Wszelkie konfiguracja została załadowana z *hosting.json* plik może być zastąpiona przez argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="aa016-313">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="aa016-314">Zbudowany konfiguracji (w `config`) służy do konfigurowania hosta z `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="aa016-314">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-315">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-315">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="aa016-316">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="aa016-316">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="aa016-317">Zastępowanie konfiguracji dostarczonych przez `UseUrls` z *hosting.json* config argument pierwszą, wiersza polecenia config drugi:</span><span class="sxs-lookup"><span data-stu-id="aa016-317">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="aa016-319">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="aa016-319">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="aa016-320">Zastępowanie konfiguracji dostarczonych przez `UseUrls` z *hosting.json* config argument pierwszą, wiersza polecenia config drugi:</span><span class="sxs-lookup"><span data-stu-id="aa016-320">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> <span data-ttu-id="aa016-321">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) — metoda rozszerzenia nie jest obecnie stanie podczas analizowania sekcji konfiguracji zwrócony przez `GetSection` (na przykład `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="aa016-321">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="aa016-322">`GetSection` — Metoda filtruje klucze konfiguracji do sekcji żądanie, jednak nie pozostawia nazwy sekcji kluczy (na przykład `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="aa016-322">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="aa016-323">`UseConfiguration` Metoda oczekuje klucze do dopasowania `WebHostBuilder` kluczy (na przykład `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="aa016-323">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="aa016-324">Występowanie nazwy sekcji kluczy uniemożliwia wartości w sekcji Konfigurowanie hosta.</span><span class="sxs-lookup"><span data-stu-id="aa016-324">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="aa016-325">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="aa016-325">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="aa016-326">Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie Sekcja konfiguracyjna do WebHostBuilder.UseConfiguration używa kluczy pełna](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="aa016-326">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="aa016-327">Aby określić hosta, uruchom na określony adres URL, żądaną wartość mogą zostać przekazane w wierszu polecenia z podczas wykonywania [dotnet Uruchom](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="aa016-327">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="aa016-328">Argument wiersza polecenia zastępuje `urls` wartość z *hosting.json* pliku, a serwer nasłuchuje na porcie 8080:</span><span class="sxs-lookup"><span data-stu-id="aa016-328">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="aa016-329">Uruchamianie hosta</span><span class="sxs-lookup"><span data-stu-id="aa016-329">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aa016-330">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aa016-330">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="aa016-331">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="aa016-331">**Run**</span></span>

<span data-ttu-id="aa016-332">`Run` Metoda uruchamiania aplikacji sieci web i blokuje wątek wywołujący do czasu zamknięcia hosta:</span><span class="sxs-lookup"><span data-stu-id="aa016-332">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="aa016-333">**Start**</span><span class="sxs-lookup"><span data-stu-id="aa016-333">**Start**</span></span>

<span data-ttu-id="aa016-334">Uruchom hosta w sposób nieblokujące przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="aa016-334">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="aa016-335">Jeśli listę adresów URL są przekazywane do `Start` metody go nasłuchuje na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="aa016-335">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="aa016-336">Inicjowanie i uruchomić nowy host, przy użyciu wstępnie skonfigurowane ustawienia domyślne aplikacji `CreateDefaultBuilder` przy użyciu metody statycznej wygody.</span><span class="sxs-lookup"><span data-stu-id="aa016-336">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="aa016-337">Te metody uruchomienia serwera bez dane wyjściowe konsoli i z [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) poczekaj, aż podział (Ctrl-C/sigint — lub sigterm —):</span><span class="sxs-lookup"><span data-stu-id="aa016-337">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="aa016-338">**Start (RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="aa016-338">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="aa016-339">Rozpoczynać `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="aa016-339">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="aa016-340">Tworzenie żądania w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="aa016-340">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="aa016-341">`WaitForShutdown` bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="aa016-341">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="aa016-342">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="aa016-342">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="aa016-343">**Start (ciąg adresu url, RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="aa016-343">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="aa016-344">Uruchom przy użyciu adresu URL i `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="aa016-344">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="aa016-345">Tworzy wynik, w postaci **Start (aplikacja RequestDelegate)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="aa016-345">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="aa016-346">**Start (akcji<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="aa016-346">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="aa016-347">Użyj wystąpienia `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) do używania oprogramowania pośredniczącego routingu:</span><span class="sxs-lookup"><span data-stu-id="aa016-347">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="aa016-348">Użyj następujących żądań przeglądarki z przykładem:</span><span class="sxs-lookup"><span data-stu-id="aa016-348">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="aa016-349">Żądanie</span><span class="sxs-lookup"><span data-stu-id="aa016-349">Request</span></span>                                    | <span data-ttu-id="aa016-350">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="aa016-350">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="aa016-351">Witaj, pole!</span><span class="sxs-lookup"><span data-stu-id="aa016-351">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="aa016-352">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="aa016-352">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="aa016-353">Zgłasza wyjątek z ciągiem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="aa016-353">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="aa016-354">Zgłasza wyjątek z ciągiem "Uh Niestety!"</span><span class="sxs-lookup"><span data-stu-id="aa016-354">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="aa016-355">Sante, Jan!</span><span class="sxs-lookup"><span data-stu-id="aa016-355">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="aa016-356">Cześć ludzie!</span><span class="sxs-lookup"><span data-stu-id="aa016-356">Hello World!</span></span>                             |

<span data-ttu-id="aa016-357">`WaitForShutdown` bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="aa016-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="aa016-358">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="aa016-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="aa016-359">**Start (ciągu adresu url, Akcja<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="aa016-359">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="aa016-360">Użyj adresu URL i wystąpienie `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="aa016-360">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="aa016-361">Tworzy wynik, w postaci **Start (akcji<IRouteBuilder> routeBuilder)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="aa016-361">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="aa016-362">**StartWith (akcji<IApplicationBuilder> aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="aa016-362">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="aa016-363">Podaj delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="aa016-363">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="aa016-364">Tworzenie żądania w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="aa016-364">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="aa016-365">`WaitForShutdown` bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="aa016-365">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="aa016-366">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="aa016-366">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="aa016-367">**StartWith (ciągu adresu url, Akcja<IApplicationBuilder> aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="aa016-367">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="aa016-368">Podaj adres URL i delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="aa016-368">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="aa016-369">Tworzy wynik, w postaci **StartWith (akcji<IApplicationBuilder> aplikacji)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="aa016-369">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aa016-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aa016-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="aa016-371">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="aa016-371">**Run**</span></span>

<span data-ttu-id="aa016-372">`Run` Metoda uruchamiania aplikacji sieci web i blokuje wątek wywołujący, dopóki nie zostanie zamknięta hosta:</span><span class="sxs-lookup"><span data-stu-id="aa016-372">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="aa016-373">**Start**</span><span class="sxs-lookup"><span data-stu-id="aa016-373">**Start**</span></span>

<span data-ttu-id="aa016-374">Uruchom hosta w sposób nieblokujące przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="aa016-374">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="aa016-375">Jeśli listę adresów URL są przekazywane do `Start` metody go nasłuchuje na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="aa016-375">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="aa016-376">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="aa016-376">IHostingEnvironment interface</span></span>

<span data-ttu-id="aa016-377">[Interfejsu IHostingEnvironment](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) zawiera informacje o aplikacji sieci web środowiska macierzystego.</span><span class="sxs-lookup"><span data-stu-id="aa016-377">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="aa016-378">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskanie `IHostingEnvironment` aby można było używać ich właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="aa016-378">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="aa016-379">A [podejście oparte na Konwencji](xref:fundamentals/environments#startup-conventions) może służyć do konfigurowania aplikacji podczas uruchamiania, na podstawie środowiska.</span><span class="sxs-lookup"><span data-stu-id="aa016-379">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="aa016-380">Możesz też wprowadzić `IHostingEnvironment` do `Startup` konstruktora do użycia w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="aa016-380">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="aa016-381">Oprócz `IsDevelopment` — metoda rozszerzenia, `IHostingEnvironment` oferuje `IsStaging`, `IsProduction`, i `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="aa016-381">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="aa016-382">Zobacz [Praca w środowiskach wielu](xref:fundamentals/environments) szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="aa016-382">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="aa016-383">`IHostingEnvironment` Usługi także mogą zostać dodane bezpośrednio do `Configure` metody do konfigurowania potoku przetwarzania:</span><span class="sxs-lookup"><span data-stu-id="aa016-383">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="aa016-384">`IHostingEnvironment` mogą zostać dodane do `Invoke` metody podczas tworzenia niestandardowego [oprogramowanie pośredniczące](xref:fundamentals/middleware/index#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="aa016-384">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="aa016-385">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="aa016-385">IApplicationLifetime interface</span></span>

<span data-ttu-id="aa016-386">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umożliwia działań po uruchamiania i wyłączania.</span><span class="sxs-lookup"><span data-stu-id="aa016-386">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="aa016-387">Anulowanie tokenów używany do rejestrowania są trzy właściwości w interfejsie `Action` metod, które definiują zdarzenia uruchamiania i wyłączania.</span><span class="sxs-lookup"><span data-stu-id="aa016-387">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="aa016-388">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="aa016-388">Cancellation Token</span></span>    | <span data-ttu-id="aa016-389">Wyzwalane podczas &#8230;</span><span class="sxs-lookup"><span data-stu-id="aa016-389">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="aa016-390">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="aa016-390">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="aa016-391">Host wykonuje łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="aa016-391">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="aa016-392">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="aa016-392">Requests may still be processing.</span></span> <span data-ttu-id="aa016-393">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="aa016-393">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="aa016-394">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="aa016-394">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="aa016-395">Wszystkie żądania powinna zostać przetworzona.</span><span class="sxs-lookup"><span data-stu-id="aa016-395">All requests should be processed.</span></span> <span data-ttu-id="aa016-396">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="aa016-396">Shutdown blocks until this event completes.</span></span> |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

<span data-ttu-id="aa016-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) żąda zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="aa016-398">Następujące klasy używa `StopApplication` jest bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="aa016-398">The following class uses `StopApplication` to gracefully shutdown an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a><span data-ttu-id="aa016-399">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="aa016-399">Scope validation</span></span>

<span data-ttu-id="aa016-400">W programie ASP.NET Core 2.0 lub nowszej [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ustawia [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) do `true` Jeśli programowanie jest środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-400">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="aa016-401">Gdy `ValidateScopes` ma ustawioną wartość `true`, domyślny dostawca usług sprawdza upewnij się, że:</span><span class="sxs-lookup"><span data-stu-id="aa016-401">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="aa016-402">Usługi w zakresie nie są bezpośrednio lub pośrednio rozwiązane od dostawcy usług głównego.</span><span class="sxs-lookup"><span data-stu-id="aa016-402">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="aa016-403">Usługi w zakresie nie są bezpośrednio lub pośrednio wprowadzić do pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="aa016-403">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="aa016-404">Dostawcy usług głównego jest tworzone, gdy [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="aa016-404">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="aa016-405">Okres istnienia dostawcy usług głównego odpowiada istnienia aplikacji/serwer. Jeśli dostawca rozpoczyna się od aplikacji i został usunięty podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa016-405">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="aa016-406">Usługi w zakresie są usuwane przez kontener, który je utworzył.</span><span class="sxs-lookup"><span data-stu-id="aa016-406">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="aa016-407">Zakresami usługi jest tworzony w kontenerze katalogu głównego, okres istnienia usługi jest skutecznie podwyższany do pojedynczego wystąpienia ponieważ tylko są usuwane przez nadrzędny kontener, gdy serwera aplikacji zostanie zamknięta.</span><span class="sxs-lookup"><span data-stu-id="aa016-407">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="aa016-408">Walidacja zakresów usługi przechwytuje tych sytuacji gdy `BuildServiceProvider` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="aa016-408">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="aa016-409">Sprawdzanie poprawności zakresy, w tym w środowisku produkcyjnym, należy skonfigurować [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) z [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) w Konstruktorze hosta:</span><span class="sxs-lookup"><span data-stu-id="aa016-409">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="aa016-410">Rozwiązywanie problemów z System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="aa016-410">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="aa016-411">**Dotyczy tylko platformy ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="aa016-411">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="aa016-412">Host może zostać utworzony przez wstrzykiwanie `IStartup` bezpośrednio do kontenera iniekcji zależności zamiast wywoływania `UseStartup` lub `Configure`:</span><span class="sxs-lookup"><span data-stu-id="aa016-412">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="aa016-413">Jeśli host jest wbudowana w ten sposób, może wystąpić następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="aa016-413">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="aa016-414">Dzieje się tak dlatego [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (bieżącego zestawu) jest wymagane do skanowania pod kątem `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="aa016-414">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="aa016-415">Jeśli aplikacja ręcznie injects `IStartup` do kontenera iniekcji zależności, Dodaj następujące wywołanie do `WebHostBuilder` o podanej nazwie zestawu:</span><span class="sxs-lookup"><span data-stu-id="aa016-415">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="aa016-416">Możesz też dodać manekina `Configure` do `WebHostBuilder`, który określa `applicationName`(`ApplicationKey`) automatycznie:</span><span class="sxs-lookup"><span data-stu-id="aa016-416">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="aa016-417">**Uwaga**: jest to tylko wymagane wraz z wydaniem programu ASP.NET 2.0 rdzeni i tylko wtedy gdy aplikacja nie wywołać `UseStartup` lub `Configure`.</span><span class="sxs-lookup"><span data-stu-id="aa016-417">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="aa016-418">Aby uzyskać więcej informacji, zobacz [anonsów: Microsoft.Extensions.PlatformAbstractions została usunięta (komentarz)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) i [próbki StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="aa016-418">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aa016-419">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="aa016-419">Additional resources</span></span>

* [<span data-ttu-id="aa016-420">Hosting w systemie Windows z usługami IIS</span><span class="sxs-lookup"><span data-stu-id="aa016-420">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="aa016-421">Hosting w systemie Linux z Nginx</span><span class="sxs-lookup"><span data-stu-id="aa016-421">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="aa016-422">Hosting w systemie Linux z Apache</span><span class="sxs-lookup"><span data-stu-id="aa016-422">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="aa016-423">Host usługi systemu Windows</span><span class="sxs-lookup"><span data-stu-id="aa016-423">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
