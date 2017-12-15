---
title: Hosting w platformy ASP.NET Core
author: guardrex
description: "Więcej informacji na temat hosta sieci web w ASP.NET Core, która jest odpowiedzialna za uruchamianie i okresem istnienia zarządzania aplikacjami."
keywords: Hosta, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime sieci web platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: dfec2a67112d40b528b97c847da3dda8ef1e63bd
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="be8cf-104">Hosting w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="be8cf-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="be8cf-105">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="be8cf-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="be8cf-106">Aplikacje platformy ASP.NET Core skonfigurować i uruchomić *hosta*, który jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="be8cf-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="be8cf-107">Co najmniej hosta konfiguruje serwer i potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="be8cf-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="be8cf-108">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="be8cf-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-109">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="be8cf-110">Utwórz hosta za pomocą wystąpienia [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="be8cf-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="be8cf-111">Jest to najczęściej wykonywane w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="be8cf-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="be8cf-112">W szablonach projektu `Main` znajduje się w *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="be8cf-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="be8cf-113">Typowe *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) do rozpoczęcia konfigurowania hosta:</span><span class="sxs-lookup"><span data-stu-id="be8cf-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="be8cf-114">`CreateDefaultBuilder`wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="be8cf-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="be8cf-115">Konfiguruje [Kestrel](servers/kestrel.md) jako serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="be8cf-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="be8cf-116">Aby Kestrel domyślnych opcji, zobacz [Kestrel opcje sekcji Kestrel implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="be8cf-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="be8cf-117">Ustawia zawartości głównego [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="be8cf-117">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="be8cf-118">Konfiguracja opcjonalna obciążeń z:</span><span class="sxs-lookup"><span data-stu-id="be8cf-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="be8cf-119">*appSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="be8cf-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="be8cf-120">*appSettings. {Środowiska} JSON*.</span><span class="sxs-lookup"><span data-stu-id="be8cf-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="be8cf-121">[Klucze tajne użytkownika](xref:security/app-secrets) po uruchomieniu aplikacji `Development` środowiska.</span><span class="sxs-lookup"><span data-stu-id="be8cf-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="be8cf-122">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="be8cf-122">Environment variables.</span></span>
  * <span data-ttu-id="be8cf-123">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="be8cf-123">Command-line arguments.</span></span>
* <span data-ttu-id="be8cf-124">Konfiguruje [rejestrowanie](xref:fundamentals/logging/index) dla danych wyjściowych konsoli i debugowania z [filtrowania dziennika](xref:fundamentals/logging/index#log-filtering) reguły określone w sekcji konfiguracji rejestrowania *appsettings.json* lub *appsettings. {Środowiska} JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="be8cf-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output with [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="be8cf-125">Podczas uruchamiania za usług IIS, umożliwia [integracji usług IIS](xref:publishing/iis) przez skonfigurowanie ścieżki podstawowej i port serwera powinien nasłuchiwać przy użyciu [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="be8cf-125">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="be8cf-126">Moduł tworzy serwer proxy wstecznego między usługami IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="be8cf-126">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="be8cf-127">Konfiguruje również aplikacji na [przechwytywania błędy uruchamiania](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="be8cf-127">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="be8cf-128">Dla opcji domyślnych usług IIS, zobacz [IIS opcje sekcji hosta platformy ASP.NET Core w systemie Windows z programem IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="be8cf-128">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="be8cf-129">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="be8cf-129">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="be8cf-130">Domyślny element zawartości jest [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="be8cf-130">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="be8cf-131">Powoduje to przy użyciu folderu głównego projektu sieci web jako główny zawartości po uruchomieniu aplikacji z folderu głównego (na przykład wywołanie elementu [dotnet Uruchom](/dotnet/core/tools/dotnet-run) z folderu projektu).</span><span class="sxs-lookup"><span data-stu-id="be8cf-131">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="be8cf-132">Jest to wartość domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="be8cf-132">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="be8cf-133">Zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index) uzyskać więcej informacji o konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="be8cf-133">See [Configuration in ASP.NET Core](xref:fundamentals/configuration/index) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="be8cf-134">Alternatywą wobec przy użyciu statycznych `CreateDefaultBuilder` metody tworzenia hosta z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) jest to obsługiwane podejście z platformy ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="be8cf-134">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="be8cf-135">Zobacz kartę 1.x platformy ASP.NET Core, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="be8cf-135">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-136">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="be8cf-137">Utwórz hosta za pomocą wystąpienia [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="be8cf-137">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="be8cf-138">Jest to najczęściej wykonywane w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="be8cf-138">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="be8cf-139">W szablonach projektu `Main` znajduje się w *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="be8cf-139">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="be8cf-140">Następujące *Program.cs* przedstawiono sposób użycia `WebHostBuilder` tworzenie hosta:</span><span class="sxs-lookup"><span data-stu-id="be8cf-140">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="be8cf-141">`WebHostBuilder`wymaga [serwera, który implementuje IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="be8cf-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="be8cf-142">Wbudowane serwery są [Kestrel](servers/kestrel.md) i [HTTP.sys](servers/httpsys.md) (przed wydaniem programu ASP.NET 2.0 Core została wywołana HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="be8cf-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="be8cf-143">W tym przykładzie [— metoda rozszerzenia UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Określa serwer Kestrel.</span><span class="sxs-lookup"><span data-stu-id="be8cf-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="be8cf-144">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="be8cf-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="be8cf-145">Domyślny element zawartości dostarczony do `UseContentRoot` jest [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="be8cf-145">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="be8cf-146">Powoduje to przy użyciu folderu głównego projektu sieci web jako główny zawartości po uruchomieniu aplikacji z folderu głównego (na przykład wywołanie elementu [dotnet Uruchom](/dotnet/core/tools/dotnet-run) z folderu projektu).</span><span class="sxs-lookup"><span data-stu-id="be8cf-146">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="be8cf-147">Jest to wartość domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="be8cf-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="be8cf-148">Do używania serwera IIS jako zwrotny serwer proxy, należy wywołać [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) w ramach tworzenia hosta.</span><span class="sxs-lookup"><span data-stu-id="be8cf-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="be8cf-149">`UseIISIntegration`nie Konfiguruj *serwera*, takiej jak [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) jest.</span><span class="sxs-lookup"><span data-stu-id="be8cf-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="be8cf-150">`UseIISIntegration`Konfiguruje serwer powinien nasłuchiwać przy użyciu portu i ścieżki bazowej [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) utworzyć proxy wstecznego między Kestrel i IIS.</span><span class="sxs-lookup"><span data-stu-id="be8cf-150">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="be8cf-151">Aby używać usług IIS z platformy ASP.NET Core, należy określić zarówno `UseKestrel` i `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-151">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="be8cf-152">`UseIISIntegration`aktywuje tylko podczas uruchamiania usług IIS lub usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="be8cf-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="be8cf-153">Aby uzyskać więcej informacji, zobacz [wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module) i [odwołania konfiguracji platformy ASP.NET Core modułu](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="be8cf-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="be8cf-154">Minimalny wdrożenia, który konfiguruje hosta (a aplikacji platformy ASP.NET Core) obejmuje określenie serwera i konfiguracji Potok żądań aplikacji:</span><span class="sxs-lookup"><span data-stu-id="be8cf-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="be8cf-155">Podczas konfigurowania hosta, możesz podać [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metody.</span><span class="sxs-lookup"><span data-stu-id="be8cf-155">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="be8cf-156">Jeśli określisz `Startup` klasy, należy zdefiniować `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="be8cf-156">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="be8cf-157">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji w ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="be8cf-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="be8cf-158">Wiele wywołań `ConfigureServices` dołączyć do siebie.</span><span class="sxs-lookup"><span data-stu-id="be8cf-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="be8cf-159">Wiele wywołań `Configure` lub `UseStartup` na `WebHostBuilder` zastąpić poprzednie ustawienia.</span><span class="sxs-lookup"><span data-stu-id="be8cf-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="be8cf-160">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="be8cf-160">Host configuration values</span></span>

<span data-ttu-id="be8cf-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) udostępnia metody do ustawiania większość wartości konfiguracji dostępne dla hosta, który można również ustawić bezpośrednio z [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) i skojarzony klucz.</span><span class="sxs-lookup"><span data-stu-id="be8cf-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="be8cf-162">Podczas ustawiania wartości z `UseSetting`, ma wartość ciągu (w cudzysłowy) niezależnie od typu.</span><span class="sxs-lookup"><span data-stu-id="be8cf-162">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="be8cf-163">Przechwyć błędy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="be8cf-163">Capture Startup Errors</span></span>

<span data-ttu-id="be8cf-164">To ustawienie określa przechwytywania błędy uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="be8cf-164">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="be8cf-165">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="be8cf-165">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="be8cf-166">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="be8cf-166">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="be8cf-167">**Domyślne**: Domyślnie `false` chyba, że aplikacja jest uruchamiana z Kestrel za usług IIS, gdzie wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-167">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="be8cf-168">**Ustawić za pomocą**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="be8cf-168">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="be8cf-169">Gdy `false`, błędy podczas wynik uruchomienia na hoście został zakończony.</span><span class="sxs-lookup"><span data-stu-id="be8cf-169">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="be8cf-170">Gdy `true`, host przechwytuje wyjątków podczas uruchamiania i podejmie próbę uruchomienia serwera.</span><span class="sxs-lookup"><span data-stu-id="be8cf-170">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-171">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-172">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="be8cf-173">Główny zawartości</span><span class="sxs-lookup"><span data-stu-id="be8cf-173">Content Root</span></span>

<span data-ttu-id="be8cf-174">To ustawienie określa, gdzie platformy ASP.NET Core rozpocznie się wyszukiwanie plików zawartości, np. widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="be8cf-174">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="be8cf-175">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="be8cf-175">**Key**: contentRoot</span></span>  
<span data-ttu-id="be8cf-176">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="be8cf-176">**Type**: *string*</span></span>  
<span data-ttu-id="be8cf-177">**Domyślna**: domyślne do folderu, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="be8cf-177">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="be8cf-178">**Ustawić za pomocą**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="be8cf-178">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="be8cf-179">Główny zawartości jest również używany jako podstawowa ścieżka dla [ustawienia sieci Web głównego](#web-root).</span><span class="sxs-lookup"><span data-stu-id="be8cf-179">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="be8cf-180">Jeśli ścieżka nie istnieje, host nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="be8cf-180">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-181">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-182">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="be8cf-183">Błędy szczegółowe</span><span class="sxs-lookup"><span data-stu-id="be8cf-183">Detailed Errors</span></span>

<span data-ttu-id="be8cf-184">Określa, czy szczegółowe błędy, które mają być przechwytywane.</span><span class="sxs-lookup"><span data-stu-id="be8cf-184">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="be8cf-185">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="be8cf-185">**Key**: detailedErrors</span></span>  
<span data-ttu-id="be8cf-186">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="be8cf-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="be8cf-187">**Domyślna**: false</span><span class="sxs-lookup"><span data-stu-id="be8cf-187">**Default**: false</span></span>  
<span data-ttu-id="be8cf-188">**Ustawić za pomocą**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="be8cf-188">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="be8cf-189">Po włączeniu (lub gdy <a href="#environment">środowiska</a> ma ustawioną wartość `Development`), aplikacji znajdują się szczegółowe wyjątki.</span><span class="sxs-lookup"><span data-stu-id="be8cf-189">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-190">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-191">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="be8cf-192">Środowisko</span><span class="sxs-lookup"><span data-stu-id="be8cf-192">Environment</span></span>

<span data-ttu-id="be8cf-193">Ustawia środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="be8cf-193">Sets the app's environment.</span></span>

<span data-ttu-id="be8cf-194">**Klucz**: środowiska</span><span class="sxs-lookup"><span data-stu-id="be8cf-194">**Key**: environment</span></span>  
<span data-ttu-id="be8cf-195">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="be8cf-195">**Type**: *string*</span></span>  
<span data-ttu-id="be8cf-196">**Domyślna**: produkcji</span><span class="sxs-lookup"><span data-stu-id="be8cf-196">**Default**: Production</span></span>  
<span data-ttu-id="be8cf-197">**Ustawić za pomocą**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="be8cf-197">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="be8cf-198">Można ustawić *środowiska* dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="be8cf-198">You can set the *Environment* to any value.</span></span> <span data-ttu-id="be8cf-199">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-199">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="be8cf-200">Wartości nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="be8cf-200">Values aren't case sensitive.</span></span> <span data-ttu-id="be8cf-201">Domyślnie *środowiska* są odczytywane z `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="be8cf-201">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="be8cf-202">Korzystając z [programu Visual Studio](https://www.visualstudio.com/), zmienne środowiskowe może być ustawiona w *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="be8cf-202">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="be8cf-203">Aby uzyskać więcej informacji, zobacz [Praca w środowiskach wielu](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="be8cf-203">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-204">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-205">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="be8cf-206">Zestawy uruchomienia hostingu</span><span class="sxs-lookup"><span data-stu-id="be8cf-206">Hosting Startup Assemblies</span></span>

<span data-ttu-id="be8cf-207">Ustawia hostingu zestawy uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="be8cf-207">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="be8cf-208">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="be8cf-208">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="be8cf-209">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="be8cf-209">**Type**: *string*</span></span>  
<span data-ttu-id="be8cf-210">**Domyślna**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="be8cf-210">**Default**: Empty string</span></span>  
<span data-ttu-id="be8cf-211">**Ustawić za pomocą**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="be8cf-211">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="be8cf-212">Ciąg rozdzielany średnikami obsługi zestawów uruchamiania załadować podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="be8cf-212">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="be8cf-213">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="be8cf-213">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="be8cf-214">Mimo że konfiguracja domyślnie przyjmowana jest wartość pustego ciągu, hostingu zestawy uruchamiania zawsze należy uwzględniać zestawu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="be8cf-214">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="be8cf-215">Podając hostingu zestawy uruchamiania, są one dodane do zestawu aplikacji ładowania, gdy aplikacja tworzy jego wspólne usługi podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="be8cf-215">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-216">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-217">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="be8cf-218">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="be8cf-218">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="be8cf-219">Preferowane jest Hosting adresy URL</span><span class="sxs-lookup"><span data-stu-id="be8cf-219">Prefer Hosting URLs</span></span>

<span data-ttu-id="be8cf-220">Wskazuje, czy host powinien nasłuchiwać adresy URL skonfigurowano `WebHostBuilder` zamiast ustawień skonfigurowanych z `IServer` implementacji.</span><span class="sxs-lookup"><span data-stu-id="be8cf-220">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="be8cf-221">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="be8cf-221">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="be8cf-222">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="be8cf-222">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="be8cf-223">**Domyślna**: true</span><span class="sxs-lookup"><span data-stu-id="be8cf-223">**Default**: true</span></span>  
<span data-ttu-id="be8cf-224">**Ustawić za pomocą**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="be8cf-224">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="be8cf-225">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="be8cf-225">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-226">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-227">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="be8cf-228">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="be8cf-228">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="be8cf-229">Zapobiegaj Hosting uruchamiania</span><span class="sxs-lookup"><span data-stu-id="be8cf-229">Prevent Hosting Startup</span></span>

<span data-ttu-id="be8cf-230">Uniemożliwia automatyczne ładowanie hosting zestawy uruchamiania, tym hosting zestawy uruchamiania konfigurowane za pomocą zestawu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="be8cf-230">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="be8cf-231">Zobacz [dodać aplikacji funkcje z zewnętrznych zestawu za pomocą IHostingStartup](xref:hosting/ihostingstartup) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="be8cf-231">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="be8cf-232">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="be8cf-232">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="be8cf-233">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="be8cf-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="be8cf-234">**Domyślna**: false</span><span class="sxs-lookup"><span data-stu-id="be8cf-234">**Default**: false</span></span>  
<span data-ttu-id="be8cf-235">**Ustawić za pomocą**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="be8cf-235">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="be8cf-236">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="be8cf-236">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-237">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-237">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-238">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-238">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="be8cf-239">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="be8cf-239">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="be8cf-240">Adresy URL serwerów</span><span class="sxs-lookup"><span data-stu-id="be8cf-240">Server URLs</span></span>

<span data-ttu-id="be8cf-241">Wskazuje adresy IP lub adresy hostów, porty i protokoły, które serwer powinien nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="be8cf-241">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="be8cf-242">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="be8cf-242">**Key**: urls</span></span>  
<span data-ttu-id="be8cf-243">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="be8cf-243">**Type**: *string*</span></span>  
<span data-ttu-id="be8cf-244">**Domyślna**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="be8cf-244">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="be8cf-245">**Ustawić za pomocą**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="be8cf-245">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="be8cf-246">Ustaw rozdzielonych średnikami (;) lista adresów URL prefiksy powinny odpowiadać serwera.</span><span class="sxs-lookup"><span data-stu-id="be8cf-246">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="be8cf-247">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-247">For example, `http://localhost:123`.</span></span> <span data-ttu-id="be8cf-248">Użyj "\*" Aby wskazać, że serwer powinien nasłuchiwać żądań adresy IP lub nazwa hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="be8cf-248">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="be8cf-249">Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="be8cf-249">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="be8cf-250">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="be8cf-250">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-251">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="be8cf-252">Kestrel ma własny interfejs API konfiguracji punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="be8cf-252">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="be8cf-253">Aby uzyskać więcej informacji, zobacz [Kestrel implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="be8cf-253">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-254">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-254">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="be8cf-255">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="be8cf-255">Shutdown Timeout</span></span>

<span data-ttu-id="be8cf-256">Określa czas oczekiwania na zamknięcie hosta sieci web.</span><span class="sxs-lookup"><span data-stu-id="be8cf-256">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="be8cf-257">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="be8cf-257">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="be8cf-258">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="be8cf-258">**Type**: *int*</span></span>  
<span data-ttu-id="be8cf-259">**Domyślna**: 5</span><span class="sxs-lookup"><span data-stu-id="be8cf-259">**Default**: 5</span></span>  
<span data-ttu-id="be8cf-260">**Ustawić za pomocą**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="be8cf-260">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="be8cf-261">Mimo że akceptuje klucz *int* z `UseSetting` (na przykład `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` przyjmuje — metoda rozszerzenia `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-261">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="be8cf-262">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="be8cf-262">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-263">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-263">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-264">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-264">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="be8cf-265">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="be8cf-265">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="be8cf-266">Zestaw startowy</span><span class="sxs-lookup"><span data-stu-id="be8cf-266">Startup Assembly</span></span>

<span data-ttu-id="be8cf-267">Określa zestaw do wyszukania `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="be8cf-267">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="be8cf-268">**Klucz**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="be8cf-268">**Key**: startupAssembly</span></span>  
<span data-ttu-id="be8cf-269">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="be8cf-269">**Type**: *string*</span></span>  
<span data-ttu-id="be8cf-270">**Domyślna**: zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="be8cf-270">**Default**: The app's assembly</span></span>  
<span data-ttu-id="be8cf-271">**Ustawić za pomocą**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="be8cf-271">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="be8cf-272">Nazwę można odwoływać się do zestawu (`string`) lub typu (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="be8cf-272">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="be8cf-273">Jeśli wiele `UseStartup` metody są nazywane, pierwszeństwo ma ostatnią.</span><span class="sxs-lookup"><span data-stu-id="be8cf-273">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-274">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-274">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-275">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-275">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="be8cf-276">Główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="be8cf-276">Web Root</span></span>

<span data-ttu-id="be8cf-277">Ustawia ścieżkę względną do statycznego zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="be8cf-277">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="be8cf-278">**Klucz**: webroot</span><span class="sxs-lookup"><span data-stu-id="be8cf-278">**Key**: webroot</span></span>  
<span data-ttu-id="be8cf-279">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="be8cf-279">**Type**: *string*</span></span>  
<span data-ttu-id="be8cf-280">**Domyślne**: Jeśli nie jest określony, wartością domyślną jest "(Content Root)/wwwroot", jeśli ścieżka istnieje.</span><span class="sxs-lookup"><span data-stu-id="be8cf-280">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="be8cf-281">Jeśli ścieżka nie istnieje, jest używany dostawca pliku zerowa.</span><span class="sxs-lookup"><span data-stu-id="be8cf-281">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="be8cf-282">**Ustawić za pomocą**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="be8cf-282">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-283">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-283">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-284">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-284">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="be8cf-285">Zastępowanie konfiguracji</span><span class="sxs-lookup"><span data-stu-id="be8cf-285">Overriding configuration</span></span>

<span data-ttu-id="be8cf-286">Użyj [konfiguracji](xref:fundamentals/configuration/index) Aby skonfigurować hosta.</span><span class="sxs-lookup"><span data-stu-id="be8cf-286">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="be8cf-287">W poniższym przykładzie konfiguracja hosta jest opcjonalnie określić w *hosting.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="be8cf-287">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="be8cf-288">Wszelkie konfiguracja została załadowana z *hosting.json* plik może być zastąpiona przez argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="be8cf-288">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="be8cf-289">Zbudowany konfiguracji (w `config`) służy do konfigurowania hosta z `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-289">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-290">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="be8cf-291">*hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="be8cf-291">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="be8cf-292">Zastępowanie konfiguracji dostarczonych przez `UseUrls` z *hosting.json* config argument pierwszą, wiersza polecenia config drugi:</span><span class="sxs-lookup"><span data-stu-id="be8cf-292">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-293">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-293">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="be8cf-294">*hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="be8cf-294">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="be8cf-295">Zastępowanie konfiguracji dostarczonych przez `UseUrls` z *hosting.json* config argument pierwszą, wiersza polecenia config drugi:</span><span class="sxs-lookup"><span data-stu-id="be8cf-295">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="be8cf-296">`UseConfiguration` — Metoda rozszerzenia nie jest obecnie stanie podczas analizowania sekcji konfiguracji zwrócony przez `GetSection` (na przykład `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-296">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="be8cf-297">`GetSection` — Metoda filtruje klucze konfiguracji do sekcji żądanie, jednak nie pozostawia nazwy sekcji kluczy (na przykład `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="be8cf-297">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="be8cf-298">`UseConfiguration` Metoda oczekuje klucze do dopasowania `WebHostBuilder` kluczy (na przykład `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="be8cf-298">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="be8cf-299">Występowanie nazwy sekcji kluczy uniemożliwia wartości w sekcji Konfigurowanie hosta.</span><span class="sxs-lookup"><span data-stu-id="be8cf-299">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="be8cf-300">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="be8cf-300">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="be8cf-301">Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie Sekcja konfiguracyjna do WebHostBuilder.UseConfiguration używa kluczy pełna](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="be8cf-301">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="be8cf-302">Aby określić hosta, uruchom na określony adres URL, możesz przesłać żądaną wartość z wiersza polecenia podczas wykonywania `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-302">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="be8cf-303">Argument wiersza polecenia zastępuje `urls` wartość z *hosting.json* pliku, a serwer nasłuchuje na porcie 8080:</span><span class="sxs-lookup"><span data-stu-id="be8cf-303">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="be8cf-304">Znaczenie kolejności</span><span class="sxs-lookup"><span data-stu-id="be8cf-304">Ordering importance</span></span>

<span data-ttu-id="be8cf-305">Niektóre `WebHostBuilder` ustawienia najpierw są odczytywane z zmiennych środowiskowych, jeśli ustawiona.</span><span class="sxs-lookup"><span data-stu-id="be8cf-305">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="be8cf-306">Te zmienne środowiskowe, użyj formatu `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-306">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="be8cf-307">Aby ustawić adresów URL, które serwer nasłuchuje na domyślne, należy ustawić `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-307">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="be8cf-308">Można zastąpić jedną z tych wartości zmiennych środowiskowych, określając konfiguracji (przy użyciu `UseConfiguration`) lub przez jawne ustawienie wartości (przy użyciu `UseSetting` lub jednej z metod rozszerzenia jawne, takich jak `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="be8cf-308">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="be8cf-309">Host używa jednego z tych opcji ustawia wartość ostatnio.</span><span class="sxs-lookup"><span data-stu-id="be8cf-309">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="be8cf-310">Jeśli chcesz programowo ustawiony domyślny adres URL na jedną wartość, ale zezwolenie na należy zastąpić konfiguracji, można użyć wiersza polecenia konfigurację po ustawieniu adres URL.</span><span class="sxs-lookup"><span data-stu-id="be8cf-310">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="be8cf-311">Zobacz [konfiguracji zastąpiona](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="be8cf-311">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="be8cf-312">Uruchamianie hosta</span><span class="sxs-lookup"><span data-stu-id="be8cf-312">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be8cf-313">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-313">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="be8cf-314">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="be8cf-314">**Run**</span></span>

<span data-ttu-id="be8cf-315">`Run` Metoda uruchamiania aplikacji sieci web i blokuje wątek wywołujący do czasu zamknięcia hosta:</span><span class="sxs-lookup"><span data-stu-id="be8cf-315">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="be8cf-316">**Start**</span><span class="sxs-lookup"><span data-stu-id="be8cf-316">**Start**</span></span>

<span data-ttu-id="be8cf-317">Można uruchomić hosta w sposób nieblokujące przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="be8cf-317">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="be8cf-318">W przypadku przekazania listę adresów URL do `Start` metody go nasłuchuje na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="be8cf-318">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="be8cf-319">Można zainicjować i rozpocząć nowego hosta za pomocą wstępnie skonfigurowane ustawienia domyślne `CreateDefaultBuilder` przy użyciu metody statycznej wygody.</span><span class="sxs-lookup"><span data-stu-id="be8cf-319">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="be8cf-320">Te metody uruchomienia serwera bez dane wyjściowe konsoli i z [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) poczekaj, aż podział (Ctrl-C/sigint — lub sigterm —):</span><span class="sxs-lookup"><span data-stu-id="be8cf-320">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="be8cf-321">**Start (RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="be8cf-321">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="be8cf-322">Rozpoczynać `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="be8cf-322">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="be8cf-323">Tworzenie żądania w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="be8cf-323">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="be8cf-324">`WaitForShutdown`bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="be8cf-324">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="be8cf-325">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="be8cf-325">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="be8cf-326">**Start (ciąg adresu url, RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="be8cf-326">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="be8cf-327">Uruchom przy użyciu adresu URL i `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="be8cf-327">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="be8cf-328">Tworzy wynik, w postaci **Start (aplikacja RequestDelegate)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-328">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="be8cf-329">**Start (akcji<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="be8cf-329">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="be8cf-330">Użyj wystąpienia `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) do używania oprogramowania pośredniczącego routingu:</span><span class="sxs-lookup"><span data-stu-id="be8cf-330">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="be8cf-331">Użyj następujących żądań przeglądarki z przykładem:</span><span class="sxs-lookup"><span data-stu-id="be8cf-331">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="be8cf-332">Żądanie</span><span class="sxs-lookup"><span data-stu-id="be8cf-332">Request</span></span>                                    | <span data-ttu-id="be8cf-333">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="be8cf-333">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="be8cf-334">Witaj, pole!</span><span class="sxs-lookup"><span data-stu-id="be8cf-334">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="be8cf-335">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="be8cf-335">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="be8cf-336">Zgłasza wyjątek z ciągiem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="be8cf-336">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="be8cf-337">Zgłasza wyjątek z ciągiem "Uh Niestety!"</span><span class="sxs-lookup"><span data-stu-id="be8cf-337">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="be8cf-338">Sante, Jan!</span><span class="sxs-lookup"><span data-stu-id="be8cf-338">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="be8cf-339">Cześć ludzie!</span><span class="sxs-lookup"><span data-stu-id="be8cf-339">Hello World!</span></span>                             |

<span data-ttu-id="be8cf-340">`WaitForShutdown`bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="be8cf-340">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="be8cf-341">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="be8cf-341">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="be8cf-342">**Start (ciągu adresu url, Akcja<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="be8cf-342">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="be8cf-343">Użyj adresu URL i wystąpienie `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="be8cf-343">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="be8cf-344">Tworzy wynik, w postaci **Start (akcji<IRouteBuilder> routeBuilder)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-344">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="be8cf-345">**StartWith (akcji<IApplicationBuilder> aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="be8cf-345">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="be8cf-346">Podaj delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="be8cf-346">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="be8cf-347">Tworzenie żądania w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="be8cf-347">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="be8cf-348">`WaitForShutdown`bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="be8cf-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="be8cf-349">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="be8cf-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="be8cf-350">**StartWith (ciągu adresu url, Akcja<IApplicationBuilder> aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="be8cf-350">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="be8cf-351">Podaj adres URL i delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="be8cf-351">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="be8cf-352">Tworzy wynik, w postaci **StartWith (akcji<IApplicationBuilder> aplikacji)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-352">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be8cf-353">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be8cf-353">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="be8cf-354">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="be8cf-354">**Run**</span></span>

<span data-ttu-id="be8cf-355">`Run` Metoda uruchamiania aplikacji sieci web i blokuje wątek wywołujący do czasu zamknięcia hosta:</span><span class="sxs-lookup"><span data-stu-id="be8cf-355">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="be8cf-356">**Start**</span><span class="sxs-lookup"><span data-stu-id="be8cf-356">**Start**</span></span>

<span data-ttu-id="be8cf-357">Można uruchomić hosta w sposób nieblokujące przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="be8cf-357">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="be8cf-358">W przypadku przekazania listę adresów URL do `Start` metody go nasłuchuje na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="be8cf-358">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="be8cf-359">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="be8cf-359">IHostingEnvironment interface</span></span>

<span data-ttu-id="be8cf-360">[Interfejsu IHostingEnvironment](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) zawiera informacje o aplikacji sieci web środowiska macierzystego.</span><span class="sxs-lookup"><span data-stu-id="be8cf-360">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="be8cf-361">Można użyć [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskanie `IHostingEnvironment` aby można było używać ich właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="be8cf-361">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="be8cf-362">Można użyć [podejście oparte na Konwencji](xref:fundamentals/environments#startup-conventions) Aby skonfigurować aplikację podczas uruchamiania na podstawie środowiska.</span><span class="sxs-lookup"><span data-stu-id="be8cf-362">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="be8cf-363">Alternatywnie można wstrzyknąć `IHostingEnvironment` do `Startup` konstruktora do użycia w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="be8cf-363">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="be8cf-364">Oprócz `IsDevelopment` — metoda rozszerzenia, `IHostingEnvironment` oferuje `IsStaging`, `IsProduction`, i `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="be8cf-364">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="be8cf-365">Zobacz [Praca w środowiskach wielu](xref:fundamentals/environments) szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="be8cf-365">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="be8cf-366">`IHostingEnvironment` Usługi także mogą zostać dodane bezpośrednio do `Configure` metody do konfigurowania potoku przetwarzania sieci:</span><span class="sxs-lookup"><span data-stu-id="be8cf-366">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="be8cf-367">Można wstrzyknąć `IHostingEnvironment` do `Invoke` metody podczas tworzenia niestandardowego [oprogramowanie pośredniczące](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="be8cf-367">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="be8cf-368">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="be8cf-368">IApplicationLifetime interface</span></span>

<span data-ttu-id="be8cf-369">[Interfejsu IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umożliwia wykonywanie czynności po uruchamiania i wyłączania.</span><span class="sxs-lookup"><span data-stu-id="be8cf-369">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="be8cf-370">Trzy właściwości w interfejsie są anulowanie tokenów, które można zarejestrować z `Action` metod, aby określić zdarzenia uruchamiania i wyłączania.</span><span class="sxs-lookup"><span data-stu-id="be8cf-370">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="be8cf-371">Istnieje również `StopApplication` metody.</span><span class="sxs-lookup"><span data-stu-id="be8cf-371">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="be8cf-372">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="be8cf-372">Cancellation Token</span></span>    | <span data-ttu-id="be8cf-373">Wyzwalane podczas &#8230;</span><span class="sxs-lookup"><span data-stu-id="be8cf-373">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="be8cf-374">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="be8cf-374">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="be8cf-375">Host wykonuje łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="be8cf-375">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="be8cf-376">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="be8cf-376">Requests may still be processing.</span></span> <span data-ttu-id="be8cf-377">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="be8cf-377">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="be8cf-378">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="be8cf-378">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="be8cf-379">Wszystkie żądania należy całkowicie przetworzone.</span><span class="sxs-lookup"><span data-stu-id="be8cf-379">All requests should be completely processed.</span></span> <span data-ttu-id="be8cf-380">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="be8cf-380">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="be8cf-381">Metoda</span><span class="sxs-lookup"><span data-stu-id="be8cf-381">Method</span></span>            | <span data-ttu-id="be8cf-382">Akcja</span><span class="sxs-lookup"><span data-stu-id="be8cf-382">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="be8cf-383">Zakończenie żądania bieżącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="be8cf-383">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="be8cf-384">Rozwiązywanie problemów z System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="be8cf-384">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="be8cf-385">**Dotyczy tylko platformy ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="be8cf-385">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="be8cf-386">W przypadku tworzenia hosta przez wstrzykiwanie `IStartup` bezpośrednio do kontenera iniekcji zależności zamiast wywoływania `UseStartup` lub `Configure`, może wystąpić następujący błąd: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-386">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="be8cf-387">Dzieje się tak dlatego [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (bieżącego zestawu) jest wymagane do skanowania pod kątem `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-387">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="be8cf-388">Jeśli ręcznie wprowadzić `IStartup` do kontenera iniekcji zależności, Dodaj następujące wywołanie do Twojej `WebHostBuilder` o podanej nazwie zestawu:</span><span class="sxs-lookup"><span data-stu-id="be8cf-388">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="be8cf-389">Możesz też dodać manekina `Configure` do Twojej `WebHostBuilder`, który określa `applicationName`(`ApplicationKey`) automatycznie:</span><span class="sxs-lookup"><span data-stu-id="be8cf-389">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="be8cf-390">**Uwaga**: jest to tylko wymagane wraz z wydaniem programu ASP.NET 2.0 rdzeni i tylko wtedy gdy zgłaszasz nie `UseStartup` lub `Configure`.</span><span class="sxs-lookup"><span data-stu-id="be8cf-390">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="be8cf-391">Aby uzyskać więcej informacji, zobacz [anonsów: Microsoft.Extensions.PlatformAbstractions została usunięta (komentarz)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) i [próbki StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="be8cf-391">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be8cf-392">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="be8cf-392">Additional resources</span></span>

* [<span data-ttu-id="be8cf-393">Publikowanie do systemu Windows za pomocą usług IIS</span><span class="sxs-lookup"><span data-stu-id="be8cf-393">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="be8cf-394">Publikowanie w systemie Linux przy użyciu Nginx</span><span class="sxs-lookup"><span data-stu-id="be8cf-394">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="be8cf-395">Publikowanie w systemie Linux przy użyciu Apache</span><span class="sxs-lookup"><span data-stu-id="be8cf-395">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="be8cf-396">Host usługi systemu Windows</span><span class="sxs-lookup"><span data-stu-id="be8cf-396">Host in a Windows Service</span></span>](xref:hosting/windows-service)
